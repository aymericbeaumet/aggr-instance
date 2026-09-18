---
title: 'Bonsai 2 27B: Near-Lossless Compression in a 9x Smaller Footprint'
link: https://prismml.com/news/bonsai-2-27b
source: hnrss-org-frontpage
published: 2026-09-17T21:13:31Z
updated: 2026-09-17T21:13:31Z
first_seen: 2026-09-18T01:14:43.309993603Z
authors:
- JonSchneider
summary: 'Article URL: https://prismml.com/news/bonsai-2-27b Comments URL: https://news.ycombinator.com/item?id=49746618 Points: 191 # Comments: 63'
content: extracted
html: 2026-09-17-bonsai-2-27b-near-lossless-compression-in-a-9x-smaller.html
preview:
  file: 2026-09-17-bonsai-2-27b-near-lossless-compression-in-a-9x-smaller.preview-8a972ac49a07.webp
  width: 256
  height: 134
  color: '#d0c9c7'
images:
- source: https://cdn.prod.website-files.com/699604cc2b9dd89bdbda0608/6a6d0e0bc0d5f9a97a5164c0_69cc089669ab0df26e921aac_prism-og-img%2520(2).png
  original:
    file: 2026-09-17-bonsai-2-27b-near-lossless-compression-in-a-9x-smaller.image-44eb64bd2ab4.png
    width: 2400
    height: 1260
  variants:
  - file: 2026-09-17-bonsai-2-27b-near-lossless-compression-in-a-9x-smaller.image-07122b2017f5.webp
    width: 320
    height: 168
  - file: 2026-09-17-bonsai-2-27b-near-lossless-compression-in-a-9x-smaller.image-387db369b6ed.webp
    width: 640
    height: 336
  - file: 2026-09-17-bonsai-2-27b-near-lossless-compression-in-a-9x-smaller.image-c7e07f6071c1.webp
    width: 960
    height: 504
  - file: 2026-09-17-bonsai-2-27b-near-lossless-compression-in-a-9x-smaller.image-15c0faa2dc13.webp
    width: 1280
    height: 672
  - file: 2026-09-17-bonsai-2-27b-near-lossless-compression-in-a-9x-smaller.image-992029c59e9a.webp
    width: 2400
    height: 1260
  color: '#e7e3de'
- source: https://cdn.prod.website-files.com/699604cc2b9dd89bdbda0608/6aac504ec0cef3807f35bcba_2%2027B%20ID.png
  original:
    file: 2026-09-17-bonsai-2-27b-near-lossless-compression-in-a-9x-smaller.image-22a994e31de0.png
    width: 3396
    height: 1290
  color: '#e6e3de'
---

Two months ago, we released our first Bonsai 27B models and showed that a 27B-class multimodal model could be compressed enough to run efficiently on a local device. Today, we’re releasing Ternary Bonsai 2 27B, our most capable model yet.

Based on Qwen3.8 27B, Ternary Bonsai 2 27B brings stronger reasoning, coding, vision, and agentic capability to the Bonsai series while preserving the deployment profile that defines it: a dramatically smaller memory footprint, high local throughput, and better energy efficiency.

Ternary Bonsai 2 27B uses ternary {−1, 0, +1} weights with FP16 group-wise scaling, for **1.76 effective bits per weight** and a total model footprint of **5.9GB**. The low-bit representation is applied end to end across the language model. It supports a **262K-token context window**, multimodal text-and-image input, and is released under the Apache 2.0 license.

Against its full-precision counterpart, Ternary Bonsai 2 27B is **more than 9x smaller** while retaining **98.2% of aggregate benchmark performance**. At this level of retention, compression becomes a deployment unlock: nearly the same capability, in a footprint that can run in far more places.

## **What changed from the first Bonsai 27B release**

Our first Bonsai 27B release was an important milestone, offering a practical way to run 27B-class intelligence on local devices. Bonsai 2 27B focuses on the next step: improving the model quality and runtime performance needed for real-world local applications. Compared with the previous Bonsai 27B generation, Bonsai 2 27B brings:

- a stronger base model, Qwen3.8 27B
- higher aggregate capability retention of 98.2% against the full-precision model
- improved reasoning, coding, vision, and long-horizon agentic performance

## **Higher capability at the same deployment point**

Across a benchmark suite spanning reasoning, math, coding, instruction following, vision, and agentic tool use, Ternary Bonsai 2 27B scores **83.9**, retaining **98.2%** of Qwen3.8 27B’s aggregate performance.

| Capability                                                           | Ternary Bonsai 2\ 27B | Qwen3.8\ 27B | Qwen3.6\ 27B |
| -------------------------------------------------------------------- | --------------------- | ------------ | ------------ |
| Agentic & Tool Calling τ²-bench, BFCLv3                              | 77.57                 | 79.74        | 80.05        |
| Coding HumanEval+, LiveCodeBench v6, MBPP+, BigCodeBench             | 81.58                 | 82.17        | 82.57        |
| Instruction Following IFBench, IFEval                                | 82.66                 | 81.25        | 74.53        |
| Knowledge & Reasoning MMLU-Redux, GPQA Diamond, AA-LCR               | 83.95                 | 86.66        | 84.71        |
| Math AIME 2026, AIME 2025, GSM8K, MATH-500                           | 96.57                 | 97.06        | 94.64        |
| Vision CharXiv, A-OKVQA, OmniDocBench v1.6, RealWorldQA, OCRBench v2 | 78.59                 | 81.64        | 79.82        |
| Overall                                                              | 83.9                  | 85.4         | 83.6         |

> ‍***Figure I:*** *Benchmark scores of Ternary Bonsai 2 27B (thinking mode) compared with the full-precision Qwen3.8 27B and Qwen3.6 27B baselines. Full per-benchmark results are in the* [*whitepaper*](https://github.com/PrismML-Eng/Bonsai-demo/blob/main/bonsai-2-27b-whitepaper.pdf)*.*‍

The key result is not only the aggregate score, but where the capability is retained. Coding agents, tool-use systems, multimodal workflows, and long-horizon tasks are particularly sensitive to model degradation because small errors can compound over many steps. Bonsai 2 27B preserves much of the full-precision model’s performance in exactly these areas while operating at a fraction of the memory footprint.

Compared with the full-precision model and other low-bit alternatives, Bonsai 2 27B stands out as an outlier on intelligence density. Many low-bit alternatives become deployable only by giving up meaningful capability in coding, vision, or agentic tool use. Bonsai 2 27B pushes the frontier toward both higher capability and lower memory usage.

![](https://cdn.prod.website-files.com/699604cc2b9dd89bdbda0608/6aac504ec0cef3807f35bcba_2%2027B%20ID.png)

**Figure II:** Intelligence density (per GB) of Ternary Bonsai 2 27B compared to other models in the same parameter class.

> Demo I: Coding agents with Cline, powered by Ternary Bonsai 2 27B on NVIDIA GeForce RTX 5090.

With Bonsai 2 27B, local models can start to take on real knowledge work: coding-agent loops, computer-use workflows, private document analysis, multimodal debugging, and hybrid orchestration where local models handle sensitive or high-frequency tasks while escalating selectively to the cloud.

## **Throughput and energy efficiency**

Ternary Bonsai 2 27B reaches up to **143 tokens/second** on NVIDIA GeForce RTX 5090 and **46.8 tokens/second** on M5 Max. On an RTX 4090, Ternary Bonsai 2 27B consumes just 0.714 mWh/token, making it 40% more energy-efficient than an 8B model running in full-precision.

For coding assistants, higher throughput means faster edit-debug loops. For multimodal agents, it means quicker iterations over screenshots, documents, and tool calls. For private local workflows, better energy efficiency means more useful inference on the same device, longer battery life, and a more realistic path to assistants that can stay available in the background without constantly calling the cloud.

## **Why this release matters**

Compared to Ternary Bonsai 27B, the new Ternary Bonsai 2 27B has closed the retention gap between the full precision model from 95% to over 98%. This is a significant improvement that makes the current release practically “lossless”. It further cements the notion that low-bit models can be the best way to deploy AI.

That has implications well beyond local inference. Low-bit models can change the economics and architecture of AI systems across devices, workstations, and datacenters: fitting larger models into the same memory envelope, serving more users on the same hardware, reducing energy per inference, and enabling hybrid systems that dynamically decide what should run locally and what should run in the cloud.

The question will increasingly be not just how capable a model is, but how much useful intelligence can be delivered within a given memory, compute, and power budget. If capability can continue to scale while those requirements fall dramatically, the deployment envelope for future models expands across the stack: from personal devices to large-scale datacenters.

## **Platform Coverage**

Bonsai 2 27B runs on NVIDIA GPUs via CUDA and on Apple devices (Mac, iPhone, iPad) via MLX, through custom low-bit kernels. Model weights are available today under the Apache 2.0 License.

Full technical details of our compression, evaluation, and benchmarking processes are available in our [whitepaper](https://github.com/PrismML-Eng/Bonsai-demo/blob/main/bonsai-2-27b-whitepaper.pdf).

## **Work with Us**

We work with teams to tailor Bonsai models to their applications, from post-training on domain-specific data to optimizing inference for target hardware. If you’re building AI products with tight memory, latency, or power requirements, we’d love to explore how Bonsai can help. Reach out at [contact@prismml.com](mailto:contact@prismml.com).

## **Join Us**

PrismML emerged from a team of Caltech researchers and was founded with support from Khosla Ventures, Cerberus, and Google, with continuing support from Samsung. We've spent years tackling one of the field's hardest problems: compressing neural networks without sacrificing their reasoning ability.

If you want to help build the next generation of state-of-the-art AI, we'd love to hear from you. Check out our [careers page](https://prismml.com/careers).
