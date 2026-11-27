---
title: Make Computers Make Computers
link: https://fly.io/blog/make-computers-make-computers/
source: fly-io
published: 2026-01-28T00:00:00Z
updated: 2026-11-06T16:35:06Z
first_seen: 2026-11-27T00:01:05.630507547Z
content: extracted
html: 2026-01-28-make-computers-make-computers.html
remote_preview:
  url: https://fly.io/blog/make-computers-make-computers/assets/mcmc.png
  alt: Sprites orchestrating other Sprites
extra:
  thumbnail: https://fly.io/blog/make-computers-make-computers/assets/mcmc.png
---

Author

![Daniel Botha](https://fly.io/static/images/danielb.webp)

Name

Daniel Botha

![Sprites orchestrating other Sprites](https://fly.io/blog/make-computers-make-computers/assets/mcmc.png)

What’s better than one [Sprite](https://fly.io/sprites/ "")? Sprites making other Sprites.

Claude (running in a Sprite) already knows how to use the [Sprites API](https://docs.fly.io/sprites/api ""). So you can ask it to create new Sprites, configure their network policies, hit their HTTP endpoints etc.

To authorise one Sprite to control others, you’ll need to visit `sprites.dev/account/{your-org}` and create a token. Then tell Claude to use it to authorise its Sprite.

Here are a few ways that the Fly team is already using this capability on the daily:

## Central Auth for Everything

A Sprite that handles OAuth flows for other Sprites.

Say you have five Sprites that all need to talk to GitHub, Slack, or some other OAuth-protected service. Each one needs tokens. Managing that across five separate environments is annoying.

Make an Auth Sprite do it for you instead. Workers call the Auth Sprite when they need a token. The Auth Sprite refreshes tokens, handles redirects and stores creds in one place.

## CI That Doesn’t Share State

Traditional CI runs tests in containers that share… something. A docker daemon. A cache directory. A network namespace.

With Sprites, each test run gets its own actual computer. Tell your orchestrator Sprite to spin up a worker for each test file, lock down its network to just what it needs (pypi, github), run the test, collect results, done.

Each test runs in full isolation. Nothing is shared. One flaky test can’t poison another.

## Preview Environments Per PR

Every pull request gets its own Sprite. Reviewers hit a real URL with a real running app.

The orchestrator watches GitHub webhooks:

- PR opened → spin up a Sprite, deploy the branch, comment the URL
- PR updated → redeploy to the same Sprite
- PR merged/closed → destroy the Sprite

No resource contention between PRs. No “sorry, all the preview slots are taken.” No shared database bleeding state between reviews.

## Agent Delegation

You have an AI agent that’s good at planning but you don’t want it directly touching production. Or you have specialized agents; one that’s great at frontend work, another that knows your database schema cold.

The manager agent runs in one Sprite. It breaks down tasks and delegates to worker agents in their own Sprites. We go deeper on this pattern in [Agent Swarms on Sprites](https://fly.io/blog/agent-swarms-on-sprites/ "").

Each worker has network policy locked down to only what it needs. The React agent can hit npm but not your production database. The DB agent can hit Postgres but not the internet.

## Running Untrusted Code

Someone (or something) gives you code. You need to run it. Fear increases. ([Here’s the longer version](https://fly.io/blog/running-untrusted-code/ "").)

Tell Claude in a Sprite to create a worker with a deny-all network policy. The worker can’t talk to anything. Run the untrusted code. Grab the output. `sprite destroy` it if it does something scary.

The code can’t phone home, can’t exfiltrate data, can’t crypto-mine on your dime.

## Team Provisioning

New developer joins. They need a Sprite with your standard tooling, your repos cloned, your services configured.

One “provisioner” Sprite has a script that:

1. Creates a new Sprite
2. Configures its network policy for your internal services
3. Clones your repos
4. Installs your standard dependencies
5. Creates a checkpoint (so they can always restore to “fresh”)
6. Sends them the URL

Onboarding is one API call.

## Bonus Level

Add checkpoints to these workflows. Your controller can checkpoint a functioning worker, then restore to that checkpoint before each job. Immutable infrastructure, but fast.

Next post ↑

[Litestream Writable VFS](https://fly.io/blog/litestream-writable-vfs/)

Previous post ↓

[Slack Bots on Sprites: OAuth is Easy Now](https://fly.io/blog/slack-bots-on-sprites/)
