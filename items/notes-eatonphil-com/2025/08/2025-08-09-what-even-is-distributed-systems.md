---
title: What even is distributed systems
link: http://notes.eatonphil.com/2025-08-09-what-even-is-distributed-systems.html
source: notes-eatonphil-com
published: 2025-08-09T00:00:00Z
updated: 2025-08-09T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
labels:
- distsys
summary: 'Distributed systems is simply the study of interactions between processes. Every two interacting processes form a distributed system, whether they are on the same host or not. Distributed systems create new challenges (compared to single-process systems) in terms of correctness (i.e. consistency), reliability, and performance (i.e. latency and throughput). The best way to learn about the principles and fundamentals of distributed systems is to 1) read Designing Data Intensive Applications and 2) read through the papers and follow the notes in the MIT Distributed Systems course. For Designing Data Intensive Applications (DDIA), I strongly encourage you to find buddies at work or online who will read it through with you. You can also always join the Software Internals Discord''s #distsys channel to ask questions as you go. But it''s still best if you have some partners to go through the book with, even if they are as new to it as you. I also used to think that you might want to wait a few years into your career before reading DDIA but when you have friends to read it with I think you need not wait. If you have only skimmed the book you should definitely go back and give it a thorough read. I have read it three times already and I will read it again as part of the Software Internals Book Club next year after the 2nd Edition is published. Keep in mind that every chapter of DDIA provides references to papers you can keep reading should you end up memorizing DDIA itself. When you''ve read parts of DDIA or the MIT Distributed Systems course and you want practice, the Fly.io x Jepsen Distributed Systems Challenge is one guided option. Other options might include simply implementing (getting progressively more complex down the list): two-phase commit three-phase commit single-decree Paxos highly available key-value store on top of a 3rd-party consensus library chain replication (or CRAQ), using a 3rd-party consensus library Raft epaxos And if you get bored there you can see Alex Miller''s Data Replication Design Spectrum for more ideas and variants. And if you want more people to follow, check out the Distributed Systems section of my favorite blogs page. If these projects and papers sound arcane or intimidating, know that you will see the problems these projects/papers solve whether or not you know and understand these solutions. Developers often end up reinventing hacky versions of these which are more likely to have subtle bugs. While instead you can recognize and use one of these well-known building blocks. Or at least have the background to better reason about correctness should you be in a situation where you must work with a novel distributed system or you end up designing a new one yourself. And again, if you want folks to bounce ideas off of or ask questions to, I strongly encourage you to join the Software Internals Discord and ask there! I wrote a short post on learning the fundamentals of distributed systems, with a few suggested resources to read and a few suggested projects to try. pic.twitter.com/b0EhDP8K0t — Phil Eaton (@eatonphil) August 9, 2025'
content: extracted
html: 2025-08-09-what-even-is-distributed-systems.html
---

Distributed systems is simply the study of interactions between processes. Every two interacting processes form a distributed system, whether they are on the same host or not. Distributed systems create new challenges (compared to single-process systems) in terms of correctness (i.e. *consistency*), reliability, and performance (i.e. latency and throughput).

The best way to learn about the principles and fundamentals of distributed systems is to 1) read Designing Data Intensive Applications and 2) read through the papers and follow the notes in the [MIT Distributed Systems course](https://pdos.csail.mit.edu/6.824/schedule.html).

For Designing Data Intensive Applications (DDIA), I strongly encourage you to find buddies at work or online who will read it through with you. You can also always join the [Software Internals Discord](https://eatonphil.com/discord.html)'s #distsys channel to ask questions as you go. But it's still best if you have some partners to go through the book with, even if they are as new to it as you.

I also used to think that you might want to wait a few years into your career before reading DDIA but when you have friends to read it with I think you need not wait.

If you have only skimmed the book you should definitely go back and give it a thorough read. I have read it three times already and I will read it again as part of the [Software Internals Book Club](https://eatonphil.com/bookclub.html) next year after the 2nd Edition is published.

Keep in mind that every chapter of DDIA provides references to papers you can keep reading should you end up memorizing DDIA itself.

When you've read parts of DDIA or the MIT Distributed Systems course and you want practice, the Fly.io x Jepsen [Distributed Systems Challenge](https://fly.io/dist-sys/) is one guided option. Other options might include simply implementing (getting progressively more complex down the list):

- two-phase commit
- three-phase commit
- single-decree Paxos
- highly available key-value store on top of a 3rd-party consensus library
- chain replication (or CRAQ), using a 3rd-party consensus library
- Raft
- epaxos

And if you get bored there you can see Alex Miller's [Data Replication Design Spectrum](https://transactional.blog/blog/2024-data-replication-design-spectrum) for more ideas and variants.

And if you want more people to follow, check out the [Distributed Systems section](https://eatonphil.com/blogs.html#Distributedsystems) of my favorite blogs page.

If these projects and papers sound arcane or intimidating, know that you will see the problems these projects/papers solve whether or not you know and understand these solutions. Developers often end up reinventing hacky versions of these which are more likely to have subtle bugs.

While instead you can recognize and use one of these well-known building blocks. Or at least have the background to better reason about correctness should you be in a situation where you must work with a novel distributed system or you end up designing a new one yourself.

And again, if you want folks to bounce ideas off of or ask questions to, I strongly encourage you to join the Software Internals Discord and ask there!

> I wrote a short post on learning the fundamentals of distributed systems, with a few suggested resources to read and a few suggested projects to try. [pic.twitter.com/b0EhDP8K0t](https://t.co/b0EhDP8K0t)
>
> — Phil Eaton (@eatonphil) [August 9, 2025](https://twitter.com/eatonphil/status/1954196346199519365?ref_src=twsrc%5Etfw)
