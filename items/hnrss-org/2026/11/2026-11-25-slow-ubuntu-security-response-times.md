---
title: Slow Ubuntu security response times
link: https://www.openwall.com/lists/oss-security/2026/11/25/9
source: hnrss-org
published: 2026-11-25T20:38:00Z
updated: 2026-11-25T20:38:00Z
first_seen: 2026-11-26T02:47:10.395377406Z
authors:
- gslin
content: extracted
html: 2026-11-25-slow-ubuntu-security-response-times.html
remote_preview:
  url: https://www.openwall.com/logo.png
  alt: Openwall
---

[![Openwall](https://www.openwall.com/logo.png)](https://www.openwall.com/)

- [Products](https://www.openwall.com/)
  - [Openwall GNU/\*/Linux   *server OS*](https://www.openwall.com/Owl/)
  - [Linux Kernel Runtime Guard](https://www.openwall.com/lkrg/)
  - [John the Ripper   *password cracker*](https://www.openwall.com/john/)
    - [Free & Open Source for any platform](https://www.openwall.com/john/)
    - [in the cloud](https://www.openwall.com/john/cloud/)
    - [Pro for Linux](https://www.openwall.com/john/pro/linux/)
    - [Pro for macOS](https://www.openwall.com/john/pro/macosx/)
  - [Wordlists   *for password cracking*](https://www.openwall.com/wordlists/)
  - [passwdqc   *policy enforcement*](https://www.openwall.com/passwdqc/)
    - [Free & Open Source for Unix](https://www.openwall.com/passwdqc/)
    - [Pro for Windows (Active Directory)](https://www.openwall.com/passwdqc/windows/)
  - [yescrypt   *KDF & password hashing*](https://www.openwall.com/yescrypt/)
  - [yespower   *Proof-of-Work (PoW)*](https://www.openwall.com/yespower/)
  - [crypt\_blowfish   *password hashing*](https://www.openwall.com/crypt/)
  - [phpass   *ditto in PHP*](https://www.openwall.com/phpass/)
  - [tcb   *better password shadowing*](https://www.openwall.com/tcb/)
  - [Pluggable Authentication Modules](https://www.openwall.com/pam/)
  - [scanlogd   *port scan detector*](https://www.openwall.com/scanlogd/)
  - [popa3d   *tiny POP3 daemon*](https://www.openwall.com/popa3d/)
  - [blists   *web interface to mailing lists*](https://www.openwall.com/blists/)
  - [msulogin   *single user mode login*](https://www.openwall.com/msulogin/)
  - [php\_mt\_seed   *mt\_rand() cracker*](https://www.openwall.com/php_mt_seed/)
- [Services](https://www.openwall.com/services/)
- Publications
  - [Articles](https://www.openwall.com/articles/)
  - [Presentations](https://www.openwall.com/presentations/)
- Resources
  - [Mailing lists](https://www.openwall.com/lists/)
  - [Community wiki](https://openwall.info/wiki/)
  - [Source code repositories (GitHub)](https://github.com/openwall)
  - [File archive & mirrors](https://www.openwall.com/mirrors/)
  - [How to verify digital signatures](https://www.openwall.com/signatures/)
  - [OVE IDs](https://www.openwall.com/ove/)
- [What's new](https://www.openwall.com/news)

[\[\<prev\]](https://www.openwall.com/lists/oss-security/2026/11/25/8) [\[next>\]](https://www.openwall.com/lists/oss-security/2026/11/25/10) [\[day\]](https://www.openwall.com/lists/oss-security/2026/11/25) [\[month\]](https://www.openwall.com/lists/oss-security/2026/11) [\[year\]](https://www.openwall.com/lists/oss-security/2026) [\[list\]](https://www.openwall.com/lists/oss-security)

```
Message-ID: <20261125172638.73c0a05b@hboeck.de>
Date: Wed, 25 Nov 2026 17:26:38 +0100
From: Hanno Böck <hanno@...eck.de>
To: oss-security@...ts.openwall.com
Subject: Slow Ubuntu security response times

Hello,

Lately, I'm observing that Ubuntu's response to severe security flaws
is concerning.

On November 16th, details about a remote memory corruption called
NoisyTCP (CVE-2026-99288) in the Linux kernel's TCP stack were
published: https://www.openwall.com/lists/oss-security/2026/11/15/2
Today, nine days later, Ubuntu still has not published a fix yet:
https://ubuntu.com/security/CVE-2026-99288
Other distros had a fix already available when the details were
published.

Likewise, a KVM host escape (CVE-2026-99312) affecting AMD
virtualization published on Nov 18th has no fix yet either [3]:
https://ubuntu.com/security/CVE-2026-99312

Ubuntu has been slow in patching another remote TCP corruption
(CVE-2026-98432) lately, where it took multiple weeks until a fix was
available. See also: https://chalmovsky.com/posts/wtf-ubuntu
Recent news indicates that this is might be exploited in the wild:
https://www.greynoise.io/blog/check-point-security-management-servers-intrusion-campaign


I think everyone understands that these times aren't easy for security
teams. Nevertheless, remote corruption vulnerabilities in the Linux
kernel are the kind of thing where I expect updates to be available
within hours, not days or weeks.

On a recent Ubuntu LTS system, the kernel version is 7.0.0-44. This
seems to indicate that Ubuntu is not relying on longterm supported
kernels from kernel.org, but is instead backporting patches to an old
kernel version. On a Debian stable system, the kernel version is
6.12.118+deb13, indicating that it's based on a very recent longterm
kernel from kernel.org. I would consider the backporting strategy for
the kernel to be hardly viable and probably everyone should base their
Linux kernels on longterm releases supported by the Linux kernel
developers.

-- 
Hanno Böck - Independent security researcher
https://itsec.hboeck.de/
https://badkeys.info/
```

[Powered by blists](https://www.openwall.com/blists/) - [more mailing lists](https://lists.openwall.net)

Please check out the [Open Source Software Security Wiki](https://oss-security.openwall.org/wiki/), which is counterpart to this [mailing list](https://oss-security.openwall.org/wiki/mailing-lists/oss-security).

Confused about [mailing lists](https://www.openwall.com/lists/) and their use? [Read about mailing lists on Wikipedia](https://en.wikipedia.org/wiki/Electronic_mailing_list) and check out these [guidelines on proper formatting of your messages](https://www.complang.tuwien.ac.at/anton/mail-news-errors.html).
