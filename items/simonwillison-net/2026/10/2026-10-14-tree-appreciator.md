---
title: Tree Appreciator
link: https://simonwillison.net/2026/Oct/14/tree-appreciator/
source: simonwillison-net
published: 2026-10-14T21:23:01Z
updated: 2026-10-14T21:23:01Z
first_seen: 2026-10-15T04:26:51.711869061Z
labels:
- ai
- claude
- game-design
- generative-ai
- javascript
- llms
- three-js
- vibe-coding
summary: 'Tool: Tree Appreciator Another game experiment with Claude. This one used the default Claude iPhone app (chat mode, not Claude Code mode) where I prompted Claude Opus 5.5 Max with: Build a three js game called "Tree appreciator" - it shows you two algorithmically generated trees and the game mechanics is about appreciating the differences between them. Think hard about how to make sure it is both fun and beautiful. Somewhat embarrassingly given my recent talk, I''ve found that telling Opus 5.5 "make sure it is fun" works uncomfortably well. In this case Claude crunched for 3 hours and 7 minutes before retuning a cosy little spot-the-difference game with music and animation and achievements that''s quite compelling to play. Tags: game-design, javascript, ai, generative-ai, llms, claude, vibe-coding, three-js'
content: extracted
html: 2026-10-14-tree-appreciator.html
remote_preview:
  url: https://static.simonwillison.net/static/2026-10-14/image_crop_1320x2195_w1320_q0.7.webp
  alt: Screenshot of a mobile spot-the-difference puzzle game with a dark interface. Header reads "No. 003 Acer autumnalis ‘Hid…" with subtitle "Autumn maple · A country lane in late a…" and icons for sound, a book, and help. Two nearly identical 3D-rendered scenes are stacked vertically, labeled "Fig. i"
---

[Tree Appreciator](https://tools.simonwillison.net/tree-appreciator) — Tree Appreciator is a slow spot-the-difference game in which each round grows two 3D trees from one seed and asks the player to find a handful of small differences between them, such as a missing bough, a visiting bird, or an early-turning branch. Players tap a difference in either tree to notice it, can use limited hints and drag or zoom to view the trees from different angles, and earn appreciation points, leaves and herbarium entries for each pair completed.

Another game experiment with Claude. This one used the default Claude iPhone app (chat mode, not Claude Code mode), where I prompted Claude Opus 5.5 Max with:

> `Build a three js game called "Tree appreciator" - it shows you two algorithmically generated trees and the game mechanics is about appreciating the differences between them. Think hard about how to make sure it is both fun and beautiful.`

Somewhat embarrassingly given [my recent talk](https://simonwillison.net/2026/Sep/27/2026-in-llms-so-far/#simon-willison-2026-in-llms.059.webp), I've found that telling Opus 5.5 "make sure it is fun" works uncomfortably well. In this case Claude [crunched for 3 hours and 7 minutes](https://claude.ai/share/ee7de555-f85c-4ff0-96cb-d51ed5e479e9) before retuning a cosy little spot-the-difference game with music and animation and achievements that's quite compelling to play.

![Screenshot of a mobile spot-the-difference puzzle game with a dark interface. Header reads \"No. 003 Acer autumnalis ‘Hid…\" with subtitle \"Autumn maple · A country lane in late a…\" and icons for sound, a book, and help. Two nearly identical 3D-rendered scenes are stacked vertically, labeled \"Fig. i\" and \"Fig. ii\", each showing a dark-trunked maple tree with red, orange and pale peach leaves, leaves falling through the air, a golden grassy field with a few small rocks, and hazy hills with pine silhouettes under a pale sky. Two differences are circled in blue in both images: circle \"a\" at the base of the trunk, where Fig. i has a small red mushroom that is missing in Fig. ii, and circle \"b\" in the right side of the canopy, where Fig. i has a patch of green leaves that are red in Fig. ii. The bottom bar shows \"2 of 4\", a reset button, a \"Hint\" button with three filled and two empty dots, and a score of \"1,803\".](https://static.simonwillison.net/static/2026-10-14/image_crop_1320x2195_w1320_q0.7.webp)
