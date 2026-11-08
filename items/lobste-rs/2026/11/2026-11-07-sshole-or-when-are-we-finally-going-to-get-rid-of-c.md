---
title: '*sshole, or when are we finally going to get rid of C?'
link: https://sunshowers.io/posts/sshole/
source: lobste-rs
published: 2026-11-07T21:05:35Z
updated: 2026-11-07T21:05:35Z
first_seen: 2026-11-08T22:46:45.931966426Z
authors:
- sunshowers.io via novedevo
labels:
- rant
summary: Comments
content: extracted
html: 2026-11-07-sshole-or-when-are-we-finally-going-to-get-rid-of-c.html
remote_preview:
  url: https://sunshowers.io/images/4-aaaaaa-card.jpg
---

![The meme of a happy person and a sad person on a bus, the sad person captioned with '4' and the happy person with a string of 257 As.](https://sunshowers.io/images/4-aaaaaa.jpg)\
Original art by Genildo Ronchi.

Yet another day, [yet another vulnerability](https://sshhole.com/) in core C code. This one happened due to two different conditions using the same sentinel value:

- On Unix, a password hash of `"*"` has meant a locked account since the very early days.
- Since 2012, musl libc’s `crypt` function has returned the string `"*"` for a password that’s too long for it to hash.

The two points combined, along with sshd hashing login attempts for locked accounts to avoid timing leaks, mean that on musl systems without PAM (like, oh, *the official Alpine AWS image*), an unauthenticated user now gets a (possibly root!) shell on your computer. See [this Alpine advisory](https://www.alpinelinux.org/posts/2026-11-05-openssh-password-authentication-bypass/) for more details.

This is not a memory safety vulnerability; rather, it is one of those issues that, people often say, languages with better type systems won’t save you from. This is true! You can very well write a Rust function that returns `"*"` if the password is too long. But the big difference between C and Rust in this regard is that *the norms of the Rust community do not allow that*.

The POSIX spec tries to account for error cases. It [says](https://pubs.opengroup.org/onlinepubs/9799919799/functions/crypt.html):

> Upon successful completion, crypt() shall return a pointer to the hashed password; \[…\] Otherwise, it shall return a null pointer and set errno to indicate the error.

But the C type system can’t force developers to check that the return value is NULL. The musl libc developers saw that some binaries weren’t checking for it, and as a result [deviated](https://git.musl-libc.org/cgit/musl/commit/?id=b3c4cc121f70faea45389fe7ddc1127ed5cbd8bb) from the spec, returning the sentinel `"*"` value instead.

As I’ve [written about before](https://sunshowers.io/posts/types/), type systems have a strong effect on engineering culture. Because C doesn’t have algebraic data types or result types, the engineering norms that form around C communities lead to this kind of in-band signaling. Meanwhile, in Rust, a password that’s too long would be returned as an error variant, and the caller would be compelled to handle it. What if there isn’t an error variant today? Then a breaking change would add an error variant, *and* a [RustSec advisory](https://rustsec.org/) would probably be published as well.

As an aside, it remains surprising to me when Linux distro maintainers complain about projects introducing Rust dependencies. Rather than adapting to the new world where the engineering norms are likely to serve users better, these maintainers insist on the world adapting to them. Yes, compile times and portability to niche architectures are real costs, but are you here to serve your own interests or the bulk of your users’? There’s a real [principal-agent problem](https://en.wikipedia.org/wiki/Principal%E2%80%93agent_problem) going on here.

## Agents won’t save you

Speaking of agents! There’s a recent strain of argument that goes roughly like this:

- Rust is admittedly useful for humans to write correct software today.
- LLMs can now, or in the near future will be able to, reason about memory safety and other error cases directly.
- So in an agent-heavy development lifecycle there’s no difference in software rigor between Rust and C (or even assembly).
- But C has certain advantages over Rust in terms of build speed, portability, etc.
- So we might as well write software in C (or even assembly).

There is plenty that’s wrong about the argument, including the dubious decision to trust a fuzzy process when we have already shown that a far more deterministic approach works. But the big missing piece is the norms of the respective communities! When you (or your agent) write Rust software, you’re probably going to use Rust libraries, and your agent is probably going to pattern-match on existing Rust code in its training set. As a result, *even if you aren’t already a skilled Rust developer*, there will likely be more invariants encoded in the type system[^1].

The engineering norms of C developers allow for `"*"` to be an acceptable sentinel value indicating an error; the engineering norms of Rust developers simply do not. These norms are the result of the accumulated contributions of a community, and part of the responsibility of a software professional is to think about the kinds of communities one’s work is a part of. But some people have [decided](https://bsky.app/profile/sunshowers.io/post/3mzkaurqkrk2v) that none of this really matters in this shiny new world, and that the contributions of other people can mostly be ignored. Their hubris will be their downfall.

*Discuss on [Hacker News](https://news.ycombinator.com/item?id=50399400).*

[^1]: If you’re a skilled Rust developer, you’ll likely find the first-pass invariants the agent encodes to be woefully inadequate, and you’ll probably go through a long review cycle where those kinds of issues are fixed.
