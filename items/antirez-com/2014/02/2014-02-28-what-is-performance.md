---
title: What is performance?
link: http://antirez.com/news/73
source: antirez-com
published: 2014-02-28T13:30:42Z
updated: 2014-02-28T13:30:42Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'The title of this blog post is an apparently trivial to answer question, however it is worth to consider a bit better what performance really means: it is easy to get confused between scalability and performance, and to decompose performance, in the specific case of database systems, in its different main components, may not be trivial. In this short blog post I’ll try to write down my current idea of what performance is in the context of database systems. A good starting point is probably the first slide I use lately in my talks about Redis. This first slide is indeed about performance, and says that performance is mainly three different things. 1) Latency: the amount of time I need to get the reply for a query. 2) Operations per unit of time per core: how many queries (operations) the system is able to reply per second, in a given reference computational unit? 3) Quality of operations: how much work those operations are able to accomplish? Latency — This is probably the simplest component of performance. In many applications it is desirable that the time needed to get a reply from the system is small. However while the average time is important, another concern is the predictability of the latency figure, and how much difference there is between the average case and the worst case. When used well, in-memory systems are able to provide very good latency characteristics, and are also able to provide a consistent latency over time. Operations per second per core — The second component I’m enumerating is what makes the difference between raw performance and scalability. We are interested in the amount of work the system is able to do, in a given unit of time, for a given reference computational unit. Linearly scalable systems can reach a big number of operations per second by using a number of nodes, however this means they are scalable, and not necessarily performant. Operations per second per core is also usually bound to the amount of queries you can perform per watt, so to the energy efficiency of the system. Quality of operations — The last point, while probably not as stressed among developers as throughput and latency, is really important in certain kind of systems, especially in-memory systems. A system that is able to perform 100 operations per second, but with operations of “poor quality” (for example just GET and SET in Redis terms) has a lower performance compared to a system that is also able to perform an INCR operation with the same latency and OPS characteristics. For instance, if the problem at hand is to increment counters, the former system will require two operations to increment a counter (we are not considering race conditions in this context), while the system providing INCR is able to use a single operation. As a result it is actually able to provide twice the performance of the former system. As you can see the quality of operations is not an absolute meter, but depends on the kind of problem to solve. The same two systems if we want to cache HTML fragments are equivalent since the INCR operation would be useless. The quality of operations is particularly important in in-memory systems, since usually the computation itself is negligible compared to the time needed to receive, dispatch the command, and create a reply, so systems like Redis with a rich set of operations are able to provide better performance in many contexts almost for free, just allowing the user to do more with a single operation. The “do more” part can actually mean a lot of things: either provide a reply to a more complex question, like for example the ZRANK command of Redis, or simply being able to provide a more *selective* reply, like HMGET command that is able to provide information just for a subset of the fields composing an Hash value, reducing the amount of bandwidth required between the server and its clients. In general quality of operations don''t only affect performances because they give less or more value to the operations per second the system is able to perform: operations quality also directly affect latency, since more complex operations are able to avoid back and forth data transfer between clients and servers required to mount multiple simpler operations into a more complex computation. Conclusion — I hope that this short exploration of what performance is uncovered some of the complexities involved in the process of evaluating the capabilities of a database system from this specific point of view. There is a lot more to say about it, but I found that the above three components of the performance are among the most interesting and important when evaluating a system and when there is to understand how to evolve an existing system to improve its performance characteristics. Thanks to Yiftach Shoolman for feedbacks about this topic. Comments'
content: extracted
html: 2014-02-28-what-is-performance.html
---

## [What is performance?](https://antirez.com/news/73)

[antirez](https://antirez.com/user/antirez) 4586 days ago. 293211 views.

```
The title of this blog post is an apparently trivial to answer question, however it is worth to consider a bit better what performance really means: it is easy to get confused between scalability and performance, and to decompose performance, in the specific case of database systems, in its different main components, may not be trivial. In this short blog post I’ll try to write down my current idea of what performance is in the context of database systems.

A good starting point is probably the first slide I use lately in my talks about Redis. This first slide is indeed about performance, and says that performance is mainly three different things.

1) Latency: the amount of time I need to get the reply for a query.

2) Operations per unit of time per core: how many queries (operations) the system is able to reply per second, in a given reference computational unit?

3) Quality of operations: how much work those operations are able to accomplish?

Latency
—

This is probably the simplest component of performance. In many applications it is desirable that the time needed to get a reply from the system is small. However while the average time is important, another concern is the predictability of the latency figure, and how much difference there is between the average case and the worst case. When used well, in-memory systems are able to provide very good latency characteristics, and are also able to provide a consistent latency over time.

Operations per second per core
—

The second component I’m enumerating is what makes the difference between raw performance and scalability. We are interested in the amount of work the system is able to do, in a given unit of time, for a given reference computational unit. Linearly scalable systems can reach a big number of operations per second by using a number of nodes, however this means they are scalable, and not necessarily performant.

Operations per second per core is also usually bound to the amount of queries you can perform per watt, so to the energy efficiency of the system. 

Quality of operations
—

The last point, while probably not as stressed among developers as throughput and latency, is really important in certain kind of systems, especially in-memory systems.

A system that is able to perform 100 operations per second, but with operations of “poor quality” (for example just GET and SET in Redis terms) has a lower performance compared to a system that is also able to perform an INCR operation with the same latency and OPS characteristics.
For instance, if the problem at hand is to increment counters, the former system will require two operations to increment a counter (we are not considering race conditions in this context), while the system providing INCR is able to use a single operation. As a result it is actually able to provide twice the performance of the former system.

As you can see the quality of operations is not an absolute meter, but depends on the kind of problem to solve. The same two systems if we want to cache HTML fragments are equivalent since the INCR operation would be useless.

The quality of operations is particularly important in in-memory systems, since usually the computation itself is negligible compared to the time needed to receive, dispatch the command, and create a reply, so systems like Redis with a rich set of operations are able to provide better performance in many contexts almost for free, just allowing the user to do more with a single operation. The “do more” part can actually mean a lot of things: either provide a reply to a more complex question, like for example the ZRANK command of Redis, or simply being able to provide a more *selective* reply, like HMGET command that is able to provide information just for a subset of the fields composing an Hash value, reducing the amount of bandwidth required between the server and its clients.

In general quality of operations don't only affect performances because they give less or more value to the operations per second the system is able to perform: operations quality also directly affect latency, since more complex operations are able to avoid back and forth data transfer between clients and servers required to mount multiple simpler operations into a more complex computation.

Conclusion
—

I hope that this short exploration of what performance is uncovered some of the complexities involved in the process of evaluating the capabilities of a database system from this specific point of view. There is a lot more to say about it, but I found that the above three components of the performance are among the most interesting and important when evaluating a system and when there is to understand how to evolve an existing system to improve its performance characteristics.

Thanks to Yiftach Shoolman for feedbacks about this topic.
```

[blog comments powered by Disqus](https://disqus.com)
