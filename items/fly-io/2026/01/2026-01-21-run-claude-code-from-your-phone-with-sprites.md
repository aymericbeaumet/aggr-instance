---
title: Run Claude Code From Your Phone With Sprites
link: https://fly.io/blog/how-sprites-accidentally-solved-mobile-development/
source: fly-io
published: 2026-01-21T00:00:00Z
updated: 2026-11-06T16:35:06Z
first_seen: 2026-11-27T00:01:05.630507547Z
content: extracted
html: 2026-01-21-run-claude-code-from-your-phone-with-sprites.html
remote_preview:
  url: https://fly.io/blog/how-sprites-accidentally-solved-mobile-development/assets/sprite-mobile.webp
  alt: Mobile development with Sprites
extra:
  thumbnail: https://fly.io/blog/how-sprites-accidentally-solved-mobile-development/assets/sprite-mobile.webp
---

Author

![Daniel Botha](https://fly.io/static/images/danielb.webp)

Name

Daniel Botha

![Mobile development with Sprites](https://fly.io/blog/how-sprites-accidentally-solved-mobile-development/assets/sprite-mobile.webp)

A few days ago, [this post](https://news.ycombinator.com/item?id=46517458 "") hit HN’s front page and generated over 400 comments. The author is talking about the setup he uses to code from his phone using Tailscale, an always-on Mac at home, SSH via Termius, and Claude Code in the terminal.

If you’re asking why anyone would want to code from their phone, you can safely stop reading now and return to a peaceful existence, where ideas only come to you when you’re sitting behind a computer. For those familiar with the need to try an idea right now, wherever you are, whatever you’re doing, you’re in the right place.

## What Was and Need No Longer Be

The HN post’s comments section showed off some genuinely impressive mobile coding setups that range from the creative to the absurdly over-engineered. Tmux sessions, VPS boxes, all the SSH clients, and a million ways to access these env’s from your phone.

For a lot of people, designing setups like this is fun. But if managing always-on servers and VPN configurations is less exciting to you than building an app in real time while brainstorming it on a hiking trail, there’s a better way. We call ‘em [Sprites](https://fly.io/sprites/ "").

## What’s a Sprite Again?

The correct answer is: Whatever you want it to be. But that isn’t super helpful in this context. So let’s start with the idea of a Sprite as the server in your mobile coding setup. Instead of running a machine 24/7 or starting and stopping a VM, use a Sprite. It’s a full Linux computer that spins up in under a second and runs only when you need it.

Your code, installed packages, and project state persist between sessions, even when the Sprite is suspended. You (or Claude) can checkpoint your Sprite at any time, which creates a restorable snapshot of the entire environment, not just your code. And since it’s a hardware-isolated computer rather than a shared container, you can run literally any code and experiment aggressively, while genuinely just not caring about breaking anything important.

If you want to unleash your code assistant, give it full file system access and tell it to build that genius app idea that occurs to you at 3am without getting out of bed, you wanna do that in a Sprite.

But wait, how are you doing this from your phone?

## Sleep Is For the Weak

We are blessed with multiple really smart guys called Chris at Fly.io. And one of them, a Christopher actually, has built a - dare I say - elegant way for you to issue orders to Claude in a Sprite from your phone. The aptly named [sprite-mobile](https://github.com/clouvet/sprite-mobile ""), a chat UI for accessing Claude Code running in YOLO mode on a sprite.

## How to Set Up Claude Code on Your Phone

1. Create a Sprite

`sprite create cheese_sandwich`

1. Download and run the Sprite-mobile install script

`curl -fsSL https://raw.githubusercontent.com/clouvet/sprite-mobile/refs/heads/main/scripts/sprite-setup.sh -o sprite-setup.sh && chmod +x sprite-setup.sh && ./sprite-setup.sh`

I shit you not. That is it.

The script will speedrun you through everything needed to get up and running:

1. Configure your sprite and git user
2. Authenticate Claude CLI, GitHub CLI, Fly.io CLI, Sprites CLI
3. Install and configure Tailscale
4. Clone and run sprite-mobile and configure a Tailscale-based Sprite network (if you want automatic discovery of other sprites in your org)
5. Set up Tailscale Serve (HTTPS for PWA support)
6. Start the Tailnet Gate (public entry point to wake up the sprite if it’s suspended)

To access this Claude-powered dev sandbox you just spun up in the same time it takes to make a midnight cheese sandwich, you’ll want to get its public url:

`sprite url update --auth public -s cheese_sandwich`

Drop that in your Tailscale-connected phone’s browser and you’re cooking.

Your Sprites automatically go to sleep after 30s of inactivity and wake up faster than a cat when you open a can of tuna a mile away (sub 1s). And you only pay for the resources your Sprites are actively using. Got 30 projects in 30 Sprites waiting for you to decide when you’re interested in them again? Cool. No charge.

Why are you still here? Check out the full details in the [sprite-mobile repo](https://github.com/clouvet/sprite-mobile "") and read more about [Sprites](https://fly.io/sprites/ "").

Next post ↑

[Run OpenClaw (Formerly Moltbot and Clawdbot) in a Sprite](https://fly.io/blog/openclaw-in-a-sprite/)

Previous post ↓

[The Design & Implementation of Sprites](https://fly.io/blog/design-and-implementation/)
