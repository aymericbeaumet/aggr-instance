---
title: An explosion of transitive dependencies
link: http://notes.eatonphil.com/2025-01-25-an-explosion-of-transitive-dependencies.html
source: notes-eatonphil-com
published: 2025-01-25T00:00:00Z
updated: 2025-01-25T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
labels:
- c-sharp
- go
- java
- python
- rust
summary: 'A small standard library means an explosion in transitive dependencies. A more comprehensive standard library helps you minimize dependencies. Don''t misunderstand me: in a real-world project, it is practically impossible to have zero dependencies. Armin Ronacher called for a vibe shift among programmers and I think that this actually exists already. Everyone I speak to on this topic has agreed that minimizing dependencies is ideal. Rust and JavaScript, with their incredibly minimal standard libraries, work against this ideal. Go, Python, Java, and C# in contrast have a decent standard library, which helps minimize the explosion of transitive dependencies. Examples I think the standard library should reasonably include: JSON, CSV, and Parquet support HTTP/2 support (which includes TLS, compression, random number generation, etc.) Support for asynchronous IO A logging abstraction A SQL client abstraction Key abstract data types (BTrees, hashmaps, sets, and growable arrays) Utilities for working with Unicode, time and timezones But I don''t think it needs to include: Excel support PostgreSQL or Oracle clients Flatbuffers support Niche data structures Neither of these are intended to be complete lists, just examples. Walled gardens Minimal standard libraries force growing companies to build out their own internal collection of "standard libraries". As one example, Bloomberg did this with C++. And I''ve heard of companies doing this already with Rust. This allows larger companies to manage and minimize the explosion of transitive dependencies over time. All growing companies likely do something like this eventually. But again, smaller standard libraries incentivize companies to build this internal standard library earlier on. And the community benefits relatively little from these internal standard libraries. The community would benefit more if large organizations contributed back to an actual standard library. Smaller organizations do not have the capacity to build these internal standard libraries. Maybe the situation will lead to libraries like Boost for JavaScript and Rust programmers. That could be fine. Versioning A comprehensive standard library does not prevent the language developers from releasing new versions of the standard library. It is trivial to do this with naming like Go has done with the v2 pattern. math/rand/v2 is an example. Conclusion I''m primarily thinking about maintainability, not security. You can read about the security risks of using a language with an ecosystem like Rust from someone who is an expert on the matter. My concern about the standard library does not stop me from using Rust and JavaScript. They could choose to invest in the standard library at any time. We have already begun to see Bun and Deno to do exactly this. But it is clearly an area for improvement in Rust and JavaScript. And a mistake for other languages to avoid repeating. While zero dependencies is practically impossible, everyone I''ve spoken to agrees that minimizing dependencies is ideal. Rust and JavaScript work against this ideal. But they could change at any time. And Bun and Deno are already examples of this.https://t.co/qkSh6oW1Yd pic.twitter.com/mY1MNErZG7 — Phil Eaton (@eatonphil) January 25, 2025'
content: extracted
html: 2025-01-25-an-explosion-of-transitive-dependencies.html
---

A small standard library means an explosion in transitive dependencies. A more comprehensive standard library helps you minimize dependencies. Don't misunderstand me: in a real-world project, it is practically impossible to have zero dependencies.

Armin Ronacher [called](https://lucumr.pocoo.org/2025/1/24/build-it-yourself/) for a vibe shift among programmers and I think that this actually exists already. Everyone I speak to on this topic has agreed that minimizing dependencies is ideal.

Rust and JavaScript, with their incredibly minimal standard libraries, [work against this ideal](https://notes.eatonphil.com/2024-03-15-zig-rust-and-other-languages.html#standard-library). Go, Python, Java, and C# in contrast have a decent standard library, which helps minimize the explosion of transitive dependencies.

### Examples

I think the standard library should reasonably include:

- JSON, CSV, and Parquet support
- HTTP/2 support (which includes TLS, compression, random number generation, etc.)
- Support for asynchronous IO
- A logging abstraction
- A SQL client abstraction
- Key abstract data types (BTrees, hashmaps, sets, and growable arrays)
- Utilities for working with Unicode, time and timezones

But I don't think it needs to include:

- Excel support
- PostgreSQL or Oracle clients
- Flatbuffers support
- Niche data structures

Neither of these are intended to be complete lists, just examples.

### Walled gardens

Minimal standard libraries force growing companies to build out their own internal collection of "standard libraries". As one example, Bloomberg [did this](https://github.com/bloomberg/bde/wiki) with C++. And I've heard of companies doing this already with Rust. This allows larger companies to manage and minimize the explosion of transitive dependencies over time.

All growing companies likely do something like this eventually. But again, smaller standard libraries incentivize companies to build this internal standard library earlier on. And the community benefits relatively little from these internal standard libraries. The community would benefit more if large organizations contributed back to an actual standard library.

Smaller organizations do not have the capacity to build these internal standard libraries.

Maybe the situation will lead to libraries like Boost for JavaScript and Rust programmers. That could be fine.

### Versioning

A comprehensive standard library does not prevent the language developers from releasing new versions of the standard library. It is trivial to do this with naming like Go has done with the [v2](https://go.dev/blog/v2-go-modules) pattern. [math/rand/v2](https://go.dev/blog/randv2) is an example.

### Conclusion

I'm primarily thinking about maintainability, not security. You can read about the [security risks](https://medium.com/@john_25313/c-isnt-a-hangover-rust-isn-t-a-hangover-cure-580c9b35b5ce#:~:text=Rust%20makes%20it,for%20their%20libraries.) of using a language with an ecosystem like Rust from someone who is an expert on the matter.

My concern about the standard library does not stop me from using Rust and JavaScript. They could choose to invest in the standard library at any time. We have already begun to see [Bun](https://bun.sh/docs/api/s3) and [Deno](https://jsr.io/@std) to do exactly this. But it is clearly an area for improvement in Rust and JavaScript. And a mistake for other languages to avoid repeating.

> While zero dependencies is practically impossible, everyone I've spoken to agrees that minimizing dependencies is ideal. Rust and JavaScript work against this ideal. But they could change at any time. And Bun and Deno are already examples of this.[https://t.co/qkSh6oW1Yd](https://t.co/qkSh6oW1Yd) [pic.twitter.com/mY1MNErZG7](https://t.co/mY1MNErZG7)
>
> — Phil Eaton (@eatonphil) [January 25, 2025](https://twitter.com/eatonphil/status/1883162142888853945?ref_src=twsrc%5Etfw)
