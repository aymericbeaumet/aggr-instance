---
title: You've Got Comments
link: https://ampcode.com/news/github-listeners
source: ampcode-com
published: 2026-10-12T00:00:00Z
updated: 2026-10-12T00:00:00Z
first_seen: 2026-10-12T22:35:36.108955126Z
summary: Amp now follows up on GitHub pull requests it opens. Comments, reviews, and CI results show up in the same thread. When Amp opens a PR, the agent can now subscribe to the PR's comments, reviews, and CI for its latest commit. A failing check or a reviewer's question shows up in the same thread, and Amp already has the context to deal with it. This is enabled by our new first-class support for GitHub Webhooks. In contrast to our existing, more generic webhook support, the new GitHub Webhook integration centrally handles all requests on the Amp side and then lets you (and your agent) decide whether to forward them to a single orb or to a new one each time. An agent can subscribe to certain events or can set up a subscription that spawns new orbs. A New Orb for Every New GitHub Issue or PR Ask Amp to start a new orb for every new issue or PR in a repository. You can also limit it to ones opened with a certain label. You don't need to keep the thread where you set it up. Each new issue or PR gets its own orb, which reads it and has a summary waiting when you open the thread. Later comments on that issue or PR go to the same thread, so the conversation stays in one place. You manage these in Settings → MCP & Integrations → GitHub, where you can edit or pause them and see the orbs each one started. Read the docs to find out how to watch CI when you push to main, follow an existing issue or PR, and ask Puck what Amp is following. Install the Amp GitHub App on your repository to get started.
content: extracted
html: 2026-10-12-you-ve-got-comments.html
remote_preview:
  url: https://ampcode.com/news/github-listeners-pr-followup.avif
  alt: Amp opening a pull request, subscribing to its updates, then receiving the CI results and a review in the same thread
---

Amp now follows up on GitHub pull requests it opens. Comments, reviews, and CI results show up in the same thread.

When Amp opens a PR, the agent can now subscribe to the PR's comments, reviews, and CI for its latest commit. A failing check or a reviewer's question shows up in the same thread, and Amp already has the context to deal with it.

![Amp opening a pull request, subscribing to its updates, then receiving the CI results and a review in the same thread](https://ampcode.com/news/github-listeners-pr-followup.avif)

This is enabled by our new first-class support for [GitHub Webhooks](https://ampcode.com/docs/github#github-webhooks). In contrast to our existing, more generic [webhook support](https://ampcode.com/docs/orbs/event-driven), the new GitHub Webhook integration centrally handles all requests on the Amp side and then lets you (and your agent) decide whether to forward them to a single orb or to a new one each time. An agent can subscribe to certain events or can set up a subscription that spawns new orbs.

## A New Orb for Every New GitHub Issue or PR

Ask Amp to start a new orb for every new issue or PR in a repository. You can also limit it to ones opened with a certain label. You don't need to keep the thread where you set it up.

![A request to start a new orb for every new issue in acme/website, the subscription tool call, and Amp noting that the thread can be archived](https://ampcode.com/news/github-listeners-spawner.avif)

Each new issue or PR gets its own orb, which reads it and has a summary waiting when you open the thread. Later comments on that issue or PR go to the same thread, so the conversation stays in one place.

![A new Amp thread started for a newly opened pull request, summarizing what the pull request changes](https://ampcode.com/news/github-listeners-new-pr-v2.avif)

You manage these in **Settings → Integrations & MCP → GitHub**, where you can edit or pause them and see the orbs each one started.

![The GitHub Event Subscriptions table in Settings with two orb spawners and a pull request subscription](https://ampcode.com/news/github-listeners-settings-v3.avif)

Read the [docs](https://ampcode.com/docs/github#github-listeners) to find out how to watch CI when you push to `main`, follow an existing issue or PR, and ask Puck what Amp is following.

[Install the Amp GitHub App](https://ampcode.com/docs/github#connect-github) on your repository to get started.
