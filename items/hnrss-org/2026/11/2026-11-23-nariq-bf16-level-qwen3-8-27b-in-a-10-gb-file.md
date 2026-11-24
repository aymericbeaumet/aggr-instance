---
title: 'NariQ: BF16-level Qwen3.8-27B in a 10 GB file'
link: https://huggingface.co/nari-labs/Qwen3.8-27B-NariQ-GGUF
source: hnrss-org
published: 2026-11-23T13:42:38Z
updated: 2026-11-23T13:42:38Z
first_seen: 2026-11-24T05:54:25.686228927Z
authors:
- toebee
summary: 'We built NariQ: 3-bit-or-lower quantizations of Qwen3.8-27B that retain near-BF16 quality across agentic tasks, math, multilingual reasoning, and image understanding. To our knowledge, this is the first 3-bit-or-lower quantization that retains BF16 lvl performance on long-horizon agentic tasks. Benchmarks (BF16 / 3-bit / 2.5-bit / 2-bit): - DeepSWE: 39.8 / 41.0 / 41.9 / 28.6 - Terminal-Bench 2.1: 62.6 / 63.8 / 60.2 / 52.4 - AIME 25+26: 97.7 / 98.3 / 96.5 / 86.0 - MSGM: 94.0 / 93.1 / 91.8 / 90.6 - RealWorldQA: 82.4 / 82.4 / 81.3 / 80.9 Decode speed is comparable to other quants on an RTX PRO 6000. Context-length results below (3-bit / 2.5-bit / 2-bit / Unsloth UD-IQ3_XXS): - 4K: 88.9 / 99.2 / 111.7 / 94.7 - 16K: 84.5 / 93.8 / 105.1 / 89.8 - 64K: 70.7 / 77.5 / 85.0 / 74.8 Running these currently require our llama.cpp fork: (https://github.com/nari-labs/nari-llama.cpp) Feedback and questions welcome. We''d love to hear how these perform on your workloads. Comments URL: https://news.ycombinator.com/item?id=50611402 Points: 146 # Comments: 30'
content: extracted
html: 2026-11-23-nariq-bf16-level-qwen3-8-27b-in-a-10-gb-file.html
remote_preview:
  url: https://cdn-thumbnails.huggingface.co/social-thumbnails/models/nari-labs/Qwen3.8-27B-NariQ-GGUF.png
---

## NariQ: Qwen3.8-27B in a 10 GB file, BF16-level on long-horizon agentic tasks

(Nov 20, 2026) We are releasing NariQ v1 GGUFs of Qwen3.8-27B at 3, 2.5 and 2 bits per weight. Run them on our **[llama.cpp fork](https://github.com/nari-labs/nari-llama.cpp) (required)**.

- **The 3-bit and 2.5-bit files match BF16 on long-horizon agent tasks**, math and image understanding. The 2-bit file scores lower.
- **NariQ** is a **trellis-coded** post-training quantization (PTQ), calibrated on real coding-agent traces and stored in a layout that runs on a single GPU.
- More details are in the [tech report](https://arxiv.org/abs/2611.26157).

| Model                      | DeepSWE v1.1 113 tasks × 3, mean pass@1 | Terminal-Bench 2.1 82 tasks × 3 | AIME 25+26 60 × 8 | MGSM 250 × 11 languages | RealWorldQA 765 × 3 |
| -------------------------- | --------------------------------------- | ------------------------------- | ----------------- | ----------------------- | ------------------- |
| BF16 (53.8 GB)             | 39.8 (45.0)                             | 62.6 (154)                      | 97.7 (469)        | **94.0 (2584)**         | **82.4 (1890)**     |
| **NariQ 3-bit** (10.1 GB)  | 41.0 (46.3)                             | **63.8 (157)**                  | **98.3 (472)**    | 93.1 (2561)             | **82.4 (1890)**     |
| **NariQ 2.5-bit** (8.4 GB) | **41.9 (47.3)**                         | 60.2 (148)                      | 96.5 (463)        | 91.8 (2525)             | 81.3 (1866)         |
| **NariQ 2-bit** (6.8 GB)   | 28.6 (32.3)                             | 52.4 (129)                      | 86.0 (413)        | 90.6 (2491)             | 80.9 (1856)         |

Each cell gives a percentage and a count in parentheses. For DeepSWE, the count is the mean number of fully solved tasks per round. Across the same three rounds, the mean share of each task's new tests that passed was 83.1% (BF16), 82.9% (3-bit), 83.2% (2.5-bit) and 77.2% (2-bit). One BF16 verifier timeout in round 3 counts as zero (338 of 339 trials graded). Each NariQ variant had all 339 trials graded. Terminal-Bench counts solved trials, and the remaining columns count correct answers. The agent runs use the Claude Code CLI (2.1.309, thinking on, effort max) driving the local model, with a 262,144-token context and the model's thinking-mode sampling (temperature 1.0, top-p 0.95, top-k 20).

![KL from BF16 on held-out coding-agent traces against file size; lower is better](https://huggingface.co/nari-labs/Qwen3.8-27B-NariQ-GGUF/resolve/main/images/agent-kl.png)

KL divergence from the BF16 model on held-out coding-agent traces, against file size. Every point was measured by us with the same code. Lower is better.

![DeepSWE fully solved tasks against file size, averaged over three rounds for NariQ and our BF16 baseline. Hollow diamonds show Unsloth's reported results.](https://huggingface.co/nari-labs/Qwen3.8-27B-NariQ-GGUF/resolve/main/images/deepswe.png)

DeepSWE fully solved tasks out of 113, against file size. Our NariQ points and solid BF16 line average three rounds with Claude Code. Our GSQ-RCO points use one round. The hollow diamonds show approximate averages from Unsloth's published Dynamic V3.1 chart (mini-swe-agent). Compare each point with the BF16 line for its harness. Higher is better.

![Terminal-Bench 2.1 score against file size; filled circles and solid lines are our harness, hollow diamonds and dashed lines are Unsloth's reported results](https://huggingface.co/nari-labs/Qwen3.8-27B-NariQ-GGUF/resolve/main/images/terminal-bench.png)

Terminal-Bench 2.1 score over 82 tasks × 3 attempts, against file size. Filled circles are our runs (Claude Code); hollow diamonds are Unsloth's reported results (mini-swe-agent), read from its published Dynamic V3.1 chart (hence ≈; its scores are averages over several passes). The two harnesses differ, so compare each point with the BF16 line of its own harness (solid: ours, dashed: Unsloth's). Higher is better.

| File                                 | Bits | Size     | For                                          |
| ------------------------------------ | ---- | -------- | -------------------------------------------- |
| `Qwen3.8-27B-NariQ-3.0bpw.gguf`      | 3.00 | 10.12 GB | the default                                  |
| `Qwen3.8-27B-NariQ-2.5bpw.gguf`      | 2.50 | 8.44 GB  | less memory; BF16-level on agent tasks       |
| `Qwen3.8-27B-NariQ-2.0bpw.gguf`      | 2.00 | 6.75 GB  | the smallest; weaker on math and agent tasks |
| `Qwen3.8-27B-NariQ-mmproj-BF16.gguf` | 16   | 0.93 GB  | the vision projector, for all three          |

## Quickstart

One command on Linux with an NVIDIA GPU and driver 570 or newer (downloads the [fork's](https://github.com/nari-labs/nari-llama.cpp) binaries and the files, checks them, and fits the context to your GPU):

```bash
curl -fsSLO https://github.com/nari-labs/nari-llama.cpp/releases/download/b12431-nariv0/run.sh
bash run.sh 3.0   # or 2.5, 2.0
```

Or download and serve yourself, with `llama-server` from the [fork](https://github.com/nari-labs/nari-llama.cpp):

```bash
hf download nari-labs/Qwen3.8-27B-NariQ-GGUF \
    Qwen3.8-27B-NariQ-3.0bpw.gguf Qwen3.8-27B-NariQ-mmproj-BF16.gguf --local-dir .
llama-server -m Qwen3.8-27B-NariQ-3.0bpw.gguf --mmproj Qwen3.8-27B-NariQ-mmproj-BF16.gguf \
    -ngl 99 -c 131072 --jinja --port 8080 \
    --temp 1.0 --top-p 0.95 --top-k 20 --min-p 0
```

The sampling flags are the thinking-mode settings our scores used. `-c 131072` fits a 24 GB GPU with little to spare; lower it if a display shares the GPU, or let `run.sh` pick the largest context that fits. Use `-c 262144` (the model's full context) on 32 GB or more. GPU memory with the vision projector:

|                                     | 3-bit  | 2.5-bit | 2-bit  |
| ----------------------------------- | ------ | ------- | ------ |
| Context that fits on a 24 GB GPU    | ≈135K  | ≈160K   | ≈184K  |
| GPU memory at the full 262K context | ≈30 GB | ≈28 GB  | ≈27 GB |

Prebuilt binaries, the Docker image, supported GPUs and build steps: the fork's [README](https://github.com/nari-labs/nari-llama.cpp).

**Decode speed** on one NVIDIA RTX PRO 6000 (Blackwell), single session, 512 greedy output tokens, tokens/s:

| Context | NariQ 3.0bpw (10.1 GB) | NariQ 2.5bpw (8.4 GB) | NariQ 2.0bpw (6.8 GB) | UD-IQ3\_XXS (10.9 GB) |
| ------- | ---------------------- | --------------------- | --------------------- | --------------------- |
| 4K      | 88.9                   | 99.2                  | 111.7                 | 94.7                  |
| 16K     | 84.5                   | 93.8                  | 105.1                 | 89.8                  |
| 64K     | 70.7                   | 77.5                  | 85.0                  | 74.8                  |

NariQ on our llama.cpp fork with `-fa on -ctk bf16 -ctv bf16 -t 1 -tb 16 --backend-sampling --samplers temperature` (greedy sampling on the GPU, BF16 KV cache). UD-IQ3\_XXS is Unsloth's file on llama.cpp, measured earlier on the same GPU type.

## Known limits and future work

- **Requires a NariQ-aware runtime**: for now, our [llama.cpp fork](https://github.com/nari-labs/nari-llama.cpp). Stock llama.cpp, Ollama and LM Studio can't load the new tensor types yet; we hope to merge support into upstream llama.cpp. Our internal SGLang plugin and our inference engine built on [Resin](https://github.com/ekzhang/resin) are a work in progress for potential public release.
- **The [llama.cpp fork](https://github.com/nari-labs/nari-llama.cpp) needs more work across hardware.** The current fork is focused on SM120 performance (e.g. RTX PRO 6000). Ampere, Ada and Hopper GPUs and Apple M-series GPUs are slower than comparable GGUFs for now; we would love open-source contributions for speedups, as this has been a solo effort. GPUs older than Ampere fall back to slow CPU execution, and AMD and Intel GPUs are not supported yet. GeForce cards have not been tested yet; tested GPUs are RTX PRO 6000, H100, L4 and A10.
- **Apple M-series (Metal): build from source**; on an M3 Pro (18 GB) decode is about 3.8 tok/s with the 3.0bpw file (4.9 with 2.0bpw), far slower than CUDA. Other Macs are untested.
- **The 2-bit file is weaker overall**; use the 2.5-bit or 3-bit for higher overall performance.
- **The vision tower stays in BF16**; only the language model is quantized. The MTP (multi-token prediction) module is not included yet; it is still to be quantized.
- **KV-cache and activation quantization are next**, so the files fit long contexts on 16 GB consumer GPUs and run faster. Today the KV cache and activations stay in BF16.

## Contact us

For technical consulting, compute sponsorships, or partnership inquiries, please contact [founders@narilabs.com](mailto:founders@narilabs.com).

## License

These files inherit the base model's [Apache-2.0](https://huggingface.co/nari-labs/Qwen3.8-27B-NariQ-GGUF/blob/main/LICENSE) license; see [NOTICE](https://huggingface.co/nari-labs/Qwen3.8-27B-NariQ-GGUF/blob/main/NOTICE).

## Acknowledgements

Thanks to the [Qwen](https://huggingface.co/Qwen) team for the amazing Qwen3.8-27B, and to the [Unsloth](https://unsloth.ai/), [ExLlamaV3](https://github.com/turboderp-org/exllamav3), [GSQ-RCO (IST Austria DASLab)](https://huggingface.co/ISTA-DASLab) and [GGML](https://github.com/ggml-org) teams, whose open-source work this release builds on. NariQ's trellis codes build on QTIP ([Tseng et al., 2024](https://arxiv.org/abs/2406.11235)).

## Citation

If you use NariQ, please cite the technical report:

```bibtex
@techreport{kim2026nariq,
  title       = {NariQ: Sub-3-Bit Quantization for Long-Horizon Agentic Tasks},
  author      = {Doyeob (Toby) Kim},
  institution = {Nari Labs},
  year        = {2026},
  month       = nov,
  eprint      = {2611.26157},
  archivePrefix = {arXiv},
  url         = {https://arxiv.org/abs/2611.26157}
}
```

* * *

Following the widespread community adoption of the Qwen3.5 and Qwen3.6 series, we are pleased to introduce Qwen3.8, the most capable generation in the Qwen open-model family to date.

Built on the architectural foundation of Qwen3.5, Qwen3.8 delivers substantial gains across coding, professional work, research, and long-horizon agentic tasks. Qwen3.8-27B brings these advances to a compact, deployment-friendly dense model: a native vision-language model that understands images and videos, with flexible thinking control, designed to carry complex, multi-step tasks through to completion with greater reliability.

## Qwen3.8 Highlights

Qwen3.8-27B features the following enhancements:

- **Core Capabilities**: Comprehensive improvements across coding, professional work, research, and long-horizon agentic tasks.
- **Agent Execution**: Stronger autonomous planning and better handling of environment feedback, leading to more reliable end-to-end task completion.
- **Downstream Compatibility**: Broader support for popular harnesses and development tools, making it easier to integrate into your existing stack.
- **Flexible Thinking Control**: Thinking mode is on by default and can be disabled per request; reasoning depth can be tuned with `reasoning_effort`, and reasoning context from historical messages is retained via `preserve_thinking`.
- **Vision-Language Understanding**: Native support for image and video understanding, from STEM diagrams and documents to hour-scale videos.

## Model Overview

- Type: Causal Language Model with Vision Encoder
- Training Stage: Pre-training & Post-training
- Language Model
  - Number of Parameters: 27B
  - Hidden Dimension: 5120
  - Token Embedding: 248,320 (Padded)
  - Number of Layers: 64
  - Hidden Layout: 16 × (3 × (Gated DeltaNet → FFN) → 1 × (Gated Attention → FFN))
  - Gated DeltaNet:
    - Number of Linear Attention Heads: 48 for V and 16 for QK
    - Head Dimension: 128
  - Gated Attention:
    - Number of Attention Heads: 24 for Q and 4 for KV
    - Head Dimension: 256
    - Rotary Position Embedding Dimension: 64
  - Feed Forward Network:
    - Intermediate Dimension: 17,408
  - LM Output: 248,320 (Padded)
  - MTP (Multi-Token Prediction): trained with multiple steps
- Context Length: 262,144 natively and extensible up to 1,000,000 tokens.

## Best Practices

To achieve optimal performance, we recommend the following settings:

1. **Sampling Parameters**: We suggest using the following sets of sampling parameters:

   - Thinking Mode: `temperature=1.0`, `top_p=0.95`, `top_k=20`, `min_p=0.0`, `presence_penalty=0.0`, `repetition_penalty=1.0`
   - Instruct (or non-thinking) mode: `temperature=0.7`, `top_p=0.80`, `top_k=20`, `min_p=0.0`, `presence_penalty=1.5`, `repetition_penalty=1.0`

   For supported frameworks, you can adjust the `presence_penalty` parameter between 0 and 2 to reduce endless repetition. However, using a higher value may occasionally result in language mixing and a slight decrease in model performance.

2. **Adequate Output Length**: To optimize performance on agentic tasks, we recommend allocating sufficient output length to allow the model to generate detailed and comprehensive responses. For frameworks that support separate token limits for internal reasoning and final outputs, we suggest the following configuration within the 1M context length:

   - Reasoning Content: Set the maximum output length to 262,144 tokens.
   - Final Response: Set the maximum output length to 131,072 tokens.

   These settings provide the necessary capacity for complex reasoning while ensuring ample space for high-quality final deliverables.

3. **Processing Ultra-Long Texts**: Qwen3.8-27B natively supports context lengths of up to 262,144 tokens. For long-horizon tasks where the total length (including both input and output) exceeds this limit, we recommend using RoPE scaling techniques to handle long texts effectively, e.g., YaRN.

4. **Long Video Understanding**: To optimize inference efficiency for plain text and images, the `size` parameter in the released `video_preprocessor_config.json` is conservatively configured. It is recommended to set the `longest_edge` parameter in the video\_preprocessor\_config file to 469,762,048 (corresponding to 224k video tokens) to enable higher frame-rate sampling for hour-scale videos and thereby achieve superior performance. For example,

   ```json
   {"longest_edge": 469762048, "shortest_edge": 4096}
   ```

   Alternatively, override the default values via engine startup parameters. For implementation details, refer to: [vLLM](https://github.com/vllm-project/vllm/pull/34330) / [SGLang](https://github.com/sgl-project/sglang/pull/18467).

## Citation

```bibtex
@misc{qwen38,
    title = {{Qwen3.8-Max}: A New Bar for Coding and Cowork},
    url = {https://qwen.ai/blog?id=qwen3.8},
    author = {{Qwen Team}},
    month = {August},
    year = {2026}
}
```
