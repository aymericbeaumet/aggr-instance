---
title: Ternary Bonsai 2.5 27B
link: https://prismml.com/news/bonsai-2-5-27b
source: hnrss-org
published: 2026-11-24T20:08:03Z
updated: 2026-11-24T20:08:03Z
first_seen: 2026-11-25T01:32:55.949141972Z
authors:
- b0m
content: extracted
html: 2026-11-24-ternary-bonsai-2-5-27b.html
remote_preview:
  url: https://cdn.prod.website-files.com/699604cc2b9dd89bdbda0608/6a6d0e0bc0d5f9a97a5164c0_69cc089669ab0df26e921aac_prism-og-img%2520(2).png
---

Today, we’re introducing Ternary Bonsai 2.5 27B, an upgrade to Ternary Bonsai 2 27B with substantially stronger agentic coding, multi-turn tool use, and multi-step task execution. Based on Qwen3.8 27B, it retains the previous release’s 5.9 GB model footprint while significantly improving the capabilities developers need to build agents that work with private files and tools on-device. The result is by far the strongest AI model of its memory footprint for agentic workloads.

With [Ternary Bonsai 2 27B](https://prismml.com/news/bonsai-2-27b), we brought stronger reasoning, visual understanding, and tool use to local devices. Since then, we’ve focused on making the model more effective across long sequences of tool calls: interpreting results, adjusting when a step fails, and maintaining the task’s objective through the whole interaction. This release also continues our work to make Bonsai more practical by addressing excessive thinking and repetitive loops in the previous model. Compared with Ternary Bonsai 2, Ternary Bonsai 2.5 completes tasks in fewer turns, uses fewer tokens, and delivers results faster and more accurately.

Agentic workflows are particularly sensitive to model degradation: small errors can compound across successive tool calls and derail an entire task. Ternary Bonsai 2.5 significantly narrows the performance gap to the full-precision base model while retaining its compact footprint. Preserving this behavior through longer tasks is an important milestone in improving the intelligence density of models.

We evaluated Ternary Bonsai 2.5 27B against both Ternary Bonsai 2 27B and the full-precision Qwen 3.8 27B base model, as reported in the full [whitepaper](https://github.com/PrismML-Eng/Bonsai-demo/blob/main/bonsai-2-5-27b-whitepaper.pdf). We see marked improvements in agentic execution, coding, and multi-turn tool use, as shown in the table below.

| Benchmark                     | Qwen3.8 27B | Bonsai 2 27B | Bonsai 2.5 27B |
| ----------------------------- | ----------- | ------------ | -------------- |
| SWE-bench Verified            | 80.6        | 60.8         | 75.6           |
| DeepSWE v1.1 (mini-swe-agent) | 30.1        | 0            | 27             |
| Terminal-Bench 2.1            | 70.8        | 52.8         | 60.1           |
| AppWorld                      | 93.2        | 51.2         | 90.5           |
| τ³-Bench                      | 72.8        | 69.1         | 73.9           |
| BigCodeBench                  | 61.5        | 57.2         | 60.3           |
| LiveCodeBench v6              | 87.6        | 83.8         | 83.0           |

On a single NVIDIA H100, Ternary Bonsai 2.5 27B achieves a generation speed of 291 tokens/s with MTP and processes a 2,000-token prompt at 3,200 tokens/s. Even on a consumer-grade RTX 4090, it delivers 204 tokens/s with MTP and a prompt processing speed of approximately 3,060 tokens/s. The demos below follow complete local workflows, from the initial request through intermediate decisions to the final result.

[Reproducing and fixing a pytest regression with Bonsai 2.5 27B](https://player.vimeo.com/video/1249622614?autopause=0&amp;player_id=0&amp;app_id=58479)

> Demo I: Reproducing and fixing a pytest regression with Bonsai 2.5 27B

For products, this creates room to build assistance directly into the applications where work already happens: a coding assistant working through issues in a private repository, a document application that prepares reviewable summaries, or a desktop assistant that handles recurring analysis. Local deployment lets those products process sensitive material on the user’s device. Ternary Bonsai 2.5 27B strengthens the model capabilities behind that experience while keeping the compact footprint that makes it practical.

## **Platform Coverage**

Ternary Bonsai 2.5 27B runs on NVIDIA and Intel GPUs via llama.cpp and on Apple devices (Mac, iPhone, iPad) via MLX.

Model weights are available today under the Apache 2.0 License.

Full technical details of our compression, evaluation, and benchmarking processes are available in our [whitepaper](https://github.com/PrismML-Eng/Bonsai-demo/blob/main/bonsai-2-5-27b-whitepaper.pdf).

## **Work with Us**

We work with teams to tailor Bonsai models to their applications, from post-training on domain-specific data to optimizing inference for target hardware. If you’re building AI products with tight memory, latency, or power requirements, we’d love to explore how Bonsai can help. Reach out at [sales@prismml.com](mailto:sales@prismml.com).

## **Join Us**

PrismML emerged from a team of Caltech researchers and was founded with support from Khosla Ventures, Cerberus, and Google, with continuing support from Samsung. We've spent years tackling one of the field's hardest problems: compressing neural networks without sacrificing their reasoning ability.

If you want to help build the next generation of state-of-the-art AI, we'd love to hear from you. Check out our [careers](https://prismml.com/careers) page.
