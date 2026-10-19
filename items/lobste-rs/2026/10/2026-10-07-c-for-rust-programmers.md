---
title: C for Rust Programmers
link: https://bd103.dev/blog/2026-10-07-c-for-rust-programmers/
source: lobste-rs
published: 2026-10-07T14:30:05Z
updated: 2026-10-07T14:30:05Z
first_seen: 2026-10-09T05:58:27.034165253Z
authors:
- bd103.dev by BD103
labels:
- c
- rust
summary: Comments
content: extracted
html: 2026-10-07-c-for-rust-programmers.html
---

The very first systems programming language I ever learned was Rust. This is uncommon compared to many other programmers; you're more likely to find someone who learned C or C++ first before coming to Rust. As such, there are plenty of "Rust for C Programmers" articles on the internet, but little to no "C for Rust Programmers" articles out there.

Well, I'm about to change that! I've been learning C and C++ recently, and *holy cow those languages are quirky*. This blog post is a collection of eyebrow-raising details I learned while teaching myself C. (No C++ today, I'm not ready to dig into that can of worms.) This is not a substitute for a proper C tutorial, you'll need to Google one of those yourself. Rather, it's a list of things to keep in mind when working in the language.

With the premise set, let's draw the curtain and see what C has to offer!

## Boolean Is Not\* Built-In

The original version of C did not have a primitive type for booleans, programs instead used the integers 0 and 1. This was changed in [C99](https://en.wikipedia.org/wiki/C99), which added booleans in the optional [`<stdbool.h>`](https://en.cppreference.com/c/header/stdbool) header [\[1\]](https://bd103.dev/blog/2026-10-07-c-for-rust-programmers/#fn-_bool):

```c
#include <stdbool.h>

int main() {
    bool yes = true;
    bool no = false;

    return 0;
}
```

Even still, `true` and `false` are not literals or keywords like in other languages. Instead, they are definitions that expand to 1 and 0:

```c
#define true 1
#define false 0
```

This was changed again in [C23](https://en.wikipedia.org/wiki/C23_\(C_standard_revision\))[\[2\]](https://bd103.dev/blog/2026-10-07-c-for-rust-programmers/#fn-c23), so now booleans are true language primitives, but if you're compiling for earlier versions you'll need to include `<stdbool.h>`.

## Null-Terminated Strings

A Rust [`&str`](https://doc.rust-lang.org/stable/std/primitive.str.html) is 16 bytes: 8 bytes for the memory address and 8 bytes for the string length. This is because `str` is a dynamically sized type, and uses [pointer metadata](https://bd103.dev/blog/2023-08-06-ptr-metadata/) to track the length of the string.

```rust
// While a normal reference uses only 8 bytes...
assert_eq!(std::mem::size_of::<&u8>(), 8);

// ...strings use 16 bytes.
assert_eq!(std::mem::size_of::<&str>(), 16);
```

This approach makes fetching the string length extremely efficient, but requires more memory per `&str` reference. C uses a different approach: it doesn't store the size of its strings separately, but instead terminates every single string with a null byte (`\0`). This is an intentional trade off that results in a few things:

- C programs don't need to keep track of an extra `size` variable alongside the string
- Every single string needs room at the end for a null byte
  - This means empty strings `""` still take up one byte of memory!
- Null bytes cannot easily be used in the middle of a string without messing up [`<string.h>`](https://cppreference.com/c/header/string)'s functions
- Forgetting the null terminator can result in out-of-bound reads

In practice, this requires you to remember to allocate extra space for the null terminator and insert it at the end of strings. For example, here is a program that reverses a string in C:

```c
 1char* reverse(char* forward) {
 2    // Calculate the string's length, excluding the null terminator.
 3    unsigned long len = strlen(forward);
 4    // Allocate enough room for the string and its null terminator.
 5    char* reversed = malloc(len + 1);
 6
 7    for (int i = 0; i < len; i++) {
 8        reversed[i] = forward[len - 1 - i];
 9    }
10
11    // Add the null terminator at the end.
12    reversed[len] = '\0';
13
14    return reversed;
15}
```

Note lines 5 and 12, which take special measure to account for the null terminator. For reference, the corresponding Rust function [\[4\]](https://bd103.dev/blog/2026-10-07-c-for-rust-programmers/#fn-rust-reverse-string) doesn't need to do so:

```rust
fn reverse(forward: &[u8]) -> Box<[u8]> {
    let len = forward.len();
    let mut reversed = Box::<[u8]>::new_uninit_slice(len);

    for i in 0..len {
        reversed[i].write(forward[len - 1 - i]);
    }

    unsafe { reversed.assume_init() }
}
```

## Target-Dependent Integer Widths

C's integer types are not guaranteed to use an exact number of bits, instead the width varies depending on the target platform.

| Type        | C Standard       | 64-bit Unix | 64-bit Windows |
| ----------- | ---------------- | ----------- | -------------- |
| `char`      | at least 8 bits  | 8 bits      | 8 bits         |
| `short`     | at least 16 bits | 16 bits     | 16 bits        |
| `int`       | at least 16 bits | 32 bits     | 32 bits        |
| `long`      | at least 32 bits | 64 bits     | 32 bits        |
| `long long` | at least 64 bits | 64 bits     | 64 bits        |

Data sourced [via cppreference.com](https://cppreference.com/c/language/arithmetic_types)

`long` being 64 bits on Unix and 32 bits on Windows particularly irks me. I recommend following the advice that [a friend of mine](https://github.com/magicalbat) gave me a few years ago: if you care about cross-platform compatibility, only use the fixed width integers provided by [`<stdint.h>`](https://cppreference.com/c/header/stdint):

| Rust    | C                                                                                                      |
| ------- | ------------------------------------------------------------------------------------------------------ |
| `u8`    | `uint8_t`                                                                                              |
| `u16`   | `uint16_t`                                                                                             |
| `u32`   | `uint32_t`                                                                                             |
| `u64`   | `uint64_t`                                                                                             |
| `usize` | `size_t`[\[5\]](https://bd103.dev/blog/2026-10-07-c-for-rust-programmers/#fn-size_t-and-ptrdiff_t)    |
| `i8`    | `int8_t`                                                                                               |
| `i16`   | `int16_t`                                                                                              |
| `i32`   | `int32_t`                                                                                              |
| `i64`   | `int64_t`                                                                                              |
| `isize` | `ptrdiff_t`[\[5\]](https://bd103.dev/blog/2026-10-07-c-for-rust-programmers/#fn-size_t-and-ptrdiff_t) |

## Error Handling Sucks

I really love Rust's error handling. [`Result`](https://doc.rust-lang.org/stable/std/result/enum.Result.html) forces you to address errors, and sum types (`enum`s) and `match` statements make doing so really easy!

C's error handling story, in comparison, is straight up tragic. It seems to boil down to functions returning a "magic integer" like -1 or a null pointer to signal there was an error. You can get a little bit more information by reading [`errno`](https://cppreference.com/c/error/errno), a thread-local integer that can be used to check for specific kinds of errors, but in terms of getting actual error messages and stack traces it is much harder.

When writing C, one of the biggest things you'll notice is that the language will never force you to handle errors. It's up to you to remember that functions can fail. For example, here's a snippet of code from [Null-Terminated Strings](https://bd103.dev/blog/2026-10-07-c-for-rust-programmers/#null-terminated-strings):

```c
// Allocate enough room for the string and its null terminator.
char* reversed = malloc(len + 1);

for (int i = 0; i < len; i++) {
    reversed[i] = forward[len - 1 - i];
}
```

To new programmers, it's not immediately obvious that [`malloc()`](https://cppreference.com/c/memory/malloc) can fail and return a null pointer. If your machine runs out of memory, `reversed[i]` will cause a segfault upon being accessed. In order to avoid unhelpful segfaults, the program should check for a null pointer and gracefully exit if one is found:

```c
char* reversed = malloc(len + 1);

if (reversed == NULL) {
    perror("Error");
    exit(1);
}
```

Doing so provides a much better experience than a segfault, or worse, other unintentional behavior:

```shellscript
$ ./main
Error: Cannot allocate memory
```

Of course, remembering to check every single allocated pointer isn't an amazing developer experience. An approach I played around with was recreating Rust's `Result` in C using tagged unions:

```c
struct MallocResult {
    enum Tag { OK, ERROR } tag;
    union Value {
        void* ptr;
        char* error_message;
    } value;
};
```

It's a complete mess to use, however, and there's *still* nothing stopping you from immediately accessing `result.value.ptr` without handling the error first. The lack of visibility modifiers like `private` and `public`, which could be used to prevent this, appear to be an intentional design decision. C places full trust in the programmer to *do things right™* and gives few facilities for contracts or safe abstractions.

I personally disagree with this approach. I'm not some mastermind programming wizard who never makes mistakes. I'd much rather encode program requirements into the type system so that the compiler can check them for me\![\[6\]](https://bd103.dev/blog/2026-10-07-c-for-rust-programmers/#fn-fearless-simd) That way, I can be reasonably certain that if the code compiles it was written correctly. But I digress.

## Field Access Syntax

C has two different field access operators: [`.`](https://cppreference.com/c/language/operator_member_access#Member_access) for values and [`->`](https://cppreference.com/c/language/operator_member_access#Member_access_through_pointer) for pointers.

```c
struct Foo {
    int field;
};

struct Foo value = { 103 };
struct Foo* ptr = &value;

// Accessing the field directly.
printf("%d\n", value.field);

// Accessing the field through a pointer.
printf("%d\n", ptr->field);
```

This caught me off guard at first, as Rust uses `.` for everything.

![An old lady holding Frank's Red Hot sauce, except Ferris the crab is photoshopped on top saying 'I put that . on everything'](https://bd103.dev/blog/2026-10-07-c-for-rust-programmers/i-put-that-dot-on-everything.jpg)

If your curious, [this Stack Overflow write up](https://stackoverflow.com/a/13366168) provides some interesting history on why the `->` operator exists.[\[7\]](https://bd103.dev/blog/2026-10-07-c-for-rust-programmers/#fn-cursed-arrow-operator)

## Arrays Become Pointers for Fun

Arrays have some weird nuance to them. Sometimes they are plain values with their length accessible using [`sizeof()`](https://cppreference.com/c/language/sizeof), other times they are pointers with an unknown size. In general, this is determined by whether you are handling the array within the function it was defined or not.

To showcase what I mean, here's a very simple C program that prints the size of two arrays:

```c
char declared_size[3] = { 1, 2, 3 };
char inferred_size[] = { 4, 5, 6 };

printf("Declared size (main): %ld bytes\n", sizeof(declared_size));
printf("Inferred size (main): %ld bytes\n", sizeof(inferred_size));
```

When run, this programs tells you that both arrays take 3 bytes of memory. Good!

```shellscript
$ ./main
Declared size (main): 3 bytes
Inferred size (main): 3 bytes
```

Now, let's make a slight modification. Let's pass `declared_size` and `inferred_size` through a function before printing its size:

```c
void function(char declared_size[3], char inferred_size[]) {
    printf("Declared size (function): %ld bytes\n", sizeof(declared_size));
    printf("Inferred size (function): %ld bytes\n", sizeof(inferred_size));
}
```

None of the logic changed, the arrays are identical to the previous example, however now the program reports each array takes **8 bytes of memory**:

```shellscript
$ ./main
Declared size (function): 8 bytes
Inferred size (function): 8 bytes
```

Why? Because any array passed through a function is *implicitly converted* to a pointer to the first element. From the C compiler's perspective, the above function actually had the following type signature:

```c
void function(char* declared_size, char* inferred_size);
```

This is why it confusingly looked like each array was 8 bytes long. The arrays themselves are 3 bytes, but the pointers take up 8 bytes of memory! Handily, Clang raises a warning when you run `sizeof()` on the pointer form of an array, making it easier to catch this mistake:

```shellscript
$ clang main.c
06-arrays/main.c:5:59: warning: sizeof on array function parameter will return size of 'char *'
      instead of 'char[3]' [-Wsizeof-array-argument]
    5 |     printf("Declared size (function): %ld bytes\n", sizeof(declared_size));
      |                                                           ^
06-arrays/main.c:4:20: note: declared here
    4 | void function(char declared_size[3], char inferred_size[]) {
      |                    ^
06-arrays/main.c:6:59: warning: sizeof on array function parameter will return size of 'char *'
      instead of 'char[]' [-Wsizeof-array-argument]
    6 |     printf("Inferred size (function): %ld bytes\n", sizeof(inferred_size));
      |                                                           ^
06-arrays/main.c:4:43: note: declared here
    4 | void function(char declared_size[3], char inferred_size[]) {
      |                                           ^
2 warnings generated.
```

## Oh Yeah References Don't Exist

I'd be remiss if I didn't mention it once. C has no borrow checking, and no concept of references. It's all [raw pointers](https://doc.rust-lang.org/stable/std/primitive.pointer.html). This means you can do fun pointer arithmetic like so:

```c
int array[] = { 0, 1, 2, 3, 4, 5 };

// Woah, iterating over pointers instead of indexes!
for (int* x = &array[0]; x < &array[6]; x++) {
    *x = 5 - *x;
}
```

However, I'm not sure how good an idea that is. 😅

Either way, be careful when you mess with pointers. Making mistakes with memory results in buffer overflows and out-of-bounds writes, which provide significant threats to the security of your code.

![A graph visualizing which vulnerability category had the most CVEs between November 2021 and January 2022. Buffer overflow is the third most common, with 346 CVEs, and out-of-bounds writes are the ninth most common, with 204 CVEs. The two categories more common than buffer overflow are denial-of-service with 488 and cross-site scripting with 683.](https://bd103.dev/blog/2026-10-07-c-for-rust-programmers/vuln-categories-2021.png)

Vulnerability category distribution for CVEs registered between Nov. 2021 and Jan. 2022, [via](https://unit42.paloaltonetworks.com/network-security-trends-cross-site-scripting/)

## Conclusion

I hope you enjoyed this article! I've honestly had a lot of fun learning C. And while I doubt I'll reach for it for personal projects, it's absolutely a crucial language to know as a systems programmer. All the examples in this blog are [available on Github](https://github.com/BD103/C-for-Rust-Programmers), if you'd like to mess with them yourself!

Until next time,

\- BD103 :)

1. Technically you can use [`_Bool`](https://en.cppreference.com/c/keyword/_Bool) without the header, but you still need it for the `true` and `false` definitions. [↩](https://bd103.dev/blog/2026-10-07-c-for-rust-programmers/#fr-_bool-1)

2. It looks like [Clang still has only partial support for C23](https://clang.llvm.org/c_status.html), so you may need to wait a little while longer before chucking away your `#include <stdbool.h>` statements. [↩](https://bd103.dev/blog/2026-10-07-c-for-rust-programmers/#fr-c23-1)

3. The corresponding Rust function is not idiomatic, and can only correctly handle ASCII text. If I were writing a production version of this function, it would be a single line of code: `forward.chars().rev().collect::<String>()`. [↩](https://bd103.dev/blog/2026-10-07-c-for-rust-programmers/#fr-rust-reverse-string-1)

4. [`usize`](https://doc.rust-lang.org/stable/std/primitive.usize.html) and [`isize`](https://doc.rust-lang.org/stable/std/primitive.isize.html) don't perfectly map semantically to [`size_t`](https://cppreference.com/c/types/size_t) and [`ptrdiff_t`](https://cppreference.com/c/types/ptrdiff_t). I don't fully understand the difference, however, so I recommend doing your own research before using these types. [↩](https://bd103.dev/blog/2026-10-07-c-for-rust-programmers/#fr-size_t-and-ptrdiff_t-1) [↩2](https://bd103.dev/blog/2026-10-07-c-for-rust-programmers/#fr-size_t-and-ptrdiff_t-2)

5. [This blog post on Fearless SIMD](https://shnatsel.github.io/safe-simd-in-rust-even-on-the-inside/) provides a great example of using Rust's type system to guarantee the correctness of code. I highly recommend giving it a read! [↩](https://bd103.dev/blog/2026-10-07-c-for-rust-programmers/#fr-fearless-simd-1)

6. My favorite line of code from that post is `100->a = 0;`, it's so cursed! [↩](https://bd103.dev/blog/2026-10-07-c-for-rust-programmers/#fr-cursed-arrow-operator-1)
