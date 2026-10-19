---
title: Scrimshaw Jukebox
link: https://simonwillison.net/2026/Oct/6/scrimshaw-jukebox/
source: simonwillison-net
published: 2026-10-06T15:17:39Z
updated: 2026-10-06T15:17:39Z
first_seen: 2026-10-06T19:48:54.930461426Z
labels:
- ai
- claude
- generative-ai
- llms
- vibe-coding
summary: 'Tool: Scrimshaw Jukebox I wanted to see if Claude Opus 5.5 could compose music, so I tried this: I want you to write some computer game music for me. First design simple text based format for the music and build an artifact that can play it out loud - include some example tracks in that artifact I am looking for music of the quality of the original secret of Monkey Island It leaned a lot harder into the Monkey Island theme than I had intended, but the results are surprisingly good. I wonder if the ability to compose competent music is similar to the 3D graphics thing - a new capability for text models that emerged in the past few months? Would need some careful experiments with other recent and not-so-recent models to confirm if this is new or if they''ve been able to do this for a while. Tags: ai, generative-ai, llms, claude, vibe-coding'
content: extracted
html: 2026-10-06-scrimshaw-jukebox.html
remote_preview:
  url: https://static.simonwillison.net/static/2026/scrimshaw.webp
  alt: Screenshot of a retro pixel-art web music player. Header in blackletter type reads "Scrimshaw Jukebox", with the text "Six original adventure-game tracks, written as plain text and played by a synthesizer running in your browser. Pick a tune, press Play, then open the score and change it." A large p
---

[Scrimshaw Jukebox](https://tools.simonwillison.net/scrimshaw-jukebox) — Play synthesized adventure-game music written as plain text scores that you can edit and modify in real time. Six original compositions in the Scrimshaw format showcase various instruments and styles, from a calypso harbour theme to a tavern jig, with a pixel-art scene that reacts to the music and an interactive piano roll display.

I wanted to see if Claude Opus 5.5 could compose music, so [I tried this](https://claude.ai/share/1f721c20-2499-4d23-b368-3ab57146d956):

> `I want you to write some computer game music for me. First design simple text based format for the music and build an artifact that can play it out loud - include some example tracks in that artifact`
>
> `I am looking for music of the quality of the original secret of Monkey Island`

It leaned a lot harder into the Monkey Island theme than I had intended, but the results are surprisingly good.

[![Screenshot of a retro pixel-art web music player. Header in blackletter type reads \"Scrimshaw Jukebox\", with the text \"Six original adventure-game tracks, written as plain text and played by a synthesizer running in your browser. Pick a tune, press Play, then open the score and change it.\" A large pixel-art scene shows a harbor at night under a purple starry sky: a full moon at top right reflecting on the water, an island silhouette on the left with palm trees and a hut with two lit windows, and a sailing ship moored at a wooden pier. Overlaid on the scene: \"Moonlit Harbor\" and \"Press Play\". Below it a bar reads \"Play Moonlit Harbor\". A control panel has buttons \"Play\", \"Stop\", \"Restart\", \"Loop: on\", \"Edit score\", \"Read guide\" and a \"Volume\" slider set to about three quarters. A track list of six cards, the first highlighted: \"Moonlit Harbor 100 bpm · 4/4 · 16 voices · 1:26\", \"The Rusty Anchor 112 bpm · 6/8 · 8 voices · 0:56\", \"The Ghost Galleon 66 bpm · 4/4 · 9 voices · 2:11\", \"The Jungle Path 92 bpm · 4/4 · 12 voices · 1:29\", \"Duel on the Docks 152 bpm · 4/4 · 12 voices · 1:13\", \"Lantern Waltz 96 bpm · 3/4 · 8 voices · 1:38\". A section titled \"Score view\" with the caption \"1:26 · 4/4 at 100 bpm · Main theme. A calypso for a harbour town after dark.\" shows a piano-roll visualization of colored horizontal note bars and percussion ticks on a dark background, with a section marker \"A\" and a yellow vertical playhead line. A color-coded legend of voices reads: \"pan steeldrum\", \"flute flute\", \"marimba marimba\", \"skank organ\", \"strings strings\", \"harp harp\", \"bass fretless\", \"timp timpani\", \"kick kick\", \"rim rim\", \"shaker shaker\", \"conga conga\", \"tumba tumba\", \"bongo bongo\", \"crash crash\", \"surf surf\". Footer text: \"Click a voice to mute it. Space bar plays and stops.\"](https://static.simonwillison.net/static/2026/scrimshaw.webp)](https://tools.simonwillison.net/scrimshaw-jukebox)

I wonder if the ability to compose competent music is similar to the 3D graphics thing - a new capability for text models that emerged in the past few months?

Would need some careful experiments with other recent and not-so-recent models to confirm if this is new or if they've been able to do this for a while.
