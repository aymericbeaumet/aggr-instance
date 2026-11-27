---
title: Mathematics of Consensus — Accidental Lecture
link: https://tigerbeetle.com/blog/2025-11-22-mathematics-of-consensus
source: tigerbeetle-com
published: 2025-11-22T00:00:00Z
updated: 2025-11-22T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- matklad
summary: Somewhat accidentally, we recorded an impromptu ~2 hour lecture explaining the mathematics behind consensus algorithms, using a Go Pro fisheye—the camera on hand 🤣
content: extracted
html: 2025-11-22-mathematics-of-consensus-accidental-lecture.html
remote_preview:
  url: https://tigerbeetle.com/blog/2025-11-22-mathematics-of-consensus/banner-af31067f.webp
---

Somewhat accidentally, we recorded an impromptu ~2 hour lecture explaining the mathematics behind consensus algorithms, using a Go Pro fisheye—the camera on hand 🤣

[YouTube video player](https://www.youtube.com/watch?v=thY12Wnrmyw)

In the lecture, I (Alex from TigerBeetle) with my colleague, Tobi, focus on the mathematics of consensus, and explain consensus using the language of set theory. In a feat of alchemical transmutation, we turn [Led Zeppelin](https://youtu.be/RlNhD0oS5pk) into [Golden Earring](https://youtu.be/el4q7sNirXY), and anyone who understands the basics of set theory into someone who understands consensus. (Conveniently, Tobi knew the basics of set theory, but didn’t yet understand consensus.)

The transcript of the first few minutes is below, so you can get the flavor and give it a watch if it sounds interesting. If you prefer reading, the lecture is loosely based on [*Notes on Paxos*](https://matklad.github.io/2020/11/01/notes-on-paxos.html) write up.

**Alex**: “Consensus algorithms have a reputation of being notoriously difficult to understand. There are two sides to this story: One is mathematics: a formal proof of correctness of the algorithm of consensus. The other is pragmatics, or engineering: building an actual real world distributed system which is correct and fast.

“This divide is evident even in the history of consensus. [The paper](https://dl.acm.org/doi/10.1145/62546.62549) which solved consensus in the presence of network partitions was published in 1988 by Brian Oki under the supervision of Barbara Liskov. It introduced the pioneering [Viewstamped Replication](http://pmg.csail.mit.edu/papers/vr-revisited.pdf) consensus algorithm as a part of a larger data management system. It was primarily an engineering paper. The famous [Paxos paper](https://lamport.azurewebsites.net/pubs/pubs.html#lamport-paxos) by Leslie Lamport was submitted in 1990. That paper stripped down the consensus algorithm to its bare essentials to reveal the mathematical beauty of the thing.

“Although mathematics is motivated by engineering and engineering would have been impossible without the underlying mathematics, trying to understand the two at the same time is where the difficulty of understanding consensus stems from. Tobi, are you ready to be transmuted into someone who understands consensus?”

**Tobi**: “Always ready!” [Continue](https://www.youtube.com/watch?v=thY12Wnrmyw&t=145s)!

P.S. Check out [notes](https://tigerbeetle.com/blog/2025-11-22-mathematics-of-consensus/lecture-notes-71a912ac.pdf) by Chaitanya (another TigerBeetle colleague), taken live!
