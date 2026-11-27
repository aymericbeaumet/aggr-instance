---
title: Run OpenClaw (Formerly Moltbot and Clawdbot) in a Sprite
link: https://fly.io/blog/openclaw-in-a-sprite/
source: fly-io
published: 2026-01-26T00:00:00Z
updated: 2026-11-06T16:35:06Z
first_seen: 2026-11-27T00:01:05.630507547Z
content: extracted
html: 2026-01-26-run-openclaw-formerly-moltbot-and-clawdbot-in-a-sprite.html
remote_preview:
  url: https://fly.io/blog/openclaw-in-a-sprite/assets/moltbot.webp
  alt: How to Run OpenClaw in a Sprite
extra:
  thumbnail: https://fly.io/blog/openclaw-in-a-sprite/assets/moltbot.webp
---

![How to Run OpenClaw in a Sprite](https://fly.io/blog/openclaw-in-a-sprite/assets/moltbot.webp)

## Running OpenClaw on a Sprite

There’s zero chance this will be the first time you’re hearing about OpenClaw, the project previously known as Clawdbot and then Moltbot. It’s an open-source AI assistant that connects to messaging platforms you probably already use: WhatsApp, Telegram, Slack, Discord, Signal, etc. It comes with a fast-growing list of connectors and Skills and self-iterating capabilities, right out of the box.

If you really want to see what a project like OpenClaw can do, you need to run it with generous permissions and give it access to its environment. That is inherently scary, which is why so many people have been snapping up Mac Minis to run their OpenClaw in an isolated environment.

You don’t need a Mac Mini. Instead, let’s learn how to run OpenClaw in a Sprite. (Prefer one click? [Deploy OpenClaw on a Sprite](https://fly.io/sprites/openclaw/ "") and skip the rest of this post.) A stateful sandbox with automatic sleep/wake behaviour that keeps running costs low. [Sprites](https://fly.io/sprites/ "") are fully hardware isolated Linux computers, so you can give OpenClaw total agency and not even have to think about building your own guardrails.

## Create a Sprite

Install the Sprites CLI and create a Sprite:

```wrapper
curl -fsSL https://sprites.dev/install.sh | bash
sprite create openclaw
```

Connect to the Sprite console:

```wrapper
sprite console -s openclaw
```

## Install and Configure OpenClaw

Run the OpenClaw install script:

```wrapper
curl -fsSL https://openclaw.ai/install.sh | bash
```

The wizard will configure your credentials, hooks, and messaging platforms. You’ll see two expected errors at the end:

**Systemd unavailable** — Sprites don’t use systemd

**Health check failed** — The Gateway isn’t running yet

This is normal. The onboarding creates your config. We’ll start the Gateway next.

**Important:** Copy the token shown in OpenClaw’s output. It looks like:

`9054a0cfca3f5be5325f988fb57ddf1f38e530d2883bbed7`

Add openclaw to your PATH so it’s available in future sessions:

```wrapper
echo 'export PATH="$PATH:/.sprite/languages/node/nvm/versions/node/v22.20.0/bin"' >> ~/.zshrc
source ~/.zshrc
```

## Register OpenClaw as a Sprite Service

Sprites have their own [service manager](https://docs.fly.io/sprites/concepts/services "") that auto-starts services when HTTP requests arrive. Register the Gateway:

```wrapper
 sprite-env services create openclaw-gateway \
    --cmd "$(which openclaw)" \
    --args "gateway" \
    --http-port 18789
```

This registers the Gateway and routes your Sprite URL to port 18789. When requests hit your Sprite URL, the Sprite wakes and the Gateway starts automatically.

## Make the URL Public (Optional)

By default, Sprite URLs require authentication. To make yours public and get the url:

```wrapper
sprite url update --auth public -s openclaw
```

## Access the Dashboard

For initial setup, access your Sprite URL + the token from onboarding:

`https://yoursprite.sprites.app/?token=YOUR_TOKEN_HERE`

You’ll see a “pairing required” message. This is OpenClaw’s security. It requires you to approve new devices before they can connect.

## Approve Your Device

Back in the Sprite console, list pending pairing requests:

```wrapper
openclaw devices list
```

Approve the pending request:

```wrapper
openclaw devices approve REQUEST_ID
```

Refresh your browser. You’re in.

## Checkpoint Your Setup

Checkpoint your working configuration:

```wrapper
sprite checkpoint create
```

If you ever need to return to this state:

```wrapper
sprite restore <id>
```

## What You’ve Built

Your OpenClaw Gateway now lives on a Sprite that:

- Sleeps when idle
- Wakes automatically when you hit the URL
- Restarts the Gateway service on wake
- Persists your config and pairing across restarts

Once paired, you can access the dashboard and start chatting with OpenClaw. It’s not bad at understanding its own capabilities, so just ask for the integrations and channels you want to set up.

## Bonus Level:

* * *

Tell OpenClaw to build a Sprites Skill for itself, so it knows how to manage its own environment:

> Create a Sprite skill by reading the documentation at /.sprite/llm.txt and /.sprite/docs/agent-context.md. The skill should teach you how to manage Sprite services (since there’s no systemd), create checkpoints, restart the openclaw-gateway, and check network policy. Save it to ~/.openclaw/skills/sprite/SKILL.md.
