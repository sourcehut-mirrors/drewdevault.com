---
title: Introducing the Himitsu keyring & password manager for Unix
date: 2022-06-20
---

[Himitsu] is a new approach to storing secret information on Unix systems, such
as passwords or private keys, and I released version 0.1 this morning. It's
available on [Alpine Linux] community and the [Arch User Repository], with [more
distributions] hopefully on the way soon.

[Himitsu]: https://himitsustore.org
[Alpine Linux]: https://wiki.alpinelinux.org/wiki/Himitsu
[Arch User Repository]: https://wiki.archlinux.org/title/Himitsu
[more distributions]: https://repology.org/project/himitsu/versions

So, what is Himitsu and what makes it special? The following video introduces
the essential concepts and gives you an idea of what's possible:

<video src="https://himitsustore.org/intro.mp4" controls></video>

If you prefer reading to watching, this blog post includes everything that's in
the video.

## What is Himitsu?

Himitsu draws inspiration from Plan 9's [factotum], but polished up and
redesigned for Unix. At its core, Himitsu is a key/value store and a simple
protocol for interacting with it. For example, a web login could be stored like
so:

[factotum]: http://man.9front.org/4/factotum

```
proto=web host=example.org user=jdoe password!=hunter2
```

Himitsu has no built-in knowledge of web logins, it just stores arbitrary keys
and values. The bang (!) indicates that the password is a "secret" value, and
the "proto" key defines additional conventions for each kind of secret. For
proto=web, each key/value pair represents a form field on a HTML login form.

We can query the key store using the "hiq" command. For instance, we can obtain
the example key above by querying for any key with "proto=web", any "host",
"user", and "password" value, and an optional "comment" value:

```
$ hiq proto=web host user password! comment?
proto=web host=example.org user=jdoe password!
```

You'll notice that the password is hidden here. In order to obtain it, we must
ask for the user's consent.

```
$ hiq -d proto=web host user password! comment?
```

![A screenshot of a GTK+ dialog confirming the operation](https://redacted.moe/f/85eb1b52.png)

```
proto=web host=example.org user=jdoe password!=hunter2
```

You can also use hiq to add or delete keys, or incorporate it into a shell
pipeline:

```
$ hiq -dFpassword host=example.org
hunter2
```

## A simple, extensible protocol

The protocol is a simple line-oriented text protocol, which is documented in the
[himitsu-ipc(5)] manual page. We can also use it via netcat:

[himitsu-ipc(5)]: https://himitsustore.org/docs/himitsu-ipc.5.html

```
$ nc -U $XDG_RUNTIME_DIR/himitsu
query host=example.org
key proto=web host=example.org user=jdoe password!
end
query -d host=example.org
key proto=web host=example.org user=jdoe password!=hunter2
end
```

The consent prompter also uses a standardized protocol, documented by
[himitsu-prompter(5)]. Based on this, you can implement new prompters for Qt, or
the TTY, or any other technology appropriate to your system, or implement a more
novel approach, such as sending a push notification to your phone to facilitate
consent.

[himitsu-prompter(5)]: https://himitsustore.org/docs/himitsu-prompter.5.html

## Additional frontends

Based on these protocols, a number of additional integrations are possible.
Martijn Braam has written a nice GTK+ frontend called [keyring]:

[keyring]: https://git.sr.ht/~martijnbraam/keyring/
![A screenshot of the GTK+ frontend](https://brixitcdn.net/metainfo/keyring.png)

There's also a [Firefox add-on] which auto-fills forms for keys with proto=web:

[Firefox add-on]: https://addons.mozilla.org/en-US/firefox/addon/himitsu-integration/
![Screenshot of himitsu-firefox](https://redacted.moe/f/73328356.png)

We also have a package called [himitsu-ssh] which provides an SSH agent:

[himitsu-ssh]: https://git.sr.ht/~sircmpwn/himitsu-ssh

```
$ hissh-import < ~/.ssh/id_ed25519
Enter SSH key passphrase: 
key proto=ssh type=ssh-ed25519 pkey=pF7SljE25sVLdWvInO4gfqpJbbjxI6j+tIUcNWzVTHU= skey! comment=sircmpwn@homura
$ ssh-add -l
256 SHA256:kPr5ZKTNE54TRHGSaanhcQYiJ56zSgcpKeLZw4/myEI sircmpwn@homura (ED25519)
$ ssh git@git.sr.ht
Hi sircmpwn! You've successfully authenticated, but I do not provide an interactive shell. Bye!
Connection to git.sr.ht closed.
```

I hope to see an ecosystem of tools built around Himitsu to grow. New frontends
like keyring would be great, and new integrations like GPG agents would also be
nice to see.

## Zero configuration

Himitsu-aware software can discover your credentials and connection details
without any additional configuration. For example, a mail client might look for
`proto=imap` and `proto=smtp` and discover something like this:

```
proto=imap host=imap.migadu.com user=sir@cmpwn.com password! port=993 enc=tls
proto=smtp host=imap.migadu.com user=sir@cmpwn.com password! port=465 enc=tls
```

After a quick consent prompt, the software can load your IMAP and SMTP
configuration and get connected without any manual steps. With an agent like
himitsu-ssh, it could even connect without actually handling your credentials
directly &mdash; a use-case we want to support with improvements to the prompter
UI (to distinguish between a case where an application will *view* versus *use*
your credentials).

## The cryptography

Your key store is located at $XDG\_DATA\_HOME/himitsu/. The key is derived by
mixing your password with argon2, and the resulting key is used for AEAD with
XChaCha20+Poly1305. The "index" file contains a list of base64-encoded encrypted
blobs, one per line, enumerating the keys in the key store.[^1] Secret keys are
encrypted and stored separately in files in this directory. If you like the pass
approach to storing your keys in git, you can easily commit this directory to a
git repository, or haul it along to each of your devices with whatever other
means is convenient to you.

[^1]: This offers an improvement over pass, for example, by not storing the list of entries in plain text.

Himitsu is written in Hare and uses cryptography primitives available from its
standard library. Note that these have not been audited.

## Future plans

I'd like to expand on Himitsu in the future. One idea is to store your full disk
encryption password in Himitsu and stick a subset of your key store into the
initramfs, which you unlock during early boot, pull FDE keys out of, and then
pre-authorize the keyring for your desktop session - which you're logged in to
automatically on the basis that you were pre-authorized during boot.

We also want to add key sharing and synchronization tools. The protocol could
easily be moved to TCP and authorized with your existing key store key (we could
make an ed25519 key out of it, or generate and store one separately), so setting
up key synchronization might be as simple as:

```
$ hiq -a proto=sync host=himitsu.sr.ht
```

You could also use Himitsu for service discovery &mdash; imagine a key ring
running on your datacenter LAN with entries for your Postgres database, SMTP
credentials, and so on.

There are some other ideas that we could use your help with:

- himitsu-firefox improvements (web devs welcome!)
- Chromium support (web devs welcome!)
- Himitsu apps for phones (mobile devs welcome!)
- More key management frontends (maybe a TUI?)
- More security options &mdash; smart cards? U2F?
- hare-ssh improvements (e.g. RSA keys)
- PGP support
- Anything else you can think of

Please join us! We hang out on IRC in #himitsu on Libera Chat. Give Himitsu a
shot and let us know what you think.

Alright, back to kernel hacking. I got multi-tasking working yesterday!
