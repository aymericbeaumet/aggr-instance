---
title: 'Granite 4.1 LLMs: How They’re Built'
link: https://huggingface.co/blog/ibm-granite/granite-4-1
source: huggingface-co-blog
published: 2026-04-29T15:01:48Z
updated: 2026-04-29T15:01:48Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2026-04-29-granite-4-1-llms-how-they-re-built.html
preview:
  file: 2026-04-29-granite-4-1-llms-how-they-re-built.preview-70e2b7a6709a.webp
  width: 256
  height: 138
  color: '#f6f6f6'
images:
- source: https://cdn-thumbnails.huggingface.co/social-thumbnails/blog/ibm-granite/granite-4-1.png
  original:
    file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-27ef42875c66.png
    width: 1200
    height: 648
  variants:
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-fcd751c867c1.webp
    width: 320
    height: 173
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-6accfe4a29e3.webp
    width: 640
    height: 346
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-87564f1b4eaa.webp
    width: 960
    height: 518
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-8eb5b096eb94.webp
    width: 1200
    height: 648
  color: '#fefefe'
- source: https://cdn-uploads.huggingface.co/production/uploads/6658c911e238275ea9efc339/G9mYhWq9PunNVyKzszCUL.png
  original:
    file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-d4cf1b2cd7ae.png
    width: 4647
    height: 545
  variants:
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-a7c65b042184.webp
    width: 320
    height: 38
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-29d38d4521e7.webp
    width: 640
    height: 75
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-41178828cbb4.webp
    width: 960
    height: 113
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-acdc1c2fd2dd.webp
    width: 1280
    height: 150
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-76c9d8d774a2.webp
    width: 1600
    height: 188
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-a00970b7be12.webp
    width: 4647
    height: 545
  color: '#d6d6d6'
- source: https://cdn-uploads.huggingface.co/production/uploads/6658c911e238275ea9efc339/Rrc_DSHSiJs7iNc8Nv8yI.png
  original:
    file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-1b2ec9228c4d.png
    width: 2322
    height: 696
  variants:
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-923769764315.webp
    width: 320
    height: 96
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-898deb90a64c.webp
    width: 640
    height: 192
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-3bd716bfe84f.webp
    width: 960
    height: 288
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-c3288f9081bc.webp
    width: 1280
    height: 384
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-e4c652f8b14f.webp
    width: 1600
    height: 480
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-430946c35b90.webp
    width: 2322
    height: 696
  color: '#fafafa'
- source: https://cdn-uploads.huggingface.co/production/uploads/6658c911e238275ea9efc339/_-jA1WScOVuIFl20cLEPU.png
  original:
    file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-bee4952f378e.png
    width: 4473
    height: 2145
  variants:
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-bfb823e6a071.webp
    width: 320
    height: 153
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-67db28415371.webp
    width: 640
    height: 307
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-6c06cf1114c9.webp
    width: 960
    height: 460
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-7dd8bb76f77a.webp
    width: 1280
    height: 614
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-b45728a5a22f.webp
    width: 1600
    height: 767
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-a1013b8eeb15.webp
    width: 4473
    height: 2145
  color: '#191919'
- source: https://cdn-uploads.huggingface.co/production/uploads/6658c911e238275ea9efc339/MAieYrC4jTq4xiFUYcqHJ.png
  original:
    file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-57bb99bff9dc.png
    width: 3623
    height: 1223
  variants:
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-cee2566e72c7.webp
    width: 320
    height: 108
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-d6f21eb160dc.webp
    width: 640
    height: 216
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-34f224396b9a.webp
    width: 960
    height: 324
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-e409255d1cfd.webp
    width: 1280
    height: 432
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-74de200187c0.webp
    width: 1600
    height: 540
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-88040ba19a7d.webp
    width: 3623
    height: 1223
  color: '#f2f2f2'
- source: https://cdn-uploads.huggingface.co/production/uploads/6658c911e238275ea9efc339/0eQZvn83EJBg-dZT-Uoo0.png
  original:
    file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-36af354c39a1.png
    width: 3858
    height: 2294
  variants:
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-a3a123a5b021.webp
    width: 320
    height: 190
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-b5f475c62608.webp
    width: 640
    height: 381
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-7bb32704ae57.webp
    width: 960
    height: 571
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-e12efc1f014e.webp
    width: 1280
    height: 761
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-4e254b0ad0cf.webp
    width: 1600
    height: 951
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-4815ad817fc4.webp
    width: 3858
    height: 2294
  color: '#fcfcfc'
- source: https://cdn-uploads.huggingface.co/production/uploads/6658c911e238275ea9efc339/MiNs_R5sA992r43HCyA92.png
  original:
    file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-e1370767d485.png
    width: 3858
    height: 2294
  variants:
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-56c904e330a7.webp
    width: 320
    height: 190
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-6e2cf22d08ea.webp
    width: 640
    height: 381
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-708bbccbfd56.webp
    width: 960
    height: 571
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-f64b33544052.webp
    width: 1280
    height: 761
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-6e84a24ba25a.webp
    width: 1600
    height: 951
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-d1c2c2c6029b.webp
    width: 3858
    height: 2294
  color: '#fcfcfc'
- source: https://cdn-uploads.huggingface.co/production/uploads/6658c911e238275ea9efc339/C7F0iXCkhlEREl_1Z3Fnn.png
  original:
    file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-b8060bd6097a.png
    width: 2818
    height: 1356
  variants:
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-98ba12a7c1c5.webp
    width: 320
    height: 154
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-aa6b62ce7afe.webp
    width: 640
    height: 308
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-9fcdb1fe52a7.webp
    width: 960
    height: 462
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-05f1d08df4f7.webp
    width: 1280
    height: 616
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-35c3f2d6d6ef.webp
    width: 1600
    height: 770
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-498f85d7c743.webp
    width: 2818
    height: 1356
  color: '#fbfbfc'
- source: https://cdn-uploads.huggingface.co/production/uploads/6658c911e238275ea9efc339/GWOOEjb2Nr07aJFaJZXmx.png
  original:
    file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-a1b705dc7ba7.png
    width: 3066
    height: 1430
  variants:
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-cb746ff6d031.webp
    width: 320
    height: 149
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-01e00d6e81d5.webp
    width: 640
    height: 298
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-b09be9e6901f.webp
    width: 960
    height: 448
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-df4753493b39.webp
    width: 1280
    height: 597
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-34853c88a6f7.webp
    width: 1600
    height: 746
  - file: 2026-04-29-granite-4-1-llms-how-they-re-built.image-935b5d25abb7.webp
    width: 3066
    height: 1430
  color: '#fbfcfc'
---

*An in-depth technical walkthrough of data engineering, pre-training, supervised fine-tuning, and reinforcement learning behind the Granite 4.1 LLMs.*

**Authors:** Granite Team, IBM

* * *

**TL;DR** — Granite 4.1 is a family of dense, decoder‑only LLMs (3B, 8B, and 30B) trained on ~15T tokens using a multi‑stage pre‑training pipeline, including long‑context extension of up to 512K tokens. The models are further refined with supervised fine‑tuning on ~4.1M high‑quality curated samples and reinforcement learning via on‑policy GRPO with DAPO loss ([Yu et al., 2025](https://arxiv.org/abs/2503.14476)). Notably, the 8B instruct model matches or surpasses the previous Granite 4.0‑H‑Small (32B‑A9B MoE) despite using a simpler dense architecture with fewer parameters. All Granite 4.1 models are released under the Apache 2.0 license.

**Links:**

- [Granite 4.1 HF Collection](https://huggingface.co/collections/ibm-granite/granite-41-language-models)
- [GitHub Repository](https://github.com/ibm-granite/granite-4.1-language-models)
- [Granite Docs](https://www.ibm.com/granite/docs/)

* * *

## [](https://huggingface.co/blog/ibm-granite/granite-4-1#overview) Overview

Building high‑quality small language models goes beyond simply scaling compute—it requires rigorous data curation throughout training. For Granite 4.1, we prioritized data quality over quantity, progressively refining the data mixture across five pre‑training stages. We further curated supervised fine‑tuning data using an LLM‑as‑Judge framework and applied a multi‑stage reinforcement learning pipeline to systematically strengthen performance in math, coding, instruction following, and general chat.

* * *

## [](https://huggingface.co/blog/ibm-granite/granite-4-1#model-architecture) Model Architecture

Granite 4.1 models use a decoder-only dense transformer architecture. The core design choices include **Grouped Query Attention (GQA)**, **Rotary Position Embeddings (RoPE)**, **SwiGLU activations**, **RMSNorm**, and **shared input/output embeddings**.

| Component                 | 3B Dense | 8B Dense | 30B Dense |
| ------------------------- | -------- | -------- | --------- |
| Embedding size            | 2560     | 4096     | 4096      |
| Number of layers          | 40       | 40       | 64        |
| Attention head size       | 64       | 128      | 128       |
| Number of attention heads | 40       | 32       | 32        |
| Number of KV heads        | 8        | 8        | 8         |
| MLP hidden size           | 8192     | 12800    | 32768     |
| MLP activation            | SwiGLU   | SwiGLU   | SwiGLU    |
| Position embedding        | RoPE     | RoPE     | RoPE      |

All three model sizes share the same training pipeline and data strategy, differing only in architecture dimensions.

* * *

## [](https://huggingface.co/blog/ibm-granite/granite-4-1#pre-training) Pre-Training

Granite 4.1 is trained from scratch on approximately 15 trillion tokens using a five‑phase training strategy. Phases 1–2 focus on foundational pre‑training, phases 3–4 perform mid‑training with progressively higher‑quality data annealing, and phase 5 introduces long‑context training, extending the context window to 512K tokens. Each phase employs a distinct data mixture and learning‑rate schedule, gradually shifting from broad web‑scale data to more curated, domain‑specific content.

[![Five-phase pre-training pipeline](https://cdn-uploads.huggingface.co/production/uploads/6658c911e238275ea9efc339/G9mYhWq9PunNVyKzszCUL.png)](https://cdn-uploads.huggingface.co/production/uploads/6658c911e238275ea9efc339/G9mYhWq9PunNVyKzszCUL.png)

***Figure 2:** The five-phase pre-training pipeline. Phases 1–2 are pre-training, Phases 3–4 are mid-training (high-quality data annealing), and Phase 5 is long context training (LCE).*

### [](https://huggingface.co/blog/ibm-granite/granite-4-1#phase-1-general-pre-training-10t-tokens) Phase 1: General Pre-Training (10T tokens)

The first phase establishes broad language understanding using a general mixture of training data with a power learning rate schedule and warmup.

**Data composition:**

- **CommonCrawl** ~59% — general web data
- **Code** ~20% — programming languages and repositories
- **Math** ~7% — mathematical reasoning data
- **Technical** ~10.5% — scientific papers, technical documentation and manuals
- **Multilingual** ~2% — non-English language data
- **Domain Specific** ~1.5% — domain-specific content

### [](https://huggingface.co/blog/ibm-granite/granite-4-1#phase-2-mathcode-pre-training-2t-tokens) Phase 2: Math/Code Pre-Training (2T tokens)

Phase 2 sharply increases the proportion of code and mathematical data, pivoting toward stronger reasoning capabilities while still maintaining general language coverage.

**Data composition:**

- **Math** ~35% — a 5x increase over Phase 1
- **Code** ~30% — a 1.5x increase
- **CommonCrawl-HQ** ~12% — high-quality common crawl subset
- **Synthetic** ~9% — synthetic high-quality data
- **Technical** ~10%
- **Multilingual** ~3%
- **Domain** ~1%

### [](https://huggingface.co/blog/ibm-granite/granite-4-1#phase-3-high-quality-data-annealing-2t-tokens) Phase 3: High-Quality Data Annealing (2T tokens)

Phase 3 transitions into **mid-training** with a more balanced, high-quality mixture and an exponential decay learning rate schedule. This is where we start blending in chain-of-thought and synthetic instruction data.

**Data composition:**

- **CommonCrawl-HQ** ~16.67%
- **Math** ~16.67%
- **Code** ~16.67%
- **Synthetic** ~8.5%
- **Technical** ~12.5%
- **Multilingual** ~4.5%
- **Long Chain-of-Thought** ~12.5% — reasoning trajectories
- **Language Instructions** ~7.5% — instruction tuning data
- **Code Instructions** ~4.5% — instruction tuning data

### [](https://huggingface.co/blog/ibm-granite/granite-4-1#phase-4-high-quality-data-annealing--refinement-05t-tokens) Phase 4: High-Quality Data Annealing — Refinement (0.5T tokens)

The fourth phase continues mid-training with a linear learning rate decay to zero, focusing the model on the highest-quality data available.

**Data composition:**

- **CommonCrawl-HQ** ~40%
- **Code** ~20%
- **Math** ~20%
- **Long Chain-of-Thought** ~6%
- **Code Instructions** ~5%
- **Language Instructions** ~9%

[![Data mix evolution across pre-training phases](https://cdn-uploads.huggingface.co/production/uploads/6658c911e238275ea9efc339/Rrc_DSHSiJs7iNc8Nv8yI.png)](https://cdn-uploads.huggingface.co/production/uploads/6658c911e238275ea9efc339/Rrc_DSHSiJs7iNc8Nv8yI.png)

***Figure 3:** How the data mix evolves across the pre-training phases. Notice the progressive shift from web-heavy (Phase 1) to quality-heavy with instruction and reasoning data (Phases 3–4).*

### [](https://huggingface.co/blog/ibm-granite/granite-4-1#phase-5-long-context-training-lce) Phase 5: Long Context Training (LCE)

The fifth and final phase also part of of mid-training extends the context window from **4K** to **512K** through a staged long-context extension process:

1. **32K extension** — using the same data mix as Phase 4
2. **128K extension** — same data mix as Phase 4
3. **512K extension** — 80% books + 20% code repository data (8b and 30b only)

The LCE phase uses an exponential learning rate schedule starting at `1e-4` and decaying to `0`. To ensure the model natively handles long sequences without degrading short-context performance, we do a model merge after each LCE stage. RULER benchmark of base models:

| Model name               | 32K  | 64K  | 128K |
| ------------------------ | ---- | ---- | ---- |
| **granite-4.1-3b-base**  | 75.0 | 66.6 | 58.0 |
| **granite-4.1-8b-base**  | 83.6 | 79.1 | 73.0 |
| **granite-4.1-30b-base** | 85.2 | 84.6 | 76.7 |

* * *

## [](https://huggingface.co/blog/ibm-granite/granite-4-1#sft-data-preparation--quality-control) SFT: Data Preparation & Quality Control

Supervised fine‑tuning (SFT) is what turns the base model into a reliable instruction‑following assistant, making data quality critically important—since even a small number of incorrect or hallucinated samples can instill undesirable behaviors. To address this, we apply a rigorous LLM‑as‑Judge framework alongside rule‑based filtering to curate high-quality samples. Together, the pipeline automatically assess each sample against structural, semantic, and behavioral criteria, fixing issues when possible and filtering out samples that fail to meet our quality standards.

[![SFT Data Quality Pipeline](https://cdn-uploads.huggingface.co/production/uploads/6658c911e238275ea9efc339/_-jA1WScOVuIFl20cLEPU.png)](https://cdn-uploads.huggingface.co/production/uploads/6658c911e238275ea9efc339/_-jA1WScOVuIFl20cLEPU.png)

***Figure 4:** The SFT data quality pipeline. Raw conversation data passes through an LLM-as-Judge with a multi-dimensional rubric, producing accept/borderline/reject verdicts. Hard-reject defects (hallucination, false premise, incorrect computation) trigger automatic rejection regardless of score.*

Our rigorous LLM‑as‑Judge framework evaluates only assistant responses, treating system prompts, user inputs, retrieved documents, and tool outputs strictly as contextual information. This ensures that the judge assesses what the model says, rather than what it was asked to do. In RAG settings, responses that are not grounded in the retrieved context are flagged as hallucinations, while tool‑use outputs are validated against the set of allowed tools and their parameter schemas.

We employ specialized judge prompts tailored to different SFT data types, including multi‑turn dialogue, RAG‑augmented responses, tool‑calling interactions, and multilingual conversations. Each response is scored across six weighted dimensions—instruction following, correctness, completeness, conciseness, naturalness, and calibration (with optional critical‑thinking checks). Samples are accepted, flagged as borderline, or rejected based on deterministic score thresholds, with hard‑reject rules overriding scores for severe defects such as hallucinations, false premises, or incorrect computations.

To complement semantic evaluation, we apply a deterministic rule‑based pipeline that enforces structural integrity through text normalization, truncation and length filtering, schema validation, and leakage detection. A final global deduplication step ensures dataset‑wide uniqueness. All filtering and correction actions are fully auditable.

### [](https://huggingface.co/blog/ibm-granite/granite-4-1#sft-training-details) SFT Training Details

After passing through the LLM-as-Judge, rule-based filtering, and global deduplication pipeline, we fine-tune base models on these approximately **4.1 million** high-quality samples. The following details apply to all three model variants:

**Training Configuration:**

| Parameter            | Value                                                 |
| -------------------- | ----------------------------------------------------- |
| Compute              | 16 nodes, 4x GB200 per node                           |
| Epochs               | 3                                                     |
| Learning rate        | 5e-6 (linear warmup 3%, linear decay over ~25K steps) |
| Sequence length      | 16,384 tokens                                         |
| Total samples        | \~4.1M                                                |
| Effective batch size | 256 samples/iter (~4.2M tokens/iter)                  |

* * *

## [](https://huggingface.co/blog/ibm-granite/granite-4-1#reinforcement-learning-multi-stage-rl-pipeline) Reinforcement Learning: Multi-Stage RL Pipeline

After SFT, we apply a multi-stage reinforcement learning pipeline to further improve the model's capabilities across specific domains. Rather than a single RL pass, we run **multiple targeted RL stages**, each optimizing for different capabilities.

### [](https://huggingface.co/blog/ibm-granite/granite-4-1#training-methodology) Training Methodology

We use **On-policy GRPO (Group Relative Policy Optimization)** ([Shao et al., 2024](https://arxiv.org/abs/2402.03300)) with **DAPO (Decoupled Clip and Dynamic sAmpling Policy Optimization) loss** ([Yu et al., 2025](https://arxiv.org/abs/2503.14476)) which provides more stable training signals compared to standard GRPO. However, due to computationally intensive nature of dynamic sampling, we switch it off during our training runs.

#### [](https://huggingface.co/blog/ibm-granite/granite-4-1#rl-training-configuration) RL training configuration

| Parameter          | Value                                                            |
| ------------------ | ---------------------------------------------------------------- |
| Algorithm          | On-policy GRPO with DAPO loss                                    |
| Training stack     | SkyRL ([NovaSky-AI, 2025](https://github.com/NovaSky-AI/SkyRL)) |
| Samples per prompt | 16                                                               |
| Train batch size   | 1024                                                             |
| Context length     | 8,192                                                            |

### [](https://huggingface.co/blog/ibm-granite/granite-4-1#rl-pipeline) RL Pipeline

[Figure 10](https://huggingface.co/blog/ibm-granite/granite-4-1#fig-rl-pipeline) depicts our Reinforcement Learning pipeline for training Granite 4.1 models. Through extensive experimentation with a variety of reinforcement learning recipes, we found that this sequence of steps minimizes catastrophic forgetting while simultaneously maximizing performance across multiple domains.

[![Granite 4.1 Reinforcement Learning Pipeline](https://cdn-uploads.huggingface.co/production/uploads/6658c911e238275ea9efc339/MAieYrC4jTq4xiFUYcqHJ.png)](https://cdn-uploads.huggingface.co/production/uploads/6658c911e238275ea9efc339/MAieYrC4jTq4xiFUYcqHJ.png)

***Figure 10:** The Granite 4.1 reinforcement learning pipeline consisting of four sequential stages: Multi-domain RL, RLHF, Identity and Knowledge-calibration RL, and Math RL.*

#### [](https://huggingface.co/blog/ibm-granite/granite-4-1#multi-domain-rl) Multi-domain RL

In this stage, the model is trained jointly on a unified mixture of data drawn from multiple domains. Every gradient update therefore reflects the full diversity of tasks, which prevents catastrophic forgetting, boosts overall benchmark performance, and minimizes regressions on any individual task.

The different domains covered in this stage include:

| Domain                         | Description                            |
| ------------------------------ | -------------------------------------- |
| **Math**                       | Mathematical reasoning and computation |
| **Science**                    | Scientific knowledge and reasoning     |
| **Logical Reasoning**          | Deductive and inductive logic          |
| **Instruction Following (IF)** | Adherence to complex instructions      |
| **Structured Output**          | Structured data output                 |
| **Text2SQL**                   | Database query generation              |
| **Temporal Reasoning**         | Time-based logic and ordering          |
| **General Chat**               | General conversational quality         |
| **In-context Learning**        | Learning from in-context examples      |

During this stage, we trained the models on 45,504 unique prompts (averaged across all Granite 4.1 models) and found that a learning rate of `5e‑7` with a KL‑loss coefficient ($\\beta$) of `0.05` performed best for multi‑domain reinforcement learning.

#### [](https://huggingface.co/blog/ibm-granite/granite-4-1#rlhf) RLHF

To further improve the model's helpfulness and chat ability, we train our model on generic-chat prompts using a multilingual scalar reward model. With this stage, we observed an average improvement of **\~18.9 points** (averaged across the three Granite 4.1 models) in Alpaca-Eval compared to the SFT checkpoints.

To mitigate policy drift from its previously learned knowledge, we use a conservative learning rate of `3e-7` and higher KL-loss coeff $\\beta$ of `0.09` in this stage. We use an average of 17,920 unique prompts in this RLHF stage.

#### [](https://huggingface.co/blog/ibm-granite/granite-4-1#identity--knowledge-calibration-rl) Identity & Knowledge-Calibration RL

In this stage, we train the model for a few steps (~40 training steps) on identity and knowledge calibration prompts. We observed that this small training stage significantly improves the model's self-identification capabilities.

Similar to the RLHF stage, we used a learning rate of `3e-7` and KL-loss coeff $\\beta$ of `0.09`, and we use 1728 unique prompts in this stage.

#### [](https://huggingface.co/blog/ibm-granite/granite-4-1#math-rl) Math RL

During our RL training, we found that the RLHF stage causes a drop in math benchmark scores (e.g., in GSM8K, DeepMind-Math). The Math RL stage enables the model to recover from this drop and surpasses the original SFT performance on math benchmarks: **\~3.8 points** on average for GSM8K, and **\~23.48 points** on average for DeepMind-Math. We use an average of 13,504 unique prompts in this stage and similar to the multi-domain RL stage, we used a learning rate of `5e-7` and KL-loss coeff $\\beta$ of `0.05`.

* * *

## [](https://huggingface.co/blog/ibm-granite/granite-4-1#results) Results

### [](https://huggingface.co/blog/ibm-granite/granite-4-1#base-model-benchmarks) Base Model Benchmarks

| Benchmark              | Metric             | 3B    | 8B    | 30B   |
| ---------------------- | ------------------ | ----- | ----- | ----- |
| **General Tasks**      |                    |       |       |       |
| MMLU                   | 5-shot             | 66.47 | 73.60 | 78.44 |
| MMLU-Pro               | 5-shot, CoT        | 37.16 | 44.58 | 49.51 |
| BBH                    | 3-shot, CoT        | 63.84 | 73.83 | 80.66 |
| AGI EVAL               | 3-shot             | 54.32 | 61.68 | 69.20 |
| DROP                   | 5-shot             | 66.04 | 72.36 | 78.57 |
| **Math Tasks**         |                    |       |       |       |
| GSM8K                  | 8-shot             | 72.93 | 73.54 | 83.78 |
| Minerva Math           | 4-shot             | 38.00 | 43.42 | 45.66 |
| **Code Tasks**         |                    |       |       |       |
| HumanEval              | pass@1 (StarCoder) | 76.19 | 79.24 | 81.52 |
| HumanEval              | pass@1             | 59.76 | 68.29 | 69.50 |
| HumanEval+             | pass@1             | 54.27 | 62.20 | 61.60 |
| Eval+ Avg              |                    | 65.94 | 62.05 | 63.90 |
| **Multilingual Tasks** |                    |       |       |       |
| MMMLU                  | 5-shot             | 56.59 | 64.73 | 73.36 |
| INCLUDE                | 5-shot             | 51.77 | 57.60 | 67.07 |
| MGSM                   | 8-shot             | 58.48 | 63.68 | 74.40 |

### [](https://huggingface.co/blog/ibm-granite/granite-4-1#instruct-model-benchmarks) Instruct Model Benchmarks

| Benchmark              | Metric      | 3B    | 8B    | 30B   |
| ---------------------- | ----------- | ----- | ----- | ----- |
| **General Tasks**      |             |       |       |       |
| MMLU                   | 5-shot      | 67.02 | 73.84 | 80.16 |
| MMLU-Pro               | 5-shot, CoT | 49.83 | 55.99 | 64.09 |
| BBH                    | 3-shot, CoT | 75.83 | 80.51 | 83.74 |
| AGI EVAL               | 0-shot, CoT | 65.16 | 72.43 | 77.80 |
| GPQA                   | 0-shot, CoT | 31.70 | 41.96 | 45.76 |
| SimpleQA               |             | 3.68  | 4.82  | 6.81  |
| **Alignment Tasks**    |             |       |       |       |
| AlpacaEval 2.0         |             | 38.57 | 50.08 | 56.16 |
| IFEval Avg             |             | 82.30 | 87.06 | 89.65 |
| ArenaHard              |             | 37.80 | 68.98 | 71.02 |
| MTBench Avg            |             | 7.53  | 8.50  | 8.53  |
| **Math Tasks**         |             |       |       |       |
| GSM8K                  | 8-shot      | 86.88 | 92.49 | 94.16 |
| GSM Symbolic           | 8-shot      | 81.32 | 83.70 | 75.70 |
| Minerva Math           | 0-shot, CoT | 67.94 | 80.10 | 81.32 |
| DeepMind Math          | 0-shot, CoT | 64.64 | 80.07 | 81.93 |
| **Code Tasks**         |             |       |       |       |
| HumanEval              | pass@1      | 79.27 | 87.20 | 89.63 |
| HumanEval+             | pass@1      | 74.39 | 80.49 | 85.98 |
| MBPP                   | pass@1      | 61.64 | 82.54 | 83.33 |
| MBPP+                  | pass@1      | 52.91 | 70.64 | 71.69 |
| CRUXEval-O             | pass@1      | 40.75 | 47.63 | 55.75 |
| BigCodeBench           | pass@1      | 32.19 | 35.00 | 38.77 |
| MULTIPLE               | pass@1      | 52.54 | 60.26 | 62.31 |
| Eval+ Avg              | pass@1      | 67.05 | 80.21 | 82.66 |
| **Tool Calling**       |             |       |       |       |
| BFCL v3                |             | 60.80 | 68.27 | 73.68 |
| **Multilingual Tasks** |             |       |       |       |
| MMMLU                  | 5-shot      | 57.61 | 64.84 | 73.71 |
| INCLUDE                | 5-shot      | 52.05 | 58.89 | 67.26 |
| MGSM                   | 8-shot      | 70.00 | 82.32 | 71.12 |
| **Safety**             |             |       |       |       |
| SALAD-Bench            |             | 93.95 | 95.80 | 96.41 |
| AttaQ                  |             | 81.88 | 81.19 | 85.76 |
| Tulu3 Safety Eval Avg  |             | 66.84 | 75.57 | 78.19 |

**Supported languages:** English, German, Spanish, French, Japanese, Portuguese, Arabic, Czech, Italian, Korean, Dutch, and Chinese.

* * *

### [](https://huggingface.co/blog/ibm-granite/granite-4-1#granite41-comparison-with-leading-opensource-models) Granite 4.1 Comparison with Leading Open‑Source Models

Granite 4.1 delivers competitive instruction‑following and tool‑calling capabilities without relying on long chains of thought. By avoiding extended reasoning traces, it provides predictable latency, stable token usage, and lower operational cost. This makes Granite 4.1 a production‑ready, open‑source choice for enterprise workloads where efficiency, reliability, and cost control are critical.

[![BFCL V3](https://cdn-uploads.huggingface.co/production/uploads/6658c911e238275ea9efc339/0eQZvn83EJBg-dZT-Uoo0.png)](https://cdn-uploads.huggingface.co/production/uploads/6658c911e238275ea9efc339/0eQZvn83EJBg-dZT-Uoo0.png)

[![IFEval](https://cdn-uploads.huggingface.co/production/uploads/6658c911e238275ea9efc339/MiNs_R5sA992r43HCyA92.png)](https://cdn-uploads.huggingface.co/production/uploads/6658c911e238275ea9efc339/MiNs_R5sA992r43HCyA92.png)

### [](https://huggingface.co/blog/ibm-granite/granite-4-1#granite-41-8b-vs-granite-40-h-small-32b-a9b) Granite 4.1-8B vs. Granite 4.0-H-Small (32B-A9B)

A striking result: the Granite 4.1-8B dense model **consistently matches or outperforms** the previous-generation Granite 4.0-H-Small, a 32B-parameter Mixture-of-Experts model with 9B active parameters.

[![Granite 4.1-8B vs Granite 4.0-H-Small comparison](https://cdn-uploads.huggingface.co/production/uploads/6658c911e238275ea9efc339/C7F0iXCkhlEREl_1Z3Fnn.png)](https://cdn-uploads.huggingface.co/production/uploads/6658c911e238275ea9efc339/C7F0iXCkhlEREl_1Z3Fnn.png)

***Figure 13:** Granite 4.1-8B (dark blue) vs. Granite 4.0-H-Small 32B-A9B (light blue) across benchmarks. The 8B dense model matches or exceeds the larger MoE model on IFEval, AlpacaEval, MMLU-Pro, BBH, GSM8K, DeepMind-Math, Evalplus, ArenaHard, BFCL V3 and MBPP(+).*

### [](https://huggingface.co/blog/ibm-granite/granite-4-1#granite-41-model-family-comparison) Granite 4.1 Model Family Comparison

[![Granite 4.1 model family comparison](https://cdn-uploads.huggingface.co/production/uploads/6658c911e238275ea9efc339/GWOOEjb2Nr07aJFaJZXmx.png)](https://cdn-uploads.huggingface.co/production/uploads/6658c911e238275ea9efc339/GWOOEjb2Nr07aJFaJZXmx.png)

***Figure 14:** Comparison across the Granite 4.1 family — 30B, 8B, and 3B models. Scores scale predictably with model size, with the 30B model leading across all benchmarks.*

## [](https://huggingface.co/blog/ibm-granite/granite-4-1#fp8-quantization) FP8 Quantization

We also released fp8 quantized variants of the Granite 4.1 models, optimized for inference with vLLM. The precision is reduced from 16‑bit to 8‑bit, resulting in approximately a 50% reduction in both disk footprint and GPU memory usage. Quantization is applied only to the weights and activations of linear operators within the transformer blocks using LLM Compressor, while all other layers are preserved at their original precision.

## [](https://huggingface.co/blog/ibm-granite/granite-4-1#infrastructure) Infrastructure

We trained the Granite 4.1 Language Models on an **NVIDIA GB200 NVL72 cluster** hosted on CoreWeave:

- **Intra-rack communication:** 72-GPU NVLink domain
- **Inter-rack communication:** Non-blocking, full Fat-Tree NDR 400 Gb/s InfiniBand network
- **Scale:** Thousands of GPUs across the cluster

This infrastructure provides the scalable, high-bandwidth interconnect needed for efficient distributed training at the token volumes required (15T+ tokens across pre-training alone).

* * *

## [](https://huggingface.co/blog/ibm-granite/granite-4-1#getting-started) Getting Started

Granite 4.1 models are available under the **Apache 2.0 license**. Here's how to get started with the 30B instruct model wiht tool calling example:

```
pip install torch torchvision torchaudio
pip install accelerate
pip install transformers
```

```
import torch
from transformers import AutoModelForCausalLM, AutoTokenizer

device = "cuda"
model_path = "ibm-granite/granite-4.1-30b"
tokenizer = AutoTokenizer.from_pretrained(model_path)
# drop device_map if running on CPU
model = AutoModelForCausalLM.from_pretrained(model_path, device_map=device)
model.eval()

tools = [
    {
        "type": "function",
        "function": {
            "name": "get_current_weather",
            "description": "Get the current weather for a specified city.",
            "parameters": {
                "type": "object",
                "properties": {
                    "city": {
                        "type": "string",
                        "description": "Name of the city"
                    }
                },
                "required": ["city"]
            }
        }
    }
]

# change input text as desired
chat = [
    { "role": "user", "content": "What's the weather like in London right now?" },
]
chat = tokenizer.apply_chat_template(chat, \
                                     tokenize=False, \
                                     tools=tools, \
                                     add_generation_prompt=True)
# tokenize the text
input_tokens = tokenizer(chat, return_tensors="pt").to(device)
# generate output tokens
output = model.generate(**input_tokens, 
                        max_new_tokens=100)
# decode output tokens into text
output = tokenizer.batch_decode(output)
# print output
print(output[0])
```

Expected Output:

```
<|start_of_role|>system<|end_of_role|>You are a helpful assistant with access to the following tools. You may call one or more tools to assist with the user query.

You are provided with function signatures within <tools></tools> XML tags:
<tools>
{"type": "function", "function": {"name": "get_current_weather", "description": "Get the current weather for a specified city.", "parameters": {"type": "object", "properties": {"city": {"type": "string", "description": "Name of the city"}}, "required": ["city"]}}}
</tools>

For each tool call, return a json object with function name and arguments within <tool_call></tool_call> XML tags:
<tool_call>
{"name": <function-name>, "arguments": <args-json-object>}
</tool_call>. If a tool does not exist in the provided list of tools, notify the user that you do not have the ability to fulfill the request.<|end_of_text|>
<|start_of_role|>user<|end_of_role|>What's the weather like in London right now?<|end_of_text|>
<|start_of_role|>assistant<|end_of_role|><tool_call>
{"name": "get_current_weather", "arguments": {"city": "London"}}
</tool_call><|end_of_text|>
```

**Resources:**

- [Granite 4.1 HF Collection](https://huggingface.co/collections/ibm-granite/granite-41-language-models)
- [PRISM: Demystifying Retention and Interaction in Mid-Training](https://huggingface.co/papers/2603.17074)
- [GitHub: ibm-granite/granite-4.1-language-models](https://github.com/ibm-granite/granite-4.1-language-models)
- [Granite Documentation](https://www.ibm.com/granite/docs/)
- [Granite Community Resources](https://github.com/ibm-granite-community/)

* * *

*Granite 4.1 marks a significant step forward for high‑quality, open‑source language models. By prioritizing data quality and rigor at every stage—from pre‑training curation to supervised fine‑tuning and multi‑stage reinforcement learning—we deliver a substantially improved post‑training pipeline. The result is stronger instruction following, tool use, and conversational performance, showing that carefully trained dense 8B models can rival much larger MoE architectures. We’re excited to see how the community adopts and builds on these models.*
