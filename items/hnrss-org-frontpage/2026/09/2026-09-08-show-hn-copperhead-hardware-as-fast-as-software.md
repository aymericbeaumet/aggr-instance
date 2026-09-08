---
title: 'Show HN: Copperhead – Hardware as Fast as Software'
link: https://copperhead.sh/
source: hnrss-org-frontpage
published: 2026-09-08T13:26:45Z
updated: 2026-09-08T13:26:45Z
first_seen: 2026-09-08T17:25:23.893129994Z
authors:
- animeshchouhan
summary: 'Article URL: https://copperhead.sh/ Comments URL: https://news.ycombinator.com/item?id=49610059 Points: 139 # Comments: 51'
content: extracted
html: 2026-09-08-show-hn-copperhead-hardware-as-fast-as-software.html
preview:
  file: 2026-09-08-show-hn-copperhead-hardware-as-fast-as-software.preview-a87c659cc740.webp
  width: 256
  height: 134
  alt: 'copperhead: an open-source AI agent for circuit board design'
  color: '#d0d0d4'
images:
- source: https://copperhead.sh/og-image.png
  original:
    file: 2026-09-08-show-hn-copperhead-hardware-as-fast-as-software.image-7380eaac29d5.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-08-show-hn-copperhead-hardware-as-fast-as-software.image-d4a372b6e885.webp
    width: 48
    height: 25
  - file: 2026-09-08-show-hn-copperhead-hardware-as-fast-as-software.image-71f465815320.webp
    width: 320
    height: 168
  - file: 2026-09-08-show-hn-copperhead-hardware-as-fast-as-software.image-15b6859ae9fa.webp
    width: 640
    height: 336
  - file: 2026-09-08-show-hn-copperhead-hardware-as-fast-as-software.image-7bd684bef63d.webp
    width: 960
    height: 504
  - file: 2026-09-08-show-hn-copperhead-hardware-as-fast-as-software.image-620977df5064.webp
    width: 1200
    height: 630
  color: '#fbfcfc'
- source: https://copperhead.sh/_astro/pcb.CaISZw8z_2oKDw0.webp
  original:
    file: 2026-09-08-show-hn-copperhead-hardware-as-fast-as-software.image-8c6f866ba8ee.webp
    width: 840
    height: 615
  variants:
  - file: 2026-09-08-show-hn-copperhead-hardware-as-fast-as-software.image-ee4fe1407416.webp
    width: 48
    height: 35
  color: '#babbd6'
---

Proven on copper

![A copperhead-designed board: the Open Telegraph key, with ESP32-S3, USB-C and the copper switch, rendered from its KiCad files.](https://copperhead.sh/_astro/pcb.CaISZw8z_2oKDw0.webp)

## Proven on real copper.

Open Telegraph: pocket-size ESP32-S3 Morse key, built end to end with this workflow. Every decision, every check, every file is public.

[Read the build story](https://chouhan.ai/building-with-claude)\
[Browse the repo](https://github.com/animesh-chouhan/open-telegraph)

How it works

## Nothing commits without its gate.

You write the brief. copperhead runs eight stages against it, each one its own agent run, each one committing only once its gate finds the work on disk. A run that goes wrong stops instead of drifting.

in

`brief.md`

what you want, written in your own words

1. 01

   ### spec

   `docs/SPEC.md`

   gatea budgets section, actually filled in

2. 02

   ### architecture

   `docs/SUBSYSTEMS.md`

   gatereal reasoning under every subsystem

3. 03

   ### parts

   `docs/BOM.md`

   gatepart numbers chosen against datasheets

4. 04

   ### schematic

   `design.kicad_sch`

   gatesymbols placed, docs agree, ERC clean

5. 05

   ### layout

   `design.kicad_pcb`

   gatefootprints on the board, DRC clean

6. 06

   ### outputs

   `gerbers / drill / STEP`

   gategerbers actually on disk

7. 07

   ### firmware

   `firmware/ / pins.h`

   gatesources on disk, pins from the pinout

8. 08

   ### dev plan

   `docs/DEVPLAN.md`

   gatea written bring-up and test plan

out

a design package

gerbers, firmware, docs. One commit per stage.

- Every hop is a gate: **no real work on disk, no commit**.
- Copper border: **verified by KiCad itself, ERC and DRC clean**.

[Read the long version, one stage at a time](https://copperhead.sh/blog/from-brief-to-gerbers)

Pricing

## Pay to not run it yourself.

copperhead is open core. The CLI is free and always will be, and it does the real work on its own. You pay to host it, to work as a team or to give an auditor what they ask for.

- ## CLI

  Free

  Apache-2.0, forever

  Design on your own, on your own machine.

  [Get started](https://copperhead.sh/#quickstart)

  Includes:

  - The full agent: do, create, init, check, watch
  - Bring your own Claude or GPT-5 key
  - Runs on your KiCad files, in your own git repo
  - Plain markdown, JSON and KiCad output. No lock-in.
  - Runs locally, never metered
  - Community support
- Most popular

  ## Cloud

  $49 per user / month

  Free for open hardware repos

  Hosted runs on private repos, with a web viewer.

  [Let's talk](https://cal.com/animeshchouhan/demo-copperhead)

  Everything in CLI, plus:

  - Hosted runs, no local setup, from anywhere
  - Private repositories
  - Web viewer: chat, live schematic and board render, ERC/DRC status
  - Run history and shareable check reports
  - One-click gerber, DXF/STEP, render and BOM export
  - BYO key, or managed inference with 200 credits a seat
- ## Team

  $49 per user / month

  \+ $199 / mo platform

  Cloud, with governance and CI for a whole team.

  [Let's talk](https://cal.com/animeshchouhan/demo-copperhead)

  Everything in Cloud, plus:

  - CI bot: run check as a required PR status check
  - Shared, versioned constraint libraries across the org
  - SSO / SAML, role management, central billing
  - Managed credits pooled across the team
  - Shared run history
- ## Enterprise

  Custom annual

  Self-hosted, built for procurement, one price a year.

  [Talk to sales](mailto:hello@copperhead.sh)

  Everything in Team, plus:

  - Self-hosted or VPC, so design data never leaves your network
  - Altium support beyond KiCad
  - RBAC, security review, dedicated support and an SLA
  - An audit trail your compliance team can hand over
  - Flat annual license, no per-run metering

FAQ

## Questions, before you ask them.

What is copperhead?

An open source AI agent that designs, documents and verifies printed circuit boards. You describe a change or hand it a product brief, and it edits your real KiCad files, updates every document that references them, and runs KiCad's own checks until they pass. [Longer introduction here](https://copperhead.sh/blog/meet-copperhead/).

What problem does it actually solve?

Drift. A hardware design spreads one decision across a schematic, a bill of materials, a power budget and several documents, and nothing breaks when they fall out of sync. The inconsistency is found at bring-up, and a respin costs 5,000 to 50,000 dollars and six to eight weeks. [The full argument is here](https://copperhead.sh/blog/drift-is-a-build-failure/).

What do I need installed?

Node 20 or newer, [KiCad](https://www.kicad.org/) with `kicad-cli` on your path and a model API key of your own. Then `npm i -g copperhead`.

Does it work on a design that already exists?

That is the main case. Point it at a KiCad repository, run `copperhead init` and start asking for changes. It can also run the full pipeline from a written brief with `copperhead create`, but iterating on real designs is what it is best at.

What will it refuse to do?

It refuses to run on a dirty git tree, refuses to edit any design file before a validated change proposal exists and refuses changes that break a budget or constraint you have documented, citing the line it would violate. It also never invents a part number it cannot justify from a datasheet.

Will it rewrite my whole schematic?

No. Edits are surgical changes to the KiCad s-expression source, so your diffs stay small and reviewable and untouched parts of the file stay byte-identical. A tool that regenerates the file to move one net has made its own work impossible to review.

[9 more questions](https://copperhead.sh/blog/faq/)

Start building

## Never ship a board your docs no longer describe.

copperhead is open source and free to use. Install once, describe the board you want in a short `brief.md` and run `create`.

```
npm i -g copperhead
export ANTHROPIC_API_KEY=<api-key>
copperhead create --brief brief.md
```

brief.md

```md
# Pocket Bluetooth speaker

A palm-size Bluetooth speaker that plays for a day on one charge.

- ESP32, Bluetooth audio (A2DP sink)
- 3 W class-D amp into a 4 Ω driver
- Li-Po cell, USB-C charging
- Standby current budget: 100 µA
```
