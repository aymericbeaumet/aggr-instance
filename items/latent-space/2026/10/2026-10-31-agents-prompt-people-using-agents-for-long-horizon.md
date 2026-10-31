---
title: 'Agents Prompt People: Using Agents for Long-Horizon Financial Work'
link: https://latent.space/p/arca/
source: latent-space
published: 2026-10-31T16:05:00Z
updated: 2026-10-31T16:05:00Z
first_seen: 2026-10-31T19:30:57.182345052Z
authors:
- Richard MacManus
summary: Arca's head of AI discusses persistent memory, the benefits of a custom harness, and agentic workflows that span days.
content: extracted
html: 2026-10-31-agents-prompt-people-using-agents-for-long-horizon.html
remote_preview:
  url: https://i.ytimg.com/vi/KM3VQZKjpiY/maxresdefault.jpg
  alt: 'Agents Prompt People: Using Agents for Long-Horizon Financial Work'
extra:
  audio_type: audio/mpeg
  audio_url: https://podcasts.beehiiv.com/3967c1f2-cb97-4eb0-a5c0-21a1d2579408/01a15fdf-0e93-7e1a-880d-d73752a1f915/01a18ea6-9508-712c-b5ff-54536059aced/01a18eb9-7b8e-7f04-b56a-8580cdb8c207.mp3
  duration_seconds: 1565
  thumbnail: https://i.ytimg.com/vi/KM3VQZKjpiY/maxresdefault.jpg
---

Arca's head of AI discusses persistent memory, the benefits of a custom harness, and agentic workflows that span days.

At [AI Engineer New York](https://ai.engineer/nyc/2026/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) this month, we sat down with [Aman Singh](https://www.linkedin.com/in/amanps/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), the lead AI Engineer at [Arca](https://arcawealth.ai/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), an “AI-native” company providing wealth management services.

We discovered that while **agents do most of the background work** in the firm, there are **guardrails** in place to ensure that human advisors are making the final decisions. The goal is for AI to guide the process; or as Singh nicely put it, **“agents prompt people more than people prompt agents.”**

Arca recently raised $48.5 Million in a round led by General Catalyst, with participation from Index Ventures and Venrock.

One of the features of using AI in financial services, explained Singh, is that it’s typically **long-horizon work** — although not long-running. In his AIE presentation, Singh noted that **agent tasks at Arca can take up to ten days.** “It's very stop-start, there's many actors in between, many decision points, many handoffs,” he said.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/f556aecc-4210-414f-8825-dba1964feb41/image.jpeg?t=1792785039)

For this reason, Arca has developed an **internal memory system**.

“So we have this **persistence for every client**,” Singh said in our podcast discussion. “What has happened so far? What are we tracking for next? What needs to happen immediately in the next couple of days \[and\] what are we tracking for the long term? Which individual is responsible for which action? **All of this is being tracked by our memory systems** that our agents contribute to.”

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/81045473-c40c-4197-909a-71806179579c/image.png?t=1792785040)

Arca uses models from frontier labs like Anthropic and OpenAI, with Claude its primary model provider. The company has **built its own harness**, which automatically selects the best model to use for a particular task, based on the company’s internal benchmarks.

Owning the harness, rather than using a managed agent harness, also means that **Arca has the flexibility to change model providers if it needs to.**

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/acde5762-7d2b-4538-a7ea-ec331f444ed1/image.png?t=1792785041)

Safety is obviously key for financial services. Singh noted that although agents do much of the work, **an agent’s role is limited to analysis and proposing solutions.** Trading and any other money movement is handled by a human advisor.

**“All of our safety restrictions are fully deterministic,”** he said. “And they take the decision making off the agent's hands.”

In other words, there are certain tools and tasks the agent can execute by itself — but **not ones that involve risk to the client.** Arca also runs verification checks on known AI security issues, such as prompt injections.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/4611a0eb-eaf0-4324-98ca-d6f4bc801803/image.png?t=1792785040)

Arca’s agent isn’t itself sandboxed, but **it does use sandboxes for computation.** Singh explained they’ve gone even further, with extra security measures for handling sensitive data.

“We've built systems to make it safe for code inside of that sandbox to access our data,” he said. “So we have this thing which translates the code it \[the agent\] wrote into a strict format that we expect, throw away the code, and **you install guardrails on that format before it reaches your back end.** So we allow the agents to use code to query our data, which means that they can aggregate it, they can munge it, they can work with it as they see fit to produce \[things\] like a financial plan.”

Another interesting process Arca has implemented is using feedback from human advisors, along with traces from the agents, to improve the harness. A **“meta-agent”** is used to analyze all this data and suggest changes to the harness.

## Podcast timestamps

- [00:00](https://www.youtube.com/watch?v=KM3VQZKjpiY&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — Introducing Arca’s AI-native wealth management firm

- [01:40](https://www.youtube.com/watch?v=KM3VQZKjpiY&t=100s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — How AI supports human financial advisors

- [02:40](https://www.youtube.com/watch?v=KM3VQZKjpiY&t=160s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — Choosing models with internal benchmarks

- [04:01](https://www.youtube.com/watch?v=KM3VQZKjpiY&t=241s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — Building a custom harness and routing tasks

- [05:25](https://www.youtube.com/watch?v=KM3VQZKjpiY&t=325s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — Handling a $100,000 tax bill: Agents and human approvals

- [07:20](https://www.youtube.com/watch?v=KM3VQZKjpiY&t=440s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — Agent memory across long-running tasks

- [09:22](https://www.youtube.com/watch?v=KM3VQZKjpiY&t=562s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — Safety guardrails and prompt injection defenses

- [11:02](https://www.youtube.com/watch?v=KM3VQZKjpiY&t=662s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — Compliance and auditability by default

- [12:14](https://www.youtube.com/watch?v=KM3VQZKjpiY&t=734s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — “Agents prompt people more than people prompt agents”

- [13:46](https://www.youtube.com/watch?v=KM3VQZKjpiY&t=826s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — Why Arca owns its agent infrastructure

- [15:05](https://www.youtube.com/watch?v=KM3VQZKjpiY&t=905s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — AI-native firms versus firms using AI tools

- [16:09](https://www.youtube.com/watch?v=KM3VQZKjpiY&t=969s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — Sandboxes and financial plan verification

- [18:40](https://www.youtube.com/watch?v=KM3VQZKjpiY&t=1120s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — Decision models and checking financial facts

- [19:51](https://www.youtube.com/watch?v=KM3VQZKjpiY&t=1191s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — Why trade execution stays with human experts

- [20:32](https://www.youtube.com/watch?v=KM3VQZKjpiY&t=1232s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — Turning staff feedback into evals and improvements

- [22:22](https://www.youtube.com/watch?v=KM3VQZKjpiY&t=1342s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — Turning operational knowledge into agent context

- [23:41](https://www.youtube.com/watch?v=KM3VQZKjpiY&t=1421s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — Client examples: Hidden fees and proactive service

## **Watch Aman’s AIE presentation**
