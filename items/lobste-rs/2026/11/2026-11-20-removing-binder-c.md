---
title: Removing binder.c
link: https://lwn.net/Articles/1102982/
source: lobste-rs
published: 2026-11-20T10:33:36Z
updated: 2026-11-20T10:33:36Z
first_seen: 2026-11-20T22:37:07.598874728Z
authors:
- lwn.net via fanf
labels:
- android
- linux
- rust
summary: Comments
content: extracted
html: 2026-11-20-removing-binder-c.html
remote_preview:
  url: https://static.lwn.net/images/2026/carlos-llamas-kangrejos-small.png
  alt: '[Carlos Llamas]'
---

> **Ready to give LWN a try?**
>
> With a subscription to LWN, you can stay current with what is happening in the Linux and free-software community and take advantage of subscriber-only site features. We are pleased to offer you **[a free trial subscription](https://lwn.net/Promo/nst-trial1/claim)**, no credit card required, so that you can see for yourself. Please, join us!

The final talk at Kangrejos 2026 was about the removal of the C implementation of Android's [Binder driver](https://source.android.com/docs/core/architecture/ipc/binder-overview), the primary interprocess communication mechanism used on Android devices. Carlos Llamas announced that he had deleted the C code to general applause, and then shared some insights about what went into replacing the C implementation. Alice Ryhl, who has worked on the Rust implementation for some years, added her own thoughts about what the most difficult and interesting parts of the transition were.

Binder was added to the kernel in 2008, Llamas said, as an interprocess communication mechanism for Android devices. Its design originated in [BeOS](https://en.wikipedia.org/wiki/BeOS), but it has grown significantly since then. It provides a rich set of operations for allowing processes to communicate with the minimum amount of overhead. It also, therefore, presents a large attack surface for Android. Many complicated chained attacks against Android have involved Binder at one step or another, Llamas explained. Google's internal "red team" (the team dedicated to proactive security research) deemed Binder to be the most vulnerability-dense component in the entire operating system, with more CVEs per line than anywhere else.

[![[Carlos Llamas]](https://static.lwn.net/images/2026/carlos-llamas-kangrejos-small.png "Carlos Llamas")](https://lwn.net/Articles/1103187#llamas)

That made it an obvious candidate for porting to Rust, Llamas said. Binder was the first user of Rust in the kernel, he added, although Ryhl did not [publicly post](https://lwn.net/ml/linux-kernel/20231101-rust-binder-v1-0-08ba9197f637@google.com/) the first version of what became the Rust implementation [until 2023](https://lwn.net/Articles/953116/), well after the Rust experiment started in 2021. All told, the effort took more than five years. Binder is used for every single screen touch, permission check, and cross-application message on Android, which runs on more than three billion devices. In total, it works out to hundreds of billions of Binder transactions per second, globally, he added. That scale meant that even the smallest discrepancy between the interface provided by the C version and the one provided by the Rust version would cause problems and regressions for some use case.

One particularly tricky aspect has been performance, Ryhl said. Throughput doesn't matter that much; what matters is the tail latencies. Most screen frames on Android have a Binder transaction in the critical path of the frame, she explained. So, a difference of two microseconds on average is not a real problem, but if Binder is ever twenty milliseconds slower, even once in a while, people will notice and care.

[![[Alice Ryhl]](https://static.lwn.net/images/2026/alice-ryhl-kangrejos-small.png "Alice Ryhl")](https://lwn.net/Articles/1103187#ryhl)

The actual algorithms that the C and Rust implementations use are identical, she continued, so there are no large differences in throughput. But, because Rust handles locking differently, there were problems with the tail latency from lock contention that she had to debug. For example, one user-reported traceback showed Binder attempting to take a mutex that was held by another process. That process was preempted, and did not resume executing for twenty milliseconds, well past the sixteen millisecond budget to render a single frame.

It was a classic case of priority inversion. To solve the problem in Binder, Ryhl moved as many operations out from under the lock as possible, and was able to replace it with a spinlock, which made the problem go away. The equivalent lock in the C implementation was already a spinlock: "I deviated from C Binder, and *pow*. It has eighteen years of accumulated quirks."

Eventually, however, the Rust implementation was good enough for most use cases. And the C implementation was still producing new vulnerabilities every time someone tried to introduce a new feature. Eventually, Llamas and Ryhl decided to only add new features to the Rust implementation, in order to motivate people to switch and to avoid headaches from the C implementation. That policy has resulted in the Rust version becoming faster than the C version, because Ryhl was able to introduce an algorithmic improvement that reduced the number of needed system calls, a change that had been considered too complex for the C implementation. Now that the C implementation has been deleted, there are more of those coming, she promised.

Another area that proved difficult was dealing with tracepoints. Users aren't supposed to rely on the presence or behavior of most kernel-internal tracepoints, but they do. This meant that Ryhl had to copy the existing tracepoints in Binder as a de facto part of the driver's API. That even extended into some of the data structures involved in the driver. In one case, she said, there was a reference-counted structure that had a slightly different reference count in the Rust implementation, because the Rust code was holding an additional pointer to the structure. That turned out to be a problem for one particular piece of client code, since it relied on checking the reference count via a tracepoint in order to determine when the C implementation had reached a particular state.

There was also a lot of toolchain support needed, Ryhl continued. The Rust implementation had to work with kernel address-space layout randomization (KASLR), control-flow integrity (CFI), and profile-guided optimization. Production Android kernels are built with all of those enabled, and since they impact code generation, the Rust code needed to account for them.

Llamas then went into more detail about the deployment plan that will be used to convert users to the Rust implementation. A major problem with just rolling the Rust implementation out is the existence of [vendor hooks](https://lwn.net/Articles/1098493/): custom tracepoints used to inject vendor-specific behavior into the kernel. The Rust implementation of Binder was not able to reproduce those in every case, and therefore many vendors are stuck on the C implementation until they update their code. That's going to be a slow process.

Old stable versions of the kernel will continue to have the C Binder, and receive security fixes, which gives the various Android vendors a little bit of leeway until they're forced to update. After the conference, the patch removing the C implementation [was merged](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=fd2bc059ed46) as part of the 7.4 merge window, so that is the version past which the Rust implementation is required. 7.3 is the designated long-term-support release for 2026.

One person asked whether this would pose a problem for people compiling the kernel with GCC. Miguel Ojeda responded that many distributions are using a workaround for the current lack of Rust support in GCC: they are building the C side of the kernel with GCC, and the Rust side with LLVM. He said that he had advised against compiling the kernel in that way, because it is "a complete hack", though people who use it are unlikely to complain about being forced onto Clang. Greg Kroah-Hartman added that it will take months or years to learn about anyone who has problems with the Rust version of the driver, given how slowly Android vendors update their kernels. Furthermore, the removal is a single patch that can be cleanly reverted, if someone needs to continue using the C version.

Llamas thought that it was worth considering what the removal process should look like for other code that gets ported to Rust. He asked what people thought should happen to the C version of converted drivers on long-term-support branches. Ojeda said that the process would be exactly the same as the existing policy for replaced C code. The only difference between Rust and C in long-term-support releases is that the Rust maintainers backport fixes for soundness problems, he said. Kroah-Hartman told Llamas not to worry about code that has been replaced: "Just delete it. I take patches for subsystems that have been deleted. It's fine."

With that declaration, the session, the conference, and the presence of the C Binder driver in the kernel were all over. Binder is the first nontrivial component in the kernel to be ported to Rust, and its journey has revealed several pitfalls that don't apply to new drivers written in Rust from scratch. Still, the most difficult problems were not Rust-specific; they were the compatibility problems that any rewrite would face. Whether any other kernel components will find the hassle of a rewrite worth it, and how long they will take to accomplish it, remains to be seen.

\[ Thanks to the Linux Foundation, LWN's travel sponsor, for supporting my travel to Kangrejos. \]

| Index entries for this article                           |                                                                             |
| -------------------------------------------------------- | --------------------------------------------------------------------------- |
| [Conference](https://lwn.net/Archives/ConferenceIndex/) | [Kangrejos/2026](https://lwn.net/Archives/ConferenceIndex/#Kangrejos-2026) |

* * *

The LWN site is currently under high scraper load, so comment display has been suppressed for anonymous users. If you are a human, you may read the comments by clicking the button below:

**Note**: you can avoid this step in the future by logging into your LWN account.
