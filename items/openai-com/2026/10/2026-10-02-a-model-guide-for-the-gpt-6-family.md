---
title: A model guide for the GPT-6 family
link: https://openai.com/index/practical-guide-building-gpt-6
source: openai-com
published: 2026-10-02T16:15:00Z
updated: 2026-10-02T16:15:00Z
first_seen: 2026-10-02T20:40:20.804075271Z
labels:
- product
summary: Learn how startups can choose GPT-6 models, tune reasoning effort, improve prompts and skills, coordinate tools, and prepare workflows for production.
content: extracted
html: 2026-10-02-a-model-guide-for-the-gpt-6-family.html
---

GPT‑6 is [our most advanced suite of models yet](https://openai.com/index/introducing-gpt-6-1-sol/), and offers you a choice of models for different kinds of work.

Whether you’re turning an idea into a working prototype, building and testing a feature, or orchestrating multi-step workflows across code repositories, databases, and external APIs, this guide explains how to choose a GPT‑6 model, give it effective instructions, manage long-running work, and prepare for production.

## TL;DR

- **Run effectively in production.** Use [caching](https://developers.openai.com/api/docs/guides/prompt-caching) and [compaction](https://developers.openai.com/api/docs/guides/compaction) to manage context and cost. Measure task success and latency, and plan for monitoring and data controls.

- **Match the model to your workload.** Balance capability, cost, and latency by choosing the model, [reasoning effort](https://developers.openai.com/api/docs/guides/reasoning), and speed that fit the task.

- **Adjust your prompts and skills.** Keep prompts, skills, and repository instructions consistent about what the model should deliver, what it can do independently, and what counts as done.

- **Keep long-running work on track.** Use [steering](https://developers.openai.com/api/docs/guides/steering), [async tools](https://developers.openai.com/api/docs/guides/async-tool-calling), and [delegation](https://developers.openai.com/api/docs/guides/responses-multi-agent#overview) to handle updates and independent work. Set clear boundaries for when the model should ask for input.

## 1\. Run effectively in production

### Prepare your workflow for production

Before deploying, there are several checks and best practices you’ll want to put into place.

- Keep efficiency in mind. [Cut context the task doesn’t need](https://developers.openai.com/api/docs/guides/cost-optimization#cost-and-latency) while keeping the evidence it does. Where your application supports it, [run independent tasks together](https://developers.openai.com/api/docs/guides/latency-optimization#parallelize) so one slow step doesn’t hold up unrelated work.

- Reuse shared context through [prompt caching](https://developers.openai.com/api/docs/guides/prompt-caching) for recurring work. Cached input tokens cost up to [95% less](https://developers.openai.com/api/docs/guides/prompt-caching#why-prompt-caching-matters) than uncached input tokens, depending on the model. Put stable instructions and reference material before changing task details, and keep tool definitions consistent. The [caching dashboard](https://platform.openai.com/usage?usage_section=prompt-caching) and [diagnostics guide](https://developers.openai.com/api/docs/guides/prompt-caching/diagnostics) help you see where that reuse breaks down. Include cache writes and any long-context rates when estimating the cost of a complete workflow.

- For longer conversations, [compaction](https://developers.openai.com/api/docs/guides/compaction) reduces context size while preserving the state needed to continue.

- Decide how you’ll [monitor behavior](https://developers.openai.com/api/docs/guides/safety-checks/misalignment-monitoring) and review the [data controls](https://developers.openai.com/api/docs/guides/your-data) for your application.

- Test before deploying: Run representative tasks and measure task success, latency, and cost per successful task. Check out our [API deployment checklist](https://developers.openai.com/api/docs/guides/deployment-checklist#choose-a-model-for-the-workload).

### Match the model to the workload

Think of the model choice and reasoning level as an intelligence/price tradeoff.

- **Model:**

  - [GPT‑6 Astra](https://developers.openai.com/api/docs/models/gpt-6-astra) for the hardest reasoning work where maximum intelligence is needed.

  - [GPT‑6.1 Sol](https://developers.openai.com/api/docs/models/gpt-6.1-sol) for complex coding, research, and computer use.

  - [GPT‑6 Luna](https://developers.openai.com/api/docs/models/gpt-6-luna) for focused tasks at scale and everyday, repeated work with a clear goal, such as extracting invoice fields, classifying requests, or producing structured summaries.

When evaluating the best model for the task, [compare pricing](https://developers.openai.com/api/docs/models/compare) for each model.

- **Reasoning level:** In the API, choose how much effort the model spends on the task.

  - Low: Routine tasks, such as extracting facts or making small edits.

  - Medium: Work requiring judgment, such as planning a feature or comparing options.

  - High: Difficult debugging, deeper analysis, or careful review.

  - Extra high / Max: Test where supported when High falls short, and keep only if the improvement justifies the added time and cost.

In Codex, start with the default reasoning level for that model, then lower it for simpler tasks or increase it for deeper analysis.

- **Speed:**

  - In the API, use [Fast mode](https://developers.openai.com/api/docs/guides/fast-mode) when response time matters, such as in chat apps or coding tools. It provides faster, more consistent response times at a higher per-token cost than Standard processing.

  - In Codex and the API, use [Ultrafast](https://developers.openai.com/api/docs/guides/ultrafast-mode) when faster responses are worth the premium, such as rapid coding iterations. It speeds up token generation independently of reasoning effort. [Available for GPT‑6 Astra](https://x.com/OpenAIDevs/status/2104996045482778973).

## 2\. Adjust your prompts and skills

### Give the model a clear assignment

—Eric Provencher, Developer Experience at OpenAI

Start with a clear assignment: the result you want, who it’s for, the relevant context and constraints, and what counts as done. Then review these four areas, summarized from [*Rethinking skills and prompts for GPT‑6 Astra*](https://developers.openai.com/blog/rethinking-skills-and-prompts-for-gpt-6-astra#better-skills), for a deeper dive into updating your instructions:

- **Create better skills:** Keep descriptions short and explicit about when each skill should run, load supporting details only when needed, and replace rigid recipes with guidance suited to the models your team uses.

- **Update your AGENTS.md:** Explain when particular documents and tests are relevant, and explicitly authorize safe routine workflows, such as running local tests with disposable data and no production access.

- **Set decision boundaries:** State which actions can proceed independently and which require approval, replacing blanket “always ask” rules with clear boundaries.

- **Be prescriptive about persistence:** Define what “done” includes—implementing the change, running it, inspecting the result, and fixing failures—and identify any decisions that require your review.

### Define the output you need

Whether you’re working in Codex or building with the API, specify which decisions the model can make, when it should ask for input, and what a useful response looks like.

Give the model enough direction to keep work moving without guessing at decisions that matter. [Tell it which choices it can make and when to ask for your input](https://developers.openai.com/api/docs/guides/latest-model#initiative-and-follow-through), for example, it can choose how to organize a summary, but should check with you before changing the project’s scope. [Describe what a useful response looks like](https://developers.openai.com/api/docs/guides/latest-model#personality-and-writing-style), for example plain language, technical detail suited to your audience, and a short handoff covering what changed, what was checked, and what still needs attention.

## 3\. Optimize long-running tasks

### Keep complex work moving

With the GPT‑6 family of models, you can now take on tasks that span hours or days. Use the following features to better manage agents on long-running tasks.

#### API

In the API, use steering, asynchronous tools, and parallel work to keep long-running tasks moving.

- **Update instructions during a run:** [Mid-turn steering](https://developers.openai.com/api/docs/guides/steering#send-a-steering-message) lets you send a correction through the [Responses WebSocket API](https://developers.openai.com/api/docs/guides/websocket-mode) while the model works. Updates are queued; they don’t cancel running tools or undo completed actions.

- **Keep working while a tool runs:** [Asynchronous tool calling](https://developers.openai.com/api/docs/guides/async-tool-calling#how-async-tools-work) lets the model continue independent work while your app runs a slower task, such as tests. Your app returns the result when it’s ready. Wait for that result before starting work that depends on it.

- **Delegate independent subtasks:** GPT‑6.1 Sol supports [multi-agent workflows in the Responses API](https://developers.openai.com/api/docs/guides/responses-multi-agent#overview). It can assign independent work to subagents (such as investigating different parts of a codebase) and combine their findings into a final response. Multi-agent is currently in beta.

#### Codex

Long-running tasks can uncover decisions you wouldn’t necessarily anticipate in the initial prompt. Use clarification and steering to keep the work on course.

- **Answer questions as work progresses:** With GPT‑6 Astra, Codex can [ask for clarification while it works](https://developers.openai.com/api/docs/guides/latest-model#initiative-and-follow-through). Resolve questions that affect the next step, and specify which independent work can continue while you decide. If you’ll be away, tell Codex which tasks can continue and when it should pause for your answer.

- **Redirect work when requirements change:** [Steer the active task](https://developers.openai.com/blog/mastering-codex-remote-for-engineering#2-learn-the-difference-between-queue-and-steer) with new information, explaining what should change and what should stay the same. This helps avoid spending more time on an approach that no longer meets your needs.

### Leverage computer use to do more of the job

[**Computer use**](https://developers.openai.com/api/docs/guides/tools-computer-use) lets GPT‑6 Astra, GPT‑6.1 Sol, and GPT‑6 Luna interact directly with websites and desktop apps, even applications without an API. For example, you can ask the model to investigate a bug, fix the code, and open your product in a browser to check that the fix works.

Choose the simplest reliable way to do each step:

- Use an API or connected tool when it can do the job directly.

- Use computer use when the model needs to read a screen, click buttons, or fill in a form for you.

If you’re building computer use into your own app, give the model a tool that can run code to control a browser or desktop. [Playwright works with browsers](https://developers.openai.com/api/docs/guides/tools-computer-use#code-execution-harness-examples); PyAutoGUI works with desktop apps.

### From testing to production: How teams are building with GPT‑6 Astra

1 of 4
