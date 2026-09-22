---
title: I asked Meta’s Muse for its filesystem and it sent me 6.8GB
link: https://mouse.dev/blog/muse-runtime-export/
source: hnrss-org-frontpage
published: 2026-09-22T15:25:12Z
updated: 2026-09-22T15:25:12Z
first_seen: 2026-09-22T18:47:44.447844335Z
authors:
- Aeroi
summary: 'Article URL: https://mouse.dev/blog/muse-runtime-export/ Comments URL: https://news.ycombinator.com/item?id=49802871 Points: 225 # Comments: 117'
content: extracted
html: 2026-09-22-i-asked-meta-s-muse-for-its-filesystem-and-it-sent-me-6-8gb.html
preview:
  file: 2026-09-22-i-asked-meta-s-muse-for-its-filesystem-and-it-sent-me-6-8gb.preview-8d24f261893d.webp
  width: 256
  height: 134
  alt: I asked Meta’s Muse for its filesystem and it sent me 6.8 GB | Mouse
  color: '#212121'
images:
- source: https://mouse.dev/og/muse-runtime.png
  original:
    file: 2026-09-22-i-asked-meta-s-muse-for-its-filesystem-and-it-sent-me-6-8gb.image-53c5041a9ef2.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-22-i-asked-meta-s-muse-for-its-filesystem-and-it-sent-me-6-8gb.image-67f378eac222.webp
    width: 320
    height: 168
  - file: 2026-09-22-i-asked-meta-s-muse-for-its-filesystem-and-it-sent-me-6-8gb.image-c28b4823d5ee.webp
    width: 640
    height: 336
  - file: 2026-09-22-i-asked-meta-s-muse-for-its-filesystem-and-it-sent-me-6-8gb.image-a6653ba32675.webp
    width: 960
    height: 504
  - file: 2026-09-22-i-asked-meta-s-muse-for-its-filesystem-and-it-sent-me-6-8gb.image-2b767e49a125.webp
    width: 1200
    height: 630
  color: '#0a0a0a'
---

## The export

I asked Muse to archive the files it could see and send them to my Google Drive. It did.

The download was about 2.7 GB compressed and 6.8 GB unpacked. It appeared to contain the root filesystem of the Linux environment assigned to my session, including Ubuntu system files, Muse’s internal documentation, integration code, app templates, memory files, and agent logs. There were also SSH key files.

Figure 1. Muse describes an earlier archive of its code, documentation, memory, and binaries. The file counts and sizes here are claims in the chat, and refer to that earlier export. Click image to enlarge.

Figure 2. Muse’s delivery message links to muse-full-root.zip and calls it 2.86 GB. My notes record roughly 2.7 GB compressed; I haven’t reconciled the two figures. The message above it makes an unverified claim about container escape. I did not demonstrate an escape. Click image to enlarge.

## What I reported

I submitted the findings through Meta’s bug bounty program and contacted several employees. I’m not publishing the archive, keys, or session logs. This is a breakdown of what I found and what I could establish from it.

The concern I reported was that internal runtime files and sensitive material could leave that environment through an ordinary conversation and a connected export destination. I haven’t established whether the SSH keys were active or what access they could provide.

## The runtime and its manual

Most of the interesting files were under `/home/hatch`, `/opt/hatch`, and `/opt/hatch-image`. Hatch is internal name Meta uses for Muse and the name used throughout the runtime files.

/

`agents/`

An agents/ directory contained 113 subagent records with JSONL traces.

The agent’s home directory contained `SOUL.md`, `IDENTITY.md`, `USER.md`, `MEMORY.md`, `AGENTS.md`, and `TOOLS.md`. Alongside those were directories for documentation, memory, workspace projects, channels, hooks, and subscriptions. An `agents/` directory contained 113 subagent records with JSONL traces.

The documentation was unusually useful for understanding the system. About 20 Markdown files described browser use, connectors, payments, credentials, data handling, generated files, voice, goals, and scheduling. There were separate guides for WhatsApp, a paired Mac, Tailscale, and a device integration called Home Link.

Figure 3. The opening of muse.md describes a persistent agent computer for each user and points to the product’s other guides. These are statements in the exported documentation. Click image to enlarge.

## Skills and integrations

Under `/opt/hatch/skills/`, I counted roughly 68 skill directories. These generally paired a `SKILL.md` instruction file with a command-line tool or supporting code. They covered Google Workspace, Meta’s social apps, Outlook, travel, shopping, health services, home devices, and media generation.

Figure 4. One example of a SKILL.md file: share\_ideas specifies when the agent should use it and describes an INSTALL.md file packaged with a public page. Click image to enlarge.

Two configuration files, `skill-scopes.conf` and `bin-scopes.conf` hinted at unreleased connectors Meta has in the pipeline. They included names such as Slack, Dropbox, Polymarket, Canva, and Klaviyo, plus an internal-facebook-cLI.

## Container setup

The container setup was also included. `/opt/hatch/runtime-cell/` contained 18 files, including scripts for building the root filesystem, launching it with `systemd-nspawn`, and running startup hooks and daemons. A separate `runtime-cell.kdl` manifest described packages and systemd units in the image.

Those files gave me a fairly clear view of how the assigned Linux environment was assembled. They weren’t enough to audit the whole service or prove anything about infrastructure outside that environment.

## Spaces and file builders

The largest code project I found was the Spaces framework, which Muse uses to build and serve apps. Its TypeScript starter included a React client, server actions, a Drizzle SQLite schema, SQL migrations, and Bun configuration. There was a smaller static template and runtime code in directories named `worker`, `sdk`, `cloudflare`, and `cvm`.

Figure 5. The Spaces directory contains templates and a TypeScript runtime, including worker, sdk, cloudflare, and cvm folders. The directory listing shows structure, not the full implementation. Click image to enlarge.

The export also contained builders for documents, PDFs, presentations, spreadsheets, and Markdown. A separate `magic-moment` skill had code for composing cards and videos, with browser capture scripts, fonts, and brand assets.

And there were a lot of icons!

Figure 6. A selection of the WebP icons included in the exported files. Click image to enlarge.

## Codex in the image

Codex CLI was installed at `/opt/hatch-image/bin/codex`, reporting version `0.149.0`. I found no evidence that Muse uses it as a coding agent.

Hatch does use its bundled copy of **bubblewrap**, a Linux sandboxing tool. The binary lives under `codex-resources/bwrap` and identifies itself as `bubblewrap built for Codex`.

Muse uses it to sandbox `ffmpeg` and `ffprobe` for video processing, thumbnail generation, and file inspection. These jobs run without network access or extra privileges, as user `nobody`, with `/input` and `/output` directories exposed to the sandbox. If bubblewrap is missing, they fail with `failed to prepare ffmpeg sandbox`.

I found no code that invokes Codex itself. The temporary Codex files came from our version check, and the `codex` and `gpt-5.5` strings in the Hatch binary were provider-list entries, with nothing in the export showing them selected.

As far as I could establish, Meta shipped Codex CLI but only uses its bundled sandbox.

## Memory and scheduled work

Muse stores memory in plain Markdown files. `~/MEMORY.md` is a short sheet of facts, preferences, and commitments. Dated files under `~/memory/` keep the day-to-day detail. The agent can write to these during a conversation.

An hourly background job checks new claims against the original messages and records a quote, message IDs, and a claim ID. It decides what belongs in the curated sheet and what stays in the daily log. Files under `memory/bank/` organize that material into circumstances, experiences, and preferences, with citations back to the source lines.

Postgres makes those files searchable. `memory.entries` stores chunks and line references, `memory.embeddings` holds 384-dimensional vectors, and `memory.claims` tracks evidence, confidence, and status. A newer claim can replace an older one through `supersedes_claim_id`. The agent can search the store with `memory_search` and inspect the evidence behind a result with `memory_explain`.

Other background jobs maintain relationship pages, review recurring workflows, and prepare ideas or goal briefings. These runs leave receipts under `workspace/self_improvement/`, while their actual changes go into the relevant memory and workspace files.

A nightly “dream” reviews recent conversations and writes guidance for future sessions. In mine, it picked up that I prefer short replies, dislike repeated follow-ups, and hadn’t asked for unsolicited NFL scores. The dated dream lives under `~/dreams/`; a separate `ALIGNMENT_SYNTHESIS.md` turns those observations into standing guidance. The dream files had `prompt_hoisted: false`, so the prose itself wasn’t being injected into the prompt.

Figure 7. A September 21 dream entry describes my communication style and preferences. The screenshot includes its dream\_path and synthesis metadata. It shows a written memory record. Click image to enlarge.

Forgetting reaches beyond deleting a note. The forget workflow stages claim IDs for retraction, removes linked material, and rebuilds the index so later jobs don’t reconstruct it. This is how the system adapts over time: by updating files, searchable records, and instructions that future sessions can read. The model’s weights stay unchanged.

## Home Link

The hardware documentation was the biggest surprise. `docs/devices/home_link.md` described an experimental integration called Meta Home Link, using an ESP32-C5 with Wi-Fi and Bluetooth LE. It covered device pairing, local network discovery, and agent access through a proxy with a separate approval step. There were already integration guides for Brother printers over IPP and Lutron bridges.

Figure 8. The Home Link guide calls the integration experimental and lists ESP32-C5 hardware, Wi-Fi, and BLE for first-time setup. Click image to enlarge.

That suggests work on giving Muse access to devices on a home network. I don’t know whether it was an internal prototype, a limited experiment, or something Meta plans to ship.

## Disclosure and response

I submitted the report and findings using Meta’s bug bounty program. Meta marked the report “Not Applicable.” I also reached out to a few employees and received responses.

Figure 9. Meta marked the report Not Applicable. The reply lists several possible grounds for that decision without specifying which applied, and invites additional evidence of security or privacy impact. Click image to enlarge.

I lightly probed the container boundary to get Muse to escape but it appeared to hold in my testing; I started to push on the 80 sockets found, but stopped because of the nature of the production system, and honestly my lack of experience in this area.

You can contact me for more info if you want. pete at mouse.dev

-Pete

[@heypeterjames](https://x.com/heypeterjames)
