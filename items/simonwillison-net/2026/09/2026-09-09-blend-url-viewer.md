---
title: .blend URL Viewer
link: https://simonwillison.net/2026/Sep/9/blender-viewer/
source: simonwillison-net
published: 2026-09-09T23:58:32Z
updated: 2026-09-09T23:58:32Z
first_seen: 2026-09-10T04:31:32.185563254Z
labels:
- 3d
- ai
- blender
- codex
- coding-agents
- generative-ai
- gpt-6-astra
- javascript
- llms
- tools
summary: 'Tool: .blend URL Viewer I''m continuing to have a lot of fun with GPT-6 Astra and Blender (see my TIL). As a big fan of the Imperial Fabergé Easter eggs, I''ve always thought it would be fun to make some new ones that celebrate popular culture. Yesterday I decided to try out the new ChatGPT Images 2.5 by running this prompt: Generate a photo of a faberge egg that''s themed after the TV show Pluribus - research first It gave me this - honestly not bad for a first attempt! Then, just to see what would happen, I pasted that image into Codex running GPT-6 Astra (high) and prompted: Use your blender local skill to create a blender model of this faverge egg (Here''s the skill file, which I created like this.) It churned away for 17m51s and built me several .blend files. I already had this vibe-coded Blender viewing experiment lying around, so I added that to my tools collection and now you can use it to see my Pluribus blender model in your browser: Tags: 3d, javascript, tools, ai, generative-ai, llms, blender, coding-agents, codex, gpt-6-astra'
content: extracted
html: 2026-09-09-blend-url-viewer.html
preview:
  file: 2026-09-09-blend-url-viewer.preview-3b1dc6b5e699.webp
  width: 256
  height: 180
  alt: Screenshot of a web-based 3D Blender file viewer showing a rendered gold Fabergé-style jeweled egg. The top of the egg is lifted open, revealing a miniature desert diorama inside with green cacti, brown rocks, and a small black-suited figure standing in the center. The egg's shell is decorated with
  color: '#857d72'
images:
- source: https://static.simonwillison.net/static/2026/pluribus-blender-viewer.webp
  original:
    file: 2026-09-09-blend-url-viewer.image-566d706a973f.webp
    width: 1327
    height: 934
  variants:
  - file: 2026-09-09-blend-url-viewer.image-7bf1102f4bac.webp
    width: 48
    height: 34
  - file: 2026-09-09-blend-url-viewer.image-f2580b898dba.webp
    width: 320
    height: 225
  color: '#86807b'
- source: https://static.simonwillison.net/static/2026/pluribus-egg.jpg
  original:
    file: 2026-09-09-blend-url-viewer.image-e23aad84bd9d.jpg
    width: 1122
    height: 1402
  variants:
  - file: 2026-09-09-blend-url-viewer.image-9cbce6290068.webp
    width: 48
    height: 60
  color: '#332c29'
---

[Tool](https://simonwillison.net/elsewhere/tool/) [.blend URL Viewer](https://tools.simonwillison.net/blender-viewer) — View Blender .blend files directly in your browser by pasting a URL to a CORS-accessible file or GitHub repository link. The viewer renders mesh geometry with materials, lighting, and optional saved camera positions from Blender 5.x files, and provides interactive orbit controls, wireframe mode, and model fitting capabilities.

I'm continuing to have a lot of fun with GPT-6 Astra and Blender (see [my TIL](https://til.simonwillison.net/llms/blender-coding-agents-macos)).

As a big fan of the [Imperial Fabergé Easter eggs](https://en.wikipedia.org/wiki/Faberg%C3%A9_egg), I've always thought it would be fun to make some new ones that celebrate popular culture.

Yesterday I decided to try out the new [ChatGPT Images 2.5](https://simonwillison.net/2026/Sep/8/introducing-chatgpt-images-25/) by [running this prompt](https://chatgpt.com/share/6aa1f6d2-a7d8-83ea-92ec-0daeb8422617):

> `Generate a photo of a faberge egg that's themed after the TV show Pluribus - research first`

It gave me this - honestly not bad for a first attempt!

![It's a yellow Fabergé egg, half open. Inside you can clearly see Carol in her black outfit facing off against a choir of creepy white-faced joined humans. There are cacti and it has an Albuquerque New Mexico feel to it. The egg stand has a Pluribus plaque.](https://static.simonwillison.net/static/2026/pluribus-egg.jpg)

Then, just to see what would happen, I pasted that image into Codex running GPT-6 Astra (high) and prompted:

> `Use your blender local skill to create a blender model of this faverge egg`

(Here's [the skill file](https://github.com/simonw/gpt-6-astra-blender-pelican-bicycle/blob/main/outputs/blender-local/SKILL.md), which I created [like this](https://til.simonwillison.net/llms/blender-coding-agents-macos#creating-a-skill).)

It churned away for 17m51s and built me [several `.blend` files](https://github.com/simonw/vibe-coded-blender-projects/tree/main/pluribus-faberge-egg/deliverables). I already had this vibe-coded Blender viewing experiment lying around, so I added that to my [tools collection](https://tools.simonwillison.net/) and now you can use it to [see my Pluribus blender model in your browser](https://tools.simonwillison.net/blender-viewer?url=https%3A%2F%2Fgithub.com%2Fsimonw%2Fvibe-coded-blender-projects%2Fblob%2Fmain%2Fpluribus-faberge-egg%2Fdeliverables%2FPluribus_Jeweled_Egg_v1.blend):

![Screenshot of a web-based 3D Blender file viewer showing a rendered gold Fabergé-style jeweled egg. The top of the egg is lifted open, revealing a miniature desert diorama inside with green cacti, brown rocks, and a small black-suited figure standing in the center. The egg's shell is decorated with green gems, pearls, ornate scrollwork, a ring of smiling white egg-shaped faces, and oval medallions painted with a cactus and desert scene. The egg stands on four gold legs over a black marbled base with a gold plaque reading PLURIBUS. The URL bar reads https://github.com/simonw/vibe-coded-blender-projects/blob/main/pluribus-faberge-egg/deliverables/Pluribus_Jeweled_Egg_v1.blend with a View button; top-right buttons read Fit, Wireframe, Clear. A bottom-left info panel reads: Loaded Blender 05.01 · 7.2 MB / 387 meshes · 1099 curves · 1 text · 783,764 vertices · 1,446,560 triangles · 17 materials / Preview: approximate materials and text; unapplied modifiers are omitted. / GitHub URL resolved through jsDelivr](https://static.simonwillison.net/static/2026/pluribus-blender-viewer.webp)
