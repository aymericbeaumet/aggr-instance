---
title: Obscure audio codec is a perfect fit for the N64
link: https://phoboslab.org/log/2026/11/n64-ulc-codec
source: hnrss-org
published: 2026-11-05T12:51:48Z
updated: 2026-11-05T12:51:48Z
first_seen: 2026-11-05T17:57:26.867095589Z
authors:
- frostbyte7
content: extracted
html: 2026-11-05-obscure-audio-codec-is-a-perfect-fit-for-the-n64.html
---

— Tuesday, November 3rd 2026

Sorry for the clickbait title. I promise this post delivers: 44100 Hz stereo with near-Opus bitrates using about 4% of the N64’s CPU time.

But first, some history: The options for playing and storing audio data on the N64 were dire. The PlayStation could just stream uncompressed redbook audio straight from the CD. On the N64, cartridge space was at a premium and time on the 93 MHz CPU was limited.

Worse, access to the vector co-processor, the RSP, was guarded by Nintendo for the longest time. Nintendo's own platform library, Libultra, internally used the RSP for various graphics and sound-related tasks, but Nintendo didn't allow third-party developers to directly write code for it.

Libultra provided some functions to transparently play back compressed audio using an ADPCM scheme (adaptive differential pulse code modulation). The RSP handled decompression on the fly, just before the audio was sent to the output buffer.

This works well enough and cuts the space needed for a sample by a factor of 4. Still, a 5-minute stereo audio track at 44100 Hz will land at about 12MB – way too much for an N64 cartridge, even when you cut the sample rate in half. The whole Mario 64 ROM is just 8MB in total. The biggest N64 games were 64MB.

So N64 games produced music similarly to a tracker: short samples were scheduled and played back with some limited effects at the right time. With the space constraints on the cartridge, fully “recorded” audio was pretty much out of the question.

Later in the N64's life, some blessed developers were allowed by Nintendo to directly access the RSP. Most famously, Conker's Bad Fur Day used this to decompress MP3 audio for the myriad of speech samples in the game. Decoding MP3 is quite expensive and the frame rate of the game suffered. It was good enough for cutscenes, but just too much for continuous background music or sound effects during gameplay.

## Audio in the Open Source Libdragon

Similar to Nintendo's Libultra, [Libdragon](https://github.com/n64brew/libdragon), the open source alternative, has a real-time ADPCM decoder running on the RSP. So with ROM space being cheaper these days, I just compressed all music for [Xibalba 64](https://phoboslab.org/log/2026/08/xibalba64-making-of) with ADPCM. About 31MB of the 32MB ROM are used for sound and music. Remember that Mario 64 just used an 8MB ROM and Xibalba 64 is a much simpler game with only 15 minutes of music. It worked, but storing music this way is a gratuitous waste of space.

Now, Libdragon also has a real-time, RSP-accelerated Opus decoder. It's a marvel in itself. The Opus format was published 16 years after the N64 and yet Giovanni Bajo managed to make it work on this *“obsolete”* hardware. Compression and audio quality are excellent, but again – it's too slow to be used during gameplay.

What's the alternative then? We could build an RSP-accelerated MP3 decoder, but the performance gain over Opus would be marginal, and quality and compression would be worse.

[QOA](https://phoboslab.org/log/2023/02/qoa-time-domain-audio-compression) would be a slight improvement over ADPCM, but it's just a poor fit for the vector co-processor: each sample relies on the previous decoded sample. Hard to vectorize.

A simple transform codec, something *like* MP3, but without all the design-by-consortium baggage, may work…

## The Ultra Low Complexity Codec

During some late-night *“research”*, I stumbled across an old Game Boy Advance demo that plays recorded music at just 70kbps. That's less than a typical MP3, but at comparable quality. I repeat: On a GBA. With a 16 MHz ARM CPU. With no vector instructions and, according to the GitHub page, some headroom to spare.

*[ulcplayer-gba](https://github.com/Aikku93/ulcplayer-gba) by Aikku93, recorded in a Game Boy Advance emulator*

Time-domain codecs (such as ADPCM) need significantly higher bitrates for this quality. Subband codecs, such as MP2, have much worse quality at comparable bitrates, and more “modern” transform codecs, such as MP3, usually [need at least an 80–100 MHz CPU](https://www.youtube.com/watch?v=b0zZpzxHSeM). For this GBA demo, no existing codec would fit the bill.

So [Aikku93](https://github.com/Aikku93) not only wrote a decoder in ARM assembly, but also created a whole new codec: The [Ultra Low Complexity Codec](https://github.com/Aikku93/ulc-codec) (ULC).

ULC *is* a transform codec and somewhat similar to MP3. ULC transforms slices of PCM data into the frequency domain. Like MP3, it uses the Modified Discrete Cosine Transform (MDCT) to do so. But unlike MP3, it does not split the signal into multiple bands (ULC's MDCT is “full-band”). Where MP3 uses classic entropy encoding (Huffman specifically), ULC does… not. Instead, transformed coefficients are stored with a very simple nibble-based syntax.

In short: ULC is what MP3 could have been. The same core idea, but without the cruft around it. The “ultra low complexity” does not come from a different algorithm, but by carefully applying and packaging it.

The nibble-based packaging is maybe the most elegant idea of ULC. Each coefficient is stored in just 4 bits, interpreted as a signed integer. Some values are reserved to indicate zero fill, noise fill, or a quantizer change:

- `-7 … -2`, `2 … 7` - coefficient value
- `0` - zero run, run length (`1 … 16`) follows in the next nibble
- `1` - long zero run, run length (`17 … 288`) follows in the next two nibbles
- `-1` - quantizer change, value follows in the next nibble
- `-8` - noise fill, length and amplitude follow in the next three nibbles

Everything is nibble aligned. This means the whole codec works without any arbitrary bit reads or entropy decoding. Reading and unpacking a block of coefficients takes just [\~150 lines of C](https://github.com/Aikku93/ulc-codec/blob/b47df8d50c2ef2d765e5dbec0bf567b5e558173a/libulc/ulcDecoder.c#L99-L197).

Also note that noise fill is a valuable addition. Cymbals, hi-hats and snare drums greatly benefit from that. MP3 does not have anything similar and needs to encode quasi-random coefficients *as is*.

ULC can produce *usable* results even at 16kbps. Quality of course suffers, but it might still be good enough for some applications.

Aikku93 has been working on ULC since 2019 and has steadily improved the encoding quality. All code is permissively licensed. Time to try this on the N64 then!

## ULC on the N64

I spent an evening in pre-LLM days compiling the ULC reference decoder for the N64 and producing a small demo. It worked without a hitch.

The only problem: it was keeping the CPU very busy. Nothing was offloaded to the RSP. As a proof of concept, it wasn't doing much: we already knew the 93 MHz CPU was enough to decode a transform-based codec, but we don't need a “music player” that does nothing else. We need this for background music and spoken dialog during gameplay. It *cannot* hog the CPU.

### Float to 16-bit Fixed Point

Obviously the IMDCT needs to be moved to the RSP coprocessor instead. We could just DMA one block of decoded coefficients to the RSP's memory, do the transform and DMA the completed samples back to RDRAM into the output buffer.

That's easier said than done, though. The RSP has only 4096 bytes of data memory (DMEM) and another 4096 bytes for instructions (IMEM). The vector instructions work in 16-bit or 32-bit fixed point, with no floats. The IMDCT will only be efficient if all the data for a transform fits into the RSP's DMEM at once. We also need some of the DMEM for temporary values and – if possible – the lapping buffer: half of the previously decoded block. So with the default 2048-sample block size of ULC, we're out of luck, even with 16-bit fixed point.

While ULC can work with smaller MDCT block sizes, it was optimized for 2048, so we should keep it as large as possible. On top of that, the ULC reference encoder/decoder uses floats, so the obvious choice would be 32-bit fixed point to maintain quality, but 16-bit *may* work. So we have two options, both arriving at `3072 bytes`:

1. 32-bit values, a 512-sample block size (256-sample lap buffer): `(512 + 256) * 4 bytes`
2. 16-bit values, a 1024-sample block size (512-sample lap buffer): `(1024 + 512) * 2 bytes`

Using 16-bit fixed point would not only reduce the DMA traffic, but also speed up computation on the RSP. The trade-off would be a higher noise floor. How high? No idea. So I carefully refactored the reference decoder, still running on the N64 CPU, to work in 16-bit fixed point (Q1.14).

After almost blowing my ears off a bunch of times, I got it working and could only hear (or imagine?) the faintest white noise when cranking it up. Case settled: 16-bit it is.

### Cramming it into the RSP

Now I *just* needed to port this C code to the RSP. My knowledge of MIPS assembly is very basic, and writing an RSP decoder would be a serious endeavor.

This is where I took a hiatus from this project – I had a [game](https://phoboslab.org/log/2026/08/xibalba64-making-of) to finish. When I returned earlier this year LLMs have improved quite a bit. Still, I didn't exactly trust GPT-5 with it. In some previous experiments it did a very bad job, often ignoring delay slots and other intricacies of the RSP, never finishing the job.

*(Aside: GPT-6 or Opus-5 would probably one-shot this now)*

So instead I opted for a more iterative approach. Max Bebök created a small C-like language to write code for the RSP, aptly named [RSPL](https://github.com/HailToDodongo/rspl). Armed with the language documentation, I asked Codex to translate the C code to RSPL piece by piece, always verifying the result. It took a few days to port and land on an API design that made sense. Then in subsequent steps Codex further optimized the RSPL assembly output by first rewriting some parts (directly in assembly this time), then minimizing pipeline stalls by [adapting](https://github.com/phoboslab/rspopt) the RSPL's optimizer.

The end result is [\~1400 lines of MIPS assembly](https://github.com/n64brew/libdragon/blob/preview/src/audio/rsp_ulc.S) handling the IMDCT, lapping, stereo interleaving and output scaling. It barely fits into the 4096 byte IMEM of the RSP.

The [C side of the decoder](https://github.com/n64brew/libdragon/blob/preview/src/audio/wav64_ulc.c) is still handwritten, but only because LLMs have no taste. The whole process took me a few weeks, but I'm still amazed that it worked at all.

*My [ulc64 demo](https://github.com/phoboslab/ulc64/) demonstrating quality at various bitrates*

To make ULC easily seekable, I added a sparse table at the end of the file that contains the byte offset of every eighth frame. The decoder quickly skips ahead from the nearest frame to the desired one. This adds about 20 bytes per second – manageable even for 16kbps audio.

ULC proved to be a *very* viable alternative to ADPCM and Opus on the N64. Quality and bitrate are close to those of Opus, but complexity is only 4x that of ADPCM. A single 44100 Hz stereo track costs about 4% of CPU time and 5% of RSP time. Perfectly usable for background music, speech, ambience and longer sound effects.

The decoder works transparently in Libdragon, just like all other audio codecs. It has been merged into the [preview branch](https://github.com/n64brew/libdragon/tree/preview) a while ago.

## Addendum

A few days ago an excellent port of Half-Life for the N64 was published. The `README` states:

> \[...\] libdragon's ULC audio codec for the 2,500 sounds and 27 music tracks, is what makes everything fit in one 64 MB cartridge image.

\~ [github.com/ToxyCola/Half-Life-Nintendo-64-Port](https://github.com/ToxyCola/Half-Life-Nintendo-64-Port)

[www.youtube.com](https://www.youtube.com/watch?v=QhxXwX_wnVI)

Many thanks to [Aikku93](https://github.com/Aikku93) for creating ULC. It's a wonderful codec. I merely found and adapted it. I seek simplicity and elegance in everything I do and ULC is something I can truly adore. Kudos!
