---
title: WTF Ubuntu?
link: https://chalmovsky.com/posts/wtf-ubuntu
source: hnrss-org
published: 2026-11-02T14:52:00Z
updated: 2026-11-02T14:52:00Z
first_seen: 2026-11-02T16:59:20.971254334Z
authors:
- chalmovsky
content: extracted
html: 2026-11-02-wtf-ubuntu.html
---

What the fuck, Ubuntu?

In case you missed it: [CVE-2026-98432](https://ubuntu.com/security/CVE-2026-98432) is a critical (CVSS 9.8) bug in the Linux kernel's TCP code. It's been there since 2016, so virtually every Linux server on the planet has it, and it can be hit remotely. Anthropic says it built a working exploit that gets root on a server. A total takeover.

Debian [patched it on October 16](https://lists.debian.org/debian-security-announce/2026/msg00482.html). Ubuntu still doesn't have a patch more than two weeks later. Every supported release, LTS included, is listed as "Vulnerable".

Ubuntu's website promises ["timely patches for critical vulnerabilities."](https://ubuntu.com/server) Ubuntu hasn't shipped the patch nine days after it was rated 9.8 and six days after a working root exploit. Very fucking timely indeed.

Software is full of this blabber where words mean nothing. Ubuntu uses words like these: "Timely." "Enterprise-grade." "Production grade." "Enterprise resilience. Developer velocity. Build with confidence on Ubuntu."

Somebody, or perhaps worse, some special team at Canonical is dedicated to writing these........

So I'm asking again... What the fuck, Ubuntu?

I'm moving all my servers off Ubuntu. I've completely lost trust in it. On the desktop I always hated it anyway. On servers I mostly used it because it's the default on most VPS providers, and I considered it the solid, no-drama LTS distro. Fuck that now.

I can't express how crazy this is. I didn't have time to migrate my services and data when I learned about this, so I had to take all my servers that run Ubuntu offline.

Oh and btw, distros run by volunteers (Debian, Arch, Alpine...) hve shipped the fix. Meanwhile Ubuntu, made by a company that sells security subscriptions, hasn't. This is true as of 2026-11-02 14:48 UTC.
