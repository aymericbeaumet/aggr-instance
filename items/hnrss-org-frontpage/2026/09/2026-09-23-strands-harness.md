---
title: Strands Harness
link: https://strandsagents.com/blog/introducing-strands-harness/
source: hnrss-org-frontpage
published: 2026-09-23T15:05:46Z
updated: 2026-09-23T15:05:46Z
first_seen: 2026-09-23T19:00:13.987032799Z
authors:
- zuckerborg0101
summary: 'Article URL: https://strandsagents.com/blog/introducing-strands-harness/ Comments URL: https://news.ycombinator.com/item?id=49817289 Points: 104 # Comments: 80'
content: extracted
html: 2026-09-23-strands-harness.html
preview:
  file: 2026-09-23-strands-harness.preview-07fef805ad94.webp
  width: 256
  height: 134
  color: '#1f2120'
images:
- source: https://strandsagents.com/blog/og/introducing-strands-harness.png
  original:
    file: 2026-09-23-strands-harness.image-b5d82a546714.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-23-strands-harness.image-c74c0b83d8cf.webp
    width: 320
    height: 168
  - file: 2026-09-23-strands-harness.image-37ef08472cb7.webp
    width: 640
    height: 336
  - file: 2026-09-23-strands-harness.image-ebd30595db99.webp
    width: 1200
    height: 630
  color: '#0e0e0e'
---

We noticed builders often wished their Claude Code or Codex setup could run in the cloud because locally their agent idea just “worked” with those harnesses.

But the moment you build your own agent, you’re on your own. It’s tricky wiring up the right primitives just well enough to match that “it just worked” feeling.

Today we’re releasing [Strands harness](https://github.com/strands-agents/harness-sdk/tree/main): a fully assembled state-of-the-art agent harness you can easily run locally or deploy to your favorite provider. It’s built to be a general-purpose agent rather than a coding agent. All you need is one line of Python or TypeScript to get it working with your choice of model. Strands harness beats other agent harnesses on cost efficiency while maintaining equal or better accuracy. It’s available under an Apache 2.0 license.

Strands harness **costs 28% less** when using the same Claude or GPT models across six benchmarks. We found Strands harness showed better token-efficiency and nearly equal benchmark scores compared to Claude Code, Codex, and other popular harnesses. That’s important because we never want to sacrifice accuracy for lower cost. Deepseek Harness proved to be the most token-efficient overall, however, it typically reported the lowest accuracy scores. Our testing setup was distributed benchmarking on EC2 with Harbor.

With Fable 5, Strands harness **cost 77% less** than Claude Code *and* **scored higher** on Terminal Bench 2.1. We’re also pleased to see two other open source harnesses perform similarly well on cost/accuracy against Claude Code.

Strands harness, built on the [Strands Harness SDK](https://github.com/strands-agents/harness-sdk), contains defaults for prompt caching and context management. Our default context management largely drove the token-efficiency and accuracy: tool results over ~1500 tokens get truncated, summarization (compaction) triggers when the context window hits above 85%, and context recovery runs within the loop if there’s an overflow.

Keep an eye out for a follow-up paper from our researchers regarding these benchmarks. In the meantime, we’re excited everyone gets to use these defaults in Strands harness.

## Easy to get started

Strands harness runs on the latest models across Amazon Bedrock, Anthropic, OpenAI, and Google. Pick one by name, or point it at a local Ollama model:

- [Amazon Bedrock](https://strandsagents.com/blog/introducing-strands-harness/#tab-panel-6-0)
- [Anthropic](https://strandsagents.com/blog/introducing-strands-harness/#tab-panel-6-1)
- [OpenAI](https://strandsagents.com/blog/introducing-strands-harness/#tab-panel-6-2)
- [Google](https://strandsagents.com/blog/introducing-strands-harness/#tab-panel-6-3)
- [Ollama](https://strandsagents.com/blog/introducing-strands-harness/#tab-panel-6-4)
- [LiteLLM](https://strandsagents.com/blog/introducing-strands-harness/#tab-panel-6-5)

- [Python](https://strandsagents.com/blog/introducing-strands-harness/#tab-panel-5-0)
- [TypeScript](https://strandsagents.com/blog/introducing-strands-harness/#tab-panel-5-1)

```
from strands_harness import create_harnessagent = create_harness(model="bedrock/global.anthropic.claude-opus-5")agent("Research the top three vector databases, compare pricing and limits, and write it up in comparison.md")
```

Out of the box, `create_harness()` returns an agent that:

- Runs on a current reasoning model across Amazon Bedrock, Anthropic, OpenAI, Google, Ollama, or LiteLLM.
- Ships with shell, file (`read` / `write` / `edit`), and web tools: the primitives a model already knows how to use, rather than a bespoke tool per task.
- Manages its own context window. It offloads bulky tool results to files and caches the reused parts of each request to save time and cost.
- Keeps long-term memory across runs, and resumes an earlier conversation when you give it a session ID.
- Delegates open-ended subtasks to a built-in helper agent, and tracks multi-step work with a checklist.
- Loads skills if they exist.

Strands harness can be deployed on any provider with a linux container, such as Modal, Cloudflare Containers, Azure Container Apps, Google Cloud Run, Amazon ECS, and Amazon Bedrock AgentCore.

We also have the Strands CLI that allows you to prototype your agent in plain English. Wire up your model provider of choice, then add prompts and tools to watch your agent come to life. Afterwards, you can run `/export` in the Strands CLI to get a copy of the code in either TypeScript or Python, a very convenient way to keep iterating on Strands harness with your preferred coding agent.

Your browser does not support the video tag.

Here we ask the Strands harness agent to “add the Playwright MCP” and then measure latency when loading a video on a blog post. After seeing the MCP tools accurately work, we simply run `/export` to get the Strands harness code. You can see the Playwright MCP got added to the boilerplate, making it easy to iterate with a coding agent or adding config to deploy.

## What you can build from Strands harness

The Strands CLI is actually built on top of Strands harness. We unlocked a bunch of ambitious ideas because of how easy it is to prototype any agent. Recently our engineer, Gautam Sirdeshmukh, inspired by agent platforms like Grokbot and Muse, built a desktop app that kicks off Strands harness remotely.

Your browser does not support the video tag.

When you’re ready to go deeper, Strands harness is fully customizable. You can easily override any default, swap models, add tools, or gradually replace components all the way down to the Strands Harness SDK. The code is yours.

We’re excited for all kinds of agents that’ll come to life with Strands harness.

## Try it out today

You can easily install [Strands harness](https://strandsagents.com/docs/user-guide/harness/) with `pip install strands-harness` for Python or `npm install @strands-agents/harness` for TypeScript. For an interactive experience, download the Strands CLI: `npm install -g @strands-agents/cli`.

We believe quickly prototyping with a batteries included harness will create more useful agents. Our team is also in [Discord](https://discord.gg/strands), so come say hi or ask us any question about agents!
