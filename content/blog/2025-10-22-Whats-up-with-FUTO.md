---
title: "What's up with FUTO?"
date: 2025-10-22
---

Some time ago, I noticed some new organization called [FUTO] popping up here and
there. I'm always interested in seeing new organizations that fund open source
popping up, and seeing as they claim several notable projects on their roster, I
explored their website with interest and gratitude. I was first confused, and
then annoyed by what I found. Confused, because their website is littered with
[bizzare manifestos][manifesto],[^manifesto] and ultimately annoyed because they
were playing fast and loose with the term "open source", using it to describe
commercial source-available software.

[^manifesto]: Here's a quote from [another one](https://futo.org/about/what-does-futo-believe/): "While the vast majority of elite coders have been neutralized by the Oligopoly, the vast majority of those remaining have become preoccupied with cryptocurrency money-making schemes."

FUTO eventually clarified their stance on "open source", first through
[satire][0] and then [somewhat more soberly][1], perpetuating the [self-serving
myth][2] that ["open source" software can privilege one party over anyone
else][3] and [still be called open source][4]. I mentally categorized them as
problematic but hoped that their donations or grants for genuinely open source
projects would do more good than the harm done by this nonsense.

By now I've learned better. **tl;dr**: FUTO is not being honest about their
"grant program", they don't have permission to pass off these logos or project
names as endorsements, and they collaborate with and promote mask-off,
self-proclaimed fascists.

[FUTO]: https://futo.org/
[manifesto]: https://futo.org/about/what-is-futo/
[0]: https://futo.org/open-source-definition/
[1]: https://futo.org/about/futo-statement-on-opensource/
[2]: https://drewdevault.com/2022/09/16/Open-source-matters.html
[3]: https://drewdevault.com/2021/01/20/FOSS-is-to-surrender-your-monopoly.html
[4]: https://drewdevault.com/2022/03/01/Open-source-is-defined-by-the-OSD.html

An early sign that something is off with FUTO is in that "sober" explanation of
their "disdain for OSI approved licenses", where they make a point of
criticizing the Open Source Initiative for banning Eric S. Raymond (aka ESR)
from their mailing lists, citing right-wing reactionary conspiracy theorist
Bryan Lunduke's blog post on the incident. Raymond is, as you may know, one of
the founders of OSI and a bigoted asshole. He was banned from the mailing lists,
not because he's a bigoted asshole, but because [he was being a toxic jerk][esr]
*on the mailing list in question*. Healthy institutions outgrow their founders.
That said, FUTO's citation and perspective on the ESR incident could be
generously explained as a simple mistake, and we should probably match
generosity with generosity given their prolific portfolio of open source grants.

[esr]: https://lists.opensource.org/pipermail/license-discuss_lists.opensource.org/2020-February/021328.html
[rms]: https://stallman-report.org/

I visited FUTO again quite recently as part of my research on [Cloudflare's
donations to fascists][cloudflare], and was pleased to discover that this
portfolio of grants had grown immensely since my last visit, and included a
number of respectable projects that I admire and depend on (and some projects I
don't especially admire, hence arriving there during my research on FOSS
projects run by fascists). But something felt fishy about this list -- surely I
would have heard about it if someone was going around giving big grants to
projects like ffmpeg, VLC, musl libc, Tor, Managarm, Blender, NeoVim -- these
projects have a lot of overlap with my social group and I hadn't heard a peep
about it.

[cloudflare]: https://drewdevault.com/2025/09/24/2025-09-24-Cloudflare-and-fascists.html
[musl libc]: https://musl.libc.org/

So I asked Rich Felker, the maintainer of [musl libc], about the FUTO grant, and
**he didn't know anything about it**. Rich and I spoke about this for a while
and eventually Rich uncovered a transaction in his [GitHub sponsors][felker
sponsors] account from FUTO: a one-time donation of $1,000. This payment
circumvents musl's established process for donations from institutional
sponsors. The donation page that FUTO used includes this explanation: "This
offer is for individuals, and may be available to small organizations on
request. Commercial entities wishing to be listed as sponsors should inquire by
email." It's pretty clear that there are special instructions for institutional
donors who wish to receive musl's endorsement as thanks for their contribution.

The extent of the FUTO "grant program", at least in the case of musl libc,
involved ignoring musl's established process for institutional sponsors, quietly
sending a modest one-time donation to one maintainer, and then plastering the
logo of a well-respected open source project on a list of "grant recipients" on
their home page. Rich eventually [posted on Mastodon][mastodon] to clarify that
the use of the musl name and logo here was unauthorized.

[felker sponsors]: https://github.com/sponsors/richfelker
[mastodon]: https://hachyderm.io/@dalias/115259232020176340

I also asked someone I know on the ffmpeg project about the grant that they had
received from FUTO and she didn't know anything about it, either. Here's what
she said:

> I'm sure we did not get a grant from them, since we tear each other to pieces
> over everything, and that would be enough to start a flame war. Unless some
> dev independently got money from them to do something, but I'm sure that we as
> a project got nothing. The only grant we've received is from the [STF] last
> year.

[STF]: https://www.sovereign.tech/

Neovim is another project FUTO lists as a grant recipient, and they also have a
[separate process for institutional sponsors](https://neovim.io/sponsors/). I
didn't reach out to anyone to confirm, but FUTO does not appear on the sponsor
list so presumably the M.O. is the same. This is also the case for
[Wireshark](https://www.wireshark.org/members),
[Conduit](https://conduit.rs/#donate), and
[KiCad](https://www.kicad.org/sponsors/sponsors/). GrapheneOS is listed
prominently as well, but [that doesn't seem to have worked
out very well for them](https://grapheneos.social/@GrapheneOS/113443396794247106).

So, it seems like FUTO is doing some shady stuff and putting a bunch of notable
FOSS projects on their home page without good reason to justify their
endorsement. Who's behind all of this?

As far as I can tell, the important figures are Eron Wolf[^wolf] and Louis
Rossmann.[^rossmann] Wolf is the founder of FUTO -- a bunch of money fell into
his lap from founding Yahoo Games before the bottom fell out of Yahoo, and he
made some smart investments to grow his wealth, which he presumably used to fund
FUTO. Rossmann is a notable figure in the right to repair movement, with a large
following on YouTube, who joined FUTO a year later and ultimately moved to
Austin to work more closely with them. His established audience and reputation
provides a marketable face for FUTO. I had heard of Rossmann prior to learning
about FUTO and held him in generally good regard, despite little specific
knowledge of his work, simply because we have a common cause in right to repair.

[^wolf]: Source: [What is FUTO?](https://futo.org/about/what-is-futo/)
[^rossmann]: Source: [Announcing the Newest Member of FUTO: Louis Rossmann!](https://www.youtube.com/watch?v=tca6xsBEuGw)

I hadn't heard of Wolf before looking into FUTO. However, in the course of my
research, several people tipped me off to his association with Curtis Yarvin
(aka moldbug), and in particular to the use of FUTO's platform and the
credentials of Wolf and Rossman to platform and promote Yarvin. Curtis Yarvin is
a full-blown, mask-off, self-proclaimed fascist. A negligible amount of due
diligence is required to verify this, but here's one source from Politico in
January 2025:

> I’ve interacted with Vance once since the election. I bumped into him at a
> party. He said, "Yarvin, you reactionary fascist." I was like, "Thank you, Mr.
> Vice President, and I’m glad I didn’t stop you from getting elected."

-- [Ian Ward for Politico](https://www.politico.com/news/magazine/2025/01/30/curtis-yarvins-ideas-00201552)

Vice President Vance and numerous other figures in the American right have cited
Yarvin as a friend and source of inspiration in shaping policy.[^yarvin policy]
Among his many political positions, Yarvin has proclaimed that black people
are genetically predisposed to a lower IQ than white people, and moreover
suggests that black people are inherently suitable for
enslavement.[^yarvin slavery]

[^yarvin policy]: Source: [Politico: The Seven Thinkers and Groups That Have Shaped JD Vance’s Unusual Worldview](https://www.politico.com/news/magazine/2024/07/18/jd-vance-world-view-sources-00168984)
[^yarvin slavery]: Source: [Inc.: Controversy Rages Over ‘Pro-Slavery’ Tech Speaker Curtis Yarvin](https://www.inc.com/tess-townsend/why-it-matters-that-an-obscure-programming-conference-is-hosting-mencius-moldbug.html)

Yarvin has appeared on FUTO's social media channels, in particular in an
interview published on
[PeerTube](https://peertube.futo.org/w/eSzKtjupL928QzxPXC5k2R) and
[Odysee](https://odysee.com/@FUTO:e/curtisyarvin:2), the latter a platform
controversial for its role in spreading hate speech and
misinformation.[^odysee hate speech] Yarvin [also appeared on stage][rossmann
moldbug] to "debate" Louis Rossmann in June 2022, in which Yarvin is permitted
to speak at length with minimal interruptions or rebuttals to argue for an
authoritarian techno-monarchy to replace democracy.

[^odysee hate speech]: Source: [The Guardian: Video platform chief says Nazi posts on white superiority do not merit removal](https://www.theguardian.com/world/2021/may/14/odysee-video-platform-nazi-content-not-grounds-for-removal)

[rossmann moldbug]: https://www.youtube.com/watch?v=iFn5t4etqNg

Rossmann caught some flack for this "debate" and gave a milquetoast response in
[a YouTube comment](https://www.youtube.com/watch?v=iFn5t4etqNg&lc=Ugx7ba2sI30CGEylY_R4AaABAg)
on this video, explaining that he agreed to this on very short notice as a favor
to Eron, who had donated "a million" to Rossmann's non-profit prior to bringing
Rossmann into the fold at FUTO. Rossmann does rebuke Yarvin's thesis, albeit
buried in this YouTube comment rather than when he had the opportunity to do so
on-stage during the debate. Don't argue with fascists, Louis -- they aren't
arguing with *you*, they are pitching their ideas[^fascist ideas] to *the
audience*. Smart fascists are experts at misdirection and bad-faith debate
tactics and as a consequence Rossmann just becomes a vehicle for fascist
propaganda -- consult the YouTube comments to see who this video resonates with
the most.

In the end, Rossmann seems to regret agreeing to this debate. I don't think that
Eron Wolf regrets it, though -- based on his facilitation of this debate and his
own interview with Yarvin on the FUTO channel a month later, I can only assume
that Wolf considers Yarvin a close associate. No surprise given that Wolf is
precisely the kind of insecure silicon valley techbro Yarvin's rhetoric is
designed to appeal to -- moderately wealthy but unknown, and according to
Yarvin, fit to be a king. Rossmann probably needs to reflect on why he
associates with and lends his reputation to an organization that openly and
unapologetically platforms its founder's fascist friends.

[^fascist ideas]: You know, authoritarianism, anti-democracy, genocide, etc.

In summary, FUTO is not just the product of some eccentric who founded a
grant-making institution that funds open source at the cost of making us read
his weird manifestos on free markets and oligopoly. It's a private, for-profit
company that associates with and uses their brand to promote fascists. They push
an open-washing narrative and they portray themselves as a grant-making
institution when, in truth, they're passing off a handful of small donations as
if they were endorsements from dozens of respectable, high-profile open source
projects, in an attempt to legitimize themselves, and, indirectly, legitimize
people they platform like Curtis Yarvin.

So, if you read this and discover that *your* project's name and logo is being
proudly displayed [on the front page](https://futo.org) of a fascist-adjacent,
washed-up millionaire's scummy vanity company, and you don't like that, maybe
you should ask them to knock it off? Eron, Louis -- you know that a lot of these
logos are trademarked, right?
