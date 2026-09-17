---
title: Developing provably correct Rust code with Verus
link: https://www.amazon.science/blog/developing-provably-correct-rust-code-with-verus
source: hnrss-org-frontpage
published: 2026-09-14T16:57:50Z
updated: 2026-09-14T16:57:50Z
first_seen: 2026-09-17T12:46:46.439381799Z
authors:
- Betelbuddy
summary: 'Article URL: https://www.amazon.science/blog/developing-provably-correct-rust-code-with-verus Comments URL: https://news.ycombinator.com/item?id=49700153 Points: 129 # Comments: 24'
content: extracted
html: 2026-09-14-developing-provably-correct-rust-code-with-verus.html
preview:
  file: 2026-09-14-developing-provably-correct-rust-code-with-verus.preview-e51ce63d9d6e.webp
  width: 256
  height: 144
  alt: Verus-16x9.gif
  color: '#d7ded2'
images:
- source: https://cdn.amazon.science/dims4/default/d5f871c/2147483647/strip/true/crop/1200x630+0+23/resize/1200x630!/quality/90/?url=https%3A%2F%2Famzn-science-production-science.s3.us-east-1.amazonaws.com%2Fscience%2F05%2Fce%2Fe48c2a0b4372889838ed019562f3%2Fverus-16x9.gif
  original:
    file: 2026-09-14-developing-provably-correct-rust-code-with-verus.image-7ef80329ecea.gif
    width: 1200
    height: 675
  color: '#e3e5e1'
---

Many open-source and industry software projects, including [several](https://www.amazon.science/blog/how-prime-video-updates-its-app-for-more-than-8-000-device-types) here [at Amazon](https://www.amazon.science/blog/ec2s-formally-verified-isolation-engine-provides-mathematical-assurance-of-virtual-machine-isolation), are embracing the Rust programming language, since it provides performance and flexibility similar to that of the C programming language, while its clever type system automatically prevents a variety of bugs and security vulnerabilities. The result is fast code that's more correct and secure than average.

However, "more correct and secure" is not the same as "actually correct and secure". For example, in C, accessing an array out of bounds — indexing into an array past the boundary of the memory allotted to it — is a dangerous mistake that can have unforeseeable consequences. In Rust, it will halt the program, which is definitely safer, but a correct program would never perform the out-of-bounds access in the first place. Similarly, Rust cannot guarantee that your program will compute the results you were expecting or that it won't leak the secrets it has access to. That's where Verus comes in.

Accessing an array out of bounds is a dangerous mistake that can have unforeseeable consequences. A correct program would not permit it.

## What is Verus?

[Verus](https://github.com/verus-lang/verus) is an open-source, automated program verifier for Rust. A "program verifier" takes in a formal mathematical specification of how your code should behave and mechanically checks that your code matches that specification for all possible inputs.

For example, your code might implement an optimized binary-search algorithm to look for a particular value within a sorted array. The specification might state that when the code successfully returns an index, the corresponding element in the array matches the target value. The verifier checks that this specification holds for all possible input arrays and target values.

In contrast, traditional testing techniques might try a few specific arrays but can miss corner cases (e.g., what if the target value is the last element in the array or not present at all?). A key aspect of program verification involves constructing a mathematical *proof* that the code matches its specification. In an *automated* program verifier like Verus, the tool automatically handles many of the boring, low-level steps of proof construction, while the human developer provides high-level guidance (e.g., setting up an inductive proof or supplying a loop invariant). As we discuss below, these days, even the high-level steps can often be automated by AI.

At Amazon, we're proud to have been a founding member of the Rust Foundation, and we use Rust extensively for projects like [Firecracker](https://www.amazon.science/tag/firecracker), which powers AWS Lambda and AWS Fargate, our serverless distributed SQL database, and the [Nitro Isolation Engine](https://www.amazon.science/blog/ec2s-formally-verified-isolation-engine-provides-mathematical-assurance-of-virtual-machine-isolation), which enforces virtual-machine isolation for the Nitro hypervisor, the software that manages virtual-machine allocation for Amazon Web Services (AWS). Amazon's excitement about Rust, combined with more than a [decade of work on automated reasoning](https://aws.amazon.com/blogs/security/an-unexpected-discovery-automated-reasoning-often-makes-systems-more-efficient-and-easier-to-maintain/), makes it natural to adopt Verus to provide even stronger guarantees for the Rust code we're writing. Indeed, we've used Verus to prove the correctness of key primitives used by the Nitro Isolation Engine, as well as a number of critical pieces of infrastructure used within Amazon. We'll explore these use cases in future posts, but for now, we want to tell you more about what it means to verify Rust code with Verus.\

## Verifying Rust code with Verus

With Verus, a Rust developer can add specifications (and proofs) for existing Rust code directly in the Rust source files. To extend the binary-search example, consider the following Verus specification (written as a Rust annotation) of the search function's existing Rust implementation:

A Verus specification of a search function's Rust implementation, written as a Rust annotation.

The *precondition* (indicated by the “requires” keyword) states the conditions that must be true before the function executes. In this case, since the code implements a binary search, we require that the array is sorted. The *postcondition* (indicated by the “ensures” keyword) states the conditions that must be true after the function executes. In this case, it says that if the function returns “Some(index)”, then “index” is within the bounds of the array, and the value at that index matches the value we were looking for.

Importantly, it also tells us that if the function returns “None”, then the target value is *not* in the array. Without this second clause, the specification could be satisfied by an implementation that always returned “None”! Note that normal Rust compilers ignore these Verus annotations, so Verus-annotated code can be consumed by both verified and unverified projects, including those that use Rust's build tool, Cargo.

This example also illustrates a key design decision that Verus makes, one that distinguishes it from many other Rust verification approaches. With Verus, *developers* write specifications and proofs in their source code, using Rust-like syntax. When a proof fails, they see Rust-style error messages expressed at the source level. This approach keeps the proofs in sync with the actual code and saves developers from needing to learn a brand-new language and tool for specifications and proofs. It also enables the developers who write the code (and hence know it best) to be involved in the process of proving it correct.

Verus also focuses on providing fast, powerful automation. To do so, it uses a variety of *solvers* to discharge the proof obligations generated from the programs and their specifications. In practice, this means that developers typically get feedback on their code and proofs in under a second, fast enough to provide an interactive development loop (including "red squiggles" inside interactive development environments like VS Code).

At the project level, Verus can verify complex projects with thousands of lines of code and proof in the time it took some prior automated program verifiers to verify individual functions. This powerful automation and quick feedback loop obviously help humans, but they also help AI agents develop Verus proofs, since the automation means the agent has less work to do and can iterate faster on its proofs.

Rust's type system provides strong safety guarantees, but sometimes it prevents developers from writing high-performance code. Hence, Rust also allows developers to write explicitly labeled "unsafe" code. This code must still uphold all of Rust's expectations for safe code, but the compiler no longer mechanically checks those expectations; it's up to the developer to get it right. With Verus, however, developers can mathematically prove the safety of their unsafe Rust code, re-establishing machine-checked safety guarantees.

Similarly, Rust famously offers "fearless concurrency", meaning that the type system will prevent various mistakes that other programming languages allow when developers write concurrent code — i.e., programs that execute in parallel at least part of the time. Verus builds on this foundation to enable developers to prove that their concurrent code is not just safe but correct.

For example, concurrent execution generally involves *locks*, which grant a processor thread exclusive access to data items it’s currently manipulating. Verus allows developers to add an *invariant* property to a lock, meaning that anyone who acquires the lock obtains a value that satisfies the invariant's property (e.g., the value is always even), and when they release the lock, they must prove that the value behind the lock still satisfies that property. Moreover, Verus supports proofs that the lock implementation itself is correct. This is particularly important for programs like the Nitro Isolation Engine, which rely on complex, custom locking schemes to achieve high performance.

Like all program verifiers, Verus's guarantees rely on the correctness of Verus itself, the "top-level" specifications of the program's intended behavior, the "bottom-level" assumptions made about the underlying run-time (e.g., the Rust standard library), and the compiler toolchain that converts source code into executable programs. In future posts, we'll go into more detail on the ways we increase our confidence in these components.\

## Verus in the open-source ecosystem

In addition to its use at Amazon, Verus has been used to prove interesting properties for a variety of open-source projects. Here are some examples:\

- [Vest](https://github.com/secure-foundations/vest) takes in a description of a binary data format and automatically generates Rust code to parse and serialize data in that format, including Verus proofs of correctness and security.
- [Verdict](https://github.com/secure-foundations/verdict) provides a provably correct and secure certificate validation library for the x.509 public-key cryptography standard, one that supports user-supplied validation policies.
- The [CapybaraKV project](https://github.com/microsoft/verified-storage) verifies the correctness and crash safety of *persistent-memory logs*, which preserve data in a well-formed state even if the system crashes or loses power unexpectedly.
- The [Atmosphere microkernel](https://mars-research.github.io/projects/atmo/) is a microkernel (minimal operating system) developed in Rust and verified for correctness with Verus.
- [Anvil](https://github.com/anvil-verifier/anvil) proves the correctness and “liveness” of controllers for Kubernetes, an open-source system for managing cloud computing. Anvil shows that under reasonable assumptions, the controllers will eventually bring the system into a stable state.
- The [CortenMM](https://dl.acm.org/doi/10.1145/3731569.3764836) memory management system includes a novel transactional interface with scalable locking protocols, and the correctness of its concurrent code is verified with Verus.

Verus itself is a free, open-source project developed by a distributed collaboration of academic and industrial researchers.
