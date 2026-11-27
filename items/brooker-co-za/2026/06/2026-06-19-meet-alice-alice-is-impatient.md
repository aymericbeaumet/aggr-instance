---
title: Meet Alice. Alice is impatient.
link: http://brooker.co.za/blog/2026/06/19/waiting.html
source: brooker-co-za
published: 2026-06-19T00:00:00Z
updated: 2026-06-19T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Marc Brooker
summary: 'Meet Alice. Alice is impatient. What do you mean? Meet Alice. Alice uses your web service. Alice, like most humans, measures her time in seconds and minutes. Alice says your service is slow. You tell Alice that the mean request to your service completes in 100ms, but Alice says that her mean wait time is 1s. You’re both right. Meet Alex. Alex uses your web service. Alex, like most humans, measures his time in seconds and minutes. Alex says that when you have outages, they last a long time and he gets really annoyed. You tell Alex that your MTTR is less than 1 minute. Alex says that he sees the mean outage lasting 1 hour. Again, you’re both right. What’s going on? What’s going on is that you’re measuring time in requests, or in outages, and Alex and Alice are measuring time in seconds and minutes. When you have a long pause or a long outage, Alex and Alice sample that outage multiple times (maybe because they have multiple customers angry at them). The number of times they experience the outage is proportional to the length of the outage. But you only count that as one. More technically, what’s going on here is the inspection paradox. Alex and Alice don’t experience your latency distribution $f(t)$, they experience a t-weighted version of it. If you have a MTTR or mean request time of $\mathbb{E}[X]$, Alex and Alice experience a mean recovery time $\mathbb{E}_a[X]$ where $\mathbb{E}_a[X] = \frac{\mathbb{E}[X^2]}{2 \mathbb{E}[X]} = \frac{1}{2} \left( \mathbb{E}[X] + \frac{\mathrm{Var}(X)}{\mathbb{E}[X]} \right)$. Let’s play with this with a little simulation. Plug in your median latency (or recovery time), and 99th percentile latency (or recovery time), we’ll fit a log-normal distribution to it, and then plot both what your service metrics see and what your customers see. Median: ms p99: ms What your service sees (mean): – ms. What your customers experience (mean): – ms. For example, put in 30 as the median (let’s ignore the milliseconds and pretend these are minutes for now) for a 30 minute Median TTR (i.e. in half of your postmortems you see a recovery time of $\leq 30$ minutes), and 600 in as the p99 (one in every 100 events, recovery takes 10 hours). Your MTTR is just over an hour. Your customers experience a mean time to recovery of around 6 hours! Reasoning About Code, Instead The above argument may be a bit abstract for you, so let’s use another small simulator, presented as code this time, to communicate the core of the idea. Here, we have a server that experiences some periodic down-time (we haven’t simulated the times when it’s up, because they don’t interest us for now), and a Poisson process of arriving clients. We directly measure what the operator would measure as system downtime (e.g. mttr), and what the clients see. As you read this code, notice how each outage is sampled multiple times by the client, and how their samples are weighted by the remaining outage time (this is the t-weighting I talk about above). import random failure_mu = 1.0 failure_sigma = 3.0 client_arrival_rate = 100.0 samples = 1000 client_saw_times = [] server_saw_times = [] for i in range(samples): this_outage = random.lognormvariate(failure_mu, failure_sigma) server_saw_times.append(this_outage) t = 0.0 while True: next_arrival = random.expovariate(client_arrival_rate) if t + next_arrival > this_outage: break client_saw_times.append(this_outage - t) t += next_arrival ... print(f"""Client saw: mean {mean(client_saw_times)} median {pctile(client_saw_times, 0.5)} p99 {pctile(client_saw_times, 0.99)}""") print(f"""Model predicted: mean {mean(square(server_saw_times))/(2.0*mean(server_saw_times))}""") print(f"""Server saw: mttr {mean(server_saw_times)} median {pctile(server_saw_times, 0.5)} p99 {pctile(server_saw_times, 0.99)}""") Caring about tail latency (and long recovery times) There are many arguments for why tail latency (and long recovery times) are so important to understand (e.g. multiple samples), but this is the one that I think is the least widely understood. For service times, timeout-and-retry can hide this latency some of the time (as long as the running request doesn’t hold locks or other exclusive resources). But, for recovery time, no such hiding is possible. The heaviness if the tail matters a great deal. This is also one of the reasons I don’t like trimmed measurements (like trimmed means) as a way of thinking about service latency or recovery time. They throw out some really critical context about the shape of the right tail that dominates the customer experience (the other reason is related to Little’s Law and capacity usage, which I’ve written about before). In slightly more mathematical terms, the difference between MTTR ($\mathbb{E}[X]$) and what clients experience ($\mathbb{E}_a[X]$) is proportional to $\frac{Var(X)}{\mathbb{E}[X]}$. It’s not unusual, in real systems, for $Var(X)$ to be very large compared to $\mathbb{E}[X]$. These distributions tend to be very heavy-tailed, partially because the solutions to simple cases (like single host or even datacenter failures) are well-known and robust, while solutions to longer outages remain elusive industry-wide. A note on log-normal: I chose log-normal here for numerical convenience. It has the nice property that $\mathrm{lognormal}(\mu, \sigma^2)$ becomes $\mathrm{lognormal}(\mu + \sigma^2, \sigma^2)$. Also it’s well-behaved around 0. I don’t believe that log-normal is a particularly good choice of distribution for latency or recovery time metrics, and generally would approach these problems entirely non-parametrically.'
content: extracted
html: 2026-06-19-meet-alice-alice-is-impatient.html
remote_preview:
  url: https://brooker.co.za/blog/images/feed-icon-14x14.png
---

What do you mean?

Meet Alice. Alice uses your web service. Alice, like most humans, measures her time in seconds and minutes. Alice says your service is slow. You tell Alice that the mean request to your service completes in 100ms, but Alice says that her mean wait time is 1s.

You’re both right.

Meet Alex. Alex uses your web service. Alex, like most humans, measures his time in seconds and minutes. Alex says that when you have outages, they last a long time and he gets really annoyed. You tell Alex that your MTTR is less than 1 minute. Alex says that he sees the mean outage lasting 1 hour.

Again, you’re both right.

What’s going on? What’s going on is that you’re measuring time in requests, or in outages, and Alex and Alice are measuring time in seconds and minutes. When you have a long pause or a long outage, Alex and Alice *sample* that outage multiple times (maybe because they have multiple customers angry at them). The number of times they experience the outage is proportional to the length of the outage. But you only count that as one.

More technically, what’s going on here is the *inspection paradox*. Alex and Alice don’t experience your latency distribution $f(t)$, they experience a t-weighted version of it. If you have a MTTR or mean request time of $\\mathbb{E}\[X\]$, Alex and Alice experience a mean recovery time $\\mathbb{E}\_a\[X\]$ where $\\mathbb{E}\_a\[X\] = \\frac{\\mathbb{E}\[X^2\]}{2 \\mathbb{E}\[X\]} = \\frac{1}{2} \\left( \\mathbb{E}\[X\] + \\frac{\\mathrm{Var}(X)}{\\mathbb{E}\[X\]} \\right)$.

Let’s play with this with a little simulation. Plug in your median latency (or recovery time), and 99th percentile latency (or recovery time), we’ll fit a log-normal distribution to it, and then plot both what your service metrics see and what your customers see.

Median: ms    p99: ms

What your service sees (mean): **– ms**. What your customers experience (mean): **– ms**.

For example, put in 30 as the median (let’s ignore the milliseconds and pretend these are minutes for now) for a 30 minute Median TTR (i.e. in half of your postmortems you see a recovery time of $\\leq 30$ minutes), and 600 in as the p99 (one in every 100 events, recovery takes 10 hours). Your MTTR is just over an hour. Your customers experience a mean time to recovery of around 6 hours!

*Reasoning About Code, Instead*

The above argument may be a bit abstract for you, so let’s use another small simulator, presented as code this time, to communicate the core of the idea. Here, we have a server that experiences some periodic down-time (we haven’t simulated the times when it’s up, because they don’t interest us for now), and a Poisson process of arriving clients. We directly measure what the operator would measure as system downtime (e.g. `mttr`), and what the clients see.

As you read this code, notice how each outage is sampled multiple times by the client, and how their samples are weighted by the remaining outage time (this is the *t-weighting* I talk about above).

```python
import random
failure_mu = 1.0
failure_sigma = 3.0
client_arrival_rate = 100.0
samples = 1000

client_saw_times = []
server_saw_times = []

for i in range(samples):
    this_outage = random.lognormvariate(failure_mu, failure_sigma)
    server_saw_times.append(this_outage)
    t = 0.0
    while True:
        next_arrival = random.expovariate(client_arrival_rate)
        if t + next_arrival > this_outage:
            break
        client_saw_times.append(this_outage - t)
        t += next_arrival

...

print(f"""Client saw:
    mean {mean(client_saw_times)} 
    median {pctile(client_saw_times, 0.5)}
    p99 {pctile(client_saw_times, 0.99)}""")
print(f"""Model predicted: 
    mean {mean(square(server_saw_times))/(2.0*mean(server_saw_times))}""")
print(f"""Server saw:
    mttr {mean(server_saw_times)}
    median {pctile(server_saw_times, 0.5)}
    p99 {pctile(server_saw_times, 0.99)}""")
```

*Caring about tail latency (and long recovery times)*

There are many arguments for why tail latency (and long recovery times) are so important to understand (e.g. [multiple samples](https://brooker.co.za/blog/2021/04/19/latency.html)), but this is the one that I think is the least widely understood. For service times, timeout-and-retry can hide this latency some of the time (as long as the running request doesn’t hold locks or other exclusive resources). But, for recovery time, no such hiding is possible. The heaviness if the tail matters a great deal. This is also one of the reasons I don’t like trimmed measurements (like trimmed means) as a way of thinking about service latency or recovery time. They throw out some really critical context about the shape of the right tail that dominates the customer experience (the other reason is related to Little’s Law and capacity usage, [which I’ve written about before](https://brooker.co.za/blog/2017/12/28/mean.html)).

In slightly more mathematical terms, the difference between MTTR ($\\mathbb{E}\[X\]$) and what clients experience ($\\mathbb{E}\_a\[X\]$) is proportional to $\\frac{Var(X)}{\\mathbb{E}\[X\]}$. It’s not unusual, in real systems, for $Var(X)$ to be very large compared to $\\mathbb{E}\[X\]$. These distributions tend to be very heavy-tailed, partially because the solutions to simple cases (like single host or even datacenter failures) are well-known and robust, while solutions to longer outages remain elusive industry-wide.

*A note on log-normal:* I chose log-normal here for numerical convenience. It has the nice property that $\\mathrm{lognormal}(\\mu, \\sigma^2)$ becomes $\\mathrm{lognormal}(\\mu + \\sigma^2, \\sigma^2)$. Also it’s well-behaved around 0. I don’t believe that log-normal is a particularly good choice of distribution for latency or recovery time metrics, and generally would approach these problems entirely non-parametrically.
