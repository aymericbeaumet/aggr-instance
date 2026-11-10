---
title: llm-anthropic 0.31
link: https://simonwillison.net/2026/Nov/9/llm-anthropic/
source: simonwillison-net
published: 2026-11-09T23:58:46Z
updated: 2026-11-09T23:58:46Z
first_seen: 2026-11-10T03:25:21.974411074Z
labels:
- ai
- anthropic
- claude
- claude-mythos-fable
- generative-ai
- llm
- llms
- pelican-riding-a-bicycle
summary: 'Release: llm-anthropic 0.31 Anthropic released Fable 5.2 and Mythos 5.2 today. I''ve added them to llm-anthropic, so you can now run prompts through Fable 5.2 like this: uv tool install llm llm install llm-anthropic llm keys set anthropic # paste key here llm -m claude-fable-5.2 "Generate an SVG of a pelican riding a bicycle" I ran pelicans at all five reasoning levels - low, medium, high, xhigh, and max. You can see them all here - here''s the best result I got, from xhigh: max failed to return an image at all, running out of tokens after spending 128,000 thinking about them. That''s the same bug that I saw with Opus 5.5 - but it''s new for the Fable series, both Fable 5 and Fable 5.1 returned max effort pelicans just fine. Tags: llm, anthropic, claude, generative-ai, pelican-riding-a-bicycle, ai, llms, claude-mythos-fable'
content: extracted
html: 2026-11-09-llm-anthropic-0-31.html
remote_preview:
  url: https://static.simonwillison.net/static/2026/fable-5.2-xhigh.avif
  alt: It's good - legs the right side of the bicycle, good frame, nice pouch, weird little fingers on the handlebars. The pouch looks slightly misaligned with the top of the beak.
---

Anthropic [released Fable 5.2 and Mythos 5.2](https://www.anthropic.com/claude-fable-and-mythos-5-2) today. I've added them to `llm-anthropic`, so you can now run prompts through Fable 5.2 like this:

```
uv tool install llm
llm install llm-anthropic
llm keys set anthropic
# paste key here
llm -m claude-fable-5.2 "Generate an SVG of a pelican riding a bicycle"
```

I ran pelicans at all five reasoning levels - `low`, `medium`, `high`, `xhigh`, and `max`. You can [see them all here](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2Fbcd1a1bf38b929b0bbbae2e38b808e97) - here's the best result I got, from `xhigh`:

![It's good - legs the right side of the bicycle, good frame, nice pouch, weird little fingers on the handlebars. The pouch looks slightly misaligned with the top of the beak.](https://static.simonwillison.net/static/2026/fable-5.2-xhigh.avif)

`max` failed to return an image at all, running out of tokens after spending 128,000 [thinking about them](https://tools.simonwillison.net/markdown-svg-renderer?url=https%3A%2F%2Fgist.github.com%2Fsimonw%2Fbcd1a1bf38b929b0bbbae2e38b808e97#reasoning-2). That's the same bug that I saw with [Opus 5.5](https://simonwillison.net/2026/Sep/22/opus-and-sol-and-luna/#claude-opus-5-5-max-over-thinks-to-the-point-of-breaking) - but it's new for the Fable series, both [Fable 5](https://simonwillison.net/2026/Jun/9/claude-fable-5/#and-some-pelicans) and [Fable 5.1](https://simonwillison.net/2026/Sep/1/claude-fable-5-1/#max) returned max effort pelicans just fine.
