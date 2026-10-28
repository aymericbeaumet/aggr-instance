---
title: Why Agents Love Supabase
link: https://latent.space/p/supabase/
source: latent-space
published: 2026-10-28T19:48:05Z
updated: 2026-10-28T19:48:05Z
first_seen: 2026-10-28T21:53:36.421640500Z
authors:
- swyx .ai
summary: Paul Copplestone and Ant Wilson on building the backend for AI agents
content: extracted
html: 2026-10-28-why-agents-love-supabase.html
remote_preview:
  url: https://i.ytimg.com/vi/zHG6sAss_kE/maxresdefault.jpg
  alt: Why Agents Love Supabase
extra:
  audio_type: audio/mpeg
  audio_url: https://podcasts.beehiiv.com/3967c1f2-cb97-4eb0-a5c0-21a1d2579408/01a15fdf-0e93-7e1a-880d-d73752a1f915/01a1794a-94ec-7f36-ab76-412cdc9ed579/01a183eb-8968-7863-ad65-64165119cad1.mp3
  duration_seconds: 4598
  thumbnail: https://i.ytimg.com/vi/zHG6sAss_kE/maxresdefault.jpg
---

Paul Copplestone and Ant Wilson on building the backend for AI agents

I first met Paul Copplestone and Ant Wilson in Singapore, shortly after they finished YC as [the hottest startup of YC S20](https://news.ycombinator.com/item?id=23319901&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv). Six years later, we finally got them on the podcast! This is their first podcast together, after taking an open-source, fully remote company from zero to a $10 billion valuation.

The title is a question I wanted to understand for myself: **why do agents love Supabase?** There are plenty of managed databases. Postgres hosting is competitive. Being familiar to a model is useful distribution, but it doesn't explain all the work needed to turn a generated application into something that runs. But when I ran [my frontier AEO tracker](https://latent.space/p/aeo/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), it was an almost complete sweep of [first choice for app backends](https://aeo.latent.space/entities?entity=Supabase&entityId=platform%3A4d7594e57f7d03f35ede2d31&cat=integrated-backend&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) (and in [contention for managed db](https://aeo.latent.space/entities?entity=Supabase&entityId=platform%3A4d7594e57f7d03f35ede2d31&cat=managed-databases&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv)) for every frontier model from Astra to Fable to Muse.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/b88865e1-46d8-4a72-8537-2a8cfc9d12c6/image.png?t=1793216715)

At Select, Supabase grouped its announcements into Build, Operate and Scale.

The things that stood out to me were surprisingly practical: start a local backend without Docker, keep the desired database schema in Git, and run compute beside the database. Each removes a piece of work that developers have been doing for years. Agents make that friction harder to ignore.

## Give the agent a backend it can read

My favorite developer-tool announcements make me think: of course I want this today. The new alpha runtime for running Supabase locally without Docker is one of those. I use Colima rather than Docker Desktop, and even then, removing another dependency from the path to a working database is welcome. Local setup is a small part of a human developer's week; an agent that repeatedly creates and tests environments encounters it over and over.

[Declarative schemas](https://supabase.com/blog/declarative-schemas-2-0-pg-delta?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) are the bigger idea. The familiar SQL workflow accumulates a history: create the schema, then migrate, migrate, migrate. A declarative representation gives the agent a description of the state you want, in files it can inspect and edit alongside the application.

In the commentary, I compare this to a Terraform moment for the database. Put the desired state in Git and the agent can read it, change it, fork it and work against it as part of the repository. Humans get the same advantage: the backend becomes easier to understand without reconstructing its entire history.

That is the part I like most about the agent-friendly direction. These are improvements I wanted as a developer already. Making them useful to agents gives the team another reason to finish the job.

## Compute deserves more than a bullet point

I think [Supabase Compute](https://supabase.com/blog/select-2026-build-anything?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), now in private alpha, may be undersold. An application needs somewhere to do its work as well as somewhere to store the result. Once you have a database, you still need a place to run the processes that chunk documents, generate embeddings or do other longer-running work.

Paul and Ant discuss running containers close to the database. That closes a gap between getting an application started and operating it: fewer separate services to configure, connect and explain to the agent. Edge functions remain useful, but they don't cover every workload.

The operating side matters too. Logs, checks, connections and context help an agent understand what it is changing. A model that can issue SQL still needs to know what the database contains, how the application uses it, and what happened when something failed. Supabase's [Operate announcements](https://supabase.com/blog/select-2026-operate-with-confidence?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) are worth reading alongside the new ways to build.

## From prototype to petabyte

The founders say Supabase is launching about five million databases a month. They are thinking about what happens if agents push that toward billions of applications. At the same time, existing customers are growing into much larger workloads. Serving more small databases and supporting a customer's increasingly large database are different engineering problems.

That helps explain the breadth of the [Scale announcements](https://supabase.com/blog/select-2026-scale-without-limits?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv): OrioleDB, Multigres and the [Turso acquisition](https://supabase.com/blog/supabase-is-acquiring-turso?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv). The conversation gets into the work behind the new “prototype to petabyte” positioning, including Postgres reliability and the choices involved in scaling up and scaling out.

One observation from Paul and Ant is particularly interesting for anyone building developer tools: as AI-driven usage grew, they say activation and conversion improved. Their explanation is that an agent can carry a user from signup to a working setup in one flow. The person doesn't have to figure out every configuration step before seeing the application work.

We also talk about the memes and community that helped Supabase grow, open-source adoption and enterprise sales, and whether agents will converge on the same application patterns. But I wanted this episode to capture the founders' chemistry as well. It is a chance to hear Paul and Ant think through the product together—and to see how much work sits behind a backend that an agent can make look easy.

## Show notes

- [Supabase Compute](https://supabase.com/compute?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — run workloads alongside the database.

- [Multigres](https://supabase.com/docs/guides/database/multigres?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — the team’s approach to highly available Postgres, currently in private alpha.

- [Supabase acquires Turso](https://supabase.com/blog/supabase-is-acquiring-turso?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — the database architecture behind its scale-out ambitions.

- [Supabase Select](https://select.supabase.com/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) — the event discussed in this conversation.

## Chapters

[0:00](https://youtu.be/zHG6sAss_kE?t=0&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) Welcome and why agents love Supabase\
[5:23](https://youtu.be/zHG6sAss_kE?t=323&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) Paul Copplestone and Ant Wilson\
[6:39](https://youtu.be/zHG6sAss_kE?t=399&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) Compute beside the database\
[8:05](https://youtu.be/zHG6sAss_kE?t=485&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) OrioleDB and Multigres\
[12:50](https://youtu.be/zHG6sAss_kE?t=770&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) Scaling up, scaling out, and Turso\
[17:53](https://youtu.be/zHG6sAss_kE?t=1073&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) Developer marketing and memes\
[23:15](https://youtu.be/zHG6sAss_kE?t=1395&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) The launch and the migration\
[31:03](https://youtu.be/zHG6sAss_kE?t=1863&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) Vectors, growth, and AI recommendations\
[41:43](https://youtu.be/zHG6sAss_kE?t=2503&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) Product-led sales and enterprise growth\
[49:38](https://youtu.be/zHG6sAss_kE?t=2978&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) Live migrations and analytics\
[58:43](https://youtu.be/zHG6sAss_kE?t=3523&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) Agent-ready local development\
[1:06:43](https://youtu.be/zHG6sAss_kE?t=4003&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) Community and company workflows
