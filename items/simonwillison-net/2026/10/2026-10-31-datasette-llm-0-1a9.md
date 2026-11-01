---
title: datasette-llm 0.1a9
link: https://simonwillison.net/2026/Oct/31/datasette-llm/
source: simonwillison-net
published: 2026-10-31T22:10:01Z
updated: 2026-10-31T22:10:01Z
first_seen: 2026-11-01T02:06:10.004030692Z
labels:
- datasette
- datasette-agent
- llm
summary: 'Release: datasette-llm 0.1a9 I have some big upgrades to Datasette Agent in the pipeline, but in the meantime here''s a tiny user experience improvement to the underlying datasette-llm plugin. It now picks up the default model used by LLM, which means you can start using Datasette Agent without needing to explicitly configure a default model: uvx --with datasette-agent datasette content.db --internal internal.db Previously you would also need to specify a model with -s plugins.datasette-agent.default_model gpt-6-astra. Tags: datasette, llm, datasette-agent'
content: extracted
html: 2026-10-31-datasette-llm-0-1a9.html
---

I have some big upgrades to [Datasette Agent](https://agent.datasette.io/) in the pipeline, but in the meantime here's a tiny user experience improvement to the underlying `datasette-llm` plugin.

It now picks up the default model used by [LLM](https://llm.datasette.io/), which means you can start using Datasette Agent without needing to explicitly configure a default model:

```
uvx --with datasette-agent datasette content.db --internal internal.db
```

Previously you would also need to specify a model with `-s plugins.datasette-agent.default_model gpt-6-astra`.
