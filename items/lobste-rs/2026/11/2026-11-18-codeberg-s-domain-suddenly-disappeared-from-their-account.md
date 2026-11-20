---
title: Codeberg's domain suddenly disappeared from their account, site is unreachable
link: https://social.anoxinon.de/@Codeberg/117636454915265360
source: lobste-rs
published: 2026-11-18T08:51:09Z
updated: 2026-11-18T08:51:09Z
first_seen: 2026-11-18T17:57:41.978411823Z
authors:
- social.anoxinon.de via mort
labels:
- vcs
summary: 'Note: codeberg''s servers are up, it''s just the domain that''s gone. It works perfectly after adding the following line to /etc/hosts: 217.197.84.140 codeberg.org Update: it''s now up again, and the above workaround is not necessary. Comments'
content: extracted
html: 2026-11-18-codeberg-s-domain-suddenly-disappeared-from-their-account.html
---

codeberg.org is unfortunately down right now due to a yet undefined issue with our domain. It was not expired and not transferred, the domain is completely gone from our account and we did not receive any email notifications, and it is currently in the status clientHold. We are currently trying to contact our registrar to get more information right now & trying to resolve this issue, but this is probably going to take some time.

We are sorry for the inconveniences & are working on a solution.

We have written up a post-mortem for the folks who have been wondering why our domain was suspended for a while on November 18.

The tl;dr: A person reported a malware-containing Codeberg repository directly to our domain registrar, which proceeded to suspend the domain without coordinating with our team.

We outline some planned steps to avoid this in the future. Read the full bit here:

[https:// codeberg.org/Codeberg/changelo g/issues/7#issuecomment-28100157](https://codeberg.org/Codeberg/changelog/issues/7#issuecomment-28100157)
