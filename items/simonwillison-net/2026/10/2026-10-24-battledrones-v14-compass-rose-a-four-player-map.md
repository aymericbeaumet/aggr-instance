---
title: 'battledrones v14: Compass Rose, a four player map'
link: https://simonwillison.net/2026/Oct/24/battledrones/
source: simonwillison-net
published: 2026-10-24T03:59:57Z
updated: 2026-10-24T03:59:57Z
first_seen: 2026-10-24T16:53:24.985256762Z
labels:
- claude
- game-design
- projects
summary: 'Release: battledrones v14: Compass Rose, a four player map I added four player online mode to Battledrones - previous maps were limited to two. Here''s the initial prompt I used: Add a new map for a four player battle - four way rotational symmetry, up to four AI players, works with the WebSocket non-audio based multiplayer mode so up to four human players can join (any spare slots use AI) Claude Opus 4.5 added a new map called "Compass Rose" and modified the WebSockets multiplayer network code to support up to four players for that map, joining via share links or QR codes. The new teams use robots themed in purple and green, joining the existing blue and red designs. You can preview those colors in the updated model browser. Here''s a replay link to watch a four way AI battle with the "Goliath Armada" mutator turned on, where each side starts with four giant Goliath mech units. Tags: game-design, projects, claude'
content: extracted
html: 2026-10-24-battledrones-v14-compass-rose-a-four-player-map.html
remote_preview:
  url: https://raw.githubusercontent.com/simonw/battledrones/v14/progress-shots/v14/compass-rose.jpg
  alt: The new map does indeed look a bit like a rose - it uses the cold and ice biome. Also shown are purple robots battling on the map.
---

I added four player online mode to [Battledrones](https://simonwillison.net/2026/Oct/20/battledrones/) - previous maps were limited to two.

Here's the initial prompt I used:

> `Add a new map for a four player battle - four way rotational symmetry, up to four AI players, works with the WebSocket non-audio based multiplayer mode so up to four human players can join (any spare slots use AI)`

Claude Opus 4.5 added a new map called "Compass Rose" and modified the WebSockets multiplayer network code to support up to four players for that map, joining via share links or QR codes.

![The new map does indeed look a bit like a rose - it uses the cold and ice biome. Also shown are purple robots battling on the map.](https://raw.githubusercontent.com/simonw/battledrones/v14/progress-shots/v14/compass-rose.jpg)

The new teams use robots themed in purple and green, joining the existing blue and red designs. You can preview those colors in [the updated model browser](https://simonw.github.io/battledrones/models.html).

Here's [a replay link](https://simonw.github.io/battledrones/v14/?replay=https://gist.githubusercontent.com/simonw/faa550a002b25a78f1c88a64217786c1/raw/4ff9b03de71d757cd33eb5d1a0e388155b6c20e5/compass.json) to watch a four way AI battle with the "Goliath Armada" mutator turned on, where each side starts with four giant Goliath mech units.
