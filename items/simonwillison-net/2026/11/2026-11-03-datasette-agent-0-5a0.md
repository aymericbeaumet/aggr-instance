---
title: datasette-agent 0.5a0
link: https://simonwillison.net/2026/Nov/3/datasette-agent/
source: simonwillison-net
published: 2026-11-03T23:13:45Z
updated: 2026-11-03T23:13:45Z
first_seen: 2026-11-04T21:49:17.908350494Z
labels:
- datasette
- datasette-agent
- projects
summary: 'Release: datasette-agent 0.5a0 This is a significant release of Datasette Agent. The most notable new feature is called the panel - it lets the agent chat open and then sit next to pages from Datasette, for example a table or query page. When combined with datasette-apps this means you can prompt a database-backed app into existence and then modify it from the chat that sits next to that app. You can try it by signing in with your GitHub account to agent.datasette.io. Tags: projects, datasette, datasette-agent'
content: extracted
html: 2026-11-03-datasette-agent-0-5a0.html
remote_preview:
  url: https://static.simonwillison.net/static/2026/datasette-agent-panel.webp
  alt: 'Screenshot of a split-screen chat interface titled "evaluate llm prices" (model tag "gpt-6-luna"): the conversation runs in a narrow column on the left, and a wide side panel on the right displays the app the assistant built, with "EXPORT" and "Close panel" buttons above it. In the chat, blue user b'
---

[datasette-agent 0.5a0](https://github.com/datasette/datasette-agent/releases/tag/0.5a0) — An LLM-powered agent for Datasette

This is a significant release of [Datasette Agent](https://agent.datasette.io/). The most notable new feature is called the **panel** - it lets the agent chat open and then sit next to pages from Datasette, for example a table or query page.

When combined with [datasette-apps](https://github.com/datasette/datasette-apps) this means you can prompt a database-backed app into existence and then modify it from the chat that sits next to that app.

![Screenshot of a split-screen chat interface titled \"evaluate llm prices\" (model tag \"gpt-6-luna\"): the conversation runs in a narrow column on the left, and a wide side panel on the right displays the app the assistant built, with \"EXPORT\" and \"Close panel\" buttons above it. In the chat, blue user bubbles read \"Build an app to help explore this data\" and \"give it a more hacker aesthetic design\", each followed by collapsed tool calls (app_create, app_render, app_view, app_str_replace), \"LLM Price Explorer v1.\" and \"v2.\" entries with \"View app\" and \"Edit\" buttons, and short assistant replies describing the app and its new dark terminal-inspired theme. The right panel shows the resulting \"LLM Price Explorer\" in neon green monospace on black: stat cards for 185 models from 12 providers with workload costs from $0.042 (Jev 1.13) to $300.00 (o1 Pro), filter and workload cost estimate controls, and a table of matching models with input, output and cached input prices per million tokens.](https://static.simonwillison.net/static/2026/datasette-agent-panel.webp)

You can try it by signing in with your GitHub account to [agent.datasette.io](https://agent.datasette.io/).

Posted [3rd November 2026](https://simonwillison.net/2026/Nov/3/) at 11:13 pm
