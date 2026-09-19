---
title: The binary search of distributed programming
link: http://antirez.com/news/102
source: antirez-com
published: 2016-02-13T16:49:13Z
updated: 2016-02-13T16:49:13Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'Yesterday night I was re-reading Redlock analysis Martin Kleppmann wrote (http://martin.kleppmann.com/2016/02/08/how-to-do-distributed-locking.html). At some point Martin wonders if there is some good way to generate monotonically increasing IDs with Redis. This apparently simple problem can be more complex than it looks at a first glance, considering that it must ensure that, in all the conditions, there is a safety property which is always guaranteed: the ID generated is always greater than all the past IDs generated, and the same ID cannot be generated multiple times. This must hold during network partitions and other failures. The system may just become unavailable if there are less than the majority of nodes that can be reached, but never provide the wrong answer (note: as we''ll see this algorithm has another liveness issue that happens during high load of requests). So for the sake of playing a bit more with distributed systems algorithms, and learn a bit more in the process, I tried to find a solution. Actually I was aware of an algorithm that could solve the problem. It’s an inefficient one, not suitable to generate tons of IDs per second. Many complex distributed algorithms, like Raft and Paxos, use it as a step in order to get monotonically increasing IDs, as a foundation to mount the full set of properties they need to provide. This algorithm is fascinating since it’s extremely easy to understand and implement, and because it’s very intuitive to understand *why* it works. I could say, it is the binary search of distributed algorithms, something easy enough but smart enough to let newcomers to distributed programming to have an ah!-moment. However I had to modify the algorithm in order to adapt it to be implemented in the client side. Hopefully it is still correct (feedbacks appreciated). While I’m not going to use this algorithm in order to improve Redlock (see my previous blog post), I think that trying to solve this kind of problems is both a good exercise, and it may be an interesting read for people approaching distributed systems for the first times looking for simple problems to play with in real world systems. ## How it works? The algorithm requirements are the following two: 1. A data store that supports the operation set_if_less_than(). 2. A data store that can fsync() data to disk on writes, before replying to the client. The above includes almost any *SQL server, Redis, and a number of other stores. We have a set of N nodes, let’s assume N = 5 for simplicity in order to explain the algorithm. We initialize the system by setting a key called “current” to the value of 0, so in Redis terms, we do: SET current 0 In all the 5 instances. This is part of the initialization and must be done only when a new “cluster” is initialized. This step can be skipped but makes the explanation simpler. In order to generate a new ID, this is what we do: 1: Get the “current” value from the majority of instances (3 or more if N=5). 2: If we failed to reach 3 instances, GOTO 1. 3: Get the maximum value among the ones we obtained, and increment it by 1. Let’s call this $NEXTID 4: Send the following write operation to all the nodes we are able to reach. IF current < $NEXTID THEN SET current $NEXTID return $NEXTID ELSE return NULL END 5: If 3 or more instances will reply $NEXTID, the algorithm succeeded, and we successfully generated a new monotonically increasing ID. 6: Otherwise, if we have not reached the majority, GOTO 1. What we send at step 4 can be easily translated to a simple Redis Lua script: local val = tonumber(redis.call(''get'',KEYS[1])) local nextid = tonumber(ARGV[1]) if val < nextid then redis.call(''set'',KEYS[1],nextid) return nextid else return nil end ## Is it safe? The reason why I intuitively believe that it works, other than the fact that, in a modified form is used as a step in deeply analyzed algorithms, is that: If we are able to obtain the majority of “votes”, it is impossible by definition that any other client obtained the majority for an ID greater or equal to the one we generated. Otherwise there were already 3 or more instances with a value of current >= $NEXTID, and it would be impossible for us to get the majority. So the IDs generated are always greater than the past IDs, and for the same condition it is impossible for two instances to generate the same ID. Maybe some kind reader could point to a bug in the algorithm or to an analysis of this algorithm as used in other analyzed systems, however given that the above is adapted to be executed client-side, there are actually more processes involved and it should be re-analyzed in order to provide a proof that it is equivalent. ## Why it’s a slow algorithm? The problem with this algorithm is concurrent accesses. If many clients are trying to generate new IDs at the same time, nobody may get the majority, and they will need to retry again, with larger numbers. Note that this also means that there could be “holes” in the sequence of generated IDs, so the clients could be able to generate the sequence 1, 2, 6, 10, 11, 21, … Because many numbers could be “burn” by the split brain condition caused by concurrent access. (Note that in the above sentence "split brain" does not mean that there is an inconsistent state between the nodes, just that it was not possible to reach the majority to agree about a given ID. Usually split brain refers to a conflict in the configuration where, for example, multiple nodes claim to be a master. However in the Raft paper the term split brain is used with the same meaning I''m using here). How many IDs per second you can generate without incurring frequent failures due to concurrent accesses depends on the network RTT and number of concurrent clients. What’s interesting however is that you can make the algorithm more scalable by creating an “IDs server” that talks to the cluster, and mediates the access of the clients by serializing the generation of the new IDs one after the other. This does not create a single point of failure since you don’t need to have a single ID server, you can run a few for redundancy, and have hundreds of clients connecting to it. Using this schema to generate 5k IDs/sec, should be viable, especially if the client is implemented in a smart way, trying to send the requests to the 5 nodes at the same time, using some multiplexing or threaded approach. Another approach when there are many clients and no node mediating the access is to use randomized and exponential delays to contact the nodes again, if a round of the algorithm failed. ## Why fsync is needed? Here fsync at every write is mandatory because if nodes go down and restart, they MUST have the latest value of the “current” key. If the value of current returns backward, it is possible to violate our safety property that states that the newly generated IDs are always greater than any other ID generated in the past. However if you use a full replicated FSM to achieve the same goal, you need fsync anyway (but you don’t have the problem of the concurrent accesses. For example using Raft in normal conditions you have a single leader you can send requests to). So, in the case of Redis, it must be configured with AOF enabled and the AOF fsync policy set to always, to make sure the write is always persisted before to reply to the client. ## What do I do with those IDs A set of IDs like that, have a property which is called “total ordering”, so they are very useful in different contexts. Usually it is hard with distributed computations to say what happened before and what happened after. Using those IDs you always know the order of certain events. To give you a simple example: different processes, using this system, may compute a list of items, and take each sub-list in their local storage. At the end, they could merge the multiple lists and get a final list in the right order, as if there was a single shared list since the start where each process was able to append an item. ## Origins of this algorithm What described here closely resembles both Paxos first phase and Raft leader election. However it seems just a special case of Lamport timestamp where the majority is used in order to create total ordering. Many thanks to Max Neunhoeffer and Martin Kleppmann for feedbacks about the initial draft of this blog post. Note that any error is mine. Comments'
content: extracted
html: 2016-02-13-the-binary-search-of-distributed-programming.html
---

## [The binary search of distributed programming](https://antirez.com/news/102)

[antirez](https://antirez.com/user/antirez) 3871 days ago. 279831 views.

```
Yesterday night I was re-reading Redlock analysis Martin Kleppmann wrote (http://martin.kleppmann.com/2016/02/08/how-to-do-distributed-locking.html). At some point Martin wonders if there is some good way to generate monotonically increasing IDs with Redis.

This apparently simple problem can be more complex than it looks at a first glance, considering that it must ensure that, in all the conditions, there is a safety property which is always guaranteed: the ID generated is always greater than all the past IDs generated, and the same ID cannot be generated multiple times. This must hold during network partitions and other failures. The system may just become unavailable if there are less than the majority of nodes that can be reached, but never provide the wrong answer (note: as we'll see this algorithm has another liveness issue that happens during high load of requests).

So for the sake of playing a bit more with distributed systems algorithms, and learn a bit more in the process, I tried to find a solution.
Actually I was aware of an algorithm that could solve the problem. It’s an inefficient one, not suitable to generate tons of IDs per second. Many complex distributed algorithms, like Raft and Paxos, use it as a step in order to get monotonically increasing IDs, as a foundation to mount the full set of properties they need to provide. This algorithm is fascinating since it’s extremely easy to understand and implement, and because it’s very intuitive to understand *why* it works. I could say, it is the binary search of distributed algorithms, something easy enough but smart enough to let newcomers to distributed programming to have an ah!-moment.

However I had to modify the algorithm in order to adapt it to be implemented in the client side. Hopefully it is still correct (feedbacks appreciated). While I’m not going to use this algorithm in order to improve Redlock (see my previous blog post), I think that trying to solve this kind of problems is both a good exercise, and it may be an interesting read for people approaching distributed systems for the first times looking for simple problems to play with in real world systems.

## How it works?

The algorithm requirements are the following two:

1. A data store that supports the operation set_if_less_than().

2. A data store that can fsync() data to disk on writes, before replying to the client.

The above includes almost any *SQL server, Redis, and a number of other stores.

We have a set of N nodes, let’s assume N = 5 for simplicity in order to explain the algorithm.
We initialize the system by setting a key called “current” to the value of 0, so in Redis terms, we do:

    SET current 0

In all the 5 instances. This is part of the initialization and must be done only when a new “cluster” is initialized. This step can be skipped but makes the explanation simpler.

In order to generate a new ID, this is what we do:

1: Get the “current” value from the majority of instances (3 or more if N=5).

2: If we failed to reach 3 instances, GOTO 1.

3: Get the maximum value among the ones we obtained, and increment it by 1. Let’s call this $NEXTID

4: Send the following write operation to all the nodes we are able to reach.

    IF current < $NEXTID THEN
        SET current $NEXTID
        return $NEXTID
    ELSE
        return NULL
    END

5: If 3 or more instances will reply $NEXTID, the algorithm succeeded, and we successfully generated a new monotonically increasing ID.

6: Otherwise, if we have not reached the majority, GOTO 1.

What we send at step 4 can be easily translated to a simple Redis Lua script:

    local val = tonumber(redis.call('get',KEYS[1]))
    local nextid = tonumber(ARGV[1])

    if val < nextid then
        redis.call('set',KEYS[1],nextid)
        return nextid
    else
        return nil
    end

## Is it safe?

The reason why I intuitively believe that it works, other than the fact that, in a modified form is used as a step in deeply analyzed algorithms, is that:

If we are able to obtain the majority of “votes”, it is impossible by definition that any other client obtained the majority for an ID greater or equal to the one we generated. Otherwise there were already 3 or more instances with a value of current >= $NEXTID, and it would be impossible for us to get the majority. So the IDs generated are always greater than the past IDs, and for the same condition it is impossible for two instances to generate the same ID.

Maybe some kind reader could point to a bug in the algorithm or to an analysis of this algorithm as used in other analyzed systems, however given that the above is adapted to be executed client-side, there are actually more processes involved and it should be re-analyzed in order to provide a proof that it is equivalent.

## Why it’s a slow algorithm?

The problem with this algorithm is concurrent accesses. If many clients are trying to generate new IDs at the same time, nobody may get the majority, and they will need to retry again, with larger numbers. Note that this also means that there could be “holes” in the sequence of generated IDs, so the clients could be able to generate the sequence 1, 2, 6, 10, 11, 21, … Because many numbers could be “burn” by the split brain condition caused by concurrent access.

(Note that in the above sentence "split brain" does not mean that there is an inconsistent state between the nodes, just that it was not possible to reach the majority to agree about a given ID. Usually split brain refers to a conflict in the configuration where, for example, multiple nodes claim to be a master. However in the Raft paper the term split brain is used with the same meaning I'm using here).

How many IDs per second you can generate without incurring frequent failures due to concurrent accesses depends on the network RTT and number of concurrent clients. What’s interesting however is that you can make the algorithm more scalable by creating an “IDs server” that talks to the cluster, and mediates the access of the clients by serializing the generation of the new IDs one after the other. This does not create a single point of failure since you don’t need to have a single ID server, you can run a few for redundancy, and have hundreds of clients connecting to it.

Using this schema to generate 5k IDs/sec, should be viable, especially if the client is implemented in a smart way, trying to send the requests to the 5 nodes at the same time, using some multiplexing or threaded approach.

Another approach when there are many clients and no node mediating the access is to use randomized and exponential delays to contact the nodes again, if a round of the algorithm failed.

## Why fsync is needed?

Here fsync at every write is mandatory because if nodes go down and restart, they MUST have the latest value of the “current” key. If the value of current returns backward, it is possible to violate our safety property that states that the newly generated IDs are always greater than any other ID generated in the past. However if you use a full replicated FSM to achieve the same goal, you need fsync anyway (but you don’t have the problem of the concurrent accesses. For example using Raft in normal conditions you have a single leader you can send requests to).

So, in the case of Redis, it must be configured with AOF enabled and the AOF fsync policy set to always, to make sure the write is always persisted before to reply to the client.

## What do I do with those IDs

A set of IDs like that, have a property which is called “total ordering”, so they are very useful in different contexts. Usually it is hard with distributed computations to say what happened before and what happened after. Using those IDs you always know the order of certain events.

To give you a simple example: different processes, using this system, may compute a list of items, and take each sub-list in their local storage. At the end, they could merge the multiple lists and get a final list in the right order, as if there was a single shared list since the start where each process was able to append an item.

## Origins of this algorithm

What described here closely resembles both Paxos first phase and Raft leader election. However it seems just a special case of Lamport timestamp where the majority is used in order to create total ordering.

Many thanks to Max Neunhoeffer and Martin Kleppmann for feedbacks about the initial draft of this blog post. Note that any error is mine.
```

[blog comments powered by Disqus](https://disqus.com)
