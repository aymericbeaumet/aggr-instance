---
title: OpenAI Agents API
link: https://developers.openai.com/api/docs/guides/agents-api/overview
source: hnrss-org-frontpage
published: 2026-09-10T19:43:22Z
updated: 2026-09-10T19:43:22Z
first_seen: 2026-09-11T05:07:40.995277925Z
authors:
- aquir
summary: 'Article URL: https://developers.openai.com/api/docs/guides/agents-api/overview Comments URL: https://news.ycombinator.com/item?id=49649213 Points: 188 # Comments: 118'
content: extracted
html: 2026-09-10-openai-agents-api.html
preview:
  file: 2026-09-10-openai-agents-api.preview-5dca813f86ba.webp
  width: 256
  height: 134
  alt: Agents API | OpenAI API
  color: '#f58583'
images:
- source: https://developers.openai.com/og/api/docs/guides/agents-api/overview.png
  original:
    file: 2026-09-10-openai-agents-api.image-aec5eb22aa53.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-10-openai-agents-api.image-3200e9b2b36e.webp
    width: 48
    height: 25
  - file: 2026-09-10-openai-agents-api.image-af53edf30aad.webp
    width: 320
    height: 168
  - file: 2026-09-10-openai-agents-api.image-6d98857db62b.webp
    width: 640
    height: 336
  - file: 2026-09-10-openai-agents-api.image-ebd20805461e.webp
    width: 960
    height: 504
  - file: 2026-09-10-openai-agents-api.image-7ccdfa4f4d49.webp
    width: 1200
    height: 630
  color: '#f58877'
- source: https://developers.openai.com/images/api/agents-api/overview-1.webp
  original:
    file: 2026-09-10-openai-agents-api.image-dbea9153f217.webp
    width: 1400
    height: 552
  variants:
  - file: 2026-09-10-openai-agents-api.image-3d3d0acc1abf.webp
    width: 48
    height: 19
  - file: 2026-09-10-openai-agents-api.image-ed69d2bdd82a.webp
    width: 320
    height: 126
  color: '#f7f8f5'
---

The Agents API gives your application access to the Codex harness through an OpenAI-managed API.

OpenAI manages sessions, orchestration, context compaction, and recovery while your application provides tools and chooses its execution environment.

Agents can operate in a sandbox where they can execute code, edit files, connect to MCP servers, and produce artifacts.

Model usage is billed at the selected model’s [API rates](https://developers.openai.com/api/docs/pricing). OpenAI tools use their [standard rates](https://developers.openai.com/api/docs/pricing#built-in-tools), and OpenAI-hosted sandboxes use standard [container rates](https://developers.openai.com/api/docs/pricing#built-in-tools).

Try these complete examples:

- [Create and run a directory-tree script](https://developers.openai.com/api/docs/guides/agents-api/quickstart#1-run-a-task) in an OpenAI-hosted sandbox.
- [Compare release notes with subagents](https://developers.openai.com/api/docs/guides/agents-api/multi-agent#example-compare-release-notes) and combine their findings into one answer.

Explore complete applications:

- [Incident response agent](https://developers.openai.com/showcase/agents-api-sev-bot): investigate alerts and request approval for recovery actions.
- [Slack bot](https://developers.openai.com/showcase/agents-api-slack-bot): investigate requests using connected workplace tools.
- [Data analyst](https://developers.openai.com/showcase/agents-api-data-analyst): answer warehouse questions with read-only SQL.
- [GitHub issue investigator](https://developers.openai.com/showcase/agents-api-github-issues): reproduce reported bugs and share findings on GitHub.
- [Document reviewer](https://developers.openai.com/showcase/agents-api-document-review): review documents with policy skills and specialist agents.

The Agents API is built around four main concepts:

- **Agent:** The model, instructions, tools, and MCP servers available to the agent.
- **Environment:** An optional sandbox or computer where the agent accesses files, loads skills, and runs commands.
- **Session:** A durable instance of an agent that works on tasks and responds to input.
- **Events and items:** The inputs sent to an agent and the output produced during a session.

### A session from start to finish

Start with an OpenAI-hosted sandbox in the [quickstart](https://developers.openai.com/api/docs/guides/agents-api/quickstart):

1. **Create a session.** Configure the agent; OpenAI provisions its environment.
2. **Give it a task.** User input starts a turn of work once the environment is ready.
3. **Follow progress.** Stream output or use webhooks to learn when the agent finishes or needs input.
4. **Continue or steer.** Send another task to the same session, or guide the agent during its current turn.

With an OpenAI-hosted session, your application sends input and receives events, while OpenAI runs the agent and provisions and manages its sandbox. See [environment options](https://developers.openai.com/api/docs/guides/agents-api/configuration#environment-settings) for setup and limitations.

![Your application starts sessions and receives events and output from the Agents API. OpenAI runs the managed Codex harness and provisions and manages its sandbox.](https://developers.openai.com/images/api/agents-api/overview-1.webp)

The managed Codex harness supports:

- Running commands and code in a sandbox.
- Applying relevant skills and instructions.
- Connecting to external data through tools or MCP.
- Steering the agent while it works.
- Summarizing previous work to manage its context window.
- Breaking work into subtasks and delegating to subagents.
- Resuming a session where it left off.

Check the [quickstart prerequisites](https://developers.openai.com/api/docs/guides/agents-api/quickstart#prerequisites) for API-key permissions and SDK setup. Configure these capabilities when you create a session:

```python
from openai import OpenAI

client = OpenAI()

session = client.beta.agents.sessions.create(
    agent={
        "model": "gpt-6-astra",
        "instructions": "Use the OpenAI documentation MCP and web search to answer technical questions accurately. Delegate independent research tasks to subagents when useful.",
        "tools": [
            {"type": "programmatic_tool_calling"},
            {
                "type": "mcp",
                "server_label": "openai_docs",
                "transport": {
                    "type": "http",
                    "server_url": "https://developers.openai.com/mcp",
                },
            },
            {"type": "web_search"},
        ],
        "multi_agent": {"enabled": True, "max_concurrent_subagents": 4},
    },
    environment={
        "type": "self_hosted",
        "workspace_directory": "/workspace",
        "capability_directories": ["/workspace/capabilities/skills"],
    },
    input=[
        {
            "role": "user",
            "content": [
                {
                    "type": "input_text",
                    "text": "Research how to connect an MCP server to an OpenAI agent, check for recent updates, and summarize the recommended setup.",
                }
            ],
        }
    ],
)
print(session.id)
```

For a runtime comparison, see the [Agents overview](https://developers.openai.com/api/docs/guides/agents#compare-agent-runtimes).

The Agents API retains session state so you can continue work across turns without rebuilding the conversation context. You can delete sessions and published artifacts when you no longer need them. The Agents API currently supports data residency only in the United States and does not support Zero Data Retention (ZDR). Choosing a self-hosted sandbox does not make the Agents API ZDR-eligible. See [Data controls in the OpenAI platform](https://developers.openai.com/api/docs/guides/your-data#storage-requirements-and-retention-controls-per-endpoint) for details on data residency and retention.
