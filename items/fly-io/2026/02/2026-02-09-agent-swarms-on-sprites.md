---
title: Agent Swarms on Sprites
link: https://fly.io/blog/agent-swarms-on-sprites/
source: fly-io
published: 2026-02-09T00:00:00Z
updated: 2026-11-06T16:35:06Z
first_seen: 2026-11-27T00:01:05.630507547Z
content: extracted
html: 2026-02-09-agent-swarms-on-sprites.html
remote_preview:
  url: https://fly.io/blog/agent-swarms-on-sprites/assets/agent-swarms-on-sprites.jpg
  alt: Multiple AI agents coordinating across Sprites
extra:
  thumbnail: https://fly.io/blog/agent-swarms-on-sprites/assets/agent-swarms-on-sprites.jpg
---

Author

![Daniel Botha](https://fly.io/static/images/danielb.webp)

Name

Daniel Botha

![Multiple AI agents coordinating across Sprites](https://fly.io/blog/agent-swarms-on-sprites/assets/agent-swarms-on-sprites.jpg)

There’s a lot of noise right now about agent teams and swarms. The idea is simple: instead of one code agent grinding through a task list, set up a group of agents that can split work, delegate, and check each other’s output. It works. Multiple agents in parallel genuinely get more done.

The part that nobody has a great answer for is the environment. Where do these agents actually run? Most setups put them all on the same machine; same filesystem, same network, same blast radius. Agent A installs Node 23, Agent B needs Node 20. Two agents write to the same file. One runs `pip install sketchy-package` and now everyone’s environment is different.

An agent swarm on [Sprites](https://fly.io/sprites/ "") comes with some handy capabilities. Full machine isolation per agent, checkpoints that let you roll back any agent independently, per-agent network policies, and persistence that survives you closing your laptop.

## One Agent, One Sprite

When each agent lives in its own computer, agent teams work more like human teams. Everyone has their own desk. They can install whatever they need, make a mess, try things — without touching anyone else’s work. The orchestrator hands out assignments and collects results. No shared state to babysit.

All you need is a Sprites API token (create one at `sprites.dev/account/{your-org}/tokens`) and Claude Code on the orchestrator.

Prompt:

> Create a new Sprite called “researcher”. When it’s ready, open a console and tell Claude to read the codebase at /home/sprite/myproject and write a summary of the architecture to /home/sprite/output/architecture.md. Use `--dangerously-skip-permissions` so it can work autonomously.

The orchestrator Sprite [creates the worker](https://fly.io/blog/make-computers-make-computers/ ""), gives it a task, and moves on. The worker does its thing independently. The orchestrator can check in later, read the output, or wait for the worker to finish.

## Checkpoints Are Your Undo Button

Every Sprite has [checkpointing](https://docs.fly.io/sprites/concepts/checkpoints "") built in. Claude inside a worker Sprite can checkpoint its own environment before doing anything risky — and the orchestrator can tell workers to do this as part of the task prompt. No human needs to be in the loop.

An agent refactors half the codebase, installs new dependencies, rewrites the database schema. If the result is garbage, restore the checkpoint. Three seconds later the Sprite is back to exactly where it was — not just the code, the whole environment.

This makes speculative execution practical. Tell the orchestrator to spin up three workers, have each one checkpoint, then attempt a different approach to the same problem. Pick the winner, restore or destroy the rest.

## Network Policy Per Agent

Every Sprite has its own [network policy](https://docs.fly.io/sprites/concepts/networking ""). The orchestrator can lock down each worker to only the domains it actually needs. Denied domains return REFUSED at the DNS level — fast-fail, no silent failures, no data going somewhere you didn’t intend.

## Communication Between Agents

Workers are isolated, but they still need to share results. A few patterns:

**HTTP endpoints.** Every Sprite has a URL. A worker can serve its results on port 8080 and the orchestrator fetches them. Good for structured output — JSON reports, test results, generated files.

**Shared database.** Spin up a Postgres Sprite. Give every worker network access to it. Workers write results to tables. Orchestrator queries. Good when agents need to build on each other’s findings.

**Orchestrator as relay.** Workers write output to a local file. Orchestrator opens a console to each worker and reads the file. Simple, no extra infrastructure. Good enough for most cases.

## Persistence for Free

Sprites don’t die when you close your laptop. They pause when idle and wake on demand. The orchestrator, the workers, the checkpoints — all still there when you come back.

Start the swarm, close your laptop, come back later. Everything is exactly where you left it.

*Bonus level:* Checkpoint your orchestrator Sprite after it’s set up with the API token and your standard prompts. Now you can restore to a ready-to-swarm state in three seconds whenever you need a fresh team.

Next post ↑

[Context Engineering Is Just Writing Things Down](https://fly.io/blog/context-engineering-is-just-writing-things-down/)

Previous post ↓

[Closing the Loop: The Missing Ingredient in Agentic Development](https://fly.io/blog/closing-the-loop/)
