---
title: Phoenix.new – The Remote AI Runtime for Phoenix
link: https://fly.io/blog/phoenix-new-the-remote-ai-runtime/
source: fly-io
published: 2025-06-20T00:00:00Z
updated: 2026-11-05T00:08:46Z
first_seen: 2026-11-27T00:01:05.630507547Z
content: extracted
html: 2025-06-20-phoenix-new-the-remote-ai-runtime-for-phoenix.html
remote_preview:
  url: https://fly.io/blog/phoenix-new-the-remote-ai-runtime/assets/phoenixnew-thumb.png
extra:
  thumbnail: https://fly.io/blog/phoenix-new-the-remote-ai-runtime/assets/phoenixnew-thumb.png
---

Author

![Chris McCord](https://fly.io/static/images/chris-m.webp)

Name

Chris McCord

@chris\_mccord

[@chris\_mccord](https://twitter.com/chris_mccord)

![phoenix.new orb](https://fly.io/blog/phoenix-new-the-remote-ai-runtime/assets/phoenixnew.webp)

I’m Chris McCord, the creator of Elixir’s Phoenix framework. For the past several months, I’ve been working on a skunkworks project at Fly.io, and it’s time to show it off.

I wanted LLM agents to work just as well with Elixir as they do with Python and JavaScript. Last December, in order to figure out what that was going to take, I started a little weekend project to find out how difficult it would be to build a coding agent in Elixir.

A few weeks later, I had it spitting out working Phoenix applications and driving a full in-browser IDE. I knew this wasn’t going to stay a weekend project.

If you follow me on Twitter, you’ve probably seen me teasing this work as it picked up steam. We’re at a point where we’re pretty serious about this thing, and so it’s time to make a formal introduction.

World, meet [Phoenix.new](https://phoenix.new ""), a batteries-included fully-online coding agent tailored to Elixir and Phoenix. I think it’s going to be the fastest way to build collaborative, real-time applications.

Let’s see it in action:

[www.youtube.com](https://www.youtube.com/watch?v=du7GmWGUM5Y)

## What’s Interesting About Phoenix.new

First, even though it runs entirely in your browser, Phoenix.new gives both you and your agent a root shell, in an ephemeral virtual machine (a [Fly Machine](https://docs.fly.io/machines/overview "")) that gives our agent loop free rein to install things and run programs — without any risk of messing up your local machine. You don’t think about any of this; you just open up the VSCode interface, push the shell button, and there you are, on the isolated machine you share with the Phoenix.new agent.

Second, it’s an agent system I built specifically for Phoenix. Phoenix is about real-time collaborative applications, and Phoenix.new knows what that means. To that end, Phoenix.new includes, in both its UI and its agent tools, a full browser. The Phoenix.new agent uses that browser “headlessly” to check its own front-end changes and interact with the app. Because it’s a full browser, instead of trying to iterate on screenshots, the agent sees real page content and JavaScript state – with or without a human present.

## What Root Access Gets Us

Agents build software the way you did when you first got started, the way you still do today when you prototype things. They don’t carefully design Docker container layers and they don’t really do release cycles. An agent wants to pop a shell and get its fingernails dirty.

A fully isolated virtual machine means Phoenix.new’s fingernails can get *arbitrarily dirty.* If it wants to add a package to `mix.exs`, it can do that and then run `mix phx.server` or `mix test` and check the output. Sure. Every agent can do that. But if it wants to add an APT package to the base operating system, it can do that too, and make sure it worked. It owns the whole environment.

This offloads a huge amount of tedious, repetitive work.

At his [AI Startup School talk last week](https://youtu.be/LCEmiRjPEtQ?si=sR_bdu6-AqPXSNmY&t=1902 ""), Andrej Karpathy related his experience of building a restaurant menu visualizer, which takes camera pictures of text menus and transforms all the menu items into pictures. The code, which he vibe-coded with an LLM agent, was the easy part; he had it working in an afternoon. But getting the app online took him a whole week.

With Phoenix.new, I’m taking dead aim at this problem. The apps we produce live in the cloud from the minute they launch. They have private, shareable URLs (we detect anything the agent generates with a bound port and give it a preview URL underneath `phx.run`, with integrated port-forwarding), they integrate with Github, and they inherit all the infrastructure guardrails of Fly.io: hardware virtualization, WireGuard, and isolated networks.

Github’s `gh` CLI is installed by default. So the agent knows how to clone any repo, or browse issues, and you can even authorize it for internal repositories to get it working with your team’s existing projects and dependencies.

Full control of the environment also closes the loop between the agent and deployment. When Phoenix.new boots an app, it watches the logs, and tests the application. When an action triggers an error, Phoenix.new notices and gets to work.

## Watch It Build In Real Time

[Phoenix.new](https://phoenix.new "") can interact with web applications the way users do: with a real browser.

The Phoenix.new environment includes a headless Chrome browser that our agent knows how to drive. Prompt it to add a front-end feature to your application, and it won’t just sketch the code out and make sure it compiles and lints. It’ll pull the app up itself and poke at the UI, simultaneously looking at the page content, JavaScript state, and server-side logs.

Phoenix is all about [“live” real-time](https://fly.io/blog/how-we-got-to-liveview/ "") interactivity, and gives us seamless live reload. The user interface for Phoenix.new itself includes a live preview of the app being worked on, so you can kick back and watch it build front-end features incrementally. Any other `.phx.run` tabs you have open also update as it goes. It’s wild.

## Not Just For Vibe Coding

Phoenix.new can already build real, full-stack applications with WebSockets, Phoenix’s Presence features, and real databases. I’m seeing it succeed at business and collaborative applications right now.

But there’s no fixed bound on the tasks you can reasonably ask it to accomplish. If you can do it with a shell and a browser, I want Phoenix.new to do it too. And it can do these tasks with or without you present.

For example: set a `$DATABASE_URL` and tell the agent about it. The agent knows enough to go explore it with `psql`, and it’ll propose apps based on the schemas it finds. It can model Ecto schemas off the database. And if MySQL is your thing, the agent will just `apt install` a MySQL client and go to town.

Frontier model LLMs have vast world knowledge. They generalize extremely well. At ElixirConfEU, I did a [demo vibe-coding Tetris](https://www.youtube.com/watch?v=ojL_VHc4gLk&t=3923s "") on stage. Phoenix.new nailed it, first try, first prompt. It’s not like there’s gobs of Phoenix LiveView Tetris examples floating around the Internet! But lots of people have published Tetris code, and lots of people have written LiveView stuff, and 2025 LLMs can connect those dots.

At this point you might be wondering – can I just ask it to build a Rails app? Or an Expo React Native app? Or Svelte? Or Go?

Yes, you can.

Our system prompt is tuned for Phoenix today, but all languages you care about are already installed. We’re still figuring out where to take this, but adding new languages and frameworks definitely ranks highly in my plans.

## Our Async Agent Future

[We’re at a massive step-change in developer workflows](https://fly.io/blog/youre-all-nuts/ "").

Agents can do real work, today, with or without a human present. Buckle up: the future of development, at least in the common case, probably looks less like cracking open a shell and finding a file to edit, and more like popping into a CI environment with agents working away around the clock.

Local development isn’t going away. But there’s going to be a shift in where the majority of our iterations take place. I’m already using Phoenix.new to triage `phoenix-core` Github issues and pick problems to solve. I close my laptop, grab a cup of coffee, and wait for a PR to arrive — Phoenix.new knows how PRs work, too. We’re already here, and this space is just getting started.

This isn’t where I thought I’d end up when I started poking around. The Phoenix and LiveView journey was much the same. Something special was there and the projects took on a life of their own. I’m excited to share this work now, and see where it might take us. I can’t wait to see what folks build.

Next post ↑

[The Future Isn't Model Agnostic](https://fly.io/blog/the-future-isn-t-model-agnostic/)

Previous post ↓

[What are MCP Servers?](https://fly.io/blog/mcps-everywhere/)
