---
title: Why we don’t have benchmarks comparing Redis with other DBs
link: http://antirez.com/news/85
source: antirez-com
published: 2015-01-29T09:21:41Z
updated: 2015-01-29T09:21:41Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'Redis speed could be one selling point for new users, so following the trend of comparative “advertising” it should be logical to have a few comparisons at Redis.io. However there are two problems with this. One is of goals: I don’t want to convince developers to adopt Redis, we just do our best in order to provide a suitable product, and we are happy if people can get work done with it, that’s where my marketing wishes end. There is more: it is almost always impossible to compare different systems in a fair way. When you compare two databases, to get fair numbers, they need to share *a lot*: data model, exact durability guarantees, data replication safety, availability during partitions, and so forth: often a system will score in a lower way than another system since it sacrifices speed to provide less “hey look at me” qualities but that are very important nonetheless. Moreover the testing suite is a complex matter as well unless different database systems talk the same exact protocol: differences in the client library alone can contribute for large differences. However there are people that beg to differ, and believe comparing different database systems for speed is a good idea anyway. For example, yesterday a benchmark of Redis and AerospikeDB was published here: http://lynnlangit.com/2015/01/28/lessons-learned-benchmarking-nosql-on-the-aws-cloud-aerospikedb-and-redis/. I’ll use this benchmark to show my point about how benchmarks are misleading beasts. In the benchmark huge EC2 instances are used, for some strange reason, since the instances are equipped with 244 GB of RAM (!). Those are R3.8xlarge instances. For my tests I’ll use a more real world m3.medium instance. Using such a beast of an instance Redis scored, in the single node case, able to provide 128k ops per second. My EC2 instance is much more limited, testing from another EC2 instance with Redis benchmark, not using pipelining, and with the same 100 bytes data size, I get 32k ops/sec, so my instance is something like 4 times slower, in the single process case. Let’s see with Redis INFO command how the system is using the CPU during this benchmark: # CPU used_cpu_sys:181.78 used_cpu_user:205.05 used_cpu_sys_children:0.12 used_cpu_user_children:0.87 127.0.0.1:6379> info cpu … after 10 seconds of test … # CPU used_cpu_sys:184.52 used_cpu_user:206.42 used_cpu_sys_children:0.12 used_cpu_user_children:0.87 Redis spent ~ 3 seconds of system time, and only ~ 1.5 seconds in user space. What happens here is that for each request the biggest part of the work is to perform the read() and write() call. Also since it’s one-query one-reply workload for each client, we pay a full RTT for each request of each client. Now let’s check what happens if I use pipelining instead, a feature very known and much exploited by Redis users, since it’s the only way to maximize the server usage, and there are usually a number of places in the application where you can perform multiple operations at a given time. With a pipeline of 32 operations the numbers changed drastically. My tiny instance was able to deliver 250k ops/sec using a single core, which is 25% of the *top* result using 32 (each faster) cores in the mentioned benchmark. Let’s look at the CPU time: # CPU used_cpu_sys:189.16 used_cpu_user:216.46 used_cpu_sys_children:0.12 used_cpu_user_children:0.87 127.0.0.1:6379> info cpu … after 10 seconds of test … # CPU used_cpu_sys:190.60 used_cpu_user:224.92 used_cpu_sys_children:0.12 used_cpu_user_children:0.87 This time we are actually using the database engine to serve queries with our CPU, we are not just losing much of the time context switching. We used ~1.5 seconds of system time, and 8.46 seconds into the Redis process itself. Using lower numbers in the pipeline gets us results in the middle. Pipeline of 4 = 100k ops/sec (that should translate to ~ 400k ops/sec in the bigger instance used in the benchmark), pipeline 8 = 180k ops/sec, and so forth. So basically it is not a coincidence that benchmarking Redis and AerospikeDB in this way we get remarkably similar results. More or less you are not testing the databases, but the network stack and the kernel. If the DB can serve queries using a read and a write system call without any other huge waste, this is what we get, and here the time to serve the actual query is small since we are talking about data fitting into memory (just a note, 10M keys of 100k in Redis will use a fraction of the memory that was allocated in those instances). However there is more about that. What about the operations we can perform? To test Redis doing GET/SET is like to test a Ferrari checking how good it is at cleaning the mirror when it rains. A fundamental part of the Redis architecture is that largely different operations have similar costs, so what about our huge Facebook game posting scores of the finished games to create the leaderboard? The same single process can do 110k ops/sec when the query is: ZADD myscores . But let’s demand more, what about estimating the cardinality with the HyperLogLog, at the same time adding new elements and reading the current guess with two redis-benchmark processes? Set size is 10 millions again. So during this test I spawned a benchmark executing PFADD with random elements of the set, and another doing PFCOUNT at the same time in the same HyperLogLog. Both scored simultaneously at 250k ops/sec, for a total of half a million ops per second with a single Redis process. In Redis doing complex operations is similar to pipelining. You want to do *more* for each read/write, otherwise your performance is dominated by I/O. Ok, so a few useful remarks. 1) GET/SET Benchmarks are not a great way to compare different database systems. 2) A better performance comparison is by use case. You say, for a given specific use case, using different data model, schema, queries, strategies, how much instances I need to handle the same traffic for the same app with two different database systems? 3) Test with instance types most people are going to actually use, huge instance types can mask inefficiencies of certain database systems, and is anyway not what most people are going to use. We’ll continue to optimize Redis for speed, and will continue to avoid posting comparative benchmarks. [Thanks to Amazon AWS for providing me free access to EC2] Comments'
content: extracted
html: 2015-01-29-why-we-don-t-have-benchmarks-comparing-redis-with-other-dbs.html
---

## [Why we don’t have benchmarks comparing Redis with other DBs](https://antirez.com/news/85)

[antirez](https://antirez.com/user/antirez) 4251 days ago. 312357 views.

```
Redis speed could be one selling point for new users, so following the trend of comparative “advertising” it should be logical to have a few comparisons at Redis.io. However there are two problems with this. One is of goals: I don’t want to convince developers to adopt Redis, we just do our best in order to provide a suitable product, and we are happy if people can get work done with it, that’s where my marketing wishes end. There is more: it is almost always impossible to compare different systems in a fair way.

When you compare two databases, to get fair numbers, they need to share *a lot*: data model, exact durability guarantees, data replication safety, availability during partitions, and so forth: often a system will score in a lower way than another system since it sacrifices speed to provide less “hey look at me” qualities but that are very important nonetheless. Moreover the testing suite is a complex matter as well unless different database systems talk the same exact protocol: differences in the client library alone can contribute for large differences.

However there are people that beg to differ, and believe comparing different database systems for speed is a good idea anyway. For example, yesterday a benchmark of Redis and AerospikeDB was published here: http://lynnlangit.com/2015/01/28/lessons-learned-benchmarking-nosql-on-the-aws-cloud-aerospikedb-and-redis/.

I’ll use this benchmark to show my point about how benchmarks are misleading beasts. In the benchmark huge EC2 instances are used, for some strange reason, since the instances are equipped with 244 GB of RAM (!). Those are R3.8xlarge instances. For my tests I’ll use a more real world m3.medium instance.

Using such a beast of an instance Redis scored, in the single node case, able to provide 128k ops per second. My EC2 instance is much more limited, testing from another EC2 instance with Redis benchmark, not using pipelining, and with the same 100 bytes data size, I get 32k ops/sec, so my instance is something like 4 times slower, in the single process case.

Let’s see with Redis INFO command how the system is using the CPU during this benchmark:

# CPU
used_cpu_sys:181.78
used_cpu_user:205.05
used_cpu_sys_children:0.12
used_cpu_user_children:0.87
127.0.0.1:6379> info cpu

… after 10 seconds of test …

# CPU
used_cpu_sys:184.52
used_cpu_user:206.42
used_cpu_sys_children:0.12
used_cpu_user_children:0.87

Redis spent ~ 3 seconds of system time, and only ~ 1.5 seconds in user space. What happens here is that for each request the biggest part of the work is to perform the read() and write() call. Also since it’s one-query one-reply workload for each client, we pay a full RTT for each request of each client.

Now let’s check what happens if I use pipelining instead, a feature very known and much exploited by Redis users, since it’s the only way to maximize the server usage, and there are usually a number of places in the application where you can perform multiple operations at a given time.

With a pipeline of 32 operations the numbers changed drastically. My tiny instance was able to deliver 250k ops/sec using a single core, which is 25% of the *top* result using 32 (each faster) cores in the mentioned benchmark.

Let’s look at the CPU time:

# CPU
used_cpu_sys:189.16
used_cpu_user:216.46
used_cpu_sys_children:0.12
used_cpu_user_children:0.87
127.0.0.1:6379> info cpu

… after 10 seconds of test …

# CPU
used_cpu_sys:190.60
used_cpu_user:224.92
used_cpu_sys_children:0.12
used_cpu_user_children:0.87

This time we are actually using the database engine to serve queries with our CPU, we are not just losing much of the time context switching. We used ~1.5 seconds of system time, and 8.46 seconds into the Redis process itself.

Using lower numbers in the pipeline gets us results in the middle. Pipeline of 4 = 100k ops/sec (that should translate to ~ 400k ops/sec in the bigger instance used in the benchmark), pipeline 8 = 180k ops/sec, and so forth.

So basically it is not a coincidence that benchmarking Redis and AerospikeDB in this way we get remarkably similar results. More or less you are not testing the databases, but the network stack and the kernel. If the DB can serve queries using a read and a write system call without any other huge waste, this is what we get, and here the time to serve the actual query is small since we are talking about data fitting into memory (just a note, 10M keys of 100k in Redis will use a fraction of the memory that was allocated in those instances).

However there is more about that. What about the operations we can perform? To test Redis doing GET/SET is like to test a Ferrari checking how good it is at cleaning the mirror when it rains.

A fundamental part of the Redis architecture is that largely different operations have similar costs, so what about our huge Facebook game posting scores of the finished games to create the leaderboard?

The same single process can do 110k ops/sec when the query is: ZADD myscores <random-int> <random-int>.

But let’s demand more, what about estimating the cardinality with the HyperLogLog, at the same time adding new elements and reading the current guess with two redis-benchmark processes? Set size is 10 millions again. So during this test I spawned a benchmark executing PFADD with random elements of the set, and another doing PFCOUNT at the same time in the same HyperLogLog. Both scored simultaneously at 250k ops/sec, for a total of half a million ops per second with a single Redis process.

In Redis doing complex operations is similar to pipelining. You want to do *more* for each read/write, otherwise your performance is dominated by I/O.

Ok, so a few useful remarks. 1) GET/SET Benchmarks are not a great way to compare different database systems. 2) A better performance comparison is by use case. You say, for a given specific use case, using different data model, schema, queries, strategies, how much instances I need to handle the same traffic for the same app with two different database systems? 3) Test with instance types most people are going to actually use, huge instance types can mask inefficiencies of certain database systems, and is anyway not what most people are going to use.

We’ll continue to optimize Redis for speed, and will continue to avoid posting comparative benchmarks.

[Thanks to Amazon AWS for providing me free access to EC2]
```

[blog comments powered by Disqus](https://disqus.com)
