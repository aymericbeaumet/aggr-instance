---
title: Sequences and other volatiles in Neki
link: https://planetscale.com/blog/neki-volatile-expressions
source: planetscale-com
published: 2026-10-14T00:00:00Z
updated: 2026-10-14T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Mats Rydberg
labels:
- engineering
- neki
summary: How Neki evaluates PostgreSQL volatile expressions with the right cardinality to preserve sequence semantics across shards.
content: extracted
html: 2026-10-14-sequences-and-other-volatiles-in-neki.html
remote_preview:
  url: https://planetscale.com/assets/neki-volatile-expressions-social-CjV0FIGF.png
---

Hi there! My name is Mats, and I have been working on database systems for a bit over ten years. My focus has shifted, but a topic that has stuck with me is the query language; its syntax, semantics, and implementation. I have contributed to ISO standardization efforts, researched and developed query language features from scratch, and implemented features in both runtime layers and query planners.

For the past eight months, that work has been the Neki query planner and its runtime engine. In this blog, I will discuss how Neki, PlanetScale's sharded Postgres, implements sequences in our sharded system to match native Postgres. I will focus on two aspects that need special attention: volatile expressions and cross-shard synchronous evaluation. For some additional background on Neki, see these blogs: [Neki's architecture](https://planetscale.com/blog/the-architecture-of-neki) and [What is a data topology](https://planetscale.com/blog/what-is-a-data-topology).

## [Implementing Postgres sequences in Neki](https://planetscale.com/blog/neki-volatile-expressions#implementing-postgres-sequences-in-neki)

In PostgreSQL, sequences are ubiquitous. A sequence is abstractly a unique number generator, which can be used to implement a counter, an identity, or a surrogate key.

A key property of sequences is their guarantee to never generate the same value more than one time. (That guarantee has a caveat of its own: a sequence can be reset or cycled, which clearly would repeat values, but these aspects are out of scope for this post.) Interestingly, a sequence does not guarantee that its output numbers are always consecutive; when transactions roll back, for example, consumed sequence numbers are not reclaimed.

## [Can Postgres handle sequences for us?](https://planetscale.com/blog/neki-volatile-expressions#can-postgres-handle-sequences-for-us)

Each Neki shard is its own complete database system with its own query planner and an extensive SQL language support, so the Neki query planner can offload a lot of work to it. In fact, we view it as a virtue to do so: if we can relay the entire query to the shard, that is the best possible case. But generally, that is not possible to do.

The most obvious reason is that no shard has all the data, they only have their slice. Detecting these best cases requires a careful and complicated analysis of the SQL statement and the [Neki data topology](https://planetscale.com/blog/what-is-a-data-topology), with a strong understanding of the semantics of the SQL language and its PostgreSQL dialect with its various unique features.

That's where we come in.

But this post is not about these best cases and how we detect them. Instead, our focus is on some of the pathological cases where we *cannot* relay all the work to one of the shards. As I will explain, sequences fall into this category where we must assume centralized control. But before we go into sequences, we must understand some details of Neki query planning.

## [The general case of Neki query planning](https://planetscale.com/blog/neki-volatile-expressions#the-general-case-of-neki-query-planning)

In the most general case, each shard only needs to provide raw data. Everything else has to be possible in the router itself. For a simple query like Query 1 below, each shard hosting the (sharded) table `animals` returns its rows with the requested columns, and the router concatenates those rowsets into the full result and returns it to the client.

**Query 1: `SELECT...FROM`**

If we extend this query with some simple expressions over the columns, seen in Query 2, we now have a choice: evaluate the expressions in the `router`, or on the `shard`?

**Query 2: add simple expressions**

Both choices will give the correct result, so the question is one of optimization, which as it turns out is nontrivial:

- Is the router or the shard more resource-constrained?
- Which of the two is actually fastest to evaluate?
- Is it more efficient to network-transfer the evaluated expression, or the raw data from the column?
- What about nested expressions; shall we split them and evaluate one part here and one part there?
- and so on

For the purpose of simplicity, we will here assume that Neki is using a simple heuristic whereby we *always push* the evaluation to the shard if we can.

### [Router-only expressions](https://planetscale.com/blog/neki-volatile-expressions#router-only-expressions)

But some expressions don’t give us that choice. Neki defines this class of expressions as *router-only*: they must be evaluated on the `router` and we must **never** push them down to the shard.

One example of such an expression is the `now()` function. It returns the wall clock time recorded at the start of the transaction.

If we let each shard evaluate it, the natural clock drift and the different arrival times of the request would produce different values on different shards, which violates the semantics of the expression. So we need to evaluate it in the router.

Consider Query 3, which adds `now()` to the mix.

**Query 3: adding `now()`**

A valid approach here is to run Query 2 on the shards, concatenate the results, and then evaluate `now()` in the router and append the value to each row. We don’t know how many rows the shards will return, so we have to make sure evaluating now() repeatedly in the router always gives the same value. There are a couple of ways to guarantee that.

We could evaluate it once into a single-row, single-column table and join that with the rows from the shards. Or we could cache the value per statement and read from the cache on repeated evaluations. Either way, every row gets the same wall clock time.

And this summarizes the Neki query planner's general model:

- Detect what parts of the query cannot run on the shards
- Compose a plan wherein those parts instead run in the `router`
- Send most other things into the shards

## [How Postgres classifies expressions](https://planetscale.com/blog/neki-volatile-expressions#how-postgres-classifies-expressions)

So far, so good, but what about sequences? In order to fully understand how to deal with sequences, we need to understand PostgreSQL's expression classification of `immutable`, `stable`, and `volatile`. During semantic analysis, one of the phases of query planning, every expression in the statement gets one of these classifications based on its own definition and its inputs.

I've already shown you examples of the first two classes.

### [The `immutable` class](https://planetscale.com/blog/neki-volatile-expressions#the-immutable-class)

In Query 2, all the expressions are `immutable`. This means that they will produce the same output each time they operate on the same inputs, regardless of how many times they are evaluated and even between statements. Such expressions are the majority, and they are simple enough to not warrant additional discussion here.

### [The `stable` class](https://planetscale.com/blog/neki-volatile-expressions#the-stable-class)

A `stable` expression produces the same output *within a statement*. The `now()` function from Query 3 is one example. `to_char()` is another, and a useful one since it takes inputs.

The output of `now()` depends on the `TimeZone` GUC, as shown below:

**Table 1**

`to_char()` takes a datetime value and a format string. Even with identical inputs, its output can differ because it depends on the `lc_time` GUC for display formatting. Table 2 shows this for a few `lc_time` values.

**Table 2**

For Neki, this means we cannot blindly push `stable` expressions to the shards. We must also make sure their dependencies are the same on every shard. Otherwise we might get `'Wednesday'` back on one row and `'Onsdag'` on another.

This can happen when a user includes a `SET lc_time = ...;` in their session. In this case, we have to push that statement to any shard that needs it for evaluation. The `set_config()` function is treated the same way. `now()` as I explained above is `router-only` for clock-synchronization reasons, but `to_char()` is not.

## [Enter volatility](https://planetscale.com/blog/neki-volatile-expressions#enter-volatility)

A `stable` expression returns the same value every time it's evaluated within a statement. A `volatile` expression makes no such promise. It may produce a different output *on every single invocation*. It doesn't always, but it can.

As an example, consider the `nextval()` function. It takes the name of a sequence and returns a new, unique value each time it's evaluated. This is very useful, and can be used to implement a counter in a query or a table.

A sequence is created either using `CREATE SEQUENCE` in a dedicated DDL statement, or declared on a column with `GENERATED AS IDENTITY` (or `serial` in older versions), or `DEFAULT nextval('seq')` syntax in a `CREATE TABLE` statement.

In Neki, these sequence tables exist on all shards, but they all start with an empty sequence. Since it is possible to reference any sequence in any query (assuming no ACL rules), evaluations on sequences require centralized governance to avoid duplicate values across shards.

Thus, sequence operations are all `router-only`. The authoritative shard is the physical host for all sequences, which the router uses to get new sequence values to distribute. This is how Neki achieves cross-shard synchronization for sequence allocation.

So, all's good then? Well, there is one more complication: cardinality. This is a fancy word for "number of rows", and is relevant in the context of volatility. In order to evaluate volatile expressions the correct number of times, we need to evaluate them with the correct cardinality (again, number of rows). So how do we know what the cardinality is? We need to understand the definition of a *table expression*.

## [Table expressions](https://planetscale.com/blog/neki-volatile-expressions#table-expressions)

The driving force to evaluate expressions in the query execution model is the row cardinality. That is, each expression must be evaluated once per row to obtain the output needed for that row. That seems simple enough, but the complexity lies in "once per row" — as it turns out, determining that is also non-trivial!

A SQL statement is made up of clauses that form an abstract data model. We will not go into all details of that here, but I will define what a *table expression* is.

Consider Query 4, which joins two tables `animals` and `feedings`, forming an intermediate result table, with an additional predicate in the `WHERE` clause.

**Query 4: Building a table expression from a `JOIN`**

The composition of `FROM ... JOIN ... WHERE ...` (and `GROUP BY`, `HAVING`, and more) form a logical unit known as a `table expression`. The table expression forms the basis for the evaluation of all expressions in the `SELECT` clause's output list. It is the rows of the table expression that determine the "once per row" condition of the `volatile` expressions. To understand the nuances of this, assume that the tables `animals` and `feedings` exist as shown in Table 3 and Table 4, and that they are both sharded.

**Table 3, `animals`**

**Table 4, `feedings`**

Joining the tables on their `id` columns gives us every pair of rows where the `id` values match. That’s an inner join, which is the default when `JOIN` has no qualifier in front of it. In our case, that matches both rows of `animals` with the first three rows of `feedings`, so the join table has three rows, shown in Table 5.

**Table 5, `animals JOIN feedings ON animals.id = feedings.id`**

But we also have the predicate in the `WHERE` clause, which only retains rows where `feedings.time` is `'morning'`. That leaves us with just one row in our final table expression, shown in Table 6.

**Table 6, `animals JOIN feedings ON animals.id = feedings.id WHERE feedings.time = 'morning'`**

It is this table expression that forms the basis of evaluation for our `SELECT` clause expressions. So, we would compute `8 + 1`, `lower('BISMUTH')`, `nextval('tag_seq')`, and then we are good to return results! We know that we will only evaluate the `volatile` `nextval('tag_seq')` expression one time, and thus we will only consume one sequence value.

Now, assume that we drop the `WHERE` predicate, and we use instead Query 5.

**Query 5: drop the `WHERE`**

Now, our table expression would be equal to Table 5, and we now have three rows to evaluate. The keen observer will note, as could a sophisticated optimizer, that the values of `age` and `name` are equal over two rows, since these are join-multiplied copies from the same source row from `animals`.

So it would be tempting to not evaluate these expressions more than is necessary. For the data scale we are using in this example, it would not make much difference, but for millions of rows it could. But here we must take into account the volatility aspect: we cannot make this optimization for the `nextval('tag_seq')` expression, since it will give a different value each time and must be evaluated afresh on each row.

## [The Neki case: a distributed query planner](https://planetscale.com/blog/neki-volatile-expressions#the-neki-case-a-distributed-query-planner)

Now, I must also mention that it is entirely valid to swap the order of evaluation between the `JOIN` and the `WHERE`. That is, it would be fine for us to first filter down `feedings` to its two `'morning'` rows, and then join that subset table with `animals`. Which order to do it is a general optimization problem, but it is often beneficial to filter first and join afterwards, since evaluating the predicate is often cheap.

Optimizing the operation of computing the table expression is a core focus of any query planner's optimizer, known generally as join ordering optimization (e.g. consider a longer list of `JOIN`s and which one to do first). The PostgreSQL planner has its own optimization algorithms for this, and it is not guaranteed that Neki and Postgres agree on the order of evaluation. If `volatile` expressions are part of any of the predicates (in `WHERE` or in `JOIN ... ON`), then results are not always deterministic, but they are correct. But let's return to the case for Neki, where we have a *distributed* query planner.

In Neki, a very large cost is the network-transport time between `router` and `shard`. In the case of a cross-shard join, which we are focusing on here, we have to go to some shards to fetch data for `animals` and some (same or different) shards to fetch data for `feedings`. Only when we have both concatenated sets of data are we able to perform our join. We are unable to do it on any given `shard`, since it does not have all of the data either. Thus, we have a great interest in minimizing the traffic going between `router` and `shard`.

Neki is able to determine when all rows of the table expression can be assembled independently on each shard, which typically requires that the join predicate uses the table's shard index. That is, if `animals` and `feedings` were both sharded on their `id` column, then we know that we can do a shard-local join, and concatenate the results from that instead of computing the join on the router. This is correct because rows from other shards would all be unequal on the join columns. In this case, each shard contributes a subset of the table expression.

## [Putting it all together](https://planetscale.com/blog/neki-volatile-expressions#putting-it-all-together)

All right, bringing everything together, assume again that we have sharded tables `animals` and `feedings` on their `id` columns, respectively, and that we are processing Query 6.

**Query 6: add `now()` back**

We note that the table expression can be evaluated shard-locally, and thus we can push down to shard-local evaluation on all the immutable expressions (first two columns). But we still have to evaluate both `now()` and `nextval('tag_seq')` on the router to avoid the uncoordinated shards from generating conflicting values.

`now()` is `stable`, so we evaluate it once and use the same value on all the outgoing rows. `nextval('tag_seq')` is `volatile`, so we must generate a new value for each row. And since we don't know how many rows each shard will return, we cannot generate the sequence values in advance and then draw from a cache. We must interpose the evaluations on top of the dynamic stream.

We will close by considering a more complex case, shown in Query 7, and by applying our concepts from above.

**Query 7: an `UPDATE` with `nextval()`**

This is a DML (Data Modifying Language) query. It updates each row of the `animals` table by setting the `tag_id` column to the next sequence value from the sequence `'tag_seq'`. In an `UPDATE` query, the table expression is expressed in a different syntactic form. You can build a complex one with `WHERE` and `FROM` clauses, but here we'll stick with the trivial case: the table expression is just the relation named after `UPDATE`, which is `animals`.

This is a challenge for Neki.

The cardinality of `animals` is unknown, and its rows are spread out across the shards in an unknown distribution. We cannot send `nextval('tag_seq')` to the shard, as we discussed above, and we don't know how many values to produce. So what query do we send to the shards?

The answer is not obvious, and there is more than one solution. What Neki does today is first establish the table expression in the router using Query 8, interpose evaluation of `nextval('tag_seq')` on top, and run the actual update, feeding it with parameterized sequence values.

**Query 8: what Neki sends first**

While in theory the trivial table expression includes all columns of `animals`, Neki knows that for Query 7, only the primary key columns and sharding index columns are necessary. In this case, `id` serves both purposes. The additional `FOR UPDATE` locks the rows until the Query 9 requests come in to complete the modification.

For each incoming row to the `router`, Neki evaluates `nextval('tag_seq')` and sends the shard-global unique sequence value down to be set on the correct shard's row. Neki routes to the shard using the `id` value as the shard index, and filters down to the correct row with a `WHERE` clause using the `id` value as primary key.

**Query 9: what Neki sends per row**

An illustration of the data flow:

I leave for the reader's exercise the question of how a `stable` expression such as `now()` would be handled for a variation of Query 7. It is much simpler than the `volatile` discussion since it is not dependent on the cardinality.

I hope this deep dive into Neki query planning brought you insights into our complex system, and how the classical problems of query planning map to the distributed system that is Neki.

Happy hacking!

Mats
