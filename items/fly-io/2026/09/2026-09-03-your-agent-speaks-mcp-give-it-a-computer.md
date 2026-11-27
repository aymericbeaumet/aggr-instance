---
title: Your Agent Speaks MCP. Give It a Computer.
link: https://fly.io/blog/sprites-mcp/
source: fly-io
published: 2026-09-03T00:00:00Z
updated: 2026-11-16T04:21:03Z
first_seen: 2026-11-27T00:01:05.630507547Z
content: extracted
html: 2026-09-03-your-agent-speaks-mcp-give-it-a-computer.html
remote_preview:
  url: https://fly.io/blog/sprites-mcp/assets/whack.webp
  alt: Fly whackamole
extra:
  thumbnail: https://fly.io/blog/sprites-mcp/assets/whack.webp
---

Author

![Kurt Mackey](https://fly.io/static/images/kurt.webp)

Name

Kurt Mackey

@mrkurt

[@mrkurt](https://twitter.com/mrkurt)

![Fly whackamole](https://fly.io/blog/sprites-mcp/assets/whack.webp)\
 Image by [Annie Ruygt](https://annieruygtillustration.com/)

Sprites are disposable cloud computers. They appear instantly, always include durable filesystems, and cost practically nothing when idle. They’re the best and safest place on the Internet to run agents and we want you to [create dozens of them](https://sprites.dev/ "").

**Update, November 2026:** the Sprites MCP server now lives inside the Fly MCP at `https://mcp.fly.io`, and we are transitioning away from the `sprites.dev/mcp` endpoint this post describes. New setups should use the new endpoint.

Sprites are a place to run agents; the first thing you should think to do with a new Sprite is to type `claude` (or `gemini` or `codex`). We’ve put a [lot of effort](https://fly.io/blog/design-and-implementation/ "") into making sure coding agents feel safe and happy when they’re on Sprites, because, to (probably) quote John von Neumann, “happy agents are productive agents.”

What’s less obvious about Sprites is that they’re great tools *for* agents. Want three different versions of a new feature? A test environment? An ensemble of cooperating services? It’s super handy to be able to start your prompts, “`On a new Sprite, do…`”.

The Sprites API is simple, discoverable, and designed for this use case. The only real question is how your agent reaches it. For most of you the answer is MCP, and the setup is already written.

## You Don’t Have To Pick

There’s an argument going around that MCP is the wrong way to extend an agent, and that command line tools and discoverable APIs are the Right Way. Half of that argument is correct, and it’s the important half, so let’s take it seriously.

Dumping thirty tool descriptions into a context window is a bad way to teach anything. Not every Sprite command matters in every session, and cramming them all in signals to the model that they all matter to you. If you’re not using network policies, `gemini` shouldn’t burn a single token learning to configure them. Capabilities should reveal themselves progressively, the way they do when an agent works out a CLI one subcommand at a time.

The wrong half is treating that as a case against MCP. Progressive disclosure is a question of what you say to the model. MCP is a question of how the bytes get there: transport, auth, structured results, a tool the model can call instead of a command whose flags it has to guess. Those are different layers, and nothing stops you from having both. Our plugins are what having both looks like.

Install [the Claude Code plugin](https://github.com/superfly/sprites-claude-plugin "") and you get the hosted MCP server underneath and skills on top. What lands in your context is roughly a sentence about when you’d want a fresh computer. The rest arrives when it’s needed. Codex, Cursor, Antigravity, opencode, Grok and the others [work the same way](https://fly.io/run-agent-code/ ""), and if yours isn’t on that list, point it at the bare endpoint and it still works.

The server got better at its half of the job too. A file read comes back as an MCP resource instead of a wall of pasted text, so an agent can point at a file without swallowing it. Every tool carries safety annotations: read-only operations are marked read-only, destructive ones are marked destructive, and `exec` and `service_start` are flagged as the two that reach past the Sprite’s boundary. A client that pays attention to those can treat “list my checkpoints” differently from “run this thing.”

And the shell never went anywhere. There’s a `sprite` CLI, there’s a plain REST API, and every Sprite carries machine-readable docs at `/.sprite/llm.txt` that teach an agent working inside it how the place operates. If your agent would rather write little scripts than call tools, let it. It’s the same API underneath either way.

## sprites.dev/mcp

Plug this URL into Claude Desktop, or any other agent tool that speaks MCP. You’ll authenticate to one of your Fly.io organizations, and your agent will speak Sprites.

Then:

`On a new Sprite, take this repository and reproduce this bug from issues/913, capturing logs.`

`On a new Sprite, benchmark this function across 1000 runs and summarize the results.`

`On a new Sprite, update all the dependencies on this project to their newest versions and test that everything works.`

`On 3 new Sprites, change this service to use each of these 3 query libraries, and use HTTP to test latency.`

`On a new Sprite, run this code with bpfwatch and show me what files it touches.`

`On a new Sprite, run a load generator against this endpoint for 60 seconds and report the results.`

`On a new Sprite, download this dataset and give me a Jupyter notebook to explore it in.`

`On a new Sprite, set up a webhook receiver and render a real-time web report of all the payloads it receives.`

I don’t know. You know your projects better than we do. Whatever. Sometimes you want a clean, cheap, disposable computer (or five of them). That’s now an available feature of all your prompts. Find ways to apply it to your project, and we think you’ll end up wondering where Sprites have been all your life.

Some of you are thinking to yourself: “this feature is going to result in robots ruining my life”. We agree. So we’ve built in guardrails. When you authenticate you’re handing your agent a single specific organization on your Fly.io account, and you can scope the session down from there. It defaults to a cap of five Sprites and an `mcp-` name prefix, so the robots are easy to spot and easy to disassemble. Both are yours to change.

## Fuck Stateless Sandboxes

We’ll keep saying this until our faces turn blue: the industry is stuck on “sandboxes” as a way of letting agents run code, and sandboxes aren’t good enough anymore. What agents want is real computers, with real filesystems, connected to real networks, and there’s no technical reason not to give them some.

[We designed Sprites so that you can fearlessly create whole bunches of them](https://fly.io/blog/code-and-let-live/ ""). They’re responsive enough to host web apps for your team, but they idle in a sleeping state where they cost virtually nothing. Everybody at Fly.io that uses them ends up with 20 or 30, just hanging around.

We think you’ll do better work when you can pull in as many computers as you need to solve problems. Your agent is already asking for one. Say yes.

Next post ↑

[SBD: We Have a Filesystem at Home](https://fly.io/blog/sprites-block-device/)

Previous post ↓

[Turn And Face The Strange](https://fly.io/blog/kurt-scott-money-sprites/)
