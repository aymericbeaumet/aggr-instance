---
title: 'Lorenz and Little: How Much Does Your Tail Cost?'
link: http://brooker.co.za/blog/2026/07/29/lorenz-and-little.html
source: brooker-co-za
published: 2026-07-29T00:00:00Z
updated: 2026-07-29T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Marc Brooker
summary: 'Lorenz and Little: How Much Does Your Tail Cost? Lorenz and Little sounds like hipster burger bar from 2015. It’s time for Marc’s Amateur Statistics Corner! Today: why I pay a lot of attention to tail latency when optimizing cost. I’ve written before on the importance of tail latency for customer experience (e.g. in 2026, 2021, and 2021, and 2017). Today, I want to talk about tail latency from the perspective of cost and capacity. Like many system operators, I think about tail latency using percentiles. Here’s a question: how much does each of my latency percentiles contributed to the mean latency? Intuitively, the answer is “quite a lot”, but can we quantify that? We can! The thing we’re looking for is the empirical Lorenz Curve. It directly calculates the answer to the question: given a latency percentile $P$ (e.g. p99=100ms), how much do requests taking shorter than $P$ contribute to the mean latency? (Let’s call it $L(P)$ , so the real answer to our question is $1 - L(P)$). Starting from latency samples, the calculation is pretty simple: L = sum(sorted(x)[:k]) / sum(x) (for a set of n latency samples x, and k=p*n). From a vector of quantiles, things get a little more complicated, because we have to choose how to interpolate between the samples and extrapolate out to the maximum. Here I’m interpolating using a power law, which is a little bit of a sin1, but good enough for our purposes. # Calculate 1 - L(p) for a vector of measured quantiles # q - an array of quantiles (e.g. [1, 10, 200, 10000, 20000]) # p - an array of percentiles they''re measured at (e.g. [0, 0.5, 0.9, 0.99, 0.999]) # OneMinusL - One minus the empirical Lorenz curve for each of the percentiles def OneMinusL(q, p): ...Show full implementationHide implementation # Calculate 1 - L(p) for a vector of measured quantiles # q - an array of quantiles (e.g. [1, 10, 200, 10000, 20000]) # p - an array of percentiles they''re measured at (e.g. [0, 0.5, 0.9, 0.99, 0.999]) # OneMinusL - One minus the empirical Lorenz curve for each of the percentiles def OneMinusL(q, p): q, p = np.asarray(q, float), np.asarray(p, float) assert q.shape == p.shape and q.size >= 2, "q, p must be same-length, size >= 2" assert p[0] == 0 and p[-1] < 1, "p must start at 0 (else mass is dropped) and end below 1" assert np.all(np.diff(p) > 0), "p must be strictly increasing" assert q[0] > 0 and np.all(np.diff(q) > 0), "q must be positive and strictly increasing" w = q*(1-p) a = np.log((1-p[1:])/(1-p[:-1]))/np.log(q[:-1]/q[1:]) assert np.all(np.abs(a-1) > 1e-9), "alpha == 1 in some cell: divide by zero" assert a[-1] > 1, f"tail alpha={a[-1]:.3f} <= 1: infinite mean, not estimable" c = np.r_[0, np.cumsum(a/(a-1)*(w[:-1]-w[1:]))] return 1 - c/(c[-1] + a[-1]/(a[-1]-1)*w[-1]) For example: print(OneMinusL([1, 10, 200, 10000, 20000], [0, 0.5, 0.9, 0.99, 0.999])) [1. 0.99377318 0.93082759 0.51712644 0.10342529] That tells us that, for this distribution, requests at or longer than the median (p50) contribute about 99% of the mean latency, at requests at or longer than the p99 contribute about 52% of the mean latency2. That’s fun, but why do I care? Because Little’s law tells us that this same value (contribution to the mean) is also the contribution to the concurrency in the system. In a simple threaded system, if $1 - L(p) = k$ , then $100k$% of the busy threads in our service are busy with requests with a latency3 above the $p$th percentile. Queues, event-based implementations, etc complicate the mapping of concurrency to cost, so you’ll need to think about those in context of your own system. In my experience, it’s not unusual in services for $1 - L(0.99)$ to be greater than $0.5$ or even $0.75$. That tells us that optimizing the tail could be a much bigger than expected contributor to reducing concurrency, and along with that reducing capacity demand, lock contention, and other things that come with higher concurrency. Tails tend to be disproportionately expensive to serve, and so disproportionately important to focus on as we think about optimization. We shouldn’t make the mistake of trimming them off, because they’re often the thing that’s driving costs! (And, of course, bad customer experiences). If you want to play with some values, type your percentiles in here, and see your curve: p0: ms p50: ms p90: ms p99: ms p99.9: ms Share of mean latency from requests at or above each percentile: p50 –, p90 –, p99 –, and p99.9 –. By Little''s law, these are also their shares of system concurrency. Footnotes There are two sins here: one of them is that I’m arbitrarily choosing a way to interpolate, and the other than I’m arbitrarily choosing a way to extrapolate. The denser your percentiles and closer your data fits a Pareto distribution, the less that matters. But if you want an exact empirical answer, you’ll need to use a different approach (specifically, the approach based on the sorted raw samples). I did say it was amateur statistics corner. The other sin here, of course, is that our measured percentiles are estimates of the true percentiles, and the uncertainty goes up as the percentile gets larger. You might get very variable results if you have heavy tails and small samples. Not ideal, but doesn’t change the conclusion. More accurately, requests which will go on to complete with a latency above the $p$th percentile. Because we’re doing this post-hoc based on monitoring and metrics, that doesn’t matter. If you’re trying to do this with live observations, that distinction may matter. Again, amateur statistics corner.'
content: extracted
html: 2026-07-29-lorenz-and-little-how-much-does-your-tail-cost.html
remote_preview:
  url: https://brooker.co.za/blog/images/feed-icon-14x14.png
extra:
  content:interactive: true
---

Lorenz and Little sounds like hipster burger bar from 2015.

It’s time for Marc’s Amateur Statistics Corner! Today: why I pay a lot of attention to tail latency when optimizing cost.

I’ve written before on the importance of tail latency for customer experience (e.g. [in 2026](https://brooker.co.za/blog/2026/06/19/waiting.html), [2021](https://brooker.co.za/blog/2021/10/20/simulation.html), and [2021](https://brooker.co.za/blog/2021/04/19/latency.html), and [2017](https://brooker.co.za/blog/2017/12/28/mean.html)). Today, I want to talk about tail latency from the perspective of cost and capacity.

Like many system operators, I think about tail latency using percentiles.

Here’s a question: how much does each of my latency percentiles contributed to the mean latency? Intuitively, the answer is “quite a lot”, but can we quantify that? We can! The thing we’re looking for is the [empirical Lorenz Curve](https://en.wikipedia.org/wiki/Lorenz_curve). It directly calculates the answer to the question: given a latency percentile $P$ (e.g. p99=100ms), how much do requests taking shorter than $P$ contribute to the mean latency? (Let’s call it $L(P)$ , so the real answer to our question is $1 - L(P)$).

Starting from latency samples, the calculation is pretty simple: `L = sum(sorted(x)[:k]) / sum(x)` (for a set of `n` latency samples `x`, and `k=p*n`). From a vector of quantiles, things get a little more complicated, because we have to choose how to interpolate between the samples and extrapolate out to the maximum. Here I’m interpolating using a power law, which is a little bit of a sin [1](http://brooker.co.za/blog/2026/07/29/lorenz-and-little.html#foot1), but good enough for our purposes.

`# Calculate 1 - L(p) for a vector of measured quantiles
# q - an array of quantiles (e.g. [1, 10, 200, 10000, 20000])
# p - an array of percentiles they're measured at (e.g. [0, 0.5, 0.9, 0.99, 0.999])
# OneMinusL - One minus the empirical Lorenz curve for each of the percentiles
def OneMinusL(q, p):
  ...` Show full implementation Hide implementation

```python

# Calculate 1 - L(p) for a vector of measured quantiles
# q - an array of quantiles (e.g. [1, 10, 200, 10000, 20000])
# p - an array of percentiles they're measured at (e.g. [0, 0.5, 0.9, 0.99, 0.999])
# OneMinusL - One minus the empirical Lorenz curve for each of the percentiles
def OneMinusL(q, p):
  q, p = np.asarray(q, float), np.asarray(p, float)
  assert q.shape == p.shape and q.size >= 2, "q, p must be same-length, size >= 2"
  assert p[0] == 0 and p[-1] < 1, "p must start at 0 (else mass is dropped) and end below 1"
  assert np.all(np.diff(p) > 0), "p must be strictly increasing"
  assert q[0] > 0 and np.all(np.diff(q) > 0), "q must be positive and strictly increasing"
  w = q*(1-p)
  a = np.log((1-p[1:])/(1-p[:-1]))/np.log(q[:-1]/q[1:])
  assert np.all(np.abs(a-1) > 1e-9), "alpha == 1 in some cell: divide by zero"
  assert a[-1] > 1, f"tail alpha={a[-1]:.3f} <= 1: infinite mean, not estimable"
  c = np.r_[0, np.cumsum(a/(a-1)*(w[:-1]-w[1:]))]
  return 1 - c/(c[-1] + a[-1]/(a[-1]-1)*w[-1])
```

For example:

```python

print(OneMinusL([1, 10, 200, 10000, 20000], [0, 0.5, 0.9, 0.99, 0.999]))
[1. 0.99377318 0.93082759 0.51712644 0.10342529]
```

That tells us that, for this distribution, requests at or longer than the median (p50) contribute about 99% of the mean latency, at requests at or longer than the p99 contribute about 52% of the mean latency [2](http://brooker.co.za/blog/2026/07/29/lorenz-and-little.html#foot2).

That’s fun, but why do I care? Because [Little’s law](https://en.wikipedia.org/wiki/Little%27s_law) tells us that this same value (contribution to the mean) is also the contribution to the concurrency in the system. In a simple threaded system, if $1 - L(p) = k$ , then $100k$% of the busy threads in our service are busy with requests with a latency [3](http://brooker.co.za/blog/2026/07/29/lorenz-and-little.html#foot3) above the $p$th percentile. Queues, event-based implementations, etc complicate the mapping of concurrency to cost, so you’ll need to think about those in context of your own system.

In my experience, it’s not unusual in services for $1 - L(0.99)$ to be greater than $0.5$ or even $0.75$. That tells us that optimizing the tail could be a much bigger than expected contributor to reducing concurrency, and along with that reducing capacity demand, lock contention, and other things that come with higher concurrency. Tails tend to be disproportionately expensive to serve, and so disproportionately important to focus on as we think about optimization. We shouldn’t make the mistake of trimming them off, because they’re often the thing that’s driving costs! (And, of course, bad customer experiences).

If you want to play with some values, type your percentiles in here, and see your curve:

p0: ms   p50: ms   p90: ms   p99: ms   p99.9: ms

Share of mean latency from requests at or above each percentile: p50 **–**, p90 **–**, p99 **–**, and p99.9 **–**. By Little's law, these are also their shares of system concurrency.

**Footnotes**

1. There are two sins here: one of them is that I’m arbitrarily choosing a way to interpolate, and the other than I’m arbitrarily choosing a way to extrapolate. The denser your percentiles and closer your data fits a Pareto distribution, the less that matters. But if you want an exact empirical answer, you’ll need to use a different approach (specifically, the approach based on the sorted raw samples). I did say it was amateur statistics corner.
2. The other sin here, of course, is that our measured percentiles are estimates of the true percentiles, and the uncertainty goes up as the percentile gets larger. You might get very variable results if you have heavy tails and small samples. Not ideal, but doesn’t change the conclusion.
3. More accurately, requests which will go on to complete with a latency above the $p$th percentile. Because we’re doing this post-hoc based on monitoring and metrics, that doesn’t matter. If you’re trying to do this with live observations, that distinction may matter. Again, amateur statistics corner.
