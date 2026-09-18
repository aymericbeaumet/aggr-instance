---
title: Replacing Pull Requests with Delta
link: https://zed.dev/blog/delta-public-beta
source: hnrss-org-frontpage
published: 2026-09-16T14:05:44Z
updated: 2026-09-16T14:05:44Z
first_seen: 2026-09-18T16:19:55.454833534Z
authors:
- vquemener
summary: 'Article URL: https://zed.dev/blog/delta-public-beta Comments URL: https://news.ycombinator.com/item?id=49727245 Points: 107 # Comments: 61'
content: extracted
html: 2026-09-16-replacing-pull-requests-with-delta.html
preview:
  file: 2026-09-16-replacing-pull-requests-with-delta.preview-2e8bfb392ce3.webp
  width: 256
  height: 144
  color: '#1c1f21'
images:
- source: https://images.zed.dev/blog/delta-public-beta/thumbnail.webp
  original:
    file: 2026-09-16-replacing-pull-requests-with-delta.image-6f694476bac6.webp
    width: 5760
    height: 3240
  variants:
  - file: 2026-09-16-replacing-pull-requests-with-delta.image-f34a53b33834.webp
    width: 320
    height: 180
  - file: 2026-09-16-replacing-pull-requests-with-delta.image-b0016ecc2cee.webp
    width: 640
    height: 360
  - file: 2026-09-16-replacing-pull-requests-with-delta.image-3aef935cc4a8.webp
    width: 960
    height: 540
  - file: 2026-09-16-replacing-pull-requests-with-delta.image-d9bf8dab6ece.webp
    width: 1280
    height: 720
  color: '#16171b'
---

Today, we're launching the public beta of [Delta](https://delta.dev), a multiplayer environment for coding with agents and reviewing what they build. We're building Delta because agents have fundamentally changed the way we write software, but our collaborative tooling isn't keeping up.

Last week, we crossed a key milestone: we disabled pull requests on Delta's own repository. We now build and collaborate on Delta entirely within Delta.

What makes Delta different from traditional workflows is that collaboration doesn't depend on committing and pushing code. You invite teammates directly into your conversations with agents. When someone joins your thread, they see the same worktrees you do and can work with them on their own machine. Teammates can ask the same agent why you chose a `Mutex` instead of an `RwLock`. If you log off, they can keep working with the agent where you left off.

Starting today, anyone can [download Delta](https://delta.dev/download) for macOS, Linux, or Windows, use it on the web without downloading anything, and keep up with threads from a mobile browser on the go.

Richard Feldman walks through an end-to-end flow in Delta: fixing an issue, getting a teammate's review, and merging it.

## [Code review without pull requests](https://zed.dev/blog/delta-public-beta#code-review-without-pull-requests)

Since GitHub introduced pull requests over 15 years ago, they've become the standard way to ask teammates to review changes to your codebase. But with agents generating so much code, the diffs we're asking each other to review have mushroomed.

Splitting a big diff across a stack of branches can make it easier to navigate, but the decisions behind the code still need review. Smaller diffs don't supply that context. A reviewer may feed your diff into another agent to help understand it, but that agent has to piece together decisions you already worked through.

Why should your teammate's agent have to guess how you got there?

In Delta, you can invite anyone to pick up a thread where you left off, or create a dedicated review subthread. A review guides you through your branch's changes with access to the original agent's context. Each review gets its own isolated copy of the parent thread's worktrees, so you and your teammates can use agents to explore the code and try changes without disrupting the original work. If a reviewer spots a problem, they can request a revision or work with an agent to fix it themselves. Fixes made during review can be incorporated into the parent thread before you ask the agent to land the change.

A short walkthrough of making a change and getting reviews, in Delta.

## [Built on DeltaDB, compatible with Git](https://zed.dev/blog/delta-public-beta#built-on-deltadb-compatible-with-git)

Delta is built on [DeltaDB](https://zed.dev/blog/introducing-deltadb), which extends Git's content-based versioning with incremental versions based on *deltas*. It records edits between commits alongside messages from humans and agents, preserving how the code evolved throughout a thread. A commit remains the checkpoint you push, pull, and build from. DeltaDB retains the work between those checkpoints.

You don't have to move your whole team into Delta to use it.

For example, `zed-industries/zed` will remain on GitHub for now because it's where our community finds issues and submits changes. We're encouraging Zed contributors to share Delta threads alongside their pull requests. Contributors can work together in Delta while continuing to submit changes through GitHub, and teammates who never open Delta still see a normal Git repository.

## [Follow our quest to replace GitHub.com](https://zed.dev/blog/delta-public-beta#follow-our-quest-to-replace-githubcom)

It seems like everyone is in a race to replace GitHub right now. Most contenders promise better uptime on top of the same old primitives: branches, commits, and diffs.

We believe that *threads* will be the new fundamental unit of software development, and the best way to model their state is with deltas.

Pull requests are the first part of the GitHub workflow we're leaving behind. In their place is the Delta thread, and with it a way of working we call *continuous engineering*. The industry made integration continuous, then delivery, while the rest of software engineering still happened in batches. In a Delta thread, the idea, implementation, review, and landing of the changes can all happen in the same place.

We're building better alternatives for the other workflows that bring developers to GitHub.com, starting with Git storage in DeltaDB. Longer term, content-based builds could bring CI-style verification directly into the thread. For now, an agent can trigger a run with an existing CI provider and check the results before landing the change.

## [Try the public beta](https://zed.dev/blog/delta-public-beta#try-the-public-beta)

Thank you to the thousands of people who requested early access and helped us find Delta's rough edges. Delta is forming, and some of the capabilities we care most about are ahead (follow [what we're building next here](https://delta.dev/roadmap)). But it's already our daily driver: 33 of us have landed 570 changes to main since we turned off pull requests.

During the public beta, Delta is free. We'll introduce paid plans soon for individuals and teams. There will always be a free version of Delta.

[Download Delta](https://delta.dev/download), kick off an agent, invite a teammate into the thread, and feel the magic. We'd love to hear how it goes.

### Related Posts

Check out similar blogs from the Zed team.

* * *

### Looking for a better editor?

You can try Zed today on macOS, Windows, or Linux. [Download now](https://zed.dev/download)!

* * *

### We are hiring!

If you're passionate about the topics we cover on our blog, please consider [joining our team](https://zed.dev/jobs) to help us ship the future of software development.
