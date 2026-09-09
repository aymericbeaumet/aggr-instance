---
title: Replacing a Rust Enum with a 64-Bit Word Made My Interpreter 17% Faster
link: https://pointersgonewild.com/2026-08-25-replacing-a-rust-enum-with-a-64-bit-word/
source: hnrss-org-frontpage
published: 2026-09-05T12:32:06Z
updated: 2026-09-05T12:32:06Z
first_seen: 2026-09-09T06:36:48.510480730Z
authors:
- metrofun
summary: 'Article URL: https://pointersgonewild.com/2026-08-25-replacing-a-rust-enum-with-a-64-bit-word/ Comments URL: https://news.ycombinator.com/item?id=49575914 Points: 117 # Comments: 45'
content: extracted
html: 2026-09-05-replacing-a-rust-enum-with-a-64-bit-word-made-my.html
preview:
  file: 2026-09-05-replacing-a-rust-enum-with-a-64-bit-word-made-my.preview-42872f349ab9.webp
  width: 256
  height: 134
  color: '#1c2427'
images:
- source: https://pointersgonewild.com/2026-08-25-replacing-a-rust-enum-with-a-64-bit-word/cover.png
  original:
    file: 2026-09-05-replacing-a-rust-enum-with-a-64-bit-word-made-my.image-254c066ebe73.png
    width: 2400
    height: 1260
  variants:
  - file: 2026-09-05-replacing-a-rust-enum-with-a-64-bit-word-made-my.image-f2b9f738d226.webp
    width: 48
    height: 25
  - file: 2026-09-05-replacing-a-rust-enum-with-a-64-bit-word-made-my.image-a0b2f28967e0.webp
    width: 320
    height: 168
  - file: 2026-09-05-replacing-a-rust-enum-with-a-64-bit-word-made-my.image-a6af00c31864.webp
    width: 640
    height: 336
  - file: 2026-09-05-replacing-a-rust-enum-with-a-64-bit-word-made-my.image-b70ca2ccccfa.webp
    width: 960
    height: 504
  - file: 2026-09-05-replacing-a-rust-enum-with-a-64-bit-word-made-my.image-1ef924f05099.webp
    width: 1280
    height: 672
  - file: 2026-09-05-replacing-a-rust-enum-with-a-64-bit-word-made-my.image-5d103f9a3d9e.webp
    width: 2400
    height: 1260
  color: '#0b0e14'
---

August 25th, 2026

![](https://pointersgonewild.com/2026-08-25-replacing-a-rust-enum-with-a-64-bit-word/cover.png)

This blog post is the sixth in a series about my work building and optimizing the [Plush](https://github.com/maximecb/plush) language interpreter and virtual machine. The previous one was [Speeding Up the Plush Garbage Collector](https://pointersgonewild.com/2026-08-17-speeding-up-the-plush-garbage-collector/). In the last post, I explained how a few simple changes made the copying GC over 16x faster, and brought the collection time for a million objects down to around 7 ms. I'm having a lot of fun optimizing Plush just for the sake of it, but I'm also doing it with the goal in mind of being able to make the language fast enough to render 3D animations in real-time, even though it's interpreted.

Plush is a dynamically-typed language, in the same family as Python, JavaScript, Ruby, Lua, and Lox. Dynamic languages like this have the property that types are attached to values rather than variables, and so to propagate values around programs, an interpreter typically has a `Value` type that can represent any value that could exist in the language. What I did with the original version of Plush is that I used a plain Rust tagged enum. This is nice because Rust makes working with tagged enums very convenient, as we can dispatch to different `Value` subtypes using the `match` statement:

```
// The old Rust Value type as a tagged enum
enum Value {
    Undef, // Uninitialized var or field, reading triggers an error
    Nil,
    False,
    True,
    Int64(i64),
    Float64(f64),
    String(*const Str),        // Immutable string
    HostFn(&'static HostFn),   // Function exposed by host VM
    Fun(FunId),                // Non-closure Plush function
    Closure(*mut Closure),     // Closure that captures variables
    Cell(*mut Value),          // Mutable variable captured by a closure
    Object(*mut Object),       // Class instances
    Array(*mut Array),         // JS/Python style array/list
    ByteArray(*mut ByteArray), // Fast raw byte array (e.g. frame buffer)
    Dict(*mut Dict),           // JS/Python style dict
    Class(ClassId),
}
```

As you can see above, Plush, even though I still consider it a toy language, has many different value types. The language has objects which are class instances, which are efficient to access, but it also has JS/Python style dictionaries, which make JSON-style syntax possible. There are also two distinct numerical types, `Int64` and `Float64`. I made this choice because it always kind of bothered me that JavaScript pretends everything is a double, while JS engines will actually keep track of what's an integer behind the scenes. The thing that's most unfortunate though is not the number of enum variants here, it's that this enum is a whole 16 bytes (128 bits) wide. Each enum variant needs only 64 bits, and the enum tag that Rust creates only needs 8 bits, but because of memory alignment constraints, Rust may need to use a whole 128 bits for each value. It might seem like no big deal, but if you have a large array of values, that array will end up with a ton of empty, wasted bytes inside of it. This is the kind of thing that makes VM engineers cry themselves to sleep at night.

For a little while now, I've been thinking that I could design a more efficient low-bit tagging scheme to make it so that the `Value` type fits inside of 64-bits. There's a [classic trick](https://www.chiark.greenend.org.uk/doc/sbcl/sbcl-internals/Type-tags.html) which is derived from the fact that on a 64-bit system, heap object addresses are typically aligned to 8-byte boundaries, which means that the lowest 3 bits of the address must be zero. That means you can essentially steal these bits to pack extra information in there. You can also borrow the two lowest bits of integer values, with the assumption that integer values will very rarely need to use the full 64-bit range, because for reference `2^64 ~= 1.84 * 10^19`. That's a very large value. If you have a variable that represents say, the number of lines in a text file, or the number of enemies in your game, or any other numerical quantity, it's very unlikely to reach that value. With a modern CPU that can dispatch multiple instructions per clock cycle, if you were to execute a loop such as `for (uint64_t i = 0; i < UINT64_MAX; ++i)`, the loop would likely take over a decade to finish executing.

A more sophisticated tagging scheme can clearly reduce memory usage, but it also means that we have to introduce bitwise operations to be able to tell what's an integer, a pointer or a float. We also need extra bitwise operations to unpack some values to operate on them. That means extra instructions the CPU has to run. A skilled VM engineer once told me that the smart thing to do is to give integers zeros as their tag bits, because then, adding or subtracting two shifted integers remains a plain `add` or `sub` machine instruction. Packing and unpacking floats though is more complex and requires several instructions. I was a bit worried about the performance impact, and needing to trade memory efficiency for a bit of a performance loss. As it turns out, my fear was completely unfounded, as we'll see later in this post.

## An Efficient Low-Bit Tagging Scheme

Claude and I co-designed and iterated on the value representation found in this [source file](https://github.com/maximecb/plush/blob/6b71f8c4bbf7cf234c289ee2b979f16195daead1/src/value.rs). It fits nicely in a Rust newtype wrapping a `u64`. As you can see, it has many convenience methods to make it easy to work with, compensating for the loss of the Rust enum. Most of the methods are marked as always inline for performance, since they're used everywhere in the interpreter loop. The diagram below illustrates how the value representation is structured in more detail:

[![The tagged value representation.](https://pointersgonewild.com/2026-08-25-replacing-a-rust-enum-with-a-64-bit-word/value-repr.svg)](https://pointersgonewild.com/2026-08-25-replacing-a-rust-enum-with-a-64-bit-word/value-repr.svg)

The tagged value representation.

The low-bit tagging scheme I settled on encodes 5 different kinds of values: fixnums, flonums, immediates, and two kinds of pointers. Fixnums are signed integers that fit in a 62-bit range. Flonums are floating point values encoded using a self-tagging scheme (more on that later). Immediates are values like `nil`, `true`, `false`, `undef`, function and class ids, as well as host functions written in Rust that can be called from Plush. In Plush, you can call host functions using the dollar sign prefix, e.g. `$read_file(file_name)`. The immediates have a 5-bit subtag to indicate what kind of immediate they are. It's 5 bits because an 8-bit comparison on the lowest byte is just one instruction on modern CPUs, which means we can compare the subtag plus the tag bits in one instruction if we need to.

The reason there are two kinds of pointers in this representation is that I wanted to make equality comparison between values fast. This comes up often because you might have loop conditions, pointer comparisons inside if statements, etc. Most values in Plush are compared using simple reference equality, but I chose to have strings be compared for structural equality to allow the possibility of a [string interning](https://en.wikipedia.org/wiki/String_interning) table later. This is like JS, where two strings with the same value are equal. Floats also need special handling because they have a sign bit, and `+0.0` must be the same as `-0.0`.

The easy way to do that is to have one bit in the tag (I chose bit index 1) that tells us that two values can be compared directly using one comparison instruction. This allows us to have a fast path to compare integers, pointers, handles and small immediate constants like `true`, `false`, `nil` and `undef` using a single machine comparison instruction. There's some minor subtlety there because some pointer types (string objects) are not comparable using direct pointer equality. Also, if an integer or float value exceeds the range we can fit in a tagged value, we have to allocate a heap object to box this value. That means we could end up with two floats that have equal value represented by distinct heap objects, but those values must test as equal using the `==` operator.

If Plush eventually gets a JIT compiler, the generated code for `if (p != nil)` can be as follows. There is no slow path here, because we know that no value that gets compared structurally can possibly be equal to `nil`:

```
; x0 = the value being tested
; nil is the immediate 0x05
cmp     x0, #5
b.eq    .ELSE_BRANCH  ; jump to else branch if equal
```

## Efficient Fixnum Operations

As stated earlier, integer values that fit in the 62-bit fixnum range have their lowest two bits set to 00, and are stored as `n << 2`, so add, sub, compares and bitwise and/or/xor run directly on the tagged words. A 64-bit overflow is also exactly the case where the result no longer fits in 62 bits, meaning we can use standard machine instructions to check for overflow (`jo` on x86-64, `bvs` on ARM64).

For a loop counter comparison like `i < n`, the ARM64 machine code can be as follows. Checking that both values are integers costs us 3 machine instructions:

```
; Fast path for `i < n`, 5 instructions
; x0 = i, x1 = n
orr     x2, x0, x1    ; combine both operands
tst     x2, #3        ; are both operands fixnums?
b.ne    .Lslow_lt     ; floats, boxed numbers and strings go that way

cmp     x0, x1        ; compare the fixnums, `00` tag bits don't matter
b.ge    .LOOP_EXIT    ; signed compare, fixnums are 62-bit signed
```

The fast path to add two fixnums would come out as follows. The Rust compiler may not be able to combine both tags in practice, but a JIT compiler easily could, and could even remove the type check entirely if it has additional run-time information about the types of values, in which case the add would be just as efficient as a C integer addition:

```
; Fast path for fixnum integer addition
; 5 instructions on the fast path
; x0 = a, x1 = b, two tagged Values
; A fixnum is n << 2, so its low two bits are 00
orr     x2, x0, x1      ; combine both tags, so one test covers the pair
tst     x2, #3          ; are both operands fixnums?
b.ne    .Lslow

adds    x0, x0, x1      ; (a << 2) + (b << 2) == (a + b) << 2
b.vs    .Loverflow      ; signed overflow is exactly the 62-bit overflow

; x0 already holds the tagged result. Nothing was untagged on the way in
; and nothing has to be retagged on the way out
```

## Self-Tagged Flonum Representation

There are many ways to tag floating-point values in a system with tagged values. One of the most famous schemes for doing that is [NaN boxing](https://craftinginterpreters.com/optimization.html#nan-boxing), which, as far as I know, is still used in the Firefox JavaScript engine to this day. NaN boxing exploits the fact that a double counts as [NaN](https://en.wikipedia.org/wiki/NaN) (not a number) whenever its 11 exponent bits are all set to one and its mantissa is non-zero. Every bit pattern that satisfies that reads as a NaN, no matter what the rest of the word holds. For 64-bit doubles, that means there are 52 mantissa bits that can be used as a payload to store any kind of data you want, or 51 in practice, since implementations normally reserve the top one for quiet NaNs. The Crafting Interpreters book also mentions that you can steal the topmost sign bit as an extra tag. It's a clever trick, and it has the advantage that you get double-precision operations "for free", more or less, no tagging or untagging necessary. The disadvantage is that other types of operations are penalized a little bit more. The practical reality is also that your typical JavaScript, Python or Ruby program does not do very much floating-point math. It's a lot more integer and pointer heavy.

For Plush, I decided to investigate other avenues for tagging floats which could work with low-bit tagging. I already knew that one possible approach was to steal some exponent bits from IEEE doubles. The range that doubles can represent is incredibly huge, with the maximum representable value being approximately `1.8 * 10^308`. It stands to reason that most values you typically see in a computer program never approach that range, and so, if we were to borrow a few bits of the exponent, we'd be left with a smaller, but still huge representable range for tagged flonums. The doubles we can't fit in that range, we can simply heap-allocate, but that should almost never be needed.

During my search into how to do this efficiently, I stumbled upon a fairly recent paper on [Float Self-Tagging](https://arxiv.org/pdf/2411.16544) by Olivier Melançon, Manuel Serrano and Marc Feeley. The scheme they present does a rotation of the upper bits so they end up in the tag bit position, after adding a bias value such that the lower bits end up being the specific tag that we want. It's clever, and it means we don't need shifting, masking, or anything else. Best of all, tagged flonum values can also represent subnormals, infinity and even NaNs without needing heap boxing, which is very convenient as these values do come up in some computations. For my boxing scheme, I picked a bias value such that the lowest two bits end up being `10`. That `1` in bit index 1 indicates that doubles can't be directly compared for equality. Amazingly, we only lose two bits of exponent, and we keep the full precision of the mantissa, meaning we lose no significant digits in the flonum representation.

[![Boxing and tagging floats using self-tagging, from Float Self-Tagging (arXiv:2411.16544).](https://pointersgonewild.com/2026-08-25-replacing-a-rust-enum-with-a-64-bit-word/flonum.svg)](https://pointersgonewild.com/2026-08-25-replacing-a-rust-enum-with-a-64-bit-word/flonum.svg)

Boxing and tagging floats using self-tagging, from Float Self-Tagging (arXiv:2411.16544).

Adding two flonums costs more than adding two fixnums, because we do need to do a bit of extra work to undo and redo the tagging:

```
; 18 instructions on the fast path, with BIAS already in a register
; x0 = a, x1 = b, two tagged Values
; x9 = BIAS, 0x6810_0000_0000_0000, materialised once
and     x2, x0, #3      ; isolate the tag bits of a
and     x3, x1, #3      ; and the tag bits of b
cmp     x2, #2          ; a flonum has 10 in its low two bits
ccmp    x3, #2, #0, eq  ; only look at b if a was one
b.ne    .Lslow

ror     x2, x0, #4      ; undo the rotate
sub     x2, x2, x9      ; undo the bias, leaving the IEEE 754 bits
fmov    d0, x2
ror     x3, x1, #4
sub     x3, x3, x9
fmov    d1, x3

fadd    d0, d0, d1      ; the actual f64 add

fmov    x2, d0          ; back to bits
add     x2, x2, x9      ; + BIAS
ror     x0, x2, #60     ; rotate left by 4: ARM64 only rotates right
and     x3, x0, #3      ; isolate the tag bits of the result
cmp     x3, #2          ; did the tag land on 10, or does this one box?
b.ne    .Lbox
```

The last instructions of the machine code snippet above test whether the lowest two bits ended up being `10`. If they didn't, that means we got a value in one of the two narrow bands of magnitudes that our encoding doesn't cover. This should be extremely rare in practice, but if it does happen, we have a slow path that simply allocates a heap box and moves on. In Plush, I'm using a bump allocator so heap boxing is actually reasonably fast too.

## Impact on Memory Usage

I have a collection of several synthetic benchmarks for Plush. Some of them are tiny microbenchmarks designed to test the performance of a very specific feature, others are a bit broader. For example, `fib` is the recursive Fibonacci numbers microbenchmark (Marc Feeley's favorite); it primarily benchmarks function calls and interpreter throughput. The `binary_tree` benchmark has both recursive function calls and object property accesses, as well as pointer-chasing and traversing heap-allocated objects. The `mlp` benchmark is a naively implemented multilayer neural network, with a lot of floating-point operations. I also have a `sha256` computation, and the classic `nbody` celestial body simulation in there. Recently, I also added a triangle rasterization benchmark (not shown here), to get an idea of what I could do with [3D graphics in Plush](https://www.youtube.com/watch?v=ElwU9PAvpRM).

The numbers shown below compare `ac75356`, the last commit before the refactor, which introduced the `Value` newtype but still has the old enum underneath, against `6b71f8c`, the refactor itself. Those two commits are adjacent, so nothing else changed in between. Experiments were done on my MacBook Air M5, with 7 interleaved rounds.

[![Peak RSS per benchmark (lower is better).](https://pointersgonewild.com/2026-08-25-replacing-a-rust-enum-with-a-64-bit-word/memory.svg)](https://pointersgonewild.com/2026-08-25-replacing-a-rust-enum-with-a-64-bit-word/memory.svg)

Peak RSS per benchmark (lower is better).

The most important thing to note is that there's a bimodal distribution. Some benchmarks show a big reduction in memory usage, as much as 37% on the `mlp` benchmark which uses a lot of arrays, while others don't really move. The benchmarks that don't move are simply not allocating much memory at all. They're using the minimum Plush GC heap size, and have a peak RSS of about 10 MB. The main takeaway is that on benchmarks that do use more memory than the baseline, we see a nice reduction. Well, that's not fully true. Two benchmarks actually showed an increase in memory usage, those are `quicksort` and `sha256_unfixed`. In the case of `quicksort`, the peak RSS measurement caught the GC in the middle of a collection cycle, so this is kind of an unfortunate outlier. However, `sha256_unfixed` shows an actual regression, a case where many boxed integers ended up being generated, and then pointers to these values were stored into an array, keeping the boxed integers live.

I went ahead and looked at `sha256_unfixed`, and found that the computation was structured in a way that a left shift caused an integer overflow. It's considered a sin in compiler engineering to modify benchmarks, but in this case, I wanted to know if this could be avoided. What if we optimized the computation for our 62-bit fixnums to make the best use of our VM? It turns out that the computation can easily be rearranged to avoid this. It's a one-line fix, and brings memory usage back all the way down to the minimum heap size. So, this is a real downside of heap-boxed representations. We're making an engineering tradeoff, and in some cases, if we're not paying attention, we could end up with code that's less memory-efficient. Though in practice, we probably win in the vast majority of cases.

## Impact on Performance

The graph below shows the speed of the interpreter with the new low-bit tagged value representation vs the old Rust enum, with numbers above 1.0x indicating a performance gain:

[![Speedup per benchmark (higher is better).](https://pointersgonewild.com/2026-08-25-replacing-a-rust-enum-with-a-64-bit-word/speedup.svg)](https://pointersgonewild.com/2026-08-25-replacing-a-rust-enum-with-a-64-bit-word/speedup.svg)

Speedup per benchmark (higher is better).

The results in the above graph surprised me. I expected some slowdowns, but in actuality, every single benchmark is faster, and some are faster by a lot. There are also more interesting things to notice in these results. For instance, we see that the updated `sha256_fixed` benchmark is faster than `sha256_unfixed`. That being said, `sha256_unfixed` is still running 12% faster with the new value representation than with the old version, even though it's allocating a total of 3,149,052 boxed integer values in a span of about 490 ms. That seemed a bit crazy to me, but having a copying GC with a bump allocator means that we can allocate objects really fast. The positive thing there is that if we do need to box some integers or floats, the performance penalty is not as much as I would have expected, at least not in an interpreted system where instruction dispatch overhead dominates.

But wait, there's more. I mentioned earlier that the `mlp` benchmark is a multilayer neural network. It's doing matrix multiplication with tagged floats, and a lot of them. The `nbody` benchmark is also floating-point heavy. Those are also clearly faster, even though every floating-point operation has to unbox two values and re-box the result. This again surprised me. We're left with the conclusion that everything is faster, even when dealing with some of the worst possible cases of our new tagged representation. We can clearly call this refactoring a success, but it's a surprising conclusion, at least to me.

So, why is everything so much faster? One thing we can see is that the biggest performance gains are in the `binary_tree` and `linked_list` benchmarks. That makes sense. These benchmarks have a large number of objects and do a lot of pointer chasing, touching many cache lines all over the place. With the new representation, objects take about half the amount of memory that they needed before. That means about half the amount of cache/memory traffic. Cache-friendliness is a big win when you're touching a lot of memory.

However, there are other benchmarks like `fib` and `nbody` which don't touch a lot of memory at all. Everything clearly should fit in the L1 cache. The latency of the L1 cache is just 1 to 4 clock cycles on modern processors. Still, the L1 cache, even though it's fast, isn't instantly accessible. It's not as fast as CPU registers, so maybe there are some small wins to be gained from the fact that even interpreter stack frames, and the few objects that `nbody` has, have shrunk. I also have another suspicion, however.

Before the `Value` type refactoring, I was using a tagged Rust enum and match statements to dispatch based on types in various interpreter operations. I suspect that the code the Rust compiler generates for this is maybe not very efficient. I placed the types I thought would be most likely to happen near the top of the match statement, but that probably means nothing to `rustc`. In contrast, the way the code is formatted now, we're explicitly testing for the likely hot path first in an `if` statement. If the code `rustc` generated for the `match` statement dispatch is potato, that would explain a lot.

This is the way my interpreter handled the `add` instruction in the old tagged enum version (`ac75356`, src/vm.rs:1498) using match arms to dispatch based on `Value` types:

```
Insn::add => {
    let mut v1 = pop!();
    let mut v0 = pop!();

    let r = match (v0, v1) {
        (Int64(v0), Int64(v1)) => Int64(v0 + v1),
        (Float64(v0), Float64(v1)) => Float64(v0 + v1),
        (Int64(v0), Float64(v1)) => Float64(v0 as f64 + v1),
        (Float64(v0), Int64(v1)) => Float64(v0 + v1 as f64),

        (Value::String(s0), Value::String(s1)) => {
            // ...string concatenation, elided
        }

        _ => error!("add", "unsupported operand types")
    };

    push!(r);
}
```

Looking at the disassembly for the `Int64 + Int64` fast path, it's spread over four disjoint basic blocks. The code is fairly massive, and it has a bunch of spills to the native C/Rust stack as well as memory accesses from the interpreter stack:

```
; ---- block A @ 0x1a8cc : pop v1, then pop v0 -----------------------
ldr   x10, [x20]              ; len = stack.len()
cbz   x10, .Lunderflow
sub   x11, x10, #1
str   x11, [x19, #0x60]
ldr   x8,  [x19, #0x50]
cmp   x11, x8
b.hs  .Lpanic
ldr   x12, [x19, #0x58]       ; stack base pointer
add   x13, x12, x11, lsl #4   ; &stack[len-1], note the 16-byte stride
ldr   w9,  [x13]              ; v1: the tag
ldr   w14, [x13, #0xc]        ; v1: payload bytes 12..16
ldur  x13, [x13, #0x4]        ; v1: payload bytes 4..12, unaligned
str   w9,  [sp, #0x2b8]       ; spill v1 into a stack slot...
ldr   x15, [sp, #0x48]        ; ...whose address is itself in a stack slot
str   x13, [x15]
str   w14, [x15, #0x8]
cbz   x11, .Lunderflow
sub   x23, x10, #2
str   x23, [x20]
add   x11, x12, x23, lsl #4   ; &stack[len-2]
ldr   w10, [x11]              ; v0: the tag
ldr   w12, [x11, #0xc]
ldur  x11, [x11, #0x4]
str   w10, [sp, #0x2d0]       ; spill v0 the same way
ldr   x13, [sp, #0x50]
str   x11, [x13]
str   w12, [x13, #0x8]

; ---- the match dispatch --------------------------------------------
ldr   x22, [sp, #0x2d8]       ; reload v0's payload we just spilled
ldr   x0,  [sp, #0x2c0]       ; reload v1's payload we just spilled
cmp   w10, #4                 ; is v0 an Int64?
b.eq  .Lv0_int                ; taken, 0x120c bytes away
cmp   w10, #5                 ; a Float64?
b.eq  .Lv0_float
cmp   w10, #6                 ; a String?
b.ne  .Ltype_error

; ---- block B @ 0x1bad8 ---------------------------------------------
.Lv0_int:
cmp   w9, #4                  ; is v1 an Int64?
b.eq  .Lint_int               ; taken, 0x5a4 bytes away

; ---- block C @ 0x1c07c ---------------------------------------------
.Lint_int:
adds  x22, x22, x0            ; the actual addition
b.vs  .Lpanic_add_overflow
mov   w24, #4                 ; result tag = Int64
b     .Lpush                  ; taken, 0x2c0 bytes away

; ---- block D @ 0x1c33c : push! -------------------------------------
.Lpush:
cmp   x23, x8
b.eq  .Lgrow
ldr   x8, [x19, #0x58]
add   x8, x8, x23, lsl #4
str   w24, [x8]               ; store the tag
str   x22, [x8, #0x8]         ; store the payload
add   x8, x23, #1
b     .Ldispatch
```

This is the `add` instruction and its fast path with the new tagged word version presented in this post (`6b71f8c`, src/vm.rs:1391):

```
Insn::add => {
    let v1 = pop!();
    let v0 = pop!();

    // Tagged fixnums add as they are, and a 64-bit
    // overflow is exactly the case where the sum no
    // longer fits in one
    if v0.is_fixnum() && v1.is_fixnum() {
        if let Some(sum) = (v0.raw() as i64).checked_add(v1.raw() as i64) {
            push!(Value::from_raw(sum as u64));
            continue;
        }
    }

    flonum_op!(v0, v1, +);

    let r = slow!("add", self.add_slow(v0, v1));
    push!(r);
}
```

The disassembly for the fast path is shown below:

```
; ---- block A @ 0x19cc0 : pop v1, pop v0, test the tags -------------
ldr   x10, [x5]               ; len = stack.len()
cbz   x10, .Lunderflow
sub   x8, x10, #1
str   x8, [x5]
ldr   x9, [x16]
cmp   x8, x9
b.hs  .Lpanic
cbz   x8, .Lunderflow
ldr   x9, [x28, #0x58]        ; stack base pointer
ldr   x3, [x9, x8,  lsl #3]   ; v1, a single load, 8-byte stride
sub   x10, x10, #2
str   x10, [x5]
ldr   x2, [x9, x10, lsl #3]   ; v0, a single load
and   x11, x2, #3             ; v0's tag bits
cmp   x11, #2                 ; a flonum?
b.ne  .Lcheck_fixnum

; ---- block B @ 0x1a624 ---------------------------------------------
.Lcheck_fixnum:
cmp   x11, #0                 ; is v0 a fixnum? x11 is still v0 & 3
and   x11, x3, #3
ccmp  x11, #0, #0, eq         ; ...and v1 too? one branch covers both
b.ne  .Lslow
adds  x11, x2, x3             ; the actual addition, on the tagged words
b.vc  .Lpush

; ---- block C @ 0x18030 : push! -------------------------------------
.Lpush:
str   x11, [x9, x10, lsl #3]  ; a single store
str   x8,  [x5]               ; stack.len -= 1
; and then it falls straight through into the interpreter dispatch
```

As we can see at first glance, the disassembly for the new version is much shorter. The code for the match dispatch itself in the old version was actually fine, but the old version made a bunch of spills and stack memory accesses. The real win is that values now fit in a single register. The generated code is a lot more efficient as a result. We're not spilling values to the native C/Rust stack immediately after popping them from the interpreter stack. You may also have noticed that LLVM independently found the trick we discussed earlier in the post to check that two values are fixnums, by fusing two type tests into a single branch, nice!

I'm not going to show the whole floating-point fast path disassembly here because it's even longer, but it's worth pointing out that the old version did zero tagging and untagging work, whereas the new one has to unbox both operands and re-box the result. Despite that, the new float fast path is straight-line code with exactly one branch at the end, and it actually ends up shorter than the old version. We went from 52 instructions, 24 memory ops and 4 branches taken to 36 instructions, 9 memory ops and 1 branch taken. This solves the `mlp` and `nbody` puzzle.

## Conclusion

In conclusion, I'm pretty happy with the way this refactoring went. Not only does the new tagged word representation reduce memory usage for benchmarks that use lots of arrays and objects, but it's also a major performance win, with every single benchmark ending up faster. The Rust compiler was simply not able to generate efficient code with the old version, but with the new version where values fit in a single register, we actually get some fairly good generated code.

The need to heap-box integers can cause some extra memory allocations in some cases. In particular, code that does left shifts, or relies on overflows for things like generating random numbers, can run into that. Those issues can easily be avoided by skilled engineers who take the VM's design and limitations into account. However, despite some values ending up boxed in some cases, the performance overhead of boxing is unlikely to ever be noticeable. If you're designing your own programming language, you could make different design choices, such as making your native integer type a 32-bit or 62-bit integer instead of a 64-bit integer. This would simply remove the boxed integer path, and make it so overflows produce a visible error instead. You also could require users to explicitly use a heap-boxed bignum (big number) type when they need extra precision. It's a big design space and there are many possible options.

Part of the motivation for my performance work has been to try and get the performance of 3D graphics at a level where it's fast enough to make a simple game, and the results have been very good so far. Plush can render somewhere in the range of 10,000 flat-shaded polygons at an interactive frame rate. I used that to build a little game where you're [riding a motorcycle](https://www.youtube.com/watch?v=ElwU9PAvpRM) on a highway through an infinite cityscape. If you want to try it, just clone the [Plush repository](https://github.com/maximecb/plush) and run `cargo run --release examples/night_ride.psh`.

In terms of next steps, I'm looking at converting the Plush interpreter from a stack-based design to a register-based design. I think that could yield a nice performance boost. I've also been thinking that I could use the Plush VM to build a minimalistic LISP dialect that explores new language design ideas. I'm thinking of calling that language JetLISP. Stay tuned for more. You can subscribe to my mailing list below if you want to get notified about future posts.

**Followup:** Plush's new [register-based interpreter](https://pointersgonewild.com/2026-09-02-plushs-new-register-based-interpreter) is now complete, and it's insanely fast!

Subscribe to my mailing list and follow this blog:

Copyright © 2011–2026 Maxime Chevalier-Boisvert. All rights reserved.
