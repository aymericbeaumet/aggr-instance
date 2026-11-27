---
title: 'Feng Shui for Robots: The Fly MCP Server'
link: https://fly.io/blog/feng-shui-for-robots/
source: fly-io
published: 2026-10-23T00:00:00Z
updated: 2026-11-05T00:08:46Z
first_seen: 2026-11-27T00:01:05.630507547Z
content: extracted
html: 2026-10-23-feng-shui-for-robots-the-fly-mcp-server.html
remote_preview:
  url: https://fly.io/blog/feng-shui-for-robots/assets/mcp-server-4f95.webp
  alt: A cool server in shutter shades pouring syrup on a stack of pancakes while a bird waits with a fork
extra:
  thumbnail: https://fly.io/blog/feng-shui-for-robots/assets/mcp-server-4f95.webp
---

Author

![Daniel Botha](https://fly.io/static/images/danielb.webp)

Name

Daniel Botha

![A cool server in shutter shades pouring syrup on a stack of pancakes while a bird waits with a fork](https://fly.io/blog/feng-shui-for-robots/assets/mcp-server-4f95.webp)\
 Image by [Annie Ruygt](https://annieruygtillustration.com/)

In Databricks’ [State of AI Agents 2026](https://www.databricks.com/sites/default/files/2026-01/State-of-AI-Agents-2026-Final.pdf) report, there’s a chart from Neon’s telemetry that’s somehow both mind-blowing and unsurprising. In October 2023, AI agents created 0.1% of the databases on Neon. By October 2025 they created 80% of them.

We’re seeing this play out all across the infra landscape, including here at Fly.io, where agents have overtaken humans as our most common customer species. Thing is, we think it’s important to remember that there’s a human with aspirations behind those agents. Often one trying to build something that actually makes it to prod.

If we want to make that easy, “computers for agents” can’t just be a sandbox where an agent can vibe. Agents need a whole cloud now. Somewhere safe to try dumb stuff, sure, but also a dead simple migration path to, and all the requisite parts of, a scalable production environment.

This is kinda our whole thing right now. So we built `mcp.fly.io`: one MCP server that connects your agent to Sprites and to your Fly.io apps.

## Start with a Sprite

Building something real with an agent should start in a sandbox. The speed and scale at which you can test different approaches and solutions is easily one of the most significant benefits of building with agents. So you want them to be able to kick all the tires to find out which of them explode.

This is why the sandbox you choose in this step is so important. There are plenty of places to safely execute agent code. But what you want is to actually install packages, write files, start a server on a port, hit the server, and see what restarting the server does in real life. This is how you get from vibes to a robust product. It’s why you should tell your agent to build in a Sprite.

## Just Linux, all the way down

When your agent builds something on a Sprite, it builds it the way it would on any Linux box. Packages go where packages go. Files sit on a disk that survives sleeps and restarts. The server runs as a **service**, a long-running process that survives the Sprite rebooting. It gets an HTTPS URL, so the webhook the agent wired up can actually receive webhooks. And before the agent tries something that might break everything, it can **checkpoint** the whole filesystem and roll back when it does, in fact, break everything.

## The not-so-great migration

In what feels like a raised middle finger to their skeptics, agents are actually building things that deserve to graduate to production infra now. Like, actual products that people rely on.

The most common reason we’re seeing for migrating something out of a Sprite is the need for it to be always on. Sprites are really good at sleeping when nobody’s using them and waking on the next request. And with scheduled jobs landing soon with Sprites 2.0, serving your product from a Sprite is totally viable. But when you start to care more about availability to users than you do about the environment being perfect for your dev agent, it’s often a sign to migrate it to good ol’ Fly Machines.

Did the word “migrate” spike your blood pressure right there? Let’s reframe what migrate means now, because it really doesn’t have to be that stressful anymore. A Sprite isn’t a sandbox pretending to be a computer. It’s a real Linux machine with its own kernel, a disk that stays put and services that keep running, and so is a Fly Machine. So when it’s time to re-home your project, there’s no foreign runtime to translate out of and no function-shaped thing to reassemble into a server. Your agent built a Linux service on a Linux box, and all it’s doing is moving it to another Linux box.

The Fly MCP server talks to both Sprites and Machines. Once the app you’re moving into exists and you’ve granted the agent access to it, the agent can create the Machine, its volume and its secrets from the same conversation it built the thing in.

It’s impossible to build a catchall “promote this Sprite” flow, because who knows what you’ll build in your Sprites. But giving your agent (which built the project, and can read everything it installed and configured on the Sprite) an MCP server that connects it to everything it needs for the migration is about as close as you can get to one. Reverse-engineering a disposable environment into an always-on production one is a tough ask for any agent, but moving a Linux service between two Linux boxes is a robot cakewalk.

## Eyes on MPG

Before you ask, we are deep into investigating how to bring MPG skills to the Fly MCP server. It will be real fun to watch the agent that built your app on a Sprite and moved it to a Machine create the Postgres cluster behind it, with automatic backups and failover, and attach it to the app in the same conversation. Watch this space.

## You beta try it

Obviously you want to be able to tell people you’ve been using this since it was in beta, so [get on it](https://mcp.fly.io/ ""). When you install the Fly MCP server, you’ll authorize it in a browser. You’ll be able to select the organization, the apps your agent can work on and what it can do with Sprites. There is no API token to paste. `mcp.fly.io` only works on the apps you select: their Machines, volumes and secrets, with IP addresses and certificates readable but not writable. Discovery only returns the permissions and apps you approved, so your agent won’t even know the apps you didn’t select exist.

## Computers for agents, from prototype to production

We feel a little like overbearing parents with big expectations of the Fly MCP server. We expect it to develop into something that helps your agent do everything from building a quick, shareable demo to a stable, robust production application. And it takes a village to raise an MCP server, so we appreciate you co-parenting with us.

[Connect your agent](https://mcp.fly.io/ "") and take it for a spin.

Next post ↑

[Well, That Wasn't Fun: October 16 '26](https://fly.io/blog/well-that-wasnt-fun/)

Previous post ↓

[SBD: We Have a Filesystem at Home](https://fly.io/blog/sprites-block-device/)
