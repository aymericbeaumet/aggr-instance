---
title: Accelerating vision-language models with LFM2.5-VL-DSpark
link: https://huggingface.co/blog/LiquidAI/lfm2-5-vl-dspark
source: huggingface-co-blog
published: 2026-09-24T14:08:57Z
updated: 2026-09-24T14:08:57Z
first_seen: 2026-09-24T15:55:53.597467136Z
content: extracted
html: 2026-09-24-accelerating-vision-language-models-with-lfm2-5-vl-dspark.html
preview:
  file: 2026-09-24-accelerating-vision-language-models-with-lfm2-5-vl-dspark.preview-4f425f40b218.webp
  width: 256
  height: 144
  color: '#ecebec'
images:
- source: https://cdn-uploads.huggingface.co/production/uploads/644249b08443bce4c9890a0f/WgfT8N8Xyb6lBxSif7XLO.gif
  original:
    file: 2026-09-24-accelerating-vision-language-models-with-lfm2-5-vl-dspark.image-fda667a3e745.gif
    width: 1280
    height: 720
  color: '#fafafa'
- source: https://cdn-uploads.huggingface.co/production/uploads/644249b08443bce4c9890a0f/P7UX63U74cbjMWDapPjFm.png
  original:
    file: 2026-09-24-accelerating-vision-language-models-with-lfm2-5-vl-dspark.image-07e0fa18c231.png
    width: 7006
    height: 3941
  variants:
  - file: 2026-09-24-accelerating-vision-language-models-with-lfm2-5-vl-dspark.image-3fde3e9b9be2.webp
    width: 320
    height: 180
  - file: 2026-09-24-accelerating-vision-language-models-with-lfm2-5-vl-dspark.image-acfb3cae7331.webp
    width: 640
    height: 360
  - file: 2026-09-24-accelerating-vision-language-models-with-lfm2-5-vl-dspark.image-c5290ead3559.webp
    width: 960
    height: 540
  - file: 2026-09-24-accelerating-vision-language-models-with-lfm2-5-vl-dspark.image-a2b806b2851b.webp
    width: 1280
    height: 720
  - file: 2026-09-24-accelerating-vision-language-models-with-lfm2-5-vl-dspark.image-ae7d41ceaf9e.webp
    width: 1600
    height: 900
  - file: 2026-09-24-accelerating-vision-language-models-with-lfm2-5-vl-dspark.image-b1e028a52f66.webp
    width: 7006
    height: 3941
  color: '#fbfbfb'
- source: https://cdn-uploads.huggingface.co/production/uploads/644249b08443bce4c9890a0f/n638hOT2C6Yoflniz2ffs.png
  original:
    file: 2026-09-24-accelerating-vision-language-models-with-lfm2-5-vl-dspark.image-18ad194cb8ac.png
    width: 1458
    height: 1020
  color: '#fdfdfd'
- source: https://cdn-uploads.huggingface.co/production/uploads/644249b08443bce4c9890a0f/QUPP6CX21dma5OiIYcEIL.png
  original:
    file: 2026-09-24-accelerating-vision-language-models-with-lfm2-5-vl-dspark.image-fb052975e9f8.png
    width: 1446
    height: 722
  color: '#fdfdfd'
---

Today, we release an experimental [**DSpark**](https://huggingface.co/papers/2607.05147) **draft model for our vision-language model** (VLM) [LFM2.5-VL-3B](https://huggingface.co/LiquidAI/LFM2.5-VL-3B). As with our [recently released LFM2.5-DSpark drafter models](https://huggingface.co/blog/LiquidAI/lfm25-dspark), it adds a speculative decoding path that trades a minimal increase in memory footprint for a larger speedup without changing output quality.

- **Faster inference:** decode speedups up to 3.13x on device and 2.66x on an H100, with end-to-end gains up to 2.62x and 2.27x.
- **Small memory cost:** the drafter adds 280M parameters, 8.9% on top of the 3B target
- **Day-one support:** LFM-compatible DSpark integrations for llama.cpp, MLX-VLM, and SGLang

## How does speculative decoding work for VLMs

The vision drafter uses the same architecture as our text LFM2.5-DSpark drafters: it captures the target model's hidden states at a fixed set of tapped layers and conditions on them to draft a block of k candidate tokens. Image patches and text tokens are projected into a shared representation before those layers, so the drafter operates on hidden-state vectors of identical dimensionality regardless of input modality. The inference algorithm is therefore unchanged from the text models. [![DSpark-Vision](https://cdn-uploads.huggingface.co/production/uploads/644249b08443bce4c9890a0f/P7UX63U74cbjMWDapPjFm.png)](https://cdn-uploads.huggingface.co/production/uploads/644249b08443bce4c9890a0f/P7UX63U74cbjMWDapPjFm.png)

## Training and Architecture

We follow the DSpark recipe with a mixture of vision-language SFT data, weighted toward the workloads we expect the model to serve. Based on ablations across 3, 4, and 5 layers, the draft model is a simplified attention-only drafter with 4 layers and a block size of 9. We ran 10 epochs on the final mixture and measured acceptance after each, which improved with additional training tokens before reaching diminishing returns. At inference time, we recommend a block size of 8 or 9 depending on the hardware.

The resulting drafter has approximately 280M parameters and increases the deployed model’s parameter count by just 8.9%.

| Component                | LFM2.5-VL-3B |
| ------------------------ | ------------ |
| Decoder stack (4 layers) | 193.0M       |
| Hidden-state projection  | 21.0M        |
| Markov head              | 65.5M        |
| Norms + confidence head  | 6.4k         |
| **Total**                | **279.5M**   |

## Inference Speedup on CPU and GPU

The DSpark draft model for LFM2.5-VL-3B ships with day-one support for [llama.cpp](https://github.com/ggml-org/llama.cpp), [MLX-VLM](https://github.com/Blaizzy/mlx-vlm), and [SGLang](https://github.com/sgl-project/sglang).

We measure both on-device inference and GPU inference. Both configurations use a DSpark block size of 8 and are evaluated on six diverse vision-based tasks, including general VQA, text VQA, image captioning, chart VQA, complex reasoning, and multi-turn conversation, following the [MMSpec benchmark](https://huggingface.co/papers/2603.14989).

**On-device inference.** With MLX on an M5 Max, decoding runs 2.30x to 3.13x faster by task. End-to-end latency improves by 1.56x to 2.62x. With llama.cpp on an M3 Ultra, decoding improves by 1.57x to 2.14x and end-to-end by 1.30x to 1.77x.

[![Screenshot 2026-09-24 at 15.49.03](https://cdn-uploads.huggingface.co/production/uploads/644249b08443bce4c9890a0f/n638hOT2C6Yoflniz2ffs.png)](https://cdn-uploads.huggingface.co/production/uploads/644249b08443bce4c9890a0f/n638hOT2C6Yoflniz2ffs.png)

**GPU inference.** On H100, the same drafter delivers 20.4x to 2.66x faster decoding, with end-to-end improvements of 1.64x to 2.27x.

[![Screenshot 2026-09-24 at 15.49.27](https://cdn-uploads.huggingface.co/production/uploads/644249b08443bce4c9890a0f/QUPP6CX21dma5OiIYcEIL.png)](https://cdn-uploads.huggingface.co/production/uploads/644249b08443bce4c9890a0f/QUPP6CX21dma5OiIYcEIL.png)

## Limitations of speculation for vision workloads

In LLMs, prefill is mostly compute-bound, and its cost grows (sub)quadratically with prompt length. VLMs add to this because the image first passes through a vision encoder, then the language backbone processes hundreds of visual tokens along with the text prompt. Edge devices have far less compute than datacenter GPUs, so prefill takes up more of the end-to-end latency, as time-to-first-token and decode measurements on Apple silicon and H100 show. (The M5's per-core GPU neural accelerators narrow this gap).

Speculative decoding speeds up only decode, not vision encoding or prefill. When those stages already take up much of the wall time, even a large decode speedup gives only a modest end-to-end gain. This is Amdahl's law, where the overall speedup is capped by the part of the workload that isn't accelerated.

## How to use LFM2.5-VL-DSpark

Running the DSpark draft models with **SGLang** requires an SGLang build with DSpark support for LFM2 targets ([PR #40651](https://github.com/sgl-project/sglang/pull/40651)). Launch the target with the draft attached:

```
python -m sglang.launch_server \
  --model-path LiquidAI/LFM2.5-VL-3B \
  --speculative-algorithm DSPARK \
  --speculative-draft-model-path LiquidAI/LFM2.5-VL-3B-DSpark \
  --speculative-draft-attention-backend flashinfer \
  --speculative-dspark-block-size 9 \
  --disable-radix-cache
```

Then query the OpenAI-compatible endpoint at `http://localhost:30000/v1`. The block size is read from the draft's `config.json`; the baseline is the same command without the three `--speculative-*` flags.

Running them with **llama.cpp** requires the respective llama.cpp build ([PR#29339](https://github.com/ggml-org/llama.cpp/pull/29339)).

```
llama-server -m models/LFM2.5-VL-3B-F16.gguf \
  --mmproj models/mmproj-LFM2.5-VL-3B-F16.gguf \
  -md LFM2.5-2.6B-DSpark-F16.gguf \
  --spec-type draft-dspark --spec-draft-n-max 8 --spec-draft-n-min 0 \
  -fa on -ngl 99 -c 8192
```

Running them with MLX-VLM requires the respective build ([PR#2280](https://github.com/Blaizzy/mlx-vlm/pull/2280)).

```
mlx_vlm.server --model LiquidAI/LFM2.5-VL-3B --draft-model LiquidAI/LFM2.5-VL-3B-DSpark
```

The block size is read from the sidecar metadata (n-max is clamped to it). Speculative decoding is **exact**: the target verifies every proposed token, so greedy output equals the target alone; per-response `timings` report `draft_n` / `draft_n_accepted`.

## Get Started

Our vision DSpark draft model is available on Hugging Face in [Safetensors](https://huggingface.co/LiquidAI/LFM2.5-VL-3B-DSpark) and [GGUF formats](https://huggingface.co/LiquidAI/LFM2.5-VL-3B-DSpark-GGUF).

With LFM2.5, we're delivering on our vision of AI that runs anywhere. These models are:

- **Open-weight** — Download, fine-tune, and deploy without restrictions.
- **Fast from day one** — Day-one support for llama.cpp, MLX, and SGLang.
- **A complete family** — From base models for customization to specialized audio and vision variants, one architecture covers diverse use cases

We can’t wait to see what you build.

## Citation

For citations, please use the following reference or BibTeX:\
*Liquid AI, "LFM2.5-VL-DSpark: Accelerating vision-language models on edge and beyond", Liquid AI Blog, Sep 2026.*

```
@article{liquidAI2026vldspark,
  author = {Liquid AI},
  title = {LFM2.5-VL-DSpark: Accelerating vision-language models on edge and beyond},
  journal = {Liquid AI Blog},
  year = {2026},
  note = {www.liquid.ai/blog/lfm2-5-vl-dspark},
}
```
