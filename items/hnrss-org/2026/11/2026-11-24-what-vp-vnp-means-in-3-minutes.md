---
title: What VP ≠ VNP Means (In 3 Minutes)
link: https://www.friedgil.com/vpvnp
source: hnrss-org
published: 2026-11-24T10:56:56Z
updated: 2026-11-24T10:56:56Z
first_seen: 2026-11-26T12:13:26.788658858Z
authors:
- vinhnx
content: extracted
html: 2026-11-24-what-vp-vnp-means-in-3-minutes.html
remote_preview:
  url: https://www.friedgil.com/image.png
  alt: Example of an arithmetic circuit
---

I assume you know about the P vs. NP problem. But what is this wacky thing [Anthropic just posted](https://github.com/anthropics/formal-math/blob/main/vp-vnp/paper.pdf)?

You learned how to take a determinant in your linear algebra class:

det(123456789)=1⋅(5⋅9-6⋅8)-2⋅(4⋅9-6⋅7)+3⋅(4⋅8-5⋅7)=0

The naive way is fine for 3×3 matrices, but notice that you need to perform Θ(n!) arithmetic operations here. Exercise for the reader is to find the exact number of operations (hint: start with a recurrence relation).

Luckily, if you paid attention, you will remember there are faster ways to do this for big n×n matrices, like Gaussian elimination. If we stick only to adding and multiplying, we can still find a determinant in polynomial time ([there are better ways, but this is an old, readable method](https://www.sciencedirect.com/science/article/pii/0020019084900188)).

A related problem to the determinant is finding the permanent. It’s like a determinant without any subtraction:

perm(123456789)=1⋅(5⋅9+6⋅8)+2⋅(4⋅9+6⋅7)+3⋅(4⋅8+5⋅7)=450

Unlike determinants, there is no known way to compute permanents in polynomial time. Since finding the permanent of a matrix of 0s and 1s is NP-hard (and in fact [#P-complete](https://en.wikipedia.org/wiki/Counting_problem_\(complexity\))), it would be pretty important if there were a way.

What the Anthropic proof that VP≠VNP means is that there is no way to do this if we are limited to arithmetic circuits of size polynomial in the input-size, which look like this:

![Example of an arithmetic circuit](https://www.friedgil.com/image.png)

This means that for P to equal NP, there must be some trick to computing permanents using AND/OR/NOT on the bits of the input that is better than addition and multiplication. There are many problems where access to Boolean operators is helpful, but it isn’t clear how they would help with the permanent. If you think of a way, go claim your $1 million.
