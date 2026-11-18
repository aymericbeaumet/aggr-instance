---
title: External sandboxes for coding agents
link: https://simonwillison.net/2026/Nov/17/external-sandboxes-for-coding-agents/
source: simonwillison-net
published: 2026-11-17T22:52:29Z
updated: 2026-11-17T22:52:29Z
first_seen: 2026-11-18T01:14:15.957291087Z
labels:
- ai
- claude-code
- codex
- coding-agents
- generative-ai
- llms
- pi
- sandboxing
- security
- webassembly
summary: 'External sandboxes for coding agents I''ve been exploring new ways to sandbox code that is executed by agents, with a focus on making it safer to try out local models without worrying that a mistake or prompt injection attack might cause them to take damaging actions on a machine. I worked on this project with Prime Radiant, the AI research lab behind Superpowers and other projects. My post on the Prime Radiant blog introduces the external sandbox pattern, where code executed by the agent runs inside a dedicated sandbox while the agent itself lives outside of that sandbox. To demonstrate this pattern we''ve released three sandbox server implementations: external-sandbox-v86 runs a Node.js server which then uses v86 to run Alpine Linux inside WebAssembly. external-sandbox-container is a Python web server which runs the sandbox in Alpine Linux inside an Apple Containers instance. external-sandbox-quicksand is another Python web server, this time using Microsoft''s Quicksand sandboxing library, which bundles QEMU and Alpine Linux in a Python package! Each of these implementations offers the same localhost API. You can then configure Pi, Claude Code, or Codex to send API calls to that sandbox any time they want to execute code. The sandboxes also include an optional HTTP/HTTPS allow-list proxy, so you can control exactly which hosts your agent''s code is allowed to communicate with - plus a credential replacement mechanism to make authenticated API calls without sharing a secret with the agent. Here''s the UI you can use to reconfigure that proxy while the sandbox is still running: I continue to be fascinated by the challenge of safely running untrusted code. This external sandbox pattern provides a neat way to experiment with different approaches. Tags: sandboxing, security, ai, webassembly, generative-ai, llms, coding-agents, claude-code, codex, pi'
content: extracted
html: 2026-11-17-external-sandboxes-for-coding-agents.html
remote_preview:
  url: https://static.simonwillison.net/static/2026/sandbox-proxy.png
  alt: The network policy page with allowed destinations, the policy JSON editor and recent proxy activity
---

**[External sandboxes for coding agents](https://blog.primeradiant.com/external-sandboxes-for-coding-agents/)**. I've been exploring new ways to sandbox code that is executed by agents, with a focus on making it safer to try out local models without worrying that a mistake or prompt injection attack might cause them to take damaging actions on a machine.

I worked on this project with [Prime Radiant](https://primeradiant.com), the AI research lab behind Superpowers and [other projects](https://github.com/orgs/prime-radiant-inc/repositories).

[My post](https://blog.primeradiant.com/external-sandboxes-for-coding-agents/) on the Prime Radiant blog introduces the **external sandbox** pattern, where code executed by the agent runs inside a dedicated sandbox while the agent itself lives outside of that sandbox.

To demonstrate this pattern we've released three sandbox server implementations:

- [external-sandbox-v86](https://github.com/prime-radiant-inc/external-sandbox-v86) runs a Node.js server which then uses [v86](https://github.com/copy/v86) to run Alpine Linux inside WebAssembly.
- [external-sandbox-container](https://github.com/prime-radiant-inc/external-sandbox-container) is a Python web server which runs the sandbox in Alpine Linux inside an Apple Containers instance.
- [external-sandbox-quicksand](https://github.com/prime-radiant-inc/external-sandbox-quicksand) is another Python web server, this time using Microsoft's [Quicksand](https://github.com/microsoft/quicksand) sandboxing library, which bundles QEMU *and* Alpine Linux in a Python package!

Each of these implementations offers the same localhost API. You can then configure Pi, Claude Code, or Codex to send API calls to that sandbox any time they want to execute code.

The sandboxes also include an optional HTTP/HTTPS allow-list proxy, so you can control exactly which hosts your agent's code is allowed to communicate with - plus a credential replacement mechanism to make authenticated API calls without sharing a secret with the agent. Here's the UI you can use to reconfigure that proxy while the sandbox is still running:

![The network policy page with allowed destinations, the policy JSON editor and recent proxy activity](https://static.simonwillison.net/static/2026/sandbox-proxy.png)

I continue to be fascinated by the challenge of safely running untrusted code. This external sandbox pattern provides a neat way to experiment with different approaches.
