---
title: CrowdSec Source Code Leak
link: https://www.crowdsec.net/blog/crowdsec-statement-source-code-exposure
source: hnrss-org-frontpage
published: 2026-09-17T15:34:01Z
updated: 2026-09-17T15:34:01Z
first_seen: 2026-09-17T23:03:54.779334643Z
authors:
- eccgecko
summary: 'Article URL: https://www.crowdsec.net/blog/crowdsec-statement-source-code-exposure Comments URL: https://news.ycombinator.com/item?id=49742355 Points: 118 # Comments: 34'
content: extracted
html: 2026-09-17-crowdsec-source-code-leak.html
preview:
  file: 2026-09-17-crowdsec-source-code-leak.preview-85849e77a410.webp
  width: 256
  height: 142
  color: '#06204a'
images:
- source: https://cms.crowdsec.net/wp-content/uploads/2026/09/Blog-images-1575-%C3%97-871px-32-1.png
  original:
    file: 2026-09-17-crowdsec-source-code-leak.image-30bcede30746.png
    width: 1575
    height: 871
  variants:
  - file: 2026-09-17-crowdsec-source-code-leak.image-ad8ca6dfac20.webp
    width: 320
    height: 177
  - file: 2026-09-17-crowdsec-source-code-leak.image-f30e246431fa.webp
    width: 640
    height: 354
  - file: 2026-09-17-crowdsec-source-code-leak.image-4df7879af153.webp
    width: 960
    height: 531
  - file: 2026-09-17-crowdsec-source-code-leak.image-4b3cf9477d6e.webp
    width: 1280
    height: 708
  - file: 2026-09-17-crowdsec-source-code-leak.image-c55d6cefba5d.webp
    width: 1575
    height: 871
  color: '#001b47'
---

On September 16, CrowdSec was informed of a source code leak involving our GitHub repository, which occurred in May 2026. Our team verified and confirmed the report. CrowdSec source code consists of two parts: a private one and another that hosts our Free Open Source Software (i.e., the Security Engine), which is public by design and therefore out of scope. The private part, though, contains the source code for our SaaS console, some AWS Cloud routines, some connectors, and automations.

The news headline claiming 300 different repositories is accurate (when you include the 130+ public ones), though that number mostly reflects the code’s subdivision rather than a specific volume. We do not confirm any “other file contained” or “internal development material”, since all the code is published in these repositories. The API related information is the token used by the CI/CD component itself. (see below)

**No client data, login/password, name, organization, or anything else was leaked, and CrowdSec doesn’t store PII or client logs; the impact is limited to CrowdSec.** Our team quickly hunted for any token, credential, or sensitive leak that could enable lateral movement but found none so far.

The code contained in these private repositories has value but cannot really harm CrowdSec, since our efficiency depends on our network effect and size, which code alone can’t replicate. We regularly audited the SaaS source code, and its leakage shouldn’t pose an immediate threat either. Most of the leaked code has evolved significantly over those four months, but we will closely monitor for any abnormal activity. Also, using it outside of CrowdSec seems unlikely because it only interacts with our data and tools and cannot really be leveraged in another context.

We will keep you updated as we continue investigating, but the Tanstack compromise is very likely to have been the leak vector (more about it [](https://www.linkedin.com/safety/go/?url=https%3A%2F%2Flnkd%2Ein%2FexcxwaYB&urlhash=Emf0&mt=XLvZe-Pes35gjZ58eNxDgZVAa7Lnzn0uf93ny13vK_ah4evS_ZzU3Qve94fOKfQ_aLu-N4gw-WjoiBEKfGlhIfZrxOCDvwrcSZyjAGoyZcGE9dbrDRFFLQ&isSdui=true)\
[here](https://cybelangel.com/blog/attaque-mistral-ai-comment-la-compromission-tanstack-a-contamine-le-sdk/)), as in the case of the Mistral AI case. This component was used in our organization in May and appears to have been backdoored to extract an API key with authorization to read the private codebase. The leak was only exploitable during a short timeframe in May 2026.

We nevertheless immediately rotated all required tokens & credentials to prevent further incidents.

The team would like to thank [Fuites Infos](https://fuitesinfos.fr/) for their timely, professional outreach in reporting the issue.
