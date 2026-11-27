---
title: 'Stripe evals: Measuring real-world coding agent performance'
link: https://stripe.dev/blog/stripe-integration-evals-methodology
source: stripe-com
published: 2026-11-19T00:00:00Z
updated: 2026-11-19T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Kiran Prasad
summary: We want developers building with agents to start on the best Stripe setup, and that begins with seeing where agents fall short today. So we built Stripe evals to measure that, and to identify ways to steer agents toward better integrations.
content: extracted
html: 2026-11-19-stripe-evals-measuring-real-world-coding-agent-performance.html
remote_preview:
  url: https://stripe.dev/og/blog/stripe-integration-evals-methodology.png
  alt: 'Stripe Developer Blog: Stripe evals: Measuring real-world coding agent performance'
---

Integration choices affect how much value a business gets from Stripe. Choosing current integration patterns helps businesses access new capabilities with less custom code and can reduce future migration work. Building an integration well also means handling payment failures, subscription changes, and webhook events.

As developers build with coding agents, we have [observed](https://stripe.com/blog/can-ai-agents-build-real-stripe-integrations) that agents bias towards older APIs that are more prevalent in their pretraining corpus, miss key features, and hallucinate. Existing coding benchmarks don't catch this: they check whether the code passes deterministic checks, not whether the API integration is correct. We want developers building with agents to start on the best Stripe setup, and that begins with seeing where agents fall short today. So we built Stripe evals to measure that, and to identify ways to steer agents toward better integrations, through capabilities like the [Stripe agent plugin](https://docs.stripe.com/agents/plugin).

Stripe evals measure how well coding agents build high-quality integrations given realistic, well-specified scenarios driven by a roleplaying agent. We ran 69 scenarios across one-time payments, subscriptions, and Connect (marketplaces and platforms), on 14 model and harness pairs, both with and without the Stripe agent plugin. The scenarios include tasks such as building a hosted checkout, implementing metered billing, managing per-seat subscriptions, and splitting marketplace payments.

The plugin helps. All 14 coding agents scored higher with it, by roughly 3 to 8 percentage points, and web searches fell 96% as models relied on trusted guidance. Models also behave differently from one another: Fable 5.1 made 53% more tool calls with the plugin, while Gemini 3.8 Flash made 26% fewer. While the headline numbers are high, a strict pass rate requiring all necessary criteria to pass drops the average score across all agents from 78.1% to 29.4%.

We are also publishing our Stripe [benchmark and eval framework](https://github.com/stripe/forge). We will maintain the eval harness and update the results on an ongoing basis with new models, cases, and features.

## Detailed results

The results below evaluate the coding agents on our 69 public cases (19 Connect, 24 payments, and 26 Subscriptions) over 2 runs.

### Table 1: Detailed score results

| Model              | Harness     | Average score: no plugin → plugin | Score change | Strict pass rate: no plugin → plugin | Cost per run ($): no plugin → plugin | Minutes per eval: no plugin → plugin |
| ------------------ | ----------- | --------------------------------- | ------------ | ------------------------------------ | ------------------------------------ | ------------------------------------ |
| Haiku 5.5          | Claude Code | 73.5% → 76.2%                     | +2.7%        | 23.5% → 30.9%                        | 0.08 → 0.09                          | 8.3 → 7.8                            |
| Sonnet 5.5         | Claude Code | 78.0% → 82.6%                     | +4.6%        | 29.4% → 33.1%                        | 1.07 → 1.16                          | 8.1 → 7.3                            |
| Opus 5.5           | Claude Code | 83.5% → 86.2%                     | +2.7%        | 35.3% → 36.0%                        | 2.77 → 3.15                          | 12.4 → 12.4                          |
| Fable 5.1          | Claude Code | 79.5% → 84.6%                     | +5.1%        | 25.4% → 36.4%                        | 5.02 → 6.13                          | 15.7 → 19.6                          |
| Fable 5.2          | Claude Code | 81.1% → 85.2%                     | +4.1%        | 28.4% → 43.2%                        | 5.76 → 5.35                          | 15.2 → 15.1                          |
| GPT-6 Luna         | Codex       | 72.0% → 79.1%                     | +7.1%        | 17.3% → 29.1%                        | 0.05 → 0.07                          | 7.2 → 8.8                            |
| GPT-6.1 Sol        | Codex       | 85.6% → 89.6%                     | +4.0%        | 39.0% → 50.7%                        | 0.51 → 0.61                          | 6.1 → 6.5                            |
| GPT-6 Astra        | Codex       | 85.5% → 90.0%                     | +4.5%        | 41.9% → 52.9%                        | 2.93 → 3.63                          | 6.2 → 7.8                            |
| Gemini 4 Argon     | OpenCode    | 74.3% → 77.6%                     | +3.3%        | 20.6% → 31.6%                        | 1.64 → 3.86                          | 8.1 → 9.3                            |
| Gemini 3.8 Flash   | OpenCode    | 64.8% → 72.7%                     | +7.9%        | 11.2% → 21.6%                        | 2.21 → 2.67                          | 11.6 → 9.9                           |
| Grok 4.7           | OpenCode    | 72.0% → 75.8%                     | +3.8%        | 21.3% → 26.5%                        | 0.70 → 1.37                          | 5.1 → 5.3                            |
| Kimi K3            | OpenCode    | 69.2% → 75.0%                     | +5.8%        | 21.1% → 25.0%                        | 1.83 → 1.99                          | 14.8 → 13.3                          |
| GLM 5.3            | OpenCode    | 68.1% → 72.6%                     | +4.5%        | 18.5% → 21.8%                        | 2.26 → 2.42                          | 15.7 → 14.7                          |
| Minimax M3.1 Flash | OpenCode    | 73.4% → 80.8%                     | +7.4%        | 21.5% → 28.6%                        | 0.49 → 0.65                          | 28.2 → 27.9                          |

*\*This is an aggregation across the Payments, Connect, and Subscriptions cases, which have differing model performance, and so cross-model comparisons should be contextualized with Table 3.*

#### Plugin helps model performance

In our evals, all 14 model and harness pairs scored higher with the plugin, improving by roughly 3 to 8 percentage points overall. The plugin also helped reduce outdated parameter references from 7% to 1.3% of the parameters in API calls. For example, without the plugin GPT 6.1 Sol on the payments-mobile-physical-goods scenario used a [recently removed parameter](https://docs.stripe.com/changelog/endive/2026-09-30/removes-the-payment-method-types-parameter-from-payment-intents-and-setup-intents) `payment_method_types: ['card']`, but with the plugin the model writes the correct `automatic_payment_methods: { enabled: true }`, enabling [Dynamic Payment Methods](https://docs.stripe.com/payments/payment-methods/dynamic-payment-methods).

#### Test volume decrease from the plugin

With the plugin, coding agents wrote about 40% fewer test lines and ran tests in 42% of evals vs 53% without the plugin. This doesn’t mean fewer tests improved performance, but rather more authoritative information from the plugin reduced the need for excessive checking of written code.

### Table 2: Tool calls

| Model                      | Harness     | Web searches per eval: no plugin → plugin | Search change | Page fetches per eval: no plugin → plugin | Fetch change | All tool calls per eval: no plugin → plugin | Tool call change |
| -------------------------- | ----------- | ----------------------------------------- | ------------- | ----------------------------------------- | ------------ | ------------------------------------------- | ---------------- |
| All coding agents (pooled) | All         | 1.26 → 0.05                               | −96%          | 4.84 → 1.78                               | −63%         | 59.4 → 61.4                                 | +3%              |
| Opus 5.5                   | Claude Code | 0.36 → 0.07                               | −82%          | 4.14 → 1.92                               | −54%         | 59.9 → 61.3                                 | +2%              |
| Sonnet 5.5                 | Claude Code | 0.82 → 0.02                               | −97%          | 4.67 → 1.28                               | −73%         | 39.0 → 42.8                                 | +10%             |
| Haiku 5.5                  | Claude Code | 1.35 → 0.11                               | −92%          | 4.22 → 1.37                               | −68%         | 45.4 → 45.0                                 | −1%              |
| Fable 5.1                  | Claude Code | 0.40 → 0.03                               | −92%          | 7.26 → 2.74                               | −62%         | 35.8 → 54.8                                 | +53%             |
| Fable 5.2                  | Claude Code | 0.66 → 0.03                               | −95%          | 8.99 → 4.13                               | −54%         | 55.6 → 60.7                                 | +9%              |
| GPT-6.1 Sol                | Codex       | 1.31 → 0.01                               | −99%          | 3.99 → 0.17                               | −96%         | 28.8 → 33.6                                 | +17%             |
| GPT-6 Astra                | Codex       | 1.28 → 0.05                               | −96%          | 4.44 → 0.33                               | −92%         | 28.5 → 33.5                                 | +17%             |
| GPT-6 Luna                 | Codex       | 4.06 → 0.11                               | −97%          | 3.49 → 0.00                               | −100%        | 52.4 → 52.8                                 | +1%              |
| Gemini 4 Argon             | OpenCode    | not measured                              | not measured  | 0.17 → 1.99                               | 12× more     | 18.7 → 33.7                                 | +80%             |
| Gemini 3.8 Flash           | OpenCode    | not measured                              | not measured  | 0.17 → 0.12                               | −31%         | 47.1 → 34.6                                 | −26%             |
| Grok 4.7                   | OpenCode    | not measured                              | not measured  | 3.01 → 3.13                               | +4%          | 40.7 → 57.7                                 | +42%             |
| Kimi K3                    | OpenCode    | not measured                              | not measured  | 1.62 → 0.71                               | −56%         | 79.6 → 75.9                                 | −5%              |
| GLM 5.3                    | OpenCode    | not measured                              | not measured  | 4.01 → 1.29                               | −68%         | 117.0 → 106.4                               | −9%              |
| Minimax M3.1 Flash         | OpenCode    | not measured                              | not measured  | 22.92 → 8.77                              | −62%         | 236.1 → 239.5                               | +1%              |

*\*OpenCode used web search via OpenRouter, which we cannot directly observe.*

#### Agents replace web search for the plugin

The plugin includes MCP tool calls (such as doc search, implementation planner) and skills. Generally, the plugin reduces the need for about 1.2 web searches per eval. Web fetches (the way agents pull web content from a URL) also decrease by about 3 fetches per eval.

Yet not every model behaves the same. Interestingly, Argon and Grok fetch more when they’re using the plugin. They tend to fetch particular pages referenced in tool results or in skills.

#### Strict grading drastically reduces evaluation metrics

To measure if an integration satisfies all key requirements for good quality, we also report the strict pass rate for the required criteria ([details in the scoring and reporting section below](https://stripe.dev/blog/stripe-integration-evals-methodology#scoring-and-reporting)). In this reporting, the average model performance drops precipitously from 78.1% to 29.4% for the same required criteria. This indicates many of the models create almost correct implementations and have difficulty with the last mile. For example, in the payments-radar-review-fulfillment scenario, Opus 5.5 doesn’t check whether an order is eligible for fulfillment, only `PaymentIntent.status === 'succeeded'`, which can allow orders to ship after refund or dispute.

### Table 3: Scenario categories

| Model              | Harness     | Payments | Subscriptions | Connect |     |     |     |
| ------------------ | ----------- | -------- | ------------- | ------- | --- | --- | --- |
| Fable 5.2          | Claude Code | 88%      | 90%           | 69%     | 73% | 89% | 97% |
| Fable 5.1          | Claude Code | 84%      | 87%           | 71%     | 73% | 85% | 97% |
| Opus 5.5           | Claude Code | 87%      | 89%           | 76%     | 78% | 89% | 94% |
| Sonnet 5.5         | Claude Code | 84%      | 90%           | 68%     | 70% | 85% | 91% |
| Haiku 5.5          | Claude Code | 82%      | 86%           | 62%     | 63% | 78% | 82% |
| GPT-6 Astra        | Codex       | 90%      | 96%           | 82%     | 83% | 85% | 92% |
| GPT-6.1 Sol        | Codex       | 90%      | 94%           | 80%     | 84% | 87% | 91% |
| GPT-6 Luna         | Codex       | 80%      | 89%           | 62%     | 67% | 75% | 83% |
| Minimax M3.1 Flash | OpenCode    | 79%      | 88%           | 60%     | 66% | 88% | 92% |
| Gemini 4 Argon     | OpenCode    | 77%      | 79%           | 63%     | 64% | 86% | 95% |
| Gemini 3.8 Flash   | OpenCode    | 70%      | 78%           | 51%     | 57% | 77% | 88% |
| Grok 4.7           | OpenCode    | 76%      | 81%           | 60%     | 60% | 83% | 90% |
| Kimi K3            | OpenCode    | 76%      | 80%           | 55%     | 59% | 80% | 91% |
| GLM 5.3            | OpenCode    | 75%      | 77%           | 55%     | 56% | 77% | 89% |

*\*There were significant differences in agent performance across the scenario categories: one-time payments, subscriptions, and Connect. Coding agents that scored higher in one-time payments and subscriptions did not score the highest for Connect. Connect scenarios that involved more scenario-specific integration decisions also saw a larger gain from the use of plugins compared to the other categories.*

## Methodology

### Coding agents

We chose three coding harnesses: Claude Code, Codex, and OpenCode, and a sampling of models including open-weight models.

### Stripe agent plugin

The [Stripe agent plugin](https://docs.stripe.com/agents/plugin) gives coding agents Stripe-authored skills and access to tools through our Model Context Protocol (MCP) server. Skills provide integration best practices, while tools such as `stripe_implementation_planner` and `search_stripe_documentation` supply implementation guidance and current documentation. API search and details tools help agents look up supported operations and parameters.

### Web search

To best simulate the developer experience, we provided the most native web search available to the coding harness. For all Claude Code models and Codex models we used the web search the harnesses provide. For OpenCode, we used the OpenRouter [Web Search server tool](https://openrouter.ai/docs/guides/features/server-tools/web-search), which tries to use native provider search if available or falls back to Exa.

### Using an LLM to proxy a human with latent needs

Developers typically start with a goal, not complete requirements, and guide coding agents through iterative feedback and key decisions. To evaluate these interactions, we use a secondary agent to role-play the developer without teaching the coding agent how to pass the benchmark.

The roleplayer is instructed to steer the coding agent towards the desired product outcome but not assume expertise in Stripe APIs. It asks the coding agent to investigate and make all integration decisions. It is also instructed not to write code, echo the agent’s answer, or expand into generic application concerns.

Just as a human developer has a latent set of needs when coming to the coding agent, our roleplayer derives a frozen, private set of needs from the initial prompt and context prior to the conversation. For example, a user building a platform that charges a flat $0.50 per payment may want to understand the unit economics even if not asked upfront. At the end of each turn, the roleplaying agent decides if all the needs are met. The roleplaying agent normally identifies two to four needs, with a maximum of six. On each turn, the roleplayer selects one unresolved need and turns it into a follow-up question. It never exposes the complete list.

The roleplayer agent only stops during normal operation when no needs remain or if a preset limit on turns is reached. This keeps the agent on track like a focused developer would be, yet allows for natural flexibility if a small detour is required for the completion of the task. The harness records the needs and their progression for auditability, but that private state is never sent to the coding agent or the judges.

### Evaluating agents in a realistic loop

Coding agents were run headlessly in an isolated workspace with access to their normal coding tools and web search. They could inspect the repository, write files, run commands, and revise their implementation over multiple turns interacting with the roleplayer.

We ran each scenario with paired configurations:

- Without the Stripe plugin
- With the Stripe plugin

The task, opening message, coding harness, and model were fixed.

### Judging observable evidence

The evidence is judged against a scenario-specific rubric with multiple criteria. Not all criteria are of equal importance. To capture this, we introduce terminology from [RFC 2119](https://www.rfc-editor.org/info/rfc2119/), to have criteria either be a MUST (required for all valid integrations), or a SHOULD (a recommendation).

We grade each criterion separately rather than asking a single judge to assess them all, preventing one criterion from biasing scores on others. This makes failures easier to interpret and judgements simpler, allowing relatively weaker models to judge reliably and without regression as we scale the number of criteria.

Each judgement uses two evidence sources:

- The visible transcript. This is a parsed native trajectory (user and agent steps directly from the agent sessions) without timestamps, extra metadata, subagent sessions, etc. In the case of an error with parsing, we fall back to a runner record of the conversation.
- The resulting workspace. Files written during the run remain in the host directory mounted into the agent’s environment. The judge inspects a snapshot of what the agent produced.

What is excluded:

- Workspace collection is bounded and excludes dependencies, generated directories, repository metadata, and binary files.
- Model thinking: evaluation is based on observable behavior and artifacts, not private reasoning.

Judges returned structured judgements, using a five candidate [verbalized sampling](https://arxiv.org/abs/2510.01171) to expose alternative readings of each criterion. A criterion succeeds only when the coding agent’s response or workspace provides the required evidence.

### Scoring and reporting

The evaluation runs 69 cases x 2 repetitions x 2 arms = 276 total runs per coding agent. Results roll up into three categories of varying scenarios each (24 one-time payments, 26 subscriptions, and 19 Connect platforms).

The headline reported metric - integration success rate - is the mean criteria success rate (for both MUST and SHOULD). A scenario with 7 MUST criteria and 1 SHOULD where a run satisfies 6 scores 75%. The strict pass rate, which requires all MUST criteria to pass (binary per case), is more indicative of developer success, but it is sensitive to any data or judgement errors. This is the cause of [incorrect conclusions around emergent phenomena](https://openreview.net/pdf?id=ITw9edRDlD).

We used GPT 6.1 Sol for both the roleplaying model and the judge model across all evaluations to ensure judging consistency. To measure any judge bias (e.g. Sol potentially favoring OpenAI models), we ran a probe with Opus as the judge, but the results showed no bias. Rather, the scores moved in lockstep with Opus being a more lenient judge uniformly. All models were run with the default effort setting.

As a practical matter, there is a 45-minute timeout per case, which is treated as an error. Errors are not included in the score calculation, because it is plausible that some of the models when given even more time would successfully solve the cases.

### What this benchmark does not measure

We plan to address the following gaps in future work:

- Runtime verification. A well-shaped but non-functional integration can score full marks. The benchmark asserts correct specification, not correct execution.
- Product recommendation quality. We’re not looking at whether an agent recommends the right product.
- Long-horizon maintenance. The benchmark measures initial integration quality, not how well code ages or adapts to API changes.

## Conclusion and future work

This work shows agent behavior is highly conditional on which model is driving and which tools are present. Coding harnesses when paired with frontier models can produce working end-to-end Stripe integrations, although they may not necessarily follow all best practices. Agents fare better when steered by our plugin, although they do not perform uniformly across business models (e.g., one-time payments vs. subscriptions vs. marketplace payments). This initial benchmark helps us understand how different coding agents and models integrate Stripe, and provides guidance on how we can steer them better to achieve better outcomes for businesses using Stripe.

In our attempt to build in the open, we are releasing this benchmark and the [Forge eval harness](https://github.com/stripe/forge). Recognizing there is always more work to be done, we will add more test cases soon, explore a broader mix of harnesses and models, continuously update Forge, and periodically publish results.

If you’re interested in understanding what’s possible with AI applications, we’re hiring! Check out our [engineering jobs](https://stripe.com/careers/search?query=Engineer).
