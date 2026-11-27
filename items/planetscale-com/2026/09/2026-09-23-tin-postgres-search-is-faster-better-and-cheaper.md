---
title: TIN Postgres search is faster, better, and cheaper
link: https://planetscale.com/blog/searching-hn-with-tin
source: planetscale-com
published: 2026-09-23T00:00:00Z
updated: 2026-09-23T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Patrick Reynolds
labels:
- engineering
- postgres
summary: A good Postgres search demo was doing the rounds online. Let's see what happens when we upgrade it to TIN.
content: extracted
html: 2026-09-23-tin-postgres-search-is-faster-better-and-cheaper.html
remote_preview:
  url: https://planetscale.com/assets/searching-hn-with-tin-social-Bz4KSkhS.png
---

Last week [Rishi Raj Jain](https://x.com/rishi_raj_jain_) built an app to search Hacker News posts and comments using Postgres full-text search, hosted on Neon Lakebase. It's a good app and a good [demo video](https://x.com/rishi_raj_jain_/status/2099387257354584289) except for one thing: this tilde.

Why can't Lakebase provide an exact count? For that matter, why should it take over 3.6 seconds to rank what Lakebase claims is just 3,400 documents?

I knew [TIN](https://planetscale.com/docs/postgres/search) could do a better job than that, so I forked [Rishi's app](https://github.com/rishi-raj-jain/hn-search) and started building. We've already shown that [TIN is really fast](https://planetscale.com/blog/introducing-tin), but sometimes being fast gives you the space to build more interesting features, too. Let me show you what I built.

## [Stop debouncing](https://planetscale.com/blog/searching-hn-with-tin#stop-debouncing)

First thing to do is stop debouncing keystrokes. Rishi's original app, on the left, waits 250ms after each keystroke before it even begins the search. The TIN version, on the right, searches immediately after every keystroke. Postgres with TIN can comfortably handle immediately sending off the query at every change, since it is so much faster.

## [Autocomplete](https://planetscale.com/blog/searching-hn-with-tin#autocomplete)

TIN supports wildcard searches. So in my app, I made any incomplete final word a wildcard: typing `planetscale datab` in the textbox actually searches for `planetscale datab*`, which naturally matches `planetscale database`. (Also: `datablindnes`, `Databall`, and `DATAbEEF`, but not in a conjunction query with `planetscale`.)

Autocomplete is nice if you don't know how to spell a word or just want to avoid typing the whole thing.

## [Fuzzy matching](https://planetscale.com/blog/searching-hn-with-tin#fuzzy-matching)

TIN supports fuzzy matching, specifically [Levenshtein edit distance](https://en.wikipedia.org/wiki/Levenshtein_distance). In my version of the app, if the search as you've typed it returns too few results overall, the app performs a `COUNT(*)` search for each term individually, then allows an edit distance of two for the least popular term. If that still doesn't find enough results, it repeats the process until all terms are fuzzed, if necessary. So here, I've typed `planetscale databse`, and because `databse` isn't a real word (it's found ten times across the whole history of Hacker News), we end up searching for `planetscale databse~2`. The results show you which word(s) got fuzzed.

## [Stop words](https://planetscale.com/blog/searching-hn-with-tin#stop-words)

Most index implementations, including Lakebase, encourage developers to configure their index to ignore the hundred-or-so most common English words, called **stop words**. That's a trade-off: smaller, faster indexes, but no way to search for common words. TIN is fast enough that it can afford to just index everything. Good luck searching for "to be or not to be" or "The Who" if your index contains none of those words.

Blink and you might miss it: as I was typing, TIN searched for `to b`, counted 1,054,718 results, and ranked the top 30 of them in 11ms.

## [OK, about that tilde](https://planetscale.com/blog/searching-hn-with-tin#ok-about-that-tilde)

TIN is [especially well optimized](https://planetscale.com/blog/introducing-tin#work-elision-and-vectorization) for [`COUNT(*)` queries](https://planetscale.com/blog/introducing-tin#when-the-index-fits-in-memory). For even very large numbers of results, even on complicated multi-term queries, TIN can provide an exact count in just a few milliseconds. Here's a search for `show hn`, the example from Rishi's original demo. Lakebase takes 3.6 seconds and estimates there are 3,400 results. Actual count: exactly 213,447.

## [How much would you expect to pay?](https://planetscale.com/blog/searching-hn-with-tin#how-much-would-you-expect-to-pay)

Rishi's original app [provisions](https://github.com/rishi-raj-jain/hn-search/blob/37bb45d9c73233107bc768a7306b483d09b16846/neon.ts#L9-L11) a Neon instance with 32 CUs, which requires the [Scale plan](https://neon.com/pricing). It also configures the instance to never sleep, so the cost continues all month long: 32 × 730 × $0.222 ≈ $5,186.

The [TIN version](https://github.com/planetscale/hn-search/tree/piki/tin-can) uses an HA cluster of three [M-160](https://planetscale.com/pricing?engine=postgres&architecture=x86-64&cluster=M-160&storage=D-METAL-118&backup=20) instances, specifically M-160s instances with x86-64 CPUs and 118 GB of NVMe storage each.

|               | Lakebase     | TIN   |
| ------------- | ------------ | ----- |
| Instance size | Scale: 32 CU | M-160 |
| CPU cores     | 32           | 2     |
| RAM           | 128 GB       | 16 GB |
| Monthly price | $5,186       | $609  |

TIN is faster and can implement many more useful features, for less than 1/8 the price.

If you like better, cheaper, faster, we have the technology. [Get started with TIN search for Postgres](https://planetscale.com/docs/postgres/search/get-started).
