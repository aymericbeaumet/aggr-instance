---
title: Thinking of ACE? We Can Do It with Fewer Tokens
link: https://huggingface.co/blog/ibm-research/altk-evolve-sldd
source: huggingface-co-blog
published: 2026-08-11T13:37:10Z
updated: 2026-08-11T13:37:10Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2026-08-11-thinking-of-ace-we-can-do-it-with-fewer-tokens.html
preview:
  file: 2026-08-11-thinking-of-ace-we-can-do-it-with-fewer-tokens.preview-04a5d62b2f24.webp
  width: 256
  height: 134
  color: '#d3dce7'
images:
- source: https://cdn-uploads.huggingface.co/production/uploads/6435a1131860001f144239ea/G2z5bU66FNnhBf_UhSNij.jpeg
  original:
    file: 2026-08-11-thinking-of-ace-we-can-do-it-with-fewer-tokens.image-09017bf61216.jpg
    width: 1910
    height: 1000
  color: '#f8fafc'
- source: https://cdn-uploads.huggingface.co/production/uploads/6435a1131860001f144239ea/RlNmW6Vps10XLGW4DuJQh.png
  original:
    file: 2026-08-11-thinking-of-ace-we-can-do-it-with-fewer-tokens.image-b2ca7be7ed39.png
    width: 1800
    height: 810
  variants:
  - file: 2026-08-11-thinking-of-ace-we-can-do-it-with-fewer-tokens.image-4050be961314.webp
    width: 320
    height: 144
  - file: 2026-08-11-thinking-of-ace-we-can-do-it-with-fewer-tokens.image-14247fbeeff7.webp
    width: 640
    height: 288
  - file: 2026-08-11-thinking-of-ace-we-can-do-it-with-fewer-tokens.image-62d3ce213748.webp
    width: 960
    height: 432
  - file: 2026-08-11-thinking-of-ace-we-can-do-it-with-fewer-tokens.image-f1ef1222e6ad.webp
    width: 1280
    height: 576
  - file: 2026-08-11-thinking-of-ace-we-can-do-it-with-fewer-tokens.image-98b1a8716216.webp
    width: 1600
    height: 720
  - file: 2026-08-11-thinking-of-ace-we-can-do-it-with-fewer-tokens.image-9eb757257b23.webp
    width: 1800
    height: 810
  color: '#fcfcfc'
---

*ALTK-Evolve and ACE both let an agent learn from its own trajectories. The difference is what they do with what they learn — and that decides the token bill.*

Give an LLM agent a realistic multi-step task — split a bill, find a song, reconcile an order across nine simulated apps — and when it fails, it usually isn't for lack of knowledge. It mis-paginates an API, resolves the wrong person, or returns a value when none was asked for. The model knows the APIs; what it hasn't internalized is *how to use them reliably*. That's learnable from the agent's own history.

Two recent systems do exactly this, on the same kind of agent: [**ACE**](https://arxiv.org/abs/2510.04618) (Agentic Context Engineering) and our **ALTK-Evolve** ([introduced here](https://huggingface.co/blog/ibm-research/altk-evolve)). Both are a form of **agentic memory** — turning an agent's past trajectories into reusable **lessons** and feeding them back at inference time, no weight updates, no human labels. They even agree on the hard part. Where they part ways is *delivery*.

A note on words, because the two systems name things differently: we'll call the raw thing an agent learns a **lesson**. ACE organizes its lessons into one comprehensive, evolving **playbook**; we consolidate ours into individually retrievable **guidelines**. Same lessons, two containers.

* * *

## [](https://huggingface.co/blog/ibm-research/altk-evolve-sldd#what-we-agree-on) What we agree on

Both systems refuse to compress.

ACE names the failure modes precisely: **brevity bias** — optimization collapsing toward short, generic instructions — and **context collapse** — a model asked to rewrite its whole context each step summarizing the detail away. Its answer is to keep a rich, itemized playbook, with a helpful/harmful counter on every bullet, and let the model distill relevance at read time.

We reach the same conclusion from the other direction. Every distinct guideline keeps a **support count** — how many independent episodes produced it — and we never summarize the store down to a handful of rules. A lesson five different tasks discovered is a different object from one that appeared once, and both are worth keeping.

So on the core question — *should you compress an agent's hard-won lessons into a tidy summary?* — ACE and ALTK-Evolve give the same answer: **no. Count them, don't collapse them.** ACE's per-bullet counters and our support counts are two spellings of the same idea.

* * *

## [](https://huggingface.co/blog/ibm-research/altk-evolve-sldd#where-we-differ) Where we differ

Two places: how the memory is **built**, and how it's **delivered** — and it's the delivery difference that shows up in the token bill.

**Consolidation (how the store is built).** ACE grows one playbook through a **Generator → Reflector → Curator** loop, applying incremental delta updates and de-duplicating by embedding. We cluster near-duplicate lessons and merge *within* a cluster, **support-conserving** — when several lessons merge, the survivor inherits their combined count, so the store shrinks without losing the record of how much experience backs each guideline. We also extract *typed* guidelines — strategy, recovery, and optimization — with causal attribution and provenance back to the source trajectory, and at subtask granularity, so a lesson learned on one app can transfer to another.

**Delivery (what reaches the model at inference).** This is the one that drives the numbers. ACE injects the **comprehensive playbook** on every step, the same way regardless of model or task. We treat delivery as a dial, not a constant: a small fixed core of high-support guidelines, extended per task with a handful selected for the task at hand (cosine or LLM-guided, priority-weighted) — or, when a model has the headroom to use it, the full consolidated set. The same lessons are *available* to both agents; the difference is that ACE always sends all of them, and we send however many a given model can actually use.

* * *

## [](https://huggingface.co/blog/ibm-research/altk-evolve-sldd#why-it-matters) Why it matters

On AppWorld, with the *same* base ReAct agent, running both systems in-house:

| Model             |                 | TGC / SGC       | Tokens/task |
| ----------------- | --------------- | --------------- | ----------- |
| **DeepSeek-V3.2** | ACE             | 80.4 / 73.2     | 634K        |
|                   | **ALTK-Evolve** | **89.3 / 80.4** | **263K**    |
| **gpt-oss-120b**  | ACE             | 54.8 / 35.7     | 777K        |
|                   | **ALTK-Evolve** | **56.0 / 37.5** | **116K**    |

On the strong model we're better on both metrics at **\~40% of ACE's inference cost**. On the weak model we edge ACE 56.0 to 54.8 — close enough that we call it a **tie on accuracy** (a repeat run of ours landed at 54.8, matching ACE almost exactly, which is within this benchmark's run-to-run noise) — at **about one-seventh** the cost.

A fair word on cost: ACE's own efficiency story is about *building* its context cheaply. Ours is on a different axis — *serving* it. Retrieving a few guidelines per task instead of injecting the whole playbook on every step is where the tokens go, and it's the direct consequence of the delivery difference above.

Where does the accuracy come from? The by-difficulty breakdown tells two different stories:

[![image](https://cdn-uploads.huggingface.co/production/uploads/6435a1131860001f144239ea/RlNmW6Vps10XLGW4DuJQh.png)](https://cdn-uploads.huggingface.co/production/uploads/6435a1131860001f144239ea/RlNmW6Vps10XLGW4DuJQh.png)

*Figure 1. Post-memory Task Goal Completion by difficulty, ours vs. ACE. On DeepSeek-V3.2 (right) we win Easy, Hard, and Overall; ACE only edges Medium. On gpt-oss-120b (left) ACE leads easy and medium, but per-task selection wins the hard tasks — and the aggregate. Each system improves from its own no-memory baseline (see the by-difficulty reference tables under Method notes below).*

The two models tell different stories. On gpt-oss-120b, ACE's full playbook has the edge on Easy and Medium — there's enough of the task solved by generic instruction-following that a comprehensive prompt helps more than it distracts. But on Hard tasks, where the model has to pick the *right* lesson rather than wade through all of them, curated retrieval pulls ahead — and that's the tier that decides the aggregate. On DeepSeek-V3.2 the story flips: the stronger model absorbs ACE's full playbook well enough to edge us on Medium, but we lead Easy, Hard, and Overall — with more capacity to spare, more lessons (delivered our way) keep helping instead of crowding each other out.

We give each model its **best configuration** — the full consolidated set for the strong model, selective retrieval for the weaker one, because a large context overwhelms a weaker model rather than helping it. (Exactly *how much* to inject, and how it scales across the capability spectrum, is the focus of our [**follow-up post**](https://huggingface.co/blog/ibm-research/altk-evolve-hmm/).)

* * *

## [](https://huggingface.co/blog/ibm-research/altk-evolve-sldd#same-lessons-different-delivery) Same lessons, different delivery

Both systems refuse to compress an agent's hard-won experience into a tidy summary — that part, we agree on. The difference is whether delivery is fixed or calibrated: ACE sends the whole playbook every step no matter what; we send however much of the guideline set a given model can actually use. That calibration is what bought the numbers above — same-or-better accuracy at a fraction of ACE's inference cost — and on the weaker model, it was the difference between guidance that helped and guidance that got in the way.

> **Try the [ALTK-Evolve](https://github.com/AgentToolkit/altk-evolve) library** — which includes the extraction, consolidation, and retrieval pipeline used here — **or read the [full technical report](https://arxiv.org/abs/2603.10600)** for the complete method and ablations.

* * *

## [](https://huggingface.co/blog/ibm-research/altk-evolve-sldd#linked-artifacts--references) Linked artifacts / references

- **Earlier post:** ALTK-Evolve introduction — [link](https://huggingface.co/blog/ibm-research/altk-evolve)
- **ACE** (Agentic Context Engineering) — [link](https://arxiv.org/abs/2510.04618)
- **AppWorld** benchmark — [link](https://appworld.dev/appworld)
- **ALTK-Evolve** — [link](https://github.com/AgentToolkit/altk-evolve)
- **Full technical report** — [link](https://arxiv.org/abs/2603.10600)

* * *

## [](https://huggingface.co/blog/ibm-research/altk-evolve-sldd#method-notes) Method notes

AppWorld `test_normal`, 168 tasks. A ReAct code agent (each step writes Python; the environment returns the output). **TGC** = Task Goal Completion; **SGC** = Scenario Goal Completion, which requires *every* variant of a scenario to pass. Memory is mined from **train/dev only**; results are single runs (pass@1), as is standard on this benchmark.

The ACE numbers are our own runs of the ACE agent, evaluated in-house on the same AppWorld splits and the same base models as ALTK-Evolve (DeepSeek-V3.2 and gpt-oss-120b). The ACE paper reports on a different base model (DeepSeek-V3.1), so running it ourselves keeps the comparison controlled for model and harness. Both systems are the *same* ReAct agent and differ only in the prompt template — which is why the two no-memory baselines differ (72.0 vs 79.8 TGC); we don't rest the comparison on that baseline gap, only on the claims a prompt tweak can't touch: same-or-better accuracy at a fraction of the tokens.

### [](https://huggingface.co/blog/ibm-research/altk-evolve-sldd#reference-tables) Reference tables

**DeepSeek-V3.2 — `test_normal` (168 tasks):**

| System              | Guidelines | TGC  | SGC  | Tokens/task |
| ------------------- | ---------- | ---- | ---- | ----------- |
| ReAct, no memory    | 0          | 79.8 | 64.3 | 148K        |
| ReAct + ACE         | 106        | 80.4 | 73.2 | 634K        |
| ReAct + ALTK-Evolve | 191        | 89.3 | 80.4 | 263K        |

**gpt-oss-120b — `test_normal`:**

| System                         | Guidelines | TGC  | SGC  | Tokens/task |
| ------------------------------ | ---------- | ---- | ---- | ----------- |
| ReAct, no memory               | 0          | 39.9 | 21.4 | 110K        |
| ReAct + ACE                    | full       | 54.8 | 35.7 | 777K        |
| ReAct + ALTK-Evolve (selected) | \~29       | 56.0 | 37.5 | 116K        |

**gpt-oss-120b — by difficulty (TGC):**

| Difficulty | Baseline | ACE  | ALTK-Evolve |
| ---------- | -------- | ---- | ----------- |
| Easy       | 66.7     | 84.2 | 82.5        |
| Medium     | 35.4     | 60.4 | 56.2        |
| Hard       | 19.1     | 23.8 | **31.8**    |
| Aggregate  | 39.9     | 54.8 | **56.0**    |

**DeepSeek-V3.2 — by difficulty (baseline → +memory):** the two systems start from different no-memory baselines (79.8 vs 72.0 TGC overall) because of the prompt-template difference above.

| Tier    | ALTK TGC    | ALTK SGC    | ACE TGC      | ACE SGC      |
| ------- | ----------- | ----------- | ------------ | ------------ |
| Overall | 79.8 → 89.3 | 64.3 → 80.4 | 72.0 → 80.4  | 57.1 → 73.2  |
| Easy    | 93.0 → 94.7 | 84.2 → 84.2 | 78.9 → 84.2  | 63.2 → 78.9  |
| Medium  | 81.2 → 97.9 | 62.5 → 93.8 | 85.4 → 100.0 | 75.0 → 100.0 |
| Hard    | 66.7 → 77.8 | 47.6 → 66.7 | 55.6 → 61.9  | 38.1 → 47.6  |
