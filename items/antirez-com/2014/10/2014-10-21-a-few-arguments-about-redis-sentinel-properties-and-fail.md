---
title: A few arguments about Redis Sentinel properties and fail scenarios.
link: http://antirez.com/news/80
source: antirez-com
published: 2014-10-21T15:18:10Z
updated: 2014-10-21T15:18:10Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'Yesterday distributed systems expert Aphyr, posted a tweet about a Redis Sentinel issue experienced by an unknown company (that wishes to remain anonymous): “OH on Redis Sentinel "They kill -9''d the master, which caused a split brain..." “then the old master popped up with no data and replicated the lack of data to all the other nodes. Literally had to restore from backups." OMG we have some nasty bug I thought. However I tried to get more information from Kyle, and he replied that the users actually disabled disk persistence at all from the master process. Yep: the master was configured on purpose to restart with a wiped data set. Guess what? A Twitter drama immediately started. People were deeply worried for Redis users. Poor Redis users! Always in danger. However while to be very worried is a trait of sure wisdom, I want to take the other path: providing some more information. Moreover this blog post is interesting to write since actually Kyle, while reporting the problem with little context, a few tweets later was able to, IMHO, isolate what is the true aspect that I believe could be improved in Redis Sentinel, which is not related to the described incident, and is in my TODO list for a long time now. But before, let’s check a bit more closely the behavior of Redis / Sentinel about the drama-incident. Welcome to the crash recovery system model === Most real world distributed systems must be designed to be resilient to the fact that processes can restart at random. Note that this is very different from the problem of being partitioned away, which is, the inability to exchange messages with other processes. It is, instead, a matter of losing state. To be more accurate about this problem, we could say that if a distributed algorithm is designed so that a process must guarantee to preserve the state after a restart, and fails to do this, it is technically experiencing a bizantine failure: the state is corrupted, and the process is no longer reliable. Now in a distributed system composed of Redis instances, and Redis Sentinel instances, it is fundamental that rebooted instances are able to restart with the old data set. Starting with a wiped data set is a byzantine failure, and Redis Sentinel is not able to recover from this problem. But let’s do a step backward. Actually Redis Sentinel may not be directly involved in an incident like that. The typical example is what happens if a misconfigured master restarts fast enough so that no failure is detected at all by Sentinels. 1. Node A is the master. 2. Node A is restarted, with persistence disabled. 3. Sentinels (may) see that Node A is not reachable… but not enough to reach the configured timeout. 4. Node A is available again, except it restarted with a totally empty data set. 5. All the slave nodes B, C, D, ... will happily synchronize an empty data set form it. Everything wiped from the master, as per configuration, after all. And everything wiped from the slaves, that are replicating from what is believed to be the current source of truth for the data set. Let’s remove Sentinel from the equation, which is, point “3” of the above time line, since Sentinel did not acted at all in the example scenario. This is what you get. You have a Redis master replicating with N slaves. The master is restarted, configured to start with a fresh (empty) data set. Salves replicate again from it (an empty data set). I think this is not a big news for Redis users, this is how Redis replication works: slaves will always try to be the exact copy of their masters. However let’s consider alternative models. For example Redis instances could have a Node ID which is persisted in the RDB / AOF file. Every time the node restarts, it loads its Node ID. If the Node ID is wrong, slaves wont replicate from the master at all. Much safer right? Only marginally, actually. The master could have a different misconfiguration, so after a restart, it could reload a data set which is weeks old since snapshotting failed for some reason. So after a bad restart, we still have the right Node ID, but the dataset is so old to be basically, the same as being wiped more or less, just more subtle do detect. However at the cost of making things only marginally more secure we now have a system that may be more complex to operate, and slaves that are in danger of not replicating from the master since the ID does not match, because of operational errors similar to disabling persistence, except, a lot less obvious than that. So, let’s change topic, and see a failure mode where Sentinel is *actually* involved, and that can be improved. Not all replicas are the same === Technically Redis Sentinel offers a very limited set of simple to understand guarantees. 1) All the Sentinels will agree about the configuration as soon as they can communicate. Actually each sub-partition will always agree. 2) Sentinels can’t start a failover without an authorization from the majority of Sentinel processes. 3) Failovers are strictly ordered: if a failover happened later in time, it has a greater configuration “number” (config epoch in Sentinel slang), that will always win over older configurations. 4) Eventually the Redis instances are configured to map with the winning logical configuration (the one with the greater config epoch). This means that the dataset semantics is “last failover wins”. However the missing information here is, during a failover, what slave is picked to replace the master? This is, all in all, a fundamental property. For example if Redis Sentinel fails by picking a wiped slave (that just restarted with a wrong configuration), *that* is a problem with Sentinel. Sentinel should make sure that, even within the limits of the fact that Redis is an asynchronously replicated system, it will try to make the best interest of the user by picking the best slave around, and refusing to failover at all if there is no viable slave reachable. This is a place where improvements are possible, and this is what happens today to select a slave when the master is failing: 1) If a slaves was restarted, and never was connected with the master after the restart, performing a successful synchronization (data transfer), it is skipped. 2) If the slave is disconnected from its master for more than 10 times the configured timeout (the time a master should be not reachable for the set of Sentinels to detect a master as failing), it is considered to be non elegible. 3) Out of the remaining slaves, Sentinel picks the one with the best “replication offset”. The replication offset is a number that Redis master-slave replication uses to take a count of the amount of bytes sent via the replication channel. it is useful in many ways, not just for failovers. For example in partial resynchronizations after a net split, slaves will ask the master, give me data starting from offset X, which is the last byte I received, and so forth. However this replication number has two issues when used in the context of picking the best slave to promote. 1) It is reset after restarts. This sounds harmless at first, since we want to pick slaves with the higher number, and anyway, after a restart if a slave can’t connect, it is skipped. However it is not harmless at all, read more. 2) It is just a number: it does not imply that a Redis slave replicated from *a given* master. Also note that when a slave is promoted to master, it inherits the master’s replication offset. So modulo restarts, the number keeps increasing. Why “1” and/or “2” are suboptimal choices and can be improved? Imagine this setup. We have nodes A B C D E. D is the current master, and is partitioned away with E in a minority partition. E still replicates from D, everything is fine from their POV. However in the majority partition, A B C can exchange messages, and A is elected master. Later A restarts, resetting its offset. B and C replicate from it, starting again with lower offsets. After some time A fails, and, at the same time, E rejoins the majority partition. E has a data set that is less updated compared to the B and C data set, however its replication offset is higher. Not just that, actually E can claim it was recently connected to its master. To improve upon this is easy. Each Redis instance has a “runid”, an unique ID that changes for each new Redis run. This is useful in partial resynchronizations in order to avoid getting an incremental stream from a wrong master. Slaves should publish what is the last master run id they replicated successful from, and Sentinel failover should make sure to only pick slaves that replicated from the master they are actually failing over. Once you tight the replication offset to a given runid, what you get is an *absolute* meter of how much updated a slave is. If two slaves are available, and both can claim continuity with the old master, the one with the higher replication offset is guaranteed to be the best pick. However this also creates availability concerns in all the cases where data is not very important but availability is. For example if when A crashes, only E becomes available, even if it used to replicate from D, it is still better than nothing. I would say that when you need an highly available cache and consistency is not a big issue, to use a Redis cluster ala-memcached (client side consistent hashing among N masters) is the way to go. Note that even without checking the runid, to make the replication offsets durable after a restart, already improves the behavior considerably. In the above example E would be picked only if when isolated in a minority partition with the slave, received more writes than the other slaves in the majority side. TLDR: we have to fix this. It is not related to restarting masters without a dataset, but is useful to have a more correct implementation. However this will limit only a class of very-hard-to-trigger issues. This is in my TODO list for some time now, and congrats to Aphyr for identifying a real implementation issue in a matter of a few tweets exchanged. About the failure reported by Aphyr from the unknown company, I don’t think it is currently viable to try to protect against serious misconfigurations, however it is a clear sign that we need better Sentinel docs which are more incremental compared to the ones we have now, that try to describe how the system works. A wiser approach could be to start with a common sane configuration, and “don’t do” list like, don’t turn persistence off, unless you are ok with wiped instances. Comments'
content: extracted
html: 2014-10-21-a-few-arguments-about-redis-sentinel-properties-and-fail.html
---

[antirez](https://antirez.com/user/antirez) 4351 days ago. 330979 views.

```
Yesterday distributed systems expert Aphyr, posted a tweet about a Redis Sentinel issue experienced by an unknown company (that wishes to remain anonymous):

“OH on Redis Sentinel "They kill -9'd the master, which caused a split brain..."
“then the old master popped up with no data and replicated the lack of data to all the other nodes. Literally had to restore from backups."

OMG we have some nasty bug I thought. However I tried to get more information from Kyle, and he replied that the users actually disabled disk persistence at all from the master process. Yep: the master was configured on purpose to restart with a wiped data set.

Guess what? A Twitter drama immediately started. People were deeply worried for Redis users. Poor Redis users! Always in danger.

However while to be very worried is a trait of sure wisdom, I want to take the other path: providing some more information. Moreover this blog post is interesting to write since actually Kyle, while reporting the problem with little context, a few tweets later was able to, IMHO, isolate what is the true aspect that I believe could be improved in Redis Sentinel, which is not related to the described incident, and is in my TODO list for a long time now. 

But before, let’s check a bit more closely the behavior of Redis / Sentinel about the drama-incident.

Welcome to the crash recovery system model
===

Most real world distributed systems must be designed to be resilient to the fact that processes can restart at random. Note that this is very different from the problem of being partitioned away, which is, the inability to exchange messages with other processes. It is, instead, a matter of losing state.

To be more accurate about this problem, we could say that if a distributed algorithm is designed so that a process must guarantee to preserve the state after a restart, and fails to do this, it is technically experiencing a bizantine failure: the state is corrupted, and the process is no longer reliable.

Now in a distributed system composed of Redis instances, and Redis Sentinel instances, it is fundamental that rebooted instances are able to restart with the old data set. Starting with a wiped data set is a byzantine failure, and Redis Sentinel is not able to recover from this problem.

But let’s do a step backward. Actually Redis Sentinel may not be directly involved in an incident like that. The typical example is what happens if a misconfigured master restarts fast enough so that no failure is detected at all by Sentinels.

1. Node A is the master.
2. Node A is restarted, with persistence disabled.
3. Sentinels (may) see that Node A is not reachable… but not enough to reach the configured timeout.
4. Node A is available again, except it restarted with a totally empty data set.
5. All the slave nodes B, C, D, ... will happily synchronize an empty data set form it.

Everything wiped from the master, as per configuration, after all. And everything wiped from the slaves, that are replicating from what is believed to be the current source of truth for the data set.

Let’s remove Sentinel from the equation, which is, point “3” of the above time line, since Sentinel did not acted at all in the example scenario.

This is what you get. You have a Redis master replicating with N slaves. The master is restarted, configured to start with a fresh (empty) data set. Salves replicate again from it (an empty data set).
I think this is not a big news for Redis users, this is how Redis replication works: slaves will always try to be the exact copy of their masters. However let’s consider alternative models.

For example Redis instances could have a Node ID which is persisted in the RDB / AOF file. Every time the node restarts, it loads its Node ID. If the Node ID is wrong, slaves wont replicate from the master at all. Much safer right? Only marginally, actually. The master could have a different misconfiguration, so after a restart, it could reload a data set which is weeks old since snapshotting failed for some reason.
So after a bad restart, we still have the right Node ID, but the dataset is so old to be basically, the same as being wiped more or less, just more subtle do detect.

However at the cost of making things only marginally more secure we now have a system that may be more complex to operate, and slaves that are in danger of not replicating from the master since the ID does not match, because of operational errors similar to disabling persistence, except, a lot less obvious than that.

So, let’s change topic, and see a failure mode where Sentinel is *actually* involved, and that can be improved.

Not all replicas are the same
===

Technically Redis Sentinel offers a very limited set of simple to understand guarantees.

1) All the Sentinels will agree about the configuration as soon as they can communicate. Actually each sub-partition will always agree.
2) Sentinels can’t start a failover without an authorization from the majority of Sentinel processes.
3) Failovers are strictly ordered: if a failover happened later in time, it has a greater configuration “number” (config epoch in Sentinel slang), that will always win over older configurations.
4) Eventually the Redis instances are configured to map with the winning logical configuration (the one with the greater config epoch).

This means that the dataset semantics is “last failover wins”. However the missing information here is, during a failover, what slave is picked to replace the master? This is, all in all, a fundamental property. For example if Redis Sentinel fails by picking a wiped slave (that just restarted with a wrong configuration), *that* is a problem with Sentinel. Sentinel should make sure that, even within the limits of the fact that Redis is an asynchronously replicated system, it will try to make the best interest of the user by picking the best slave around, and refusing to failover at all if there is no viable slave reachable.

This is a place where improvements are possible, and this is what happens today to select a slave when the master is failing:

1) If a slaves was restarted, and never was connected with the master after the restart, performing a successful synchronization (data transfer), it is skipped.
2) If the slave is disconnected from its master for more than 10 times the configured timeout (the time a master should be not reachable for the set of Sentinels to detect a master as failing), it is considered to be non elegible.
3) Out of the remaining slaves, Sentinel picks the one with the best “replication offset”.

The replication offset is a number that Redis master-slave replication uses to take a count of the amount of bytes sent via the replication channel. it is useful in many ways, not just for failovers. For example in partial resynchronizations after a net split, slaves will ask the master, give me data starting from offset X, which is the last byte I received, and so forth.

However this replication number has two issues when used in the context of picking the best slave to promote.

1) It is reset after restarts. This sounds harmless at first, since we want to pick slaves with the higher number, and anyway, after a restart if a slave can’t connect, it is skipped. However it is not harmless at all, read more.
2) It is just a number: it does not imply that a Redis slave replicated from *a given* master.

Also note that when a slave is promoted to master, it inherits the master’s replication offset. So modulo restarts, the number keeps increasing.

Why “1” and/or “2” are suboptimal choices and can be improved?

Imagine this setup. We have nodes A B C D E.
D is the current master, and is partitioned away with E in a minority partition. E still replicates from D, everything is fine from their POV.

However in the majority partition, A B C can exchange messages, and A is elected master.
Later A restarts, resetting its offset. B and C replicate from it, starting again with lower offsets.
After some time A fails, and, at the same time, E rejoins the majority partition.

E has a data set that is less updated compared to the B and C data set, however its replication offset is higher. Not just that, actually E can claim it was recently connected to its master.

To improve upon this is easy. Each Redis instance has a “runid”, an unique ID that changes for each new Redis run. This is useful in partial resynchronizations in order to avoid getting an incremental stream from a wrong master. Slaves should publish what is the last master run id they replicated successful from, and Sentinel failover should make sure to only pick slaves that replicated from the master they are actually failing over.

Once you tight the replication offset to a given runid, what you get is an *absolute* meter of how much updated a slave is. If two slaves are available, and both can claim continuity with the old master, the one with the higher replication offset is guaranteed to be the best pick.

However this also creates availability concerns in all the cases where data is not very important but availability is. For example if when A crashes, only E becomes available, even if it used to replicate from D, it is still better than nothing. I would say that when you need an highly available cache and consistency is not a big issue, to use a Redis cluster ala-memcached (client side consistent hashing among N masters) is the way to go.

Note that even without checking the runid, to make the replication offsets durable after a restart, already improves the behavior considerably. In the above example E would be picked only if when isolated in a minority partition with the slave, received more writes than the other slaves in the majority side.

TLDR: we have to fix this. It is not related to restarting masters without a dataset, but is useful to have a more correct implementation. However this will limit only a class of very-hard-to-trigger issues.

This is in my TODO list for some time now, and congrats to Aphyr for identifying a real implementation issue in a matter of a few tweets exchanged. About the failure reported by Aphyr from the unknown company, I don’t think it is currently viable to try to protect against serious misconfigurations, however it is a clear sign that we need better Sentinel docs which are more incremental compared to the ones we have now, that try to describe how the system works. A wiser approach could be to start with a common sane configuration, and “don’t do” list like, don’t turn persistence off, unless you are ok with wiped instances.
```
