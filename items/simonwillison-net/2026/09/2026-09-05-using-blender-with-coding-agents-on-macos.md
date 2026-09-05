---
title: Using Blender with coding agents on macOS
link: https://simonwillison.net/2026/Sep/5/blender-coding-agents-macos/
source: simonwillison-net
published: 2026-09-05T15:51:09Z
updated: 2026-09-05T15:51:09Z
first_seen: 2026-09-05T17:32:09.253098500Z
labels:
- ai
- blender
- coding-agents
- generative-ai
- gpt-6-astra
- llms
- pelican-riding-a-bicycle
summary: 'TIL: Using Blender with coding agents on macOS I''ve been having fun with Blender in ChatGPT Codex on my Mac recently. Getting it to work with coding agents is really easy: install the full Mac application from blender.org and run a prompt like this: Use the already install /Applications/Blender to render a scene of a pelican riding a bicycle In this case I followed that up with these two prompts: OK add a background and a lot of flair Then: OK make it a whole lot better And got this image, generated using Blender''s Python API: Tags: ai, generative-ai, llms, blender, pelican-riding-a-bicycle, coding-agents, gpt-6-astra'
content: extracted
html: 2026-09-05-using-blender-with-coding-agents-on-macos.html
---

[TIL](https://simonwillison.net/elsewhere/til/) [Using Blender with coding agents on macOS](https://til.simonwillison.net/llms/blender-coding-agents-macos) — Modern frontier models have got \*really good\* at using Blender. I've been having a lot of fun trying this out recently - models can produce \`.blend\` files you can edit in Blender itself, and can also render images and even movies (by rendering a sequence of images and combining them with \`ffmpeg\`).

I've been having fun with Blender in ChatGPT Codex on my Mac recently. Getting it to work with coding agents is really easy: install the full Mac application from [blender.org](https://www.blender.org) and run a prompt like this:

> `Use the already install /Applications/Blender to render a scene of a pelican riding a bicycle`

In this case I followed that up with these two prompts:

> `OK add a background and a lot of flair`

Then:

> `OK make it a whole lot better`

And got this image, generated [using Blender's Python API](https://github.com/simonw/gpt-6-astra-blender-pelican-bicycle/blob/main/work/pelican_final.py):

![A 3D illustration of a white pelican cycling along a seaside boardwalk at sunset. It wears a cream boater hat and a coral scarf, with wings on the handlebars and long orange legs reaching the pedals of a turquoise bicycle. A wicker front basket holds pink and white flowers, and three balloons float behind. Pastel bunting stretches overhead between palm trees. Striped beach huts stand beside a teal sea with a small sailboat, beneath a large peach-colored sun. The scene has a softly lit, toy-like style.](https://static.simonwillison.net/static/2026/astra-blender-pelican.jpg)
