---
title: Workflows in Grok Build
link: https://x.ai/news/workflows
source: x-ai-news
published: 2026-07-23T00:00:00Z
first_seen: 2026-09-08T10:17:10.166315Z
summary: 'Grok Build can now write and run workflows: orchestration scripts that fan a task out across hundreds of parallel agents, verify the results, and report back in one background run.'
content: extracted
html: 2026-07-23-workflows-in-grok-build.html
preview:
  file: 2026-07-23-workflows-in-grok-build.preview-550444d97a1e.webp
  width: 256
  height: 134
  color: '#224595'
images:
- source: https://media.x.ai/v1/website/workflows-og-de52eecb.png
  original:
    file: 2026-07-23-workflows-in-grok-build.image-6632c2797090.png
    width: 1024
    height: 537
  color: '#2561e4'
---

Grok Build can now run **workflows**: describe a large task in plain language, and Grok plans it, fans it out across hundreds of parallel agents in the background, and reports back when everything is done. Your session stays free the whole time.

## [When to use a workflow](https://x.ai/news/workflows#when-to-use-a-workflow)

Workflows are built for complex, multi-faceted tasks a single conversation can't hold: reviewing every feature in a large PR, triaging the last 100 issues, auditing a codebase for one class of bug. If the work splits into many independent pieces and should end in one clear report, ask for a workflow.

## [How it works](https://x.ai/news/workflows#how-it-works)

Grok plans the task as a small script: the phases of work, the agents in each phase, and how their results roll up. Each agent starts with a clean, focused context, and the plan can build in checks a single pass can't, like independent skeptics verifying every finding before it reaches the final report.

Runs get a budget of 128 agents, and up to 1,024 for big jobs. Progress is saved as the run goes, so pausing and resuming never redoes finished work. Run `/workflows` to watch it live, phase by phase, with per-agent token counts.

## [Saving and reusing workflows](https://x.ai/news/workflows#saving-and-reusing-workflows)

Grok authors the workflow from your request, smoke-checks it before launching, and improves it between runs; you never write the script yourself. When one works, keep it: workflows in `.grok/workflows/` are shared with your team, and ones in `~/.grok/workflows/` follow you everywhere. Each saved workflow becomes its own slash command that takes arguments, so once you keep the PR review from the demo above, the next review is just `/pr-review 5137`.

`/deep-research` ships built in: it fans research questions out to parallel investigators, verifies every claim against its sources, and returns a cited report.

## [Try it today](https://x.ai/news/workflows#try-it-today)

Some examples to try:

- "use a workflow to review each feature in this PR"
- "triage the last 100 Linear issues and give me a top-10 action list"
- "audit every route handler for missing auth checks, and verify each finding"
