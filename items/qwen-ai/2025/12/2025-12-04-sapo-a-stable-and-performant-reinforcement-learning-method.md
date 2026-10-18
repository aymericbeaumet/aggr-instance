---
title: 'SAPO: A Stable and Performant Reinforcement Learning Method for Training Large Language Models'
link: https://qwen.ai/blog?id=sapo
source: qwen-ai
published: 2025-12-04T20:00:00Z
first_seen: 2026-10-18T23:27:08.867657739Z
authors:
- QwenTeam
labels:
- research
content: feed
html: 2025-12-04-sapo-a-stable-and-performant-reinforcement-learning-method.html
remote_preview:
  url: https://img.alicdn.com/imgextra/i4/O1CN018ECcH61qkAXZSFBFU_!!6000000005533-2-tps-1890-1134.png
---

## Introduction

Reinforcement learning (RL) has become a core ingredient in advancing the reasoning capabilities of large language models (LLMs). Modern RL pipelines enable models to solve harder mathematical problems, write complex code, and reason over multimodal inputs. In practice, group‑based policy optimization—where multiple responses are sampled per prompt and their rewards are normalized within the group—has emerged as a dominant training paradigm for LLMs. However, despite its empirical success, stable and performant policy optimization remains challenging. A critical challenge lies in the variance of token‑level importance ratios, especially in large Mixture‑of‑Experts (MoE) models. These ratios quantify how far the current policy deviates from the behavior policy used to generate the training samples. When ratios fluctuate excessively (as they often do with expert routing or long autoregressive outputs), policy updates become noisy and unstable.

Existing solutions such as GRPO (token‑level clipping) and GSPO (sequence‑level clipping) attempt to control this instability by enforcing **hard clipping**: whenever the importance ratio falls outside a fixed band, gradients are truncated. While this reduces catastrophic updates, it introduces two inherent limitations:

- **Loss of learning signal.** Hard clipping discards all gradient information outside the clipping range. In sequence-level methods such as GSPO, a few off‑policy tokens can cause an entire sequence to be ignored.
- **Hard to strike a favorable trade-off.** When the clipping range is tight, many informative samples contribute zero gradient; when the range is wide, off‑policy noisy gradients destabilize training. This brittle trade‑off becomes especially problematic in MoE architectures.

As a result, GRPO and GSPO often struggle to strike a balance between stability, sample efficiency, and consistent learning progress. To address these limitations, we propose **Soft Adaptive Policy Optimization (SAPO)**, an RL method designed for stable and performant optimization of LLMs. SAPO replaces hard clipping with a **smooth, temperature‑controlled gating function** that adaptively down‑weights off‑policy updates while preserving useful gradients. Unlike existing methods, SAPO offers:

- **Continuous trust regions**, avoiding the discontinuities of clipping.
- **Sequence-level coherence** similar to GSPO, but without discarding entire sequences.
- **Token‑level adaptivity**, enabling selective suppression of problematic tokens.
- **Asymmetric temperature design**, reflecting the empirically different behaviors of positive and negative tokens in large‑vocabulary models.

This unified design allows SAPO to achieve stable and effective learning.

## Soft Adaptive Policy Optimization (SAPO)

SAPO optimizes the following surrogate objective:

$$ \\mathcal{J}(\\theta) = \\mathbb{E}\\left\[ \\frac{1}{G}\\sum\_{i=1}^{G} \\frac{1}{|y\_i|} \\sum\_{t=1}^{|y\_i|} f\_{i,t}(r\_{i,t}(\\theta))\\widehat{A}\_{i,t} \\right\] $$

where

- $r\_{i,t}(\\theta)$ is the token‑level importance ratio
- $\\widehat{A}\_{i,t}$ is the group‑normalized advantage
- $f\_{i,t}(\\cdot)$ is a smooth gating function defined as $f\_{i,t}(x) = \\frac{4}{\\tau\_{i,t}} \\cdot \\sigma\\big(\\tau\_{i,t}(x - 1)\\big)$, with different temperatures $\\tau\_{i,t}=\\tau\_{\\text{pos}}$ and $\\tau\_{i,t} = \\tau\_{\\text{neg}}$ for positive and negative advantages, respectively.

The gradient takes the form

$$ \\nabla\_\\theta \\mathcal{J}(\\theta) = \\mathbb{E}\\left\[ \\frac{1}{G}\\sum\_{i=1}^{G} \\frac{1}{|y\_i|} \\sum\_{t=1}^{|y\_i|} w\_{i,t}(\\theta) r\_{i,t}(\\theta) \\nabla\_\\theta \\log\\pi\_\\theta \\widehat{A}\_{i,t} \\right\] $$

where the weight is

$$ w\_{i,t}(\\theta) = 4,p\_{i,t}(\\theta)(1-p\_{i,t}(\\theta)), \\quad p\_{i,t}(\\theta) = \\sigma\\big(\\tau\_{i,t}(r\_{i,t}(\\theta)-1)\\big). $$

This weight peaks at $r\_{i,t}(\\theta)=1$ and decays smoothly on both sides.

![](https://qianwen-res.oss-accelerate-overseas.aliyuncs.com/SAPO/soft_gate.png)

## Why SAPO Works: A Gating-Function Perspective

### SAPO recovers sequence‑level coherence (connection to GSPO)

Let $s\_i(\\theta)$ be the length‑normalized sequence‑level importance ratio: $\\log s\_i(\\theta) = \\frac{1}{|y\_i|} \\sum\_t \\log r\_{i,t}(\\theta)$.

If the policy updates are small and the token log‑ratios within a sequence have low variance—two assumptions that empirically hold for most sequences—then the average SAPO token gate becomes approximately a sequence‑level gate of the form $g(\\log s\_i(\\theta)) \\approx \\text{sech}^2\\left(\\frac{\\tau}{2}\\log s\_i(\\theta)\\right)$. This means SAPO behaves like GSPO at the sequence level but with a continuous trust region instead of hard clipping.

Key advantage over GSPO: If a few tokens in a sequence are very off‑policy,

- GSPO suppresses the entire sequence
- SAPO suppresses only those tokens, preserving other useful gradients

This improves sample efficiency.

### SAPO provides smooth token‑level adaptivity (connection to GRPO)

GRPO uses hard clipping:

- inside hard clipping band → full gradient
- outside → zero gradient

This creates brittle, discontinuous optimization behavior.

SAPO replaces the hard cutoff with a smooth decay:

- no abrupt gradient drops
- no exploding contributions
- gradual suppression as deviation increases

This allows SAPO to provide a more balanced way to retain useful learning signals while preventing unstable policy shifts.

### Asymmetric temperature for negative advantages improves stability

Negative advantages increase the logits of many inappropriate tokens, especially in large vocabularies.\
SAPO uses higher temperature for negative tokens ($\\tau\_{\\text{neg}} > \\tau\_{\\text{pos}}$), which causes negative contributions to decay faster when off‑policy. Empirically, this simple asymmetry significantly improves RL training stability and performance.

## Experimental Results

### 1\. Controlled RL on Mathematical Reasoning (Qwen3‑30B‑A3B)

We compare SAPO against GSPO and GRPO‑R2 (GRPO with routing replay) using a cold‑start model fine-tuned from Qwen3-30B-A3B-Base.

Findings:

- SAPO maintains stable training longer than GSPO and GRPO‑R2.
- SAPO achieves higher final Pass@1 on AIME25, HMMT25, and BeyondAIME.
- SAPO does not require routing replay, simplifying RL pipelines.

![](https://qianwen-res.oss-accelerate-overseas.aliyuncs.com/SAPO/controlled_exp.png)

Temperature ablations confirm that:

- $\\tau\_{\\text{neg}} > \\tau\_{\\text{pos}}$ provides the most stable training
- Reversing this relationship causes significant instability

![](https://qianwen-res.oss-accelerate-overseas.aliyuncs.com/SAPO/temperature.png)

### 2\. Large‑Scale RL for Qwen3‑VL Models

SAPO consistently improves performance across models of varying sizes and across both MoE and dense architectures. For comparison, we train a preliminary cold-start checkpoint of Qwen3‑VL‑30B‑A3B on a mixture of math, coding, logic, and multimodal tasks. Evaluation benchmarks include:

- AIME25 (math)
- LiveCodeBench v6 (coding)
- ZebraLogic (logic)
- MathVision (multimodal math)

Results: SAPO consistently outperforms both GSPO and GRPO‑R2 under the same compute budget.

![](https://qianwen-res.oss-accelerate-overseas.aliyuncs.com/SAPO/vl_exp.png)

## What SAPO Means for the Future of RL-Trained LLMs

SAPO offers a practical way to stabilize and enhance RL training for LLMs:

- **Smooth gating** provides a continuous trust‑region mechanism, avoiding the brittleness and discontinuities associated with hard clipping.
- **Sequence coherence** ensures that updates remain aligned with sequence‑level behavior, yielding more interpretable optimization dynamics while still allowing token‑level flexibility.
- **Token‑level adaptivity** preserves informative gradients and improves sample efficiency, especially when only a subset of tokens are off‑policy.
- **Asymmetric temperature control** significantly enhances stability, reducing the impact of high‑variance negative‑advantage updates that commonly destabilize large‑scale LLM training.

As RL continues to drive frontier LLM capabilities, we expect that SAPO will become a foundational component of RL training pipelines.

## Want to Learn More?

For full technical details, theoretical analysis, and extensive experiments, please refer to our paper:

[Soft Adaptive Policy Optimization](https://arxiv.org/abs/2511.20347)

If you find our work helpful, feel free to cite it.

```
@article{sapo,
title={Soft Adaptive Policy Optimization},
author={Gao, Chang and Zheng, Chujie and Chen, Xiong-Hui and Dang, Kai and Liu, Shixuan and Yu, Bowen and Yang, An and Bai, Shuai and Zhou, Jingren and Lin, Junyang},
journal={arXiv preprint arXiv:2511.20347},
year={2025}
}
```
