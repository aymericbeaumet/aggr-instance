---
title: Introducing Kimi K3.1 Pro
link: https://simonwillison.net/2026/Oct/23/kimi-k31-pro/
source: simonwillison-net
published: 2026-10-23T19:40:54Z
updated: 2026-10-23T19:40:54Z
first_seen: 2026-10-23T22:00:18.516360473Z
labels:
- ai
- ai-in-china
- generative-ai
- kimi
- llm
- llm-pricing
- llm-release
- llms
- moonshot
- pelican-riding-a-bicycle
summary: 'Introducing Kimi K3.1 Pro Kimi K3.1 Pro landed on the Kimi API today - it''s not yet open weights, but Moonshot promise "Full model weights will be released by Nov 4, 2026". They''re also offering a "Highspeed" version that''s 2-3x faster, at twice the price (except for input token cache hits, which are priced the same). The US dollar pricing is $2.00/million tokens for input and $10/million for output - the same as GPT-6.1 Sol and half the price of Claude Opus 5.5. Moonshot''s own benchmarks show Kimi as approximately level with Opus 5.5 and GPT-6 Astra - beating them on a few benchmarks, coming close on others. Artificial Analysis scored Kimi at 55 - ahead of Claude Fable 5.1, GPT-6 Astra, and Gemini 4 Argon (all at 53) and behind only Claude Opus 5.5 (58). This is the first time a (soon-to-be) open weight model has scored this highly against the frontier. I tried it out via the Moonshot API using the llm openai endpoint command I added to LLM 0.32 back in August: llm openai endpoint \ https://api.moonshot.ai/v1 \ --key kimi \ -m kimi-k3.1-pro \ "Generate an SVG of a pelican riding a bicycle" \ --responses \ -o reasoning_effort low \ > kimi-k3.1-pro-pelican-low.stdout.txt \ 2> kimi-k3.1-pro-pelican-low.stderr.txt llm openai endpoint can run prompts against any OpenAI-compatible endpoint but doesn''t log the results, hence the extra options to save stdout and stderr (the reasoning trace). Here are the pelicans I got for Kimi K3.1 Pro on the three reasoning levels it supports - low, medium, and max. This is max: For comparison, I ran fresh pelicans against Kimi K3 on those three reasoning levels. Via Hacker News Tags: ai, generative-ai, llms, llm, llm-pricing, pelican-riding-a-bicycle, llm-release, ai-in-china, moonshot, kimi'
content: extracted
html: 2026-10-23-introducing-kimi-k3-1-pro.html
remote_preview:
  url: https://static.simonwillison.net/static/2026/kimi-k3.1-card.jpg
---

**[Introducing Kimi K3.1 Pro](https://www.kimi.ai/blog/kimi-k3-1-pro)** ([via](https://news.ycombinator.com/item?id=50212065 "Hacker News")) Kimi K3.1 Pro landed on the Kimi API today - it's not yet open weights, but Moonshot promise "Full model weights will be released by Nov 4, 2026".

They're also offering a "Highspeed" version that's 2-3x faster, at twice the price (except for input token cache hits, which are priced the same).

The [US dollar pricing](https://platform.kimi.ai/docs/pricing/chat) is $2.00/million tokens for input and $10/million for output - the same as GPT-6.1 Sol and half the price of Claude Opus 5.5.

Moonshot's own benchmarks show Kimi as approximately level with Opus 5.5 and GPT-6 Astra - beating them on a few benchmarks, coming close on others.

Artificial Analysis [scored Kimi at 55](https://artificialanalysis.ai/models/kimi-k3-1-pro) - ahead of Claude Fable 5.1, GPT-6 Astra, and Gemini 4 Argon (all at 53) and behind only Claude Opus 5.5 (58).

This is the first time a (soon-to-be) open weight model has scored this highly against the frontier.

I tried it out via the [Moonshot API](https://platform.kimi.ai/docs/get-your-api-key) using the [llm openai endpoint](https://llm.datasette.io/en/stable/other-models.html#openai-endpoint) command I added to [LLM 0.32](https://llm.datasette.io/en/stable/changelog.html#v0-32) back in August:

```
llm openai endpoint \
  https://api.moonshot.ai/v1 \
  --key kimi \
  -m kimi-k3.1-pro \
  "Generate an SVG of a pelican riding a bicycle" \
  --responses \
  -o reasoning_effort low \
  > kimi-k3.1-pro-pelican-low.stdout.txt \
  2> kimi-k3.1-pro-pelican-low.stderr.txt
```

`llm openai endpoint` can run prompts against any OpenAI-compatible endpoint but doesn't log the results, hence the extra options to save stdout and stderr (the reasoning trace).

Here [are the pelicans](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F9b7181492209387da1d4f53002f26663) I got for Kimi K3.1 Pro on the three reasoning levels it supports - low, medium, and max. This is max:

![It's pretty solid - feet are on the pedals, bicycle frame is good, nice beak. Both legs are on the same side of the Fram though.](https://static.simonwillison.net/static/2026-10-23/image-2.png)

For comparison, I ran fresh [pelicans against Kimi K3](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2Feba9a4283c9e94cbbb5325902a3921a8) on those three reasoning levels.
