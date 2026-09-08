---
title: 'FlashQLA: CP-/Bwd-Friendly Fused Linear Attention Kernels for GDN'
link: https://qwen.ai/blog?id=flashqla
source: qwen-ai-blog
published: 2026-04-28T02:00:00Z
first_seen: 2026-09-08T10:17:10.166315Z
authors:
- QwenTeam
labels:
- open-source
content: feed
html: 2026-04-28-flashqla-cp-bwd-friendly-fused-linear-attention-kernels-for.html
preview:
  file: 2026-04-28-flashqla-cp-bwd-friendly-fused-linear-attention-kernels-for.preview-bb776e53a795.webp
  width: 256
  height: 154
  color: '#a8b5fd'
images:
- source: https://img.alicdn.com/imgextra/i2/O1CN01IUOCqg1RG48rlBwP7_!!6000000002083-2-tps-1590-954.png
  original:
    file: 2026-04-28-flashqla-cp-bwd-friendly-fused-linear-attention-kernels-for.image-a27a10bf7594.png
    width: 1590
    height: 954
  variants:
  - file: 2026-04-28-flashqla-cp-bwd-friendly-fused-linear-attention-kernels-for.image-593b82fa0ad9.webp
    width: 48
    height: 29
  color: '#a6b7fd'
- source: https://qianwen-res.oss-cn-beijing.aliyuncs.com/flashqla/flashqla.png
  original:
    file: 2026-04-28-flashqla-cp-bwd-friendly-fused-linear-attention-kernels-for.image-4a30efb12ce1.png
    width: 1536
    height: 1024
  variants:
  - file: 2026-04-28-flashqla-cp-bwd-friendly-fused-linear-attention-kernels-for.image-5062056a58f1.webp
    width: 48
    height: 32
  - file: 2026-04-28-flashqla-cp-bwd-friendly-fused-linear-attention-kernels-for.image-5ce144085e95.webp
    width: 320
    height: 213
  - file: 2026-04-28-flashqla-cp-bwd-friendly-fused-linear-attention-kernels-for.image-c78db4d43d67.webp
    width: 640
    height: 427
  - file: 2026-04-28-flashqla-cp-bwd-friendly-fused-linear-attention-kernels-for.image-47ca2eb4893f.webp
    width: 960
    height: 640
  - file: 2026-04-28-flashqla-cp-bwd-friendly-fused-linear-attention-kernels-for.image-2c553ad806cc.webp
    width: 1280
    height: 853
  - file: 2026-04-28-flashqla-cp-bwd-friendly-fused-linear-attention-kernels-for.image-f50a9676a2af.webp
    width: 1536
    height: 1024
  color: '#fbfbfd'
- source: https://qianwen-res.oss-cn-beijing.aliyuncs.com/flashqla/fwd_bwd_latency_comparison.png
  original:
    file: 2026-04-28-flashqla-cp-bwd-friendly-fused-linear-attention-kernels-for.image-372985b80141.png
    width: 5818
    height: 2870
  variants:
  - file: 2026-04-28-flashqla-cp-bwd-friendly-fused-linear-attention-kernels-for.image-b57a9770f1fb.webp
    width: 48
    height: 24
  color: '#fbfbfb'
---

![](https://qianwen-res.oss-cn-beijing.aliyuncs.com/flashqla/flashqla.png)

[GitHub](https://github.com/QwenLM/FlashQLA)

## Introduction [#](https://qwen.ai/blog?id=flashqla#introduction)

Following the release of [Qwen3-Next](https://qwen.ai/blog?id=qwen3-next), Gated Delta Network (GDN) has become the workhorse attention layer across the Qwen family — from Qwen3-Next-80B-A3B all the way to the subsequent [Qwen3.5](https://qwen.ai/blog?id=qwen3.5) / [Qwen3.6](https://qwen.ai/blog?id=qwen3.6) series. As models scale to **397A17B / 122A10B / 35B / 27B** and context windows stretch beyond 256K, the overhead of the GDN block in end-to-end training and inference has become non-negligible.

Today we officially open-source **FlashQLA**: a high-performance linear attention kernel library built on [TileLang](https://github.com/tile-ai/tilelang). FlashQLA applies **reasonable operator fusion and performance optimization** to the forward and backward passes of GDN Chunked Prefill, achieving **2-3× forward speedup** and **2× backward speedup** over the FLA Triton kernel across multiple scenarios on NVIDIA Hopper. The efficiency gains are particularly pronounced in pretraining scenarios and edge-side agentic inference.

Key highlights of this release:

1. **Gate-driven automatic intra-card context parallelism**. By exploiting the exponential decay property of the GDN gate, FlashQLA automatically enables intra-card CP under TP, long-sequence, and small-head-count settings, improving GPU SM utilization.

2. **Hardware-friendly algebraic reformulation**. We reformulate the forward and backward flows of GDN Chunked Prefill to a certain extent, effectively reducing Tensor Core, CUDA Core, and SFU overhead without sacrificing numerical precision.

3. **TileLang fused warp-specialized kernels**. Rather than following the step-by-step decomposition into independent kernels, nor fusing the entire computation flow into a single kernel, we take CP and backward requirements into account, use TileLang to build several key fused kernels, and manually implement warpgroup specialization to overlap data movement, Tensor Core computation, and CUDA Core computation.

FlashQLA code and benchmarks are open-sourced at [github.com/QwenLM/FlashQLA](https://github.com/QwenLM/FlashQLA).

## Key Problems in FLA GDN Chunked Prefill [#](https://qwen.ai/blog?id=flashqla#key-problems-in-fla-gdn-chunked-prefill)

Let us first review the forward computation flow of GDN Chunked Prefill, taking chunk index $i$ as an example:

1. $A\_i \\gets \\left(I+\\mathrm{StrictLower}\\left( \\mathrm{diag}(\\beta\_i)(\\Gamma\_i \\odot K\_iK\_i^\\intercal) \\right)\\right)^{-1}$
2. $\\left\\{\\begin{aligned} W\_i &\\gets A\_i\\mathrm{diag}(\\beta\_i)\\mathrm{diag}(\\gamma\_i)K\_i \\\\ U\_i &\\gets A\_i\\mathrm{diag}(\\beta\_i)V\_i \\end{aligned}\\right.$
3. $\\left\\{\\begin{aligned} V\_i’ &\\gets U\_i-W\_iS\_i \\\\ S\_{i+1} &\\gets \\gamma\_{i,C-1}S\_i + K\_i^\\intercal\\mathrm{diag}\\left(\\frac{\\gamma\_{i,C-1}}{\\gamma\_i}\\right)V\_i’ \\end{aligned}\\right.$
4. $O\_i \\gets \\mathrm{diag}({\\gamma})Q\_iS\_i + \\left(\\mathrm{Lower}(\\Gamma\_i) \\odot Q\_iK\_i^\\intercal\\right)V\_i'$

Ignoring gate preprocessing and CP, each step of this flow corresponds to one kernel [in FLA](https://github.com/fla-org/flash-linear-attention/blob/v0.5.0/fla/ops/gated_delta_rule/chunk.py). This flow has two main efficiency problems:

1. Most of the above are **memory-bound kernels**. The flow repeatedly reads $K$, $V$ and other data, while $W$, $U$, $S$ as intermediate variables must be written to HBM and then read by the next kernel, incurring significant memory access overhead.
2. The recurrent nature of the SSM state means that the corresponding third step `chunk_gated_delta_rule_fwd_kernel` can only launch `batch_size * num_heads` thread blocks simultaneously, resulting in **low GPU utilization** in small-model, small-batch, or TP scenarios.

The solutions to these two problems are contradictory. For the first problem, the most intuitive solution is to write a [fully-fused kernel](https://github.com/flashinfer-ai/flashinfer/pull/2276), where all data is accessed only once and all intermediate variables are kept on-chip. When `batch_size * num_heads` is large enough, this is certainly optimal. However, such a solution obviously runs into the second problem: for edge-side inference with small models and `batch_size=1`, or for large-model online deployments with TP where long-sequence inputs from coding agents etc. cannot launch a large enough batch for chunked prefill, the speedup of a fully-fused kernel over the original FLA implementation is limited.

The earliest solution to the second problem comes from [how DeltaNet does context parallelism](https://yywangcs.notion.site/DeltaNet-2a9fc9f5d8058013a498f34e0b25bd52), which splits a long sequence into multiple sub-sequences, uses $S\_0=0$ to parallelize the recurrence, and then computes an additional $M$ matrix to correct the recurrent results. This scheme was later optimized to insert a step before the recurrence kernel to compute the $S\_0$ of each sub-sequence, and has now been [merged into the FLA repository](https://github.com/fla-org/flash-linear-attention/blob/main/fla/ops/cp/README.md). For CP rank $j$, the specific preprocessing flow is:

1. $\\left\\{\\begin{aligned} S^\\ast\_{j,i+1} &\\gets \\gamma\_{j,i,C-1}S^\\ast\_{j,i} + K\_{j,i}^\\intercal \\mathrm{diag}\\left(\\frac{\\gamma\_{j,i,C-1}}{\\gamma\_{j,i}}\\right) V’\_{j,i} \\\\ M\_{j,i+1} &\\gets \\left( \\gamma\_{j,i,C-1} I -K\_{j,i}^\\intercal \\mathrm{diag}\\left(\\frac{\\gamma\_{j,i,C-1}}{\\gamma\_{j,i}}\\right) W\_{j,i} \\right) M\_{j,i} \\end{aligned}\\right.$
2. $S\_{j,0} \\gets S^\\ast\_{j,0} + M\_{j,0}S\_{j-1,0}$

However, this CP scheme also has its drawbacks: first, it introduces significant extra computation, with the time complexity of recurrently computing the $M$ matrix even exceeding that of the $S$ matrix; second, it does not work well with fully-fused kernels, because matrix inversion and other steps must be performed before the $S\_0$ of each sub-sequence can be computed.

## A Balanced Solution: Fusing Kernels While Enabling Intra-Card CP [#](https://qwen.ai/blog?id=flashqla#a-balanced-solution-fusing-kernels-while-enabling-intra-card-cp)

Based on the two problems above, a compromise solution can be derived: split the GDN Chunked Prefill forward computation into two fused kernels, inserting CP-related preprocessing steps between them. After some transformations and simplifications, the following computation flow is obtained:

1. $A\_i \\gets \\left(I+\\mathrm{StrictLower}\\left( \\mathrm{diag}(\\beta\_i)K\_iK\_i^\\intercal\\right)\\right)^{-1}$
2. CP Preprocess
   - 2.1. $\\left\\{\\begin{aligned} X\_{j,i} &\\gets -\\beta\_{j,i} A\_{j,i}’^\\intercal K\_{j,i} \\\\ Y\_{j,i} &\\gets \\gamma\_{j,i,C-1} K\_{j,i} S^\\ast\_{j,i} - \\mathrm{diag}\\left(\\frac{\\gamma\_{j,i,C-1}}{\\gamma\_{j,i}}\\right) V\_{j,i} \\\\ Z\_{j,i} &\\gets K\_{j,i} M\_{j,i} \\\\ S^\\ast\_{j,i+1} &\\gets \\gamma\_{j,i,C-1}S^\\ast\_{j,i} + X\_{j,i}^\\intercal Y\_{j,i} \\\\ M\_{j,i+1} &\\gets \\gamma\_{j,i,C-1} \\left( M\_{j,i} + X\_{j,i}^\\intercal Z\_{j,i} \\right) \\end{aligned}\\right.$
   - 2.2. $S\_{j,0} \\gets S^\\ast\_{j,0} + M\_{j,0}S\_{j-1,0}$
3. $\\left\\{\\begin{aligned} V\_i^\\Delta &\\gets V\_i - \\mathrm{diag}(\\gamma\_i)K\_iS\_i \\\\ V\_i’ &\\gets \\left(\\Gamma\_i \\odot A\_i\\right)\\mathrm{diag}(\\beta\_i)V\_i^\\Delta \\\\ S\_{i+1} &\\gets \\gamma\_{i,C-1}S\_i + K\_i^\\intercal\\mathrm{diag}\\left(\\frac{\\gamma\_{i,C-1}}{\\gamma\_i}\\right)V\_i’ \\\\ O\_i &\\gets \\mathrm{diag}({\\gamma\_i})Q\_iS\_i + \\left(\\mathrm{Lower}(\\Gamma\_i) \\odot Q\_iK\_i^\\intercal\\right)V\_i’ \\end{aligned}\\right.$

We also designed a simple mathematical model to automatically determine the degree of parallelism. Let $N$ be the number of chunks in a sequence and $L$ be the number of chunks per CP rank. It is easy to see that the runtime of steps 2.1 and 3 is proportional to $L$, while the runtime of step 2.2 is proportional to $\\frac NL$; therefore we can choose $L=\\lambda \\sqrt N$ to minimize total time, where $\\lambda$ is a coefficient composed of `batch_size`, `num_heads`, and other hyperparameters.

In production, intra-card CP is not always needed. Following the original FLA implementation, step 3 can also increase parallelism by 2-4× via splitting `v_head_dim`, at the cost of redundant memory access to Q and K. Based on measured data, we enable CP only when `batch_size * num_heads <= 40` or `batch_size * num_heads <= 56 && seq_len >= 8192`.

## Further Optimization via Gate Decay [#](https://qwen.ai/blog?id=flashqla#further-optimization-via-gate-decay)

Revisiting the GDN recurrence:

$$S\_{i+1} = \\alpha\_iS\_i(I-\\beta\_ik\_ik\_i^\\intercal)+\\beta\_iv\_ik\_i^\\intercal$$

For $\\alpha\_i\\in(0,1)$, the influence of each $S\_i$ on subsequent states decays exponentially, giving it a sliding-window property. For a sufficiently long window size of $W$, starting computation from $S\_{i-W}=0$ can obtain the accurate $S\_i$, without the need to start from $S\_0$. We refer to this process as warmup. On real data, we find that $\\alpha\_i$ is not constantly 1 on 60–80% of linear attention heads, and **6–8 chunks of warmup** are sufficient to drive the $S\_i$ error below the noise floor.

Therefore, for linear attention heads with the sliding-window property, we can design a lighter CP preprocessing flow that discards the computation of the correction term $M$ and directly obtains an equally accurate sub-sequence $S\_0$ through warmup:

|    | C0 | C1 | C2 | C3 | C4 | C5 | C6 | C7 | C8 | C9 | C10 | C11 | C12 |
| -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | --- | --- | --- |
| R1 | O  | O  | O  | O  | O  |    |    |    |    |    |     |     |     |
| R2 |    |    |    | X  | X  | O  | O  | O  | O  |    |     |     |     |
| R3 |    |    |    |    |    |    |    | X  | X  | O  | O   | O   | O   |

`X` denotes warming up with a zero initial state until the gate has decayed sufficiently, then writing out the $S\_0$ of that CP rank; `O` denotes subsequent normal recurrent computation. The warmup length for each rank is determined by an independent kernel that collects gate statistics, and the cost of this step is negligible.

## TileLang Warp-Specialized Kernel [#](https://qwen.ai/blog?id=flashqla#tilelang-warp-specialized-kernel)

We implement FlashQLA in [TileLang](https://github.com/tile-ai/tilelang) using a warpgroup-specialization pattern: one producer warpgroup and three consumer warpgroups reside in the same SM, exchange data through shared memory, and synchronize via mbarriers.

### Forward [#](https://qwen.ai/blog?id=flashqla#forward)

In the forward pass, the three consumer warp groups compute $V’$, $S$, and $O$ respectively, overlapping computation and memory traffic through a ping-pong structure.

|           | WG3       | WG2                                         | WG1                                  | WG0                                           |                                                                                                          |                                  |
| --------- | --------- | ------------------------------------------- | ------------------------------------ | --------------------------------------------- | -------------------------------------------------------------------------------------------------------- | -------------------------------- |
| **BAR 0** | **LD**$Q$ | **LD**$\\gamma$                             | **ST**$O$                            | $\\gamma, \\gamma\_{C-1}\\gamma^{-1}$         | **TC**$P = Q K^\\intercal$                                                                               |                                  |
| **BAR 1** | **LD**$K$ | **LD**$\\beta$                              | **ST**$S\_i$                         | **TC**$U = K S\_i$                            | $\\Gamma = L(\\gamma I \\gamma^{-1})$\ $A\_\\gamma = \\Gamma \\odot A$\ $P\_\\gamma = s\\Gamma \\odot P$ | $S\_{i+1} = \\gamma\_{C-1} S\_i$ |
| **BAR 2** | **LD**$V$ |                                             | $W = \\beta (V - \\gamma U)$         | **TC**$O = Q S\_i$                            |                                                                                                          |                                  |
| **BAR 3** | **LD**$A$ |                                             | **TC**$V^\\Delta = A\_\\gamma W$     | $O = s\\gamma O$                              |                                                                                                          |                                  |
| **BAR 4** |           | $V’ = \\gamma\_{C-1}\\gamma^{-1} V^\\Delta$ | **TC**$O = O + P\_\\gamma V^\\Delta$ |                                               |                                                                                                          |                                  |
| **BAR 5** |           |                                             |                                      | **TC**$S\_{i+1} = S\_{i+1} + K^\\intercal V'$ |                                                                                                          |                                  |

Notes:

- $S$ output per chunk is for debugging only; normally only $O$ and the last chunk’s $S$ are output.

### CP Preprocessing [#](https://qwen.ai/blog?id=flashqla#cp-preprocessing)

As mentioned earlier, the CP preprocessing splits into two cases: the original approach (computing both $M$ and $S$) and the sliding-window approach (computing only $S$). We designed a single fused kernel that handles both:

|           | WG3       | WG2                                                       | WG1                                                       | WG0                                                                        |                                              |                                  |
| --------- | --------- | --------------------------------------------------------- | --------------------------------------------------------- | -------------------------------------------------------------------------- | -------------------------------------------- | -------------------------------- |
| **BAR 0** | **LD**$K$ | **LD**$\\gamma$                                           |                                                           | $\\gamma\_{C-1}\\gamma^{-1}$                                               | **TC**$X = A^\\intercal K$                   |                                  |
| **BAR 1** | **LD**$V$ | **LD**$\\beta$                                            | **ST**$S\_i$                                              | **TC**$U = K S\_i$\ $Y = -\\gamma\_{C-1}\\gamma^{-1} V + \\gamma\_{C-1} U$ | $X = -\\beta X$                              | $S\_{i+1} = \\gamma\_{C-1} S\_i$ |
| **BAR 2** | **LD**$A$ |                                                           | $\\gamma^\\pi = \\gamma^\\pi \\gamma\_{C-1}$              | $\\gamma^\\pi = \\gamma^\\pi \\gamma\_{C-1}$                               | **TC**$S\_{i+1} = S\_{i+1} + X^\\intercal Y$ |                                  |
| **BAR 3** |           | **TC**$Z^L = K M^L$\ **TC**$M^L = M^L + X^\\intercal Z^L$ | **TC**$Z^R = K M^R$\ **TC**$M^R = M^R + X^\\intercal Z^R$ |                                                                            |                                              |                                  |

Notes:

- The last two steps of WG1 and WG2 correspond to the $M$ matrix computation and are triggered only when required.
- $S$ is output per chunk only during backward recomputation.

### Backward [#](https://qwen.ai/blog?id=flashqla#backward)

In the backward pass, we reuse the CP preprocessing kernel from the previous section to recompute the $S$ matrix, then fuse `bwd_dv`, `bwd_dhu`, `bwd_dqkwg`, `bwd_wy` into a single kernel with corresponding algebraic optimizations. Because of on-chip resource constraints, the backward kernel does not use multi-stage pipelining; instead it relies on the long compute chain to hide memory traffic. The full schedule is available in the [FlashQLA repo](https://github.com/QwenLM/FlashQLA).

|            | WG3        | WG2                                                                                                        | WG1                                                                        | WG0                                                                                                                                                                                                 |                                                   |
| ---------- | ---------- | ---------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------- |
| **BAR 00** | **ST**$dK$ |                                                                                                            |                                                                            | **TC** $P=QK^\\intercal$                                                                                                                                                                            | $\\gamma, \\gamma\_{C-1}\\gamma^{-1}$             |
| **BAR 01** |            | **TC** $dV’=KdS\_{i+1}$\ $dV’=\\gamma\_{C-1}\\gamma^{-1}dV'$                                               | $\\Gamma=\\gamma I \\gamma^{-1}$\ $P\_\\gamma=sL(\\Gamma)\\odot P$         | $dS\_i = \\gamma\_{C-1} dS\_{i+1}$                                                                                                                                                                  |                                                   |
| **BAR 02** |            | **TC** $dV’=dV’+P\_\\gamma^\\intercal dO$                                                                  | $A\_\\beta = A \\beta$\ $A\_\\gamma = \\Gamma \\odot A\_\\beta$            |                                                                                                                                                                                                     |                                                   |
| **BAR 03** |            |                                                                                                            | **TC**$U=KS\_i$                                                            |                                                                                                                                                                                                     |                                                   |
| **BAR 04** |            | **TC** $dV=A\_\\gamma^\\intercal dV’$                                                                      | \ $W=V-\\gamma U$                                                          | $d\\gamma\_{C-1}=\\sum S\_i \\odot dS\_{i+1}$                                                                                                                                                       |                                                   |
| **BAR 05** | **ST**$dV$ | **LD**$V$                                                                                                  | $dV\_\\gamma = -\\gamma dV$\ $d\\gamma = \\sum\_i dV\_\\gamma \\odot U$    | **TC** $dA\_\\gamma = dV’W^T$\ **TC** $V’=A\_\\gamma W$                                                                                                                                             |                                                   |
| **BAR 06** |            |                                                                                                            | **TC** $dP\_\\gamma = dO V’^\\intercal$                                    |                                                                                                                                                                                                     |                                                   |
| **BAR 07** |            | **LD**$K$                                                                                                  | **TC** $dK=V’dS\_{i+1}^\\intercal$                                         | $dA\_\\beta = \\Gamma \\odot dA\_\\gamma$\ $d\\gamma = d\\gamma + \\sum\_i dP\_\\gamma \\odot L(P)$\ $d\\gamma = d\\gamma - \\sum\_j dP\_\\gamma \\odot L(P)$\ $dP = sL(\\Gamma)\\odot dP\_\\gamma$ |                                                   |
| **BAR 08** |            | $dK=\\gamma\_{C-1}\\gamma^{-1}dK$\ $d\\gamma\_{C-1}=\\sum K \\odot dK$\ $d\\gamma = -\\sum\_i K \\odot dK$ | **TC** $dQ=dOS\_i^T$                                                       |                                                                                                                                                                                                     |                                                   |
| **BAR 09** |            | **LD**$Q$                                                                                                  | **TC** $dK=dK+dV\_\\gamma S\_i^\\intercal$                                 | $dQ=s\\gamma dQ$\ $d\\gamma = \\sum Q \\odot dQ$                                                                                                                                                    |                                                   |
| **BAR 10** |            | **LD**$S$                                                                                                  |                                                                            | **TC** $dQ=dQ+dPK$                                                                                                                                                                                  |                                                   |
| **BAR 11** | **ST**$dQ$ |                                                                                                            |                                                                            | $d\\gamma = d\\gamma + \\sum\_i dA\_\\beta \\odot A \\beta$\ $d\\gamma = d\\gamma - \\sum\_j dA\_\\beta \\odot A \\beta$\ $d\\beta = \\sum\_j dA\_\\beta \\odot A$\ $dA=dA\_\\beta \\beta$          | **TC** $dS\_i = dS\_i + K^\\intercal dV\_\\gamma$ |
| **BAR 12** |            | **TC** $dK=dK+dP^\\intercal Q$                                                                             |                                                                            |                                                                                                                                                                                                     |                                                   |
| **BAR 13** |            |                                                                                                            | **TC** $dA = -A^\\intercal dA A^\\intercal$\ **TC** $A\_T = KK^\\intercal$ | $dO\_\\gamma=s\\gamma dO$                                                                                                                                                                           |                                                   |
| **BAR 14** |            | **LD**$dO$\ **LD**$A$                                                                                      |                                                                            | $d\\beta = d\\beta + \\sum\_i dA \\odot A\_T$\ $dA\_T = \\beta dA$\ $dA\_S = dA\_T + dA\_T^\\intercal$                                                                                              | **TC** $dS\_0 = dS\_0 + Q^\\intercal dO\_\\gamma$ |
| **BAR 15** |            | **TC** $dK=dK+dA\_S K$                                                                                     |                                                                            |                                                                                                                                                                                                     |                                                   |

## Benchmark [#](https://qwen.ai/blog?id=flashqla#benchmark)

We benchmarked FlashQLA against the FLA Triton and FlashInfer baseline (FLA 0.5.0, Triton 3.5.1, FlashInfer 0.6.9, TileLang 0.1.8) on the head configurations used by the Qwen3.5 / Qwen3.6 family — $h\_v \\in {64, 48, 32, 24, 16, 8}$, corresponding to TP1 through TP8.

![FlashQLA vs FLA and FlashInfer on H200](https://qianwen-res.oss-cn-beijing.aliyuncs.com/flashqla/fwd_bwd_latency_comparison.png)

Specifically, the forward (FWD) benchmarks measure single-kernel latency for different models and TP settings under varying batch lengths, while the backward (BWD) benchmarks examine the relationship between total token count within a batch and latency during a single update step.

Selected H200 single-layer forward results:

| Model / TP    | Seqlen     | $h\_{qk}$ | $h\_v$ | FlashQLA | FlashInfer | FLA     | vs FLA | vs FI |
| ------------- | ---------- | --------- | ------ | -------- | ---------- | ------- | ------ | ----- |
| 397B/122B TP8 | 1x32768    | 2         | 8      | 0.310ms  | 1.653ms    | 0.913ms | 2.95×  | 5.33× |
| 397B/122B TP8 | 1x16384    | 2         | 8      | 0.184ms  | 0.833ms    | 0.465ms | 2.53×  | 4.53× |
| 397B/122B TP8 | 24576+8192 | 2         | 8      | 0.302ms  | 1.242ms    | 0.767ms | 2.54×  | 4.11× |
| 397B/122B TP4 | 1x32768    | 4         | 16     | 0.486ms  | 1.654ms    | 1.250ms | 2.57×  | 3.40× |
| 397B/122B TP4 | 1x16384    | 4         | 16     | 0.292ms  | 0.832ms    | 0.623ms | 2.13×  | 2.85× |
| 27B TP2       | 1x32768    | 8         | 24     | 0.659ms  | 1.616ms    | 1.564ms | 2.37×  | 2.45× |
| 2B/0.8B TP1   | 1x32768    | 16        | 16     | 0.493ms  | 1.640ms    | 1.285ms | 2.60×  | 3.33× |
| Sym h32       | 1x32768    | 32        | 32     | 0.877ms  | 1.554ms    | 1.952ms | 2.23×  | 1.77× |

The speedup grows with TP degree because FlashQLA improves SM utilization via intra-card AutoCP in the exact regimes — TP sharding and small head number — where the baseline leaves SMs idle.

## Usage [#](https://qwen.ai/blog?id=flashqla#usage)

FlashQLA exposes both a high-level API matching FLA’s signature and low-level fwd/bwd entry points:

```python
import torch
from qla import chunk_gated_delta_rule

o, final_state = chunk_gated_delta_rule(
    q=q,                            # [B, T, H_q, K]
    k=k,                            # [B, T, H_q, K]
    v=v,                            # [B, T, H_v, V]
    g=g,                            # [B, T, H_v]
    beta=beta,                      # [B, T, H_v]
    scale=scale,
    initial_state=initial_state,    # optional, [B, H_v, K, V]
    output_final_state=True,
    cu_seqlens=cu_seqlens,          # optional, varlen support
)
```

Requirements: SM90, CUDA 12.8+, PyTorch 2.8+. Install:

```bash
git clone https://github.com/QwenLM/FlashQLA.git
cd FlashQLA && pip install -v .
```

## Acknowledgments [#](https://qwen.ai/blog?id=flashqla#acknowledgments)

FlashQLA is inspired by [Flash Linear Attention](https://github.com/fla-org/flash-linear-attention), [FlashInfer](https://github.com/flashinfer-ai/flashinfer) and [TileLang](https://github.com/tile-ai/tilelang). We thank these communities for the reference implementations.

## Citation [#](https://qwen.ai/blog?id=flashqla#citation)

If FlashQLA is useful for your research, please cite:

```bibtex
@misc{flashqla2026,
    title  = {FlashQLA: Flash Qwen Linear Attention},
    author = {Zhang, Chengruidong and Lin, Xi and Jiang, Huiqiang and Wang, Zekun and
              Li, Xiao and Cao, Yizhong and Zhuang, Bohan and Men, Rui and Zhang, Jianwei and
              Zheng, Bo and Lin, Junyang and Liu, Dayiheng and Zhou, Jingren},
    year   = {2026},
    publisher = {GitHub},
    howpublished = {\url{https://github.com/QwenLM/FlashQLA}}
}
```
