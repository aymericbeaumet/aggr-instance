---
title: Leaking Secrets from Air-Gap Computers by Spelling Radio Signals from RAM (2024)
link: https://arxiv.org/abs/2409.02292
source: hnrss-org
published: 2026-11-01T01:46:29Z
updated: 2026-11-01T01:46:29Z
first_seen: 2026-11-01T16:16:54.446869743Z
authors:
- hapax_legomena
content: extracted
html: 2026-11-01-leaking-secrets-from-air-gap-computers-by-spelling-radio.html
remote_preview:
  url: https://arxiv.org/static/browse/0.3.4/images/arxiv-logo-fb.png
  alt: arXiv logo
---

[View PDF](https://arxiv.org/pdf/2409.02292) [HTML (experimental)](https://arxiv.org/html/2409.02292v1)

> Abstract:Air-gapped systems are physically separated from external networks, including the Internet. This isolation is achieved by keeping the air-gap computers disconnected from wired or wireless networks, preventing direct or remote communication with other devices or networks. Air-gap measures may be used in sensitive environments where security and isolation are critical to prevent private and confidential information leakage. \
> In this paper, we present an attack allowing adversaries to leak information from air-gapped computers. We show that malware on a compromised computer can generate radio signals from memory buses (RAM). Using software-generated radio signals, malware can encode sensitive information such as files, images, keylogging, biometric information, and encryption keys. With software-defined radio (SDR) hardware, and a simple off-the-shelf antenna, an attacker can intercept transmitted raw radio signals from a distance. The signals can then be decoded and translated back into binary information. We discuss the design and implementation and present related work and evaluation results. This paper presents fast modification methods to leak data from air-gapped computers at 1000 bits per second. Finally, we propose countermeasures to mitigate this out-of-band air-gap threat.

|           |                                                                                                                        |
| --------- | ---------------------------------------------------------------------------------------------------------------------- |
| Comments: | Version of this work accepted to Nordic Conference on Secure IT Systems, 2023                                          |
| Subjects: | Cryptography and Security (cs.CR)                                                                                      |
| Cite as:  | [arXiv:2409.02292](https://arxiv.org/abs/2409.02292) \[cs.CR\]                                                        |
|           | (or [arXiv:2409.02292v1](https://arxiv.org/abs/2409.02292v1) \[cs.CR\] for this version)                              |
|           | [https://doi.org/10.48550/arXiv.2409.02292](https://doi.org/10.48550/arXiv.2409.02292)  arXiv-issued DOI via DataCite |

## Submission history

From: Mordechai Guri \[[view email](https://arxiv.org/show-email/3a271016/2409.02292)\] \
 **\[v1\]** Tue, 3 Sep 2024 21:06:04 UTC (18,145 KB)
