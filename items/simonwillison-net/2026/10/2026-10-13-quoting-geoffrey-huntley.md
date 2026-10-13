---
title: Quoting Geoffrey Huntley
link: https://simonwillison.net/2026/Oct/13/geoffrey-huntley/
source: simonwillison-net
published: 2026-10-13T13:32:00Z
updated: 2026-10-13T13:32:00Z
first_seen: 2026-10-13T18:08:43.541012300Z
labels:
- agentic-engineering
- ai
- coding-agents
- generative-ai
- geoffrey-huntley
- git
- llms
summary: 'If the agent isn''t using my diagnostics library, I could introspect and update AGENTS.md, but there''s still a delta, and it''ll drift. Instead, I write a pre-commit hook that checks the git diff for direct calls to observability modules that bypass my diagnostics module. Boom: fail the commit, inject a prompt telling the agent to use the diagnostics library. All of a sudden it''s ten out of ten. — Geoffrey Huntley, nobody should type code by hand anymore. here''s what''s left. Tags: coding-agents, generative-ai, git, agentic-engineering, ai, geoffrey-huntley, llms'
content: feed
html: 2026-10-13-quoting-geoffrey-huntley.html
---

> If the agent isn't using my diagnostics library, I could introspect and update AGENTS.md, but there's still a delta, and it'll drift. Instead, I write a pre-commit hook that checks the git diff for direct calls to observability modules that bypass my diagnostics module. Boom: fail the commit, inject a prompt telling the agent to use the diagnostics library. All of a sudden it's ten out of ten.

— [Geoffrey Huntley](https://ghuntley.com/agency/), nobody should type code by hand anymore. here's what's left.

Tags: [coding-agents](https://simonwillison.net/tags/coding-agents), [generative-ai](https://simonwillison.net/tags/generative-ai), [git](https://simonwillison.net/tags/git), [agentic-engineering](https://simonwillison.net/tags/agentic-engineering), [ai](https://simonwillison.net/tags/ai), [geoffrey-huntley](https://simonwillison.net/tags/geoffrey-huntley), [llms](https://simonwillison.net/tags/llms)
