---
title: Introducing Grok Voice 3.0
link: https://x.ai/news/grok-voice-3
source: x-ai
published: 2026-10-14T00:00:00Z
first_seen: 2026-10-18T23:27:08.867657739Z
summary: Oct 14, 2026
content: extracted
html: 2026-10-14-introducing-grok-voice-3-0.html
remote_preview:
  url: https://media.x.ai/cdn-cgi/image/fit=scale-down,onerror=redirect,width=6016,f=auto/v1/website/og-grok-voice-3-0-b281361b.webp
  alt: Grok Voice 3.0
---

Today, we’re announcing Grok Voice 3.0, the world’s most intelligent speech-to-speech model. It sets a new record for resolving customer support calls, has class-leading latency at under one second, and costs less than other frontier models. Grok Voice powers millions of conversations a day across Tesla and SpaceX, and is available today in the API at the same price as its predecessor.

## [The Most Accurate Voice Model](https://x.ai/news/grok-voice-3#the-most-accurate-voice-model)

In Artificial Analysis’s independent testing, Grok Voice 3.0 ranks #1 on the Speech-to-Speech Index, with a score of 86.9. The Index combines speech reasoning, τ-Voice, and preference and task success from blind head-to-head calls. Grok Voice 3.0 also scores 97% on Full Duplex Bench, a measure of how naturally it handles pauses, turn-taking, and interruptions.

Speech-to-speech benchmark comparison between Grok Voice 3.0 and competing models, per Artificial Analysis.
| Benchmark | Grok Voice 3.0 (this release) | Grok Voice 2.0 | GPT-Live-1 (Astra, Medium) | Gemini 3.8 Live (Ext. Thinking, High) |
| --------- | ----------------------------- | -------------- | -------------------------- | ------------------------------------- |
| 86.9      | 81.3                          | 83.2           | 82.6                       |                                       |
| 98.5%     | 97.2%                         | 90.1%          | 97.7%                      |                                       |
| 1070      | 1011                          | 1048           | 990                        |                                       |
| 94.8%     | 94.6%                         | 87.4%          | 89.1%                      |                                       |
| 97.0%     | 95.1%                         | 94.9%          | 91.9%                      |                                       |

τ-Voice measures how well agents handle support calls with diverse accents, background noise, and interruptions. Grok Voice 3.0 scores 71.6%, comparable with GPT-Live-1 (Astra), but at 40% lower latency, setting a new frontier in agent intelligence versus responsiveness.

It's also cheaper to run than comparable models. Artificial Analysis measures what each model costs to complete a task in its Big Bench Audio run, and Grok Voice 3.0 comes in under both GPT-Live-1 (Astra, Medium) and GPT-Realtime-2.1.

Lower cost per task than comparable models

Average cost per task on Artificial Analysis's Big Bench Audio run, USD. Lower is better.

On [EVA-Bench](https://servicenow.github.io/eva), ServiceNow’s benchmark for enterprise voice agents, Grok Voice 3.0 achieves the highest accuracy score at 74% and is the best-performing model when jointly scored across accuracy and customer experience.

> “Grok Voice 3.0 sets a new Pareto frontier on EVA-Bench, ServiceNow’s open-source benchmark scoring voice agents on accuracy and experience across 200+ realistic enterprise scenarios. It pairs top accuracy with a natural, responsive conversation, setting a new standard for what enterprises can expect from AI on the front line of customer service.”

Nitin Aggarwal, Senior Director of Product Management, Agentic Evals and RAI, ServiceNow

## [Used by Millions of Customers at Tesla & SpaceX](https://x.ai/news/grok-voice-3#used-by-millions-of-customers-at-tesla--spacex)

Grok Voice 3.0 handles millions of conversations a day in Tesla vehicles. Drivers can use Connectors for simple requests like checking their calendar or answering an email, and Grok Bot for bigger tasks like ordering a coffee or booking dinner—all completely hands-free ([see it on X](https://x.com/Tesla/status/2102430656349544590)). Grok Voice 3.0 also powers Grok Bot’s voice mode.

Starlink call centers use Grok Voice to answer 20,000 customer calls every day across multiple languages. Roughly 70% are resolved with no human in the loop, and one in four sales calls ends in a purchase before the caller hangs up. A single agent built on Grok Voice 3.0 verifies customers, troubleshoots hardware, issues credits, and ships replacement equipment.

## [Pricing and Availability](https://x.ai/news/grok-voice-3#pricing-and-availability)

Grok Voice 3.0 is available today in the Voice Agent API at [$0.08 / min ($4.80 / hr)](https://docs.x.ai/developers/pricing?campaign=voice-3-blog#voice-api-pricing), the same price as Grok Voice 2.0. Agents using `grok-voice-latest` have already been switched over automatically, with no changes needed.

## Start building with Grok Voice

[Try It Free](https://console.x.ai/team/default/voice/agents?campaign=voice-3-blog&utm_source=website&utm_medium=referral&utm_campaign=voice-3-blog) [View Docs](https://docs.x.ai/developers/model-capabilities/audio/voice?campaign=voice-3-blog)
