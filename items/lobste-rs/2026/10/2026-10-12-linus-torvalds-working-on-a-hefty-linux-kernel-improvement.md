---
title: Linus Torvalds Working On A Hefty Linux Kernel Improvement With Claude Code
link: https://www.phoronix.com/news/Linus-Torvalds-Claude-Code
source: lobste-rs
published: 2026-10-12T10:11:29Z
updated: 2026-10-12T10:11:29Z
first_seen: 2026-10-13T18:08:43.541012300Z
authors:
- phoronix.com via oceanhaiyang
labels:
- linux
- vibecoding
summary: Comments
content: extracted
html: 2026-10-12-linus-torvalds-working-on-a-hefty-linux-kernel-improvement.html
---

![LINUX KERNEL](https://www.phoronix.com/assets/categories/linuxkernel.webp)

Linux creator Linus Torvalds has been known to [use AI to help with kernel debugging](https://www.phoronix.com/news/Linus-Torvalds-Debug-AI) and outside of the kernel tree has also done [some vibe coding with Google's Antigravity](https://www.phoronix.com/news/Linus-Torvalds-Vide-Coding). He has previously said [he views AI as a useful tool](https://www.phoronix.com/news/Linux-Is-Not-Anti-AI). It turns out he's even now using Claude Code for working on new Linux kernel features.

While waiting for the Linux 7.3-rc7 kernel release and refreshing his *torvalds/linux.git* repository, I was surprised to find a new kernel branch appear. Typically Torvalds isn't doing much work on branches in his main repository and typically just hosting the *master* branch.

![noref-filemap-read-with-llm Git branch](https://www.phoronix.net/image.php?id=2026&image=linux_llm_branch)

The new branch to appear is *noref-filemap-read-with-llm* and as implied by the "LLM" postfix, is some new kernel improvements he's been working on with the help of AI / LLMs. Claude Code in particular. The branched code is working on adapting the memory management code to read from the page cache without folio references.

The code is working on addressing a Linux issue of small reads of one file from many threads contend on the folio reference counting. So the goal of the branch / patch series is to read from the page cache without folio references.

For those passionate about AI for loving or hating it, in the main [patch message](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?h=noref-filemap-read-with-llm&id=4c0f3697118eb3a708ea79175f7f07c43339bfab) Linus Torvalds confirmed his AI use in creating this work and related testing and even the initial draft of the cover letter:

> "The code, the test programs, the review behind the list above and the first draft of this text were done with an LLM coding assistant (Claude Code) under my direction. The measurements were run on my machine."

And for the impact on this situation of addressing the bottleneck of small reads of one file from many threads, the impact of the series is significant:

![performance results of Linus Torvalds AI code](https://www.phoronix.net/image.php?id=2026&image=linus_llm_performance)

Quite a difference. The patches are currently on Torvalds' [noref-filemap-read-with-llm Git branch](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/log/?h=noref-filemap-read-with-llm). We'll see if he merges his branch for the upcoming Linux 7.4 merge window.

**Update:** Some additional AI kernel usage by Torvalds with [Linus Torvalds "Did What I Preach", Used AI To Make Code Decipherable & Uncovered More Bugs](https://www.phoronix.com/news/Linus-Torvalds-Assoc-Array).
