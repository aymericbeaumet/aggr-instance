---
title: 'How We Built TLX: A Hardware-Native GPU Compiler for Production ML at Scale'
link: https://engineering.fb.com/2026/11/10/open-source/tlx-a-hardware-native-gpu-compiler-for-production-ml/
source: engineering-fb-com
published: 2026-11-10T16:59:31Z
updated: 2026-11-10T16:59:31Z
first_seen: 2026-11-27T00:01:05.630507547Z
labels:
- devinfra
- ml applications
- open source
- production engineering
summary: 'Triton Low-Level Extensions (TLX), is an extension to Triton that helps engineers optimize GPU performance while preserving Triton’s productive programming model. TLX introduces explicit warp-group and cluster-level orchestration, letting developers adopt new hardware capabilities without sacrificing velocity. TLX has demonstrated performance improvements in RecSys training, RecSys inference, and large-scale LLM pretraining. Modern GPU architectures increasingly [...] Read More... The post How We Built TLX: A Hardware-Native GPU Compiler for Production ML at Scale appeared first on Engineering at Meta.'
content: extracted
html: 2026-11-10-how-we-built-tlx-a-hardware-native-gpu-compiler-for.html
remote_preview:
  url: https://engineering.fb.com/wp-content/uploads/2026/11/Meta-TLX-hero.png
---

- [Triton Low-Level Extensions (TLX)](https://arxiv.org/abs/2605.10905), is an extension to Triton that helps engineers optimize GPU performance while preserving Triton’s productive programming model.
- TLX introduces explicit warp-group and cluster-level orchestration, letting developers adopt new hardware capabilities without sacrificing velocity.
- TLX has demonstrated performance improvements in RecSys training, RecSys inference, and large-scale LLM pretraining.

Modern GPU architectures increasingly rely on execution-oriented features such as warp specialization, asynchronous memory movement, tensor memory, and cluster-level execution to optimize their performance. But these same features also expose a growing gap between low-level programming models for control and high-level programming models for productivity. To bridge this gap, we built Triton Low-Level Extensions (TLX), an extension that gives engineers explicit control over GPU execution while preserving Triton’s productive programming model.

Today, TLX powers production workloads across Meta, from AI training to large-scale LLM pretraining, enabling engineers to adopt new hardware capabilities quickly without sacrificing developer velocity. Across NVIDIA and AMD GPUs, TLX is pushing the frontier of GPU performance — combining state-of-the-art kernels with explicit execution orchestration and model-aware fusion to deliver end-to-end improvements beyond what standalone libraries can achieve.

## From AI Models to GPU Kernels

To understand where TLX fits, it helps to trace the path from an AI model to the GPU instructions that ultimately execute it. At Meta, machine learning (ML) workloads are built on a common software stack centered around PyTorch. Model code is transformed through graph capture, optimization, compilation, and lowering before eventually becoming GPU kernels running across large accelerator fleets. Over the past several years, Triton has evolved from an experimental kernel language into a foundational component of this stack. A significant share of production GPU kernels — both compiler-generated and hand-optimized — are now implemented in Triton. Improvements at this layer can therefore propagate broadly across training and inference workloads, making kernel performance an increasingly important lever for improving end-to-end system efficiency.

![](https://engineering.fb.com/wp-content/uploads/2026/11/Meta-TLX-Figure-1.png)\
Figure 1: Normalized kernel count growth at Meta over a 10-month period (start = 1.0 in month 1). Over this period, Triton kernel count grew ~3.8x compared with ~2.0x for CUDA, reflecting Triton’s rapidly expanding role in Meta’s GPU software stack.

This shift has also changed who writes kernels. Kernel optimization is no longer the exclusive domain of GPU specialists. ML engineers routinely profile workloads, identify bottlenecks, and optimize the hottest kernels through autotuning, fusion, memory-layout improvements, or algorithmic restructuring. Triton’s productivity allows many of these iterations to happen in hours rather than days, enabling engineers to stay close to the model-development workflow. As a result, GPU kernel development has increasingly become part of the standard optimization toolkit for ML engineers.

Triton already provides a productive foundation for many memory-bound workloads through fusion and compiler-managed memory-access optimizations. Building on these strengths, TLX addresses a complementary challenge: coordinating execution groups, asynchronous data movement, and shared resources on modern GPUs.

## The Problem: When Hardware Evolves Faster Than Programming Models

While Triton dramatically improved developer productivity, modern GPU architectures have evolved in ways that place increasing emphasis on execution orchestration rather than computation alone. NVIDIA’s Hopper introduced warp specialization and the Tensor Memory Accelerator (TMA). NVIDIA’s Blackwell GPU expanded this trend further with Tensor Memory (TMEM), cluster-level execution, paired-CTA execution, and increasingly asynchronous execution mechanisms. These capabilities are not simply new instructions that can be hidden behind compiler optimizations, they fundamentally change how high-performance kernels are organized.

For example, a modern FlashAttention kernel may dedicate one warp group to issuing TMA transfers while another executes tensor-core computation. A Blackwell GEMM kernel may coordinate multiple warp groups, TMEM allocations, and cluster resources simultaneously. In both cases, performance depends not only on the computation being performed, but also on how multiple execution agents cooperate.

As we adopted Hopper and Blackwell across Meta’s ML infrastructure, we repeatedly encountered the same challenge: CUDA could express these execution structures, but doing so often required large amounts of architecture-specific orchestration code. Triton provided a dramatically more productive programming model, but many of the execution relationships required by modern hardware remained hidden behind compiler abstractions.

The result was a growing compiler catch-up problem. Each new hardware generation introduced new execution capabilities, while programmers either waited for compiler support to arrive or dropped into increasingly specialized implementations. We realized that the challenge was not simply missing compiler features, it was the absence of an appropriate programming abstraction.

## The Missing Middle: Why SIMT and SIMB Weren’t Enough

The capabilities that matter most on recent GPUs operate neither at thread granularity nor at block granularity. Warp specialization partitions work across cooperating warp groups. TMA introduces dedicated agents for memory movement. TMEM enables new communication patterns between execution groups. Cluster execution lets multiple CTAs cooperate within a shared execution context. Organizing a kernel around these features means reasoning about how several execution agents cooperate, communicate, and synchronize — a level of structure that sits between CUDA and Triton.

This exposed what we came to view as a missing abstraction. CUDA’s SIMT model operates at too fine a granularity: It provides complete control, but requires developers to express every execution relationship manually. Triton’s blocked programming model operates at too coarse a granularity: It improves productivity, but turns many execution structures into implicit compiler decisions. Modern hardware increasingly requires something in between.

We refer to this model as Multiple Instructions, Multiple Warps (MIMW). MIMW elevates the warp group to the fundamental unit of orchestration. Rather than reasoning about individual threads or entire blocks, programmers express how multiple execution agents cooperate, communicate, synchronize, and share resources. This is the natural level at which many Hopper and Blackwell optimizations are expressed, and it is the abstraction layer TLX implements.

Figure 2 illustrates the evolution of GPU programming models. CUDA’s SIMT model exposed execution directly at thread granularity, giving developers complete control over scheduling, synchronization, and memory movement. Triton moved toward a SIMB model, raising the abstraction to blocked tensor computations and improving productivity. MIMW occupies the space between these two extremes. Rather than treating threads or tensor blocks as the primary execution abstraction, it elevates warp groups to first-class execution agents.

![](https://engineering.fb.com/wp-content/uploads/2026/11/Meta-TLX-Figure-2.png)\
Figure 2: GPU architecture and execution model evolution. Recent NVIDIA GPUs evolved from mostly synchronous SIMT-style execution toward increasingly specialized and asynchronous execution, including Tensor Cores, Async Copy, TMA, WGMMA, TMEM, register reallocation, and paired-CTA MMA. This hardware evolution makes warp-group orchestration a programming-model concern rather than merely a compiler optimization.

The emergence of MIMW is not a software-design preference but a consequence of how hardware has evolved. Volta and Ampere exposed tensor cores through thread- and warp-level programming. Hopper made asynchronous data movement (enabled byTMA) and warp-group execution (enabled by WGMMA) central to high-performance kernels. Blackwell extends this trend further with TMEM, register reallocation, cluster execution, and paired-CTA MMA. Each generation pushes more cooperation across warp groups — and sometimes across CTAs — into territory that the programming model, not just the compiler, must address.

![](https://engineering.fb.com/wp-content/uploads/2026/11/Meta-TLX-Figure-3.png)\
Figure 3: MIMW occupies the missing middle between SIMT’s fine-grained thread control and SIMB’s block-level productivity. TLX implements MIMW by adding explicit warp-group orchestration to Triton’s productive base.

Making MIMW practical requires expressing cooperating warp or wave groups as first-class execution tasks, with explicit control over shared-memory buffer access, synchronization, and handoff between them. TLX brings these capabilities into Triton’s existing tensor programming model, letting developers combine explicit execution control with familiar high-level tensor operations.

## Introducing TLX: The Missing Middle, Implemented

We wanted  to avoid creating yet another standalone low-level language with TLX. Instead, TLX extends Triton with a second layer of execution-oriented constructs. Programmers continue to express computation using familiar Triton tensor operations, while TLX is used only where explicit control over execution, memory movement, or synchronization is required. In other words, Triton answers *what* computation should be performed, while TLX answers *how* that computation should be orchestrated. The difference is easiest to see through a simple tiled matrix multiplication. In standard Triton, execution follows a synchronous load-compute loop: Each tile is loaded, consumed, and then discarded before the next tile begins.

```none
# Standard Triton: synchronous load-compute loop semantics, 
# with compiler-managed software pipelining  
@triton.jit
def matmul_kernel(a_ptr, b_ptr, c_ptr, M, N, K, ...):
    pid_m = tl.program_id(0)
    pid_n = tl.program_id(1)
    acc = tl.zeros((BLOCK_M, BLOCK_N), dtype=tl.float32)
    for k in range(0, K, BLOCK_K):
        a = tl.load(a_ptr + ...)   # Load A tile from global memory
        b = tl.load(b_ptr + ...)   # Load B tile from global memory
        acc += tl.dot(a, b)        # Compute matmul 
    tl.store(c_ptr + ..., acc)
```

TLX preserves the same computation but makes the execution structure explicit. Producer warp groups move data asynchronously, consumer warp groups perform tensor-core computation, and synchronization is expressed directly through lightweight barriers. The computation itself remains Triton; only the orchestration layer changes. The result is a deeply pipelined kernel that overlaps memory movement, computation, and output processing without leaving Python or sacrificing Triton’s autotuning infrastructure. More importantly, the execution structure is visible in the source code rather than hidden inside compiler heuristics.

```none
# TLX on Blackwell: explicit warp-group orchestration and pipelining 
@triton.jit
def matmul_kernel(desc_a, desc_b, desc_c, M, N, K, ...):
    buffers_a = tlx.local_alloc((BLOCK_M, BLOCK_K), tl.float16, NUM_STAGES)
    buffers_b = tlx.local_alloc((BLOCK_K, BLOCK_N), tl.float16, NUM_STAGES)
    acc = tlx.local_alloc((BLOCK_M, BLOCK_N), tl.float32, 1, tlx.storage_kind.tmem)
    load_bars = tlx.alloc_barriers(num_barriers=NUM_STAGES, arrive_count=1)
    dot_bars  = tlx.alloc_barriers(num_barriers=NUM_STAGES, arrive_count=1)
    with tlx.async_tasks():
        # Warp group 0: PRODUCER — async TMA loads into shared memory
        with tlx.async_task("default"):
             # Wait for last matmul
            tlx.barrier_wait(dot_bars[num_k_tiles - 1], phase)  
            result = tlx.local_load(acc[0])
            desc_c.store(offsets, result.to(tl.float16))
            
        # Warp group 1: CONSUMER — tensor-core matmul from shared memory
        with tlx.async_task(num_warps=1):
            for k in range(num_k_tiles):
                buf = k % NUM_STAGES
                 # Wait for TMA load to complete
                tlx.barrier_wait(load_bars[buf], phase)      
                tlx.async_dot(buffers_a[buf], buffers_b[buf], acc[0],
                              use_acc=(k > 0), mBarriers=[dot_bars[buf]])
         
        # Warp group 2: EPILOGUE — store result to global memory
        with tlx.async_task(num_warps=1):
            for k in range(num_k_tiles):
                buf = k % NUM_STAGES
                 # Wait for consumer to release buffer
                tlx.barrier_wait(dot_bars[buf], phase)       
                tlx.barrier_expect_bytes(load_bars[buf], ...)
                tlx.async_descriptor_load(desc_a, buffers_a[buf], offsets, load_bars[buf])
                tlx.async_descriptor_load(desc_b, buffers_b[buf], offsets, load_bars[buf])
```

TLX extends Triton in five key areas:

**Warp-group orchestration:** TLX treats warp groups as first-class execution agents. Programmers can assign explicit roles to cooperating warp groups — for example, producer warp groups that move data, consumer warp groups that execute tensor-core computation, and epilogue warp groups that process results.

**Synchronization**: TLX exposes synchronization at the warp-group level rather than the thread level. Programmers express dependencies between cooperating execution groups — for example, producer-consumer relationships in a software pipeline — using hardware primitives such as mbarriers. Fine-grained thread-level synchronization remains handled by the underlying execution model and compiler, letting developers focus on coordinating larger execution structures that increasingly determine performance on modern GPUs.

**Memory orchestration:** TLX provides direct access to hardware capabilities such as TMA, asynchronous tensor-core operations, and TMEM. These primitives allow memory movement and computation to overlap naturally within the execution structure defined by the program, while avoiding much of the low-level setup and bookkeeping typically required in CUDA.

**Cluster execution:** TLX exposes cluster-level execution as a first-class programming construct. Multiple CTAs can cooperate through distributed shared memory and cluster-wide synchronization primitives, enabling on-chip communication patterns that would otherwise require global-memory round trips.

**Optional layout**: Unlike CUDA and CUTLASS, TLX does not require programmers to explicitly specify layouts before computation can be expressed. The compiler infers reasonable defaults automatically, and programmers only specify layouts when they want to override them for performance reasons. A correct kernel can therefore be written without any layout annotations at all, with layout tuning introduced incrementally during optimization.

An important consequence is that kernel fusion becomes significantly easier. Fusion is one of the most effective GPU optimizations because it eliminates intermediate memory traffic and kernel launch overhead. In traditional CUDA-style workflows, however, fusion often becomes dominated by layout management. Each stage may assume a different tiling, swizzling, or shared-memory mapping, forcing programmers to manually reconcile layouts across operation boundaries.

Because TLX infers layouts automatically, operations can often be fused without manually reconciling intermediate layouts. The compiler resolves layout compatibility where possible, and programmers intervene only when a particular layout choice improves performance. This significantly lowers the engineering resources needed for building complex fused kernels and enables multi-stage pipelines that would otherwise require multiple kernels or substantial layout-specific infrastructure.

Taken together, these capabilities allow TLX to occupy the space between Triton’s productivity and CUDA’s control. Developers retain the advantages of Python, autotuning, and blocked tensor programming while gaining direct access to the execution structures that increasingly determine performance on modern GPUs.

## TLX in Production: Real-World Impact

We’ve been running TLX in production across Meta’s ML infrastructure for the past several months. Three deployments — spanning RecSys training, RecSys inference, and large-scale LLM pretraining — highlight different aspects of what TLX enables.

|                                                                                                                                                                                      |                  |                                                                                                                                                      |                                                                                      |                                            |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ | ------------------------------------------ |
| **Case study**                                                                                                                                                                       | **Hardware**     | **Problem TLX Addresses**                                                                                                                            | **Headline Results**                                                                 | **Deployment**                             |
| [**GEM**](https://engineering.fb.com/2025/11/10/ml-applications/metas-generative-ads-model-gem-the-central-brain-accelerating-ads-recommendation-ai-innovation/) **Model Training** | NVIDIA           | Reaching roofline MFU on heterogeneous RecSys foundation models needs warp specialization and explicit tensor-memory control beyond standard Triton. | 20-25% E2E MFU on thousands of GPUs                                                  | GEM training stack.                        |
| **In-Kernel Broadcast Optimization (IKBO)**                                                                                                                                          | NVIDIA Hopper    | User features replicated many times per candidate batch, wasting bandwidth and compute; eliminating it needs warp-specialized multi-stage fusion.    | Up to 2/3 net-latency reduction in compute-intensive latency for co-designed models. | RecSys inference, early→late ranking, GPU. |
| **Distributed Grouped GEMM for LLM Pretraining**                                                                                                                                     | NVIDIA Blackwell | Expert-parallel MoE layers require grouped GEMM with inter-node communication; existing implementations leave performance on the table at scale.     | Production-validated at large-fleet scale over an extended period.                   | LLM pretraining.                           |

### Case Study 1: GEM Model Training

Training Meta’s [Generative Ads Model (GEM)](https://engineering.fb.com/2025/11/10/ml-applications/metas-generative-ads-model-gem-the-central-brain-accelerating-ads-recommendation-ai-innovation/) is more difficult to optimize than LLM training because smaller input shapes, 10+ norms per layer, heterogeneous module types, and architectural diversity all limit MFU. Through a systematic model-kernel co-design approach spanning model architecture, custom kernels, compiler optimizations, and hardware-specific tuning, GEM training, in its latest version, reached 20-25% E2E MFU on the latest GPUs.

#### TLX’s Role in the Multi-DSL Strategy

We evaluated three programming models on Blackwell — CUTLASS, CuTeDSL, and TLX — and found that TLX provides the best balance between productivity and performance for most kernels. It combines Triton-like simplicity, autotuning, and composability with direct access to Blackwell-specific capabilities such as async\_dot, tensor memory operations, and mbarrier synchronization.

For Generalized Dot Product Attention (GDPA), a kernel in GEM, TLX achieved near-optimal performance with a fraction of the implementation complexity, speeding up training by targeting at unifying the models’ many attention-like operations into a single GPU kernel, delivering up to 2x forward speedup. While different kernels impose different requirements, TLX proved to be the right abstraction for the majority of GEM workloads where both developer velocity and performance matter.

TLX enabled us to move beyond optimizing individual kernels toward kernelizing and fusing larger portions of the model. For GEM workloads, reducing memory movement often provides greater benefits than optimizing compute alone, making composability and fusion first-class concerns. To accelerate this process, we built Agent for Compute Efficiency (ACE), an AI-assisted optimization framework that automates profiling, analysis, and tuning. ACE reduced the optimization cycle for a kernel, allowing engineers to focus on higher-level optimization opportunities rather than low-level performance debugging.

### Case Study 2: In-Kernel Broadcast Optimization for RecSys

In RecSys inference, user features are identical across all scored candidates, yet they are typically replicated dozens of times per batch to match the candidate dimension. Validated across a broad spectrum of candidate-to-user broadcast ratios, this broadcast becomes a significant source of wasted memory bandwidth and compute, with overhead that scales linearly with the number of candidates.

In-Kernel Broadcast Optimization (IKBO) eliminates this inefficiency by moving the broadcast into the kernel itself. Rather than materializing replicated user features, kernels consume user and candidate inputs at their natural, mismatched batch sizes and perform the broadcast implicitly during computation. This approach is enabled through a co-design of the model architecture, inference runtime, and kernel implementation.

#### TLX Provides a Building Block for the Final Stage of This Optimization

Consider our linear compression kernel. After a series of optimizations — including matmul decomposition, memory alignment, and broadcast fusion — the remaining bottleneck is addressed through a warp-specialized, multi-stage TLX kernel that delivers a 4x speedup on the NVIDIA H100. The final implementation collapses the entire computation into a single persistent mega-kernel. Producer and consumer warp groups execute concurrently using ping-pong scheduling, while user and candidate GEMMs share circular buffers and synchronize through mbarriers without draining the pipeline between stages. Tile-level release-acquire synchronization further enables coordination across CTAs.

For FlashAttention, IKBO co-design shifts the kernel from I/O-bound to compute-bound by amortizing K/V reads across candidates sharing the same user context. TLX then layers on FA3-style warp specialization with persistence, eliminating cross-CTA launch stalls that dominate at RecSys’s short query lengths and delivering up to a two-thirds reduction in compute-intensive net latency for co-designed models. Expressing this execution structure directly in CUDA would require substantial manual orchestration; TLX allows it to be expressed concisely within Triton’s Python programming model and autotuning framework.

Today, IKBO helps optimize [Meta’s Adaptive Ranking Model](https://engineering.fb.com/2026/03/31/ml-applications/meta-adaptive-ranking-model-bending-the-inference-scaling-curve-to-serve-llm-scale-models-for-ads/) across both GPUs and the [Meta Training and Inference Accelerator (MTIA)](https://ai.meta.com/blog/meta-mtia-scale-ai-chips-for-billions/). More broadly, it illustrates a shift in how we approach RecSys inference. Rather than optimizing around broadcast overhead, we eliminate it at the computational primitive itself, enabling dense interaction quality regardless of candidate count.

### Case Study 3: Distributed Grouped GEMM for LLM Pretraining

TLX’s production impact extends beyond recommendation workloads. A TLX-based distributed grouped GEMM kernel is currently deployed in Meta’s large-scale LLM pretraining infrastructure, where it has been operating continuously across a large GPU fleet for an extended training period. The kernel accelerates the grouped matrix multiplications required by mixture-of-experts (MoE) models, while tightly integrating computation and communication to reduce end-to-end latency.

At this scale, peak throughput is only one part of the equation. Reliability, debuggability, and long-term performance stability are equally important. Production kernels must sustain performance over weeks of continuous operation while remaining maintainable as models, hardware, and system software evolve.

TLX proved particularly well suited for this environment. Its support for warp-specialized execution, explicit memory orchestration, and asynchronous pipelines enabled the performance characteristics required by large-scale distributed training. At the same time, its integration with Triton’s Python programming model preserved the developer productivity needed to iterate, validate, and deploy complex kernels at production velocity.

This deployment highlights an important aspect of TLX’s design philosophy: It is not intended solely for achieving peak kernel performance, but for making advanced hardware capabilities practical to adopt in real production systems.

## Take a Deeper Look Into TLX Kernels

For deeper technical dives into TLX kernels, see our companion PyTorch blog posts:

- [In-Kernel Broadcast Optimization: Co-Designing Kernels for RecSys Inference](https://pytorch.org/blog/in-kernel-broadcast-optimization-co-designing-kernels-for-recsys-inference).
- [TLX Block Attention: A Warp-Specialized Blackwell Kernel for Fixed-Block Sparse Self-Attention](https://pytorch.org/blog/tlx-block-attention-a-warp-specialized-blackwell-kernel-for-fixed-block-sparse-self-attention).
- [Fast 2-Simplicial Attention: Hardware-Efficient Kernels in TLX](https://pytorch.org/blog/fast-2-simplicial-attention-hardware-efficient-kernels-in-tlx/).
- [Enabling Cluster Launch Control with TLX](https://pytorch.org/blog/enabling-cluster-launch-control-with-tlx/).
- [Achieving Free Normalization: Fusing Norms into GEMM and Attention via TLX](https://pytorch.org/blog/towards-free-normalization-fusing-normalization-into-gemm-and-attention-kernels/).
- [Optimizing Jagged Flash Attention with TLX: The Road Toward SOTA FA4 on Blackwell](https://pytorch.org/blog/optimizing-jagged-flash-attention-with-tlx-the-road-toward-sota-fa4-on-blackwell/%20).
- [Optimizing GEMM + Activation on CDNA4 with TLX](https://www.amd.com/en/developer/resources/technical-articles/2026/optimizing-gemm-with-tlx.html).

## What We Learned: Advice for Building GPU Compilers

### Start With a Real Workload, Not a Benchmark

We built TLX to solve actual production problems at Meta rather than to win benchmark contests. Both the GEM MFU project and IKBO illustrate this principle. Each began with a concrete pain point (low MFU in GEM, broadcast overhead in RecSys), and the tooling followed. This focus kept us oriented toward real-world performance rather than synthetic scores.

### Productivity and Performance Are Multipliers, Not Trade-Offs

TLX typically delivers  performance on-par with hand-tuned CUDA performance, but engineers can iterate significantly faster. A kernel that is marginally faster but takes three times as long to write is a net loss at fleet scale. With GEM, the multi-DSL approach let us select TLX for rapid iteration; with IKBO, the TLX-based implementation took weeks rather than months.

### Hardware Features Are Only Useful if They Are Accessible

WGMMA, TMA, and TMEM are powerful capabilities, but they saw little use because the programming model was too complex. TLX makes them accessible through concise annotations. The warp-specialized fusion in IKBO and the multi-DSL kernels in GEM would have been prohibitively complex in raw CUDA.

### The Compiler Is Part of the User Interface

Error messages, debugging support, and profiling integration matter as much as code-generation quality. We invested as much in the developer experience as in the optimizer. When GEM engineers debugged MFU issues or IKBO engineers encountered warp-group synchronization problems, they required clear feedback rather than obscure compiler errors.

### Evolution Outperforms Revolution

By building TLX as an extension to Triton, we leveraged the existing ecosystem, tooling, and expertise. The GEM team could use TLX alongside CUTLASS and CuTeDSL; the IKBO team, already familiar with Triton, needed only to learn the TLX annotations for performance-critical sections. A clean-slate design might have been more elegant, but might never have shipped.

## What’s Next for TLX

TLX is going further. Here’s what we’re working on:

- **More Hardware**: Right now, TLX targets NVIDIA Hopper and Blackwell, and AMD Mi300 and Mi350x. We’re working on support for more architectures, including new generations from NVIDIA and AMD, and custom accelerators like MTIA. The MIMW model is hardware-agnostic — the challenge is mapping it efficiently to different ISAs.
- **Higher-Level Abstractions**: TLX gives developers control over warp groups. Next, we want to raise the abstraction level again — automatically inferring warp-group orchestration from higher-level patterns. Think “auto-TLX” that writes the annotations for you. The patterns we learned from GEM (multi-DSL selection) and IKBO (three-stage warp specialization) are candidates for automation.
- **Integration with PyTorch 2.x**: We’re working on making TLX a backend for torch.compile. The vision is to write PyTorch code and get TLX-optimized kernels automatically, with no manual annotation required for common patterns. GEM shows what’s possible with systematic co-design, and IKBO shows the value of pattern-specific optimization. We want to make both automatic.

The missing middle is real, and it’s not just for GPUs. As accelerators get more complex, we need programming models that expose the right level of control without sacrificing productivity. TLX is our first step in that direction.

## Acknowledgements

*TLX is a collaboration across the Triton, PyTorch, MSL, Ads Training Infra, Ads Ranking, RecSys, and AI Compilers teams at Meta. We’d like to thank the following people for their contributions:*

***Triton:*** *Shane Nay, Manman Ren, Oleksandr Stashuk, Karthik Manivannan, Zhijing Li, Yuanwei Fang, Shunting Zhang, Arda Unal, Yueming Hao, Taylor Robie, Puyan Lotfi, Corbin Robeck, Bert Maher, Alexey Loginov, Ying Liu, Bill Yoshimi.*

***PG:*** *Jackie Xu, Jacky Zhou, Han Xu, Hao Yan, Jake Siso, Devashish Shankar, Shikai Li, Jianyu Huang, Andrew Tulloch, Boda Li, Jiao Jian, Zhuoran Zhao, Ao Cai, Shuyao Bi, Yuanjuan Yao, Ruizhen Guo, Doris Xiang, Jason Lu, Marvin Fan, Hai Zheng, Zhaokang Ke, Albert Chen, Mingyu Guan*

***UCSD:*** *Yufei Ding and Yue Guan.*
