---
title: Go will reject certificate chains containing eight distrusted SPKIs
link: https://github.com/golang/go/issues/82345
source: hnrss-org
published: 2026-10-20T03:05:07Z
updated: 2026-10-20T03:05:07Z
first_seen: 2026-10-20T12:56:01.525437407Z
authors:
- mendym
content: extracted
html: 2026-10-20-go-will-reject-certificate-chains-containing-eight.html
remote_preview:
  url: https://opengraph.githubassets.com/90d06a2ec5816d5fc7b049bf1df65c1e9fcbdd836299c80184753809a74f7e69/golang/go/issues/82345
  alt: We're blocking trust in a small set of intermediate certificates due to specific security concerns around their operation. Verify will now filter out chains which contain these intermediates, preve...
---

We're blocking trust in a small set of intermediate certificates due to specific security concerns around their operation.

Verify will now filter out chains which contain these intermediates, preventing them from being returned to the caller.

We highly recommend immediately upgrading to this release.

This is CVE-2026-111943 and Go issue [https://go.dev/issue/82345](https://go.dev/issue/82345).

* * *

This was an [URGENT track](https://go.dev/doc/security/policy#urgent) issue originally tracked in [http://b/575512169](http://b/575512169).
