---
title: MCP was always a bad idea?
link: https://simonwillison.net/2026/Sep/20/hn-49779718/
source: simonwillison-net
published: 2026-09-20T20:24:41Z
updated: 2026-09-20T20:24:41Z
first_seen: 2026-09-22T00:03:48.971012939Z
labels:
- hacker-news
- model-context-protocol
summary: 'My comment on MCP was always a bad idea? — Hacker News. This article entirely misses the value that MCP brings today. Sure, there''s almost no reason to use MCPs if you are running a full-blown terminal agent (Claude Code, Codex, Meta Muse, OpenClaw etc) with unfettered internet access - just let it call APIs directly. If you want to operate something that''s less YOLO than that, you''ll find yourself wanting: Control over exactly which external services it can access A way to handle authentication that doesn''t allow the agent to directly access API keys A sensible UI to allow users to connect and authenticate further services Strong audit logging for what''s going on MCP makes all of that so much easier to provide. Thinking MCP is obsolete because full coding agents don''t need it misses out on all of the other things we might want to build. Tags: hacker-news, model-context-protocol'
content: extracted
html: 2026-09-20-mcp-was-always-a-bad-idea.html
---

This article entirely misses the value that MCP brings today.

Sure, there's almost no reason to use MCPs if you are running a full-blown terminal agent (Claude Code, Codex, Meta Muse, OpenClaw etc) with unfettered internet access - just let it call APIs directly.

If you want to operate something that's less YOLO than that, you'll find yourself wanting:

1. Control over exactly which external services it can access
2. A way to handle authentication that doesn't allow the agent to directly access API keys
3. A sensible UI to allow users to connect and authenticate further services
4. Strong audit logging for what's going on

MCP makes all of that so much easier to provide.

Thinking MCP is obsolete because full coding agents don't need it misses out on all of the other things we might want to build.
