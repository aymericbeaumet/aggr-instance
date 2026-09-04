---
title: Devin September '24 Product Update
link: https://cognition.com/blog/sept-24-product-update
source: cognition-com-blog
published: 2024-09-05T00:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2024-09-05-devin-september-24-product-update.html
---

## Devin automatically responds to comments on its PRs

Try reviewing Devin’s code via Github or Github Mobile — Devin will automatically respond as long as the session hasn’t ended and Devin isn’t sleeping.

Github integration demo

## Devin automatically suggests Knowledge

Try giving Devin feedback in chat! Devin will automatically suggest new additions to Knowledge if something seems useful for future sessions.

Knowledge Suggestion Demo

Knowledge is a collection of tips, documentation, and instructions that Devin “knows” across all future sessions. Devin will automatically recall relevant Knowledge as necessary, and you can always manually add or review Knowledge in **Settings & Library** > **Knowledge.**

Add Knowledge Demo

## Roll back and return to a previous point in time

This has been one of our most requested features! Scrub Devin’s timeline and click the “restore checkpoint” icon at the bottom right corner.

This restores Devin to a previous point in time, rolling back Devin’s files and memory. **This is particularly useful when…**

- **Devin made good progress, but then makes a mistake —** it can be faster to revert Devin’s changes and let Devin retry with hints (alternatively, take over and complete the task yourself by [using Devin’s machine](https://www.cognition.ai/blog/june-24-product-update#use-devins-browser-editor-and-shell))
- **Iterating on a Playbook / prompt —** Try rolling back and editing a Playbook to test whether the edit helps Devin succeed more reliably

![Devin September '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/484f1a5a0232be076124a6f2bf5df9bc7f1e65a7-936x720.gif?w=1600&fit=max)

## With MultiDevin, let Devin create other Devins to delegate and accelerate work

Tackle large backlogs of tasks by delegating to a team of Devins that work in parallel. MultiDevin consists of 1 “manager” Devin and up to 10 “worker” Devins.

The manager Devin distributes a task to each worker Devin, then merges the changes from all *successful* worker Devins into one branch or pull request. MultiDevin is great for repeated, isolated tasks like lint errors, code clean-ups, migrations, refactors, and more!

This feature is currently available for users on our Enterprise plan.

![Devin September '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/67b075fd7a10dcc2050fd39c5fd39653023f0cf8-1626x1030.png?w=1600&fit=max)

## Devin in your VPC

Devin offers an enterprise deployment option tailored for organizations with stringent security and compliance requirements. With support for all major clouds, Devin’s dev boxes are deployed within your Virtual Private Cloud (VPC), and customer data is always saved within your controlled environment.

## Custom Devins

Custom Devins are fine-tuned versions of Devin specialized for specific use cases and/or proprietary datasets. Custom Devins are faster and more reliable for a narrower set of use cases. We recommend Custom Devins for repetitive engineering tasks, now available for Enterprise users!

## Old Devin sessions can now be “woken up”

Previously, Devin sessions ended after long periods of inactivity. Now, most sessions will “sleep” instead, meaning that you can wake Devin up and resume the session at any point.

You can still end sessions manually with the “stop” button at the top right corner of the chat.

![Devin September '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/8aef908507c1956d0b2e68dc94e8d3bbff1a3df2-851x570.png?w=1600&fit=max)

## Devin’s Work Log

Devin now maintains a work log in its planner to help you more quickly grok what it’s accomplished.

Open the accordions to read Devin’s retro of its work at each step. 🟢/ 🟠 / 🔴 correspond to A/B/C grades. You’ll also find timestamps and how long Devin spent at each step.

![Devin September '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/b673128f334303fcf6c8e8ca070a5ed86efb6d4d-985x814.png?w=1600&fit=max)

## Devin’s Shell improvements

It’s now easier to navigate Devin’s shell - hover over the blue dots on the right hand side to preview commands, and click to jump. Use the new scrollbar to quickly navigate.

![Devin September '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/736e2b91326ca9af32e2fcd04a332be0b0fef760-847x949.png?w=1600&fit=max)

## Ask Devin about Devin

Devin is now aware of its own product features and improvements. Try asking Devin what it knows about the Devin web app, and it’ll explain its features and where to find them.\

![Devin September '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/5e57a1be802c8501b4d6550a03823a7280c557ef-906x1066.png?w=1600&fit=max)

## Use Shortcuts to save commonly used Snapshots + Playbooks

**Snapshots** are ‘save’ states for Devin. After you take a snapshot, you can start from that machine state (with repos cloned, environment set up, etc) on any future Devin run. [Learn more here](https://docs.devin.ai/Onboard_Devin/snapshots).

**Playbooks** are easily reusable prompts — useful for common, recurring tasks or reminders to Devin.

With **Shortcuts**, you can now save Snapshot + Playbook combinations to your home page! We recommend creating one Shortcut for each repo you work on with Devin.

![Devin September '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/990a322299c49dcbc276f020f872373ca3a80ed1-624x480.gif?w=1600&fit=max)

## The PR metrics view aggregates all PRs made by Devin

Learn how your teammates are using Devin and explore Devin’s PRs in the new PR metrics view.

![Devin September '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/21b85fba3e87ce2f07ef58496388fefb879d9de7-1422x571.png?w=1600&fit=max)

## Send Devin code reviews in product

Ask Devin questions or ask for edits to specific lines of code. The code you comment on will be sent to Devin in one chat message.

Simply highlight any text in Devin's editor and click "Add to chat" or “Add a comment".

![Devin September '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/fd141ce9a61cc4d3dc9daf7f13903363f2268a7f-624x480.gif?w=1600&fit=max)

## Session filtering

Quickly filter all of your sessions by creator, content, status, playbook, date, etc.

![Devin September '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/adb22cff4357f17cd1dc607d920b1d584e9956a3-433x234.png?w=1600&fit=max)

## And many improvements to Devin’s brain!

Most of our improvements have been behind the scenes improvements to Devin’s capabilities. Devin is now **faster, more accurate with code edits, more reliable at following your instructions, and better at independent decision making.**

In our evaluations, we've seen up to an 80% reduction in the time needed for Devin to complete common tasks ranging from full stack development and devops to code refactor and cleanup.

As an example, a prompt popular with early users asked Devin to create an interactive map of wildfires in California. In June, this often took Devin upwards of 30 minutes. Today, Devin [completes it in 8 minutes or less](https://preview.devin.ai/sessions/d710fa73360d4c45b8e82b48ef840552). These speed improvements aren't just the result of faster individual actions. They also reflect Devin's improved decision making. Devin now loops less and gets itself unstuck more quickly.

Explore our [updated documentation](https://docs.devin.ai/Get_Started/devin-intro) for example prompts to see these improvements in action in your next session with Devin!

![Devin September '24 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/07289478e2ac1f9dec7c230ece8e61183e892019-1482x844.png?w=1600&fit=max)

## Hire Devin

We are working hard to scale up access to Devin, and we continue to do weekly invite releases. To start using Devin for engineering work, [join the waitlist](https://www.cognition.ai/get-started) or get in touch at [info@cognition.ai](mailto:info@cognition.ai).
