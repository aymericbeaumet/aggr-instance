---
title: Four Months of Zig
link: https://gracefulliberty.com/articles/four-months-of-zig/
source: hnrss-org
published: 2026-10-27T20:13:27Z
updated: 2026-10-27T20:13:27Z
first_seen: 2026-10-28T11:34:18.317588229Z
authors:
- ibobev
content: extracted
html: 2026-10-27-four-months-of-zig.html
remote_preview:
  url: https://gracefulliberty.com/avatar.png
---

2026-10-27 · 15 min · [#computing](https://gracefulliberty.com/articles/tags/computing)

It has been about four months since I [returned to Zig](https://gracefulliberty.com/articles/return-to-zig) after being absent for a few years. In this time, I have written some software in Zig and reengaged with the community, online and in person. I decided to share some of that experience, including what I have liked about Zig so far and what I haven't. I'll include some comparisons to Rust along the way since that was my previous go-to language.

## Projects

The first project I started in Zig was [snurl](https://codeberg.org/liberty/snurl). It is a library for interacting with the smallnet (Gemini, Finger, Gopher, Spartan, and such). My goal has been to provide an alternative to curl for the smallnet, with similar testing and fuzzing requirements but on a smaller scale. I wanted to roughly follow [TigerStyle](https://tigerstyle.dev/) insofar as it made sense for the project.

Something I ran into early on was that the fuzzer didn't actually work. When getting excited about fuzzing in my last post, I didn't realize that the feature had not yet been completed. This isn't surprising, considering Zig's status, but it was disappointing, especially since the fuzzer is demonstrated in the generated `main.zig` file provided by `zig init`.

After this, I wrote a two programs which I have not published yet. The first is a small TUI typer racer. It was simpler than I expected to get a minimum version working so far. The second is an n-body simulator with stellar evolution using raylib.

## Zig Day

The most exciting opportunity in the last few months was the chance to go to a local Zig Day. I learned of Zig Day Seattle occurring on [Lobsters](https://lobste.rs) on August 20th. I was ecstatic to learn that there was a Zig Day in my area. Then I saw Andrew Kelley would be there and became more excited.

Zig Days are local meetups where people interested in Zig hang out, work on their projects together, and talk about Zig. For this Zig Day, I decided to work on snurl. A friend, [Evin Yulo](https://yujiri.xyz) also decided to attend.

The event itself occurred in the Bothell library, moved there from the Bellevue library after so many people signed up that capacity was overwhelmed. Having the creator of Zig at the event will do that.

I love the format of these Zig Days and I'm glad they are a part of the community. Since they are run by local groups, it gives the whole Zig community a more interconnected feeling. It's a way to interact with other people who are interested in Zig in person. And, in this case, the creator of Zig itself.

I got the chance to talk with other programmers about their projects. Hopefully some ideological cross-pollination occurred that will make all our projects stronger. I ended up working on snurl with evin, who started work on integrating Zig's standard library TLS implementation as alternative backend.

I also briefly talked with Andrew Kelley. I hope Zig's built-in fuzzing gets fixed sometime soon!

Ryan Hendrickson [wrote an excellent article](https://www.ryanjh.com/articles/my-first-zig-day/) outlining more of the events of that day from his perspective if you want more details.

I will definitely be attending Zig Day Seattle again in the future. I may even head down to Portland to catch one before the next local one comes around. I enjoy the opportunity to connect with other people interested in Zig and its values.

## What I've Liked

### anyzig

I love [anyzig](https://github.com/marler8997/anyzig). I've had to use zig master and 0.16.0 for different projects, and anyzig has made that doable.

Unfortunately, it breaks outside of directories without a `build.zig.zon`, meaning if you have tooling that requires a `zig` command to work, it can only occur in project directories. This breaks autoformatting for my one-off Zig scripts. Maybe a default zig version could be provided in that case.

### IO and Allocators

I know it frustrates some people, but I love the fact that you have to pass IO and allocator parameters to functions in Zig.

#### Allocators

I'll start with allocators, since they've been around longer. For each Zig function you have to accept an explicit allocator parameter. This is different from most programming languages which either use a default allocator that may be changed globally or one changed by a local `context` (see Odin).

I like being able to change allocators for different parts of code. You might want an arena allocator for some things but need a general purpose allocator for others. Sometimes a `FixedBufferAllocator` is all you need to perform an operation. Zig's style makes this all obvious to handle.

I prefer Zig's system to Odin's implicit `context` system since Zig keeps things explicit. It's clear whether a function allocates memory or not, and hard to accidentally pass in the wrong allocator.

This is also superior to Rust's hidden, infallible allocations that panic when memory is exhausted. They're working on adding explicit allocations, but this will be yet another feature tacked onto the language rather than something built from the ground up.

It's much easier to reason about programs that allocate memory in Zig.

#### IO

The IO parameter is even more powerful, even if it is newer. It is well worth the breaking changes required to bring it about (even if I skipped the 0.15.0 -> 0.16.0 transition). In Zig, every function that interacts with the underlying operating system (with caveats) requires an "IO" parameter. This provides the context for input, output, and everything else. Porting Zig to new operating systems is mostly just porting IO.

Practically, this is really nice for testing. When testing behavior, you just need to put in a testing IO instance that works as you need it to. You can change the input stream however you want, cause writes to fail, and anything else.

Just like with allocation, passing an IO argument makes it clear which programs are pure and which ones are not. You can tell at a glace whether or not a program is going to modify something external.

Zig has a really good track record of keeping things simple while also adding powerful features. Instead of adding 100 small features that do slightly different similar things, it tends to provide one major feature that supports all the various other use-cases. Low floor, high ceiling.

### Comptime

I love comptime. It's a seamless way to do metaprogramming and precomputation all in one. Other people have written more detailed accounts so I shall move on.

### Build System

[Zig's build system](https://ziglang.org/learn/build-system/) is stellar. So much so that companies like Uber have adopted it for their software written in other languages.

One thing I like about it is that I can integrate other tools with it seamlessly in ways that Cargo complains about. This is especially important for one of my projects that involves code generation.

### Dependency Culture

Zig has an excellent dependency culture. The main ethos is simple: avoid. When you don't need a dependency, don't add one. However, when you do, it's often a battle-tested library that Zig can effortlessly integrate with translate-c. You can easily also vendor in external dependencies and audit their code.

Most of my Zig programs have about one or two direct Zig dependencies, with zero indirect dependencies. This makes the codebase much easier to audit and control.

This stands in stark contrast to Rust, where the smallest of my projects regularly have a few direct and dozens of indirect dependencies.

This is thanks both to Zig's dependency culture and the fact that essential features like reflection, random number generation, clean error handling, and JSON parsing are built into the language or standard library rather than maintained as separate individual libraries. No need to depend on serde or syn in every program.

Zig encourages careful consideration before adding dependencies, and I appreciate that.

### Handling Errors Effortlessly

Zig pushes programmers to handle errors correctly by default. The laziest way to handle errors in Zig is to use the `try` keyword and add `!` to the function's return type.

This contrasts to Rust, which leads programmers to insert panics into their code. The issue is that it's often easier to panic when prototyping, and then leave that in the code longer than it should be. I've used production code written in Rust that panic instead of returning a proper error.

In Rust you can use `?`, which is quicker than `unwrap`, but you also have to do something like `Result<Type, Box<dyn std::error::Error>`. That's a mouthful! Alternatively, you can use the anyhow crate and return `anyhow::Result`, which also provides some nice helper methods. But then you have to add a dependency for reasonable error handling. There's also Zig-style [composable errors](https://mcmah309.github.io/posts/the-missing-piece-in-rust-error-handling/) with yet another library.

### Incremental Compilation

The core team members at Zig have been doing amazing work at incremental compilation. If you run `zig build --watch -fincremental`, the compiler will watch for changes, check exactly what you changed, and then modify only what you've changed down to linking. No effort beyond what's required, so it's faster that human perception even for large codebases. Zig was designed from the ground up to support this behavior.

This makes for a really nice workflow. I can just run the compiler in incremental mode right next to my text editor, and get updates on my code as I save. No time to get distracted with something else. With Rust projects 10+ seconds incremental compilation loops were common even in smaller projects. That's enough time to get distracted but not enough time to be productive doing something else, leaving annoying gaps that Zig eliminates.

Rust is disadvantaged here. It's a stable language that needs to maintain backwards compatibility. This means it can't afford to break things in order to improve compilation speed, at least not to any large degree. Meanwhile, if it makes Zig better, the maintainers are willing to break just about anything to ensure the end result will be perfect.

Currently, Zig only supports incremental compilation on x86\_64 on Linux and macOS. If you use another operating system or architecture you won't have access to this feature yet, but in that case it's another thing to look out for!

This makes Zig just a bit more wonderful to work with. I'll take it.

## What I'd Like to See

### Improved Stability

Like every other user of Zig, I would like to see the standard library become more stable. I know that's one of the last tasks to do before 1.0, and there are many others before then, but it is frustrating to deal with the constant breakage in the ecosystem. My understanding is that that's why 0.17.0 took so long to be released.

Andrew Kelley said at Zig Day that he would like to see 1.0 more than anyone. I believe him, so I will have patience and trust that the final version will be excellent.

This is the one reason I still use Rust for some projects. Some cases require a stable programming language and the ecosystem that provides. But considering the impressive software that's being built in Zig these days, stability isn't everything. And there are projects for which the niceties of Zig outweigh its instability. I can't wait for 1.0.

### Lifetime Support

I would like to see more assistance with lifetimes in the Zig compiler. Specifically, I would like to see returning local variables to fail in debug mode, either as a kind of built-in lint (as Andrew Kelley has mentioned wanting to implement) or as literally invalidating that memory after the function returns. That way new users quickly learn to avoid that pattern. Currently, it's easy to unintentionally break your code this way, even if you think you understand lifetimes.

Relatedly, the compiler currently accepts code that will be broken in the future. The compiler lets variables live the entire scope instead of the scope of a block. This means that if you stack-allocate a buffer and return it from a block, that buffer should be invalidated but isn't. My understanding is that this could silently break in a future Zig version.

```zig
// This works but will be invalid.
const foo = blk: {
	var buf: [1024]u8 = undefined;
	const n = try writeToBuf(&buf);
	break :blk buf[0..n];
};
```

### More Documentation

Zig is poorly documented. It has enough to get you started, but to continue onward you have to parse intention from sparse doc comments that are sometimes attached to functions. Examples are rare. I still haven't got Zig's standard library TLS working in snurl because neither I nor a friend can figure out how to set up the buffers correctly.

Because of this lack of documentation, a lot of the knowledge for building solid Zig programs is distributed across the Web. If you're well integrated into Zig communities, having read the existing documentation, watched the presentations, and read the code, you can succeed. If you aren't that well integrated, however, it can be a challenge to work effectively.

The benefit of this is that it encourages people to engage in [Zig communities](https://ziglang.org/community) (I'm an admin for the Matrix room, for example). While LLMs seek to demolish human collaboration, we need human-to-human collaboration now more than ever. However, we can still have that and nice documentation too. As we all know, people will still ask questions if you have things well-documented. And a good community knows how to point people towards good resources rather than just saying "RTFM".

I would also be curious to see what the intersection of Zig and literate programming could be. My literate programming tool is written in Rust right now, but that might change.

### Comptime Allocators

I would love to parse JSON at compile time. For one of my projects, there is a JSON file that is stored inside the binary. It's static and could be parsed at compile time into a collection of structs.

However, the standard library JSON parser requires an allocator. Because there are no allocators that work at compile time, this means that any JSON parsing must be done at runtime. So that's what I did with my project.

I believe there is an intention of adding a JSON parser that does not require an allocator (the data already exists in the JSON string, after all). That will solve the immediate problem, but there are more general cases that may require an allocator. I believe `FixedBufferAllocator` worked at compile time before and served this purpose. It could do so again.

### Better Bootstrapping

Zig's bootstrapping story is impressive. [They erased the C++ version of their compiler and replaced it with a small WASM blob and WASM interpreter](https://ziglang.org/news/goodbye-cpp/). The WASM file is a stripped-down version of the compiler that compiles the full Zig compiler to C, which is then compiled by the system C compiler. It's an interesting approach and you should read the linked article if you want more details.

The one caveat to this is that it requires the compiled binary blob checked into the source code. This is better than requiring the full previous compiler (like Rust), but it's not as good as having the full source available for every step of the process at all points. A malicious actor could still sneak something into the blob. More importantly, many distributions (like Debian) cannot properly package Zig at the moment because it's impossible to do so in a way that's compliant with their guidelines.

Their original plan was to use the compiler's C backend to generate a C version of the compiler which would then be maintained separately. They would then have two different source versions. They decided this approach would be too difficult, unfortunately.

Because of this, someone will have to make another Zig compiler in another language (probably C) that will improve the bootstrap process. This is explicitly not something the Zig Software Foundation wants to develop right now. This means that at the moment there is no way to bootstrap Zig fully from source.

This issue isn't just a theoretical problem. Because Zig can't be packaged on Debian, I had to choose another language for one of my projects to ensure wider possible usage.

Fortunately, this issue is temporary. Once Zig is stable, building an alternative compiler should be straightforward. Zig is designed to be a simple language where alternative implementations are humanly achievable, unlike [other languages](https://rust-lang.org/). Currently, the instability means any alternative compiler will be playing a constant game of catch-up, which is probably why the core team doesn't want to manage it while they're rapidly iterating.

Alas, this may also require waiting for 1.0. May it come soon!

## Moving Forward

I will continue to use Zig. I'm excited about the direction it's going and I look forward to continuing to be a part of its community. So far, I've found no other programming language that I prefer more than Zig, even with its shortcomings. It's just stable enough that I don't have to make massive changes between releases. It will continue to be my go-to programming language for most projects.

One of these projects is [ReTangled](https://codeberg.org/liberty/retangled). It was previously written in Rust but I grew tired of the massive dependency trees and want to take full advantage of Zig's build system.

I've also been looking to contribute more to Zig directly. Between my limited time as a student, Zig's overflow of contributors already, and the poor documentation, this will be a challenge. But I want to contribute back, make an impact on the project, and become a better software engineer. Thankfully, improving contributor onboarding seems to be a priority for Andrew Kelley moving forward.

I'll also be at future [Zig Days](https://zig.day/) in Seattle, if you want to meet and work on Zig projects together!
