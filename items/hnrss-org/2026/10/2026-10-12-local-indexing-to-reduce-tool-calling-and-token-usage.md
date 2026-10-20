---
title: Local Indexing to reduce tool calling and token usage
link: https://tedhaley.ca/pensieve/
source: hnrss-org
published: 2026-10-12T00:03:53Z
updated: 2026-10-12T00:03:53Z
first_seen: 2026-10-12T15:12:49.996093425Z
authors:
- TedHaley
content: extracted
html: 2026-10-12-local-indexing-to-reduce-tool-calling-and-token-usage.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50049508
  num_comments: 31
  points: 100
---

## Never lose the thread.

Pensieve for developers: a local MCP server that gives AI agents like Claude Code, Codex and Cursor the right context.

**Pensieve keeps every thread, file and agent session in one place**, so you and your coding agents always pick up with the right context.

Free and open source · runs on your Mac

So much context.\
Scattered everywhere.
---

Slack threads, Jira tickets, Notion docs, spreadsheets, PRs and agent sessions. Every tool adds more, and soon you can't tell where anything came from.

All your context.\
One place.
---

Pensieve links related information into a single thread.

Retry declined card payments

acme-app · fix-card-retries

Open

1. **#payments** Renewals failing for EU cards again?Slack · 3 replies linked
2. **PAY-142** Retry soft declinesJira · In progress linked
3. N **Payments retry spec** Notion · edited Tuesday linked
4. **Decline codes by issuer** Google Sheets linked
5. **PR #412** Retry declined cardsGitHub · checks pass linked
6. **Add backoff to retry.py** Claude Code session · 20 min ago linked
7. **Soft decline rate up 12**%Gmail · from Stripe linked

**Next:** get a review on #412, then close PAY-142.

[Remember](https://tedhaley.ca/pensieve/#context) [Accelerate](https://tedhaley.ca/pensieve/#agents) [Share](https://tedhaley.ca/pensieve/#share) [Ask](https://tedhaley.ca/pensieve/#owners)

**01**Threads

One task.\
*One thread.*
---

Pensieve joins your agent sessions, branch, PRs, tickets and messages into **one thread per goal**: the problem, the goal, where it stands, and what's left to close it. Resume the last session in one click.

To do Open Loose ends Complete Forgotten

Retry declined card payments

acme-app · fix-card-retries · 3 files changed

Open

Problem

Card payments aren't retried after a soft decline, so renewals fail.

State

Backoff added in `payments/retry.py`. PR #412 is open; checks pass.

Next: get a review on #412then close PAY-142 ▶ Resume session

**#412** checks pass **PAY-142** in progress **2** sessions **#payments** 3 messages

### Sorts itself

Each thread's state comes from its sessions, PRs and tickets. Quiet work becomes a loose end, so you see what to close.

### Reads your other tools

Tickets and threads from Jira, Linear or Slack, through the MCP servers you already use. Read-only.

### Remembers over time

Each thread follows one goal across days, with a saved version at each change. Start a new agent session from any thread.

**02**Native to your agent

## Your agent starts *on the right code.*

Connected over MCP, your coding agent asks Pensieve one question and gets the few passages that matter, with file and line. **Less grepping, fewer tokens, better answers.**

Without Pensieve grep and read

› Why do card retries stop after a decline?

● Grep "retry" 212 matches

● Read payments/retry.py

● Read payments/gateway.py

● Read billing/invoices.py

● Read tests/test\_payments.py

… still looking

Tool calls**6 and counting**

With Pensieve one search

› Why do card retries stop after a decline?

● pensieve · search "card retry after decline"

  payments/retry.py:42

stop on hard\_decline

  Incident review.md

why we changed it

● pensieve · read payments/retry.py:42

Hard declines skip the retry queue.

Tool calls**2**

47%

fewer tool calls

1.8 per lookup, not 3.4

23%

cheaper

$0.039 per lookup, not $0.051

29%

faster

6.8 s per lookup, not 9.5 s

90%

of tasks solved

the same as with grep alone

Latest batch, October 2026: Claude Sonnet in Claude Code, 72 held-out tasks in four real repos (504 to 34,847 files), each run twice, with and without Pensieve. [Method and scripts](https://github.com/TedHaley/pensieve/tree/main/bench). The terminals above are illustrative.

Works with the agents you already use

Claude Code Codex Cursor Gemini CLI Qwen Code Windsurf Claude Desktop VS Code any MCP client

**03**Share a thread

## Hand off the thread, *not a wall of text.*

Share any thread as **one open HTML file**: the whole context of a goal, ready to pass on or keep a project on track. Your teammate reads the page; their agent reads the thread inside it. Pensieve links what they can open, summarizes what they can't, and leaves out your paths and secrets.

With a shared thread **85%**

With no thread **11%**

Facts a teammate's agent recalled in a fresh clone, with no wrong claims.

\~/Downloads/retry-declined-card-payments.thread.html

Retry declined card payments

Shared from Pensieve · Oct 17

Open

Goal

Retry soft declines with backoff, so renewals go through.

State

Backoff is in; PR #412 is open and its checks pass.

Next

Review #412, then close PAY-142.

⎇

PR #412 · fix-card-retrieson GitHub

Linked

\>\_

2 agent sessionswhat was tried, what's left

Summarized

±

Uncommitted changesretry.py, gateway.py

Summarized

"goal": "Retry soft declines with backoff", "next": "Review #412" …

### Open by default

No account and no server. A page anyone can open, with the thread's JSON inside for any agent.

### You approve it

Preview before you share: edit the summaries and untick anything that doesn't belong.

### Agents can share too

Your agent hands off with `share_thread`. The next one picks up from the file.

Who knows about **“billing retries”**

MR

Maya Riverawrote most of payments/retry

**62%**

JO

Jonah Okaforactive this week

**24%**

You

You2 commits nearby

**9%**

**billing/tax** Who wrote it

Daniel Kim · moved on · 78% Priya Nair · 14% Others · 8%

Priya knows it best today. Ask her, and write down what Daniel knew.

**04**Domain insights

Share more,\
*silo less.*
---

Ask about a topic, not a file, and Pensieve names the people who know it best, from git blame. It also shows **where knowledge sits with one person**, or with someone who has moved on, so your team can spread it around.

## And it stays out of your way.

⌃⇧

### Search from anywhere

Files, code and past agent sessions, by meaning or by exact words. Tap Control+Shift.

### Always current

Edit a file and search it seconds later. Uncommitted changes and every worktree count.

### Stays on your Mac

A local search model and a local index. No account, no cloud. Your own agent writes the notes.

Get\
Pensieve.
---

Free and open source. One command installs it, and your coding agent does the rest.

### 1\. Install the app in Terminal

```
curl -fsSL https://pensieve.tools/install.sh | sh
```

### 2\. Ask your agent to connect it in any coding agent

```
Pensieve is installed on my Mac. Run pensieve integrate recommended to connect my coding agents. Then run pensieve setup, show me what it found and how long each source takes to index, ask me which sources to index, and run pensieve setup with my choices.
```

Apple Silicon · macOS 14 or later · [Download the app instead](https://github.com/TedHaley/pensieve/releases/latest/download/Pensieve.dmg)

## Questions.

What does Pensieve index?

Your files and folders, your git repos with uncommitted changes and worktrees, and your agent sessions. With MCP connections, also information such as tickets from Jira or Linear and posts from Slack. You choose each source. Pensieve doesn't expose private messages.

Does anything leave my Mac?

Your files, code and sessions don't. Indexing and search run on your Mac. Pensieve downloads its models and checks GitHub for updates. Pensieve is a tool that your own agent drives. If you use a cloud agent with Pensieve, what that agent reads goes to its provider.

Do I need an AI model?

No. Search, agent context, ownership and knowledge gaps need none. The notes on threads come from the engine that you pick at setup: Claude Code with Haiku, the built-in Qwen 3.5 9B on your Mac, or a model server that you run, such as LM Studio.

How do I get my agent to use it instead of grep?

`pensieve integrate recommended` adds Pensieve as an MCP server to each agent on your Mac, plus a short “search before you grep” block and skills. In the benchmark, the agent used Pensieve in 143 or more of 144 runs.

Does it work on Intel Macs or Linux?

The app needs Apple Silicon. On Linux and Intel Macs, the same install command installs the backend and the browser app.

Is it free?

Yes. Pensieve is free and open source under the MIT license. The code is on [GitHub](https://github.com/TedHaley/pensieve).
