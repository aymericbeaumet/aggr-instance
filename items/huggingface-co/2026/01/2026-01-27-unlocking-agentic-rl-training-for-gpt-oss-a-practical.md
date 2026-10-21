---
title: 'Unlocking Agentic RL Training for GPT-OSS: A Practical Retrospective'
link: https://huggingface.co/blog/LinkedIn/gpt-oss-agentic-rl
source: huggingface-co
published: 2026-01-27T01:53:15Z
updated: 2026-01-27T01:53:15Z
first_seen: 2026-10-04T12:50:30.804479567Z
content: extracted
html: 2026-01-27-unlocking-agentic-rl-training-for-gpt-oss-a-practical.html
---

Agentic reinforcement learning (RL) extends traditional LLM training by optimizing not just a single-turn response, but an entire decision-making process learned through direct interaction with an environment during training. Unlike traditional single-turn reinforcement learning or offline preference-based methods that rely on static datasets, agentic RL trains policies by actively collecting on-policy data as the agent plans actions, invokes tools, observes outcomes, and adapts its behavior over multi-step trajectories in either simulated or real environments. This interaction-driven optimization assigns credit across long-horizon decisions, where intermediate choices such as query reformulation, tool selection, and execution order directly influence downstream success. Training follows an iterative closed loop in which the agent interacts with the environment to collect rollout trajectories, computes rewards over these trajectories, updates the policy based on observed outcomes, and then uses the updated policy to drive the next round of interaction and data collection such as GRPO or PPO algorithms..

LinkedIn is an AI-first company that's built agents to help professionals be more successful. In this setting, models must reason over incomplete information, interact with structured services, and adapt to evolving user intent across multiple steps rather than produce a single static response. These capabilities are especially critical for agents that support the goals of recruiters, job and knowledge seekers, and learners end users, such as retrieving information, refining queries, coordinating tools, and executing multi-step workflows. By learning robust decision policies through interaction, agentic RL provides a principled foundation for building scalable, reliable, and adaptable AI systems through end-to-end optimization.

The GPT-OSS model has shown comparable performance to OpenAI o3-mini and o4-mini \[[ref](https://openai.com/index/introducing-gpt-oss/)\], but its suitability for agentic reinforcement learning training has not yet been validated. Most recent work focuses on fine-tuning without tool calling, such as: [Fine-tuning with gpt-oss and Hugging Face Transformers](https://cookbook.openai.com/articles/gpt-oss/fine-tune-transfomers) and [unsloth tutorial: how to fine-tune gpt-oss](https://unsloth.ai/docs/models/gpt-oss-how-to-run-and-fine-tune/tutorial-how-to-fine-tune-gpt-oss). This blog explores the journey to unlock [agentic RL](https://github.com/volcengine/verl/issues/3794) training for GPT-OSS as a potential backbone model for agentic applications.

In our experiments, we use [verl](https://github.com/volcengine/verl) as our training framework since it is one of the most popular adopted frameworks in the open source community. We use gsm8k, [Retool](https://github.com/verl-project/verl-recipe/tree/21892b9276936efab5375c3f6b8415e472ef7118/retool) task, [verifiable instruction following task](https://arxiv.org/abs/2507.02833), which are commonly used in RL training. We focus on presenting experimental results for the GPT-OSS-20B model, and our attention-sink fix also works for GPT-OSS-120B. The Qwen-2.5-32B model is additionally used to benchmark standard metric trends during RL training.

## Challenges of GPT-OSS RL Training

verl has been an OSS framework used by the team, and the team has previously collaborated and contributed to it to help democratize agentic reinforcement learning training. With the introduction of the new Harmony chat template in GPT-OSS, the first step is to ensure that the training framework fully supports the updated message format and conversation semantics required by Harmony. This step helps rollout generation, trajectory construction, and tool parsing remain consistent and correct under the new template.

The team uses ReTool as a representative example to verify code correctness. ReTool is an agentic coding task in which the model is asked to solve a math problem with the assistance of a code compiler tool. This setup allows the model to focus on core reasoning and algorithmic logic, while delegating the actual arithmetic and execution to the tool. During an episode, the model interacts with the code tool multiple times, using execution results as feedback to refine its solution. At the end of the trajectory, the model produces a final answer, on which the reward is computed.

During the initial training runs, we observed exploding KL divergence and entropy, along with non-increasing rewards, indicating underlying issues in the GPT-OSS training setup, as shown in Figure 1.

| Average Gradient Norm                                                                                                                                                                                                                                      | Average Reward                                                                                                                                                                                                                                      |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [![Average gradient norm in a batch](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/5eWLHQ-EAWKjPc7T0c6CK.png)](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/5eWLHQ-EAWKjPc7T0c6CK.png) | [![Average reward in a batch](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/ZpBvRXLKyAT-PIdsuRi11.png)](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/ZpBvRXLKyAT-PIdsuRi11.png) |

**Figure 1.** Left: Qwen32b has significantly higher rewards compared to GPT-OSS 20B; Right: The gradient norm exploded as training progressed.

## A Practical Debugging Journey in verl: Restoring PPO On-Policy Integrity

### Restoring PPO On-Policy Integrity: A Fix for MoE Log-Probability Mismatch

![](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/21xIlXKeAlTK5qKSp-TkX.png)

**Figure 2.** Non-zero importance sampling clip value even for on-policy training.

We focus on on-policy methods because they provide greater stability and more reliable convergence. The foundation of pure on-policy Proximal Policy Optimization (PPO) mandates that the importance sampling ratio must be exactly 1. The mathematical definition of the importance ratio is:

ratio=π(a∣s)πold(a∣s) \\text{ratio} = \\frac{\\pi(a \\mid s)}{\\pi\_{\\text{old}}(a \\mid s)}

This requirement ensures that the policy update is executed only on the data generated by the current policy π(a | s) = πold(a | s), preventing unintended clipping.

We have observed the non-zero clipping value in our ReTool training, as shown in Figure 2, stemming from a mismatch between the two log-probabilities:

- Current log-probability `log_prob`: log(π(a | s))
- Old log-probability `old_log_prob`: log(πold(a | s))

**Root Cause: The Dual Forward Pass and MoE Architecture**

Prior to verl 0.3.0, the implementation relied on two separate forward passes (one to compute the current `log_prob` and one to retrieve the stored `old_log_prob`) for the same state-action pair.

In a Mixture of Experts (MoE) architecture like GPT-OSS, the gating network routes the input to different experts. Due to implementation factors (e.g., subtle floating-point differences or explicit stochasticity), the expert routing can differ slightly between the two passes. Readers who are interested can further read *[Stabilizing MoE Reinforcement Learning by Aligning Training and Inference Routers](https://arxiv.org/pdf/2510.11370v1)*.

This difference in routing leads to:

log⁡(π(a∣s))≠log⁡(πold(a∣s)) \\log(\\pi(a \\mid s)) \\neq \\log(\\pi\_{\\text{old}}(a \\mid s))

The resulting ratio deviates from 1, falsely triggering the PPO clip and violating the core on-policy assumption.

**Solution: Enforcing Ratio = 1 via Log-Probability Substitution**

The fix resolves the issue by logically overriding the flawed computation when the environment is known to be on-policy (i.e., when the minibatch size equals the global batch size):

```python
if on_policy:
    old_log_prob = log_prob.detach()
else:
    old_log_prob = model_inputs["old_log_probs"]
```

By setting `old_log_prob` equal to the newly computed `log_prob` (detached to prevent gradient flow through the reference value), the importance ratio is mathematically forced back to 1. This strategy bypasses the instability caused by MoE's non-deterministic routing and guarantees strict on-policy behavior during PPO training.

* * *

### Correcting Training–Inference Mismatch

Although fixing the log-probability mismatch reduced the importance-sampling clip ratio to zero, gradient norms continued to explode and rewards failed to improve. To isolate the issue, we simplified training to GSM8K, a single-step task without agentic tool use. The same instability persisted, as shown in the green curves in Figure 3, indicating a **fundamental issue in basic RL training with GPT-OSS under verl.**

We hypothesize that **training–inference mismatch** could be a potential cause: discrepancies between inference-time execution—where engines such as vLLM and SGLang aggressively optimize for throughput—and training-time execution under FSDP, which prioritizes numerical precision and stability, can effectively turn otherwise **on-policy RL into off-policy optimization.**

This [blog](https://yingru.notion.site/When-Speed-Kills-Stability-Demystifying-RL-Collapse-from-the-Training-Inference-Mismatch-271211a558b7808d8b12d403fd15edda) details why such mismatches lead to unstable gradients and non-improving rewards. Figure 3 compares training runs with and without rollout correction (see this [verl blog](https://verl.readthedocs.io/en/latest/algo/rollout_corr.html) for details). After applying rollout correction, training dynamics improve significantly, with gradient norms remaining stable rather than exploding.

However, as shown in the left plot of Figure 4, the reward increases only modestly, and convergence on the simple GSM8K task remains substantially slower compared to smaller dense model variants.

| Average Entropy                                                                                               | Average Gradient Norm                                                                                         | Average KL Loss                                                                                               |
| ------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| ![](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/MoBnatEipCi_OScgm5fAJ.png) | ![](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/k4Su03zqq6Cg-7jM6Il6J.png) | ![](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/g77U9c8a-FY1rvwru96Ez.png) |

**Figure 3.** Gradient norm behavior under different training configurations. Green: Training without rollout correction, exhibiting unstable gradients. Red: Training with the attention layer frozen to isolate the issue to the attention mechanism, resulting in partial stabilization. Blue: Training with rollout correction enabled (sequence-level importance sampling), yielding stable gradient norms.

| Average Reward                                                                                                                                                                                                                                      | Max Log-Perplexity Difference                                                                                                                                                                                                                                                                                              |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [![Average reward in a batch](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/ihx-XsWH51V0-JM46jODE.png)](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/ihx-XsWH51V0-JM46jODE.png) | [![Maximum absolute log-perplexity difference in a batch between rollout policy and training policy](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/fP5KNR2XYY7EH-muYBk_X.png)](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/fP5KNR2XYY7EH-muYBk_X.png) |

**Figure 4.** Left: Reward improvement on GSM8K remains slow even after applying rollout correction, with performance comparable to runs where the attention layer is frozen during training. Right: A substantial log-ppl mismatch is observed between the inference engine (SGLang with Triton kernels supporting attention-sink forward passes) and the training stack (FSDP with FlashAttention-v2), indicating a large training–inference inconsistency.

To further isolate the root cause, we freeze the attention layers during training and observe reward dynamics similar to those of runs without freezing (blue curve vs yellow curve in Figure 4). This indicates that learning is primarily driven by the MoE layers, while the attention mechanism contributes less effectively than expected. In addition, we observe a substantial token-level probability mismatch between the inference engine and the distributed training stack which are using different attention kernels. Together, these observations motivate a deeper investigation into the attention mechanism.

* * *

## Attention Sink Support in FlashAttentionV3

Attention sinks used in GPT-OSS are learnable scalar parameters (one per attention head) that act as "virtual tokens" in the softmax computation. They allow the model to allocate attention mass to a learned sink rather than forcing all attention to content tokens, which has been shown to improve attention stability in streaming inference and training with sliding-window attention.

After a deeper investigation, we identified several major issues:

- verl hard-codes FlashAttention v2 in `fsdp_worker`, which does not support attention sinks.
- The attention sink backward pass is not supported in FlashAttention v2 and v3, so it does not work as expected even when FlashAttention v3 is enabled.
- Since the forward pass has not yet been merged into the original FlashAttention v3 repository, we leveraged the forward pass from the vLLM FlashAttention fork ([PR #75](https://github.com/vllm-project/flash-attention/pull/75)) and implemented the backward pass to compute the sink gradient.

### Standard Attention

```python
scores = QK^T / sqrt(d)               # [B, H, N_q, N_k]
probs = softmax(scores, dim=-1)       # Σ_j P_ij = 1
output = probs @ V                   # [B, H, N_q, d_v]
```

### Attention with Sinks (GPT-OSS)

```python
scores = QK^T / sqrt(d)                               # [B, H, N_q, N_k]
combined = concat([scores, sink_param], dim=-1)       # [B, H, N_q, N_k+1]
probs = softmax(combined, dim=-1)                     # Σ_j P_ij + P_sink = 1
probs_content = probs[..., :-1]                       # Drop sink component
output = probs_content @ V                           # [B, H, N_q, d_v]
```

**Key difference:** The sink participates in softmax normalization but doesn't contribute to the output.

### Mathematical Formulation

The attention weight for content token j in row i is defined as:

Pij=exp⁡(Sij)∑j′=1Nkexp⁡(Sij′)+exp⁡(Sh) P\_{ij} = \\frac{\\exp(S\_{ij})} {\\sum\_{j'=1}^{N\_k} \\exp(S\_{ij'}) + \\exp(S\_h)}

**Where:**

- Sij = Qi Kj⊤ / √d are the attention scores
- Pij are the attention weights for the content tokens
- Sh is the learnable sink parameter for head h

**Sink Probability:**

The sink probability is computed but not used in the output:

Pi,h=exp⁡(Sh)∑j′=1Nkexp⁡(Sij′)+exp⁡(Sh) P\_{i,h} = \\frac{\\exp(S\_h)} {\\sum\_{j'=1}^{N\_k} \\exp(S\_{ij'}) + \\exp(S\_h)}

### Backward Pass

The gradient of the loss L with respect to the sink parameter Sh is:

∂L∂Sh=−∑iPi,h(∂L∂Si,h−∑j∈{1,…,Nk}Pij∂L∂Sij) \\frac{\\partial L}{\\partial S\_h} = - \\sum\_i P\_{i,h} \\left( \\frac{\\partial L}{\\partial S\_{i,h}} - \\sum\_{j \\in \\{1,\\ldots,N\_k\\}} P\_{ij} \\frac{\\partial L}{\\partial S\_{ij}} \\right)

**Where:**

- Pi,h is the sink attention probability for row i
- ∂L/∂Sij is the gradient with respect to the attention scores, including the sink

**Simplified Gradient:**

Since the sink is computed but not used in the output, its gradient ∂L/∂Si,h = 0.

Therefore, the backward equation simplifies to:

∂L∂Sh=−∑iPi,h(∑j∈{1,…,Nk}Pij∂L∂Sij) \\frac{\\partial L}{\\partial S\_h} = - \\sum\_i P\_{i,h} \\left( \\sum\_{j \\in \\{1,\\ldots,N\_k\\}} P\_{ij} \\frac{\\partial L}{\\partial S\_{ij}} \\right)

The forward pass was adapted from vLLM's FlashAttention fork, and we implemented the backward pass to compute gradients for the sink parameters. The implementation will be released following the internal review process.

* * *

### Results

After applying the fix in FlashAttention v3, we observe substantially faster convergence for GPT-OSS-20B across a range of reinforcement learning tasks. These include single-turn RL on math reasoning (GSM8K — red curve in Figure 5), instruction following (VerifyIf, evaluated on an out-of-domain multi-if benchmark — Figure 6), and multi-turn agentic RL with tool use (ReTool — Figure 7).

Across all settings, training becomes stable and exhibits steady reward improvement.

![](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/6TYGevydK99nQ-I1QTouf.png)

**Figure 5.**. Single Turn GSM8K, the red curve converges much faster than the rest without the fix

| Average Entropy                                                                                                                                                                                                                                      | Average Gradient Norm                                                                                                                                                                                                                                      | Average Reward                                                                                                                                                                                                                                      |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [![Average entropy in a batch](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/ydmBLCSGlD9YKWiIocI1S.png)](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/ydmBLCSGlD9YKWiIocI1S.png) | [![Average gradient norm in a batch](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/_kl8mn_CXPsRYJ467IbFs.png)](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/_kl8mn_CXPsRYJ467IbFs.png) | [![Average reward in a batch](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/vb2JmmSu-LI5szC_84KsM.png)](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/vb2JmmSu-LI5szC_84KsM.png) |

**Figure 6**. On verifiable instruction following the task, the run without the fix collapsed (blue), and the run with fix showed steady reward improvement.

| Average Gradient Norm                                                                                                                                                                                                                                      | Average Reward                                                                                                                                                                                                                                      | Validation Accuracy                                                                                                                                                                                                                                                |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [![Average gradient norm in a batch](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/_Xz7_RLhYuYhzGAeMjDXs.png)](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/_Xz7_RLhYuYhzGAeMjDXs.png) | [![Average reward in a batch](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/uFX-sTWI6knecIf56uahk.png)](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/uFX-sTWI6knecIf56uahk.png) | [![val score accuracy mean@30 for aime_2025](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/5x2mdkpHcdvctZ96yfv58.png)](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/5x2mdkpHcdvctZ96yfv58.png) |

**Figure 7**. On the Retool task, the run with fix showed steady reward improvement and no gradient exploding (fa2 is the flash attention 2 without the fix while fa3 is the flash attention 3 with the fix). After the fix, the validation accuracy score goes up now.

## Memory-Efficient Training

### Mitigating FSDP Memory Blow-Ups Caused by Repeated MoE Expert Materialization

One issue we consistently encountered was excessive memory allocation during the FSDP forward pass, which led to repeated out-of-memory (OOM) failures when training GPT-OSS-20B bf16 models on 16 H200 nodes (max response length: 16k, prompt length: 8k). This behavior is highly unexpected for a 20B-parameter MoE model.

```text
2025-11-27T11:15:27.927Z [36m(TaskRunner pid=32081)[0m File "/home/jobuser/.local/lib/python3.10/site-packages/transformers/models/gpt_oss/modeling_gpt_oss.py", line 123, in forward
2025-11-27T11:15:27.927Z [36m(TaskRunner pid=32081)[0m hidden_states = hidden_states.repeat(num_experts, 1)
2025-11-27T11:15:27.927Z [36m(TaskRunner pid=32081)[0m torch.OutOfMemoryError: CUDA out of memory. Tried to allocate 180.00 GiB. GPU 0 has a total capacity of 139.72 GiB of which 110.94 GiB is free. Process 685851 has 24.88 GiB memory in use. Process 692458 has 3.87 GiB memory in use. Of the allocated memory 23.28 GiB is allocated by PyTorch, and 84.43 MiB is reserved by PyTorch but unallocated.
```

We identified the issue as originating from two different implementations of the MoE forward path in Hugging Face Transformers. This issue has also been reported by other users: [https://github.com/huggingface/transformers/issues/40073](https://github.com/huggingface/transformers/issues/40073); When verl computes log-probabilities under FSDP, the inference forward path is triggered. In the current Hugging Face implementation, this path duplicates hidden states for all experts and performs batched matrix multiplication, materializing extremely large tensors in GPU memory. By contrast, the training forward path uses a for-loop to process each expert sequentially and then combines the results. While slower, this approach is significantly more memory efficient.

```python
    @GPUMemoryLogger(role="dp actor", logger=logger)
    def compute_log_prob(self, data: DataProto, calculate_entropy=False) -> torch.Tensor:
        """
        ....
        """
        # set to eval, this essentially prioritizes parallelism at the cost of memory efficiency
        self.actor_module.eval()
        ...
```

We patched the Hugging Face implementation to use a more memory-efficient execution path, avoiding repeated materialization of experts.

### Sequence Parallel with Flash Attention V3

Agentic RL requires the agent to interact with the environment over multiple steps while maintaining an ever-expanding context. Observations and environment feedback from each step are appended to the context and used as input for subsequent decision-making, which introduces significant challenges for memory efficiency and scalability during training.

Under fully sharded data parallelism (FSDP), model parameters, optimizer states, and gradients are sharded across the entire world size (i.e., all GPUs in the training cluster). Each GPU stores and updates only its assigned parameter shards, while rollout data are replicated across all GPUs—meaning every GPU processes the full agent interaction history for each rollout.

During the forward pass, when computation reaches a layer whose parameters are not locally available, an `all_gather` operation is triggered to materialize the full parameters across GPUs. During the backward pass, a corresponding `reduce_scatter` operation aggregates gradients and ensures that each GPU retains only its local shard. This provides a degree of scaling: as the number of GPUs increases, the per-GPU memory footprint decreases.

FSDP provides model-level scaling by sharding model parameters, gradients, and optimizer states across GPUs. Sequence parallelism (or context parallelism) further reduces per-GPU memory consumption by partitioning the input sequence across devices, thereby lowering the peak activation memory on each GPU.

As the number of sequence-parallel dimensions increases, the maximum activation memory per GPU correspondingly decreases. We have implemented sequence parallelism to be attention-sink-aware and compatible with FlashAttention v3 (Figure 8, right).

[![SP  (2)](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/ryT_y9BpbFSdMDxNYlVlK.png)](https://cdn-uploads.huggingface.co/production/uploads/64efbd469e7770db74cb72f5/ryT_y9BpbFSdMDxNYlVlK.png)

**Figure 8**. Left: Inference without sequence parallelism. Right: Inference with sequence parallelism, where additional all-to-all communication is performed before and after the attention layer. This partitions the sequence across parallel workers and reduces the peak memory footprint of attention computation by a factor proportional to the sequence-parallelism degree.

Sequence parallelism scales along the sequence dimension to reduce the per-GPU activation footprint. Input tokens from all sequences are packed into a single contiguous list by removing padding tokens, while position IDs are used to distinguish tokens belonging to different sequences. This design naturally benefits from FlashAttention’s variable-length support. For sequence parallelism, layers other than the attention layer do not have inter-position dependencies; therefore, they do not require each GPU to hold a complete sequence shard, and no additional communication is needed for these layers.

The attention layer, however, requires all tokens belonging to the same sequence to be present on the same GPU in order to compute attention weights correctly. To satisfy this constraint, an all-to-all communication is performed to gather sequence elements, with the split performed at the attention-head level. This design avoids communication within the attention computation itself, which would otherwise be prohibitively expensive. After the attention layer, a single all-to-all communication redistributes the outputs back to their original sequence-parallel layout, after which the remaining non-attention layers can proceed without further synchronization.

## Conclusion

Our journey to enable agentic RL training for the GPT-OSS backbone model was a practical retrospective, highlighting that unlocking advanced capabilities in open-source LLMs requires meticulous, deep-dive engineering.

We made contributions that transformed the viability of GPT-OSS for agentic applications, specifically by:

- **Stabilizing PPO:** We contributed a fix to restore on-policy integrity, overriding the log-probability mismatch caused by the MoE architecture’s non-determinism (Figure 2).

- **Enabling Attention Sink Support:** We successfully implemented and integrated the attention sink backward pass into FlashAttention v3, correcting the catastrophic training–inference mismatch that had previously caused instability and slow convergence (Figures 5, 6, and 7).

- **Scaling Memory Efficiency:** We introduced crucial memory optimizations, including patching the MoE materialization process and integrating sequence parallelism with the new attention sink support, enabling training with the long context windows essential for multi-step agents (Figure 8).

These engineering efforts validate GPT-OSS as a scalable and high-performance backbone for building the next generation of intelligent, multi-step decision-making agents.

## Acknowledgments

Thanks to Deepak Agarwal, Bee-Chung Chen, Animesh Singh, Gungor Polatkan, Balaji Krishnapuram, and Jitendra Agarwal for their leadership support.

## References

1. Feng, Jiazhan, et al. *Retool: Reinforcement Learning for Strategic Tool Use in LLMs.* arXiv preprint arXiv:2504.11536 (2025).

2. Xiao, Guangxuan, et al. *Efficient Streaming Language Models with Attention Sinks.* arXiv preprint arXiv:2309.17453 (2023).

3. When Speed Kills Stability: Demystifying RL Collapse from the Training–Inference Mismatch.\
   [https://yingru.notion.site/When-Speed-Kills-Stability-Demystifying-RL-Collapse-from-the-Training-Inference-Mismatch-271211a558b7808d8b12d403fd15edda](https://yingru.notion.site/When-Speed-Kills-Stability-Demystifying-RL-Collapse-from-the-Training-Inference-Mismatch-271211a558b7808d8b12d403fd15edda)
