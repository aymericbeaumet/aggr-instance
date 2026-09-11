---
title: Datasette 1.0a39 and 0.65.4 security releases
link: https://simonwillison.net/2026/Sep/11/datasette-security/
source: simonwillison-net
published: 2026-09-11T03:27:16Z
updated: 2026-09-11T03:27:16Z
first_seen: 2026-09-11T05:07:40.995277925Z
labels:
- agentic-engineering
- ai
- ai-security-research
- datasette
- generative-ai
- llms
- releases
- security
summary: 'Datasette 1.0a39 and 0.65.4 security releases Today we''re releasing two new security patch versions of Datasette: 1.0a39 and 0.65.4 - one for the current alpha series and one for the stable 0.65.x family. These are security fixes which you should apply if you are running a Datasette instance on the public web - in particular if that instance mixes both public and private tables. Following issues reported by Sevban Dönmez, Alex Garcia and I ran an extensive audit of Datasette using Claude Fable 5.1, GPT-5.6, and GPT-6 Astra. We then spent almost a week collaborating on and reviewing the fixes. They helped find some very subtle bugs. We''ll be incorporating security audits by frontier models into all of our development work going forward. Alex came up with a way of splitting the work which I found extremely productive: Alex Garcia and I worked together running and then responding to the audit, working in a shared private repository. For most of the issues we split the work: one of us would create the automated tests highlighting the issue, then the other would implement the fix. This ensured that two separate humans had eyes on each of the issues, in addition to our coding agents running different models. Tags: releases, security, ai, datasette, generative-ai, llms, agentic-engineering, ai-security-research'
content: extracted
html: 2026-09-11-datasette-1-0a39-and-0-65-4-security-releases.html
---

**[Datasette 1.0a39 and 0.65.4 security releases](https://datasette.io/blog/2026/september-security-releases/)**. Today we're releasing two new security patch versions of Datasette: [1.0a39](https://docs.datasette.io/en/latest/changelog.html#v1-0-a39) and [0.65.4](https://docs.datasette.io/en/stable/changelog.html#v0-65-4) - one for the current alpha series and one for the stable 0.65.x family.

These are security fixes which you should apply if you are running a Datasette instance on the public web - in particular if that instance mixes both public and private tables.

Following issues reported by [Sevban Dönmez](https://github.com/jankesec), [Alex Garcia](https://alexgarcia.xyz) and I ran an extensive audit of Datasette using Claude Fable 5.1, GPT-5.6, and GPT-6 Astra. We then spent almost a week collaborating on and reviewing the fixes.

They helped find some *very* subtle bugs. We'll be incorporating security audits by frontier models into all of our development work going forward.

Alex came up with a way of splitting the work which I found extremely productive:

> Alex Garcia and I worked together running and then responding to the audit, working in a shared private repository. For most of the issues we split the work: one of us would create the automated tests highlighting the issue, then the other would implement the fix. This ensured that two separate humans had eyes on each of the issues, in addition to our coding agents running different models.
