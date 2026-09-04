---
title: 'Devin CLI: Start Local, Hand Off to the Cloud'
link: https://cognition.com/blog/devin-for-terminal
source: cognition-com-blog
published: 2026-04-27T00:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2026-04-27-devin-cli-start-local-hand-off-to-the-cloud.html
---

By The Cognition Team04.27.26

Run Devin right where you already work. When the work outgrows your laptop, hand the same session off to the cloud.

curl -fsSL https://cli.devin.ai/install.sh | bash

## **The full power of Devin, now local**

We took everything we learned building Devin and rebuilt it for working together with your agent, right in your shell.

Devin CLI is a local coding agent with full access to your codebase, your tools, and your environment. Choose between any frontier model, including Opus 4.7, GPT-5.5, and our own SWE-1.6. We wrote a custom terminal rendering library in Rust to make the UI as fast and snappy as possible.

The difference is what happens when local isn't enough.

## **Delegate to Devin in the cloud**

When the work outgrows your laptop, hand the session to a cloud agent with its own computer. Devin keeps working while you don't, so you come back to a finished PR.

Handoff to cloud agents changes what's possible with a terminal agent:

- Run multiple agents against the same codebase without fiddling with worktrees or setup scripts.
- Ship a feature, then move on while the agent tests it in its own browser.
- Hand off a bug fix and let Devin open a PR and resolve review comments for you.
- Stop worrying about rm -rf because the agent works in its own sandbox, not yours.

Devin CLI gives you access to agents in the cloud with their own computer, so they can keep working even when you close your laptop.

Get started in under a minute:

curl -fsSL https://cli.devin.ai/install.sh | bash

*P.S. We got Devin running on a VT-100, the terminal that's shaped modern development since 1978. \[[Watch the film here](https://www.devin.ai/terminal)\]*
