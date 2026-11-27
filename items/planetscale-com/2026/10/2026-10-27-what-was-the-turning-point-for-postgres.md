---
title: What was the turning point for Postgres?
link: https://planetscale.com/blog/what-was-the-turning-point-for-postgres
source: planetscale-com
published: 2026-10-27T00:00:00Z
updated: 2026-10-27T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Simeon Griggs
labels:
- engineering
- postgres
summary: Postgres is the most popular database choice among developers. Postgres is the default choice for agents, powering countless applications where those new to app building don't even know what a database is. Every major cloud offers hosted Postgres.
content: extracted
html: 2026-10-27-what-was-the-turning-point-for-postgres.html
remote_preview:
  url: https://planetscale.com/assets/what-was-the-turning-point-for-postgres-social-CilirxPd.png
---

Postgres is the most popular database choice among developers. Postgres is the default choice for agents, powering countless applications where those new to app building don't even know what a database is. Every major cloud offers hosted Postgres.

But in a time where software development is being turned on its head, and new tooling unlocks an age of ambition never before seen, was there a pivotal moment that led to a database technology from the 80s becoming the default choice it is today?

## [Clarifying "default choice"](https://planetscale.com/blog/what-was-the-turning-point-for-postgres#clarifying-default-choice)

I can already hear your objections, mostly because I've heard them from my colleagues here at PlanetScale. Of course we still love and maintain massive MySQL workloads running on Vitess. MySQL's not dead. WordPress and its massive install base of MySQL databases haven't gone away. Meanwhile, Oracle might edge out MySQL and Postgres in revenue.

However, current data suggests Postgres is the default choice for most new applications. Whether builders choose it knowingly or their agents choose it for them.

Several [tools](https://armature.tech/leaderboards/databases) [track](https://preseason.ai/rankings/devtools/database) the trends of what choices LLMs are most likely to make. Postgres dominates as the preferred database choice.

*Preseason database rankings, share of 32,955 LLM database decisions. Hosted services are counted under the engine they run.*

So is Postgres' success due to its decades-old foundations, its ability to survive by fending off competitors, its timing in the agentic era, or something else? Was it deliberate or random? Let's look backward from today through the pivotal moments that made Postgres feel inevitable, and try to pinpoint which one had the most profound impact.

## [2025: Here comes the money](https://planetscale.com/blog/what-was-the-turning-point-for-postgres#2025-here-comes-the-money)

In 2025, investors and cloud providers put their money and time where developers were already going with acquisitions and infrastructure.

And, of course, PlanetScale (the household name in MySQL and Vitess) made PlanetScale Postgres generally available in September, bringing years of operational experience for one relational database to another.

Stack Overflow's [yearly survey](https://survey.stackoverflow.co/2025/technology#1-databases) saw Postgres take its biggest one-year jump in popularity, increasing 6.9 points. That increased preference by humans was not lost on LLMs, as agents chose Postgres when creating databases faster than humans ever had.

And so as developers looked less at the code, Postgres became the de facto choice as many people simply stopped caring (or arguing) about which database engine to choose.

Not only did agents choose Postgres, but it also became the most popular choice for building agentic applications. Developers didn't need to choose a vector-specific database engine since pgvector adds that support to Postgres.

Right now, Postgres is well-funded, available everywhere, and the preferred choice for people and agents building almost any application.

But this isn't *the* moment, rather, the growth explosion that previous decades had been leading up to.

## [2023: Postgres takes the lead](https://planetscale.com/blog/what-was-the-turning-point-for-postgres#2023-postgres-takes-the-lead)

Stack Overflow's annual survey is a reasonable proxy for developer preference and had shown Postgres' increasing popularity for five years, but 2023 was the year it finally eclipsed MySQL (45.6% to 41.1%).

*Stack Overflow Developer Survey, share of all respondents*

This could be seen as *the* moment Postgres ascended as it hasn't relinquished this lead since (and as mentioned, the gap has only widened). This is remarkable compared with technology trends that regularly come and go.

But you might also only see this as the moment Postgres's popularity was *visible*, not *inevitable*. And surveys show *stated* preferences, not *demonstrated* ones. Postgres never arrives at this moment if it doesn't survive competition that threatened not only it, but an entire class of databases.

## [2013/14: Relational holds off document databases](https://planetscale.com/blog/what-was-the-turning-point-for-postgres#201314-relational-holds-off-document-databases)

While Postgres spent many years competing with MySQL, document databases were threatening to overthrow both around this time. NoSQL databases like MongoDB promised flexible schemas with no migrations and data stored as JSON-like documents. It touted horizontal scaling and popularized ([for better or worse](https://www.youtube.com/watch?v=b2F-DItXtZs)) the phrase "web scale." Mongo was a tight fit for JavaScript and Node applications, and so the M in the MEAN/MERN stack (MongoDB) surged in popularity over the M in the LAMP stack (MySQL) of years earlier.

Relational databases simply couldn't, or chose not to, compete.

Fighting back, Postgres added JSONB support in December 2014, at the height of MongoDB's popularity. Something MySQL wouldn't get for almost another year. Postgres was now able to store, but more importantly, *index* JSON-shaped data, making it fast to access. This didn't make Postgres a direct competitor to document databases, but it at least covered much of their feature set and use cases.

> With the new JSONB data type for PostgreSQL, users no longer have to choose between relational and non-relational data stores: they can have both at the same time.

— [Postgres 9.4 Release Announcement](https://www.postgresql.org/about/news/postgresql-94-increases-flexibility-scalability-and-performance-1557/)

Instead of using distinct services for different use cases, you could stuff all your data shapes and workloads in Postgres.

This kept Postgres relevant when it and its relational siblings were under threat, but Mongo was still the predominant choice. So at this time it's hard to claim Postgres had *won*, but it certainly didn't *die*.

It would take a few years for Postgres' popularity to overtake MongoDB's. In the chart below you'll see how Postgres was threatened by, and then surpassed MongoDB before opening a gap as MongoDB's popularity declined.

*DB-Engines popularity score, each January, plus October 2026*

JSONB may have kept developers on Postgres, but it didn't bring them there. Much of the Rails and Python community had already moved to Postgres years earlier for one main reason: convenience.

## [2011: Heroku makes Postgres the default](https://planetscale.com/blog/what-was-the-turning-point-for-postgres#2011-heroku-makes-postgres-the-default)

If you're relatively new to web application development, you might not remember a time when databases couldn't just be created on demand, managed by a cloud platform.

The *peak* of technology at one time was one-click WordPress installs on your host's cPanel along with phpMyAdmin for hosted MySQL.

Heroku bet on making creating and interfacing with databases easier than ever before, and chose Postgres as the engine to offer. First offered in 2007, Heroku Postgres launched as a standalone service in November 2011. Remarkably, the product team responsible had no strong opinion of which engine to offer, it just so happened that one engineer seemed to like it.

> Most folks didn't have strong opinion, but one of our security/ops engineers chimed in "Postgres has always had a good record of being safe and reliable for data, I'd go with it." And with that, we were building and launching Heroku Postgres.

— [Unfinished Business with Postgres](https://www.craigkerstiens.com/2022/05/18/unfinished-business-with-postgres/), Craig Kerstiens

Developers followed not necessarily because they loved Postgres, but because they preferred Heroku's convenience.

Heroku also put money into Postgres development. Funding work to improve `pg_stat_statements`, working directly with Postgres developers on JSON and JSONB, pushing for `DATABASE_URL` connection strings, and later hiring contributors to work on upstream Postgres.

The audience and demand that Heroku created for Postgres attracted other providers. AWS launched RDS Postgres in 2013, which was inspired by what Heroku had done.

Heroku's investment in Postgres would pay off in the following years, with its biggest one year spike (36%) on DB-Engines rankings in 2013.

Now in a "[sustaining engineering](https://www.heroku.com/blog/an-update-on-heroku/)" model, Heroku's momentum didn't last. But the deliberate choice to make cloud-hosted databases easy, and the almost random choice of Postgres as the database engine, built an audience with those expectations. It could be the moment that led to Postgres's massive adoption today.

(If you're still on Heroku Postgres, we have tooling to help get you onto PlanetScale Postgres *fast* and with minimal downtime. Check out [Migrate from Heroku to PlanetScale](https://planetscale.com/docs/postgres/imports/heroku) for more details.)

There was little unique about Postgres that made it the choice for Heroku, so what stopped MySQL from being that choice? Especially since it dominated the early web. Its ownership perhaps played a pivotal role.

## [2010: Did Postgres increase or MySQL decrease?](https://planetscale.com/blog/what-was-the-turning-point-for-postgres#2010-did-postgres-increase-or-mysql-decrease)

Sun acquired MySQL in 2008; Oracle acquired Sun in January 2010. On the same day as the acquisition was announced in 2009, MySQL's creator forked MySQL into MariaDB because he didn't trust Oracle with it. Suddenly, the most popular open-source database fell under the purview of the largest proprietary database vendor, and its community began to fragment. While this acquisition could've been seen as a positive signal that MySQL would be well funded, the move cast doubts over its future governance and direction. The sort of doubt that makes developers think twice before choosing a database.

While the Oracle deal spooked the community, MySQL adoption continued to grow for years afterward.

Curiously, the European Commission named Postgres as a viable alternative when it approved the deal.

Postgres still lagged behind MySQL on some features, but was catching up. Postgres 9.0 shipped in late 2010 with built-in replication, considered one of the last remaining practical reasons to choose MySQL over Postgres.

Despite this, in the same year YouTube created Vitess to scale MySQL beyond a single primary database. MySQL was boasting performance and scale that few Postgres deployments had attempted.

So while Postgres' largest competitor began to lose momentum in developer communities, MySQL still did (and still does) run some of the industry's largest and most mature workloads for the web's most popular applications such as Facebook and WordPress.

This was the moment Postgres became the most likely alternative to MySQL.

But it could never have reached this position if it had simply gone away years earlier.

## [1994-96: One foot in the grave](https://planetscale.com/blog/what-was-the-turning-point-for-postgres#1994-96-one-foot-in-the-grave)

Postgres' popularity was never assured. 30 years ago it was almost dead. Less than a decade after Michael Stonebraker had created POSTGRES at Berkeley, he moved on, co-founding Illustra in 1992 to sell a commercial version and so the research project ended with version 4.2 in 1994.

Two grad students, Andrew Yu and Jolly Chen, needed a database for other projects and began creating Postgres95 from a fork of the final release from Berkeley.

They replaced Postgres' own query language PostQUEL with SQL, trimmed the codebase by about 25%, and improved performance 30-50%. What they had created got attention and was made publicly available.

> At the time, we had no idea where we were going with the project. By the spring of 1995 however, it had occurred to us that the Internet user community really needed an open source, SQL-based multiuser database.

— Yu and Chen, in the foreword to [PostgreSQL: Introduction and Concepts](https://momjian.us/main/writings/pgsql/aw_pgsql_book/node5.html) by Bruce Momjian

In 1996, Bruce Momjian found the Postgres95 project with only one part-time maintainer. He and Marc Fournier began creating patches, issuing releases and renamed the project PostgreSQL. That volunteer group became the core team.

This organizational structure combined with its already permissive license, Postgres, unlike MySQL, has never had a single owner. That is why every cloud can provide, fork, and extend Postgres as it likes.

In this moment, the seeds of what would make Postgres popular were sown, but MySQL would still dominate open-source relational databases on the web for the next 15 years. Postgres' popularity would have to wait until after the turn of the new millennium, but then perhaps *Postgres' plans are measured in centuries*.

## [1986: Was it destiny?](https://planetscale.com/blog/what-was-the-turning-point-for-postgres#1986-was-it-destiny)

Was Postgres always destined to succeed thanks to its original design?

From the start, Postgres was designed to be extensible, and its rich ecosystem of extensions is critical to its success. You don't get "just use Postgres" almost 40 years later without it.

The permissive BSD-style license issued by Berkeley played a massive role in widespread adoption. Today's PostgreSQL License is similar.

Some might argue this flexibility should attribute success to the Postgres wire protocol, not only Postgres itself. PlanetScale offers Postgres on a range of compute and storage options, plus custom extensions like Database Traffic Control™ and TIN search. While other providers completely change the storage method or sell opinionated configurations. There's Postgres in the browser. Of course Postgres is being rewritten in Rust. And yet, it's all Postgres.

## [The work still left to do](https://planetscale.com/blog/what-was-the-turning-point-for-postgres#the-work-still-left-to-do)

Some use cases for Postgres remain unsolved despite its overwhelming popularity. Multiple vendors are approaching horizontal sharding, for example. Meanwhile, Vitess has powered sharded MySQL since 2010 at the likes of Slack and GitHub.

At PlanetScale, [we launched Neki](https://neki.dev) in platform preview in September 2026. It is a net-new platform informed by Vitess's (sharded MySQL) design and our experience running it in production. As for performance, Neki has already sustained [118 million queries per second](https://planetscale.com/blog/118-million-queries-per-second-on-neki) across 512 shards.

Postgres isn't perfect, but it's everywhere, and we're doing our part for it to catch up to MySQL's proven scale.

## [What moment do you think it was?](https://planetscale.com/blog/what-was-the-turning-point-for-postgres#what-moment-do-you-think-it-was)

Along this journey, some *deliberate* design choices played a role in Postgres' success. Others, like Heroku's choice of Postgres, seemed almost *random*.

In fairness, it's not as though Postgres "deserved" its current status until recently. MySQL dominated the early web for a number of reasons. It was designed for short, simple queries, which were enough to power the web's biggest applications at the time. MySQL could also be run on Windows (the most popular developer environment of the time). Meanwhile, Postgres was more focused on correctness, full transactions, and complex queries.

In the era of "Web2" applications, document databases solved real performance problems that relational databases exposed.

And for Postgres to succeed didn't mean MySQL had to fail. Famously, [Uber migrated from Postgres *to* MySQL](https://www.uber.com/us/en/blog/postgres-to-mysql-migration/) in 2016. MySQL and Vitess continue today and are still a growing part of PlanetScale.

But as of right now, you won't be on your own if you *just use Postgres*.

Which moment resonates most with you, or do you think there's another reason Postgres is as popular as it is today?
