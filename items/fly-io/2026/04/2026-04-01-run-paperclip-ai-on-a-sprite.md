---
title: Run Paperclip AI on a Sprite
link: https://fly.io/blog/paperclip-on-a-sprite/
source: fly-io
published: 2026-04-01T00:00:00Z
updated: 2026-11-06T16:35:06Z
first_seen: 2026-11-27T00:01:05.630507547Z
content: extracted
html: 2026-04-01-run-paperclip-ai-on-a-sprite.html
remote_preview:
  url: https://fly.io/blog/paperclip-on-a-sprite/assets/paperclip-on-a-sprite.jpg
  alt: Running Paperclip on a Sprite
extra:
  thumbnail: https://fly.io/blog/paperclip-on-a-sprite/assets/paperclip-on-a-sprite.jpg
---

Author

![Daniel Botha](https://fly.io/static/images/danielb.webp)

Name

Daniel Botha

![Running Paperclip on a Sprite](https://fly.io/blog/paperclip-on-a-sprite/assets/paperclip-on-a-sprite.jpg)

Paperclip is extremely hot right now. I’m not going to explain what it is in any detail here because you have YouTube. But in short: it’s an open-source orchestration platform that lets you set up teams of AI agents that operate like a company. Org charts, goals, budgets, autonomous execution. It’s perfect for people who want a team of robots to do all the work that they issue from a dimly lit room in which they stroke a cat and sip from a crystal glass.

It is genuinely cool and appears to have real utility. So it’s a safe bet that Mac Minis are sold out in every store in your city again. Fortunately, Sprites people have already forgotten what it feels like to have something as trivial as hardware scarcity get in the way of building cool things.

Let’s get a [Sprite](https://fly.io/sprites/ "") to run our Paperclip on:

```wrapper
sprite create paperclip
```

Now we have a hardware-isolated Linux computer with 100GB of storage, its own URL with TLS, and a pause/resume lifecycle that means you only pay for it when it’s actually doing something.

## Setup

Node 22 is already in your Sprite. So this is the whole install:

```wrapper
npx paperclipai onboard -y
```

The `-y` flag accepts all defaults: embedded PostgreSQL, local file storage, port 3100. It creates your config, sets up the database, generates secrets. Done.

### Make It Reachable and Register the Service

Paperclip’s default config binds to `127.0.0.1` in `local_trusted` mode. You’re not sitting at this machine, so it needs to bind to `0.0.0.0` in `authenticated` mode, get told about your Sprite’s hostname, and be registered as a Sprite service. Tell Claude:

> In `~/.paperclip/instances/default/config.json`, change `server.deploymentMode` to `"authenticated"` and `server.host` to `"0.0.0.0"`. Then run `npx paperclipai allowed-hostname` with this Sprite’s hostname (get it from `sprite-env info`). Then register Paperclip as a Sprite service called “paperclip” using `sprite-env services create` with the command `npx` (use `which npx` to get the full path), args `paperclipai,run`, and `--http-port 3100`. Wait for it to start and verify the health endpoint at `http://localhost:3100/api/health` returns ok. Then grep the service logs at `/.sprite/logs/services/paperclip.log` for the `board-claim` URL, replace `http://localhost:3100` with this Sprite’s URL, and give me the link.

### Fix “This hostname is not allowed for this Paperclip instance”

If your browser shows this error instead of the Paperclip UI:

```wrapper
This hostname is not allowed for this Paperclip instance. If you want to allow a hostname, run npx paperclipai allowed-hostname <host>.
```

Paperclip only answers to hostnames on its allowlist, and it doesn’t know your Sprite’s URL yet. Add the hostname, without the `https://`, then restart the service:

```wrapper
npx paperclipai allowed-hostname your-sprite.sprites.app
sprite-env services restart paperclip
```

### Checkpoint

You’ve got a Paperclip instance with an embedded database, auth, and a live URL. Claude should checkpoint the sprite on its own, but in case it’s having a slow day:

```wrapper
sprite checkpoint create --comment "Paperclip configured and running" -s paperclip
```

## Access It From Anywhere

Your Sprite already has a URL. Check it:

```wrapper
sprite url
```

That URL, with automatic TLS, is serving your Paperclip dashboard. Open it in a browser, create your account, and start hiring your AI workforce. From your phone if you want.

The first time you hit the UI, Paperclip will ask you to claim the instance and create an admin account. This is the `authenticated` mode doing its job. After that, anyone who accesses the URL needs to log in.

Want to share it with a teammate?

```wrapper
sprite url update --auth public
```

That makes the Sprite URL reachable without Fly.io authentication. Paperclip’s own auth layer still protects the dashboard.

## Why a Sprite?

Sprites are designed to be the ideal environment for running projects like Paperclip. These are some of the baked in features that make agents way more fun to use in a Sprite.

**Checkpoints.** So you’ve achieved an inclusive and supportive corporate culture in your robot team, only to have it all go haywire thanks to an unruly new hire or a badly worded prompt? [`sprite checkpoint restore <id>`](https://docs.fly.io/sprites/concepts/checkpoints "") and the entire environment, database, config, agent state, everything, rolls back in seconds.

**Pause/resume.** It’s conceivable that some people will build fully autonomous pipelines that run 24/7 in Paperclip, but the rest of us will be issuing instructions and coming back to check on progress when we get a chance. If our agents aren’t doing anything, we don’t want to be paying to run infra. Sprites automatically pause when work isn’t being done and cost next to nothing while asleep.

**Isolation without ownership.** Each Sprite is hardware-isolated. This is entirely different to container isolation. You can sleep soundly knowing that even if your agents try the dumbest shit imaginable, it will never affect anything outside the Sprite env.

## Network Policy

Paperclip agents make HTTP requests. They call APIs, they fetch data, they do things on the internet. If you’re going to run something like this on your local machine, you need to be super careful that those agents don’t have unrestricted access to your entire home network. Every device, every NAS, every printer.

In a Sprite, [controlling exactly which domains the agents can reach](https://docs.fly.io/sprites/concepts/networking "") is simple:

```wrapper
cat /.sprite/policy/network.json
```

By default, Sprites allow common dev domains (GitHub, npm, Anthropic, etc). You can lock the policy down further from outside the Sprite so that even a compromised agent can’t phone home to somewhere unexpected. DNS resolution returns REFUSED for blocked domains. The agent doesn’t even know the domain exists.

## Robots Running Side Hustles

Want more than one robot company? [Agent swarms on Sprites](https://fly.io/blog/agent-swarms-on-sprites/ "") work the same way.

Right, I’m off to surround myself with robots smarter than me and see if that somehow results in me ever shipping a side project.

Next post ↑

[Building Agents that Don't Break Themselves](https://fly.io/blog/building-agents-that-dont-break-themselves/)

Previous post ↓

[Run a Local LLM on a CPU With BitNet and a Sprite](https://fly.io/blog/local-llm-on-a-sprite/)
