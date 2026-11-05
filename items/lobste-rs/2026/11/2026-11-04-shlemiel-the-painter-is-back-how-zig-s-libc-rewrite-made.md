---
title: 'Shlemiel the Painter Is Back: How Zig’s libc Rewrite Made SQLite Slower'
link: https://dennisforbes.ca/blog/microblog/2026/10/the_accidental_12000x_sqlite_slowdown/
source: lobste-rs
published: 2026-11-04T15:23:09Z
updated: 2026-11-04T15:23:09Z
first_seen: 2026-11-05T02:15:24.192048012Z
authors:
- dennisforbes.ca via ohrv
labels:
- performance
- rust
- zig
summary: Comments
content: extracted
html: 2026-11-04-shlemiel-the-painter-is-back-how-zig-s-libc-rewrite-made.html
remote_preview:
  url: https://dennisforbes.ca/include/images/og_dennis_forbes.jpg
---

## Spolsky Wisdom

Back in 2001, Joel Spolsky – whose writings had a big influence my career – wrote a great piece titled [Back to Basics](https://www.joelonsoftware.com/2001/12/11/back-to-basics/). The premise of the piece is that practitioners should understand the basics: However abstract and remote we may seem from the basic algorithms that drive computing, they can unexpectedly have enormous consequences on our projects.

One of his examples is the “Shlemiel the Painter” trap, where a seemingly simple choice leads to quadratic runtime complexity. In that parable, a street line painter never moves his can of paint. As he gets further and further along in the line, increasingly more of his time is spent walking to and from his can of paint.

It is astonishing how often this Shlemiel the Painter pattern recurs [1](https://dennisforbes.ca/blog/microblog/2026/10/the_accidental_12000x_sqlite_slowdown/#user-content-fn-firefox), including now in 2026.

## My Chess Analysis Service Staggers Under Load

A few days ago I posted about a [chess analysis service I put online](https://dennisforbes.ca/blog/microblog/2026/10/chess_analysis_service "Improving My Chess Abilities") ↗[2](https://dennisforbes.ca/blog/microblog/2026/10/the_accidental_12000x_sqlite_slowdown/#user-content-fn-motive). This is primarily a Rust service backed by sqlite that dishes out analysis tasks to runners. Each runner processes upwards of hundreds of chess games and return a large set of JSON bundles back to the main service, where they are processed and then stuffed into sqlite.

While the analysis pegs two dozen cores on a runner for minutes on end, the process of storing the data should take milliseconds. Sub-milliseconds.

Imagine my surprise when on the first really large set of games, the simple process of inserting the JSON data – maybe about 40,000 records – into the data store took *minutes*. During all of my testing this never really appeared as a critical path, but with a large set of records it suddenly was a breaking problem.

What made this more perplexing was that it completely saturated a core, being a CPU-bound operation. How could inserting simple JSON into SQLite yield minutes of CPU time?

Shlemiel the painter has made an appearance. To drop a spoiler, he was introduced to the project via Zig’s cc compiler and its replacement for standard library functions.

## A Minor Issue In Zig Becomes A Major Performance Issue

The project isn’t built in Zig, but instead uses Rust. The sqlx sqlite crate needs to be compiled for the target environment – sqlite is of course a C project – so to facilitate cross-platform building, generating both Mac ARM64 and Linux ARM64 / x86-64 binaries from this machine in a convenient build and deploy workflow, zig cc is used to compile sqlite when targeting Linux, generously offering up cross-platform targeting via cargo-zigbuild.

From one build and deploy script on my Mac I can target all of my platforms without containers or build machines. Zig’s elegant libzigc re-implements the standard library so you can static link the necessary functionality.

Zig uses musl (the standard library rewrite), but they have been rewriting it piece by piece in pure Zig (via libzigc), removing the C dependencies one at a time.

And that’s where the performance issue was introduced.

In the [rewrite of strspn](https://codeberg.org/ziglang/zig/src/commit/ae9a8acc7f232cb78a131b6dc05633f34d0990ad/lib/c/string.zig#L154-L157), [replacing the musl version with the pure zig variant](https://codeberg.org/ziglang/zig/commit/f0649dd9782780f5b608af338b6d4add19ce2730)), the first step measures the entire length of the string. This is also seen in the rewrites of strcspn, strpbrk and strstr.

This is a problem for tokenizers. A tokenizer takes a giant string and a common iterating step is to skip whitespace, getting the new string location and then repeating this pattern repeatedly. With this shlemiel the painter pattern introduced with these rewrites, as it steps through the text it is tokenizing it is remeasuring the entire length of the string endlessly.

This is what caused my performance issue, and it’s why it appeared on Linux but was never seen on my Mac (where zig wasn’t used for compiling sqlite).

Tokenizing a 1.2MB file, which was seen passing json to sqlite, is approximately 12,000x slower with this pattern. As the file gets larger, the slowdown is quadratic.

## The Classic Issues Recur

I mean no malice or disrespect to anyone involved with these great tools that enable so much. But it’s interesting that in the year 2026, these same old issues can recur, and we all need to be vigilant for them. Something so trivial was grinding a process to a halt, in the most unexpectedly trivial of places.

My original fix for this issue, at least in my own code, was to pre-prune whitespace from JSON, which greatly reduced the cost when tokenizing in sqlite3, but not knowing where else it might bite I moved my Linux builds over to Linux containers.

## Footnotes

1. Years and years ago, back when Sunspider was the go-to benchmark and browsers would herald their supremacy, the same pattern could be seen in the way Firefox handled string concatenations, causing it to be a laggard in that benchmark. Sunspider’s benchmark run somehow entailed an enormous number of string concatenations – even the tests having little to do with strings – and a simple profile [3](https://dennisforbes.ca/blog/microblog/2026/10/the_accidental_12000x_sqlite_slowdown/#user-content-fn-profilers) of a benchmark run showed that a significant amount of the runtime was consumed constantly allocating slightly larger string memory, copying to the new location, and then repeating this pattern. It was Shlemiel the Painter.

   I corrected this (overallocate strings with a size-dependent buffer such that subsequent concatenations could reuse the buffer until it actually need to be resized again, which again would overallocate), blogged about it, Brendon Eich took note, and voila Firefox took the lead in the benchmark [4](https://dennisforbes.ca/blog/microblog/2026/10/the_accidental_12000x_sqlite_slowdown/#user-content-fn-real). Such a trivial thing turned it from slowpoke to benchmark.

2. I’m under no delusions I’m going to support my family with a niche chess analysis service, and at most I’m looking to break even with the costs of running it. I put it online because I found my work to these ends useful to my own game, and hope that occasionally it will find the right user without burdening me with the costs of doing so.

3. Profilers are awesome. They are the most powerful tool we have available to us, so it’s strange to me how seldom they are utilized.

4. And for what it’s worth, string concatenation was extremely common in almost all web projects, so the benefit was substantial in the real world as well. There was a best practice of using string builders, but so often people just slammed strings together in a loop.

## See Also

- [Improving My Chess Abilities](https://dennisforbes.ca/blog/microblog/2026/10/chess_analysis_service)

  Sharing what I built to get better at the game
