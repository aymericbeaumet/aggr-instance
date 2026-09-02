---
title: ChatGPT search now uses the site:operator at scale
link: https://simonwillison.net/2026/Aug/20/chatgpt-search-now-uses-the-siteoperator-at-scale/
source: simon-willison
published: 2026-08-20T23:57:32Z
updated: 2026-08-20T23:57:32Z
first_seen: 2026-09-02T14:07:29.769486161Z
tags:
- reddit
- seo
- openai
- chatgpt
- ai-assisted-search
- system-prompts
summary: 'ChatGPT search now uses the site:operator at scale Promptwatch is part of the emerging "GEO" space, for Generative Engine Optimization - the chatbot version of SEO, where companies offer tools and consulting to help your site increase its presence in replies to prompts inside tools like ChatGPT. The Promptwatch product uses automation to track responses to prompts across end-user chat products like ChatGPT, Claude, and Gemini. They publish aggregate reports on this as part of their own content marketing strategy, which do seem to provide credible hints as to otherwise invisible design changes to those products. Their own tracking shows a notable change aligned with the GPT-5.6 rollout earlier this month: The percentage of all ChatGPT Search fanout queries that contain the site:operator, per day. The share hovered between 0.3% and 0.5% for weeks, dipped briefly to 0.15% on August 3 to 5 (consistent with a staged rollout or pre-launch experiment), then jumped to 16-17% on August 8. It''s important to note that these figures only reflect the prompts for which they have automated tracking enabled. This corresponds to OpenAI''s somewhat vague August 6th announcement: For Plus and Pro users, we’re updating GPT‑5.6 Sol in Chat to be more reliable with facts and provide more focused answers. Once again I am hampered by OpenAI''s decision to actively obscure their system prompts, but from poking at ChatGPT I believe their latest search tool has a shape like search(query, recency, domains) rather than encouraging a site: operator directly. In a follow-up on August 18th Promptwatch reported that ChatGPT appeared to have greatly reduced the likelihood of Reddit being used in those searches. My own attempts to ascertain if the system prompt has been updated to discourage Reddit sourcing have been unsuccessful - the most thorough leaked system prompt collection I know of doesn''t yet show any relevant changes. Tags: reddit, seo, openai, chatgpt, ai-assisted-search, system-prompts'
content: feed
html: 2026-08-20-chatgpt-search-now-uses-the-site-operator-at-scale.html
---

**[ChatGPT search now uses the site:operator at scale](https://promptwatch.com/data/chatgpt-site-operator-fanouts)**

Promptwatch is part of the emerging "GEO" space, for Generative Engine Optimization - the chatbot version of SEO, where companies offer tools and consulting to help your site increase its presence in replies to prompts inside tools like ChatGPT.

The Promptwatch product uses automation to track responses to prompts across end-user chat products like ChatGPT, Claude, and Gemini. They publish aggregate reports on this as part of their own content marketing strategy, which do seem to provide credible hints as to otherwise invisible design changes to those products.

Their own tracking shows a notable change aligned with the GPT-5.6 rollout earlier this month:

> The percentage of all ChatGPT Search fanout queries that contain the site:operator, per day. The share hovered between 0.3% and 0.5% for weeks, dipped briefly to 0.15% on August 3 to 5 (consistent with a staged rollout or pre-launch experiment), then jumped to 16-17% on August 8.

It's important to note that these figures only reflect the prompts for which they have automated tracking enabled.

This corresponds to OpenAI's somewhat vague [August 6th announcement](https://openai.com/index/improving-gpt-5-6-sol-in-chatgpt/):

> For Plus and Pro users, we’re updating GPT‑5.6 Sol in Chat to be more reliable with facts and provide more focused answers.

Once again I am hampered by OpenAI's decision to actively obscure their system prompts, but from poking at ChatGPT I believe their latest search tool has a shape like `search(query, recency, domains)` rather than encouraging a `site:` operator directly.

In [a follow-up](https://promptwatch.com/data/reddit-citations-are-dropping-in-chatgpt) on August 18th Promptwatch reported that ChatGPT appeared to have greatly reduced the likelihood of Reddit being used in those searches. My own attempts to ascertain if the system prompt has been updated to discourage Reddit sourcing have been unsuccessful - the [most thorough leaked system prompt](https://github.com/asgeirtj/system_prompts_leaks/commits/main/OpenAI) collection I know of doesn't yet show any relevant changes.

Tags: [reddit](https://simonwillison.net/tags/reddit), [seo](https://simonwillison.net/tags/seo), [openai](https://simonwillison.net/tags/openai), [chatgpt](https://simonwillison.net/tags/chatgpt), [ai-assisted-search](https://simonwillison.net/tags/ai-assisted-search), [system-prompts](https://simonwillison.net/tags/system-prompts)
