---
title: Anatomy of a (Postgres) search engine
link: https://planetscale.com/blog/anatomy-of-a-postgres-search-engine
source: planetscale-com
published: 2026-09-22T00:00:00Z
updated: 2026-09-22T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Patrick Reynolds
- Eric Ridge
labels:
- engineering
- postgres
summary: How a full-text search engine works, and how it works in Postgres
content: extracted
html: 2026-09-22-anatomy-of-a-postgres-search-engine.html
remote_preview:
  url: https://planetscale.com/assets/anatomy-of-a-postgres-search-engine-social-CHwvn6kz.png
---

Databases use indexes to make queries fast. Rather than check every row in a table for a matching value, look up that value in an index and seek directly to the right rows.

Most database indexes are [b-trees or b+trees](https://planetscale.com/blog/btrees-and-database-indexes), which sort a column's values in a total order and can quickly find matching values by exact value, a range of values, or a prefix. So, for example, a b-tree can quickly find all users with the name `Rick`, all products with a price below $5, or all repositories with a creation date in November of a given year. But a b-tree is useless for matching content in the middle of a string. That's where full-text search indexes come in.

Do not do this, which checks every `name` in your `users` table:

```sql
SELECT * FROM users
  WHERE name LIKE '% Royal';
```

And especially do not do this, which checks every `description` in your `vendors` table three times:

```sql
SELECT * FROM vendors
  WHERE description LIKE '%postgres%'
  AND description LIKE '%reliable%'
  AND description LIKE '%fast%';
```

The right tool for that job is an inverted index, which is how essentially all full-text search engines locate documents quickly by the words they contain.

Note

PlanetScale TIN is a comprehensive full-text search index for Postgres. We have a deep dive on TIN's features, performance, and implementation [in another article](https://planetscale.com/blog/introducing-tin). Anyone who needs a general refresher on full-text search indexes should continue here.

## [Inverted index overview](https://planetscale.com/blog/anatomy-of-a-postgres-search-engine#inverted-index-overview)

An inverted index is a data structure, usually on disk, that maps terms to locations. It works like the index at the back of a book. It even works a lot like a b-tree, except that instead of being keyed by a text column's entire value, an inverted index is keyed by each individual word within a text field.

Quick aside: then why is it "inverted?" It's inverted relative to the text itself, not relative to other indexes. The text is a series of implicit locations, each with a word. An inverted index is a list of words, each with one or more locations where it can be found.

Back to the structure of the thing. At a minimum, an inverted index has a term dictionary and postings lists. Depending on its feature set, it may also have positional data and frequency data.

Try some searches here, and see how your queries compute either the union or the intersection of the document IDs in postings lists in the index.

### [Term dictionary](https://planetscale.com/blog/anatomy-of-a-postgres-search-engine#term-dictionary)

The **term dictionary** maps all the terms found across all your documents to postings lists. On disk, this is often a b-tree or some other lexicographically sorted structure. When a search query arrives, it looks up all the query's terms in the term dictionary.

Queries with wildcards and fuzzy matches may scan part or all of the term dictionary looking for appropriate exact terms. For example, `g*` would look up both `gonna` and `give`, while `u~2` would look up all terms [within two character edits](https://en.wikipedia.org/wiki/Levenshtein_distance) of `u`: the terms `you` and `up`. Try it in the figure above!

### [Postings lists](https://planetscale.com/blog/anatomy-of-a-postgres-search-engine#postings-lists)

Each term in the term dictionary points to a list of locations called a **postings list**. These locations are document identifiers of some kind, enough for the database or search engine to find the document in a table or file storage. In most inverted indexes, these are sequential numbers; an index with ten documents uses the numbers zero through nine (or one through ten). However, any unambiguous identifier will do. Since a database needs to map values back to a row rather than to the nth document added to an inverted index, the inverted index either needs to store an additional map of document IDs to rows, or it needs to store the row identifiers directly in the postings list.

Postings lists are almost always sorted, then compressed in some way. If all the document IDs are under 256, they would be stored with at most eight bits each. If they are dense (i.e., many documents contain a given term), then the postings list may store only the differences between the numbers: `id1`, `id2-id1`, `id3-id2`, and so forth. These differences are always smaller than the IDs themselves, so they can be stored with fewer bits. Consider, for example, an index with 300,000 documents, of which 100,000 contain the word "who." Storing each ID literally would take `lg(n) = 19` bits per posting. But the average gap between any two successive IDs (remember, the postings list is sorted) is just three, which can be stored in two bits.

If the postings are very dense, the postings list may store a bitmap. If document *n* contains a word, then the *nth* bit in the bitmap is one; otherwise, it's zero. Such an encoding takes exactly *n* bits for *n* documents and is optimal once around half of all documents contain a given term.

Taking the union or intersection of two postings lists that are sorted is fast, because it can be done in a single, O(n) pass. Taking the union or intersection of two postings lists stored as bitmaps is extremely fast, because recent CPUs with vector instructions can `OR` or `AND` 128, 256, or even 512 bits in a single instruction.

### [Positional data](https://planetscale.com/blog/anatomy-of-a-postgres-search-engine#positional-data)

Some search engines support span queries and phrase queries. A **span query** is a query that requires terms to be in a specific part of the document or requires them to be within a specific maximum distance of each other. For example, `rules IN FIRST 5%` or `gotta NEAR/3 understand.` A **phrase query** is a special case of a span query that requires words to appear in exact sequence.

To support span and phrase queries, an inverted index can store positional data. The postings list identifies which documents each term appears in; positional data identifies where in the documents the term appears.

Because most queries aren't span or positional queries, positional data is usually stored separately from the postings lists so it can be loaded only when needed.

### [Frequency data](https://planetscale.com/blog/anatomy-of-a-postgres-search-engine#frequency-data)

Some search engines support scoring and ranking documents. One common scoring method is BM25 (more on that [below](https://planetscale.com/blog/anatomy-of-a-postgres-search-engine#scoring-bm25-and-top-k)), which needs to know some statistics about the indexed documents: the length of each document, the length of the average document, how often each term *t* appears in each document *d*, and how many total documents contain the term *t*. The index precomputes all of this data so it can be fetched quickly when scoring results for each query.

### [Index size](https://planetscale.com/blog/anatomy-of-a-postgres-search-engine#index-size)

The set of all indexed documents is called the **corpus**. The size of the index is usually some factor of the size of the corpus, with that factor depending on what features the index supports. An index with no positional or score data might be roughly 20% of the size of the corpus. A code-search index with overlapping tokens (to support exact-match and regular-expression search) and positional data could be as much as 300% of the size of the corpus. English-language text indexes with positional data and frequency statistics often are 30-50% the size of the corpus. Those numbers can vary depending on factors like document length and vocabulary size, but they give you an idea of the relative cost of implementing different features.

## [Tokenizers](https://planetscale.com/blog/anatomy-of-a-postgres-search-engine#tokenizers)

Each indexed document is a long string of text. Breaking that text into terms is called **tokenizing** it. Different use cases, especially different languages, require different tokenizers. Is `you're` one token or two? Are there any tokens at all in `{[] => []}`?

Unicode defines [a good default](https://www.unicode.org/reports/tr29/) set of rules to identify word boundaries.

After tokenization, a search engine may modify or omit terms before adding them to the inverted index. Eliminating linguistic suffixes is called **stemming**. For example, mapping `strangers` to `stranger` or mapping `thinking` to `think` allows a user to search for a word but match documents containing any form of that word.

Words so common they're not (usually) useful for searches are called **stop words**, and many search engines do not index them at all. However, an index that considers `new` a stop word can't distinguish between documents containing `York` or `New York`. An index that drops both `the` and `who` can't search for The Who at all. Stop words are a trade-off: the index is smaller and faster, but it's less precise in cases where those words matter.

## [Scoring: BM25 and top-k](https://planetscale.com/blog/anatomy-of-a-postgres-search-engine#scoring-bm25-and-top-k)

Some search use cases require retrieving a few "best" results, rather than all matching results. This is in contrast to SQL, where `SELECT <something> LIMIT 10` is allowed to return any ten rows available. One of many ways to determine the best results is to score them with **BM25**. BM25 is [a formula](https://en.wikipedia.org/wiki/Okapi_BM25) that scores how good a given document is as an answer for all the terms in a given query. It multiplies a function capturing term frequency (how many times a term appears in the given document, relative to that document's length) with a function capturing the inverse document frequency (what fraction of documents contain the term at least once), then sums up that subscore across all the terms in the query. BM25 captures the intuition that rarer terms are more significant, and a document that mentions a given term lots of times is a better result for that term.

Scoring is almost always used in conjunction with a desired number of results, like SQL's `LIMIT 10`. That allows an important optimization.\
Postings lists can be broken up into blocks, with [frequency statistics](https://planetscale.com/blog/anatomy-of-a-postgres-search-engine#frequency-data) for each block. When an index looks for the *k* documents with the highest score, known as a **top-k query**, it may be able to skip whole blocks of postings if the statistics for those blocks indicate that none of the documents in the block would produce a higher score than the best *k* documents the index has already found. This significantly speeds up top-k queries, especially for small values of *k*.

## [Segments, merging, and mutability](https://planetscale.com/blog/anatomy-of-a-postgres-search-engine#segments-merging-and-mutability)

The simplest way to build an inverted index is all at once, in one shot. A small enough index can be built entirely in memory; after all, an inverted index is little more than a `Map<String, Vector<ID>>`. Indexes larger than memory are written piece by piece. The index creation process runs until memory is full, then dumps a self-contained inverted index to disk representing the first *n* documents. Then it repeats until memory is full again, dumps that self-contained inverted index to disk for the next *n* documents, and so on. Each self-contained inverted index is called a **segment**. When processing a query, the overall index must check for matches in all segments and then merge the results.

Of course, many data sets change over time. A corpus can grow as more documents are added. New documents can be batched in memory until there are enough to form a segment, or each can be added immediately to a mutable data structure on disk. That mutable storage has a less efficient layout than an immutable segment, but it has the advantage of being, well, mutable: documents can be added efficiently without knowing all of them in advance. Then the documents in mutable storage are searched alongside all the immutable segments in each query.

TIN uses a mutable segment containing postings lists, like a less efficient version of the immutable segments. Because the mutable segment is slow, it must eventually be **sealed** and converted to an immutable segment.

Try some inserts, updates, and deletes in the example segments below. For simplicity, the example shows at most two immutable segments. When the mutable segment reaches its size limit, the example immediately merges it into whichever immutable segment is smaller. In practice, a mutable segment that gets sealed would exist for a while as a small, standalone immutable segment.

Eventually, there will be too many segments. An index that searches hundreds or thousands of small segments will spend some amount of CPU and memory just tracking all the segments and merging the query results. So, inverted indexes normally need to **merge** segments. In a merge, two or more segments become a single, larger one. If the document IDs are sequential numbers local to each segment, they all must be reassigned; document *i* from one segment must not be confused with document *i* from another.

Merging is algorithmically straightforward; it's just a linear pass through the term dictionary and each postings list. But a merge requires a lot of disk space and a lot of I/O. Merging takes two or more immutable segments as input and produces a new output segment equal to the size of all the input segments, minus the postings for any documents that have been deleted. Segments can easily be many gigabytes each, so a merge process might read and write tens of gigabytes and consume, temporarily, that much extra disk space. Segment merging is always a trade-off between the I/O cost of merging and the efficiency penalty of keeping a larger number of segments around.

But that's just how we insert documents. What about deleting them? It's impractical to delete document IDs from the middle of a postings list, which would require recompressing part or all of the list. So deletion just creates a **tombstone**. A tombstone is an entry in a table indicating that a given document ID is no longer valid. The inverted index will still produce that document ID as a query result, but it checks each result against the tombstones and will remove that ID before returning it to the caller.

That creates another trade-off. Every deleted document still takes up disk space in the index and wastes CPU time retrieving it from a postings list and filtering it out of the list of results. But the only way to get rid of tombstones is to merge segments and filter the document IDs written to the output segment against the tombstone list. Sometimes, when a large fraction (nearing half) of the documents in a segment have been deleted, it may be worth rewriting that segment by itself, just to get rid of the deleted documents.

Updating a document is nothing more than deleting (tombstoning) its old version and inserting its new one. Immutable segments as described here can't do in-place updates.

## [How it works in Postgres](https://planetscale.com/blog/anatomy-of-a-postgres-search-engine#how-it-works-in-postgres)

Finally, we come to how inverted indexes work to provide full-text search in Postgres. How can we make this work?

```sql
CREATE INDEX ON songs USING tin(lyrics);
SELECT title, performer
  FROM songs
  WHERE lyrics ==> 'make you cry'
  ORDER BY tin.score(ctid) DESC
  LIMIT 10;
```

Each document is a row's value for a single text column. So if an index is on the column `lyrics` in the table `songs`, then each song's lyrics would be a single document. The inverted index has to provide a row identifier, the ctid, to tell the Postgres executor which ten rows to fetch `title` and `performer` from.

The whole inverted index needs to be stored somewhere, preferably as a WAL-logged index relation so Postgres replication and backups include it. When segments get deleted, their old storage is freed, but space in the middle of a relation can't be returned to the OS. Instead, the index must maintain a list of freed pages so it can reuse them later.

Merges are often triggered when a segment crosses some size threshold or its tombstone list crosses a threshold fraction of the total documents. But we'd really prefer not to perform a merge (remember: tens of gigabytes read and written, possibly several minutes to execute) inline in an `INSERT`, `UPDATE`, or `DELETE`. So we need a job queue and background maintenance workers.

Because it's Postgres, [VACUUM](https://www.postgresql.org/docs/current/sql-vacuum.html) needs to work. `VACUUM` removes dead row versions from the heap and asks each index to remove entries that point to them. `VACUUM FULL` rewrites all the rows in a table, so the ctids change. If the inverted index stores ctids, it needs to update them.

Document insertions and deletions must be associated with a transaction number so they aren't visible to other transactions until committed, and so that they can be rolled back. The index must return all the results that are visible and none that aren't. Even in the face of that requirement, a top-k query must actually return *k* rows. To make `COUNT(*)` and top-k queries work efficiently, the inverted index needs access to row-level visibility and page-level [visibility maps](https://www.postgresql.org/docs/current/storage-vm.html).

Queries often combine inverted full-text constraints like `lyrics ==> 'make you cry'` with traditional SQL constraints like `year = 1987`. The query planner needs to know when to use a full-text index, when to use a b-tree, and when to use both and combine the results with a bitmap intersection. A query with full-text constraints on multiple columns, like `title` and `lyrics`, should use a CustomScan to filter both entirely within the index implementation, because that's much faster than returning a result set for each column and letting Postgres take the intersection.

## [Solving this optimally for Postgres](https://planetscale.com/blog/anatomy-of-a-postgres-search-engine#solving-this-optimally-for-postgres)

For details on how we solved all these challenges and how well it worked, go read the [deep dive on TIN](https://planetscale.com/blog/introducing-tin), PlanetScale's new full-text search extension for Postgres.
