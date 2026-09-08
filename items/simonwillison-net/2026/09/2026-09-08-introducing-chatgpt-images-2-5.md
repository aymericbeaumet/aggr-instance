---
title: Introducing ChatGPT Images 2.5
link: https://simonwillison.net/2026/Sep/8/introducing-chatgpt-images-25/
source: simonwillison-net
published: 2026-09-08T22:46:33Z
updated: 2026-09-08T22:46:33Z
first_seen: 2026-09-08T22:56:52.093968329Z
labels:
- ai
- generative-ai
- openai
- text-to-image
- tools
- uv
summary: 'Introducing ChatGPT Images 2.5 OpenAI''s image generation models are apparently used "more than 3 billion images across ChatGPT Images and the GPT‑Image models in the API". This latest release improves their instruction-following ability across multiple turns, responds faster, and "is better at preserving the subjects in your reference photos". There are two new model IDs in the API: gpt-image-2.5-sunburst and gpt-image-2.5-flare. Based on this I think Sunburst is the stronger option: Choose Sunburst for workflows where editing precision matters most, and Flare for fast, high-quality everyday image generation. I upgraded my openai_image.py CLI tool to support passing in one or more reference images, so now this works: uv run https://tools.simonwillison.net/python/openai_image.py \ ''add a raccoon scientist studying the chart thoughtfully'' \ -i https://static.simonwillison.net/static/2026/openai-agent-usage.webp \ -m gpt-image-2.5-sunburst This is the original image, and here''s what I got back from that prompt to "add a raccoon scientist studying the chart thoughtfully": Tags: tools, ai, openai, generative-ai, uv, text-to-image'
content: extracted
html: 2026-09-08-introducing-chatgpt-images-2-5.html
preview:
  file: 2026-09-08-introducing-chatgpt-images-2-5.preview-728bea06a087.webp
  width: 256
  height: 193
  alt: 'Line chart with cartoon illustration. Title: "Usage of internal coding agents is increasing significantly—Median researcher". Y-axis labeled "Daily $ / researcher" from 0 to 700; x-axis shows Feb 2026, Apr 2026, Jun 2026, Aug 2026. A blue line stays near zero through April, rises gradually to around'
  color: '#dddad9'
images:
- source: https://static.simonwillison.net/static/2026/racoon-chart.webp
  original:
    file: 2026-09-08-introducing-chatgpt-images-2-5.image-513fe90d19e9.webp
    width: 1443
    height: 1090
  variants:
  - file: 2026-09-08-introducing-chatgpt-images-2-5.image-20020249bc7b.webp
    width: 48
    height: 36
  - file: 2026-09-08-introducing-chatgpt-images-2-5.image-a58b4e1a0e39.webp
    width: 320
    height: 242
  color: '#fdfdfd'
---

**[Introducing ChatGPT Images 2.5](https://openai.com/index/introducing-chatgpt-images-2-5/)**. OpenAI's image generation models are apparently used "more than 3 billion images across ChatGPT Images and the GPT‑Image models in the API". This latest release improves their instruction-following ability across multiple turns, responds faster, and "is better at preserving the subjects in your reference photos".

There are two new model IDs in the API: `gpt-image-2.5-sunburst` and `gpt-image-2.5-flare`. Based [on this](https://developers.openai.com/api/docs/guides/image-generation#overview) I think Sunburst is the stronger option:

> Choose Sunburst for workflows where editing precision matters most, and Flare for fast, high-quality everyday image generation.

I [upgraded](https://github.com/simonw/tools/pull/333) my [openai\_image.py](https://tools.simonwillison.net/python/#openai_imagepy) CLI tool to support passing in one or more reference images, so now this works:

```
uv run https://tools.simonwillison.net/python/openai_image.py \
  'add a raccoon scientist studying the chart thoughtfully' \
  -i https://static.simonwillison.net/static/2026/openai-agent-usage.webp \
  -m gpt-image-2.5-sunburst
```

This is the [original image](https://static.simonwillison.net/static/2026/openai-agent-usage.webp), and here's what I got back from that prompt to "add a raccoon scientist studying the chart thoughtfully":

![Line chart with cartoon illustration. Title: \"Usage of internal coding agents is increasing significantly—Median researcher\". Y-axis labeled \"Daily $ / researcher\" from 0 to 700; x-axis shows Feb 2026, Apr 2026, Jun 2026, Aug 2026. A blue line stays near zero through April, rises gradually to around 150 by June and July, then climbs steeply to about 600 by late August. In the foreground a cartoon raccoon in glasses and a white lab coat, chin in hand, holds a clipboard at a desk with a mug bearing the OpenAI logo, some printed charts, and a stack of three books titled \"AI AGENTS\", \"SOFTWARE ENGINEERING\", and \"PRODUCTIVITY\". An OpenAI logo appears in the top right corner.](https://static.simonwillison.net/static/2026/racoon-chart.webp)
