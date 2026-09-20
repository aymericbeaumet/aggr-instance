---
title: ChatGPT now knows what you do on other websites via ad collector
link: https://www.buchodi.com/chatgpt-now-knows-what-you-do-on-other-websites-via-ad-collector/
source: hnrss-org-frontpage
published: 2026-09-20T15:18:44Z
updated: 2026-09-20T15:18:44Z
first_seen: 2026-09-20T19:43:52.083046544Z
authors:
- lmbbuchodi
summary: 'Article URL: https://www.buchodi.com/chatgpt-now-knows-what-you-do-on-other-websites-via-ad-collector/ Comments URL: https://news.ycombinator.com/item?id=49776729 Points: 268 # Comments: 129'
content: extracted
html: 2026-09-20-chatgpt-now-knows-what-you-do-on-other-websites-via-ad.html
preview:
  file: 2026-09-20-chatgpt-now-knows-what-you-do-on-other-websites-via-ad.preview-220b2d8d6581.webp
  width: 256
  height: 194
  color: '#f8f7f7'
images:
- source: https://storage.ghost.io/c/b8/53/b853e3d4-3186-409d-9c7f-7da931a60431/content/images/2026/09/Screenshot-2026-09-20-at-5.00.35---PM.png
  original:
    file: 2026-09-20-chatgpt-now-knows-what-you-do-on-other-websites-via-ad.image-e968c4ff1b33.png
    width: 2346
    height: 1776
  color: '#fcfcfc'
---

OpenAI's ad collector at `bzr.openai.com` sets a cookie called `__obi`, scoped to `.openai.com`. The value is while you are on ChatGPT and tied to your ChatGPT account. `__obi` is then sent to OpenAI from ordinary websites you visit.

Any company that buys ads on ChatGPT installs a [small piece of OpenAI code](https://developers.openai.com/ads/measurement-pixel?ref=buchodi.com) on its own site, the same way retailers already install Meta and Google tracking code. Loading that code, sends `__obi` to OpenAI along with data about the page you are browsing. This includes products you are searching for, articles you are reading, and purchase behaviors.

The bottom line is that OpenAI can connect what you do on those sites to your ChatGPT account.

I reproduced the full mechanism on my own phone, verified with two independent capture methods, and cross-checked against several months of observed traffic covering 936 distinct advertiser pixels across 1,029 hostnames.

![](https://storage.ghost.io/c/b8/53/b853e3d4-3186-409d-9c7f-7da931a60431/content/images/2026/09/Screenshot-2026-09-20-at-5.00.35---PM.png)

## How it works

**Step 1. ChatGPT creates an identifier and signs it.**

On `chatgpt.com`, the client generates 16 random bytes and calls `POST /backend-api/bazaar/obi/sync-token` (or `/backend-anon/` when signed out). The backend returns an RS256 JWT:

```
{
  "iss": "chatgpt-wadi",
  "aud": "bzr.openai.com",
  "purpose": "obi_sync",
  "operation": "set",
  "consent_decision": "analytics_allowed",
  "consent_policy_version": "user_granular_consent_v1",
  "sub": "«redacted: 64-hex account subject»",
  "subject_type": "account_user",
  "obi": "«redacted: 22-char identifier»",
  "exp": "«iat + 60s»"
}
```

`sub` is the account. `obi` is the identifier. The token binds them, is scoped to the collector, and expires in 60 seconds. `bzr` stands for `bazaar`, OpenAI's internal name for the ads platform; `wadi` is the issuing service.

**Step 2. The identifier becomes a cookie on OpenAI's domain.**

The client POSTs `{"token": "«JWT»"}` cross-site to `bzr.openai.com/v1/obi/sync`. The response:

```
Set-Cookie: __obi=«redacted»; Domain=.openai.com; HttpOnly;
            Max-Age=31536000; Path=/; SameSite=none; Secure
```

`SameSite=none` with `Secure` is the configuration a cookie needs to be sent on cross-site requests. `Max-Age` is one year. The `obi` value in the JWT and the value in the cookie are identical.

**Step 3. Advertiser sites send it back.**

Three request classes go from an advertiser's page to OpenAI's hosts. On a phone with `__obi` in the jar, all three carried it:

| Request                                          | Carried `__obi`         | Notes                           |
| ------------------------------------------------ | ----------------------- | ------------------------------- |
| `GET bzrcdn.openai.com/sdk/oaiq.min.js`          | yes                     | the script load itself          |
| `POST bzr.openai.com/v1/sdk/events` with `obref` | yes                     | conversion events               |
| `POST bzr.openai.com/v1/sdk/events`, bare body   | yes                     | the SDK's "no credentials" path |
| `GET bzrcdn.openai.com/pixel-config/…`           | no cookie header at all | control                         |

The first row is particularly interesting. The pixel SDK has a code path that omits credentials, and it does not help: the browser attaches cookies to the `<script src>` request that loads the SDK before any of OpenAI's code runs. By the virtue of loading the tag the identifier is disclosed.

## What travels with it

The same SDK also collects identity from the advertiser's page. The payload separates four sources, labelled by OpenAI itself: `in` for values the advertiser passes deliberately, and `fm`, `ht`, `js` for values the SDK scrapes from form fields, rendered page text, and the tag-manager bus. In observed traffic, scraped identity outnumbered advertiser-supplied identity 685 events to 255.

The tag-manager bus is the largest source of email. The SDK replaces `window.dataLayer.push` with its own function, also reads `adobeDataLayer`, and locates renamed GTM layers by parsing the `l=` parameter off the `gtm.js` script tag. Current versions take email and phone from it. Version 0.1.31 also took names and geography before the scope was narrowed on 27 August.

Email, phone, first and last name are SHA-256 hashed before transmission. Country, region, city and postal code are sent in the clear. Postal code was the most-harvested form field, 100 events across 28 sites.

URLs are reduced to origin plus path before sending; none of 23,929 observed carried a query string. Paths survive, and paths reaching the collector included a medical condition, a debt-solutions funnel and a litigation intake form.

Automatic matching was enabled for 638 of 881 pixels with a known setting, including every credit and lending advertiser observed. It is controlled from OpenAI's Ads Manager. A denylist excludes passwords, one-time codes, card numbers, SSN, date of birth, medical history, diagnosis and court fields.

## The cookie is built to cross sites

On the same advertiser-page requests, every other OpenAI cookie was blocked by the browser:

| Cookie                                  | Outcome                  |
| --------------------------------------- | ------------------------ |
| `oai-did`, `oaicom-stable-id`           | blocked, `SameSite=Lax`  |
| `oai-client-auth-info`, session cookies | blocked, domain mismatch |
| `__obi`                                 | sent                     |

`__obi` is the only OpenAI identifier configured with `SameSite=None`.

## Observed reach

On my device, one `__obi` value was sent to OpenAI from 12 commercial websites under 13 distinct pixel IDs, including Chewy, Wayfair, ThriftBooks, Eventbrite, HelloFresh, Coursera and SeatGeek. Every request was accepted with `202`.

In the broader traffic, 12 of 30 distinct `__obi` values appeared under more than one advertiser, one under ten.

## It works when you are logged out

Across 932 decoded sync tokens, 736 carried `subject_type: account_user` and 196 carried `anonymous`. The anonymous subject is as stable as the account subject: one per device, persisting at least 27 days.

## What OpenAI's cookie policy says

OpenAI's [cookie policy](https://openai.com/policies/cookie-policy/?ref=buchodi.com) lists `__obi` under Analytics cookies, one year, on `chatgpt.com` and `openai.com`. It is the only entry in that section. The policy describes analytics cookies as helping OpenAI understand how its services perform and are used.

OpenAI runs analytics and marketing as two separate consent choices, `oai_consent_analytics` and `oai_consent_marketing`, and every sync token I decoded carried `consent_decision: analytics_allowed`. Someone who allows analytics and refuses marketing gets this.

## OpenAI's response

I sent the mechanism and two questions to [press@openai.com](mailto:press@openai.com) and [privacy@openai.com](mailto:privacy@openai.com) on 14 September: why `__obi` is classified as an analytics cookie, and whether a user who grants analytics consent and refuses marketing consent still receives it. The reply came from OpenAI Support. It acknowledged the inquiry, said the observations would be shared internally for review, and did not answer either question. The script-load observation above was made after the inquiry was sent. I will update this post if OpenAI responds.

## Limits

**Browsers.** Observed on Chrome for Android. [Safari's Intelligent Tracking Prevention](https://webkit.org/tracking-prevention/?ref=buchodi.com) blocks all third-party cookies, and Chrome on iOS runs on WebKit, so the mechanism does not operate on any iOS browser. Desktop Chrome is untested.

**Gating.** Roughly one ChatGPT session in five produced a sync token. ChatGPT's mobile web client serves ads without syncing at all. Someone following the steps below may see the pixel fire with no cookie attached.

**The join is not observed.** `202` means the collector accepted the event with the cookie attached. That OpenAI resolves it to the account server-side follows from the design; I did not watch it happen.

**Meta built the structural equivalent years ago.** A logged-in account, third-party cookies on pixel fires, off-site conversions resolved to a profile. The mechanism is standard adtech. What has no precedent is running it on an AI chat product. People tell these products things they would not put on a social network, and these products increasingly act on their behalf.

**The pixel's other cookie does not do this.** `__obref` is set on the advertiser's own domain. Each site gets a different value and no site can see another's. Of 2,860 values observed, 2,828 appeared under exactly one advertiser.

**Advertisers cannot see this.** `__obi` belongs to a domain their scripts cannot read. They installed a conversion pixel and have no way to know their visitors are being resolved to a ChatGPT identity.
