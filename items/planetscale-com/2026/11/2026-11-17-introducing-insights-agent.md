---
title: Introducing Insights Agent
link: https://planetscale.com/blog/introducing-insights-agent
source: planetscale-com
published: 2026-11-17T00:00:00Z
updated: 2026-11-17T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Elom Gomez
- Enrique Gonzalez
labels:
- product
summary: Insights Agent connect to your codebase to turn slow queries and schema recommendations into pull requests.
content: extracted
html: 2026-11-17-introducing-insights-agent.html
remote_preview:
  url: https://planetscale.com/assets/introducing-insights-agent-social-7yjbYypm.png
---

[PlanetScale Insights](https://planetscale.com/docs/postgres/monitoring/query-insights) has long shown you exactly how your queries perform in production and recommended fixes using deterministic algorithms. However, fixing what it finds has been mostly up to *you* and *your* agents.

Today we're introducing **Insights Agent**. These are agents built right into the PlanetScale platform that combine your code, database schema, and insights data in sandboxes that investigate and fix performance issues in your database and code. Better yet, you can set these up to trigger on a repeatable schedule, so you automatically get database speedup PRs into your repos.

We have run a ton of internal evaluations to tune these. We've carefully tested and chosen the right combinations of models, prompts, and sandbox environment to maximize the benefit of these agent runs.

> But I want to use *my* agent!

Great! If you prefer to have a self-improving database powered by your own agent, the [PlanetScale CLI](https://planetscale.com/docs/cli) and [MCP server](https://planetscale.com/docs/mcp-server) have you covered. Combine these with your code, custom prompts, and agent-harness of choice and you can build a similar system on your own infrastructure or third-party providers. We even have a doc dedicated to helping you set up your own [self-improving database experience](https://planetscale.com/docs/self-improving-database).

Insights Agent has the advantage of (a) being super easy to get started with, and (b) comes tuned by us, allowing us to imbue our database expertise into the agents making your software better. Getting started is as simple as:

1. Log in to the PlanetScale dashboard
2. Connect your GitHub or Cursor Origin repository

…and let Insights Agent cook. Let's look at what it does and how it works.

## [Suggestions while you sleep](https://planetscale.com/blog/introducing-insights-agent#suggestions-while-you-sleep)

At launch, Insights Agent can perform two different types of "run." Each run has a deliberately narrow focus with an instruction to create improvements to your database performance, though that might mean more than one file, query or schema change is proposed. You can initiate runs on-demand (via the app, MCP, or CLI) or on a schedule.

Within each run you'll see a timeline of its investigation and decisions made along the way.

### [Schema recommendation runs](https://planetscale.com/blog/introducing-insights-agent#schema-recommendation-runs)

These inspect the schema of your database and gather evidence from Query Insights such as rows read against rows returned, index usage patterns, and query run stats. From its sandbox, it compares that against your code and if the evidence supports a change, it opens a pull request. If not, the run finishes with a short explanation and is closed.

The agent will use conventions established in your code for its suggested changes. For example a pull request may be opened with updates as appropriate for Drizzle, Laravel, Go, or even raw SQL queries. On Vitess, a [deploy request](https://planetscale.com/docs/vitess/schema-changes/deploy-requests) will also be opened.

### [Slow query runs](https://planetscale.com/blog/introducing-insights-agent#slow-query-runs)

These look through Insights data for slow queries and follow the same path to look for code changes which should speed up slow queries. The agent evaluates possible changes within its sandbox and will either close the suggestion or create a pull request.

In either type of run, if you choose not to proceed with the pull request, the run is closed and on Vitess the deploy request is closed also.

Neither run will automatically merge or deploy any changes. Pull requests are opened as suggestions and are left up to you (or your agent) to review and either merge or close.

## [Fixes on-demand](https://planetscale.com/blog/introducing-insights-agent#fixes-on-demand)

Insights already contains recommendations and slow query pages (and has for a long time). These now have a "Fix with agent" button to perform a run against that specific issue on demand.

## [Costs and privacy](https://planetscale.com/blog/introducing-insights-agent#costs-and-privacy)

Runs are only charged if they work to completion. Costs are accumulated per-run, added to your monthly bill, and based on the tokens required to perform the investigation and create the pull request.

Spend limits can be configured by organization administrators to set a maximum spend on runs.

Insights Agent uses models from OpenAI and Anthropic with zero data retention. The sandbox environment used to perform runs only exists for the duration of the run. On Vitess, opening a deploy request can also create a development branch, and the associated branch is billed as normal.

Insights Agent is on by default, and takes no action until a repository is connected to a database. An organization administrator can turn it off from your organization page under Settings → Overview → Insights Agent.

Administrators are responsible for the integration between each database and a repository. At launch, only one GitHub or Cursor Origin repository can be connected to a database, but the same repository can be connected to multiple databases.

## [Is MCP, CLI or Insights Agent for me?](https://planetscale.com/blog/introducing-insights-agent#is-mcp-cli-or-insights-agent-for-me)

You may be asking, should I use **Insights Agent**, use my own agent, or both?

With a combination of scheduled cloud agents (eg: [Cursor automations](https://cursor.com/docs/cloud-agent/automations)), the PlanetScale MCP Server and CLI, and proper access to your git repositories, you can [string together](https://planetscale.com/docs/self-improving-database) a similar experience yourself.

Our MCP server and CLI can both be authenticated by a service token for use in automation / CI environments, and can fetch data from Query Insights. In this case, you're in charge of writing prompts for the sorts of improvements you'd like to see, or problems you're looking to solve, as well as setting up an automation schedule or on-demand trigger.

Building these processes yourself gives you more control. You may have specific quirks or requirements in your codebase, and you want to give these automations custom prompts, skills, and other context from your organization. It also may be preferable if you want to use your committed token spend or subscription usage.

In contrast, the **Insights Agent** works inside of PlanetScale's authentication, is tied to your application's code via a connection to your repository, and is given curated and targeted prompts when performing the two types of "runs" which are currently available. We manage all the moving parts: the inference, sandboxing, and creation of resulting reports and pull requests. **Insights Agent** is the perfect out-of-the-box solution for getting improvements into your database schema and code with minimal setup on your end.

## [Availability](https://planetscale.com/blog/introducing-insights-agent#availability)

Insights Agent is available today on all plans, for all engines (Vitess, Neki, and Postgres) on all hosts.

Connect your repository to get started. See the [Insights Agent documentation](https://planetscale.com/docs/insights-agent) for setup, scheduling, and billing details.
