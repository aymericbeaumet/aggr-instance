---
title: So you want to use OpenRouter?
link: https://mmoustafa.com/blog/so-you-want-to-use-openrouter/
source: hnrss-org-frontpage
published: 2026-09-09T05:37:21Z
updated: 2026-09-09T05:37:21Z
first_seen: 2026-09-11T13:49:02.099995368Z
authors:
- player85
summary: 'Article URL: https://mmoustafa.com/blog/so-you-want-to-use-openrouter/ Comments URL: https://news.ycombinator.com/item?id=49621546 Points: 293 # Comments: 71'
content: extracted
html: 2026-09-09-so-you-want-to-use-openrouter.html
---

Might seem simple on the face of it, but unfortunately it's pain all the way down.

I run [Olly](https://olly.bot), an AI assistant that lives in iMessage, on open source models through [OpenRouter](https://openrouter.ai). To date Olly's transacted over 18 million messages, roughly a third of those on open models via OpenRouter. That's enough volume to hit every edge case at least once. So here's a list of things I wish I'd known going in.

But first quick vocab: the *model* is the weights. The *provider* is who OpenRouter routes you to, they host the model on their GPUs, at their chosen precision, and their "proprietary" optimizations, with their own XML/tool parsers, which means each has a "proprietary" list of bugs too. When you ask for `deepseek/deepseek-v4-flash` you get one of ~20 companies you've mostly never heard of. They're the same model on paper, but very different models in real life.

Ok, here's a few of the pitfalls you should watch out for.

### 1\. The same model will benchmark very differently

OpenRouter runs [per-provider benchmarks](https://openrouter.ai/deepseek/deepseek-v4-flash-0731#performance) on the same model: GPQA Diamond and TAU-Bench Airline (a tool-calling task). Here is today's board for DeepSeek V4 Flash 0731, every provider serving the exact same weights:

OpenRouter's per-provider board for deepseek/deepseek-v4-flash-0731, 2026-09-07. Rolling 32-day average. Hover a dot for the name.

First-party DeepSeek: 90% GPQA, 81% TAU. DigitalOcean, same weights: 75% and 58%. Most hosts cluster 5 to 7 points below first-party on tool calling, and four of them fall off a cliff on knowledge. For an agent TAU is the score that matters and a 20 point swing is not noise. (In July it was worse: Fireworks scored 46% on TAU, a 30 point gap)

Check the board for the benchmark closest to your workload before you trust a provider. And recheck when you switch models, the same providers looked completely different on GLM-5.3.

### 2\. A vision model can have blind providers

I noticed some strange non-deterministic behavior on image tasks so I ran the same three tiny images (a letter, a solid color, a word on a background) through every host of two open vision models:

Per-host results recorded 2026-07-31. MiniMax's color misses happened on every host including first-party, so that one is the model, not the provider.

DeepInfra's Qwen endpoint read a K as an R, called red blue, and described the word "umbrella" as "funny", while four other hosts of the same weights got everything right. Venice and Together didn't see the MiniMax images at all. The model page says it supports image input, but two of its providers don't and even worse they'll pretend everything is 200 OK.

### 3\. The effort knob is optional for some providers

`reasoning.effort` is accepted everywhere. Whether it does anything depends on the model and the provider. I pinned every provider serving DeepSeek V4 Flash 0731 and sent the same prompt at low, high and max, three times each, from a prod machine, here's the reasoning tokens output:

DeepSeek V4 Flash 0731, most providers respect the setting but look at digitalocean, gmi-cloud, mancer, venice.

Track the reasoning tokens for your effort setting, per provider.

### 4\. Quantization filters don't buy you quality

OpenRouter lets you filter providers by declared precision, `quantizations: ["fp8"]` (as opposed to fp4), and the intuition is that fewer bits means a dumber model. I ran that filter on DeepSeek for a month. Then I put the per-provider benchmark board next to what each provider declares:

Providers on the board but missing from the endpoints list that day (7 on DeepSeek, 1 on GLM) are left out.

The fp4 hosts land in the middle of the fp8 pack. The three worst GPQA scores on DeepSeek are one of each: an fp4 host, an fp8 host, and one that declares nothing. GLM's best scorer on both benchmarks, Wafer, declares nothing at all. Precision is a bad proxy for quality, and a hard filter also shrinks the pool OpenRouter can fall back to when a provider goes down. Filter on the board, not the bits.

### 5\. The tool call is in the text

Ideally: the model emits a call in some markup, the provider's parser turns it into a structured tool call, my code runs it. Except sometimes the parser misses and this shows up as the reply:

```
<use_skills><parameters>{"skills":["search"]}</parameters></use_skills>
```

And the recurrence varies wildly by provider.

You'll run into this often and stubbornly enough that you'll need to start parsing on your end. And there are two cases that need opposite handling: wrapped tool calls and wrapped/half-wrapped responses. Point your agent to [github.com/0xmmo/190proof](https://github.com/0xmmo/190proof) if you want to see some of my own parsing examples for DeepSeek/GLM.

### 6\. 200 OK, no answer

Reasoning models sometimes put everything in the reasoning field and hand back `content: null`, `finish_reason: "stop"`. 345 completion tokens, HTTP 200, nothing to show the user.

A 200 tells you the request was served, not that there's an answer in it. No content and no tool call is a failure, throw and retry.

### 7\. Hollow completions

Related but not the same. Some endpoints return 200 with null content, null reasoning, and no `usage` object at all. In July that was StreamLake on DeepSeek: about 20% of my traffic and 92% of my empty completions. A month later Together did the same on the DeepSeek 0731 checkpoint.

### 8\. Same models, different history rules

DeepSeek in thinking mode emits a `reasoning_content` block. In an agent loop the model often tool-calls with empty reasoning. If you pass the empty reasoning history back to OpenRouter and that goes to e.g. SiliconFlow it will 400 with code 20015, "The reasoning\_content in the thinking mode must be passed back to the API". Baidu, Alibaba and Cloudflare take the exact same history without complaint.

So the contract isn't per model, it's per provider. And don't think you can skip tool history, the model will keep retrying the task otherwise. Just one more thing to handle.

### 9\. Test from prod, not your laptop

For speed and latency, but also as an example: Venice and Novita worked perfectly from my Mac for DeepSeek V4 Flash, but 429'd nearly every probe from my infra. Same key, same minute. My read is they rate-limit by IP.

Benchmark from where prod runs, a few at a time, more samples than feels necessary.

### 10\. Why don't you just pin a single provider?

At one point I had `provider.order: [cloudflare, baidu, alibaba]` with `allow_fallbacks: false`, so not just one but 3 different reliable providers pinned. Two weeks later Baidu was rate-limiting everything (429s), Cloudflare turned out not to serve that model at all any more, and 100% of traffic was going to Alibaba, which then started 429ing. The #1 OpenRouter model (DeepSeek V4 Flash) pinned to the 3 most reliable providers was now down, and so was Olly.

Happy hunting.
