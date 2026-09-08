---
title: Agent Dashboard in Grok Build
link: https://x.ai/news/agent-dashboard
source: x-ai-news
published: 2026-06-15T00:00:00Z
first_seen: 2026-09-08T10:17:10.166315Z
summary: Manage many coding sessions at once. See what each is doing, reply to the ones that need you, and dispatch new work.
content: extracted
html: 2026-06-15-agent-dashboard-in-grok-build.html
preview:
  file: 2026-06-15-agent-dashboard-in-grok-build.preview-e6f9601844e3.webp
  width: 256
  height: 134
  color: '#626f46'
images:
- source: https://media.x.ai/v1/website/agent-dashboard-og-22afca7c.jpg
  original:
    file: 2026-06-15-agent-dashboard-in-grok-build.image-3b917217bfa7.jpg
    width: 2400
    height: 1260
  variants:
  - file: 2026-06-15-agent-dashboard-in-grok-build.image-93a1fbab94b8.webp
    width: 48
    height: 25
  color: '#62714a'
---

The Agent Dashboard puts every Grok Build session on one screen. See what each is doing, run them in parallel, and step in only when input is needed.

Run `grok dashboard` from your shell, or `/dashboard` (`Ctrl+\`) from inside any session.

## [See every session at a glance](https://x.ai/news/agent-dashboard#see-every-session-at-a-glance)

The dashboard sorts sessions by state, with anything waiting for input pulled to the top, so you handle blockers first and leave the rest running. A quick scan shows what each session is doing and for how long, so you stay oriented without opening anything.

Spread across repos? Group by working directory with `Ctrl+S`. Subagents roll up under the session that launched them, so the list shows the work you dispatched, not the fan-out beneath it.

## [Peek and reply](https://x.ai/news/agent-dashboard#peek-and-reply)

Select a row to peek at its latest output without leaving the dashboard, then reply from there. Idle sessions send immediately; active ones queue your message until the current turn ends.

When a session requests approval or asks a question, its options appear inline. Answer with the arrow or number keys to continue. Multi-part questions arrive one at a time.

## [Dispatch new sessions](https://x.ai/news/agent-dashboard#dispatch-new-sessions)

The input at the bottom starts a new session. `Enter` dispatches it and keeps you on the dashboard; `Shift+Enter` dispatches and opens it right away. Before sending, set the model, start in plan mode, or let the session approve its own edits.

## [Take over any session](https://x.ai/news/agent-dashboard#take-over-any-session)

Open any session to take over its full conversation. Cycle to the next or previous session without returning to the list, then drop back to the dashboard when you're done. Closing the dashboard leaves every session running, and they're all there when you reopen it.

## [Get started](https://x.ai/news/agent-dashboard#get-started)

The Agent Dashboard ships with Grok Build. Install it with a single command, then run `grok dashboard`, or `/dashboard` from any session you already have open.
