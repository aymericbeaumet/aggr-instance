---
title: 'LFM2.5-24B-A2B: Built for fast on-device agents'
link: https://www.liquid.ai/blog/lfm2-5-24b-a2b
source: hnrss-org
published: 2026-10-14T23:59:37Z
updated: 2026-10-14T23:59:37Z
first_seen: 2026-10-15T04:26:51.711869061Z
authors:
- gmays
content: extracted
html: 2026-10-14-lfm2-5-24b-a2b-built-for-fast-on-device-agents.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50097369
  num_comments: 30
  points: 113
---

Today, we release **LFM2.5-24B-A2B**, a 24B-parameter reasoning model with just 2B active parameters, built for fast, private agents on-device. It is the largest and final model in the LFM2.5 family, which now spans two orders of magnitude: from [LFM2.5-230M](https://www.liquid.ai/blog/lfm2-5-230m) to [LFM2.5-24B-A2B](https://huggingface.co/LiquidAI/LFM2.5-24B-A2B).

LFM2.5-24B-A2B is competitive with Qwen3.6-35B-A3B, a model with ~50% more total and active parameters, on five of eight agentic and tool-use benchmarks, scoring 48.09 on LiveClaw versus 38.19 and 27.69 on AutomationBench versus 8.72.  Because only 2B parameters are active per token, it runs up to 1.7× faster than comparable MoEs on-device, with 184 tokens/s on an Apple M5 Max and up to 21K output tokens/s on a single NVIDIA B300 GPU.

The base (LFM2.5-24B-A2B-Base) and post-trained (LFM2.5-24B-A2B) models are open-weight and available today on [Hugging Face](https://huggingface.co/LiquidAI/LFM2.5-24B-A2B) and [our Playground](https://playground.liquid.ai/chat?model=LFM2.5-24B-A2B). Check out our [docs](https://docs.liquid.ai/lfm/models/lfm25-24b-a2b) on how to run and fine-tune it locally.

## What changed since LFM2-24B-A2B

LFM2.5-24B-A2B builds on our previous [LFM2-24B-A2B](https://www.liquid.ai/blog/lfm2-24b-a2b) release, with an expanded vocabulary and context window, scaled-up pretraining, and large-scale reinforcement learning. Unlike its predecessor, LFM2.5-24B-A2B is a reasoning-only model that produces an explicit chain of thought before its final answer. We adopted this strategy to leverage its high decode speed and improve answer quality.

**Architecture.** It keeps the 40 layers, 64 experts per MoE block with top-4 routing, and the same hybrid of gated short convolutions and GQA. The two architectural changes are the context window extension from 32,768 to 131,072 tokens, and the [in-place tokenizer expansion](https://www.liquid.ai/blog/tokenizer-expansion), which doubles the vocabulary from 65K to 128K tokens for better non-Latin tokenization.

**Pre-training** was scaled up from 17T tokens to 33T tokens.

**Mid-training** has two phases. The first runs 2T tokens at 32K context on curated, higher-quality data weighted toward reasoning, math, code, instruction following, and multilingual text. The second runs 300B tokens at 131K context, weighted toward long context data. Tool calls and multi-turn trajectories appear throughout both phases to prepare the model for agentic workloads.

**Post-training** turns LFM2.5-24B-A2B-Base into the agentic LFM2.5-24B-A2B in a five-stage pipeline as illustrated below: (1) Supervised Fine-Tuning (SFT), (2) Teacher Specialization, (3) non-agentic and (4) agentic Multi-Domain On-Policy Distillation (MOPD), and (5) Agentic Reinforcement Learning (Agentic RL), training the model inside agent harnesses, such as [OpenClaw](https://openclaw.ai/), [Hermes Agent](https://hermes-agent.nousresearch.com/), [Pi](https://pi.dev/), and [OpenCode](https://opencode.ai/).

Figure 2: LFM2.5-24B-A2B Post-training recipe

During teacher specialization, LFM2.5-24B-A2B exhibited well-known reward-hacking behaviors: searching for the final answer in a repository's git history or on platforms like GitHub, PyPI, and Hugging Face. More surprisingly, it even figured out it was in a sandbox, as shown in the examples below.

This behavior looks emergent but might be explained by training data containing similar patterns. We tightened our sandboxes in response, and all reported results below come from sealed evaluation environments.

## Benchmarks

We evaluated LFM2.5-24B-A2B across 16 benchmarks covering tool use and agentic workflows, knowledge, instruction following, math, and coding. Despite being the smallest model in total and active number of parameters, it is competitive with, and often outperforms, larger MoEs with ~50% more total and active parameters, such as Qwen3.6-35B-A3B.

|                               |                    |                     |                        |                                      |
| ----------------------------- | ------------------ | ------------------- | ---------------------- | ------------------------------------ |
| **Benchmark**                 | **LFM2.5-24B-A2B** | **Qwen3.6-35B-A3B** | **Gemma-4-26B-A4B-it** | **Nemotron-3.5-Lightning (30B-A3B)** |
| AutomationBench               | **27.69**          | 8.72                | 5.46                   | 2.61                                 |
| BFCLv4                        | **65.01**          | 65.00               | 57.09                  | 53.78                                |
| ToolSandbox                   | **80.71**          | 75.02               | 72.09                  | 75.13                                |
| LiveClaw                      | **48.09**          | 38.19               | 41.25                  | 37.72                                |
| PinchBench                    | 83.18              | **89.93**           | 77.12                  | 84.20                                |
| τ³-Bench Banking              | **8.04**           | 7.63                | 4.54                   | 7.01                                 |
| WebArena Verified             | 26.75              | **37.62**           | 25.62                  | 28.69                                |
| GPQA Diamond                  | 74.42              | **83.69**           | 74.90                  | 72.42                                |
| AA-Omniscience                | -24.90             | -24.35              | -59.39                 | **-3.58**                            |
| HLE                           | 7.40               | 7.69                | **8.57**               | 5.65                                 |
| IFEval                        | **96.75**          | 93.06               | 92.00                  | 94.19                                |
| IFBench                       | **74.69**          | 64.21               | 47.44                  | 71.67                                |
| Multi-IF                      | **84.38**          | 81.53               | 82.19                  | 82.34                                |
| MATH-500                      | 96.04              | **96.73**           | 95.66                  | 96.40                                |
| SWE-Bench Verified (OpenCode) | 51.00              | **66.20**           | 25.80                  | 53.40                                |
| SciCode                       | 32.71              | **37.35**           | 35.42                  | 27.78                                |

Table 1: LFM2.5-24B-A2B benchmark performance\*.

LFM2.5-24B-A2B leads or matches the competition on five of the eight tool-use and agentic benchmarks (AutomationBench, BFCLv4, ToolSandbox, LiveClaw, and τ³-Bench Banking), with its widest margin on AutomationBench, where it scores more than three times the next-best model. It also leads on all three instruction-following benchmarks. Larger models keep an edge on knowledge-heavy benchmarks like GPQA Diamond, HLE, and AA-Omniscience, and on coding benchmarks like SWE-Bench Verified (OpenCode) and SciCode.

The results make LFM2.5-24B-A2B an ideal foundation for agents that need to reason, call tools, and run reliably on your device. For knowledge-heavy workloads, we recommend pairing LFM2.5-24B-A2B with retrieval or web search, which plays to its tool-calling strength.

## Fast inference everywhere

LFM2.5-24B-A2B ships with day-one support across the inference ecosystem:

- **llama.cpp** — GGUF checkpoints for efficient edge inference
- **MLX** — Optimized inference for Apple Silicon
- **vLLM** — GPU-accelerated serving for production throughput
- **SGLang** — High-performance GPU serving framework
- **ONNX** — Cross-platform inference across diverse accelerators

All runs were collected on [Pipette](https://pipette.liquid.ai/), the same benchmarking infrastructure behind Liquid AI's public device performance data.

**Edge inference.** Due to the efficient LFM2 architecture, LFM2.5-24B-A2B is the fastest of the compared MoEs at reading in prompts and generating answers. It decodes 184 tokens/s on an Apple M5 Max and 114 tokens/s on an AMD Ryzen AI Max+ 395, up to 1.7x faster than the next-best model, while using the least memory.

**GPU inference.** To demonstrate the competitiveness of LFM architecture we compare the GPU inference throughput to other similarly sized MoE models. We measure GPU throughput (total output tokens / wall time) on a single NVIDIA B300 GPU using a sustained-load setting: at each concurrency level, we continuously maintain the target number of in-flight requests, replacing each completed request immediately.

We benchmark each model with SGLang 0.5.22, 1,024 input tokens, up to 256 output tokens, in BF16. LFM2.5-24B-A2B is the fastest model in its size class, reaching over 21K output tokens per second at high concurrency.

## Run local agents with LFM2.5-24B-A2B

LFM2.5-24B-A2B’s size, speed, and capabilities make it a great choice for agents that run continuously on your own hardware. In the demo below, we run it inside Hermes Agent on an M5 Max MacBook Pro, where it plans, calls tools, and works through a real task entirely on-device, without any cloud API calls.

Video 1: Local Hermes Agent with LFM2.5-24B-A2B creating a morning briefing about trending models on Hugging Face and responding to follow up questions.

The point of the demo is not the individual tools. It is that the tool-dispatch loop feels interactive on consumer hardware: ask, propose, confirm, run, repeat, all in well under a second per dispatch, with full audit trails and your data never leaving the device. Check out [our local agent guide](https://docs.liquid.ai/examples/agent-harnesses) for how to serve the model locally and connect it with the agent harness of your choice.

## Get Started

Start building today with LFM2.5-24B-A2B, available on [Hugging Face](https://huggingface.co/LiquidAI/LFM2.5-24B-A2B).

For API inference, you can also sign up for [Liquid Console](https://console.liquid.ai/) where you'll receive $2 in credit to get started. The model is priced at $.10/1M input tokens (at $.05 for cached input) and $1.00/1M output tokens.

With LFM2.5, we're delivering on our vision of AI that runs anywhere. These models are:

- **Open-weight** — Download, fine-tune, and deploy. See our [model license](https://www.liquid.ai/lfm-license).
- **Fast from day one** — Native support for llama.cpp, MLX, and vLLM across Apple, AMD, Qualcomm, and Nvidia hardware
- **A complete family** — From base models for customization to specialized audio and vision variants, one architecture covers diverse use cases

We can't wait to see what you build.

[![hugging face logo liquid AI](https://www.liquid.ai/_next/image?url=https%3A%2F%2Faypchzzf9pftwuto.public.blob.vercel-storage.com%2Fhugging-face-logo-he4Sh4nCz7LFuUCKOPouBvPe9edHKx&w=48&q=75)Download on Hugging Face](https://huggingface.co/LiquidAI/LFM2.5-24B-A2B)\
[![Liquid AI logo](https://www.liquid.ai/_next/image?url=https%3A%2F%2Faypchzzf9pftwuto.public.blob.vercel-storage.com%2Fliquid-logo-xIjdtFz2499igEZp35jOGE8qF8wpBA&w=48&q=75)Run your harness of choice locally with LFM2.5-24B-A2B](https://docs.liquid.ai/examples/agent-harnesses)\
[![Liquid AI logo](https://www.liquid.ai/_next/image?url=https%3A%2F%2Faypchzzf9pftwuto.public.blob.vercel-storage.com%2Fliquid-logo-xIjdtFz2499igEZp35jOGE8qF8wpBA&w=48&q=75)Integrate with our API](https://console.liquid.ai/)

## Citation

For citations, please use the following reference or BibTeX:

Liquid AI, "LFM2.5-24B-A2B: Built for Fast On-Device Agents", Liquid AI Blog, Oct 2026.

\*All the models were evaluated with vLLM and the following generation parameters:

- **AutomationBench:** temperature = 0.6, top\_p = 0.95, no max output tokens (bounded by the model's context window).
- **BFCLv4**: temperature = 0.001, max output tokens = 4096.
- **ToolSandbox**: temperature = 0, max output tokens = 1024; user simulator: GPT-4o-2024-08-06, temperature = 0, max output tokens = 1024.
- **LiveClaw**: temperature = 1.0 (vLLM default; the agent sets no temperature), max output tokens = 8192.
- **τ³-Bench Banking**: official user model (GPT-5.2, low reasoning effort) with default BM25 retrieval, temperature = 0, max output tokens = 16384 per turn.
- **WebArena:** temperature = 0.6, top\_p=0.95, max\_turns=40 (no of browser actions allowed), no max output tokens (bounded by the model's context window))
- **Other evals**: temperature = 0.6, max output tokens = 32768.
