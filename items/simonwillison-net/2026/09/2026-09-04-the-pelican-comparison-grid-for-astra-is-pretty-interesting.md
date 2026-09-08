---
title: The Pelican comparison grid for Astra is pretty interesting
link: https://simonwillison.net/2026/Sep/4/astra-pelicans/
source: simonwillison-net
published: 2026-09-04T23:59:05Z
updated: 2026-09-04T23:59:05Z
first_seen: 2026-09-08T10:17:10.166315Z
labels:
- ai
- generative-ai
- gpt-6-astra
- llms
- openai
- pelican-riding-a-bicycle
summary: 'I got access to GPT-6 Astra this afternoon, so naturally I used it to generate SVGs of pelicans riding bicycles - at low, medium, high, xhigh and max reasoning levels (Astra doesn''t support reasoning=none). Then I rendered those pelicans in a comparison grid with GPT-5.6 Sol, Terra, and Luna, and beyond being fun the result was surprisingly useful. See the grid for full quality images. Here''s the transcript that created the GPT-6 Nova pelicans. There are a few interesting things that stand out from this grid. The Astra pelicans are much better. The very best GPT-5.6-Sol pelican (I liked xhigh better than max) is still pretty clearly a bunch of abstract shapes. Every single one of the Astra pelicans, from low to xhigh, looks better than that. The Astra max one is really good. Astra below max still doesn''t reliably get the pelican legs on both sides of the frame. In terms of cost, Astra may be around twice the price of Sol ($10/million input, $50/million output, compared to $5/$30 for Sol), but it uses significantly less tokens at each of the levels, making the prices at the different levels closer than they might otherwise be. Astra low produces a better pelican than ANY of the GPT-5.6 Sol models at any level, for 9.55 cents. Spending 10 cents on any other model gets a much worse result. Look at the input token counts: Astra and Luna both used 16 input tokens, Sol and Terra used 26. That''s interesting. I wonder if Astra and Luna are more related to each other than OpenAI let on? Tags: ai, openai, generative-ai, llms, pelican-riding-a-bicycle, gpt-6-astra'
content: extracted
html: 2026-09-04-the-pelican-comparison-grid-for-astra-is-pretty-interesting.html
preview:
  file: 2026-09-04-the-pelican-comparison-grid-for-astra-is-pretty-interesting.preview-7ff73c3593e2.webp
  width: 256
  height: 128
  color: '#dae5e1'
images:
- source: https://static.simonwillison.net/static/2026/astra-social-2.jpg
  original:
    file: 2026-09-04-the-pelican-comparison-grid-for-astra-is-pretty-interesting.image-6538b6c39a9b.jpg
    width: 1200
    height: 600
  variants:
  - file: 2026-09-04-the-pelican-comparison-grid-for-astra-is-pretty-interesting.image-9f4dbd1be418.webp
    width: 48
    height: 24
  color: '#fafafa'
- source: https://static.simonwillison.net/static/2026/astra-grid-3.webp
  original:
    file: 2026-09-04-the-pelican-comparison-grid-for-astra-is-pretty-interesting.image-573394dbb18a.webp
    width: 1280
    height: 1736
  variants:
  - file: 2026-09-04-the-pelican-comparison-grid-for-astra-is-pretty-interesting.image-4a910d25d923.webp
    width: 48
    height: 65
  - file: 2026-09-04-the-pelican-comparison-grid-for-astra-is-pretty-interesting.image-ffcafe24b925.webp
    width: 320
    height: 434
  color: '#f9f9f9'
---

I got access to GPT-6 Astra this afternoon, so naturally I used it to generate [SVGs of pelicans riding bicycles](https://simonwillison.net/tags/pelican-riding-a-bicycle/)—at low, medium, high, xhigh and max reasoning levels (Astra doesn’t support reasoning=none). Then I rendered those pelicans in [a comparison grid](https://static.simonwillison.net/static/2026/gpt-6-and-5.6-pelicans.html) with GPT-5.6 Sol, Terra, and Luna, and beyond being fun the result was surprisingly useful.

![Comparison grid showing gpt-6-astra, gpt-5.6-sol, gpt-5.6-terra, gpt-5.6-luna at 6 different reasoning levels with pelicans and token counts and prices for each one.](https://static.simonwillison.net/static/2026/astra-grid-3.webp) See [the grid](https://static.simonwillison.net/static/2026/gpt-6-and-5.6-pelicans.html) for full quality images. Here’s [the transcript](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2Ff789d2784fc6c5b870cc80f0b7cd9d01) that created the GPT-6 Nova pelicans.

There are a few interesting things that stand out from this grid.

- The Astra pelicans are *much better*. The very best GPT-5.6-Sol pelican (I liked xhigh better than max) is still pretty clearly a bunch of abstract shapes. Every single one of the Astra pelicans, from low to xhigh, looks better than that. The Astra max one is really good.
- Astra below max still doesn’t reliably get the pelican legs on both sides of the frame.
- In terms of cost, Astra may be around twice the price of Sol ($10/million input, $50/million output, compared to $5/$30 for Sol), but it uses significantly less tokens at each of the levels, making the prices at the different levels closer than they might otherwise be.
- Astra low produces a better pelican than ANY of the GPT-5.6 Sol models at any level, for 9.55 cents. Spending 10 cents on any other model gets a much worse result.
- Look at the input token counts: Astra and Luna both used 16 input tokens, Sol and Terra used 26. That’s interesting.

I wonder if Astra and Luna are more related to each other than OpenAI let on?
