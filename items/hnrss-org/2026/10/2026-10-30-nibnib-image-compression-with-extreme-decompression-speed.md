---
title: 'Nibnib: Image compression with extreme decompression speed'
link: https://github.com/raphlinus/nibnib
source: hnrss-org
published: 2026-10-30T04:17:06Z
updated: 2026-10-30T04:17:06Z
first_seen: 2026-11-02T06:43:52.954701233Z
authors:
- raphlinus
content: extracted
html: 2026-10-30-nibnib-image-compression-with-extreme-decompression-speed.html
remote_preview:
  url: https://opengraph.githubassets.com/b8eead71a47a237095fde3796b0762287625f4cb8deea209bce2ddb7f3dee2b3/raphlinus/nibnib
  alt: Image compression at 4bpp with extreme decompression speed - raphlinus/nibnib
---

An image format tuned for extreme decompression speed.

This image format originated as an experiment to see what image quality and performance were possible for DVI output on an RP2350 microcontroller. On devices of that class, it's common to use low bit depth images and palette lookup. There's a budget of approximately 6 cycles per pixel, and that's enough for a pixel fetch, palette lookup, and store into the line buffer, but not a lot of room to spare.

The nibnib format uses a palette at its core, but surpasses palette images in quality because the palette stores a *delta* rather than the absolute color, vastly improving soft regions. It achieves its extreme speed by decoding two pixels at a time, from a single byte of input, thus attaining 4bpp compression.

## How it works

The decompression process is simple and deterministic. There are many choices when encoding; the algorithm is asymmetric in that regard.

Decoding works in pixel pairs. The last decoded pixel is doubled up. Then we read a byte from the input stream, and look it up in the palette. That 32-bit value is added to the doubled-up previous pixel, yielding two 16bpp (RGB 565 format) pixels. The low 16 bits are the left pixel. That's it.

[![A diagram of how the algorithm works](https://github.com/raphlinus/nibnib/raw/main/images/algorithm.png)](https://github.com/raphlinus/nibnib/blob/main/images/algorithm.png)

Here's the inner loop (lightly adapted from the Rust source):

```rust
    // first two pixels copied from src[0..4], initializing pix
    for x in 1..(width / 2) {
        let inp = src[x + 3];
        pix = ((pix >> 16) * 0x10001).wrapping_add(pal[inp as usize]);
        out[x * 2] = pix & 0xffff;
        out[x * 2 + 1] = pix >> 16;
    }
```

The RP2350 on-the-fly decoder implementation is included in the `image` demo of the [vic-2350](https://github.com/raphlinus/vic-2350) repo.

## The simple encoder

As with many image compression formats, nibnib is defined by the decoding process, which is deterministic and straightforward. Encoding is fundamentally a search through the space of possible encoded images to find one that decodes to a (hopefully) high quality replica of the original. In nibnib, this search is quite asymmetrical; the current encoder is orders of magnitude slower than decoding. By contrast, in JPEG, the DCT transform is invertible by its mathematical nature, and quantization is also easy; at baseline, encoding and decoding are about the same speed, though search can be used to fine-tune parameters and micro-optimize quality.

Nibnib has two main dimensions for the search: choice of palette, and selection of codewords. For the initial version, the palette has a cube of small delta values plus a block of random values, chosen by weighted Gaussians. The cube is necessary to ensure that values near 0 and full scale can be attained without risk of overflow; without the cube entries, you get bad artifacts. First is the original image, then the image encoded with a random-only palette, finally with both the cube and random values.

[![A test image](https://github.com/raphlinus/nibnib/raw/main/images/flowers7.png)](https://github.com/raphlinus/nibnib/blob/main/images/flowers7.png)

[![An image encoded with artifacts](https://github.com/raphlinus/nibnib/raw/main/images/no_cube.png)](https://github.com/raphlinus/nibnib/blob/main/images/no_cube.png)

[![An image encoded with fewer artifacts](https://github.com/raphlinus/nibnib/raw/main/images/with_cube.png)](https://github.com/raphlinus/nibnib/blob/main/images/with_cube.png)

The random Gaussian process was refined on an ad-hoc basis, using guesses about reasonable values. In particular, the delta between the two pixels gives a tradeoff between sharpness and noise; keeping it small makes the image smoother at the cost of reduced ability to encode sharp edges.

In the current encoder, the palette is not chosen adaptively based on the input image, but that is most definitely an opportunity to improve quality.

## Floyd-Steinberg error diffusion

The basic encoding loop is conceptually straightforward, but the details are a bit subtle.

Conceptually, it simply tries all 256 palette entries, and chooses the one with the minimum error. Then it takes that error and distributes it using [Floyd-Steinberg](https://en.wikipedia.org/wiki/Floyd%E2%80%93Steinberg_dithering) weights. Subsequent pixels fit not the source pixel value, but the source pixel plus errors from previous iterations, weighted.

The loop as described above is quite similar to error diffusion with a quantized palette, a very popular technique. However, that's usually done with one pixel at a time, while the nibnib algorithm generates two pixels at a time. The palette entry is chosen to minimize the error of *both* pixels, giving somewhat higher weight to the sum rather than the difference, modeling the fact that the human visual system is more sensitive to lower spatial frequencies.

## Future improvements

What's presented is a basic, working image compression technique. Quality can be improved significantly by tuning both major aspects of the search: coming up with an optimized palette, and also a deeper search for error-minimizing pixels. For *this* pixel, two choices might have roughly equal error, but one might make the search for the *next* to be considerably more favorable.

Currently, the artifacts trail to the right of sharp edges, but that's a consequence of the greedy search. It's not hard to see how to make encoding work right to left (or even boustrophedonically, alternating direction each row) while decoding is still left to right, as the addition step is invertible in practice (there's a bit of subtlety around the interaction of the carry and the pixel duplication).

Both improvements would slow the encoder down considerably, though decoding speed would be unchanged. Another direction is to make the encoder faster. For example, brute force search through palette entries can almost certainly be improved, perhaps in combination with making the palette more structured. All these improvements can be made with the existing decoder.

## Image format

The image format is extremely unsophisticated: 2 bytes each for width and height, then 256 32-bit palette entries, then the rows in order.

Each row begins with two pixels (RGB 565 each), followed by ((width - 2) / 2) bytes, each indexing the palette. The row stride is rounded up to a multiple of 4 bytes.

Currently there's no other metadata; the format is not intended for archival, and is much more likely to be used when integrated tightly with the decoder.

## License

Licensed under either of

- Apache License, Version 2.0 ([LICENSE-APACHE](https://github.com/raphlinus/nibnib/blob/main/LICENSE-APACHE) or [http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0))
- MIT license ([LICENSE-MIT](https://github.com/raphlinus/nibnib/blob/main/LICENSE-MIT) or [http://opensource.org/licenses/MIT](http://opensource.org/licenses/MIT))

at your option.
