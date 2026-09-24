---
title: 'Show HN: Make cursed fonts like Times New Bastard'
link: https://bastardica.mitpit.com/
source: hnrss-org-frontpage
published: 2026-09-23T22:53:28Z
updated: 2026-09-23T22:53:28Z
first_seen: 2026-09-24T20:00:20.682990735Z
authors:
- MitPitt
summary: 'A joke tool that abuses OpenType''s ligature feature to mix fonts. It works pretty fast on client-side by loading Python in WASM. Comments URL: https://news.ycombinator.com/item?id=49823738 Points: 165 # Comments: 28'
content: extracted
html: 2026-09-23-show-hn-make-cursed-fonts-like-times-new-bastard.html
preview:
  file: 2026-09-23-show-hn-make-cursed-fonts-like-times-new-bastard.preview-7c3700d56358.webp
  width: 256
  height: 113
  color: '#282622'
images:
- source: https://bastardica.mitpit.com/scr2.png
  original:
    file: 2026-09-23-show-hn-make-cursed-fonts-like-times-new-bastard.image-9ead416d10fe.png
    width: 1189
    height: 523
  color: '#1a1814'
---

A foundry for bastard web fonts. \
Mix, stretch and / or squish them.

Base font

Upload font…

Mix-in font

Upload font…

Base font but every th glyph is mix-in font

Options

Effects applied to mix-in font

Sample font Live update

Output formats

Presets click one to load settings

FAQ

Where can I use bastard fonts?

Every download is a normal OpenType font. The swap is a `liga` contextual substitution registered for every script, so browsers turn it on by default.

It works anywhere OpenType text is shaped: browsers, design tools, print. If a font looks unchanged, check that ligatures aren't switched off in the app you're using.

Are my fonts uploaded anywhere?

No. Everything runs locally in your browser with [Pyodide](https://pyodide.org/) and [fontTools](https://github.com/fonttools/fonttools).

Any pro tips?

Bastardica can make simple fonts feel a little, hmm, richer? Use Y-offset and scale effects to make glyphs align perfectly.

When mixing 3 or more fonts, they will intersect (e.g. every 5th and every 7th will collide on every 35th). The first font wins. The stride won't break for either.

Use prime numbers for strides, so the mix-in fonts collide more rarely.

Some websites to grab free fonts to play with: [Google Fonts](https://fonts.google.com), [UNCUT](https://uncut.wtf/), [Velvetyne](https://velvetyne.fr/), [Font Squirrel](https://www.fontsquirrel.com/), [FontSpace](https://www.fontspace.com/), [DaFont](https://www.dafont.com/).

What about font licensing?

Mixing two fonts produces a derivative work, so make sure to check licenses of both source fonts if you plan to use a bastard font commercially. Bastardica adds no conditions of its own. A credit is appreciated, but optional.

Bastardica was inspired by *[Times New Bastard](https://github.com/weiweihuanghuang/Times-New-Bastard)* and *[Easy Pete](https://www.youtube.com/watch?v=Gm_d8EU_PjM)*.

You can ask me about anything at [\[email protected\]](https://bastardica.mitpit.com/cdn-cgi/l/email-protection#a0c8c5cccccfe0cdc9d4d0c9d48ec3cfcd)
