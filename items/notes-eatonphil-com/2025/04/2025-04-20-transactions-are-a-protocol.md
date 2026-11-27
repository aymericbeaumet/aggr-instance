---
title: Transactions are a protocol
link: http://notes.eatonphil.com/2025-04-20-transactions-are-a-protocol.html
source: notes-eatonphil-com
published: 2025-04-20T00:00:00Z
updated: 2025-04-20T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
labels:
- databases
- transactions
summary: 'Transactions are not an intrinsic part of a storage system. Any storage system can be made transactional: Redis, S3, the filesystem, etc. Delta Lake and Orleans demonstrated techniques to make S3 (or cloud storage in general) transactional. Epoxy demonstrated techniques to make Redis (and any other system) transactional. And of course there''s always good old Two-Phase Commit. If you don''t want to read those papers, I wrote about a simplified implementation of Delta Lake and also wrote about a simplified MVCC implementation over a generic key-value storage layer. It is both the beauty and the burden of transactions that they are not intrinsic to a storage system. Postgres and MySQL and SQLite have transactions. But you don''t need to use them. It isn''t possible to require you to use transactions. Many developers, myself a few years ago included, do not know why you should use them. (Hint: read Designing Data Intensive Applications.) And you can take it even further by ignoring the transaction layer of an existing transactional database and implement your own transaction layer as Convex has done (the Epoxy paper above also does this). It isn''t entirely clear that you have a lot to lose by implementing your own transaction layer since the indexes you''d want on the version field of a value would only be as expensive or slow as any other secondary index in a transactional database. Though why you''d do this isn''t entirely clear (I will like to read about this from Convex some time). It''s useful to see transaction protocols as another tool in your system design tool chest when you care about consistency, atomicity, and isolation. Especially as you build systems that span data systems. Maybe, as Ben Hindman hinted at the last NYC Systems, even proprietary APIs will eventually provide something like two-phase commit so physical systems outside our control can become transactional too. Transactions are a protocol short new post pic.twitter.com/nTj5LZUpUr — Phil Eaton (@eatonphil) April 20, 2025'
content: extracted
html: 2025-04-20-transactions-are-a-protocol.html
---

Transactions are not an intrinsic part of a storage system. Any storage system can be made transactional: Redis, S3, the filesystem, etc. [Delta Lake](https://www.vldb.org/pvldb/vol13/p3411-armbrust.pdf) and [Orleans](https://www.vldb.org/pvldb/vol17/p3720-eldeeb.pdf) demonstrated techniques to make S3 (or cloud storage in general) transactional. [Epoxy](https://petereliaskraft.net/res/p2732-kraft.pdf) demonstrated techniques to make Redis (and any other system) transactional. And of course there's always good old [Two-Phase Commit](https://www.the-paper-trail.org/post/2008-11-27-consensus-protocols-two-phase-commit/).

If you don't want to read those papers, I wrote about a [simplified implementation](https://notes.eatonphil.com/2024-09-29-build-a-serverless-acid-database-with-this-one-neat-trick.html) of Delta Lake and also wrote about a [simplified MVCC implementation](https://notes.eatonphil.com/2024-05-16-mvcc.html) over a generic key-value storage layer.

It is both the beauty and the burden of transactions that they are not intrinsic to a storage system. Postgres and MySQL and SQLite have transactions. But you don't need to use them. It isn't possible to require you to use transactions. Many developers, myself a few years ago included, do not know why you should use them. (Hint: read *Designing Data Intensive Applications*.)

And you can take it even further by ignoring the transaction layer of an existing transactional database and implement your own transaction layer as [Convex has done](https://stack.convex.dev/how-convex-works#the-transaction-log) (the Epoxy paper above also does this). It isn't entirely clear that you have a lot to lose by implementing your own transaction layer since the indexes you'd want on the version field of a value would only be as expensive or slow as any other secondary index in a transactional database. Though why you'd do this isn't entirely clear (I will like to read about this from Convex some time).

It's useful to see transaction protocols as another tool in your system design tool chest when you care about consistency, atomicity, and isolation. Especially as you build systems that span data systems. Maybe, as Ben Hindman hinted at the last [NYC Systems](https://nycsystems.xyz/2025/april.html), even proprietary APIs will eventually provide something like two-phase commit so physical systems outside our control can become transactional too.

> — Phil Eaton (@eatonphil) [April 20, 2025](https://twitter.com/eatonphil/status/1913967934097035372?ref_src=twsrc%5Etfw)
