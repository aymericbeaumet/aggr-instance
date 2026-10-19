---
title: Last rites for Gentoo's Chromium package
link: https://lwn.net/SubscriberLink/1097760/2be4d9e3eeb59039/
source: lobste-rs
published: 2026-10-06T17:46:53Z
updated: 2026-10-06T17:46:53Z
first_seen: 2026-10-07T19:30:21.779631669Z
authors:
- lwn.net via calvin
labels:
- browsers
- linux
- practices
summary: Comments
content: extracted
html: 2026-10-06-last-rites-for-gentoo-s-chromium-package.html
---

> **Did you know...?**
>
> LWN.net is a subscriber-supported publication; we rely on subscribers to keep the entire operation going. Please help out by [buying a subscription](https://lwn.net/Promo/nst-nag4/subscribe) and keeping LWN on the net.

[Chromium](https://www.chromium.org/Home/), the open-source upstream project for Google's Chrome web browser, is the browser of choice for many Linux users. It has also gained a reputation as being [difficult for Linux distributions to package and build](https://lwn.net/Articles/619158/): Chromium has a complex build system, the project bundles many of its dependencies, and it has frequent releases. All of that, plus user complaints, has led the maintainers of the [Gentoo Chromium package](https://packages.gentoo.org/packages/www-client/chromium) to give up on trying to maintain the package.

Gentoo focuses on allowing users to build their own software from source using the [Portage](https://wiki.gentoo.org/wiki/Handbook:X86/Working/Portage) software-management tool. Gentoo packagers create [ebuild](https://wiki.gentoo.org/wiki/Ebuild) files, which are text files written in a Bash-like syntax that contain the information needed by Portage to build the software. Users then build packages themselves using [`emerge`](https://wiki.gentoo.org/wiki/Emerge), which is the command-line interface for Portage. [A recent ebuild for Chromium](https://github.com/gentoo-mirror/gentoo/blob/stable/www-client/chromium/chromium-151.0.7922.169-r1.ebuild) illustrates the complexity of the package.

Gentoo does offer prebuilt binary packages for some software via its [Binhost](https://wiki.gentoo.org/wiki/Project:Binhost) project. However, building from source gives users the flexibility to declare [`USE` flags](https://wiki.gentoo.org/wiki/USE_flag) to set compile-time options or change a package's configuration. For example, a user may wish to specify which optional libraries are linked with a package, or whether to install the accompanying documentation. With Chromium in particular, a user might employ the `-bundled-toolchain` `USE` flag (which tells Portage *not* to use the bundled toolchain) in order to use their system's version of Clang to compile Chromium rather than the [bundled version included by the upstream project](https://chromium.googlesource.com/chromium/src/+/90.0.4430.72/docs/clang_sheriffing.md). That is optional for Gentoo users on x86-64 systems, but `-bundled-toolchain` is required for users on other architectures, since Chromium's bundled toolchain is only shipped for x86-64.

It is worth noting that Chromium is currently not available as a binary package from Gentoo due to problems with building the package with proprietary codecs.

#### Last rites

Many distributions, including Arch Linux, Debian, and Fedora, use the term "orphan" to refer to packages that have been abandoned by their maintainer; the developer announces that a package has been orphaned, and other contributors have the opportunity to claim maintainership of it if they wish to do so. Gentoo's terminology is a bit more dramatic: a package maintainer announces "last rites" for a package on the gentoo-dev-announce mailing list to notify the project that it will be unmaintained, and as an opportunity to allow other developers to claim maintainership.

After a developer has declared last rites, the package's ebuild is [masked](https://wiki.gentoo.org/wiki/Knowledge_Base:Masking_a_package) in [Gentoo's ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository) to ensure that users will not accidentally install a package that is no longer being maintained. Users who have the package installed and attempt an upgrade will receive a warning that it has been masked. Chromium package maintainer Sam James [declared last rites for the Chromium package](https://lwn.net/ml/all/87ld8rb0p1.fsf%40gentoo.org/) on September 24, after a lengthy conversation in a [bug report filed by Roman Žilka](https://bugs.gentoo.org/982204).

On September 9, Žilka [had complained](https://bugs.gentoo.org/982204#c0) that the latest stable version of the Chromium ebuild was outdated:

> The latest stable www-client/chromium (and google-chrome and possibly others) is 151.0.7922.169, which contains a catastrophic 602 known vulnerabilities of all severities. Measures need to be taken by somebody qualified that this doesn't keep happening in packages as important as these.

He had attached [an ebuild for a newer version of Chromium](https://982204.bugs.gentoo.org/attachment.cgi?id=970220) to the bug report, but noted that it would not work if a user tried to compile Chromium with the `-bundled-toolchain` `USE` flag.

James [replied](https://bugs.gentoo.org/982204#c5), "Chromium is horrible to maintain as a source package. It is easily the package in Gentoo that burns through maintainers the most." Complaining about its maintenance isn't helpful, he said, and added that the maintainers have come close to giving Chromium last rites several times over the years due to the difficulty in keeping it up to date. He also noted that he had mentioned this to Žilka before, and said that it would be welcome if he wanted to assist with the package.

Indeed, Žilka had [filed a similar bug](https://bugs.gentoo.org/980389) in August complaining about the number of vulnerabilities present in the Chromium ebuild; James had [replied then](https://bugs.gentoo.org/980389#c1) that the package had always been difficult to maintain, "and now they've added Go" as a dependency as well. Prior to that, Žilka had shown up in a [bug report filed by Matt Jolly about Chromium vulnerabilities](https://bugs.gentoo.org/show_bug.cgi?id=CVE-2025-14372) to [complain](https://bugs.gentoo.org/show_bug.cgi?id=CVE-2025-14372#c4) that "the update process needs fixing, so that events like these don't happen again in packages as critical as these!"

Jolly had [replied](https://bugs.gentoo.org/show_bug.cgi?id=CVE-2025-14372#c7) that the maintainers are a team of volunteers who do their best, but "build failures, human factors ('I messed up assigning the bug and automation didn't pick it up'), and my own limited free time are the biggest causes of delays". The best way to change the situation, he said, would be for Žilka to volunteer his help.

#### Trial and error

There are various third-party repositories, called [overlays](https://wiki.gentoo.org/wiki/Project:Overlays), that provide additional and alternative ebuilds for Gentoo. On September 11, Nick Reale [said](https://bugs.gentoo.org/982204#c11) that he had tried ebuilds for a newer version of Chromium from the [Bentoo](https://obentoo.org/v/v3e/) and [Fireburn](https://github.com/FireBurn/Overlay) overlays, but he couldn't get either one to work. A few days later he [reported success](https://bugs.gentoo.org/982204#c16) with Bentoo's ebuild and a specific set of `USE` flags. There was some follow-on discussion from Reale and François Valenduc about various incantations they were able to use to compile an updated version of Chromium on Gentoo from other ebuilds. On September 18, Valenduc [commented](https://bugs.gentoo.org/982204#c20) that version 153.0.8010.47 of Chromium was out, and that version was already available in Arch Linux, openSUSE, SUSE, and Ubuntu. "We are still waiting for Gentoo despite all the proposals made here."

James [replied](https://bugs.gentoo.org/982204#c21) that none of the maintainers have had a chance to look into the problems with the Chromium package yet. He said that the patches and ebuilds that had been submitted with the bug report were "useful and appreciated, but it's not as simple as just grabbing it and committing it". Users would complain, he said, if the maintainers had not tested the ebuilds and it broke on their systems.

> It'll get done when it's done, but it's more likely to be last-rited if nobody is interested in handling it in the way these things have to be done (which goes beyond just attaching ebuilds).

Valenduc [said](https://bugs.gentoo.org/982204#c22) that he was not able to maintain the package, "but taking the ebuild from the bentoo overlay was not that difficult \[...\] It's just frustrating to see no updates in Gentoo." James [replied](https://bugs.gentoo.org/982204#c23) that just because the ebuild worked for Valenduc did not mean it had all `USE` flags working. He added that no one had submitted a patch to the Gentoo ebuild, which meant that the maintainers would need to reverse-engineer the changes by diffing the ebuilds and work out what had been modified. "All of that is a non-trivial amount of work and while having some ebuild to start with is useful for that, it doesn't mean one can just git add && git commit && git push it." Valenduc [acknowledged](https://bugs.gentoo.org/982204#c24) that he could not test all possible `USE` flags, since it took between 15 and 16 hours to compile Chromium on his laptop.

On September 24, Žilka [commented](https://bugs.gentoo.org/982204#c30): "This is ridiculous". A few minutes later, James [replied](https://bugs.gentoo.org/982204#c31) "OK then", and [issued the last rites](https://bugs.gentoo.org/982204#c32) for the package.

#### Can we keep it?

Maciej S. Szmigiero [asked](https://bugs.gentoo.org/982204#c35) if it would be possible to keep the ebuild in Gentoo's repository and just declare that it was maintained at "best effort" level. "Removing buildable/patchable Chromium is a huge loss for a Gentoo desktop." James [said](https://bugs.gentoo.org/982204#c36) that he did not think that was possible. He had already explained that Chromium was being maintained by volunteers, and that explanation had not been accepted.

People were free to maintain the ebuild in another repository, he said, but that meant they would have to do the work themselves or accept someone doing it to a lower standard. He pointed out that the ebuilds from the Bentoo overlay had been cited in the discussion, but creation of those ebuilds seemed to be "heavily automated" and had missed some fixes in the Gentoo version. "Copying ebuilds works fine until it doesn't."

James added that someone could still pick up the Chromium package before the end of the last-rites period on October 24, or that the package could be restored in the future. He did not want to see that, though, unless there was a plan for sustainable maintenance. "Chromium has a history of burning out maintainers in Gentoo because of the high rate of churn upstream, though some of the contents of this bug don't help motivation at all either."

Matt Whitlock [wondered](https://bugs.gentoo.org/982204#c42) if the developer-burnout factor could be reduced by eliminating most of the `USE` flags for Chromium, or if the maintainers stopped unbundling third-party libraries.

> Basically, what I am asking is: can we give up \_all\_ of the flexibility to which we have grown accustomed in www-client/chromium, in exchange for a package that is actually maintainable but that is still built from sources by a toolchain that is not distributed as opaque binaries?

He [added later](https://bugs.gentoo.org/982204#c46) that part of the problem was that the upstream project developers "do not test their builds with any of their allegedly 'optional' features disabled". That left the responsibility with the downstream package maintainers. "In the case of Chromium, keeping all the toggle switches working (despite chronic neglect from upstream) is a monumental, nigh-insurmountable task."

#### Complexity

That was a good question, James [said](https://bugs.gentoo.org/982204#c47), but part of the recent complexity with Chromium was the addition of Rust and Go dependencies. The packagers had to do some wrangling to ensure that the Go build did not require network access, which is forbidden with Gentoo ebuilds, but that was not the only problem. He said that Google developers seem to keep using experimental Rust features that break on new releases as well. On top of that, "yet another problem is that upstream's CI to produce release tarballs isn't reliable". One of the Gentoo Chromium maintainers had sent patches to fix that, to little avail. "It took ages to get \[the patches\] reviewed, and it's broken again a few times since. They didn't seem to notice or care that it either wasn't producing tarballs or was producing incomplete ones."

Jolly [added some of his observations](https://bugs.gentoo.org/982204#c53) about problems with maintaining Chromium for Gentoo. He said that maintaining Chromium can take more than six hours each week, and that was counting only compilation time on a fast PC with "an absurd amount of RAM". Compilation time is even longer for 64-bit Arm machines.

> It's a commitment to volunteer at least a business day's worth of your free time each week when you include rebasing patches, cherry-picking fixes, iterating on \`ebuild ... clean compile\`, plus trying to identify and fix any new bugs, working out how much you violate Gentoo policy by not unbundling more of the browser, spending time on automation to save time and mistakes later, etc. There's immense pressure to keep builds up-to-date, especially in the age of hundreds of weekly browser CVEs.

He included a "pre-emptive FAQ" in his reply about the last rites for Chromium, and what it would require to restore the package. He said that there were no insurmountable technical issues to maintaining Chromium, but it would need multiple maintainers with a broad set of skills—including familiarity with C, C++, Go, JavaScript, and Rust. "There are also a huge number of NIH \[not invented here\] Google tools that you'll \[have to\] develop skills for and never be able to use anywhere else - you have been warned."

Using Google's toolchain would be simpler, but it would also restrict the ebuild to x86-64, he said. "We'd lose arm64, ppc64, and the RISC-V overlay - better to just rip the band-aid off now as it doesn't materially make the situation better". He added that he would be willing to review commits and offer advice to anyone who wanted to maintain the source ebuild, but he felt that at least three people would be needed. "History has shown that one maintainer isn't working, and we've had the release cycle shorten twice since I started maintaining the package in 2023; it was bad then, and it's worse now."

The [release cadence](https://chromiumdash.appspot.com/releases?platform=Linux) for Chromium is aggressive, to say the least. Chromium has multiple channels including stable, beta, and development. There is a new major branch of the browser roughly every two weeks via the stable channel, and a new version is shipped every week.

#### Getting Chromium

For now, it appears that Gentoo users will need to search elsewhere for a suitable Chromium ebuild, choose a different browser, or find a different distribution method to acquire Chromium. For example, Gentoo users could install the [Flatpak from Flathub](https://flathub.org/en/apps/org.chromium.Chromium), but it is an unverified package built by a third party—not by the Chromium upstream project. The Flatpak is also of no help for users who are not on x86-64 or 64-bit Arm systems.

It is technically possible for Chromium users to build and compile the browser on their own using the upstream's instructions, but it does not appear that the Chromium project cares a great deal about making that easy to do. It does have a [page on getting the code](https://www.chromium.org/developers/how-tos/get-the-code/) but the links to instructions for Linux (as well as macOS and Windows) return a 503 error. Searching for instructions turns up [this page](https://chromium.googlesource.com/chromium/src/+/main/docs/linux/build_instructions.md), though it is undated so it is unclear if it is the most up-to-date. Assuming the instructions are current and correct, building Chromium using those directions is quite an ordeal. Users can [download pre-built binaries](https://www.chromium.org/getting-involved/download-chromium/) from the project, but only for x86-64 systems.

Other distributions continue to supply Chromium packages, of course, though their approach does not provide the same flexibility as Gentoo's. Fedora's [package](https://packages.fedoraproject.org/pkgs/chromium/chromium/) (which is described as "A WebKit (Blink) powered web browser that Google doesn't want you to use") is at version 154.0.8037.92—one version behind the upstream (154.0.8037.97) release that was published on October 1. Likewise, the package in Debian's security repository is the same version (currently) as Fedora's and the [information page](https://security-tracker.debian.org/tracker/source-package/chromium) shows that there are at least 11 known CVEs addressed in 154.0.8037.97. Keeping up with Chromium is not an easy task.

Perhaps Gentoo will find a team that is willing and able to take on the burden of maintaining the Chromium ebuild up to the standards that Gentoo users expect. Whatever happens, it seems unlikely that Chromium will become any easier to maintain in the interim.
