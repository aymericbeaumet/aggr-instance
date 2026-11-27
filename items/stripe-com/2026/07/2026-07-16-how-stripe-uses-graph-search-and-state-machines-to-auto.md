---
title: How Stripe uses graph search and state machines to auto-remediate a global database fleet
link: https://stripe.dev/blog/how-stripe-uses-graph-search-and-state-machines-to-auto-remediate-a-global-database-fleet
source: stripe-com
published: 2026-07-16T00:00:00Z
updated: 2026-07-16T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Pragya Mehta
summary: Discover how we modeled our MongoDB infrastructure as a traversable graph and then use pathfinding algorithms to dynamically compute and execute recovery plans. This automated approach reduced pager volume by 30% (~200 pages/year), eliminated 12 days of unhealthy shard states annually, and supports new shard layouts with zero manual effort.
content: extracted
html: 2026-07-16-how-stripe-uses-graph-search-and-state-machines-to-auto.html
remote_preview:
  url: https://stripe.dev/og/blog/how-stripe-uses-graph-search-and-state-machines-to-auto-remediate-a-global-database-fleet.png
  alt: 'Stripe Developer Blog: How Stripe uses graph search and state machines to auto-remediate a global database fleet'
extra:
  content:interactive: true
---

At 2 a.m. on a Tuesday, an on-call engineer's pager fires. A MongoDB shard—one of 2,000 that collectively processed $1.9 trillion in payments in 2025—has entered a degraded state. Two nodes are down, and a third is blocking an index build due to a misconfigured vote. To nurse the shard back to health, the engineer will need to spend 15 minutes carefully sequencing manual operations, then up to 3 hours triggering and monitoring each operation.

This scenario played out hundreds of times a year across Stripe's Document Database fleet—our global MongoDB infrastructure spanning over 40 distinct shard layouts, from cost-optimized configurations to high-availability setups designed for disaster recovery. The combinatorial explosion of failure modes made it impossible to anticipate every scenario with static, hand-coded automation. We needed a fundamentally different approach.

Our solution treats infrastructure state as a traversable graph and lets a pathfinding algorithm discover recovery sequences at runtime. By applying breadth-first search, and later, Dijkstra's algorithm, over a simulated state machine, we built a system that dynamically computes and executes multi-step recovery plans. As a result, we’ve seen a 30% reduction in pager volume (~200 fewer pages per year), 12 fewer days of shards stuck in unhealthy states annually, and zero additional work required to support new shard layouts.

## Why static automation breaks at scale

Our original auto-remediation system (V1) operated as a single-step, hard-coded state machine. A set of plugins—each responsible for one issue, like "fix votes," "fix priority," or "rebuild downed node"—ran in a carefully ordered sequence based on hard-coded ranking and assumptions within a reconciliation loop.

This worked well for simple cases, like if a single node went down, or one or two misconfigured votes, but it had fundamental limitations:

- Implicit dependencies between plugins meant that ordering was fragile. The "fix votes" and "fix priorities" plugins required no nodes to be down, but they had higher priority than the "node down" plugin. We could only resolve these circular dependencies with manual intervention.

- Multi-failure scenarios exposed blind spots. When multiple impaired nodes combined with voting and priority misconfigurations, the sequential, single-issue-at-a-time approach couldn't find a valid ordering. The system would exhaust its options and page an operator.

- Layout coupling was everywhere. Each plugin contained layout-specific safety checks, so onboarding a new layout to the V1 remediation platform required auditing the logic of every plugin and workflow, a process that took about 1 week for each new layout.

- Workflow cancellations left shards stranded. A canceled workflow could leave the shard in an intermediate state that V1 never anticipated. These orphaned states fell outside any plugin's preconditions and required manual cleanup.

In one six-month window, the control plane paged operators 124 times for misconfigured shards, and 32 times for single-node-down scenarios complicated by additional health issues. Critical operations like index builds and planned maintenance were blocked for an average of one hour per incident.

## Infrastructure as a traversable graph

### Reframing the question

The breakthrough came when we stopped asking, "What's the most important issue to fix right now?", which is a local, greedy question. Instead, we asked, "What sequence of operations transforms the current shard state into a healthy state without ever violating safety invariants?" Once we framed it that way, we realized we were looking at a graph search problem.

![](https://stripe.dev/images/how-stripe-uses-graph-search-and-state-machines-to-auto-remediate-a-global-database-fleet/image1.gif)

### Modeling a shard’s health as a state machine

In MongoDB, cluster topology is controlled by assigning nodes specific roles: voting rights determine participation in leader elections and quorum writes, priority levels control a node's eligibility to become the primary, and hidden status keeps a node out of the client read path. We capture these per-node attributes, along with shard-level properties like oplog sizes, regional distribution, and quorum status, to model a shard’s state as a node in a directed graph.

Edges in the graph represent atomic infrastructure operations: adding or removing a vote or priority, hiding or unhiding a node, rebuilding a node, or resizing an oplog. These are steps we need to take on the path to getting the shard healthy again.

### Finding the path with breadth-first search over simulated states

The initial state is the shard's current ground reality. The goal state, the desired, healthy configuration, is the shard's Source of Truth (SOT). Invalid states violate safety invariants. These could be an even number of votes, a minority of healthy voting nodes, which puts write availability at risk, or a node in an invalid state, like priority=1 and vote=0.

With the state space defined, we use breadth-first search (BFS) to find the shortest sequence of operations from the current state to the goal state. The algorithm generates successor states at each step, prunes any that are invalid, and returns the first path that reaches the goal. Since BFS traverses level-by-level, it guarantees that the first solution uses the minimum number of operations.

Consider a healthy shard where three things go wrong simultaneously: a data node goes down, the backup node goes down, and another data node loses its vote and priority. V1 is completely stuck—the vote-fix plugin requires no nodes to be down, and the node-rebuild plugin requires votes to be correct. Our state machine, however, explores all valid orderings:

```
Current State → [Rebuild backup] → [Fix vote+priority] → [Rebuild data node] → Healthy
Current State → [Fix vote+priority] → [Rebuild backup] → [Rebuild data node] → Healthy
Current State → [Fix vote+priority] → [Rebuild data node] → [Rebuild backup] → Healthy
```

Each path is validated against invariants at every intermediate step. Fixing the vote/priority issue first is safe since it maintains quorum, which then unlocks subsequent operations.

### In-memory simulation and durable execution with Temporal

We orchestrate our remediation workflows with [Temporal](https://temporal.io/), a durable execution framework that guarantees a workflow interrupted by a deploy, crash, or timeout can resume exactly where it left off—essential for node rebuilds that can take hours.

But Temporal's durability model creates a challenge for pathfinding. Our remediation building blocks are implemented as Temporal workflows and activities that are tightly coupled to Temporal's workflow.Context. They make network calls, wait for infrastructure changes, and checkpoint progress. We can't call these functions in a loop to simulate thousands of state transitions. A single simulated path through a five-step recovery would take minutes of real I/O; exploring hundreds of candidate paths would be intractable.

We solved this by introducing an abstraction layer: a CommonContext interface that can be backed by either Temporal's real workflow context, or a lightweight in-memory implementation.

```
// CommonContext abstracts Temporal's workflow.Context, enabling both
// production execution and local simulation.
type CommonContext interface {
        ExecuteActivity(opts workflow.ActivityOptions, resultPtr any, activity any, args ...any) error

        ExecuteLocalActivity(opts workflow.LocalActivityOptions, resultPtr any, activity any, args ...any) error

        ExecuteWorkflow(opts workflow.ChildWorkflowOptions, resultPtr any, workflow any, args ...any) error

        WithExternalDeps(sotProvider, grProvider SerializedShardProviderData, simState *SMShardState) (CommonContext, error)
}
```

The interface wraps Temporal's core primitives so that remediation logic can call them without knowing whether it's running inside a real workflow or a local simulation. The key method is WithExternalDeps, which swaps in simulated SOT and Ground Reality providers along with an in-memory shard state, allowing the system to explore thousands of state transitions without touching real infrastructure. In production, CommonContext delegates to Temporal and makes real API calls. In simulation, it delegates to an in-memory state provider that applies operations as pure state transformations. Both modes run the same business logic.

This gives us a critical property: what you simulate is what you execute. There’s no separate planning codebase that could diverge from the execution codebase. When the simulator determines that a sequence of operations will heal a shard, we have very high confidence that executing those same operations in production will produce the same result.

The simulation approach transforms minutes of I/O-bound execution into just milliseconds of CPU-bound computation. The BFS pathfinder can explore hundreds of state transitions per second, evaluating complete multi-step recovery paths in the time it would take to make a single network call.

## Evolving the algorithm: From BFS to Dijkstra

BFS finds the optimal path to a fully healthy state, but it fails entirely if no complete path exists. Consider a shard with an extra node that automation can’t reach, alongside another node that has a missing vote. BFS returns no path at all, because the full goal state is unreachable—even though it’s trivial to fix the missing vote, and leaving it unfixed puts the shard at elevated risk of losing write quorum.

When our algorithm couldn’t reach a perfectly healthy state, could it find the least degraded state instead?

### Weighted graph search

We evolved the algorithm to use Dijkstra's shortest-path algorithm over a weighted graph. The edge weight for each operation accounts for misconfiguration and estimated time:

```
edgeWeight(state, operation) = misconfiguration(state) × estimatedTime(operation)
```

misconfiguration(state) quantifies how far the current state deviates from healthy, using a rules-based scoring system that accounts for missing votes, incorrect priorities, downed nodes, and other issues. estimatedTime(operation) estimates wall-clock duration–a vote change takes seconds, while a node rebuild takes an hour. Using the product of these two for edge weight means the algorithm minimizes time spent in misconfigured states, prioritizing quick fixes that reduce exposure before committing to longer operations.

![](https://stripe.dev/images/how-stripe-uses-graph-search-and-state-machines-to-auto-remediate-a-global-database-fleet/image2.png)

### Partial remediation

Because Dijkstra's algorithm explores paths to all reachable states rather than only the goal state, we can also get partial remediation. When no complete path exists, the algorithm returns the path to the least misconfigured state. The ephemeral-node example resolves itself: Dijkstra fixes the vote, reduces the shard's risk, and leaves the ephemeral node for operator attention.

![](https://stripe.dev/images/how-stripe-uses-graph-search-and-state-machines-to-auto-remediate-a-global-database-fleet/image3.png)

### Extensibility through composable rules

The misconfiguration scoring is designed as a set of composable rules. The core function curries the SOT and returns a closure that can rapidly score any candidate state during graph traversal:

```
type MisconfigRule func(*ar.SMShardState, *ar.SMShardState) int

func GetMisconfigFunc(misconfigRules map[string]MisconfigRule, sot *ar.SMShardState) func(current *ar.SMShardState) int {
    return func(current *ar.SMShardState) int {
        misconfig := 0

        for _, rule := range misconfigRules {
            misconfig += rule(current, sot)
        }
  
        return misconfig
    }
}
```

Each rule targets a specific class of misconfiguration—downed nodes, voting mismatches, priority mismatches, oplog sizing, hidden status, and availability zone imbalance—and we can add new rules independently as our understanding of severity evolves:

```
func ListAllMisconfigRules() map[string]MisconfigRule {
      return map[string]MisconfigRule{
          "DownNodeMisconfigRule":     DownNodeMisconfigRule,
          "VotingMisconfigRule":       VotingMisconfigRule,
          "PriorityMisconfigRule":     PriorityMisconfigRule,
          "OplogResizeMisconfigRule":  OplogResizeMisconfigRule,
          "HiddenStatusMisconfigRule": HiddenStatusMisconfigRule,
          "AZMisconfigRule":           AZMisconfigRule,
      }
  }
```

Operation costs are expressed as relative weights that reflect how dramatically operations differ in duration—a node rebuild takes roughly 33 times longer than a config change:

```
const (

      OPTIME_NODE_UPDATE_MONGO_CONFIG = 3
      OPTIME_SHARD_UPDATE_OPLOG       = 19
      OPTIME_NODE_REBUILD             = 101

  )
```

The named rule map also makes debugging straightforward. The logs for a generated path show exactly how each rule contributed to the misconfiguration score at every intermediate state.

## Impact

The state machine auto-remediation system has fundamentally changed our operational baseline. We’ve seen a 30% reduction in pager volume, or ~200 fewer pages a year, along with 12 days of fleet health reclaimed annually from shards that used to sit in degraded states until someone noticed them. Critical operations like index builds and planned maintenance now proceed without getting bottlenecked by manual stabilization.

The system is also layout-independent: when we onboarded a new layout for a planned failover, auto-remediation worked with zero code changes. And because workflows can be canceled mid-execution, the system can compute a fresh recovery path from whatever intermediate state was left behind.

Beyond metrics, the system has changed how we think about failure. Instead of writing runbooks for anticipated scenarios, we define invariants and available operations, and let the algorithm compose them into recovery plans.

## Future directions

Right now, the state machine answers one question: "How do I get this shard back to its declared healthy state?" But the framework is extensible.

The most immediate extension is layout transformation. Our database fleet has over 40 shard layouts, and migrating a shard from one topology to another currently means writing a bespoke workflow that encodes every intermediate step. Instead, we can set the goal state and let the algorithm discover a safe migration sequence the same way it discovers a recovery sequence. We can use simple config updates for fleet-wide topology changes rather than needing to write bespoke migration workflows.

We can also adapt the state model to handle blue-green migrations, where a shard temporarily runs two "colors" of nodes during a version rollout. The pathfinder must be topology-aware, understanding which color is active and what "healthy" means at each phase of the migration. We want to be able to overlay migration context onto the shard's SOT, allowing the same Dijkstra-based planner to rebalance votes, rebuild failed nodes with the correct version binary, and resize oplogs. Then we can safely automate rollbacks when health signals degrade after a traffic shift.

Further out, we want the system to orchestrate planned maintenance alongside reactive healing. Right now, we execute one operation at a time on a single node across the entire shard, but when a node is already offline for a version upgrade, we could batch an oplog resize or index build into the same window. The system could discover these batching opportunities by modeling "node offline for maintenance" as a state where multiple operations become safe to execute together.

## Conclusion

If you can model your infrastructure's state space and define valid transitions, recovery becomes a search problem. The engineering challenge is making that search practical—building a simulation layer that’s fast enough for exhaustive exploration, ensuring simulation fidelity matches production execution, and maintaining safety invariants that prevent the algorithm from proposing dangerous intermediate states.

For teams managing complex distributed infrastructure, this pattern of state machine modeling, simulation-based planning, and runtime pathfinding offers a compelling alternative to accumulating ever-more-specific runbooks. Runbooks encode known recovery procedures; a state machine discovers novel ones.

Special thanks to Aarush Gupta, Jessica Glustien, Neeraj Joshi, Puneet Oberai, Runchen Yan, Shalin Shekhar Mangar and Srivatsan Sridharan for their contributions to this project.

If building systems like this interests you, [we're hiring](https://stripe.com/jobs/search?query=infrastructure).
