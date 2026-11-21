---
title: 'Announcing jj-musubi: live sync for Jujutsu'
link: https://www.serpentine.com/posts/2026/announcing-jj-musubi/
source: lobste-rs
published: 2026-11-20T09:39:11Z
updated: 2026-11-20T09:39:11Z
first_seen: 2026-11-21T19:04:53.634570545Z
authors:
- serpentine.com via Picnoir
labels:
- release
- vcs
summary: Comments
content: extracted
html: 2026-11-20-announcing-jj-musubi-live-sync-for-jujutsu.html
remote_preview:
  url: https://www.serpentine.com/software/jj-musubi/social.jpg
---

Pick up unfinished Jujutsu work live, on another machine, without temporary commit or pushing a branch.

18 November 2026 · 5 min · Bryan O'Sullivan

I’m pleased to announce a new source control tool, [jj-musubi](https://www.serpentine.com/software/jj-musubi/), that live-syncs copies of a Jujutsu (`jj`) repository across multiple machines. Every time jj snapshots my work, jj-musubi syncs it to every machine in moments, and also commits, rebases, fetches, and so on.

This simple capability is surprisingly powerful.

- I often work on one machine, and build or test on others. Now they get my changes as I make them, and I don’t need to remember whether I was editing on my laptop or someplace else.
- If I fetch changes from or push changes to upstream, all of my copies stay up to date.

jj-musubi makes repos on multiple machines feel like jj workspaces (worktrees if you speak git).

It’s intended for use by one person who works across several machines, so it has no security model (or rather, that *is* the extent of its security model).

## Things you can do

Jujutsu already treats the working copy as a commit. Running `jj status` records a snapshot of my edits in that commit, which jj-musubi then syncs to my other machines. I like to have my shell prompt run `jj status`, so in practice I rarely have to think about snapshots.

jj-musubi does not write to the working copy. Each machine has its own working copy and workspaces as usual, so syncs of commits, snapshots, and rewrites can bring in new history while a build is running, without changing the files under it. On a laptop, I can work offline, and my machine will sync when I’m back on the network.

This has also proven handy when I have agents working on several machines. I can give each agent its own jj workspace on different systems, then run `jj diff -r devbox/agent-1@` on the laptop to see what one of them has done so far. If one finds a bug that another needs to fix, they can coordinate and see each other’s work without overhead.

## Ephemeral uses

jj-musubi refers to a collection of copies of a repo as a *hive*.

I often run agents in temporary disposable VMs. jj-musubi supports this by allowing an agent to *lease* its repo’s membership in the hive: if it doesn’t renew that lease in time, its repo gets forgotten about, so temporary repos or vanished containers can’t just stack up and clutter things.

## Why I wanted this

Back when I was at Meta, I kicked off the project that became Sapling and Mononoke. Sapling’s Commit Cloud gives engineers there a similar way to move work between machines, and Google engineers have something similar with an internal jj server.

Since then, I’d missed that everyday convenience for the machines I already use, and felt like I could get it without massive infrastructure or hosting. With jj-musubi, one machine runs a hub, and daemons exchange jj history with it over SSH. That’s almost as lightweight as you can get.

## Safety

What’s tricky is getting concurrent work *right*. That’s already hard in any distributed source control system, and when syncs are live *and* we plan for disconnected work in an unreliable network, the difficulty goes way up.

As a user, I shouldn’t have to care about when exactly I can safely close my laptop after finishing an edit; all of my changes should make it through either just before I close it, or right after I reopen. No work should get lost or corrupted in the presence of interrupts, resends, delays, or machines sleeping, and the system should stay reliable even when machines go offline forever.

I used Lean to prove key properties of jj-musubi’s network protocol, and tests compare the Rust code against that model. An extensive body of other tests run real jj repositories through concurrent edits, interrupted syncs, hub restarts, and a myriad of other nasty situations. Mutation and adversarial testing ensure that when bugs are injected, sensible things happen. The [safety guide](https://www.serpentine.com/software/jj-musubi/safety/) details all the checks (and the parts I don’t yet cover).

## Concurrent edits without commits…!?

It might sound spooky or dangerous to be able to see in-progress work on one machine from another. How can that be safe?

If you use jj in multiple workspaces, you’re already used to the novelty of being able to check out the same change in two workspaces and modify it. jj simply marks the change as divergent, but it won’t update either working copy with changes from the other until you decide to, e.g. by calling `jj workspace update-stale`. In jj, dealing with divergent versions of a change is similar to any other kind of conflicted merge.

jj-musubi uses jj’s existing workspace machinery, so everything will feel familiar. The only difference is that when you run jj-musubi, a workspace’s name is prefixed with the name of the machine it lives on.

## Try it

Start with e.g. a laptop and another machine that’s always on (let’s call it `devbox`), with a copy of a jj repo on each. On each machine, [download the binary](https://github.com/bos/jj-musubi/releases/) or build it yourself:

```sh
cargo install --locked jj-musubi
```

Run `musubi hub` on `devbox`; it will keep your sync data in `~/musubi-hub`. In the laptop’s repo, run `musubi create --hub devbox`. In `devbox`’s repo, run `musubi join`.

I provide [builds for Linux, macOS and Windows](https://github.com/bos/jj-musubi/releases/), and the [quick start](https://www.serpentine.com/software/jj-musubi/get-started/) walks through setup and the first sync. If you try it and something surprises you, I’d like to [hear about it](https://github.com/bos/jj-musubi/issues).
