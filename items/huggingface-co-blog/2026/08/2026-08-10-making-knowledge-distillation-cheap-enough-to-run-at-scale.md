---
title: Making Knowledge Distillation Cheap Enough to Run at Scale
link: https://huggingface.co/blog/MultiverseComputingCAI/efficient-knowledge-distillation
source: huggingface-co-blog
published: 2026-08-10T10:05:36Z
updated: 2026-08-10T10:05:36Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.html
preview:
  file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.preview-0db2f80e3d8d.webp
  width: 256
  height: 148
  color: '#d7dfe1'
images:
- source: https://cdn-uploads.huggingface.co/production/uploads/668e37fd9c9aa124a3c867e8/nB5NK4bdfr7AY1IAO14Gh.png
  original:
    file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-8d68ce08416c.png
    width: 2199
    height: 1275
  color: '#fdf7f8'
- source: https://img.shields.io/badge/%F0%9F%8C%90%20Multiverse%20Computing-Website-0BAA8F?style=for-the-badge
  original:
    file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-112ee9e0a127.png
    width: 280
    height: 28
  variants:
  - file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-d0dec62309c3.webp
    width: 280
    height: 28
  color: '#555555'
- source: https://img.shields.io/badge/GitHub-Full--Chunked--KL--Loss-181717?style=for-the-badge&logo=github&logoColor=white
  original:
    file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-eb799be27b62.png
    width: 276
    height: 28
  variants:
  - file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-bf57f18518a9.webp
    width: 276
    height: 28
  color: '#181717'
- source: https://img.shields.io/badge/%F0%9F%93%84%20arXiv-Paper-B31B1B?style=for-the-badge
  original:
    file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-9cf137bf07ff.png
    width: 146
    height: 28
  variants:
  - file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-e8bfe2327ce7.webp
    width: 146
    height: 28
  color: '#555555'
- source: https://cdn-uploads.huggingface.co/production/uploads/614a1ebb8f82f1df64d55126/jsYblMP3R5futGF9Y8Kl1.png
  original:
    file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-8799e79bd614.png
    width: 3420
    height: 1512
  variants:
  - file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-94a804b4bbef.webp
    width: 320
    height: 141
  - file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-f0f59fe57759.webp
    width: 640
    height: 283
  - file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-da3ffb2d366c.webp
    width: 960
    height: 424
  - file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-9851da0a9ec7.webp
    width: 1280
    height: 566
  - file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-100f13d3a727.webp
    width: 1600
    height: 707
  - file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-d301684856cd.webp
    width: 3420
    height: 1512
  color: '#fbfcfc'
- source: https://cdn-uploads.huggingface.co/production/uploads/668e37fd9c9aa124a3c867e8/-whmAUit96bo1Yy0vLPis.png
  original:
    file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-8d68ce08416c.png
    width: 2199
    height: 1275
  color: '#fdf7f8'
- source: https://cdn-uploads.huggingface.co/production/uploads/614a1ebb8f82f1df64d55126/k4K6L0g7crbOk4ZqJuFuy.gif
  original:
    file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-0e703396094f.gif
    width: 1400
    height: 860
  color: '#f9fafa'
- source: https://cdn-uploads.huggingface.co/production/uploads/668e37fd9c9aa124a3c867e8/hV9Hu8he7f1kZJy_SS9X6.png
  original:
    file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-c1c3f4d25ebf.png
    width: 2527
    height: 1933
  variants:
  - file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-11baba04cf52.webp
    width: 320
    height: 245
  - file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-5f73dc93ab47.webp
    width: 640
    height: 490
  - file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-297a7244cce8.webp
    width: 960
    height: 734
  - file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-cc42a5b1b3ff.webp
    width: 1280
    height: 979
  - file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-4b9512fd9d6d.webp
    width: 1600
    height: 1224
  - file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-6f7a1da29271.webp
    width: 2527
    height: 1933
  color: '#fdfdfd'
- source: https://cdn-uploads.huggingface.co/production/uploads/614a1ebb8f82f1df64d55126/e0VvKmFktQCyoOhhOTD4n.png
  original:
    file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-270d42df14da.png
    width: 1600
    height: 886
  variants:
  - file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-3ef246c765aa.webp
    width: 320
    height: 177
  - file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-20d7fa4d2a82.webp
    width: 640
    height: 354
  - file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-e22fb5912c4d.webp
    width: 960
    height: 532
  - file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-25b3347be8b2.webp
    width: 1280
    height: 709
  - file: 2026-08-10-making-knowledge-distillation-cheap-enough-to-run-at-scale.image-99af2f826c4d.webp
    width: 1600
    height: 886
  color: '#fdfdfd'
---

[![🌐 Multiverse Computing](https://img.shields.io/badge/%F0%9F%8C%90%20Multiverse%20Computing-Website-0BAA8F?style=for-the-badge)](https://multiversecomputing.com) [![GitHub Repository](https://img.shields.io/badge/GitHub-Full--Chunked--KL--Loss-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/CompactifAI/Full-Chunked-KL-Loss/tree/main) [![📄 arXiv Paper](https://img.shields.io/badge/%F0%9F%93%84%20arXiv-Paper-B31B1B?style=for-the-badge)](https://arxiv.org/abs/2608.03796)

* * *

[![Peak VRAM usage by loss](https://cdn-uploads.huggingface.co/production/uploads/614a1ebb8f82f1df64d55126/jsYblMP3R5futGF9Y8Kl1.png)](https://cdn-uploads.huggingface.co/production/uploads/614a1ebb8f82f1df64d55126/jsYblMP3R5futGF9Y8Kl1.png)

[Knowledge distillation](https://arxiv.org/abs/1503.02531), training a smaller student model to match the performance of a larger teacher, is a well-known technique in Machine Learning. With the recent wave of open-source Large Language Models, such as [gpt-oss](https://huggingface.co/openai/gpt-oss-120b), [Qwen](https://huggingface.co/collections/Qwen/qwen35), [GLM](https://huggingface.co/zai-org/GLM-5.2), or [Kimi](https://huggingface.co/moonshotai/Kimi-K3), it has become a mainstream research topic again. Deploying these very large models is expensive: the recent [Kimi-K3 model](https://huggingface.co/moonshotai/Kimi-K3) has 2.8 trillion parameters and needs roughly 3TB of VRAM just to load. Compressing them into smaller models and recovering the original capabilities through knowledge distillation has therefore become standard practice, with companies like Nvidia ([Nemotron 3 Puzzle 75B](https://huggingface.co/nvidia/NVIDIA-Nemotron-Labs-3-Puzzle-75B-A9B-NVFP4)) or Multiverse Computing ([Hypernova 60B](https://huggingface.co/MultiverseComputingCAI/Hypernova-60B-2605)) recently releasing high-quality compressed models.

The distillation step is what decides most of the final quality, but it's also usually the most expensive part of the pipeline. Keeping both the teacher and student loaded, and producing a probability distribution over the entire vocabulary for every token, requires enormous amounts of VRAM, typically feasible only with hundreds of GPUs and careful tensor-parallelism strategies. Our latest paper, [Efficient Knowledge Distillation for LLMs: Offline Top-K Logits and a Fused Chunked KL Loss](https://huggingface.co/papers/2608.03796), tackles this with two systems changes: caching the teacher's top-K logits once so the teacher never has to sit in memory alongside the student, and a new, memory-efficient KL-divergence loss that avoids ever materializing the full vocabulary-size × sequence-length matrix, cutting VRAM use far below what the default implementations in libraries like [PyTorch](https://pytorch.org) or [NVIDIA Megatron-Bridge](https://github.com/NVIDIA-NeMo/Megatron-Bridge) achieve. Together, these two changes cut training cost enough to make long-context healing possible on a single GPU, and cheap enough to make large-scale experimentation practical.

## [](https://huggingface.co/blog/MultiverseComputingCAI/efficient-knowledge-distillation#why-distillation-recovery-is-expensive) Why distillation recovery is expensive

The standard setup, *online* distillation using the [Kullback-Leibler divergence loss](https://docs.pytorch.org/docs/2.13/generated/torch.nn.KLDivLoss.html) (KL loss), keeps both the teacher and the student loaded at the same time. At every training step, the teacher runs a full forward pass to produce its output distribution, and the student is trained to match it. This is the most expressive setup, since the full teacher distribution is available, but it is also the most memory- and compute-intensive: two full-vocabulary tensors have to be held per token position, and the teacher has to be recomputed on every single step even though its behavior does not change across a training run.

As a practical example, gpt-oss-120b has a vocabulary of 201,088 tokens. At a sequence length of 32K and batch size 4, the teacher-probability tensor alone has shape `4 × 201,088 × 32,768`; in bfloat16, that's already about 50GB of VRAM for a single tensor. Add gradients, activations, model weights, and optimizer states, and a single training iteration of distillation can peak at roughly 250GB of VRAM, more than even an H200 or B200 GPU can provide. In this post, we show that reformulating the KL loss to process the data in chunks reduces this cost to almost nothing.

[![Peak GPU memory over one training iteration at 32K context, comparing dense KL against the fused chunked KL loss](https://cdn-uploads.huggingface.co/production/uploads/668e37fd9c9aa124a3c867e8/-whmAUit96bo1Yy0vLPis.png)](https://cdn-uploads.huggingface.co/production/uploads/668e37fd9c9aa124a3c867e8/-whmAUit96bo1Yy0vLPis.png) *Dense KL spikes to roughly 250GB, above a single H200's 141GB capacity. The fused chunked loss never forms that spike and peaks at about 128GB. Source: paper Figure 1.*

## [](https://huggingface.co/blog/MultiverseComputingCAI/efficient-knowledge-distillation#two-systems-changes) Two systems changes

**Offline distillation.** Instead of recomputing the teacher at every step, we compute its output once, cache the top-100 most likely tokens per position, and train the student against that cache. The teacher never has to sit in memory during training and does not need to be run again once the cache exists, so the same cache can be reused across many ablations.

**A fused, chunked KL loss.** To see why the loss itself is expensive, picture what it actually builds: for every token position in a sequence and every word in the vocabulary, the loss needs a number describing how much the student's prediction disagrees with the teacher's. Laid out as a grid, that's one row per vocabulary entry and one column per sequence position, for a vocabulary of 100K+ words and a long sequence, that grid is enormous, and the default way of computing a KL loss builds the whole thing before it can produce a single number.

We compare three ways of computing this same loss, all mathematically equivalent:

- **Dense KL** is the textbook approach. It rebuilds a full, dense teacher-probability grid from the cached top-100 logits and compares it against the student's own dense grid of log-probabilities. This is the version closest to how online distillation already works, so we use it as our correctness baseline, but it holds the full vocabulary × sequence grid in memory, twice over.
- **Forward-chunked KL** keeps the teacher sparse (only its cached top-100 logits per position, never expanded into a dense grid) and computes the loss piece by piece, one slice of sequence positions at a time. This removes the dense teacher and the dense comparison, and turns out to be the fastest of the three methods in our benchmarks. It still has one blind spot, though: the student's own logits, the grid produced by the model's output layer, are still computed in full and kept around for the backward pass, so memory still grows steeply with sequence length.
- **Fused chunked KL**, our main contribution, goes a step further and fuses the model's output projection directly into the loss computation. It never produces the student's full logits grid at all: it processes one chunk of the sequence at a time end to end, projecting hidden states to logits for that chunk, folding the result into the running loss, and discarding the chunk before moving to the next one. The backward pass recomputes each chunk on the fly instead of storing it. The cost is doing that projection twice, once forward, once in backward, but in exchange, peak memory grows only linearly with sequence length instead of spiking with the full vocabulary × sequence size.

The GIF below shows the difference between the dense and fused-chunked approaches: one builds the whole comparison grid and holds onto all of it, the other builds and discards one slice at a time, so memory never grows beyond a single chunk.

We have open-sourced the chunked-loss implementation: [github.com/CompactifAI/Full-Chunked-KL-Loss](https://github.com/CompactifAI/Full-Chunked-KL-Loss)

[![How the dense KL loss holds the full vocabulary-by-sequence grid in memory, while the fused chunked loss builds and discards one slice at a time](https://cdn-uploads.huggingface.co/production/uploads/614a1ebb8f82f1df64d55126/k4K6L0g7crbOk4ZqJuFuy.gif)](https://cdn-uploads.huggingface.co/production/uploads/614a1ebb8f82f1df64d55126/k4K6L0g7crbOk4ZqJuFuy.gif)

## [](https://huggingface.co/blog/MultiverseComputingCAI/efficient-knowledge-distillation#what-this-changes-in-practice) What this changes in practice

The table below puts all four setups head to head: online distillation, and the three offline loss implementations just described. Comparing them on a single H200 GPU with [Llama 3.1 8B Instruct](https://huggingface.co/meta-llama/Llama-3.1-8B-Instruct) as teacher and a 3.2B Llama model as student at an 8K token context, all four reach near-identical training loss, even though the offline runs train against only the cached top-100 logits per token.

| Method (8K context, single H200) | Peak memory | Iteration time | Throughput  |
| -------------------------------- | ----------- | -------------- | ----------- |
| Online distillation              | 102.8 GB    | 25.9 s         | 237 TFLOP/s |
| Offline, dense KL                | 78.3 GB     | 18.5 s         | 331 TFLOP/s |
| Offline, forward-chunked KL      | 61.8 GB     | 18.4 s         | 335 TFLOP/s |
| Offline, fused chunked KL        | 58.3 GB     | 20.2 s         | 304 TFLOP/s |

[![Training loss, iteration time and throughput, and peak GPU memory breakdown for online distillation versus the three offline loss implementations, at 8K context on a single H200](https://cdn-uploads.huggingface.co/production/uploads/668e37fd9c9aa124a3c867e8/hV9Hu8he7f1kZJy_SS9X6.png)](https://cdn-uploads.huggingface.co/production/uploads/668e37fd9c9aa124a3c867e8/hV9Hu8he7f1kZJy_SS9X6.png)

The loss curves overlap almost exactly across all four methods, confirming offline distillation with top-100 cached logits is lossless relative to online distillation. Source: paper Figure 2. At this sequence length, the fused chunked loss is not yet the fastest option, its extra backward-pass projection costs a bit of speed, but its real advantage only shows up as context length grows, which the next section demonstrates.

### [](https://huggingface.co/blog/MultiverseComputingCAI/efficient-knowledge-distillation#scaling-to-long-context-lengths) Scaling to long context lengths

To see the scaling pattern more starkly, we ran an isolated benchmark on a toy output-projection network (no transformer body, just the loss kernel). At 32K tokens, peak memory falls from 85.2 GiB with the dense loss to 5.45 GiB with the fully chunked version, a 15.6× reduction, and the dense loss fails outright from 64K tokens onward. At 256K tokens, the fully chunked loss uses 11.6 GiB against 134.2 GiB for the next-best chunked variant, and is about 3.3× faster per iteration at that length.

[![Peak VRAM usage by loss](https://cdn-uploads.huggingface.co/production/uploads/614a1ebb8f82f1df64d55126/jsYblMP3R5futGF9Y8Kl1.png)](https://cdn-uploads.huggingface.co/production/uploads/614a1ebb8f82f1df64d55126/jsYblMP3R5futGF9Y8Kl1.png)

Distilling a GPT-OSS 20B model at a 32,768-token context, the memory freed by the fused loss let the setup shrink from four GPU nodes down to one. Step time fell from 57.0 to 12.23 seconds, about 5× faster, and throughput per GPU rose from 74.2 to 345.7 TFLOP/s.

## [](https://huggingface.co/blog/MultiverseComputingCAI/efficient-knowledge-distillation#the-resulting-student) The resulting student

The efficient offline setup is what made a large-scale distillation campaign affordable in the first place. The resulting compact student, distilled from Llama 3.1 8B Instruct down to about 3.2B parameters, retains most of the teacher's accuracy on BoolQ and HellaSwag, stays within about nine points of it on MMLU, at less than half the parameter count.

[![Short-context accuracy of the compact 3.2B student against the Llama 3.1 8B Instruct teacher on BoolQ, WinoGrande, MMLU, HellaSwag, and GSM8K](https://cdn-uploads.huggingface.co/production/uploads/614a1ebb8f82f1df64d55126/e0VvKmFktQCyoOhhOTD4n.png)](https://cdn-uploads.huggingface.co/production/uploads/614a1ebb8f82f1df64d55126/e0VvKmFktQCyoOhhOTD4n.png) *The student retains most of the teacher's short-context accuracy at less than half the size. Source: paper Figure 6.*

This work is part of [Multiverse Computing's](https://multiversecomputing.com) ongoing [research into making distillation and healing practical to run at scale](https://multiversecomputing.com/compactifai), not just as a one-off recipe, but as something teams can iterate on cheaply. The paper also covers additional ablations, such as how the choice of loss function and sequence packing affect recovery quality.

Want the full technical details, including the closed-form gradient behind the fused chunked loss and the complete training configuration? Read the [full paper](https://arxiv.org/abs/2608.03796), or get in touch with our team to talk about applying this to your own distillation pipelines.

We have also open-sourced the chunked-loss implementation: [github.com/CompactifAI/Full-Chunked-KL-Loss](https://github.com/CompactifAI/Full-Chunked-KL-Loss)
