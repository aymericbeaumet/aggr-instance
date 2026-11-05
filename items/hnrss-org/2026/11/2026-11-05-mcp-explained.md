---
title: MCP Explained
link: https://mcp-explained.com/
source: hnrss-org
published: 2026-11-05T14:08:35Z
updated: 2026-11-05T14:08:35Z
first_seen: 2026-11-05T17:57:26.867095589Z
authors:
- rwilinski
content: extracted
html: 2026-11-05-mcp-explained.html
remote_preview:
  url: https://mcp-explained.com/og.png
  alt: 'MCP Explained by Runlayer: a diagram of a model, an MCP client and GitHub, Linear and Slack MCP servers.'
---

## What is MCP?

An open standard for connecting AI apps to the tools and data they use. A server says what it can do, the app shows that to the model, and when the model wants something the app calls the server and hands the result back.

Host

the AI app you use: Claude, Cursor, ChatGPT, an agent

Server

a small program exposing tools, data or prompts

Model

decides what to use; never talks to a server directly

Not REST

JSON-RPC over stdin/stdout or one HTTP endpoint, and servers can talk back

Not just JSON-RPC

JSON-RPC is the envelope. MCP defines what goes in it: the methods, capability negotiation, transports and auth

or jump to [Tools](https://mcp-explained.com/#tools) · [the spec time machine](https://mcp-explained.com/#transports@2024-11-05) · [Break it](https://mcp-explained.com/#poisoning)

Spec

| # | From | To | Method | Info |
| - | ---- | -- | ------ | ---- |
