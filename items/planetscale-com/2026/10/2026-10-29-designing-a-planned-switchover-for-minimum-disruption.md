---
title: Designing a planned switchover for minimum disruption
link: https://planetscale.com/blog/designing-a-planned-switchover-for-minimum-disruption
source: planetscale-com
published: 2026-10-29T00:00:00Z
updated: 2026-10-29T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Shlomi Noach
labels:
- engineering
- neki
summary: How Neki uses preflight checks, query buffering, synchronous replication, and fast rollback to make Postgres primary switchovers less disruptive.
content: extracted
html: 2026-10-29-designing-a-planned-switchover-for-minimum-disruption.html
remote_preview:
  url: https://planetscale.com/assets/designing-a-planned-switchover-for-minimum-disruption-social-DtLvDiix.png
---

A [Neki](https://planetscale.com/neki) shard consists of one Postgres primary and multiple replicas. The primary accepts writes; the replicas do not. From time to time, we need one of those replicas to take over as the primary. We may be replacing a (Kubernetes) pod, upgrading the kernel, or moving the primary to another cell.

This is what we call a *planned switchover*. As opposed to an *unplanned failover*, which happens when there is unexpected server disruption, it is a routine operation that comes at an unavoidable cost: there is some period of time where Neki cannot reliably route writes to either the old or the new primary.

In this post we discuss how Neki minimizes that period, how it keeps applications from experiencing errors, and what happens if the switchover cannot complete.

## [The three parts of a switchover](https://planetscale.com/blog/designing-a-planned-switchover-for-minimum-disruption#the-three-parts-of-a-switchover)

We can break down a planned switchover into three parts:

1. **Preflight validation.** Admin, Neki's control plane, checks the state of the shard and determines whether there is a viable replica to promote.
2. **Convergence and promotion.** One replica is brought fully up to date and promoted.
3. **Client recovery.** Queries that would have gone to the old primary must find their way to the new one.

The first part takes place while the primary is still serving normally. The other two make up the actual disruption period.

## [Planned switchover does not need failure detection](https://planetscale.com/blog/designing-a-planned-switchover-for-minimum-disruption#planned-switchover-does-not-need-failure-detection)

An unplanned failover first needs to establish that the primary has failed. A primary that does not answer may be dead, or it may be inaccessible due to a network partition. Promoting a replica while the old primary is still writable can result in two writable primaries.

Failure detection therefore takes time. This is necessary time in an unplanned failover, because the system needs to understand what happened before it makes changes to cluster topology.

In a planned switchover, however, we already know the primary is going to step aside. Admin can use the time before that happens to check the shard and the potential promotion candidates. If any check fails, the switchover is rejected and the primary remains unchanged.

## [Establishing a final WAL position](https://planetscale.com/blog/designing-a-planned-switchover-for-minimum-disruption#establishing-a-final-wal-position)

To safely promote a replica, we need to know it has received and applied all the [WAL](https://planetscale.com/blog/postgres-backups-under-the-hood#the-wal) we expect it to have. But the primary is still running and can keep generating WAL while we're doing this! We can sample the primary's position and wait for a replica to reach it, only to find that the primary has since moved ahead.

Let's consider a simplified shard. We have primary `P` and replicas `R1`, `R2`, and `R3`. Assume Admin chooses `R2` as the promotion target.

The obvious way to stop the WAL position from moving is to stop Postgres on the primary. Patroni, a popular open-source Postgres cluster management tool, takes this approach. This is a well-considered design. A clean shutdown produces an unambiguous final WAL position, with no WAL creep left to account for. Patroni can validate a candidate named by the operator, or let the eligible replicas choose through its distributed leader election. The flow is simpler, with less state to reason about, and is proven at large scale.

However, stopping Postgres is a problem for HA. If something later prevents the replica from taking over, returning to the old primary requires starting Postgres again and may also require rewinding it. Until that completes, there is no primary to take writes. We wanted to keep the old primary available for as long as it is safe to do so, so that a failed switchover can return to it quickly.

Neki does things differently. It first asks the [Routers](https://planetscale.com/blog/what-is-a-neki-router) to buffer queries that would go to the primary. Admin then changes the default for new transactions on the primary to read-only.

Neither of these actions prevents all future writes, however. Existing transactions can still commit, sessions can override the read-only default, and background processes can continue to generate WAL.

The switchover is designed with that in mind. Admin:

1. Samples `P`'s WAL position and the replication state of its replicas.
2. Selects `R2`, which has flushed through the sampled position.
3. Disconnects `R1` and `R3` from `P` and records the final `receive_lsn` of each.
4. Takes the higher of the sampled primary position and the highest recorded `receive_lsn`.
5. Waits for `R2` to replay through that position.

Once `R1` and `R3` are disconnected, they cannot receive any more WAL. We now have a final WAL position, and must wait for `R2` to catch up to it.

## [Preserving synchronous replication](https://planetscale.com/blog/designing-a-planned-switchover-for-minimum-disruption#preserving-synchronous-replication)

Neki configures synchronous replication as `ANY 1 (...)`, listing all eligible replicas. We don't want one slow or unavailable replica to block the primary while another can acknowledge.

But which replica acknowledged a given commit? Suppose `R1` acknowledged the latest commit, while Admin intends to promote `R2`. First, `R2` must have flushed through the sampled primary position. Next, the final positions of the disconnected replicas become part of the position `R2` must replay. Once the others are disconnected, `R2` is the only synchronous standby still streaming, and no further commit can be acknowledged unless `R2` has flushed it.

This replication must also work after promotion. Admin prepares `R2`'s replication slots and synchronous configuration, then attempts to attach the other replicas.

After this happens, at least one attached replica must appear in the configuration. Otherwise, `R2` could become primary and immediately block waiting for an acknowledgement that no connected replica can provide. In that case, Admin rolls back instead of promoting it.

What about `P`? Once `R2` is promoted, Admin turns `P` into a replica of `R2`, through the same flow that creates any other replica. After it restarts as a replica, `P` participates in synchronous acknowledgements.

## [What does the application see?](https://planetscale.com/blog/designing-a-planned-switchover-for-minimum-disruption#what-does-the-application-see)

Neki Routers watch for the buffering signal published by Admin. During this period, a query that would go to the shard's primary is held by the Router instead of being sent to Postgres. Reads served by replicas are unaffected.

When the switchover completes, Admin removes the signal and Routers replan each buffered query using the new topology and sends it to the new primary. From the application's point of view, the query took longer to complete, but did not fail because the primary changed.

This is proactive buffering. In the case of an unplanned failure, buffering can only begin after the first query discovers the primary is unavailable. With a planned switchover, we can begin buffering before making any change to the primary.

There are a few limitations:

- Admin does not wait for every Router to acknowledge the buffer signal. Buffering reduces disruption, but is not required for correctness.
- The buffer has finite capacity, and each query has a deadline. Queries fail if the buffer is full or the switchover takes too long.

## [Validating before acting](https://planetscale.com/blog/designing-a-planned-switchover-for-minimum-disruption#validating-before-acting)

A planned switchover should not begin unless Admin has good reason to believe it can complete. Neki prefers to reject the operation while the primary is still serving normally, rather than discover an avoidable problem after making it read-only.

Before changing the primary, Admin validates:

- **The shard state.** Only explicitly accepted states may proceed. Replication lag, divergent timelines, and other bad cluster states prevent the switchover from starting.
- **An operator-selected target.** If the operator names a target, Admin still verifies that it is a viable promotion candidate.
- **Logical failover slots.** If the shard uses logical failover slots, Admin waits for them to copy over to the promotion candidate.
- **Postgres capacity.** The candidate's settings must be compatible with all replicas. A replica cannot follow a primary with larger values for settings such as `max_connections` and `max_worker_processes`.

Only after these checks pass does Admin initiate a planned switchover.

Some checks necessarily take place after that point:

- The target must have flushed through the sampled primary position and replayed through the final position.
- The target must have all replication slots it will need, including failover slots created or changed since preflight.
- At least one replica must be connected to the target and included in the synchronous configuration the target applied.

Admin reverts to the old primary if any of these fail. Only after they all pass does Admin attempt to promote the target.

## [What if the target does not catch up?](https://planetscale.com/blog/designing-a-planned-switchover-for-minimum-disruption#what-if-the-target-does-not-catch-up)

Suppose `R2` passes all preflight checks but does not replay through the final WAL position. A rare scenario, but technically possible. Admin cannot wait forever as by this point `P` is read-only. The switchover therefore has a deadline.

At this point `P` is still running, while `R1` and `R3` are temporarily disconnected from the cluster.

When a rollback due to a switchover timeout is needed, Admin can make `P` writable again with a configuration reload and reconnect `R1` and `R3`. This immediately restores service.

## [Nothing acknowledged is lost](https://planetscale.com/blog/designing-a-planned-switchover-for-minimum-disruption#nothing-acknowledged-is-lost)

The resulting guarantee of all this: under Neki's synchronous durability policy and the default `synchronous_commit = on`, every commit Postgres acknowledges is present on the target by the time it is promoted.

## [Summary](https://planetscale.com/blog/designing-a-planned-switchover-for-minimum-disruption#summary)

Planned switchover gives Neki an opportunity to prepare before it causes any disruption. We have crafted the process of to be as seamless as possible, between Admin's handling of the various phases, and the Router's query buffering capabilities. Planned switchovers are the foundation for shard resizes, version upgrades, and other parts of the Postgres cluster management lifecycle. Getting them right is a key part to why Neki is the best way to operate highly-available Postgres databases.

[Try out Neki today.](https://app.planetscale.com)
