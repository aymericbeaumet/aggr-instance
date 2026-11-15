---
title: datasette-llm 0.1a11
link: https://simonwillison.net/2026/Nov/14/datasette-llm/
source: simonwillison-net
published: 2026-11-14T23:59:28Z
updated: 2026-11-14T23:59:28Z
first_seen: 2026-11-15T07:05:25.029264630Z
labels:
- ai
- datasette
- datasette-agent
- generative-ai
- llm
- llms
- uv
summary: 'Release: datasette-llm 0.1a11 datasette-llm is the intermediate plugin for Datasette that supports configuring LLMs for use with other Datasette plugins. This release adds support for OpenAI-compatible endpoints, so you can configure Datasette to use an LLM hosted anywhere that talks the OpenAI pseudo-standard web protocol. This means that if you''re running local models with LM Studio or Ollama or llama.cpp (which just hit v1.0.0) you can run Datasette via uvx like this: wget https://datasette.io/content.db uvx --with datasette-agent datasette \ --root --secret 1 \ -s plugins.datasette-llm.default_model ''{ "model": "lmstudio-qwen", "model_name": "qwen/qwen3.8-27b", "api_base": "http://localhost:1234/v1" }'' content.db And you''ll get a local server running a Datasette Agent instance powered by your local model! Tags: uv, datasette-agent, llm, datasette, llms, ai, generative-ai'
content: extracted
html: 2026-11-14-datasette-llm-0-1a11.html
remote_preview:
  url: https://static.simonwillison.net/static/2026/lmstudio-datasette-agent.webp
  alt: Screenshot of a chat interface. Header bar reads "home" on the left and "root" with a hamburger menu icon on the right. Below is a back arrow, the heading "Chat", a model label "lmstudio-qwen" and an "EXPORT" button. A blue user message bubble says "how many repos and how many releases are there?" B
---

`datasette-llm` is the intermediate plugin for Datasette that supports configuring LLMs for use with other Datasette plugins.

This release adds support for OpenAI-compatible endpoints, so you can configure Datasette to use an LLM hosted anywhere that talks the OpenAI pseudo-standard web protocol.

This means that if you're running local models with [LM Studio](https://lmstudio.ai) or [Ollama](https://ollama.com) or [llama.cpp](https://github.com/ggml-org/llama.cpp) (which just hit [v1.0.0](https://huggingface.co/blog/ggml-org/llama-cpp-v1)) you can run Datasette via `uvx` like this:

```shell
wget https://datasette.io/content.db

uvx --with datasette-agent datasette \
  --root --secret 1 \
  -s plugins.datasette-llm.default_model '{
    "model": "lmstudio-qwen",
    "model_name": "qwen/qwen3.8-27b",
    "api_base": "http://localhost:1234/v1"
  }' content.db
```

And you'll get a local server running a Datasette Agent instance powered by your local model!

![Screenshot of a chat interface. Header bar reads \"home\" on the left and \"root\" with a hamburger menu icon on the right. Below is a back arrow, the heading \"Chat\", a model label \"lmstudio-qwen\" and an \"EXPORT\" button. A blue user message bubble says \"how many repos and how many releases are there?\" Below it is a collapsed tool call \"list_databases_and_tables\" and an expanded tool call \"sql_query\". Its REQUEST section shows database \"content\", display \"both\" and the SQL: SELECT (SELECT COUNT(*) FROM repos) AS repo_count, (SELECT COUNT(*) FROM releases) AS release_count. Its RESPONSE section shows columns repo_count and release_count with a single row of 200 and 1889. Below that is a table with columns repo_count and release_count and one row: 200, 1889, followed by a \"View SQL query\" button. The assistant reply reads \"There are 200 repos and 1,889 releases in the content database.\" At the bottom is a text input with placeholder \"Type a message... (# to tag a table or query)\" and a blue \"Send\" button.](https://static.simonwillison.net/static/2026/lmstudio-datasette-agent.webp)
