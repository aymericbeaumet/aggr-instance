---
title: 'Show HN: Zero C# – C# without a runtime or GC'
link: https://onurgumus.github.io/zero-csharp/index.html
source: hnrss-org
published: 2026-11-21T15:39:09Z
updated: 2026-11-21T15:39:09Z
first_seen: 2026-11-21T23:45:44.123432357Z
authors:
- reverseblade2
content: extracted
html: 2026-11-21-show-hn-zero-c-c-without-a-runtime-or-gc.html
remote_preview:
  url: https://onurgumus.github.io/zero-csharp/img/social.png?v=2
  alt: 'Zero C#, C# with nothing underneath. Kept: C# 15, real generics, interfaces, records and lambdas. Left out: the garbage collector and the .NET runtime. Runs on Linux, macOS, Windows, WebAssembly and DOS.'
---

A fork of bflat compiles C# straight to a small native program: no .NET runtime, no garbage collector, nothing to install where it runs.

You keep C#'s syntax, type checking and generics. Memory comes from arenas you free all at once, and a small library gives you strings, collections, input and output on those terms. The same source runs on Linux, macOS and Windows, on WebAssembly, and on DOS on a 386.

```
// Read into a stack buffer: nothing allocated per answer.
Span<char> buffer = stackalloc char[32];

Out.Write($"Q{i}: {a} + {b} = ");
int length = In.ReadLine(buffer);

// Kept until the end, so it goes into an arena.
mistakes.Add(Str.Format(keep,
    $"{a} + {b} = {a + b}, not {value}"));
```

From the quiz the [tutorial](https://onurgumus.github.io/zero-csharp/tutorial/index.html) builds, step by step.

## The language stays, the runtime goes

The compiler still checks everything it checks in .NET. What's missing is what a runtime would do while your program runs.

### You keep

- **C# 15** Unions, extension members and the new memory safety rules.
- **Real generics** A specialized copy for each type, and static abstract members.
- **Interfaces and virtual methods** Default interface methods, casts, `is` and `as`.
- **Records, tuples, anonymous types** Value equality, `with`, and the generated `ToString`.
- **Lambdas and queries** `Where`, `Select` and `GroupBy`, from a small library instead of LINQ.
- **Threads** `Parallel.For` runs a loop on every core.

### You do without

- **Garbage collector** Arenas free their memory all at once, when you say.
- **.NET runtime** Hello world is 11 KB on Windows and 40 KB on Linux, with nothing to install.
- **Catching exceptions** `throw` ends the program. Return errors as unions.
- **Reflection and async** A type can be compared, but it has no name.

## One program, five machines

The hello-world sample, unchanged, built for each target. Each build is checked by the same test suite, which requires the same output everywhere.

```
$ ./build.sh Minimal
$ ./run.sh Minimal
Hello from zero-mode!
$ wc -c bin/Minimal
39848 bin/Minimal
```

### Linux, x64 or arm64

A native program that needs only the C library. On macOS it builds and runs in a container.

```
$ ./build.sh --mac Minimal
$ bin/mac/Minimal
Hello from zero-mode!
$ wc -c bin/mac/Minimal
64728 bin/mac/Minimal
```

### macOS on Apple Silicon

A native Mac program: bflat compiles it, the Mac's own clang links it. No container to run it.

```
$ ./build.sh --win Minimal
$ ./run.sh --win Minimal
Hello from zero-mode!
$ wc -c bin/win/Minimal.exe
10752 bin/win/Minimal.exe
```

### Windows x64

A console program on Windows' own C library, built from Linux or macOS. Tested under Wine.

```
$ ./build.sh --wasm Minimal
$ ./run.sh --wasm Minimal
Hello from zero-mode!
$ wc -c bin/wasm/Minimal.wasm
15833 bin/wasm/Minimal.wasm
```

### WebAssembly

A WASI module, through NativeAOT-LLVM. Runs under wasmtime.

```
C:\>D:
D:\>PROGRAM
Hello from zero-mode!

D:\>DIR PROGRAM.EXE
PROGRAM  EXE     16,384

D:\>
```

### DOS on a 386

The same bitcode compiled for a 386, run by the HX DOS extender. Tested on Bochs's i386 model.

## What's different, and what you get

Without a runtime, a few things work differently. Each has a page that explains why and what to do instead.

- ### Memory is yours to free

  There is no garbage collector, so `new` is never freed. Arenas hand out memory and free it all at once, one per lifetime.

  [Memory without a garbage collector](https://onurgumus.github.io/zero-csharp/concepts/memory.html)
- ### Text without throwaway strings

  `Out.Line($"...")` formats straight into the output buffer, and `In.ReadLine` reads into a span. Strings that must last go into an arena.

  [Strings without a garbage collector](https://onurgumus.github.io/zero-csharp/concepts/strings.html)
- ### Collections that live in arenas

  `List`, `Dict`, and their versions for references and string keys, all handles into an arena.

  [Choose a collection](https://onurgumus.github.io/zero-csharp/how-to/choose-a-collection.html)
- ### C# 15, with the safety rules on

  Every program compiles under the updated memory safety rules: creating a pointer is safe, reading through one is marked and explained.

  [The memory safety rules](https://onurgumus.github.io/zero-csharp/concepts/safety-rules.html)

## Where to start

If you're new to it, these four pages take you from nothing to an interactive program.

1. [Install](https://onurgumus.github.io/zero-csharp/get-started/install-with-dotnet.html)

   With dotnet on Linux, or from the repository for every target.

2. [Your first program](https://onurgumus.github.io/zero-csharp/get-started/first-program.html)

   Build it, run it, see how small it is.

3. [Run it on DOS](https://onurgumus.github.io/zero-csharp/get-started/dos.html)

   The same program under FreeDOS, in an emulator.

4. [The tutorial](https://onurgumus.github.io/zero-csharp/tutorial/index.html)

   A quiz: output, input, bad input, arenas.

Looking for something specific? The [task guides](https://onurgumus.github.io/zero-csharp/how-to/index.html) answer one question each, and the [reference](https://onurgumus.github.io/zero-csharp/reference/index.html) lists every type in the library.
