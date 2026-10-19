---
title: Ending the Casuarina Linux Experiment
link: https://casuarina.org/news/ending-the-casuarina-linux-experiment/
source: lobste-rs
published: 2026-10-08T13:08:00Z
updated: 2026-10-08T13:08:00Z
first_seen: 2026-10-09T23:10:41.116203336Z
authors:
- casuarina.org via FedericoSchonborn
labels:
- linux
summary: Comments
content: extracted
html: 2026-10-08-ending-the-casuarina-linux-experiment.html
---

by [Wesley Moore](https://www.wezm.net/)

I didn’t expect this to be the second post after the announcement, but here we are. The short version is that I’ve decided to wind down the Casuarina Linux project. Things will continue as-is until the end of October 2026. After which, depending how my migration to another distro has gone, package updates will stop. The infrastructure will remain up until the end of 2026, after which I may retire some things. There are no plans to retire the package repo or website for now.

## So how did we get here?

As alluded to [in the Q&A](https://casuarina.org/faq/#are-you-qualified-to-build-a-distro) I am [the proverbial dog](https://knowyourmeme.com/memes/i-have-no-idea-what-im-doing) that has no idea what I’m doing. I thought that after I fought through all the changes to bootstrap the system most of the hard stuff was done, and that from then on maintenance would mostly be updating packages. I also expected that the distro would pique the interest of at least a couple of other people and we could share the load of keeping things moving, but that didn’t happen.

In practice what happened was that on launch day it was brought to my attention that C++ standard libraries can’t co-exist. My setup of building everything against LLVM’s `libc++`, whilst providing GNU `libstdc++` for compatibility was not guaranteed to work properly if an application ended up loading both. In practice the one proprietary application I used that was exposed to this (Beyond Compare) worked fine. Still it was something that needed fixing.

Additionally, on the same day q66 (the creator of Chimera Linux) [posted some thoughts “about chimera and glibc compatibility woes”](https://gts.q66.moe/@q66/statuses/01KS041CQA3H3CTZSA3TA9AK8T). It was insightful and full of wisdom. There were two parts that really stuck with me:

> OR, you can just accept things as they are and help work towards making the stuff you want work
>
> in userland, you have containers etc which you can use to get stuff that doesn’t work yet to work (or for proprietary software), and there are ways to make that pretty seamless

> i’d much rather like to see the work go into improving what we have

and:

> but i can’t be totally happy about it because it’s sort of taking apart stuff i put a lot of thought into and in the process turns it into something i explicitly wanted to avoid

The latter in particular was not a way I’d thought about it from their perspective before. It made a lot of sense when faced with having to swap `libc++` for `libstdc++`, and when reflecting on the changes that were necessary to bootstrap the system. Such as bringing `gcc`, `gmp`, `mpc`, `mpfr`, and GNU `binutils` into the bootstrap path (in addition to LLVM). There were also things that were lost, like cross-compilation and fully static binaries (`apk` in particular).

All of this continued to rattle around in the back of my mind while I’ve used the system to do my day job, and stayed on top of pulling package updates from Chimera over the last four months. In that time I did not muster the motivation to address the C++ standard library situation though, nor some other issues that I ran into.

Also as time went on I became less convinced I actually wanted other people to use the system. Each additional user is another person that may bring [legit problems](https://forum.casuarina.org/viewtopic.php?t=7) to my attention that I haven’t worked out how to fix, or don’t really want to allocate time to when I have free time.

In hindsight some of this was 100% foreseeable. Why did I even make the thing public if I didn’t want people to use it? I guess I hadn’t fully thought it through. It was working great for me, surely it would be the same for others. Obviously this was naïve, which I know now.

## Ending the experiment

From the start Casuarina was described as “experimental but usable”, so I am now declaring the experiment over. I increased my understanding of all sorts of things about how a distro is built, how it works, some of pros and cons of glibc, debugging build failures, segfaults, Buildbot, and much more. I also had a lot of fun building the website. While I gained a lot of knowledge it’s clear I still have plenty to learn, and don’t feel equipped to be the primary maintainer of a distro that other people use.

So for now this is the end of the road for Casuarina. My current plan is to migrate my primary desktop (that I use for work as well personal computing) to Chimera, and switch to that from the end of October 2026. From then I’ll stop doing Casuarina package updates. I’ll make the forum read-only, and update the website to make it clear the experiment is over. The rest of the infrastructure will stay up until the end of 2026, after which some of it may be retired/repurposed. I plan to keep the package repo and website online for the foreseeable future.

Happy computing.

*WM*
