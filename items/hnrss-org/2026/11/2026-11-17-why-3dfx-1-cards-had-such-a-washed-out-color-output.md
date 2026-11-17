---
title: Why 3dfx 1 cards had such a washed out color output?
link: https://twitter.com/id_aa_carmack/status/2122478083319992513
source: hnrss-org
published: 2026-11-17T00:16:24Z
updated: 2026-11-17T00:16:24Z
first_seen: 2026-11-17T09:02:46.426369911Z
authors:
- mfiguiere
content: extracted
html: 2026-11-17-why-3dfx-1-cards-had-such-a-washed-out-color-output.html
remote_preview:
  url: https://pbs.twimg.com/profile_images/1560764938083352577/B1X3m4NN_400x400.jpg
  alt: John Carmack profile picture
---

[![@ID_AA_Carmack](https://pbs.twimg.com/profile_images/1560764938083352577/B1X3m4NN_reasonably_small.jpg)](https://x.com/ID_AA_Carmack)

Both NeXT and 3DFX tried to “do the right thing” with color spaces, and it wound up generally making them look worse than competing options. I didn’t really understand it properly until years later. It is natural to think that a pixel with a value of 1.0 is emitting twice the photons as a pixel with a value of 0.5, so you can do the obvious linear math to calculate partial pixel coverage, blending a foreground over a background, or interpolating between colors in a texture. If displays are “linear”, that is correct. NeXT and 3DFX were much closer to linear than other contemporary displays. NeXT had crossover from the deep graphics understanding at Pixar, and 3DFX had the SGI heritage. They did this intentionally. If you have 10+ bits per channel like the professional rendering systems, linear displays are The Right Thing. Unfortunately, NeXTStation Colors were 4 bits per channel and 3DFX was 5 or 6 bits per channel, and there… it was not the right thing. Human eyes aren’t linear intensity sensors. We mostly register relative changes in intensity, so we can tell the difference between 0.01 and 0.02 as a huge doubling, while 0.99 to 1.00 is almost imperceptible. This distribution of intensities is often referred to as the “gamma” of the display system, but the actual curves like sRGB are a bit more complicated. If you only have a small number of discrete pixel values, you definitely don’t want to spread them evenly from 0 to 1! If you do, you will have good precision in the brights, but very stark “intensity bands” in the dark colors. If everything was authored specifically for 3DFX, you would still have the fundamental problem with the dark bands, but more accurate blending and filtering. The problem was that artists made all of their textures in paint programs on conventional non-linear displays, so when they were drawn by 3DFX cards, they got the characteristic “washed out color” look. You can mathematically correct textures for differences in display gamma, but I didn’t really understand the issue back in the glQuake days, and we were so enthralled by the absolute performance of 3DFX that the color fidelity didn’t get as much attention as it should have. Even today, tons of games don’t properly handle gamma across their textures and render targets. High resolutions and 24 bit color masks most of it, but you still see it in some slight shimmerings in high contrast textures and edges.

[![@fabynou](https://pbs.twimg.com/profile_images/1606428066141548544/BJe7On8y_bigger.jpg)](https://x.com/fabynou)

Does anybody know why 3DfX 1 cards had such a washed out color output? In my research, circa '97 I found many people complaining about the colors. Is it gamma? 16-bit color? What can it be?

![](https://pbs.twimg.com/media/HXIeifMagAAV9Vf?format=webp&name=large)
