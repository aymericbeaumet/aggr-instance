---
title: Among European Companies That Use a CDN, Nearly 9 in 10 Use Cloudflare
link: https://ciphercue.com/blog/european-cdn-concentration-cloudflare-nine-in-ten
source: hnrss-org-frontpage
published: 2026-09-08T08:42:56Z
updated: 2026-09-08T08:42:56Z
first_seen: 2026-09-08T10:44:40.429509Z
authors:
- adulion
summary: 'Article URL: https://ciphercue.com/blog/european-cdn-concentration-cloudflare-nine-in-ten Comments URL: https://news.ycombinator.com/item?id=49607443 Points: 106 # Comments: 106'
content: extracted
html: 2026-09-08-among-european-companies-that-use-a-cdn-nearly-9-in-10-use.html
preview:
  file: 2026-09-08-among-european-companies-that-use-a-cdn-nearly-9-in-10-use.preview-0d936835d9bb.webp
  width: 256
  height: 134
  color: '#212d49'
images:
- source: https://ciphercue.com/img/og-card.png
  original:
    file: 2026-09-08-among-european-companies-that-use-a-cdn-nearly-9-in-10-use.image-a8349b7f92f2.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-08-among-european-companies-that-use-a-cdn-nearly-9-in-10-use.image-5c96037603bd.webp
    width: 48
    height: 25
  color: '#172544'
---

A content delivery network sits in front of a website. Requests hit the CDN's edge servers first, which serve cached content, terminate TLS, filter traffic, and pass the rest back to the origin. That means the CDN is a shared dependency: if it goes down, every site behind it goes down together, whether or not those sites have anything else in common.

I went through CipherCue's tracked European companies to see which CDN fronts their websites. Of the 44,143 companies where we detected a CDN at all, 39,547 are behind Cloudflare. That is 89.6%.

89.6% of 44,143 European companies with a detected CDN sit behind Cloudflare

For a reference point, W3Techs put Cloudflare at 84.1% of sites where the reverse proxy provider can be identified, as of 28 July 2026 ([w3techs.com](https://w3techs.com/technologies/details/cn-cloudflare)). Our 89.6% is measured the same way, as a share of the identified set rather than of all sites, so the two are close; the European cut here runs a little higher.

I only counted companies that actually run a CDN. If a company serves its site straight from its own origin, it is not in these numbers at all. So this is not "Cloudflare vs the entire web"; it is who the CDN users went with.

## First place and everyone else

The four CDN vendors we classify, counted as distinct companies where each was detected:

39,547

Cloudflare

3,112

Amazon

1,299

Fastly

396

Akamai

Distinct European companies where each CDN vendor was detected (a company can appear under more than one vendor)

Amazon is second at 3,112, but that number needs a caveat. We detect Amazon through CloudFront, and Amazon is also a general cloud host, so some of these are companies whose origin happens to sit on AWS rather than companies that deliberately chose CloudFront as their front door. Fastly does not have that ambiguity. It is a pure CDN, and it comes third at 1,299, which is roughly one company for every thirty behind Cloudflare. Akamai, the oldest name in the category, is fourth at 396.

The raw counts add up to more than 44,143 because a company can sit behind more than one vendor and is counted under each (a Fastly front door with an AWS origin, say). That double counting pads the smaller vendors' totals, not Cloudflare's share, which is measured against the whole CDN-using set.

## Per country

Rolling eight countries into one figure hides the differences between them, so here is the same measurement per country. Cloudflare is the majority front door in all of them, but the share ranges from about four in five in Spain and Ireland to about nineteen in twenty in the Netherlands.

| Country        | Companies with a CDN | Behind Cloudflare | Cloudflare share |
| -------------- | -------------------- | ----------------- | ---------------- |
| Netherlands    | 7,939                | 7,587             | 95.6%            |
| United Kingdom | 17,007               | 15,846            | 93.2%            |
| Poland         | 2,896                | 2,682             | 92.6%            |
| France         | 4,008                | 3,456             | 86.2%            |
| Italy          | 3,661                | 3,126             | 85.4%            |
| Germany        | 5,715                | 4,650             | 81.4%            |
| Spain          | 2,001                | 1,576             | 78.8%            |
| Ireland        | 792                  | 624               | 78.8%            |

The UK has the largest absolute count, 15,846 CDN-using companies behind Cloudflare in one market. Germany is the lowest of the big markets at 81.4%, which is still four in five.

## What the concentration costs

Part of why a company buys a CDN is resilience, and one advantage of independent suppliers is that they fail independently. That advantage disappears when most of a market is behind the same supplier. An incident there is no longer one company's outage; it is most of the market's outage, on the same afternoon.

Cloudflare has published postmortems for several incidents in the last fifteen months. Three were global and hit customers:

- **18 November 2025**, roughly 11:20 to 17:06 UTC. A database permissions change caused a Bot Management feature file to fill with duplicate entries and double in size, past a limit the proxy enforced, which crashed core CDN and security serving. Cloudflare's writeup states the outage was "not caused, directly or indirectly, by a cyber attack or malicious activity of any kind" ([blog.cloudflare.com](https://blog.cloudflare.com/18-november-2025-outage/)).
- **5 December 2025**. A configuration change applied while mitigating an industry-wide React Server Components vulnerability caused a global disruption ([Cloudflare outage postmortems](https://blog.cloudflare.com/tag/outage/)).
- **20 February 2026**, from 17:48 UTC, lasting 6 hours 7 minutes. A cleanup automation task read a buggy API response as an instruction to withdraw BYOIP prefixes, and 25% of them were pulled from the internet via BGP. Cloudflare again states it was "not caused, directly or indirectly, by a cyberattack or malicious activity of any kind" ([blog.cloudflare.com](https://blog.cloudflare.com/cloudflare-outage-february-20-2026/)).

None of the three was an attack. A doubled config file, a change made while patching someone else's vulnerability, a cleanup job that deleted too much: routine internal work that reached the edge and took a lot of sites down with it. The companies in the table did not need anything in common to go offline together on 18 November. Sharing a front door was enough.

This is not about Cloudflare being sloppy. Fastly and Akamai ship bugs like this too; theirs just take fewer sites down because fewer sites are behind them. That is the whole point. When one provider fronts most of a market, its mistakes stop being its own problem and become everyone's, at the same time.

## The CDN is not where the data lives

This says nothing about where a company keeps its data. The CDN is just the front door; the origin server, the database, and whoever processes the data sit behind it, and for a GDPR or data-residency question that back end is what counts. Measure that instead and Europe looks better: people who dig into API subdomains rather than the front door usually turn up far more OVH and Hetzner than I see here. I stuck to the front door because it is the part that goes down for everyone at once, which is what happened in the outages above.

## Check your own front door

You can see which CDN, if any, fronts a domain from its response headers:

```
curl -sI https://example.com | grep -i 'server\|cf-ray\|x-served-by\|x-amz-cf-id'
```

A `cf-ray` header or `server: cloudflare` indicates Cloudflare. `x-served-by` with a Fastly cache node indicates Fastly. `x-amz-cf-id` indicates Amazon CloudFront. No such header, and a `server` value naming your own web server or origin host, usually means no CDN in front.

## Method note

**Source and cohort:** CipherCue's own observations of European company websites (HTTP response fingerprinting and DNS), not a third-party dataset. The cohort is the companies in our tracked entity set with a country attribution in one of Germany, the United Kingdom, the Netherlands, Poland, France, Italy, Spain, or Ireland, and at least one CDN component detected. That gives 44,143 companies, using the latest observation per company as of 2026-09-07. This is a company count within our dataset, not a representative sample of every company in these countries, and not a claim about companies that use no CDN at all. The cohort also skews toward small and mid-sized companies rather than large enterprises, and Cloudflare's free tier is strongest in exactly that segment, so read the figure as concentration among CDN-using companies of this mix, not as a statement about large enterprises specifically.

**What "detected a CDN" means:** we classify a component as a CDN when the response headers or serving fingerprint match a known CDN vendor's pattern (for example a `cf-ray` header for Cloudflare, a Fastly cache-node `x-served-by`, a CloudFront `x-amz-cf-id`). Companies that serve directly from an origin with no recognised CDN fingerprint are excluded from the denominator. A misconfigured or unusual setup that hides these headers would be undercounted.

**Double counting:** a company can be detected behind more than one CDN vendor and is then counted under each. The Cloudflare share (89.6%) is Cloudflare-detected companies divided by all companies with any CDN detected, so a company behind both Cloudflare and Fastly counts once in the numerator and once in the denominator. Raw per-vendor counts therefore sum to more than 44,143.

**The Amazon caveat:** Amazon is detected via CloudFront, but Amazon is also a general cloud host. Some Amazon detections are front-door CloudFront choices and some are AWS-origin serving; we do not separate the two here, which is why Fastly (an unambiguous pure CDN) is called out as the nearest clean comparison.

**Outage details** are from Cloudflare's own published postmortems, cited inline. Times and durations are as stated by Cloudflare. We link the primary source for each rather than restating figures from secondary coverage.

**Country grouping** is our choice and is stated explicitly so the per-country table can be read on its own; the headline figure is the union of these eight markets, not a single country.

## Finding this in your own market

CipherCue tracks CDN, hosting, email, and DNS infrastructure per company, filterable by vendor, country, and sector. If you sell a European alternative in one of these categories, the directory shows which companies in your market currently run which incumbent: the [companies we detect behind Cloudflare](https://ciphercue.com/directory/companies-using/cloudflare) are listed there, and the [EU vendor directory](https://ciphercue.com/directory/eu) covers the alternatives by category.
