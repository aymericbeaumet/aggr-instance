---
title: 'Atuin Threads: All your agent sessions on every machine'
link: https://blog.atuin.sh/threads-agent-sessions/
source: hnrss-org
published: 2026-11-11T14:10:46Z
updated: 2026-11-11T14:10:46Z
first_seen: 2026-11-11T21:51:32.649893059Z
authors:
- linsomniac
content: extracted
html: 2026-11-11-atuin-threads-all-your-agent-sessions-on-every-machine.html
remote_preview:
  url: https://storage.ghost.io/c/66/46/6646b777-02d8-44b3-9d01-dd721730b905/content/images/size/w160/2025/10/me.jpg
  alt: Ellie Huxtable
---

> Atuin Threads: one searchable, synced history for every agent session, on every machine
>
> Start a task in Claude Code, then resume it in Codex on another machine — with all context intact. Install, then run `atuin resume`

0:00

/0:13

![](https://storage.ghost.io/c/66/46/6646b777-02d8-44b3-9d01-dd721730b905/content/media/2026/10/resume_thumb.jpg)

Over the past year or two, how I use my terminal has changed dramatically. For years I was the sole user of my shell, and everything in my Atuin history was authored by me. Now, more than half the commands running on my machines weren’t written by me.

These days, much of my work happens through a coding agent, typed into a little “Topsy-turvying” text box. But while my shell history follows me everywhere, my agent sessions do not. They're scattered across different tools and machines (a laptop, a dev server, ephemeral instances on [exe.dev](https://exe.dev/?ref=blog.atuin.sh), a surprisingly cheap bare metal box in Germany), difficult to search, and often forgotten once I've moved on.

It’s a familiar problem!

I started working on Atuin because the history of my work was ephemeral: every command I typed was destined to vanish on line 501 of `.bash_history`. But once this work became indexed, everlasting, and easy to recall, I could find the exact command I’d used to fix something years ago, in seconds.

Over the years, this data compounded. It became the history of all of my work, the record of how I solved problems and built things.

Shell history has always been an excellent record of what happened, but it’s only the surface. It never kept the reasoning… or the intentions behind what I was trying to do. I could only find a command if I remembered a part of it, though often I remembered what I was trying to achieve more readily.

Agent sessions store the lot of it. Every command, its output, my intent, and everything I tried along the way to the eventual solution. It’s all there, in plain English, in my home directory! But just as with shell history all those years ago, we don’t put it to work.

### Atuin Threads

That is why we built Atuin Threads.

Starting with Atuin 18.24, you can search your agent sessions across machines, resume them anywhere, or fork the session into a different agent harness. Start something in Claude Code on a dev server, then finish it in Codex on your laptop.

Behind the scenes, this was a natural extension of Atuin. We put a lot of time and effort into building a reliable, encrypted sync engine. It’s generic, and can sync anything that can be modelled as an append-only log. As it turns out, most AI sessions are exactly this.

Like shell history, Threads is end-to-end encrypted when synced, so we can't read your sessions. You can use our hosted sync server, [run your own](https://docs.atuin.sh/latest/self-hosting/server-setup/?ref=blog.atuin.sh), or keep everything local. Capturing agent sessions is opt-in, too.

Run `atuin resume`, and after setting up, you will see the TUI from the top of this post!

It doesn’t matter which agent you started with - you can resume it anywhere. With Atuin’s sync, this works across devices, and keeps your history backed up forever.

Right now, we support

- Claude Code
- Codex
- Pi
- Omp
- OpenCode

with more agents coming soon.

### MCP

With MCP, agents can search and reference previous sessions themselves. They can find work you’ve done before, understand why a decision was made, and build on previous investigations without having to dig up the context.

Our MCP is available as `atuin mcp`, and can be automatically installed with

```bash
atuin mcp install claude/codex/pi/opencode
```

There are all sorts of ways to put this history to work. A few examples:

#### Work that never shipped

I’ve iterated on our onboarding and setup so many times over the past few months, but never quite been happy with it. When I ask an agent to work on onboarding now, it finds all my previous attempts, as well as why they had been abandoned and what can be done better.

These sessions contain the code that never made it into `main`, as well as my initial prompts and iteration. Subsequent agents can use this to figure out exactly what I want, without me having to write up a giant spec.

#### Object store outage

With all the security issues going on in the industry recently, we rolled our keys as a precaution. One of the new object store keys did not have the correct permissions for avatar uploads! We didn’t notice straight away 😬

My daily Sentry triage agent picked up on it, and traced the failure down to a command run earlier that day. I hadn’t gotten around to reading it yet, but prompting another agent with

> seeing some tigris avatar issues?

led to the agent discovering the previous investigation, highlighting the cause, and developing the fix much faster than if it had done the whole thing from scratch.

#### Forking threads across machines

Before releasing Threads, I did some testing on a variety of machines and setups.

One machine did not perform well. The agent investigating proposed that this was because of poor IO performance, but that it needed something with a faster disk to confirm.

I forked the session onto my MacBook, and it continued from exactly where it left off. With the faster SSD, the investigation concluded that slow disk was only part of the story - we could speed up imports elsewhere, too.

Once the benchmarks were complete, the thread running on the original machine could pull the results from the Mac and build a fix. I didn’t have to copy-paste anything or co-ordinate across machines, and the data is now part of the record of my work.

### Teams

While this is tremendously useful as a solo engineer, it gets even more interesting with a team.

So much of what we do never makes it into Git, issue trackers, or documentation. The context behind that work ends up scattered across individual agent sessions. What if the rest of the team could learn from it too? What if we could hand off an agent thread just like a Git branch?

We’re building opt-in session sharing for teams, and would love to hear from folks using agents day to day. If this sounds useful to you, [please get in touch](mailto:ellie@atuin.sh)!

While agents have radically changed my daily workflow, shell history isn’t going anywhere, and is arguably more useful than ever. Threads is an extension of the same idea that started Atuin in the first place - the history of your work should be useful, accessible, and yours to keep.

Threads is out in Atuin 18.24+

1. Install or update Atuin with `curl --proto '=https' --tlsv1.2 -LsSf https://setup.atuin.sh | sh`
2. Run `atuin resume` to get started
3. Use the MCP with `atuin mcp install`

Please don’t hesitate to reach out with any questions or feedback!

- [GitHub](https://github.com/atuinsh/atuin?ref=blog.atuin.sh)
- [Forum](https://forum.atuin.sh/?ref=blog.atuin.sh)
- [Discord](https://discord.gg/Fq8bJSKPHh?ref=blog.atuin.sh)
