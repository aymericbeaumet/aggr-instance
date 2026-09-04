---
title: Linktree expands social media platform coverage and ships new features with Devin
link: https://cognition.com/blog/linktree
source: cognition-com-blog
published: 2025-02-13T00:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2025-02-13-linktree-expands-social-media-platform-coverage-and-ships.html
---

## Background

[Linktree](https://linktr.ee/)’s mission is to empower anyone to own, grow, and monetize their digital presence. There are 50M+ creators and businesses using it. While the product may appear simple, it has lots of rich functionalities and it’s integrated with hundreds of partners to help people grow their audiences, sell products, and streamline their creative processes.

We at Linktree are optimistic about the power of AI to unlock greater velocity and value for creators, and the engineering team is always seeking the best AI tools available. That’s why we started exploring Devin as soon as it became generally available.

Over the past month Devin has **authored ~300 PRs** and **merged ~100**. Most of these are fixing customer-reported bugs, implementing small features, and prototyping larger features.

## Devin ships new social media platforms

To support creators, Linktree is continually evaluating what new social media platforms to add. For example, the team recently discussed adding support for RedNote and Lemon8, given their recent rise in popularity. Before Devin it would have taken the team some time to debate the priority and trade-off to take on this task. However, giving it to Devin meant skipping the lengthy prioritization process to simply see what would happen.

Adding a social platform is conceptually straightforward but it involves a few PRs spanning multiple repos; for example, the database persistence, URL parsing and validation, updating the component library and the UI repos that surface the social icon. Linktree kicked off five Devins, one for each repo and PR.

The prompt for each Devin is pretty simple:

![Linktree expands social media platform coverage and ships new features with Devin](https://cdn.sanity.io/images/2mc9cv2v/production/c694e41dea3e3c63931c09f6ab91549bfef8d6ca-1626x964.png?w=1600&fit=max)

In some repos there are quite a few files to update, so we leverage the knowledge feature to teach Devin where to look:

![Linktree expands social media platform coverage and ships new features with Devin](https://cdn.sanity.io/images/2mc9cv2v/production/b4386c172a82aa903a2f2fdc16d47bd6b27ff663-1750x1216.png?w=1600&fit=max)

The team provided some guidance to Devin about the URL format to expect from these platforms, and fixed up an issue with SVG rendering, and shipped it on the same day:

![Linktree expands social media platform coverage and ships new features with Devin](https://cdn.sanity.io/images/2mc9cv2v/production/e5addae362590e94b6ed1a0718f963d034201bde-671x384.png?w=1600&fit=max)

## Streamlining with the Devin API

Since Linktree plans to implement many more integrations like this in the future, it’s important to streamline the workflow even more. For example, instead of manually managing multiple Devins for complex changes, the team wanted Devin to be able to compose a series of PRs on its own. After a quick discussion with Cognition team, they shared a [Playbook script](https://gist.github.com/jyu-lt/428c44235ade1fbd7d8fb37f4331bdc4) that leverages the Devin API to automatically spawn multiple Devins to implement the entire feature, which Linktree was able to leverage right away:

![Linktree expands social media platform coverage and ships new features with Devin](https://cdn.sanity.io/images/2mc9cv2v/production/d47345f413c654338666f19074e2869b0fbde182-1776x742.png?w=1600&fit=max)

## What we've learned

For some tasks Devin can nail it in one shot and it feels like magic. But it also struggles with a lot of tasks, so the key is for the team to develop the intuition on what tasks are the best candidates for Devin.

Once we find a type of common task that works well, we try to operationalize it. For example, Devin has a very good success rate with instrumenting analytics event tracking. We are able to eliminate this significant workload from all feature work by leveraging Devin and keep refining the same prompt.

Understand what tasks to give Devin can be challenging, but the Linktree team learned it’s about:

- The scope of the change
- The amount of ambiguity Devin has to overcome

It’s been better to use Devin liberally for tasks that can be completed by an engineer within a couple of hours, rather than overthink the types of tasks to give Devin upfront. Devin usually produces a complete or partially useful PR, and if not then there wasn’t much time scoping and prompting lost.

Sometimes the team lets Devin work on new features, even though it's likely to struggle. Devin's attempted solution could still serve as a useful proof of concept. For example, the team recently needed a cron job in our legacy PHP backend to notify users about expiring social connections. Our limited PHP expertise made scoping the change time-consuming, so we asked Devin to implement it. That attempt led to a productive design discussion after which the team was able to provide more concrete guidance and completed the change.

Linktree’s main advice on how to work with Devin:

- Devin performs best when given concise and specific instructions so avoid chit chat
- Consider starting a new session on a sub task when the context gets too long
- Once you find a type of common task that works well, try to operationalize it

## The future with Devin

Linktree is excited about the future as Devin and the AI models continue to improve. Before Devin a lot of these bugs / features wouldn’t have made the cut in our team prioritization / scheduling. Now before spending any time debating about the priority the team asks: **Can I just Devin it?**
