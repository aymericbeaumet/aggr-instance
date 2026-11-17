---
title: We don't need more apps. We need better open source
link: https://jordankrueger.com/blog/better-open-source-not-more-apps/
source: hnrss-org
published: 2026-11-17T14:45:36Z
updated: 2026-11-17T14:45:36Z
first_seen: 2026-11-17T18:26:16.162530996Z
authors:
- pandemicsoul
content: extracted
html: 2026-11-17-we-don-t-need-more-apps-we-need-better-open-source.html
remote_preview:
  url: https://jordankrueger.com/images/gallery/blog-better-open-source-not-more-apps.jpg
  alt: Flat illustration in warm earth tones of four people at a workshop bench repairing an old machine of gears, pipes and small glowing screens, with a laptop, toolbox, coffee mug and plants, while a shelf of identical new boxes sits untouched in the background
---

Every day, a dozen people post a new app they built with AI over a weekend: a habit tracker, a to-do list, another note-taking app. I’m glad! More people building things is one of the best parts of this moment.

But far less of that energy goes into the open source software we already rely on.

People call this new way of building “vibecoding.” I hate the name; it makes a real shift in who gets to write software sound like a joke. I’ll call it AI-assisted coding, and I think it could aim somewhere more useful.

Free and open source software already does most of what my clients and I need. What it often can’t do is run well on a cheap server, install in one step, or stay up without forum threads at 11pm.

## Where I’m coming from

I self-host a lot of my own stuff, and I help progressive nonprofits with their tech for a living. I run a Matrix server (Synapse, Element and a moderation bot) for a community with thousands of users. I also run apps on Coolify and Hetzner, keep my paperwork in Paperless-ngx, and own a Mac Mini that’s always one download from a full disk.

The software is usually great. Running it is the hard part. Install guides assume you know five other tools. Upgrades break things. Apps eat 4GB of RAM to serve a handful of people. Error messages tell you nothing.

My nonprofit clients would love to leave Big Tech for privacy, cost and owning their data. But the open source option usually needs a part-time sysadmin they don’t have, so they pay for the corporate tool.

## Four jobs worth your weekend

1. **Make it faster.** [uv](https://astral.sh/blog/uv) took a job Python developers do every day, installing packages, and made it 8 to 10 times faster than pip in its makers’ benchmarks (up to 115 times with a warm cache). Find the slow part of something people already use, measure it, fix it.
2. **Make it easy to install.** Too many apps need a database, a cache, a background worker, a reverse proxy and a long config file before you see a login screen. Aim for one command, sensible defaults and a setup that works the first time on a cheap server.
3. **Make it friendly to run.** Installing happens once; running it happens every day after. Give it a simple admin screen, safe upgrades, built-in backups, errors that say what to do next, and an easy, secure way to manage logins and add admins.
4. **Make it lighter.** If a tool runs happily on a $5 server, an old laptop or a Raspberry Pi, far more people can use it. Lighter software costs less to run and uses less energy.

## Why this matters

A new to-do app usually helps the few people who find it. Making a tool millions already use 20% lighter helps all of them, every day.

This is also the work that slides when maintainers are tired, and many are unpaid volunteers. We saw the risk in 2024. Someone spent years earning the trust of the worn-out solo maintainer of xz, a compression tool inside most Linux systems, then slipped a backdoor into it.

I think this unglamorous work suits AI well: profiling slow code, writing tests that make big changes safe, testing installers, rewriting cryptic errors, writing the missing docs. It takes patience more than brilliance, plus a person who checks every line.

It matters politically too. If organizations are going to own their data instead of renting it from a few giant companies, open source has to be something a 20-person nonprofit can run. I want AI’s gains shared widely. Better public tools that anyone can use, change and share are one way to get there.

## Don’t be a jerk about it

Maintainers have already seen the wrong way. The curl project [ended its bug bounty](https://daniel.haxx.se/blog/2026/01/26/the-end-of-the-curl-bug-bounty/) in January 2026 after a flood of fake, AI-generated security reports. By April, its lead developer [wrote](https://daniel.haxx.se/blog/2026/04/22/high-quality-chaos/) that the fakes were mostly gone and almost every report used AI, most of them very good. The new problem is volume. Even good work can bury a maintainer. So:

- **Use the thing first** and find what actually hurts.
- **Talk to maintainers before writing code.** Nobody wants a surprise 4,000-line pull request.
- **Bring numbers.** “Startup went from 40 seconds to 4, here’s how I measured it” beats “faster.”
- **Keep changes small and tested.**
- **Say you used AI, and own the result.** Stick around to fix what breaks.
- **If they say no, help downstream** with a better installer or setup guide, and keep it current.

## Pick one tool

Pick one open source tool you use. Find the most annoying thing about installing or running it. Spend a weekend fixing it, with the people who maintain it. Then tell me about it.

* * *

**Sources**

- [uv: Python packaging in Rust](https://astral.sh/blog/uv), Astral
- [The end of the curl bug-bounty](https://daniel.haxx.se/blog/2026/01/26/the-end-of-the-curl-bug-bounty/), Daniel Stenberg
- [High quality chaos](https://daniel.haxx.se/blog/2026/04/22/high-quality-chaos/), Daniel Stenberg
- [XZ Utils backdoor](https://tukaani.org/xz-backdoor/), Tukaani Project
