---
title: MCP was always a bad idea?
link: https://maharship.com/blog/why-mcp-was-always-a-bad-idea/
source: hnrss-org-frontpage
published: 2026-09-20T19:44:40Z
updated: 2026-09-20T19:44:40Z
first_seen: 2026-09-21T11:04:21.981356257Z
authors:
- maharshi365
summary: 'Article URL: https://maharship.com/blog/why-mcp-was-always-a-bad-idea/ Comments URL: https://news.ycombinator.com/item?id=49779329 Points: 182 # Comments: 133'
content: extracted
html: 2026-09-20-mcp-was-always-a-bad-idea.html
preview:
  file: 2026-09-20-mcp-was-always-a-bad-idea.preview-0724a8a28969.webp
  width: 256
  height: 134
  color: '#262732'
images:
- source: https://maharship.com/og-image.png
  original:
    file: 2026-09-20-mcp-was-always-a-bad-idea.image-e401c358ecfa.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-20-mcp-was-always-a-bad-idea.image-c78a7a5abafb.webp
    width: 320
    height: 168
  - file: 2026-09-20-mcp-was-always-a-bad-idea.image-3edad9b157ad.webp
    width: 640
    height: 336
  - file: 2026-09-20-mcp-was-always-a-bad-idea.image-76bce7c80f68.webp
    width: 960
    height: 504
  - file: 2026-09-20-mcp-was-always-a-bad-idea.image-f1fb14bfaeae.webp
    width: 1200
    height: 630
  color: '#1a1b26'
- source: https://maharship.com/_astro/im-tired.76t17z_Q_Z2kioeJ.webp
  original:
    file: 2026-09-20-mcp-was-always-a-bad-idea.image-b093705c5561.webp
    width: 500
    height: 524
  color: '#03090c'
---

## Why MCP Was Always a Bad Idea

Recently I went to an all-day event centered around the latest and greatest in the MCP world. While all of the presenters were awesome and seemed to be passionate about the work they were doing, I’m honestly tired of MCP. It’s a horrible protocol built for a time when LLMs weren’t that smart, and we’ve outgrown it.

![Let me get this straight, you think MCP is a bad idea? I do, and I’m tired of pretending it’s not.](https://maharship.com/_astro/im-tired.76t17z_Q_Z2kioeJ.webp)

## A Brief History

MCP was released in November 2024 by the Anthropic team as a protocol designed to help agents connect to external services and data sources.[^1] The models of the time were still relatively primitive, at least compared to what we have right now. We didn’t even have Claude Code back then, and general-purpose agentic workflows were far less reliable.

Users started to see the usefulness of giving their AI models access to external services. It enabled a level of productivity that we hadn’t seen before. We saw an explosion in MCP adoption, coinciding with a similar, if not more explosive, growth in LLM adoption across the economy.

Over time, MCP continued to evolve under Anthropic’s stewardship before it was eventually donated to the Agentic AI Foundation, under the Linux Foundation, in 2025.[^2]

## The MCP Industrial Complex

With the huge growth in adoption, users started to add many MCP servers to their setups, and they started running into the context bloat issue. Each server would come with multiple tools, each with its own schema, which started to overload the context of all of these models. Harness developers found many tricks around this, including generic search/execute patterns now offered by platforms like Composio, MintMCP, and Pipedream. They all effectively solve the problem of having one place to put your credentials for the various external services and give your agent a minimal set of tools (to reduce context bloat) that it can use to access them. I want to make clear that this is a good thing, *for the short term*.

With all the stuff we’ve built around MCP, what we didn’t take into account, or maybe have ignored, is the models getting better. We now have whole systems dedicated to monitoring MCP servers, making sure the responses are good, making sure that agents are able to easily access the tools, figuring out schemas, and determining what we need to give agents so that they can make the right call at the right time.

## Surprise, Surprise, the Big Labs Were Right

The models got better. They are now able to execute code on a computer, reason about large codebases, and generally act much more autonomously than ever before. A big part of that work was writing/running scripts for coding purposes. A side effect (though is it?) is that now they are good at calling APIs directly. They can write scripts, compose multiple different services, and call APIs they haven’t seen before, all in useful workflows with minimal intervention from the user side.

LLMs have gotten so good at this, Cloudflare even launched Code Mode, a better way to use MCP by having LLMs compose the various calls into scripts that can be executed in a sandbox.[^3]

But even better than that, the LLMs have figured out how to use the `--help` command to discover CLIs, so they no longer need MCP servers to access many services available through documented APIs or CLIs. Most remote-service MCP servers ultimately wrap APIs that already exist.

## What Now?

We delete most of our MCP servers. That’s it. Agents with terminal access can replace most MCP servers and often are more capable There are still some issues, like CLIs returning machine-readable responses (JSON/XML, etc.), which tend to be very verbose and heavy on token usage, but we have ways to fix this.

Much of the alternative already exists: documented HTTP APIs, standard content negotiation, and mature authentication mechanisms.

We should start to standardize how agents use HTTP APIs directly. For example, agent clients could attach headers to identify themselves as agents, and servers could automatically send them response data as Markdown or text instead of HTML or verbose JSON.

### Some Real Examples

1. The Accept Markdown Header

   A growing number of LLM-friendly servers, especially text-heavy sites like documentation sites, honor the `Accept: text/markdown` header. These servers can automatically send a rendered Markdown file instead of the HTML response they would usually send. The media type itself is standardized, and using it for agent-oriented content negotiation is gaining adoption.

2. Documentation Sites Using the Accept-Language Header

   Recently, a Vercel engineer called on harnesses to send the programming language the client prefers, so documentation sites can serve more specific examples. For example, adding Python could prioritize docs for the Python SDK instead of sending something generic. Tobi Lutke of Shopify liked it so much that it now ships in Shopify docs.

[Malte Ubl (@cramforce): Request to harnesses: I love that you now send “Accept: text/markdown”. Next thing is: Put the programming language you prefer into the Accept-Language header.](https://x.com/cramforce/status/2096609086649647324)

[Tobi Lutke (@tobi): Great idea. Will support this on Shopify docs.](https://x.com/tobi/status/2097775334254907414)

## Closing Thoughts

Standardizing around common protocols grew the internet into what it is today. MCP is now a protocol of a bygone era. Agents are smart, capable of writing scripts and asking for exactly what they want. Instead of continuing down the rabbit hole of MCP, I say it’s time to end-of-life it and rely directly on HTTP APIs and CLIs where they already provide the necessary interface.

[^1]: Anthropic, [“Introducing the Model Context Protocol”](https://www.anthropic.com/news/model-context-protocol), November 25, 2024.

[^2]: Anthropic, [“Donating the Model Context Protocol and establishing the Agentic AI Foundation”](https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation), December 9, 2025.

[^3]: Kenton Varda and Sunil Pai, [“Code Mode: the better way to use MCP”](https://blog.cloudflare.com/code-mode/), Cloudflare Blog, September 26, 2025.
