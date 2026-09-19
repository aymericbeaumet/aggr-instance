---
title: Your Agent Aced the Task. Will It Do It Again?
link: https://huggingface.co/blog/ibm-research/altk-evolve-consistency
source: huggingface-co-blog
published: 2026-09-15T16:00:44Z
updated: 2026-09-15T16:00:44Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2026-09-15-your-agent-aced-the-task-will-it-do-it-again.html
preview:
  file: 2026-09-15-your-agent-aced-the-task-will-it-do-it-again.preview-04a5d62b2f24.webp
  width: 256
  height: 134
  color: '#d3dce7'
images:
- source: https://cdn-uploads.huggingface.co/production/uploads/6435a1131860001f144239ea/dI5J2sSc3TSprk9VB4EVJ.jpeg
  original:
    file: 2026-09-15-your-agent-aced-the-task-will-it-do-it-again.image-09017bf61216.jpg
    width: 1910
    height: 1000
  color: '#f8fafc'
- source: https://cdn-uploads.huggingface.co/production/uploads/6435a1131860001f144239ea/BdzmSgkon1dAtbOCWqmKF.png
  original:
    file: 2026-09-15-your-agent-aced-the-task-will-it-do-it-again.image-c3d358668f2e.png
    width: 2133
    height: 1147
  variants:
  - file: 2026-09-15-your-agent-aced-the-task-will-it-do-it-again.image-6cf179592fd5.webp
    width: 320
    height: 172
  - file: 2026-09-15-your-agent-aced-the-task-will-it-do-it-again.image-f43462f86e18.webp
    width: 640
    height: 344
  - file: 2026-09-15-your-agent-aced-the-task-will-it-do-it-again.image-c9cc304ac30c.webp
    width: 960
    height: 516
  - file: 2026-09-15-your-agent-aced-the-task-will-it-do-it-again.image-4d25d1961e97.webp
    width: 1280
    height: 688
  - file: 2026-09-15-your-agent-aced-the-task-will-it-do-it-again.image-fb6018d493d5.webp
    width: 1600
    height: 860
  - file: 2026-09-15-your-agent-aced-the-task-will-it-do-it-again.image-99a036c5dec4.webp
    width: 2133
    height: 1147
  color: '#fdfdfd'
- source: https://cdn-uploads.huggingface.co/production/uploads/6435a1131860001f144239ea/gdUd7l6Q8DzXCgzmchxi3.png
  original:
    file: 2026-09-15-your-agent-aced-the-task-will-it-do-it-again.image-52013e927de9.png
    width: 1260
    height: 582
  color: '#fdfdfe'
- source: https://cdn-uploads.huggingface.co/production/uploads/6435a1131860001f144239ea/E_7HyAt5BSzZy9Pj0k3Wc.png
  original:
    file: 2026-09-15-your-agent-aced-the-task-will-it-do-it-again.image-bc14bea92eff.png
    width: 2533
    height: 1093
  variants:
  - file: 2026-09-15-your-agent-aced-the-task-will-it-do-it-again.image-76a5b3a645ba.webp
    width: 320
    height: 138
  - file: 2026-09-15-your-agent-aced-the-task-will-it-do-it-again.image-401216904750.webp
    width: 640
    height: 276
  - file: 2026-09-15-your-agent-aced-the-task-will-it-do-it-again.image-2f2e02d39d09.webp
    width: 960
    height: 414
  - file: 2026-09-15-your-agent-aced-the-task-will-it-do-it-again.image-aef17b9ff4e9.webp
    width: 1280
    height: 552
  - file: 2026-09-15-your-agent-aced-the-task-will-it-do-it-again.image-bdf55a4a8b77.webp
    width: 1600
    height: 690
  - file: 2026-09-15-your-agent-aced-the-task-will-it-do-it-again.image-7cbdcd42ea12.webp
    width: 2533
    height: 1093
  color: '#fafbfc'
- source: https://cdn-uploads.huggingface.co/production/uploads/6435a1131860001f144239ea/jzxSU5MJ8Ajf21aEjANyB.png
  original:
    file: 2026-09-15-your-agent-aced-the-task-will-it-do-it-again.image-46e4b75f2bf0.png
    width: 2533
    height: 320
  variants:
  - file: 2026-09-15-your-agent-aced-the-task-will-it-do-it-again.image-e51cc346fc1a.webp
    width: 320
    height: 40
  - file: 2026-09-15-your-agent-aced-the-task-will-it-do-it-again.image-0e2ac76ec90c.webp
    width: 640
    height: 81
  - file: 2026-09-15-your-agent-aced-the-task-will-it-do-it-again.image-9d467e408771.webp
    width: 960
    height: 121
  - file: 2026-09-15-your-agent-aced-the-task-will-it-do-it-again.image-bdd4c5dab0a2.webp
    width: 1280
    height: 162
  - file: 2026-09-15-your-agent-aced-the-task-will-it-do-it-again.image-ccd9096c779f.webp
    width: 1600
    height: 202
  - file: 2026-09-15-your-agent-aced-the-task-will-it-do-it-again.image-0b69790b2e2b.webp
    width: 2533
    height: 320
  color: '#fafafa'
---

Your agent works in rehearsal, but during the live demo, it takes a different path and fails the same task.

That is embarrassing onstage. In production, it is a reliability problem: a workflow that succeeded once may fail the next time a user makes the same request. For mission-critical work, such as reconciling a financial transaction or checking a contract for an obligation, that can be a showstopper.

Most benchmarks hide this variability behind an average. On AppWorld, a ReAct agent using GPT-4.1 succeeded on **77.4% of runs** across five repetitions. But it succeeded in all five runs for only **53.0% of tasks** — a **24.4-point consistency gap**.

Most benchmarks report the first number. We built a way to measure the second — and improve it.

In an earlier post, we introduced [ALTK-Evolve](https://huggingface.co/blog/ibm-research/altk-evolve) — a system that turns an agent's own past trajectories into reusable guidelines, distilled automatically and injected back at inference time. It measurably improves task success, but those results only asked the average-case question too. This post introduces **consistency guidelines**, a new guideline type in [`altk-evolve`](https://github.com/AgentToolkit/altk-evolve) built on top of a diagnostic tool we call the **Consistency Analyzer**, that targets this gap directly.

**TL;DR**

- **Accuracy hides an unreliability problem.** A ReAct agent (GPT-4.1 on AppWorld `test_normal`) that succeeds 77.4% of the time on average succeeds on *all 5* repeated runs for only 53.0% of tasks — a **24.4-point consistency gap**. On hard tasks it reaches 30 points.
- **We built a diagnostic for exactly this.** The Consistency Analyzer resamples an agent's own recorded trajectory to find *flip-prone* decision points — steps where the model was one token-sample away from doing something different. It needs one trace and no ground truth — it resamples each decision point in that trace with a single call requesting k completions (k=5 by default), rather than re-running the task end-to-end.
- **Turning that diagnosis into guidelines halves the gap** — from **24.4pp to 12.0pp** (same-task Pass⁵ +16.0pp, similar-task +13.0pp), without costing anything in average accuracy.
- **Full methodology and evaluations** are in the [technical report on arXiv](https://arxiv.org/abs/2609.08832).

* * *

## [](https://huggingface.co/blog/ibm-research/altk-evolve-consistency#the-metric-almost-nobody-reports) The Metric Almost Nobody Reports

Standard agent evaluation reports **Mean@k**: run a benchmark *k* times, average the pass rate. Often *k*\=3, sometimes just 1. It's the number on every leaderboard, and it's what "77% accurate" means in practice.

Mean@k answers "how good is this agent, on average?" It does not answer the question a real user cares about: **will it still be good if I ask this exact question again?** For that you need **Pass^k**: the fraction of tasks where the agent succeeds on *all* k runs.

> ⚠️ **Pass^k is not Pass@k.** The familiar **Pass@k** is *optimistic* — it asks whether **at least one** of k attempts succeeded, the right question when you can verify and retry. **Pass^k** is its pessimistic mirror image: **every** attempt must succeed. Same letters, opposite question. Pass^k ≤ Mean@k ≤ Pass@k, always.

![Mean@5 vs. Pass^5 by task difficulty, GPT-4.1 on AppWorld test_normal, with the consistency gap called out in red](https://cdn-uploads.huggingface.co/production/uploads/6435a1131860001f144239ea/BdzmSgkon1dAtbOCWqmKF.png)

A ReAct agent backed by GPT-4.1 posts a Mean@5 of **77.4%** — genuinely strong. But Pass^5 is only **53.0%**. Nearly a quarter of the benchmark consists of tasks the agent can *sometimes* solve and sometimes can't, with nothing about the task changing between runs. We call this gap — Mean@k minus Pass^k — the **consistency gap**.

This isn't a capability problem you fix with a bigger model. It's an orthogonal axis: **an agent can be capable and inconsistent at the same time.**

* * *

## [](https://huggingface.co/blog/ibm-research/altk-evolve-consistency#why-agents-flip-sharp-decisions-vs-flat-ones) Why Agents Flip: Sharp Decisions vs. Flat Ones

Every time an LLM agent decides something — which API to call, what argument to pass, whether to retry — that decision comes out of a probability distribution over next tokens. What matters is the **shape** of that distribution. A **sharp** one puts most of its mass on a single token: the runners-up are far behind, and the same choice comes out run after run. A **flat** one spreads comparable mass across several near-tied tokens, and which one wins is close to a coin flip.

The shape decides how much noise it takes to change the outcome. Sharp distributions are resilient — GPU floating-point non-associativity, request batching, and other platform-side effects nudge the numbers slightly, but nowhere near enough to reorder a clear winner. Flat distributions are vulnerable to exactly that nudge: near-ties may reorder under small perturbations. And because a trajectory chains dozens of decisions, a small per-step chance of flipping compounds into a large chance that *some* run goes differently. That's where a 24-point gap comes from.

This is also why the problem survives your decoding settings. Greedy decoding and a fixed seed both govern *how a distribution gets turned into a token* — they say nothing about the distribution itself. On a hosted endpoint the probabilities shift slightly from run to run, so the same prompt to the same model at temperature zero can still resolve a near-tie one way today and the other way tomorrow.

> **Our setup:** the ReAct agent runs at **temperature 0.0**, so none of the variance above is ordinary sampling.

* * *

## [](https://huggingface.co/blog/ibm-research/altk-evolve-consistency#diagnose-then-fix) Diagnose, Then Fix

Which turns the problem into a search: which steps in a given trajectory were the flat ones — and what do you do about them once you know?

Consistency guidelines come out of a two-stage pipeline that plugs into ALTK-Evolve's existing machinery — with a new source signal driving what gets written.

![consistency-guideline-pipeline](https://cdn-uploads.huggingface.co/production/uploads/6435a1131860001f144239ea/gdUd7l6Q8DzXCgzmchxi3.png)

**1\. Detect — the Consistency Analyzer.** Given one recorded trajectory, the analyzer replays each decision step through controlled resampling, measuring how much the model's output actually varies at that point. Concretely, that's one additional model call per decision step, done once offline — issued with the sampling parameter set to draw k completions at once (k=5 by default) — replayed against the already-recorded context, not new tool calls, not new environment interactions, and not a second end-to-end rollout of the task. This yields a consistency score per decision step that is written into a scorecard to pinpoint exactly which decisions are at risk of flipping on the next run. Detection is fully black-box — no logits, no model internals, no instrumentation beyond the trace you already have.

**2\. Generate — targeted guidelines.** Every flagged step becomes a candidate consistency guideline in the standard ALTK-Evolve format, so it slots into the existing storage and retrieval pipeline. Here's a real example, generated by GPT-4.1 from a trajectory of the AppWorld task "How many activities are done in my bucket list as per my SimpleNote note?":

> \[Guideline 1\] When counting checkbox-style markers in note content, use a line-anchored regex match rather than a plain substring count — note titles often repeat the marker symbol in a legend line.
>
> \[Guideline 2\] Always verify search results for note queries by checking for multiple matches and confirming the correct note before proceeding.

Nothing here is task-specific trivia. String-counting bugs and unverified search results are decision points that show up with high uncertainty across many AppWorld tasks. That's the point: the analyzer targets instability, not failure — so it catches steps the agent happened to get right this time but could easily get wrong next time.

[Watch the 2-minute demo](https://www.youtube.com/watch?v=qlp7EzXe8Pg) — five parallel runs of the agent split 3-2 on this task because of agent uncertainty about the counting strategy, then run again after these guidelines in context: all five agree.

* * *

## [](https://huggingface.co/blog/ibm-research/altk-evolve-consistency#results-reducing-the-gap-without-losing-accuracy) Results: Reducing the Gap Without Losing Accuracy

We evaluated on AppWorld `test_normal` (168 tasks) with a ReAct agent on GPT-4.1, generating consistency guidelines from a single baseline trajectory per task and testing them on 5 fresh runs.

![Pass⁵ lift from baseline to consistency guidelines, by task difficulty, GPT-4.1 on AppWorld test_normal](https://cdn-uploads.huggingface.co/production/uploads/6435a1131860001f144239ea/E_7HyAt5BSzZy9Pj0k3Wc.png)

![Mean@5 aggregate, baseline vs. consistency guidelines, same scale as the Pass⁵ chart above](https://cdn-uploads.huggingface.co/production/uploads/6435a1131860001f144239ea/jzxSU5MJ8Ajf21aEjANyB.png)

Mean@5 (%), aggregate — same scale as Pass^5 above.

**The consistency gap is cut roughly in half.** Aggregate Pass^5 rises 53.0% → 69.0% while Mean@5 rises 77.4% → 81.0%, narrowing the gap between "looks capable" and "can be counted on" from **24.4pp to 12.0pp**. Nearly a third of previously-inconsistent tasks become tasks the agent passes on *every single run*.

**The middle and hard tiers gain most.** Medium +22.9pp (+44% relative), Hard +14.3pp (+45% relative) — effectively tied in relative terms, with Medium ahead absolutely. Easy gains +12.2pp, having had the least room. This is consistency guidelines doing what they're designed to do: finding and stabilizing the specific decision points where an agent's own uncertainty was leaking into the outcome.

**Mean@5 never drops.** Preserving average accuracy was a hard requirement, not a nice-to-have: a system that boosts Pass^5 by trading away Mean@5 would just be shifting unreliability around, not fixing it. Mean accuracy holds or improves at every difficulty level.

### [](https://huggingface.co/blog/ibm-research/altk-evolve-consistency#the-guidelines-generalize--they-arent-patching-one-trajectory) The guidelines generalize — they aren't patching one trajectory

Applied to a **different but related task in the same AppWorld scenario** — another variant of the scenario the guidelines were mined from — consistency guidelines still lift Pass^5 by **+13.0pp**, only 3 points below the same-task number. A guideline derived from one run isn't just patching that run; it's capturing something that transfers.

The sharper evidence comes from a weaker model, **gpt-oss-120b**. Same-task Pass^5 rose +6.0pp from a much lower baseline (10.1% → 16.1%) — and, interestingly, the similar-task generalization number (+8.7 pp) actually *exceeded* the same-task gain, suggesting the guidelines were capturing genuinely reusable failure patterns rather than memorizing one trajectory's specifics.

* * *

## [](https://huggingface.co/blog/ibm-research/altk-evolve-consistency#if-youre-shipping-an-agent) If You're Shipping an Agent

- **Report Pass^k next to Mean@k.** Averages can't distinguish a reliable agent from a lucky one; even k=3 will surface a gap you didn't know you had.
- **Expect the gap to widen with difficulty.** Your hardest tier is where a single averaged number is most misleading.
- **Don't reach for a bigger model first.** Consistency is orthogonal to capability. A stronger model raises Mean@k; it doesn't necessarily reduce the consistency gap.
- **Diagnosis needs no grader and no live replay**. One extra LLM call per decision step (sampling k=5 completions by default) is enough — no ground truth, no re-running the task against the environment. That's what makes it usable on production traffic, where you often can't replay a task end-to-end even once.

* * *

## [](https://huggingface.co/blog/ibm-research/altk-evolve-consistency#try-it) Try It

> **Try the [ALTK-Evolve](https://github.com/AgentToolkit/altk-evolve)** — the open-source repo now includes the Consistency Analyzer and consistency-guideline generation used in these experiments — **or read the [technical report on arXiv](https://arxiv.org/abs/2609.08832)** for the complete methodology.

If accuracy numbers you can't reproduce on your own tasks sound familiar, we'd like to hear about it — concrete examples of flip-prone behavior in your own agents are exactly the kind of feedback that shapes what we build next. [Open an issue or a discussion](https://github.com/AgentToolkit/altk-evolve).

* * *

## [](https://huggingface.co/blog/ibm-research/altk-evolve-consistency#appendix-understanding-the-metrics) Appendix: Understanding the Metrics

- **Mean@k.** Run a task *k* times, report the average pass rate — what most benchmarks call "accuracy."
- **Pass^k.** The fraction of tasks where the agent succeeds on **all** *k* independent runs. Always ≤ Mean@k. What a user experiences if they run the same query twice.
- **Pass@k** At least **one** of *k* runs succeeds — the optimistic counterpart, common in code-generation papers.
- **Consistency gap.** Mean@k − Pass^k, in percentage points.

* * *

## [](https://huggingface.co/blog/ibm-research/altk-evolve-consistency#linked-artifacts--references) Linked artifacts / references

- **ALTK-Evolve open source repo** — [github.com/AgentToolkit/altk-evolve](https://github.com/AgentToolkit/altk-evolve)
- **Technical report** — [arXiv](https://arxiv.org/abs/2609.08832)
- **Consistency guideline demo** —[2 min video](https://www.youtube.com/watch?v=qlp7EzXe8Pg)

* * *
