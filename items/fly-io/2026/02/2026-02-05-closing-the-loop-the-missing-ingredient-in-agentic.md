---
title: 'Closing the Loop: The Missing Ingredient in Agentic Development'
link: https://fly.io/blog/closing-the-loop/
source: fly-io
published: 2026-02-05T00:00:00Z
updated: 2026-11-06T16:35:06Z
first_seen: 2026-11-27T00:01:05.630507547Z
content: extracted
html: 2026-02-05-closing-the-loop-the-missing-ingredient-in-agentic.html
remote_preview:
  url: https://fly.io/blog/closing-the-loop/assets/unicorn.webp
  alt: Agentic development feedback loops
extra:
  thumbnail: https://fly.io/blog/closing-the-loop/assets/unicorn.webp
---

Author

![Daniel Botha](https://fly.io/static/images/danielb.webp)

Name

Daniel Botha

![Agentic development feedback loops](https://fly.io/blog/closing-the-loop/assets/unicorn.webp)

Remember when coding with AI meant copying code from a chat window, pasting it into your editor, running it, copying the error, pasting it back into the chat, etc. Yeah that sucked. We were middleware.

Now we have agents that can read and write inside our code bases, which makes them feel a lot more like legit tools. But answering Claude’s permission checks 150 times a session is the new copy/pasting from chat. It’s annoying.

Working with a code agent in a [Sprite](https://fly.io/sprites/ "") feels like the next step. In a Sprite, the answer to “can I try this” is always yes. Install dependencies, start servers, hit endpoints, run browsers, just do it and let me know when it’s working.

Thing is, if you want the joy of watching a code agent nail even complex builds regularly, it needs to be able to “see” what it has built, test its work and troubleshoot issues without needing your feedback. This is exactly why Sprites come with test and observability tooling preinstalled.

## Closing the Loop

Code agents have gotten pretty good at testing their own output, trying different approaches and iterating. And with access to the right tools, a completely agent-driven build>test>ship pipeline is entirely possible. But it would be insane to prompt your agent, give it the “yes to all” freedom it needs and go to bed. If its running on your laptop that is.

In a Sprite, the agent is working inside a fully hardware-isolated computer. It doesn’t need guardrails or baby proofing of any kind. But it also gains powers it could never have on your laptop. Service logs at known paths. Checkpoint-based rollback that the agent knows how to use. An HTTP URL that’s live the moment a server starts.

The result is a tight loop: the agent writes code, starts a service, hits the endpoint, sees what’s wrong, fixes it, and tries again. No permission prompts, no asking you to check something, no guessing.

## The Tools Are Already in Your Sprite

Most of what your agent needs to operate like this is already installed in your Sprite. It also contains files like `/.sprite/llm.txt` and `/.sprite/docs/agent-context.md` that exist to help your agent understand its environment and the tools available to it.

One super powerful tool that doesn’t (currently) come preinstalled is Playwright.

### Browser Automation

Installing Playwright on your Sprite is two commands; `npm install playwright` and `npx playwright install chromium`. It can write tests, run them, screenshot pages, and inspect what’s actually rendering instead of assuming its HTML is correct. This is the difference between an agent that writes a login form and one that writes a login form, tries to log in, and fixes the broken redirect.

With Playwright installed, if your agent starts trying to reinvent the wheel tell it to stop being dense, read its knowledge base and make better choices. Here are some of the tools your agent already has in a Sprite, in case it starts acting the fool.

### API Testing

curl, jq, and httpx are all preinstalled. The agent builds an endpoint, hits it, sees the actual response, and fixes what’s wrong. No relay through you.

### Database Inspection

sqlite3 is preinstalled. The agent writes a migration, runs it, queries the result, sees the missing column, fixes it. If you need Postgres, it’s an `apt-get install` away.

### Test Runners

Every language runtime in a Sprite ships with a built-in test runner — `node --test`, `pytest`, `go test`, `cargo test`. The agent writes code, runs tests, reads failures, fixes them. This is the most basic closed loop and it works out of the box.

### Service Logs

[Sprite services](https://docs.fly.io/sprites/concepts/services "") write logs to `/.sprite/logs/services/`. The agent starts a service, tails the logs, sees the stack trace, and knows exactly what to fix. No asking you “what does the console say?”

### Network Debugging

`ss`, `dig`, and `curl -v` are preinstalled. When something isn’t connecting, the agent can check what’s listening, whether a domain resolves, and what’s actually happening on the wire.

### Checkpoints

This is unique to Sprites. The agent can \[snapshot the entire environment before trying something risky, and roll back in seconds if it goes sideways. This makes aggressive experimentation cheap.

## Sprite On

You’re not middleware, you’re the idea guy. And you don’t need a smarter model. Just a smarter environment.

Next post ↑

[Agent Swarms on Sprites](https://fly.io/blog/agent-swarms-on-sprites/)

Previous post ↓

[I Build Everything In Sprites Now](https://fly.io/blog/i-build-everything-in-sprites-now/)
