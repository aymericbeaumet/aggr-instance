---
title: Introducing Fusion in Devin Desktop & CLI
link: https://cognition.com/blog/local-fusion
source: cognition-com-blog
published: 2026-09-11T17:00:00Z
first_seen: 2026-09-11T17:43:44.917817170Z
content: extracted
html: 2026-09-11-introducing-fusion-in-devin-desktop-cli.html
preview:
  file: 2026-09-11-introducing-fusion-in-devin-desktop-cli.preview-0c9bc8dc3705.webp
  width: 256
  height: 144
  alt: Introducing Fusion in Devin Desktop & CLI
  color: '#171614'
images:
- source: https://cognition.com/images/fusion-in-devin-cli/cover.jpg
  original:
    file: 2026-09-11-introducing-fusion-in-devin-desktop-cli.image-e458138f990c.jpg
    width: 1920
    height: 1080
  variants:
  - file: 2026-09-11-introducing-fusion-in-devin-desktop-cli.image-592b503b3a6f.webp
    width: 48
    height: 27
  color: '#141312'
- source: https://cognition.com/images/fusion-in-devin-cli/fusion.gif
  original:
    file: 2026-09-11-introducing-fusion-in-devin-desktop-cli.image-cd384433340c.gif
    width: 1572
    height: 720
  color: '#090f13'
- source: https://cognition.com/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fsidekick-diagram.153unbtaywtzg.png&w=3840&q=75&dpl=dpl_EEjWLFQxrDRrVH8rUBkXkLz9AC6m
  original:
    file: 2026-09-11-introducing-fusion-in-devin-desktop-cli.image-fafc3cc03023.png
    width: 2304
    height: 1614
  variants:
  - file: 2026-09-11-introducing-fusion-in-devin-desktop-cli.image-ce95f3282e60.webp
    width: 48
    height: 34
  color: '#f6f5f4'
- source: https://cognition.com/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fsession-stats.13d-2wxig7dhh.png&w=3840&q=75&dpl=dpl_EEjWLFQxrDRrVH8rUBkXkLz9AC6m
  original:
    file: 2026-09-11-introducing-fusion-in-devin-desktop-cli.image-a040c9e3c057.png
    width: 3200
    height: 1780
  variants:
  - file: 2026-09-11-introducing-fusion-in-devin-desktop-cli.image-d8bd43756eee.webp
    width: 48
    height: 27
  color: '#1a1a1a'
---

09.11.26

Fusion is the most efficient frontier harness for Fable and Astra, up to **39% more efficient** compared to other model harnesses across major coding benchmarks. Today, we’re making it available in Devin Desktop and CLI.

#### Artificial Analysis Coding Agent Indexv1.5

When selecting Fusion, you pick two models instead of one. Pick a frontier model for planning and review (the “lead”), and a cost-effective model for execution (the “[sidekick](https://cognition.com/blog/devin-fusion)”). For best results, we recommend pairing Fable 5.1 with SWE-2.

![Selecting Fusion in Devin CLI: choosing a lead model and a sidekick model](https://cognition.com/images/fusion-in-devin-cli/fusion.gif)

You can try it for yourself by installing [Devin CLI](http://devin.ai/cli):

## Savings across benchmarks

We partnered with Artificial Analysis and Vals AI to evaluate Fusion with Fable 5.1 and GPT-6 Astra with SWE-2 as the sidekick across several coding agent benchmarks. Devin Fusion saves significant costs across the board while maintaining frontier performance.

| Benchmark                  | Fable 5.1   | Fusion (Fable 5.1 + SWE-2) | Astra       | Fusion (Astra + SWE-2) |
| -------------------------- | ----------- | -------------------------- | ----------- | ---------------------- |
| DeepSWE 1.1                | 64.3 $14.63 | 63.1 $7.88 (−46%)          | 67.6 $7.88  | 67.3 $4.69 (−40%)      |
| Terminal-Bench 4           | 57.6 $17.46 | 56.1 $13.37 (−23%)         | 55.6 $10.08 | 50.0 $6.06 (−40%)      |
| SWE-Atlas QnA              | 64.8 $7.57  | 65.9 $5.00 (−34%)          | 61.8 $5.72  | 59.4 $3.59 (−37%)      |
| Vals Code Migration        | 54.6 $70.97 | 57.3 $42.00 (−41%)         | 67.7 $44.36 | 61.3 $35.51 (−20%)     |
| FrontierCode 1.1(Extended) | 63.6 $2.68  | 63.5 $1.67 (−38%)          | 63.1 $2.62  | 63.4 $2.34 (−11%)      |

## Model routing is not enough

Not all software engineering tasks require frontier intelligence. Model routing seems like an attractive solution —human generated em dash let cheaper models take care of easy tasks, and reserve costly models for when serious reasoning and planning is required.

#### Where an agent spends its turns on FrontierCode

32%

34%

5%

17%

9%

- Plan
- Setup
- Implementation
- Debug
- Validate
- Closeout

But the initial prompt isn’t enough to know the difficulty of the task. “Fix xyz bug” could be a one-line edge case or could require rearchitecting your entire product; you can’t know until you’ve actually investigated the code. Additionally, you break prompt caches by switching models mid-task, incurring $$$ for frontier models and defeating the purpose of routing.

There are many clever ways to engineer around these problems, and we think Fusion offers a simple, elegant approach.

## The Fusion architecture

Fusion works around the common pitfalls of routing with a key idea: running two parallel agents, each with its own persistent context and tools. The **lead** agent runs with a frontier model and is in charge of the session. We pair it with a more cost-effective **sidekick** to which work is delegated.

The lead owns the plan, interpretation of ambiguity, and review. It hands the sidekick a brief of each task it delegates, with constraints and success criteria. The sidekick explores code, implements changes, runs tests, and reports back. This works because we can separate most coding tasks into well-defined phases.

![Fusion architecture: a frontier lead agent and a cost-effective sidekick agent running in parallel, exchanging briefs, results, and feedback](https://cognition.com/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fsidekick-diagram.153unbtaywtzg.png&w=3840&q=75&dpl=dpl_EEjWLFQxrDRrVH8rUBkXkLz9AC6m)

Instead of passing entire conversations between models, the lead and sidekick only exchange briefs, results, and feedback. The sidekick doesn’t need the lead’s entire history to implement a change, and the lead doesn’t need every intermediate tool result to review the work. Each agent builds its own persistent context, taking full advantage of prompt caching.

The Fusion architecture ensures that frontier intelligence is always in charge. We don’t assign tasks to a cheaper model hoping that the routing decision was correct. The lead always reviews the work, identifies problems, and can take control back when the sidekick is out of its depth.

Additionally, since the lead model is in charge of the session, the user always interfaces with frontier intelligence for the best user experience. Many smaller models are becoming more capable, but are still less polished as user-facing agents.

## More expensive models can make Fusion cheaper

![Devin CLI session stats showing Fusion savings: 39% cheaper than Claude Fable 5.1 alone, with 113k tokens on Fable 5.1 and 97k on SWE-2 out of 210k total](https://cognition.com/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fsession-stats.13d-2wxig7dhh.png&w=3840&q=75&dpl=dpl_EEjWLFQxrDRrVH8rUBkXkLz9AC6m)

We’ve spent a lot of time studying how models work together in Fusion: what and how leads delegate, how much instruction different sidekicks need, and more.

One of our key findings is that **using more expensive models can make the entire system cheaper**. This applies to both the lead and the sidekick. Price per token is only part of the equation, because frontier models are increasingly more token efficient, and also more efficient in how much back-and-forth work the lead and the sidekick create for each other. In 2026, **models (and model-harness combos) should be evaluated on price per task rather than price per token**.

We saw this when we [replaced Opus 4.8 with Fable 5 as the lead](https://cognition.com/blog/making-fable-cheaper-than-opus). Fable nominally costs twice as much per token. But with the same sidekick, Fable-led sessions cost 9% less on average, while scoring higher on FrontierCode. Fable delegated earlier and gave better briefs, while Opus micromanaged the sidekick and redid much of its work.

The same principle applies to the sidekick. Using a stronger model like SWE-2 instead of a smaller model like GPT-5.6 Luna doesn’t substantially increase overall cost:

- Stronger sidekicks tend to be more turn and token efficient. A lower price per token is less useful if the model needs more attempts to get the implementation right.
- A stronger sidekick also makes the lead cheaper. Its work needs less review and correction rounds. Every mistake a stronger sidekick avoids can save the frontier model rounds of reasoning. Those savings often offset the higher cost of the sidekick itself.

| Sidekick            | List price        | Astra (high) Fusion, FrontierCode |
| ------------------- | ----------------- | --------------------------------- |
| GPT-5.6 Luna (high) | $0.20/Mtok        | 62.0 @ $2.39                      |
| SWE-2 (medium)      | $0.75/Mtok(+275%) | 63.4 @ $2.34(−2%)                 |

Thus, the models’ cost and intelligence are coupled. What matters is how efficiently the pair completes work together.

## Tuning the harness for different model pairings

Picking a lead and a sidekick is not enough to get the best out of either model. Instructions that help one pair work efficiently can make another perform worse. We continuously tune the harness around how models actually work together, rather than hoping that any combination will work out of the box.

In practice, that means answering a few questions differently for each model pair:

- **How much detail should the lead provide?** Paired with a weaker sidekick, Fable 5.1 needs to provide more prescriptive briefs. We incur more lead tokens upfront, but avoid extra review rounds later. With SWE-2 as the sidekick, Fable can leave more implementation details for the sidekick to figure out.
- **Should the sidekick be allowed to push back?** We also tune how much room the sidekick has to challenge its instructions. With stronger sidekicks, encouraging pushback can help catch mistakes in the lead’s plan. Allowing weaker sidekicks to be opinionated ends up hurting overall performance and cost.
- **What exploration should be delegated?** We find that exploration needed for planning should not be delegated to a weaker sidekick, as it shapes the lead’s plan. A weaker sidekick may not have the ability to properly decide what information matters for a given task. On the other hand, we encourage stronger sidekicks to support initial exploration. Tuning this boundary remains an active area of our research.

## Try Fusion, on your machine

We have seen great results with Fusion on Devin Cloud over the past few months, and we are excited to bring it to your machine.

We’re continuing to make improvements to the Fusion harness and exploring more multi-agent architectures. If you find our work interesting, consider [working with us](https://cognition.com/careers)!
