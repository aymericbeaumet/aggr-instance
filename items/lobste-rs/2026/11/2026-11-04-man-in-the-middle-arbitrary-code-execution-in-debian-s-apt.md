---
title: Man-in-the-middle arbitrary code execution in Debian’s apt
link: https://lists.debian.org/debian-security-announce/2026/msg00526.html
source: lobste-rs
published: 2026-11-04T13:36:43Z
updated: 2026-11-04T13:36:43Z
first_seen: 2026-11-06T18:21:10.909146084Z
authors:
- lists.debian.org via gnyeki
labels:
- linux
- security
summary: Comments
content: extracted
html: 2026-11-04-man-in-the-middle-arbitrary-code-execution-in-debian-s-apt.html
---

* * *

- *To*: [debian-security-announce@lists.debian.org](mailto:debian-security-announce%40lists.debian.org)
- *Subject*: \[SECURITY\] \[DSA 6607-1\] apt security update
- *From*: Salvatore Bonaccorso <[carnil@debian.org](mailto:carnil%40debian.org)\>
- *Date*: Wed, 04 Nov 2026 12:07:32 +0000
- *Message-id*: <[\[🔎\]](https://lists.debian.org/msgid-search/E1xOZm8-00000002Ddo-3pY0@seger.debian.org) [E1xOZm8-00000002Ddo-3pY0@seger.debian.org](https://lists.debian.org/debian-security-announce/2026/msg00526.html)\>
- *Reply-to*: [debian-security-announce-request@lists.debian.org](mailto:debian-security-announce-request%40lists.debian.org)

* * *

```
-----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA512

- -------------------------------------------------------------------------
Debian Security Advisory DSA-6607-1                   security@debian.org
https://www.debian.org/security/                     Salvatore Bonaccorso
November 04, 2026                     https://www.debian.org/security/faq
- -------------------------------------------------------------------------

Package        : apt
CVE ID         : CVE-2026-109518

Max Justicz discovered a vulnerability in the APT package manager. APT
can merge two different package downloads after an HTTP redirect gives
them the same URI. Improper file handling when resuming an HTTP download
allows a man-in-the-middle attacker to replace a package after
verification, resulting in the execution of arbitrary code as root.

For the stable distribution (trixie), this problem has been fixed in
version 3.0.4. Additionally this update addresses several out-of-bounds
access issues.

We recommend that you upgrade your apt packages.

For the detailed security status of apt please refer to
its security tracker page at:
https://security-tracker.debian.org/tracker/apt

Further information about Debian Security Advisories, how to apply
these updates to your system and frequently asked questions can be
found at: https://www.debian.org/security/

Mailing list: debian-security-announce@lists.debian.org
-----BEGIN PGP SIGNATURE-----

iQKTBAEBCgB9FiEERkRAmAjBceBVMd3uBUy48xNDz0QFAmrrH7dfFIAAAAAALgAo
aXNzdWVyLWZwckBub3RhdGlvbnMub3BlbnBncC5maWZ0aGhvcnNlbWFuLm5ldDQ2
NDQ0MDk4MDhDMTcxRTA1NTMxRERFRTA1NENCOEYzMTM0M0NGNDQACgkQBUy48xND
z0Sn4RAAiiQ1hEeN76ANeF3IeVAzYi+0bzUs91pkeLW6eGRlRfEgQX/wLfQ0ewkh
8nmaxpWW4ZCm+1jcwuu/MtNJgVx0ALgJ9cNNS/3CkQJmT0BVnj5ORgY/MHwwRCa+
lU6JkTiAYcQflM//9gFZjvGJMYvVMs8RFqukkLHZoV4CAhLLk3yjQhCxkFa6rjeA
9+1U7lYjuWXQLm/lv+gy9Kp+CvGFuoEuG8v8EI+mAthUmcY4uCFb1Kf8X1XDMJLD
YTNYnIucJ3eAhmPtdZcjVAuGR0BxigJmzrKSthV+gkY62shH/aH8UPzCaic/MoSk
OXgi7gQgGT3tLqE2s7jGzdUG9ZUihA6ZHmqQgUrFUcu3KlQ/90H6hkxiFvwtX9E8
iTmhNXfFzmO/0YErF9V1/5XBqee+gaj2s919nLljjridutuYrjINpHjm3jkv1QWo
+kN9/bAF9ooVxRvMnY1EjMhHSt4s4tZrwTQwil8oSM+hDovascWMRzz8DBBl9KSf
n85yBboNXV951YJEeprNDK9FcIHaApZzuFiIlrN2MoifTGhJGyJ8k9qtSKpnGdKf
vT7/B9x7rZANn3ltOm8KEaSlieznUmLL5u0ZZGhqs/Dus5CKKTE+8+mvo+CmaJQm
ChA8vvi41w4gQiP/2W59NF1ECJlzykNMFposuJsoFXNoMp36PLM=
=VlLy
-----END PGP SIGNATURE-----
```

* * *

**Reply to:**

- [debian-security-announce@lists.debian.org](<mailto:debian-security-announce@lists.debian.org?in-reply-to=<E1xOZm8-00000002Ddo-3pY0@seger.debian.org>&subject=Re:%20[SECURITY] [DSA 6607-1] apt security update>)
- [Salvatore Bonaccorso (on-list)](<mailto:carnil@debian.org?in-reply-to=<E1xOZm8-00000002Ddo-3pY0@seger.debian.org>&subject=Re:%20[SECURITY] [DSA 6607-1] apt security update&cc=debian-security-announce@lists.debian.org>)
- [Salvatore Bonaccorso (off-list)](<mailto:carnil@debian.org?in-reply-to=<E1xOZm8-00000002Ddo-3pY0@seger.debian.org>&subject=Re:%20[SECURITY] [DSA 6607-1] apt security update>)

* * *

- Prev by Date: **[\[SECURITY\] \[DSA 6606-1\] squid security update](https://lists.debian.org/debian-security-announce/2026/msg00525.html)**
- Next by Date: **[\[SECURITY\] \[DSA 6608-1\] puppet-agent security update](https://lists.debian.org/debian-security-announce/2026/msg00527.html)**
- Previous by thread: **[\[SECURITY\] \[DSA 6606-1\] squid security update](https://lists.debian.org/debian-security-announce/2026/msg00525.html)**
- Next by thread: **[\[SECURITY\] \[DSA 6608-1\] puppet-agent security update](https://lists.debian.org/debian-security-announce/2026/msg00527.html)**
- Index(es):
  - [**Date**](https://lists.debian.org/debian-security-announce/2026/mail2.html#00526)
  - [**Thread**](https://lists.debian.org/debian-security-announce/2026/thrd2.html#00526)
