---
title: Calling a function in C without naming it
link: https://wiro.world/posts/calling-c-func-without-naming-it/
source: lobste-rs
published: 2026-10-07T18:15:02Z
updated: 2026-10-07T18:15:02Z
first_seen: 2026-10-08T19:25:56.508743454Z
authors:
- wiro.world by mrnossiom
labels:
- c
- security
summary: Comments
content: extracted
html: 2026-10-07-calling-a-function-in-c-without-naming-it.html
---

My school has a controlled remote code execution environment to automatically grade the code we submit. The system checks execution, verifies proper formatting and surely has other checks not shown to the user (e.g. a code plagiarism detection tool [1](https://wiro.world/posts/calling-c-func-without-naming-it/#gconf-plagiarism)). The interface reports results mismatches and compilation errors.

A friend and I started to search for a way to have a single solution that could solve every exercise. The first step in that direction was to have a primitive that could give us arbitrary shell code execution. We assumed there would be an `sh` binary on the grading [*VM*](https://en.wikipedia.org/wiki/Virtual_machine) such that we could simply call [`execve`](https://en.wikipedia.org/wiki/Exec_\(system_call\)).

So... are we done? You guessed it, no.

Remember when I told that the grading system had other sanity checks? Well, one of them is to check that you only use functions allowed in the subject of the exercise. And `execve` is never one of them, so it fails the submission before your code even gets to run. From what we experienced, the system is a little more involved than a simple `grep` of the source code. My guess is that it uses `clang` to pre-process and parse the code and then does its checks against the resulting [AST](https://en.wikipedia.org/wiki/Abstract_syntax_tree).

This is where we get to the core of the problem. We need to call `execve` but we are not allowed to name the symbol. So how can we do it?

Zooming back a bit, we know that our C code essentially gets compiled to a binary and calling a function boils down to a jump to some place in memory. Could we not retrieve the address of `execve` in memory, hardcode it in our program and call it directly?

In modern laptop software, memory addresses are often execution-dependent. This happens because modern kernels implement [*Address Space Layout Randomization* (ASLR)](https://en.wikipedia.org/wiki/Address_space_layout_randomization) which randomizes addresses at which your binary, its libraries, heap and stack are placed. Nowadays, C code compiles using the `-pie` flag by default, this creates a [*position-independent executable*](https://en.wikipedia.org/wiki/Position-independent_code#Position-independent_executables) and means that your binary can indeed take advantage of *ASLR*. We could use the `-no-pie` C compiler flag to disable this behavior and force fixed addresses for functions in our binary. This doesn't help because we most often do not control the build process in our case. Second, the `execve` function is not directly part of our executable but dynamically loaded, so it still is at a random place.

Static addresses won't work. But *ASLR* only acts on the mapping of the segments, their content is left intact. Inside a segment, functions are still ordered in the same way. If we manage to know the static offset between the memory addresses of two functions fA and fB, obtaining the address of fA allows us to construct the address of fB and vice-versa.

The offset varies from a machine to another because of a different version of the C library, or maybe a different architecture, or maybe for some other reason. There are multiple ways to obtain the fixed offset between two functions on the target machine. One of them is to just leak the offset in an exercise where you can name symbols of both fA and fB.

Our target function is still `execve` which is part of `libc`. We assume we always have access to `printf`. We just have to leak the offset between the two functions on the VM. Let's test this locally first:

```c
printf("%td", (ptrdiff_t)((size_t)execve - (size_t)printf));
```

Running the program a couple of times gives us different results. Didn't we just say that the offset was fixed? Let's open the binary to investigate.

```plain
$ objdump --dynamic-syms ./offset | grep -E "\s(execve|printf)"
0000000000000000      DF *UND*	0000000000000000 (GLIBC_2.2.5) execve
000000000004dd64  w   DF .text	0000000000000005  Base        printf
```

`objdump` is useful for any kind of object file analysis. The `--dynamic-syms` option gives information about each symbol that will be loaded at runtime. The sixth column gives us a bit of information about where the symbol comes from. `execve` indeed comes from `glibc`, which is the implementation of the C standard library available on my laptop. But `printf` doesn't seem to come from `glibc`. Actually, the raw output of `objdump` has a bunch of `__interceptor_*` symbols which gives a clue on what's going on here.

Since the beginning, I have been compiling all my code with [ASan](https://en.wikipedia.org/wiki/Code_sanitizer) (a.k.a `-fsanitize=address`) to catch memory errors early. *ASan* works by replacing a bunch of code, including memory reads and write, by a bunch of slow function calls that check the correctness of each action. It also replaces a bunch of functions in the C standard library like `malloc` or `printf`. By doing so, these functions do not live in the `glibc` object anymore as opposed to functions that aren't replaced, in our case `execve`. The automatic grader system also happens to check for memory errors and compiles with *ASan*.

We could try to use a different symbol than `printf` that lives in the same segment as `execve`, but we still need a symbol that is allowed most of the time. Most of these symbols live in the *ASan* replaced segment, so let's commit to it. Maybe `execve` is not the only way. Looking for other useful symbols in this segment, `mmap` catches my eye.

Why is `mmap` special? Because of the [W^X (write-xor-execute)](https://en.wikipedia.org/wiki/W%5EX) security policy, there is no section of memory that is writable and executable at the same time. Thus we cannot just write raw x86 instructions in a buffer and jump to it as if it was a function nor can we overwrite an existing function with our own code. But `mmap` solves this problem because we can just tell it to give us a page that has write and execute permissions.

But let's not get ahead of ourselves. We can leak the offset between `printf` and `mmap` on the VM via another exercise where we can name both symbols. To use the offset we could use a bunch of casts but the code we hand to the grading system has to compile with `-pedantic` and this disallows cast from, to and between function pointers types. Because function pointers are still pointers in the end, we can try to modify it without the compiler noticing. I started to modify memory based on unoptimized stack slots [2](https://wiro.world/posts/calling-c-func-without-naming-it/#stack-snippet) but I was reminded that in C, *unsafety is a feature*, so we can actually just use unions.

```c
union notmmap_build {
  int (*basefn)(const char *);
  size_t addr;
  void *(*fn)(void *addr, size_t length, int prot, int flags, int fd,
              long offset);
};

union notmmap_build notmmap = {.basefn = printf};
notmmap.addr += OFFSET; // whatever offset you leaked earlier
```

We can just make a bad x86 assembly routine that lets us make Linux syscalls as if it was a C function. This routine allows us to call any syscall with up to 3 arguments.

```plain
000000000040106a <notsyscall>:
  40106a:	48 89 f8             	mov    %rdi,%rax
  40106d:	48 89 f7             	mov    %rsi,%rdi
  401070:	48 89 d6             	mov    %rdx,%rsi
  401073:	48 89 ca             	mov    %rcx,%rdx
  401076:	0f 05                	syscall
  401078:	c3                   	ret
```

Copy those bytes into a newly mapped writable and executable page.

```c
unsigned char instructions[] = {
  0x48, 0x89, 0xf8, 0x48, 0x89, 0xf7, 0x48, 0x89,
  0xd6, 0x48, 0x89, 0xca, 0x0f, 0x05, 0xc3,
};

char *code =
  notmmap.fn(NULL, 4096, PROT_READ | PROT_WRITE | PROT_EXEC,
             MAP_PRIVATE | MAP_ANONYMOUS, -1, 0);
```

And call our routine as if nothing happened. You can check [chromium syscall docs](https://www.chromium.org/chromium-os/developer-library/reference/linux-constants/syscalls/) for the arguments Linux expects.

```c
char *argv[] = {
    "/bin/sh",
    "-c",
    "echo 'Hello, World!'",
    NULL,
};

union notsyscall_build {
  char *ptr;
  // execve is syscall 59 on x86-64 Linux
  int (*notexecve)(int sys, char *path, char **argv, char **envp);
};

union notsyscall_build notsyscall = {.ptr = code};

notsyscall.notexecve(59, argv[0], argv, NULL);
```

Running it on the target VM successfully prints `Hello, World!`.

You can find the finished proof-of-concept [on GitHub](https://github.com/mrnossiom/learning-asm/tree/802b8374e24bde9f699cc6c88520ed13e0a245fa/execve).

## Conclusion

Because we know our code is compiled with `ASan`, we chose to leak the offset between `printf`, a function that is always allowed, and `mmap`, a function that gives us arbitrary assembly code execution. We can thus bypass the school's checks and call any syscall, in our case `execve` to get shell access.

We did not investigate further and simply reported this possible issue to the school.

I see no trivial way of patching this. As we've seen *ASLR* is not going to help here. One possible idea would be to relink the C standard library with a different order for each execution, randomizing the functions order each time (OpenBSD does that at boot time [here](https://isopenbsdsecu.re/mitigations/libc_symbols_randomization/)). But, you have to keep in mind that *ASan* still replaces functions with its own (so it doesn't mitigate anything here).

The exploit is not that complicated but it was fun to understand the whole process by ourselves. It's in these kind of moment that all the rabbit holes you followed finally click together and help you move forward. Also the simplicity of the exploit resides in the fact that we already have quite liberal remote code execution (as a feature). Still, it involves *ASLR* bypass and requires a fair amount of understanding of what is going on.

Discuss this article on [Lobsters](https://lobste.rs/s/ucwapn/calling_function_c_without_naming_it), [Hacker News](https://news.ycombinator.com/item?id=50005719) or [Reddit](https://www.reddit.com/r/cprogramming/comments/1x1itzo/calling_a_function_in_c_without_naming_it/).

* * *

1

You can [watch a talk](https://www.youtube.com/watch?v=kc5P3ep3ViQ) about it (in French) from the research lab of the school.
