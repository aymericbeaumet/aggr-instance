---
title: 'parse_number() as a SQLite extension: C vs Rust'
link: https://simonwillison.net/2026/Nov/8/sqlite-parse-number-c-vs-rust/
source: simonwillison-net
published: 2026-11-08T22:26:00Z
updated: 2026-11-08T22:26:00Z
first_seen: 2026-11-08T22:46:45.931966426Z
labels:
- c
- claude
- research
- rust
- sqlite
summary: 'Research: parse_number() as a SQLite extension: C vs Rust I was out walking the dog when I got curious about how easy/difficult it would be to write a SQLite C extension that adds a parse_number() function for turning strings like -$1,234.56 or ''12.5%'' into numeric values that could be summed and sorted. I fired up regular Claude chat (Opus 5.5) and prompted: I''m thinking about a SQLite custom function ''parse_number(value)'' that strips '','', ''$'', ''£'' and ''%'', handles a leading ''-'', and returns ''NULL'' for anything it can’t parse Explore the difference between building this as a SQLite extension in C v.s. in Rust - I want to compare performance and code complexity and size of the resulting.so(or whatever) file This gave me a solid starting point - the Rust extension was 321 KB (using Rusqlite) and 298 KB (using sqlite-loadable-rs), while the C extension was 14 KB. But the Rust extension was faster! Turns out that''s because the Rust equivalent of C''s strtod is "twice as fast". So I prompted: Try writing your own C function for parsing text into a decimal number that catches up to or beats the Rust implementation And it did: Net cost per row over 2 million rows, lower is better: first C new C rusqlite sqlite-loadable Mixed data 67 ns 19 ns 46 ns 41 ns Integers only 25 ns 16 ns 36 ns 32 ns Decimals only 106 ns 18 ns 55 ns 47 ns Finally, I had it rewrite the prototype as a research report to match the others in simonw/research by prompting this: Clone simonw/research from GitHub and turn your above investigation into a research report as seen in that repository - you can give me a zip file to download that contains your new folder Here''s the resulting report. Tags: c, research, sqlite, rust, claude'
content: extracted
html: 2026-11-08-parse-number-as-a-sqlite-extension-c-vs-rust.html
---

[parse\_number() as a SQLite extension: C vs Rust](https://github.com/simonw/research/tree/main/sqlite-parse-number-c-vs-rust#readme) — Four SQLite extensions implement the same \`parse\_number()\` behavior in C and Rust, with tests covering type preservation, parsing rules, and floating-point accuracy. The optimized C implementation was fastest at 16–19 ns per row, about twice as fast as the Rust versions and far faster than SQL \`replace()\`. Plain C was slower on decimals because it relied on \`strtod()\`, while a compact fast path closed that gap; all versions matched across 8 million fuzz checks.

I was out walking the dog when I got curious about how easy/difficult it would be to write a SQLite C extension that adds a `parse_number()` function for turning strings like `-$1,234.56` or `'12.5%'` into numeric values that could be summed and sorted.

I fired up regular Claude chat (Opus 5.5) [and prompted](https://claude.ai/share/7f18f157-6c96-4d49-a1d4-f02403e3b261):

> `I'm thinking about a SQLite custom function 'parse_number(value)' that strips ',', '$', '£' and '%', handles a leading '-', and returns 'NULL' for anything it can’t parse`
>
> `Explore the difference between building this as a SQLite extension in C v.s. in Rust - I want to compare performance and code complexity and size of the resulting`.so`(or whatever) file`

This gave me a solid starting point - the Rust extension was 321 KB (using [Rusqlite](https://github.com/rusqlite/rusqlite)) and 298 KB (using [sqlite-loadable-rs](https://github.com/asg017/sqlite-loadable-rs)), while the C extension was 14 KB. But the Rust extension was faster!

Turns out that's because the Rust equivalent of C's `strtod` is "twice as fast". So I prompted:

> `Try writing your own C function for parsing text into a decimal number that catches up to or beats the Rust implementation`

And it did:

> Net cost per row over 2 million rows, lower is better:
>
> |               | first C | new C     | rusqlite | sqlite-loadable |
> | ------------- | ------- | --------- | -------- | --------------- |
> | Mixed data    | 67 ns   | **19 ns** | 46 ns    | 41 ns           |
> | Integers only | 25 ns   | **16 ns** | 36 ns    | 32 ns           |
> | Decimals only | 106 ns  | **18 ns** | 55 ns    | 47 ns           |

Finally, I had it rewrite the prototype as a research report to match the others in [simonw/research](https://github.com/simonw/research) by prompting this:

> `Clone simonw/research from GitHub and turn your above investigation into a research report as seen in that repository - you can give me a zip file to download that contains your new folder`

Here's [the resulting report](https://github.com/simonw/research/tree/main/sqlite-parse-number-c-vs-rust#readme).
