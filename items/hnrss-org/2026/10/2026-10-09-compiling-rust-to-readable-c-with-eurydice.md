---
title: Compiling Rust to readable C with Eurydice
link: https://lwn.net/Articles/1055211/
source: hnrss-org
published: 2026-10-09T23:28:36Z
updated: 2026-10-09T23:28:36Z
first_seen: 2026-10-10T15:51:15.675508634Z
authors:
- peter_d_sherman
content: extracted
html: 2026-10-09-compiling-rust-to-readable-c-with-eurydice.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50027853
  num_comments: 33
  points: 112
---

> **Please consider subscribing to LWN**
>
> Subscriptions are the lifeblood of LWN.net. If you appreciate this content and would like to see more of it, your subscription will help to ensure that LWN continues to thrive. Please visit [this page](https://lwn.net/Promo/nst-nag1/subscribe) to join up and keep LWN on the net.

A few years ago, the only way to compile Rust code was using the rustc compiler with LLVM as a backend. Since then, several projects, including [Mutabah's Rust Compiler](https://github.com/thepowersgang/mrustc?tab=readme-ov-file#mutabahs-rust-compiler) (mrustc), [GCC's Rust support](https://lwn.net/Articles/1040197/) (gccrs), [rust\_codegen\_gcc](https://lwn.net/Articles/907405/#rust_codegen_gcc), and [Cranelift](https://lwn.net/Articles/964735/) have made enormous progress on diversifying Rust's compiler implementations. The most recent such project, [Eurydice](https://github.com/AeneasVerif/eurydice?tab=readme-ov-file#eurydice), has a more ambitious goal: converting Rust code to clean C code. This is especially useful in high-assurance software, where existing verification and compliance tools expect C. Until such tools can be updated to work with Rust, Eurydice could provide a smoother transition for these projects, as well as a stepping-stone for environments that have a C compiler but no working Rust compiler. Eurydice has been used to compile some post-quantum-cryptography routines from Rust to C, for example.

Eurydice was started in 2023, and includes some code under the MIT license and some under the Apache-2.0 license. It's part of the [Aeneas](https://aeneasverif.github.io/) project, which works to develop several different tools related to applying formal verification tools to Rust code. The various [Aeneas projects](https://aeneasverif.github.io/projects/) are maintained by a group of people employed by [Inria](https://en.wikipedia.org/wiki/French_Institute_for_Research_in_Computer_Science_and_Automation) (France's national computer-science-research institution) and Microsoft, but they do accept outside contributions.

Eurydice follows the same general structure as many compilers: take a Rust program, convert it into an intermediate representation (IR), modify the IR with a series of passes, and then output it as code in a lower-level language (in this case, C). Jonathan Protzenko, the most prolific contributor to Eurydice, has [a blog post](https://jonathan.protzenko.fr/2025/10/28/eurydice.html) where he explains the project's approach. Unlike other compilers, however, Eurydice is concerned with preserving the overall structure of the code while removing constructs that exist in Rust but not in C. For example, consider this Rust function that calculates the least common multiple of two numbers using their greatest common denominator:

```
    fn gcd(a: u64, b: u64) -> u64 {
        if b == 0 {
            a
        } else {
            gcd(b, a%b)
        }
    }

    fn lcm(a: u64, b: u64) -> u64 {
        (a * b) / gcd(a, b)
    }
```

Here's how Eurydice compiles those functions to C:

```
    uint64_t example_gcd(uint64_t a, uint64_t b)
    {
        uint64_t uu____0;
        if (b == 0ULL)
        {
            uu____0 = a;
        }
        else
        {
            uu____0 = example_gcd(b, a % b);
        }
        return uu____0;
    }

    uint64_t example_lcm(uint64_t a, uint64_t b)
    {
        uint64_t uu____0 = a * b;
        return uu____0 / example_gcd(a, b);
    }
```

Whether this C code counts as "readable" is probably a matter of individual taste. It does, however, preserve the structure of the code. Even the evaluation order of the original is preserved by adding extra temporary variables (`uu____0` in `example_lcm()`) where necessary to define an order. (Rust guarantees that if the multiplication overflows and causes a panic, that will happen before any side effects caused by calling `example_gcd()`, but C only guarantees that if the multiplication is performed in a separate statement.) Compiling the same functions with rustc results in a pair of entangled loops filled with bit-twiddling operations, instead — which is appropriate for machine-code output, but much less readable.

Of course, not all Rust programs can be faithfully represented in C. For example, `for` loops that use an iterator instead of a range need to be compiled to `while` loops that call into some of Eurydice's support code to manage the state of the iterator. More importantly, C has no concept of generics, so Rust code needs to be monomorphized during conversion. This can result in several different implementations of a function that differ only by type — often, the more idiomatic C approach would be to use macros or `void *` arguments.

The implementation of dynamically sized types also poses certain challenges. In Rust, a structure can be defined where one of its fields does not have a fixed size — like flexible array members in C:

```
    struct DynamicallySized<U: ?Sized> {
        header: usize,
        my_data: U, // The compiler does not know the size of U, here
    }
```

But if that structure is generic, and one of the generic users of the type gives the flexibly sized field a type with a known size, the compiler can take advantage of that knowledge to elide bounds checks where appropriate.

```
    let foo: DynamicallySized<[u8; 4]> = ...;
    // No bounds check emitted, since the array size is known to be 4:
    let bar = foo.my_data[2];
```

This kind of separation, where some parts of the code may know the size of a type and some may not, is an important semantic detail to preserve in C because of how it interacts with the possibility of formal verification. If Eurydice compiled `DynamicallySized` to use a flexible array member everywhere, analysis of the C code might point out "missing" bounds checks that were not required in Rust. Conversely, if Eurydice added extra bounds checks, it would need to manufacture extra error paths that don't appear in the Rust source and that should be completely unused.

So, Eurydice emits two different types: a version of the dynamically sized type that has a flexible array member, and one that has a known-length array member. Converting between the two representations is a no-op at run time, but it technically violates C's strict-aliasing rule. Therefore Protzenko recommends compiling Eurydice-generated code with `-fno-strict-aliasing`.

#### Associated tooling

This approach, of compiling a more abstract language to C in a way that preserves the structure of the code, is not new. The [KaRaMeL](https://github.com/FStarLang/karamel/?tab=readme-ov-file#karamel) project, upon which Eurydice is based, does the same thing for the [F\*](https://fstar-lang.org/) programming language. F\* is a dependently typed functional programming language used to develop cryptographic libraries. Compiling provably correct F\* programs to equivalent C lets those libraries be used in programs where performance is a concern.

Unfortunately, Eurydice doesn't currently scale much beyond small examples. Rather than implement its own parser and typechecker for Rust code, Eurydice uses another Aeneas tool — [Charon](https://github.com/AeneasVerif/charon/?tab=readme-ov-file#charon) — to extract the parsed and preprocessed program from rustc. When I tested Charon on a variety of Rust packages, it was routinely foiled by more recent Rust features such as [const generics](https://doc.rust-lang.org/reference/items/generics.html#const-generics).

When Charon does work, however, it dumps rustc's medium-level intermediate representation (MIR) as JSON, along with any compiler flags necessary to understand the compilation. Eurydice reads this JSON representation and converts it to KaRaMeL's intermediate representation. Then it uses a series of small passes over the KaRaMeL code to eliminate some Rust-specific details, before handing things over to the same code-generation logic that KaRaMeL uses for F\*.

In its current form, Eurydice works best for small, self-contained programs that avoid complex Rust features. Within that niche, however, it works well. The generated code maintains the same structure as the original Rust code, except for places where Eurydice emits extra intermediate variables or needs some glue code to implement a more complicated feature. On the other hand, small self-contained code is also the easiest to rewrite by hand, so bringing in Eurydice is probably only worthwhile if the original Rust code is going to be updated and one wants an automatic solution to keep them in sync. In any case, Eurydice is only the newest tool in a rapidly expanding collection of ways to fold, spindle, and mutilate Rust code to fit into more environments.

\[ Thanks to Henri Sivonen for the topic suggestion. \]
