---
title: Gemini 4 Carbon, Hydrogen, Decisions API, self-hosted agent sandboxes and more
link: https://www.philschmid.de/google-deepmind-roundup-nov-2026
source: philschmid-de
published: 2026-11-16T00:00:00Z
updated: 2026-11-16T00:00:00Z
first_seen: 2026-11-16T17:21:52.095799575Z
summary: 'Last week we shipped 7 launches across models, agent infrastructure, benchmarks, and science: two Gemini 4 models, our first Decision Model and API, self-hosted agent sandboxes, a new visual knowledge benchmark, and new robotics and genomics models.'
content: extracted
html: 2026-11-16-gemini-4-carbon-hydrogen-decisions-api-self-hosted-agent.html
remote_preview:
  url: https://www.philschmid.de/static/blog/google-deepmind-roundup-nov-2026/nov-2026-roundup-og.png
  alt: Gemini 4 Carbon, Hydrogen, Decisions API, self-hosted agent sandboxes and more
---

Last week we shipped 7 launches across models, agent infrastructure, benchmarks, and science: two Gemini 4 models, our first Decision Model & API, self-hosted agent sandboxes, a new visual knowledge benchmark, and new robotics and genomics models.

## 1\. Gemini 4 Carbon

[Gemini 4 Carbon](https://aistudio.google.com/docs/whats-new-gemini-4-carbon?e=0), available as `gemini-4-carbon`, is our strongest reasoning model for repository-scale engineering, formal proofs, and cybersecurity defense.

- Shared 1M-token pool across input, thinking, and output, with [long decode](https://aistudio.google.com/docs/long-decode) chaining 64K hops via [background execution](https://aistudio.google.com/docs/background-execution).
- Tunable low, medium, and high [thinking levels](https://aistudio.google.com/docs/thinking); `temperature`, `top_p`, and `top_k` are ignored.
- Automatic [implicit caching](https://aistudio.google.com/docs/caching) from 4,096 tokens at a 95% discount, retiring `cachedContents`.
- Introductory [pricing](https://aistudio.google.com/docs/pricing#gemini-4-carbon) at $2.00 input and $10.00 output per 1M tokens through November 30, then $4.00 and $20.00 starting December 1; see the [model card](https://aistudio.google.com/docs/models/gemini-4-carbon) and [Carbon developer guide](https://aistudio.google.com/learn/gemini-4-carbon-developer-guide).

TypeScript

```typescript
import { GoogleGenAI } from "@google/genai";
 
const client = new GoogleGenAI({});
 
const interaction = await client.interactions.create({
  model: "gemini-4-carbon",
  input: "Find lock-ordering deadlocks in a two-phase commit coordinator and sketch a lock-free alternative.",
  generation_config: {
    thinking_level: "medium",
  },
});
 
console.log(interaction.output_text);
```

## 2\. Gemini 4 Hydrogen

[Gemini 4 Hydrogen](https://aistudio.google.com/docs/whats-new-gemini-4-hydrogen?e=0), available as `gemini-4-hydrogen`, is our [recommended upgrade from Gemini 3.8 Flash](https://aistudio.google.com/docs/whats-new-gemini-4-hydrogen?e=0#migrate-from-gemini-3-8-flash) for everyday coding and fast agentic workflows.

- Priced at a flat $1.50 input and $7.50 output per 1M tokens across the 1M-token window; see the [model card](https://aistudio.google.com/docs/models/gemini-4-hydrogen) and [rate limits](https://aistudio.google.com/docs/rate-limits).
- Flags missing context when data is absent, batches parallel read and search tool calls, and inspects individual frames during [agentic video understanding](https://aistudio.google.com/docs/video-understanding#agentic-video-understanding).
- Defaults to the `high` [thinking level](https://aistudio.google.com/docs/thinking#understanding-thinking-levels), adds `xhigh` for multi-step coding problems, and rejects the legacy `minimal` setting.
- Powers our default [Antigravity agent](https://aistudio.google.com/docs/antigravity-agent) in Gemini Managed Agents.

TypeScript

```typescript
import { GoogleGenAI } from "@google/genai";
 
const client = new GoogleGenAI({});
 
const interaction = await client.interactions.create({
  model: "gemini-4-hydrogen",
  input: "Write a TypeScript token-bucket rate limiter that avoids floating-point burst-boundary drift.",
  generation_config: {
    thinking_level: "xhigh",
  },
});
 
console.log(interaction.output_text);
```

## 3\. Choosing your model

Hydrogen and Carbon share the same 1M unified context window, long decode architecture, and [Interactions API](https://aistudio.google.com/docs/interactions-overview) schema. [Pick the model](https://aistudio.google.com/docs/whats-new-gemini-4-hydrogen?e=0#choosing-between-models) that matches your reasoning depth, latency, and cost target:

- Gemini 4 Hydrogen: everyday coding, interactive apps, tool-using agents, and parallel subagents.
- Gemini 4 Carbon: hard architecture design, debugging, scientific proofs, and security audits.

You can also pair Carbon and Hydrogen. Use Carbon to plan and orchestrate and Hydrogen to execute.

![Choosing your Gemini 4 model](https://www.philschmid.de/static/blog/google-deepmind-roundup-nov-2026/gemini-4-orchestrator.png)![](https://www.philschmid.de/static/blog/google-deepmind-roundup-nov-2026/gemini-4-orchestrator-dark.png)

## 4\. Gemini Decisions API

The [Gemini Decisions API](https://aistudio.google.com/docs/decisions-api?e=0), powered by [Gemini Decision 1](https://aistudio.google.com/docs/models/gemini-decision-1), evaluates text, images, video, [PDFs](https://aistudio.google.com/docs/decisions-api?e=0#documents), or [audio](https://aistudio.google.com/docs/decisions-api?e=0#audio) across up to 256K tokens against up to 1,000 isolated questions with 0 output tokens billed.

![Gemini Decisions API multimodal scoring architecture](https://www.philschmid.de/static/blog/google-deepmind-roundup-nov-2026/decisions-architecture.png)![](https://www.philschmid.de/static/blog/google-deepmind-roundup-nov-2026/decisions-architecture-dark.png)

Three [question types](https://aistudio.google.com/docs/decisions-api?e=0#question-types) return calibrated probabilities and a confidence score: `noul` for binary yes/no checks, `choice` across 2 to 255 discrete keys, and `score` for ordered rubrics. Pass media via base64, HTTPS URLs, or the [Files API](https://aistudio.google.com/docs/files), and test rubrics in the [Gemini Decisions Playground](https://aistudio.google.com/apps/f6cbef57-8bad-443a-813f-4d4c8f9750af?fullscreenApplet=true). Here's a runnable check over our sample WAV file:

TypeScript

```typescript
import { GoogleGenAI } from "@google/genai";
 
const ai = new GoogleGenAI({ apiVersion: "v1beta" });
 
const result = await ai.decisions.create({
  model: "gemini-decision-1",
  input: [
    {
      type: "audio",
      uri: "https://storage.googleapis.com/generativeai-downloads/audio/rehearsing.wav",
      mime_type: "audio/wav",
    },
  ],
  questions: {
    mentions_launch: {
      type: "noul",
      instructions: "Is the speaker talking about a product campaign launch?",
    },
    delivery: {
      type: "score",
      instructions: "How fluent and prepared does the speaker sound?",
      criteria: [
        "Hesitant, searching for notes, or self-correcting.",
        "Mostly steady with minor pauses.",
        "Polished, fluent, and confident throughout.",
      ],
    },
  },
});
 
const launch = result.answers?.mentions_launch;
const delivery = result.answers?.delivery;
if (launch?.type === "noul" && delivery?.type === "score") {
  console.log({ pLaunch: launch.noul, deliveryScore: delivery.score });
}
```

## 5\. Bring your own sandbox for managed agents

[Gemini Managed Agents](https://aistudio.google.com/docs/agents) now supports [self-hosted environments](https://aistudio.google.com/docs/agent-environment?e=0#self-hosted-environments). We run model inference and the agent loop on Google infrastructure while bash commands and file edits execute inside your compute.

![Self-hosted environments for managed agents](https://www.philschmid.de/static/blog/google-deepmind-roundup-nov-2026/self-hosted-sandbox.png)![](https://www.philschmid.de/static/blog/google-deepmind-roundup-nov-2026/self-hosted-sandbox-dark.png)

Your worker connects over outbound HTTPS with a scoped key and zero inbound firewall ports. In production, you can deploy across Modal, E2B, Cloudflare, Daytona, [Fly.io](https://fly.io/), Vercel, or Cloud Run using our [self-hosted provider manifests](https://github.com/google-gemini/gemini-api-cli/tree/main/docs/self-hosted).

To test it end to end on your own machine, install our open-source [gemini-api CLI](https://github.com/google-gemini/gemini-api-cli) from the [CLI guide](https://aistudio.google.com/docs/cli) and run this single script. It creates a self-hosted environment via REST, spawns a local worker confined to `./sandbox`, runs the managed agent locally, and cleans up the environment when finished. Local mode executes bash directly on your host unless you run inside a container or pass `--disable-bash`:

TypeScript

```typescript
import { spawn } from "node:child_process";
import { mkdirSync } from "node:fs";
import { GoogleGenAI } from "@google/genai";
 
const client = new GoogleGenAI({ httpOptions: { apiVersion: "v1beta" } });
mkdirSync("./sandbox", { recursive: true });
 
const res = await fetch("https://generativelanguage.googleapis.com/v1beta/environments", {
  method: "POST",
  headers: { "x-goog-api-key": process.env.GEMINI_API_KEY!, "Content-Type": "application/json" },
  body: JSON.stringify({ mode: "self_hosted" }),
});
const { id, environment_key } = (await res.json()) as { id: string; environment_key: string };
 
const worker = spawn("gemini-api", [
  "environments", "connect", "--environment-key", environment_key,
  "--root-dir", "./sandbox", "--workdir", "./sandbox", "--worker-id", "local-sandbox",
], { stdio: ["ignore", "inherit", "inherit"] });
 
try {
  const run = await client.interactions.create({
    agent: "antigravity-preview-09-2026",
    input: "Write a fib.ts script that prints 8 Fibonacci numbers, run it with bun, and return the output.",
    environment: id,
  }, { timeout: 300000 });
  console.log(run.output_text);
} finally {
  worker.kill();
  await client.environments.delete(id);
}
```

Running the script prints the local worker logs alongside the agent output:

Bash

```bash
$ bun run sandbox.ts
2026/11/16 13:40:30 Worker local-sandbox polling (maxConcurrent=4)
2026/11/16 13:40:30 [v1_Chd5d2Y3...] Claimed interaction lease (env=f5621577-89ef-4698-999f-b139b9121d00)
2026/11/16 13:40:31 [v1_Chd5d2Y3...] Executing tool execute_bash
2026/11/16 13:40:35 [v1_Chd5d2Y3...] Executing tool write_to_file
2026/11/16 13:40:38 [v1_Chd5d2Y3...] Executing tool execute_bash
2026/11/16 13:40:54 [v1_Chd5d2Y3...] Completed interaction session
Created fib.ts and ran it with bun fib.ts: 0 1 1 2 3 5 8 13
```

## 6\. Multimodal Knowledge Agent benchmark

We partnered with Artificial Analysis to release the [Multimodal Knowledge Agent benchmark](https://blog.google/innovation-and-ai/models-and-research/gemini-models/multimodal-knowledge-agent-benchmark/), evaluating 200 multi-step tasks across 10 verticals built around load-bearing visuals: CAD blueprints, RF schematics, GIS maps, gel plates, and charts. We published 10 sample tasks on [Hugging Face](https://huggingface.co/datasets/google/multimodal-knowledge-agent).

![Multimodal Knowledge Agent benchmark quality and cost comparison](https://www.philschmid.de/static/blog/google-deepmind-roundup-nov-2026/mka-results.webp)

## 7\. Beyond the API: Gemini Robotics 2.5 and AlphaGenome 2

We also shipped two domain models:

- [Gemini Robotics 2.5](https://deepmind.google/blog/gemini-robotics-2-5-accelerates-real-world-physical-intelligence/): our whole-body [Vision-Language-Action model](https://deepmind.google/models/gemini-robotics/vla/) across arms and humanoids. On live Mercedes-Benz assembly lines, Apptronik's Apollo 2 humanoid sorted and kitted metal brackets at a 96% success rate.
- [AlphaGenome 2](https://deepmind.google/blog/alphagenome-2-deciphering-how-cells-read-dna-to-better-understand-disease/): our single-cell DNA model trained on 107M+ cell profiles across 68 tissues. Researchers can pass scRNA-seq profiles without retraining via the [AlphaGenome API](https://deepmind.google/science/alphagenome/) for non-commercial research to predict cell-type-specific variant effects.

## Start building with Cloud credits

We added [Google Developer Program Cloud credit claiming](https://blog.google/innovation-and-ai/technology/developers-tools/google-cloud-credits-gdp-ai-studio/) directly into Google AI Studio. Google AI Pro and Ultra subscribers can now [claim and track their monthly Cloud credits in AI Studio](https://aistudio.google.com/learn/claim-google-ai-plan-credits-in-ai-studio) alongside [API billing](https://aistudio.google.com/docs/billing) to offset Gemini 4, Gemini Decisions API, and agent usage.

Start building today or leave feedback below!
