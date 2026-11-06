---
title: 'Launch HN: Rivet (YC W23) – Scale and Orchestrate Agents on S3'
link: https://github.com/rivet-dev/rivet
source: hnrss-org
published: 2026-11-05T17:17:42Z
updated: 2026-11-05T17:17:42Z
first_seen: 2026-11-06T11:27:16.888185679Z
authors:
- NathanFlurry
summary: 'Hey HN, this is Nathan and Nicholas from Rivet (https://rivet.dev) – a fast, high-density, and scalable orchestrator for agentic workloads using the actor model. Rather than running a heavyweight sandbox per agent, each agent gets a lightweight actor backed by S3. Actors provide a long-lived process for long agent turns, in-memory state for context, a WebSocket server for multiplayer, durable SQLite storage backed by S3, and hibernation when idle. It''s a similar model to Cloudflare Durable Objects, but it’s open source and self-hostable. Here’s a video walkthrough: https://www.youtube.com/watch?v=3Ev6I7DOV9c When we went through YC in 2023, we were building infrastructure for multiplayer games like Diep and Krunker. This involved heavily optimizing high-throughput matchmaking, real-time player messages, and blitz scaling. Through that process, we spent a lot of time on workloads where Postgres couldn''t keep up with our throughput. We started out by hacking together Lua scripts on top of Redis, but this became too cumbersome to maintain and upgrade. This led us to extend our game server orchestrator to support what we call Rivet Actors today: many lightweight workers with in-memory state and durable storage. By using actors, we were able to serve tens of millions of monthly active users. As agents grew in popularity, it became clear that running agents in sandboxes is inefficient. Sandboxes are expensive, mostly idle, and slow to start. Developers started using actors because they are far more memory efficient (72 KB baseline for actors vs 512 MB for a minimal sandbox) and fast to start (4.9 ms p99 for actors vs 1.7 s p99 for sandboxes). For example, Amp is using Rivet to orchestrate multiplayer agents. They originally tried to build on Cloudflare Durable Objects, but couldn’t launch because of the platform’s limitations and unresponsive support. They migrated to Rivet Actors in a weekend and deploy on-prem. Rivet’s flexible architecture lets them adapt it to their own use cases without being constrained by a framework or the memory, storage, and runtime limits of Cloudflare Durable Objects. Under the hood, Rivet is backed by S3 while maintaining SSD speeds. Why S3? Previously, Rivet ran on FoundationDB. However, we were surprised to find that companies are storing orders of magnitude more data on Rivet Actors than in Postgres, such as large session transcripts, agent artifacts, and full POSIX filesystems. As their data grew, they had to start paying for SSDs that sat mostly idle with old transcripts. This led us to double down on S3 as our primary storage medium without sacrificing performance. This choice means that data stored on Rivet is 10× cheaper than on EBS gp3 while maintaining SSD speeds. It’s not as simple as writing data straight to S3. Lots of PUTs for tiny blobs of data are slow and expensive. Instead, we use an SSD tier to optimize speed and cost: When you write data to an actor’s SQLite database, the write is acknowledged once SSDs in 2 of 3 availability zones have it. Then, the data is batched to S3 in the background. This means that S3 is never on the write path, which lowers latency and cost. When reading data or waking an actor, Rivet reads from SSDs if already cached and falls back to S3 if not. SSD caches can be tuned to meet your needs, giving you configurable tiered storage for your actors. This architecture lets Rivet provide 1.07 ms durable writes (3 AZ, p99), 1.36 ms reads (p99), and 138 ms reads from object storage (p99). We’ve spent a lot of time making sure that Rivet can run wherever you need it. Self-hosting requires deploying a single Rust binary with S3 as the only dependency, Rivet Cloud lets you deploy dedicated clusters on AWS or GCP in any region, and BYOC lets us deploy Rivet to your VPC without having to operate it yourself. Rivet also runs identically in local dev as in production, since Rivet is a lightweight binary that runs anywhere Rivet Actors can be written in TypeScript or Rust. Out of the box, they support agents (Pi Durable), workflows, real-time, multiplayer, running AI-generated code, Durable Streams, and integrations with tools like Vercel’s Eve and Turso. See all of our available guides, or point your agent at them: https://rivet.dev/docs/#guides Try it out now, by reading our quickstart guides (https://rivet.dev/docs/) or ask your agent: “Run npx skills add rivet-dev/skills, then build a multiplayer chat agent and run it locally.” We’d love to get feedback on your experience with Rivet! We’re deeply invested in making Rivet an easy-to-self-host, flexible, and efficient way to run agentic workloads. GitHub: https://github.com/rivet-dev/rivet Architecture: https://rivet.dev/docs/architecture/ Benchmarks: https://rivet.dev/docs/benchmarking/ Self-hosting: https://rivet.dev/docs/deploy/self-host/ Comments URL: https://news.ycombinator.com/item?id=50381687 Points: 100 # Comments: 52'
content: extracted
html: 2026-11-05-launch-hn-rivet-yc-w23-scale-and-orchestrate-agents-on-s3.html
remote_preview:
  url: https://repository-images.githubusercontent.com/654560183/d4b2fdae-1a15-4420-a2a3-17256a51ae24
  alt: The orchestrator for agentic workloads. A fast, high-density and scalable orchestrator for every workload an agent needs. - rivet-dev/rivet
---

[![Rivet](https://github.com/rivet-dev/rivet/raw/main/.github/media/logo/icon-text-black.svg)](https://www.rivet.dev)

### Fast, high-density, scalable orchestration with Rivet Actors. Built on S3 at SSD speeds.

4.99 ms p99 cold starts, 72 KB RAM per Actor, scales to billions

[Quickstart](https://www.rivet.dev/docs) • [Documentation](https://www.rivet.dev/actors/docs) • [Blog](https://www.rivet.dev/blog) • [Discord](https://www.rivet.dev/discord) • [X](https://x.com/rivet_dev)

## What is Rivet?

Rivet Actors are long-lived processes with in-memory state, a SQLite database, WebSockets, and scheduling. You can create one for each agent, user, session, or tenant.

- **72 KB per Actor**: Actors share a node process instead of each booting a sandbox, so one server holds hundreds of thousands.
- **4.99 ms p99 cold starts**: No image pull and no VM boot.
- **Built for scale**: All data lives in S3, so a cluster scales to billions of Actors.
- **Hibernates when idle, wakes on demand**: Idle Actors cost nothing and wake on the next request with nothing lost.
- **SQLite backed by S3, at SSD speeds**: 1.07 ms durable writes and 1.36 ms reads at p99, for about 10.4× less than replicated SSD storage.

### Use cases

- **[Durable & multiplayer AI agents](https://www.rivet.dev/guides/ai-agent)**: memory that survives restarts, shared live with every client
- **[Workflows](https://www.rivet.dev/workflows/docs)**: multi-step operations that replay instead of starting over
- **[AI-generated apps](https://www.rivet.dev/dynamic-apps/docs)**: run the apps and Actors your agent generates
- **[Collaborative editing](https://www.rivet.dev/guides/collaborative-text-editor)**: one Actor per document, synced to every editor

### Example

A minimal agent written from scratch. For a ready-made agent, use [Pi Durable](https://www.rivet.dev/agents/docs/quickstart/).

```ts
// server.ts
import { actor, setup } from "rivetkit";
import { db } from "rivetkit/db";
import { streamText } from "ai";
import { openai } from "@ai-sdk/openai";

// One Actor per agent, created on first use: agent.getOrCreate(["agent-123"])
export const agent = actor({
  // 1. SQLite: every agent has its own database, so history survives restarts
  db: db({
    onMigrate: async (db) => {
      await db.execute("CREATE TABLE IF NOT EXISTS messages (role TEXT, content TEXT)");
    },
  }),

  // 2. In-memory state: plain variables that live with the Actor, with no database round trip
  createVars: () => ({ online: new Map<string, string>() }),

  // 3. WebSockets: each client holds a live connection, and the Actor pushes to all of them
  onDisconnect: (c, conn) => {
    c.vars.online.delete(conn.id);
    c.broadcast("online", [...c.vars.online.values()]);
  },

  actions: {
    join: (c, user: string) => {
      c.vars.online.set(c.conn.id, user);
      c.broadcast("online", [...c.vars.online.values()]);
    },

    sendMessage: async (c, text: string) => {
      // Save the message
      await c.db.execute("INSERT INTO messages VALUES (?, ?)", "user", text);
      const messages = await c.db.execute("SELECT role, content FROM messages");

      // Stream tokens to every connected client
      const response = streamText({ model: openai("gpt-5"), messages });
      for await (const delta of response.textStream) {
        c.broadcast("token", delta);
      }

      // Save the response
      await c.db.execute("INSERT INTO messages VALUES (?, ?)", "assistant", await response.text);
    },
  },
});

export const registry = setup({ use: { agent } });
```

```ts
// client.ts (frontend or backend)
import { createClient } from "rivetkit/client";
import type { registry } from "./server";

const client = createClient<typeof registry>();

// Open a WebSocket connection to the agent
const agent = client.agent.getOrCreate(["agent-123"]).connect();

// Listen for realtime events
agent.on("online", users => console.log("online:", users));
agent.on("token", delta => process.stdout.write(delta));

// Call actions on the Actor
await agent.join("nathan");
await agent.sendMessage("how many r's in strawberry?");
```

### Why run agents on Rivet Actors?

Each Actor gives the agent a **long-lived process** for long agent turns, **in-memory state** for context, a **WebSocket server** for multiplayer, **durable SQLite storage** for session transcripts and agent artifacts, and **hibernation** when idle.

Most of an agent's time is spent **waiting on inference**. When the agent loop runs inside a sandbox, you pay for a whole VM that sits **mostly idle** with low CPU usage.

Rivet Actors run the agent loop outside of the sandbox instead, often called **"separating the head from the hands."** The agent's state, memory, and context live in the Actor, and it reaches the sandbox through tools only when it needs to run code. This uses a fraction of the memory and limits the **blast radius** and **trust boundary** of the code it runs. Read more about [running the harness outside of the sandbox](https://www.rivet.dev/blog/2026-07-27-run-your-harness-outside-the-sandbox/).

### Do I need a sandbox with a Rivet Actor?

Often not. An agent needs a sandbox only to run code it writes. Developers on Rivet use three patterns:

- **Agents with tools**: Many agents only need tool calls, MCP, and APIs, such as customer support or personal agents. No sandbox needed.
- **Agents with a lightweight sandbox**: [agentOS](https://www.rivet.dev/agentos/docs) provides Linux-compatible sandboxing for JS and Python using V8 and WebAssembly, with full POSIX filesystems, networking stacks, and process trees. It covers most workloads, like data analysis, file processing, evals, and code mode.
- **Agents with a full sandbox**: Keep your existing sandbox provider and expose its shell and filesystem to the agent as tools, such as a `bash` tool that runs commands in the sandbox. The Actor reattaches to it across sleeps and restarts. See the [sandbox coding agent example](https://github.com/rivet-dev/rivet/blob/main/examples/sandbox-coding-agent).

## Getting Started

### Use with your coding agent

Paste this prompt into Claude Code, Cursor, Codex, or any coding agent:

```
Run npx skills add rivet-dev/skills, then ask me what to build and run it locally.
```

### Start from scratch

Rivet ships libraries built on top of Actors for common use cases.

- **[Actors](https://www.rivet.dev/actors/docs)**: Long-lived processes with state, realtime, and scheduling built in.
- **[Agents](https://www.rivet.dev/agents/docs)**: Durable agents built on Actors, with memory that survives restarts.
- **[Workflows](https://www.rivet.dev/workflows/docs)**: Multi-step operations built on Actors that replay instead of starting over.
- **[agentOS](https://www.rivet.dev/agentos/docs)**: Run the code your agent writes, securely, with granular control over network access, filesystem access, and resource limits.
- **[Dynamic Apps](https://www.rivet.dev/dynamic-apps/docs)** (preview): A backend per user, deployed the moment it is generated.

## How Actors compare to sandboxes

| Metric              | Rivet Actor       | Sandbox          |
| ------------------- | ----------------- | ---------------- |
| Cold start (p99)    | **4.99 ms**       | 1.70 s           |
| Memory per instance | **72.4 KB**       | 512 MiB          |
| Storage             | **SQLite on S3**  | Disk per sandbox |
| Storage cost        | **10.4× cheaper** | Replicated SSDs  |

Benchmark details & methodology

- **Rivet numbers** come from the published [Rivet Actors vs Cloudflare Durable Objects benchmark](https://github.com/rivet-dev/bench-rivet-actors-vs-durable-objects), run on Rivet 3.0.3. Its repository has the raw samples and the scripts to reproduce them.
- **Sandbox numbers** are for [E2B](https://e2b.dev): 512 MiB is its default memory allocation from [E2B's billing documentation](https://docs.e2b.dev/billing) as of November 4, 2026, and 1.70 s is the p99 cold start from [ComputeSDK's October 30, 2026 sandbox benchmark](https://www.computesdk.com/benchmarks/sandboxes/burst-tti).
- **Storage cost** compares S3 Standard with three Amazon EBS gp3 copies at AWS list prices.
- Sources, dates, and the full method are at [rivet.dev/docs/benchmarking](https://www.rivet.dev/docs/benchmarking/).

## Features

- **Fault tolerance**: If a node goes down, its Actors move elsewhere with their state intact.
- **HTTP & WebSocket networking**: Call an Actor over HTTP or hold a live WebSocket to it.
- **End-to-end type safety**: Typed clients for your backend and React hooks for your frontend.
- **Cron & scheduling**: Wake an Actor on a schedule or at a time it sets for itself.
- **Actor-to-Actor calls**: Actors call each other by key across the cluster.
- **Scoped JWTs**: Give each client a [JWT](https://www.rivet.dev/docs/jwt) scoped to the Actors it may reach.
- **OpenTelemetry**: Traces, metrics, and logs in OTel format.

## Deployment Options

| Option                | Description                                                                                                                        | Get started                                                                      |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| **Local & self-host** | Run Rivet locally while you build. In production, run the control plane on Docker, Kubernetes, or a VM. S3 is the only dependency. | [Self-hosting docs](https://www.rivet.dev/docs/deploy/self-host/control-plane/) |
| **Rivet Cloud**       | Fully managed, dedicated clusters on AWS or GCP in any region. Run your nodes on Rivet Cloud or bring your own.                    | [Dashboard](https://dashboard.rivet.dev)                                        |
| **BYOC**              | The control plane runs inside your own VPC on AWS, GCP, or Azure, fully managed by Rivet. Your data never leaves your cloud.       | [BYOC docs](https://www.rivet.dev/docs/deploy/byoc/)                            |

Every option runs the same control plane, from local dev to production. [Talk to an engineer →](https://www.rivet.dev/talk-to-an-engineer)

## Works with the tools you already use

Typed SDKs for your backend and React hooks for your frontend. Rivet runs on native Node.js and Bun with no custom runtime, so your existing packages, tooling, and tests work unchanged.

**Deploy nodes on**: [Docker Compose](https://www.rivet.dev/docs/deploy/self-host/nodes/docker-compose/) • [Kubernetes](https://www.rivet.dev/docs/deploy/self-host/nodes/kubernetes/) • [VM & bare metal](https://www.rivet.dev/docs/deploy/self-host/nodes/vm/) • [Hetzner](https://www.rivet.dev/docs/deploy/self-host/nodes/hetzner/) • [AWS ECS](https://www.rivet.dev/docs/deploy/self-host/nodes/aws-ecs/) • [Railway](https://www.rivet.dev/docs/deploy/self-host/nodes/railway/) • [Render](https://www.rivet.dev/docs/deploy/self-host/nodes/render/)

**Frameworks**: [React](https://www.rivet.dev/actors/docs/clients/react) • [Effect](https://github.com/rivet-dev/rivet/blob/main/examples/ai-agent-effect)

**Runtimes**: [Node.js](https://www.rivet.dev/actors/docs/quickstart/backend) • [Bun](https://www.rivet.dev/actors/docs/quickstart/backend) • [Rust](https://www.rivet.dev/actors/docs/quickstart/rust)

**Tools**: [Vitest](https://www.rivet.dev/actors/docs/testing) • [OpenTelemetry](https://www.rivet.dev/docs/tracing) • [Pino](https://www.rivet.dev/actors/docs/logging) • [AI SDK](https://github.com/rivet-dev/rivet/blob/main/examples/ai-agent) • [OpenAPI](https://github.com/rivet-dev/rivet/blob/main/rivetkit-openapi) • [AsyncAPI](https://github.com/rivet-dev/rivet/blob/main/rivetkit-asyncapi)

[Request an integration →](https://github.com/rivet-dev/rivet/issues/new)

## FAQ

**Does Rivet have BYOC?**

Yes. Rivet deploys into your own cloud account, including air-gapped environments: the control plane and storage run inside your VPC while Rivet operates them. See the [BYOC docs](https://www.rivet.dev/docs/deploy/byoc/).

**Does Rivet have a cloud?**

Yes. [Rivet Cloud](https://dashboard.rivet.dev) runs fully managed, dedicated clusters on AWS or GCP in any region. It can run your app's Docker image as [nodes on Rivet Cloud](https://www.rivet.dev/docs/deploy/cloud/compute/), or you can [bring your own nodes](https://www.rivet.dev/docs/deploy/self-host/nodes/).

**Can I self-host Rivet?**

Yes. Rivet is fully self-hostable; see the [self-hosting docs](https://www.rivet.dev/docs/deploy/self-host/control-plane/). The control plane runs anywhere a container runs, with guides for Docker Compose, Kubernetes, VMs and bare metal, Hetzner, Railway, Render, and AWS ECS. For enterprise support with a self-hosted deployment, [contact us](https://www.rivet.dev/enterprise/).

**Do I still need Postgres, FoundationDB, or NATS?**

No. An object storage bucket is the only dependency: there is no database to run and no message bus, and control plane instances find each other through the bucket. Rivet works with Amazon S3, Google Cloud Storage, Azure Blob Storage, and any S3-compatible store such as MinIO, Cloudflare R2, or Hetzner Object Storage. A local filesystem backend is available for trying Rivet on one machine.

**Is Rivet open-source?**

Yes. Rivet is open source under the permissive Apache 2.0 license. You're looking at it.

**Are Rivet Actors only for agents?**

No. Rivet is a general actor platform at its core, with nothing specific to agents. For agents, you can use the optional [agent library](https://www.rivet.dev/agents/docs/quickstart/), which runs Pi Durable on Rivet Actors, or write your own agent from scratch.

**How does Rivet compare to sandboxes?**

A sandbox boots a VM or container per agent, which is slow to start and sits mostly idle. A Rivet Actor runs inside a shared node process: 72 KB and 4.99 ms p99 to start, versus 512 MiB and 1.70 s for a sandbox, and it hibernates when idle. To run untrusted code your agent writes, use [agentOS](https://www.rivet.dev/agentos/docs).

**How does Rivet compare to Cloudflare Durable Objects?**

Rivet is often used as an open-source alternative to Cloudflare Durable Objects that is easy to self-host and scales. It also fixes many of the constraints of Durable Objects: no 128 MB memory cap, bottomless storage instead of a 10 GB SQLite limit, Actors are not randomly evicted, and you control the runtime because Actors run on vanilla Node.js or Bun instead of a custom runtime. Rivet also adds built-in connection handling, event broadcasting, durable scheduling and cron, full Vitest support, lifecycle hooks for draining, and control over Actor upgrades. See the [Cloudflare Durable Objects comparison](https://www.rivet.dev/actors/compare/rivet-actors-vs-cloudflare-durable-objects/).

**Does Rivet use a custom V8 runtime?**

No. Rivet runs on native Node.js and Bun, so your existing packages, tooling, and tests work unchanged.

**Is Rivet like Erlang, Akka, or Orleans?**

Rivet implements the virtual actor pattern, which is closest to Orleans, but focuses on what modern workloads need: SQLite persistence instead of simple JSON stores and support for modern runtimes. Erlang and Akka do not provide virtual actors out of the box, though ecosystem packages exist that offer functionality comparable to Rivet Actors.

**Does Rivet require microVMs, gVisor, or nested virtualization?**

No. Rivet only needs a standard process that connects to the control plane over WebSocket. That process can be Node.js, Bun, Rust, or anything else.

## Projects in This Repository

| Project                                                                                  | Description                                                            |
| ---------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| [RivetKit TypeScript](https://github.com/rivet-dev/rivet/blob/main/rivetkit-typescript) | Client & server library for building Actors                            |
| [RivetKit Rust](https://github.com/rivet-dev/rivet/blob/main/rivetkit-rust)             | Rust SDK                                                               |
| [RivetKit Python](https://github.com/rivet-dev/rivet/blob/main/rivetkit-python)         | Python client (experimental)                                           |
| [RivetKit Swift](https://github.com/rivet-dev/rivet/blob/main/rivetkit-swift)           | Swift client (experimental)                                            |
| [Control Plane](https://github.com/rivet-dev/rivet/blob/main/engine)                    | Rust control plane that schedules, routes, and persists Actors         |
| ↳ [Pegboard](https://github.com/rivet-dev/rivet/blob/main/engine/packages/pegboard)     | Actor scheduling & networking                                          |
| ↳ [Guard](https://github.com/rivet-dev/rivet/blob/main/engine/packages/guard)           | Traffic routing proxy                                                  |
| ↳ [Epoxy](https://github.com/rivet-dev/rivet/blob/main/engine/packages/epoxy)           | Multi-region KV store (EPaxos)                                         |
| [Documentation](https://github.com/rivet-dev/rivet/blob/main/docs)                      | Source for [rivet.dev/actors/docs](https://www.rivet.dev/actors/docs) |
| [Examples](https://github.com/rivet-dev/rivet/blob/main/examples)                       | Runnable examples                                                      |

## Community

- [Discord](https://www.rivet.dev/discord) - Chat with the community
- [X/Twitter](https://x.com/rivet_dev) - Follow for updates
- [GitHub Discussions](https://github.com/rivet-dev/rivet/discussions) - Ask questions
- [GitHub Issues](https://github.com/rivet-dev/rivet/issues) - Report bugs
- [Talk to an engineer](https://www.rivet.dev/talk-to-an-engineer) - Discuss your use case

## License

[Apache 2.0](https://github.com/rivet-dev/rivet/blob/main/LICENSE)
