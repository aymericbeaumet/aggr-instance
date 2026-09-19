---
title: 'Zig: Already More Knowable Than C'
link: https://andrewkelley.me/post/zig-already-more-knowable-than-c.html
source: andrewkelley-me
published: 2017-02-14T04:49:59Z
updated: 2017-02-14T04:49:59Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'Zig: Already More Knowable Than C There is a nifty article created back in 2015 that made its way onto Hacker News today: I Do Not Know C. The author creates a delightful set of source code samples in which the reader is intended to determine the correctness, and if correct, predict the output of the provided code. If you have not done the exercises, I encourage you to take a moment to do so. What follows are some of the examples translated into Zig for comparison. The numbers correspond to the numbers from the original post linked above, but the C code is embedded here for comparison. 1. Declaring the same variable twice int i; int i = 10; In Zig: var i = undefined; var i = 10; Output: ./test.zig:2:1: error: redefinition of ''i'' var i = 10; ^ ./test.zig:1:1: note: previous definition is here var i = undefined; ^ 2. Null pointer extern void bar(void); void foo(int *x) { int y = *x; /* (1) */ if (!x) { /* (2) */ return; /* (3) */ } bar(); } When this example is translated to Zig, you have to explicitly decide if the pointer is nullable. For example, if you used a bare pointer in Zig: extern fn bar(); export fn foo(x: &c_int) { var y = *x; // (1) if (x == null) { // (2) return; // (3) } bar(); } Then it wouldn''t even compile: ./test.zig:7:11: error: operator not allowed for type ''?&c_int'' if (x == null) { // (2) ^ I think this error message can be improved, but even so it prevents a possible null related bug here. If the code author makes the pointer nullable, then the natural way to port the C code to Zig would be: extern fn bar(); export fn foo(x: ?&c_int) { var y = x ?? return; // (1), (2), (3) bar(); } This compiles to: 0000000000000000 <foo>: 0: 48 85 ff test %rdi,%rdi 3: 74 05 je a <foo+0xa> 5: e9 00 00 00 00 jmpq a <foo+0xa> a: c3 retq This does a null check, returns if null, and otherwise calls bar. Perhaps a more faithful way to port the C code to Zig would be this: extern fn bar(); fn foo(x: ?&c_int) { var y = ??x; // (1) if (x == null) { // (2) return; // (3) } bar(); } pub fn main() -> %void { foo(null); } The ?? operator unwraps the nullable value. It asserts that the value is not null, and returns the value. If the value is null then the behavior is undefined, just like in C. However, in Zig, undefined behavior causes a crash in debug mode: $ ./test attempt to unwrap null /home/andy/dev/zig/build/lib/zig/std/special/zigrt.zig:16:35: 0x0000000000203395 in ??? (test) @import("std").debug.panic("{}", message_ptr[0..message_len]); ^ /home/andy/tmp/test.zig:4:13: 0x000000000020a61d in ??? (test) var y = ??x; // (1) ^ /home/andy/tmp/test.zig:12:8: 0x00000000002046fd in ??? (test) foo(null); ^ /home/andy/dev/zig/build/lib/zig/std/special/bootstrap.zig:60:21: 0x0000000000203697 in ??? (test) return root.main(); ^ /home/andy/dev/zig/build/lib/zig/std/special/bootstrap.zig:47:13: 0x0000000000203420 in ??? (test) callMain(argc, argv, envp) %% std.os.posix.exit(1); ^ /home/andy/dev/zig/build/lib/zig/std/special/bootstrap.zig:34:25: 0x0000000000203290 in ??? (test) posixCallMainAndExit() ^ Aborted This is a half-finished traceback implementation (lots of TODO items to complete before the 0.1.0 milestone), but the point is that Zig detected the undefined behavior and aborted. In release mode, this example invokes undefined behavior just like in C. To avoid this, programmers are expected to choose one of these options: Test code sufficiently in debug mode to catch undefined behavior abuse. Utilize the safe-release option which includes the runtime undefined behavior safety checks. 5. strlen int my_strlen(const char *x) { int res = 0; while(*x) { res++; x++; } return res; } In Zig, pointers generally point to single objects, while slices are used to refer to ranges of memory. So in practice you wouldn''t need a strlen function, you would use some_bytes.len. But we can port this code over anyway: export fn my_strlen(x: &const u8) -> c_int { var res: c_int = 0; while (x[res] != 0) { res += 1; } return res; } Here we must use pointer indexing because Zig does not support direct pointer arithmetic. The compiler catches this problem: ./test.zig:3:14: error: expected type ''usize'', found ''c_int'' while (x[res] != 0) { ^ 6. Print string of bytes backwards #include <stdio.h> #include <string.h> int main() { const char *str = "hello"; size_t length = strlen(str); size_t i; for(i = length - 1; i >= 0; i--) { putchar(str[i]); } putchar(''\n''); return 0; } Ported to Zig: const c = @cImport({ @cInclude("stdio.h"); @cInclude("string.h"); }); export fn main(argc: c_int, argv: &&u8) -> c_int { const str = c"hello"; const length: c.size_t = c.strlen(str); var i: c.size_t = length - 1; while (i >= 0) : (i -= 1) { _ = c.putchar(str[i]); } _ = c.putchar(''\n''); return 0; } It compiles fine but produces this output when run: integer overflow test.zig:10:25: 0x000000000020346d in ??? (test) while (i >= 0) : (i -= 1) { ^ Aborted 8. Weirdo syntax #include <stdio.h> int main() { int array[] = { 0, 1, 2 }; printf("%d %d %d\n", 10, (5, array[1, 2]), 10); } There is no way to express this code in Zig. Good riddance. 9. Unsigned overflow unsigned int add(unsigned int a, unsigned int b) { return a + b; } In Zig: const io = @import("std").io; export fn add(a: c_uint, b: c_uint) -> c_uint { return a + b; } pub fn main() -> %void { %%io.stdout.printf("{}\n", add(@maxValue(c_uint), 1)); } Output: $ ./test integer overflow test.zig:4:14: 0x00000000002032b4 in ??? (test) return a + b; ^ test.zig:8:35: 0x0000000000204747 in ??? (test) %%io.stdout.printf("{}\n", add(@maxValue(c_uint), 1)); ^ lib/zig/std/special/bootstrap.zig:60:21: 0x00000000002036d7 in ??? (test) return root.main(); ^ lib/zig/std/special/bootstrap.zig:47:13: 0x0000000000203460 in ??? (test) callMain(argc, argv, envp) %% std.os.posix.exit(1); ^ lib/zig/std/special/bootstrap.zig:34:25: 0x00000000002032d0 in ??? (test) posixCallMainAndExit() ^ Aborted The + operator asserts that there will be no overflow. If you want twos complement wraparound behavior, that is possible with the +% operator instead: export fn add(a: c_uint, b: c_uint) -> c_uint { return a +% b; } Now the output is: $ ./test 0 10. Signed overflow int add(int a, int b) { return a + b; } In C signed and unsigned integer overflow work differently. In Zig, they work the same. + asserts that no overflow occurs, and +% performs twos complement wraparound behavior. 11. Negation overflow int neg(int a) { return -a; } By now you can probably predict how this works in Zig. const io = @import("std").io; export fn neg(a: c_int) -> c_int { return -a; } pub fn main() -> %void { %%io.stdout.printf("{}\n", neg(@minValue(c_int))); } Output: $ ./test integer overflow test.zig:4:12: 0x00000000002032b0 in ??? (test) return -a; ^ test.zig:8:35: 0x0000000000204742 in ??? (test) %%io.stdout.printf("{}\n", neg(@minValue(c_int))); ^ lib/zig/std/special/bootstrap.zig:60:21: 0x00000000002036d7 in ??? (test) return root.main(); ^ lib/zig/std/special/bootstrap.zig:47:13: 0x0000000000203460 in ??? (test) callMain(argc, argv, envp) %% std.os.posix.exit(1); ^ lib/zig/std/special/bootstrap.zig:34:25: 0x00000000002032d0 in ??? (test) posixCallMainAndExit() ^ Aborted The -% wraparound variant of the negation operator works here too: export fn neg(a: c_int) -> c_int { return -%a; } Output: $ ./test -2147483648 12. Division overflow int div(int a, int b) { assert(b != 0); return a / b; } Different operation, same deal. const io = @import("std").io; fn div(a: i32, b: i32) -> i32 { return a / b; } pub fn main() -> %void { %%io.stdout.printf("{}\n", div(@minValue(i32), -1)); } First of all, Zig doesn''t let us do this operation because it''s unclear whether we want floored division or truncated division: test.zig:4:14: error: division with ''i32'' and ''i32'': signed integers must use @divTrunc, @divFloor, or @divExact return a / b; ^ Some languages use truncation division (C) while others (Python) use floored division. Zig makes the programmer choose explicitly. fn div(a: i32, b: i32) -> i32 { return @divTrunc(a, b); } Output: $ ./test integer overflow test.zig:4:12: 0x000000000020a683 in ??? (test) return @divTrunc(a, b); ^ test.zig:8:35: 0x0000000000204707 in ??? (test) %%io.stdout.printf("{}\n", div(@minValue(i32), -1)); ^ lib/zig/std/special/bootstrap.zig:60:21: 0x0000000000203697 in ??? (test) return root.main(); ^ lib/zig/std/special/bootstrap.zig:47:13: 0x0000000000203420 in ??? (test) callMain(argc, argv, envp) %% std.os.posix.exit(1); ^ lib/zig/std/special/bootstrap.zig:34:25: 0x0000000000203290 in ??? (test) posixCallMainAndExit() ^ Aborted Notably, if you execute the division operation at compile time, the overflow becomes a compile error (same for the other operations): %%io.stdout.printf("{}\n", comptime div(@minValue(i32), -1)); Output: ./test.zig:4:14: error: operation caused overflow return a / b; ^ ./test.zig:8:44: note: called from here %%io.stdout.printf("{}\n", comptime div(@minValue(i32), -1)); ^ Conclusion Zig is on track to boot out C as the simple, straightforward way to write system code.'
content: extracted
html: 2017-02-14-zig-already-more-knowable-than-c.html
---

There is a nifty article created back in 2015 that made its way onto Hacker News today: [I Do Not Know C](http://kukuruku.co/hub/programming/i-do-not-know-c).

The author creates a delightful set of source code samples in which the reader is intended to determine the correctness, and if correct, predict the output of the provided code. If you have not done the exercises, I encourage you to take a moment to do so.

What follows are some of the examples translated into [Zig](http://ziglang.org/) for comparison. The numbers correspond to the numbers from the original post linked above, but the C code is embedded here for comparison.

## 1\. Declaring the same variable twice

```
int i;
int i = 10;
```

In Zig:

```
var i = undefined;
var i = 10;
```

Output:

```
./test.zig:2:1: error: redefinition of 'i'
var i = 10;
^
./test.zig:1:1: note: previous definition is here
var i = undefined;
^
```

## 2\. Null pointer

```
extern void bar(void);
void foo(int *x) {
    int y = *x;  /* (1) */
    if (!x) {    /* (2) */
        return;  /* (3) */
    }
    bar();
}
```

When this example is translated to Zig, you have to explicitly decide if the pointer is nullable. For example, if you used a bare pointer in Zig:

```
extern fn bar();

export fn foo(x: &c_int) {
    var y = *x;  // (1)
    if (x == null) {     // (2)
        return;    // (3)
    }
    bar();
}
```

Then it wouldn't even compile:

```
./test.zig:7:11: error: operator not allowed for type '?&c_int'
    if (x == null) {     // (2)
          ^
```

I think this error message can be improved, but even so it prevents a possible null related bug here.

If the code author makes the pointer nullable, then the natural way to port the C code to Zig would be:

```
extern fn bar();

export fn foo(x: ?&c_int) {
    var y = x ?? return;  // (1), (2), (3)
    bar();
}
```

This compiles to:

```
0000000000000000 <foo>:
   0:	48 85 ff             	test   %rdi,%rdi
   3:	74 05                	je     a <foo+0xa>
   5:	e9 00 00 00 00       	jmpq   a <foo+0xa>
   a:	c3                   	retq   
```

This does a null check, returns if null, and otherwise calls bar.

Perhaps a more faithful way to port the C code to Zig would be this:

```
extern fn bar();

fn foo(x: ?&c_int) {
    var y = ??x;  // (1)
    if (x == null) {     // (2)
        return;    // (3)
    }
    bar();
}

pub fn main() -> %void {
    foo(null);
}
```

The `??` operator unwraps the nullable value. It asserts that the value is not null, and returns the value. If the value is null then the behavior is undefined, just like in C.

However, in Zig, undefined behavior causes a crash in debug mode:

```
$ ./test
attempt to unwrap null
/home/andy/dev/zig/build/lib/zig/std/special/zigrt.zig:16:35: 0x0000000000203395 in ??? (test)
        @import("std").debug.panic("{}", message_ptr[0..message_len]);
                                  ^
/home/andy/tmp/test.zig:4:13: 0x000000000020a61d in ??? (test)
    var y = ??x;  // (1)
            ^
/home/andy/tmp/test.zig:12:8: 0x00000000002046fd in ??? (test)
    foo(null);
       ^
/home/andy/dev/zig/build/lib/zig/std/special/bootstrap.zig:60:21: 0x0000000000203697 in ??? (test)
    return root.main();
                    ^
/home/andy/dev/zig/build/lib/zig/std/special/bootstrap.zig:47:13: 0x0000000000203420 in ??? (test)
    callMain(argc, argv, envp) %% std.os.posix.exit(1);
            ^
/home/andy/dev/zig/build/lib/zig/std/special/bootstrap.zig:34:25: 0x0000000000203290 in ??? (test)
    posixCallMainAndExit()
                        ^
Aborted
```

This is a half-finished traceback implementation (lots of TODO items to complete before the [0.1.0 milestone](https://github.com/andrewrk/zig/milestone/1)), but the point is that Zig detected the undefined behavior and aborted.

In release mode, this example invokes undefined behavior just like in C. To avoid this, programmers are expected to choose one of these options:

- Test code sufficiently in debug mode to catch undefined behavior abuse.
- Utilize the safe-release option which includes the runtime undefined behavior safety checks.

## 5\. strlen

```
int my_strlen(const char *x) {
    int res = 0;
    while(*x) {
        res++;
        x++;
    }
    return res;
}
```

In Zig, pointers generally point to single objects, while *slices* are used to refer to ranges of memory. So in practice you wouldn't need a strlen function, you would use `some_bytes.len`. But we can port this code over anyway:

```
export fn my_strlen(x: &const u8) -> c_int {
    var res: c_int = 0;
    while (x[res] != 0) {
        res += 1;
    }
    return res;
}
```

Here we must use pointer indexing because Zig does not support direct pointer arithmetic.

The compiler catches this problem:

```
./test.zig:3:14: error: expected type 'usize', found 'c_int'
    while (x[res] != 0) {
             ^
```

## 6\. Print string of bytes backwards

```
#include <stdio.h>
#include <string.h>
int main() {
    const char *str = "hello";
    size_t length = strlen(str);
    size_t i;
    for(i = length - 1; i >= 0; i--) {
        putchar(str[i]);
    }
    putchar('\n');
    return 0;
}
```

Ported to Zig:

```
const c = @cImport({
    @cInclude("stdio.h");
    @cInclude("string.h");
});

export fn main(argc: c_int, argv: &&u8) -> c_int {
    const str = c"hello";
    const length: c.size_t = c.strlen(str);
    var i: c.size_t = length - 1;
    while (i >= 0) : (i -= 1) {
        _ = c.putchar(str[i]);
    }
    _ = c.putchar('\n');
    return 0;
}
```

It compiles fine but produces this output when run:

```
integer overflow
test.zig:10:25: 0x000000000020346d in ??? (test)
    while (i >= 0) : (i -= 1) {
                        ^
Aborted
```

## 8\. Weirdo syntax

```
#include <stdio.h>
int main() {
    int array[] = { 0, 1, 2 };
    printf("%d %d %d\n", 10, (5, array[1, 2]), 10);
}
```

There is no way to express this code in Zig. Good riddance.

## 9\. Unsigned overflow

```
unsigned int add(unsigned int a, unsigned int b) {
    return a + b;
}
```

In Zig:

```
const io = @import("std").io;

export fn add(a: c_uint, b: c_uint) -> c_uint {
    return a + b;
}

pub fn main() -> %void {
    %%io.stdout.printf("{}\n", add(@maxValue(c_uint), 1));
}
```

Output:

```
$ ./test
integer overflow
test.zig:4:14: 0x00000000002032b4 in ??? (test)
    return a + b;
             ^
test.zig:8:35: 0x0000000000204747 in ??? (test)
    %%io.stdout.printf("{}\n", add(@maxValue(c_uint), 1));
                                  ^
lib/zig/std/special/bootstrap.zig:60:21: 0x00000000002036d7 in ??? (test)
    return root.main();
                    ^
lib/zig/std/special/bootstrap.zig:47:13: 0x0000000000203460 in ??? (test)
    callMain(argc, argv, envp) %% std.os.posix.exit(1);
            ^
lib/zig/std/special/bootstrap.zig:34:25: 0x00000000002032d0 in ??? (test)
    posixCallMainAndExit()
                        ^
Aborted
```

The `+` operator asserts that there will be no overflow. If you want twos complement wraparound behavior, that is possible with the `+%` operator instead:

```
export fn add(a: c_uint, b: c_uint) -> c_uint {
    return a +% b;
}
```

Now the output is:

```
$ ./test 
0
```

## 10\. Signed overflow

```
int add(int a, int b) {
    return a + b;
}
```

In C signed and unsigned integer overflow work differently. In Zig, they work the same. `+` asserts that no overflow occurs, and `+%` performs twos complement wraparound behavior.

## 11\. Negation overflow

```
int neg(int a) {
    return -a;
}
```

By now you can probably predict how this works in Zig.

```
const io = @import("std").io;

export fn neg(a: c_int) -> c_int {
    return -a;
}

pub fn main() -> %void {
    %%io.stdout.printf("{}\n", neg(@minValue(c_int)));
}
```

Output:

```
$ ./test
integer overflow
test.zig:4:12: 0x00000000002032b0 in ??? (test)
    return -a;
           ^
test.zig:8:35: 0x0000000000204742 in ??? (test)
    %%io.stdout.printf("{}\n", neg(@minValue(c_int)));
                                  ^
lib/zig/std/special/bootstrap.zig:60:21: 0x00000000002036d7 in ??? (test)
    return root.main();
                    ^
lib/zig/std/special/bootstrap.zig:47:13: 0x0000000000203460 in ??? (test)
    callMain(argc, argv, envp) %% std.os.posix.exit(1);
            ^
lib/zig/std/special/bootstrap.zig:34:25: 0x00000000002032d0 in ??? (test)
    posixCallMainAndExit()
                        ^
Aborted
```

The `-%` wraparound variant of the negation operator works here too:

```
export fn neg(a: c_int) -> c_int {
    return -%a;
}
```

Output:

```
$ ./test 
-2147483648
```

## 12\. Division overflow

```
int div(int a, int b) {
    assert(b != 0);
    return a / b;
}
```

Different operation, same deal.

```
const io = @import("std").io;

fn div(a: i32, b: i32) -> i32 {
    return a / b;
}

pub fn main() -> %void {
    %%io.stdout.printf("{}\n", div(@minValue(i32), -1));
}
```

First of all, Zig doesn't let us do this operation because it's unclear whether we want floored division or truncated division:

```

test.zig:4:14: error: division with 'i32' and 'i32': signed integers must use @divTrunc, @divFloor, or @divExact
    return a / b;
             ^
```

Some languages use truncation division (C) while others (Python) use floored division. Zig makes the programmer choose explicitly.

```
fn div(a: i32, b: i32) -> i32 {
    return @divTrunc(a, b);
}
```

Output:

```
$ ./test
integer overflow
test.zig:4:12: 0x000000000020a683 in ??? (test)
    return @divTrunc(a, b);
           ^
test.zig:8:35: 0x0000000000204707 in ??? (test)
    %%io.stdout.printf("{}\n", div(@minValue(i32), -1));
                                  ^
lib/zig/std/special/bootstrap.zig:60:21: 0x0000000000203697 in ??? (test)
    return root.main();
                    ^
lib/zig/std/special/bootstrap.zig:47:13: 0x0000000000203420 in ??? (test)
    callMain(argc, argv, envp) %% std.os.posix.exit(1);
            ^
lib/zig/std/special/bootstrap.zig:34:25: 0x0000000000203290 in ??? (test)
    posixCallMainAndExit()
                        ^
Aborted
```

Notably, if you execute the division operation at compile time, the overflow becomes a compile error (same for the other operations):

```
    %%io.stdout.printf("{}\n", comptime div(@minValue(i32), -1));
```

Output:

```
./test.zig:4:14: error: operation caused overflow
    return a / b;
             ^
./test.zig:8:44: note: called from here
    %%io.stdout.printf("{}\n", comptime div(@minValue(i32), -1));
                                           ^
```

## Conclusion

Zig is on track to boot out C as the simple, straightforward way to write system code.

Thanks for reading my blog post.
