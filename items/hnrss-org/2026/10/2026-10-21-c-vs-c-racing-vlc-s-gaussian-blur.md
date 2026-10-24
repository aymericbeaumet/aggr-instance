---
title: 'C vs. C#: racing VLC''s Gaussian blur'
link: https://martinfinkel.com/blog/gaussian-blur-c-vs-csharp/
source: hnrss-org
published: 2026-10-21T05:48:12Z
updated: 2026-10-21T05:48:12Z
first_seen: 2026-10-24T13:33:06.828343888Z
authors:
- martz
content: extracted
html: 2026-10-21-c-vs-c-racing-vlc-s-gaussian-blur.html
remote_preview:
  url: https://martinfinkel.com/og/blog/gaussian-blur-c-vs-csharp.png
  alt: 'C vs C#: racing VLC''s Gaussian blur - Martin Finkel'
---

October 12, 2026

*Update, October 12: two small source changes bring the line-by-line C# within 8% of the C. Details at the end of [Round 1](https://martinfinkel.com/blog/gaussian-blur-c-vs-csharp/#round-1-line-by-line).*

In my [last post](https://martinfinkel.com/blog/vlc-plugin-csharp/), I showed that a native VLC 4 plugin can be written entirely in C#, thanks to .NET Native AOT. The obvious next question is not *how*, but *how fast*. VLC is written in C, video filters touch every pixel of every frame, and C# has a garbage collector. Surely that costs something?

Rather than guess, I picked a real VLC filter, ported it to C# line by line, and raced the two inside the same VLC, on the same frames, with byte-identical output. Then I optimized both and raced them again. Twice.

## A blur with a history

A Gaussian blur replaces every pixel with a weighted average of its neighbours: the closest count the most, and the weight fades along the bell curve Carl Friedrich Gauss used in 1809 to describe measurement errors. That smooth fade is what makes it look like an out-of-focus lens rather than a boxy smear. One number controls it, **sigma**: VLC looks at neighbours up to 3 x sigma pixels away, so sigma 4 means a 25-pixel window.

![The same frame, original and blurred with VLC's filter at sigma 2, 4 and 8](https://martinfinkel.com/assets/gaussian-blur-sigmas.webp)

It is one of the oldest effects in digital imaging: [Photoshop 1.0](https://guidebookgallery.org/apps/photoshop/gaussianblur) had it in 1990, and GIMP, FFmpeg and GStreamer all have one. VLC has had it since 2007, in *Tools > Effects and Filters > Video Effects > Advanced*, as “Spatial blur”:

![VLC's Adjustments and Effects dialog, Video Effects, Advanced tab, with Spatial blur enabled and its sigma slider](https://martinfinkel.com/assets/vlc-effects-spatial-blur.webp)

## How VLC does it

A Gaussian blur is *separable*: blurring every row, then every column, gives exactly the same result as the full 2D window, so a 25 x 25 window (625 multiplications per pixel) becomes two passes of 25. VLC’s [gaussianblur.c](https://code.videolan.org/videolan/vlc/-/blob/master/modules/video_filter/gaussianblur.c) does exactly that, in integers: a horizontal pass into a buffer, a vertical pass over the buffer, then a division by the sum of the weights. Here is the horizontal pass:

```c
for( int i_line = 0; i_line < i_visible_lines; i_line++ )
{
    for( int i_col = 0; i_col < i_visible_pitch; i_col++ )
    {
        type_t t_value = 0;
        const int c = i_line*i_in_pitch+i_col;
        for( int x = __MAX( -i_dim, -i_col*(x_factor+1) );
             x <= __MIN( i_dim, (i_visible_pitch - i_col)*(x_factor+1) + 1 );
             x++ )
        {
            t_value += pt_distribution[x+i_dim] *
                       p_in[c+(x>>x_factor)];
        }
        pt_buffer[c] = t_value;
    }
}
```

## The same code, in C#

The C# version is a deliberate line-by-line port: same loops, same bounds, same integer math, raw pointers where the C uses raw pointers.

```csharp
for (int line = 0; line < visibleLines; line++)
{
    for (int col = 0; col < visiblePitch; col++)
    {
        int value = 0;
        int c = line * inPitch + col;
        for (int x = Math.Max(-dim, -col * (xFactor + 1));
             x <= Math.Min(dim, (visiblePitch - col) * (xFactor + 1) + 1);
             x++)
        {
            value += distribution[x + dim] * input[c + (x >> xFactor)];
        }
        buffer[c] = value;
    }
}
```

It runs as a VLC plugin through [VLCLR](https://github.com/mfkl/vlclr), compiled with Native AOT: a native DLL that VLC loads like any other plugin. The filter allocates nothing on the managed heap per frame, so the garbage collector has nothing to collect while it runs.

To keep things fair, every version below runs in the same VLC 4 release build, against that build’s own unmodified filter, and produces byte-identical frames to it. Each filter call is timed the same way, on a 1080p clip, on a laptop with an 11th-gen Intel Core i7 (Tiger Lake, 4 cores). Each round shows milliseconds per frame on .NET 10 at three blur strengths, next to frames per second at sigma 4. .NET 8 and 11 land within a few percent.

## Round 1: line by line

Milliseconds per 1080p frame (C# on .NET 10), lower is better.

|             | sigma 2 | sigma 4 | sigma 8 |
| ----------- | ------- | ------- | ------- |
| Current VLC | 62.0    | 125.9   | 252.9   |
| C# port     | 78.9    | 155.2   | 298.1   |

![Frames per second at sigma 4: current VLC 7.9, C# port 6.4](https://martinfinkel.com/assets/gaussian-blur-round1.webp)

C wins, as expected: the C# port takes about 23% longer per frame. Not a disaster, but a clear gap.

The machine code shows why. For the inner loop above, clang keeps the running sum in a register and computes the loop bound once. .NET’s code generator, [RyuJIT](https://github.com/dotnet/runtime/blob/main/docs/design/coreclr/jit/ryujit-overview.md), emits about twice as many instructions: it recomputes `Math.Min(...)` on every step and keeps the sum in memory. Same arithmetic, more bookkeeping around it.

**Update, October 12.** After publishing, I checked how much of this gap is the code generator and how much is the way the C# is written. Two small edits close most of it: compute the loop bounds into locals before the loop, so RyuJIT stops re-evaluating `Math.Min(...)` on every step, and use `nint` for the indices, so they no longer need a 32 to 64-bit conversion before every memory access. Clang does both on its own. Same loops, same arithmetic, byte-identical output:

```diff
-        int c = line * inPitch + col;
-        for (int x = Math.Max(-dim, -col * (xFactor + 1));
-             x <= Math.Min(dim, (visiblePitch - col) * (xFactor + 1) + 1);
-             x++)
+        nint c = (nint)line * inPitch + col;
+        nint xStart = Math.Max(-dim, -col * (xFactor + 1));
+        nint xEnd = Math.Min(dim, (visiblePitch - col) * (xFactor + 1) + 1);
+        for (nint x = xStart; x <= xEnd; x++)
```

Milliseconds per 1080p frame (C# on .NET 10), lower is better. A new run, so the first two rows differ slightly from the table above.

|                         | sigma 2 | sigma 4 | sigma 8 |
| ----------------------- | ------- | ------- | ------- |
| Current VLC             | 62.6    | 137.9   | 256.7   |
| C# port                 | 78.9    | 169.0   | 303.6   |
| C# port, bounds hoisted | 67.9    | 126.4   | 263.5   |

With those two edits, the line-by-line C# lands within 8% of the C, and is ahead at sigma 4. Part of the gap was the compiler, and part was code the compiler needed a little help with.

## Round 2: same algorithm, both optimized

Before reaching for SIMD, there is a lot to gain from the algorithm itself. I applied the same four changes to both, still plain scalar code with no SIMD and no threads:

1. **Replace the division by a multiplication.** Away from the borders the divisor never changes, so a precomputed “magic” reciprocal and a shift give exactly the same result, without the slowest instruction in the loop.
2. **Read memory in the order it is stored.** The vertical pass walked down columns, jumping a whole row ahead at every step. Accumulating whole rows instead reads memory sequentially.
3. **Use the symmetry of the bell curve.** `w*a + w*b` becomes `w*(a+b)`: half the multiplications.
4. **Handle the borders separately,** so the interior runs a simpler loop with no bounds to compute.

Milliseconds per 1080p frame (C# on .NET 10), lower is better.

|              | sigma 2 | sigma 4 | sigma 8 |
| ------------ | ------- | ------- | ------- |
| Current VLC  | 62.0    | 125.9   | 252.9   |
| Optimized C  | 23.3    | 40.7    | 75.4    |
| Optimized C# | 28.1    | 50.6    | 98.9    |

![Frames per second at sigma 4: current VLC 7.9, optimized C 25, optimized C# 20](https://martinfinkel.com/assets/gaussian-blur-round2.webp)

**The C# version, still plain scalar code, is 2.5 times faster than the current VLC code.** C stays ahead, though: the C# takes about 24% longer, for the same reasons as in round 1. One more C advantage: clang turns these simpler loops into SIMD code by itself. I turned that off to compare the same code, but with it on, the C reaches 34 fps without a single line of SIMD in the source.

## Round 3: tuned to the limit

The last round lets both sides use everything they have: AVX2 SIMD, on a single thread, with the same byte-identical output. C uses intrinsics from `immintrin.h`, C# uses `System.Runtime.Intrinsics`. The design is the same in both: 16 pixels per instruction in the horizontal pass, merged taps for the colour planes, and a division done exactly in floating point. The inner loop of the horizontal pass:

```c
__m256i sum = _mm256_mullo_epi16(
    _mm256_cvtepu8_epi16( _mm_loadu_si128( (const __m128i *)p ) ), vw0 );
for( int i = 0; i < pair_count; i++ )
{
    const int o = pair_offsets[i];
    const __m256i both = _mm256_add_epi16(
        _mm256_cvtepu8_epi16( _mm_loadu_si128( (const __m128i *)(p - o) ) ),
        _mm256_cvtepu8_epi16( _mm_loadu_si128( (const __m128i *)(p + o) ) ) );
    sum = _mm256_add_epi16( sum,
        _mm256_mullo_epi16( both, _mm256_set1_epi16( (short)pair_weights[i] ) ) );
}
```

```csharp
Vector256<ushort> sum = Avx2.MultiplyLow(
    Avx2.ConvertToVector256Int16(p).AsUInt16(), vw0);
for (int i = 0; i < pairCount; i++)
{
    int o = pairOffsets[i];
    Vector256<ushort> both = Avx2.Add(
        Avx2.ConvertToVector256Int16(p - o).AsUInt16(),
        Avx2.ConvertToVector256Int16(p + o).AsUInt16());
    sum = Avx2.Add(sum,
        Avx2.MultiplyLow(both, Vector256.Create((ushort)pairWeights[i])));
}
```

If anything, the C# *reads* better: the types say what is in each register, where C has one `__m256i` for everything.

In my first SIMD version, the C# took 14% longer, and 30% longer at the largest blur: same vector instructions, but clang unrolled the loops around them and kept their pointers in registers. So I kept going on both sides, with wider loops, two taps per multiply (`vpmaddwd`), vectorized borders and no scalar tails. On the C# side only, I also reshaped the code until .NET produced what clang produces on its own: the hot loop in its own method, and weights broadcast straight from memory.

Milliseconds per 1080p frame (C# on .NET 10), lower is better.

|             | sigma 2 | sigma 4 | sigma 8 |
| ----------- | ------- | ------- | ------- |
| Current VLC | 62.5    | 128.9   | 253.8   |
| Tuned C     | 3.08    | 4.17    | 6.62    |
| Tuned C#    | 3.05    | 4.23    | 7.22    |

![Frames per second at sigma 4: current VLC 7.8, tuned C 240, tuned C# 236](https://martinfinkel.com/assets/gaussian-blur-round3.webp)

Now it is close: at sigma 2 and 4 the two are on par, within the measurement noise, and the C# takes 9% longer at the largest blur. Both are 30 times faster than the current VLC code.

## What I take away

![Original on the left, VLC's Gaussian blur at sigma 8 on the right](https://martinfinkel.com/assets/gaussian-blur-split.webp)

| sigma 4, 1080p                       | C       | C#      |
| ------------------------------------ | ------- | ------- |
| Current VLC code / line-by-line port | 8 fps   | 6 fps   |
| Same algorithm, optimized            | 25 fps  | 20 fps  |
| Tuned to the limit (SIMD)            | 240 fps | 236 fps |

- **Line for line, the C# port takes about 23% longer.** Not because of the garbage collector, which has nothing to collect here, or bounds checks, which raw pointers avoid. It is the code generator: LLVM does a better job on these loops than RyuJIT. *Update:* two small edits that keep the loops and the arithmetic bring it within 8% of the C (see [Round 1](https://martinfinkel.com/blog/gaussian-blur-c-vs-csharp/#round-1-line-by-line)).
- **With work, the gap nearly closes.** Part of that work was rewriting C# so .NET emits what clang emits by default. That is the real cost of choosing C# here: knowing what the compiler does with your loops.
- **C# is a realistic choice for a VLC plugin that does its work on the CPU.** The C# filter blurs 1080p video at about 140 frames per second even at sigma 8, on one core.

Along the way, I also found two VLC bugs that have nothing to do with speed: with hardware decoding, any CPU filter turned half the picture green, and the blur reads two bytes past the right edge of each line. I’ve opened merge requests for both.

There are ways to make both versions faster still, such as AVX-512 or spreading the work over several cores, but I stopped there.

I would also like to know how much of the line-by-line gap really is the code generator. .NET has an experimental [NativeAOT-LLVM](https://github.com/dotnet/runtimelab/tree/feature/NativeAOT-LLVM) branch that compiles C# with LLVM, the same backend clang uses. It only targets WebAssembly for now, so it can build a VLC plugin for the WebAssembly version of VLC but not for the desktop VLC used here.

This was an experiment to compare the two languages on the same CPU code, not the best way to blur video: a filter like this belongs on the GPU, and GPU versions of a few VLC filters are in progress.

If the last 10% matters, C is still the answer. For everything else, the language you are most productive in, plus a good algorithm, will get you further.

## Try it yourself

The blur is now part of VLCLR, in a new [`VLCLR.Filters`](https://github.com/mfkl/vlclr/tree/main/src/VLCLR.Filters) library: `GaussianBlur` runs all of these versions on VLC pictures. The [GaussianBlur sample](https://github.com/mfkl/vlclr/tree/main/samples/GaussianBlur) wraps it in a complete VLC filter plugin of about 70 lines, from reading the sigma option to allocating the output picture.

As with the previous post, I designed the experiment and the rules, and [Claude](https://claude.ai) wrote most of the code and the benchmark harness.
