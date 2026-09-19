---
title: Zig Programming Language Blurs the Line Between Compile-Time and Run-Time
link: https://andrewkelley.me/post/zig-programming-language-blurs-line-compile-time-run-time.html
source: andrewkelley-me
published: 2017-01-30T08:19:59Z
updated: 2017-01-30T08:19:59Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'Zig Programming Language Blurs the Line Between Compile-Time and Run-Time Zig places importance on the concept of whether an expression is known at compile-time. There are a few different places this concept is used, and these building blocks are used to keep the language small, readable, and powerful. Introducing the Compile-Time Concept Compile-time parameters Compile-time variables Compile-time expressions Generic Data Structures Case Study: printf in C, Rust, and Zig Conclusion Introducing the Compile-Time Concept Compile-Time Parameters Compile-time parameters is how Zig implements generics. It is compile-time duck typing and it works mostly the same way that C++ template parameters work. Example: fn max(comptime T: type, a: T, b: T) -> T { if (a > b) a else b } fn gimmeTheBiggerFloat(a: f32, b: f32) -> f32 { max(f32, a, b) } fn gimmeTheBiggerInteger(a: u64, b: u64) -> u64 { max(u64, a, b) } In Zig, types are first-class citizens. They can be assigned to variables, passed as parameters to functions, and returned from functions. However, they can only be used in expressions which are known at compile-time, which is why the parameter T in the above snippet must be marked with comptime. A comptime parameter means that: At the callsite, the value must be known at compile-time, or it is a compile error. In the function definition, the value is known at compile-time. For example, if we were to introduce another function to the above snippet: fn max(comptime T: type, a: T, b: T) -> T { if (a > b) a else b } fn letsTryToPassARuntimeType(condition: bool) { const result = max( if (condition) f32 else u64, 1234, 5678); } Then we get this result from the compiler: ./test.zig:6:9: error: unable to evaluate constant expression if (condition) f32 else u64, ^ This is an error because the programmer attempted to pass a value only known at run-time to a function which expects a value known at compile-time. Another way to get an error is if we pass a type that violates the type checker when the function is analyzed. This is what it means to have compile-time duck typing. For example: fn max(comptime T: type, a: T, b: T) -> T { if (a > b) a else b } fn letsTryToCompareBools(a: bool, b: bool) -> bool { max(bool, a, b) } The code produces this error message: ./test.zig:2:11: error: operator not allowed for type ''bool'' if (a > b) a else b ^ ./test.zig:5:8: note: called from here max(bool, a, b) ^ On the flip side, inside the function definition with the comptime parameter, the value is known at compile-time. This means that we actually could make this work for the bool type if we wanted to: fn max(comptime T: type, a: T, b: T) -> T { if (T == bool) { return a or b; } else if (a > b) { return a; } else { return b; } } fn letsTryToCompareBools(a: bool, b: bool) -> bool { max(bool, a, b) } This works because Zig implicitly inlines if expressions when the condition is known at compile-time, and the compiler guarantees that it will skip analysis of the branch not taken. This means that the actual function generated for max in this situation looks like this: fn max(a: bool, b: bool) -> bool { return a or b; } All the code that dealt with compile-time known values is eliminated and we are left with only the necessary run-time code to accomplish the task. This works the same way for switch expressions - they are implicitly inlined when the target expression is compile-time known. Compile-Time Variables In Zig, the programmer can label variables as comptime. This guarantees to the compiler that every load and store of the variable is performed at compile-time. Any violation of this results in a compile error. This combined with the fact that we can inline loops allows us to write a function which is partially evaluated at compile-time and partially at run-time. For example: const CmdFn = struct { name: []const u8, func: fn(i32) -> i32, }; const cmd_fns = []CmdFn{ CmdFn {.name = "one", .func = one}, CmdFn {.name = "two", .func = two}, CmdFn {.name = "three", .func = three}, }; fn one(value: i32) -> i32 { value + 1 } fn two(value: i32) -> i32 { value + 2 } fn three(value: i32) -> i32 { value + 3 } fn performFn(comptime prefix_char: u8, start_value: i32) -> i32 { var result: i32 = start_value; comptime var i = 0; inline while (i < cmd_fns.len) : (i += 1) { if (cmd_fns[i].name[0] == prefix_char) { result = cmd_fns[i].func(result); } } return result; } fn testPerformFn() { @setFnTest(this); assert(performFn(''t'', 1) == 6); assert(performFn(''o'', 0) == 1); assert(performFn(''w'', 99) == 99); } fn assert(ok: bool) { if (!ok) unreachable; } This example is a bit contrived, because the compile-time evaluation component is unnecessary; this code would work fine if it was all done at run-time. But it does end up generating different code. In this example, the function performFn is generated three different times, for the different values of prefix_char provided: // From the line: // assert(performFn(''t'', 1) == 6); fn performFn(start_value: i32) -> i32 { var result: i32 = start_value; result = two(result); result = three(result); return result; } // From the line: // assert(performFn(''o'', 0) == 1); fn performFn(start_value: i32) -> i32 { var result: i32 = start_value; result = one(result); return result; } // From the line: // assert(performFn(''w'', 99) == 99); fn performFn(start_value: i32) -> i32 { var result: i32 = start_value; return result; } Note that this happens even in a debug build; in a release build these generated functions still pass through rigorous LLVM optimizations. The important thing to note, however, is not that this is a way to write more optimized code, but that it is a way to make sure that what should happen at compile-time, does happen at compile-time. This catches more errors and as demonstrated later in this article, allows expressiveness that in other languages requires using macros, generated code, or a preprocessor to accomplish. Compile-Time Expressions In Zig, it matters whether a given expression is known at compile-time or run-time. A programmer can use a comptime expression to guarantee that the expression will be evaluated at compile-time. If this cannot be accomplished, the compiler will emit an error. For example: extern fn exit() -> unreachable; fn foo() { comptime { exit(); } } ./test.zig:5:9: error: unable to evaluate constant expression exit(); ^ It doesn''t make sense that a program could call exit() (or any other external function) at compile-time, so this is a compile error. However, a comptime expression does much more than sometimes cause a compile error. Within a comptime expression: All variables are comptime variables. All if, while, for, switch, and goto expressions are evaluated at compile-time, or emit a compile error if this is not possible. All function calls cause the compiler to interpret the function at compile-time, emitting a compile error if the function tries to do something that has global run-time side effects. This means that a programmer can create a function which is called both at compile-time and run-time, with no modification to the function required. Let''s look at an example: fn fibonacci(index: u32) -> u32 { if (index < 2) return index; return fibonacci(index - 1) + fibonacci(index - 2); } fn testFibonacci() { @setFnTest(this); // test fibonacci at run-time assert(fibonacci(7) == 13); // test fibonacci at compile-time comptime { assert(fibonacci(7) == 13); } } fn assert(ok: bool) { if (!ok) unreachable; } $ zig test test.zig Test 1/1 testFibonacci...OK Imagine if we had forgotten the base case of the recursive function and tried to run the tests: fn fibonacci(index: u32) -> u32 { //if (index < 2) return index; return fibonacci(index - 1) + fibonacci(index - 2); } fn testFibonacci() { @setFnTest(this); comptime { assert(fibonacci(7) == 13); } } fn assert(ok: bool) { if (!ok) unreachable; } $ zig test test.zig ./test.zig:3:28: error: operation caused overflow return fibonacci(index - 1) + fibonacci(index - 2); ^ ./test.zig:3:21: note: called from here return fibonacci(index - 1) + fibonacci(index - 2); ^ ./test.zig:3:21: note: called from here return fibonacci(index - 1) + fibonacci(index - 2); ^ ./test.zig:3:21: note: called from here return fibonacci(index - 1) + fibonacci(index - 2); ^ ./test.zig:3:21: note: called from here return fibonacci(index - 1) + fibonacci(index - 2); ^ ./test.zig:3:21: note: called from here return fibonacci(index - 1) + fibonacci(index - 2); ^ ./test.zig:3:21: note: called from here return fibonacci(index - 1) + fibonacci(index - 2); ^ ./test.zig:3:21: note: called from here return fibonacci(index - 1) + fibonacci(index - 2); ^ ./test.zig:14:25: note: called from here assert(fibonacci(7) == 13); ^ The compiler produces an error which is a stack trace from trying to evaluate the function at compile-time. Luckily, we used an unsigned integer, and so when we tried to subtract 1 from 0, it triggered undefined behavior, which is always a compile error if the compiler knows it happened. But what would have happened if we used a signed integer? fn fibonacci(index: i32) -> i32 { //if (index < 2) return index; return fibonacci(index - 1) + fibonacci(index - 2); } fn testFibonacci() { @setFnTest(this); comptime { assert(fibonacci(7) == 13); } } fn assert(ok: bool) { if (!ok) unreachable; } ./test.zig:3:21: error: evaluation exceeded 1000 backwards branches return fibonacci(index - 1) + fibonacci(index - 2); ^ ./test.zig:3:21: note: called from here return fibonacci(index - 1) + fibonacci(index - 2); ^ ./test.zig:3:21: note: called from here return fibonacci(index - 1) + fibonacci(index - 2); ^ ./test.zig:3:21: note: called from here return fibonacci(index - 1) + fibonacci(index - 2); ^ ./test.zig:3:21: note: called from here return fibonacci(index - 1) + fibonacci(index - 2); ^ ./test.zig:3:21: note: called from here return fibonacci(index - 1) + fibonacci(index - 2); ^ ./test.zig:3:21: note: called from here return fibonacci(index - 1) + fibonacci(index - 2); ^ ./test.zig:3:21: note: called from here return fibonacci(index - 1) + fibonacci(index - 2); ^ ./test.zig:3:21: note: called from here return fibonacci(index - 1) + fibonacci(index - 2); ^ ./test.zig:3:21: note: called from here return fibonacci(index - 1) + fibonacci(index - 2); ^ ./test.zig:3:21: note: called from here return fibonacci(index - 1) + fibonacci(index - 2); ^ ./test.zig:3:21: note: called from here return fibonacci(index - 1) + fibonacci(index - 2); ^ The compiler noticed that evaluating this function at compile-time took a long time, and thus emitted a compile error and gave up. If the programmer wants to increase the budget for compile-time computation, they can use a built-in function called @setEvalBranchQuota to change the default number 1000 to something else. What if we fix the base case, but put the wrong value in the assert line? comptime { assert(fibonacci(7) == 99999); } ./test.zig:15:14: error: unable to evaluate constant expression if (!ok) unreachable; ^ ./test.zig:10:15: note: called from here assert(fibonacci(7) == 99999); ^ What happened is Zig started interpreting the assert function with the parameter ok set to false. When the interpreter hit unreachable it emitted a compile error, because reaching unreachable code is undefined behavior, and undefined behavior causes a compile error if it is detected at compile-time. In the global scope (outside of any function), all expressions are implicitly comptime expressions. This means that we can use functions to initialize complex static data. For example: const first_25_primes = firstNPrimes(25); const sum_of_first_25_primes = sum(first_25_primes); fn firstNPrimes(comptime n: usize) -> [n]i32 { var prime_list: [n]i32 = undefined; var next_index: usize = 0; var test_number: i32 = 2; while (next_index < prime_list.len) : (test_number += 1) { var test_prime_index: usize = 0; var is_prime = true; while (test_prime_index < next_index) : (test_prime_index += 1) { if (test_number % prime_list[test_prime_index] == 0) { is_prime = false; break; } } if (is_prime) { prime_list[next_index] = test_number; next_index += 1; } } return prime_list; } fn sum(numbers: []i32) -> i32 { var result: i32 = 0; for (numbers) |x| { result += x; } return result; } When we compile this program, Zig generates the constants with the answer pre-computed. Here are the lines from the generated LLVM IR: @0 = internal unnamed_addr constant [25 x i32] [i32 2, i32 3, i32 5, i32 7, i32 11, i32 13, i32 17, i32 19, i32 23, i32 29, i32 31, i32 37, i32 41, i32 43, i32 47, i32 53, i32 59, i32 61, i32 67, i32 71, i32 73, i32 79, i32 83, i32 89, i32 97] @1 = internal unnamed_addr constant i32 1060 Note that we did not have to do anything special with the syntax of these functions. For example, we could call the sum function as is with a slice of numbers whose length and values were only known at run-time. Generic Data Structures Zig uses these capabilities to implement generic data structures without introducing any special-case syntax. If you followed along so far, you may already know how to create a generic data structure. Here is an example of a generic List data structure, that we will instantiate with the type i32. Whereas in C++ or Rust we would refer to the instantiated type as List<i32>, in Zig we refer to the type as List(i32). fn List(comptime T: type) -> type { struct { items: []T, len: usize, } } That''s it. It''s a function that returns an anonymous struct. For the purposes of error messages and debugging, Zig infers the name "List(i32)" from the function name and parameters invoked when creating the anonymous struct. To keep the language small and uniform, all aggregate types in Zig are anonymous. To give a type a name, we assign it to a constant: const Node = struct { next: &Node, name: []u8, }; This works because all top level declarations are order-independent, and as long as there isn''t an actual infinite regression, values can refer to themselves, directly or indirectly. In this case, Node refers to itself as a pointer, which is not actually an infinite regression, so it works fine. Case Study: printf in C, Rust, and Zig Putting all of this together, let''s compare how printf works in C, Rust, and Zig. Here''s how printf work in C: #include <stdio.h> static const int a_number = 1234; static const char * a_string = "foobar"; int main(int argc, char **argv) { fprintf(stderr, "here is a string: ''%s'' here is a number: %d\n", a_string, a_number); return 0; } here is a string: ''foobar'' here is a number: 1234 What happens here is the printf implementation iterates over the format string at run-time, and when it encounters a format specifier such as %d it looks at the next argument which is passed in an architecture-specific way, interprets the argument as a type depending on the format specifier, and attempts to print it. If the types are incorrect or not enough arguments are passed, undefined behavior occurs - it may crash, print garbage data, or access invalid memory. Luckily, the compiler defines an attribute that you can use like this: __attribute__ ((format (printf, x, y))); Where x and y are the 1-based indexes of the argument parameters that correspond to the format string and the first var args parameter, respectively. This attribute adds type checking to the function it decorates, to prevent the above problems, and the printf function from stdio.h has this attribute on it, so these problems are solved. But what if you want to invent your own format string syntax and have the compiler check it for you? You can''t. That''s how it works in C. It is hard-coded into the compiler. If you wanted to write your own format string printing code and have it checked by the compiler, you would have to use the preprocessor or metaprogramming - generate C code as output from some other code. Zig is a programming language which is intended to replace C. We can do better than this. Here''s the equivalent program in Zig: const io = @import("std").io; const a_number: i32 = 1234; const a_string = "foobar"; pub fn main(args: [][]u8) -> %void { %%io.stderr.printf("here is a string: ''{}'' here is a number: {}\n", a_string, a_number); } here is a string: ''foobar'' here is a number: 1234 Let''s crack open the implementation of this and see how it works: /// Calls print and then flushes the buffer. pub fn printf(self: &OutStream, comptime format: []const u8, args: ...) -> %void { const State = enum { Start, OpenBrace, CloseBrace, }; comptime var start_index: usize = 0; comptime var state = State.Start; comptime var next_arg: usize = 0; inline for (format) |c, i| { switch (state) { State.Start => switch (c) { ''{'' => { if (start_index < i) %return self.write(format[start_index...i]); state = State.OpenBrace; }, ''}'' => { if (start_index < i) %return self.write(format[start_index...i]); state = State.CloseBrace; }, else => {}, }, State.OpenBrace => switch (c) { ''{'' => { state = State.Start; start_index = i; }, ''}'' => { %return self.printValue(args[next_arg]); next_arg += 1; state = State.Start; start_index = i + 1; }, else => @compileError("Unknown format character: " ++ c), }, State.CloseBrace => switch (c) { ''}'' => { state = State.Start; start_index = i; }, else => @compileError("Single ''}'' encountered in format string"), }, } } comptime { if (args.len != next_arg) { @compileError("Unused arguments"); } if (state != State.Start) { @compileError("Incomplete format string: " ++ format); } } if (start_index < format.len) { %return self.write(format[start_index...format.len]); } %return self.flush(); } This is a proof of concept implementation; it will gain more formatting capabilities before Zig reaches its first release. Note that this is not hard-coded into the Zig compiler; this userland code in the standard library. When this function is analyzed from our example code above, Zig partially evaluates the function and emits a function that actually looks like this: pub fn printf(self: &OutStream, arg0: i32, arg1: []const u8) -> %void { %return self.write("here is a string: ''"); %return self.printValue(arg0); %return self.write("'' here is a number: "); %return self.printValue(arg1); %return self.write("\n"); %return self.flush(); } printValue is a function that takes a parameter of any type, and does different things depending on the type: pub fn printValue(self: &OutStream, value: var) -> %void { const T = @typeOf(value); if (@isInteger(T)) { return self.printInt(T, value); } else if (@isFloat(T)) { return self.printFloat(T, value); } else if (@canImplicitCast([]const u8, value)) { const casted_value = ([]const u8)(value); return self.write(casted_value); } else { @compileError("Unable to print type ''" ++ @typeName(T) ++ "''"); } } And now, what happens if we give too many arguments to printf? %%io.stdout.printf("here is a string: ''{}'' here is a number: {}\n", a_string, a_number, a_number); .../std/io.zig:147:17: error: Unused arguments @compileError("Unused arguments"); ^ ./test.zig:7:23: note: called from here %%io.stdout.printf("here is a number: {} and here is a string: {}\n", ^ Zig gives programmers the tools needed to protect themselves against their own mistakes. Let''s take a look at how Rust handles this problem. Here''s the equivalent program: const A_NUMBER: i32 = 1234; const A_STRING: &''static str = "foobar"; fn main() { print!("here is a string: ''{}'' here is a number: {}\n", A_STRING, A_NUMBER); } here is a string: ''foobar'' here is a number: 1234 print!, as evidenced by the exclamation point, is a macro. Here is the definition: #[macro_export] #[stable(feature = "rust1", since = "1.0.0")] #[allow_internal_unstable] macro_rules! print { ($($arg:tt)*) => ($crate::io::_print(format_args!($($arg)*))); } #[stable(feature = "rust1", since = "1.0.0")] #[macro_export] macro_rules! format_args { ($fmt:expr, $($args:tt)*) => ({ /* compiler built-in */ }) } Rust accomplishes the syntax that one would want from a var args print implementation, but it requires using a macro to do so. Macros have some limitations. For example, in this case, if you move the format string to a global variable, the Rust example can no longer compile: const A_NUMBER: i32 = 1234; const A_STRING: &''static str = "foobar"; const FMT: &''static str = "here is a string: ''{}'' here is a number: {}\n"; fn main() { print!(FMT, A_STRING, A_NUMBER); } error: format argument must be a string literal. --> test.rs:6:12 | 6 | print!(FMT, A_STRING, A_NUMBER); | ^^^ On the other hand, Zig doesn''t care whether the format argument is a string literal, only that it is a compile-time known value that is implicitly castable to a []const u8: const io = @import("std").io; const a_number: i32 = 1234; const a_string = "foobar"; const fmt = "here is a string: ''{}'' here is a number: {}\n"; pub fn main(args: [][]u8) -> %void { %%io.stderr.printf(fmt, a_string, a_number); } This works fine. A macro is a reasonable solution to this problem, but it comes at the cost of readability. From Rust''s own documentation: The drawback is that macro-based code can be harder to understand, because fewer of the built-in rules apply. Like an ordinary function, a well-behaved macro can be used without understanding its implementation. However, it can be difficult to design a well-behaved macro! Additionally, compiler errors in macro code are harder to interpret, because they describe problems in the expanded code, not the source-level form that developers use. These drawbacks make macros something of a "feature of last resort". That’s not to say that macros are bad; they are part of Rust because sometimes they’re needed for truly concise, well-abstracted code. Just keep this tradeoff in mind. One of the goals of Zig is to avoid these drawbacks while still providing enough of the power that macros provide in order to make them unnecessary. There is another thing I noticed, and I hope someone from the Rust community can correct me if I''m wrong, but it looks like Rust also special cased format_args! in the compiler by making it a built-in. If my understanding is correct, this would make Zig stand out as the only language of the three mentioned here which does not special case string formatting in the compiler and instead exposes enough power to accomplish this task in userland. But more importantly, it does so without introducing another language on top of Zig, such as a macro language or a preprocessor language. It''s Zig all the way down. Conclusion Thank you for following along and checking out what I''ve been working on lately. As always, I welcome discussion, criticism, and users. Please keep in mind that this is alpha software; I am working toward a first beta release, but the project is not there yet.'
content: extracted
html: 2017-01-30-zig-programming-language-blurs-the-line-between-compile.html
---

[Andrew Kelley](https://andrewkelley.me/) - Zig Programming Language Blurs the Line Between Compile-Time and Run-Time (2017 Jan 30)

Zig places importance on the concept of whether an expression is known at compile-time. There are a few different places this concept is used, and these building blocks are used to keep the language small, readable, and powerful.

- [Introducing the Compile-Time Concept](https://andrewkelley.me/post/zig-programming-language-blurs-line-compile-time-run-time.html#introducing-compile-time-concept)

- [Compile-time parameters](https://andrewkelley.me/post/zig-programming-language-blurs-line-compile-time-run-time.html#compile-time-parameters)
- [Compile-time variables](https://andrewkelley.me/post/zig-programming-language-blurs-line-compile-time-run-time.html#compile-time-variables)
- [Compile-time expressions](https://andrewkelley.me/post/zig-programming-language-blurs-line-compile-time-run-time.html#compile-time-expressions)

- [Generic Data Structures](https://andrewkelley.me/post/zig-programming-language-blurs-line-compile-time-run-time.html#generic-data-structures)
- [Case Study: printf in C, Rust, and Zig](https://andrewkelley.me/post/zig-programming-language-blurs-line-compile-time-run-time.html#case-study-printf)
- [Conclusion](https://andrewkelley.me/post/zig-programming-language-blurs-line-compile-time-run-time.html#conclusion)

### Introducing the Compile-Time Concept

#### Compile-Time Parameters

Compile-time parameters is how Zig implements generics. It is compile-time duck typing and it works mostly the same way that C++ template parameters work. Example:

```
fn max(comptime T: type, a: T, b: T) -> T {
    if (a > b) a else b
}
fn gimmeTheBiggerFloat(a: f32, b: f32) -> f32 {
    max(f32, a, b)
}
fn gimmeTheBiggerInteger(a: u64, b: u64) -> u64 {
    max(u64, a, b)
}
```

In Zig, types are first-class citizens. They can be assigned to variables, passed as parameters to functions, and returned from functions. However, they can only be used in expressions which are known at *compile-time*, which is why the parameter `T` in the above snippet must be marked with `comptime`.

A `comptime` parameter means that:

- At the callsite, the value must be known at compile-time, or it is a compile error.
- In the function definition, the value is known at compile-time.

For example, if we were to introduce another function to the above snippet:

```
fn max(comptime T: type, a: T, b: T) -> T {
    if (a > b) a else b
}
fn letsTryToPassARuntimeType(condition: bool) {
    const result = max(
        if (condition) f32 else u64,
        1234,
        5678);
}
```

Then we get this result from the compiler:

```
./test.zig:6:9: error: unable to evaluate constant expression
        if (condition) f32 else u64,
        ^
```

This is an error because the programmer attempted to pass a value only known at run-time to a function which expects a value known at compile-time.

Another way to get an error is if we pass a type that violates the type checker when the function is analyzed. This is what it means to have *compile-time duck typing*.

For example:

```
fn max(comptime T: type, a: T, b: T) -> T {
    if (a > b) a else b
}
fn letsTryToCompareBools(a: bool, b: bool) -> bool {
    max(bool, a, b)
}
```

The code produces this error message:

```
./test.zig:2:11: error: operator not allowed for type 'bool'
    if (a > b) a else b
          ^
./test.zig:5:8: note: called from here
    max(bool, a, b)
       ^
```

On the flip side, inside the function definition with the `comptime` parameter, the value is known at compile-time. This means that we actually could make this work for the bool type if we wanted to:

```
fn max(comptime T: type, a: T, b: T) -> T {
    if (T == bool) {
        return a or b;
    } else if (a > b) {
        return a;
    } else {
        return b;
    }
}
fn letsTryToCompareBools(a: bool, b: bool) -> bool {
    max(bool, a, b)
}
```

This works because Zig implicitly inlines `if` expressions when the condition is known at compile-time, and the compiler guarantees that it will skip analysis of the branch not taken.

This means that the actual function generated for `max` in this situation looks like this:

```
fn max(a: bool, b: bool) -> bool {
    return a or b;
}
```

All the code that dealt with compile-time known values is eliminated and we are left with only the necessary run-time code to accomplish the task.

This works the same way for `switch` expressions - they are implicitly inlined when the target expression is compile-time known.

#### Compile-Time Variables

In Zig, the programmer can label variables as `comptime`. This guarantees to the compiler that every load and store of the variable is performed at compile-time. Any violation of this results in a compile error.

This combined with the fact that we can `inline` loops allows us to write a function which is partially evaluated at compile-time and partially at run-time.

For example:

```
const CmdFn = struct {
    name: []const u8,
    func: fn(i32) -> i32,
};

const cmd_fns = []CmdFn{
    CmdFn {.name = "one", .func = one},
    CmdFn {.name = "two", .func = two},
    CmdFn {.name = "three", .func = three},
};
fn one(value: i32) -> i32 { value + 1 }
fn two(value: i32) -> i32 { value + 2 }
fn three(value: i32) -> i32 { value + 3 }

fn performFn(comptime prefix_char: u8, start_value: i32) -> i32 {
    var result: i32 = start_value;
    comptime var i = 0;
    inline while (i < cmd_fns.len) : (i += 1) {
        if (cmd_fns[i].name[0] == prefix_char) {
            result = cmd_fns[i].func(result);
        }
    }
    return result;
}

fn testPerformFn() {
    @setFnTest(this);

    assert(performFn('t', 1) == 6);
    assert(performFn('o', 0) == 1);
    assert(performFn('w', 99) == 99);
}

fn assert(ok: bool) {
    if (!ok) unreachable;
}
```

This example is a bit contrived, because the compile-time evaluation component is unnecessary; this code would work fine if it was all done at run-time. But it does end up generating different code. In this example, the function `performFn` is generated three different times, for the different values of `prefix_char` provided:

```
// From the line:
// assert(performFn('t', 1) == 6);
fn performFn(start_value: i32) -> i32 {
    var result: i32 = start_value;
    result = two(result);
    result = three(result);
    return result;
}

// From the line:
// assert(performFn('o', 0) == 1);
fn performFn(start_value: i32) -> i32 {
    var result: i32 = start_value;
    result = one(result);
    return result;
}

// From the line:
// assert(performFn('w', 99) == 99);
fn performFn(start_value: i32) -> i32 {
    var result: i32 = start_value;
    return result;
}
```

Note that this happens even in a debug build; in a release build these generated functions still pass through rigorous LLVM optimizations. The important thing to note, however, is not that this is a way to write more optimized code, but that it is a way to make sure that what *should* happen at compile-time, *does* happen at compile-time. This catches more errors and as demonstrated later in this article, allows expressiveness that in other languages requires using macros, generated code, or a preprocessor to accomplish.

#### Compile-Time Expressions

In Zig, it matters whether a given expression is known at compile-time or run-time. A programmer can use a `comptime` expression to guarantee that the expression will be evaluated at compile-time. If this cannot be accomplished, the compiler will emit an error. For example:

```
extern fn exit() -> unreachable;

fn foo() {
    comptime {
        exit();
    }
}
```

```
./test.zig:5:9: error: unable to evaluate constant expression
        exit();
        ^
```

It doesn't make sense that a program could call `exit()` (or any other external function) at compile-time, so this is a compile error. However, a `comptime` expression does much more than sometimes cause a compile error.

Within a `comptime` expression:

- All variables are `comptime` variables.
- All `if`, `while`, `for`, `switch`, and `goto` expressions are evaluated at compile-time, or emit a compile error if this is not possible.
- All function calls cause the compiler to interpret the function at compile-time, emitting a compile error if the function tries to do something that has global run-time side effects.

This means that a programmer can create a function which is called both at compile-time and run-time, with no modification to the function required.

Let's look at an example:

```
fn fibonacci(index: u32) -> u32 {
    if (index < 2) return index;
    return fibonacci(index - 1) + fibonacci(index - 2);
}

fn testFibonacci() {
    @setFnTest(this);

    // test fibonacci at run-time
    assert(fibonacci(7) == 13);

    // test fibonacci at compile-time
    comptime {
        assert(fibonacci(7) == 13);
    }
}

fn assert(ok: bool) {
    if (!ok) unreachable;
}
```

```
$ zig test test.zig
Test 1/1 testFibonacci...OK
```

Imagine if we had forgotten the base case of the recursive function and tried to run the tests:

```
fn fibonacci(index: u32) -> u32 {
    //if (index < 2) return index;
    return fibonacci(index - 1) + fibonacci(index - 2);
}

fn testFibonacci() {
    @setFnTest(this);

    comptime {
        assert(fibonacci(7) == 13);
    }
}

fn assert(ok: bool) {
    if (!ok) unreachable;
}
```

```
$ zig test test.zig
./test.zig:3:28: error: operation caused overflow
    return fibonacci(index - 1) + fibonacci(index - 2);
                           ^
./test.zig:3:21: note: called from here
    return fibonacci(index - 1) + fibonacci(index - 2);
                    ^
./test.zig:3:21: note: called from here
    return fibonacci(index - 1) + fibonacci(index - 2);
                    ^
./test.zig:3:21: note: called from here
    return fibonacci(index - 1) + fibonacci(index - 2);
                    ^
./test.zig:3:21: note: called from here
    return fibonacci(index - 1) + fibonacci(index - 2);
                    ^
./test.zig:3:21: note: called from here
    return fibonacci(index - 1) + fibonacci(index - 2);
                    ^
./test.zig:3:21: note: called from here
    return fibonacci(index - 1) + fibonacci(index - 2);
                    ^
./test.zig:3:21: note: called from here
    return fibonacci(index - 1) + fibonacci(index - 2);
                    ^
./test.zig:14:25: note: called from here
        assert(fibonacci(7) == 13);
                        ^
```

The compiler produces an error which is a stack trace from trying to evaluate the function at compile-time.

Luckily, we used an unsigned integer, and so when we tried to subtract 1 from 0, it triggered undefined behavior, which is always a compile error if the compiler knows it happened. But what would have happened if we used a signed integer?

```
fn fibonacci(index: i32) -> i32 {
    //if (index < 2) return index;
    return fibonacci(index - 1) + fibonacci(index - 2);
}

fn testFibonacci() {
    @setFnTest(this);

    comptime {
        assert(fibonacci(7) == 13);
    }
}

fn assert(ok: bool) {
    if (!ok) unreachable;
}
```

```
./test.zig:3:21: error: evaluation exceeded 1000 backwards branches
    return fibonacci(index - 1) + fibonacci(index - 2);
                    ^
./test.zig:3:21: note: called from here
    return fibonacci(index - 1) + fibonacci(index - 2);
                    ^
./test.zig:3:21: note: called from here
    return fibonacci(index - 1) + fibonacci(index - 2);
                    ^
./test.zig:3:21: note: called from here
    return fibonacci(index - 1) + fibonacci(index - 2);
                    ^
./test.zig:3:21: note: called from here
    return fibonacci(index - 1) + fibonacci(index - 2);
                    ^
./test.zig:3:21: note: called from here
    return fibonacci(index - 1) + fibonacci(index - 2);
                    ^
./test.zig:3:21: note: called from here
    return fibonacci(index - 1) + fibonacci(index - 2);
                    ^
./test.zig:3:21: note: called from here
    return fibonacci(index - 1) + fibonacci(index - 2);
                    ^
./test.zig:3:21: note: called from here
    return fibonacci(index - 1) + fibonacci(index - 2);
                    ^
./test.zig:3:21: note: called from here
    return fibonacci(index - 1) + fibonacci(index - 2);
                    ^
./test.zig:3:21: note: called from here
    return fibonacci(index - 1) + fibonacci(index - 2);
                    ^
./test.zig:3:21: note: called from here
    return fibonacci(index - 1) + fibonacci(index - 2);
                    ^
```

The compiler noticed that evaluating this function at compile-time took a long time, and thus emitted a compile error and gave up. If the programmer wants to increase the budget for compile-time computation, they can use a built-in function called `@setEvalBranchQuota` to change the default number 1000 to something else.

What if we fix the base case, but put the wrong value in the `assert` line?

```
comptime {
    assert(fibonacci(7) == 99999);
}
```

```
./test.zig:15:14: error: unable to evaluate constant expression
    if (!ok) unreachable;
             ^
./test.zig:10:15: note: called from here
        assert(fibonacci(7) == 99999);
              ^
```

What happened is Zig started interpreting the `assert` function with the parameter `ok` set to `false`. When the interpreter hit `unreachable` it emitted a compile error, because reaching unreachable code is undefined behavior, and undefined behavior causes a compile error if it is detected at compile-time.

In the global scope (outside of any function), all expressions are implicitly `comptime` expressions. This means that we can use functions to initialize complex static data. For example:

```
const first_25_primes = firstNPrimes(25);
const sum_of_first_25_primes = sum(first_25_primes);

fn firstNPrimes(comptime n: usize) -> [n]i32 {
    var prime_list: [n]i32 = undefined;
    var next_index: usize = 0;
    var test_number: i32 = 2;
    while (next_index < prime_list.len) : (test_number += 1) {
        var test_prime_index: usize = 0;
        var is_prime = true;
        while (test_prime_index < next_index) : (test_prime_index += 1) {
            if (test_number % prime_list[test_prime_index] == 0) {
                is_prime = false;
                break;
            }
        }
        if (is_prime) {
            prime_list[next_index] = test_number;
            next_index += 1;
        }
    }
    return prime_list;
}

fn sum(numbers: []i32) -> i32 {
    var result: i32 = 0;
    for (numbers) |x| {
        result += x;
    }
    return result;
}
```

When we compile this program, Zig generates the constants with the answer pre-computed. Here are the lines from the generated LLVM IR:

```
@0 = internal unnamed_addr constant [25 x i32] [i32 2, i32 3, i32 5, i32 7, i32 11, i32 13, i32 17, i32 19, i32 23, i32 29, i32 31, i32 37, i32 41, i32 43, i32 47, i32 53, i32 59, i32 61, i32 67, i32 71, i32 73, i32 79, i32 83, i32 89, i32 97]
@1 = internal unnamed_addr constant i32 1060
```

Note that we did not have to do anything special with the syntax of these functions. For example, we could call the `sum` function as is with a slice of numbers whose length and values were only known at run-time.

### Generic Data Structures

Zig uses these capabilities to implement generic data structures without introducing any special-case syntax. If you followed along so far, you may already know how to create a generic data structure.

Here is an example of a generic `List` data structure, that we will instantiate with the type `i32`. Whereas in C++ or Rust we would refer to the instantiated type as `List<i32>`, in Zig we refer to the type as `List(i32)`.

```
fn List(comptime T: type) -> type {
    struct {
        items: []T,
        len: usize,
    }
}
```

That's it. It's a function that returns an anonymous `struct`. For the purposes of error messages and debugging, Zig infers the name `"List(i32)"` from the function name and parameters invoked when creating the anonymous struct.

To keep the language small and uniform, all aggregate types in Zig are anonymous. To give a type a name, we assign it to a constant:

```
const Node = struct {
    next: &Node,
    name: []u8,
};
```

This works because all top level declarations are order-independent, and as long as there isn't an actual infinite regression, values can refer to themselves, directly or indirectly. In this case, `Node` refers to itself as a pointer, which is not actually an infinite regression, so it works fine.

### Case Study: printf in C, Rust, and Zig

Putting all of this together, let's compare how `printf` works in C, Rust, and Zig.

Here's how `printf` work in C:

```
#include <stdio.h>

static const int a_number = 1234;
static const char * a_string = "foobar";

int main(int argc, char **argv) {
    fprintf(stderr, "here is a string: '%s' here is a number: %d\n", a_string, a_number);
    return 0;
}
```

```
here is a string: 'foobar' here is a number: 1234
```

What happens here is the `printf` implementation iterates over the format string at run-time, and when it encounters a format specifier such as `%d` it looks at the next argument which is passed in an architecture-specific way, interprets the argument as a type depending on the format specifier, and attempts to print it. If the types are incorrect or not enough arguments are passed, undefined behavior occurs - it may crash, print garbage data, or access invalid memory.

Luckily, the compiler defines an attribute that you can use like this:

```
__attribute__ ((format (printf, x, y)));
```

Where x and y are the 1-based indexes of the argument parameters that correspond to the format string and the first var args parameter, respectively.

This attribute adds type checking to the function it decorates, to prevent the above problems, and the `printf` function from `stdio.h` has this attribute on it, so these problems are solved.

But what if you want to invent your own format string syntax and have the compiler check it for you?

You can't.

That's how it works in C. It is hard-coded into the compiler. If you wanted to write your own format string printing code and have it checked by the compiler, you would have to use the preprocessor or metaprogramming - generate C code as output from some other code.

Zig is a programming language which is intended to replace C. We can do better than this.

Here's the equivalent program in Zig:

```
const io = @import("std").io;

const a_number: i32 = 1234;
const a_string = "foobar";

pub fn main(args: [][]u8) -> %void {
    %%io.stderr.printf("here is a string: '{}' here is a number: {}\n", a_string, a_number);
}
```

```
here is a string: 'foobar' here is a number: 1234
```

Let's crack open the implementation of this and see how it works:

```
/// Calls print and then flushes the buffer.
pub fn printf(self: &OutStream, comptime format: []const u8, args: ...) -> %void {
    const State = enum {
        Start,
        OpenBrace,
        CloseBrace,
    };

    comptime var start_index: usize = 0;
    comptime var state = State.Start;
    comptime var next_arg: usize = 0;

    inline for (format) |c, i| {
        switch (state) {
            State.Start => switch (c) {
                '{' => {
                    if (start_index < i) %return self.write(format[start_index...i]);
                    state = State.OpenBrace;
                },
                '}' => {
                    if (start_index < i) %return self.write(format[start_index...i]);
                    state = State.CloseBrace;
                },
                else => {},
            },
            State.OpenBrace => switch (c) {
                '{' => {
                    state = State.Start;
                    start_index = i;
                },
                '}' => {
                    %return self.printValue(args[next_arg]);
                    next_arg += 1;
                    state = State.Start;
                    start_index = i + 1;
                },
                else => @compileError("Unknown format character: " ++ c),
            },
            State.CloseBrace => switch (c) {
                '}' => {
                    state = State.Start;
                    start_index = i;
                },
                else => @compileError("Single '}' encountered in format string"),
            },
        }
    }
    comptime {
        if (args.len != next_arg) {
            @compileError("Unused arguments");
        }
        if (state != State.Start) {
            @compileError("Incomplete format string: " ++ format);
        }
    }
    if (start_index < format.len) {
        %return self.write(format[start_index...format.len]);
    }
    %return self.flush();
}
```

This is a proof of concept implementation; it will gain more formatting capabilities before Zig reaches its first release.

Note that this is not hard-coded into the Zig compiler; this userland code in the standard library.

When this function is analyzed from our example code above, Zig partially evaluates the function and emits a function that actually looks like this:

```
pub fn printf(self: &OutStream, arg0: i32, arg1: []const u8) -> %void {
    %return self.write("here is a string: '");
    %return self.printValue(arg0);
    %return self.write("' here is a number: ");
    %return self.printValue(arg1);
    %return self.write("\n");
    %return self.flush();
}
```

`printValue` is a function that takes a parameter of any type, and does different things depending on the type:

```
pub fn printValue(self: &OutStream, value: var) -> %void {
    const T = @typeOf(value);
    if (@isInteger(T)) {
        return self.printInt(T, value);
    } else if (@isFloat(T)) {
        return self.printFloat(T, value);
    } else if (@canImplicitCast([]const u8, value)) {
        const casted_value = ([]const u8)(value);
        return self.write(casted_value);
    } else {
        @compileError("Unable to print type '" ++ @typeName(T) ++ "'");
    }
}
```

And now, what happens if we give too many arguments to `printf`?

```
%%io.stdout.printf("here is a string: '{}' here is a number: {}\n",
        a_string, a_number, a_number);
```

```
.../std/io.zig:147:17: error: Unused arguments
                @compileError("Unused arguments");
                ^
./test.zig:7:23: note: called from here
    %%io.stdout.printf("here is a number: {} and here is a string: {}\n",
                      ^
```

Zig gives programmers the tools needed to protect themselves against their own mistakes.

Let's take a look at how [Rust](https://www.rust-lang.org/en-US/) handles this problem. Here's the equivalent program:

```
const A_NUMBER: i32 = 1234;
const A_STRING: &'static str = "foobar";

fn main() {
    print!("here is a string: '{}' here is a number: {}\n",
        A_STRING, A_NUMBER);
}
```

```
here is a string: 'foobar' here is a number: 1234
```

`print!`, as evidenced by the exclamation point, is a macro. Here is the definition:

```
#[macro_export]
#[stable(feature = "rust1", since = "1.0.0")]
#[allow_internal_unstable]
macro_rules! print {
    ($($arg:tt)*) => ($crate::io::_print(format_args!($($arg)*)));
}
#[stable(feature = "rust1", since = "1.0.0")]
#[macro_export]
macro_rules! format_args { ($fmt:expr, $($args:tt)*) => ({
/* compiler built-in */
}) }
```

Rust accomplishes the syntax that one would want from a var args print implementation, but it requires using a macro to do so.

Macros have some limitations. For example, in this case, if you move the format string to a global variable, the Rust example can no longer compile:

```
const A_NUMBER: i32 = 1234;
const A_STRING: &'static str = "foobar";
const FMT: &'static str = "here is a string: '{}' here is a number: {}\n";

fn main() {
    print!(FMT, A_STRING, A_NUMBER);
}
```

```
error: format argument must be a string literal.
 --> test.rs:6:12
  |
6 |     print!(FMT, A_STRING, A_NUMBER);
  |            ^^^
```

On the other hand, Zig doesn't care whether the format argument is a string literal, only that it is a compile-time known value that is implicitly castable to a `[]const u8`:

```
const io = @import("std").io;

const a_number: i32 = 1234;
const a_string = "foobar";
const fmt = "here is a string: '{}' here is a number: {}\n";

pub fn main(args: [][]u8) -> %void {
    %%io.stderr.printf(fmt, a_string, a_number);
}
```

This works fine.

A macro is a reasonable solution to this problem, but it comes at the cost of readability. From [Rust's own documentation](https://doc.rust-lang.org/beta/book/macros.html):

> The drawback is that macro-based code can be harder to understand, because fewer of the built-in rules apply. Like an ordinary function, a well-behaved macro can be used without understanding its implementation. However, it can be difficult to design a well-behaved macro! Additionally, compiler errors in macro code are harder to interpret, because they describe problems in the expanded code, not the source-level form that developers use.
>
> These drawbacks make macros something of a "feature of last resort". That’s not to say that macros are bad; they are part of Rust because sometimes they’re needed for truly concise, well-abstracted code. Just keep this tradeoff in mind.

One of the goals of Zig is to avoid these drawbacks while still providing enough of the power that macros provide in order to make them unnecessary.

There is another thing I noticed, and I hope someone from the Rust community can correct me if I'm wrong, but it looks like Rust also special cased `format_args!` in the compiler by making it a built-in. If my understanding is correct, this would make Zig stand out as the only language of the three mentioned here which does not special case string formatting in the compiler and instead exposes enough power to accomplish this task in userland.

But more importantly, it does so without introducing another language on top of Zig, such as a macro language or a preprocessor language. It's Zig all the way down.

### Conclusion

Thank you for following along and checking out what I've been working on lately.

As always, I welcome discussion, criticism, and users. Please keep in mind that this is alpha software; I am working toward a first beta release, but the project is not there yet.

Thanks for reading my blog post.
