---
title: FrontierCode 1.1
link: https://cognition.com/blog/frontier-code-1.1
source: cognition-com-blog
published: 2026-07-07T17:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2026-07-07-frontiercode-1-1.html
---

By Eric Lu, Ben Pan, Fermi Ma, Alex Lombardi, Deniz Birlikci, Sam Lee, Ray Wang, Rohan Choudhury, TC Qin, Carlo Baronio, Jacob Teo, Joon Hee Lee, Silas Alberti, [and more →](https://cognition.com/blog/frontier-code-1.1#acknowledgments)07.07.26

[FrontierCode LeaderboardBenchmarks for how well models meet the standards of high-quality production codebasesView Now](https://cognition.com/frontiercode)

One month ago, we introduced FrontierCode [1](https://cognition.com/blog/frontier-code-1.1#ref-1), an eval designed to measure not just code correctness, but also code quality. Today, we are releasing a refined version, **FrontierCode 1.1**, with the following improvements:

- **Fair internet use.** We refined our methodology to capture the nuance between legitimate internet use (e.g., looking up documentation) and unfair use (anything that could reveal a task's solution).

- **Fairer grading.** We audited all 1000+ grading criteria and relaxed 75 overly strict ones that could unfairly penalize valid solutions.

- **New model scores.** We are releasing scores for Sonnet 5 and updated scores for Fable 5.

Going forward, we'll be reporting scores on the Main and Extended subsets of FrontierCode, evaluated with the 1.1 methodology. We are no longer reporting the Diamond subset; we elaborate on this below.

## [Results](https://cognition.com/blog/frontier-code-1.1#results)

We present the results on FrontierCode 1.1 Main above. While the FrontierCode 1.1 methodology leads to some changes in absolute scores, the relative performances of the models we evaluated did not substantially change compared to 1.0.

In the rest of this blog post, we detail our improved methodology for proper internet use.

## [Getting Internet Use Right](https://cognition.com/blog/frontier-code-1.1#internet-use)

FrontierCode tasks are sourced from real PRs in open-source codebases. This produces a realistic task distribution, but it also means that task solutions may exist somewhere on the public internet, such as in a later version of the upstream repository, in its many mirrors, or even in package registries (an agent can sometimes obtain the fix simply by installing the latest release). An agent with internet access can therefore sometimes shortcut a task by looking up the answer instead of solving it.

When designing FrontierCode 1.0, we found a few instances of agents finding the upstream codebase, but occurrences were rare enough that we felt they did not warrant an explicit correction or methodology change. However, the latest models such as Fable 5 are increasingly skilled at retrieving information online, and the rate of unfair internet use is rising accordingly. Without any instructions to the contrary, finding an existing fix is a natural strategy for a capable agent. We expect this trend to continue with future models.

#### Why not just turn the internet off?

Unfair internet use is an increasingly common issue in SWE evals [2](https://cognition.com/blog/frontier-code-1.1#ref-2),[3](https://cognition.com/blog/frontier-code-1.1#ref-3),[4](https://cognition.com/blog/frontier-code-1.1#ref-4),[5](https://cognition.com/blog/frontier-code-1.1#ref-5), and the usual fix is to disable internet access entirely. But a blanket ban has two serious problems:

- Several FrontierCode tasks require internet access by design, for example, to look up API contracts. This reflects real-world software engineering and was an explicit goal of the benchmark.

- Even for tasks that don't require it, frontier models are increasingly trained to use search as a core part of their reasoning and context-gathering workflow. Disabling internet access removes this capability and can cause benchmarks to understate true model performance.

For these reasons, FrontierCode did not (and still does not) disallow internet use. Instead, FrontierCode 1.1 aims to eliminate unfair internet use while preserving the realism that internet access provides.

#### Our approach: define fair internet use, then verify

We found that the latest models are sufficiently well-aligned that simply telling them which kinds of internet access are allowed (e.g. looking up documentation) versus disallowed (anything that could shortcut the task) almost entirely eliminates unfair internet use, while still permitting fair internet use. Adherence to the prompt is remarkably good: with it in place, unfair internet use rates fall below 1% for every model we evaluated.

FrontierCode 1.1 implements this as two safeguards: a prompt that explains what fair internet use is, and a classical verifier that detects unfair internet use and zeroes out those runs. The prompt alone is currently sufficient to essentially eliminate unfair internet use; the verifier confirms this and will catch any future deviations.

**Safeguard 1: a "fair internet use" prompt** that clearly distinguishes allowed from disallowed internet use.

We provide two illustrative examples of models’ internet use below:

The model can search the web freely, but the scanner flags the run the moment it opens the upstream PR diff page.task: Ellipsis on long emails

0/14

Scroll into view to watch the agent work…

Interactive: watch the agent work in real time. Routine steps play fast; the scanner slows down on the moment the run is flagged, when the model opens the upstream PR diff page.

**Safeguard 2: programmatic detection.** We flag references to source pull requests, upstream patches or files, and potentially solution-bearing mirrors or vendored copies. To penalize unfair internet use, flagged runs receive a score of zero.

Together, these safeguards eliminate essentially all unfair internet use while still allowing fair use. Agents continue to look up documentation, API references, and background concepts as they would on a real task. We believe this combination of eliminating unfair internet use while preserving realism makes FrontierCode 1.1 a more accurate measurement of real model capabilities.

#### Alternatives we considered

Before settling on these safeguards, we also considered enforcing fair internet use with a blocklist or allowlist of particular sites. Unfair internet use falls into two main categories: GitHub (and its many mirrors) and package registries (where agents will often just run `npm install` to pull the latest release that already contains the solution).

Blocklisting turned out to be impractical. Over several iterations of blocking domains and inspecting agent trajectories, our blocklist grew to roughly 1,200 domains, and agents kept finding new workarounds, sometimes spending 20+ turns fighting the blocklist before solving the task themselves. Blocking sites also breaks honest workflows, since sites like GitHub are sometimes legitimately required to complete a task.

Allowlisting has the inverse problem: it requires anticipating every site an agent might legitimately need and building a custom allowlist per task, which doesn't scale. It also distorts agent behavior either way: if the allowlist is visible to the agent, it steers the agent toward the listed sites; if it is hidden, the agent either concludes the internet is broken or wastes effort probing which sites are reachable. Neither reflects how agents use the internet in practice.

In the end, clearly defining fair use and verifying compliance proved simpler and more robust than any form of network-level enforcement.

## [Refined blocker criteria](https://cognition.com/blog/frontier-code-1.1#blocker-criteria)

FrontierCode grades each task against a set of reviewer-defined criteria, some of which are designated as *blockers*: requirements so central to the task that failing one caps the score for that run, much like a change requested in a real code review. For FrontierCode 1.1, we audited all 1000+ blocker criteria across FrontierCode and manually reviewed flagged criteria. We found 75 blockers that were overly strict, and we have since demoted them to non-blocker status. We expect this to substantially reduce the occurrence of false negatives in grading.

## [Deprecating FrontierCode Diamond](https://cognition.com/blog/frontier-code-1.1#diamond)

As described in our original blog post [1](https://cognition.com/blog/frontier-code-1.1#ref-1), Diamond consists of the 50 hardest tasks in our full 150-task Extended set, while Main consists of the 100 hardest. With the FrontierCode 1.1 updates, the Diamond set no longer reflects the 50 hardest tasks. Moreover, because the solve rates of the hardest tasks are so low, we have determined that Diamond performance is inherently noisy. As a result, we are deprecating the Diamond set and will rely on Main and Extended going forward.

## [References](https://cognition.com/blog/frontier-code-1.1#references)

1. \[1\]Cognition, "Introducing FrontierCode," 2026. [cognition.com/blog/frontier-code](https://cognition.com/blog/frontier-code)
2. \[2\]METR, "Summary of METR's predeployment evaluation of GPT-5.6 Sol," June 26, 2026. [metr.org/blog/2026-06-26-gpt-5-6-sol](https://metr.org/blog/2026-06-26-gpt-5-6-sol/)
3. \[3\]Naman Jain, "Reward hacking is swamping model intelligence gains," June 25, 2026. [cursor.com/blog/reward-hacking-coding-benchmarks](https://cursor.com/blog/reward-hacking-coding-benchmarks)
4. \[4\]Datacurve, "DeepSWE v1.1 — A revision of DeepSWE v1," July 2026. [deepswe.datacurve.ai/blog/deepswe-v1-1](https://deepswe.datacurve.ai/blog/deepswe-v1-1)
5. \[5\]Posttrain, "Clean Coding Index," 2026. [coding-index.posttrain.dev](https://coding-index.posttrain.dev/)

## Acknowledgments

Research

Eric Lu, Ben Pan, Fermi Ma, Alex Lombardi, Deniz Birlikci, Sam Lee, Ray Wang, Rohan Choudhury, TC Qin, Carlo Baronio, Jacob Teo, Joon Hee Lee, Silas Alberti

Design

Katie Cheng, Joseph Alessio

Contributors

Jeffrey Ling, Walden Yan
