---
title: PerceptionBench
link: https://www.kimi.com/en/blog/perception-bench
source: kimi-com-en-blog
published: 2026-07-16T00:00:00Z
first_seen: 2026-09-08T10:17:10.166315Z
summary: 2026-07-16
content: extracted
html: 2026-07-16-perceptionbench.html
preview:
  file: 2026-07-16-perceptionbench.preview-a3c7f817a55a.webp
  width: 256
  height: 143
  alt: PerceptionBench
  color: '#383940'
images:
- source: https://kimi-file.kimi.ai/prod-chat-kimi/kfs/4/2/2026-07-17/d9cs7m9l51jas5bslg30?x-tos-process=image%2Fauto-orient%2C1%2Fstrip%2Fignore-error%2C1
  original:
    file: 2026-07-16-perceptionbench.image-44a254493f51.webp
    width: 2912
    height: 1632
  variants:
  - file: 2026-07-16-perceptionbench.image-1f70edc9378c.webp
    width: 48
    height: 27
  color: '#040608'
- source: https://kimi-file.kimi.ai/prod-chat-kimi/kfs/4/2/2026-07-24/1d9hj9od3v89kken0j8dg?x-tos-process=image%2Fauto-orient%2C1%2Fstrip%2Fignore-error%2C1
  original:
    file: 2026-07-16-perceptionbench.image-fe28110088af.webp
    width: 2843
    height: 1132
  variants:
  - file: 2026-07-16-perceptionbench.image-d95506f8cfe7.webp
    width: 48
    height: 19
  - file: 2026-07-16-perceptionbench.image-b1b329a5e01a.webp
    width: 320
    height: 127
  - file: 2026-07-16-perceptionbench.image-3efad36bfb93.webp
    width: 640
    height: 255
  color: '#f4f3f4'
- source: https://kimi-file.kimi.ai/prod-chat-kimi/kfs/4/2/2026-07-23/1d9h01rl3v89kkemvehg0?x-tos-process=image%2Fauto-orient%2C1%2Fstrip%2Fignore-error%2C1
  original:
    file: 2026-07-16-perceptionbench.image-80f4aee05d86.webp
    width: 2781
    height: 2699
  variants:
  - file: 2026-07-16-perceptionbench.image-85a02da7e2af.webp
    width: 48
    height: 47
  - file: 2026-07-16-perceptionbench.image-8bc43c4b0b4e.webp
    width: 320
    height: 311
  - file: 2026-07-16-perceptionbench.image-4b42eeead6b1.webp
    width: 640
    height: 621
  color: '#fafafa'
---

1. [Research](https://www.kimi.com/en/blog/)

## Introducing PerceptionBench

**Evaluating Atomic Visual Perception in Multimodal Large Language Models**

**Authors** Kimi Team

* * *

## Overview

We are releasing **PerceptionBench**, a benchmark that isolates visual perception and evaluates it as a set of atomic capabilities—**discovered from how today's models fail, not defined in advance.** From frontier-model failures across 42 benchmarks, we derive 10 atomic perceptual capabilities and construct 3,000 verified questions, each isolating a single capability and answerable by looking, with no reasoning or external knowledge required.

Across sixteen frontier MLLMs, no model reaches 60% accuracy, and perception-related hallucination is the weakest capability on average. Models with nearly identical overall scores can diverge sharply in what they actually perceive. PerceptionBench is built to expose exactly where perception breaks, and to drive progress toward multimodal AI that sees faithfully and consistently.

![PerceptionBench versus existing evaluation: an existing benchmark item couples perception with knowledge and reasoning in a single question, while PerceptionBench evaluates ten atomic perceptual categories with one isolating question each](https://kimi-file.kimi.ai/prod-chat-kimi/kfs/4/2/2026-07-24/1d9hj9od3v89kken0j8dg?x-tos-process=image%2Fauto-orient%2C1%2Fstrip%2Fignore-error%2C1)

## The Dataset

Guided by the induced taxonomy, we select the most informative failures from the source benchmarks, decompose them into atomic sub-questions, and author additional questions on supplemented images. The retained and constructed samples together form an in-house pool of **17,000+** verified questions. The released benchmark subsamples **3,000** verified questions from the constructed portion—**60% decomposed** from attributed model failures and **40% newly authored**—with category-level balancing and difficulty stratification to isolate atomic perceptual capabilities from confounding factors. The released benchmark distinguishes itself through three core design principles:

- **Failure-Driven Taxonomy:** Every category is discovered from real model failures, attributed to the earliest erroneous step across 42 existing benchmarks.
- **Ten Atomic Perceptual Categories:** Visual Relation, Counting, Attribute, Depth & 3D, Localization, Comparison, Fine-grained Recognition, Context Integration, OCR, and Hallucination.
- **Perception, Not Reasoning or Knowledge:** Samples are curated, decomposed, and difficulty-balanced so that difficulty stems from perception rather than reasoning or external knowledge.

![Qualitative examples: four source-benchmark items whose original questions require multi-step solutions are decomposed into atomic perception-only sub-questions, shown with ground truth and the answers of Kimi K3, GPT-5.6-Sol, Claude-Fable-5, and Gemini-3.1-Pro](https://kimi-file.kimi.ai/prod-chat-kimi/kfs/4/2/2026-07-23/1d9h01rl3v89kkemvehg0?x-tos-process=image%2Fauto-orient%2C1%2Fstrip%2Fignore-error%2C1)

* * *

Each source benchmark captures a narrow slice of perception errors, and these slices overlap only weakly (mean pairwise weighted Jaccard 0.20). No single benchmark—or small group of them—covers perception as a whole, which motivates a capability-centric benchmark that aggregates and rebalances these fragmented views.

![Distribution of attributed failures across error types for each of the 42 aggregated open-source benchmarks; each benchmark's failures concentrate on one or a few error types, while the ten perception-branch types recur across nearly all benchmarks](https://www.kimi.com/landing-ui/assets/blog-assets/perception-bench/openbench_jaccard.svg)

### Distribution of Tasks per Category

| Statistics                     | Number       |
| ------------------------------ | ------------ |
| Data                           |              |
| Total                          | 3,000        |
| Atomic perceptual categories   | 10           |
| Task Categories                |              |
| Depth 3D Perception Error      | 330 (11.00%) |
| Visual Counting Error          | 330 (11.00%) |
| Fine-Grained Recognition Error | 290 (9.67%)  |
| Visual Relation Error          | 330 (11.00%) |
| Visual Attribute Error         | 330 (11.00%) |
| Visual Localization Error      | 330 (11.00%) |
| Visual Comparison Error        | 279 (9.30%)  |
| Context Integration Error      | 255 (8.50%)  |
| Hallucination                  | 271 (9.03%)  |
| OCR Error                      | 255 (8.50%)  |

## Using PerceptionBench to Compare Models

## Conclusion

PerceptionBench is a simple but challenging benchmark for evaluating the atomic visual perception of frontier models. It provides a capability-level standard for measuring and diagnosing the visual perception boundaries of multimodal models. We are open-sourcing the PerceptionBench dataset and evaluation code to help the community address the visual perception gap.

- **Download the Data:** [https://github.com/MoonshotAI/PerceptionBench](https://github.com/MoonshotAI/PerceptionBench)
