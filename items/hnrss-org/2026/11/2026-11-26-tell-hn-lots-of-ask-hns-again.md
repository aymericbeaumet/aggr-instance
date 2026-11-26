---
title: 'Tell HN: Lots of Ask HNs again'
link: https://news.ycombinator.com/item?id=50649864
source: hnrss-org
published: 2026-11-26T00:43:34Z
updated: 2026-11-26T00:43:34Z
first_seen: 2026-11-26T12:13:26.788658858Z
authors:
- dang
summary: 'There have been questions about why the /ask page (https://news.ycombinator.com/ask) was running dry of Ask HNs - e.g.: Ask HN: Why is Ask HN only showing me 14 posts? - https://news.ycombinator.com/item?id=49984484 - Oct 2026 (47 comments) Ask HN: Why is pagination gone on "Ask HN", and showing only 19 items? - https://news.ycombinator.com/item?id=50280719 - Oct 2026 (3 comments) I believe I found the problem. The pool of stories for https://news.ycombinator.com/ask has traditionally been drawn from the ranked stories list from which we generate the frontpage. That list is restricted to the top 1500 ranked submissions, and it seems the % of submissions which are text posts (i.e. no linked URL) had declined enough to starve the Ask HN feed. The solution is to compute a separate list of ranked Ask HNs. p.s. "Ask HN" is the conventional name but it means text posts. The title can also start with "Tell HN" or nothing in particular. They''re just posts which were submitted without a URL. Comments URL: https://news.ycombinator.com/item?id=50649864 Points: 286 # Comments: 111'
content: extracted
html: 2026-11-26-tell-hn-lots-of-ask-hns-again.html
---

There have been questions about why the /ask page ([https://news.ycombinator.com/ask](https://news.ycombinator.com/ask)) was running dry of Ask HNs - e.g.:

*Ask HN: Why is Ask HN only showing me 14 posts?* - [https://news.ycombinator.com/item?id=49984484](https://news.ycombinator.com/item?id=49984484) - Oct 2026 (47 comments)

*Ask HN: Why is pagination gone on "Ask HN", and showing only 19 items?* - [https://news.ycombinator.com/item?id=50280719](https://news.ycombinator.com/item?id=50280719) - Oct 2026 (3 comments)

I believe I found the problem. The pool of stories for [https://news.ycombinator.com/ask](https://news.ycombinator.com/ask) has traditionally been drawn from the ranked stories list from which we generate the frontpage. That list is restricted to the top 1500 ranked submissions, and it seems the % of submissions which are text posts (i.e. no linked URL) had declined enough to starve the Ask HN feed. The solution is to compute a separate list of ranked Ask HNs.

p.s. "Ask HN" is the conventional name but it means text posts. The title can also start with "Tell HN" or nothing in particular. They're just posts which were submitted without a URL.
