---
title: Introducing the llama.cpp benchmark dashboard for AMD Ryzen and Radeon
link: https://huggingface.co/blog/ggml-org/llamacpp-amd-bench
source: huggingface-co
published: 2026-11-26T11:37:17Z
updated: 2026-11-26T11:37:17Z
first_seen: 2026-11-26T12:13:26.788658858Z
content: extracted
html: 2026-11-26-introducing-the-llama-cpp-benchmark-dashboard-for-amd-ryzen.html
remote_preview:
  url: https://cdn-uploads.huggingface.co/production/uploads/67289c93ff90038d9a6e5675/tbIvyMSISXCm64WYP3atl.png
---

The [llama.cpp × AMD Benchmark](https://ggml-org-llamacpp-amd-bench.static.hf.space/index.html) dashboard is available on Hugging Face. It lets you track prompt processing and text generation performance across llama.cpp builds, models, and quantizations, so you can explore how local inference performs on AMD Ryzen and Radeon hardware.

Local inference performance changes as models, kernels, and builds evolve. We want those changes to be easy to inspect for any AMD developer: pick a hardware, a model and a build, compare quantizations, and follow performance over time. The benchmark workflow and underlying results are public, so you can also inspect how we benchmark inference performance.

## Quick start

You can choose **Hardware**, **Model**, and **llama.cpp build** in the Configuration sidebar. The current hardware choices are Radeon 8060S (gfx1151) and Radeon AI Pro R9700S (gfx1201).

1. In the **Overview** tab, compare quantizations for one hardware, model, and build. The two charts show PP512 (512 input tokens, no generation) and TG128 (128 generated tokens), both starting at context depth zero.
2. In the **Build history** tab, track PP512 and TG128 across all recorded builds for the selected hardware, model, and quantization.
3. In the **Context** **scaling** tab, see how token-generation throughput changes as the existing context grows, for the selected build, hardware, model, and quantization.

## What the numbers mean

The dashboard shows two separate tests, both measured in tokens per second:

| Test      | Workload                 | What it helps you assess                 |
| --------- | ------------------------ | ---------------------------------------- |
| **PP512** | Process 512 input tokens | How quickly the model processes a prompt |
| **TG128** | Generate 128 tokens      | How quickly the model produces output    |

Both tests start at context depth zero (`n_depth=0`). PP512 measures processing 512 input tokens without generation; TG128 measures generating 128 tokens without a pre-existing context. The plotted value is the mean throughput (`avg_ts`). These are useful reference workloads, while the Context scaling tab lets you explore token generation throughput as the context grows.

## How we run the benchmarks

We benchmark the prebuilt binaries distributed through [llama-install.sh](https://github.com/ggml-org/llama-install.sh), the build and installation project behind [llama.app](https://llama.app/). The public [gfx1151](https://github.com/ggml-org/llama-install.sh/blob/master/.github/workflows/bench-gfx1151.yml) and [gfx1201](https://github.com/ggml-org/llama-install.sh/blob/master/.github/workflows/bench-gfx1201.yml) workflows are scheduled weekly and run the standard benchmarks using the [llama-bench tool](https://github.com/ggml-org/llama.cpp/tree/master/tools/llama-bench) with Flash Attention on and off, with 10 repetitions per configuration.

The model set evolves and currently includes Inkling 2 Mini, Qwen, Gemma, gpt-oss, and LFM models, covering both dense and mixture-of-experts architectures. Available GGUF formats include `Q4_K_M`, `Q8_0`, other quantizations, and higher-precision formats such as `BF16` and `F16`.

## Inspect and reuse the results

The explorer reads directly from the public [ggml-org/install.sh-benches-data Storage Bucket](https://huggingface.co/buckets/ggml-org/install.sh-benches-data) on Hugging Face. The **data source** link beside the chart title opens the folder for the selected build, hardware, and model.

Standard results use `<build>/<hardware>/<model>/<quant>.jsonl`. Workload sweeps use the same folder `<quant>-tg.jsonl` filename.

Use them to investigate a result, make your own charts, or compare with a local run.

## Join the conversation

Thanks to the AMD team for providing test infrastructure and helping validate the setup! Try the [dashboard](https://ggml-org-llamacpp-amd-bench.static.hf.space/index.html), inspect the [data](https://huggingface.co/buckets/ggml-org/install.sh-benches-data), and let us know which models and workloads you would like to see next.
