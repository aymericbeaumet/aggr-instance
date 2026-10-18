---
title: llm-anthropic 0.30
link: https://simonwillison.net/2026/Sep/28/llm-anthropic/
source: simonwillison-net
published: 2026-09-28T23:06:21Z
updated: 2026-09-28T23:06:21Z
first_seen: 2026-10-05T18:49:38.624325496Z
labels:
- anthropic
- llm
summary: 'Release: llm-anthropic 0.30 In addition to Claude Sonnet 5.5, this release adds the ability to run llm anthropic refresh to refresh the list of Anthropic models directly from their API - which means I don''t need to push a new release just to add support for a newly released model. I also added an llm anthropic count command which can use their free token counting API to return a count of tokens that will be used by any prompt, before you send that prompt. Tags: llm, anthropic'
content: extracted
html: 2026-09-28-llm-anthropic-0-30.html
---

[llm-anthropic 0.30](https://github.com/simonw/llm-anthropic/releases/tag/0.30) — LLM access to models by Anthropic, including the Claude series

In addition to Claude Sonnet 5.5, this release adds the ability to run `llm anthropic refresh` to refresh the list of Anthropic models directly from their API - which means I don't need to push a new release just to add support for a newly released model.

I also added an `llm anthropic count` command which can use their free token counting API to return a count of tokens that will be used by any prompt, before you send that prompt.

Posted [28th September 2026](https://simonwillison.net/2026/Sep/28/) at 11:06 pm
