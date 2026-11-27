---
title: Choosing QuePaxa, an unusual consensus algorithm
link: https://blog.cloudflare.com/choosing-quepaxa/
source: blog-cloudflare-com
published: 2026-11-11T14:24:46Z
updated: 2026-11-11T14:24:46Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- João Pedro Leite
- Bob Halley
- James Larisch
labels:
- consensus
- distributed systems
- meerkat
- research
summary: Cloudflare is building an experimental consensus service using a never-before-deployed consensus algorithm, QuePaxa. In this post we explain how QuePaxa works and why we chose it.
content: extracted
html: 2026-11-11-choosing-quepaxa-an-unusual-consensus-algorithm.html
remote_preview:
  url: https://blog.cloudflare.com/_emdash/api/media/file/01M73MMXB6M86NA1F97GH7Q8TG.png
---

Cloudflare [recently announced *Meerkat*](https://blog.cloudflare.com/meerkat-introduction/), an experimental distributed consensus service designed to manage our internal global control plane. It is powered by a new consensus algorithm published by researchers Tennage & Băsescu et al. called [*QuePaxa*](https://dl.acm.org/doi/10.1145/3600006.3613150) (2023).

QuePaxa is an unusual choice; most consensus systems use well-established consensus algorithms like Raft, (Multi-)Paxos or Viewstamped Replication. These algorithms are well understood, have mature implementations, and usually work well.

Unfortunately, common consensus algorithms often suffer in wide-area networks with volatile network connectivity and latency, i.e., networks like the Internet, which is where Meerkat runs. These algorithms rely on a single *leader* process being available and having predictable latency. If the leader goes down, experiences higher latency than usual, or is disconnected, the entire consensus system slows down or becomes unavailable until the leader is replaced. This is unacceptable for Meerkat.

Meerkat’s global deployment environment led us to a newer consensus algorithm, QuePaxa, which promises better stability despite volatility, in large part by not relying on a leader to make progress. In this post we’ll explain why we chose QuePaxa and how the QuePaxa algorithm works.

## What we’re building and why it’s unique

Meerkat is a distributed system in which replicas agree upon an ordered log of events, where each event is decided by a consensus algorithm.

Consensus algorithms allow a group of machines to agree on a single shared truth, even if some of those machines crash or disconnect. Consensus algorithms provide an important guarantee: machines never decide conflicting values. This ensures Meerkat replicas’ logs never diverge.

Meerkat uses its consensus-powered log to expose applications like a key-value store, where each entry in the log is a `get` or a `put`. Because the consensus algorithm guarantees replicas’ logs never diverge, they all construct the same key-value (KV) state. Comparable consensus-powered systems include Consul, etcd, and Apache ZooKeeper.

Unlike similar systems, however, Meerkat was designed for *global scale*. A Meerkat cluster may be distributed across continents and connected by links that often exhibit high and variable latency. This is an unusual setup for consensus systems, which are usually deployed in smaller, more local networks, and have [strict latency requirements](https://developer.hashicorp.com/consul/tutorials/production-vms/reference-architecture#network-latency-and-bandwidth).

## The challenges of consensus at a global scale

While all consensus algorithms ensure that replicas agree on the same value, many of them become disproportionately unavailable or slow when network conditions degrade (but machines do not crash). We have seen this happen firsthand: in 2020, the *partial* [failure](https://blog.cloudflare.com/a-byzantine-failure-in-the-real-world/) of a core Cloudflare network switch caused a consensus-powered system to degrade and the dashboard to operate 80x slower than usual, for six hours. Critically, the system lost no data. But it became unavailable for writes until the issue was resolved, which is unacceptable.

Ideally, a consensus algorithm should be able to make progress despite latency spikes and broken connections, not just replica failures. This is particularly a problem at global scale, where faults are more likely to originate in the network, and not our own servers.

Let’s examine two common traits of consensus algorithms that can lead to degradation under hostile network conditions: destructive interference and fault detection. Later, we’ll see how QuePaxa sidesteps both of these issues.

### Destructive interference and leaders

Consider Paxos, the veritable grandfather of most consensus algorithms. A lot of consensus algorithms inherit the structure of Paxos – both the good and the *not-so-good* bits.

Leaving an egregious amount of detail out, here’s how Paxos works. There are two kinds of processes: proposers and acceptors. Abstractly, the goal of the system is for a majority of acceptors to agree on a value proposed by *some* proposer. There are two phases: the `Prepare` phase, and the `Accept` phase. In the `Prepare` phase, any proposer can send a ballot number out to a majority of acceptors. This ballot number serves as a promise – acceptors will only accept proposals with that ballot number or higher. Then, in the `Accept` phase, the proposer sends its proposal along with the ballot number to a majority of acceptors again. If a majority of acceptors still hold the ballot number of the proposal, the majority accepts it, and the proposer can safely decide the proposal.

![In the Prepare phase, a proposer sends a ballot numbered “1” to three acceptors. The three acceptors reply. Then, in the Accept phase, the proposer broadcasts its proposal to the three acceptors. The acceptors have not received any other ballot, and so they accept the proposal. They reply to the proposer with this information, and the proposer decides.](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M742V6S0NQX62HFS0RFYV0SN.png&w=1172&h=1000&f=webp&fit=cover&position=center)

Challenges arise when two or more proposers submit proposals at the same time. Specifically, another proposer can interleave its own higher ballot number in between the two phases, blocking any lower ballots. Two proposers can repeatedly block each other from progressing – effectively competing for acceptance.

![There are two proposers: one red, and one blue. In the Prepare phase, the blue proposer sends a ballot numbered “1” to three acceptors. The three acceptors reply. Then, the red proposer, in its own Prepare phase, sends a ballot numbered “2” to the three acceptors, before the blue proposer can send its proposal in the Accept phase. Because the red proposer reached the three acceptors with a higher ballot number, the blue proposer’s proposal was rejected. The blue proposer then sends a new ballot numbered “3” to all three acceptors. The blue ballots arrive before the red proposer can send its proposal to the acceptors, and so the acceptors reject the red proposer’s proposal. This then continues forever.](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M742V18ADGMJDHBXQ0KJ249G.png&w=1302&h=1200&f=webp&fit=cover&position=center)

When proposers compete and block each other in this way, we say that they destructively interfere. Destructive interference gets worse as more proposers propose their own ballots. The more concurrent proposers there are, the more likely it is that the system gets stuck repeatedly proposing. Even if none of the replicas have crashed or are unavailable, multiple proposers and misbehaving networks can block Paxos. Formally, we say that Paxos does not guarantee liveness (i.e., that proposals are eventually decided) without assuming that, at some point, all messages arrive within a bounded amount of time — an assumption known as partial synchrony.

To avoid multiple proposers constantly competing, most deployed consensus algorithms, like Raft, Multi-Paxos, and Viewstamped Replication, add a special **leader**proposer. The leader is the only replica that can make proposals. Centralizing the proposing power in one replica forces an order on concurrent requests, avoids competing proposals, and has the added benefit of getting decisions done in an optimal single network round-trip, instead of two. While the leader is healthy and the network is well-behaved, leader-based protocols are fast.

![There is a single leader replica, wearing a crown, that is the only one allowed to send proposals to other servers, called “replicas”. All they do is accept the leader’s proposal.](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M742V8KHK5Y2Q17PJ8FWF5B4.png&w=1200&h=1000&f=webp&fit=cover&position=center)

The trade-off is that the leader must remain operational and reachable. When it fails, becomes too slow, or is disconnected from the cluster, the system stalls. The only solution is for the remaining replicas to detect that the leader has failed, and elect a new one in its place.

### When leader recovery is unstable

When a leader fails, the system must recover by electing a new one. Interestingly, leader recovery is often how destructive interference sneaks right back into a leader-based system.

Consider Raft. For safety reasons, Raft elects a new leader using a majority vote. A replica that suspects that the leader has failed announces itself as a candidate for leadership and requests a vote from other replicas. If more than half of the replicas agree to vote for the candidate, it becomes the new leader. If they don’t, then a new election is called, and the candidates try again.

Multiple replicas can become candidates — all they need to do is detect that the leader is down. The candidates may then all try to acquire a majority vote simultaneously. The problem is that the candidates will *compete* for the majority vote, which means Raft’s elections suffer from the destructive interference we just saw in Paxos: two replicas can mutually prevent each other from getting support from a majority and becoming the leader. The more elections fail, the more replicas will try to get themselves elected leader.

![There is a greyed out leader that has crashed, and four other replicas. Two replicas are candidates for leadership, and are trying to acquire a majority of votes to become leader. They both need three votes, but they each have only acquired two votes (their own and another replica’s). No candidate is elected.](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M742VSBYHFVS6QM6PXYM2HRY.png&w=1114&h=1000&f=webp&fit=cover&position=center)

Even if the leader stops being faulty, elections continue to disrupt what could have been normal system functioning. This is known as a [*metastable failure*](https://brooker.co.za/blog/2021/05/24/metastable.html): a failure pattern that perpetuates itself even when the initial cause of failure is removed. A two-second network outage — or a momentarily faulty network switch — can make the leader *appear* to be faulty and force a leader election, which can fail, and force further elections. The outage will be over in two seconds, but the election can continue for much longer as candidates continue to block each other.

Ideally, consensus algorithms wouldn’t need any recovery process, as they are complicated, prone to interference, and introduce another source of downtime.

### When fault detection goes awry

The problem of destructive interference in leader elections is compounded by trigger-happy fault-detection, especially in wide-area networks. In order for leader-based consensus algorithms to choose a new leader, replicas must first *detect* that the leader has failed. However, if replicas detect leader-failures inaccurately, the system may spend an inordinate amount of time in recovery, not handling requests.

In a distributed system, where machines are separated over a network, fault detection will ultimately depend on expectations. A process cannot directly observe that a remote process has failed; it can only observe that an expected message has or hasn’t arrived. We need a predictable pattern of communication, such as heartbeats or acknowledgements, to tell us that a machine is healthy. If an expected message does not arrive within the expected time, we suspect a fault. The difficulty lies in managing those expectations.

In real networks, especially wide-area networks, messages don’t *always* arrive when predicted. There is no guaranteed upper bound on the time that it takes for messages to arrive. That’s not to say that the network is slow, or that it is unreliable — *most* messages arrive within the time we expect them to. Instead, sudden bursts of latency and losses in connectivity can, momentarily, and without warning, increase the time it takes for any message to traverse the network. We call such networks asynchronous.

The big issue with asynchronous networks is that if some server doesn’t reply in a given amount of time, we don’t really know if the server has failed or if the network is just slow — the two are indistinguishable over the network. Any practical fault detector *has* to make a guess based on how long it has been waiting compared to its expectation, and sometimes that guess will be wrong.

![A user is waiting for a response from a server. The response is taking a while. The user questions whether the network is slow, or if the server crashed.](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M742VAJ48T6ACHV7FGDC7375.png&w=1042&h=1000&f=webp&fit=cover&position=center)

In leader-based algorithms, that guess is exactly what triggers leader elections. Sometimes the leader is down; sometimes its messages are late, or the connection is silently broken. The protocol cannot know which one happened before it reacts. If the guess is wrong, and a replica thinks the leader is faulty when it actually isn’t, then an election begins and the entire system must stop. Until the leader is replaced, no progress is made — and as we just saw, that can take a while.

Fault detection is fundamentally error-prone in asynchronous networks, and when the detector makes a mistake, we get downtime.

### When fault detection is slow

Even when a fault detector guesses correctly, there are issues. Fault detectors react to failures by observing messages on the network, and so their response time has to scale with network latency — the detector’s timeout can’t be below the network latency. This is why Raft is commonly deployed with an election timeout of [five to ten times the network round-trip time.](https://etcd.io/docs/v3.4/tuning/#time-parameters) It has to be high enough to give a chance for the leader to signal that it is alive, and avoid falsely triggering elections. The election timeout *can* be much lower than that, but since leader elections completely stop the system, timeouts are usually set conservatively high.

Roughly speaking, a round trip halfway across the world is measured in hundreds of milliseconds. If an election timeout is several round trips, detecting a failed coordinator can take around a second. Even when fault detection is completely accurate, it is slow, and it is slowest in high-latency global networks. Until the detector reacts, the system is down.

## Why QuePaxa is the right fit

We realized that any algorithm relying on a strict leader or accurate timeouts was inevitably going to stall in a consensus system that runs over the Internet. We searched for consensus algorithms designed for asynchronous environments — protocols that didn't rely on a well-behaved network or precise timing to function. This led us to [QuePaxa](https://dl.acm.org/doi/10.1145/3600006.3613150), published in 2023 by Tennage & Băsescu et al.

The most notable thing about QuePaxa is how it can efficiently reach consensus in a single round trip under favorable network conditions, but also gracefully degrades performance during latency spikes, connectivity issues, and server failures. QuePaxa achieves this by solving the exact problems we just discussed through three key design choices: having an *optional* leader, completely removing the reliance on fault detection, and designing for *constructive interference* between proposers.

In QuePaxa, much like in Paxos, there are two roles: *proposers* and *recorders*. Proposers submit proposals to be decided on, and recorders serve as intermediaries between proposers, recording every proposal they see.

![Two proposers, one blue and the other red, are opposite each other, separated by three servers labeled “Recorder”. Each proposer sends messages to each recorder.](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M742VNQ6JQ4K4V9KPGBX1AHV.png&w=1430&h=624&f=webp&fit=cover&position=center)

Instead of having proposers compete to get their own proposals decided, in QuePaxa, proposers will cooperatively work towards a proposal that everyone supports. QuePaxa does this by assigning a *priority* to proposals, in the form of a random number. The optional leader gets an exclusive maximum priority. During execution, proposers submit their proposals to the recorders, who propagate the proposal to other active proposers. When proposers receive a proposal from a recorder with a higher priority than theirs, they adopt the new proposal as their own, i.e., begin broadcasting the higher priority proposal instead. In general, proposers in QuePaxa work together to decide on the highest priority proposal that they collectively know. This means proposers can’t lock each other out of deciding on a proposal, indefinitely.

![Two proposers, one blue and the other red, are opposite each other, separated by a single recorder. The blue recorder proposes a proposal with priority “2”. The recorder records the blue proposal and replies to the blue recorder, with the same blue proposal. Then, the red proposer sends a proposal with priority “1” to the recorder. The recorder replies to the red proposer with the higher priority blue proposal. The red proposer, upon receiving the reply adopts the blue proposal with priority “2” as its new proposal.](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M742V34J1F8AM3QGMG1AMEKP.png&w=1430&h=290&f=webp&fit=cover&position=center)

The caveat is that it might not be possible to spread proposals around well enough to agree on a proposal, and so proposers have to try again, with each attempt constituting a *round*. However, as more proposers submit proposals, the more likely they are to converge on the highest priority proposal. This is what makes QuePaxa a randomized algorithm: the randomly generated priorities of non-leader proposers are likely — but not guaranteed — to lead to consensus on the *highest* *known* priority, in a round. Yet, given enough rounds, it is guaranteed that proposers agree on a proposal.

One proposer is designated the leader. The leader is the only proposer capable of deciding in a single round trip. The leader always proposes with the highest priority; if the leader knows it spread its proposal before any other proposer, then it can decide immediately. Clients aren’t forced to make requests to the leader — any proposer can propose. If the leader goes down, then clients can keep making requests to other proposers. The leader is **completely optional**. The leader is simply more likely to decide, than any other proposer, and do so faster, in a single round trip.

However, because the leader is the only one that can decide in a single round trip, we want to use it whenever possible. To exploit this *fast path* without creating a single point of failure, clients sending proposals to the system ***hedge*** their bets: they send their requests for proposals to *every* replica, and the non-leader replicas will wait for a small timeout, so the leader can propose and decide on its own. In our own implementation of QuePaxa, we actually deviate from the paper and have clients wait the hedge timeout instead of the replicas, to reduce the amount of messages we send over the network, but the overall result is the same.

This *hedging timeout* is very different from the timeouts used in Raft or Multi-Paxos. It is *not* fault detection, and so isn’t constrained by network latency; it is locally determined by replicas and can be as small as they want it to be. In Raft, having a five-millisecond timeout in a network with 100-millisecond latency would completely freeze the system. But in QuePaxa, a five-millisecond hedge timeout only increases the number of active proposers.

However, hedging isn’t free: the more proposers there are, the more proposals get broadcast, and the more likely it is that replicas fail to decide in a round. Instead, hedging provides a tradeoff: we can react faster to leader changes by lowering the hedge timeout, but we incur extra bandwidth costs and may take more time to decide. That’s a tradeoff we're more than willing to make.

If the QuePaxa leader goes down, we can recover as fast as we want — we’re not bound by network latency. This is how QuePaxa gets around fault detection: since *anyone* can propose, not just the leader, QuePaxa can proactively deal with leader failures by hedging, rather than reacting to failures.

### How QuePaxa works under the hood

To understand how QuePaxa gives us all of these nice properties, let’s look at how QuePaxa actually works.

QuePaxa works in rounds made up of four phases. If a proposal hasn’t been decided on in the last phase, a new *round* is started, and QuePaxa starts over. At the start of each phase, proposers broadcast their currently adopted proposal to all recorders. Once a proposer receives a reply from a majority of recorders, they advance to the next phase. Here’s the general outline of the phases:

- **Phase 0 (Propose and Adopt):** Proposers randomly select a priority, send their proposals to recorders, and adopt the highest-priority proposal they receive from a majority of recorders. If it’s the first round and the leader knows its proposal reached a majority of recorders first, the leader decides immediately.
- **Phase 1 (Spread):** Proposers spread their adopted proposal to a majority of recorders.
- **Phase 2 (Check and Decide):** Proposers ask a majority of recorders for the highest-priority proposal seen in the previous phase. If it matches their own adopted proposal, they decide on it.
- **Phase 3 (Carry Forward):** Any proposer that failed to decide prepares for the next round by adopting the best proposal they see from a majority of recorders.

![A flowchart shows how QuePaxa’s phases progress. The flowchart is vertically aligned. There are four major steps in the flowchart, each one representing a phase in the QuePaxa protocol. The flowchart starts with a block titled “Phase 0 (Propose & Adopt)”, which has two arrows: one to the right, pointing at block labelled “Decide”, via an arrow labelled “Leader Fast Path”, and another arrow labelled “Majority Replies” pointing down at a block titled “Phase 1 (Spread)”. The Phase 1 block has a single arrow labelled “Majority Replies”  pointing below to the next phase block “Phase 2 (Check & Decide)”. The Phase 2 block has an arrow labelled “Proposal has Max Priority” pointing to the right, to a block titled “Decide”, and another arrow labelled “Majority Replies” pointing down to the final Phase block: “Phase 3 (Carry Forward)”. The Phase 3 block has a single arrow labelled “New Round” pointing back up to the Phase 0 block.](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M742V4V70RZ0GWP04ZEPQQAS.png&w=954&h=1000&f=webp&fit=cover&position=center)

Flowchart of QuePaxa’s phase and round progression.

Let’s walk through a couple of example executions and unpack how each phase contributes to the overall behavior we want out of QuePaxa. Each execution will have two proposers (one of which is the leader) and three recorders.

**The Leader Fast Path** **(Phase 0**): The first phase does three important things: it randomizes priorities (which it *must* do to circumvent [the FLP impossibility theorem](https://www.cs.princeton.edu/courses/archive/fall07/cos518/papers/flp.pdf)), checks if the leader reached a majority of support first, and helps proposers converge if the leader didn't. The leader’s proposal is not randomized, and will always have the highest possible priority.

![The image shows a message passing diagram, where time progresses from left to right. There are five lanes. At the leftmost side of the diagram, there are, ordered vertically: a blue server with a crown, labelled “Leader”, with a blue ribbon attached to it, and an infinity sign at the center of the ribbon; three violet servers with a pen and paper, each labelled “Recorder”; a red server (with no crown), with a red ribbon attached to it, and a 1 at the center of the ribbon. The blue leader and the red proposer both broadcast their current proposal to every recorder. The recorders record and reply with the first proposal they receive. The leader reaches the first and second recorder first, while the red proposer reaches the third recorder first. The leader receives a majority (2) of replies each carrying the leader’s proposal. The leader sees that all proposals in the majority are its own and decides. The red proposer receives a majority (2) of the recorder’s replies, from the second and third recorders, who replied with the leader’s proposal and the red proposer’s proposal, respectively. The red proposer adopts the leader’s proposal, since it is the one with maximum priority in the majority it received.](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M742VECT0ZQJRH9PQH874FZH.png&w=1066&h=1200&f=webp&fit=cover&position=center)

In the example above, the leader's proposal reaches a majority of recorders before anyone else, and receives replies confirming it. The leader can safely decide right away. This might seem dangerous, but it is entirely safe thanks to a property called *majority intersection*: any two majorities of recorders always share at least one recorder. Because the leader reached a majority, any other proposer querying a majority will inevitably encounter the leader's proposal and adopt it. Since the leader’s proposal has the highest priority, any other proposer will adopt the leader’s proposal. It is impossible to decide on any other proposal.

![Diagram with three recorders. Two of them have recorded the leader’s proposal, while the other has recorded the red proposer’s proposal. The diagram shows how any groupings of two recorders must share one recorder by adding a semi-transparent Venn diagram on top of the recorders.](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M73MMMP959BZ4SP3780CZ89T.png&w=1264&h=1000&f=webp&fit=cover&position=center)

**Concurrent Slow Path (Phases 0-3)**: When there are concurrent proposers, however, it is not guaranteed that the leader will immediately decide. The leader might fail to reach a majority of recorders first, or it might receive a single reply supporting another proposer. Let's look at what happens when another proposer contests the leader.

**Phase 0:** Pictured below, a second proposer *(shown in red)* reaches the second and third recorders first. The fast path is blocked. Instead of competing, proposers adopt the highest-priority proposal they receive from a majority and move to the second phase. The two proposers *could* both converge to the leader’s proposal in this phase. For illustrative purposes, our example will prevent convergence by delaying the leader’s messages.

![The blue leader proposer and the red proposer both broadcast their current proposal to every recorder. The recorders record and reply with the first proposal they receive. The leader reaches the first recorder first, while the red proposer reaches the second and third recorders first. The leader receives a majority (2) of replies, containing its own maximum priority proposal and the red proposer’s proposal with priority 1. The leader adopts the proposal with the greatest priority which is its own. The red proposer receives a majority (2) of the recorder’s replies, from the second and third recorders, both containing the red proposer’s proposal. The red proposer adopts its own proposal, since it is the one with maximum priority in the majority it received. The reply from the third recorder arrives late, not in the majority, so it isn’t considered for adoption.](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M742VKAZBA67FVNDAVN6JVS2.png&w=988&h=1200&f=webp&fit=cover&position=center)

**Phase 1:** Proposers spread the proposal *they just adopted* in Phase 0 to a majority of recorders. The recorders keep track of the highest-priority proposal they observe. It is a simple but critical step: it prevents the protocol from later deciding on a lower-priority proposal. This is once again thanks to majority intersection: any majority queried in the following phase must include at least one recorder that saw the adopted proposal, in this phase. For illustrative purposes, we prevent convergence in this phase by delaying the leader’s message to the third recorder, so the recorder does not adopt the leader’s proposal.

![The blue leader proposer and the red proposer both broadcast their current proposal to every recorder. The leader’s message to the third recorder is lost and never reaches the recorder. The first and second recorder each receive the leader’s proposal, with maximum priority, and the red proposer’s proposal, with priority 1. The third recorder only receives the red proposer’s proposal. Each recorder records the proposal with highest priority for the next phase. The first and second recorder record the leader’s proposal, while the third recorder records the red proposer’s proposal.](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M742VQM2B2DY4QVWE39FNJ55.png&w=988&h=1200&f=webp&fit=cover&position=center)

**Phase 2:** Now, a decision can happen. Each proposer determines if a higher-priority proposal gained enough support to overtake its own by asking a majority of recorders for the highest-priority proposal observed during *Phase 1*. If the highest proposal returned is still their adopted proposal, they decide. In our diagram, the leader sees its own proposal returned by a majority and safely decides. The other proposer receives conflicting summaries, so it cannot decide yet. Like in Phase 1, recorders keep track of the highest priority proposal they received.

![The blue leader and the red proposer both broadcast their current proposal to every recorder. The first two recorders recorded the leader’s proposal with maximum priority in the previous phase, while the third and final recorder recorded the red proposer’s proposal, with priority 1. Each recorder replies to the proposers with the proposal that the recorder has adopted. The leader receives a majority (2) of replies from the recorders, each carrying its proposal. Of that majority, the proposal with the maximum priority is its own, so it decides it. The red proposer receives a majority (2) of replies from the recorders, one carrying the red proposer’s proposal, and another carrying the leader’s proposal. Of those two, the proposer's own proposal does not have the greater priority, so the red proposer cannot decide, and progresses to the next phase.](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M742VCDQMNM2V6QATKGGGG1P.png&w=1146&h=1200&f=webp&fit=cover&position=center)

**Phase 3:** If a proposer doesn’t decide in Phase 2, it moves to the final cleanup phase to prepare for the next round. Proposers spread their proposals again and adopt the best they find among a majority of recorders. Here, the remaining proposer discovers the decided proposal and adopts it, heading into the next round.

![The red proposer fetches the recorded proposals at each recorder, all of which are the leader proposal with maximum priority. When the proposer receives a majority (2) of the replies, it adopts the proposal with the maximum value, which is the leader’s proposal.](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M742VGRKT5NEFYTQ42KSK6RG.png&w=964&h=1200&f=webp&fit=cover&position=center)

This is deliberate — if there was some decision in the previous phase, then a majority of the recorders will have that proposal, and any remaining proposers *are guaranteed* to adopt it. We see here another instance of cooperation between proposers — even when a round fails, proposers try to converge on the same proposal heading into the next round.

**Moving to the next round:**  After the fourth phase, proposers move on to the next round. At this point, there is no more leader fast path, and the only way to decide is by converging in the third phase. Importantly, due to proposal adoption, it becomes exponentially easier to decide as rounds progress. The QuePaxa paper predicts that each round has a 50% chance of deciding, but in practice, that probability is much higher, thanks to convergence.

Putting all four phases together, we can see where QuePaxa gets its resilience. The first phase gives the leader its one-round-trip fast path. If that fast path fails, random priorities and proposal adoption help proposers converge instead of compete. The second and third phases make it safe to decide, thanks to majority intersection, while the fourth carries what proposers learned into the next round, in case proposers don’t decide.

No proposer has to detect that the leader has failed before making progress, and a failed round doesn’t throw away the work already done. When the network behaves, QuePaxa is fast. When it doesn’t, QuePaxa keeps moving.

## How QuePaxa makes Meerkat possible

In our own evaluations of Meerkat, where we had servers spanning nearly all continents, we’ve directly experienced the impact that QuePaxa has on the overall performance and availability of the system. Meerkat continuously processes requests, even when the leader crashes, disconnects, or just becomes slow. At no point have we ever seen it completely stall. This is exactly what we wanted.

![Two graphs, labelled “RELATIVE DECISION RATE” and “SHARE OF DECISIONS BY PHASE”, both ranging from 0% to 100% on the y-axis, and from 14:58 to 15:09 UTC on the x-axis. The decision-share graph is split among two phases: Phase 0 (1 round trip) and Phase 2 (3 round trips). A red arrow points to a vertical dashed line, aligned at 15:00:32, labelled “leader failure”, and another arrow points to another vertical dashed line at 15:01:25, labelled “leader replaced”. Between those two dashed lines, the relative decision rate falls from 100% to around 65%. During that same time, the share of decisions changes first, from 100% in Phase 0 to around 95%, then drastically falls to 0%, with 100% of decisions happening in Phase 2. After the leader is replaced at 15:01:25, the decision rate climbs back up to 100%, and the decision rate similarly recovers to 100% in Phase 0.](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M73MN2G9VJ180TS373QNGVJJ.png&w=1430&h=822&f=webp&fit=cover&position=center)

QuePaxa falling back to Phase 2 decisions due to a leader failure. The decision rate decreases to around 65% of its original rate, and returns to 100% when the faulty leader is replaced, a minute after failing. At no point did the system halt.

The rate at which Meerkat handles requests has also been generally predictable. Most decisions — upwards of 95% — are made by the leader in the first phase, which keeps our throughput fairly stable. In the instances where a decision isn't reached in the first phase, the throughput drops momentarily, down to at most a third of the usual rate, signaling that three round trips — so three phases — were needed to decide instead of just one. The system slows down, but does not grind to a halt.

QuePaxa also allows us to confidently switch out which replica acts as leader while the cluster is running. Since the leader isn’t the exclusive driver behind decisions and proposers are cooperative, a temporarily non-optimal leader isn’t as limiting as in other algorithms, where a leader misconfiguration could stall the system completely. We can dynamically tune which proposer is the leader without worrying too much about impacting the stability of a Meerkat cluster.

## What we’ll be showing next

Designing Meerkat for a global network forced us to re-evaluate how we approach distributed consensus. Instead of sticking to familiar defaults that didn't quite fit our constraints, we looked to the decades of research produced by academia and industry. That search led us to QuePaxa.

Choosing QuePaxa meant choosing a less-traveled path. It has not been battle-tested as extensively as Raft or Paxos, and we’re also extending the original crash-stop design to support crash *recovery*. This demands careful engineering and stronger evidence of correctness than usual systems.

That is why we are formally verifying our QuePaxa implementation and Meerkat as a whole, alongside extensive simulation and fault-injection testing. Future posts in this series will explain how we verify Meerkat and QuePaxa’s Rust implementation, how we test failures that are difficult to reproduce in production, benchmarks, and what we have learned while turning an academic protocol into a production system.

Stay on the lookout for further Meerkat updates on the Cloudflare Blog, and if you want to learn more about research at Cloudflare, have a look at our work at [Cloudflare Research](https://research.cloudflare.com/).
