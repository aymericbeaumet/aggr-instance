---
title: 'Introducing Mistral Large 4: Le chonk'
link: https://simonwillison.net/2026/Oct/6/le-chonk/
source: simonwillison-net
published: 2026-10-06T20:18:19Z
updated: 2026-10-06T20:18:19Z
first_seen: 2026-10-06T23:45:23.660353536Z
labels:
- ai
- generative-ai
- llm-release
- llms
- mistral
- pelican-riding-a-bicycle
summary: 'Introducing Mistral Large 4: Le chonk Mistral are back in the game. Today they''re releasing a preview of Mistral Large 4, a 1 trillion parameter, 49 billion active parameter model trained on their own cluster of 3,800 NVIDIA Grace Blackwell GPUs. The preview is available via their API. They promise to release the open weights model at the "end of this month". The model only supports two reasoning levels - "none" and "high" - via the Mistral API. Here are both pelicans - the "high" one looks better, though surprisingly it only used 2,717 output tokens compared to "none" which used 3,275: On Artificial Analysis it scores 38, just behind DeepSeek 4.1 Flash, which is a 552B model. It''s a huge improvement on last December''s Mistral Large 3, which drew this terrible pelican and scored 9 on AA. It''s certainly not a Fable-class model, but it''s great to see Mistral put out a model that''s back to being maybe about 6 months behind the frontier. Via Hacker News Tags: ai, generative-ai, llms, mistral, pelican-riding-a-bicycle, llm-release'
content: extracted
html: 2026-10-06-introducing-mistral-large-4-le-chonk.html
remote_preview:
  url: https://static.simonwillison.net/static/2026-10-06/mistral-large-4-pelican.webp
  alt: It's good. The pouch is great, the bicycle frame is the right size, it has feet on pedals. Both pedals appear in front of the frame though. Nice gradients.
---

**[Introducing Mistral Large 4: Le chonk](https://mistral.ai/news/mistral-large-4/)** ([via](https://news.ycombinator.com/item?id=49977979 "Hacker News")) Mistral are back in the game. Today they're releasing a preview of Mistral Large 4, a 1 trillion parameter, 49 billion active parameter model trained on their own cluster of 3,800 NVIDIA Grace Blackwell GPUs.

The preview is available via their API. They promise to release the open weights model at the "end of this month".

The model only supports two reasoning levels - "none" and "high" - via the Mistral API. Here are [both pelicans](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F0db8b8196e57711b352f6c5857cf5b35) - the "high" one looks better, though surprisingly it only used 2,717 output tokens compared to "none" which used 3,275:

![It's good. The pouch is great, the bicycle frame is the right size, it has feet on pedals. Both pedals appear in front of the frame though. Nice gradients.](https://static.simonwillison.net/static/2026-10-06/mistral-large-4-pelican.webp)

On Artificial Analysis [it scores 38](https://artificialanalysis.ai/models/mistral-large-4), just behind DeepSeek 4.1 Flash, which is a 552B model. It's a *huge* improvement on last December's Mistral Large 3, which drew [this terrible pelican](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F0df5e656291d5a7a1bf012fabc9edc3f#response-1) and [scored 9 on AA](https://artificialanalysis.ai/models/mistral-large-3).

It's certainly not a Fable-class model, but it's great to see Mistral put out a model that's back to being maybe about 6 months behind the frontier.
