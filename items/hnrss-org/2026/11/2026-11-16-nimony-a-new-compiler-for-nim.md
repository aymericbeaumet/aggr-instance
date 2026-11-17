---
title: 'Nimony: A New Compiler for Nim'
link: https://nimony.org/
source: hnrss-org
published: 2026-11-16T01:05:22Z
updated: 2026-11-16T01:05:22Z
first_seen: 2026-11-17T18:26:16.162530996Z
authors:
- Fervicus
content: extracted
html: 2026-11-16-nimony-a-new-compiler-for-nim.html
---

## Nimony

## Efficient, expressive, elegant

Get the nightly toolchain

Built every day from nimony’s `master` branch.

Unpack, put `bin/` on your `PATH`, done — and with `nimony n` you do not even need a C compiler. [Download page](https://nimony.org/download.html) · [Build from source](https://nimony.org/install.html) · [Tools](https://nimony.org/tools.html)

**Nimony** is a new compiler for Nim and the home of the **Nim 3** language: borrow checking, explicit nilability, sum types, checked generics and more, with **incremental** and **parallel** builds from the start. For how it relates to Nim 2, see the [FAQ](https://nimony.org/faq.html).

Nimony for…

Embedded development

- **Native code generation** for x86\_64, ARM64, ARM32, AVR and RISC-V — independent of LLVM and GCC, so there are fewer surprises from the implicit assumptions about C’s undefined behavior that plague these toolchains.
- A runtime **hardened against heap exhaustion**: running out of memory is a condition you handle, not a crash.
- A **structured, typesafe inline assembler** that uses Nim’s own syntax.
- **Memory safe** thanks to optimized automatic reference counting and a variant of borrow checking that is easy to understand.
- **Provable absence of out-of-bounds indexing.** Indexing into arrays, `seq`s and `string`s is a `requires` contract checked at the call site: an index that is provably out of range is a compile-time error, and a module with `{.feature: "staticContracts".}` must prove *every* access in bounds — no buffer overruns and no run-time checks left in the binary. Your own routines state their preconditions the same way.

Game development

- **Standalone binaries** optimized for your target architecture.
- `.passive` procs turn **linear code into state machines** — ideal for splitting game logic across frames.
- **Mutable state** is well supported, not an afterthought.
- **Short feedback loops.** Incremental and parallel builds keep the edit–compile–fix cycle fast.

Full stack development

- **A single language for the full stack.** No matter how low level or high level your tasks are: get familiar with Nimony and end up with readable, maintainable code. The same code base compiles to C, to native code, to JavaScript and to WebAssembly — from microcontrollers to the browser.
- **Batteries included.** The standard library ships what applications need: linear-time regular expressions, sockets with a TLS 1.3 client written in Nim, an HTTP client and server, JSON and a math library.
- **The compiler as a reviewer.** Generics are type checked where they are *defined*, sum types are matched exhaustively, and the borrow checker rejects mutation under active borrows. Whole classes of plausible-looking mistakes become compile errors, pointing at the line that caused them.
- **Little ceremony.** Nim’s concise, indentation-based syntax means less code to write and less code to review.

Compiler and tool developers

- **Built around [NIF](https://github.com/nim-lang/nifspec)**, an interchange format that every stage of the pipeline reads and writes. You can inspect, diff and transform the output of any stage — and it is what makes incremental and parallel builds possible.
- **Plugins** go far beyond what a macro system can accomplish easily:
  - **Custom validators** that look for possible **deadlocks**, **race conditions** or risky **recursion** (embedded targets often have very small stacks).
  - **Custom code generators** that compile a **subset of Nim to GPUs** or target **FPGAs**.
  - **Custom DSLs**, supported more cleanly than in Nim today — for example **lexer** and **parser generators**.

Here is a taste of the language.

* * *

## Sum types (algebraic data types)

Variant objects no longer need a separate discriminator enum: tags live in the `case` section, and **\`case value of Tag(fields):\`** pattern matching binds the fields for that arm — like expressions you’d write in ML-family languages, checked for exhaustiveness.

```
type
  Expr = ref object
    case
    of Lit:
      value: int
    of Add, Sub:
      left, right: Expr

proc eval(e: Expr): int =
  case e
  of Lit(value):
    result = value
  of Add(left, right):
    result = eval(left) + eval(right)
  of Sub(left, right):
    result = eval(left) - eval(right)

echo eval(Add(left: Lit(value: 10), right: Lit(value: 32)))  # 42
```

Shared fields can live outside the `case`, variants can nest (`seq[Tree]` in a branch), and grouped matchers like `{Add, Sub}(left, right)` appear where multiple tags share the same shape — see **Case in object** in the [manual](https://nimony.org/language.html).

* * *

## Borrow checking — iterator safety and aliasing

Nimony rejects classic footguns at compile time:

```
proc grow(s: var seq[int]; use: int) =
  s.add use

var s = @[1, 2, 3]
# for v in s:
#   grow(s, v)   # Error: `s` is borrowed during iteration — no realloc under active borrows
```

This follows **prefix exclusion**: while `s` (or `s.elements`) is borrowed, that path cannot be mutated until the borrow ends — sibling fields can still be updated.

* * *

## Type checked generics

By default generics are type checked: the code is checked when the generic is **defined**, not only when it is instantiated! Duck typing is still available, you get it via the `{.untyped.}` pragma.

The required operations are described via concepts:

```
type
  Comparable = concept
    proc `<`(a, b: Self): bool

proc min[T: Comparable](a, b: T): T =
  if a < b: a else: b

echo min(3, 7)
echo min("b", "a")
```

Container-style concepts (e.g. `Findable[T]`) work the same way with iterators and indexed access — see **Concepts** and **Generics** in the [manual](https://nimony.org/language.html).

* * *

## Latest news

**2026-09-08** Nimony **0.6.2** has been released. It ships with a guest report from a user who spent a year porting real projects over. A Vulkan visualiser, a web framework, an embedded server: [Nim, one year in](https://nimony.org/version0_6.html).

Older entries, including the [0.2 release](https://nimony.org/version0_2.html), live on the [News](https://nimony.org/news.html) page.
