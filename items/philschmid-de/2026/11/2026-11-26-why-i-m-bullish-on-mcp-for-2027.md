---
title: Why I'm Bullish on MCP for 2027
link: https://www.philschmid.de/mcp-2027
source: philschmid-de
published: 2026-11-26T00:00:00Z
updated: 2026-11-26T00:00:00Z
first_seen: 2026-11-26T21:52:31.976721529Z
summary: Early MCP deserved its backlash. Code Mode, typed outputSchema, native multimodality, Agent Skills inside MCP, always-on MCP Events, and MCP Apps are turning it into the integration layer for agent harnesses in 2027.
content: extracted
html: 2026-11-26-why-i-m-bullish-on-mcp-for-2027.html
remote_preview:
  url: https://www.philschmid.de/_og/adb76b9639b8-og.jpg
  alt: Why I'm Bullish on MCP for 2027
---

MCP went through the full hype cycle in the last 18 months. Early excitement turned into backlash and confusion fast, with context bloat as the main complaint. When Skills arrived, most developers moved to CLIs and progressive disclosure instead. I never believed in that, and that sentiment is now changing.

At [Linear](https://linear.app/now/sharing-growth-with-the-people-building-linear) and [Notion](https://www.notion.com/blog/the-next-era-of-notion), agents now create roughly half of all issues (~2.4M/week) and pages, with Notion reporting 60x MCP traffic growth. At [Atlassian](https://www.atlassian.com/blog/company-news/inside-rovo-mcp-usage), MCP handles over 5M tool calls per workday, and 44% of those users sit outside software teams where CLIs were never an option. At [Neon](https://neon.com/mcp-server/stats), agent usage over MCP grows 14% week over week, and even Apple now ships local MCP servers for Pages, Numbers, and Keynote.

The protocol is healthy and actively improving. The [2026-07-28 spec](https://modelcontextprotocol.io/docs/2026-07-28/getting-started/intro) dropped stateful sessions and added official extensions, and clients are getting smarter about how they use them.

I'm bullish on MCP going into 2027.

![MCP 2025 vs MCP 2027](https://www.philschmid.de/_img/57e73d6a2bb9-1280.webp)

## Code Mode, structured outputs, and multimodality

Real tasks almost always take more than 1 tool call, e.g. you list open issues, grab details, check errors, and write a summary.

With plain tool calling, every step needs another model turn and dumps information into the context. Joining data across 2 MCP servers uses the model's context as memory for intermediate payloads.

[Code Mode](https://www.philschmid.de/code-mode) is changing how agents call MCP servers. Instead of putting 50 tool schemas into the prompt and calling 1 tool per turn, the harness gives the model a sandbox and exposes MCP tools as callable functions. It's already becoming the default in OpenCode 2.0, Claude Code, OpenAI Codex, Pi, and Factory Droid.

Code Mode works best when the model knows what a tool returns before writing code. When an MCP server declares `outputSchema` alongside `inputSchema`, the client can generate typed stubs upfront and pass back `structuredContent` directly. The model chains multiple calls in 1 script, filters and joins in memory where compute is cheap, and returns only what matters.

TypeScript

```typescript
// Auto-generated from MCP inputSchema + outputSchema
// interface LinearIssue { identifier: string; title: string; priority: number }
// interface SentryStats { count24h: number; p95Ms: number }
//
// declare const linear: {
//   listIssues(args: { team: string; state: "open" | "closed" }): Promise<{ items: LinearIssue[] }>;
// };
// declare const sentry: {
//   getIssueStats(args: { query: string }): Promise<SentryStats>;
// };
 
// Model writes 1 script instead of N tool-calling turns
const issues = await linear.listIssues({ team: "core", state: "open" });
const p0s = issues.items.filter(i => i.priority === 1);
 
const errors = await Promise.all(
  p0s.map(i => sentry.getIssueStats({ query: i.identifier }))
);
 
// Only the joined summary enters model context
return p0s.map((i, idx) => ({
  id: i.identifier,
  title: i.title,
  events24h: errors[idx].count24h,
}));
```

Compared to CLIs, MCP servers are natively multimodal. A tool call can return typed `structuredContent` for code alongside separate image, audio, or embedded resource blocks.

Auth is also cleaner. CLIs and raw `curl` force you to store API keys on disk or in environment variables with broad permissions. An MCP client handles per-user OAuth 2.1 login flows and scoped tool permissions outside the sandbox, so connecting a remote SaaS product works without handing raw credentials to the agent.

## Skills move inside MCP

The debate of Skills plus a CLI versus MCP might come to an end as well. So far teams shipped CLIs or MCP servers and maintained separate `SKILL.md` files to teach agents how to use them.

The [Skills extension](https://modelcontextprotocol.io/seps/2640-skills-extension) (`io.modelcontextprotocol/skills`) brings Agent Skills inside MCP. Each skill exposes a `skill://` URI, parsed YAML frontmatter, and a file manifest with SHA-256 digests. Those digests solve one of the biggest headaches with Agent Skills: keeping them updated across a team. When a vendor updates a skill on the server, the digest changes and clients pull the new version automatically instead of relying on manual file syncs.

Anthropic just shipped [Skills over MCP](https://claude.com/resources/articles/skills-over-mcp-tasks-and-more) in public beta across Claude and Claude Code, letting servers like Shopify, HubSpot, and Atlassian serve playbooks tailored to each logged-in user's account or organization.

The lifecycle stays light on context:

1. On connect, the harness calls `skills/list` and injects only the short `name` and `description` from `frontmatter` into the system instructions.
2. When the agent needs a skill, the harness calls `resources/read` (or `skills/get` for unlisted skills) to load the full `SKILL.md` into context, or caches the files to a local path keyed by the `sha256` digest so the agent can read them from disk.

JSON

```json
// 1. skills/list response (metadata goes into system instructions)
{
  "uri": "skill://jira/SKILL.md",
  "frontmatter": {
    "name": "jira-triage",
    "description": "Format and route bug tickets for engineering teams"
  },
  "resources": [
    { "uri": "skill://jira/SKILL.md", "size": 2410, "sha256": "sha256:9f3a...c812" }
  ]
}
```

![Skills bundled inside MCP](https://www.philschmid.de/_img/f75298d8727d-1280.webp)

## Always-on and multiplayer agents with MCP Events

The next step for agents is multiplayer and always-on workflows. Until recently, MCP integrations were strictly pull-based: they couldn't receive notifications or wake an agent up when something happened in an external system.

The [MCP Events extension](https://modelcontextprotocol.io/extensions/events/overview) (now in beta) changes that. A server advertises `io.modelcontextprotocol/events` in `server/discover` and lists available events via `events/list`. A harness can expose a subscribe tool (like [OpenCode 2.4.0's automation tool](https://x.com/opencode/status/2124886266835505473)) or user actions across servers, and the MCP client runs the subscription in the background using 1 of 3 delivery modes:

- **Webhook (`events/subscribe`):** The server sends signed HTTP POST requests to a host callback URL while the user is away.
- **Push (`events/stream`):** The server streams live updates over an open connection, like build or deploy logs.
- **Poll (`events/poll`):** The client checks on a timer. Useful for local CLIs and desktop apps without a public URL.

Take a P0 security alert at 2 a.m. or an urgent customer email. When an event fires, the MCP client tracks the `cursor`, deduplicates by `eventId`, and hands the payload to the harness. The harness wakes the agent, which pulls logs or recent thread history and posts a triage summary before anyone checks their inbox. Early servers like [Shop MCP](https://shop.dev) (order updates) and Vercel (`deployment.succeeded`) already ship event support. Combined with [MCP Tasks](https://claude.com/resources/articles/skills-over-mcp-tasks-and-more) (`io.modelcontextprotocol/tasks`), agents can handle async workflows without custom webhook glue.

![MCP Events subscribe and webhook wake-up flow](https://www.philschmid.de/_img/e17f7be78aa8-1280.webp)

Remote or durable harnesses like [ChatGPT](https://developers.openai.com/plugins/build/mcp-events) expose a callback endpoint so agents can wake up while you're offline. The model or user sets the event filters and standing instructions once, and the MCP client registers the callback under a renewable TTL.

JSON

```json
// 1. MCP client registers the harness's callback endpoint
{
  "method": "events/subscribe",
  "params": {
    "name": "security.alert.opened",
    "arguments": { "severity": "p0" },
    "delivery": {
      "type": "webhook",
      "callbackUrl": "https://host.example/mcp/webhook/sub_01H",
      "secret": "whsec_..."
    }
  }
}
 
// 2. Server POSTs the event occurrence when the alert fires
{
  "eventId": "evt_9821",
  "timestamp": "2026-11-24T02:14:00Z",
  "name": "security.alert.opened",
  "cursor": "cur_00491",
  "data": { "alertId": "SEC-412", "service": "auth-api", "severity": "p0" }
}
```

**Continuation:** When a webhook POST arrives, the harness injects the event back into the **same trajectory** (the subscribed chat thread) or spawns a **new isolated trajectory** per alert.

## Products are becoming agent-native with MCP Apps

Raw JSON and plain text aren't enough when you want to inspect a p95 latency chart, filter a live dashboard, or browse a design deck. MCP Apps (`io.modelcontextprotocol/ui`) let servers return sandboxed interactive HTML views (`text/html;profile=mcp-app`) directly inside compatible clients instead of forcing you across 4 browser tabs.

[Slack's new MCP client](https://slack.com/blog/news/slackbots-mcp-client), Claude's interactive connectors, and [Google Antigravity](https://antigravity.google/docs/mcp-apps/) already support this pattern, rendering MCP Apps inline in chat, in side panels like release boards, and as custom file viewers.

![Interactive latency chart rendered inline via MCP Apps](https://www.philschmid.de/_img/3d74ea9cfc3e-1280.webp)

## What still needs work

There are still real gaps to sort out:

- **Observability and intent:** MCP servers only see tool calls and miss the user's conversation. Passing `telemetry.intent` metadata or exposing feedback tools like [Cloudflare's `submit_feedback`](https://developers.cloudflare.com/changelog/post/2026-11-23-agent-feedback/) helps, though most servers still don't know why they were called.
- **Discovery:** Submitting your server to 5 different registries doesn't scale. We need cleaner discovery patterns like [`/.well-known`](https://rhys.dev/blog/mcp-discovery-proposal) with automated security scanning.
- **Security:** Untrusted webhook payloads, Code Mode sandboxes, and embedded UI iframes all widen the prompt-injection surface.

## Beyond remote tool calls

MCP is moving past simple tool calls. Clients are growing into full runtimes that combine Code Mode, typed outputs, Agent Skills, async Tasks, Events, and embedded UI in 1 protocol.

I think the best agent harnesses in 2027 will win on their MCP integration, and the harness itself should be an MCP server too.

Instead of inventing a custom multi-agent protocol, a harness can expose its own MCP server so agents coordinate with the same primitives they already use. Subagents run as MCP Tasks. A shared "swarm board" exposes tools to post findings and claim work. MCP Events wake the coordinator or peer agents when a task finishes or a new update lands.

![Agent harness using MCP for external tools and multi-agent coordination](https://www.philschmid.de/_img/89c7770d6b2a-1280.webp)

Good MCP servers expose tools for agents, not agents as tools. Wrapping an entire SaaS product behind a single `ask_agent` endpoint starves it of the context your agent already holds. Keep regular servers focused on clean, typed tools and structured data so your own agent can combine them, and reserve agent-as-MCP-server for long-running coordination across subagents, teams, or companies.
