---
title: Synaptics patent claims stop work on DisplayLink drivers
link: https://lwn.net/SubscriberLink/1102549/acbc92e00cee3a3f/
source: hnrss-org
published: 2026-11-10T18:02:02Z
updated: 2026-11-10T18:02:02Z
first_seen: 2026-11-11T00:34:12.222309031Z
authors:
- speckx
content: extracted
html: 2026-11-10-synaptics-patent-claims-stop-work-on-displaylink-drivers.html
remote_preview:
  url: https://static.lwn.net/images/logo/barepenguin-70.webp
  alt: LWN.net Logo
---

> ### Welcome to LWN.net
>
> The following subscription-only content has been made available to you by an LWN subscriber. Thousands of subscribers depend on LWN for the best news from the Linux and free software communities. If you enjoy this article, please consider [subscribing to LWN](https://lwn.net/subscribe/). Thank you for visiting LWN.net!

Mike Lothian owns a docking station (or dock) that uses a [DisplayLink](https://en.wikipedia.org/wiki/DisplayLink) chipset that had, at least in his opinion, subpar performance on Linux. He decided to try improving the Linux driver, with LLM assistance, but ran into one problem that he could not solve—so he asked for help, hoping that Synaptics's developers would lend a hand. (DisplayLink was [acquired by Synaptics](https://www.synaptics.com/company/news/displaylink) in 2022.) Rather than help him in improving the driver, however, the Synaptics legal team responded with patent infringement claims, a [demand](https://lists.freedesktop.org/archives/dri-devel/attachments/20261006/d690bb16/attachment-0001.pdf) that he cease unauthorized implementation of DisplayLink technology, and threats of expensive legal penalties. For now, it appears that Linux users of the company's docks will have to continue to live without feature parity with other platforms.

#### Synaptics and Displaylink

The dock that he was trying to support is a Dell device that connects to a system via USB 3 and allows users to connect multiple monitors and other devices to their computer. It provides its [DisplayLink Manager (DLM) software for Linux](https://www.synaptics.com/products/displaylink-graphics/downloads/ubuntu) for the devices via an APT repository for Ubuntu.

The company only provides DLM software in binary form, and only for x86-based platforms. The software is proprietary, but the company does maintain an open-source, out-of-tree kernel module that is included with DLM, the [Extensible Virtual Display Interface](https://github.com/DisplayLink/evdi#readme) (EVDI) project. It provides a driver that's compatible with [Linux DRM](https://docs.kernel.org/gpu/introduction.html) and allows users to manage their docked displays with standard tools used for display settings, such as [`xrandr`](https://linux.die.net/man/1/xrandr) or GNOME and KDE Plasma's utilities.

A [short article](https://support.displaylink.com/knowledgebase/articles/1104056-why-has-displaylink-not-released-source-code-for-t) explains why the company has not released source code for DLM: the software uses a proprietary graphics technology called DL3 to create a compressed stream of the various channels (e.g., USB, DisplayLink video, audio) over a communication channel (USB 3 in this case) and decode it again for output to the user's devices.

Unfortunately, the vendor-supplied software did not work well for Lothian. I spoke and chatted with him via Signal while writing this article; he said that the driver kept crashing under Linux, so he decided to use his Claude subscription to try to develop a new driver and support software.

#### Vino

He posted the [first patch series](https://lwn.net/ml/all/20260617151249.2937-1-mike%40fireburn.co.uk/) for Vino—his Rust-based DRM/KMS driver—to the kernel's dri-devel list as an RFC on June 17. Lothian noted that it was in an incomplete state, and asked for help with a blocker that he had run into trying to communicate with the dock's control plane. "If you have knowledge of the DisplayLink DL3 control-plane engagement sequence, or ideas for isolating a whole-bus timing/ordering property a per-channel diff cannot see, help would be very welcome." He received feedback from a few developers, including [guidance](https://lwn.net/ml/all/DJBG3H7N9GS9.7AMMRJIHGHRN@kernel.org/) from Danilo Krummrich to rework the driver to use the Rust [KMS bindings](https://lwn.net/Articles/997850/#kms), but no advice specific to the problem he was trying to solve.

Lothian also opened [an issue](https://github.com/DisplayLink/evdi/issues/577) on the EVDI GitHub repository on June 17. It explained the work he'd done so far, outlined the approach in trying to reverse-engineer the DL3 protocol, provided links to his GitHub repository with the Vino code, and asked for clarification on a few issues. There was no reply to his issue.

He also reached out to Łukasz Spintzyk of Synaptics directly by email; his correspondence with the company between July 5 and October 7 is available in [this PDF compiled by Lothian](https://lwn.net/images/pdf/synaptics-correspondence-jul-oct-2026.pdf). He noted that he had found a few bugs in EVDI and opened a [pull request](https://github.com/DisplayLink/evdi/pull/581) (that went unreviewed for several months), and also asked for assistance where he was blocked on Vino. On July 20, he received a reply, not from Spintzyk, but from Michal Lukaszek of Synaptics:

> The reason I'm writing is that DisplayLink is not an open standard. The protocol, codec, and other technologies used by DisplayLink devices are proprietary to Synaptics and are protected by a substantial portfolio of patents in a number of jurisdictions, including the UK.
>
> From what you've published, it appears that the VINO project is progressing beyond interoperability work and toward implementing technology that we believe is covered by those patents. While many developers understandably focus on avoiding copyrighted source code, patent law is different. An independently written or clean-room implementation does not necessarily avoid patent infringement if it implements patented inventions.

Lukaszek said it was entirely possible that Lothian was unaware of the "intellectual property landscape surrounding DisplayLink", so he wanted to make him aware now rather than "have the situation become more formal later". Lothian lives in Scotland and has a ".co.uk" email address, which explains why the email made reference to the UK.

#### Cease and desist

On August 17, Andrew Bateman, an assistant general council of intellectual property for Synaptics, sent a [cease-and-desist notice](https://lwn.net/images/pdf/Cease_and_Desist_Notice_Mike_Lothian_08-17-2026.pdf). The letter stated that Lothian had not "substantially responded" to Lukaszek's communication, and had continued with "public statements and further development activities". It included an appendix with a list of 125 patents that "cover fundamental aspects of DisplayLink's architecture and operation", but did not specify which patents might be infringed by the work.

Synaptics included a list of eight demands it expected immediate compliance with, including ceasing development of "any software or other implementation that infringes any Synaptics patent", withdrawing submissions of materials "made to Linux kernel maintainers, mailing lists, code-hosting platforms, package maintainers, distributions, or any other person or organization", and to identify in writing every location "in which the relevant materials have been published, submitted, stored, or distributed". It also demanded that he provide Synaptics with a signed confirmation, by August 28, that he had complied with its demands.

Lothian, instead, posted [version three of his Vino](https://lwn.net/ml/all/20260826163913.7052-1-mike@fireburn.co.uk/) work for the DisplayLink docks to the dri-devel list on August 26. He said that the previous two versions had "never lit a panel", but the new work could control three generations of DL3 docks without any vendor software. There were no substantive reviews of the patch set posted to the list, though there were some automated reviews from Sashiko.

On August 27, Lothian responded to Bateman and asked him to elaborate on the patents that Synaptics thought were being infringed. "A claim chart mapping the relevant claims to the relevant code would make it much easier for me to understand and properly respond to your concerns."

He also expressed confusion about Synaptics's position that his work was being treated as patent infringement. Synaptics is a member of the [Open Invention Network](https://openinventionnetwork.com/) (OIN), which was [created](https://lwn.net/Articles/163038/) to help open-source developers and vendors deal with the threats of expensive patent litigation. It holds a large, cross-licensed portfolio that serves to provide patent protection to individuals, open-source projects, and large vendors alike. Lothian said that he was now an OIN licensee as well, and he thought that it was supposed to "reduce exactly this sort of patent problem around Linux and open source software".

He asked Bateman to explain how Synaptics claims "interact with our respective OIN commitments", and expressed a desire to sort things out with Synaptics constructively. "I don't think a public dispute between Synaptics and an individual open source developer would be particularly helpful for either of us, especially within the Linux and open source communities".

Lothian did not think he could "reasonably provide the signed undertaking" Synaptics had demanded by August 28 without understanding which patent claims it was alleging. He asked for an extension to the deadline. There was no response from Bateman, so Lothian sent a follow-up email on September 18. He said he was concerned there was no acknowledgment to his last email, and wondered it was safe to assume he could continue his work. He had been running the Vino driver himself for quite a while, and was eager to move it upstream without having patent threats hanging over the work.

Bateman responded the next day and said that he should be concerned:

> As you previously requested, we have been documenting your ongoing patent infringement by mapping your Vino driver to specific claims of our DisplayLink patents. I will provide this claim mapping documentation to you within the next two weeks. I will also be sending to you our demand for substantial monetary compensation to recoup the cost and effort required to create such documentation of your infringing acts. I previously gave you the opportunity to stop your infringement, but you decided to push back and not heed my warnings, and so now we are escalating as we deem appropriate.

The letter continued to claim that Lothian would be "single-handedly eviscerating DisplayLink's competitive advantage in this crowded market by educating our competitors and other third parties as to how we implement our proprietary technology". He said that DisplayLink is not a member of OIN, and none of its technology is licensable through OIN or any other organization.

Bateman sent a [follow-up letter](https://lwn.net/images/pdf/Cease_and_Desist_Notice_Mike_Lothian_10-05-2026.pdf), dated October 5, that included two claim charts for UK patents: [GB2484736B](https://lwn.net/images/pdf/GB2484736.pdf) ("Display control device") and [GB2485613B](https://lwn.net/images/pdf/GB2485613.pdf) ("Image generation"), both filed in 2010 and published in 2012. He reiterated Synaptics's demands from its previous letter, and said that the deadline was now October 11; failure to comply would result in the company seeking "monetary compensation from you in the amount of US$25,000 to recoup the costs and effort required to create the attached documentation of your infringing acts". That amount would be increased by $25,000 each week until the company's demands were complied with.

Lothian responded the next day, by email, to say that he was on vacation, and would not be back home until October 13. He asked for an extension of the deadline to return home and go through the claim charts "ideally with someone qualified to help me read them". Bateman [replied](https://lwn.net/ml/all/MW4PR03MB6540F02E3810CC632C82302598952%40MW4PR03MB6540.namprd03.prod.outlook.com/) that Synaptics would agree to extend the deadline to October 18, though no further extensions would be granted. He reiterated that "DisplayLink is not a member of OIN" and none of its technology was licensable through OIN.

#### OIN to the rescue?

Synaptics *is* a member of OIN, and is listed in the [community directory](https://openinventionnetwork.com/community-directory-alphabetical/). In addition to joining OIN, Lothian had reached out to the organization to try to solicit its help and has been in communication with some of the staff there. OIN indicated that it would look into the matter and see if he had any protection as an OIN member. That process took longer than Lothian hoped, though OIN did eventually provide its conclusion.

OIN's portfolio covers a wide range of software that matches its [Linux System Definition](https://openinventionnetwork.com/linux-system/), but it does not cover everything that might be implemented for a Linux system. In particular, its definition specifies that codecs ("a component of a program capable of performing transformations of a video, still image or audio data stream or signal") are not covered unless specifically mentioned in [this list](https://definition.openinventionnetwork.com/table-0/) of components.

I spoke briefly to OIN CEO Keith Bergelt on October 23, shortly after OIN staff informed Lothian of the result of their research. Bergelt said that OIN's conclusion was that, since codecs are explicitly not covered by OIN's Linux System Definition, its protections did not extend to Lothian's attempted kernel contributions. OIN had recommended that Lothian reach out to a lawyer in the UK, and provided a recommendation.

#### Status

Unfortunately, that referral did not pan out, Lothian said. He has not given up entirely, but he has taken down the GitHub repository to comply with Synaptics's demands. For now, he may continue working on some of components that are not affected by the patent claims, but the work on Vino will not be submitted to the kernel. "Vino itself has to remain local until this is sorted out, I just can't afford to defend myself against Synaptics."

Perhaps amusingly, Lothian told me that someone from Synaptics [finally replied](https://github.com/DisplayLink/evdi/pull/581#pullrequestreview-5439551411) to the pull request on October 7. Lothian told me he does not intend to follow up on the comments: "You can't threaten to sue me and then say 'can you update your pull request?'"

For the time being, Synaptics has succeeded in shutting down an effort to improve the way DisplayLink docks work with Linux. It is an unpleasant reminder that, while much of the current conversations in open-source communities are concerned with LLM-driven development, software patents still pose problems for free-software developers and users.
