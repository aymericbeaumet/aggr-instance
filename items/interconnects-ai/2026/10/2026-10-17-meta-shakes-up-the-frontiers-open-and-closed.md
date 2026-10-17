---
title: Meta shakes up the frontiers, open and closed
link: https://www.interconnects.ai/p/meta-shakes-up-the-frontiers-open
source: interconnects-ai
published: 2026-10-17T14:06:54Z
updated: 2026-10-17T14:06:54Z
first_seen: 2026-10-17T16:38:09.894543738Z
authors:
- Nathan Lambert
summary: US open weights are back!
content: feed
html: 2026-10-17-meta-shakes-up-the-frontiers-open-and-closed.html
remote_preview:
  url: https://substack-post-media.s3.amazonaws.com/public/images/95f73e17-9e3c-49ea-8d4c-e0ae87c89257_3182x1790.png
---

This has been a week where the balance of American open-weight models took a step change back toward the frontier. Meta took the lead of this charge with [Muse Spark Open](https://research.meta.ai/blog/muse-spark-open), which is now the strongest American-made open model by quite some margin, but the addition of Reflection’s first model [Beam](https://reflection.ai/blog/introducing-beam) is also relevant. Without these two models, there would be 20+ open-weight models built and released by Chinese companies better than the best Western models. At the time of writing this, Mistral has also announced a strong model, [Mistral Large 4](https://mistral.ai/news/mistral-large-4/), with an index score of 38 (see below), but its weights are not yet released. There are rumors of a few more strong American open-weight models coming from other organizations too!

In many ways, the gap between the best American and Chinese models has been reduced roughly by half. We’ve gone from the best American open models being largely irrelevant to consumers who require the cutting edge of performance, to them being behind but relevant.

[![](https://substackcdn.com/image/fetch/$s_!eZYA!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F98319a15-087c-4f7f-acd3-d729927f0c16_2024x1952.png)](https://substackcdn.com/image/fetch/$s_!eZYA!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F98319a15-087c-4f7f-acd3-d729927f0c16_2024x1952.png)

This paints the picture of the cut-throat, competitive nature of the frontier model business. Reflection and Mistral *announced* their model scores and Meta dropped the weights of a smarter model before these other two labs could get their own weights out. All of these models are competing with Chinese models released months ago, where the industry is collectively asking — where have Kimi and Zhipu AI been since the releases of GLM-5.3 and Kimi K3 on Aug. 18th and Jul. 16th, respectively?

That is a very nuanced question, which I’ll dig into another time. Today’s piece is focused on Meta’s turnaround, what Muse Spark Open means for the ecosystem, and the balance of power across Western frontier labs.

[Share](https://www.interconnects.ai/p/meta-shakes-up-the-frontiers-open?utm_source=substack&utm_medium=email&utm_content=share&action=share)

### Why Muse Spark Open should take off — and why it may not

One thing we see again and again at Interconnects through our open model analysis is that Western open-weight models get astonishingly more adoption than Chinese models with the same performance-to-size footprint (even though the Chinese models usually come out first)! Muse Spark Open is landing into this environment, at the same time as when open-ended LLM agents are finally real (~[GLM 5.2 capabilities and on](https://www.interconnects.ai/p/glm-52-is-the-step-change-for-open)).

Riding off a rapid increase in performance of Chinese models, the open ecosystem has been living through an acceleration in demand. The inference providers are rapidly scaling, the open post-training economy is forming, and countless enterprises are looking to own their intelligence. Muse Spark Open’s performance falls somewhere above GLM 5.2, near DeepSeek V4.1 Flash, and behind GLM 5.3 or Kimi K3. It should be squarely “good enough” to slot right into this.

For some background, from the Muse Spark Open [model card](https://huggingface.co/meta-models/Muse-Spark-Open-816B-A42B) on Hugging Face:

> Muse Spark Open is an 816B total/42B active parameter open weights model with a context window of 1M tokens. Built for coding and agentic work, Muse Spark Open is natively multimodal. This open weights version has additional safety post-training to make it more robust against misuse as a standalone model without system guardrails. We recommend deploying Muse Spark Open as part of a broader AI system, with additional guardrails suited to the specific use case.

Here’s the distribution of the various Muse models on the Artificial Analysis Intelligence Index.

[![](https://substackcdn.com/image/fetch/$s_!CFQC!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6dfa64c4-5907-4e2b-88db-a02ca1ddfd8b_2464x944.png "")](https://substackcdn.com/image/fetch/$s_!CFQC!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6dfa64c4-5907-4e2b-88db-a02ca1ddfd8b_2464x944.png)

This is included to loop back to the original model card snippet. It was shared that this open-weight model underwent additional safety training to prepare for this open release. Many observers saw this release as potentially a model that was “broken” during the safety training, as it came out after Muse Spark 1.3.

In reality, this was much more likely a safety tune of Muse Spark 1.2, which would only be a 1 point drop on Artificial Analysis to shore up safety behaviors. When Meta released [Muse Glimmer](https://huggingface.co/meta-models/Muse-Glimmer-30B), the 30B agentic model, [Alex Wang said](https://x.com/alexandr_wang/status/2086756152034066792) that they were preparing to release the Muse Spark 1.2 model as open weights, which fits with the above.

If anything will hold Muse Spark Open back, it’ll be the [specific license](https://huggingface.co/meta-models/Muse-Spark-Open-816B-A42B/blob/main/LICENSE.md) they designed for it. In summary, this custom license has pretty much all of the restrictive license behaviors we’ve seen (and criticized here) in the last few years of building with open models. They are as follows:

1. Users of the models in products must display “Built with Muse” on their products. Meta did this with Llama, and Kimi has tried it, but almost no one complies.

2. There is a revenue-gating factor, where companies that make more than $20M per month across their entire business deploying the model need a separate license with Meta. Other prominent open model providers are doing this too, in order to set up [revenue share agreements](https://interconnect.substack.com/p/kimis-tightrope-openness-and-revenue) for re-selling inference.

3. Muse Spark Open makes you agree to an acceptable use policy, which in the true “open source software” corner of the world makes this model extremely not open-source.

4. All of the above applies to outputs from the model as well, and derivatives, so there is legal ambiguity if you can safely use the model for synthetic data.

The license *does* permit using Muse Spark Open for internal research and applications.

We’re at a weird equilibrium where this license is complex and unproven in court, but many players will likely ignore it. Many, many people will build on this model from various inference providers, and Meta will only likely go after egregious violations.

For now, licenses haven’t been the determining factor of AI adoption, but this could change in the future. This is why we think it’s an unnecessary strategic risk for labs trying to compete and regain adoption to have complex, non-standard open licenses for their models.

Interconnects AI is a reader-supported publication. Consider becoming a subscriber.

### Is Meta a tier 1 (OpenAI/Anthropic) or tier 2 (Gemini/xAI) frontier lab?

Just a few days before Muse Spark Open, Meta unveiled their closed frontier models, [Muse Wonder and Muse Wonder Full](https://research.meta.ai/blog/muse-wonder).[1](https://www.interconnects.ai/p/meta-shakes-up-the-frontiers-open#footnote-1) On traditional benchmarks, these are very strong models only behind Anthropic’s Opus 5.5 and similar to OpenAI’s GPT-6-Astra or Google’s Gemini-4-Argon.

These major releases are coming just over a year out from Meta’s massive AI redirection, characterized by CEO Mark Zuckerberg’s [Personal Superintelligence](https://www.meta.com/superintelligence/?srsltid=AU7gw4XKcgMv1WYya2PFc2kUjMo91dGE7wynRverFh7HmN5-rEmKYJag) post, which got a lot of criticism at the time. Meta is the only *American* company with frontier open and closed models. Alibaba’s Qwen has followed a similar strategy, but with a far more open-leaning approach.

The vibe checks across all of these models have been solid, but the expectations were for them to not immediately challenge the level of refinement of the most recent Anthropic and OpenAI models in real-world usage. I’m excited to try them more — time to sign up for another AI subscription.

I like this recent run of form from Meta, driving their own products and directions in the popular Muse agent, taking wins like the leading US open-weight crown when useful. Whether or not it’s financially successful, it’s a unique perspective that fits their history as a business.

It often feels like there’s a revolving door of which lab is nipping at the heels of the Anthropic-OpenAI leading duo. It’s not clear that in the long run Gemini and xAI are out of it, but those two are much more muddled in their offerings than Meta today. If you listen to [Zuck and Alexandr Wang on The Cheeky Pint](https://www.youtube.com/watch?v=XfdIhd1g6EI&t=7752s&pp=ygUQY2hlZWt5IHBpbnQgenVjaw%3D%3D), they clearly had a very specific vision of a high-ownership, high-compute per researcher lab that I would expect to pay off. Many labs die due to layers of bureaucracy and misaligned incentives. Meanwhile Alex Wang has over 200 direct reports — i.e. it’s a very flat org with minimal management — and they’re releasing very consistent model and product improvements.

[Leave a comment](https://www.interconnects.ai/p/meta-shakes-up-the-frontiers-open/comments)

Much of my worldview of building models is that you need the resources, the compute, the data, and the talent (all downstream of large sums of money), but organizing them is very hard. Gemini is sometimes described as a place where it’s impossible to get any work done or grow — which is embellishing the issue, as many people love it there, but there are kernels of truth to it. Layers of management easily get in the way of making a great model.

While I’m praising Meta today, the Llama failure has often been described as that sort of management going too far, with model decisions being made out of politics rather than performance. xAI is in its own lane, where it may be closer to having too much talent turnover or not enough organization to sustain great models for a long period of time.

When we look back at the last few years, all of Meta, Gemini, and xAI have produced models pretty much at the frontier of performance, but what has distinguished OpenAI and Anthropic is their consistency in maintaining frontier status. Anthropic seems the most poised and organized, with a plan and vision for how these organizations should work, and they’ve gained a lead from it. OpenAI has been more hot and cold, but manages to keep up at the frontier of deployed models while being the leader in public scientific breakthroughs (from Navier-Stokes to o1’s inference time scaling).

Here’s the state of play among the companies that this post focused on, plus Xiaomi V2.6 Pro representing the leading open-weight model overall. The ranking would be Anthropic > OpenAI > Meta ~ Gemini > xAI ~ Chinese Open Weights > U.S. Open Weights.

[![](https://substackcdn.com/image/fetch/$s_!p7ID!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6ec13bec-5d2f-4a3f-b11f-64fe7fd78bcf_2464x944.png)](https://substackcdn.com/image/fetch/$s_!p7ID!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6ec13bec-5d2f-4a3f-b11f-64fe7fd78bcf_2464x944.png)

[Share](https://www.interconnects.ai/p/meta-shakes-up-the-frontiers-open?utm_source=substack&utm_medium=email&utm_content=share&action=share)

### Open questions for the AI industry

After a long time of predicting the frontier model game getting too expensive for many to keep up, we may be seeing this play out among Western models. We seem very far from “one person wins” with RSI given how close all the models are to each other, but we’re narrowing down the number of labs who can compete in the US from a much wider net with surprises, to the 5 of Anthropic, OpenAI, Meta, Gemini, and xAI. If Microsoft starts releasing models, as they’re posting [strong scaling science results](https://microsoft.ai/news/frontier-pre-training-at-microsoft-ai/), they could join this list — given their financial backing with a hyper-scale business and direct access to OpenAI’s intellectual property for training models.

The best models we’ve seen from the “tier 3” players like Mistral, Nemotron, Reflection, Arcee, etc. have had an increasingly hard time impacting either the open or closed frontiers. These models still can get substantial adoption, enough to start growing a business, but the costs of building true frontier models are now measured in billions per major model and only huge companies can sustain that.

Despite this massive scaling in resources, the technical core of the industry still doesn’t have a high hit rate on predicting how successful a model will be in real-world usage. Yes, evaluations are a good proxy, but why do the leading labs like Anthropic go from hit models like Claude Fable 5 to total duds in Claude Opus 5 and then back to hits with Opus 5.5? There are billions of dollars to be made in predicting this accurately, and the labs are surely trying, but the models fundamentally still are largely unknowable in the long-tail.

Of all of the players left in the race, betting on Meta, Gemini, and Microsoft financially seems much easier from a long-term perspective due to their rich balance sheets. This is the “AI will take a while and OpenAI/Anthropic do not have secret sauce” perspective. After all, it’s the fastest revenue growth industry ever — competition will naturally be immense.

Currently, Meta is valued under $2T and Anthropic is approaching their barnstorming IPO at the same price. Meta seems half a step behind on the same technology, but in the long-term view these seem to be starting from almost the same point and Meta has an extremely profitable core business. One of the two valuations needs to correct massively. A fun thought experiment I’ve been pondering with friends is: What do you expect Anthropic’s public valuation to be in November of 2027? How about 2028?

[Leave a comment](https://www.interconnects.ai/p/meta-shakes-up-the-frontiers-open/comments)

The longer Meta and the “competitors” to OpenAI and Anthropic stick around, the more negative valuation pressure will exist around the AI IPOs (and that’s not to say anything about pricing in safety risks). Being at the absolute frontier could be financial tumultuous, while valuable. Time will tell — we know markets can stay irrational a very long time.

[1](https://www.interconnects.ai/p/meta-shakes-up-the-frontiers-open#footnote-anchor-1)

Muse Wonder and Muse Wonder Full are presumably different pretraining runs, with Full being a larger model, but it’s not clear and this is up there with the AI industry’s traditions of horrible names. There’s a chance that “full” is just a more looped version, given all the recent news on looped architectures, but I’m not confident enough to put that hypothesis in the main text.
