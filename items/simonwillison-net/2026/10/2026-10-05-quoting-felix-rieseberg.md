---
title: Quoting Felix Rieseberg
link: https://simonwillison.net/2026/Oct/5/felix-rieseberg/
source: simonwillison-net
published: 2026-10-05T23:56:47Z
updated: 2026-10-05T23:56:47Z
first_seen: 2026-10-06T00:45:55.302474734Z
labels:
- ai
- anthropic
- claude
- claude-cowork
- general-agents
- generative-ai
- llms
summary: 'The "old" version of Cowork runs model inference in the cloud, executing tool calls in an Anthropic-provided VM we shipped to your computer. We added the VM for capability, safety, and security reasons - mapping in just the data you explicitly added to your session. People loved what they were able to do with Claude but didn''t love the disk, battery, and performance cost of running the VM locally. Also, people didn''t love that closing your laptop means the work stops. The "new" version of Cowork runs model inference and the VM in the cloud. Each session gets its own sandbox, not sharing state with other sessions. When the VM needs something on the users'' device (like a file), the desktop app is responsible for that file access tool call. [...] We think this solves a lot of problems we''ve heard about (like using Cowork from a phone, keeping work running, or getting all the same power without losing battery to the VM) — Felix Rieseberg, Anthropic, see also this help page Tags: claude-cowork, anthropic, claude, generative-ai, ai, general-agents, llms'
content: extracted
html: 2026-10-05-quoting-felix-rieseberg.html
---

> The "old" version of Cowork runs model inference in the cloud, executing tool calls in an Anthropic-provided VM we shipped to your computer. We added the VM for capability, safety, and security reasons - mapping in just the data you explicitly added to your session. People loved what they were able to do with Claude but didn't love the disk, battery, and performance cost of running the VM locally. Also, people didn't love that closing your laptop means the work stops.
>
> The "new" version of Cowork runs model inference and the VM in the cloud. Each session gets its own sandbox, not sharing state with other sessions. When the VM needs something on the users' device (like a file), the desktop app is responsible for that file access tool call. \[...\]
>
> We think this solves a lot of problems we've heard about (like using Cowork from a phone, keeping work running, or getting all the same power without losing battery to the VM)

— [Felix Rieseberg](https://twitter.com/felixrieseberg/status/2107206431376334975), Anthropic, see also [this help page](https://support.claude.com/en/articles/15520349-use-claude-cowork-on-web-desktop-and-mobile#h_f951c27c48)
