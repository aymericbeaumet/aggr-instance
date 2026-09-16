---
title: Vectorized and performance-portable Quicksort
link: https://opensource.googleblog.com/2022/06/Vectorized%20and%20performance%20portable%20Quicksort.html
source: hnrss-org-frontpage
published: 2026-09-16T18:31:35Z
updated: 2026-09-16T18:31:35Z
first_seen: 2026-09-16T20:24:49.200228376Z
authors:
- mococa
summary: 'Article URL: https://opensource.googleblog.com/2022/06/Vectorized%20and%20performance%20portable%20Quicksort.html Comments URL: https://news.ycombinator.com/item?id=49731054 Points: 130 # Comments: 19'
content: extracted
html: 2026-09-16-vectorized-and-performance-portable-quicksort.html
preview:
  file: 2026-09-16-vectorized-and-performance-portable-quicksort.preview-308ba74b17df.webp
  width: 256
  height: 75
  color: '#fafafa'
images:
- source: https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjqY5Zp8L4lAAwsahUx-H9A_6l6iyw5nSvuu6-HtJr1_Gnh0OtDeo_7OrlGfDkW5oVIHLVSTlRcvznaA3fBVi_GRD_iXbhryVoo8B6EbBIm3cZctlm3tyaJvGzNptPhvXbJ2TyUOTCoP3lXI3fH8fd4U4gsELVNP_KawFQNYnPbcbysAMRsP5BmaAnc/w1200-h630-p-k-no-nu/Screen%20Shot%202022-06-01%20at%201.08.53%20PM.png
  original:
    file: 2026-09-16-vectorized-and-performance-portable-quicksort.image-0f15157c3b44.png
    width: 1188
    height: 346
  color: '#fcfcfc'
- source: https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjqY5Zp8L4lAAwsahUx-H9A_6l6iyw5nSvuu6-HtJr1_Gnh0OtDeo_7OrlGfDkW5oVIHLVSTlRcvznaA3fBVi_GRD_iXbhryVoo8B6EbBIm3cZctlm3tyaJvGzNptPhvXbJ2TyUOTCoP3lXI3fH8fd4U4gsELVNP_KawFQNYnPbcbysAMRsP5BmaAnc/s16000/Screen%20Shot%202022-06-01%20at%201.08.53%20PM.png
  original:
    file: 2026-09-16-vectorized-and-performance-portable-quicksort.image-0f15157c3b44.png
    width: 1188
    height: 346
  color: '#fcfcfc'
- source: https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEinMQFHyn-5mc4if8A57r_6I3EdvqcjfzML2TFqHxBN0rO_Yiihd4RMdTbKNXZ79eiQk0hQ8SMHMc7kvQ_CTZVSvY2k2uoUgTHr9MnIaQY9KDGXzJRfmidRLKV3U5LHm-LAkU0XwPs_MvyCM9MQM5mk-LLGdjkLYLzeLmV7s3iwbydy7OyC_QMo46P6/s16000/CC%20BY%20Oak%20Ridge%20National%20Laboratory%20Summit_supercomputer_(44552259580).jpeg
  original:
    file: 2026-09-16-vectorized-and-performance-portable-quicksort.image-33a30970ca16.jpg
    width: 4216
    height: 2371
  color: '#16160b'
---

Today we're sharing open source code that can sort arrays of numbers about ten times as fast as the C++ std::sort, and outperforms state of the art architecture-specific algorithms, while being portable across all modern CPU architectures. Below we discuss how we achieved this.

First, some background. There is a recent trend towards columnar databases that consecutively store all values from a particular column, as opposed to storing all fields of a record or "row" before those of the next record. This can be faster to filter or sort, which are key building blocks for SQL queries; thus we focus on this data layout.

Given that sorting has been heavily studied, how can we possibly find a 10x speedup? The answer lies in SIMD/vector instructions. These carry out operations on multiple independent elements in a single instruction—for example, operating on 16 float32 at once when using the AVX-512 instruction set, or four on Arm NEON:

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjqY5Zp8L4lAAwsahUx-H9A_6l6iyw5nSvuu6-HtJr1_Gnh0OtDeo_7OrlGfDkW5oVIHLVSTlRcvznaA3fBVi_GRD_iXbhryVoo8B6EbBIm3cZctlm3tyaJvGzNptPhvXbJ2TyUOTCoP3lXI3fH8fd4U4gsELVNP_KawFQNYnPbcbysAMRsP5BmaAnc/s16000/Screen%20Shot%202022-06-01%20at%201.08.53%20PM.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEjqY5Zp8L4lAAwsahUx-H9A_6l6iyw5nSvuu6-HtJr1_Gnh0OtDeo_7OrlGfDkW5oVIHLVSTlRcvznaA3fBVi_GRD_iXbhryVoo8B6EbBIm3cZctlm3tyaJvGzNptPhvXbJ2TyUOTCoP3lXI3fH8fd4U4gsELVNP_KawFQNYnPbcbysAMRsP5BmaAnc/s1188/Screen%20Shot%202022-06-01%20at%201.08.53%20PM.png)

[![Summit supercomputer](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEinMQFHyn-5mc4if8A57r_6I3EdvqcjfzML2TFqHxBN0rO_Yiihd4RMdTbKNXZ79eiQk0hQ8SMHMc7kvQ_CTZVSvY2k2uoUgTHr9MnIaQY9KDGXzJRfmidRLKV3U5LHm-LAkU0XwPs_MvyCM9MQM5mk-LLGdjkLYLzeLmV7s3iwbydy7OyC_QMo46P6/s16000/CC%20BY%20Oak%20Ridge%20National%20Laboratory%20Summit_supercomputer_\(44552259580\).jpeg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEinMQFHyn-5mc4if8A57r_6I3EdvqcjfzML2TFqHxBN0rO_Yiihd4RMdTbKNXZ79eiQk0hQ8SMHMc7kvQ_CTZVSvY2k2uoUgTHr9MnIaQY9KDGXzJRfmidRLKV3U5LHm-LAkU0XwPs_MvyCM9MQM5mk-LLGdjkLYLzeLmV7s3iwbydy7OyC_QMo46P6/s4216/CC%20BY%20Oak%20Ridge%20National%20Laboratory%20Summit_supercomputer_\(44552259580\).jpeg)

If you are already familiar with SIMD, you may have heard of it being used in supercomputers, linear algebra for machine learning applications, video processing, or image codecs such as [JPEG XL](https://en.wikipedia.org/wiki/JPEG_XL). But if SIMD operations only involve independent elements, how can we sort them, which involves re-arranging adjacent array elements?

Imagine we have some special way to sort, for instance 256 element arrays. Then, the [Quicksort algorithm](https://en.wikipedia.org/wiki/Quicksort) for sorting a larger array consists of partitioning it into two sub-arrays: those less than a "pivot" value (ideally the median), and all others; then recursing until a sub-array is at most 256 elements large, and using our special method for sorting those. Partitioning accounts for most of the CPU time, so if we can speed it up using SIMD, we have a fast sort.

Happily, modern instruction sets (Arm SVE, RISC-V V, x86 AVX-512) include a special instruction suitable for partitioning. Given a separate input of yes/no values (whether an element is less than the pivot), this "compress-store" instruction stores to consecutive memory only the elements whose corresponding input is "yes". We can then logically negate the yes/no values and apply the instruction again to write the elements to the other partition. This strategy has been used in an [AVX-512-specific Quicksort](https://arxiv.org/pdf/1704.08579.pdf). But what about other instruction sets such as AVX2 that don't have compress-store? [Previous work](https://academic.oup.com/comjnl/article-abstract/59/1/83/2568602?login=false) has shown how to emulate this instruction using permute instructions.

We build on these techniques to achieve the first vectorized Quicksort that is portable to six instruction sets across three architectures, and in fact outperforms prior architecture-specific sorts. Our implementation uses [Highway's](https://github.com/google/highway) portable SIMD functions, so we do not have to re-implement about 3,000 lines of C++ for each platform. Highway uses compress-store when available and otherwise the equivalent permute instructions. In contrast to the previous [state of the art](https://drops.dagstuhl.de/opus/volltexte/2021/13775/pdf/LIPIcs-SEA-2021-3.pdf)—which was also specific to 32-bit integers—we support a full range of 16-128 bit inputs.

Despite our single portable implementation, we reach record-setting speeds on both AVX2, AVX-512 (Intel Skylake) and Arm NEON (Apple M1). For one million 32/64/128-bit numbers, our code running on Apple M1 can produce sorted output at rates of 499/471/466 MB/s. On a 3 GHz Skylake with AVX-512, the speeds are 1123/1119/1120 MB/s. Interestingly, AVX-512 is 1.4-1.6 times as fast as AVX2 - a worthwhile speedup for zero additional effort (Highway checks what instructions are available on the CPU and uses the best available ones). When running on AVX2, we measure 798 MB/s, whereas the prior state of the art optimized for AVX2 only manages 699 MB/s. By comparison, the standard library reaches 58/128/117 MB/s on the same CPU, so we have managed a **9-19x** speedup depending on the type of numbers.

Previously, sorting has been considered expensive. We are interested to see what new applications and capabilities will be unlocked by being able to sort at 1 GB/s on a single CPU core. The Apache2-licensed [source code](https://github.com/google/highway/tree/master/hwy/contrib/sort) is available on Github (feel free to [open an issue](https://github.com/google/highway/issues) if you have any questions or comments) and our [paper](https://arxiv.org/pdf/2205.05982.pdf) offers a detailed explanation and evaluation of the implementation (including the special case for 256 elements).

*By Jan Wassenberg – Brain Computer Architecture Research*
