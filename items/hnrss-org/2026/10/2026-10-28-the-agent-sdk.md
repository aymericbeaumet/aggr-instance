---
title: The Agent SDK
link: https://cursor.com/blog/agent-sdk
source: hnrss-org
published: 2026-10-28T14:02:20Z
updated: 2026-10-28T14:02:20Z
first_seen: 2026-10-29T08:21:53.512644349Z
authors:
- torutofu
content: extracted
html: 2026-10-28-the-agent-sdk.html
remote_preview:
  url: https://ptht05hbb1ssoooe.public.blob.vercel-storage.com/OG%20Images/agent-sdk-blog-og-WlJMIsDgzkUUfvyf1R9PJSIfytr7Sx.png
---

Software teams use agents defined as code to handle recurring work across the development lifecycle. These agents connect to the places where a team works and act when relevant events occur, whether that means reviewing a pull request, executing a compliance workflow, or investigating an incident.

The most useful agents are tailored to how a particular team operates. Until now, building one has generally meant adopting a product designed around someone else's workflow or starting from scratch.

Today, we're releasing the Agent SDK, the framework we use to build our own managed agents like Bugbot, Security Reviewer, and Rollouts. Agent SDK allows you to define agents in code and manage them like any other software project. Start from a template or point your coding agent at our docs. When the agent is ready, `agent-sdk deploy` it to Cursor infrastructure with one command.

## Build around your workflow

The Agent SDK lets you quickly build and deploy agents that work the way your team does. They respond to the events your team already handles and follow policies your team can inspect and change.

Code review is one example. Each team has its own standards for what requires attention and who should review it. With the Agent SDK, you can build an agent that responds to pull request events from GitHub, GitLab, or Bitbucket. The agent can assess the risk of a change, route it to the right team, and approve changes that meet your policy.

As an example, a team within Cursor had built a large, fast-growing knowledge base of lessons learned and wanted every pull request checked against it. This way, the team's past decisions would hold even as the codebase changed. With the Agent SDK, they built kb-reviewer in about a day. It now runs on every PR in their main codebase, flags changes that contradict a recorded lesson, cites the note behind each finding, and encourages engineers to reconcile the discrepancies it reports.

For more complicated work, agents can hand a task to another specialized agent with its own tools and sessions. They can also divide a job among subagents working in parallel.

The Agent SDK also supports evals and event replay out of the box, allowing for rapid development with assurances that critical workflows execute the way you expect. The PGA Tour uses the Agent SDK to power their software development factory. Evals keep them moving quickly.

> The Agent SDK has enabled PGA to build every component of our software factory on Cursor's infrastructure. It provides the flexibility we need to build agents that drive our development process. And built-in support for evals allows us to quickly iterate on new ideas and optimize existing capabilities reliably.

Michael Gyarmathy

Principal Engineer, PGA America

## Define everything in code

An Agent SDK agent is organized like a small software project. The instructions that guide it sit alongside code for its tools, connections to outside services, and the evals used to test it.

Each file represents something the agent knows or can do. Prompts are written in Markdown, while tools are defined as TypeScript functions. Because everything is code, changes can go through the same version control and review process as the rest of your software.

## Deploy agents wherever you need them

When you're ready to put an agent into use, you can deploy it to Cursor by running `agent-sdk deploy` from the project directory. The command builds the agent, gives it a URL, and starts it on our infrastructure. You can manage secrets and integrations from the dashboard, while continuing to use the same logs, evals, and playground available locally.

You can also run the agent on your own infrastructure. Each agent runs as a single Node process that can be packaged in a container.

## Get started

Getting started with the Agent SDK looks much like starting any other software project. Create a project directory, then launch the local playground:

```
import { homedir } from "node:os";
import { join } from "node:path";
import { defineAgent } from "@cursor/agent-sdk";

export default defineAgent({
  name: "Robo",
  model: {
    id: "composer-2.5",
    params: [
      { id: "effort", value: "high" },
      { id: "fast", value: "true" },
    ],
  },
  local: {
    cwd: join(homedir(), ".cache", "factory", "robo"),
  },
})
```

From there, you can iterate locally and run evals before deploying with `agent-sdk deploy`.

The Agent SDK also ships with skills that guide coding agents through the development process. You can point an agent at the docs and have it help build the bot with you.

The Agent SDK is generally available today. Read the [docs](https://cursor.com/docs) and get started.
