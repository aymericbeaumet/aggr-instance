---
title: Video compressor
link: https://simonwillison.net/2026/Sep/7/video-compressor/
source: simonwillison-net
published: 2026-09-07T18:29:07Z
updated: 2026-09-07T18:29:07Z
first_seen: 2026-09-07T22:34:46.032173704Z
labels:
- claude
- claude-code
- claude-mythos-fable
- ffmpeg
- video
- webassembly
summary: 'Tool: Video compressor I recorded a short demo video of my Equal Earth animation on my phone and wanted to publish an optimized version of that video (using FFMPEG) on my blog, so I had Claude Fable 5.1 in Claude Code for web build me this tool using the WebAssembly build of FFMPEG. Tags: ffmpeg, video, webassembly, claude, claude-code, claude-mythos-fable'
content: extracted
html: 2026-09-07-video-compressor.html
preview:
  file: 2026-09-07-video-compressor.preview-336d7f265751.webp
  width: 256
  height: 128
  color: '#d8d9d8'
images:
- source: https://static.simonwillison.net/static/2026/video-compressor-card.jpg
  original:
    file: 2026-09-07-video-compressor.image-6cad701c12c3.jpg
    width: 1200
    height: 600
  variants:
  - file: 2026-09-07-video-compressor.image-9c492e43121a.webp
    width: 48
    height: 24
  color: '#fafafa'
- source: https://static.simonwillison.net/static/2026-09-07/video-compressor.webp
  original:
    file: 2026-09-07-video-compressor.image-141f18f9142a.webp
    width: 1800
    height: 1496
  variants:
  - file: 2026-09-07-video-compressor.image-6e1976c58cf0.webp
    width: 48
    height: 40
  - file: 2026-09-07-video-compressor.image-bc7186edb021.webp
    width: 320
    height: 266
  - file: 2026-09-07-video-compressor.image-aabfc312a126.webp
    width: 640
    height: 532
  color: '#fbfbfb'
---

I recorded a short demo video of [my Equal Earth](https://simonwillison.net/2026/Sep/7/equal-earth/) animation on my phone and wanted to publish an optimized version of that video (using FFMPEG) on my blog, so I had Claude Fable 5.1 in Claude Code for web [build me this tool](https://claude.ai/code/session_01QHTdJZ4xg6TZfDXCmuvAE9) using the WebAssembly build of FFMPEG.

![Screenshot of a video compression web tool. Under \"Versions to generate\" is a table of five presets (Largest, Large, Medium, Small, Smallest) with output sizes of 854×370 or 640×276, CRF quality settings from 22 to 28, and audio bitrates from 128 to 64 kbps, plus options for encoder speed, H.264 profile, 30 fps limit, stripping metadata, dropping audio, and encoding only the first 10 seconds. A green \"Generate versions\" button reads \"Done: 5 versions in 11.8s.\" Below, \"Results, smallest first\" shows three video players: Smallest at 145 KB (48% of original), Medium at 241 KB (79%), and Small at 264 KB (87%), each with a Download .mp4 button and a collapsible ffmpeg command.](https://static.simonwillison.net/static/2026-09-07/video-compressor.webp)
