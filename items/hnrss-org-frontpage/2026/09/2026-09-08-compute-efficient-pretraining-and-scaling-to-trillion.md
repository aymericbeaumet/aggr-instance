---
title: Compute-efficient pretraining and scaling to trillion-parameter models
link: https://magic.dev/blog/pretraining#
source: hnrss-org-frontpage
published: 2026-09-08T16:59:53Z
updated: 2026-09-08T16:59:53Z
first_seen: 2026-09-10T22:27:56.458410404Z
authors:
- ronfriedhaber
summary: 'Article URL: https://magic.dev/blog/pretraining# Comments URL: https://news.ycombinator.com/item?id=49613072 Points: 105 # Comments: 58'
content: extracted
html: 2026-09-08-compute-efficient-pretraining-and-scaling-to-trillion.html
preview:
  file: 2026-09-08-compute-efficient-pretraining-and-scaling-to-trillion.preview-690e5cd3bd8f.webp
  width: 256
  height: 146
  alt: '>10x More Efficient Pretraining'
  color: '#d6d8d8'
images:
- source: https://magic.dev/blog/pretraining/hero.jpg
  original:
    file: 2026-09-08-compute-efficient-pretraining-and-scaling-to-trillion.image-8283d404c8a8.jpg
    width: 1400
    height: 800
  variants:
  - file: 2026-09-08-compute-efficient-pretraining-and-scaling-to-trillion.image-73c8ef0b7c18.webp
    width: 48
    height: 27
  color: '#e9e7e4'
---

Research update on compute-efficient pretraining and scaling to trillion-parameter models.

Frontier pretraining is said to be a big-lab-only game. We don’t have 100k chips yet, so there’s only one way: algorithmic efficiency. After compounding for … a while …, our pretraining recipe is now >10x more compute-efficient than that of leading open-weight base models.

We match DeepSeek V4 Pro Base using ~50x fewer FLOPs – that’s around half of GPT3’s pretraining compute, or ~$0.5M on GB200. We continued scaling 10x (~$4M) and meaningfully outperformed all publicly available open base models on perplexity evals. By the scaling laws in Figure 1, training a model this capable would cost >$100M under DeepSeek V4 Pro’s recipe (and this is ignoring how much data exists). Of course, we won’t stop scaling there.

We believe pretraining, agentic RL, and long-context are sufficient to build superhuman coding agents and automate AI R&D. We started with [long-context](https://magic.dev/blog/100m-token-context-windows). Today’s blog post is about pretraining.

bits per byte (lower is better)

Private Code Repos

Heldout Research Papers

Reasoning on heldout math problems

6·N·D training FLOPs

Figure 1: Pretraining scaling laws against training compute, comparing to leading available open-weight base models.

[1](https://magic.dev/blog/pretraining#user-content-fn-flops)

We measured bits-per-byte loss (a metric that normalizes out differences in tokenizers) on heldout data and fit a [scaling law](https://arxiv.org/abs/2203.15556) to project how much compute is needed to reach a given level of capability. Better training compute efficiency means stronger models at all budgets.

We evaluated the latest available open-weight base models [2](https://magic.dev/blog/pretraining#user-content-fn-base) from DeepSeek, Moonshot (Kimi), and NVIDIA. Base models for Claude, Gemini, GPT-n, and many others aren’t openly available, but [Kimi K3](https://arxiv.org/abs/2607.24653) and [Meta’s Muse Spark](https://ai.meta.com/blog/introducing-muse-spark-msl/) indicate a 2.5x and 3.3x gain over Kimi K2, respectively. We evaluated logprobs for open models in both vLLM and SGLang on both GB200 and GB300 and found [issues](https://github.com/vllm-project/vllm/issues/53411) with some [backends](https://github.com/vllm-project/vllm/issues/54723) in the process. For further confirmation, we partnered with [Fireworks](https://fireworks.ai/) to verify baseline logprobs in their in-house inference engine. Since models can learn their training parser’s characteristics, we built our eval sets using a different parser/OCR than the one our pretraining pipeline uses.

## Evaluating generalization

To measure generalization, we evaluated loss on heldout data (Figure 1). Our code evals consist of our own codebase and private codebases we acquired from other startups. For reasoning evals, we generated CoT and step-by-step walkthroughs to heldout, private math problems using Kimi K3 and filtered for correct answers. For text and research, we used recent, low-citation research papers. We removed vendored OSS code and any document with a matching 96-character window of normalized text or Jaccard similarity above a sensitive threshold compared to our training data.[3](https://magic.dev/blog/pretraining#user-content-fn-contamination)

## Evaluating knowledge

In addition to generalization, we are interested in testing our model’s knowledge in key domains to identify gaps in our dataset. For example, we can decompose our heldout research text eval set by subject.

bits per byte (lower is better)

Heldout Computer Science Papers

Heldout Engineering Papers

Heldout Math Papers

Heldout Physics Papers

6·N·D training FLOPs

Figure 2: Effective-compute per research area.

By collecting granular buckets of content (e.g. documentation of a particular software tool or key papers in alignment research) we can get even more precise signals. Unlike for our generalization eval, we don’t want to fully remove much of this information (e.g. key papers in a field) from the pretraining corpus, but we still need to avoid rewarding sequence memorization [4](https://magic.dev/blog/pretraining#user-content-fn-memorization). To do this, we reworded/summarized these documents using a third-party frontier LLM. To avoid overfitting to granular evals, we created and evaluated them once per model generation; the ones below were made last week.

Magic’s goal is to build the best model for coding and autonomous AI R&D. To intentionally balance data mixing trade-offs, we also evaluate domains we deprioritize (e.g. facts about notable people, local news, or sports/events).

Our recipe vs.

effective-compute multiplier vs. best open model per eval

Figure 3: Effective-compute multiplier across domains.

We fit scaling laws on eval sets across 167 domains and show compute efficiency gains per dataset.

## No shortcuts

In late 2024, we trained a [small dense model](https://magic.dev/blog/100m-token-context-windows) with an architecture designed for very long context windows. Our initial pretraining scale-ups kept blowing up in a wide variety of ways. We learned quickly that we had to build a stable foundation first. Smooth convergence, low-precision training quality equivalent to FP32, fast and stable infra, correct hyperparameter scaling rules. And most importantly: hunt the bugs.

Once we had that in place, we needed to find enough compute efficiency improvements to close the gap to the frontier with less compute. We had a few big bets to start with, but our progress ended up being the multiplicative result of tens of changes across model architecture, optimizer, training objective, and data curation.

[NanoGPT speedruns](https://github.com/kellerjordan/modded-nanogpt) provide a fast feedback cycle to evaluate new ideas, but we found that many things that improve tiny models don’t improve big models. Similarly, we found that some features present in most LLMs can be [deleted](https://grugbrain.dev/) without harming large scale performance.

To evaluate each model, optimizer, or data change, we train 3 models spanning 2 orders of magnitude of compute. We consider a change worth keeping if its power law fit suggests it will help at scale. Every few weeks, we scaled up to 1/10th of our hero scale and every few months we ran a full-scale hero run (V3, V4, V5 in Figure 4).

Figure 4: Acceleration of our pretraining research progress.

To sanity check how pretraining loss translates to post-RL performance, we ran a short math RL run with a 16k CoT budget (Figure 5). All of our RL starts directly from the base model without SFT or distillation.

Figure 5: Pass@1 on heldout competition math problems during low-compute RL.

[5](https://magic.dev/blog/pretraining#user-content-fn-aime) FLOPs are 6·N·D, as in Figure 1.

## What’s next

Our pretraining and long-context work is now quite mature. We’ll now scale long-horizon RL, training agents to keep learning after deployment through long-context. We’re also putting significant work towards alignment training techniques that present robust theoretical properties. And last but not least, we look forward to releasing the thing!

Concrete problems we’re tackling include:

- Exploration and credit assignment in long-horizon RL (and systems work to scale up).
- Alignment training against narrowly [elicited latent knowledge](https://docs.google.com/document/d/1WwsnJQstPq91_Yh-Ch2XRL8H_EpsnjrC1dwZXR37PC8/edit?tab=t.0).[6](https://magic.dev/blog/pretraining#user-content-fn-agi)
- Further improvements to pretraining.

We are likely the smallest team in the world training trillion parameter models. The impact a single person with strong judgement can have has never been higher. If you want to help build aligned superintelligence, [consider joining](https://magic.dev/careers).

## Footnotes

1. We report 6·N·D in place of exact training flops, where N is the activated parameter count and D is the pretraining token count each report states. Sequence-dimension (e.g. attention, etc.) cost makes up a minority of the FLOPs for these (and our) models but depends on the exact sequence length distribution used. These aren’t reported for all public models, so we opted for the 6·N·D approximation to avoid guessing. The “6” appears because (add+mul) \* (fwd+bwd\*2). We derived active parameter counts by downloading the checkpoints’ safetensors headers from HuggingFace, reading each tensor’s shape, and adding up the sizes of all active parameters except token embeddings and MTP heads. 6·N·D for each model:

   | Model               | N              | D     | 6·N·D   |
   | ------------------- | -------------- | ----- | ------- |
   | Current\_e24 (ours) | -              | -     | 1.63e24 |
   | Current\_e23 (ours) | -              | -     | 1.58e23 |
   | Current\_e22 (ours) | -              | -     | 1.12e22 |
   | V4 (ours)           | -              | -     | 1.91e24 |
   | V3 (ours)           | -              | -     | 3.85e24 |
   | V2 (ours)           | -              | -     | 7.20e23 |
   | DeepSeek V4 Pro     | 48,852,265,054 | 33T   | 9.67e24 |
   | DeepSeek V4 Flash   | 13,270,025,810 | 32T   | 2.55e24 |
   | Kimi K2             | 31,687,072,768 | 15.5T | 2.95e24 |
   | Nemotron 3 Ultra    | 54,985,076,736 | 20T   | 6.60e24 |

   Sources: Kimi K2: 31.6B params, 15.5T tokens ([tech report](https://arxiv.org/abs/2507.20534), Section 2.5). DeepSeek V4 Flash: 13.2B params, 32T tokens ([tech report](https://arxiv.org/abs/2606.19348), Section 4.2.2) and DeepSeek V4 Pro: 48.8B params, 33T tokens (Section 4.2.2). Nemotron 3 Ultra: 55B params, 20T tokens ([tech report](https://arxiv.org/abs/2512.20856), abstract). [↩](https://magic.dev/blog/pretraining#user-content-fnref-flops)

2. “Base models” are pretrained models that have not yet undergone reinforcement learning, SFT, or other post-training. They are highly sensitive to prompting, making sampling-based evals unreliable. Instead, we measured bits-per-byte loss on heldout data, which does not suffer from prompt sensitivity and smooths measurement of otherwise [emergent abilities](http://arxiv.org/abs/2206.07682). As a side note, we were surprised that Nemotron 3 outperforms DeepSeek V4 Pro across the board but found this to be consistent across domains and inference engines. This might indicate that Nemotron’s weak performance on benchmarks after RL is due to weaker post-training, but the pretrain was ahead of Chinese open-weight competitors. [↩](https://magic.dev/blog/pretraining#user-content-fnref-base)

3. We can of course only decontaminate evals for our own models, not open-weight models. This means some of the data in our heldout internet-based evals may naturally be in their training data, but any contamination on their side would favor open models, not ours. [↩](https://magic.dev/blog/pretraining#user-content-fnref-contamination)

4. There are some outliers where we suspect source memorization to distort results. For example, we find that Nemotron has memorized specific numbers and phrases from source documents in the medicine/health world knowledge category and predicts these shockingly well even after LLM rewriting of the documents. We still included these outliers in our plot. All knowledge evals were created after our current-generation pretraining run started training and the vast majority of categories are new (i.e. they had no direct feedback loop into the run), but knowledge evals intentionally target semantic content that we expect appears frequently (and intentionally so) in anyone’s training data. [↩](https://magic.dev/blog/pretraining#user-content-fnref-memorization)

5. We evaluated pass rate on a private heldout competition math eval (400 problems) that’s harder than AIME. Baseline models presumably use orders of magnitude more RL compute. The e24 model’s AIME26 pass@1 first crossed 90% (and 100% pass@16) at ~0.2% of its pretraining compute budget, using just over 6k CoT tokens on average, and displays continued log-linear climbing. We report performance on a heldout dataset instead as we found AIME to be contaminated in some third-party models. [↩](https://magic.dev/blog/pretraining#user-content-fnref-aime)

6. We plan to release an updated [AGI Readiness Policy](https://magic.dev/agi-readiness-policy) covering deployment gates, safety requirements during RL training, and how we approach alignment research. [↩](https://magic.dev/blog/pretraining#user-content-fnref-agi)
