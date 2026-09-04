---
title: Introducing Auto-Triage
link: https://cognition.com/blog/auto-triage
source: cognition-com-blog
published: 2026-05-18T00:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2026-05-18-introducing-auto-triage.html
---

![Introducing Auto-Triage](https://cdn.sanity.io/images/2mc9cv2v/production/3a84ce188766faa5ce1ef3eb0d98d9c6ea7a581e-1200x630.png?w=1600&fit=max)

Devin can now monitor incoming alerts, investigate them automatically, and come back with an investigation, next steps, and even a PR.

Engineering teams already have systems that tell them when something might be wrong: Slack channels, Linear issues, GitHub checks, Sentry alerts, Datadog dashboards, customer escalations, and custom webhooks. Most of these systems usually stop at detection: they create a message, ticket, or alert, and then a human has to reconstruct the context around it.

With Auto-Triage, Devin proactively responds to new bug reports, incidents, and requests, and starts investigating immediately. Devin can inspect the codebase, check observability tools, look through related tickets or threads, ask for missing context, and spin up sub-Devins to investigate in parallel. If it finds the likely cause, it can post a summary. When the issue needs a human, Devin can tag the right owner. If the fix is clear, Devin can open a PR.

By the time an engineer looks at the issue, Devin is well underway with incident response, investigation, and remediation.

## **Customers are using Auto-Triage today**

> We’ve been using Devin Automations to automatically triage incidents for Modal’s inference team. It monitors our channel, so we don’t have to prompt Devin at all. Because it has context across our codebase and observability stack, it can investigate quickly and come back with fixes or next steps. It feels different from using Devin traditionally: Devin works on its own, and we can wake up to really good investigation without prompting it.

— Hari Subbaraj, Member of Technical Staff @ Modal

## **Devin gets smarter with every incident**

In Auto-Triage, Devin builds and maintains long-running context of prior investigations, recurring issues, and how your team prefers issues to be routed.

Devin’s memory makes each future investigation more useful: Devin will learn to tag the correct owner or reference the right part of the codebase for related issues. If a known issue fires new alerts, Devin can connect that to the earlier thread, de-duplicating incidents and saving significant triage time.

## **Auto-Triage works where issues pop up**

Devin can respond to Slack messages, Linear events, GitHub activity, schedules, and incoming webhooks, - right where reports first happen. Devin can investigate using connected tools like observability systems, issue trackers, and the codebase itself.

That makes it useful for a range of triage workflows: investigating production alerts, routing new bug reports, looking into failed CI runs, summarizing recurring health issues, and opening fixes.

## **Built for untrusted inputs**

Auto-Triage operates on messy inputs: Slack messages, alert payloads, tickets, logs, and webhooks. Those inputs can contain arbitrary text and should not be treated as trusted instructions.

So Devin runs in secure, network-sandboxed environments. Auto-Triage also includes additional protections against prompt injection and data exfiltration, so Devin can investigate real external inputs while keeping its execution environment and your code and data safe.

## **Try Auto-Triage**

Auto-Triage is available now in Devin Automations.

Connect the systems where your team handles bugs, alerts, and incidents, and give Devin an ongoing responsibility: respond immediately, remember what happened last time, link related issues, investigate with your tools, and bring back the next action.

For a limited time, we're offering $200 in credits when you set up your first automation. Try it out today: [Create your first automation](https://app.devin.ai/signup?from=auto-triage)
