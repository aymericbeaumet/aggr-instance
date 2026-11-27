---
title: 'SFQ: Simple, Stateless, Stochastic Fairness'
link: http://brooker.co.za/blog/2026/02/25/sfq.html
source: brooker-co-za
published: 2026-02-25T00:00:00Z
updated: 2026-02-25T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Marc Brooker
summary: 'SFQ: Simple, Stateless, Stochastic Fairness Roll the dice. Paul E. McKenney’s 1990 paper Stochastic Fairness Queuing contains one of my favorite little algorithms for distributed systems. Stochastic Fairness Queuing is a way to stochastically isolate workloads from different customers in a way that significantly mitigates the effects of noisy neighbors, with O(1) queues and O(1) time. McKenney starts by describing Fairness Queuing (or queue per client): This fairness-queuing algorithm operates by maintaining a separate first-come-first-served (FCFS) queue for each conversation. … Since the queues are serviced in a bit-by-bit round-robin fashion ill-behaved conversations that attempt to use more than their fair share of network resources will face longer delays and larger packet-loss rates than well-behaved conversations that remain within their fair share. That’s a network packet focused view, but the same thing can apply to RPC requests, for example, just by using a different key (e.g. the authorized customer id). The big downside of this in distributed systems is that it requires O(customers) queues, and the related O(customers) work of doing round-robin across those queues. Stochastic fairness queuing can be most easily understood by comparing it to strict fairness queuing. The major differences are that the queues are serviced in strict round-robin order and that a simple hash function is used to map from source-destination address pair into a fixed set of queues. In SFQ, on the other hand, a fixed set of queues is used (so O(1) queues, not O(customers) queues), and customers are assigned to the queues based on a hash. That’s great, but still causes the problem of long-term bad luck. If I end up on a queue with a noisy neighbor, I end up there forever. If two conversations collide, they will continue to collide, resulting in each conversation of the pair persistently receiving less than its share of bandwidth. This situation is deviated by periodically perturbing the hash function … so that conversations that collide during one time period are very unlikely to collide during the next. So noisy neighbors and non-noisy clients move around, preventing somebody from getting bad service for too long. You can use this on a single host for servicing multiple clients, or for load balancing across multiple hosts. SFQ, Shuffle Sharding, and Best-of-2 We can avoid even those periodic times of bad service by combining three of my favorite small algorithms: SFQ, shuffle sharding, and best of two. In this variant we: Map each customer to a subset of the queues (that subset could be only two), and For each request put the customer’s request in the shortest queue in their subset, and Periodically perturb the subsets. This approach has absolutely great properties: O(1) queues, O(1) enqueue effort, O(1) dequeue effort, and strong isolation of noisy neighbors from other customers (assuming only a relatively small portion of customers are noisy neighbors). Single Hash Two-Choice Hash (Power of Two) Pause Speed: Normal Perturb Hash Reset Customers: Noisy (3x)'
content: extracted
html: 2026-02-25-sfq-simple-stateless-stochastic-fairness.html
remote_preview:
  url: https://brooker.co.za/blog/images/feed-icon-14x14.png
extra:
  content:interactive: true
---

## About Me

My name is Marc Brooker. I like to build things that work, and do cool stuff. I like building big things. I also dabble in machining, welding, cooking, and skiing.

I am an engineer at Amazon Web Services (AWS) in Seattle, where I work on agentic AI, especially safety and policy for agentic AI. Before that, I worked on EC2, EBS, databases, serverless, and serverless databases.\
 All opinions are my own.

## Links

[My Publications and Videos](https://brooker.co.za/blog/publications.html)\
 [@marcbrooker on Mastodon](https://fediscience.org/@marcbrooker) [@MarcJBrooker on Twitter](https://twitter.com/MarcJBrooker)

[Is this blog written by AI?](https://brooker.co.za/blog/2026/06/18/my-blog-and-ai.html)

Roll the dice.

Paul E. McKenney’s 1990 paper [Stochastic Fairness Queuing](https://courses.cs.duke.edu/spring09/cps214/papers/sfq.pdf) contains one of my favorite little algorithms for distributed systems. Stochastic Fairness Queuing is a way to stochastically isolate workloads from different customers in a way that significantly mitigates the effects of noisy neighbors, with O(1) queues and O(1) time.

McKenney starts by describing Fairness Queuing (or *queue per client*):

> This fairness-queuing algorithm operates by maintaining a separate first-come-first-served (FCFS) queue for each conversation. … Since the queues are serviced in a bit-by-bit round-robin fashion ill-behaved conversations that attempt to use more than their fair share of network resources will face longer delays and larger packet-loss rates than well-behaved conversations that remain within their fair share.

That’s a network packet focused view, but the same thing can apply to RPC requests, for example, just by using a different key (e.g. the authorized customer id). The big downside of this in distributed systems is that it requires O(customers) queues, and the related O(customers) work of doing round-robin across those queues.

> Stochastic fairness queuing can be most easily understood by comparing it to strict fairness queuing. The major differences are that the queues are serviced in strict round-robin order and that a simple hash function is used to map from source-destination address pair into a fixed set of queues.

In SFQ, on the other hand, a fixed set of queues is used (so O(1) queues, not O(customers) queues), and customers are assigned to the queues based on a hash. That’s great, but still causes the problem of long-term bad luck. If I end up on a queue with a noisy neighbor, I end up there forever.

> If two conversations collide, they will continue to collide, resulting in each conversation of the pair persistently receiving less than its share of bandwidth. This situation is deviated by periodically perturbing the hash function … so that conversations that collide during one time period are very unlikely to collide during the next.

So noisy neighbors and non-noisy clients move around, preventing somebody from getting bad service for too long. You can use this on a single host for servicing multiple clients, or for load balancing across multiple hosts.

*SFQ, Shuffle Sharding, and Best-of-2*

We can avoid even those periodic times of bad service by combining three of my favorite small algorithms: SFQ, [shuffle sharding](https://aws.amazon.com/blogs/architecture/shuffle-sharding-massive-and-magical-fault-isolation/), and [best of two](https://brooker.co.za/blog/2012/01/17/two-random.html).

In this variant we:

1. Map each customer to a subset of the queues (that subset could be only two), and
2. For each request put the customer’s request in the shortest queue in their subset, and
3. Periodically perturb the subsets.

This approach has absolutely great properties: O(1) queues, O(1) enqueue effort, O(1) dequeue effort, and strong isolation of noisy neighbors from other customers (assuming only a relatively small portion of customers are noisy neighbors).

## Single Hash

## Two-Choice Hash (Power of Two)

**Customers:**

Noisy (3x)
