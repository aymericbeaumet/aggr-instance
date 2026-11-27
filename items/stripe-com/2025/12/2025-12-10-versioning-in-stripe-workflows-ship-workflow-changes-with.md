---
title: 'Versioning in Stripe Workflows: Ship workflow changes with confidence'
link: https://stripe.dev/blog/introducing-versioning-for-stripe-workflows
source: stripe-com
published: 2025-12-10T00:00:00Z
updated: 2025-12-10T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Tanya Boiteau
summary: Versioning in Stripe Workflows lets you edit automations safely with drafts, roll back changes instantly, and see exactly what ran. Update with confidence.
content: extracted
html: 2025-12-10-versioning-in-stripe-workflows-ship-workflow-changes-with.html
remote_preview:
  url: https://stripe.dev/og/blog/introducing-versioning-for-stripe-workflows.png
  alt: 'Stripe Developer Blog: Versioning in Stripe Workflows: Ship workflow changes with confidence'
---

[Stripe Workflows](https://docs.stripe.com/workflows?locale=en-GB) helps you automate the routine parts of your business, send a receipt after a payment succeeds, notify your team when a payout fails, route orders based on inventory, or trigger follow-ups when a dispute is opened. They’re the kind of thing you set up, trust to run, and don’t think about again until something in your business changes. And something always does.

Maybe you tighten a fraud check, add a retry, or adjust the timing of an email. These sound like small changes, but they all require touching a workflow that’s already in production. And that’s where things often get stressful.

## The challenge of updating live workflows

A common pattern we see is that “I hope this doesn’t break anything” moment. You open a workflow that’s been running smoothly for months, change a step, and worry you might affect a live flow. If the change behaves unexpectedly, there’s no easy way to revert. If a colleague edited it last week, you may not know what changed or why. And if a customer hits a strange edge case, you can’t always inspect what the workflow looked like when it ran. These moments aren’t rare, they’re the natural result of updating logic that’s already in use, without having the tools to track changes or recover easily when something goes wrong.

Before versioning, updating a workflow required a level of caution that didn’t match the simplicity of the change you were trying to make. You might only want to adjust one condition or add one more action, but doing so meant editing the live workflow definition directly. There was no draft state, no safety net, and no way to preserve what previously worked. Each save overwrote the last version entirely.

Sandboxes helped, but only in a limited way. You could build and test a workflow safely in a sandbox, but once it was deployed to live mode, any future edits, no matter how small, risked unintended side effects. And because Workflows didn’t keep historical definitions, reverting wasn't possible. If a change caused unexpected behaviour, you had to manually reconstruct the old logic from memory or screenshots, then hope you rebuilt it correctly. Even small mistakes could ripple into production without an easy path back to a known state.

Troubleshooting was just as constrained. The run details page lets you inspect what happened during a run, but only the path taken. If a branch wasn’t run, you couldn’t see what the structure of the workflow looked like at the time. If an issue appeared days later, you might be looking at a run that used a workflow definition that no longer exists. This meant debugging required guesswork, and often trying to remember how the workflow used to be configured.

And for teams, collaboration added another layer of uncertainty. If someone updated a condition or rewired a branch, there was no way to know who made the change or why. Context lived in people’s heads, slack threads, or direct messages, but not in the workflow itself.

In practice, this meant teams were slower to improve automations, more hesitant to make changes to logic, and less confident when debugging. The workflows were reliable, but the process of updating and understanding them wasn’t as easy as it should have been. The friction came not from the workflows themselves, but from the lack of tooling around how they changed over time.

## A safer way to evolve workflows

Versioning gives you a safe, predictable way to evolve your workflows, without losing track of what changed along the way. Instead of editing the active workflow directly, you now have a dedicated draft where you can make updates at your own pace whether that’s adjusting a condition, adding a new step, or reorganizing branches. This separates the process of iterating on logic from the logic that’s currently running in production.

Each published version captures a full snapshot of the workflow definition at that moment in time. That means you can always see exactly which version ran, what branch was taken, and which branches weren’t, something that simply wasn’t possible before. And if a new change doesn’t behave the way you expected, you can roll back to a known good version in a single click. This turns recovery from a manual reconstruction exercise into a straightforward action.

You can also annotate each version with a description explaining the “why” behind the change. Over time, this builds a clear, shared record of updates across your team so there’s no more guessing and no more diff hunting. Teams gain the context that normally lives in Slack threads or personal notes, directly in the workflow itself.

Versioning is designed to make workflow updates safe, reversible, and understandable. It takes the stress out of shipping changes and gives you the visibility you’ve been missing. You can update workflows with the same confidence you have when building software, with drafts, history, and the ability to look back at exactly what ran.

![](https://stripe.dev/images/introducing-versioning-for-stripe-workflows/image1.png)

## How versioning works

Versioning adds three core elements to Workflows: drafts, published versions, and version history, and each one supports a different part of how you update automations safely. Together, they give you a controlled, predictable way to evolve a workflow while keeping the active version stable.

### Drafts: a dedicated place to iterate

Every workflow has a draft. When you create a workflow for the first time, you start directly in this draft. When you return later and choose “Edit workflow”, you’re editing the draft again, not the active version.

Drafts are where all edits happen. You can add new steps, introduce branches, or adjust conditions, update messages, or refine logic over time. The draft doesn’t run in production, and changes aren’t applied until you publish.

Drafts also behave like a workspace that you can return to. Saving a draft simply keeps your progress so you can continue editing it later without losing any changes. If you leave without saving, the editor prompts you that the changes will be lost.

Drafts make it possible to evolve a workflow gradually. You can make a few edits today, return tomorrow, and continue exactly where you left off, all without affecting the active version that you rely on.

When you’re ready to make the workflow live, you publish the draft. Publishing is what creates a new active version and determines how future runs behave. Until then, the draft is isolated and has no effect on production.

### Published versions: immutable snapshots

When you publish a workflow, Stripe creates a new numbered version. This is a complete, frozen snapshot of the workflow’s definition at that exact moment. This includes every step, branch, condition, variable reference, call, and configuration detail.

This published version becomes the active version. All new workflow runs use this definition immediately. Any runs already in progress finish on the version they started on, ensuring consistency and preventing mid-run surprises.

Published versions are immutable. Once created, they cannot be edited or modified. Any future change, whether it’s adding a step, adjusting a condition, or restructuring a branch, always begins in the draft, never in a live version. Publishing again will generate the next numbered version, for example, promoting a draft after v2 creates v3.

Because each version is fully preserved, you can always return to it. Selecting a past version shows information such as its status, author, notes, run count, and gives you the option to create a new draft from it. If you publish that draft, Stripe increments the version number and promotes your changes as the new active version. Older versions remain intact for reference.

This snapshot model guarantees that active workflows stay stable, every change is intentional, and your team always has a precise historical record of how the workflow evolved. And because every run now records the exact version it used, you can inspect the workflow definition that was active at the time, including the branch that ran and the branches that didn’t. This level of visibility wasn’t possible before versioning and significantly improves debugging.

![](https://stripe.dev/images/introducing-versioning-for-stripe-workflows/image2.png)

## What this means for you

Versioning makes it easier to update your workflows with the same confidence you have when they’re running. Instead of treating changes as risky or irreversible, you can evolve logic gradually, publish only when the update is ready, and return to any previous version if something doesn’t behave as expected.

It also gives you clearer insight into how your workflows work. Every run points to the version that ran, so you can understand exactly why something happened, down to the branches taken and the ones skipped. And with version descriptions and built-in history, you no longer have to rely on memory or Slack threads to understand how a workflow changed over time.

This means you have a system that’s easier to improve, easier to debug, and easier to maintain as your business evolves.

You can start using versioning in Stripe Workflows today. Every new workflow now includes a draft by default, so you can explore the builder, make a change, and publish when ready. To try it out, open the [Stripe Dashboard and navigate to Products > Workflows](https://dashboard.stripe.com/workflows). Create a new workflow and see how drafts and versions shape a clearer, more reliable development flow.

For more Stripe developer learning resources, subscribe to our [YouTube Channel](https://www.youtube.com/@StripeDev).
