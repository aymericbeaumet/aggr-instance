---
title: Quoting Prompting Claude Fable 5.2
link: https://simonwillison.net/2026/Nov/10/prompting-claude-fable-52/
source: simonwillison-net
published: 2026-11-10T05:34:14Z
updated: 2026-11-10T05:34:14Z
first_seen: 2026-11-10T11:40:21.505980627Z
labels:
- ai
- anthropic
- claude-mythos-fable
- generative-ai
- llms
- prompt-engineering
summary: 'Claude Fable 5.2 responds closely to what the prompt says about who owns the code it works on. In Anthropic’s testing on small coding tasks, when a prompt said nothing about ownership or presented the code base as someone else’s, the model tended to add to the existing structure, even when that repeated existing code. [...] If you want the model to improve the structure as it works, say that the code is its own: This codebase is yours: what it''s for and how it''s built are your call, and you''ll keep building on it. In Anthropic’s testing on small coding tasks, the model merged repeated code in about half of the tries with this sentence, and in under a tenth of the tries without it. — Prompting Claude Fable 5.2, Anthropic documentation Tags: prompt-engineering, anthropic, generative-ai, ai, llms, claude-mythos-fable'
content: extracted
html: 2026-11-10-quoting-prompting-claude-fable-5-2.html
---

> Claude Fable 5.2 responds closely to what the prompt says about who owns the code it works on. In Anthropic’s testing on small coding tasks, when a prompt said nothing about ownership or presented the code base as someone else’s, the model tended to add to the existing structure, even when that repeated existing code. \[...\]
>
> If you want the model to improve the structure as it works, say that the code is its own:
>
> > `This codebase is yours: what it's for and how it's built are your call, and you'll keep building on it.`
>
> In Anthropic’s testing on small coding tasks, the model merged repeated code in about half of the tries with this sentence, and in under a tenth of the tries without it.

— [Prompting Claude Fable 5.2](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/prompting-claude-fable-5-2#say-who-owns-the-code), Anthropic documentation
