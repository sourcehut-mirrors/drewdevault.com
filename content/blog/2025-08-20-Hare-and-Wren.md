---
title: Embedding Wren in Hare
date: 2025-08-20
---

I've been on the lookout for a scripting language which can be neatly embedded
into Hare programs. Perhaps the obvious candidate is [Lua] -- but I'm not
particularly enthusiastic about it. When I was evaluating the landscape of tools
which are "like Lua, but not Lua", I found an interesting contender: [Wren].

[Lua]: https://www.lua.org/
[Wren]: https://wren.io/

I found that Wren punches far above its weight for such a simple language. It's
object oriented, which, you know, take it or leave it depending on your
use-case, but it's very straightforwardly interesting for what it is. I found a
few things to complain about, of course -- its scope rules are silly, the C API
has some odd limitations here and there, and in my opinion the "standard
library" provided by wren CLI is poorly designed. But, surprisingly, my list of
complaints more or less ends there, and I was excited to build a nice interface
to it from Hare.

The result is [hare-wren]. Check it out!

[hare-wren]: https://wren.builtwithhare.org

The basic Wren C API is relatively straightforwardly exposed to Hare via the
wren module, though I elected to mold it into a more idiomatic Hare interface
rather than expose the C API directly to Hare. You can use it something like
this:

```hare
use wren;

export fn main() void = {
	const vm = wren::new(wren::stdio_config);
	defer wren::destroy(vm);
	wren::interpret(vm, "main", `
		System.print("Hello world!")
	`)!;
};
```

---

```
$ hare run -lc main.ha
Hello world!
```

Calling Hare from Wren and vice-versa is also possible with hare-wren, of
course. Here's another example:

```hare
use fmt;
use wren;

export fn main() void = {
	let config = *wren::stdio_config;
	config.bind_foreign_method = &bind_foreign_method;

	const vm = wren::new(&config);
	defer wren::destroy(vm);

	wren::interpret(vm, "main", `
	class Example {
		foreign static greet(user)
	}

	System.print(Example.greet("Harriet"))
	`)!;
};

fn bind_foreign_method(
	vm: *wren::vm,
	module: str,
	class_name: str,
	is_static: bool,
	signature: str,
) nullable *wren::foreign_method_fn = {
	const is_valid = class_name == "Example" &&
		signature == "greet(_)" && is_static;
	if (!is_valid) {
		return null;
	};
	return &greet_user;
};

fn greet_user(vm: *wren::vm) void = {
	const user = wren::get_string(vm, 1)!;
	const greeting = fmt::asprintf("Hello, {}!", user)!;
	defer free(greeting);
	wren::set_string(vm, 0, greeting);
};
```

---

```
$ hare run -lc main.ha
Hello, Harriet!
```

In addition to exposing the basic Wren virtual machine to Hare, hare-wren has an
optional submodule, wren::api, which implements a simple async runtime based on
[hare-ev] and a modest "standard" library, much like [Wren CLI]. I felt that the
Wren CLI libraries had a lot of room for improvement, so I made the call to
implement a standard library which is only somewhat compatible with Wren CLI.

[hare-ev]: https://sr.ht/~sircmpwn/hare-ev
[Wren CLI]: https://wren.io/cli/

In addition to the async runtime, Hare's wren::api runtime provides some basic
features for reading and writing files, querying the process arguments and
environment, etc. It's not much but it is, perhaps, an interesting place to
begin building out something a bit more interesting. A simple module loader is
also included, which introduces some conventions for installing third-party Wren
modules that may be of use for future projects to add new libraries and such.

Much like wren-cli, hare-wren also provides the `hwren` command, which makes
this runtime, standard library, and module loader conveniently available from
the command line. It does not, however, support a REPL at the moment.

I hope you find it interesting! I have a few projects down the line which might
take advantage of hare-wren, and it would be nice to expand the wren::api
library a bit more as well. If you have a Hare project which would benefit from
embedding Wren, please let me know -- and consider sending some patches to
improve it!
