---
title: 'Quantization-Aware Healing: a compressed, 4-bit model that outperforms its full-precision original'
link: https://huggingface.co/blog/MultiverseComputingCAI/quantization-aware-healing
source: huggingface-co-blog
published: 2026-08-25T11:39:24Z
updated: 2026-08-25T11:39:24Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2026-08-25-quantization-aware-healing-a-compressed-4-bit-model-that.html
preview:
  file: 2026-08-25-quantization-aware-healing-a-compressed-4-bit-model-that.preview-aadd5a86b0e2.webp
  width: 256
  height: 77
  color: '#ebe5ec'
images:
- source: https://cdn-uploads.huggingface.co/production/uploads/668e37fd9c9aa124a3c867e8/2DSE26PMq2hJqKqEDOLIn.png
  original:
    file: 2026-08-25-quantization-aware-healing-a-compressed-4-bit-model-that.image-779e6ea0d13b.png
    width: 1649
    height: 495
  color: '#fdfdfd'
- source: https://cdn-uploads.huggingface.co/production/uploads/668e37fd9c9aa124a3c867e8/_oc2ExO8yVJ9xIyRsHzGy.png
  original:
    file: 2026-08-25-quantization-aware-healing-a-compressed-4-bit-model-that.image-cd5803b8d690.png
    width: 1316
    height: 1002
  color: '#fdfcfd'
- source: https://cdn-uploads.huggingface.co/production/uploads/668e37fd9c9aa124a3c867e8/fDOD3YXBoRPXFo2xDaiC0.png
  original:
    file: 2026-08-25-quantization-aware-healing-a-compressed-4-bit-model-that.image-779e6ea0d13b.png
    width: 1649
    height: 495
  color: '#fdfdfd'
- source: https://cdn-uploads.huggingface.co/production/uploads/668e37fd9c9aa124a3c867e8/r-bfWuDWpn0KnQ0Kqosai.png
  original:
    file: 2026-08-25-quantization-aware-healing-a-compressed-4-bit-model-that.image-78558d6f9655.png
    width: 1589
    height: 517
  color: '#fdfdfe'
---

Making a large language model smaller almost always comes with a cost. The now-standard recipe for efficient deployment is to compress the architecture first, cutting the parameter count by removing layers, heads, or neurons, and then quantize the remaining weights down to 4 bits to shrink memory and compute further. Both steps save a lot, but together they systematically degrade the capabilities people actually care about: reasoning, mathematical problem-solving, and code generation. Because of this, serious deployment pipelines add a recovery step, usually called healing, before the model goes into production. Recent open-weight releases such as [gpt-oss](https://huggingface.co/openai/gpt-oss-120b), NVIDIA's [Nemotron](https://huggingface.co/nvidia) family, and our own [Hypernova 60B](https://huggingface.co/MultiverseComputingCAI/Hypernova-60B-2605) all rely on some version of this compress-then-heal approach.

Our latest paper, [*Quantization-Aware Healing: A Practical Recipe for Recovering Compressed, 4-Bit LLMs*](https://huggingface.co/papers/2608.20953), asks a question that the field has mostly left open: once a model has already been through structural compression, not just quantization, how well does that recovery step actually work, and what is the right way to do it? We introduce Quantization-Aware Healing (QAH), and applied to a GPT-OSS 120B model compressed to 60B parameters and quantized to MXFP4, it produces a model that beats its own full-precision (bfloat16) version on 7 of 9 benchmarks. The 4-bit model ends up smaller, cheaper to run, and more accurate than the checkpoint it was quantized from. This inverts the usual relationship between a 4-bit model and the 16-bit model it came from.

## [](https://huggingface.co/blog/MultiverseComputingCAI/quantization-aware-healing#why-the-usual-healing-methods-fall-short-here) Why the usual healing methods fall short here

Most efficiency pipelines follow the same three steps: compress the architecture, quantize the compressed weights, then heal the damage. The difference between methods is entirely in that last step.

The dominant healing recipe is quantization-aware training (QAT). It inserts fake-quantization operators into the forward pass and keeps fine-tuning the model on a task loss, so the weights learn to tolerate the low-precision representation. In practice this means re-running an already expensive multi-stage post-training process, supervised fine-tuning, RLHF, agentic tuning, through a noisier, lower-precision forward pass. It is costly, and as our results show, it can also become unstable if training continues too long past its best point.

An alternative, quantization-aware distillation (QAD), avoids re-running that history. Instead of a task loss, it distills a frozen full-precision teacher directly into the quantized student through a [KL-divergence](https://docs.pytorch.org/docs/2.13/generated/torch.nn.KLDivLoss.html) loss on the output logits. This works well when the only change is quantization, because a genuine full-precision version of the exact same model exists to act as teacher. But once a model has gone through structural compression, fewer layers, heads, or neurons, and not just fewer bits, that assumption breaks. There is no independently trained full-precision version of the smaller architecture. The only candidate teacher is the recovered bfloat16 checkpoint, which is itself a distilled approximation of the original model. Distilling from it anchors the quantized student to a degraded target and caps its accuracy at that recovered checkpoint's own ceiling.

So the question of how to heal a model that has been both structurally compressed and quantized was, until now, genuinely open.

## [](https://huggingface.co/blog/MultiverseComputingCAI/quantization-aware-healing#our-approach) Our approach

QAH removes that ceiling with one change: it distills directly from the original, pre-compression model rather than from the recovered one. Teacher and student do not even share an architecture. The teacher is full-size and full-precision, the student is half the size and running in MXFP4. Because a teacher's output distribution is architecture-agnostic, nothing about the size or shape mismatch prevents the transfer. The student never sees hard labels, only the teacher's output distribution, matched through KL divergence on the logits.

This reframes what the quantization stage is doing. Under QAH it is no longer a lossy postprocessing step applied after healing is finished. It is a second, full pass of distillation against the original teacher, supervision that the bfloat16 checkpoint never received. The 4-bit student is not compensating for information lost to quantization; it is picking up information the earlier recovery stage did not have the time or data to transfer.

There is also a stability benefit that falls out of the loss itself. Because KL distillation ties the student to a fixed teacher distribution, once the student catches up there is no further pressure for it to drift. A cross-entropy task loss, by contrast, keeps pushing the student toward hard labels indefinitely. That difference turns out to matter for both accuracy and training stability, as the comparison below shows.

To make QAH work at long context, where the healing corpus includes documents up to 32k tokens, we reuse the memory-efficient chunked KL-divergence loss from our companion paper on efficient distillation. That loss computes the KL one slice of the sequence at a time and never materializes the full vocabulary-by-sequence grid, which is what makes 32k-token healing fit inside a fixed GPU memory budget. We covered the mechanics of that loss in a [previous post](https://huggingface.co/blog/MultiverseComputingCAI/efficient-knowledge-distillation).

[![QAH overview: after structural compression and quantization, capabilities drop sharply. QAH distills from the original pre-compression model as a frozen teacher, restoring performance without retracing the multi-stage post-training.](https://cdn-uploads.huggingface.co/production/uploads/668e37fd9c9aa124a3c867e8/_oc2ExO8yVJ9xIyRsHzGy.png)](https://cdn-uploads.huggingface.co/production/uploads/668e37fd9c9aa124a3c867e8/_oc2ExO8yVJ9xIyRsHzGy.png)

*QAH overview. After structural compression and quantization, capabilities drop sharply. QAH distills from the original model, a frozen teacher whose logits are precomputed offline, rather than from the recovered checkpoint. Source: paper Figure 1.*

## [](https://huggingface.co/blog/MultiverseComputingCAI/quantization-aware-healing#results) Results

We applied QAH to a GPT-OSS 120B model, compressed to 60B parameters and recovered in bfloat16, then re-quantized to MXFP4 under QAH. The natural comparison is against that same 60B model's bfloat16 checkpoint, the best full-precision version of this architecture that exists. The QAH model wins on 7 of the 9 benchmarks.

| Benchmark                       | 120B teacher (MXFP4) | 60B BF16 (recovered) | 60B MXFP4 (QAH) | QAH vs BF16 |
| ------------------------------- | -------------------- | -------------------- | --------------- | ----------- |
| AA-LCR (long-context reasoning) | 50.0                 | 35.3                 | 42.7            | +7.4        |
| AIME 2025 (math)                | 80.0                 | 70.7                 | 76.3            | +5.6        |
| Aider (agentic coding)          | 45.3                 | 38.2                 | 40.9            | +2.7        |
| τ²-bench (tool use)             | 68.4                 | 59.4                 | 61.7            | +2.3        |
| GPQA Diamond (science)          | 69.0                 | 65.7                 | 67.4            | +1.7        |
| IFBench (instruction following) | 63.3                 | 58.4                 | 59.9            | +1.5        |
| LiveCodeBench (coding)          | 66.0                 | 65.5                 | 66.5            | +1.0        |
| MMLU-Pro (knowledge)            | 78.0                 | 74.0                 | 73.8            | −0.2        |
| SciCode (science coding)        | 37.5                 | 35.6                 | 34.2            | −1.4        |

The two benchmarks where QAH trails, MMLU-Pro and SciCode, lose by less than a point and a half. Everywhere else the 4-bit model is ahead of its own 16-bit source, and the largest gains land on exactly the capabilities compression usually damages most: long-context reasoning (+7.4 on AA-LCR) and math (+5.6 on AIME 2025).

The comparison against the original 120B teacher is just as telling. Despite running at half the teacher's parameter count and roughly a quarter of its weight memory, the QAH model surpasses the full-size teacher on LiveCodeBench (66.5 vs. 66.0) and comes within 1.6 points on GPQA Diamond (67.4 vs. 69.0). The largest remaining gap against the teacher is on AA-LCR, an extreme long-context benchmark where the capacity lost to compression is intrinsically the hardest to recover.

[![Benchmark performance of the three checkpoints: the original GPT-OSS-120B teacher (MXFP4), the compressed and recovered 60B model in bfloat16, and the same 60B model re-quantized to MXFP4 with QAH, across nine benchmarks.](https://cdn-uploads.huggingface.co/production/uploads/668e37fd9c9aa124a3c867e8/fDOD3YXBoRPXFo2xDaiC0.png)](https://cdn-uploads.huggingface.co/production/uploads/668e37fd9c9aa124a3c867e8/fDOD3YXBoRPXFo2xDaiC0.png)

*The 4-bit QAH model matches or beats its bfloat16 source on 7 of 9 benchmarks, and beats the full-size teacher on LiveCodeBench. Source: paper Figure 2.*

### [](https://huggingface.co/blog/MultiverseComputingCAI/quantization-aware-healing#qah-against-qat-head-to-head) QAH against QAT, head to head

To isolate the effect of the loss function from everything else, we also compared QAH directly against QAT under matched conditions, quantizing a GPT-OSS 9B model to MXFP4 and tracking average performance across MMLU-Pro, LiveCodeBench, and GPQA Diamond as training progresses.

Both methods reach a similar peak, 54.9 for QAH against 54.6 for QAT, so on best-case accuracy they are effectively tied. The difference is in how they get there and what happens afterwards. QAH reaches its peak in about 100 steps, roughly 7 times faster than QAT's 700, and then stays within about two points of that peak for the rest of training. QAT collapses sharply once past its peak, shedding nearly 19 points by step 1,200.

The practical consequence is a real deployment risk difference. A QAT checkpoint needs careful early stopping against a held-out signal to avoid shipping a model that has already started to degrade, whereas a sufficiently trained QAH checkpoint can be served safely because it simply does not drift. This is consistent with the mechanism: KL distillation against a frozen teacher gives the student no incentive to move once it matches the teacher, while a cross-entropy objective keeps pushing on hard labels and eventually erodes capabilities the model inherited from the original.

[![Average performance of QAH and QAT as training progresses, quantizing GPT-OSS 9B to MXFP4. QAH peaks early and stays stable through 1,200 steps; QAT reaches a comparable peak much later, then collapses.](https://cdn-uploads.huggingface.co/production/uploads/668e37fd9c9aa124a3c867e8/r-bfWuDWpn0KnQ0Kqosai.png)](https://cdn-uploads.huggingface.co/production/uploads/668e37fd9c9aa124a3c867e8/r-bfWuDWpn0KnQ0Kqosai.png)

*QAH peaks at 54.9 in roughly 100 steps and holds; QAT reaches 54.6 only around step 700, then loses nearly 19 points by step 1,200. Source: paper Figure 3.*

## [](https://huggingface.co/blog/MultiverseComputingCAI/quantization-aware-healing#what-this-changes-in-practice) What this changes in practice

The accuracy story comes paired with the efficiency story that motivated compression in the first place. At 4-bit precision the QAH model uses roughly 4 times less weight memory than the bfloat16 student, and at half the parameter count of the 120B teacher it roughly halves compute per token, which is what lets it run on substantially smaller hardware. For model families that ship in bfloat16 rather than 4-bit, the combined parameter and precision reduction would be closer to 8 times less compute per token.

The takeaway is that a compressed, 4-bit model does not have to be a lower-accuracy version of its full-precision counterpart. With this healing recipe it can be smaller, cheaper to serve, and more accurate at the same time, and it reaches that point in a fraction of the training a QAT recipe would need. Quantization stops being a tax you pay for efficiency and becomes an extra opportunity to teach the model.

This work is part of Multiverse Computing's ongoing research into making large models smaller and cheaper to run without giving up the capabilities that make them useful. It sits alongside our companion work on efficient distillation, which supplies the long-context training machinery QAH depends on.

Want the full technical details, including the healing pipeline, the chunked KL implementation for long-context healing, and the distributed-training findings? Read the [full paper](https://huggingface.co/papers/2608.20953), or get in touch with our team to talk about applying compression and healing to your own models.
