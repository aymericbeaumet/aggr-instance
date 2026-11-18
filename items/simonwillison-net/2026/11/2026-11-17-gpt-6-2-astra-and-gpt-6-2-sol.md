---
title: GPT‑6.2 Astra and GPT‑6.2 Sol
link: https://simonwillison.net/2026/Nov/17/gpt62-astra-and-sol/
source: simonwillison-net
published: 2026-11-17T22:38:52Z
updated: 2026-11-17T22:38:52Z
first_seen: 2026-11-18T01:14:15.957291087Z
labels:
- ai
- generative-ai
- gpt
- llm-pricing
- llm-release
- llms
- openai
- pelican-riding-a-bicycle
- theo-browne
- yonkers-rising
summary: 'GPT‑6.2 Astra and GPT‑6.2 Sol OpenAI: We’re introducing GPT‑6.2 Astra, our new frontier model for your hardest projects, and GPT‑6.2 Sol, our efficient, versatile model for everyday work. They''re available in the API as gpt-6.2-astra and gpt-6.2-sol. I shipped LLM 0.38 with support for the new models. 6.2 Astra is 80% of the price of 6 Astra - $8/MTok input and $40/MTok output, the same price as Claude Fable 5.2 (which was also 80% of its predecessor). 6 Astra and Fable 5 were both $10/$50. The cached price is an even bigger discount - $0.40/million compared to $1/million for the older model. This is great news for longer running agentic sessions. GPT-6.2 Sol is the same $2/$10 price as 6.1 Sol and 6 Sol - which are half the price of GPT-5.6 Sol ($4/$20). Here are the pelicans for GPT-6.2 Sol and for GPT-6.2 Astra. The 6.2 Astra max pelican looks like this: I have a full comparison grid here, but here''s how the high/xhigh/max pelicans compare with the previous Sol and Astra releases: I also ran my most recent benchmark prompt against the two models, using "Ultra'' effort levels in the Codex CLI: Make a three js animation where yonkers stands up as a big mech (cool stand up motion) and stomps around manhattan and queens and Brooklyn, use openstreetmap mapping data Here''s Sol and here''s Astra. Neither of them did a good enough job of morphing Yonkers into a big mech, at least in comparison to Claude Fable 5.2 and Gemini 4 Carbon. This video showing the Astra and Sol animations playing out at the same time: I created that video using GPT-6.2 Astra (Ultra) in the Codex desktop app by prompting: https://static.simonwillison.net/static/2026/yonkers-gpt-6.2-astra-ultra/index.html and https://static.simonwillison.net/static/2026/yonkers-gpt-6.2-sol-ultra/index.html are two videos of the same thing by two different models I need a video of the first 10 seconds where the top half of the video plays the first and the bottom half plays the second I need an overlaid label of "GPT-6.2 Astra" on the top half and "GPT-6.2 Sol" on the bottom half. These labels should be tastefully positioned. Make the video square and 2000x2000 pixels Then later: OK make it 20s Here''s the exported transcript. The buzz around these new models is mixed at the moment. Astra 6.2 isn''t scoring quite as high as Fable 5.2 on various benchmarks, and I''ve seen a few complaints that its communication style can be confusing. Theo Browne''s review (in the form of a feedback letter he sent OpenAI during pre-release) has this useful example: When this model is working on long tasks, it can pull in a lot of subagents and manage them pretty well. It will then proceed to randomly ask you some weird internal implementation detail bullshit that you could not under any earthly circumstance understand, much less have input on. I sent "/goal make a new level for Outer Wilds". 2 hours in, it''s "blocked" because it wanted to ask me "does r24''s reviewer write it''s clips to 24_g521.mp4 or 24_p11.mp4?" Tags: ai, openai, generative-ai, llms, llm-pricing, pelican-riding-a-bicycle, llm-release, theo-browne, gpt, yonkers-rising'
content: extracted
html: 2026-11-17-gpt-6-2-astra-and-gpt-6-2-sol.html
remote_preview:
  url: https://static.simonwillison.net/static/2026/astra-6-2-max.webp
  alt: Good bicycle, good pelican, one wing is on the handlebars with fingers, the other wing is behind. Feet on the correct side of the frame. Nice gradiants. Good pouch, hooked bill.
---

**[GPT‑6.2 Astra and GPT‑6.2 Sol](https://openai.com/index/introducing-gpt-6-2-astra-sol/)**. OpenAI:

> We’re introducing GPT‑6.2 Astra, our new frontier model for your hardest projects, and GPT‑6.2 Sol, our efficient, versatile model for everyday work.

They're available in the API as `gpt-6.2-astra` and `gpt-6.2-sol`. I shipped [LLM 0.38](https://llm.datasette.io/en/stable/changelog.html#v0-38) with support for the new models.

6.2 Astra is 80% of the price of 6 Astra - $8/MTok input and $40/MTok output, the same price as Claude Fable 5.2 (which was also 80% of its predecessor). 6 Astra and Fable 5 were both $10/$50. The cached price is an even bigger discount - $0.40/million compared to $1/million for the older model. This is great news for longer running agentic sessions.

GPT-6.2 Sol is the same $2/$10 price as 6.1 Sol and 6 Sol - which are half the price of GPT-5.6 Sol ($4/$20).

Here are the pelicans [for GPT-6.2 Sol](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2Fa940cc2750162ac9edf0ddf881067b83) and [for GPT-6.2 Astra](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2Ffe836e13c90c00233c9e9e499a941805). The 6.2 Astra max pelican looks like this:

![Good bicycle, good pelican, one wing is on the handlebars with fingers, the other wing is behind. Feet on the correct side of the frame. Nice gradiants. Good pouch, hooked bill.](https://static.simonwillison.net/static/2026/astra-6-2-max.webp)

I have a [full comparison grid here](https://static.simonwillison.net/static/2026/openai-pelicans-november-2026.html), but here's how the high/xhigh/max pelicans compare with the previous Sol and Astra releases:

![Twelve pastel illustrations compare four models drawing a pelican riding a bicycle at three effort levels. All twelve have a recognizable pelican and bicycle. The clearest differences are in the feather detail and how convincingly the bird steers and pedals. gpt-6.2-astra has the most detailed Max drawing (88.71¢), with layered feathers, a trailing red scarf, and a wing on the handlebars. Xhigh (17.70¢) adds a green cap and keeps the seaside setting. High (11.55¢) simplifies the feathers while keeping the riding pose clear. gpt-6.2-sol shows a convincing pedaling pose at both Max (13.69¢) and Xhigh (6.74¢), with one foot higher than the other and a wing reaching forward. At High (2.41¢), the bird sits lower and the handlebars are harder to distinguish. gpt-6-astra gives the bird a long wing that follows the curve of the handlebars at Max (63.21¢). Xhigh (33.85¢) shows two bent legs on the pedals. At High (18.37¢), one leg stretches almost straight down, making the pose look stiffer. gpt-6.1-sol uses nearly the same long-necked bird with a wing on the bars at Max (13.48¢) and Xhigh (5.39¢). At High (3.66¢), the bird folds its wing against its body and appears to ride hands-free.](https://static.simonwillison.net/static/2026/gpt-6-2-grid.webp)

I also ran my most recent benchmark prompt against the two models, using "Ultra' effort levels in the Codex CLI:

> `Make a three js animation where yonkers stands up as a big mech (cool stand up motion) and stomps around manhattan and queens and Brooklyn, use openstreetmap mapping data`

Here's [Sol](https://static.simonwillison.net/static/2026/yonkers-gpt-6.2-sol-ultra/index.html) and here's [Astra](https://static.simonwillison.net/static/2026/yonkers-gpt-6.2-astra-ultra/index.html). Neither of them did a good enough job of morphing Yonkers into a big mech, at least in comparison to [Claude Fable 5.2](https://tools.simonwillison.net/yonkers-rises) and [Gemini 4 Carbon](https://static.simonwillison.net/static/2026/yonkers-mech-standalone.html).

This video showing the Astra and Sol animations playing out at the same time:

I created that video using GPT-6.2 Astra (Ultra) in the Codex desktop app by prompting:

> `https://static.simonwillison.net/static/2026/yonkers-gpt-6.2-astra-ultra/index.html and https://static.simonwillison.net/static/2026/yonkers-gpt-6.2-sol-ultra/index.html  are two videos of the same thing by two different models`
>
> `I need a video of the first 10 seconds where the top half of the video plays the first and the bottom half plays the second`
>
> `I need an overlaid label of "GPT-6.2 Astra" on the top half and "GPT-6.2 Sol" on the bottom half. These labels should be tastefully positioned.`
>
> `Make the video square and 2000x2000 pixels`

Then later:

> `OK make it 20s`

Here's the [exported transcript](https://gist.github.com/simonw/6b44c588a1321b60799dbbfd13db6d23).

The buzz around these new models is mixed at the moment. Astra 6.2 isn't scoring quite as high as Fable 5.2 on various benchmarks, and I've seen a few complaints that its communication style can be confusing. Theo Browne's [review](https://twitter.com/theo/status/2122760447103484410) (in the form of a feedback letter he sent OpenAI during pre-release) has this useful example:

> When this model is working on long tasks, it can pull in a lot of subagents and manage them pretty well. It will then proceed to randomly *ask you some weird internal implementation detail bullshit that you could not under any earthly circumstance understand, much less have input on*.
>
> I sent "/goal make a new level for Outer Wilds". 2 hours in, it's "blocked" because it wanted to ask me "does r24's reviewer write it's clips to 24\_g521.mp4 or 24\_p11.mp4?"
