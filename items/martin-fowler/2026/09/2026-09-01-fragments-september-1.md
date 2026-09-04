---
title: 'Fragments: September 1'
link: https://martinfowler.com/fragments/2026-09-01.html
source: martin-fowler
published: 2026-09-01T19:50:00Z
updated: 2026-09-01T19:50:00Z
first_seen: 2026-09-04T09:21:49.209340827Z
authors:
- Martin Fowler
content: extracted
html: 2026-09-01-fragments-september-1.html
---

Like many readers, I’m wary of AI generated prose. Simon Wilison has written an [LLM cliché highlighter](https://tools.simonwillison.net/llm-cliche-highlighter#https://martinfowler.com/articles/making-data-ready-for-agentic-ai.html) - paste in some text, or a URL, and it will flag various patterns common to LLMs. It references a [wikipedia page of signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing). That page points out that:

> Humans are notoriously bad at distinguishing human and LLM-generated text. While research on humans’ abilities to detect AI-generated text is still limited, a 2025 study has shown that human ability to distinguish LLM text from human is no better than random chance. Another 2025 study on German theses has shown that humans managed a “recognition rate of 57% for AI texts and 64% for human-generated texts”.\[

Not just do I find myself repelled by prose with an LLM-voice, I also wonder how accurate my reaction is. I’m old enough to see all sorts of new tic-phrases appear, and in the past would just chalk it up to youngsters or airport business books. (Not to mention Americanisms, which I’ll get used to momentarily.)

 ❄                ❄                ❄                ❄                ❄

NVIDIA’s technical blog reports on an [Architecture for Long-Horizon Autonomous Agents](https://developer.nvidia.com/blog/nvidia-avo-reaches-100-on-arc-agi-3-demonstrating-a-frontier-level-general-purpose-architecture-for-long-horizon-autonomous-agents/). Their research group used a combination of Claude Opus 5 and a harness called AVO, and used it first to do GPU kernel optimization and then a broader reasoning benchmark (ARC-AGI-3). Both of these were long-term tasks, for the kernel optimization the agent ran for seven days.

> AVO is designed to preserve progress beyond a single model context. Two mechanisms are particularly important: persistent memory and supervision.
>
> Persistent memory carries forward prior implementations, evaluation results, compiler and profiler outputs, and accumulated reasoning, allowing the agent to resume from the current state rather than repeatedly reconstructing the search.
>
> The supervisor monitors the broader trajectory for stagnation or repeated unproductive cycles and can redirect the main agent toward alternative strategies when needed. During the seven-day attention-kernel run, the main agent remained responsible for deciding what to inspect, change, test, and evaluate, while the supervisor helped maintain forward progress when the search plateaued.

The team was encouraged that AVO did well at two different kinds of long-horizon tasks, indicating that it’s a general-purpose tool.

 ❄                ❄                ❄                ❄                ❄

[Mickey Petersen](https://x.com/mickeynp/status/2092525399209058394):

> MCP is SOAP for Zoomers.

 ❄                ❄                ❄                ❄                ❄

Paul Stack writes that [AI Broke the Assumptions Behind CI](https://stack72.dev/ai-broke-the-assumptions-behind-ci/). Here’s his description of CI with agents.

> An agent writes a change, opens a PR, and CI picks it up instantly. The compile fails, the agent pushes a fix, CI picks it up instantly again. A test fails, another fix, another instant run. Each iteration is fast, but the agent is still discovering that its change doesn’t work only after it crosses the PR boundary. The feedback loop is in the wrong place regardless of how fast CI runs.

He points out that all of this breaks the pipeline, because “CI” keeps failing, and advocates doing verification before the agent pushes. This is where I get to be the grumpy old guy, and point out that was [always how Continuous Integration works](https://martinfowler.com/articles/continuousIntegration.html#BuildingAFeatureWithContinuousIntegration). When I’m done with a change, first I pull (to get everyone else’s change since I started), I build and test locally, and if all is well I push and let the CI server do its thing. The only reason the CI server should fail is if there’s some funky mismatch between my machine and the CI server. Tests that take a while to run aren’t part of this loop, instead they are run further down the [deployment pipeline](https://martinfowler.com/bliki/DeploymentPipeline.html), downstream of CI. Any failures there imply missing tests in CI.

(I’m being a bit unfair dumping on this article here. After all I could have filled a full working day correcting misleading descriptions of Continuous Integration for most of the last twenty years. Maybe I’m just after an excuse to point readers to the [extensive range of articles](https://martinfowler.com/delivery.html) hosted here about what’s needed to get code from laptop to production.)

Stack is right that we should question how the deployment pipelines should work with agents in play. He’s also right that CI with humans relies on them being disciplined to run commit tests locally before pushing to the CI server - and that we can (and should) automate that when using agents. I also don’t know more about his setup than what he’s written in his post, so there’s likely complications he faces that I don’t understand. But when thinking about designing pipelines it’s important to understand the principles that underlie [Continuous Delivery](https://martinfowler.com/bliki/ContinuousDelivery.html), understand how the practices really work, and understand why they are in place. Above all, Continuous Integration is a practice, not just the CI server. Yes, CI does conflate two jobs: executing verification and coordinating merges. But that’s the point: verification is a necessary part of merging if we want to retain a healthy [mainline](https://martinfowler.com/articles/branching-patterns.html#mainline).

 ❄                ❄                ❄                ❄                ❄

Recently Noah Smith posted an article about how he was worried about an [AI-generated super-virus savaging humanity](https://www.noahpinion.blog/p/heres-how-were-all-going-to-die). It’s a worry I’ve heard a few times, seen as a greater concern than AI turning us into labradors or paper-clips. Claus Wilke, who works in the field, [isn’t so concerned](https://blog.genesmindsmachines.com/p/im-sorry-youre-not-going-to-die-from).

> Computational design of biological systems is unfathomably difficult. Experts who have dedicated their life to this topic routinely hit their head against the wall when nothing they try seems to work. PhD students in 2026 using state-of-the-art AI software are spending months or years trying to design simple peptide binders that inhibit some enzyme or pull down some protein, and the majority of their designs fail, or don’t express, or are toxic. But in Smith’s fictitious world a disgruntled teenager with no special training in biology can just solve a problem thousands of times more complicated than designing a peptide binder. The distance between where we are today and where we would have to be for Smith’s story to have any realism is enormous.

 ❄                ❄                ❄                ❄                ❄

It seems that a couple of remarkably talented academics are experts in a staggeringly wide range of fields. [Or maybe they are just ghosts.](https://arxiv.org/pdf/2606.02184)

> These names do not exist. Elena Vasquez and Marcus Chen have appeared as volcano experts, astronauts, thriller protagonists, podcast hosts, and academic co-authors across hundreds of independently produced AI-generated documents, never having lived. We show that large language models do not merely default to high-probability individual names when generating fictional experts: they produce correlated character ensembles: pairs and trios whose co-occurrence rates far exceed chance and are consistent across independent generations.
