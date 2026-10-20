---
title: 'Maxwell: A new kind of model that sits between code and hardware'
link: https://heymaxwell.ai/
source: hnrss-org
published: 2026-10-19T13:47:56Z
updated: 2026-10-19T13:47:56Z
first_seen: 2026-10-20T12:56:01.525437407Z
authors:
- laginimaineb
summary: 'Hi HN, I''m Gal from doubleAI. We''ve built Maxwell, a new kind of frontier model we call a Code Execution Model. It sits between your code and the hardware. No humans in the loop! Maxwell makes the code run faster as it runs, without ever changing a single line. Maxwell sees programs from the inside: traces, profiling information, and the data flowing through them. It adapts dynamically to the environment, including the hardware, what else is running, and what the inputs look like. Code execution models change the way we approach coding. Today, Claude/Codex (and humans) keep re-optimising code for every different use-case hardware and data flow (think of LLM inference engines, like vLLM!). Our bet is that this is unnecessary, suboptimal, and that it does not scale. Instead, we think the right answer is to make code a living, thinking, thing. Early access is open at heymaxwell.ai. Happy to answer questions here. Comments URL: https://news.ycombinator.com/item?id=50152466 Points: 100 # Comments: 43'
content: extracted
html: 2026-10-19-maxwell-a-new-kind-of-model-that-sits-between-code-and.html
remote_preview:
  url: https://heymaxwell.ai/og/og-dark.jpg
  alt: Maxwell, the first Code Execution Model, by doubleAI
---

by [doubleAI](https://www.doubleai.com)

Maxwell understands and improves code while it runs, reasoning about hardware and data, not just code. Every improvement is formally proven equivalent to the original code. [Read the manifesto ](https://heymaxwell.ai/manifesto)

- 38× faster data processing pipelines
- 4.25× faster AI inference kernels

## What is a code execution model?

A short animated film, with narration and captions, introducing Maxwell: most code never sees the hardware it runs on or the data it processes. Maxwell watches a program while it runs, rewrites the binary underneath to fit that exact machine and data, and formally proves every improvement equivalent to the original. It runs on CPUs and GPUs, speeding up data pipelines and AI inference kernels.

Featured customer

## LTX-2.5 generates 4K video in half the time

LTX built LTX-2.5 for efficiency from day one, compiled end to end, sequence-parallel across GPUs, decoding on a hand-written attention kernel. A pipeline built and optimized by top human experts. Maxwell made their 4K generation nearly twice as fast.

[(opens ltx.io)](https://ltx.io/)

- 1.87× faster 4K video generation
- 134s per 4K clip on two B200s, down from 250s

[Read the story ](https://heymaxwell.ai/customers/ltx)

## Maxwell’s Demonstration

See Maxwell in action optimizing both GPU-based and CPU-based code. Once Maxwell has enough runtime data, it swaps in formally proven optimizations tailored to that exact machine while the program keeps running.

GPU-based speech-to-text using Whisper

Two terminals run the same Python script on the same RTX 4090: 180 transcriptions with Whisper-small from a cold start. Without Maxwell, on the left, it takes 106.23 seconds. With Maxwell, on the right, it completes in 49.82 seconds. Both produce identical transcripts. The recording plays at 4× speed.

CPU-based graph algorithms

Two terminals run the same Python script on the same machine: NetworkX betweenness centrality, 15 runs. Without Maxwell, on the left, it takes 61.44 seconds. With Maxwell, on the right, it completes in 17.50 seconds. Both print identical results. The recording plays at 4× speed.

## Research behind Maxwell

1. ### [Maxwell approaches Speed of Light on Blackwell](https://www.doubleai.com/research/maxwell-approaches-speed-of-light-on-blackwell)

   After one day of search, Maxwell beats NVIDIA’s PyTorch baselines on 90% of SOL-ExecBench’s Blackwell kernels.

    (opens on doubleai.com)
2. ### [doubleAI’s Maxwell: Surpassing Expert-Written Kernels At Scale](https://www.doubleai.com/research/doubleais-maxwell-surpassing-expert-written-kernels-at-scale)

   Independently generated and verified a hyper-optimized drop-in version of NVIDIA cuGraph, 3.6× faster on average.

    (opens on doubleai.com)
3. ### [Blazing Fast pandas Without Changing a Line](https://www.doubleai.com/research/maxwell-pandas-blogpost)

   Maxwell completely rewrote pandas’ kernels from scratch, specialized for modern hardware.

    (opens on doubleai.com)
