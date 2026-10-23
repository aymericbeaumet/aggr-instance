---
title: Postgres 19 delayed after AI reviewers find more bugs than humans can fix
link: https://www.theregister.com/databases/2026/10/16/postgres-19-delayed-after-ai-reviewers-find-more-bugs-than-humans-can-fix/5303471
source: hnrss-org
published: 2026-10-16T15:55:40Z
updated: 2026-10-16T15:55:40Z
first_seen: 2026-10-16T18:46:36.999527378Z
authors:
- magoghm
labels:
- databases
- open source
- postgresql
content: extracted
html: 2026-10-16-postgres-19-delayed-after-ai-reviewers-find-more-bugs-than.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50119991
  num_comments: 34
  points: 110
---

REG AD

[databases](https://www.theregister.com/tag/databases)

Core devs revert up to 10 features, push release to November, and blame better tooling rather than vibe coding

The PostgreSQL Global Development Group has delayed the release of PostgreSQL 19, the latest iteration of the popular open source database, and held back an unusual number of features.

Once slated for release in late September or early October, PostgreSQL 19 is now expected to be generally available around November 19. Developers have also dropped between eight and 10 significant features.

Explaining the number of reverts — features whose rollouts have now been postponed to next year's release, PostgreSQL 20 — Peter Eisentraut, veep at EDB, a PostgreSQL support and consultancy, said the reason could be at least in part attributed to the LLM code reviews used by developers.

REG AD

In [a blog post,](https://peter.eisentraut.org/blog/2026/10/09/the-reverts-will-continue-until-morale-improves) Eisentraut, who is a leading PostgreSQL committer, said some of the reverted features had significant architectural defects that would have been hard to fix "quickly and during beta."

REG AD

However, the introduction of LLM-assisted code review also uncovered a long tail of “relatively harmless issues involving various edge cases” which, in past times, developers might not have noticed until much later, most likely issuing bug fixes in the five years following launch. “With LLM-assisted code review, these kinds of things can get found much faster, and then you’re staring at a list of forty defect reports. Even if each of them requires only a three-line fix on average, there is a fixed overhead to dealing with each of these, and then you run out of time.”

When the big updates for PostgreSQL 19 were written, LLM-assisted code review was not as useful as it is now, we're told, but in the months leading up to the launch, the AI-supported reviews became better understood and more effective.

“The effective quality bar has been raised after the time the feature was written and committed. Unless the capabilities of LLM-assisted code review keep improving at the current rate, this situation should sort itself out for future development cycles,” Eisentraut said.

He was also at pains to point out that the reverts were not down to “vibe coding” in development, as the technique was not available when most of the new features were written.

Postgres was [first proposed](https://dsf.berkeley.edu/papers/ERL-M85-95.pdf) by Michael Stonebraker in 1986. In 1994, Andrew Yu and Jolly Chen replaced the query language POSTQUEL with SQL, and in 1995 made the database freely available and modifiable under a more permissive license, renaming it PostgreSQL. It has since become the most popular database among professional developers owing to the services the big three hyperscalers built on it as well as the range of wire-compatible distributed systems available.

Among the features pulled from the PostgreSQL 19 iteration is support for SQL Property Graph Queries (SQL/PGQ), which became part of the SQL standard in 2023. The feature provides syntax for exploring relationships between nodes connected by edges. PostgreSQL's implementation was removed over unresolved issues.

At the time, [PostgreSQL contributor Tom Lane warned](https://www.theregister.com/databases/2026/09/15/postgresql-19-graph-queries-fail-the-would-you-ship-this-test/5296343): "At this point I'd be willing to bet dinner that if we ship it in v19 there will be post-release bug discoveries that are unfixable until v20."

Despite the delay, Eisentraut promises PostgreSQL 19 will still be “a great release,” that includes REPACK CONCURRENTLY, which avoids the table-lock VACUUM FULL operation used to reclaim disk space.

REG AD

“And it will be very robust, after all the additional scrutiny,” he said.

The features reverted from PostgreSQL 19 will likely make it into PostgreSQL 20. Just a few months ago, developers expected PostgreSQL 20 to have no new features, just bug fixes. Now, it is likely to have the new features left over from the earlier release, Eisentraut said.®
