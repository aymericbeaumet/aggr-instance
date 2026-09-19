---
title: Introduction to the Zig Programming Language
link: https://andrewkelley.me/post/intro-to-zig.html
source: andrewkelley-me
published: 2016-02-08T16:07:35Z
updated: 2016-02-08T16:07:35Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'Introduction to the Zig Programming Language The past few months I took a break from working on Genesis Digital Audio Workstation to work, instead, on creating a new programming language. I am nothing if not ambitious, and my goal is to create a new programming language that is more pragmatic than C. This is like to trying to be more evil than the devil himself. So, in order, these are the priorities of Zig: Pragmatic: At the end of the day, all that really matters is whether the language helped you do what you were trying to do better than any other language. Optimal: The most natural way to write a program should result in top-of-the-line runtime performance, equivalent to or better than C. In places where performance is king, the optimal code should be clearly expressible. Safe: Optimality may be sitting in the driver''s seat, but safety is sitting in the passenger''s seat, wearing its seatbelt, and asking nicely for the other passengers to do the same. Readable: Zig prioritizes reading code over writing it. Avoid complicated syntax. Generally there should be a canonical way to do everything. Table of Contents Table of Contents Design Decisions Widely Diverging Debug and Release Builds C ABI Compatibility Maybe Type Instead of Null Pointer The Error Type Alternate Standard Library Alternatives to the Preprocessor Milestone: Tetris Implemented in Zig Resources Design Decisions Widely Diverging Debug and Release Builds Zig has the concept of a debug build vs a release build. Here is a comparison of priorities for debug mode vs release mode: Debug Mode Release Mode Time Spent Compiling Code must compile fast. Use all manner of caching, shared objects, multithreading, whatever must be done in order to produce a binary as soon as possible. Making a release build could take orders of magnitude longer than a debug build and that is acceptable. Runtime Performance Could be order of magnitude slower than release build and that is acceptable. Optimal performance. Aggressive optimizations. Take the time needed to produce a highly efficient runtime efficiency. No compromises here. Undefined Behavior What would be undefined behavior in a release build, is defined behavior in a debug build, and that is for the runtime to trap. That is, crash. This includes things like array bounds checking, integer overflow, reaching unreachable code. Not all undefined behavior can be caught, but a comfortably large amount can. Undefined behavior in release mode has unspecified consequences, and this lets the optimizer produce optimal code. The build mode is available to the source code via the expression @import("builtin").mode. Note: Since this blog post, Zig has gained two more release modes: Release Safe Release Small Complete C ABI Compatibility Part of being pragmatic is recognizing C''s existing success. Interop with C is crucial. Zig embraces C like the mean older brother who you are a little afraid of but you still want to like you and be your friend. In Zig, functions look like this: fn doSomething() { // ... } The compiler is free to inline this function, change its parameters, and otherwise do whatever it wants, since this is an internal function. However if you decide to export it: export fn doSomething() { // ... } Now this function has the C ABI, and the name shows up in the symbol table verbatim. Likewise, you can declare an external function prototype: extern fn puts(s: [*]const u8) c_int; In Zig, like in C, you typically do not create a "wrapper" or "bindings" to a library, you just use it. But if you had to type out or generate all the extern function prototypes, this would be a binding. That is why Zig has the ability to parse .h files: use @cImport({ @cInclude("stdio.h"); }); This exposes all the symbols in stdio.h - including the #define statements - to the zig program, and then you can call puts or printf just like you would in C. One of Zig''s use cases is slowly transitioning a large C project to Zig. Zig can produce simple .o files for linking against other .o files, and it can also generate .h files based on what you export. So you could write part of your application in C and part in Zig, link all the .o files together and everything plays nicely with each other. Optional Type Instead of Null Pointer One area that Zig provides safety without compromising efficiency or readability is with the optional type. The question mark symbolizes the optional type. You can convert a type to an optional type by putting a question mark in front of it, like this: // normal integer const normal_int: i32 = 1234; // optional integer const optional_int: ?i32 = 5678; Now the variable optional_int could be an i32, or null. Instead of integers, let''s talk about pointers. Null references are the source of many runtime exceptions, and even stand accused of being the worst mistake of computer science. Zig does not have them. Instead, you can use an optional pointer. This secretly compiles down to a normal pointer, since we know we can use 0 as the null value for the maybe type. But the compiler can check your work and make sure you don''t assign null to something that can''t be null. Typically the downside of not having null is that it makes the code more verbose to write. But, let''s compare some equivalent C code and Zig code. Task: call malloc, if the result is null, return null. C code // malloc prototype included for reference void *malloc(size_t size); struct Foo *do_a_thing(void) { char *ptr = malloc(1234); if (!ptr) return NULL; // ... } Zig code // malloc prototype included for reference extern fn malloc(size: size_t) ?[*]u8; fn doAThing() ?*Foo { const ptr = malloc(1234) orelse return null; // ... } Here, Zig is at least as convenient, if not more, than C. And, the type of "ptr" is [*]u8 not ?[*]u8. The orelse operator unwrapped the maybe type and therefore ptr is guaranteed to be non-null everywhere it is used in the function. The other form of checking against NULL you might see looks like this: void do_a_thing(struct Foo *foo) { // do some stuff if (foo) { do_something_with_foo(foo); } // do some stuff } In Zig you can accomplish the same thing: fn doAThing(optional_foo: ?*Foo) { // do some stuff if (optional_foo) |foo| { doSomethingWithFoo(foo); } // do some stuff } Once again, the notable thing here is that inside the if block, foo is no longer an optional pointer, it is a pointer, which cannot be null. One benefit to this is that functions which take pointers as arguments can be annotated with the "nonnull" attribute - __attribute__((nonnull)) in GCC. The optimizer can sometimes make better decisions knowing that pointer arguments cannot be null. Note: when this blog post was written, Zig did not distinguish between Single Item Pointers and Unknown Length Pointers. You can read about this in the documentation. Errors One of the distinguishing features of Zig is its exception handling strategy. Zig introduces two primitive types: Error Sets Error Unions An error set can be declared like this: const FileOpenError = error { FileNotFound, OutOfMemory, UnexpectedToken, }; An error set is a lot like an enum, except errors from different error sets which share a name, are defined to have the same numerical value. So each error name has a globally unique integer associated with it. The integer value 0 is reserved. You can refer to these error values with field access syntax, such as FileOpenError.FileNotFound. There is syntactic sugar for creating an ad-hoc error set and referring to one of its errors: error.SomethingBroke. This is equivalent to error{SomethingBroke}.SomethingBroke. In the same way that pointers cannot be null, an error set value is always an error. const err = error.FileNotFound; Most of the time you will not find yourself using an error set type. Instead, likely you will be using the error union type. Error unions are created with the binary operator !, with the error set on the left and any other type on the right: ErrorSet!OtherType. Here is a function to parse a string into a 64-bit integer: const ParseError = error { InvalidChar, Overflow, }; pub fn parseU64(buf: []const u8, radix: u8) ParseError!u64 { var x: u64 = 0; for (buf) |c| { const digit = charToDigit(c); if (digit >= radix) { return error.InvalidChar; } // x *= radix if (@mulWithOverflow(u64, x, radix, &x)) { return error.Overflow; } // x += digit if (@addWithOverflow(u64, x, digit, &x)) { return error.Overflow; } } return x; } Notice the return type is ParseError!u64. This means that the function either returns an unsigned 64 bit integer, or one of the ParseError errors. Within the function definition, you can see some return statements that return an error set value, and at the bottom a return statement that returns a u64. Both types implicitly cast to ParseError!u64. Note: this blog post was written before Zig had the concept of Error Sets vs anyerror, and before Zig had Error Set Inference. Most functions in Zig can rely on error set inference, which would make the prototype of parseU64 look like this: pub fn parseU64(buf: []const u8, radix: u8) !u64 { ... What it looks like to use this function varies depending on what you''re trying to do. One of the following: You want to provide a default value if it returned an error. If it returned an error then you want to return the same error. You know with complete certainty it will not return an error, so want to unconditionally unwrap it. You want to take a different action for each possible error. If you want to provide a default value, you can use the catch expression: fn doAThing(str: []u8) void { const number = parseU64(str, 10) catch 13; // ... } In this code, number will be equal to the successfully parsed string, or a default value of 13. The type of the right hand side of the catch expression must match the unwrapped error union type, or of type noreturn. Let''s say you wanted to return the error if you got one, otherwise continue with the function logic: fn doAThing(str: []u8) !void { const number = parseU64(str, 10) catch |err| return err; // ... } There is a shortcut for this. The try expression: fn doAThing(str: []u8) !void { const number = try parseU64(str, 10); // ... } try evaluates an error union expression. If it is an error, it returns from the current function with the same error. Otherwise, the expression results in the unwrapped value. Maybe you know with complete certainty that an expression will never be an error. In this case you can do this: const number = parseU64("1234", 10) catch unreachable; Here we know for sure that "1234" will parse successfully. So we put the unreachable keyword on the right hand side. unreachable generates a panic in debug mode and undefined behavior in release mode. So, while we''re debugging the application, if there was a surprise error here, the application would crash appropriately. There is no syntactic shortcut for catch unreachable. This encourages programmers to think carefully before using it. Finally, you may want to take a different action for every situation. For that, we have if combined with switch: fn doAThing(str: []u8) { if (parseU64(str, 10)) |number| { doSomethingWithNumber(number); } else |err| switch (err) { error.Overflow => { // handle overflow... }, // we promise that InvalidChar won''t happen (or crash in debug mode if it does) error.InvalidChar => unreachable, } } The important thing to note here is that if parseU64 is modified to return a different set of errors, Zig will emit compile errors for handling impossible error codes, and for not handling possible error codes. The other component to error handling is defer statements. In addition to an unconditional defer, Zig has errdefer, which evaluates the deferred expression on block exit path if and only if the function returned with an error from the block. Example: fn createFoo(param: i32) !Foo { const foo = try tryToAllocateFoo(); // now we have allocated foo. we need to free it if the function fails. // but we want to return it if the function succeeds. errdefer deallocateFoo(foo); const tmp_buf = allocateTmpBuffer() orelse return error.OutOfMemory; // tmp_buf is truly a temporary resource, and we for sure want to clean it up // before this block leaves scope defer deallocateTmpBuffer(tmp_buf); if (param > 1337) return error.InvalidParam; // here the errdefer will not run since we''re returning success from the function. // but the defer will run! return foo; } The neat thing about this is that you get robust error handling without the verbosity and cognitive overhead of trying to make sure every exit path is covered. The deallocation code is always directly following the allocation code. A couple of other tidbits about error handling: These primitives give enough expressiveness that it''s completely practical that failing to check for an error is a compile error. If you really want to ignore the error, you can use catch unreachable and get the added benefit of crashing in debug mode if your assumption was wrong. Since Zig understands error types, it can pre-weight branches in favor of errors not occuring. Just a small optimization benefit that is not available in other languages. There are no C++ style exceptions or stack unwinding or anything fancy like that. Zig simply makes it convenient to pass error codes around. Alternate Standard Library Part of the Zig project is providing an alternative to libc. libc has a lot of useful stuff in it, but it also has cruft. Since we''re starting fresh here, we can create a new API without some of the mistakes of the 70s still haunting us, and with our 20-20 hindsight. Further, calling dynamically linked functions is slow. Zig''s philosophy is that compiling against the standard library in source form is worth it. In C this would be called Link Time Optimization - where you generate Intermediate Representation instead of machine code and then do another compile step at link time. In Zig, we skip the middle man, and create a single compilation unit with everything in it, then run the optimizations. So, you can choose to link against libc and take advantage of it, or you can choose to ignore it and use the Zig standard library instead. Note, however, that virtually every C library you depend on probably also depends on libc, which drags libc as a dependency into your project. Using libc is still a first class use case for Zig. Alternatives to the Preprocessor The C preprocessor is extremely powerful. Maybe a little too powerful. The problem with the preprocessor is that it turns one language into two languages that don''t know about each other. Here are some examples of where the preprocessor messes things up: The compiler cannot catch even simple syntax errors in code that is excluded via #ifdef. IDEs cannot implement a function, variable, or field renaming feature that works correctly. Among other mistakes, it will miss renaming things that are in code excluded via #ifdef. Preprocessor defines do not show up in debug symbols by default. #include is the single biggest contributor to slow compile times in both C and C++. Preprocessor defines are problematic for bindings generators for other languages. Regardless of the flaws, C programmers find ourselves using the preprocessor because it provides necessary features, such as conditional compilation, a constant that can be used for array sizes, and generics. Zig plans to provide better alternatives to solve these problems. For example, the constant expression evaluator of Zig allows you to do this: const array_len = 10 * 2 + 1; const Foo = struct { array: [array_len]i32, }; This is not an amazing concept, but it eliminates one use case for #define. Next, conditional compilation. In Zig, compilation variables are available via @import("builtin"). The declarations available in this import evaluate to constant expressions. You can write normal code using these constants: const builtin = @import("builtin"); fn doSomething() { if (builtin.mode == builtin.Mode.ReleaseFast) { // do the release behavior } else { // do the debug behavior } } This is guaranteed to leave out the if statement when the code is generated. One use case for conditional compilation is demonstrated in libsoundio: static const enum SoundIoBackend available_backends[] = { #ifdef SOUNDIO_HAVE_JACK SoundIoBackendJack, #endif #ifdef SOUNDIO_HAVE_PULSEAUDIO SoundIoBackendPulseAudio, #endif #ifdef SOUNDIO_HAVE_ALSA SoundIoBackendAlsa, #endif #ifdef SOUNDIO_HAVE_COREAUDIO SoundIoBackendCoreAudio, #endif #ifdef SOUNDIO_HAVE_WASAPI SoundIoBackendWasapi, #endif SoundIoBackendDummy, }; Here, we want a statically sized array to have different contents depending on whether we have certain libraries present. In Zig, it would look something like this: const opts = @import("build_options"); const available_backends = (if (opts.have_jack) []SoundIoBackend{SoundIoBackend.Jack} else []SoundIoBackend{}) ++ (if (opts.have_pulse_audio) []SoundIoBackend{SoundIoBackend.PulseAudio} else []SoundIoBackend{}) ++ (if (opts.have_alsa) []SoundIoBackend{SoundIoBackend.Alsa} else []SoundIoBackend{}) ++ (if (opts.have_core_audio) []SoundIoBackend{SoundIoBackend.CoreAudio} else []SoundIoBackend{}) ++ (if (opts.have_wasapi) []SoundIoBackend{SoundIoBackend.Wasapi} else []SoundIoBackend{}) ++ []SoundIoBackend{SoundIoBackend.Dummy}; Here we take advantage of the compile-time array concatenation operator, ++. It''s a bit more verbose than the C equivalent, but the important thing is that it''s one language, not two. Finally, generics. Zig implements generics by allowing programmers to mark parameters to functions as known at compile-time. Milestone: Tetris Implemented in Zig This past week I achieved a fun milestone: a fully playable Tetris clone implemented in Zig, with the help of libc, GLFW, and libpng. If you''re using Linux on the x86_64 architecture, which is currently the only supported target, you could download a Zig build and then build this Tetris game. Otherwise, here''s a video of me demoing it: Resources If you are interested in the language, feel free to participate. Home Page Source code and issue tracker: https://github.com/ziglang/zig IRC channel: #zig on Freenode Financial Support: Become a sponsor Official Documentation'
content: extracted
html: 2016-02-08-introduction-to-the-zig-programming-language.html
---

The past few months I took a break from working on [Genesis Digital Audio Workstation](http://genesisdaw.org/) to work, instead, on creating a [new programming language](https://ziglang.org/).

I am nothing if not ambitious, and my goal is to create a new programming language that is *more pragmatic than C*. This is like to trying to be more evil than the devil himself.

So, in order, these are the priorities of Zig:

1. **Pragmatic**: At the end of the day, all that really matters is whether the language helped you do what you were trying to do better than any other language.
2. **Optimal**: The most natural way to write a program should result in top-of-the-line runtime performance, equivalent to or better than C. In places where performance is king, the optimal code should be clearly expressible.
3. **Safe**: Optimality may be sitting in the driver's seat, but safety is sitting in the passenger's seat, wearing its seatbelt, and asking nicely for the other passengers to do the same.
4. **Readable**: Zig prioritizes reading code over writing it. Avoid complicated syntax. Generally there should be a canonical way to do everything.

## Table of Contents

1. [Table of Contents](https://andrewkelley.me/post/intro-to-zig.html#toc)
2. [Design Decisions](https://andrewkelley.me/post/intro-to-zig.html#design-decisions)
   1. [Widely Diverging Debug and Release Builds](https://andrewkelley.me/post/intro-to-zig.html#debug-release)
   2. [C ABI Compatibility](https://andrewkelley.me/post/intro-to-zig.html#c-abi)
   3. [Maybe Type Instead of Null Pointer](https://andrewkelley.me/post/intro-to-zig.html#maybe-type)
   4. [The Error Type](https://andrewkelley.me/post/intro-to-zig.html#error-type)
   5. [Alternate Standard Library](https://andrewkelley.me/post/intro-to-zig.html#stdlib)
   6. [Alternatives to the Preprocessor](https://andrewkelley.me/post/intro-to-zig.html#preprocessor-alternatives)
3. [Milestone: Tetris Implemented in Zig](https://andrewkelley.me/post/intro-to-zig.html#tetris)
4. [Resources](https://andrewkelley.me/post/intro-to-zig.html#resources)

## Design Decisions

### Widely Diverging Debug and Release Builds

Zig has the concept of a *debug build* vs a *release build*. Here is a comparison of priorities for debug mode vs release mode:

|                                                                                                                                                                                                                                                                                                                               | Debug Mode                                                                                                                                 | Release Mode |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | ------------ |
| Code must compile fast. Use all manner of caching, shared objects, multithreading, whatever must be done in order to produce a binary as soon as possible.                                                                                                                                                                    | Making a release build could take orders of magnitude longer than a debug build and that is acceptable.                                    |              |
| Could be order of magnitude slower than release build and that is acceptable.                                                                                                                                                                                                                                                 | Optimal performance. Aggressive optimizations. Take the time needed to produce a highly efficient runtime efficiency. No compromises here. |              |
| What *would* be undefined behavior in a release build, is defined behavior in a debug build, and that is for the runtime to trap. That is, crash. This includes things like array bounds checking, integer overflow, reaching unreachable code. Not all undefined behavior can be caught, but a comfortably large amount can. | Undefined behavior in release mode has unspecified consequences, and this lets the optimizer produce optimal code.                         |              |

The build mode is available to the source code via the expression `@import("builtin").mode`.

Note: Since this blog post, Zig has gained [two more release modes](https://ziglang.org/documentation/master/#Build-Mode):

- Release Safe
- Release Small

### Complete C ABI Compatibility

Part of being pragmatic is recognizing C's existing success. Interop with C is crucial. Zig embraces C like the mean older brother who you are a little afraid of but you still want to like you and be your friend.

In Zig, functions look like this:

```
fn doSomething() {
    // ...
}
```

The compiler is free to inline this function, change its parameters, and otherwise do whatever it wants, since this is an internal function. However if you decide to export it:

```
export fn doSomething() {
    // ...
}
```

Now this function has the C ABI, and the name shows up in the symbol table verbatim. Likewise, you can declare an external function prototype:

```
extern fn puts(s: [*]const u8) c_int;
```

In Zig, like in C, you typically do not create a "wrapper" or "bindings" to a library, you just use it. But if you had to type out or generate all the extern function prototypes, this would be a binding. That is why Zig has the ability to parse .h files:

```
use @cImport({
    @cInclude("stdio.h");
});
```

This exposes all the symbols in stdio.h - including the `#define` statements - to the zig program, and then you can call `puts` or `printf` just like you would in C.

One of Zig's use cases is [slowly transitioning a large C project to Zig](http://tiehuis.github.io/iterative-replacement-of-c-with-zig). Zig can produce simple .o files for linking against other .o files, and it can also generate .h files based on what you export. So you could write part of your application in C and part in Zig, link all the .o files together and everything plays nicely with each other.

### Optional Type Instead of Null Pointer

One area that Zig provides safety without compromising efficiency or readability is with the optional type.

The question mark symbolizes the optional type. You can convert a type to an optional type by putting a question mark in front of it, like this:

```
// normal integer
const normal_int: i32 = 1234;

// optional integer
const optional_int: ?i32 = 5678;
```

Now the variable `optional_int` could be an `i32`, or `null`.

Instead of integers, let's talk about pointers. Null references are the source of many runtime exceptions, and even stand accused of being [the worst mistake of computer science](https://www.lucidchart.com/techblog/2015/08/31/the-worst-mistake-of-computer-science/).

Zig does not have them.

Instead, you can use an optional pointer. This secretly compiles down to a normal pointer, since we know we can use 0 as the null value for the maybe type. But the compiler can check your work and make sure you don't assign null to something that can't be null.

Typically the downside of not having null is that it makes the code more verbose to write. But, let's compare some equivalent C code and Zig code.

Task: call malloc, if the result is null, return null.

C code

```
// malloc prototype included for reference
void *malloc(size_t size);

struct Foo *do_a_thing(void) {
    char *ptr = malloc(1234);
    if (!ptr) return NULL;
    // ...
}
```

Zig code

```
// malloc prototype included for reference
extern fn malloc(size: size_t) ?[*]u8;

fn doAThing() ?*Foo {
    const ptr = malloc(1234) orelse return null;
    // ...
}
```

Here, Zig is at least as convenient, if not more, than C. And, the type of "ptr" is `[*]u8` *not* `?[*]u8`. The `orelse` operator unwrapped the maybe type and therefore `ptr` is guaranteed to be non-null everywhere it is used in the function.

The other form of checking against NULL you might see looks like this:

```
void do_a_thing(struct Foo *foo) {
    // do some stuff

    if (foo) {
        do_something_with_foo(foo);
    }

    // do some stuff
}
```

In Zig you can accomplish the same thing:

```
fn doAThing(optional_foo: ?*Foo) {
    // do some stuff

    if (optional_foo) |foo| {
      doSomethingWithFoo(foo);
    }

    // do some stuff
}
```

Once again, the notable thing here is that inside the if block, `foo` is no longer an optional pointer, it is a pointer, which cannot be null.

One benefit to this is that functions which take pointers as arguments can be annotated with the "nonnull" attribute - `__attribute__((nonnull))` in [GCC](https://gcc.gnu.org/onlinedocs/gcc-4.0.0/gcc/Function-Attributes.html). The optimizer can sometimes make better decisions knowing that pointer arguments cannot be null.

Note: when this blog post was written, Zig did not distinguish between Single Item Pointers and Unknown Length Pointers. You can [read about this in the documentation](https://ziglang.org/documentation/master/#Pointers).

### Errors

One of the distinguishing features of Zig is its exception handling strategy.

Zig introduces two primitive types:

- Error Sets
- Error Unions

An error set can be declared like this:

```
const FileOpenError = error {
  FileNotFound,
  OutOfMemory,
  UnexpectedToken,
};
```

An error set is a lot like an enum, except errors from different error sets which share a name, are defined to have the same numerical value. So each error name has a globally unique integer associated with it. The integer value 0 is reserved.

You can refer to these error values with field access syntax, such as `FileOpenError.FileNotFound`. There is syntactic sugar for creating an ad-hoc error set and referring to one of its errors: `error.SomethingBroke`. This is equivalent to `error{SomethingBroke}.SomethingBroke`.

In the same way that pointers cannot be null, an error set value is always an error.

```
const err = error.FileNotFound;
```

Most of the time you will not find yourself using an error set type. Instead, likely you will be using the error union type. Error unions are created with the binary operator `!`, with the error set on the left and any other type on the right: `ErrorSet!OtherType`.

Here is a function to parse a string into a 64-bit integer:

```
const ParseError = error {
    InvalidChar,
    Overflow,
};

pub fn parseU64(buf: []const u8, radix: u8) ParseError!u64 {
    var x: u64 = 0;

    for (buf) |c| {
        const digit = charToDigit(c);

        if (digit >= radix) {
            return error.InvalidChar;
        }

        // x *= radix
        if (@mulWithOverflow(u64, x, radix, &x)) {
            return error.Overflow;
        }

        // x += digit
        if (@addWithOverflow(u64, x, digit, &x)) {
            return error.Overflow;
        }
    }

    return x;
}
```

Notice the return type is `ParseError!u64`. This means that the function either returns an unsigned 64 bit integer, or one of the `ParseError` errors.

Within the function definition, you can see some return statements that return an error set value, and at the bottom a return statement that returns a `u64`. Both types implicitly cast to `ParseError!u64`.

Note: this blog post was written before Zig had the concept of [Error Sets](https://ziglang.org/documentation/master/#Error-Set-Type) vs [anyerror](https://ziglang.org/documentation/master/#The-Global-Error-Set), and before Zig had [Error Set Inference](https://ziglang.org/documentation/master/#Inferred-Error-Sets). Most functions in Zig can rely on error set inference, which would make the prototype of `parseU64` look like this:

```
pub fn parseU64(buf: []const u8, radix: u8) !u64 {
    ...
```

What it looks like to use this function varies depending on what you're trying to do. One of the following:

- You want to provide a default value if it returned an error.
- If it returned an error then you want to return the same error.
- You know with complete certainty it will not return an error, so want to unconditionally unwrap it.
- You want to take a different action for each possible error.

If you want to provide a default value, you can use the `catch` expression:

```
fn doAThing(str: []u8) void {
    const number = parseU64(str, 10) catch 13;
    // ...
}
```

In this code, `number` will be equal to the successfully parsed string, or a default value of 13. The type of the right hand side of the `catch` expression must match the unwrapped error union type, or of type `noreturn`.

Let's say you wanted to return the error if you got one, otherwise continue with the function logic:

```
fn doAThing(str: []u8) !void {
    const number = parseU64(str, 10) catch |err| return err;
    // ...
}
```

There is a shortcut for this. The `try` expression:

```
fn doAThing(str: []u8) !void {
    const number = try parseU64(str, 10);
    // ...
}
```

`try` evaluates an error union expression. If it is an error, it returns from the current function with the same error. Otherwise, the expression results in the unwrapped value.

Maybe you know with complete certainty that an expression will never be an error. In this case you can do this:

```
const number = parseU64("1234", 10) catch unreachable;
```

Here we know for sure that "1234" will parse successfully. So we put the `unreachable` keyword on the right hand side. `unreachable` generates a panic in debug mode and undefined behavior in release mode. So, while we're debugging the application, if there *was* a surprise error here, the application would crash appropriately.

There is no syntactic shortcut for `catch unreachable`. This encourages programmers to think carefully before using it.

Finally, you may want to take a different action for every situation. For that, we have `if` combined with `switch`:

```
fn doAThing(str: []u8) {
    if (parseU64(str, 10)) |number| {
        doSomethingWithNumber(number);
    } else |err| switch (err) {
        error.Overflow => {
            // handle overflow...
        },
        // we promise that InvalidChar won't happen (or crash in debug mode if it does)
        error.InvalidChar => unreachable,
    }
}
```

The important thing to note here is that if `parseU64` is modified to return a different set of errors, Zig will emit compile errors for handling impossible error codes, and for not handling possible error codes.

The other component to error handling is defer statements. In addition to an unconditional `defer`, Zig has `errdefer`, which evaluates the deferred expression on block exit path if and only if the function returned with an error from the block.

Example:

```
fn createFoo(param: i32) !Foo {
    const foo = try tryToAllocateFoo();
    // now we have allocated foo. we need to free it if the function fails.
    // but we want to return it if the function succeeds.
    errdefer deallocateFoo(foo);

    const tmp_buf = allocateTmpBuffer() orelse return error.OutOfMemory;
    // tmp_buf is truly a temporary resource, and we for sure want to clean it up
    // before this block leaves scope
    defer deallocateTmpBuffer(tmp_buf);

    if (param > 1337) return error.InvalidParam;

    // here the errdefer will not run since we're returning success from the function.
    // but the defer will run!
    return foo;
}
```

The neat thing about this is that you get robust error handling without the verbosity and cognitive overhead of trying to make sure every exit path is covered. The deallocation code is always directly following the allocation code.

A couple of other tidbits about error handling:

- These primitives give enough expressiveness that it's completely practical that failing to check for an error is a compile error. If you really want to ignore the error, you can use `catch unreachable` and get the added benefit of crashing in debug mode if your assumption was wrong.
- Since Zig understands error types, it can pre-weight branches in favor of errors not occuring. Just a small optimization benefit that is not available in other languages.
- There are no C++ style exceptions or stack unwinding or anything fancy like that. Zig simply makes it convenient to pass error codes around.

### Alternate Standard Library

Part of the Zig project is providing an alternative to libc.

libc has a lot of useful stuff in it, but it also has [cruft](https://gcc.gnu.org/ml/gcc/1998-12/msg00083.html). Since we're starting fresh here, we can create a new API without some of the mistakes of the 70s still haunting us, and with our 20-20 hindsight.

Further, calling dynamically linked functions is [slow](http://ewontfix.com/18/). Zig's philosophy is that compiling against the standard library in source form is worth it. In C this would be called Link Time Optimization - where you generate Intermediate Representation instead of machine code and then do another compile step at link time. In Zig, we skip the middle man, and create a single compilation unit with everything in it, then run the optimizations.

So, you can choose to link against libc and take advantage of it, or you can choose to ignore it and use the Zig standard library instead. Note, however, that virtually every C library you depend on probably also depends on libc, which drags libc as a dependency into your project. Using libc is still a first class use case for Zig.

### Alternatives to the Preprocessor

The C preprocessor is extremely powerful. Maybe a little *too* powerful.

The problem with the preprocessor is that it turns one language into two languages that don't know about each other.

Here are some examples of where the preprocessor messes things up:

- The compiler cannot catch even simple syntax errors in code that is excluded via `#ifdef`.
- IDEs cannot implement a function, variable, or field renaming feature that works correctly. Among other mistakes, it will miss renaming things that are in code excluded via `#ifdef`.
- Preprocessor defines do not show up in debug symbols by default.
- `#include` is the single biggest contributor to slow compile times in both C and C++.
- Preprocessor defines are problematic for bindings generators for other languages.

Regardless of the flaws, C programmers find ourselves using the preprocessor because it provides necessary features, such as conditional compilation, a constant that can be used for array sizes, and generics.

Zig plans to provide better alternatives to solve these problems. For example, the constant expression evaluator of Zig allows you to do this:

```
const array_len = 10 * 2 + 1;
const Foo = struct {
    array: [array_len]i32,
};
```

This is not an amazing concept, but it eliminates one use case for `#define`.

Next, conditional compilation. In Zig, compilation variables are available via `@import("builtin")`.

The declarations available in this import evaluate to constant expressions. You can write normal code using these constants:

```
const builtin = @import("builtin");
fn doSomething() {
    if (builtin.mode == builtin.Mode.ReleaseFast) {
        // do the release behavior
    } else {
        // do the debug behavior
    }
}
```

This is [guaranteed to leave out the if statement when the code is generated](https://andrewkelley.me/post/zig-programming-language-blurs-line-compile-time-run-time.html).

One use case for conditional compilation is demonstrated in [libsoundio](http://libsound.io/):

```
static const enum SoundIoBackend available_backends[] = {
#ifdef SOUNDIO_HAVE_JACK
    SoundIoBackendJack,
#endif
#ifdef SOUNDIO_HAVE_PULSEAUDIO
    SoundIoBackendPulseAudio,
#endif
#ifdef SOUNDIO_HAVE_ALSA
    SoundIoBackendAlsa,
#endif
#ifdef SOUNDIO_HAVE_COREAUDIO
    SoundIoBackendCoreAudio,
#endif
#ifdef SOUNDIO_HAVE_WASAPI
    SoundIoBackendWasapi,
#endif
    SoundIoBackendDummy,
};
```

Here, we want a statically sized array to have different contents depending on whether we have certain libraries present.

In Zig, it would look something like this:

```
const opts = @import("build_options");
const available_backends =
    (if (opts.have_jack)
        []SoundIoBackend{SoundIoBackend.Jack}
    else
        []SoundIoBackend{})
    ++
    (if (opts.have_pulse_audio)
        []SoundIoBackend{SoundIoBackend.PulseAudio}
    else
        []SoundIoBackend{})
    ++
    (if (opts.have_alsa)
        []SoundIoBackend{SoundIoBackend.Alsa}
    else
        []SoundIoBackend{})
    ++
    (if (opts.have_core_audio)
        []SoundIoBackend{SoundIoBackend.CoreAudio}
    else
        []SoundIoBackend{})
    ++
    (if (opts.have_wasapi)
        []SoundIoBackend{SoundIoBackend.Wasapi}
    else
        []SoundIoBackend{})
    ++
    []SoundIoBackend{SoundIoBackend.Dummy};
```

Here we take advantage of the compile-time array concatenation operator, `++`. It's a bit more verbose than the C equivalent, but the important thing is that it's one language, not two.

Finally, generics. [Zig implements generics by allowing programmers to mark parameters to functions as known at compile-time](https://andrewkelley.me/post/zig-programming-language-blurs-line-compile-time-run-time.html).

## Milestone: Tetris Implemented in Zig

This past week I achieved a fun milestone: a fully playable Tetris clone implemented in Zig, with the help of libc, [GLFW](http://www.glfw.org/), and [libpng](http://www.libpng.org/pub/png/libpng.html).

If you're using Linux on the x86\_64 architecture, which is currently the only supported target, you could [download a Zig build](https://ziglang.org/download/) and then [build this Tetris game](https://github.com/andrewrk/tetris#building-and-running).

Otherwise, here's a video of me demoing it:

## Resources

If you are interested in the language, feel free to participate.

- [Home Page](https://ziglang.org/)
- **Source code and issue tracker**: [https://github.com/ziglang/zig](https://github.com/ziglang/zig)
- **IRC channel**: `#zig` on Freenode
- **Financial Support**: [Become a sponsor](https://github.com/users/andrewrk/sponsorship)
- [Official Documentation](https://ziglang.org/documentation/master/)

Thanks for reading my blog post.
