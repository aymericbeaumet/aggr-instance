---
title: Introducing Claude Haiku 5.5
link: https://www.anthropic.com/claude-haiku-5-5
source: anthropic-com-news
published: 2026-10-06T00:00:00Z
first_seen: 2026-10-18T23:27:08.867657739Z
summary: We’re launching a new, expanded version of our Cyber Verification Program, which makes advanced cyber capabilities and reduced blocking classifiers available to qualifying security professionals.
content: extracted
html: 2026-10-06-introducing-claude-haiku-5-5.html
remote_preview:
  url: https://www-cdn.anthropic.com/images/4zrzovbb/website/d3dd09ad16c68461dc3fb01df5e84cf7ccafda6c-1000x1000.svg
  alt: Expanding the Cyber Verification Program
---

Introducing Claude Haiku 5.5: the cheapest, fastest, and most capable small model we’ve ever released.

Claude Haiku 5.5 is designed for high-volume, cost-sensitive tasks. It reliably handles quick and repetitive workloads (like summaries, compactions, database queries, and classification requests). It pairs well with Opus 5.5 and Sonnet 5.5 as a subagent on coding work. And, since it’s also our fastest model to date, it works especially well for speed-sensitive tasks like live customer support and browser use.¹

Haiku 5.5 is available at a much lower price than Haiku 4.5. On average, it now costs around 75% less to run.²

Along with this launch, we’re making improvements to the value of our model range. We’re halving the price of Claude Sonnet 5.5’s cache reads, which means Sonnet 5.5 now runs around 20% cheaper on most agentic work. And we’re introducing a new monthly API credit for our Claude Max and Team subscribers, designed to support our users in building new agents and applications that run on the Claude Platform.

## Performance

Here’s how Claude Haiku 5.5 performs across a range of benchmarks:

| Haiku 5.5            | Haiku 4.5            | GPT-6 Luna           | Sonnet 5.5For reference |
| -------------------- | -------------------- | -------------------- | ----------------------- |
| 1620                 | 735                  | 1437                 | 1840                    |
| 1578                 | 614                  | 1336                 | 1824                    |
| 72.4% Offline subset | 15.7% Offline subset | 48.9% Offline subset | 83.9% Offline subset    |
| 45.9% no tools       | 10.2% no tools       | —                    | 56.9% no tools          |
| 57.4% with tools     | 18.7% with tools     | —                    | 64.5% with tools        |
| 39.2%                | 0.0%                 | 16.4%                | 70.6%                   |
| 46.4%                | —                    | 42.4%                | 52.1% Xhigh             |
| 46.4% no tools       | 6.4% no tools        | 29.1% no tools       | 61.6% no tools          |

For details on how we run our evaluations, see the [Haiku 5.5 System Card](https://www.anthropic.com/claude-haiku-5-5-system-card).

Haiku 5.5 is our first Haiku-class model to come with an adjustable effort setting. This means that, as with our other models, users can decide whether to optimize for cost or intelligence. The charts below show how Haiku 5.5 performs on three benchmarks at each effort setting:

OSWorld 2.1 (offline subset) Accuracy vs. cost

OSWorld 2.1 measures how well agents can operate a real computer to finish long, multi-step tasks.

In early testing, our customers reported results consistent with the performance and cost improvements shown above. Here’s what they told us about the new model:

Quote

> “We’re very impressed with Claude Haiku 5.5, particularly its speed. We ran it through our eval suite for AI Teammates, our AI agent product, covering use cases like triaging bugs, setting up projects, and searching large portfolios to surface high-risk or overdue work. Compared with the model we use today, we saw over a 30% reduction in latency for task completions and up to 2.5x faster inference per agent turn. It’s a noticeably snappier experience.”

Company Asana

Author Aaron Vinh, Staff Software Engineer

## Pricing

The table below shows how Claude Haiku 5.5’s pricing compares to our other models. Haiku 5.5 is especially good value when used for tasks with prompts up to 100,000 tokens, which make up around 90% of requests to our previous Haiku model.

| Price per 1 million tokens | **Haiku 5.5** prompts up to / over 100k | **Haiku 4.5** | **Sonnet 5.5** |
| -------------------------- | --------------------------------------- | ------------- | -------------- |
| Cache reads                | $0.01 / $0.05                           | $0.10         | $0.10          |
| Cache writes               | $0.125 / $0.625                         | $1.25         | $2.50          |
| Input tokens               | $0.10 / $0.50                           | $1.00         | $2.00          |
| Output tokens              | $0.50 / $2.50                           | $5.00         | $10.00         |

## Safety

**Alignment.** Claude Haiku 5.5 shows major improvements across almost all of our alignment evaluations relative to Haiku 4.5. In particular, we found far fewer instances of misaligned behavior, and a lower willingness to cooperate with misuse. The model’s [system card](https://www.anthropic.com/claude-haiku-5-5-system-card) describes our evaluation process and results in more detail.

**Safeguards.** Consistent with its capabilities, Haiku 5.5’s cybersecurity safeguards are more restrictive than Haiku 4.5’s, but somewhat less restrictive than those we’ve applied to other recent models. In cybersecurity, they permit a wider range of defensive tasks than our safeguards for Sonnet 5.5, but they still block penetration testing and other techniques more likely to be used by attackers.

Haiku 5.5’s biology safeguards are the same as for Sonnet 5, Sonnet 5.5, and Opus 5. They allow research biology questions but restrict access to requests that we judge as likely to cause harm. Organizations working on wider-ranging biology and cyber activities can apply to our [Life Sciences Verification Program](https://www.anthropic.com/news/life-sciences-verification-program) and [Cyber Verification Program](https://www.anthropic.com/news/cyber-verification-program).

## Availability

Claude Haiku 5.5 is available now on all platforms, including Amazon Web Services, Google Cloud, and Microsoft Azure. On the Claude Platform, developers can get started with `claude-haiku-5-5`.

See our [migration guide](https://platform.claude.com/docs/en/models/haiku-5-5/migration-guide) for details.

## Further updates

Alongside our new pricing for Claude Haiku 5.5, we’re making further improvements to the value of our models and products.

First, starting today, we’re **lowering the price of cache reads on Claude Sonnet 5.5**. Cache reads now cost 50% less: $0.10 per million tokens rather than $0.20. Because cache reads make up a large share of models’ token consumption, this reduces the cost of Sonnet 5.5 on most agentic tasks by around 20%.

For instance, here’s what the price cut means for Sonnet 5.5’s performance relative to cost on Terminal-Bench 4.0:

Terminal-Bench 4.0 Accuracy vs. cost

Terminal-Bench 4.0 measures how well a model can complete complex, multi-step professional tasks within a command-line interface.

This chart illustrates an important difference between Haiku 5.5 and our larger models. Sonnet 5.5 and Opus 5.5 remain better choices for complex agentic coding tasks like those measured by Terminal-Bench 4.0. By contrast, Haiku 5.5 is best suited to more narrowly scoped tasks that might otherwise have been cost-prohibitive with previous versions of Claude—like compaction, summarization, or subagent work.

Second, this week, we’ll roll out **a new monthly API credit to all Max and Team subscribers for use on the Claude Platform**. Max 5x users will get $100 in credits per month, Max 20x users will get $200, and Team subscribers will receive up to $500, pooled across their users. These credits are designed to allow our users to experiment with building tools, apps, and agents that call our API. They can be used on any of our models. For more information, [see our Help Center article](https://support.claude.com/en/articles/17154008).

For developers, we’re also **updating our Claude Python and TypeScript SDKs to add support for computer use and browser use** in beta. Haiku 5.5 is especially well-suited to these tasks, given its combination of speed, capability, and price. You can read more about this [in our Claude Platform docs](https://platform.claude.com/docs/en/agents-and-tools/tool-use/browser-use-sdk).
