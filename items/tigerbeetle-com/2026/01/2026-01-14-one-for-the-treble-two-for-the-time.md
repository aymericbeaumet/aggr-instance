---
title: One for the Treble, Two for the Time
link: https://tigerbeetle.com/blog/2026-01-14-bitemporality
source: tigerbeetle-com
published: 2026-01-14T00:00:00Z
updated: 2026-01-14T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Lewis Daly
summary: Time is like a river made up of events which happen, and a violent stream; for as soon as a thing has been seen, it is carried away, and another comes in its place. — Meditations, Book IV
content: extracted
html: 2026-01-14-one-for-the-treble-two-for-the-time.html
remote_preview:
  url: https://tigerbeetle.com/blog/2026-01-14-bitemporality/banner-750eb3b9.webp
---

> Time is like a river made up of events which happen, and a violent stream; for as soon as a thing has been seen, it is carried away, and another comes in its place.
>
> — [*Meditations, Book IV*](https://classics.mit.edu/Antoninus/meditations.4.four.html)

There’s a question that sometimes comes up when building on TigerBeetle: *“It’s great that TigerBeetle is immutable and leaves an audit trail, but what happens when we need to fix the past?”.*

When we record information, mistakes happen. We thought we knew a fact about the world, but were wrong, or there was something we didn’t know then but know now.

The art of modelling information across two timelines at once like this is known as bitemporality, and can be confusing enough to make Christopher Nolan tear his hair out.

When it comes to bitemporality, at least in transaction processing, the trick is this:

> Record facts as soon as you learn them.

Those facts could apply to right here and now (e.g. the price of AAPL is $208.62) or could apply to the past (Rafael worked 10 hours overtime last month when we thought it was 5).

TigerBeetle is a fact-ingesting machine. You set up your entities as accounts in TigerBeetle, send in the transfers you want to record between them, and if anything violates your business rules, TigerBeetle will tell you. Transfers are facts that happened in the real world, as interpreted by your business, for example *“Lewis paid for Alex’s lunch on Tuesday to the tune of $14.50”*, or *“Oasis sold 1.4 million tickets across 17 UK and Ireland dates in minutes”*.

Beyond the business rules you encode when recording transfers, TigerBeetle takes no part in interpreting those facts. TigerBeetle is the *recording layer*, and the application interprets those facts in the *reporting layer*. In other words, TigerBeetle takes responsibility for the debit/credit *primitives*, and expects (and respects) the application to take responsibility for the *policy* (since this varies from country to country, business to business, or product to product).

We’re always recording new facts to bring ourselves up to date with reality. But sometimes the ship has sailed. Perhaps Rafael was already paid for last month’s overtime, or we learned on Wednesday that on Monday, a 4-for-1 stock split of AAPL shares took effect, shares we were holding on behalf of a customer. Now, we’ve got to retroactively adjust their position from 25 shares to 100 and correct the downstream calculations which depended on the old quantity and price.

This is where adjustments come in. When we learn new facts about the world we need to:

1. Understand what we *thought* the reality was at a given point in time,
2. Understand what we *think* we know about reality now, and
3. Apply an adjustment to bridge the delta between these two realities.

In other words, as soon as we learn a fact, we record it! But we record it along with the timestamp to which that fact relates. So in the end, we record two timestamps:

1. **Recorded**: when the fact was learned, and
2. **Effective**: when the fact took place.

Let’s return to the Lewis and Alex example. Say we want to model the money that Alex and Lewis owe each other. After Lewis paid for Alex’s lunch, Alex bought movie tickets for [Becoming Led Zeppelin](https://www.youtube.com/watch?v=EDKC77QS8WM) for $10 each. Our sequence of facts that we want to record is now:

| Recorded | Effective | Fact                               |
| -------- | --------- | ---------------------------------- |
| 12 May   | 12 May    | Lewis paid for Alex’s lunch: $14.5 |
| 13 May   | 13 May    | Alex paid for Lewis’ movie: $10    |

Given these facts, on 13 May, we can produce a set of accounts and a net balance that tracks the liability between the parties:

Balances @ 13 May

*\*wrt = With Respect To*

So far so good, right? Now, let’s say that we find out on May 14th that the lunch calculations were wrong, and Alex’s lunch actually cost $19? Our series of facts now looks like:

| Recorded | Effective | Fact                                   |
| -------- | --------- | -------------------------------------- |
| 12 May   | 12 May    | Lewis paid for Alex’s lunch: $14.5     |
| 13 May   | 13 May    | Alex paid for Lewis’ Movie Ticket: $10 |
| 14 May   | 12 May    | Lewis paid for Alex’s lunch: $19       |

Notice how the `Effective` date is backdated to 12 May? We’re adding information about the lunch that supersedes the first recording, while preserving both.

We’ve implicitly answered the question: *“What was our understanding of the world on 13 May?”*, and now, given these new facts, we can answer the question: *“How did our understanding of the world change on 14 May?”*

Balances @ 14 May

Did you notice? This is a projection! We’ve not removed information from the *recording layer*, but in the *reporting layer* we’ve surfaced a new report based on the new information recorded. We’ll see how to use this information in a moment.

But you could also imagine a situation where we learn new facts about the world that don’t supersede old facts, for example, an earlier transaction between Alex and Lewis that we didn’t originally know about.

So far, we’ve answered the first 2 questions: (1) understand what we *thought* reality was at a point in time, and (2) understand what we *think* we know about reality from now. It’s time to tackle step (3) to apply an adjustment to bridge the delta between the two realities.

On 14 May, we see that Alex owes $9, but on 13 May, it was $4.5. So, let’s apply an *adjustment* of $4.5 to both accounts, to bring the Recording layer in line with reality. For this we can use a new account: `Adjustments`:

Adjusted Balances @ 14 May

We’ve now applied the adjustments to both accounts (which net to 0), and our correction is brought into line with our understanding of reality on 14 May!

And thus ends our brief foray into the world of bitemporality. Hopefully you can see that when we *logically* separate out recording and reporting into two different layers, we no longer have to choose between the immutability of append-only and the ability to fix mistakes or add information.

1. [Temporal Patterns](https://martinfowler.com/eaaDev/timeNarrative.html) by Martin Fowler is a great overview on general modelling approaches to preserving the historical state of a database.
2. [github/tigerbeetle#157](https://github.com/tigerbeetle/tigerbeetle/issues/157) is the basis of our thinking on how to model Bitemporality in TigerBeetle, going into more detail around accounting policy and separating out the read and write paths.
3. Snodgrass’ [Developing Time-Oriented Applications in SQL](https://www2.cs.arizona.edu/~rts/tdbbook.pdf) (especially chapter 1 and 2) has some great examples of modelling and reasoning about bitemporal problems in SQL.
