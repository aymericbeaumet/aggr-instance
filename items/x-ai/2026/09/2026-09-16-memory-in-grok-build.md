---
title: Memory in Grok Build
link: https://x.ai/news/grok-build-memory
source: x-ai
published: 2026-09-16T00:00:00Z
first_seen: 2026-10-19T01:26:53.998049939Z
summary: Grok Build now carries conventions, decisions, and project facts from one session to the next. Notes are written in the background as you work and read back when you return to the project.
content: extracted
html: 2026-09-16-memory-in-grok-build.html
remote_preview:
  url: https://x.ai/images/news/grok-build-memory-og.webp
---

Grok Build now has memory. As you work, it keeps notes on the conventions, decisions, and project facts that come up, and later sessions read those notes before touching related code. Grok Build gets better the more you use it.

### Captured after the turn

After a turn completes, Grok reviews it in the background and records anything durable: conventions, decisions, and project facts. Capture runs on every completed turn and does not interrupt the session.

## [What it remembers](https://x.ai/news/grok-build-memory#what-it-remembers)

Memory holds the details most likely to matter in a later session: how the team writes and reviews code, decisions and the reasoning behind them, and durable facts about the project, from where a subsystem lives to which command runs the suite. Task state, tentative conclusions, secrets, and anything the repository or its docs already cover are left out.

Notes are kept per project, plus a global set for preferences that apply everywhere.

## [Written in the background](https://x.ai/news/grok-build-memory#written-in-the-background)

Capture runs after a turn completes and never blocks the session. Grok reviews the finished turn, writes anything worth keeping as a markdown note, and continues. Over time, `/dream` folds those notes into topic files, one per subject, so each project ends up with a small set of organized references.

When you return to a project, Grok reads the topics that cover the area it is about to work in. Instructions in the current conversation take precedence over anything in a note.

## [Browsing memory](https://x.ai/news/grok-build-memory#browsing-memory)

`/memory` opens a read-only browser of every memory file, grouped by scope, with a preview of the selected file. It is the fastest way to see what a session produced, and to find the file to edit when a note is wrong.

## [New commands](https://x.ai/news/grok-build-memory#new-commands)

- `/memory` opens the browser above.
- `/dream` organizes recent notes into topic files, for example `topics/testing.md`. Dream also runs on its own periodically in the background.

## [Availability](https://x.ai/news/grok-build-memory#availability)

Memory is available in Grok Build now. It applies to new sessions: run `/new` or start a fresh `grok`, and notes begin after the first completed turn.
