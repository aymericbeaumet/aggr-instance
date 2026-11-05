---
title: Baidu released ERNIE 5.5 Flash Base
link: https://ernie.baidu.com/release/ernie-5-5-flash-base/
source: hnrss-org
published: 2026-11-05T09:29:12Z
updated: 2026-11-05T09:29:12Z
first_seen: 2026-11-05T17:57:26.867095589Z
authors:
- pretext
content: extracted
html: 2026-11-05-baidu-released-ernie-5-5-flash-base.html
remote_preview:
  url: https://ernie.baidu.com/release/ernie-5-5-flash-base/architecture.png
  alt: 'ERNIE 5.5 Flash architecture: 43-layer backbone, hybrid sparse attention, Multi-Router Latent MoE, mHC, MultiMax, ECHO, and R2L tokenizer.'
---

An efficient starting point for what comes next

Table of Contents

## 1\. Introduction

A base model is valuable both for the capabilities that pre-training builds and for the starting point it offers for further training. Different domains, reasoning tasks and agentic applications call for different data, training objectives and behaviors. Releasing the weights at the end of pre-training lets developers build on the general capabilities already in place and choose for themselves where to take the model next.

Today we are releasing **ERNIE-5.5-Flash-Base**, a sparse Mixture-of-Experts (MoE) language model with **280B** total parameters, **14B** of which are activated per token. It was pre-trained from scratch on **33.5T tokens** with a training context of up to **128K**, and supports a **1M-token** context for fine-tuning and inference. The weights are the checkpoint at the end of pre-training, before any mid- or post-training, and can serve as a common starting point for domain adaptation, instruction tuning and reinforcement-learning research.

To be worth building on, a base model has to stay affordable to train and run, so efficiency was a design goal from the outset. Further training samples from the model repeatedly, and agentic applications bring multi-turn interactions and long contexts. Both add to compute and memory costs. ERNIE 5.5 Flash therefore revisits the trade-off between capability and efficiency in every component, from attention and residual connections to the MoE layers and the output head, and weighs activated compute, KV cache and long-context cost together rather than one at a time.

The same thinking shaped the data and the training system. Every later stage of training builds on what pre-training provides, so the corpus emphasizes capabilities that are hard to add afterwards, such as repository-scale code and long-context understanding. And because an efficient architecture pays off only if it can also be trained efficiently at scale, we designed the model and the training system together.

We hope this release spares more developers the cost of large-scale pre-training, so they can spend their own resources on the capabilities, tasks and training methods they care about.

Key highlights:

- **An open starting point for further training.** Base weights with no mid- or post-training, leaving the community free to pursue its own approaches to domain adaptation, reasoning and agent training.
- **Built for efficient further training and inference.** Sparse MoE and hybrid sparse attention balance model capacity, activated compute and long-context cost in the architecture itself.
- **Broad data, efficient training.** Diverse, high-quality pre-training data gives later training a well-rounded foundation, and a training system co-designed with the architecture keeps large-scale pre-training efficient.
- **A clean research baseline.** A single checkpoint taken at the end of pre-training makes it easy to compare training methods under controlled conditions and to measure what each one adds.

## 2\. Model Architecture

[![ERNIE 5.5 Flash architecture: 43-layer backbone, hybrid sparse attention, Multi-Router Latent MoE, mHC, MultiMax, ECHO, and R2L tokenizer.](https://ernie.baidu.com/release/ernie-5-5-flash-base/architecture.png)](https://ernie.baidu.com/release/ernie-5-5-flash-base/architecture.png) \
ERNIE 5.5 Flash model architectureClick to view full image ↗

ERNIE 5.5 Flash has 43 Transformer layers, a hidden size of 4096 and 64 attention heads.

### 2.1 Latent-Gated Sparse Attention

**Hybrid MLA + HCA attention:** Most layers need only a coarse view of the distant past, so only one layer in seven keeps token-level access to the full context, using Multi-head Latent Attention (MLA). The remaining layers use Heavily Compressed Attention (HCA): each compresses every 128 tokens into a single cached entry, then attends to all of these entries plus a short window of recent, uncompressed tokens. An HCA layer therefore still sees the whole sequence, while caching only a small fraction of what a full-context layer needs.

**Layer-specific head dimensions:** Instead of using the same head dimension in every layer, we set it according to how expensive each layer's attention is: cheap layers get wide heads, expensive layers get narrower ones. HCA layers attend over a sequence compressed 128×, so their attention is cheap and they can afford wide 512-dim heads at little extra compute. MLA layers attend at token level and cost more, so they use narrower 256-dim heads for both query/key and value.

**Concentrating the KV-cache budget on MLA layers:** With HCA in most layers, the main question is where to spend the rest of the KV-cache budget. One option is many Compressed Sparse Attention (CSA) layers, which compress the history 4× and attend only to a selected part of it. We instead use a few MLA layers that keep every token. In our pre-training comparisons at a similar KV-cache budget, HCA combined with MLA outperforms HCA combined with CSA.

**Sparse attention in the full-context layers:** The MLA layers are the only layers that see every distant token individually. With dense attention, they would account for most of the attention compute at long context lengths, so they use sparse attention (DSA) instead. A lightweight indexer scores past positions for each query, and core attention is computed only over the top-k highest-scoring positions. The per-query cost of core attention therefore stays constant as the context grows. With DSA, these layers match dense attention in pre-training loss and train substantially faster.

Two further changes apply inside the attention layers:

- **Low-Rank Gated Attention:** Gated attention multiplies the attention output by a learned gate, which has been shown to improve both quality and training stability. Normally the gate is computed from the layer's hidden state. We instead compute it from the low-rank query latent, a compressed form of the query that each attention layer already computes. Because this latent is 4× narrower than the hidden state, the gate projection has 75% fewer weights, and our ablations also show better performance. Every attention layer in ERNIE 5.5 Flash, MLA and HCA alike, uses Low-Rank Gated Attention.
- **[Virtual Head Attention (VHA)](https://arxiv.org/abs/2610.17959):** After attention, VHA adds a lightweight low-rank mixing step across heads, with an identity skip connection. This lets heads share information, which improves expressiveness and training quality, and at inference it is fused into the attention kernel with negligible overhead.

### 2.2 Multi-Router Latent MoE

A conventional MoE layer picks experts with a single router and a hard top-k. We find that **a single router's scores correlate only weakly with how useful each expert actually is, especially in the middle and deeper layers**. And because experts left out of the top-k receive no gradient from that token, the router gets no signal that it chose wrongly, so wrong selections tend to reinforce themselves.

[Multi-Router](https://arxiv.org/abs/2610.40577) replaces the single router with several independently initialized sigmoid routers that score the experts jointly. **The routers develop clearly distinct preferences**: an expert can be chosen because one router strongly favors it or because several favor it together, so a wrong choice is easier to correct. The errors of any one router no longer decide the selection, and routing becomes less sensitive to noisy features and distribution shift. The number of activated experts, expert FLOPs and communication volume stay the same. Compared with a single router, **routing scores correlate noticeably better with how useful each expert is**, with the largest gains in the middle and deeper layers.

All layers except the first use an MoE feed-forward block:

- **512 routed experts (top-10) plus 1 shared expert**, with auxiliary-loss-free, bias-corrected load balancing.
- **Latent experts.** Each MoE layer first projects tokens from 4096 to a 2048-d latent space; the experts (intermediate size 2048) run in that space, and a single shared up-projection maps the result back to 4096. This roughly halves dispatch/combine traffic and expert FLOPs relative to full-width experts.
- **Stable LatentMoE design with learnable scalars.** The MoE blocks follow the Stable LatentMoE design (an RMSNorm before the up-projection and SiTU-GLU expert activations), which keeps expert outputs well scaled across all 43 layers and stabilizes optimization. In addition, each routed expert's routing weight is multiplied by its own learnable scalar.

### 2.3 Manifold-Constrained Hyper-Connections (mHC)

ERNIE 5.5 Flash replaces the standard residual connection with mHC: each layer reads from and writes to 4 parallel residual streams through a constrained mixing matrix, which gives layers a wider channel for passing information to one another at negligible extra compute.

### 2.4 MultiMax Output Head

A single softmax temperature forces a trade-off: lowering it makes the distribution sparse but also suppresses other plausible tokens; raising it keeps them but fattens the long tail of implausible ones. MultiMax replaces the single temperature with a learnable piecewise temperature, so the output head no longer has to choose: it suppresses the long tail of implausible tokens while keeping several plausible next tokens. The logits are rescaled element-wise inside the chunked, Triton-fused cross-entropy kernel just before the softmax. This adds negligible training overhead and no extra memory, and significantly lowers pre-training loss.

### 2.5 ECHO: Replacing the MTP Module

Conventional multi-token prediction (MTP) attaches extra modules to the backbone to predict future tokens, so the loss on future tokens mostly trains those modules, and the backbone is still trained mainly to predict the next token. ERNIE 5.5 Flash replaces the MTP module with [ECHO](https://arxiv.org/abs/2610.24766), which **reuses the backbone's own final layer** to predict future tokens. Because there is no separate MTP layer, the loss on future tokens directly trains the same parameters that produce the next token. Learning to look past the next token encourages the backbone to anticipate where the sequence is heading, with clear gains on reasoning and code. ERNIE 5.5 Flash is pre-trained with single-step ECHO. The same layer also serves as the draft model for speculative decoding, and it generalizes beyond its training: although trained to predict only one extra token, it drafts several tokens ahead, with a higher acceptance rate than an auxiliary MTP module trained the same way.

### 2.6 Tokenizer

ERNIE 5.5 Flash uses a new BPE tokenizer with a vocabulary of about 200K entries. Numbers are tokenized **right-to-left (R2L) in groups of up to three digits**, so that digits with the same place value line up across numbers of different lengths (e.g. 1,234,567 → `1` `234` `567`). This helps with arithmetic and number-heavy text.

## 3\. Pre-training

ERNIE-5.5-Flash-Base was pre-trained from scratch on **33.5T tokens** with the Muon optimizer and FP8 mixed precision. The training context was gradually extended to **128K tokens** over the course of pre-training, and fine-tuning and inference support a **1M-token** context. The released checkpoint is taken at the end of pre-training and has not gone through any mid-training or post-training.

### 3.1 Data

The pre-training corpus covers a wide range of domains and languages and is curated for both breadth of knowledge and quality. Beyond this broad base, we focused on three areas:

- **Web data.** Multilingual web data is crawled at scale, extracted with high fidelity and filtered by models that score quality along several dimensions. The result covers a broad range of knowledge and is dense in useful training signal.
- **Code data.** Code collected from repositories and the web is filtered to remove non-code content, aggressively deduplicated and graded for quality at several granularities. Beyond static code, the corpus includes data on how software evolves and executes: commits, issues, pull requests, code reviews, requirement-to-PR and multi-commit trajectories, generated tests and CI-verified fixes. This data targets repository-level reasoning and agentic software engineering.
- **Long-context data.** Three complementary sources: natural long documents, selected for long-range dependencies and cleaned with embedding-based deduplication, LLM quality scoring and rules; long sequences built by concatenating natural texts with retrieved, semantically similar distractors; and synthetic data for MRCR-style retrieval, citation-grounded QA and multi-hop QA.

### 3.2 Training Infrastructure

We built two infrastructure components for this run: a distributed Muon optimizer that stays efficient across thousands of GPUs, and [FlashMask-4](https://github.com/PaddlePaddle/FlashMask), an attention kernel for large head dimensions.

**Distributed Muon.** Unlike element-wise optimizers such as Adam, Muon orthogonalizes the update of each parameter matrix as a whole. This changes how distributed training communicates and how memory is spread across GPUs. Two optimizations make it practical at the scale of thousands of GPUs:

- **Communication-efficient updates.** Conventionally, dense parameters are synchronized over a single ring that passes through every GPU in the cluster. Data travels across a long chain of inter-node links in many small messages, which leaves the links underused, and latency and jitter grow as the cluster gets larger. MoE parameters are synchronized differently: each GPU exchanges data only with the GPUs that hold the same rank on other nodes. We move dense-parameter synchronization onto this same path and combine the dense and MoE transfers into one collective operation. Messages become larger, so bandwidth is used more fully; the communication pattern becomes more regular and fits expert-parallel communication; and inter-node latency and jitter drop significantly. The optimizer's updates stay exactly the same.
- **Memory-balanced sharding.** Muon needs large messages to communicate efficiently, so naive sharding groups many parameter tensors together and places them on the first few GPUs. A handful of nodes then hold far more optimizer state than the rest. They are prone to OOM failures and slow to save, upload and download checkpoints, which lengthens failure recovery, and the problem grows with GPU count. Our inter-node balanced sharding spreads these optimizer states over more GPUs, including those later in the order. Memory is then balanced almost evenly across nodes, while messages stay large enough for efficient communication.

**FlashMask-4 for large head dimensions.** At large head dimensions, FlashAttention-style kernels run up against the limits of on-chip storage such as shared memory and tensor memory: compute utilization drops, and some configurations are hard to implement at all. FlashMask-4 is built on CuTe-DSL for both SM90 and SM100 GPUs. It works around these limits with 2-CTA tiled parallel computation and, on SM100, double-buffer rotation in tensor memory. It keeps FlashMask's compact O(N) column-wise mask representation and still skips fully masked blocks. It supports head dimensions up to qk\_head\_dim = 576 and v\_head\_dim = 512, beyond what FlashAttention-4 supports. On SM100 GPUs, it is up to 175% faster in the forward pass and up to 74% faster in the backward pass than FlashAttention-4 MaskMod.

## 4\. Evaluation

### 4.1 Setup

For comparison, we selected the latest open-source base models of similar size publicly available at the time of release: DeepSeek-V4-Flash-Base (284B-A13B), Hy3-preview-Base (295B-A21A) and MiMo-V2.5-Base (310B-A15B). All four models are evaluated from their publicly released checkpoints with the same evaluation harness, prompts and few-shot examples, and with the same LLM judge for free-form QA.

### 4.2 Overall Results

[![Overall comparison of ERNIE 5.5 Flash Base, DeepSeek V4 Flash Base, Hy3 preview Base, and MiMo V2.5 Base across 30 benchmarks in seven categories. Full scores are provided in Detailed Results.](https://ernie.baidu.com/release/ernie-5-5-flash-base/overall-results.png)](https://ernie.baidu.com/release/ernie-5-5-flash-base/overall-results.png) \
ERNIE 5.5 Flash Base overall evaluation resultsClick to view full image ↗

ERNIE-5.5-Flash-Base performs strongly across all benchmark categories, with reasoning, code and exam benchmarks among its best results. Although 37 of the 43 layers attend over a sequence compressed 128×, the six MLA-DSA layers are enough to keep performance strong on the six long-context benchmarks.

### 4.3 Detailed Results

Complete evaluation of ERNIE 5.5 Flash Base and comparable base models
| Benchmark (Metric) | \# Shots | ERNIE 5.5 Flash Base | DeepSeek V4 Flash Base | Hy3 preview Base | MiMo V2.5 Base |
| ------------------ | -------- | -------------------- | ---------------------- | ---------------- | -------------- |
| -                  | 280B     | 284B                 | 295B                   | 310B             |                |
| -                  | 14B      | 13B                  | 21B                    | 15B              |                |
| 5-shot             | **93.3** | 89.4                 | 87.5                   | 87.6             |                |
| 5-shot             | **94.1** | 90.9                 | 88.7                   | 89.4             |                |
| 5-shot             | **78.4** | 69.2                 | 66.9                   | 65.7             |                |
| 5-shot             | **90.0** | 89.7                 | 89.3                   | 87.7             |                |
| 10-shot            | **34.1** | 29.1                 | 25.9                   | 20.8             |                |
| 10-shot            | 73.0     | **75.3**             | 73.2                   | 68.8             |                |
| 10-shot            | **59.3** | 52.0                 | 50.3                   | 46.0             |                |
| 10-shot            | **57.3** | 51.8                 | 50.9                   | 50.1             |                |
| 10-shot            | **52.2** | 48.7                 | 49.3                   | 45.7             |                |
| 5-shot             | **60.2** | 55.9                 | 50.2                   | 51.4             |                |
| 8-shot CoT         | **87.0** | 84.6                 | 76.9                   | 83.8             |                |
| 5-shot             | **80.8** | 78.0                 | 75.3                   | 65.6             |                |
| 3-shot             | **77.1** | 69.0                 | 67.5                   | 68.0             |                |
| 3-shot CoT         | 89.2     | 87.5                 | **90.0**               | 89.8             |                |
| 5-shot             | **87.8** | 84.9                 | 82.4                   | 85.2             |                |
| 5-shot             | **96.0** | 95.0                 | 95.2                   | 93.6             |                |
| 5-shot             | **81.0** | 78.5                 | 76.1                   | 72.2             |                |
| 5-shot             | 79.5     | **79.8**             | 77.7                   | 74.5             |                |
| 5-shot             | **43.3** | 42.8                 | 39.5                   | 36.3             |                |
| 1-shot             | **82.3** | 69.5                 | 78.0                   | 74.4             |                |
| 0-shot             | **81.5** | 67.2                 | 78.3                   | 74.3             |                |
| 1-shot             | **40.1** | 31.9                 | 33.5                   | 32.4             |                |
| 1-shot             | 54.6     | 43.3                 | **56.7**               | 54.1             |                |
| 0-shot             | **50.3** | 49.1                 | 49.1                   | 48.9             |                |
| 2-shot             | **71.5** | 68.9                 | 67.2                   | 69.0             |                |
| 0-shot             | 56.2     | **60.0**             | 55.7                   | 56.6             |                |
| many-shot          | **85.9** | 83.8                 | 83.0                   | 80.2             |                |
| 0-shot             | **91.7** | 81.0                 | 91.3                   | 56.5             |                |
| 0-shot             | **48.7** | 44.5                 | 39.8                   | 44.6             |                |
| 0-shot             | 46.5     | **48.0**             | 33.0                   | 46.7             |                |

The best result in each row is shown in bold. Swipe horizontally on mobile to view the full table.

## 5\. Quick Start

ERNIE-5.5-Flash-Base is available on [Hugging Face](https://huggingface.co/baidu/ERNIE-5.5-Flash-Base) in FP8 and BF16 safetensors (the BF16 version is [ERNIE-5.5-Flash-Base-BF16](https://huggingface.co/baidu/ERNIE-5.5-Flash-Base-BF16)), under the [ERNIE 5.5 Flash Base Model License Agreement v1.0](https://huggingface.co/baidu/ERNIE-5.5-Flash-Base/blob/main/LICENSE). The model expects a BOS token at the start of every input, so keep `add_special_tokens=True` when tokenizing.

Python · Transformers

```
from transformers import AutoModelForCausalLM, AutoTokenizer
import torch

model_name = "baidu/ERNIE-5.5-Flash-Base-BF16"
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForCausalLM.from_pretrained(model_name, torch_dtype=torch.bfloat16, device_map="auto")
model_inputs = tokenizer(["The hybrid attention architecture of ERNIE 5.5 Flash"], add_special_tokens=True, return_tensors="pt").to(model.device)
print(tokenizer.decode(model.generate(**model_inputs, max_new_tokens=256)[0], skip_special_tokens=True))
```

SGLang and vLLM both serve the model through the `completions` API; see the model card for deployment instructions. For fine-tuning, please refer to the [ERNIE 5.5 example](https://github.com/NVIDIA-NeMo/Megatron-Bridge/blob/main/examples/models/ernie55/).

## 6\. Conclusion and Outlook

ERNIE-5.5-Flash-Base starts from one premise: a base model meant for further training should be efficient by design. We balanced capability and efficiency in every component, from attention and residual connections to the MoE layers and the output head. The architecture is backed by broad, high-quality pre-training data and by a training system designed together with the model. With 14B parameters activated per token and a compact KV cache, the model performs strongly across a broad range of benchmarks, including long-context ones.

This base model is only a starting point. The post-trained ERNIE 5.5 Flash will follow, and we look forward to seeing what the community builds on top of it, whether for domain adaptation, reasoning or agents.

## Citation

If you find ERNIE 5.5 useful in your research or projects, please consider citing this blog post:

BibTeX

```
@misc{ernie5.5flash,
  title = {ERNIE 5.5 Flash Base},
  author = {{Baidu ERNIE Team}},
  year = {2026},
  howpublished = {ERNIE Blog},
  url = {https://ernie.baidu.com/release/ernie-5-5-flash-base/}
}
```

ERNIE Team, Baidu [Back to top ↑](https://ernie.baidu.com/release/ernie-5-5-flash-base/#top)
