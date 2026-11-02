---
title: Ubuntu still has not released patch for Linux 0day RCE in almost 3 weeks
link: https://freenode.net/article/ubuntu-lts-kernels-still-unfixed-for-critical-cve-2026-98432
source: hnrss-org
published: 2026-11-02T18:39:38Z
updated: 2026-11-02T18:39:38Z
first_seen: 2026-11-02T23:50:57.210604418Z
authors:
- majorchord
content: extracted
html: 2026-11-02-ubuntu-still-has-not-released-patch-for-linux-0day-rce-in.html
remote_preview:
  url: https://freenode.net/media/og-default.png
---

No patched kernels are available yet for 22.04, 24.04, or 26.04 despite a CVSS 9.8 rating.

Ubuntu has not shipped fixed kernels for [CVE-2026-98432](https://www.cve.org/CVERecord?id=CVE-2026-98432), a critical vulnerability scored CVSS 9.8, leaving the 22.04, 24.04, and 26.04 LTS releases without a vendor update.

Operators running those releases therefore have no official kernel package that closes the hole. The gap matters most for internet-facing and multi-tenant self-hosted systems that rely on Ubuntu’s LTS kernels for isolation and privilege boundaries; until packages appear, those hosts remain exposed to whatever remote or local attack the CVE enables.

Ubuntu has not announced an ETA for the fixes. In the interim, affected administrators are left to decide whether to stay on the stock kernel, apply out-of-tree mitigations if any exist, or move workloads onto releases or kernels that already carry the patch.
