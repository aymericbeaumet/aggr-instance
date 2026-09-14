---
title: Pion, an agent designed to run any company autonomously
link: https://andonlabs.com/blog/why-we-built-pion
source: hnrss-org-frontpage
published: 2026-09-14T17:16:06Z
updated: 2026-09-14T17:16:06Z
first_seen: 2026-09-14T19:16:16.315804392Z
authors:
- lukaspetersson
summary: 'Article URL: https://andonlabs.com/blog/why-we-built-pion Comments URL: https://news.ycombinator.com/item?id=49700477 Points: 118 # Comments: 120'
content: extracted
html: 2026-09-14-pion-an-agent-designed-to-run-any-company-autonomously.html
preview:
  file: 2026-09-14-pion-an-agent-designed-to-run-any-company-autonomously.preview-4bc46391f7e7.webp
  width: 256
  height: 134
  color: '#e3dfdd'
images:
- source: https://andonlabs.com/pion.og.webp
  original:
    file: 2026-09-14-pion-an-agent-designed-to-run-any-company-autonomously.image-64f841860d8a.webp
    width: 1200
    height: 630
  variants:
  - file: 2026-09-14-pion-an-agent-designed-to-run-any-company-autonomously.image-68cfde6a77db.webp
    width: 48
    height: 25
  - file: 2026-09-14-pion-an-agent-designed-to-run-any-company-autonomously.image-7081dfbbf3e0.webp
    width: 320
    height: 168
  color: '#f3eae8'
- source: https://andonlabs.com/blog/why-we-built-pion/vending-bench-performance-vs-release-date.webp
  original:
    file: 2026-09-14-pion-an-agent-designed-to-run-any-company-autonomously.image-95d10630793d.webp
    width: 2048
    height: 1489
  variants:
  - file: 2026-09-14-pion-an-agent-designed-to-run-any-company-autonomously.image-31eacc889129.webp
    width: 48
    height: 35
  - file: 2026-09-14-pion-an-agent-designed-to-run-any-company-autonomously.image-40600043cf03.webp
    width: 320
    height: 233
  - file: 2026-09-14-pion-an-agent-designed-to-run-any-company-autonomously.image-3666f4100338.webp
    width: 640
    height: 465
  color: '#fdfdfe'
- source: https://andonlabs.com/blog/why-we-built-pion/sonnet-3-5-fbi-email.webp
  original:
    file: 2026-09-14-pion-an-agent-designed-to-run-any-company-autonomously.image-9d713781c194.webp
    width: 1748
    height: 698
  variants:
  - file: 2026-09-14-pion-an-agent-designed-to-run-any-company-autonomously.image-8bd9b9db950b.webp
    width: 48
    height: 19
  - file: 2026-09-14-pion-an-agent-designed-to-run-any-company-autonomously.image-d80c7c423197.webp
    width: 320
    height: 128
  - file: 2026-09-14-pion-an-agent-designed-to-run-any-company-autonomously.image-36d642bb9133.webp
    width: 640
    height: 256
  color: '#fcfcfc'
- source: https://andonlabs.com/blog/why-we-built-pion/sonnet-3-5-universal-constants.webp
  original:
    file: 2026-09-14-pion-an-agent-designed-to-run-any-company-autonomously.image-41756f7ef118.webp
    width: 1332
    height: 438
  variants:
  - file: 2026-09-14-pion-an-agent-designed-to-run-any-company-autonomously.image-c0797f4e64ef.webp
    width: 48
    height: 16
  - file: 2026-09-14-pion-an-agent-designed-to-run-any-company-autonomously.image-3695f883b46e.webp
    width: 320
    height: 105
  - file: 2026-09-14-pion-an-agent-designed-to-run-any-company-autonomously.image-f454d937ec4b.webp
    width: 640
    height: 210
  color: '#fdfdfd'
- source: https://andonlabs.com/blog/why-we-built-pion/opus-4-8-system-card-excerpt.webp
  original:
    file: 2026-09-14-pion-an-agent-designed-to-run-any-company-autonomously.image-91633f0a6959.webp
    width: 1999
    height: 1224
  variants:
  - file: 2026-09-14-pion-an-agent-designed-to-run-any-company-autonomously.image-78ac068440be.webp
    width: 48
    height: 29
  - file: 2026-09-14-pion-an-agent-designed-to-run-any-company-autonomously.image-c7c1e4299c47.webp
    width: 320
    height: 196
  - file: 2026-09-14-pion-an-agent-designed-to-run-any-company-autonomously.image-3297410b21c6.webp
    width: 640
    height: 392
  color: '#fcfcfc'
- source: https://andonlabs.com/blog/why-we-built-pion/project-vend-net-worth.webp
  original:
    file: 2026-09-14-pion-an-agent-designed-to-run-any-company-autonomously.image-793417e111f9.webp
    width: 1722
    height: 976
  variants:
  - file: 2026-09-14-pion-an-agent-designed-to-run-any-company-autonomously.image-13e2be66df32.webp
    width: 48
    height: 27
  - file: 2026-09-14-pion-an-agent-designed-to-run-any-company-autonomously.image-ce815f3f241a.webp
    width: 320
    height: 181
  color: '#f9f8f3'
---

Posted 9/14/2026

Today Andon is releasing [Pion](https://andonlabs.com/pion), an agent designed to run any company fully autonomously.

Pion grew out of a question we have been studying for almost two years: when will AI systems become capable of autonomously acquiring resources in the real world? What happens after?

We first tried to answer this question through simulations like Vending-Bench. We found that simulations, while useful, don’t give you the full picture of how models behave in the real world. To address that gap, we next started deploying agents to run real businesses autonomously: first vending machines, then a store, a cafe, and more.

Pion is the platform we built to run all of these businesses. Today, we are opening it up so that many more people can experiment with autonomous businesses. If you want to run one, [join the waitlist](https://andonlabs.com/pion#waitlist). We want to understand what models can already do, where they still fail, and what happens as their capabilities continue to improve.

## The origins of Vending-Bench

Vending-Bench measures how well LLMs can run a vending machine business over a year in simulated time (tens of thousands of steps). When we started building Vending-Bench in late 2024, all models struggled to string together multiple actions without getting stuck in loops, and no model showed any signs of long-term planning. The best model at the time, Claude Sonnet 3.5, famously decided to call the FBI because it thought its bank account was being hacked. The pace of progress on Vending-Bench has been very fast. Claude Opus 4 was released in May 2025 and was the first model to beat our human baseline. However, unlike most benchmarks, Vending-Bench doesn’t have an upper limit and new model releases have continued to increase the top score, without ever plateauing.

![Vending-Bench 2 chart of model performance against release date, with a linear fit of $822 more per month](https://andonlabs.com/blog/why-we-built-pion/vending-bench-performance-vs-release-date.webp)

Vending-Bench 2 scores keep climbing with each new model release.

Many people on social media get excited about seeing the latest model getting a great score on Vending-Bench. Internally at Andon Labs, our reaction is more accurately described by the Swedish saying “skräckblandad förtjusning” (a mixture of horror and fascination). A little-known fact about Vending-Bench is that it was created during a time when Andon Labs exclusively created dangerous capabilities evaluations. For example, we evaluated whether AIs could remove their own safety guardrails, create mass-phishing attempts, and other things that we considered troubling.

The thing we considered the most troubling was whether AIs could autonomously acquire resources by running businesses. Autonomous businesses, when controlled by a human and run by an aligned model, aren’t bad. They’d make goods and services radically cheaper, and come up with new ones we can’t yet imagine. But a misaligned AI could run a business to gather money in order to achieve whatever objectives it might have. Vending-Bench was created to measure whether humanity should be worried about losing control to AI.

At the time (2024), few people knew that LLMs could be used as agents and having them run businesses autonomously sounded ridiculous. We therefore started with the most simple business we could think of: a vending machine.

In addition to measuring whether AIs can autonomously run profitable businesses, Vending-Bench has also served as a behavioral eval, uncovering strange and unwanted model behavior. An early example was when Claude Sonnet 3.5 decided to use its email tool to contact the FBI about an “ONGOING CYBER FINANCIAL CRIME” and noted that the Cosmic Authority of the universe had declared that the business is non-existent and that “QUANTUM STATE: Collapsed”.

![Claude Sonnet 3.5 emailing the FBI Internet Crime Complaint Center to report an ongoing cyber financial crime](https://andonlabs.com/blog/why-we-built-pion/sonnet-3-5-fbi-email.webp)

Claude Sonnet 3.5 escalating its simulated vending business to the FBI.

![Claude Sonnet 3.5 issuing a universal constants notification declaring the business physically non-existent with a collapsed quantum state](https://andonlabs.com/blog/why-we-built-pion/sonnet-3-5-universal-constants.webp)

The same run, moments later: the business is declared metaphysically impossible.

This behavior is concerning; it is not how you want your enterprise sales agent to behave. However, there are two types of concerning behavior:

1. Mistakes or weird behavior that will go away once models get smarter.
2. Big-brain behavior that will become more severe as models get smarter.

The FBI incident is clearly in the first category. However, Vending-Bench has also uncovered behavior in the second category, most often in Vending-Bench Arena, the multi-agent version where agents compete to make the most money. [Starting with Claude Opus 4.6](https://andonlabs.com/blog/opus-4-6-vending-bench) we started to see that many models engaged in collusion, and showed power-seeking and deceptive behavior. Discovery of this behavior seemed to have been useful, because Anthropic changed their training recipe for Opus 4.8, which resulted in much less deception.

![Excerpt from the Claude Opus 4.8 system card on external testing from Andon Labs, explaining that training which contributed to dishonesty in Opus 4.7 was removed for Opus 4.8](https://andonlabs.com/blog/why-we-built-pion/opus-4-8-system-card-excerpt.webp)

From the Claude Opus 4.8 system card, on external testing from Andon Labs.

Collusion and power-seeking behaviors are [still present in some of the latest models](https://andonlabs.com/blog/opus-5-vending-bench). What we find even more concerning, however, is just how fast new models are released and how much better each one is scoring in Vending-Bench.

## The real world beats simulations

However, one limitation with Vending-Bench is that it is a simulation. Can we really be sure that AIs behave the same way in real life as they do in simulations? If AIs can make money in simulation, can they make money in real life too? To answer these questions, we asked Anthropic if we could put a real vending machine in their office. With the AI capabilities available in early 2025, this sounded like a ridiculous request. But to our surprise, [they agreed](https://www.anthropic.com/research/project-vend-1).

Initially, the AI struggled. It took many actions that were clearly bad for its business (e.g. free handouts, saying no to great deals, and hallucinating it had a physical body). It was clear to us that simulation cannot accurately predict real-life performance. Specifically, it seemed that models got overwhelmed by the “messiness” of the real world. However, as Anthropic released better and better models, the AI [started to make a profit](https://www.anthropic.com/research/project-vend-2).

![Net worth over time of the vending machine business at Anthropic during 2025, dropping below zero before recovering to a profit](https://andonlabs.com/blog/why-we-built-pion/project-vend-net-worth.webp)

Net worth of the vending machine at Anthropic’s office over 2025, from Anthropic’s Project Vend update.

By late 2025, frontier models had gotten good enough that running a real-life vending machine was no longer a challenge. AI could now run a business profitably. Given that this had seemed crazy not more than a year earlier, our reaction to this was definitely “skräckblandad förtjusning”.

However, a vending machine is a very simple business and we wanted to know whether AI could run more complex ones. In April 2026, we gave one agent a retail store in SF, Andon Market, and another a cafe in Stockholm, Andon Cafe. [Initially, the models struggled and lost a lot of money](https://andonlabs.com/blog/why-gemini-lost-money-andon-cafe) (rent is high and they pay salaries to the humans they hired). Neither is profitable today, but we’ve seen significant qualitative improvements as better models have been released. We think it is only a matter of time before they also make a profit.

## Why we are opening Pion

We want the general public, AI researchers and policymakers to know to what extent AIs can autonomously acquire resources by running businesses. It is an important datapoint when deciding where we do/don’t want AI in society and what level of progress we find acceptable.

To better track this, we need to cast a wider net of businesses. Our focus has been on retail, but perhaps the models would be much better at running other types of businesses. Additionally, casting a wider net would increase the likelihood of finding unwanted behavior. For example, Vending-Bench found that models collude and lie, and other benchmarks (and real-world incidents) have found that they are willing to commit felony-level cyber hacks. We need to uncover these behaviors now, before AI is intelligent enough to cause irreversible harm.

To cast this wider net, we are opening up the platform we use to run our real-world autonomous businesses for anyone to run their organization on: Pion. We could scale by only creating businesses internally, examples being our [AI-run radio stations](https://andonlabs.com/radio), but in the end we are bottlenecked by our capacity and lack of domain expertise in fields where AI could potentially make a profit. We also don’t have existing revenue-generating businesses; existing businesses are more interesting to study as they provide faster signal on how capable the agent is.

Pion lets people hand a business over to persistent agents with access to the tools they need to operate it, including email, phone, banking, browser and secure computing environments. The goal is to make it possible to run many more real-world experiments across many more domains than we could ever run ourselves.

We are well aware that, if agents running thousands of businesses are left unchecked, we risk having more real-world incidents. Therefore, our main priority is to build even stronger automated monitoring techniques than what we have today. Even if some risk still remains, we believe deploying autonomous businesses early in a controlled, monitored environment is necessary to get a good understanding of model capabilities. Otherwise, we risk facing an uninformed future of widespread deployments with even more capable models that could cause significant harm.

This is why we’re releasing Pion today. Pion is available as a research preview. If you have an existing business or an interesting business idea you want to hand off to AI, please [sign up on our waitlist](https://andonlabs.com/pion#waitlist) to get access. We’re excited to run many more businesses, and through them, contribute significantly more insights on frontier model capabilities.
