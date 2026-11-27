---
title: 'Catching Bugs Before They Land: JiTTesting’s Human-on-the-Loop Approach at Full Scale'
link: https://engineering.fb.com/2026/10/29/security/catching-bugs-before-they-land-jittestings-human-on-the-loop-approach-at-full-scale/
source: engineering-fb-com
published: 2026-10-29T16:00:12Z
updated: 2026-10-29T16:00:12Z
first_seen: 2026-11-27T00:01:05.630507547Z
labels:
- ai research
- ml applications
- security & privacy
summary: 'Over the last year we’ve been sharing our work leveraging large language models to catch bugs on the fly — a novel approach we call Catching Just-in-Time Tests (JiTTests). Now, we’re sharing the results and positive impact we’ve seen with Catching JiTTests deployed into production. Our paper, “Just-in-Time Catching Test Generation at Meta,” fully details [...] Read More... The post Catching Bugs Before They Land: JiTTesting’s Human-on-the-Loop Approach at Full Scale appeared first on Engineering at Meta.'
content: extracted
html: 2026-10-29-catching-bugs-before-they-land-jittesting-s-human-on-the.html
remote_preview:
  url: https://engineering.fb.com/wp-content/uploads/2026/10/Catching-JiTTests-Hero-3.png
---

- Over the last year we’ve been sharing our work leveraging large language models to catch bugs on the fly — a novel approach we call Catching Just-in-Time Tests (JiTTests).
- Now, we’re sharing the results and positive impact we’ve seen with Catching JiTTests deployed into production.
- Our paper, “[Just-in-Time Catching Test Generation at Meta](https://arxiv.org/pdf/2601.22832),” fully details our initial deployment and testing with Catching JiTTests.

Every code change improves our platform, but it also introduces new complexity. With hundreds of millions of lines of code constantly evolving at Meta, traditional testing can’t keep pace. Catching risks proactively, before code lands, enables us to identify risks earlier, respond more effectively, and deliver reliable experiences at scale.

Over the last year, we’ve shared our research into [applying large language models (LLMs) to generate software tests](https://engineering.fb.com/2025/02/05/security/revolutionizing-software-testing-llm-powered-bug-catchers-meta-ach/). More specifically, we’ve shared our work with [Just-in-Time Tests (JiTTests)](https://engineering.fb.com/2025/09/30/security/llms-are-the-key-to-mutation-testing-and-better-compliance/), a novel testing approach where we leverage LLMs to catch bugs on the fly, even ones that traditional testing might not catch.

In the five months since we first unveiled this work, we’ve deployed our Catching JiTTests approach to determine its scalability and applicability to the wider industry. Today, Catching JiTTests are no longer a research prototype; they’re part of Meta’s infrastructure, and they’re helping our engineers catch more bugs with less load and a smaller chance of false positives.

[YouTube video player](https://www.youtube.com/watch?v=Pf10kLLcFR0)

## The Advantage of Catching JiTTests

Most AI-based approaches to preventing bugs in code use LLMs and/or agentic AI to review code and detect faults based on prior experience. They work through the same review process that a human reviewer would, only faster. Catching JiTTests is a **fundamentally different** approach (Table 1): It generates executable tests that demonstrate a concrete behavioral difference introduced by the change. While it requires more computational steps, it catches classes of bugs that LLM- or agent-based code reviews might not.

![](https://engineering.fb.com/wp-content/uploads/2026/10/Catching-JiTTests-Table-1.png)\
Table 1. Key differences between traditional LLM-based code review and Catching JiTTests.

## How We Implemented Catching JiTTests at Meta

JiTTests are automatically generated and designed to fail, surfacing bugs in code changes (diffs) before they land in production. When they’re applied to a codebase as large as Meta’s, one challenge is distinguishing strong catches (i.e., true-positive failures that reveal real bugs) from weak catches (i.e., tests that may be false positives).

It boils down to a specific question: For every proposed diff, how can we automatically generate tests that find the most severe bugs before they cause severe issues, and do so with a high true-positive rate and without impeding the velocity of our engineers?

To address this, we developed two tandem workflows:

- An “intent-aware” workflow that infers the intention and risks of a code change and targets test generation toward likely failure points.
- A  “dodgy diff” workflow that treats every code change as if it were a mutant/bug and generates tests to expose its behavioral differences without reasoning about intent. It implicitly asserts that a diff is buggy (hence the term “dodgy”).

![](https://engineering.fb.com/wp-content/uploads/2026/10/JiTTesting-image-1.png)\
Figure 1. The architecture of the dodgy diff and intent-aware workflows for Catching JiTTests.

To filter false positives, we employed a traditional rule-based approach combined with an ensemble approach we call “LLM-as-Judge,” which aggregates judgements from several state-of-the-art (SOTA) LLMs, all tasked with classifying weak catches as likely true or false positives.

We also maintained human review, with an eye toward reducing review load. Our goal is to maintain the consistency of human labeling but reduce the likelihood of an engineer being brought in to review a weak catch.

## How It Works

The entire system workflow happens in five stages:

### Stage 1: Targeting and Triggering

The system runs overnight on spare capacity and targets diffs submitted the previous day that are deemed “risky” using a mechanism similar to [Diff Risk Score (DRS)](https://engineering.fb.com/2025/08/06/developer-tools/diff-risk-score-drs-ai-risk-aware-software-development-meta/) and trained on previous code changes. This helps ensure test generation is focused on diffs that are most likely to contain severe regressions.

### Stage 2: Two Diff-Aware Test-Generation Workflows

The **dodgy diff workflow** treats each diff **as if it were a mutant** and uses [mutation-guided LLM-based test generation](https://engineering.fb.com/2025/02/05/security/revolutionizing-software-testing-llm-powered-bug-catchers-meta-ach/) to produce tests that distinguish the diff’s behavior from its parent. The goal here is to maximize the pool of weak catches and with it the chances of finding a strong catch among them.

At the same time, the **intent-aware workflow** uses an LLM to **infer the diff’s intention** using the code changes, diff title, and summary. The LLM then describes the **“risks”** — the ways an attempt to implement the intent could produce errors. It then uses those risk descriptions to **construct mutants** — each representing a bug that could be introduced while implementing the inferred intent.

Tests are generated to **kill each mutant** (i.e., distinguish mutant behavior from parent).

Those tests are then run on the actual diff. Any that fail are harvested as weak catches.

### Stage 3: LLM-as-Judge Ensemble (Automated Assessment)

Once weak catches are generated, the system must determine whether each is a false positive or actually a strong catch (true positive).

The LLM-as-Judge ensemble consists of a collection of LLMs that review:

- T he weak-catch test-case code.
- T he test-execution outcome (failure code, message, execution trace).
- The code changes between parent and child (the diff).
- The inferred diff intent.

### Stage 4: Rule-Based Assessor (RubFake)

We complement LLM-as-Judge with a more traditional, rule-based approach called Rule-Based False-Positive Killing Environment (RubFake).

LLM-as-Judge has rich semantics, but its judgements are non-deterministic and challenging to verify. By contrast, RubFake provides **verified, deterministic** judgments. Working together, RubFake and LLM-as-Judge produce an overall ranking of weak-catch candidates, with those ranked sufficiently highly being worthy of further consideration.

### Stage 5: Human-on-the-Loop (Engineer Reach-Out)

Based on the ranking from LLM-as-Judge and RubFake, the system then decides whether to reach out to human engineers.

Rather than a human-in-the-loop approach, where a human engineer reviews every output, we’ve reduced friction through a human-**on** -the-loop approach, where human engineers set criteria and review outcomes at an aggregate level rather than supervising every output.

![](https://engineering.fb.com/wp-content/uploads/2026/10/Catching-JiTTests-Table-2.png)\
Table 2. Operational differences between human-in-the-loop and human-on-the-loop approaches.

### The Impact of Catching JiTTests at Meta

With the testing system in place, our goal is to get engineers to engage with its results.

As of this writing, 589 engineers at Meta (64% of our outreach) have engaged with Catching JiTTests. These engineers have reported that Catching JiTTests surfaces genuinely actionable findings, with a 60% rate of true positives.

In the first half of 2026, Catching JiTTests played a key role in catching 74 serious (SEV) privacy bugs at the diff stage, before they landed in production and lowering the overall occurrence of privacy SEVs by 20% during that timeframe.

![](https://engineering.fb.com/wp-content/uploads/2026/10/Catching-JiTTests-impact.png)\
Figure 2. The impact of Catching JiTTests at Meta in the first half of 2026.

## Why It Matters

Agentic software development is helping engineers and developers across the industry write, review, and ship code faster than ever before. But faster deployment means there’s also a need for faster testing. Every bug caught before it reaches production is one that end users never need to deal with. We’ve shown that LLMs can provide an additional safety layer capable of catching what traditional tests may miss, demonstrating that AI-generated testing can scale enough to reduce organizational risk.

Catching JiTTests is a solution that is scalable, both technologically and socially. Combining these tests with a human-on-the-loop approach helps ensure that engineers rarely need to read test code and only need to confirm whether an observed behavioral change was intended.

The approach is generalizable: Engineers define success criteria early, let AI execute the testing, monitor the outcomes at an aggregate level, and intervene at the policy level rather than per item.

## Read the Paper

Our paper, “[Just-in-Time Catching Test Generation at Meta](https://arxiv.org/pdf/2601.22832),” which was presented at FSE 2026 and featured at [Product@Scale](https://atscaleconference.com/product-2026/#agenda-item-7), details our initial deployment and testing with Catching JiTTests in Meta’s infrastructure.
