---
title: Attention people who think UB is fine
link: https://blog.habets.se/2026/10/Attention-people-who-think-UB-is-fine.html
source: lobste-rs
published: 2026-10-23T20:47:58Z
updated: 2026-10-23T20:47:58Z
first_seen: 2026-10-24T19:24:41.714243814Z
authors:
- blog.habets.se via abareplace
labels:
- c
- c++
summary: Comments
content: extracted
html: 2026-10-23-attention-people-who-think-ub-is-fine.html
---

After [my post on undefined behavior in C](https://blog.habets.se/2026/05/Everything-in-C-is-undefined-behavior.html) some people insisted that it’s demonstrably fine, we know what the code will do, and at worst we’ll get a segfault.

This is false.

Here’s an example.

## C code

```c
#include<utility>

// Let's pretend this is a memory mapped device controlling
// motors and gears and stuff.
volatile int device_command = 0;

void serve_hot_chocolate() {
        device_command = 42;

        // Oh no, we spilled some hot chocolate, invoking UB.
        //
        // The definition of `std::unreachable()` is:
        //     Invokes undefined behavior at a given point.
        std::unreachable();
}

void explode() {
        device_command = 3;
}
```

## Example assembly output

With GCC 16.2 with `-O1`.

```nasm
"serve_hot_chocolate()":
        mov     DWORD PTR "device_command"[rip], 42
"explode()":
        mov     DWORD PTR "device_command"[rip], 3
        ret
"device_command":
        .zero   4
```

Ruh roh. Exploding chocolate! The function `explode()` will be executed, even though it was never called!

[godbolt link](https://godbolt.org/z/Y5844eTKe)

## Another example: dereferencing a null pointer

Some people insisted that dereferencing a null pointer is *fine*. It’ll just try to access address zero! And these people supposedly know what happens when you try: if nothing is mapped there, you’ll get a segfault.

Again, not true.

Let’s say we change `std::unreachable()` to `int* ptr = nullptr; *ptr = 99;` in the previous example. Now we [get this](https://godbolt.org/z/efqKrzefa):

```nasm
"serve_hot_chocolate()":
        mov     DWORD PTR "device_command"[rip], 42
        mov     DWORD PTR ds:0, 0
        ud2
```

Curious, eh? First it writes zero, **not** 99, to memory address zero. Then it crashes the program using an invalid instruction. The number 99 never appears in the generated program. But the null pointer write does. Presumably the null pointer write is done in order to improve debugger error messages.

Now let’s check [clang with godbolt](https://godbolt.org/z/86M7MWTT8):

```nasm
"serve_hot_chocolate()":
        mov     dword ptr [rip + "device_command"], 42

"explode()":
        mov     dword ptr [rip + "device_command"], 3
        ret

"device_command":
        .long   0
```

Again we get exploding chocolate.

## Let’s do some pointer arithmetic on the null pointer

Ok, so what happens if we add `ptr += 10`. That’s UB for three reasons:

1. using a null pointer for anything other than comparison against other pointers,
2. even if an object would be valid at the null pointer (which it’s not), creating a pointer that points outside that object, or if an array, beyond one-past the array, is UB,
3. even if that were valid (which it’s not), we then dereference the address.

Let’s [try it](https://godbolt.org/z/8z6hPPsvG).

```nasm
"serve_hot_chocolate":
        mov     DWORD PTR "device_command"[rip], 42
        mov     DWORD PTR ds:40, 99
        ret
```

Wow. *Now* it did what someone naively would expect, taking the code at face value. Lucky.

Why did GCC (and clang, by the way) not detect that `*(ptr + 10) = 99` is equally UB as `*ptr = 99`, and do the same thing?

Wrong question. No compiler is obligated to “detect” UB at all. Looks to me like GCC and clang, if we want to anthropomorphize them, saw “oh, null pointer dereference? Well that can’t happen, so I’ll stop emitting instructions for this function, to save on the binary size”. Because of the [halting problem](https://en.wikipedia.org/wiki/Halting_problem), it cannot in the general case know if the code will ever be executed.

## So what does UB do?

Anything. I have now demonstrated three things UB can do, when UB was triggered in the same place:

1. Run whatever happens to be next in memory (exploding chocolate).
2. Store a (for all intents and purposes) *random* value at address zero (wrote zero, not 99), and then deliberately crash.
3. Do what one would naively expect: write the value to the address seen in the code at face value.

And to think that some people don’t believe in [nasal demons](https://en.wikipedia.org/wiki/Undefined_behavior).

[Someone submitted this to HN](https://news.ycombinator.com/item?id=50203996).

disqus has started showing ads. :-(

Showing (probably incomplete) comments in a static read-only view. Click button to be able to leave comments.
