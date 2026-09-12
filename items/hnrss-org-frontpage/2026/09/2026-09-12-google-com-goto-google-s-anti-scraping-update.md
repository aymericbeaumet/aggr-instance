---
title: 'google.com/goto: Google''s anti-scraping update'
link: https://www.autom.dev/blog/google-search-goto-links
source: hnrss-org-frontpage
published: 2026-09-12T03:14:20Z
updated: 2026-09-12T03:14:20Z
first_seen: 2026-09-12T19:37:44.082475059Z
authors:
- 1e1a
summary: 'Article URL: https://www.autom.dev/blog/google-search-goto-links Comments URL: https://news.ycombinator.com/item?id=49668386 Points: 575 # Comments: 455'
content: extracted
html: 2026-09-12-google-com-goto-google-s-anti-scraping-update.html
preview:
  file: 2026-09-12-google-com-goto-google-s-anti-scraping-update.preview-e8979450615e.webp
  width: 256
  height: 134
  alt: 'google.com/goto: Google''s anti-scraping update'
  color: '#f6f7f8'
images:
- source: https://www.autom.dev/api/og?slug=google-search-goto-links&locale=en
  original:
    file: 2026-09-12-google-com-goto-google-s-anti-scraping-update.image-a1ba44c088ad.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-12-google-com-goto-google-s-anti-scraping-update.image-092ad4a25af8.webp
    width: 48
    height: 25
  color: '#fcfdfe'
---

## What's happening

Google Search is rewriting organic result links to `google.com/goto?url=...` instead of exposing the destination URL directly in the HTML.

When you click a result, Google redirects you to the real page. The `url` parameter uses a custom, Google-specific encoding. It is not a plain base64 of the target URL. In practice, it looks like an opaque reference to Google's index record for that page.

As of late August 2026, this is showing up consistently across searches when you are logged out or browsing in private mode. It may still be an experiment, but it is no longer limited to a small slice of SERPs.

## Not the same as google.com/url

Google has used redirect wrappers before. The older format is `google.com/url?q=[URL-encoded destination]`, where the target link is readable in the query string.

The new `goto` format is different:

- The result `href` is `/goto`, not the destination
- You cannot decode the `url=` blob offline
- The real URL is in the `Location` header on `/goto`. Request that URL. Do not follow the redirect.

Google still needs the destination to draw the SERP (domain, favicon, attribution), so copies of the URL remain on the page. That is a separate story from reading `Location`. The walkthrough is here: [google.com/goto: read Location with HEAD](https://www.autom.dev/blog/google-goto-url-fix).

That shift matters for anyone building a search index from SERP data at scale.

## Why Google is doing this

This fits Google's broader push against automated SERP harvesting, especially from AI crawlers and SEO scrapers that bulk-extract result URLs to build their own indexes.

With plaintext links, a scraper could parse thousands of URLs from HTML without touching Google again. With `goto`, each result needs a request back to Google just to learn the destination. You read `Location`; you do not follow through to the page. That is slower, noisier, and gives Google a clear signal when the same client resolves hundreds of links in sequence.

Combined with earlier moves like removing `&num=100` and tightening BotGuard/SearchGuard, Google is steadily raising the cost of naive SERP scraping.

## What we saw at Autom

We first spotted `goto` links on a small percentage of SERPs. At that level, it was hard to ship a reliable fix without breaking responses for everyone else.

As of late August 2026, the pattern is much more consistent for logged-out and private sessions. Result URLs on Google Search are effectively all `goto` in those conditions.

We have been monitoring the rollout and testing against it.

## Update at Autom.dev

**We have updated our Google Search pipeline** to resolve `google.com/goto` links (read `Location`, no follow) and return the final destination URL in API responses, in the same structured fields customers already use.

If you call Autom's Google Search endpoints, you should keep getting usable destination URLs without changing your integration. We will keep watching Google's rollout and adjust if the redirect format shifts again.

## Related reading

- [google.com/goto: read Location with HEAD](https://www.autom.dev/blog/google-goto-url-fix)
- [Google killed num=100](https://www.autom.dev/blog/google-num-100-removed)
- [Google sues SerpAPI: What SearchGuard reveals](https://www.autom.dev/blog/google-searchguard-serpapi-lawsuit)
- [Scraping SERP with Google, Bing, and Brave](https://www.autom.dev/blog/scraping-serp-google-bing-brave-openclaw)

Need live SERP data while Google keeps moving the goalposts? Try 1,000 free requests on [Autom pricing](https://www.autom.dev/pricing), or get an API key at [app.autom.dev/register](https://app.autom.dev/register).
