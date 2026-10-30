---
title: 'Sleep framework: A new way for LLMs to retain knowledge in continual learning'
link: https://research.google/blog/the-sleep-framework-a-new-way-for-llms-to-retain-knowledge-in-continual-learning/
source: hnrss-org
published: 2026-10-28T12:24:25Z
updated: 2026-10-28T12:24:25Z
first_seen: 2026-10-30T22:55:36.634700404Z
authors:
- acossta
summary: 'https://arxiv.org/pdf/2606.03979 Comments URL: https://news.ycombinator.com/item?id=50272479 Points: 138 # Comments: 12'
content: extracted
html: 2026-10-28-sleep-framework-a-new-way-for-llms-to-retain-knowledge-in.html
remote_preview:
  url: https://storage.googleapis.com/gweb-research2023-media/images/HO_previewImage1.width-800.format-jpeg.jpg
---

Modern LLMs achieve impressive performance via pre-training and [in-context learning](https://ai.stanford.edu/blog/understanding-incontext/) (ICL). However, once deployed, they remain largely static, operating with a fixed knowledge cutoff date that leaves them unable to acquire long-term capabilities or adapt to new information.

While updating models with new data seems like the obvious solution, continual parameter updates often trigger [catastrophic forgetting](https://en.wikipedia.org/wiki/Catastrophic_interference), a fundamental issue where learning new tasks severely degrades performance on previously learned ones. Alternatively, relying strictly on ICL limits knowledge retention to the immediate context window: once the context session ends, the acquired short-term information is forgotten.

Inspired by how the human brain processes and stabilizes memory during rest, we introduce a [sleep framework](https://arxiv.org/pdf/2606.03979) in continual learning: a learning paradigm that equips large language models (LLMs) with a continuous *periodic* lifecycle, enabling them to continually learn, consolidate fragile short-term context into stable long-term parameters. With this framework, we demonstrate that alternating between active interaction and offline memory consolidation allows LLMs to continuously acquire new facts, retain long-context knowledge up to 10M tokens, and improve complex reasoning.

## Rethinking machine learning: Active (wake) vs. sleep phases

Traditional machine learning relies on a strict separation between training and inference. In a continual learning framework, this distinction becomes artificial; an intelligent agent must constantly learn, adapt, and process information.

Under the sleep paradigm, an LLM operates across two primary states:

1. **Active (wake) phase**: The model actively receives, processes, and interacts with inputs, storing temporary information in fast, short-term memory components.
2. **Sleep phase**: The model pauses processing new inputs and redirects its internal computations toward memory consolidation and self-improvement.

While terms like "sleep," "dreaming," and "self-improvement" borrow from neuroscience, our framework operates entirely within a fixed model architecture and human-defined training rules. During the offline sleep phase, the model unlocks pre-allocated Mixture-of-Experts (MoE) memory blocks and updates its numerical weights using self-generated practice examples. This automated post-training cycle helps a deployed model absorb new facts and sharpen its reasoning accuracy while keeping its core codebase and design unchanged.

## Key components of the sleep process

The sleep paradigm addresses catastrophic forgetting and continuous adaptation through two stages: memory consolidation and dreaming.

### 1\. Memory consolidation

In the human brain, [non-rapid eye movement](https://en.wikipedia.org/wiki/Non-rapid_eye_movement_sleep) (NREM) slow-wave sleep consolidates fragile memories into long-term structures via abstraction and distillation. Our proposed sleep framework mirrors this natural phenomenon through two key technical mechanisms:

- **Parameter expansion & continuum memory hierarchy:** To prevent catastrophic forgetting, the model dynamically scales its capacity during sleep. Using a [Mixture of Experts](https://huggingface.co/blog/moe) (MoE) and [Hope](https://arxiv.org/abs/2512.24695) architecture , newly unlocked, low-rank experts are integrated into stable, lower-frequency memory blocks to store incoming knowledge without disrupting older parameters.
- **Knowledge seeding (upward distillation):** Fragile short-term memories stored in high-frequency layers (e.g., in-context memory) are transferred into the newly expanded long-term parameters. This is achieved through [Generalized Knowledge Distillation](https://huggingface.co/docs/trl/en/gkd_trainer) (GKD) combined with [reinforcement learning](https://www.geeksforgeeks.org/machine-learning/what-is-reinforcement-learning/) (RL)-based [imitation learning](https://en.wikipedia.org/wiki/Imitation_learning):
  - **On-policy distillation:** The expanded student model learns token-specific feedback from teacher distributions generated on self-sampled sequences.
  - **Learning to imitate (LTI):** The student model is rewarded based on semantic and absolute similarity ([Levenshtein distance](https://en.wikipedia.org/wiki/Levenshtein_distance)) when completing prefixes of teacher-generated text.
- **Synaptic pruning:** Once the memory consolidation into lower-frequency modules is complete, the high-frequency temporary parameters are reset, freeing up capacity for future active sessions.

### 2\. Dreaming for self-improvement

During [REM sleep](https://www.thensf.org/what-is-rem-sleep/), the human brain actively simulates scenarios, consolidates emotional/semantic connections, and explores novel neural pathways. Mirroring this, during the “dreaming stage”, the sleep framework implements an autonomous self-improvement loop:

- **Random expert selection:** During synthetic data generation, the MoE router randomly activates non-standard experts. This forces the model to mix disparate domains of knowledge and uncover latent patterns.
- **Curated rehearsal:** The model generates synthetic training samples ("dreams") and filters them using gradient-based data selection to prioritize samples with the highest potential to boost overall model capability.
- **RL refinement:** The model performs supervised fine-tuning on top-rated dream samples and evaluates whether the generation improves performance on downstream tasks.

## Theoretical foundations

Beyond the empirical results, our sleep paradigm is grounded in a theory that clarifies why its two stages (memory consolidation and dreaming) can support continual learning without simply trading new knowledge for old capabilities.

For memory consolidation, the analysis shows that knowledge seeding can be understood as a structured, low-rank compression problem. The model is not asked to copy every detail of its short-term traces into its parameters. Instead, it identifies a compact set of directions that best preserve both the model’s own on-policy behavior and the semantic qualities rewarded by the learning objective.

This perspective explains the complementary roles of the two signals: on-policy distillation aligns consolidation with the trajectories the model will actually produce at inference time, reducing the mismatch that can arise when training only on teacher-generated distributions; meanwhile, the semantic reward changes what is worth retaining, prioritizing directions that preserve meaning and task-relevant behavior rather than merely matching token probabilities. Parameter expansion then provides the capacity needed to write these selected memories without overwriting previously consolidated knowledge.

For dreaming, the theory characterizes random expert activation and reward-based sample selection as mechanisms for improving the geometry of learning. Injecting different MoEs broadens the set of gradient directions explored during synthetic generation, exposing the model to a richer collection of updates than those induced by its ordinary wake-time data alone. Gradient-based filtering then emphasizes the synthetic experiences that carry the most useful learning signal, strengthening informative directions while preserving the structure of the underlying gradient space.

Together, these steps yield a more balanced optimization landscape: learning is less dominated by a small number of directions, gradients are better conditioned, and the model can make more stable progress when refining its capabilities. In this view, dreaming is not simply self-generated data augmentation; it is a deliberate process for diversifying, selecting, and consolidating the updates through which the model improves itself.

For [continual translation](https://arxiv.org/abs/2512.24695), in unseen languages, such as Manchu and Kalamang, the sleep framework maintained a near-zero performance drop in multi-stage evaluations, whereas standard baselines like ICL, [Cartridges](https://arxiv.org/abs/2506.06266), and [supervised fine-tuning](https://cameronrwolfe.substack.com/p/understanding-and-using-supervised) (SFT) suffered severe catastrophic forgetting during sequential learning.

In long-context understanding on the [BABILong](https://arxiv.org/abs/2406.10149) benchmark, our sleep method demonstrated near-perfect scaling up to 10M tokens, outperforming strong baselines like [GPT-4](http://openai.com/index/gpt-4-research/), and [Titans](https://research.google/blog/titans-miras-helping-ai-have-long-term-memory/) by using sleep-time consolidation to compress long-context information directly into parametric memory.

For factual knowledge incorporation evaluated on [SQuAD](https://deepeval.com/docs/benchmarks-squad) and [continued pre-training](https://docs.aws.amazon.com/nova/latest/userguide/nova-cpt.html) (CPT), our sleep method achieved 48.9% on single-task and 46.2% on CPT evaluations, outperforming baseline methods including the base model, SFT, and [Self-Adapting Language Models](https://arxiv.org/abs/2506.10943) (SEAL) by successfully integrating new factual information without degrading existing model representations.

Finally, on mathematical reasoning benchmarks including [AIME 24/25](https://artificialanalysis.ai/evaluations/aime-2025) and [HMMT 25](https://intuitionlabs.ai/articles/hmmt25-ai-benchmark-explained), sleep-enhanced base-model achieved accuracy scores of 79.2%, 69.0%, and 46.1%, respectively. This outperformed baselines like SFT, group relative policy optimization (GRPO), and on-policy self-distillation (OPSD), showing that pairing memory consolidation with imitation learning drives superior reasoning performance.

## Conclusion

Our sleep paradigm provides a path toward autonomous, continually learning AI systems. By moving beyond static pre-training and fragile context windows, alternating between active learning and offline rest empowers LLMs to retain short-term memories, abstract new skills into long-term parameter structures, and continuously self-improve without catastrophic forgetting.

## Acknowledgements

[*This research*](https://arxiv.org/abs/2606.03979) *was conducted by Ali Behrouz (Google and Cornell University), Farnoosh Hashemi (Cornell University), Adel Javanmard (Google and University of Southern California), and Vahab Mirrokni. We thank Yuan Deng and Corinna Cortes for reviewing the work and their valuable suggestions. We also thank Mark Simborg, Kimberly Schwede, and Tim Herrmann for their help in crafting this blog post.*
