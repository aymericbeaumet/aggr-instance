---
title: Grok 4.1 Fast and Agent Tools API
link: https://x.ai/news/grok-4-1-fast
source: x-ai
published: 2025-11-19T00:00:00Z
first_seen: 2026-10-19T01:26:53.998049939Z
summary: Bringing the next generation of tool-calling agents to the xAI API
content: extracted
html: 2025-11-19-grok-4-1-fast-and-agent-tools-api.html
remote_preview:
  url: https://media.x.ai/v1/website/grok-4-1-fast-api-e43bcc8c.webp
---

Hi, I'd like to upgrade my current booking to an Executive Suite.

Sure, give me moment to find your booking...

Finding your account...

Searching available rooms...

Upgrading booking to Executive Suite...

Okay, all done. You’re now booked into the Executive Suite - enjoy your stay!

Thank you!

Plan of action

1

Identify guest

2

Check availability

3

Upgrade booking

Today, we’re excited to launch two powerful new additions to the xAI API:

- **Grok 4.1 Fast**, our best tool-calling model with a 2M context window. It reasons and completes agentic tasks accurately and rapidly, excelling at complex real-world use cases such as customer support and finance.
- **The Agent Tools API**, which gives agents access to real-time X data, web search, remote code execution, and more.

Paired together, Grok 4.1 Fast and the Agent Tools API empower developers to build production-grade agents that specialize in tool calling and agentic search.

* * *

## [Trained for the real world](https://x.ai/news/grok-4-1-fast#trained-for-the-real-world)

We built Grok 4.1 Fast specifically for real-world enterprise use cases.

Through RL training in simulated environments, Grok 4.1 Fast was exposed to a wide variety of tools covering dozens of domains. This diverse training gives Grok 4.1 Fast exceptional performance on τ²-bench Telecom, a challenging benchmark that evaluates agentic tool use in real-world customer support scenarios.

## [State-of-the-art tool calling](https://x.ai/news/grok-4-1-fast#state-of-the-art-tool-calling)

As developers build increasingly capable autonomous agents that plan over long horizons and operate independently, models must deliver intelligence without compromising speed and cost.

Grok 4.1 Fast is our answer: a model that combines frontier tool-calling performance with blazing-fast inference and cost effectiveness.

\*Gemini 3 Pro's score is an estimate provided by an independent evaluator, pending official results.

A common challenge for agentic models is that performance degrades as context length increases. We trained Grok 4.1 Fast using long-horizon reinforcement learning with a strong emphasis on multi-turn scenarios, ensuring consistent performance across its full 2-million-token context window.

## [Agent Tools API](https://x.ai/news/grok-4-1-fast#agent-tools-api)

We’re also launching the Agent Tools API, a suite of powerful server-side tools that allow Grok 4.1 Fast to operate as a fully autonomous agent.

With just a few lines of code, developers can enable Grok to browse the web, search X posts, execute code, retrieve uploaded documents, and more.

python

These tools run entirely on xAI’s infrastructure, so developers no longer need to manage API keys, rate limits, sandboxes, or retrieval pipelines. Grok decides when and how to use them, often invoking multiple tools in parallel across several turns, until it has everything it needs to deliver a final answer.

## [A full-featured toolset](https://x.ai/news/grok-4-1-fast#a-full-featured-toolset)

The Agent Tools API is a versatile suite that lets you significantly extend the capabilities of our base Grok models. Key features include:

## [The best agent for deep research](https://x.ai/news/grok-4-1-fast#the-best-agent-for-deep-research)

Real-time information retrieval and deep research are core strengths of Grok 4.1 Fast. With our native integration into the X ecosystem and powerful web-browsing capabilities, search agents powered by the xAI API are state-of-the-art on challenging agentic search benchmarks.

|                                | Research-Eval Reka | FRAMES | X Browse\* |        |      |        |
| ------------------------------ | ------------------ | ------ | ---------- | ------ | ---- | ------ |
|                                |                    |        |            |        |      |        |
| Grok 4.1 Fast  Agent Tools API | 63.9               | $0.046 | 87.6       | $0.048 | 56.3 | $0.091 |
| GPT-5                          | 45.5               | $0.107 | 86.0       | $0.058 | 24.2 | $0.198 |
| Claude Sonnet 4.5              | 41.2               | $0.065 | 85.0       | $0.078 | 14.6 | $0.126 |
| Gemini 3 Pro                   | 55.9               | -      | 90.9       | -      | 26.5 | -      |

\*X Browse is an internal benchmark that evaluates an agent's multihop search and browsing capabilities on X.

Grok 4.1 Fast sets a new standard in factuality, cutting the hallucination rate in half compared to Grok 4 Fast while still delivering performance on par with Grok 4 when evaluated on FActScore.

## [Start Building](https://x.ai/news/grok-4-1-fast#start-building)

We’re releasing two variants of Grok 4.1 Fast on the API:

- `grok-4-1-fast-reasoning` for maximal intelligence
- `grok-4-1-fast-non-reasoning` for instant responses

### Input pricing

Input tokens

$0.20 / 1M tokens

Cached input tokens

$0.05 / 1M tokens

### Output pricing

Output tokens

$0.5 / 1M tokens

Tool calls

From $5 / 1000 successful invocations

We can’t wait to see what you build. Please share your creations and feedback with the community on X!
