---
title: Apple iPod Engraver (2019)
link: https://dunstanorchard.com/apple-ipod-engraver/
source: hnrss-org-frontpage
published: 2026-09-09T01:57:37Z
updated: 2026-09-09T01:57:37Z
first_seen: 2026-09-13T01:23:10.731749865Z
authors:
- NaOH
summary: 'Article URL: https://dunstanorchard.com/apple-ipod-engraver/ Comments URL: https://news.ycombinator.com/item?id=49619848 Points: 114 # Comments: 22'
content: extracted
html: 2026-09-09-apple-ipod-engraver-2019.html
preview:
  file: 2026-09-09-apple-ipod-engraver-2019.preview-384c8f030235.webp
  width: 256
  height: 256
  color: '#f3f4f4'
images:
- source: https://dunstanorchard.com/apple-ipod-engraver/og.jpg
  original:
    file: 2026-09-09-apple-ipod-engraver-2019.image-8b14fc846669.jpg
    width: 1000
    height: 1000
  color: '#fefefe'
- source: https://dunstanorchard.com/apple-ipod-engraver/hero-2050.png
  original:
    file: 2026-09-09-apple-ipod-engraver-2019.image-add7b9d9ba88.png
    width: 2050
    height: 494
  variants:
  - file: 2026-09-09-apple-ipod-engraver-2019.image-68a7b72979c0.webp
    width: 48
    height: 12
  color: '#d6d7d7'
---

![Eight views of an original iPod showing the front, sides, and back](https://dunstanorchard.com/apple-ipod-engraver/hero-2050.png)

Dec 12, 2019

I spent 2004–2006 working at Apple as a UI engineer for their online store. Part of my job was to prototype concepts that would add some interactive sparkle to the site.

One such project improved the “Personalize your iPod” page, where customers could submit two lines of text to be engraved on the back of the iPod they were ordering.

As originally designed the page offered no interaction beyond a plain form. I added a rotatable iPod, a live “engraved” preview of the customer’s text, and a highlight of any change in shipping times.

I animated the iPod by cycling through a series of JPEGs using JavaScript. The engraving was created using [imagemagick](https://imagemagick.org/), which took the user’s text and returned an “engraved” image to be overlaid on the rear of the iPod. The shipping highlight was achieved by switching CSS classes to change the background color, in the style of a [classic yellow fade](https://signalvnoise.com/archives/000558.php).

There’s an old [working demo](https://dunstanorchard.com/apple-ipod-engraver/demo/) if you’d like to try it yourself.

We obviously have much better ways of doing such things today, but in 2005, with limited browser technology, those solutions seemed a little bit magical.
