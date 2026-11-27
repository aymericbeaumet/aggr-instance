---
title: Designing a reliable unplanned failover
link: https://planetscale.com/blog/designing-a-reliable-unplanned-failover
source: planetscale-com
published: 2026-11-05T00:00:00Z
updated: 2026-11-05T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Shlomi Noach
labels:
- engineering
- neki
summary: How Neki detects a failed Postgres primary, and chooses a new one from the WAL that survived.
content: extracted
html: 2026-11-05-designing-a-reliable-unplanned-failover.html
remote_preview:
  url: https://planetscale.com/assets/designing-a-reliable-unplanned-failover-social-DgSU2lXL.png
---

In [Designing a planned switchover for minimum disruption](https://planetscale.com/blog/designing-a-planned-switchover-for-minimum-disruption), we discussed how Neki moves Postgres primaryship when all the participants are healthy. Admin, Neki's control plane, knows when the primary is going to step aside. It can inspect the shard, select a target, and prepare the handoff before causing any disruption. This is the "planned" in "planned switchover."

An unplanned failover begins with none of those advantages. This happens in circumstances with unexpected (though still well-handled) failure modes, like a network link being cut or server hardware failure.

Neki's Admin system does not know whether the primary has failed or is merely unreachable from where Admin happens to run. It has no promotion target selected, and it cannot ask the primary for a final WAL position. Meanwhile, applications cannot reliably write.

There are two questions to answer in this scenario:

1. Is the primary effectively unavailable?
2. Which replica can safely take over?

## [When is a primary dead?](https://planetscale.com/blog/designing-a-reliable-unplanned-failover#when-is-a-primary-dead)

A Neki shard consists of one Postgres primary and multiple replicas. The primary accepts writes, and each replica continuously receives and replays the primary's WAL. If the primary fails, one of those replicas needs to take over.

Suppose our shard has a primary `P` and replicas `R1`, `R2`, and `R3`. Admin runs a health check against `P` and gets... no response!

You might be asking if Admin should promote a replica. The answer is not yet. `P` may have crashed or its network may be isolated. Or the failure may be only between Admin and `P`, while applications and replicas are all still communicating normally.

A common way to gain confidence is for Admin to repeat the health check with the primary several times. If the primary does not answer after some number of attempts, you can declare it dead. Repetition helps with transient packet loss, but if Admin is on the isolated side of a network partition, this won't help so long as the partition exists.

Other high-availability systems use a lease. An agent on the primary holds a regularly-renewed key in a distributed store. If the key expires, the primary is considered failed. This is a well-established design. While Neki also has a distributed topology service, it does not use leases for primary health determination.

Neki, instead, asks the replicas.

Each replica reports its view of physical replication, taken from `pg_stat_wal_receiver`. We can now distinguish between a few cases:

- If Admin cannot reach `P`, but a replica is still streaming from it, the evidence does not justify failover.
- If Admin cannot reach `P`, at least one reachable replica reports broken replication, and no reachable replica contradicts it, Neki can classify `P` as dead and attempt recovery.
- If Admin cannot reach `P` or any replica, Neki does not fail over. It has neither corroborating evidence nor a visible candidate to promote.

In the last case, Admin waits. Promoting from its failed health check alone would risk creating a second writable primary.

## [Why the replicas' view matters](https://planetscale.com/blog/designing-a-reliable-unplanned-failover#why-the-replicas-view-matters)

Neki's default synchronous durability policy configures Postgres with:

```text
synchronous_standby_names = 'ANY 1 (R1, R2, R3)'
```

With `synchronous_commit = on`, `P` does not acknowledge a commit until at least one eligible replica has durably flushed its WAL. The acknowledging replica can vary from one commit to the next. `R1` may acknowledge a commit at time `T` and `R3` the next one, at `T+1`.

Now consider the case that triggers a failover: Admin cannot reach `P`, and neither can any replica. `P` may have crashed, or it may still be running and is simply unreachable from Admin nodes. Either way, `P` cannot complete a synchronous commit, because no replica can ack. Writes are effectively blocked even if `P` is still alive. Promoting a replica replaces the now-unwritable primary.

When some replicas are also unreachable, their view is unknown: one (say `R1`) might still be connected to `P`. We are thus unable to determine that `P` is not taking writes.

The two could be functioning correctly together, but in isolation from Admin and the other replicas. This all counts as within the bounds of the `ANY 1` contract. A way to mitigate this issue is to configure Neki to only accept replication acknowledgements from cross-availability-zone replicas, reducing the possibility of the two being network isolated together.

Neki still fails over under such conditions, as long as at least one reachable replica reports broken replication and none contradict it.

Neki also supports asynchronous durability. With async, `P` does not wait for replicas to acknowledge writes, so we cannot deduce that it stopped accepting writes. An unreachable primary with every replica disconnected is a very strong signal, but not a guarantee.

## [Detecting failure](https://planetscale.com/blog/designing-a-reliable-unplanned-failover#detecting-failure)

Detection logic and speed vary by failure type. Let's consider some common scenarios.

Say the Postgres process crashes and exits with an error for whatever reason. Neki runs Postgres via PostgresManager, a component that starts, stops, configures, and otherwise controls the Postgres process. A goroutine waits on the Postgres process, and when the process exits on primary `P`, PostgresManager knows immediately. It sends a `PROCESS_EXITED` event to Admin, which marks `P` as unreachable and begins a burst of shard analysis to cross-validate with `P`'s replicas. The detection is still incomplete, waiting for confirmation from the replicas.

Another common scenario is rejected connections. Perhaps a firewall now stands between `P` and the rest of the shard, and actively rejects its traffic. This is straightforward: Admin's health checks and the replicas' replication connections fail right away with an error.

Dropped packets are harder. If `P`'s host is down, or the network silently discards its traffic, nobody gets an error. A connection can look established long after there is anything on the other side.

And a frozen Postgres process is perhaps the hardest of all. The server's kernel keeps answering at the TCP level, so the connection looks healthy even though Postgres is doing no work.

How does a replica tell these cases apart? It doesn't need to. Neki tunes the Postgres replication settings so that each of them, and the many combinations we did not list, ends the same way: the replica reports broken replication.

- Short TCP keepalive settings and `tcp_user_timeout` on the replication connection catch dropped packets.
- `wal_receiver_timeout` catches the frozen process, which TCP alone cannot. If `P` sends nothing for a few seconds, the replica closes the connection and reports replication as broken.

## [Committing to a failover](https://planetscale.com/blog/designing-a-reliable-unplanned-failover#committing-to-a-failover)

Once Neki considers `P` dead, the shard needs a new primary. We must pick a suitable candidate replica, and proceed to promote it while reconfiguring the rest of the replicas to accept it as their new primary. We have two problems, though:

1. `P` may return to being "alive" at any moment.
2. The replicas may have received different amounts of WAL before replication broke.

## [Disconnecting the replicas](https://planetscale.com/blog/designing-a-reliable-unplanned-failover#disconnecting-the-replicas)

Why would we need to disconnect replicas whose replication is already broken? Because `P` may return. If it returns before Admin decides to fail over, there is no harm. The replicas just reconnect, and after a short blip the shard is healthy. The problem is if `P` returns after Admin has decided to fail over. By then, Admin is converging the shard on a new primary, but a broken connection can still reconnect. WAL positions would begin moving again, and `P` would once again have replicas that can acknowledge its commits.

There is a MySQL command that allows an administrator to stop receiving changes from the primary, without stopping replication altogether: `STOP REPLICA IO_THREAD`. Postgres does not offer an equivalent command for physical replication. Instead, Neki makes a small, reversible change to each reachable replica's `primary_conninfo`: it prefixes the hostname of `P` with `//`. The replica can no longer find `P`, and so it cannot reconnect.

This is the same mechanism the [planned switchover](https://planetscale.com/blog/designing-a-planned-switchover-for-minimum-disruption) uses to freeze replica WAL positions before the handoff.

Disconnecting the replicas serves two purposes.

First, it protects the shard from `P` becoming healthy unexpectedly. If `P` comes back, the disconnected replicas will not reconnect to it. With `ANY 1`, `P` cannot complete a commit without a replica to acknowledge it, so it will not be able to accept any writes.

Second, it freezes the replicas' WAL positions. Once no replica can receive more WAL from `P`, each one's reported position is final. The positions are a stable account of the current view of the WAL, which is what we'll use to choose a new primary.

## [Choosing from the WAL position](https://planetscale.com/blog/designing-a-reliable-unplanned-failover#choosing-from-the-wal-position)

In a planned switchover, Admin picks the new primary in advance, while the old primary is still healthy. An unplanned failover has no such luxury, so we must choose from the WAL that survived.

Suppose the replicas stop at these positions:

```text
R1  receive: 120  replay: 120
R2  receive: 125  replay: 122
R3  receive: 123  replay: 123
```

The highest received position is 125, on `R2`. Admin first looks for a promotable replica that has already replayed through 125. There is none in this snapshot, so it waits briefly and checks again. If `R2` replays its locally stored WAL through 125, it becomes a candidate.

If no replica catches up within that window, Admin chooses the promotable replica with the highest `receive_lsn`, using `replay_lsn` as the next criterion.

Admin can take the maximum across replicas because WAL is sequential: a replica that received position 125 also received the WAL before it. Neki does not need to know whether `R1`, `R2`, or `R3` acknowledged each individual commit. It needs a surviving candidate whose WAL covers the acknowledged history.

The latest WAL may also be on a replica we cannot promote. `ANY 1` promises that each acknowledged commit arrived at *some* replica, not that the replica still exists. If `P` and that replica fail together, the commit is lost. Requiring cross-availability-zone acknowledgements makes that much less likely, at the cost of commit latency.

Admin creates replication slots on the new primary for the other replicas and promotes it in Postgres. It then marks it as the shard's primary, and Neki's Routers start sending writes its way. Last, Admin repoints the other replicas to the new primary.

Choosing the highest received position pays off a second time here. All other replicas hold a prefix of the new primary's WAL, so they can simply follow it onto the new timeline. The exception is a replica that was ahead of the candidate but could not be promoted, for example while under maintenance. Neki must rewind or rebuild it before it can follow.

Writes do not resume quite yet. The new primary runs with `ANY 1`, so its first commits wait until a replica attaches and acknowledges them.

## [What about `P`?](https://planetscale.com/blog/designing-a-reliable-unplanned-failover#what-about-p)

Admin marks `P` as `TEARDOWN`. A `TEARDOWN` node serves no traffic, takes no part in replication, and Admin stops monitoring it. As far as the shard is concerned, `P` is gone, even if it comes back to life.

After a planned switchover, Admin turns `P` into a replica of the new primary. After an unplanned failover, we do not. `P` may have writes that don't exist on the new primary, and we prefer not to trust it just because it became reachable again.

That does not mean we must get rid of it right away. A failed primary is often the best evidence of what went wrong: its logs, its last WAL, the state of its disk. We may want to keep it around for a postmortem. Only then do we destroy it, recycle it, or, by explicit decision, rejoin it to the shard as a replica.

## [Summary](https://planetscale.com/blog/designing-a-reliable-unplanned-failover#summary)

An unplanned failover starts with uncertainty: Admin does not know whether `P` is dead, or only out of its sight. Neki does not act on Admin's view alone. It asks the replicas, and fails over only when those it can reach agree that `P` is gone.

From there, we assume the worst about `P`: it may come back at any moment. Disconnecting the replicas keeps a returning `P` from regaining them, and freezes their WAL. Choosing the highest WAL position for the replacement keeps every acknowledged commit that a promotable replica holds, and lets the other replicas follow easily.

Some outages are beyond automatic recovery. Admin cannot promote a replica it cannot contact, nor recover WAL that disappeared with every machine that held it.

Admin itself can fail, too, but that is a topic for a future post.
