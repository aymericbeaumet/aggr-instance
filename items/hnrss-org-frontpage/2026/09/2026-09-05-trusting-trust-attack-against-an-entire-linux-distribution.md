---
title: Trusting-Trust Attack against an Entire Linux Distribution
link: https://arxiv.org/abs/2607.24888
source: hnrss-org-frontpage
published: 2026-09-05T11:25:18Z
updated: 2026-09-05T11:25:18Z
first_seen: 2026-09-08T10:17:10.166315Z
authors:
- signa11
summary: 'Article URL: https://arxiv.org/abs/2607.24888 Comments URL: https://news.ycombinator.com/item?id=49575515 Points: 208 # Comments: 44'
content: extracted
html: 2026-09-05-trusting-trust-attack-against-an-entire-linux-distribution.html
preview:
  file: 2026-09-05-trusting-trust-attack-against-an-entire-linux-distribution.preview-41145fe12e79.webp
  width: 256
  height: 149
  alt: arXiv logo
  color: '#ece6e5'
images:
- source: https://arxiv.org/static/browse/0.3.4/images/arxiv-logo-fb.png
  original:
    file: 2026-09-05-trusting-trust-attack-against-an-entire-linux-distribution.image-88f5088404c9.png
    width: 1200
    height: 700
  variants:
  - file: 2026-09-05-trusting-trust-attack-against-an-entire-linux-distribution.image-03edab4f04dd.webp
    width: 48
    height: 28
  - file: 2026-09-05-trusting-trust-attack-against-an-entire-linux-distribution.image-fc6386ce1e2e.webp
    width: 320
    height: 187
  - file: 2026-09-05-trusting-trust-attack-against-an-entire-linux-distribution.image-49eabc2e18a4.webp
    width: 640
    height: 373
  - file: 2026-09-05-trusting-trust-attack-against-an-entire-linux-distribution.image-f275dcf4bd8a.webp
    width: 1200
    height: 700
  color: '#fefefe'
---

[View PDF](https://arxiv.org/pdf/2607.24888) [HTML (experimental)](https://arxiv.org/html/2607.24888v1)

> Abstract:Ken Thompson's trusting-trust attack, in which a compromised compiler backdoors the programs it builds and reproduces the backdoor in subsequent rebuilds of itself, is widely regarded as a threat specific to compilers. We show that it is not. We construct a complete trusting-trust attack around GNU strip, an ordinary build utility that neither inspects nor generates source code, using only manipulations of finished ELF files. In the bootstrap of the NixOS Linux distribution, a single tampered strip in the binary seed implants a payload that propagates from one generation of strip to the next and survives into the final standard environment after the seed leaves the dependency closure. On a real nixpkgs revision, the attack builds a complete graphical installer without failures and backdoors almost every one of its binaries, enabling arbitrary malicious behavior of the subverted packages.

Subjects:

Cryptography and Security (cs.CR); Software Engineering (cs.SE)

Cite as:

[arXiv:2607.24888](https://arxiv.org/abs/2607.24888) \[cs.CR\]

(or [arXiv:2607.24888v1](https://arxiv.org/abs/2607.24888v1) \[cs.CR\] for this version)

[https://doi.org/10.48550/arXiv.2607.24888](https://doi.org/10.48550/arXiv.2607.24888)

arXiv-issued DOI via DataCite

## Submission history

From: Aman Sharma \[[view email](https://arxiv.org/show-email/473a6cf3/2607.24888)\] \
 **\[v1\]** Mon, 27 Jul 2026 12:59:15 UTC (1,848 KB)
