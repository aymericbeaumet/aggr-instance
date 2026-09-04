---
title: Devin in Windsurf
link: https://cognition.com/blog/devin-in-windsurf
source: cognition-com-blog
published: 2026-04-15T00:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2026-04-15-devin-in-windsurf.html
---

By The Cognition Team04.15.26

![Devin in Windsurf](https://cdn.sanity.io/images/2mc9cv2v/production/caa9b5c9a1bf7cdd99db57306bd3a462f5c1c6bc-1920x1080.jpg?w=1600&fit=max)

Most people treat local and cloud agents as the same thing, but they're not. A local agent runs on your machine, in your session, but when you close your laptop, it stops. The ceiling on a local agent is your attention.

Devin is a cloud agent. It runs in its own infrastructure and in its own environment. Devin can work for minutes or hours, past [the async valley of death](https://cognition.ai/blog/swe-grep#fast-context-as-the-first-step-to-fast-agents). It opens PRs, runs tests, QAs its own work using computer vision, and lets you know when it’s done.

A local agent makes you faster, but cloud agents do the work while you’re not there.

We've been building Devin to operate independently from the start. Each release has pushed it further toward working without you in the loop: self-testing with computer use, reviewing and auto-fixing its own code, managing teams of sub-agents in parallel, and scheduling its own work.

### Local and cloud agents

The best workflow isn't restricted to just local or cloud agents, though. A local agent is where you think. You use one or multiple local agents to plan, prototype, and iterate the work that requires your hands on the keyboard. A cloud agent is where you delegate work that needs to get done but doesn’t need you watching over the shoulder: implementation, testing, QA, and deployment.

With a local agent, you're faster. With a cloud agent, you can parallelize yourself.

### **Devin in Windsurf**

Today, with Windsurf 2.0, that workflow is built in.

You work locally in Windsurf to understand the codebase and put together a plan. With a single click, you send it to Devin for implementation. Devin spins up its own machine and gets to work. In the meantime, you keep coding, or you close your laptop and grab a coffee.

When Devin opens a PR, you review it right in Windsurf. You can check the diff, run tests, or hand it off to your local agent for touch-ups. The whole loop of planning, delegating, monitoring, and reviewing all happens in one place.

## Try it out now

[Read the Windsurf 2.0 launch post.](http://windsurf.com/blog/windsurf-2-0)\
[Download Windsurf 2.0](https://windsurf.com/download)
