---
title: JPEG XL vs AVIF for web use-cases
link: https://jakearchibald.com/2026/jpeg-xl-vs-avif/
source: lobste-rs
published: 2026-10-15T14:42:45Z
updated: 2026-10-15T14:42:45Z
first_seen: 2026-10-16T09:40:32.304738087Z
authors:
- jakearchibald.com by jaffathecake
labels:
- compsci
- web
summary: Comments
content: extracted
html: 2026-10-15-jpeg-xl-vs-avif-for-web-use-cases.html
---

JPEG XL has now landed in all major browsers, so I figured I'd take a closer look at what it's good/bad at compared to other formats.

## [Conclusions](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#conclusions)

I'll go into full detail, with examples and benchmarks, but for those of you who don't have the time, here are the headlines:

**For lossy photos on the web, AVIF almost always produces smaller files than JPEG XL.** At default effort settings, the best JPEG XL version is typically 15–25% larger than the equivalent AVIF. At maximum effort settings, the gap narrows a little, to typically 6–20%. There are outliers, where JPEG XL is close to, or even beats AVIF, so if you want the absolute best result for a particular image, it's worth trying both. See the [F1 image](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#lossy---f1-image) for a typical example, and the [weasel](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#lossy---weasel) for an outlier.

**The JPEG XL encoder matters a lot.** [jixel](https://github.com/awxkee/jixel), a newer JPEG XL encoder, usually produces smaller files than [libjxl](https://github.com/libjxl/libjxl), except when the image has detailed transparency. However, there are cases where jixel appears to 'game' the metric, and clearly produces a worse result visually. See the [F1 image](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#lossy---f1-image) and [illustration](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#lossy---illustration) examples.

**For screenshots and illustrations, AVIF is much better than JPEG XL.** The equivalent JPEG XL is 30–90% larger at default effort settings, and 20–70% larger at maximum effort settings. Lossy AVIF handles these well - you don't need to use lossless formats unless you really need bit-exact results. See the [screenshot](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#lossy---screenshot) and [illustration](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#lossy---illustration) examples.

**For images with transparency, AVIF is much better than JPEG XL.** The libjxl version is 31–44% larger at default effort settings, and 61–70% larger at maximum effort settings. jixel always encodes transparency losslessly, which makes its files 2–3x the size of AVIF's. [See the data](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#lossy---alpha).

**AVIF is generally faster to encode, and uses less memory, when you factor in file size.** jixel is faster than AVIF and uses less memory, but its files are typically significantly larger, so AVIF is better on balance. See the [F1 image](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#lossy---f1-image) for one example.

**AVIF is faster to decode.** JPEG XL typically takes 2–3x as long to decode at medium quality, but the gap narrows at very high qualities, where Chrome can sometimes decode JPEG XL faster. The [people image](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#lossy---people) is one example.

**Higher effort settings are worth it for AVIF and jixel, but not libjxl.** libjxl's higher efforts are much slower, use a lot more memory, and barely improve the result. Sometimes they produce larger files than the default effort. The [screenshot](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#lossy---screenshot) is an example of this.

**AVIF's 4:2:0 mode (half colour resolution) shouldn't be your default.** It rarely reduces file size, and it makes higher qualities harder to reach. Although, it is quicker to encode and decode. Stick with 4:4:4 if you don't have time to check both. The [F1 image](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#lossy---f1-image) is one example, and the [screenshot](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#lossy---screenshot) is an exception.

**Quality settings are reliable for all the encoders.** With a single quality setting, the resulting quality varies a bit between images, but all the encoders vary in the same direction, and in ways that seem reasonable. [See the data](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#quality-stability).

**Progressive JPEG XL is basically free at high effort.** jixel recently added support for early progressive previews, and at its slowest setting they cost little or nothing, but at its default setting the files are 12% bigger. Firefox and Chrome support progressive JPEG XL. Safari still renders the final image, but doesn't show progressive previews. However, is this type of progressive rendering a good user experience? [See the examples and data](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#progressive).

**AVIF supports progressive rendering!** It's Chrome-only for now, but it's in progress for Firefox. It's more complex than progressive JPEG XL, but it gives you more control over what the preview looks like, the files are usually smaller, and they're much faster to decode. [See for yourself](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#progressive).

**For HDR, JPEG XL lacks gain maps, which is a significant limitation compared to AVIF.** The JPEG XL format supports gain maps, but they aren't supported by browser decoders, and the encoding story is messy/experimental. When comparing non-gain mapped HDR content, the two quality metrics I used disagree a lot, but they agree that JPEG XL produces smaller files compared to AVIF. [See the examples](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#lossy---hdr).

**JPEG XL can make existing JPEGs 9–20% smaller, without further loss.** It's also as fast or faster to decode, except for small images. [See the data](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#jpeg-transcoding).

**For lossless, JPEG XL is the clear winner.** Lossless WebP is 9–17% larger for photos, and lossless AVIF is 30–120% larger. But lossless images are almost never what you want on the web. If you do use them on the web (like I do on this page), watch out for jixel's higher efforts - it often produces files that are too slow to decode. [See the data](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#lossless).

**Animated image formats continue to be a dumb idea.** Please don't use them. [Learn more](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#animation), if you must.

For web use-cases, I'd currently default to AVIF 4:4:4, with the highest effort level I can be bothered with. If I felt AVIF was producing an unusually large file, I'd take a look at JPEG XL via jixel, and see if it did better.

Both AVIF and JPEG XL (via jixel) have seen massive improvements recently, so it's possible that this recommendation could change in future. There's been a lot of misinformation around this stuff, and your AI will happily deliver that misinformation to you. Instead, test things with your own images.

[AVIF folks suggest](https://giannirosato.com/blog/post/case-against-jxl/) that JPEG XL is technically limited compared to AVIF. That might be true, but in this article I'm focusing on results with today's implementations.

## [Methodology](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#methodology)

I 'created' [a benchmarking script](https://github.com/jakearchibald/image-codec-bench), that encodes images using various codecs at various settings, and records various details.

I'm using the latest released versions of the image codecs, which at time of writing is:

- [avifenc](https://github.com/aomediacodec/libavif) 1.4.2
- [libjxl](https://github.com/libjxl/libjxl) 0.12.0 (using its `cjxl` encoder)
- [jixel](https://github.com/awxkee/jixel) 0.4.2 (git `f487299`), a second JPEG XL encoder
- [cwebp](https://chromium.googlesource.com/webm/libwebp/) 1.6.0 (lossless WebP is used later in the article)

I chose a selection of images, and encoded most of them to a width of 1598 pixels - which is 2x the maximum width images appear on this blog. This means they [look sharp on high density displays](https://jakearchibald.com/2021/serving-sharp-images-to-high-density-screens/), which most users have. As a result, if you're viewing this on a small or low density screen, you're not getting the full experience.

I ran these benchmarks on my 2024 MacBook Pro (M4 Pro).

## [Lossy - F1 image](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#lossy---f1-image)

When I [first wrote about AVIF](https://jakearchibald.com/2020/avif-has-landed/), I used an F1 car as the first image, so I figured I'd do that again!

I encoded the image at various codec qualities, and chose versions that were closest to particular [SSIMULACRA2](https://github.com/cloudinary/ssimulacra2) scores - a perceptual image quality metric by the folks behind JPEG XL. 50 is considered medium quality, 70 high, 80 very high, and 90 visually lossless. Although, I'm not sure if these ratings take high density displays into account.

Throughout the article, the visual comparisons, like above, use each encoder's maximum effort, except in cases where maximum effort results in a larger file (yes, that happens).

For archival, I'd tend towards near-lossless qualities, but if I'm just displaying an illustrative image on my blog, performance is more important to me. I'm personally happy with the 70 result above. If you zoom in, you can see the quality loss, but if I was really concerned about the zoom-in case, I'd use something like the [`VisualViewport` API](https://developer.mozilla.org/en-US/docs/Web/API/VisualViewport) to load in a higher resolution image when that happens, rather than make the download bigger for everyone.

At lower qualities you can see the difference in strategy between the codecs. The detail loss is around the same, but AVIF tends to smooth things out compared to the original, whereas JPEG XL seems to introduce rougher patterns.

Also, I feel that, at medium quality (50), the AVIF and libjxl look better than the jixel version. The jixel version has a lot of visible noise around the BWT logo, and the Microsoft text on the front wing is way less legible. It makes me wonder about the metric a bit.

Anyway, here's my interpretation of the results:

- **File size: AVIF wins.** At default efforts, the best JPEG XL image is 14–17% larger than the AVIF image for the same quality. jixel is the better JPEG XL encoder at medium/high quality, and libjxl is slightly better at very high quality. At maximum efforts, the best JPEG XL image (jixel 'extraslow') is 6–15% larger than AVIF (s0).
- **jixel at its slowest setting slightly beats AVIF's default effort**, with files 1–10% smaller. The biggest gain is at very high quality.
- **Encode time: AVIF wins.** At default efforts, jixel is 12–36% faster than AVIF, but its files are 14–18% larger. To match the file size of AVIF's default, jixel needs its 'extraslow' setting, which takes 18–27x as long as AVIF. libjxl's default takes 1.5–2x as long as AVIF's default, and still produces bigger files.
- **Encode memory: AVIF wins.** At default efforts, jixel uses around half the memory of AVIF, but again, its files are 14–18% larger. To match the file size of AVIF's default, jixel 'extraslow' uses ~10x the memory. libjxl's default uses 2–3x the memory of AVIF, and its higher efforts use ~7x.
- **Decode time: AVIF wins.** JPEG XL takes 2–3x as long to decode at medium quality, narrowing to ~1.5–1.8x at very high quality. Safari's JPEG XL decoder is the slowest.
- **Higher effort is worth it for AVIF.** AVIF's s5 gets you ~10% smaller files for ~3x the encode time.
- **Higher effort is worth it for jixel too, up to 'slow'.** 'slow' gets you 9–13% smaller files than its default 'fast', for ~3.7x the encode time. 'extraslow' saves another 3–8%, but takes ~8x longer than 'slow' and uses ~9x the memory. Even so, AVIF still wins when given the same encode time: AVIF s5 takes about as long as jixel 'slow', and its files are ~13% smaller.
- **Higher effort isn't worth it for libjxl.** libjxl's maximum effort is barely any better than its default, but takes ~18x longer.
- **AVIF's 4:2:0 mode doesn't help much.** It's slightly faster to encode/decode, and uses less memory, but the files aren't meaningfully smaller, and it struggles to reach higher qualities.

Here's the full data, so you can make up your own mind:

Show me the graphs!

### [Image quality](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#image-quality)

Ok, let's get to our first graph, the SSIMULACRA2 scores:

File size

SSIMULACRA2 (higher is closer to the original)

There's going to be a lot of these graphs, so here's what's going on:

- **The x-axis is visual quality** according to SSIMULACRA2.
- **The y-axis is file size**. Lower is better, for a given visual quality.
- **The orange lines represent JPEG XL encoded with libjxl.**
- **The green lines represent JPEG XL encoded with jixel.**
- **The blue lines represent AVIF.**
- **The dashed blue lines represent AVIF with its color data at half resolution.** Known as 4:2:0, or chroma subsampling.
- **The darker lines represent higher effort levels for that codec.** libjxl calls this 'effort', so the e10 line is the top effort. Annoyingly, avifenc calls this 'speed', so the s0 line is the top effort. jixel names its levels, from 'fast' (its default) to 'extraslow'.
- **Only the default and maximum efforts are shown initially.** Click the legend to toggle the others.

Here are the quality results using a different quality metric, [ColorVideoVDP](https://github.com/gfxdisp/colorvideovdp):

File size

ColorVideoVDP, JOD (10 = no visible difference)

The result is roughly the same except this metric puts more of a gap between AVIF and the JPEG XL images.

It also suggests there's some advantage to AVIF with 4:2:0 chroma subsampling at lower qualities, but it's minor.

### [Encoding performance](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#encoding-performance)

When it comes to images on my blog, I encode with maximum effort settings. I'm happy to wait a couple of seconds to get an image that's faster to load for every user. However, if you're doing a lot of encoding on a server, encode time becomes more of a concern.

Encode time

SSIMULACRA2 (higher is closer to the original)

### [Encoding memory usage](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#encoding-memory-usage)

Another thing that might be a concern if you're encoding images on a server is peak memory usage:

Peak encoder memory

SSIMULACRA2 (higher is closer to the original)

### [Decoding performance](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#decoding-performance)

It's worth considering decoding performance. If decoding is particularly slow, then it might make the file size benefit less valuable, especially when images are loaded from a cache.

Decode time

SSIMULACRA2 (higher is closer to the original)

JPEG XL becomes suddenly faster to decode at very high quality settings (but still slower than AVIF). I think this is due to an edge preserving filter that does more work at lower quality settings.

## [Lossy - people](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#lossy---people)

I've heard people say that AVIF's tendency to smooth out details makes it bad for human faces, so I picked this image [from Unsplash](https://unsplash.com/photos/two-smiling-women-looking-at-a-smartphone-together-_t15AWPJNmg).

The results of the benchmark for this image:

- **File size: AVIF wins.** At default efforts, the best JPEG XL image is 17–24% larger than the AVIF image for the same quality. jixel is the better JPEG XL encoder across the whole quality range this time. libjxl is 30–50% larger than AVIF, which is a much bigger gap than the F1 image. At maximum efforts, the best JPEG XL image (jixel 'extraslow') is 12–17% larger than AVIF (s0).
- **jixel at its slowest setting doesn't quite match AVIF's default effort** this time. It's a few percent larger at medium/high quality, and only edges ahead at very high quality.
- **Encode time: AVIF wins.** At default efforts, jixel is 18–33% faster than AVIF, but its files are 17–24% larger. Even jixel's 'extraslow' setting, which takes 25–39x as long as AVIF, only matches AVIF's default file size at very high quality. libjxl's default takes 1.5–2.2x as long as AVIF's default, and still produces bigger files.
- **Encode memory: AVIF wins.** At default efforts, jixel uses around half the memory of AVIF, but again, its files are 17–24% larger. jixel 'extraslow' gets closest to AVIF's default file size, but uses ~10x the memory. libjxl's default uses 2–3.3x the memory of AVIF, and its higher efforts use ~7x.
- **Decode time: AVIF wins.** JPEG XL takes 2–2.9x as long to decode at medium quality, narrowing to ~1.3–1.7x at very high quality. At the very top of the quality range, libjxl images decode as fast as AVIF in Chrome, but not in the other browsers.
- **Higher effort is worth it for AVIF.** AVIF's s5 gets you 5–8% smaller files for ~3x the encode time. That's a smaller gain than the F1 image.
- **Higher effort is worth it for jixel too, up to 'slow'.** 'slow' gets you 7–14% smaller files than its default 'fast', for ~3.9x the encode time. 'extraslow' saves another 4–6%, but takes ~10x longer than 'slow' and uses ~9x the memory. AVIF still wins when given the same encode time: AVIF s5 takes about as long as jixel 'slow', and its files are 8–17% smaller.
- **Higher effort is somewhat worth it for libjxl on this image.** Unlike the F1 image, libjxl's maximum effort gets you 4–11% smaller files than its default. But it takes ~10x longer, and it still produces much bigger files than AVIF at its default effort.
- **AVIF's 4:2:0 mode doesn't help much.** Same as the F1 image. ColorVideoVDP gives it a small (~4%) advantage, but SSIMULACRA2 doesn't.

At lower qualities, you can see the smoothing on the face, but it's the same for both AVIF and JPEG XL.

Show me the graphs!

### [Image quality](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#image-quality-1)

File size

SSIMULACRA2 (higher is closer to the original)

File size

ColorVideoVDP, JOD (10 = no visible difference)

### [Encoding performance](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#encoding-performance-1)

Encode time

SSIMULACRA2 (higher is closer to the original)

### [Encoding memory usage](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#encoding-memory-usage-1)

Peak encoder memory

SSIMULACRA2 (higher is closer to the original)

### [Decoding performance](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#decoding-performance-1)

Decode time

SSIMULACRA2 (higher is closer to the original)

It's interesting that, at lower qualities, jixel decodes much slower in Safari.

## [Lossy - red panda](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#lossy---red-panda)

I threw this one in because it's the default image in [Squoosh](https://squoosh.app/), but I guess the sharp lines of the hairs make it different to the previous images. In fact, even at medium quality, the difference from the original is much less perceptible to my eyes compared to the other images.

The results are much closer than the previous two:

- **File size: AVIF wins, narrowly.** At default efforts, the best JPEG XL image (jixel) is only 2–6% larger than the AVIF image for the same quality. libjxl does much worse, at 10–18% larger. At maximum efforts, jixel 'extraslow' and AVIF s0 are roughly tied, with jixel up to 5% smaller at very high quality. However, ColorVideoVDP disagrees, and puts AVIF 5–12% ahead at maximum efforts.
- **Encode time: JPEG XL (jixel) wins, according to SSIMULACRA2.** At default efforts, jixel is 19–35% faster than AVIF, and its files are only 2–6% larger. Comparing higher efforts, jixel 'slow' produces files the same size or slightly smaller than AVIF s5, in the same time or less. And jixel 'extraslow' matches or beats AVIF s0 while being 12–34% faster. But, since ColorVideoVDP gives AVIF more of a file size advantage, it isn't clear-cut. libjxl's default takes 1.3–1.9x as long as AVIF's default, and produces bigger files.
- **Encode memory: JPEG XL (jixel) wins, at lower efforts.** At default efforts, jixel uses 37–48% less memory than AVIF, and its files are only 2–6% larger. jixel 'slow' uses about the same memory as AVIF s5 to produce a similar file size, but 'extraslow' uses ~9x the memory of AVIF s0. libjxl's default uses ~2x the memory of AVIF, and its higher efforts use 6–7x.
- **Decode time: AVIF wins.** JPEG XL takes 2–2.4x as long to decode at medium quality. But the gap closes at very high quality, where jixel's files decode about as fast as AVIF in Firefox and Safari, and ~25% faster in Chrome.
- **Higher effort is less worth it for AVIF on this image.** AVIF's s5 gets you 3–6% smaller files for 3–4x the encode time.
- **Higher effort is worth it for jixel, up to 'slow'.** 'slow' gets you 6–14% smaller files than its default 'fast', for 3.5–4.3x the encode time. 'extraslow' saves another 2–9%, but takes 9–14x longer than 'slow' and uses ~9x the memory.
- **Higher effort isn't worth it for libjxl.** libjxl's maximum effort gets you 1–2% smaller files than its default, but takes ~12x longer.
- **AVIF's 4:2:0 mode doesn't help.** The files are the same size as 4:4:4.

Show me the graphs!

### [Image quality](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#image-quality-2)

File size

SSIMULACRA2 (higher is closer to the original)

File size

ColorVideoVDP, JOD (10 = no visible difference)

### [Encoding performance](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#encoding-performance-2)

Encode time

SSIMULACRA2 (higher is closer to the original)

### [Encoding memory usage](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#encoding-memory-usage-2)

Peak encoder memory

SSIMULACRA2 (higher is closer to the original)

### [Decoding performance](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#decoding-performance-2)

Decode time

SSIMULACRA2 (higher is closer to the original)

## [Lossy - weasel](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#lossy---weasel)

I've played with both codecs a lot, and for photos, what I've shown so far is representative, where AVIF beats JPEG XL. However, every now and then there's an outlier, such as this image.

- **File size: JPEG XL wins.** At default efforts, the best JPEG XL image is 4–11% smaller than the AVIF image for the same quality. libjxl is the better JPEG XL encoder at medium/high quality, and jixel at very high quality. At maximum efforts, jixel 'extraslow' is 12–27% smaller than AVIF s0. This time, ColorVideoVDP agrees.
- **jixel's 'medium' setting is a big step up on this image.** Its files are 11–25% smaller than AVIF's default, and 4–18% smaller than AVIF s0, the maximum effort.
- **Encode time: JPEG XL (jixel) wins.** At default efforts, jixel is 28–38% faster than AVIF, and produces files that are around the same size or smaller. At higher efforts, it's even more one-sided: jixel 'medium' is 37–65% faster than AVIF s5, and produces files 9–23% smaller. libjxl's default takes 1.5–2.2x as long as AVIF's default.
- **Encode memory: JPEG XL (jixel) wins.** At default efforts, jixel uses 14–24% less memory than AVIF, and produces files that are around the same size or smaller. AVIF s0 uses even more memory (~1.55x its default), and still produces bigger files than jixel 'medium', which uses 41–55% less memory than AVIF s0. libjxl's default uses 2.4–3.6x the memory of AVIF, and the maximum efforts of both JPEG XL encoders use 10–18x.
- **Decode time: AVIF wins, except in Chrome at higher qualities.** JPEG XL takes 1.3–1.9x as long to decode in Firefox and Safari. In Chrome, JPEG XL takes ~1.25x as long at medium quality, but from high quality upwards, it's 11–36% faster than AVIF.
- **Unlike the previous images, jixel images aren't particularly slow to decode in Safari.** In fact, they decode slightly faster than libjxl images.
- **Higher effort isn't really worth it for AVIF on this image.** AVIF's s5 only gets you 1–3% smaller files for 3–4.5x the encode time. s0 gets you 7–8% smaller files, but takes ~50x as long.
- **Higher effort is worth it for jixel, up to 'slow'.** 'medium' gets you 6–14% smaller files than its default 'fast', for 1.9–4.1x the encode time. 'slow' gets you 9–19% smaller files than 'fast', for 3.9–6.5x the encode time. 'extraslow' saves another 5–9%, but takes 8–18x longer than 'slow' and uses ~11x the memory.
- **Higher effort isn't worth it for libjxl.** libjxl's maximum effort gets you 1–6% smaller files than its default, but takes 11–14x longer.
- **AVIF's 4:2:0 mode doesn't help.** The files are the same size as 4:4:4.
- **All three encoders produce lower SSIMULACRA2 scores for this image.** As in, with the same encoder quality value, they score lower. It seems like the encoders are pretty happy smoothing out some of the background noise, which seems fine.

Show me the graphs!

### [Image quality](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#image-quality-3)

File size

SSIMULACRA2 (higher is closer to the original)

File size

ColorVideoVDP, JOD (10 = no visible difference)

### [Encoding performance](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#encoding-performance-3)

Encode time

SSIMULACRA2 (higher is closer to the original)

### [Encoding memory usage](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#encoding-memory-usage-3)

Peak encoder memory

SSIMULACRA2 (higher is closer to the original)

### [Decoding performance](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#decoding-performance-3)

Decode time

SSIMULACRA2 (higher is closer to the original)

## [Lossy - screenshot](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#lossy---screenshot)

Ok, let's try something other than a photo:

The results of the benchmark:

- **File size: AVIF wins, by a lot.** At default efforts, the best JPEG XL image is 73–89% larger than the AVIF image at medium/high quality (jixel), and 29–44% larger at very high quality (libjxl). At maximum efforts, the best JPEG XL image (jixel 'extraslow' with patches) is 23–73% larger than AVIF (s0).
- **libjxl can't produce anything between SSIMULACRA2 ~45 and ~78** at its default effort (~45 and ~75 at higher efforts). jixel doesn't have this problem.
- **jixel's patches feature helps a lot with this image**, but it's off by default. With patches, jixel's files are 10–29% smaller at its higher efforts, from SSIMULACRA2 ~65 upwards. Patches have no effect at jixel's default 'fast' setting.
- **Encode time: AVIF wins.** At default efforts, jixel is ~50% faster than AVIF, but its files are 70–98% larger. libjxl's default takes 1.7–1.8x as long as AVIF's default, and still produces much bigger files.
- **Encode memory: AVIF wins.** At default efforts, jixel uses ~40% less memory than AVIF, but again, its files are much larger. libjxl's default uses 1.2–2.2x the memory of AVIF. jixel 'extraslow' with patches gets closest to AVIF's file sizes, but uses ~7.5x the memory of AVIF's default.
- **Decode time: AVIF wins, by a lot.** Taking the faster of the two JPEG XL encoders' output, JPEG XL takes 2–3.8x as long to decode, depending on browser and quality.
- **Higher effort is more worth it for AVIF on this image.** AVIF's s5 gets you 6–13% smaller files for 2.5–3x the encode time, and s0 gets you 16–23% smaller files for 30–45x the encode time.
- **Higher effort is worth it for jixel, as long as you enable patches.** 'medium' with patches gets you 11–44% smaller files than its default 'fast', for 3–4x the encode time. 'slow' with patches saves another 7–16% for ~1.7x the time of 'medium'. 'extraslow' with patches saves another few percent, but takes ~5x longer than 'slow' and uses ~4.5x the memory.
- **Higher effort is harmful for libjxl.** At very high quality, libjxl's maximum effort produces files 33–40% *larger* than its default effort, and takes ~7x longer.
- **AVIF's 4:2:0 mode helps at higher qualities.** At SSIMULACRA2 70 and above, it produces files 7–18% smaller than 4:4:4. At lower qualities, it makes no difference.
- **All three encoders produce higher SSIMULACRA2 scores for this image.** Opposite to the weasel image. But, this seems correct. At SSIMULACRA2 70 the artifacts are pretty noticeable around the Firefox logo (to me, anyway), so I'd go with 80, which is where the codecs lean anyway.

Some developers, especially those who've been around as long as I have, seem to think things like screenshots need to be delivered in a lossless format. But I think this is because they're used to formats like JPEG and WebP (and seemingly JPEG XL) which handle this kind of image badly, whereas lossy AVIF handles them really well.

Show me the graphs!

### [Image quality](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#image-quality-4)

File size

SSIMULACRA2 (higher is closer to the original)

I'm not including the ColorVideoVDP chart because the metric doesn't support alpha channels.

### [Encoding performance](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#encoding-performance-4)

Encode time

SSIMULACRA2 (higher is closer to the original)

### [Encoding memory usage](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#encoding-memory-usage-4)

Peak encoder memory

SSIMULACRA2 (higher is closer to the original)

### [Decoding performance](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#decoding-performance-4)

Decode time

SSIMULACRA2 (higher is closer to the original)

The high JPEG XL decoding times here are likely a side effect of it producing much larger files for a given quality.

## [Lossy - illustration](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#lossy---illustration)

In many cases, for largely flat-colour images, you'd use SVG. But sometimes that isn't possible (it's a screenshot), or undesirable, such as this case. This image, [also from Unsplash](https://unsplash.com/illustrations/hands-feeding-orange-cat-a-treat-on-bed-9tXfoalzAqY), is still 1.1 MB as a compressed and minified SVG. This is due to the complexity of the shaded areas. As a result, a raster format wins on file size and decode/draw time.

At quality 50 in the demo above, the jixel has some pretty severe colour blocks, e.g. on the cat's face. Again, it makes me question the metric a bit.

The results of the benchmark:

- **File size: AVIF wins, by a lot.** At default efforts, the best JPEG XL image (jixel) is 32–65% larger than the AVIF image up to SSIMULACRA2 80, and ~90% larger beyond that. At maximum efforts, the best JPEG XL image (jixel 'extraslow') is 22–55% larger than AVIF (s0) up to SSIMULACRA2 80, and more beyond that. ColorVideoVDP agrees, and puts the gap at 27–58%.
- **libjxl performs particularly badly with this image.** At default efforts, it's 71–106% larger than AVIF.
- **Lossy AVIF can't reach SSIMULACRA2 90 with this image.** Even at its maximum quality setting, it tops out at ~87, so the 90 example above isn't a totally fair comparison. JPEG XL can reach 90, but the result is 8–50% larger than JPEG XL's lossless version, so you may as well use lossless.
- **Encode time: AVIF wins.** At default efforts, jixel is 35–44% faster than AVIF, but its files are 32–65% larger, which is a bad trade. jixel needs its 'slow' setting to get within 7–27% of AVIF's default file size, which takes 2.4–2.6x as long as AVIF. libjxl's default takes 1.3–2.3x as long as AVIF's default, and produces much bigger files.
- **Encode memory: AVIF wins.** At default efforts, jixel uses ~37% less memory than AVIF, but again, its files are much larger. jixel 'slow' uses about the same memory as AVIF, and 'extraslow' uses ~12x. libjxl's default uses 1.5–3x the memory of AVIF.
- **Decode time: AVIF wins, by a lot.** JPEG XL takes 2–4.6x as long to decode at medium/high quality, narrowing to 1.6–2.1x at very high quality.
- **Higher effort is more worth it for AVIF on this image.** AVIF's s5 gets you 5–8% smaller files for ~2.7x the encode time, and s0 gets you 14–21% smaller files for 35–48x the encode time.
- **Higher effort is worth it for jixel, up to 'slow'.** 'medium' only gets you 4–13% smaller files than its default 'fast', but 'slow' gets you 19–24% smaller files (up to 36% at very high quality), for ~4x the encode time. 'extraslow' saves another 2–3%, but takes 8–9x longer than 'slow' and uses ~11x the memory.
- **jixel's patches feature doesn't help with this image.** Unlike the screenshot, the file sizes are identical with and without it, but it does increase encode time and memory.
- **Higher effort isn't worth it for libjxl.** libjxl's maximum effort gets you 3–9% smaller files than its default, but takes 10–13x longer.
- **AVIF's 4:2:0 mode doesn't help.** SSIMULACRA2 rates it slightly worse than 4:4:4, and ColorVideoVDP rates it slightly better (~3–4%).

Show me the graphs!

### [Image quality](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#image-quality-5)

File size

SSIMULACRA2 (higher is closer to the original)

File size

ColorVideoVDP, JOD (10 = no visible difference)

### [Encoding performance](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#encoding-performance-5)

Encode time

SSIMULACRA2 (higher is closer to the original)

### [Encoding memory usage](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#encoding-memory-usage-5)

Peak encoder memory

SSIMULACRA2 (higher is closer to the original)

### [Decoding performance](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#decoding-performance-5)

Decode time

SSIMULACRA2 (higher is closer to the original)

## [Lossy - alpha](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#lossy---alpha)

So far, all the images have been mostly opaque. What about an image with transparency?

There are a few things that make this one different:

- **jixel always encodes the alpha channel losslessly.** It doesn't have a setting for alpha quality. This puts jixel at a big disadvantage with this image. AVIF by default compresses the alpha channel lossily, tracking the main quality setting. libjxl compresses the alpha channel losslessly by default, so watch out for that. For this benchmark, I set libjxl's alpha quality to the same as the main quality.
- **SSIMULACRA2 scores fully-transparent pixels as perfect**, so with lots of transparency, the scores will be higher than they would be for an opaque image.

The results of the benchmark:

- **File size: AVIF wins, by a lot.** At default efforts, the best JPEG XL image (libjxl) is 31–44% larger than the AVIF image for the same quality. At maximum efforts, it's 61–70% larger. Due to its lossless alpha, jixel is 1.9–3.1x the size of AVIF at default efforts.
- **Encode time: AVIF wins.** libjxl's default takes 1.2–1.35x as long as AVIF's default, and produces much bigger files. jixel's default is ~2x faster than AVIF, but its files are around 2–3x the size.
- **Encode memory: AVIF wins.** libjxl's default uses 2.4–2.7x the memory of AVIF's default, although in absolute terms, that's ~120 MB vs ~46 MB. jixel's default uses about the same memory as AVIF, but again, its files are much bigger.
- **Decode time: AVIF wins.** libjxl's output takes 2–3x as long to decode at medium quality, narrowing to 1.4–1.8x at very high quality. jixel's output decodes faster than libjxl's, and at very high quality it's around the same speed as AVIF. Although, it's slow in Safari at lower qualities, taking ~2.4x as long as AVIF.
- **Higher effort is worth it for AVIF.** AVIF's s5 gets you 4–7% smaller files for 2.3–2.9x the encode time, and s0 gets you 10–15% smaller files for 24–29x the encode time.
- **Higher effort barely helps jixel on this image.** 'extraslow' only gets you 5–8% smaller files than its default 'fast', for 21–26x the encode time.
- **Higher effort is harmful for libjxl.** libjxl's maximum effort produces files up to 8% *larger* than its default effort, and takes ~6.5x longer.
- **AVIF's 4:2:0 mode is harmful.** It produces files 4–27% larger than 4:4:4, and can't get beyond SSIMULACRA2 ~79.

Show me the graphs!

### [Image quality](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#image-quality-6)

File size

SSIMULACRA2 (higher is closer to the original)

### [Encoding performance](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#encoding-performance-6)

Encode time

SSIMULACRA2 (higher is closer to the original)

### [Encoding memory usage](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#encoding-memory-usage-6)

Peak encoder memory

SSIMULACRA2 (higher is closer to the original)

### [Decoding performance](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#decoding-performance-6)

Decode time

SSIMULACRA2 (higher is closer to the original)

## [Lossy - small](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#lossy---small)

The images so far have been a similar resolution. Are the results different with smaller images?

Not really. The results are pretty similar to the full-size F1 image:

- **File size: AVIF wins**, by about the same margin. At default efforts, the best JPEG XL image is 13–18% larger. At maximum efforts, the gap is slightly bigger than the full-size image, at 12–20%.
- **Encode time & memory: AVIF wins**, although libjxl's default is now around the same speed as AVIF's default. jixel's default is ~2x faster than AVIF, but its files are 13–18% larger, and this time it uses ~30% more memory. In any case, at default efforts, everything encodes in under 50ms, using under 40 MB, so these differences don't really matter at this size.
- **Decode time: AVIF wins**, by a similar margin, but again, everything decodes in a few milliseconds.

Show me the graphs!

### [Image quality](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#image-quality-7)

File size

SSIMULACRA2 (higher is closer to the original)

File size

ColorVideoVDP, JOD (10 = no visible difference)

### [Encoding performance](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#encoding-performance-7)

Encode time

SSIMULACRA2 (higher is closer to the original)

### [Encoding memory usage](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#encoding-memory-usage-7)

Peak encoder memory

SSIMULACRA2 (higher is closer to the original)

### [Decoding performance](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#decoding-performance-7)

Decode time

SSIMULACRA2 (higher is closer to the original)

The numbers Safari gives are rounded, so the data is really noisy.

## [Lossy - large](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#lossy---large)

What about an 18 megapixel image?

Mostly the same story as the regular F1 image, but with a few differences:

- **File size: AVIF wins at medium/high quality, by more than the regular image.** At default efforts, the best JPEG XL image is 18–46% larger, and at maximum efforts, 28–54% larger.
- **But JPEG XL catches up at very high quality.** Around SSIMULACRA2 85, libjxl's default matches AVIF's default, and at maximum efforts, jixel 'extraslow' is ~6% smaller than AVIF s0. jixel 'slow' is 10–13% smaller than AVIF's default from SSIMULACRA2 80 upwards.
- **Encode time: AVIF wins at medium/high quality, but JPEG XL (jixel) wins at very high quality.** AVIF's default takes ~0.5 seconds, whereas libjxl's default takes 1–2 seconds. jixel's default is ~2x faster than AVIF, but at medium/high quality its files are 29–57% larger. At very high quality, that drops to 6–12% larger, which seems like a reasonable trade-off for the speed. And jixel 'slow' produces files around the same size as AVIF s5, while being 33–41% faster.
- **Encode memory: AVIF wins, and the costs are now significant.** AVIF's default uses ~560 MB. libjxl's default uses ~2.5 GB at medium/high quality. jixel's default uses ~35% less than AVIF, and 'slow' uses ~18% less than AVIF s5. At maximum efforts, libjxl uses 5–6.4 GB, and jixel 'extraslow' uses 6.8–8.3 GB.
- **Decode time: AVIF wins** at medium/high quality, by a similar margin to the regular image. jixel images take over 5x as long as AVIF to decode in Safari at those qualities. At very high quality, JPEG XL decodes faster than AVIF in Chrome.

Show me the graphs!

### [Image quality](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#image-quality-8)

File size

SSIMULACRA2 (higher is closer to the original)

File size

ColorVideoVDP, JOD (10 = no visible difference)

### [Encoding performance](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#encoding-performance-8)

Encode time

SSIMULACRA2 (higher is closer to the original)

### [Encoding memory usage](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#encoding-memory-usage-8)

Peak encoder memory

SSIMULACRA2 (higher is closer to the original)

### [Decoding performance](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#decoding-performance-8)

Decode time

SSIMULACRA2 (higher is closer to the original)

## [Quality stability](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#quality-stability)

Another criticism I've heard levelled at AVIF is that its quality is unreliable. As in, a single quality setting is great for some images, but no good for others, meaning for automatically encoded images, you need to pick a higher quality setting to be safe, resulting in larger file sizes than necessary for some images.

For the images I've looked at so far, I encoded them with `avifenc -q 50`, `cjxl -q 70`, and jixel at quality 80, and recorded the resulting SSIMULACRA2 scores:

| Image | AVIF | JPEG XL | JPEG XL (jixel) |      |      |      |      |
| ----- | ---- | ------- | --------------- | ---- | ---- | ---- | ---- |
| 74.6  | 75.9 | 73.4    | 73.8            | 79.2 | 78.0 | 75.3 | 77.3 |
| 70.9  | 73.5 | 68.4    | 69.5            | 75.6 | 73.9 | 70.6 | 72.6 |
| 77.0  | 77.4 | 76.2    | 76.6            | 80.1 | 79.2 | 77.5 | 78.9 |
| 70.7  | 72.0 | 69.8    | 70.5            | 69.6 | 70.3 | 69.1 | 72.3 |
| 67.5  | 68.8 | 67.0    | 67.2            | 69.5 | 68.4 | 69.2 | 72.3 |
| 56.7  | 57.2 | 56.0    | 56.3            | 56.6 | 55.4 | 54.9 | 59.4 |
| 80.2  | 80.4 | 82.3    | 81.9            | 86.5 | 87.6 | 76.6 | 80.3 |
| 78.6  | 79.7 | 76.8    | 77.9            | 80.3 | 78.5 | 75.4 | 77.7 |
| 67.3  | 69.8 | 63.6    | 64.9            | 69.4 | 66.9 | 66.8 | 69.6 |

The behaviour of the codecs here seems really similar. All produce a range of SSIMULACRA2 scores, and they all go higher or lower for the same images.

And I think this is ok? Like, all three go for a lower quality for the Weasel picture, which removed noise without reducing quality of the image subject. With the screenshot, all go for a higher quality, which seems justified to my eyes.

Obviously this is a small sample size, but I haven't seen anything that suggests using a single quality setting is a risk, for any of the three codecs.

Also see [Halide Compression's deeper dive into this](https://halide.cx/blog/consistency/).

## [Progressive](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#progressive)

Both AVIF and JPEG XL are capable of showing something progressively as the image loads. Chrome and Firefox support progressive JPEG XL, whereas only Chrome supports progressive AVIF. So, and I say this through gritted teeth, this next demo is best viewed in Chrome (I have [a Firefox patch in progress](https://bugzilla.mozilla.org/show_bug.cgi?id=2072679)):

Downloaded: 11.5 kB

- AVIF: 11.5 kB of 151.3 kB
- JPEG XL: 11.5 kB of 166.0 kB

This is the 18 megapixel image again, so if you're viewing this on a device with a smaller screen, you're not getting the full effect.

Progressive AVIF and JPEG XL work very differently. With JPEG XL, you can opt into the data being stored in such a way that a decoder can render low resolution versions, before it brings in the higher resolution tiles. jixel recently (like, yesterday at time of writing) added support for this too. It's called "progressive DC", and it's the most important kind of progressive rendering for JPEG XL.

With AVIF, progressions work more like frames of a video. You provide up to four frames, so that's three phases before the final image. Again, like a video, each frame can use data from the previous frame as an optimisation. An advantage of this is you can choose exactly how those frames look. In the example above, I went with a single preview frame: a 1/8th resolution version, with a bit of blurring. But, your preview frames can be completely different images if you really want.

There's a lack of data when it comes to progressive images and user experience, so these are just my reckons:

If you're on a bad connection, data doesn't arrive gradually like some throttling tools will have you believe. It tends to come in bursts. So, there's little benefit to progressive rendering on smaller images, and I'm not sure that many progressive phases is particularly useful.

I think that it's nice to have a *representative* preview for images that may take some time to load. I've experienced this personally, where a site or app has given me something clearly not-loaded, but with enough detail to know what's coming. That's useful to me, as I can decide things like, if it's worth waiting for the whole thing. This is also why I think things like BlurHash are cute, but useless. They're not a *representative* preview, so there's little a user can do with the information provided.

The preview frame I chose for the AVIF is enough to tell it's an open-wheel race car. If you know F1, you'll know what team it is. You might even know where the photo was taken. And it delivers that in ~11.5 kB.

The JPEG XL gets a BlurHash-like preview on screen in ~2 kB, but it takes until ~63 kB to match the preview quality of the AVIF. After that, it continues to improve gradually, providing a more detailed and higher quality image as more data arrives.

However, I'm not convinced this is a good user experience, as it presents the user with something they may mistake for fully-loaded, but poor quality. For example:

Here's a crop of the JPEG XL, 80% loaded. It seems reasonable that someone would look at that, and just assume I took the picture badly, leaving parts out of focus. You could probably convince me that blurring the ApeCoin logo is a societal benefit, but it still feels like a suboptimal user experience.

AVIF's system gives me more control, but it's also more complex.

### [Progressive overhead](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#progressive-overhead)

I encoded the large F1 image with and without progressive rendering enabled, adjusting the quality setting to keep the SSIMULACRA2 score around 70:

- **With libjxl, progressive is basically free.** In fact, it produced files 6–10% smaller. At default effort, it was also faster to encode, and used less memory. Decoding was ~10% faster in Firefox, but 12–20% slower in Chrome and Safari.
- **With jixel, it depends on the effort.** For jixel, I used progressive DC, with the centre of the image loading first. At jixel's default ('fast'), progressive made the file 12% bigger. At 'extraslow', it made the file 10% smaller, and 11% faster to encode, although it used 17% more memory. Decoding was barely affected, other than being 7–9% slower in Chrome.
- **But ColorVideoVDP is less kind to progressive jixel.** It rates jixel's progressive files slightly lower than the non-progressive versions, whereas it rates libjxl's progressive files slightly higher. Taking that into account, progressive jixel at 'extraslow' is only ~2% smaller than non-progressive, and at 'fast' it's around 18% bigger.
- **Regular progressive AVIF has a bigger cost when it comes to encoding.** It uses around twice the memory, and at default effort (s6), it takes twice as long to encode. At s0, encode time is only 9% longer, but the file is 14% bigger. And this is pure encode time - it doesn't factor in how you might generate the preview frame, e.g. adding some blur.
- **Regular progressive AVIF is 29–44% slower to decode in Chrome**, although the difference is smaller in other browsers (6–15%). Since Chrome is the only browser that supports rendering the progressive frames, that might explain it. Although Firefox and Safari are still having to decode the progressive frame, as the main image builds upon it.
- **With AVIF, making the final frame a keyframe fixes the decoding overhead**, at the cost of a longer encode time. At maximum effort, it results in a smaller file too, only 3–5% bigger than the non-progressive version. You could call this cheating, as there's now no 'progression' in terms of data - it's just two images in one. But, if it's a better user experience, who cares?
- **Even with the overhead, progressive AVIF wins on decoding time, and usually on file size.** The progressive AVIFs are 152–181 kB. jixel's 'extraslow' progressive JPEG XL is in that range at 166 kB, but jixel's default produces 252 kB, and libjxl's progressive files are 233–242 kB. The AVIFs decode 1.6–6x faster, with the biggest gap in Safari, which is particularly slow to decode jixel's files.

Show me the data!

| Encoding | Quality | File size | SSIMU2  | Encode  | Decode |        |        |
| -------- | ------- | --------- | ------- | ------- | ------ | ------ | ------ |
| 41       | 175 kB  | 69.9      | 405 ms  | 381 MB  | 46 ms  | 41 ms  | 53 ms  |
| 42       | 173 kB  | 70.2      | 815 ms  | 764 MB  | 49 ms  | 53 ms  | 56 ms  |
| +1       | −1%     | +0.3      | +101%   | +101%   | +7%    | +29%   | +6%    |
| 42       | 181 kB  | 70.3      | 1.09 s  | 740 MB  | 45 ms  | 35 ms  | 49 ms  |
| +1       | +3%     | +0.4      | +169%   | +94%    | −2%    | −15%   | −8%    |
| 41       | 145 kB  | 69.9      | 8.00 s  | 450 MB  | 47 ms  | 41 ms  | 48 ms  |
| 42       | 165 kB  | 70.5      | 8.71 s  | 847 MB  | 54 ms  | 59 ms  | 52 ms  |
| +1       | +14%    | +0.6      | +9%     | +88%    | +15%   | +44%   | +8%    |
| 41       | 152 kB  | 69.6      | 10.72 s | 798 MB  | 47 ms  | 40 ms  | 46 ms  |
| ±0       | +5%     | −0.3      | +34%    | +77%    | ±0%    | −2%    | −4%    |
| 35.5     | 258 kB  | 70.0      | 1.62 s  | 2.49 GB | 131 ms | 105 ms | 162 ms |
| 25.3     | 233 kB  | 70.0      | 1.40 s  | 1.90 GB | 119 ms | 121 ms | 183 ms |
| −10.2    | −10%    | ±0.0      | −14%    | −24%    | −9%    | +15%   | +13%   |
| 41.2     | 258 kB  | 70.0      | 22.74 s | 5.67 GB | 132 ms | 107 ms | 162 ms |
| 30.2     | 242 kB  | 70.0      | 25.54 s | 6.08 GB | 119 ms | 128 ms | 181 ms |
| −11.0    | −6%     | ±0.0      | +12%    | +7%     | −10%   | +20%   | +12%   |
| 66.1     | 225 kB  | 70.0      | 304 ms  | 361 MB  | 131 ms | 89 ms  | 281 ms |
| 60.1     | 252 kB  | 70.0      | 333 ms  | 393 MB  | 132 ms | 97 ms  | 290 ms |
| −6.0     | +12%    | ±0.0      | +10%    | +9%     | +1%    | +9%    | +3%    |
| 65.1     | 185 kB  | 70.0      | 16.75 s | 7.61 GB | 141 ms | 92 ms  | 294 ms |
| 55.4     | 166 kB  | 70.0      | 14.90 s | 8.94 GB | 140 ms | 98 ms  | 293 ms |
| −9.7     | −10%    | ±0.0      | −11%    | +17%    | −1%    | +7%    | ±0%    |

## [Lossy - HDR](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#lossy---hdr)

Let's get back to lossy compression, but this time HDR! There are a few ways to encode HDR images, and we're going to look at two.

**Perceptual Quantization** (PQ) tells each pixel what to be. Each value maps to an absolute brightness, up to 10,000 nits, with the curve shaped so the data is spent where our eyes are most sensitive. If the image asks for colours and brightnesses the display can't show (no consumer display can hit 10,000 nits), the browser/OS has to tone-map it down, and how well that's done varies between platforms.

**Gain maps** are different. The main image is a regular SDR image (usually), and alongside it is a 'gain map', which applies a brightness and colour change to each pixel to achieve the HDR version.

This has two benefits:

1. The same image will look exactly as you want on an SDR display - the gain map is simply ignored.
2. If the display supports HDR, but maybe not as much as the image needs (for example, the screen is at low or maximum brightness), the browser/OS can apply as much of the gain map as it can handle.

CSS and `<picture>` let us serve an SDR or PQ image to the user via [the `dynamic-range` media query](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/At-rules/@media/dynamic-range), but then you're relying on tone mapping in cases where the browser signals HDR support, but it doesn't support the full range.

Although the JPEG XL format supports gain maps, no browser handles them, and the encoding story is messy.

AVIF supports gain maps, and you encode them by providing two images: the SDR image and the HDR image. It uses those to calculate and encode the gain map.

**Your browser/screen doesn't support HDR**, so the PQ images will look bad.

At medium brightnesses, on an HDR screen, the gain map and PQ images look the same. At maximum brightnesses, on my Pixel 10 Pro and 2024 MacBook Pro, the PQ images get a bit blown out, and the gain map image looks better. At low brightness on my Pixel 10 Pro, again the gain map image looks better.

This is because, at extreme brightness levels, the device doesn't have the full HDR headroom to display the image, and gain maps can handle these cases better. So, although gain maps seem to add a lot to the file size, I think it's worth it.

Maybe there's a future where tone mapping gets much better, and the advantage of a gain map becomes less significant.

Regular SSIMULACRA2 only understands SDR, so for this image, the scores come from an experimental HDR mode in [fast-ssim2](https://github.com/imazen/fast-ssim2). That means these scores can't be compared to the SSIMULACRA2 scores elsewhere in this article. ColorVideoVDP handles HDR natively, and here it's modelling a 1,500 nit HDR monitor.

**The two metrics disagree a lot with this image**. And they don't factor in the benefits of gain maps, so treat these results as a comparison between the PQ variants:

- **File size: JPEG XL wins, but the metrics disagree on how much, and with which encoder.**
  - According to HDR SSIMULACRA2, jixel's default ranges from 8% smaller to 3% larger than PQ AVIF's default, and libjxl is 5–33% larger. At maximum efforts, jixel 'extraslow' is up to 15% smaller than PQ AVIF s0.
  - According to ColorVideoVDP, libjxl is the clear winner. At default efforts, it's 28–53% smaller than PQ AVIF, and 17–37% smaller than gain map AVIF. At maximum efforts, it's up to 23% smaller than the best AVIF, but jixel 'extraslow' is roughly level with it, ranging from 12% smaller to 14% larger.
- **The metrics also disagree about gain maps.** HDR SSIMULACRA2 says gain map AVIF is 31–38% larger than PQ AVIF, but ColorVideoVDP says gain map AVIF is 11–25% *smaller* than PQ AVIF.
- **Encode time: JPEG XL (jixel) wins.** At default efforts, jixel is ~2x faster than PQ AVIF, and produces files around the same size or smaller, depending on the metric. libjxl's default is 5–47% slower than PQ AVIF, but given ColorVideoVDP's view of the file sizes, that's a reasonable trade. Gain map AVIF is the slowest, as it's calculating a gain map, then essentially encoding two images: ~3x slower than PQ AVIF.
- **Encode memory: JPEG XL (jixel) wins.** At default efforts, jixel uses ~30% less memory than PQ AVIF. libjxl's default uses ~1.9x the memory of PQ AVIF, and gain map AVIF uses ~1.55x. At maximum efforts, libjxl uses ~6x the memory of PQ AVIF's default, and jixel 'extraslow' uses ~10x.
- **Decode time: AVIF wins.** PQ and gain map AVIF decode at around the same speed. JPEG XL takes 1.2–1.7x as long to decode in Chrome, although it's slightly faster at the top of the quality range. In Safari, it takes 1.6–3x as long. Firefox is particularly slow with these HDR JPEG XL images, taking ~4x as long as AVIF at all qualities. We're [working on it](https://bugzilla.mozilla.org/show_bug.cgi?id=2081138).
- **Higher effort is worth it for AVIF.** For gain map AVIF, s5 gets you 9–12% smaller files for ~1.9x the encode time, and s0 gets you 14–20% smaller files for ~11x the encode time. For PQ AVIF, s0 gets you 14–19% smaller files, for 17–20x the encode time.
- **Higher effort is worth it for jixel, up to 'slow'.** 'slow' gets you 7–32% smaller files than its default 'fast', for ~3.8x the encode time. 'extraslow' saves another 1–9%, but takes ~10x longer than 'slow' and uses ~8.5x the memory. jixel only has a few data points for this image, so these numbers are rough.
- **Higher effort isn't worth it for libjxl.** libjxl's maximum effort gets you 2–7% smaller files than its default, but takes ~15x longer.

It will be interesting to see how JPEG XL handles gain maps once they're fully supported.

By the way, these are signs that [indicate whether my cats are inside or outside](https://www.tiktok.com/@jaffathejake/video/7524298335885217046), and the little display indicates how long until their next automated feed.

Show me the graphs!

### [Image quality](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#image-quality-9)

File size

SSIMULACRA2 (higher is closer to the original)

File size

ColorVideoVDP, JOD (10 = no visible difference)

### [Encoding performance](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#encoding-performance-9)

Encode time

SSIMULACRA2 (higher is closer to the original)

### [Encoding memory usage](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#encoding-memory-usage-9)

Peak encoder memory

SSIMULACRA2 (higher is closer to the original)

### [Decoding performance](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#decoding-performance-9)

Decode time

SSIMULACRA2 (higher is closer to the original)

## [JPEG transcoding](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#jpeg-transcoding)

JPEG XL allows you to convert an existing JPEG to JPEG XL without additional loss. I tested this with the F1 image at a range of sizes and JPEG qualities:

- **The JPEG XL is 9–20% smaller than the JPEG.** The saving grows with resolution, from ~9% at 400 pixels wide to ~20% at 4,900 pixels wide. The saving is slightly smaller at higher JPEG qualities. libjxl and jixel produce practically identical file sizes.
- **For small images, JPEG XL is slower to decode than JPEG**, taking 1.4–2.7x as long, depending on the browser. But it's just a millisecond or two.
- **For larger images, JPEG XL is faster to decode in Chrome and Safari.** From ~1,000 pixels wide, it matches JPEG, and at the largest sizes, it decodes in around half the time. In Firefox, it ends up around the same speed as JPEG.
- **jixel's output is slightly slower to decode than libjxl's** in Chrome and Firefox, by up to ~25%. But again, these are small numbers, at most a few milliseconds.

Of course, if you have the original, you'd be way better off creating a proper new lossy version with AVIF or JPEG XL, but it's pretty cool that the JPEGs end up smaller, and sometimes faster to decode. Although, I suppose this is just pointing to a missing optimisation in browser JPEG decoders.

Show me the graphs!

File size

Megapixels

Decode time

Megapixels

## [Lossless](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#lossless)

It is very rare to need lossless images on the web. As we've already seen, even for things like screenshots and illustrations, lossy AVIF handles them really well. Ironically, this page is one of the outliers, because I wanted to show the compressed versions vs their lossless counterparts. Thankfully, JPEG XL is great at lossless!

The results:

- **File size: JPEG XL wins.** At default efforts, libjxl produces the smallest files for every image except the illustration, where WebP matches it. WebP is 9–17% larger for photos, and AVIF is 33–122% larger. At highest efforts, libjxl and jixel are within a few percent of each other, WebP is 5–44% larger, and AVIF is 29–176% larger.
- **Encode time: JPEG XL wins.** libjxl's default takes 0.1–0.5 seconds, which is faster than WebP's default, and similar to AVIF's default, while producing smaller files than both.
- **Encode memory: JPEG XL wins or WebP wins, depending on the image.** WebP's default uses around half the memory of libjxl's default (55–110 MB vs 110–250 MB), but libjxl's files are smaller. For photos, WebP can't match libjxl's default file size even at its maximum effort, which uses more memory than libjxl's default. For the screenshot, they use around the same memory, but libjxl's file is smaller. The illustration is the exception: WebP's maximum effort produces a file 20% smaller than libjxl's default, using the same memory.
- **Decode time: WebP wins or JPEG XL wins, depending on the image.** WebP decodes photos in 17–39 ms in all browsers, and the screenshot in ~5 ms. libjxl's output takes 1–2.2x as long in Chrome, 1.4–2.7x as long in Firefox, and 2.1–3.2x as long in Safari. But for photos, JPEG XL's smaller files more than make up for this on a first load, unless the connection is really fast. For the screenshot and illustration, WebP's decode advantage wins. Lossless AVIF is particularly slow to decode, taking up to 4x as long as libjxl's output in Chrome.
- **Lossless AVIF is bad.** It's significantly larger than the others, and slower to decode.
- **jixel's 'medium' setting is the sweet spot for file size.** It beats libjxl's default on every image (0.5–23% smaller), and is within 2–3% of the best result for photos, taking 0.4–0.7 seconds. But it uses 470–670 MB of memory, and its output is slower to decode than libjxl's, particularly in Safari.
- **jixel's 'slow' and 'extraslow' output is very slow to decode for photos.** ~200–290 ms, compared to 22–95 ms for libjxl's output. That's 2–13x slower, depending on the browser. It's also slow for the alpha image, at 80–123 ms compared to 14–27 ms for libjxl's output. This doesn't happen with the screenshot and illustration. This means, for my use-case on this page, I'd avoid it.
- **libjxl's maximum effort is very slow.** It produces files 2–7% smaller than its default for photos, and ~25% smaller for the screenshot and illustration, but takes 9–46 seconds, which is 50–180x as long as its default. jixel gets the same results in a couple of seconds.
- **jixel's default 'fast' setting isn't great for lossless.** It's the fastest encoder by far (40–80 ms), but its files are 16–32% larger than libjxl's default for photos, and ~80% larger for the screenshot and illustration.
- **jixel's patches feature helps a bit with the screenshot** (4–9% smaller), but makes no difference to the illustration.

Show me the data!

### [File size](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#file-size)

| Image   | Default effort | Highest effort |       |         |     |      |       |
| ------- | -------------- | -------------- | ----- | ------- | --- | ---- | ----- |
| 1.07 MB | +32%           | +16%           | +49%  | 1.01 MB | −1% | +20% | +47%  |
| 1.39 MB | +22%           | +17%           | +49%  | 1.34 MB | −1% | +19% | +44%  |
| 1.78 MB | +17%           | +9%            | +40%  | 1.74 MB | −1% | +11% | +31%  |
| 2.91 MB | +16%           | +13%           | +96%  | 2.79 MB | −2% | +17% | +29%  |
| 92 kB   | +81%           | +9%            | +122% | 68 kB   | +6% | +44% | +176% |
| 361 kB  | +78%           | −1%            | +100% | 273 kB  | −6% | +5%  | +139% |
| 522 kB  | +21%           | +12%           | +33%  | 487 kB  | −3% | +13% | +32%  |

### [Encoding performance](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#encoding-performance-10)

File size

Encode time

### [Encoding memory usage](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#encoding-memory-usage-10)

File size

Peak encoder memory

### [Decoding performance](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#decoding-performance-10)

File size

Decode time

## [Animation](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#animation)

Animated image formats are dumb and animated JPEG XL is no exception. Video formats are better at video. They're built for it. They're optimised for it. Not just in terms of encoding, but also in terms of playback.

I encoded a short clip, and the animated AVIF was 725 kB, and the JPEG XL was 2.17 MB for the same ColorVideoVDP score (9.5).

The JPEG XL is super noisy compared to the AVIF, because each frame is encoded individually, and arttifacts land in different places per frame.

AVIF is basically the keyframe format of AV1 video, with a bunch of still-image-specific optimisations thrown in. Animated AVIF however, is pretty much just a repackaging of AV1 video. You can convert an AV1 MP4 to AVIF and back losslessly - it's just a container change. That's why it performs so much better, because it's built for motion.

However, animated AVIF plays like shit in Chrome and Safari. It's fine in Firefox, so this is purely an implementation problem. But… `<video muted autoplay playsinline>` is right there.

The interesting detail of animated AVIF is that it supports transparency, whereas AV1 video doesn't. But really, that just highlights that [it's dumb that AV1 *didn't* support transparency](https://jakearchibald.com/2024/video-with-transparency/).

No, I'm not adding a demo. I don't want it on my server. Use video formats for video.

## [And that's it!](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#and-thats-it)

I already gave you my [conclusions](https://jakearchibald.com/2026/jpeg-xl-vs-avif/#conclusions) at the start, so I guess I'm ready to hit publish.

I'm a little nervous about this, because discourse around JPEG XL can get a little… weird. I see a lot of conspiracy theories about Google trying to 'silence' JPEG XL, even though one of the leads of the project worked at Google, as part of a team that were working on JPEG XL. That [same leader claimed](https://x.com/jyzg/status/1592811472073416704) that the post-COVID layoffs wouldn't have happened if companies simply used JPEG XL. I was recently [called a racist](https://github.com/web-platform-dx/developer-signals/issues/215#issuecomment-4930663101) for asking about JPEG XL use-cases. This kind of thing just doesn't happen with other technologies and standards I follow. So, urm, I'm kinda bracing for impact here.

My first version of this article was very negative about JPEG XL, but then I heard about jixel, and honestly, I'm really excited about it. There's serious competition between AVIF and JPEG XL now, and whoever wins, we'll all benefit from better image compression.
