---
title: A plea not to use uv-build
link: https://blogs.gentoo.org/mgorny/2026/10/12/a-plea-not-to-use-uv-build/
source: lobste-rs
published: 2026-10-13T14:36:52Z
updated: 2026-10-13T14:36:52Z
first_seen: 2026-10-14T01:50:10.887121900Z
authors:
- blogs.gentoo.org via lim
labels:
- devops
- gentoo
- python
summary: Comments
content: extracted
html: 2026-10-13-a-plea-not-to-use-uv-build.html
---

After seeing yet another dozen packages lose keywords on Gentoo because of a single key dependency switching to [uv-build](https://pypi.org/project/uv-build/), I would like to plea on behalf of Gentoo users: please don’t use uv-build as a build backend. Just to be clear, this is not about [uv](https://github.com/astral-sh/uv) itself or any other Astral tools; these are good tools with a nice and responsive upstream. The uv-build backend is one thing we have to disagree on. I’m not saying it’s bad; I’m saying that its benefits don’t outweigh the drawbacks.

What are the drawbacks? It’s really big and it’s written in Rust. Rust has many advantages, but none of them really matter for a build backend that is used to build pure Python packages. It’s a huge dependency (as of 0.13.0, its wheels are between 1.2 MiB and 1.5 MiB, depending on the platform) and it has no benefit over using a small pure Python backend (say, [flit-core](https://pypi.org/project/flit-core/) whose wheel is 45 KiB). It means that all Gentoo users need to build it from source, and given that it’s released quite frequently (12 releases in the 30 days prior to writing this post), that’s a lot of building. Like, for every 100 ms spent running uv-build to install one release of your package, Gentoo users will spend many hours building new uv-build versions over and over again.

However, build times aren’t the biggest problem; portability is. You can run Python pretty much anywhere where you can get a working C compiler. Some minor features may not work, but the core is reasonably portable. [The list of platforms supported by Rust](https://doc.rust-lang.org/nightly/rustc/platform-support.html) is growing slowly, and even if you can get a working Rust compiler on a given platform, uv may not work. The problems can range from bugs in the lot of packages that uv depends on (for example, [rustix::fs and its dependencies skipping SPARC, for no apparent reason](https://github.com/bytecodealliance/rustix/issues/1700)), to the resulting executable being so big that the toolchain runs out of address space (and unlike system memory, you can’t really add address space to a system; the Gentoo [uv ebuild](https://gitweb.gentoo.org/repo/gentoo.git/tree/dev-python/uv/uv-9999.ebuild?id=a18e010bf93fcc1dcba9285f72c07890650508d7) already carries two hacks to make it possible to build uv on 32-bit platforms). And then, if your platform is not supported at all… well, let’s just say that adding and maintaining a new platform is a full-time job that may involve a lot of swearing at the code churn.

What about the advantages? Well, I’m aware of two reasons to use uv-build. One is consistency with using uv for your project, and it doesn’t really matter. It’s a backend and the whole point of [PEP 517](https://peps.python.org/pep-0517/) is that you can use any backend with any tool. You can use flit-core, you can use [hatchling](https://pypi.org/project/hatchling/), and either of them will work just fine with uv (or any other frontend). The other reason is performance and that argument makes even less sense. Even if it mattered, we’re realistically talking about a difference in the ballpark of 100 ms, for an action executed rarely; it is equally possible that any gain will be outweighed by the time needed to fetch the 1.5 MiB wheel for uv-build. But it doesn’t matter, because realistically the backend is only used by project developers, contributors, CI/CD workflows, downstream packagers and users of source distributions; all regular users use wheels instead. Of these groups, source distribution user group is the largest and the most impacted, and any performance gain is defeated by the time spent building uv-build.
