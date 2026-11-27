---
title: Snapshot Isolation vs Serializability
link: http://brooker.co.za/blog/2024/12/17/occ-and-isolation.html
source: brooker-co-za
published: 2024-12-17T00:00:00Z
updated: 2024-12-17T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Marc Brooker
summary: 'Snapshot Isolation vs Serializability Getting into some fundamentals. In my re:Invent talk on the internals of Aurora DSQL I mentioned that I think snapshot isolation is a sweet spot in the database isolation spectrum for most kinds of applications. Today, I want to dive in a little deeper into why I think that, and some of the trade-offs of going stronger and weaker. This post is going to be a little deeper than the last few. If you’re not deeply familiar with SQL’s isolation levels, I recommend checking out Crooks et al’s Seeing is Believing: A Client-Centric Specification of Database Isolation, Berenson et al’s A Critique of ANSI SQL Isolation Levels, or Adya et al’s Generalized Isolation Level Definitions. Specifically, I’m going to talk about one very specific mental model of transaction isolation: read-write conflicts, and write-write conflicts. Let’s start our journey with a transaction, T1: BEGIN; SELECT amnt FROM food WHERE id = 1 OR id = 2; UPDATE food SET amnt = amnt - 1 WHERE id = 1; COMMIT; There are a few things to notice about this transaction, when run in a database system that offers interactive (i.e. back-and-forth with the client) transactions: It directly reads two rows from the database, the rows 1 and 2 from the table food. It directly writes one row in the database, the rows 1 from the table food. It starts and ends at different times: starting during BEGIN and ending during COMMIT. Depending on the way that database is implemented, it also likely reads other data (e.g. the system catalog which tells it which tables exist), and may write other data (e.g. a secondary index on the amnt column of food). The forth point here is critical in real systems, but let’s ignore it for now and focus only on the first three2. Before we do that, let’s introduce a second transaction, the first one’s parallel universe clone, T2. We’ll also assume these are the only transactions running at this time. BEGIN; SELECT amnt FROM food WHERE id = 1 OR id = 2; UPDATE food SET amnt = amnt - 1 WHERE id = 2; COMMIT; We’ll go a step further and explicitly interleave these transactions (as though they were happening at the same time on two different connections to the same database), taking a page from Hermitage4. BEGIN; -- T1, t1_start BEGIN; -- T2, t2_start SELECT amnt FROM food WHERE id = 1 OR id = 2; -- T1 SELECT amnt FROM food WHERE id = 1 OR id = 2; -- T2 UPDATE food SET amnt = amnt - 1 WHERE id = 1; -- T1 UPDATE food SET amnt = amnt - 1 WHERE id = 2; -- T2 COMMIT; -- T1, t1_commit COMMIT; -- T2, t2_commit, WHAT HAPPENS HERE? What should happen to the second commit? Accepting that second commit is an example of write skew, and is generally a good minimal example of the difference between snapshot isolation (SI) and serializability. But that’s well documented, and not what I’m focussing on. What I’m focussing on is how we might prevent write skew. Read Sets and Write Sets Let’s abstract T1 and T2 a step further: into read sets and write sets. We’ll simplify everything more here by pretending our database has a single table (food). T1 then has the read set $R_1 = {1,2}$ and the write set $W_1 = {1}$ T2 then has the read set $R_2 = {1,2}$ and the write set $W_2 = {2}$ Under serializability, once T1 is committed, T2 can only commit if $R_2 \cap W_1 = \emptyset$ (or, more generally, the set of all writes accepted between t2_start and t2_commit does not intersect with R_21). Under snapshot isolation, once T1 is committed, T2 can only commit if $W_2 \cap W_1 = \emptyset$ (or, more generally, the set of all writes accepted between t2_start and t2_commit does not intersect with W_2). Ok? Notice how similar those two statements are: one about $R_2 \cap W_1$ and one about $W_2 \cap W_1$. That’s the only real difference in the rules. But it’s a crucial difference. It’s a crucial difference because of one of the cool and powerful things that SQL databases make easy: SELECTs. You can grow a transaction’s write set with UPDATE and INSERT and friends, but most OLTP applications don’t tend to. You can grow a transaction’s read set with any SELECT, and many applications do that. If you don’t believe me, go look at the ratio between predicate (i.e. not exact PK equality) SELECTs in your code base versus predicate UPDATEs and INSERTs. If the ratios are even close, you’re a little unusual. Concurrency versus Isolation This is where we enter a world of trade-offs3: avoiding SI’s write skew requires the database to abort (or, sometimes, just block) transactions based on what they read. That’s true for OCC, for PCC, or for nearly any scheme you can devise. To get good performance (and scalability) in the face of concurrency, applications using serializable isolation need to be extremely careful about reads. The trouble is that isolation primarily exists to simplify the lives of application programmers, and make it so they don’t have to deal with concurrency. SQL-like isolation models do that quite effectively, and are (in my opinion) one of the best ideas in the history of computing. But as we move up the isolation levels to serializability, we start pushing more complexity onto the application programmer by forcing them to worry more about concurrency from a performance and throughput perspective. This is the cause of my belief that snapshot isolation, combined with strong consistency, is the right default for most applications and most teams of application programmers: it provides a useful minimum in the sum of worries about anomalies and performance. Fundamentally, it does that by observing that write sets are smaller than read sets, for the majority of OLTP applications (often MUCH smaller). How Low Can We Go? How Low Should We Go? That raises the question of whether its worth going even lower in the isolation spectrum. I don’t have as crisp a general answer, but in Aurora DSQL’s case the answer is, mostly no. Reducing the isolation level from SI to read committed does not save any distributed coordination, because of the use of physical clocks and MVCC to provide a consistent read snapshot to transactions without coordination. It does save some local coordination on each storage replica, and the implementation of multiversioning in storage, but our measurements indicate those are minimal. If you’d like to learn more about that architecture, here’s me talking about it at re:Invent 2024: Optimistic versus Pessimistic Concurrency Control What I said above about SI is, from my perspective, equally true in both OCC and PCC designs, when combined with MVCC to form read snapshots and timestamps to choose a read snapshot. In both cases, the only coordination strictly required for SI is to look for write-write conflicts ($W_2 \cap W_1$) that occured between t_start and t_commit, and to choose a commit timestamp based on that detection. The big advantage OCC has in avoiding coordination has to do with how those write-write conflicts are detected. In backward validation OCC, the only state that is needed to decide whether to commit transaction $T$ is from already committed transactions. This means that all state in the commit protocol is transient, and can be reconstructed from the log of committed transactions, without causing any false aborts. This is a significant benefit! The other benefit of OCC, as I covered in my first post on DSQL is that we can avoid all coordination during the process of executing a transaction, and only coordinate at COMMIT time. This is a huge advantage when coordination latency is considerable, like in the multi-region setting. As with SI versus serializability, the story of trade-offs between optimistic and pessimistic approaches is a long one. But, for similar reasons, I think OCC (when combined with MVCC) is the right choice for most transactional workloads. Footnotes This general framing is my OCC backward validation bias showing, but it extends to most pessimistic approaches too. These special cases, like the catalog and FOR UPDATE, don’t fit nicely into the academic framework of SI, and most databases will need to handle them differently from regular key accesses. You will find, typically online, folks who deny that these trade-offs exist. Those people are wrong, both theoretically and practically. You can find Aurora DSQL’s hermitage SQL test set here. You may notice that it’s very similar to PostgreSQL’s repeatable read set, but non-blocking instead of blocking.'
content: extracted
html: 2024-12-17-snapshot-isolation-vs-serializability.html
remote_preview:
  url: https://brooker.co.za/blog/images/feed-icon-14x14.png
---

Getting into some fundamentals.

In my [re:Invent talk on the internals of Aurora DSQL](https://www.youtube.com/watch?v=huGmR_mi5dQ) I mentioned that I think snapshot isolation is a sweet spot in the database isolation spectrum for most kinds of applications. Today, I want to dive in a little deeper into why I think that, and some of the trade-offs of going stronger and weaker.

This post is going to be a little deeper than the last few. If you’re not deeply familiar with SQL’s isolation levels, I recommend checking out Crooks et al’s [Seeing is Believing: A Client-Centric Specification of Database Isolation](https://dl.acm.org/doi/10.1145/3087801.3087802), Berenson et al’s [A Critique of ANSI SQL Isolation Levels](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/tr-95-51.pdf), or Adya et al’s [Generalized Isolation Level Definitions](https://pmg.csail.mit.edu/papers/icde00.pdf).

Specifically, I’m going to talk about one very specific mental model of transaction isolation: read-write conflicts, and write-write conflicts.

Let’s start our journey with a transaction, `T1`:

```plaintext
BEGIN;
SELECT amnt FROM food WHERE id = 1 OR id = 2;
UPDATE food SET amnt = amnt - 1 WHERE id = 1;
COMMIT;
```

There are a few things to notice about this transaction, when run in a database system that offers interactive (i.e. back-and-forth with the client) transactions:

- It directly reads two rows from the database, the rows `1` and `2` from the table `food`.
- It directly writes one row in the database, the rows `1` from the table food.
- It starts and ends at different times: starting during `BEGIN` and ending during `COMMIT`.
- Depending on the way that database is implemented, it also likely reads other data (e.g. the system catalog which tells it which tables exist), and may write other data (e.g. a secondary index on the `amnt` column of `food`).

The forth point here is critical in real systems, but let’s ignore it for now and focus only on the first three [2](http://brooker.co.za/blog/2024/12/17/occ-and-isolation.html#foot2). Before we do that, let’s introduce a second transaction, the first one’s parallel universe clone, `T2`. We’ll also assume these are the only transactions running at this time.

```plaintext
BEGIN;
SELECT amnt FROM food WHERE id = 1 OR id = 2;
UPDATE food SET amnt = amnt - 1 WHERE id = 2;
COMMIT;
```

We’ll go a step further and explicitly interleave these transactions (as though they were happening at the same time on two different connections to the same database), taking a page from [Hermitage](https://github.com/ept/hermitage/blob/master/postgres.md)[4](http://brooker.co.za/blog/2024/12/17/occ-and-isolation.html#foot4).

```plaintext
BEGIN; -- T1, t1_start
BEGIN; -- T2, t2_start
SELECT amnt FROM food WHERE id = 1 OR id = 2; -- T1
SELECT amnt FROM food WHERE id = 1 OR id = 2; -- T2
UPDATE food SET amnt = amnt - 1 WHERE id = 1; -- T1
UPDATE food SET amnt = amnt - 1 WHERE id = 2; -- T2
COMMIT; -- T1, t1_commit
COMMIT; -- T2, t2_commit, WHAT HAPPENS HERE?
```

What should happen to the second commit?

Accepting that second commit is an example of *write skew*, and is generally a good minimal example of the difference between snapshot isolation (SI) and serializability.

But that’s well documented, and not what I’m focussing on. What I’m focussing on is how we might prevent write skew.

*Read Sets and Write Sets*

Let’s abstract `T1` and `T2` a step further: into read sets and write sets. We’ll simplify everything more here by pretending our database has a single table (`food`).

`T1` then has the read set $R\_1 = {1,2}$ and the write set $W\_1 = {1}$

`T2` then has the read set $R\_2 = {1,2}$ and the write set $W\_2 = {2}$

Under serializability, once `T1` is committed, `T2` can only commit if $R\_2 \\cap W\_1 = \\emptyset$ (or, more generally, the set of all writes accepted between `t2_start` and `t2_commit` does not intersect with `R_2`[1](http://brooker.co.za/blog/2024/12/17/occ-and-isolation.html#foot1)).

Under snapshot isolation, once `T1` is committed, `T2` can only commit if $W\_2 \\cap W\_1 = \\emptyset$ (or, more generally, the set of all writes accepted between `t2_start` and `t2_commit` does not intersect with `W_2`).

Ok?

Notice how similar those two statements are: one about $R\_2 \\cap W\_1$ and one about $W\_2 \\cap W\_1$. That’s the only real difference in the rules.

But it’s a *crucial* difference.

It’s a crucial difference because of one of the cool and powerful things that SQL databases make easy: `SELECT`s. You can grow a transaction’s write set with `UPDATE` and `INSERT` and friends, but most OLTP applications don’t tend to. You can grow a transaction’s read set with any `SELECT`, and many applications do that. If you don’t believe me, go look at the ratio between predicate (i.e. not exact PK equality) `SELECT`s in your code base versus predicate `UPDATE`s and `INSERT`s. If the ratios are even close, you’re a little unusual.

*Concurrency versus Isolation*

This is where we enter a world of trade-offs [3](http://brooker.co.za/blog/2024/12/17/occ-and-isolation.html#foot3): avoiding SI’s write skew requires the database to abort (or, sometimes, just block) transactions based on what they *read*. That’s true for OCC, for PCC, or for nearly any scheme you can devise. To get good performance (and scalability) in the face of concurrency, applications using serializable isolation need to be extremely careful about *reads*.

The trouble is that isolation primarily exists to simplify the lives of application programmers, and make it so they don’t have to deal with concurrency. SQL-like isolation models do that quite effectively, and are (in my opinion) one of the best ideas in the history of computing. But as we move up the isolation levels to serializability, we start pushing more complexity onto the application programmer by forcing them to worry more about concurrency from a performance and throughput perspective.

This is the cause of my belief that snapshot isolation, combined with strong consistency, is the right default for most applications and most teams of application programmers: it provides a useful minimum in the sum of worries about anomalies and performance.

Fundamentally, it does that by observing that write sets are smaller than read sets, for the majority of OLTP applications (often MUCH smaller).

*How Low Can We Go? How Low Should We Go?*

That raises the question of whether its worth going even lower in the isolation spectrum. I don’t have as crisp a general answer, but in Aurora DSQL’s case the answer is, mostly *no*. Reducing the isolation level from SI to `read committed` does not save any distributed coordination, because of the use of physical clocks and MVCC to provide a consistent read snapshot to transactions without coordination. It does save some local coordination on each storage replica, and the implementation of multiversioning in storage, but our measurements indicate those are minimal.

If you’d like to learn more about that architecture, here’s me talking about it at re:Invent 2024:

[YouTube video player](https://www.youtube.com/watch?v=huGmR_mi5dQ)

*Optimistic versus Pessimistic Concurrency Control*

What I said above about SI is, from my perspective, equally true in both OCC and PCC designs, when combined with MVCC to form read snapshots and timestamps to choose a read snapshot. In both cases, the only coordination strictly required for SI is to look for write-write conflicts ($W\_2 \\cap W\_1$) that occured between `t_start` and `t_commit`, and to choose a commit timestamp based on that detection.

The big advantage OCC has in avoiding coordination has to do with how those write-write conflicts are detected. In backward validation OCC, the only state that is needed to decide whether to commit transaction $T$ is from *already committed transactions*. This means that all state in the commit protocol is transient, and can be reconstructed from the log of committed transactions, without causing any false aborts. This is a significant benefit!

The other benefit of OCC, [as I covered in my first post on DSQL](https://brooker.co.za/blog/2024/12/03/aurora-dsql.html) is that we can avoid all coordination during the process of executing a transaction, and only coordinate at `COMMIT` time. This is a huge advantage when coordination latency is considerable, like in the multi-region setting.

As with SI versus serializability, the story of trade-offs between optimistic and pessimistic approaches is a long one. But, for similar reasons, I think OCC (when combined with MVCC) is the right choice for most transactional workloads.

*Footnotes*

1. This general framing is my OCC backward validation bias showing, but it extends to most pessimistic approaches too.
2. These special cases, like the catalog and `FOR UPDATE`, don’t fit nicely into the academic framework of SI, and most databases will need to handle them differently from regular key accesses.
3. You will find, typically online, folks who deny that these trade-offs exist. Those people are wrong, both theoretically and practically.
4. You can find Aurora DSQL’s [hermitage SQL test set here](https://github.com/marcbrooker/hermitage/blob/master/dsql.md). You may notice that it’s very similar to PostgreSQL’s `repeatable read` set, but non-blocking instead of blocking.
