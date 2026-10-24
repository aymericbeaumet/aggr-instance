---
title: UUIDv7 doesn't have the properties you think it does
link: https://alexsci.com/blog/uuidv7-is-weird/
source: hnrss-org
published: 2026-10-23T16:39:45Z
updated: 2026-10-23T16:39:45Z
first_seen: 2026-10-24T00:30:03.664601594Z
authors:
- ibobev
content: extracted
html: 2026-10-23-uuidv7-doesn-t-have-the-properties-you-think-it-does.html
remote_preview:
  url: https://alexsci.com/blog/static/head-shot.png
  alt: Robert Head Shot
---

[Built on Shards of Silicon](https://alexsci.com/blog/)

* * *

*This post is part of a [collection on UUIDs](https://alexsci.com/blog/uuid-collection/).*

I’m going to call it. UUIDv7 is weird.

Most blog posts about UUIDv7 tout these benefits:

- monotonically increasing IDs
- a helpful timestamp of when the object was created; and
- unguessable IDs

But none of these properties are strictly held. Certain RFC compliant implementations of UUIDv7 violate these properties to extreme degrees. The only useful property UUIDv7 appears to strictly provide is uniqueness and therefore collision resistance. I’ve already discussed how [UUIDv7 IDs can be guessable](https://alexsci.com/blog/uuids-and-idor/), so this post explores the other properties.

## Is the timestamp useful?

Superficially, it seems that embedding a timestamp in the ID can remove the need to track the creation time in a separate database column, providing marginally more efficient database operations. If you need to list things ordered by creation time very often, it’s useful if the primary index gives that behavior, removing the need for another index.

Unfortunately, the timestamp embedded in a UUIDv7 ID may not relate to the object creation time. The Postgres [docs for `uuid_extract_timestamp`](https://alexsci.com/blog/uuidv7-is-weird/www.postgresql.org/docs/current/functions-uuid.html) helpfully warn that “the extracted timestamp is not necessarily exactly equal to the time the UUID was generated”.

[RFC 9562](https://www.rfc-editor.org/rfc/rfc9562.html) allows implementations to alter the timestamp field, using values that aren’t strictly derived from the system clock. This relates to the tension between UUID monotonicity and timestamp accuracy. In certain scenarios, the difference between the timestamp and the current time can be arbitrarily large.

> Implementations MAY alter the actual timestamp. — [RFC 9562 §6.1](https://www.rfc-editor.org/rfc/rfc9562.html#name-timestamp-considerations)

> This specification makes no requirement or guarantee about how close the clock value needs to be to the actual time. — [RFC 9562 §6.1](https://www.rfc-editor.org/rfc/rfc9562.html#name-timestamp-considerations)

> … implementations MAY increment the timestamp ahead of the actual time … — [RFC 9562 §6.2](https://www.rfc-editor.org/rfc/rfc9562.html#name-monotonicity-and-counters)

Incrementing the timestamp ahead of the system clock is an interesting problem. If the generator is seeing so much load that a counter field is regularly overflowing then the timestamp could accelerate into the future. Using a sufficiently large counter would be wise and the RFC says the minimum size SHOULD be 12 bits. A counter overflow would require a pretty aggressive creation rate, over 4,096 IDs per millisecond per node, which is beyond the limits of many systems. This overflow becomes relevant, however, if the timestamp freezes; which I’ll discuss in a later section.

### Example: Python uuid.uuid7()

One prominent implementation that increments the timestamp is the Python standard library’s UUIDv7. The increment happens only in certain circumstances, like when the counter overflows. Unfortunately, the generator can get stuck in an increment cycle if the clock ever jumps backwards and the timestamp can race ahead of the system time by an arbitrarily large amount.

Here’s a simple example:

```
import uuid
from datetime import datetime

# Prime the generator with the current time
t = uuid.uuid7().time
now = datetime.now()
delta = (datetime.fromtimestamp(t/1000) - now).total_seconds()
print("Initial delta: ", delta)
# Ask the user to change the time
input("Set clock back one hour")

while True:
    for _ in range(100000):
        uuid.uuid7()
    t = uuid.uuid7().time
    now = datetime.now()
    delta = (datetime.fromtimestamp(t/1000) - now).total_seconds()
    print("New delta: ", delta)
```

Which gives:

```
$ uv run --python 3.14 main.py
Initial delta:  -0.000748
Set clock back one hour
New delta:  3695.277581
New delta:  3795.042411
New delta:  3894.80756
New delta:  3994.573106
^C
```

We expect the difference to be 3600 seconds (one hour) as the implementation refuses to allow the embedded timestamp to jump backwards and we expect the difference to decrease as the system clock catches up to the frozen timestamp. But when the load is above one ID per millisecond the difference will increase without bound. At lower loads it will take longer for the system clock to catch the timestamp, but we don’t see the runaway effect.

The [related issue](https://github.com/python/cpython/issues/138862#issuecomment-4232986682) shows Python developers reasoning that this is consistent with the RFC, is consistent with other implementations, and that there isn’t “much we can do” on a “flawed system”. I think these points are correct as I see this same behavior in other implementations. Nevertheless, it would be helpful if existing UUIDv7 implementations could handle system clock jumps without runaway behavior. There are many real-world examples of clocks jumping backwards, [even decades out of sync](https://www.itnews.com.au/news/telstra-broke-its-network-with-undocumented-time-fix-627442) in time-sensitive environments.

## Is leaking a timestamp harmful?

Embedding the timestamp in the ID means that you cannot share the ID without also sharing the timestamp.

It’s pretty difficult to think of an example where this really matters. One example could be a user profile page that doesn’t disclose when the account was created, but the UUIDv7 ID in the URL leaks the high-precision timestamp.

This issue would typically be very low on my list of security concerns because I find much more severe problems when I do security assessments. Nevertheless, there are contexts where this matters and engineering resources would be focused on addressing it. If you’re in the latter camp, be wary of UUIDv7 leaking metadata.

## Are UUIDv7s monotonic?

This section is even more pedantic, so skip ahead if you’ve had enough. Briefly: UUIDv7 IDs provide some sort of rough order, which may be suitable for your requirements.

When I was reading the UUIDv7 RFC I was surprised to see it describing a method for creating IDs across different nodes that are monotonically ordered. This was [proven impossible back in the 1970s](https://en.wikipedia.org/wiki/Lamport_clock), and lead to the development of vector clocks and similar technology. At a high level, the problem is that system clocks cannot be synchronized to a suitable precision. Ordering things by timestamp only works if you know precisely what time it is. Any uncertainty in the time results in uncertainty of event ordering.

Succinctly: Timestamps are only time-ordered if you know what time it is and you don’t know what time it is.

For this reason, similar ID formats claim to be nearly sorted. It seems odd that UUIDv7 makes a stricter claim and does not deliver on them. At the very least, the [challenges of synchronizing clocks across systems](https://alexsci.com/blog/clock-skew/) make the millisecond and especially the sub-millisecond timestamps quite suspect.

But even on a single node, using a single implementation, UUIDv7 IDs are not strictly monotonic. The easiest way to get out-of-order UUIDv7 IDs is to use an implementation that uses a millisecond timestamp without a counter. This approach [trivially generates out-of-order IDs inside a small for loop](https://github.com/fboulnois/pg_uuidv7/issues/10).

So no, there’s nothing in the UUIDv7 specification that would ensure the IDs are strictly monotonically increasing. Specific implementations may do better than others (with sub-millisecond timestamps, counters, well-synchronized clocks, etc), but we’ve known since the 1970s that this problem is not strictly solvable.

In UUIDv7’s favor, roughly ordered IDs are still quite useful. So don’t rip UUIDv7 out of your systems just yet. Just be wary of what assumptions you have.

## Restarting a UUIDv7 generator

The RFC has lots of discussion about the handling of clocks that jump backwards. Timestamps are only time-ordered if clocks tick forwards, so special care is needed.

We’ve already discussed one mitigation: tracking the last generated timestamp to ensure that an earlier timestamp is never used. To do this safely you’ll need to write the last used timestamp to persistent storage, such that you can read it again after a system reboot or a process restart.

I’ve looked at several UUIDv7 implementations I couldn’t find one that writes timestamps to persistent storage. So implementers seems disinterested in addressing the issue and they generate out-of-order IDs when a process restart follows a clock jumping backwards.

Looking at real-world clock synchronization problems, you’ll find many examples of [clocks getting set to future dates](https://arstechnica.com/security/2023/08/windows-feature-that-resets-system-clocks-based-on-random-data-is-wreaking-havoc/). When this happens, the backwards jump would align the timestamp to the actual time, but a last-used-timestamp guard would force timestamps to use keep using that incorrect timestamp until the system clock catches up. If the system clock takes a long time to catch up, you may start experiencing counter rollovers.

## So what is UUIDv7?

My mental model for UUIDv7 is that it’s a mash-up of a bunch of different ideas which implementations are free to choose as they see fit. There’s a wide variance of properties that UUIDv7 implementations can provide, with varying degrees of certainty they can successfully provide those properties. If you’re building your own implementation you can likely build something that has the properties you need. But most of us want to reach for an off-the-shelf version, especially when standard libraries provide an implementation, so we’re generally at the mercy of someone else’s implementation decisions. Implementations can change over time, making broad changes to their behavior while still being RFC compliant. Distributed systems may have multiple implementations across their subsystems, each with distinct properties. The only thing you can really expect is strong collision avoidance and ID uniqueness.

Be sure to ask these questions when picking a UUIDv7 implementation:

- Is the implementation compliant with the RFC?
- What additional properties does this implementation guarantee?
- Under what limits can those guarantees be met?
- Are there any assumptions required for these properties to hold, such as having an accurate system clock?
- Under what conditions will the implementation alter the timestamp?
- How guessable are the generated IDs?

Unfortunately, this sort of analysis is much more difficult than building an RFC compliant generator. Authors of UUIDv7 generators may not know the answers to these questions and may themselves be surprised that the RFC does handle this for them.

* * *

If you'd like to show your appreciation and help support my writing, you can do so via this link.

 Statements are my own and do not represent the positions or opinions of my employer.
