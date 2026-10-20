---
title: Making big downloads resumable in Firefox
link: https://lunarwatcher.codeberg.page/til/firefox/resumable-big-downloads.html
source: hnrss-org
published: 2026-10-12T19:36:02Z
updated: 2026-10-12T19:36:02Z
first_seen: 2026-10-13T03:09:55.529316228Z
authors:
- speckx
content: extracted
html: 2026-10-12-making-big-downloads-resumable-in-firefox.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50061791
  num_comments: 43
  points: 110
---

firefox

* * *

For whatever reason, Firefox 28 and newer (i.e. every modern version after like [2014](https://morgb.blogspot.com/2014/05/firefox-29-and-http-response-timeout.html)) ships with `network.http.response.timeout` set to 300 seconds. This seems to interfere with resuming big downloads (read: tens of gigs).

Setting `network.http.response.timeout` to `0` (or something very big) in `about:config` seems to make big downloads resumable. I don't understand why this works, but experimental testing supports it. This may or may not just generally affect flaky downloads that take a long time to finish, which at least for my general use-case only includes big downloads. For now at least.

Note that this assumes the server supports resumable downloads; if it doesn't, no settings in firefox or any other browser are going to help. However, the problem this fixes is firefox being unable to resume a resumable download on error. The test case for this was the SE data dump (the full one released earlier this month), as well as the SO dump in particular, which are 90GiB and ~60GiB respectively. I don't entirely know if this is the case, but it experimentally looks like resumable downloads in firefox rely on connection timeouts rather than the `.part` files they download. Firefox will happily ignore an existing `.part` without this option, and I do not understand why.

Resumable downloads in firefox generally appear to be fairly unstable though. This has been a known problem in some form since [as far back as 2005](https://forums.mozillazine.org/viewtopic.php?t=238764). I also found that there's [a manual `.part` shuffle to make firefox actually resume downloads](https://superuser.com/a/1103144) across sessions, or presumably FF deciding to ignore the `.part` in a running session because of the timeout option, but I'm not doing that shit. As a non-firefox workaround, using `wget -c -O "$filename" "$url"` also works, and wget overall appears to be fairly stable. Using wget for resuming is also much easier if the URL changes, which the SO data dump URLs may or may not do depending on whether or not SE has reset the expiration time of the URL for any given data dump. The only way to get that with Firefox is the `.part` shuffle, and fuck that.

I have not attempted to confirm any of this with FF's source code, because I have no clue where to look.
