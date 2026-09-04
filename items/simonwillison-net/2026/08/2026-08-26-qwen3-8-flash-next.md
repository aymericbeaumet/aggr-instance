---
title: Qwen3.8-Flash-Next
link: https://simonwillison.net/2026/Aug/26/qwen38-flash-next/
source: simonwillison-net
published: 2026-08-26T23:52:58Z
updated: 2026-08-26T23:52:58Z
first_seen: 2026-09-04T12:14:41.802665207Z
labels:
- ai
- ai-in-china
- generative-ai
- llm-release
- llms
- nvidia-spark
- pelican-riding-a-bicycle
- qwen
summary: 'Qwen3.8-Flash-Next Another open weights model from Qwen. This one is "a multimodal MoE model that also serves as an early preview of the architecture used in Qwen4". It''s pretty big: 125B parameters but only 6B active which means it gets a significant performance boost. I''ve been trying it out on a DGX Spark using these Unsloth quantized models. I''m still exploring the model - so far I''ve tried the 72.5GB UD-IQ1_S one (producing these pelicans) and the 78.9GB UD-Q2_K_XL (producing these). My favorite so far was this xhigh reasoning effort one from UD-Q2_K_XL: Via Hacker News Tags: ai, generative-ai, llms, qwen, pelican-riding-a-bicycle, llm-release, ai-in-china, nvidia-spark'
content: extracted
html: 2026-08-26-qwen3-8-flash-next.html
---

**[Qwen3.8-Flash-Next](https://qwen.ai/blog?id=qwen3.8-flash-next)** ([via](https://news.ycombinator.com/item?id=49448210 "Hacker News")) Another open weights model from Qwen. This one is "a multimodal MoE model that also serves as an early preview of the architecture used in Qwen4".

It's pretty big: 125B parameters but only 6B active which means it gets a significant performance boost.

I've been trying it out on a DGX Spark using [these Unsloth quantized models](https://huggingface.co/unsloth/Qwen3.8-Flash-Next-GGUF). I'm still exploring the model - so far I've tried the 72.5GB UD-IQ1\_S one (producing [these pelicans](https://tools.simonwillison.net/markdown-svg-renderer#url=https%3A%2F%2Fgist.github.com%2Fsimonw%2Ff9c69ebdab90d8a45b8de4742cc7b840)) and the 78.9GB UD-Q2\_K\_XL (producing [these](https://tools.simonwillison.net/markdown-svg-renderer#url=https%3A%2F%2Fgist.github.com%2Fsimonw%2F6ba7cbfc1a9336986703b41f7fccd73a)).

My favorite so far was this xhigh reasoning effort one from UD-Q2\_K\_XL:

![Flat vector illustration: a white pelican with an orange beak and orange legs rides a red bicycle along a sandy path, a wicker basket on the handlebars holding a blue fish, with green rolling hills, a small tree and bushes, white clouds and a bright yellow sun in a blue sky behind it](https://static.simonwillison.net/static/2026-08-27/IMG_7667.png)
