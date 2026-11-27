---
title: I Build Everything In Sprites Now
link: https://fly.io/blog/i-build-everything-in-sprites-now/
source: fly-io
published: 2026-02-03T00:00:00Z
updated: 2026-11-06T16:35:06Z
first_seen: 2026-11-27T00:01:05.630507547Z
content: extracted
html: 2026-02-03-i-build-everything-in-sprites-now.html
remote_preview:
  url: https://fly.io/blog/i-build-everything-in-sprites-now/assets/buildinsprites.webp
  alt: Multiple isolated development environments
extra:
  thumbnail: https://fly.io/blog/i-build-everything-in-sprites-now/assets/buildinsprites.webp
---

Author

![Daniel Botha](https://fly.io/static/images/danielb.webp)

Name

Daniel Botha

![Multiple isolated development environments](https://fly.io/blog/i-build-everything-in-sprites-now/assets/buildinsprites.webp)

I just realised I build everything in [Sprites](https://fly.io/sprites/ "") now. It feels weird to say because Sprites are still really new (even for people who work at Fly.io). They’re obviously super exciting for people building agents, but when I finally “got” Sprites, it pretty much instantly felt weird to build anything anywhere else.

If, like me, you have to flit between many projects every day - because breaking only one thing a day is lazy - you’ll know that local development is a joke.

Let me pause what I’m working on to fix this tiny bug over on this other project! Wrong Node version. `asdf local nodejs 18.17.0`. Oh wait, this project uses nvm. Fine. `nvm use`. No `.nvmrc` file. Check the README. Last updated in 1996. Check package.json engines field. Install that version. Now the wrong packages are installed. Ah, productivity.

Oh of course, the wrong database is running. Start Postgres. Wait, this project uses MySQL. Stop Postgres, start MySQL. Except the other project’s MySQL data is still there. Where’s the docker-compose for this one?

Ah I have screwy environment variables, let me just update the … wait is it `.env`, `.env.local`, `.env.development`, `.env.example`, `env.thisone`? This project needs `API_KEY` set but that collides with the other project’s `API_KEY` which points somewhere else. Swear I have a `.env.projectname` file somewhere. Maybe. Which shell session has the right exports?

Yes! It’s running! Wait what was I doing again?

## No. Just No.

Now every project I’m working on is in its own Sprite. It feels like having a different computer for every project because that’s exactly what it is. Every Sprite is a persistent, fully hardware isolated linux computer with 100gb storage that only costs anything when it’s actually doing something. I can have 30 projects in 30 Sprites and I’m only ever paying for the one I’m currently working on.

```wrapper
sprite create dashboard
sprite create api-backend
sprite create billion-dollar-side-project
```

Each one has its own versions, its own services, its own env vars, its own ports. Nothing collides because nothing is shared.

## Waking Is Instant

Impatient? Me too! Sprites wake in a few seconds.

```wrapper
sprite console -s api-backend
```

If the api-backend Sprite is paused, it wakes up. Services come back up. I’m in the project, ready to work.

Let me hop into `sprite console -s dashboard` on another terminal quickly. Look at that, it just works. Prompt Claude in both Sprites, feel like Tony Stark

I couldn’t care less if I forget about a Sprite I was working in. It puts itself to sleep when its inactive, but nothing about its state changes. Next time I open a console, it’s exactly where I left it.

## Real URLs, Not Localhost

Every Sprite has its own URL with automatic TLS. Run your dev server on port 8080 and it’s just… there. On the internet. With HTTPS.

No ngrok. No “can you see my screen?” No tunneling. OAuth callbacks just work. Webhooks just work. That API integration that refuses to talk to localhost? It just works.

Need to show someone what you’re working on? Send them the URL. They see exactly what you see, in exactly the state it’s in. Checkpoint first if you want to freeze it there.

## Checkpoints Are the Whole Environment

Git is to checkpoints what World Series MLB is to the World Cup. Your code might be version controlled in Github, but what about … everything else? A [checkpoint](https://docs.fly.io/sprites/concepts/checkpoints "") in a Sprite is a snapshot of the entire environment. Node\_modules, database state, running services, env vars, the whole shebang

```wrapper
sprite checkpoint create
```

Wanna see if upgrading to Node 23 breaks everything? Do it! And laugh when it happens. Wait … you did checkpoint right?

Wanna tell Claude to go wild on an ambitious refactor just before you go to bed? Nighty night.

```wrapper
sprite checkpoint restore abc123
```

Git branches for experiments are so 2025.

## Specialist Claudes > One Very Confused Claude

I like to tell Claude to build a good knowledge base from project files and any other documentation I can find as a first step when I recruit it’s help on a project. A good claude.md and some project-specific skills [completely change the game](https://fly.io/blog/context-engineering-is-just-writing-things-down/ "") when it comes to how effective a coding agent can be in a given project.

That’s why I want specialist Claudes. One per project. I don’t want the context polluted or diluted by work on any other project.

What if I build a skill in one Sprite that I want in all my others? I tell Claude in that Sprite to use the Sprites API to propagate that skill to all my other project Sprites.

OK Article done.

```wrapper
sprite checkpoint create
```

Next post ↑

[Closing the Loop: The Missing Ingredient in Agentic Development](https://fly.io/blog/closing-the-loop/)

Previous post ↓

[How to Run Untrusted Code Safely](https://fly.io/blog/running-untrusted-code/)
