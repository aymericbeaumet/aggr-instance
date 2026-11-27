---
title: Novita, StreamLake, etc. are ByteDance Ark wrappers
link: https://kendell.dev/blog/arkwrapper/
source: hnrss-org
published: 2026-11-26T21:59:19Z
updated: 2026-11-26T21:59:19Z
first_seen: 2026-11-27T13:45:44.456295755Z
authors:
- leafingensemble
content: extracted
html: 2026-11-26-novita-streamlake-etc-are-bytedance-ark-wrappers.html
---

**DeepSeek tokens on routes traced to ByteDance Ark on Nov 21**, Nov 1, 2026 to Nov 24, 2026 · Novita, StreamLake, Ant Digital · selected models only (lower bound)

- Novita
- StreamLake
- Ant Digital

Weekly table

| Week of           | Novita | StreamLake | Ant Digital |
| ----------------- | ------ | ---------- | ----------- |
| Nov 1, 2026       | 685B   | 565B       | 0           |
| Nov 8, 2026       | 1.1T   | 429B       | 0           |
| Nov 15, 2026      | 730B   | 366B       | 43B         |
| Nov 22, 2026 (3d) | 430B   | 50B        | 219B        |

Sit back, but you probably won't be able to relax - this one's a doozy. First, , who looks like a standard US ZDR inference provider at first:

[Zero Data Retention is the default for all endpoints, including serverless API calls.](https://trust.novita.ai/faq?search=What+data+do+you+collect%2C+and+for+how+long+is+it+retained%3F)

But the more you look, the weirder it gets. Why did OpenRouter drop their ZDR status sometime before 2026-11-20 22:40 UTC? Why is their family tree so complicated?

- 🇰🇾 PPLabs Technology Limited
  - 🇭🇰 PPLabs HK
    - 🇨🇳 Shanghai DAO
      - 🇨🇳 PPIO Shanghai (via VIE) [PPIO 派欧云](https://ppio.com)
        - 🇨🇳 Generative Era
  - 🇸🇬 SWARMNET Singapore
  - 🇺🇸 Hivemind Labs, Inc. [Novita AI](https://novita.ai)

And who are their subprocessors? Many ([45.8%](https://x.com/KTibow/status/2104285290429116759)) think they would have none, since most OpenRouter providers host models themselves. And PPLabs's draft IPO assures:

[A defining feature of our model API services is that we do not simply provide a pass-through gateway to underlying models.](https://web.archive.org/web/20260927181216/https://www1.hkexnews.hk/app/sehk/2026/108628/documents/sehk26061000116.pdf#page=13)

But ["Who are your sub-processors?"](https://trust.novita.ai/faq?search=Who+are+your+sub-processors%3F) details "Model provider" subprocessors. But it promises a ["full and up-to-date sub-processor list"](https://trust.novita.ai/subprocessors). But this list doesn't include any model subprocessors. But PPIO clearly implies the existence of subprocessors:

[You understand and agree that, \[for\] the large-model API product, third-party technical-support providers provide the large-model API. PPIO makes no warranty of any kind regarding the large-model API, whether express or implied, including but not limited to its authenticity, suitability, non-infringement and security. Furthermore, regarding the third-party technical-support providers' large-model API, PPIO will not participate in the development and operation of the large-model API; PPIO only provides a neutral platform and related technical services.](https://web.archive.org/web/20260909113349/https://ppio.com/legal/terms-of-service#:~:text=8%2E6)

I also contacted their support and privacy contacts, and all they told me as of writing is that models labeled as "Partner" may go to Qwen and InclusionAI. You can't get a straight answer.

What about ? It's Kuaishou's model platform, and it looks like a host too. But some of its error messages aren't StreamLake's. They're word for word Novita's gateway errors: `invalid request error trace_id: …`. And on GLM 5.3 Flash, it sometimes returns a tool-call id format (`call_` + 16 hex digits) that only Novita produces, out of 41 hosts. Why would StreamLake's requests come back from Novita?

It keeps going. passes through StreamLake's own request ids (`req-` + 6 characters + 19 digits) on V4.1 Flash, V4 Pro and GLM 5. And then there's , a Singapore subsidiary of Ant Group that listed DeepSeek V4.1 Flash on November 18 at "60% off", with ZDR (until OpenRouter removed it on November 22), and went from 1B tokens a day to 35.8B on November 21. Where is all this actually going?

## The MiniMax investigation

You might've already seen OpenRouter's notice that requests sent to MiniMax models "may have been read". A security researcher put unique URLs in prompts, and a third party opened them 40 to 130 hours later. This happened even with non-MiniMax providers:

> AtlasCloud, Novita and StreamLake have told us they forwarded OpenRouter requests for MiniMax models to MiniMax.

So the forwarding is admitted. [MiniMax's own notice](https://www.minimax.io/blog/update-on-url-access-in-forwarded-api-requests-20261124) names the same three and says its security system, on by default, opened URLs from forwarded requests, ZDR ones included. [Novita's notice](https://novita.ai/blog/minimax-requests-privacy-notice/) calls it a "routing configuration issue" on a path "intended for specific customers", and promises better "Partner API labeling", the same "Partner" label its support told me means a request goes to Qwen or inclusionAI. Since then, 16 of Novita's endpoints have come off OpenRouter: every Qwen and inclusionAI model, plus StepFun, Tencent and all but one Kimi.

But MiniMax is a very small model series. What about DeepSeek?

## It goes to ByteDance

Ark is ByteDance's model API: Volcano Engine Ark in mainland China, BytePlus ModelArk everywhere else. It sells DeepSeek, GLM and Kimi, and it leaves fingerprints:

- **Who downloads your images.** StreamLake's V4.1 Flash images are downloaded from Beijing Volcano Engine (AS137718), ByteDance's cloud. Ant Digital's pass through Ant's own gateway on Alibaba Cloud, then get downloaded by `101.47.5.59`, BytePlus in Singapore. Novita's own API used to do the same (`124.174.46.139`, Volcano Engine, on November 12), until Novita started downloading images itself first, from AWS.
- **Request IDs.** Every Ant Digital response's upstream ID is a ByteDance one: `02` + a millisecond timestamp + hex.
- **Tool-call IDs.** Novita's and StreamLake's V4.1 Flash return Ark's format (`call_00_` + 24 lowercase letters and digits).
- **Censorship.** Ask StreamLake's V4 Pro 0813 who Tank Man was, or Novita's GLM 5.1 about June 4, 1989, and you can get ByteDance's canned refusal: 你好，我无法给到相关内容。 ("Hello, I can't provide related content.")
- **Price.** Ant Digital launched at $0.12/$0.48 per million tokens, exactly BytePlus's off-peak "Economy" rate for that model. By November 25 it was down to $0.09/$0.36, below BytePlus's own list price.

Other tests, which I won't detail here, put Novita's V4 Pro 0813 and V4.1 Flash 1017 on BytePlus in Singapore and its V4.1 Flash on Volcano Engine in Beijing, as of November 21. That's the chart at the top: Novita alone sent 120-160B tokens a day on these models until it lost its ZDR label on November 20, then 33B on November 21. Routes move, though; one of those changed within an afternoon, and Novita's V4.1 Flash 1017 had stopped showing Ark's fingerprints by November 24. As for the weird errors, part of StreamLake's traffic goes through Novita on its way to ByteDance. So a V4.1 Flash request to SiliconFlow can go SiliconFlow → StreamLake → Novita → ByteDance.

Bonus: Novita's "DeepSeek V4 Pro" isn't the original V4 Pro. On Novita's own API, it sometimes answers with DeepSeek's `system_fingerprint` for the 0813 checkpoint (`d400d062…`). ByteDance sells 0813 as "DeepSeek-V4-Pro正式版" (V4 Pro GA) and marks the original as about to be retired.

Why bother? Price. BytePlus advertises savings plans of 30-50% off for high volume, and the clouds come to you: [Morph's Tejas Bhakta posted](https://x.com/tejasybhakta/status/2124195706479202377) a Tencent Cloud email offering "up to 50-60% off" to route requests to them.

Attested proof of the image downloads, request IDs, tool-call IDs and refusals is available [here](https://github.com/KTibow/the-ark-of-deepseek-v4.1-flash/releases/tag/probe-44960395726) (run [44960395726](https://github.com/KTibow/the-ark-of-deepseek-v4.1-flash/actions/runs/44960395726) on [6dbc036](https://github.com/KTibow/the-ark-of-deepseek-v4.1-flash/commit/6dbc036f555e94966ab36a8747fe452504193438)). It asks every OpenRouter host of DeepSeek V4.1 Flash, so you can compare against everyone else too.

## Disclosures

While a grant from OpenRouter helped with the research here and I work for an employer who would prefer to go unnamed, **this post does not represent or reflect anyone else's opinion** - it is simply research I am publishing.
