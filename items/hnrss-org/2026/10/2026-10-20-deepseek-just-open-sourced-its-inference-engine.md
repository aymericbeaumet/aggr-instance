---
title: DeepSeek just open-sourced its inference engine
link: https://github.com/deepseek-ai/DeepInfer
source: hnrss-org
published: 2026-10-20T09:08:54Z
updated: 2026-10-20T09:08:54Z
first_seen: 2026-10-20T12:56:01.525437407Z
authors:
- ggcr
content: extracted
html: 2026-10-20-deepseek-just-open-sourced-its-inference-engine.html
remote_preview:
  url: https://opengraph.githubassets.com/2f1c2fc17da059a0f4782846a6f07a98efb17f74bbfdcd9195516247e979bcfc/deepseek-ai/DeepInfer
  alt: 'DeepInfer: High-performance, large-scale inference framework for DeepSeek models - deepseek-ai/DeepInfer'
---

DeepInfer is the high-performance inference framework that powers DeepSeek's large-scale production services. This open-source release provides offline inference with the core features needed for serving at scale.

Supported models:

- DeepSeek-V4.1-Flash

Supported features:

- Optimized kernels, model and sampler runtime
- Parallelism: Attention DP / balanced CP, MoE EP + EPLB
- DSpark and confidence-scheduled verification
- SWA bounded replay
- Asynchronous scheduling (zero-overhead batch scheduler)
- Batch-invariant inference
- Deterministic sampling
- Structured outputs

Feature details and configuration

- **Optimized kernels:** [FlashMLA](https://github.com/deepseek-ai/FlashMLA), [DeepGEMM](https://github.com/deepseek-ai/DeepGEMM), [DeepEP](https://github.com/deepseek-ai/DeepEP), [TileKernels](https://github.com/deepseek-ai/TileKernels), [DeepSelect](https://github.com/deepseek-ai/DeepSelect), and custom [TileLang kernels](https://github.com/deepseek-ai/DeepInfer/blob/main/deepinfer/model_executor/layers/tilelang_ops).
- **DSpark and confidence-scheduled verification:** accelerate decoding with draft tokens and use confidence estimates to schedule verification. Set `DEEPINFER_ENABLE_MTP_VARLEN=1`; `DEEPINFER_USE_NEXTN_DECODE` controls the maximum draft length (default: 5).
- **Decoder SWA bounded replay:** reduce decoder prefill work by replaying only a bounded suffix. Set `DEEPINFER_YOCO_PREFILL_TRUNCATION_MODE=compact` and `DEEPINFER_YOCO_PREFILL_KEEP_TOKENS=140` for the default V4.1 settings; the retained suffix must cover the SWA window plus the maximum backtracking length.
- **Asynchronous scheduling:** overlap CPU batch scheduling with device execution. Enabled by default.
- **Batch-invariant inference:** keep model results identical across different batch compositions. Enable with `DEEPINFER_BATCH_INVARIANT=1` (NVIDIA only).
- **Deterministic sampling:** produce identical outputs for identical input tokens and sampling parameters. Set `DEEPINFER_BATCH_INVARIANT=1 DEEPINFER_ENABLE_MTP_VARLEN=0` and pass a fixed `seq_seed` to `SamplingParams` (NVIDIA only).
- **Structured outputs:** constrain generation to a regex, JSON schema, or EBNF grammar. Pass `so_args={"schema_type": ..., "schema": ...}` to `SamplingParams`; see [examples](https://github.com/deepseek-ai/DeepInfer/blob/main/examples/_structured_output.py).

Supported hardware:

- NVIDIA SM100 GPUs
- Huawei Ascend 950 NPUs

Required for a complete inference service (not included in this release):

- A KV cache system
- VE/Prefill/Decode disaggregation (most required engine functionality is already implemented)
- Load balancing across data-parallel replicas for both prefill and decode

## Getting started

### Prerequisites

- Linux
- GCC >= 13.0
- Python 3.12
- CUDA 13 (NVIDIA only)
- CANN 9.2 (Ascend only)

### Build from source

Before building DeepInfer, please create and activate an empty Python 3.12 environment.

```shell
# Use `ascend` instead for the Ascend platform
# Keep it exported in the environment where you run DeepInfer
export DEEPINFER_ACCELERATOR=nvidia
./build.sh
```

### Offline inference

The default checkpoint, [`deepseek-ai/DeepSeek-V4.1-Flash-1017`](https://huggingface.co/deepseek-ai/DeepSeek-V4.1-Flash-1017), is downloaded into the Hugging Face cache on the first run; set `DEEPINFER_HF_MODEL_DIR=/path/to/DeepSeek-V4.1-Flash-1017` to run from a local copy instead.

```shell
python examples/offline_inference.py
```

See [examples](https://github.com/deepseek-ai/DeepInfer/blob/main/examples/README.md) for details.

## Performance benchmarks

| Prefill                                                                                                                                                                                                                   | Decode                                                                                                                                                                                                              |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [![Prefill: non-cached input throughput vs. latency](https://github.com/deepseek-ai/DeepInfer/raw/main/doc/images/prefill_pareto.png)](https://github.com/deepseek-ai/DeepInfer/blob/main/doc/images/prefill_pareto.png) | [![Decode: output throughput vs. per-user speed](https://github.com/deepseek-ai/DeepInfer/raw/main/doc/images/decode_pareto.png)](https://github.com/deepseek-ai/DeepInfer/blob/main/doc/images/decode_pareto.png) |

Measured on 4 SM100 GPUs for prefill and 64 for decode — each within one scale-up domain. To reproduce the benchmarks, see [run\_sweep.sh](https://github.com/deepseek-ai/DeepInfer/blob/main/examples/sweep/run_sweep.sh).

## Evaluation benchmarks

See [eval/README.md](https://github.com/deepseek-ai/DeepInfer/blob/main/eval/README.md).

## License

DeepInfer is released under the Apache License 2.0. See [LICENSE](https://github.com/deepseek-ai/DeepInfer/blob/main/LICENSE) for details.

## Acknowledgments

DeepInfer was originally built primarily on [vLLM](https://github.com/vllm-project/vllm), with additional inspiration from [SGLang](https://github.com/sgl-project/sglang) and [TensorRT-LLM](https://github.com/NVIDIA/TensorRT-LLM). We thank their developers.

## Citation

```text-bibtex
@misc{deepinfer2026,
  title = {DeepInfer: High-performance, large-scale inference framework for DeepSeek models},
  author = {DeepSeek-AI},
  year = {2026},
  publisher = {GitHub},
  howpublished = {\url{https://github.com/deepseek-ai/DeepInfer}},
}
```
