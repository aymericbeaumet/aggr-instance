---
title: Intuitive concurrent programming in any language
link: https://www.lf-lang.org/
source: hnrss-org
published: 2026-10-15T13:18:44Z
updated: 2026-10-15T13:18:44Z
first_seen: 2026-10-17T16:38:09.894543738Z
authors:
- mpweiher
content: extracted
html: 2026-10-15-intuitive-concurrent-programming-in-any-language.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50103255
  num_comments: 28
  points: 105
---

## Build **predictable**, **concurrent**, **time-sensitive**, and **distributed** systems.

Lingua Franca is a coordination language for efficient, deterministic, multi-threaded, time-sensitive, embedded, and distributed programs. The result of decades of research, it offers more repeatable behavior than other concurrent programming frameworks, including threads, pub-sub, actors, and service-oriented architectures.

### Deterministic Concurrency

Lingua Franca offers a straightforward way to write deterministic multi-threaded and distributed applications, eliminating concerns about thread management, synchronization, and race conditions.

### Built-in Timing Semantics

Timing is embedded within Lingua Franca's core, empowering developers to handle time-sensitive tasks with precision and without the complex timing logic typically required in concurrent programming.

### Simplified Distribution

Lingua Franca enables seamless transition from single to distributed system architectures with minimal modifications, abstracting away the complexities of distributed systems programming.

Architect your application in LF

```
import Player from "../Player.lf"

main reactor RockPaperScissors {
  // Instantiate two Player reactors here
  player1 = new Player(id=1)
  player2 = new Player(id=2)

  // Make connections between them
  player2.reveal -> player1.observe
  player1.reveal -> player2.observe
}
```

[![](https://raw.githubusercontent.com/gitpod-io/gitpod/30da76375c996109f243491b23e47feefab7217f/components/dashboard/public/button/open-in-gitpod.svg)](https://gitpod.io/new#https://github.com/lf-lang/playground-lingua-franca/tree/main)

## A New Programming Paradigm

Lingua Franca is the first reactor-oriented coordination language. It allows you to specify reactive components and compose them. Lingua Franca eliminates race conditions by construction, makes it easy to specify timed behavior, and removes the need to perform manual synchronization.

Consider a game of "rock paper scissors" where two players need to reveal their choice simultaneously. In Lingua Franca, "at the same time" has a clear and precise meaning. This makes the implementation simple and intuitive, and guarantees it to be fair. If the Player class were to observe the other's choice before revealing its own, Lingua Franca's causality analysis would find a causality loop and tell you that the program is invalid.

## Not a New Programming Language

With Lingua Franca, you forget about thread libraries or message passing middlewares, but you continue using the languages you like. The bodies of reactive code that make up the functionality of reactors are written in the programming language of your choice. We currently support C, C++, Python, TypeScript, and Rust.

## Open-source and Supported by Research

Lingua Franca was first developed at UC Berkeley where it was influenced by decades worth of research in models of computation.

Shortly after its inception, researchers from Kiel University and TU Dresden joined the team, contributing diagram synthesis and layout technology, highly-efficient runtime implementations, and various compiler improvements. Since then, we have worked with real-time systems experts from UT Dallas, embedded systems specialists from NTNU, and networking and security researchers from Hanyang University and ASU.
