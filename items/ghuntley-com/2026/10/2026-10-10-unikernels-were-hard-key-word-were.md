---
title: 'unikernels were hard. key word: were.'
link: https://ghuntley.com/unikernels/
source: ghuntley-com
published: 2026-10-10T14:23:33Z
updated: 2026-10-10T14:23:33Z
first_seen: 2026-10-10T15:51:15.675508634Z
authors:
- Geoffrey Huntley
labels:
- ai
summary: Justin Cormack, who worked on MirageOS back in the day, called me at the pub to talk about why unikernels are coming back. The friction that made them hard is gone, the operating system is design debt, and if you care about security you should seriously consider them.
content: extracted
html: 2026-10-10-unikernels-were-hard-key-word-were.html
remote_preview:
  url: https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/2026/10/Symbolic-traditional-tattoo-art-print-portraying-a-unikernel-operating-system-as-a-lively-streamlined-digital-core-with-complex-ornamental-circuitry--bright-vibrant-retro-colors--intense-dramatic-lighting--energetic-glowing-accents--sharp-h.jpg
  alt: 'unikernels were hard. key word: were.'
---

Justin Cormack, who worked on MirageOS and Unikernel Systems back in the day, has been noticing what I've been noticing: people are discovering (or rediscovering) unikernels again. He's running a series of conversations on the topic for his newsletter, and I was first up. He emailed me, and five minutes later I was talking to him from the pub with a stein of beer in hand. We went deep on Mirage, Orleans, Haskell, Nix, Cursed and a whole lot more.

[Unikernels in 2026: Justin Cormack in conversation with Geoffrey Huntley](https://www.youtube.com/watch?v=DOjHBwdfBpA)

Here's the gist, written up properly, with chapter links at the bottom if you want to jump to a specific part of the conversation. Justin's edited transcript is over at [Ignore Previous Directions](https://buttondown.com/justincormack/archive/unikernels-in-2026-part-1-geoffrey-huntley/?ref=ghuntley.com).

> Unikernels were hard. Key word: were. Now we have AI.

## what a unikernel is, and why they were hard

I first ran into unikernels around 2015. I'd staffed up a team of Haskellers and gone pretty deep on functional programming. Where there are Haskellers, there are OCaml programmers, and from there you find [MirageOS](https://mirage.io/?ref=ghuntley.com). Great idea. I played with it back then.

A unikernel is the idea that your application *is* the operating system. There's no userland. If you want a web server, DNS, or to send an email, there's nothing you can fork or spawn. You have to write those things as libraries in your application.

That was the friction. Justin remembers it well: when they were building Mirage, they had a TCP stack and an HTTPS stack, but there was almost nothing for storage. They were pulling drivers out of NetBSD because they could run them in userspace. It was hard back then.

There's a lot of dogma in our industry. Nix is hard. Bazel is hard. Unikernels are hard. Yes, they *were*. These hard concepts are now in the model weights. All you've got to do is prompt for them and, cognitively, get rid of the dogma that they're hard.

## the operating system is design debt

Every application that isn't a unikernel was built on the assumption that there's an application, and then there's an operating system underneath. Why do we even have an operating system? Because forty years ago there was a human operator. I've done IBM 5250, AIX, Solaris, and mainframes. The multi-user operating system exists because a person sat in front of it, and then we put the application on top.

I consider that design debt, and here's why it matters right now. Applications get popped. They were getting popped before AI. Someone pops the userland application and gets a shell. That shell is a VIP butler service for exfiltration.

With a unikernel, the attack surface is much smaller. If the functionality isn't in the application (the operating system), then the attacker is screwed. There's no next hop.

Justin pushed back here, and fairly. Attack-surface reduction is something people are very fuzzy about. You can remove the shell from a Linux container, but almost every Linux environment still has something that's effectively an interpreter. You can execute a new program without a writable filesystem. You've still got memory safety and gadgets to worry about.

All true. But look at what we've been doing for twenty-six years. The earliest adage I remember from the SunOS and cgi-bin days was *"don't put the compiler on production."* Then came build containers and production containers. Then Chainguard. We keep chipping away at the attack surface, instead of going in the opposite direction and ensuring there *is* no attack surface.

And this is the bit people miss: if there's no shell and no interpreter, there's nothing in the model weights that knows what to do next. That turns a drive-by (pick your framework's RCE of the week and you've got a shell, and the model weights *know* what to do with shell access) into a targeted attack that needs your source code.

## you can just port the missing libraries now

The classic objection: your unikernel needs to talk to Stripe, and OCaml doesn't have a Stripe library. Before AI you'd sigh and write it. Now? [Run a loop](https://ghuntley.com/loop/) to port the Go library to OCaml. Here you go: Stripe in a unikernel.

Justin had a great example of the same thing. He'd been building minimal Linux OS images for appliances, which is halfway to a unikernel anyway, since you're only running one application as PID 1. He needed to make an XFS filesystem. Rather than drag in xfsprogs and everything it brings with it, he sat down with an agent and had it write `mkfs.xfs` in Rust, producing byte-for-byte identical output, with every flag interpreted. It reverse-engineered the on-disk formats one by one, with tests across block sizes. It took a few hours.

That works because the original tool is a golden oracle. Generate filesystems at different sizes with both implementations and diff them. Port the tests across. Automate it.

[porting software has been trivial for a while now. here's how you do it.](https://ghuntley.com/porting/)

If you have an oracle, porting is a loop.

Geoffrey Huntley Geoffrey Huntley

Storage was the other big gap. Most workloads these days are cloud-shaped, even on-prem, so take the [turbopuffer](https://turbopuffer.com/?ref=ghuntley.com) approach: S3 as your primary, infinitely growable storage, with a local NVMe block cache and an LRU (or whatever caching algorithm you like) for the hot bits. Justin is a massive "S3 for everything" fan too. As long as latency isn't the constraint, you get infinite storage with multi-user access, and you can build everything on it.

## nix machine tests and overlays

Justin had been experimenting with Nix too, and was surprised that the first time he got an agent to prototype an OS, it built all the tests into flakes.

Nix the language sucks. Nixpkgs is great. [NixOS machine tests](https://nixos.org/manual/nixos/stable/?ref=ghuntley.com#sec-nixos-tests) are the bee's knees: you write a test that spins up a fleet of machines and exercises the interaction between your network rules and your application. It's the thing people don't know about.

And when something upstream is broken, or there's a supply chain problem in your dependencies, that's just an overlay.

[the world hasn't figured out yet that you can literally just fix everything with a Nix overlay](https://ghuntley.com/nix/)

Patch the world.

Geoffrey Huntley Geoffrey Huntley

> If you have to tool-call a human, also known as "Dear Maintainer", who might be on holiday or might have abandoned the project, and wait a day, two days, or even five minutes, that's not AGI.

We're building recursive products here. Agents need the ability to modify the world as first-party source, not as third-party bundled binaries. We're going back to the contrib folder and Unix patches.

## if you care about security, you have two choices

Justin asked what unikernels still need for people to discover them. Honestly, it's this. We've raised two generations of developers who don't even know they're a thing.

If you deeply care about security, there are really two choices:

1. You're sending satellites into space, and you should probably use [seL4](https://sel4.systems/?ref=ghuntley.com), a formally verified operating system. (I'm still a bit salty about the Australian government disbanding that team.)
2. Everyone else should seriously consider unikernels. Stop trying to harden something that is very hackable. Invert it and design from the other direction.

The other classic criticism is that many early unikernel designs ran everything at a single privilege level: your application in the same ring as the OS. In 2026, that's a prompt away from being fixed if you want ring separation. It's certainly more secure than praying to god your systemd cgroup configuration is right.

Think about how much time enterprises spend patching the world every time something new drops in Linux. Upstream now expects you to patch your kernel weekly. The week we recorded, someone popped KVM (essentially Firecracker, the core primitive we all thought was good sandboxing) and collected $50,000 from Vercel and a few other vendors. That is not much money for something that could root every managed cloud provider in the world.

## spaceleans: a distributed unikernel operating system

About seven months ago, I went deep on unikernels to check whether my mental model was right. I showed Justin my Mirage folder, which holds all the functionality I needed to add.

- There was no way for a unikernel fleet to keep time, so I took an NTP client from another language and ported it. Then I built an NTP server based on RADclock and borrowed ideas from [how TigerBeetle handles time](https://tigerbeetle.com/blog/2021-08-30-three-clocks-are-better-than-one/?ref=ghuntley.com): not one clock source but many, packaged as a library.
- Network stack, DNS, HTTP clients and servers, structured logging, OTel, Anthropic and OpenAI clients, and payments via Airwallex.
- A generic retry library for handling back pressure over HTTP, plus some PPX metaprogramming for fun.
- A PII wrapper at the logging boundary, so secrets and PII never leak through the logging subsystem. Every project should have one. It's pluggable; just use a functor.

And then the most cooked thing, which I'd never shown anyone before: **Spaceleans**, [Microsoft Orleans](https://learn.microsoft.com/en-us/dotnet/orleans/overview?ref=ghuntley.com) ported to OCaml, running as a unikernel.

Orleans is a distributed actor system with transactions. You take many physical machines and merge them into one addressable heap. An actor always exists: `await GetCustomer()`, and if it isn't in memory, it gets rehydrated from a pluggable storage provider. You collapse your n-tier architecture into actors and stop caring whether something lives on machine A, B, C or D. The runtime handles it as an infrastructure primitive.

So in a weird sense, I built a distributed unikernel operating system out of actors, with a filesystem on top. Justin called it Erlang-esque, and he's right. I did all of it in a week. I'll probably never release it, but it falsified the idea that unikernels are hard.

## sampling history

Being a little older means you can sample history, like an experienced DJ such as Carl Cox, who's been in the scene long enough to pull from previous repertoire and bring it forward. All of these ideas existed in the eighties. The models have read the papers. They've got TAPL and the most advanced type theory in their training data.

> The thing that's lacking is people's curiosity and ambition to do these unhinged things, and the knowledge that previous records exist that can be sampled from.

How do we get people to try this stuff? We just do it. If you've got a turbo Lamborghini alien space rocket that's more efficient and more secure, good for you; you've got a leg up. Do cool things, attract curious newcomers, mentor them, grow. Same as it's always been.

Meanwhile, everyone else will be trying to Chainguard their Ruby on Rails application and managing AWS with fifty AWS-certified engineers, when two people with Nix and Hetzner would do. Eventually, it comes down to money. Higher-powered tools are more efficient, and efficiency wins, especially as AI collapses margins.

## ocaml, rust, haskell and back pressure

Has OCaml's time come again? It's still going strong. A certain trading firm is using it very well. When I caught up with Yaron at the start of the year, I asked him whether [OxCaml](https://oxcaml.org/?ref=ghuntley.com) exists so their language extensions end up in the training data and lift the whole company. I got a very "no comment" smile.

For agents, OCaml is lovely. Functors between modules are beautiful. The `.mli` files, a typed header explaining how a module should work, are really efficient context for agents. opam and Dune are legitimately good. Hindley–Milner. And compile times are fast. I see no reason to do F# these days.

Justin has mostly been writing Rust, and agents are good at Rust. But compile time is the tax on [back pressure](https://ghuntley.com/pressure/). LLMs hallucinate, and when compilation is slow, each hallucination is expensive because you get fewer attempts per minute. Justin's S3 clone is about a million lines of Rust; with four agents compiling at once, they fight over disk and CPU. You end up spending more on fast machines than on tokens.

Haskell's type system is great, and the models do it really well. But I don't feel good running it in production: a space leak lives in the runtime state space and only shows up in production. Justin pointed out that the linear-type ideas in Rust came out of Haskell papers trying to solve exactly that. Then there's Zig's approach: allocate everything up front and never allocate again, which is what game devs did in the eighties and nineties. It's hard to persuade an agent to do that in Rust, though, because constant-memory programs aren't in its training set.

I think dependent types are the winner for next-generation languages. Anything that lets you codify more into the type system is more back pressure. You probably won't be surprised to hear I've got a fork of the Rust toolchain with dependent types. You can just do things now.

## languages for agents, and what cursed taught me

The pace of language development has been held back by how fast humans can learn new concepts. Operator chaining is essentially sugar for humans. If agents write the code, we can lean on forty years of academic PLT research, as long as you know how to sample it.

The industry codified "do not make breaking changes" after Python 2 to 3. Justin knew companies with hundreds of people on that migration for years. I think that rule is no longer true. Ship a skill pack with the breaking change and let agents auto-migrate.

Justin asked what it actually costs to make a new language successful now. Go was the last language a company spent real money on, and it took a long time. I can answer that one.

[i ran Claude in a loop for three months, and it created a genz programming language called cursed](https://ghuntley.com/cursed/)

It's the only compiled language that lets you code with sus, slay and vibes.

Geoffrey Huntley Geoffrey Huntley

Cursed was built with Sonnet 3.5 and 3.7, a deliberately underspecified prompt, and three months of running it in a loop. I started in C (not enough back pressure; I wasted too much time in Valgrind as the agent clobbered its own updates), then Rust, then Zig. Zig was a mistake; it would work today if I'd stayed with Rust. It cost roughly US$6,000, and I did it three times over. Compare that to what Go cost.

Now the real bit, the part that still scares me. If you allocate the context window correctly — a lookup table of the lexical structure and grammar — the model can program in a language that isn't in its weights. It's brute force and inefficient, but it works.

Think T-diagrams (tombstone diagrams). Lock down your grammar and lexical structure, reach a stage-two self-hosting compiler, ship a sensible standard library, and start the next training run. From there, you can reach a Roslyn-style self-hosted compiler with language services stupidly fast. Justin asked whether fine-tuning an open model would help bootstrap a language like this. It's not needed.

That was true a year and a half ago with much weaker models. It'll take just one programming language designer going all in with the good models to shock the world.

## what next

As the pub was shutting, Justin asked what was on my mind. If you haven't read my latest post, go read it. If you manage people, create the space and time for them to experiment now, because within six months, leadership will ask you to put people on a vitality curve.

[if your team is too busy doing their 'normal job' to experiment with AI, you're preparing them to be replaced](https://ghuntley.com/replaced/)

AI use is now mandatory for employability.

Geoffrey Huntley Geoffrey Huntley

Yes, the labs trained on the commons. I hate that, and I get it. But you trade time and skill for money; employers have minimum standards, and those standards have changed faster than ever before in our industry. Be curious, learn how to build an agent, and go create beautiful stuff. We're in a renaissance.

It's a time-compression device. The more experience you have, the more you can sample. Not everything ships; some of what I showed Justin may never see the light of day. I use these projects as katas and redo them when the models get better.

But if you want to build something secure, seriously consider unikernels.

## chapters

- [0:21](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=21s&ref=ghuntley.com) — discovering unikernels: Haskell to OCaml to Mirage
- [0:55](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=55s&ref=ghuntley.com) — what a unikernel is, and why it was hard
- [2:45](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=165s&ref=ghuntley.com) — hard was past tense
- [3:33](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=213s&ref=ghuntley.com) — why do we have an operating system?
- [4:17](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=257s&ref=ghuntley.com) — the shell is a butler service for exfiltration
- [5:12](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=312s&ref=ghuntley.com) — attack surface reduction is fuzzy
- [7:26](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=446s&ref=ghuntley.com) — don't put the compiler on production
- [9:13](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=553s&ref=ghuntley.com) — porting Stripe into a unikernel
- [9:54](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=594s&ref=ghuntley.com) — minimal Linux and mkfs.xfs in Rust
- [12:22](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=742s&ref=ghuntley.com) — S3 for everything
- [13:48](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=828s&ref=ghuntley.com) — Nix, machine tests and overlays
- [15:28](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=928s&ref=ghuntley.com) — tool-calling a human is not AGI
- [15:53](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=953s&ref=ghuntley.com) — seL4 or unikernels
- [16:56](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=1016s&ref=ghuntley.com) — privilege rings
- [18:24](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=1104s&ref=ghuntley.com) — weekly kernel patches and the KVM escape
- [19:38](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=1178s&ref=ghuntley.com) — demo: the Mirage folder, NTP and time
- [22:05](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=1325s&ref=ghuntley.com) — Spaceleans: Orleans in OCaml as a unikernel
- [25:31](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=1531s&ref=ghuntley.com) — sampling history
- [26:52](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=1612s&ref=ghuntley.com) — how to get people to try this: just do it
- [28:57](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=1737s&ref=ghuntley.com) — OCaml and OxCaml
- [31:45](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=1905s&ref=ghuntley.com) — Rust compile times and back pressure
- [33:06](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=1986s&ref=ghuntley.com) — Haskell, space leaks and dependent types
- [35:13](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=2113s&ref=ghuntley.com) — memory strategies: Rust, Zig and game devs
- [36:10](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=2170s&ref=ghuntley.com) — languages designed for agents
- [37:41](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=2261s&ref=ghuntley.com) — breaking changes and skill packs
- [38:53](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=2333s&ref=ghuntley.com) — Cursed
- [42:36](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=2556s&ref=ghuntley.com) — what Cursed taught me
- [46:47](https://www.youtube.com/watch?v=DOjHBwdfBpA&t=2807s&ref=ghuntley.com) — what's next: AI use is mandatory

Keep curious.
