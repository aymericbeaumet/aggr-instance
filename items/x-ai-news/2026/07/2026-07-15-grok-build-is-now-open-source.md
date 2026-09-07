---
title: Grok Build is Now Open Source
link: https://x.ai/news/grok-build-open-source
source: x-ai-news
published: 2026-07-15T00:00:00Z
first_seen: 2026-09-07T17:03:44.343711062Z
summary: Explore the harness behind our coding agent and TUI.
content: extracted
html: 2026-07-15-grok-build-is-now-open-source.html
preview:
  file: 2026-07-15-grok-build-is-now-open-source.preview-60fa872b7ee8.webp
  width: 256
  height: 134
  color: '#141414'
images:
- source: https://media.x.ai/v1/website/open-source-cover-930d1067.png
  original:
    file: 2026-07-15-grok-build-is-now-open-source.image-1dc9c0d5c2d9.png
    width: 1200
    height: 630
  variants:
  - file: 2026-07-15-grok-build-is-now-open-source.image-9fd22060542d.webp
    width: 48
    height: 25
  - file: 2026-07-15-grok-build-is-now-open-source.image-269af5cbcd75.webp
    width: 320
    height: 168
  - file: 2026-07-15-grok-build-is-now-open-source.image-b74b27e0f301.webp
    width: 640
    height: 336
  - file: 2026-07-15-grok-build-is-now-open-source.image-c05cd7d2bc85.webp
    width: 960
    height: 504
  - file: 2026-07-15-grok-build-is-now-open-source.image-748bb26bbd46.webp
    width: 1200
    height: 630
  color: '#070707'
---

We're open-sourcing Grok Build, SpaceXAI's coding agent and TUI. The source is now available on [GitHub](https://github.com/xai-org/grok-build).

Publishing the code is the most direct way to build toward a robust and reliable harness. You can read the source to see exactly how it works, from context assembly to tool-call dispatch.

Open-sourcing also makes the harness easier to explore and extend: if you're working with skills, plugins, hooks, MCP servers, or subagents, the source is the definitive reference for how each is loaded and invoked.

Finally, Grok Build can now run fully local-first: compile it yourself, point it at your own local inference, and drive everything from your `config.toml`.

## [About the codebase](https://x.ai/news/grok-build-open-source#about-the-codebase)

The published source includes:

- The agent loop: how context is assembled, how model responses are parsed, and how tool calls are dispatched
- The tools: how the agent reads, edits, and searches code, and how it runs commands
- The terminal UI: rendering, input handling, plan review, and the inline diff viewer
- The extension system: skills, plugins, hooks, MCP servers, and subagents

Explore the source on [GitHub](https://github.com/xai-org/grok-build).
