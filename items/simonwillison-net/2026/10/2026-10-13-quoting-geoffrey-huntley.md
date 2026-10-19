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
content: extracted
html: 2026-10-13-quoting-geoffrey-huntley.html
---

> If the agent isn't using my diagnostics library, I could introspect and update AGENTS.md, but there's still a delta, and it'll drift. Instead, I write a pre-commit hook that checks the git diff for direct calls to observability modules that bypass my diagnostics module. Boom: fail the commit, inject a prompt telling the agent to use the diagnostics library. All of a sudden it's ten out of ten.

— [Geoffrey Huntley](https://ghuntley.com/agency/), nobody should type code by hand anymore. here's what's left.

Posted [13th October 2026](https://simonwillison.net/2026/Oct/13/) at 1:32 pm

## Recent articles

- [Hello Python 3.15, goodbye 3.10, and now I can use 3.11](https://simonwillison.net/2026/Oct/12/python-3-15/) - 12th October 2026
- [A new feature for my blog, built using my voice](https://simonwillison.net/2026/Oct/9/built-using-my-voice/) - 9th October 2026
- [Claude Haiku 5.5](https://simonwillison.net/2026/Oct/7/claude-haiku-5-5/) - 7th October 2026
- [We're going to need default hard budget caps on pretty much everything](https://simonwillison.net/2026/Oct/3/default-hard-budget-caps/) - 3rd October 2026

This is a **quotation** collected by Simon Willison, posted on [13th October 2026](https://simonwillison.net/2026/Oct/13/).

[git 58](https://simonwillison.net/tags/git/) [ai 2,285](https://simonwillison.net/tags/ai/) [generative-ai 2,027](https://simonwillison.net/tags/generative-ai/) [llms 1,993](https://simonwillison.net/tags/llms/) [coding-agents 259](https://simonwillison.net/tags/coding-agents/) [geoffrey-huntley 4](https://simonwillison.net/tags/geoffrey-huntley/) [agentic-engineering 67](https://simonwillison.net/tags/agentic-engineering/)
