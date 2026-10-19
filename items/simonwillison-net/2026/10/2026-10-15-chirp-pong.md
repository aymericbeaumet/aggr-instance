---
title: Chirp Pong
link: https://simonwillison.net/2026/Oct/15/chirp-pong/
source: simonwillison-net
published: 2026-10-15T19:47:52Z
updated: 2026-10-15T19:47:52Z
first_seen: 2026-10-15T21:44:29.602546406Z
labels:
- ai
- claude-code
- game-design
- generative-ai
- llms
summary: 'Tool: Chirp Pong Another game experiment - I wanted to see if it was feasible to have a two player game on two separate phones running in static HTML where the networking code uses chirps sent via the microphone. It seems to work! Build a two player competitive pong like game where two players use their phones close to each other and the game state is sent via audio noises which the web microphone API can interpret - but add graphical and gameplay twists to make it a lot more fun and intertwine than pong Claude Opus 5.5 Extra in Claude Code. Tags: game-design, ai, generative-ai, llms, claude-code'
content: extracted
html: 2026-10-15-chirp-pong.html
---

[Chirp Pong](https://tools.simonwillison.net/chirp-pong) — Chirp Pong is a two-player paddle game where two phones pair and exchange the ball over sound, using each device's speaker and microphone instead of Wi-Fi or Bluetooth. The page offers a duel mode that pairs two phones through a chirp handshake and a practice mode against a CPU opponent, with selectable match lengths of 7, 11, or 15 points.

Another game experiment - I wanted to see if it was feasible to have a two player game on two separate phones running in static HTML where the networking code uses chirps sent via the microphone. It seems to work!

> `Build a two player competitive pong like game where two players use their phones close to each other and the game state is sent via audio noises which the web microphone API can interpret - but add graphical and gameplay twists to make it a lot more fun and intertwine than pong`

Claude Opus 5.5 Extra [in Claude Code](https://claude.ai/code/session_019awUnNB7NoYd7noBNAsNdd).
