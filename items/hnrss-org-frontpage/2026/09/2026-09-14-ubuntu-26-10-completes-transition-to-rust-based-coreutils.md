---
title: Ubuntu 26.10 completes transition to Rust-based coreutils
link: https://www.omgubuntu.co.uk/2026/09/ubuntu-2610-rust-coreutils-complete
source: hnrss-org-frontpage
published: 2026-09-14T13:38:58Z
updated: 2026-09-14T13:38:58Z
first_seen: 2026-09-15T06:38:21.934843588Z
authors:
- theanonymousone
summary: 'Article URL: https://www.omgubuntu.co.uk/2026/09/ubuntu-2610-rust-coreutils-complete Comments URL: https://news.ycombinator.com/item?id=49696697 Points: 142 # Comments: 117'
content: extracted
html: 2026-09-14-ubuntu-26-10-completes-transition-to-rust-based-coreutils.html
preview:
  file: 2026-09-14-ubuntu-26-10-completes-transition-to-rust-based-coreutils.preview-378bf82ce397.webp
  width: 256
  height: 134
  alt: Ubuntu logo with the Rust lang logo
  color: '#4e4e4d'
images:
- source: https://www.omgubuntu.co.uk/wp-content/uploads/2025/05/ubuntu-rust.jpg
  original:
    file: 2026-09-14-ubuntu-26-10-completes-transition-to-rust-based-coreutils.image-9b8af12f0d8c.jpg
    width: 1920
    height: 1008
  color: '#171715'
---

**Ubuntu 26.10 completes the distro’s move to Rust-based core utilities, with the commands previously held back due to security issues now migrated to memory-safe versions.**

`cp`, `mv` and `rm` were held back on their GNU versions in Ubuntu 26.04 LTS due to a crop of TOCTOU (time-of-check to time-of-use) issues that needed to be fixed in the `uutils` versions.

With those issues resolved upstream, Ubuntu 26.10 finishes the job. The *‘Stonking Stingray’* ships a full set of Rust core utilities, which encompasses common command-line tools like `ls`, `cat`, `chmod` and  `du`.

> Canonical donates €40k a year to help fund work on Rust software

Canonical’s engineers began ‘oxidising’ the distro – replacing foundational software with Rust alternatives – in 2025. It [sees security benefits](https://discourse.ubuntu.com/t/carefully-but-purposefully-oxidising-ubuntu/56995?u=d0od) in doing so, since Rust catches memory bugs at compile time, whereas C compilers don’t.

Ubuntu 25.10 was the first release to ship with Rust-based utilities and made [Rust-based sudo the default](https://www.omgubuntu.co.uk/2025/05/ubuntu-25-10-rust-sudo-rs-change).

Migrating [hasn’t been without hiccups](https://www.omgubuntu.co.uk/2025/10/ubuntu-25-10-rust-coreutils-date-bug), but Canonical has been studious.

It [commissioned a security audit](https://github.com/Zellic/publications/blob/master/uutils%20coreutils%20-%20Zellic%20Audit%20Report.pdf) of `uutils` ahead of 26.04, which found the issues that kept the three commands back on their GNU versions. It’s also a gold sponsor of the [*Trifecta Tech Foundation*](https://trifectatech.org), giving €40,000 a year to fund its work on Rust software.

The non-profit foundation is undertaking a Rust-based [rewrite of the Network Time Protocol (NTP)](https://www.omgubuntu.co.uk/2026/07/ubuntu-ntpd-rs-rust-time-sync), and Ubuntu plans to use it as the default time sync client by 27.10.

Here, the completion of the coreutils migration offers no functional difference to end users. The Rust-based `uutils` aims for drop-in compatibility with GNU versions, and treats any deviances as a bug. That’s by designed; the point is one of improved security.

Ubuntu 26.10 ‘Stonking Stingray’ beta arrives later this month, before the stable release on 15 October, 2026.
