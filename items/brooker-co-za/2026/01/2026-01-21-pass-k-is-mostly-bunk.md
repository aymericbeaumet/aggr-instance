---
title: Pass@k is Mostly Bunk
link: http://brooker.co.za/blog/2026/01/21/pass-k.html
source: brooker-co-za
published: 2026-01-21T00:00:00Z
updated: 2026-01-21T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Marc Brooker
summary: 'Pass@k is Mostly Bunk Exponentially better results? I''ll take three! Measuring the success of AI agents isn’t easy. It’s very sensitive to what success means, it can require a lot of samples, its highly context sensitive. Generally hard. So it doesn’t help that one of the most common metrics used for agents is (mostly) bunk. I’m talking about pass@k. What is pass@k? It’s the probability that at least one of k different attempts will succeed. A six-sided die, where pass means rolling a 6, has a pass@3 of 45% and a pass@10 of 83%. A D20 has a pass@25 of 72%, and a pass@100 of 99.4%. 99.4%! What a great evaluation result! Clearly the model is doing something meaningful and useful! No, it’s doing something meaningful and useful 5% of the time. The problem with pass@k is that’s exponentially forgiving. There’s a value of k, a fairly low one generally, that can make anything look good. Here’s that six-sided die again: Humans interacting with agents aren’t nearly that forgiving. They, in general, aren’t saying “well, I tried 10 times and it worked once, so I’m happy”. They’re saying “I tried 10 times and it only worked once, what a piece of junk”. They’re also doing multiple steps, and only happy when they all work. Exponentially unforgiving (for which pass^k is a much better metric). Why only mostly bunk? There are cases, where tasks are simple, evaluators are reliable, and humans are out of the loop, that the idea of getting exponentially better success rate with linear additional cost is good. I’ve made a similar argument about distributed systems in the past. But these tasks aren’t ubiquitous. Pass@k should be a metric that’s rarely used, and carefully justified every time it is used. If we’re going to drive the field of agentic AI forward, we need to keep ourselves honest on metrics. Footnotes It’s mildly interesting how none of the image generation models reliably generate images of legal dice.'
content: extracted
html: 2026-01-21-pass-k-is-mostly-bunk.html
remote_preview:
  url: https://brooker.co.za/blog/images/d6_trimmed.jpg
---

## About Me

My name is Marc Brooker. I like to build things that work, and do cool stuff. I like building big things. I also dabble in machining, welding, cooking, and skiing.

I am an engineer at Amazon Web Services (AWS) in Seattle, where I work on agentic AI, especially safety and policy for agentic AI. Before that, I worked on EC2, EBS, databases, serverless, and serverless databases.\
 All opinions are my own.

## Links

[My Publications and Videos](https://brooker.co.za/blog/publications.html)\
 [@marcbrooker on Mastodon](https://fediscience.org/@marcbrooker) [@MarcJBrooker on Twitter](https://twitter.com/MarcJBrooker)

[Is this blog written by AI?](https://brooker.co.za/blog/2026/06/18/my-blog-and-ai.html)

## Pass@k is Mostly Bunk

Exponentially better results? I'll take three!

Measuring the success of AI agents isn’t easy. It’s very sensitive to what *success* means, it can require a lot of samples, its highly context sensitive. Generally hard. So it doesn’t help that one of the most common metrics used for agents is (mostly) bunk. I’m talking about *pass@k*.

What is *pass@k*? It’s the probability that at least one of *k* different attempts will succeed. A six-sided die, where *pass* means rolling a 6, has a pass@3 of 45% and a pass@10 of 83%. A D20 has a pass@25 of 72%, and a pass@100 of 99.4%.

![](https://brooker.co.za/blog/images/d6_trimmed.jpg)

99.4%! What a great evaluation result! Clearly the model is doing something meaningful and useful! No, it’s doing something meaningful and useful 5% of the time.

The problem with pass@k is that’s *exponentially* forgiving. There’s a value of *k*, a fairly low one generally, that can make anything look good. Here’s that six-sided die again:

Humans interacting with agents aren’t nearly that forgiving. They, in general, aren’t saying “well, I tried 10 times and it worked once, so I’m happy”. They’re saying “I tried 10 times and it only worked once, what a piece of junk”. They’re also doing multiple steps, and only happy when they all work. Exponentially unforgiving (for which pass^k is a much better metric).

Why only *mostly* bunk? There are cases, where tasks are simple, evaluators are reliable, and humans are out of the loop, that the idea of getting exponentially better success rate with linear additional cost is good. I’ve made a similar argument about distributed systems [in the past](https://brooker.co.za/blog/2023/09/08/exponential.html). But these tasks aren’t ubiquitous. Pass@k should be a metric that’s rarely used, and carefully justified every time it is used.

If we’re going to drive the field of agentic AI forward, we need to keep ourselves honest on metrics.

*Footnotes*

1. It’s mildly interesting how none of the image generation models reliably generate images of legal dice.
