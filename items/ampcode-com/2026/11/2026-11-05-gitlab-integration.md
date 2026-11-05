---
title: GitLab Integration
link: https://ampcode.com/news/gitlab
source: ampcode-com
published: 2026-11-05T00:00:00Z
updated: 2026-11-05T00:00:00Z
first_seen: 2026-11-05T23:32:43.152740887Z
summary: Amp now integrates with GitLab, on gitlab.com and on self-managed instances. Connect your GitLab account and you can start projects from your GitLab repositories. Orbs clone and push as you and come with glab signed in, so the agent can open merge requests, comment on them, and read issues and pipelines. Amp can also subscribe to comments and CI for a merge request it opens. A reviewer's request or a failed pipeline lands in the thread that wrote the code, so Amp already has the context. A New Orb for Every New Merge Request or Issue Ask Amp to start a new orb for every new merge request or issue in a project. You don't need to keep the thread where you set it up. See Create an Orb Spawner. Each new merge request or issue gets its own orb, which reads it and has a summary ready when you open the thread. Later comments go to that same thread. Workspace admins can also run a spawner as a service account. You manage all of these in Settings → Integrations & MCP → GitLab under Event Subscriptions, where you can pause or delete them, edit a spawner, and see the orbs each spawner started. Set It Up Each person connects GitLab in Settings → Integrations & MCP. Then a group Owner installs Push Updates once for the workspace, with one button in Workspace Settings → Integrations & MCP. Amp also supports self-managed GitLab. See Self-Managed Instances. Connect GitLab to get started.
content: extracted
html: 2026-11-05-gitlab-integration.html
remote_preview:
  url: https://ampcode.com/og?_template=news&_version=7&title=GitLab+Integration&date=November+5%2C+2026&tagline=Connect+gitlab.com+or+your+own+GitLab+instance+to+Amp.&screenshot=https%3A%2F%2Fstatic.ampcode.com%2Fnews%2Fgitlab-mr-followup.avif&sig=879c88409ea968062fa54a6c63eeacf216a1c3fe38c3ecbaa90c9c79b5075264
---

Amp now integrates with GitLab, on gitlab.com and on self-managed instances.

Connect your GitLab account and you can start projects from your GitLab repositories. Orbs clone and push as you and come with `glab` signed in, so the agent can open merge requests, comment on them, and read issues and pipelines.

Amp can also subscribe to comments and CI for a merge request it opens. A reviewer's request or a failed pipeline lands in the thread that wrote the code, so Amp already has the context.

![An Amp thread receiving a comment on merge request !1 in amp-test/api, wrapping the README at 100 columns as the reviewer asked, then receiving the CI result: all pipelines complete](https://static.ampcode.com/news/gitlab-mr-followup.avif) ![An Amp thread receiving a comment on merge request !1 in amp-test/api, wrapping the README at 100 columns as the reviewer asked, then receiving the CI result: all pipelines complete](https://static.ampcode.com/news/gitlab-mr-followup-dark.avif)

## A New Orb for Every New Merge Request or Issue

Ask Amp to start a new orb for every new merge request or issue in a project. You don't need to keep the thread where you set it up. See [Create an Orb Spawner](https://ampcode.com/docs/gitlab#create-an-orb-spawner).

Each new merge request or issue gets its own orb, which reads it and has a summary ready when you open the thread. Later comments go to that same thread. Workspace admins can also run a spawner as a [service account](https://ampcode.com/docs/gitlab#run-a-spawner-as-a-service-account).

You manage all of these in **Settings → Integrations & MCP → GitLab** under **Event Subscriptions**, where you can pause or delete them, edit a spawner, and see the orbs each spawner started.

![The GitLab Event Subscriptions table in Settings with a spawner for new amp-test/api issues expanded to show its Spawned Orbs, above subscriptions to issue #5, issue #1, and merge request !1](https://static.ampcode.com/news/gitlab-event-subscriptions.avif) ![The GitLab Event Subscriptions table in Settings with a spawner for new amp-test/api issues expanded to show its Spawned Orbs, above subscriptions to issue #5, issue #1, and merge request !1](https://static.ampcode.com/news/gitlab-event-subscriptions-dark.avif)

## Set It Up

Each person connects GitLab in [Settings → Integrations & MCP](https://ampcode.com/settings/integrations#gitlab). Then a group Owner installs Push Updates once for the workspace, with one button in [Workspace Settings → Integrations & MCP](https://ampcode.com/workspace/integrations).

Amp also supports self-managed GitLab. See [Self-Managed Instances](https://ampcode.com/docs/gitlab#self-managed-instances).

[Connect GitLab](https://ampcode.com/docs/gitlab#connect-gitlab) to get started.
