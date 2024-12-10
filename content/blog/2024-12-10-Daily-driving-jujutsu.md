---
title: I'm daily driving Jujutsu, and maybe you should too
date: 2024-12-10
---

I'm not the first to write about how [Jujutsu][0] won me over. I've seen it off
and on, and each time it came across my feed it was bumped a bit higher in my
"list of things to look at eventually". It finally reached the top spot, I
think, when I saw [Tony Finn's post][1] and made some time for it that week. I
was skeptical; jj is one of many git-but-not-git tools currently and previously
on my "list", and I have kept an open mind but ultimately have always been
underwhelmed by such endeavours.

[0]: https://martinvonz.github.io/jj/latest/
[1]: https://tonyfinn.com/blog/jj/

Jujutsu is a version control system. They aim to be independent at some point
but for now it is a heady frontend on top of git (a big advantage -- all of your
existing git repos and tools are trivially compatible with it). Like many other
tools in this niche, the jj pitch begins from the thesis that git's user
interface is bad. Every time I've heard this pitch, for jj or otherwise, my
enthusiasm has rapidly waned. I really like git! I think that its internals are
the platonic ideal version control system and its porcelain[^porcelain] makes a
lot more sense if you grok its internals -- though indeed I would agree that the
porcelain is far from perfect.

[^porcelain]: The user-interface, as contrasted from the internals -- the "plumbing". Ha ha ha.

Every not-git VCS I have evaluated over the past few years have soured me by
answering the "git's user interface is bad" premise with "and therefore we
should simplify it to the lowest common denominator", which is to say, "we are
taking all of your toys away, power user, for the sake of the noob". I began to
explore Jujutsu expecting to find more of the same. Where
<abbr title="Jujutsu">jj</abbr> differs, however, from the other not-gits, is
that it begins from "git's user interface is bad" and follows with "but you,
power user, *your workflow is the correct way to use git*, and our raison d'être
is to make it easier." Wow! Consider me flattered, and intrigued.

As a git power user, I rely heavily on [git rebase] to edit my git history as I
work, frequently squashing and splitting and editing commits as I work, and I
used "stacked diffs" without branches [before it was cool][previously]. jj makes
every part of my workflow easier and faster. Enough ink has been spilled
presenting jj in depth, so instead I'll just share with you an anecdote of
my "wow" moment with Jujutsu.

[git rebase]: https://git-rebase.io
[previously]: https://drewdevault.com/2020/04/06/My-weird-branchless-git-workflow.html

One day I was working on a large-ish change. I had written a few commits over
the course of the day towards this end. However, I noticed that I had overlooked
something in a commit three or four commits earlier. So I touched up the
relevant code and then ran `jj squash -i -t <commit ID>` to squash the changes
into the earlier `<commit ID>`. This command fires up an interface similar to
git add -p, which interactively presented me with hunks out of my working
directory to choose from. I found the one I wanted, selected it, then dismissed
the interactive thingy with a quick keystroke. And it was done!

There are some hidden details in this story that I want to draw your attention
to. When I edited this earlier commit, I was in the middle of working on
something else and I *hadn't committed or even staged it*. I did not run git
stash, nor git commit -m"WIP", nor git add, nor git checkout, nor git rebase, at
any point. The only command I ran was jj squash.[^log] When it was done, I was
returned immediately to where I left off, with a half-written, uncommitted
change in my workdir. It took all of two seconds to complete this operation and
pick up where I left off.

[^log]: A white lie: I also ran jj log to remind myself of the change ID that I wanted to edit.

The "wow" moment came when I realized that I had done this several times that
day without finding it particularly remarkable. Jujutsu makes editing history
absolutely effortless.

Before I add any further breathless praise for jj, I will note three criticisms.

First, jj lacks any first-class support for the [git send-email][gse] workflow
that I depend on for almost all of the projects I work on. Second, jj lacks a
"jj grep" command, and the [recommended workaround][workaround] is Not Good™. I
work around both problems by using jj with a co-located git repo at all times,
which causes jj and git to share the same repository in the same directory and
allows for either git(1) or jj(1) to be used as the need demands.

[gse]: https://git-send-email.io
[workaround]: https://martinvonz.github.io/jj/latest/git-comparison/#command-equivalence-table

I would have contributed patches to address these shortcomings if it were not
for my third criticism, which addresses the elephant in the room: Jujutsu is a
Google employee's "20% project", and thus all contributors are required to sign
the Google <abbr title="contributor license agreement">CLA</abbr> to
participate. I refuse to sign any such thing and [so should you][cla]. I have
[raised the issue on GitHub][cla discussion] but it hasn't attracted any sort of
official response. This stiffly limits my enthusiasm for the project and any
kind of collaboration. I would be very excited to work on Jujutsu, and in
particular explore some very interesting possibilities regarding integrations
with SourceHut and email generally, if it weren't for this problem.

[cla]: https://drewdevault.com/2023/07/04/Dont-sign-a-CLA-2.html
[cla discussion]: https://github.com/martinvonz/jj/discussions/4849

Nevertheless, I have adopted jj as my daily driver for private use, and if and
when the need arises I will maintain [some personal patches][my tree] until the
Google problem goes away. Feel free to email me your own patches if you want to
share them around but don't want to sign the CLA, either.

[my tree]: https://git.sr.ht/~sircmpwn/jj
