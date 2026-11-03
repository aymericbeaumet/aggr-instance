---
title: Service Accounts
link: https://ampcode.com/news/service-accounts
source: ampcode-com
published: 2026-11-03T00:00:00Z
updated: 2026-11-03T00:00:00Z
first_seen: 2026-11-03T20:48:58.593592327Z
summary: 'Amp now has Service Accounts. A service account is a workspace-owned account that''s not tied to a single user. Service accounts are created by workspace admins. They have their own API keys and third-party credentials. Each thread created under a service account can be viewed by other members of the workspace. These threads are multiplayer too, so workspace members can send messages in them. Since anyone in the workspace can drive a service account''s threads, make sure to only assign credentials to it that you would be happy bestowing on all workspace members. When to Use Service Accounts Service accounts fit any work that belongs to the team rather than to one person. For example, threads with automations that workspace members want to have access to, even if the person who set them up is on vacation. We''ve found two patterns where service accounts work especially well. Codebase Review and Maintenance Reviewing every pull request or keeping docs accurate is work that belongs to the team, not to a single user. When a service account owns that work, anyone on the team can step into its threads and steer them. Here are some example prompts for setting that up via Puck: Trigger a Workspace-Scoped Orb for New GitHub PRs Hey Puck, let''s set up a listener so that any new PR that lands triggers a new orb and reviews the changes. **The orbs should be owned by the whole team, not just me.** Amp knows how to set up github PR listeners like this. Because the user asked for the threads to be owned by the workspace and not by the user, Amp will use a service account for these threads. A Recurring Review of Public Docs Hey Puck, **start a workspace-owned thread** which scans our public documentation every 24hr and checks it''s up to date. If you find an issue, @ mention the person responsible for the change. Service accounts are useful for continuous clean up and maintenance tasks like this, which are shared by a team rather than an individual. Programmatic Work There''s work that starts from a script, like a CI job that spawns a thread via amp -ox every time a build fails. By using a service account''s API key for that amp invocation, the thread it starts belongs to the service account instead of whoever set up the pipeline. Here''s an example: A Review Bot Running in CI run_tests if tests_failed; then AMP_API_KEY=<SERVICE_ACCOUNT_API_KEY> \ amp -ox "Tests failed. Is this a real regression or a flaky test?" fi CI and programmatic invocation of Amp is another useful use case for service-accounts, which should not be tied to any individual user.'
content: extracted
html: 2026-11-03-service-accounts.html
remote_preview:
  url: https://ampcode.com/og?_template=news&_version=7&title=Service+Accounts&date=November+3%2C+2026&tagline=Service+accounts+for+threads+that+belong+to+the+team%2C+not+to+one+person.&backgroundImage=https%3A%2F%2Fstatic.ampcode.com%2Fnews%2Fservice-accounts-cover.avif&sig=58b2c8dbae89f55a0bd4da683b8eeeabbb10f1878cbafd17d7431ac108294c9c
---

Amp now has Service Accounts. A service account is a workspace-owned account that's not tied to a single user.

Service accounts are created by workspace admins. They have their own API keys and third-party credentials.

Each thread created under a service account can be viewed by other members of the workspace. These threads are multiplayer too, so workspace members can send messages in them.

Since anyone in the workspace can drive a service account's threads, **make sure to only assign credentials to it that you would be happy bestowing on all workspace members**.

## When to Use Service Accounts

Service accounts fit any work that belongs to the team rather than to one person. For example, threads with automations that workspace members want to have access to, even if the person who set them up is on vacation.

We've found two patterns where service accounts work especially well.

**Codebase Review and Maintenance**

Reviewing every pull request or keeping docs accurate is work that belongs to the team, not to a single user. When a service account owns that work, anyone on the team can step into its threads and steer them.

Here are some example prompts for setting that up via Puck:

- Trigger a Workspace-Scoped Orb for New GitHub PRs

  Hey Puck, let's set up a listener so that any new PR that lands triggers a new orb and reviews the changes. **The orbs should be owned by the whole team, not just me.**

  Amp knows how to [set up github PR listeners](https://ampcode.com/news/github-listeners) like this. Because the user asked for the threads to be owned by the workspace and not by the user, Amp will use a service account for these threads.

- A Recurring Review of Public Docs

  Hey Puck, **start a workspace-owned thread** which scans our public documentation every 24hr and checks it's up to date. If you find an issue, @ mention the person responsible for the change.

  Service accounts are useful for continuous clean up and maintenance tasks like this, which are shared by a team rather than an individual.

**Programmatic Work**

There's work that starts from a script, like a CI job that spawns a thread via `amp -ox` every time a build fails. By using a service account's API key for that `amp` invocation, the thread it starts belongs to the service account instead of whoever set up the pipeline.

Here's an example:

- A Review Bot Running in CI

  ```bash
  run_tests

  if tests_failed; then
    AMP_API_KEY=<SERVICE_ACCOUNT_API_KEY> \
      amp -ox "Tests failed. Is this a real regression or a flaky test?"
  fi
  ```

  CI and programmatic invocation of Amp is another useful use case for service-accounts, which should not be tied to any individual user.
