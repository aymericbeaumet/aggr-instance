---
title: Introducing Tool Retrieval in the Gemini API
link: https://blog.google/innovation-and-ai/technology/developers-tools/gemini-api-tool-retrieval/
source: blog-google
published: 2026-11-19T18:00:00Z
updated: 2026-11-19T18:00:00Z
first_seen: 2026-11-19T23:46:32.431702950Z
labels:
- ai
- developer tools
summary: Give your agents access to hundreds of tools while using fewer tokens with Tool Retrieval, now in preview in the Gemini API.
content: extracted
html: 2026-11-19-introducing-tool-retrieval-in-the-gemini-api.html
remote_preview:
  url: https://storage.googleapis.com/gweb-uniblog-publish-prod/images/Tool_retrieval_hero.width-1300.png
---

Tool Retrieval is now in preview in the Gemini Interactions API. Load tool parameter schemas on demand instead of sending your full tool library in every prompt,designed to reduce input tokens and keep tool selection accurate as your agent scales.

* * *

Lucia Loher

Product Manager, Gemini API

Nora Kassner

Research Scientist, Google DeepMind

* * *

![Gemini spark on a space background](https://storage.googleapis.com/gweb-uniblog-publish-prod/images/Tool_retrieval_hero.width-2200.format-webp.webp)

Today we're launching Tool Retrieval in the Gemini API, available in preview in the [Interactions API](https://aistudio.google.com/docs/interactions-overview).

When an agent connects to dozens or even hundreds of tools, sending every full parameter schema on every request adds up fast. It inflates input token counts, adds latency, and lowers tool-selection accuracy as the candidate list grows. Tool Retrieval lets your agent keep its full tool library on standby and load parameter schemas only when the current task needs them.

## How it works

To turn on Tool Retrieval, add a `tool_search` tool to your request and set `defer_loading: true` on the tools you want to hold back. The model still sees the name and description of every deferred tool, so it knows what's available, but the parameter schemas stay out of context. When the model needs a tool, it asks for it by name, the API loads the full schema, and the model calls the function, all inside the same request.

```py
from google import genai

client = genai.Client()

tools = [
   {"type": "tool_search"},
   {
       "type": "function",
       "name": "issue_refund",
       "description": "Refund an order to the original payment method.",
       "defer_loading": True,
       "parameters": {
           "type": "object",
           "properties": {
               "order_id": {"type": "string"},
               "amount": {"type": "number"},
           },
           "required": ["order_id", "amount"],
       },
   },
   # ... 50+ other deferred functions (leave frequent tools undeferred)
]

interaction = client.interactions.create(
   model="gemini-4-hydrogen",
   input="Order A-1042 arrived damaged. Please refund 40 dollars.",
   tools=tools,
)
```

Inside that single API call, the model searches for `issue_refund`, the API loads its schema, and the model emits the `function_call`. All three steps appear in `interaction.steps`, making it easy to inspect which tools the model considered when debugging tool names or descriptions:

```js
"steps": [
 {
   "type": "tool_search_call",
   "id": "tsc_7f2a",
   "arguments": {"function_names": ["issue_refund"]},
   "signature": "..."
 },
 {
   "type": "tool_search_result",
   "call_id": "tsc_7f2a",
   "result": [{"type": "function", "name": "issue_refund", "parameters": {...}}],
   "signature": "..."
 },
 {
   "type": "function_call",
   "id": "fc_91c3",
   "name": "issue_refund",
   "arguments": {"order_id": "A-1042", "amount": 40}
 }
]
```

From here it's normal function calling: your code runs the refund and sends back a `function_result`.

There's no index to build or host. Gemini runs the search on the server, so the only changes to your code are the new tool and a flag on the tools you want to defer. You can keep your most-used tools loaded up front and defer the long tail.

Tool Retrieval also plays well with prefix caching. Deferred schemas stay out of the start of the prompt, and the schemas the model loads are added later in the conversation, so the cached prefix stays the same from turn to turn.

Tool Retrieval also works with [remote MCP servers in the Gemini API](https://ai.google.dev/gemini-api/docs/function-calling#mcp). Set `defer_loading: true` on an `mcp_server` tool and every tool that server exposes is loaded on demand, so you can connect a large server without listing its tools one by one.

## How client-side retrieval works

If you maintain your own tool registry, embedding index, or per-user permission filter, declare the `tool_search` with `"execution": "client"` and define your search parameters. Client-side retrieval adds one round trip per search, because your app runs the lookup. Tools do not need to be declared upfront, so you can also expose new tools mid-conversation when a user connects an integration:

```py
tools = [{
    "type": "tool_search",
    "execution": "client",
    "name": "search_tools",
    "description": "Search the internal tool registry for functions.",
    "parameters": {
        "type": "object",
        "properties": {"query": {"type": "string"}},
        "required": ["query"],
    },
}]

# When the model emits a function_call for "search_tools",
# return matching {"type": "function", ...} schemas in function_result:
followup = client.interactions.create(
    model="gemini-4-hydrogen",
    previous_interaction_id=interaction.id,
    tools=tools,
    input=[{
        "type": "function_result",
        "name": "search_tools",
        "call_id": step.id,
        "result": my_registry.search(**step.arguments),
    }],
)
```

## How pricing and supported models work

Tool Retrieval has no separate charge. Input and output tokens are billed at the standard rates for the model you call. Turning on `tool_search` adds a small, fixed block of input tokens, and each schema the model loads is billed as input for the rest of the conversation, the same as a tool declared up front. You save tokens when the schemas you never load outweigh the ones you do, which is the usual case for large libraries. Measure it against your own traffic.

Tool Retrieval works with Gemini 3.6 and later models, including [all Gemini 4 models](https://aistudio.google.com/docs/latest-model), in the Interactions API.

## How to get started

- [**Read the documentation**](https://aistudio.google.com/docs/tool-retrieval) — Full setup guide with examples
- [**Compare Gemini models**](https://aistudio.google.com/docs/choosing-a-model?e=0) — Find the best model for your use case
- [**Join the discussion on Discord**](https://discuss.ai.google.dev/) — Share feedback with the team

## Get the latest news from Google in your inbox

Sign up for our newsletters with product updates, event information, special offers, and more.

Your information will be used in accordance with [Google's privacy policy.](https://policies.google.com/privacy) You may opt out at any time.
