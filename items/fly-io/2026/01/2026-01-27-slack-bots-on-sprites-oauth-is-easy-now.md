---
title: 'Slack Bots on Sprites: OAuth is Easy Now'
link: https://fly.io/blog/slack-bots-on-sprites/
source: fly-io
published: 2026-01-27T00:00:00Z
updated: 2026-11-06T16:35:06Z
first_seen: 2026-11-27T00:01:05.630507547Z
content: extracted
html: 2026-01-27-slack-bots-on-sprites-oauth-is-easy-now.html
remote_preview:
  url: https://fly.io/blog/slack-bots-on-sprites/assets/slack_bots.webp
  alt: build Slack bots on Sprites
extra:
  thumbnail: https://fly.io/blog/slack-bots-on-sprites/assets/slack_bots.webp
---

Author

![Daniel Botha](https://fly.io/static/images/danielb.webp)

Name

Daniel Botha

![build Slack bots on Sprites](https://fly.io/blog/slack-bots-on-sprites/assets/slack_bots.webp)

Slack bots are handy, but they typically require OAuth, which means you need a server with a public URL to handle callbacks. This is annoying. But with [Sprites](https://fly.io/sprites/ ""), you get an instant internet-accessible environment with a persistent filesystem that makes it trivial.

## OAuth Pain

Getting your Slack bot working involves jumping through these OAuth hurdles:

1. Create an endpoint to receive Slack’s authorization callback
2. Get that endpoint online with a public URL
3. Securely store tokens after the callback
4. Maintain those tokens across restarts

This requires deploying something to a host, setting up Ngrok and implementing a storage solution for tokens.

## Slack Bot Sprites

Create a Sprite for your bot:

```wrapper
sprite create slack-bot
sprite console -s slack-bot
```

Install your dependencies:

```wrapper
npm init -y
npm install @slack/bolt express
```

### Build an OAuth Handler

Now we need a server that:

1. Handles the OAuth callback from Slack
2. Stores access tokens to the filesystem
3. Loads those tokens when the server restarts
4. Implements your bot’s functionality

**Ask your code assistant to:** *“Create a server.js file that handles Slack OAuth callbacks using the Bolt framework, stores tokens to the filesystem, and implements a simple slash command. The server should run on port 8080.”*

### Register Your Slack Bot as a Sprite Service

Sprites don’t use systemd or traditional init systems. Instead, they have their own [service manager](https://docs.fly.io/sprites/concepts/services "") that enables the Sprite’s automatic sleep/wake behavior.

So you’ll want to register your bot as a service with the HTTP port flag:

```wrapper
sprite-env services create slack-bot --cmd node --args server.js --http-port 8080
```

This does three critical things:

1. **Auto-start on HTTP requests**: When a request hits your Sprite URL, the Sprite wakes from hibernation
2. **Auto-route requests:** The Sprite proxy routes incoming traffic to your specified port (8080)
3. **Persistent registration**: Your service registration survives Sprite restarts and hibernation

When idle, your Sprite hibernates to save costs and wakes in under a second when Slack sends events or users visit the install URL.

### Get Your Public URL

```wrapper
sprite url update --auth public -s slack-bot
```

### Configure Your Slack App

The rest is standard Slack stuff:

1. Visit [https://api.slack.com/apps](https://api.slack.com/apps)
2. Create a new app
3. Under “OAuth & Permissions”, add your Sprite’s URL
4. Copy your Client ID and Client Secret
5. Set them as environment variables in your Sprite:

```wrapper
sprite-env env set SLACK_CLIENT_ID=your_client_id
sprite-env env set SLACK_CLIENT_SECRET=your_client_secret
sprite-env env set SLACK_SIGNING_SECRET=your_signing_secret
```

### Install Your Bot

Visit: [https://your-slack-bot.sprites.app/slack/install](https://your-slack-bot.sprites.app/slack/install)

This triggers the OAuth flow. Slack redirects back to your Sprite, which handles the callback and stores the token in its filesystem.

### Checkpoint Your Working Setup

Once everything is working, create a checkpoint:

```wrapper
sprite checkpoint create
```

A [Sprite checkpoint](https://docs.fly.io/sprites/concepts/checkpoints "") is a snapshot of the entire environment, not just your code. So if/when you break your bot, just:

```wrapper
sprite restore <checkpoint-id>
```

## More Time for Cheeky Slack Messages

Building a Slack bot in a Sprite means no longer having to:

- Set up and maintain the server
- Configure networking and SSL
- Implement your own hibernation/wake logic (or pay for idle time)
- Handle security updates and monitoring

Instead, write your bot, deploy it, forget about it. The Sprite wakes up when Slack calls, sleeps when it doesn’t, and you only pay for the moments it’s actually working.

Next post ↑

[Make Computers Make Computers](https://fly.io/blog/make-computers-make-computers/)

Previous post ↓

[Run OpenClaw (Formerly Moltbot and Clawdbot) in a Sprite](https://fly.io/blog/openclaw-in-a-sprite/)
