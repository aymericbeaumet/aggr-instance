---
title: Devin Fusion
link: https://cognition.com/blog/devin-fusion
source: cognition-com-blog
published: 2026-06-29T18:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2026-06-29-devin-fusion.html
---

## Devin Fusion: Frontier Performance at 60% Lower Cost†

By The Cognition Team06.29.26

Engineering teams are lighting money on fire.

It's no longer sustainable to use the most expensive models on every task. But existing tools for mixing models suck. They look nice on most benchmarks but fail to write code you'd actually merge.

At Cognition, we specialize in routing across frontier models without sacrificing intelligence. Today, we're sharing our work on a new kind of multi-model harness, **Devin Fusion**, that is substantially better at mixing models while reducing costs and maintaining intelligence on real-world usage. We found it maintains **frontier and Fable 5-level performance at 60% lower cost**† on [FrontierCode](https://cognition.com/blog/frontier-code), a new state-of-the-art coding benchmark that measures both code correctness and quality.

#### Devin Fusion: Frontier Performance at up to 60%† Lower Cost

Score on FrontierCode 1.1 Extended Benchmark and average cost per task

CostScore

$10.53

Fable 5(xhigh)

64.9

$3.51

Opus 5(medium)

63.6

$1.35

Devin Fusion

63.1

$3.41

GPT-5.6 Sol(high)

58.7

$3.12

Kimi K3

58.2

$1.09

Grok 4.5(high)

56.6

Data last updated 8/7/2026 with current frontier models

† We initially reported a 35% cost reduction at publication. On the latest FrontierCode 1.1 Extended data (updated 8/7/2026), Fusion is up to 60% cheaper, as shown in the updated charts. Largest efficiency gains are seen in implementation-focused sessions.

In the rest of this post, we break down why good model routing is so hard, and the two techniques that make it all work: **the "sidekick" approach and dynamic mid-session routing**.

We welcome you to try Devin Fusion in preview at [app.devin.ai/signup](https://app.devin.ai/signup?source=fusion_blog).

## [The Trick: Sidekick](https://cognition.com/blog/devin-fusion#sidekick)

The key idea behind our architecture is to run two parallel agents: one with a frontier model, the other with a more cost-effective "sidekick" model. Both are fully capable agents with their own toolsets and ability to gather & act on their own context.

![Sidekick architecture: a frontier main agent and a small sidekick agent running in parallel, each with its own cached context](https://cognition.com/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Fsidekick-diagram.153unbtaywtzg.png&w=3840&q=75&dpl=dpl_Eh1kkjXK88zwkS8H6sv7kT1Q2Yrm)

As the task progresses, the main agent decides which tasks to give the sidekick and which tasks to do itself. Making sidekick work well in practice, however, requires deeply tuning the interaction patterns. We've found that the main agent should take minimal actions, and only read what is absolutely necessary. By default it should delegate and monitor, while making the significant decisions: the plan, the interpretation of ambiguity, the final review.

This approach fixes the primary problems with more basic model routing:

- **It retains real frontier intelligence rather than "benchmark-score" intelligence.** Routers often over-fit to specific benchmarks. By keeping a frontier model in the mix, the sidekick approach continues to benefit from frontier model creativity and general intelligence.

- **It generalizes beyond single-prompt tasks and question-answering.** Model routers often route to a single model for the entire task. Prompts often do not contain enough information about the task to properly discern difficulty. Moreover, the user might have difficult followups to simple initial prompts. Being able to move between the smart model and sidekick dynamically makes this system much more robust.

- **It avoids costly cache misses when routing between models.** We've previously explored a ["Smart Friend"](https://cognition.com/blog/multi-agents-working) tool, and Anthropic released a similar ["Advisor"](https://platform.claude.com/docs/en/agents-and-tools/tool-use/advisor-tool) tool. The core of both these ideas is to give one model a tool to query another model for helpful advice. The catch? Upon every call to the other model, the context for the task is not shared in a way that is cached, and you pay a very expensive price. In the sidekick setup, both the main model and sidekick model maintain their own persistent, cached contexts.

Of course, there are many implementation details we had to overcome to achieve the capabilities of Devin Fusion. For example, most cached inputs only have a 5-minute expiry. We encourage the reader to think about how to engineer around this. We'd love to [trade notes](mailto:walden@cognition.ai)!

### [Sidekick scales better as models get smarter](https://cognition.com/blog/devin-fusion#sidekick-scales-better-as-models-get-smarter)

Recent models, and Fable 5 especially, perform unusually well in these multi-agent setups. Fable delegates work more intelligently, requests context more efficiently, and plans more precisely, all of which yield a larger cost improvement with minimal impact on intelligence. This suggests that the sidekick pattern is one that will become more useful as base models get better.

In our testing, Fusion with Fable 5 is 41% cheaper than a pure Fable 5 harness, versus up to 60% with Opus and GPT-5.5-level models. The non-Fable numbers reflect many rounds of tuning of the Devin Fusion harness; the Fable 5 numbers don't, since access was cut off before we could apply them.‡

### [Examples of Sidekick in Action](https://cognition.com/blog/devin-fusion#examples-of-sidekick-in-action)

To better understand how the sidekick works, we inspected how using sidekick impacts cost and performance on a representative sample of FrontierCode tasks. Here we present both good and bad examples of sidekick usage.

## [Dynamic Mid-Session Routing](https://cognition.com/blog/devin-fusion#dynamic-routing)

With sidekick in your arsenal, you must still make sure to choose the right models for the task. We decide on different models for the main agent or sidekick depending on task type and complexity. It can be dangerous, however, to choose a model at the start and then realize later on that a different one would be better suited. Similarly, you might also want to move the task from the sidekick back to the main agent if it is proving too challenging. To handle these cases, we use lightweight classifiers during task execution to signal when we need to switch to the main agent or use a different model entirely.

We would like to be cache-efficient when switching between models, and doing so requires some artful engineering. We accomplish this by switching the model during context compaction, which would trigger a cache miss anyway. Each time we trigger compaction, we take it as an opportunity to evaluate the situation and switch the model that's in charge, effectively getting model switching “for free”. Note that this means we can even “upgrade” our sidekick model without going back to the main model, at no extra cache penalty.

To illustrate, we chose a task from our internal development that used Devin Fusion.

## [Results](https://cognition.com/blog/devin-fusion#results)

We benchmarked our new harness with and without the ability to use Fable 5‡, and found exciting improvements with both configurations.

Without including Fable 5, our Devin Fusion multi-model harness **delivers up to a 60% cost improvement on FrontierCode** while maintaining frontier-level performance.†

Fable 5 proved to be exceptionally performant in this multi-model harness, **achieving a 41% cost reduction**, while **maintaining the same performance as Fable 5** in a traditional agent harness. While Fable is currently not a generally available model, we are excited to extend Fable to users of Devin Fusion once access is restored.

#### FrontierCode 1.1 ExtendedScore vs Cost

Data last updated 8/7/2026 with current frontier models

[See how models rank on the FrontierCode leaderboard](https://cognition.com/frontiercode)

‡ On June 12, 2026, access to Fable 5 was suspended in accordance with a US government directive ([anthropic.com/news/fable-mythos-access](https://www.anthropic.com/news/fable-mythos-access)). As of this blogpost, access has not been restored. Results with Fable 5 are based on measurements from before this suspension and on the internal version of Devin Fusion at the time.

### [The Sanity Check](https://cognition.com/blog/devin-fusion#the-sanity-check)

We set out to build a harness that not only performs well on benchmarks, but actually feels good in real use. We enabled Fusion for a set of users internally at Cognition, and we found that **88% of their merged PRs were driven entirely by the automated Fusion router**.

Of course, our end desire is to test this harness on a much wider set of tasks than what our internal usage covers. That's why we're very excited to release a preview of Devin Fusion to users in our cloud agent.

## [The rising importance of hybrid-model harnesses](https://cognition.com/blog/devin-fusion#why-hybrid)

**The age of using one model for all of your work is coming to an end.** The rising costs of frontier intelligence are [reaching prohibitive levels](https://www.nytimes.com/2026/06/18/technology/ai-token-minimizing.html) in engineering organizations small and large. Moreover, there is now a growing range of model options at different price and intelligence levels, and with the right prompting, many of the sub-frontier models are fully capable of doing most engineering work. You wouldn't drive a Lamborghini to the grocery store, so why should you take a model that can discover zero-day vulnerabilities in software and use it to round the corner of a button?

Moreover, using a multi-model harness allows you to **capture the relative strengths of various frontier models**. For example, at Cognition, we find some models to be particularly good at UI testing, and different models to be good at identifying complicated bugs in PRs. There is also a growing set of [capable open-source models](https://devin.ai/blog/kimi-k27-glm-52-devin-desktop). This makes it easier to train specialized intelligence on specific domains. And as models emerge that excel at particular languages, tasks, or libraries, investing in multi-model capabilities only becomes more important.

We share the techniques we use here, because it will only become more important for agent builders to harness the growing diversity of intelligent models. We're just getting started, and Devin Fusion is just one of many steps on this journey.

We'd love for you to put it to work on real tasks and tell us where it breaks. Try it at [app.devin.ai/signup](https://app.devin.ai/signup?source=fusion_blog), and if you find our work interesting, consider [working with us](https://cognition.com/careers)!
