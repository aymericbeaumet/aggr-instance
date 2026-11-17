---
title: Redict is deprecated
link: https://redict.io/posts/2026-11-17-redict-deprecated/
source: lobste-rs
published: 2026-11-17T08:20:30Z
updated: 2026-11-17T08:20:30Z
first_seen: 2026-11-17T18:26:16.162530996Z
authors:
- redict.io via val
labels:
- databases
summary: Comments
content: extracted
html: 2026-11-17-redict-is-deprecated.html
remote_preview:
  url: https://redict.io/svg/calendar.svg
---

[Redict is deprecated](https://redict.io/posts/2026-11-17-redict-deprecated/) \
Drew DeVault
---

![](https://redict.io/svg/calendar.svg) November 17, 2026

I must unfortunately announce that Redict is officially deprecated, and Redict 7.3.6 is the final release.

I had hoped to rally the Redis diaspora around a copyleft initiative, to protect ourselves from future rug-pulls, and made Redict to serve this purpose. However, with the valkey project’s success, and Redis’s return to a copyleft license, Redict was not able to build a sustainable community.

We recommend that Redict users switch to [valkey](https://valkey.io/), which uses a permissive license but has better long-term prospects. We advise against switching back to Redis; though it is once again open source, and copyleft, it uses a contributor license agreement which will facilitate another rug-pull when Redis has a bad financial quarter.

Switching to valkey is straightforward: if you install from your package manager, simply install valkey and move your config file:

```
# mv /etc/redict.conf /etc/valkey.conf
```

If you use containers, simply pull the valkey container instead and ensure that it runs with the appropriate config file.

Thanks to everyone who has contributed to Redict, endorsed the fork as the future of Redis, and switched to it on their own machines. We had a good run :)
