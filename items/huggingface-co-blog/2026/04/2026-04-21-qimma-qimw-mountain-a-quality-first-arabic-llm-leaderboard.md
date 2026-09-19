---
title: 'QIMMA قِمّة ⛰: A Quality-First Arabic LLM Leaderboard'
link: https://huggingface.co/blog/tiiuae/qimma-arabic-leaderboard
source: huggingface-co-blog
published: 2026-04-21T10:09:58Z
updated: 2026-04-21T10:09:58Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2026-04-21-qimma-qimw-mountain-a-quality-first-arabic-llm-leaderboard.html
preview:
  file: 2026-04-21-qimma-qimw-mountain-a-quality-first-arabic-llm-leaderboard.preview-84c18cabf50f.webp
  width: 256
  height: 171
  color: '#e3decd'
images:
- source: https://cdn-uploads.huggingface.co/production/uploads/659bc8a7b0f43ed69f0b2300/KdH3z5dRfyIAyxgGPN5Ey.png
  original:
    file: 2026-04-21-qimma-qimw-mountain-a-quality-first-arabic-llm-leaderboard.image-d55cd240a195.png
    width: 1536
    height: 1024
  variants:
  - file: 2026-04-21-qimma-qimw-mountain-a-quality-first-arabic-llm-leaderboard.image-2e95d28deac3.webp
    width: 320
    height: 213
  - file: 2026-04-21-qimma-qimw-mountain-a-quality-first-arabic-llm-leaderboard.image-ba97ed501668.webp
    width: 640
    height: 427
  - file: 2026-04-21-qimma-qimw-mountain-a-quality-first-arabic-llm-leaderboard.image-6c38b0156a99.webp
    width: 960
    height: 640
  - file: 2026-04-21-qimma-qimw-mountain-a-quality-first-arabic-llm-leaderboard.image-c49987d5413d.webp
    width: 1280
    height: 853
  - file: 2026-04-21-qimma-qimw-mountain-a-quality-first-arabic-llm-leaderboard.image-1ae18985be75.webp
    width: 1536
    height: 1024
  color: '#f8fafb'
- source: https://cdn-uploads.huggingface.co/production/uploads/66c8620a79b42e5c941b0265/NCnP1M7Ce__41kiS-V7hW.png
  original:
    file: 2026-04-21-qimma-qimw-mountain-a-quality-first-arabic-llm-leaderboard.image-dfb53e5a475d.png
    width: 2874
    height: 1488
  color: '#111729'
- source: https://cdn-uploads.huggingface.co/production/uploads/66c8620a79b42e5c941b0265/JFGUze00t2twAEsj6XxCm.png
  original:
    file: 2026-04-21-qimma-qimw-mountain-a-quality-first-arabic-llm-leaderboard.image-9d69fa864413.png
    width: 2860
    height: 1622
  color: '#131828'
- source: https://cdn-uploads.huggingface.co/production/uploads/66c8620a79b42e5c941b0265/KkWnEuAunRTalBhMkPQ02.png
  original:
    file: 2026-04-21-qimma-qimw-mountain-a-quality-first-arabic-llm-leaderboard.image-37e275f276c1.png
    width: 1329
    height: 693
  variants:
  - file: 2026-04-21-qimma-qimw-mountain-a-quality-first-arabic-llm-leaderboard.image-e970ec15d972.webp
    width: 320
    height: 167
  - file: 2026-04-21-qimma-qimw-mountain-a-quality-first-arabic-llm-leaderboard.image-c3a36634f306.webp
    width: 640
    height: 334
  - file: 2026-04-21-qimma-qimw-mountain-a-quality-first-arabic-llm-leaderboard.image-f54841125410.webp
    width: 960
    height: 501
  - file: 2026-04-21-qimma-qimw-mountain-a-quality-first-arabic-llm-leaderboard.image-0941fbb1d381.webp
    width: 1329
    height: 693
  color: '#f3e8f9'
- source: https://cdn-uploads.huggingface.co/production/uploads/659bc8a7b0f43ed69f0b2300/C-WxGverw4w_pnf6IRUsB.png
  original:
    file: 2026-04-21-qimma-qimw-mountain-a-quality-first-arabic-llm-leaderboard.image-fcec2f19611e.png
    width: 1556
    height: 780
  color: '#fbfcfc'
- source: https://cdn-uploads.huggingface.co/production/uploads/66c8620a79b42e5c941b0265/KPdYTBwzMvJEyALTYfGji.png
  original:
    file: 2026-04-21-qimma-qimw-mountain-a-quality-first-arabic-llm-leaderboard.image-5289cda4f846.png
    width: 2433
    height: 1500
  color: '#fcfcfc'
---

[![image](https://cdn-uploads.huggingface.co/production/uploads/66c8620a79b42e5c941b0265/NCnP1M7Ce__41kiS-V7hW.png)](https://cdn-uploads.huggingface.co/production/uploads/66c8620a79b42e5c941b0265/NCnP1M7Ce__41kiS-V7hW.png)

> **QIMMA validates benchmarks before evaluating models, ensuring reported scores reflect genuine Arabic language capability in LLMs.**

If you've been tracking Arabic LLM evaluation, you've probably noticed a growing tension: the number of benchmarks and leaderboards is expanding rapidly, but **are we actually measuring what we think we're measuring?**

We built **QIMMA** قمّة (Arabic for "summit"), to answer that question systematically. Instead of aggregating existing Arabic benchmarks as-is and running models on them, we applied a rigorous quality validation pipeline *before* any evaluation took place. What we found was sobering: even widely-used, well-regarded Arabic benchmarks contain systematic quality issues that can quietly corrupt evaluation results.

This post walks through what QIMMA is, how we built it, what problems we found, and what the model rankings look like once you clean things up.

[![image](https://cdn-uploads.huggingface.co/production/uploads/66c8620a79b42e5c941b0265/JFGUze00t2twAEsj6XxCm.png)](https://cdn-uploads.huggingface.co/production/uploads/66c8620a79b42e5c941b0265/JFGUze00t2twAEsj6XxCm.png)

* * *

## [](https://huggingface.co/blog/tiiuae/qimma-arabic-leaderboard#%F0%9F%94%8D-the-problem-arabic-nlp-evaluation-is-fragmented-and-unvalidated) 🔍 The Problem: Arabic NLP Evaluation Is Fragmented and Unvalidated

Arabic is spoken by over 400 million people across diverse dialects and cultural contexts, yet the Arabic NLP evaluation landscape remains fragmented. A few key pain points have motivated this work:

**Translation issues.** Many Arabic benchmarks are translations from English. This introduces distributional shifts. Questions that feel natural in English become awkward or culturally misaligned in Arabic, making benchmark data less representative of how Arabic is naturally used.

**Absent quality validation.** Even *native* Arabic benchmarks are often released without rigorous quality checks. Annotation inconsistencies, incorrect gold answers, encoding errors, and cultural bias in ground-truth labels have all been documented in established resources.

**Reproducibility gaps.** Evaluation scripts and per-sample outputs are rarely released publicly, making it hard to audit results or build on prior work.

**Coverage fragmentation.** Existing leaderboards cover isolated tasks and narrow domains, making holistic model assessment difficult.

To illustrate where QIMMA sits relative to existing platforms:

| Leaderboard | Open Source | Native Arabic | Quality Validation | Coding Eval | Public Outputs |
| ----------- | ----------- | ------------- | ------------------ | ----------- | -------------- |
| OALL v1     | ✅           | Mixed         | ❌                  | ❌           | ✅              |
| OALL v2     | ✅           | Mostly        | ❌                  | ❌           | ✅              |
| BALSAM      | Partial     | 50%           | ❌                  | ❌           | ❌              |
| AraGen      | ✅           | 100%          | ✅                  | ❌           | ❌              |
| SILMA ABL   | ✅           | 100%          | ✅                  | ❌           | ✅              |
| ILMAAM      | Partial     | 100%          | ✅                  | ❌           | ❌              |
| HELM Arabic | ✅           | Mixed         | ❌                  | ❌           | ✅              |
| ⛰ QIMMA     | ✅           | 99%           | ✅                  | ✅           | ✅              |

QIMMA is the only platform combining all five properties: open source, predominantly native Arabic content, systematic quality validation, code evaluation, and public per-sample inference outputs.

* * *

## [](https://huggingface.co/blog/tiiuae/qimma-arabic-leaderboard#%E2%9B%B0-whats-in-qimma) ⛰ What's in QIMMA?

QIMMA consolidates **109 subsets** from **14 source benchmarks** into a unified evaluation suite of over **52,000 samples**, spanning 7 domains:

| Domain                  | Benchmarks                          | Task Types |
| ----------------------- | ----------------------------------- | ---------- |
| **Cultural**            | AraDiCE-Culture, ArabCulture, PalmX | MCQ        |
| **STEM**                | ArabicMMLU, GAT, 3LM STEM           | MCQ        |
| **Legal**               | ArabLegalQA, MizanQA                | MCQ, QA    |
| **Medical**             | MedArabiQ, MedAraBench              | MCQ, QA    |
| **Safety**              | AraTrust                            | MCQ        |
| **Poetry & Literature** | FannOrFlop                          | QA         |
| **Coding**              | 3LM HumanEval+, 3LM MBPP+           | Code       |

A few things stand out about this design:

- **99% native Arabic content.** The only exception is code evaluation, which is inherently language-agnostic.
- **First Arabic leaderboard with code evaluation.** QIMMA integrates Arabic-adapted versions of HumanEval+ and MBPP+, making it possible to assess coding capability with Arabic-language problem statements.
- **Diversity in Domains and Tasks.** QIMMA evaluates real-world competency areas including education, governance, healthcare, creative expression, and software development.

* * *

## [](https://huggingface.co/blog/tiiuae/qimma-arabic-leaderboard#%F0%9F%94%AC-the-quality-validation-pipeline) 🔬 The Quality Validation Pipeline

This is the methodological heart of QIMMA. Before running a single model, we applied a **multi-stage validation pipeline** to every sample in every benchmark.

### [](https://huggingface.co/blog/tiiuae/qimma-arabic-leaderboard#stage-1-multi-model-automated-assessment) Stage 1: Multi-Model Automated Assessment

Each sample was independently evaluated by two state-of-the-art LLMs:

- **Qwen3-235B-A22B-Instruct**
- **DeepSeek-V3-671B**

We chose two models with strong Arabic capability but different training data compositions, so that their *combined* judgment is more robust than either alone.

Each model scores a sample against a **10-point rubric**, with binary scores (0 or 1) per criterion:

![QIMMA pipeline](https://cdn-uploads.huggingface.co/production/uploads/66c8620a79b42e5c941b0265/KkWnEuAunRTalBhMkPQ02.png)

A sample is eliminated if either model scores it below 7/10. Samples where both models agree on elimination are dropped immediately. However, where only one model flags a sample, it proceeds to human review in Stage 2.

### [](https://huggingface.co/blog/tiiuae/qimma-arabic-leaderboard#stage-2-human-annotation-and-review) Stage 2: Human Annotation and Review

Flagged samples are reviewed by **native Arabic speakers** with cultural and dialectal familiarity. Human annotators make final calls on:

- Cultural context and regional variation
- Dialectal nuance
- Subjective interpretation
- Subtle quality issues automated assessment may miss

For culturally sensitive content, multiple perspectives are considered, since "correctness" can genuinely vary across Arab regions.

* * *

## [](https://huggingface.co/blog/tiiuae/qimma-arabic-leaderboard#%E2%9A%A0%EF%B8%8F-what-we-found-systematic-quality-problems) ⚠️ What We Found: Systematic Quality Problems

The pipeline revealed recurring quality issues across benchmarks; not isolated errors, but **systematic patterns** reflecting gaps in how benchmarks were originally constructed.

### [](https://huggingface.co/blog/tiiuae/qimma-arabic-leaderboard#by-the-numbers) By the Numbers

| Benchmark       | Total Samples | Discarded | Discard Rate |
| --------------- | ------------- | --------- | ------------ |
| ArabicMMLU      | 14,163        | 436       | 3.1%         |
| MizanQA         | 1,769         | 41        | 2.3%         |
| PalmX           | 3,001         | 25        | 0.8%         |
| MedAraBench     | 4,960         | 33        | 0.7%         |
| FannOrFlop      | 6,984         | 43        | 0.6%         |
| ArabCulture     | 3,482         | 7         | 0.2%         |
| MedArabiQ       | 499           | 1         | 0.2%         |
| GAT             | 13,986        | 1         | \~0.0%       |
| 3LM STEM        | 2,609         | 1         | \~0.0%       |
| AraDiCE-Culture | 180           | 0         | 0.0%         |
| ArabLegalQA     | 79            | 0         | 0.0%         |
| AraTrust        | 522           | 0         | 0.0%         |

### [](https://huggingface.co/blog/tiiuae/qimma-arabic-leaderboard#taxonomy-of-issues-found) Taxonomy of Issues Found

⚖️ Answer Quality

False or mismatched gold indices, factually wrong answers, missing or raw text answers.

📄 Text & Formatting Quality

Corrupt or illegible text, spelling and grammar errors, and duplicate samples.

💬 Cultural Sensitivity

Stereotype reinforcement and monolithic generalizations about diverse communities.

🤝 Gold Answer Compliance

Misalignment of gold answers with evaluation protocols.

* * *

## [](https://huggingface.co/blog/tiiuae/qimma-arabic-leaderboard#%F0%9F%92%BB-code-benchmark-a-different-kind-of-quality-work) 💻 Code Benchmark: A Different Kind of Quality Work

Code benchmarks required a different intervention. Rather than discarding samples, we **refined the Arabic problem statements** in 3LM's Arabic adaptations of HumanEval+ and MBPP+, leaving task identifiers, reference solutions, and test suites completely unchanged.

The modification rates were striking:

| Benchmark      | Total Prompts | Modified | Unchanged | Modification Rate |
| -------------- | ------------- | -------- | --------- | ----------------- |
| 3LM HumanEval+ | 164           | 145      | 19        | **88%**           |
| 3LM MBPP+      | 378           | 308      | 70        | **81%**           |

Modifications fell into five categories:

1. **Linguistic refinement** : normalizing toward natural Modern Standard Arabic and consistent imperative style
2. **Clarity improvements** : fixing ambiguous instructions and unclear constraints
3. **Consistency normalization** : standardizing mathematical terminology, punctuation, and example formatting
4. **Structural corrections** : fixing broken triple-quoted strings, indentation errors, corrupted text fragments
5. **Semantic refinements** : clarifying whether ranges are inclusive/exclusive, preserving task intent

* * *

## [](https://huggingface.co/blog/tiiuae/qimma-arabic-leaderboard#%E2%9A%99%EF%B8%8F-evaluation-setup) ⚙️ Evaluation Setup

### [](https://huggingface.co/blog/tiiuae/qimma-arabic-leaderboard#evaluation-framework) Evaluation Framework

QIMMA uses [LightEval](https://github.com/huggingface/lighteval), [EvalPlus](https://github.com/evalplus/evalplus) and [FannOrFlop](https://github.com/mbzuai-oryx/FannOrFlop) as its evaluation framework, chosen for consistency, multilingual community adoption, and reproducibility.

### [](https://huggingface.co/blog/tiiuae/qimma-arabic-leaderboard#metrics-by-task-type) Metrics by Task Type

| Task Type            | Metric                             | Benchmarks                                                                                       |
| -------------------- | ---------------------------------- | ------------------------------------------------------------------------------------------------ |
| **MCQ**              | Normalized Log-Likelihood Accuracy | AraDiCE-Culture, ArabicMMLU, ArabCulture, PalmX, 3LM STEM, MedArabiQ, GAT, MedAraBench, AraTrust |
| **Multi-select MCQ** | Probability Mass on Gold Choices   | MizanQA                                                                                          |
| **Generative QA**    | F1 BERTScore (AraBERT v02)         | MedArabiQ, ArabLegalQA, FannOrFlop                                                               |
| **Code**             | Pass@1                             | 3LM HumanEval+, 3LM MBPP+                                                                        |

### [](https://huggingface.co/blog/tiiuae/qimma-arabic-leaderboard#prompt-templates) Prompt Templates

QIMMA standardizes prompting by question format, with six template types:

![QIMMA prompt templates](https://cdn-uploads.huggingface.co/production/uploads/659bc8a7b0f43ed69f0b2300/C-WxGverw4w_pnf6IRUsB.png)

**MCQ**: generic multiple choice · **MCQ-C**: multiple choice with context passage · **MCQ-I**: multiple choice with specific instructions (GAT analogy/completion) · **QA**: generic open-ended QA · **QA-C**: QA with context · **QA-F**: fill-in-the-blank QA

All prompts are in Arabic. For MizanQA and ArabCulture, benchmark-specific system prompts from the original papers are preserved.

* * *

## [](https://huggingface.co/blog/tiiuae/qimma-arabic-leaderboard#%F0%9F%8F%86-leaderboard-results) 🏆 Leaderboard Results

*Results as of April 2026; covering top 10 evaluated models. Visit the [live leaderboard](https://huggingface.co/spaces/qimma/leaderboard) for current rankings.*

| Rank | Model                                  | AVERAGE   | AraDiCE-Culture | ArabicMMLU | ArabCulture | PALMX     | 3LM STEM  | AraTrust  | MizanQA   | MedArabiQ | ArabLegalQA | GAT       | MedAraBench | HumanEval+ | MBPP+     | FannOrFlop |
| ---- | -------------------------------------- | --------- | --------------- | ---------- | ----------- | --------- | --------- | --------- | --------- | --------- | ----------- | --------- | ----------- | ---------- | --------- | ---------- |
| 🥇 1  | Qwen/Qwen3.5-397B-A17B-FP8             | **68.06** | **82.78**       | 77.54      | 61.75       | **83.91** | 88.67     | 90.04     | **73.36** | 47.30     | 54.94       | 55.89     | 47.97       | **67.68**  | **76.72** | 44.33      |
| 🥈 2  | Applied-Innovation-Center/Karnak       | 66.20     | 73.33           | 80.94      | 53.49       | 81.40     | **93.10** | 89.08     | 55.92     | 55.78     | **71.58**   | 61.06     | 54.19       | 33.54      | 64.55     | 58.91      |
| 🥉 3  | inceptionai/Jais-2-70B-Chat            | 65.81     | 78.89           | **81.29**  | **83.24**   | 83.73     | 87.96     | 90.23     | 71.78     | 52.79     | 69.60       | 51.67     | 50.89       | 19.51      | 43.65     | 56.13      |
| #4   | Qwen/Qwen2.5-72B-Instruct              | 65.75     | 77.22           | 73.78      | 63.83       | 77.77     | 87.55     | 88.51     | 63.49     | 50.06     | 70.74       | 55.90     | 44.19       | 37.20      | 72.75     | 57.51      |
| #5   | Applied-Innovation-Center/AIC-1        | 65.37     | 73.33           | 72.02      | 77.52       | 76.11     | 88.13     | **90.61** | 56.36     | 53.75     | 68.96       | **62.11** | 50.78       | 28.05      | 69.58     | 47.83      |
| #6   | Qwen/Qwen3.5-122B-A10B                 | 64.84     | 74.44           | 73.17      | 37.78       | 81.46     | 86.18     | 86.97     | 64.01     | 47.04     | 55.11       | 50.90     | 52.49       | 65.24      | 72.43     | 60.54      |
| #7   | Sakalti/Ultiima-72B                    | 64.49     | 78.33           | 72.28      | 68.79       | 76.75     | 83.70     | 89.08     | 60.44     | 44.58     | 69.12       | 46.91     | 42.25       | 39.02      | 74.07     | 57.56      |
| #8   | meta-llama/Llama-3.3-70B-Instruct      | 63.96     | 77.22           | 71.57      | 78.05       | 77.95     | 88.28     | 85.63     | 67.44     | **56.25** | 64.00       | 51.13     | **54.86**   | 27.44      | 71.16     | 24.43      |
| #9   | Qwen/Qwen2.5-32B-Instruct              | 63.26     | 70.56           | 68.76      | 75.80       | 72.07     | 81.03     | 85.82     | 53.78     | 48.08     | 69.27       | 56.94     | 36.51       | 34.15      | 72.75     | **93.10**  |
| #10  | FreedomIntelligence/AceGPT-v2-32B-Chat | 61.14     | 76.67           | 70.62      | 79.79       | 74.46     | 84.88     | 86.97     | 63.89     | 49.96     | 71.46       | 56.04     | 47.32       | 23.78      | 54.50     | 15.56      |

- **Scale does not guarantee best performance.** The top 10 spans models from 32B to 397B parameters, with several mid-size models outperforming larger ones on specific domains.
- **Arabic-specialized models lead on cultural and linguistic tasks.** Jais-2-70B-Chat ranks highest on ArabicMMLU and ArabCulture, while Karnak leads on 3LM STEM and ArabLegalQA.
- **Coding remains the hardest domain for Arabic-specialized models.** The top HumanEval+ and MBPP+ scores belong to multilingual models, with Qwen3.5-397B leading both.

### [](https://huggingface.co/blog/tiiuae/qimma-arabic-leaderboard#the-size-performance-relationship) The Size-Performance Relationship

Across the full leaderboard (46 models), a clear but imperfect size-performance correlation emerges. However, there are interesting exceptions:

[![c64aafc7-1](https://cdn-uploads.huggingface.co/production/uploads/66c8620a79b42e5c941b0265/KPdYTBwzMvJEyALTYfGji.png)](https://cdn-uploads.huggingface.co/production/uploads/66c8620a79b42e5c941b0265/KPdYTBwzMvJEyALTYfGji.png)

- Arabic-specialized models often outperform size-matched multilingual models
- Instruction-tuned models consistently outperform their base counterparts except for Qwen3
- Some smaller Arabic-specialized models (Fanar-1-9B, ALLaM-7B) outperform much larger multilingual models on specific domains

* * *

## [](https://huggingface.co/blog/tiiuae/qimma-arabic-leaderboard#%F0%9F%8C%9F-what-makes-qimma-different) 🌟 What Makes QIMMA Different

To summarize the distinctive properties of QIMMA:

| Property                     | Details                                                                   |
| ---------------------------- | ------------------------------------------------------------------------- |
| **Quality-first philosophy** | Validation runs *before* evaluation, not as an afterthought               |
| **Multi-model validation**   | Two LLMs with different training + human review for flagged cases         |
| **99% native Arabic**        | Avoids translation artifacts almost entirely                              |
| **Multi-domain, multi-task** | 7 domains, 3 task types (MCQ, QA, code), 109 subsets                      |
| **Code evaluation**          | First Arabic leaderboard to include code generation                       |
| **Full transparency**        | Per-sample inference outputs publicly released, not just aggregate scores |
| **LightEval-based**          | Unified, reproducible evaluation codebase                                 |
| **Dialectal awareness**      | Explicit handling of MSA vs. dialectal variation in prompts and rubrics   |

* * *

## [](https://huggingface.co/blog/tiiuae/qimma-arabic-leaderboard#%F0%9F%94%97-resources) 🔗 Resources

- 🏆 **Leaderboard**: [QIMMA Leaderboard](https://huggingface.co/spaces/qimma/leaderboard)
- 💻 **Code**: [GitHub](https://github.com/tiiuae/QIMMA-leaderboard.git)
- 📄 **Paper**: [Are Arabic Benchmarks Reliable? QIMMA's Quality-First Approach to LLM Evaluation](https://arxiv.org/pdf/2604.03395)

* * *

## [](https://huggingface.co/blog/tiiuae/qimma-arabic-leaderboard#%F0%9F%94%96-citation) 🔖 Citation

```
@misc{alqadi2026arabicbenchmarksreliableqimmas,
      title={Are Arabic Benchmarks Reliable? QIMMA's Quality-First Approach to LLM Evaluation}, 
      author={Leen AlQadi and Ahmed Alzubaidi and Mohammed Alyafeai and Hamza Alobeidli and Maitha Alhammadi and Shaikha Alsuwaidi and Omar Alkaabi and Basma El Amel Boussaha and Hakim Hacid},
      year={2026},
      eprint={2604.03395},
      archivePrefix={arXiv},
      primaryClass={cs.CL},
      url={https://arxiv.org/abs/2604.03395}, 
}
```
