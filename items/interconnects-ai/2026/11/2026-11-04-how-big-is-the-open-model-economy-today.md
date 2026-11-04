---
title: How big is the open model economy today?
link: https://www.interconnects.ai/p/how-well-can-we-measure-open-model
source: interconnects-ai
published: 2026-11-04T16:35:44Z
updated: 2026-11-04T16:35:44Z
first_seen: 2026-11-04T21:49:17.908350494Z
authors:
- Nathan Lambert
summary: Our first open model economy research note for paid subscribers.
content: extracted
html: 2026-11-04-how-big-is-the-open-model-economy-today.html
remote_preview:
  url: https://substackcdn.com/image/fetch/$s_!hFZ7!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6c5f2798-10bc-487a-8932-5f851ad61b24_1600x840.svg
---

Our first open model economy research note for paid subscribers.

My longest running agent testbed has been building the [Interconnects Dashboard](https://dashboard.interconnects.ai/) — an aggregator of all of the best data on open model adoption and performance from across the web. This dashboard is freely available and becoming quite useful. Still, all the measurements we have on open model adoption are incredibly imprecise. We have noisy platform metrics, marketing-related metrics from startups, and imperfect public evaluations. It takes an informed eye to figure out where things are going. These research notes are my current best approximations of where we stand.

As with most things on Interconnects, our goal is to build well more value than we capture, so the data on the dashboard will remain free for the foreseeable future. These notes will be more raw than many of my articles, a place for sharing what I’m mulling over but don’t have complete evidence for (yet). They will be primarily paid, enabling me to speak more freely. If you work in AI policy or policy-facing research, please reach out to us and we can likely get discounted or free access.

The primary question I wanted to address today is: What is the market for open model inference? Is it growing? Is it a rounding error relative to OpenAI/Anthropic?

[Share](https://www.interconnects.ai/p/how-well-can-we-measure-open-model?utm_source=substack&utm_medium=email&utm_content=share&action=share)

I’ve been building the best measurements I can here, primarily using the open model inference startups Baseten, Fireworks, and Together AI as the bellwethers here (along with the open model share of OpenRouter’s token volume, which they thankfully report in their API). The hyperscalers also likely have a major footprint here, e.g. Amazon Bedrock and Azure’s model garden, but I haven’t seen them break down usage by model category. The startups’ fates are much more directly correlated with the success of open model inference as a category.

For the latest data, we can start with a slide from my [talk on the open model economy](https://natolambert.com/slides/2026/fireworks/talk#6) (recording [here](https://www.youtube.com/watch?v=zsO5RktNB7k)). These are the public disclosures from the open model startups on their token volume, over time, log-scale.

[![Open model inference exponential growth](https://substackcdn.com/image/fetch/$s_!hFZ7!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6c5f2798-10bc-487a-8932-5f851ad61b24_1600x840.svg "Open model inference exponential growth")](https://substackcdn.com/image/fetch/$s_!hFZ7!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6c5f2798-10bc-487a-8932-5f851ad61b24_1600x840.svg)

This shows near-exponential growth, before the uptick that would follow [GLM 5.2’s step change in open agents](https://www.interconnects.ai/p/glm-52-is-the-step-change-for-open?utm_source=publication-search). For example, Fireworks announced at their first company conference this week that they’re [serving more than 40T tokens per day](https://x.com/natolambert/status/2117672040807489553) to customers’ customized models. This is just one slice of their inference, a vertical that they said grew over 2x from this summer, so I would expect their total number this fall to be well above 100T tokens when we get the next update.

My first instinct with these numbers has been to ground them versus any closed model or hyperscale company we can, at least in terms of token volume. Thankfully, Google releases token volumes for the entire platform and API usage in their earnings reports. We can overlay those two numbers on top of the open model inference platforms, with the resulting plot below:
