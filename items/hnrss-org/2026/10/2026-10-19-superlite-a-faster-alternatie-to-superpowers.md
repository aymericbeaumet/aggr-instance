---
title: 'Superlite: A Faster Alternatie to Superpowers'
link: https://contextfirst.blog/superlite-hand-an-agent-a-spec/
source: hnrss-org
published: 2026-10-19T02:01:16Z
updated: 2026-10-19T02:01:16Z
first_seen: 2026-10-19T12:57:26.464953148Z
authors:
- iheatuchriswogu
labels:
- agentic delivery
- engineering management
- field notes
content: extracted
html: 2026-10-19-superlite-a-faster-alternatie-to-superpowers.html
remote_preview:
  url: https://static.ghost.org/v5.0.0/images/publication-cover.jpg
---

Why I built superlite, an autonomous build workflow for Claude Code that doesn't ask permission every four minutes: how it works, how to use it, and the benchmarks.

I had a dream. I would hand an agent a spec, close the laptop, make a sandwich, and come back to finished, tested, committed work.

Here's what actually happened. I handed over the spec. I made the sandwich. I came back. The agent had written one paragraph and was asking, very politely, whether I approved section 1 of the design.

I approved it. I sat down. I waited.

Section 2. Approve? Yes. Section 3. Approve? *Yes.* The spec? Yes. The plan? Yes, obviously, it's the plan I just watched you write. And how would I like the plan executed? *However is fastest, please, I beg you.*

Then it started building. One task. Then a review of that task. Then a fix for the review. Then a re-review of the fix. Then the next task. I watched a progress log scroll past like a train at a level crossing: no stopping it, no getting past it, and no idea how many carriages were left.

I'd refresh the terminal. Still on task 3. I'd answer an email. Still on task 3. I'd start a second sandwich purely to have something to do with my hands. Task 4, under review.

The sandwiches were very good. The software didn't exist yet. And the worst part wasn't the time. It was that I couldn't leave. Every time I walked away, I came back to a question that had been sitting there for twenty minutes, waiting for me to type "yes."

I didn't want a collaborator that needed me every four minutes. I wanted a contractor. Here's the spec, here's the key, call me when it's done.

So I built [**superlite**](https://github.com/iha2/superlite?ref=contextfirst.blog): a Claude Code plugin that plans a build, splits it into tasks that can run side by side, hands each one to the cheapest agent that can do it well, checks the seams between them, and reviews the result. Then it leaves you a tidy list of every decision it made while you were busy with the sandwich.

This post covers why it exists, how it works, and how to use it. The benchmarks are there to show I'm not just saying it's fast because I wrote it.

## Why I built it

First, a disclaimer, because I like this project and don't want angry emails. I use [superpowers](https://github.com/obra/superpowers?ref=contextfirst.blog) a lot. It's a careful, collaborative workflow that keeps a human close to every step, and it's genuinely excellent at that. Superlite borrows its bones shamelessly: the plugin layout, the session-start hook, the lifecycle skills. Credit where it's due, and then some.

But when I already knew exactly what I wanted, most of the time went on the workflow waiting for itself. I profiled it, the way you'd check your bank statement after a suspicious weekend. The time went to five places:

- **8–12 human gates per feature.** One question per message, designs approved section by section, then separate reviews of the spec, the plan, and the method of executing the plan. It's very thorough. It's also how a four-hour meeting gets scheduled.
- **Strictly sequential execution.** One implementer, then one reviewer, then the next task. On my first benchmark that was a single-file queue of 21 dispatches, like a bank with one teller and a very long lunch.
- **A review after every task, with re-review loops.** About 12 minutes of a 34-minute build.
- **The full test suite, re-run by several skills.** In case it changed its mind.
- **Opus subagents on the critical path,** writing code at about half Sonnet's speed. Brilliant, thoughtful, and in no hurry whatsoever.

None of these are mistakes. Each one is a sensible safety net. Stacked together, they're perfect when you want to steer every step, and slightly maddening when you just want the thing built.

So [superlite](https://github.com/iha2/superlite?ref=contextfirst.blog) isn't a rival to superpowers. It's tuned for a different job: **autonomous development, at speed.** If you want to steer each step, use superpowers. If you want to hand over a spec and come back to working code, use [superlite](https://github.com/iha2/superlite?ref=contextfirst.blog). Just don't enable both in the same session. They share skill names, both inject context at startup, and they argue like siblings in the back seat.

## How it works

The short version: one approval, a plan written for parallelism, agents matched to the work, and rules enforced by code because, as it turns out, agents read the rules and then do what they like.

flowchart LR A(\[Your request\]) --> B\[One batch of questions\] B --> C\[Plan\] C --> D\[Agents build\<br/>in parallel\] D --> E\[Review and fix\] E --> F(\[Done\])

**One batch of questions, one approval.** Superlite sizes the request first. A typo just gets fixed, without a design document about the typo. A change to an existing flow gets a short design in chat. A new project gets one batch of up to four questions, then one design-and-plan document. You approve it once. After that it only stops for things that are destructive, outward-facing or security-sensitive. It won't push to `main` while you're getting lunch. It will do everything else.

**You choose who's in charge.** With a human orchestrator, you see the whole plan with its predicted schedule and approve it before anything runs. With an AI orchestrator, you're not needed at all, which is either freeing or mildly insulting depending on your mood. Every choice it would normally ask about gets recorded as a ruling: what it decided, why, and what it costs if it got that wrong. When you come back, you know exactly which decisions to second-guess first.

**A plan written for the split.** The plan is a task graph. Each task lists the files it owns, the exact interfaces it consumes and produces, and the tests it has to pass. Tasks that don't share files and don't depend on each other run at the same time. The graph decides how wide the build goes. On the current benchmark that peaked at 18 agents working at once, which is roughly the size of a small startup, minus the ping-pong table.

**Agents matched to the work.** The org chart:

- **Opus coordinates.** It plans and dispatches, and it is not allowed to touch the code. Management, basically.
- **Sonnet builds** most of the tasks.
- **Haiku builds** only small, fully specified "mechanical" tasks that aren't on the critical path. The intern gets the well-defined stuff.
- **Sonnet reviews** the packages.
- **A fresh Opus final reviewer** reads a review packet built from the run's logs and returns findings only. It's never allowed to fix anything. It just points.
- **Fix-only agents** in three tiers take those findings, routed by how well the cause is understood.

**Gated by code, not by prompts.** I learned this one the hard way. You can write "the coordinator never edits code" in a prompt in bold, in capitals, with feeling. The coordinator will read it, nod, and edit the code. So now hooks enforce it. One hook blocks the coordinator from editing project code. Another catches any raw `pytest` or `npm test` it runs and quietly turns it into a background run, so tests never block it. Another refuses to let anyone kill processes by name (there's a story there, and it's in the deep dive). A plan has to pass `lint --stable` before work starts, and stable tasks are sent off the moment it passes.

**Every run is measured.** Hooks record each agent's tokens, cost and time. That feeds the next run's predictions and a soft budget that taps you on the shoulder at 70% of the cap.

## How to use it

You'll need Claude Code, git, Python 3.10 or newer (for superlite's own tools; the project you build can be in any language), and bash (Git Bash on Windows).

The source is [iha2/superlite](https://github.com/iha2/superlite?ref=contextfirst.blog) on GitHub. Install it from the marketplace:

```text
/plugin marketplace add iha2/superlite
/plugin install superlite@superlite-dev
```

Or clone [iha2/superlite](https://github.com/iha2/superlite?ref=contextfirst.blog) and load a local checkout for one session:

```bash
claude --plugin-dir <path-to-superlite>
```

To check it's working, open a fresh session and send `Let's make a react todo list`. A working install starts brainstorming before it writes any code. It's the "hello world" of agent workflows, and honestly the world doesn't need another todo list, but it's a good smoke test.

Then:

1. **Describe what you want.** A paragraph or a spec file is enough. No need for a 40-page requirements document. Although it will read one.
2. **Choose the orchestrator** when it asks: you check the plan, or the AI does everything.
3. **Let it run.** Watch `.superlite/` in your project like a nature documentary. `progress.md` has the rulings and the log, `test-summary.json` has the latest test result, and `metrics.jsonl` has the time and cost per agent.

What you say shapes the flow:

| Situation                  | Say something like                                             |
| -------------------------- | -------------------------------------------------------------- |
| A new project              | "Build me a static site generator from this spec"              |
| A feature on existing code | "Add CSV export with column widths to the existing app"        |
| Debugging                  | "This test fails on leap days: find the root cause and fix it" |
| A small change             | "Rename `build` to `render` everywhere"                        |
| Hands off                  | "Build X. I'm away, decide yourself"                           |
| Shipping                   | "We're done: finish the branch"                                |

It never pushes or merges without you. Autonomous, not reckless.

## Does it actually work?

Anyone can call their own tool fast. I once called a script I wrote "blazing" and it took eleven minutes to rename some files. So every number below comes from a headless benchmark harness. It builds the same project from the same spec, on Opus, and scores the result with a hidden test suite the agents never get to see. No peeking, no studying for the exam.

### The latest run: 16.1 minutes

The test project is deliberately not a todo list. It's a static site generator *plus* a spreadsheet engine (formula evaluator, HTTP API and a browser UI) built as one project from one spec, and scored by 177 hidden tests.

Here's the latest run, on the current build:

|                                    |                                           |
| ---------------------------------- | ----------------------------------------- |
| Wall-clock, spec to finished build | **16.1 min**                              |
| Hidden tests                       | **177 / 177**                             |
| Agents                             | 21 in total, **up to 18 working at once** |
| First agents building              | about 3 minutes in                        |
| Final review                       | 30 seconds                                |
| Stops to ask a human anything      | **0**                                     |

Compared with superpowers on the same project:

|                                            | Wall-clock   |                                                              |
| ------------------------------------------ | ------------ | ------------------------------------------------------------ |
| superpowers, stock (mean of 3 runs)        | 33.6 min     |                                                              |
| superpowers, orchestrated (mean of 3 runs) | 23.5 min     |                                                              |
| **superlite, latest run**                  | **16.1 min** | **52% less time** than stock, **31% less** than orchestrated |

Same 177 out of 177 hidden tests, in about half the time. That's one run of the current build against earlier runs on the same machine, so call it a strong hint rather than peer-reviewed science. But it's a very strong hint.

It also beat its own previous build (which averaged 26.9 minutes) by 40%. Yes, I'm competing with my own past self. It's the only rival I can bully freely.

### How it got there

The latest run didn't come out of nowhere. The first build was already quick on a smaller task: `sitegen` alone, nine modules, 89 hidden tests.

|                       | superpowers   | superlite   |
| --------------------- | ------------- | ----------- |
| Wall-clock            | 34.4 min      | **8.8 min** |
| Hidden tests          | 89 / 89       | 89 / 89     |
| Serial dispatch chain | 21 dispatches | 3 levels    |

Most of that speed came from getting rid of the queue. Superpowers dispatched 21 agents, one after another. Superlite dispatched 10, up to five at once. Same work, fewer people standing in line.

On the full 177-test project, an earlier superlite build ran three times against superpowers:

|                            | superpowers            | superlite              |
| -------------------------- | ---------------------- | ---------------------- |
| Mean wall-clock            | 33.6 min               | **25.1 min**           |
| Runs                       | 21.2 / 31.9 / 47.6 min | 21.4 / 23.2 / 30.5 min |
| Spread, fastest to slowest | 26.4 min               | **9.1 min**            |
| Hidden tests               | 177 / 177 every run    | 177 / 177 every run    |

Faster on average and much more predictable. A 47-minute worst case is the difference between "done before standup" and "done after standup, during which you had to say it's nearly done."

But the data also showed where superlite was still waiting on itself. The build was finishing at about 13 minutes, and then 10 to 18 more minutes went on reviews, fixes, and the coordinator running tests in the foreground and staring at them. So:

- **The coordinator lost the ability to edit code.** It took it surprisingly well.
- **Every test run moved to the background.**
- **Fixes went to tiered fix-only agents.**
- **Stable tasks started building while the rest of the plan was still being fixed.**

That's the build that ran in 16.1 minutes.

## The takeaway

If your agent workflow is slow, it's probably not slow because the model is thinking deep thoughts. It's slow because work is waiting in a line, and something keeps asking permission.

Split the work along interface boundaries. Let independent tasks run at once. Move the mechanical steps into scripts. And enforce the rules that matter with code, because asking nicely only works on people, and only sometimes.

The plugin is on GitHub: [github.com/iha2/superlite](https://github.com/iha2/superlite?ref=contextfirst.blog).

In the follow-up post I go much deeper into the philosophy and the decisions behind superlite: the gates, the agent tiers, and the graveyard of ideas I was very confident about right up until I measured them.
