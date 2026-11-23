---
title: How Jump Trading is scaling quant research with ChatGPT
link: https://openai.com/index/jump-trading
source: openai-com
published: 2026-10-06T12:00:00Z
updated: 2026-10-06T12:00:00Z
first_seen: 2026-10-06T23:45:23.660353536Z
summary: Jump Trading uses OpenAI to expand quantitative research. See how longer-running AI workflows combine multiple data sources with human review.
content: extracted
html: 2026-10-06-how-jump-trading-is-scaling-quant-research-with-chatgpt.html
---

As a quantitative trading firm, Jump Trading creates predictive models that use market data, news and events, and a range of alternative data sources to make the best possible predictions about asset prices. Because markets are complex, noisy, and changing over time, it is rarely possible to anticipate exactly what will happen. But according to Lucas Baker, Head of LLM R&D at Jump, predicting even slightly better than a coin flip at scale is enough to result in a successful strategy.

Baker leads agentic research and development, and he’s focused on building the agents, harnesses, and infrastructure that let quantitative researchers explore their ideas in greater breadth and depth. Adding GPT‑6 Astra has dramatically expanded the scale and complexity of workflows that can be handed off to agents, from day-to-day coding to advanced quantitative studies to validate new hypotheses.

> “With the GPT-6 series, especially GPT-6 Astra, OpenAI has unlocked a new tier of autonomy for long-horizon tasks that require flexible agent coordination and extreme persistence on complex workflows. Where we used to require frequent human guidance and intervention, we can now focus fully on defining a secure and well-monitored environment with clear goals and letting the agents find their own way.”

—Lucas Baker, Head of LLM R&D, Jump Trading

## From short snippets of code to comprehensive analysis

Over the past year, AI has transformed from a helpful tool, useful for writing one-off code snippets or finding small bugs into a capable, versatile system that can develop entire codebases and services by itself. Now, Baker and his team find that AI works best when treated more like a colleague. Researchers can define a key problem, a work environment, and a way of evaluating the quality and significance of results, then steer one or many agents in real time about where to focus the analysis or which job to run next.

Baker says that with GPT‑6 Astra, agents are now capable of not only finding meaningful and practical changes, but merging and stacking those wins together in a process of recursive improvement. Over the course of a single long-running task, the system can analyze its findings, judge them against the agreed-upon criteria from an initial proposal, and actively redirect its efforts rather than needing a person to analyze each round of changes. As he puts it, “You can define something that needs to run for days—it needs to pull from many data sources, it needs to make those subtle calls about what is important and what is not, and it needs to interrelate everything—to create a comprehensive analysis that actually works now,” he says.

## Keeping human judgment at the center

Jump Trading works across every time horizon and asset class. Every step of the process is complex, and in a heavily regulated industry such as finance, mistakes can have both financial and compliance consequences. Baker says that it is critical to be aware of the risks that arise from entrusting work to AI, but also that agentic intelligence can also be applied to improving quality, security, and monitoring, not just adding features. Strong system design and boundaries, clear constraints, infrastructure that promotes steerability and observability, and human review of changes help Jump Trading’s team ensure that AI-enabled workflows are ready to scale in a regulated environment.

“If you have a safe environment where the agent or system is free to produce any output that it needs, but there is also a human review process at the end of it where critical validation takes place with human acceptance, that’s what gives us confidence,” he says. For example, if an agent produces a trading signal, it’s scoped and reviewed the same way any output would be: as a signal, usually informative but potentially wrong, and integrated with every other signal in a stringently reviewed and controlled execution environment.

## What’s next

Baker says he thinks we’re headed towards a world where “autoresearch,” or the recursive improvement of measurable systems by agent researchers, will become so ubiquitous it is considered simply part of a quantitative researcher’s ordinary workflow. Today, even GPT‑6 Astra’s longest-running work still involves regular check-ins with the person defining the task—not only what data to pull, how long to run, and what counts as important, but whether the intermediate results make sense. Advanced autoresearch would still begin with a human-defined system, including inputs, environment definition, evaluation metrics, trade-offs, and overall priorities, but would entrust the rest of the process to a loosely structured “fleet” of agents themselves coordinated by other agents. Within a well-structured research pipeline, these agents would be able to make intelligent decisions about how to explore ideas, allocate compute time, and integrate promising findings, all starting with little more than an open question.

“What does it look like when you can put all of this end to end, ask a general question that even you don’t know the answer to, and have a useful result come back?” he asks. “If you can solve a Millennium problem, you can probably also figure out some pretty interesting facts about quant finance.”

Every year has brought not only steady advancements in benchmark measurements, but also step changes in the type of work models were capable of performing. Often these developments were difficult to predict even months in advance: while it was considered impressive in 2024 for early agents to write a single file without mistakes, it became possible in 2025 to create entire codebases from scratch, and in 2026 to make progress on open research questions with many agents collaborating dynamically side by side. “What are we going to have next year?” he says. “It’s a pretty incredible prospect.”
