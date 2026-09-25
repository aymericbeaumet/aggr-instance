---
title: Opus 5.5 is good at explainer videos
link: https://launchvideo.io/
source: hnrss-org-frontpage
published: 2026-09-24T20:28:47Z
updated: 2026-09-24T20:28:47Z
first_seen: 2026-09-25T01:51:33.336207068Z
authors:
- iacguy
summary: 'Article URL: https://launchvideo.io Comments URL: https://news.ycombinator.com/item?id=49836374 Points: 132 # Comments: 89'
content: extracted
html: 2026-09-24-opus-5-5-is-good-at-explainer-videos.html
preview:
  file: 2026-09-24-opus-5-5-is-good-at-explainer-videos.preview-b5baaece9b76.webp
  width: 256
  height: 144
  alt: End card of a launch video made by LaunchVideo
  color: '#171910'
images:
- source: https://gzvxcspoxhhgoeog.public.blob.vercel-storage.com/examples/infera-gXTtuX8hOzbsPSy3SAAdqKElG42trA.jpg
  original:
    file: 2026-09-24-opus-5-5-is-good-at-explainer-videos.image-c0bc13966a33.jpg
    width: 1280
    height: 720
  color: '#0a0a0a'
---

## Ship a launch video.

Paste a URL or describe the product. Opus 5.5 writes the film and a serverless agent renders it. About four minutes and roughly 100k tokens per video.

examples

## Made by this page, untouched.

Each one is a single run: a URL or a prompt in, an MP4 out. No edits.

NVIDIA

nvidia.com

Jev, by TypeSafe AI

typesafe.ai

OpenComputer

opencomputer.dev

Linear

linear.app

Infera (from a prompt)

“make a modern slick and punchy video for a modern startup that works on inference”

how it runs

## A serverless agent on OpenComputer. Yours in one click.

The whole product is one agent file, three tools, and this form. OpenComputer runs the agent, the microVM it renders in, the model gateway, and the session API the page polls.

Agent

One OpenComputer serverless agent, defined in TypeScript and deployed with `opencomputer deploy` . No framework, no queue, no server of ours.

Model

anthropic/claude-opus-5.5 through OpenComputer's model gateway. Roughly 90k input and 15k output tokens per film, most of it the HTML itself.

Runtime

Every job is one session in a fresh microVM: Amazon Linux 2023 on arm64, 4 vCPU, 8 GB RAM, Node 22. The first tool call installs Playwright's headless Chromium and a static ffmpeg (about a minute); the VM is thrown away after.

Tools

Three `defineTool` functions. web\_fetch returns page text plus title, headings, the most used hex colors, and Google Fonts. check\_scene loads the film and reports JS errors and the visible text at sample timestamps. render\_video renders and uploads.

Rendering

No video model. The page's clocks (requestAnimationFrame, timers, Date, CSS and Web Animations) are replaced with a virtual clock, so every frame is a deterministic seek. 1920x1080 at 30 fps, JPEG frames piped into libx264, crf 18.

Storage

The agent holds no secrets. The form mints a Vercel Blob upload token scoped to one path for three hours, parks it in a per-job manifest, and the tool fetches it by job id. The finished MP4 is a public Blob URL.

Control plane

This page uses the same API the CLI does: create a session, send one turn, poll the event stream (tool.started, tool.completed, turn.completed) to show progress, and treat the MP4 appearing in Blob as done.

```
// opencomputer/agents/director/agent.ts
import { useInput, useModel, useTool } from "@opencomputer/agent";
import { checkScene, renderVideo } from "./tools/scene.js";
import { webFetch } from "./tools/web.js";

export default function Agent() {
  const input = useInput();           // the JOB block from the form
  useModel("anthropic/claude-opus-5.5");
  useTool(webFetch);                  // read the product's site
  useTool(checkScene);                // load the HTML, report errors + visible text
  useTool(renderVideo);               // headless Chromium → ffmpeg → Blob
  return `You are a motion designer who writes code. ...`;
}
```

$`npx opencomputer template deploy https://github.com/diggerhq/shipvideo`

Everything above is in the repo, and [one click deploys it to your account](https://app.opencomputer.dev/new?repository-url=https%3A%2F%2Fgithub.com%2Fdiggerhq%2Fshipvideo). The idea comes from [Deedy's post](https://x.com/deedydas/status/2102787937482252537) on Opus 5.5 and instructional video: the model writes the film as code, and code renders the same every time.
