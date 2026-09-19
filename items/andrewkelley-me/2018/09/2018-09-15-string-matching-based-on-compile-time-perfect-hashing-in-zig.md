---
title: String Matching based on Compile Time Perfect Hashing in Zig
link: https://andrewkelley.me/post/string-matching-comptime-perfect-hashing-zig.html
source: andrewkelley-me
published: 2018-09-15T16:06:33Z
updated: 2018-09-15T16:06:33Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'String Matching based on Compile Time Perfect Hashing in Zig Inspired by cttrie - Compile time TRIE based string matching, I decided to see what this solution would look like in Zig. Here''s the API I came up with: const ph = perfectHash([][]const u8{ "a", "ab", "abc", }); switch (ph.hash(target)) { ph.case("a") => std.debug.warn("handle the a case"), ph.case("ab") => std.debug.warn("handle the ab case"), ph.case("abc") => std.debug.warn("handle the abc case"), else => unreachable, } It notices at compile-time if you forget to declare one of the cases. For example, if I comment out the last item: const ph = perfectHash([][]const u8{ "a", "ab", //"abc", }); When compiled this gives: perfect-hashing.zig:147:13: error: case value ''abc'' not declared @compileError("case value ''" ++ s ++ "'' not declared"); ^ perfect-hashing.zig:18:16: note: called from here ph.case("abc") => std.debug.warn("handle the abc case\n"), ^ It also has runtime safety if you pass in a string that was not prepared for: const std = @import("std"); const assert = std.debug.assert; test "perfect hashing" { basedOnLength("zzz"); } fn basedOnLength(target: []const u8) void { const ph = perfectHash([][]const u8{ "a", "ab", "abc", }); switch (ph.hash(target)) { ph.case("a") => @panic("wrong one a"), ph.case("ab") => {}, // test pass ph.case("abc") => @panic("wrong one abc"), else => unreachable, } } When run this gives: Test 1/1 perfect hashing...attempt to perfect hash zzz which was not declared perfect-hashing.zig:156:36: 0x205a21 in ??? (test) std.debug.panic("attempt to perfect hash {} which was not declared", s); ^ perfect-hashing.zig:15:20: 0x2051bd in ??? (test) switch (ph.hash(target)) { ^ perfect-hashing.zig:5:18: 0x205050 in ??? (test) basedOnLength("zzz"); ^ /home/andy/dev/zig/build/lib/zig/std/special/test_runner.zig:13:25: 0x2238ea in ??? (test) if (test_fn.func()) |_| { ^ /home/andy/dev/zig/build/lib/zig/std/special/bootstrap.zig:96:22: 0x22369b in ??? (test) root.main() catch |err| { ^ /home/andy/dev/zig/build/lib/zig/std/special/bootstrap.zig:70:20: 0x223615 in ??? (test) return callMain(); ^ /home/andy/dev/zig/build/lib/zig/std/special/bootstrap.zig:64:39: 0x223478 in ??? (test) std.os.posix.exit(callMainWithArgs(argc, argv, envp)); ^ /home/andy/dev/zig/build/lib/zig/std/special/bootstrap.zig:37:5: 0x223330 in ??? (test) @noInlineCall(posixCallMainAndExit); ^ Tests failed. Use the following command to reproduce the failure: /home/andy/dev/zig/build/zig-cache/test So there''s the API. How does it work? Here''s the implementation of the perfectHash function: fn perfectHash(comptime strs: []const []const u8) type { const Op = union(enum) { /// add the length of the string Length, /// add the byte at index % len Index: usize, /// right shift then xor with constant XorShiftMultiply: u32, }; const S = struct { fn hash(comptime plan: []Op, s: []const u8) u32 { var h: u32 = 0; inline for (plan) |op| { switch (op) { Op.Length => { h +%= @truncate(u32, s.len); }, Op.Index => |index| { h +%= s[index % s.len]; }, Op.XorShiftMultiply => |x| { h ^= x >> 16; }, } } return h; } fn testPlan(comptime plan: []Op) bool { var hit = [1]bool{false} ** strs.len; for (strs) |s| { const h = hash(plan, s); const i = h % hit.len; if (hit[i]) { // hit this index twice return false; } hit[i] = true; } return true; } }; var ops_buf: [10]Op = undefined; const plan = have_a_plan: { var seed: u32 = 0x45d9f3b; var index_i: usize = 0; const try_seed_count = 50; const try_index_count = 50; while (index_i < try_index_count) : (index_i += 1) { const bool_values = if (index_i == 0) []bool{true} else []bool{ false, true }; for (bool_values) |try_length| { var seed_i: usize = 0; while (seed_i < try_seed_count) : (seed_i += 1) { comptime var rand_state = std.rand.Xoroshiro128.init(seed + seed_i); const rng = &rand_state.random; var ops_index = 0; if (try_length) { ops_buf[ops_index] = Op.Length; ops_index += 1; if (S.testPlan(ops_buf[0..ops_index])) break :have_a_plan ops_buf[0..ops_index]; ops_buf[ops_index] = Op{ .XorShiftMultiply = rng.scalar(u32) }; ops_index += 1; if (S.testPlan(ops_buf[0..ops_index])) break :have_a_plan ops_buf[0..ops_index]; } ops_buf[ops_index] = Op{ .XorShiftMultiply = rng.scalar(u32) }; ops_index += 1; if (S.testPlan(ops_buf[0..ops_index])) break :have_a_plan ops_buf[0..ops_index]; const before_bytes_it_index = ops_index; var byte_index = 0; while (byte_index < index_i) : (byte_index += 1) { ops_index = before_bytes_it_index; ops_buf[ops_index] = Op{ .Index = rng.scalar(u32) % try_index_count }; ops_index += 1; if (S.testPlan(ops_buf[0..ops_index])) break :have_a_plan ops_buf[0..ops_index]; ops_buf[ops_index] = Op{ .XorShiftMultiply = rng.scalar(u32) }; ops_index += 1; if (S.testPlan(ops_buf[0..ops_index])) break :have_a_plan ops_buf[0..ops_index]; } } } } @compileError("unable to come up with perfect hash"); }; return struct { fn case(comptime s: []const u8) usize { inline for (strs) |str| { if (std.mem.eql(u8, str, s)) return hash(s); } @compileError("case value ''" ++ s ++ "'' not declared"); } fn hash(s: []const u8) usize { if (std.debug.runtime_safety) { const ok = for (strs) |str| { if (std.mem.eql(u8, str, s)) break true; } else false; if (!ok) { std.debug.panic("attempt to perfect hash {} which was not declared", s); } } return S.hash(plan, s) % strs.len; } }; } Here''s what this is doing: Create the concept of a hashing operation. The operations that can happen are: Length - use wraparound addition to add the length of the string to the hash value. Index - use wraparound addition to add the byte from the string at the specified index. Use modulus arithmetic in case the index is out of bounds. XorShiftMultiply - perform a right shift by 16 bits of the hash value and then xor with the specified value (which will come from a random number generator executed at compile time). Next, we''re going to try a series of "plans" which is a sequence of operations strung together. We have this function testPlan which performs the hash operations on all the strings and sees if there are any collisions. If we ever find a plan that results in no collisions, we have found a perfect hashing strategy. First we test a plan that is simply the Length operation. If this works, then all the hash function has to do is take the length of the string mod the number of strings. Easy. You may notice this is true for the above example. Don''t worry, I have a more complicated example below. Next we iterate over the count of how many different bytes we are willing to look at in the hash function. We start with 0. If we can xor the length with a random number and it fixes the collisions, then we''re done. We try 50 seeds before giving up and deciding to inspect a random byte from the string in the hash function. If the addition of inspecting a random byte from the string to hash doesn''t solve the problem, we try 50 seeds in order to choose different random byte indexes. If that still doesn''t work, we look at 2 random bytes, again trying 50 different seeds with these 2 bytes. And so on until every combination of 50 seeds x 50 bytes inspected, at which point we give up and emit a compile error "unable to come up with perfect hash". We can use @compileLog to see the plan that the function came up with: for (plan) |op| { @compileLog(@TagType(Op)(op)); } This outputs: | @TagType(Op).Length So this means that the hash function only has to look at the length for this example. The nice thing about this is that the plan is all known at compile time. Indeed, you can see that the hash function uses inline for to iterate over the operations in the plan. This means that LLVM is able to fully optimize the hash function. Here''s what it gets compiled to in release mode for x86_64: 0000000000000000 <basedOnLength>: 0: 89 f0 mov %esi,%eax 2: b9 ab aa aa aa mov $0xaaaaaaab,%ecx 7: 48 0f af c8 imul %rax,%rcx b: 48 c1 e9 21 shr $0x21,%rcx f: 8d 04 49 lea (%rcx,%rcx,2),%eax 12: 29 c6 sub %eax,%esi 14: 48 89 f0 mov %rsi,%rax 17: c3 retq 18: 0f 1f 84 00 00 00 00 nopl 0x0(%rax,%rax,1) 1f: 00 You can see there is not even a jump instruction in there. And it will output numbers in sequential order from 0, so that the switch statement can be a jump table. Here is the LLVM IR of the switch: %2 = call fastcc i64 @"perfectHash((struct []const []const u8 constant))_hash"(%"[]u8"* %0), !dbg !736 store i64 %2, i64* %1, align 8, !dbg !736 %3 = load i64, i64* %1, align 8, !dbg !740 switch i64 %3, label %SwitchElse [ i64 1, label %SwitchProng i64 2, label %SwitchProng1 i64 0, label %SwitchProng2 ], !dbg !740 How about a harder example? @setEvalBranchQuota(100000); const ph = perfectHash([][]const u8{ "one", "two", "three", "four", "five", }); switch (ph.hash(target)) { ph.case("one") => std.debug.warn("handle the one case"), ph.case("two") => std.debug.warn("handle the two case"), ph.case("three") => std.debug.warn("handle the three case"), ph.case("four") => std.debug.warn("handle the four case"), ph.case("five") => std.debug.warn("handle the five case"), else => unreachable, } This example is interesting because there are 2 pairs of length collisions (one/two, four/five) and 2 pairs of byte collisions (two/three, four/five). Here we have to use @setEvalBranchQuota because it takes a bit of computation to come up with the answer. Again, the hash function comes up with a mapping to 0, 1, 2, 3, 4 (but not necessarily in the same order as specified): %2 = call fastcc i64 @"perfectHash((struct []const []const u8 constant))_hash.11"(%"[]u8"* %0), !dbg !749 store i64 %2, i64* %1, align 8, !dbg !749 %3 = load i64, i64* %1, align 8, !dbg !753 switch i64 %3, label %SwitchElse [ i64 4, label %SwitchProng i64 2, label %SwitchProng1 i64 3, label %SwitchProng2 i64 0, label %SwitchProng3 i64 1, label %SwitchProng4 ], !dbg !753 And the optimized assembly: 0000000000000020 <basedOnOtherStuff>: 20: 48 83 fe 22 cmp $0x22,%rsi 24: 77 0b ja 31 <basedOnOtherStuff+0x11> 26: b8 22 00 00 00 mov $0x22,%eax 2b: 31 d2 xor %edx,%edx 2d: f7 f6 div %esi 2f: eb 05 jmp 36 <basedOnOtherStuff+0x16> 31: ba 22 00 00 00 mov $0x22,%edx 36: 0f b6 04 17 movzbl (%rdi,%rdx,1),%eax 3a: 05 ef 3d 00 00 add $0x3def,%eax 3f: b9 cd cc cc cc mov $0xcccccccd,%ecx 44: 48 0f af c8 imul %rax,%rcx 48: 48 c1 e9 22 shr $0x22,%rcx 4c: 8d 0c 89 lea (%rcx,%rcx,4),%ecx 4f: 29 c8 sub %ecx,%eax 51: c3 retq We have a couple of jumps here, but no loop. This code looks at only 1 byte from the target string to determine the corresponding case index. We can see that more clearly with the @compileLog snippet from earlier: | @TagType(Op).XorShiftMultiply | @TagType(Op).Index So it initializes the hash with a constant value of predetermined random bits, then adds the byte from a randomly chosen index of the string. Using @compileLog(plan[1].Index) I determined that it is choosing the value 34, which means that: For one, 34 % 3 == 1, it looks at the ''n'' For two, 34 % 3 == 1, it looks at the ''w'' For three, 34 % 5 == 4, it looks at the ''e'' For four, 34 % 4 == 2, it looks at the ''u'' For five, 34 % 4 == 2, it looks at the ''i'' So the perfect hashing exploited that these bytes are all different, and when combined with the random constant that it found, the final modulus spreads out the value across the full range of 0, 1, 2, 3, 4. Can we do better? There are lots of ways this can be improved - this is just a proof of concept. For example, we could start by looking for a perfect hash in the subset of bytes that are within the smallest string length. For example, if we used index 1 in the above example, we could avoid the jumps and remainder division instructions in the hash function. Another way this can be improved, to reduce compile times, is to have the perfectHash function accept the RNG seed as a parameter. If the seed worked first try, great. Otherwise the function would find the seed that does work, and emit a compile error instructing the programmer to switch the seed argument to the good one, saving time in future compilations. Conclusion If you like this, you should check out Zig. Consider becoming a sponsor.'
content: extracted
html: 2018-09-15-string-matching-based-on-compile-time-perfect-hashing-in-zig.html
---

[Andrew Kelley](https://andrewkelley.me/) - String Matching based on Compile Time Perfect Hashing in Zig (2018 Sep 15)

Inspired by [cttrie - Compile time TRIE based string matching](https://smilingthax.github.io/slides/cttrie/), I decided to see what this solution would look like in Zig.

Here's the API I came up with:

```
const ph = perfectHash([][]const u8{
    "a",
    "ab",
    "abc",
});
switch (ph.hash(target)) {
    ph.case("a") => std.debug.warn("handle the a case"),
    ph.case("ab") => std.debug.warn("handle the ab case"),
    ph.case("abc") => std.debug.warn("handle the abc case"),
    else => unreachable,
}
```

It notices at compile-time if you forget to declare one of the cases. For example, if I comment out the last item:

```
const ph = perfectHash([][]const u8{
    "a",
    "ab",
    //"abc",
});
```

When compiled this gives:

```
perfect-hashing.zig:147:13: error: case value 'abc' not declared
            @compileError("case value '" ++ s ++ "' not declared");
            ^
perfect-hashing.zig:18:16: note: called from here
        ph.case("abc") => std.debug.warn("handle the abc case\n"),
               ^
```

It also has runtime safety if you pass in a string that was not prepared for:

```
const std = @import("std");
const assert = std.debug.assert;

test "perfect hashing" {
    basedOnLength("zzz");
}

fn basedOnLength(target: []const u8) void {
    const ph = perfectHash([][]const u8{
        "a",
        "ab",
        "abc",
    });
    switch (ph.hash(target)) {
        ph.case("a") => @panic("wrong one a"),
        ph.case("ab") => {}, // test pass
        ph.case("abc") => @panic("wrong one abc"),
        else => unreachable,
    }
}
```

When run this gives:

```
Test 1/1 perfect hashing...attempt to perfect hash zzz which was not declared
perfect-hashing.zig:156:36: 0x205a21 in ??? (test)
                    std.debug.panic("attempt to perfect hash {} which was not declared", s);
                                   ^
perfect-hashing.zig:15:20: 0x2051bd in ??? (test)
    switch (ph.hash(target)) {
                   ^
perfect-hashing.zig:5:18: 0x205050 in ??? (test)
    basedOnLength("zzz");
                 ^
/home/andy/dev/zig/build/lib/zig/std/special/test_runner.zig:13:25: 0x2238ea in ??? (test)
        if (test_fn.func()) |_| {
                        ^
/home/andy/dev/zig/build/lib/zig/std/special/bootstrap.zig:96:22: 0x22369b in ??? (test)
            root.main() catch |err| {
                     ^
/home/andy/dev/zig/build/lib/zig/std/special/bootstrap.zig:70:20: 0x223615 in ??? (test)
    return callMain();
                   ^
/home/andy/dev/zig/build/lib/zig/std/special/bootstrap.zig:64:39: 0x223478 in ??? (test)
    std.os.posix.exit(callMainWithArgs(argc, argv, envp));
                                      ^
/home/andy/dev/zig/build/lib/zig/std/special/bootstrap.zig:37:5: 0x223330 in ??? (test)
    @noInlineCall(posixCallMainAndExit);
    ^

Tests failed. Use the following command to reproduce the failure:
/home/andy/dev/zig/build/zig-cache/test
```

So there's the API. How does it work?

Here's the implementation of the `perfectHash` function:

```
fn perfectHash(comptime strs: []const []const u8) type {
    const Op = union(enum) {
        /// add the length of the string
        Length,

        /// add the byte at index % len
        Index: usize,

        /// right shift then xor with constant
        XorShiftMultiply: u32,
    };
    const S = struct {
        fn hash(comptime plan: []Op, s: []const u8) u32 {
            var h: u32 = 0;
            inline for (plan) |op| {
                switch (op) {
                    Op.Length => {
                        h +%= @truncate(u32, s.len);
                    },
                    Op.Index => |index| {
                        h +%= s[index % s.len];
                    },
                    Op.XorShiftMultiply => |x| {
                        h ^= x >> 16;
                    },
                }
            }
            return h;
        }

        fn testPlan(comptime plan: []Op) bool {
            var hit = [1]bool{false} ** strs.len;
            for (strs) |s| {
                const h = hash(plan, s);
                const i = h % hit.len;
                if (hit[i]) {
                    // hit this index twice
                    return false;
                }
                hit[i] = true;
            }
            return true;
        }
    };

    var ops_buf: [10]Op = undefined;

    const plan = have_a_plan: {
        var seed: u32 = 0x45d9f3b;
        var index_i: usize = 0;
        const try_seed_count = 50;
        const try_index_count = 50;

        while (index_i < try_index_count) : (index_i += 1) {
            const bool_values = if (index_i == 0) []bool{true} else []bool{ false, true };
            for (bool_values) |try_length| {
                var seed_i: usize = 0;
                while (seed_i < try_seed_count) : (seed_i += 1) {
                    comptime var rand_state = std.rand.Xoroshiro128.init(seed + seed_i);
                    const rng = &rand_state.random;

                    var ops_index = 0;

                    if (try_length) {
                        ops_buf[ops_index] = Op.Length;
                        ops_index += 1;

                        if (S.testPlan(ops_buf[0..ops_index]))
                            break :have_a_plan ops_buf[0..ops_index];

                        ops_buf[ops_index] = Op{ .XorShiftMultiply = rng.scalar(u32) };
                        ops_index += 1;

                        if (S.testPlan(ops_buf[0..ops_index]))
                            break :have_a_plan ops_buf[0..ops_index];
                    }

                    ops_buf[ops_index] = Op{ .XorShiftMultiply = rng.scalar(u32) };
                    ops_index += 1;

                    if (S.testPlan(ops_buf[0..ops_index]))
                        break :have_a_plan ops_buf[0..ops_index];

                    const before_bytes_it_index = ops_index;

                    var byte_index = 0;
                    while (byte_index < index_i) : (byte_index += 1) {
                        ops_index = before_bytes_it_index;

                        ops_buf[ops_index] = Op{ .Index = rng.scalar(u32) % try_index_count };
                        ops_index += 1;

                        if (S.testPlan(ops_buf[0..ops_index]))
                            break :have_a_plan ops_buf[0..ops_index];

                        ops_buf[ops_index] = Op{ .XorShiftMultiply = rng.scalar(u32) };
                        ops_index += 1;

                        if (S.testPlan(ops_buf[0..ops_index]))
                            break :have_a_plan ops_buf[0..ops_index];
                    }
                }
            }
        }

        @compileError("unable to come up with perfect hash");
    };

    return struct {
        fn case(comptime s: []const u8) usize {
            inline for (strs) |str| {
                if (std.mem.eql(u8, str, s))
                    return hash(s);
            }
            @compileError("case value '" ++ s ++ "' not declared");
        }
        fn hash(s: []const u8) usize {
            if (std.debug.runtime_safety) {
                const ok = for (strs) |str| {
                    if (std.mem.eql(u8, str, s))
                        break true;
                } else false;
                if (!ok) {
                    std.debug.panic("attempt to perfect hash {} which was not declared", s);
                }
            }
            return S.hash(plan, s) % strs.len;
        }
    };
}
```

Here's what this is doing:

- Create the concept of a hashing operation. The operations that can happen are:
  - `Length` - use wraparound addition to add the length of the string to the hash value.
  - `Index` - use wraparound addition to add the byte from the string at the specified index. Use modulus arithmetic in case the index is out of bounds.
  - `XorShiftMultiply` - perform a right shift by 16 bits of the hash value and then xor with the specified value (which will come from a random number generator executed at compile time).
- Next, we're going to try a series of "plans" which is a sequence of operations strung together. We have this function `testPlan` which performs the hash operations on all the strings and sees if there are any collisions. If we ever find a plan that results in no collisions, we have found a perfect hashing strategy.
- First we test a plan that is simply the `Length` operation. If this works, then all the hash function has to do is take the length of the string mod the number of strings. Easy. You may notice this is true for the above example. Don't worry, I have a more complicated example below.
- Next we iterate over the count of how many different bytes we are willing to look at in the hash function. We start with 0. If we can xor the length with a random number and it fixes the collisions, then we're done. We try 50 seeds before giving up and deciding to inspect a random byte from the string in the hash function. If the addition of inspecting a random byte from the string to hash doesn't solve the problem, we try 50 seeds in order to choose different random byte indexes. If that still doesn't work, we look at 2 random bytes, again trying 50 different seeds with these 2 bytes. And so on until every combination of 50 seeds x 50 bytes inspected, at which point we give up and emit a compile error "unable to come up with perfect hash".

We can use `@compileLog` to see the plan that the function came up with:

```
for (plan) |op| {
    @compileLog(@TagType(Op)(op));
}
```

This outputs:

```
| @TagType(Op).Length
```

So this means that the hash function only has to look at the length for this example.

The nice thing about this is that the plan is all known at compile time. Indeed, you can see that the `hash` function uses [inline for](https://ziglang.org/documentation/master/#inline-for) to iterate over the operations in the plan. This means that LLVM is able to fully optimize the hash function.

Here's what it gets compiled to in release mode for x86\_64:

```
0000000000000000 <basedOnLength>:
   0:	89 f0                	mov    %esi,%eax
   2:	b9 ab aa aa aa       	mov    $0xaaaaaaab,%ecx
   7:	48 0f af c8          	imul   %rax,%rcx
   b:	48 c1 e9 21          	shr    $0x21,%rcx
   f:	8d 04 49             	lea    (%rcx,%rcx,2),%eax
  12:	29 c6                	sub    %eax,%esi
  14:	48 89 f0             	mov    %rsi,%rax
  17:	c3                   	retq   
  18:	0f 1f 84 00 00 00 00 	nopl   0x0(%rax,%rax,1)
  1f:	00 
```

You can see there is not even a jump instruction in there. And it will output numbers in sequential order from 0, so that the switch statement can be a jump table. Here is the LLVM IR of the switch:

```
 %2 = call fastcc i64 @"perfectHash((struct []const []const u8 constant))_hash"(%"[]u8"* %0), !dbg !736
  store i64 %2, i64* %1, align 8, !dbg !736
  %3 = load i64, i64* %1, align 8, !dbg !740
  switch i64 %3, label %SwitchElse [
    i64 1, label %SwitchProng
    i64 2, label %SwitchProng1
    i64 0, label %SwitchProng2
  ], !dbg !740
```

How about a harder example?

```
@setEvalBranchQuota(100000);
const ph = perfectHash([][]const u8{
    "one",
    "two",
    "three",
    "four",
    "five",
});
switch (ph.hash(target)) {
    ph.case("one") => std.debug.warn("handle the one case"),
    ph.case("two") => std.debug.warn("handle the two case"),
    ph.case("three") => std.debug.warn("handle the three case"),
    ph.case("four") => std.debug.warn("handle the four case"),
    ph.case("five") => std.debug.warn("handle the five case"),
    else => unreachable,
}
```

This example is interesting because there are 2 pairs of length collisions (one/two, four/five) and 2 pairs of byte collisions (two/three, four/five).

Here we have to use [@setEvalBranchQuota](https://ziglang.org/documentation/master/#setEvalBranchQuota) because it takes a bit of computation to come up with the answer.

Again, the hash function comes up with a mapping to 0, 1, 2, 3, 4 (but not necessarily in the same order as specified):

```
 %2 = call fastcc i64 @"perfectHash((struct []const []const u8 constant))_hash.11"(%"[]u8"* %0), !dbg !749
  store i64 %2, i64* %1, align 8, !dbg !749
  %3 = load i64, i64* %1, align 8, !dbg !753
  switch i64 %3, label %SwitchElse [
    i64 4, label %SwitchProng
    i64 2, label %SwitchProng1
    i64 3, label %SwitchProng2
    i64 0, label %SwitchProng3
    i64 1, label %SwitchProng4
  ], !dbg !753
```

And the optimized assembly:

```
0000000000000020 <basedOnOtherStuff>:
  20:	48 83 fe 22          	cmp    $0x22,%rsi
  24:	77 0b                	ja     31 <basedOnOtherStuff+0x11>
  26:	b8 22 00 00 00       	mov    $0x22,%eax
  2b:	31 d2                	xor    %edx,%edx
  2d:	f7 f6                	div    %esi
  2f:	eb 05                	jmp    36 <basedOnOtherStuff+0x16>
  31:	ba 22 00 00 00       	mov    $0x22,%edx
  36:	0f b6 04 17          	movzbl (%rdi,%rdx,1),%eax
  3a:	05 ef 3d 00 00       	add    $0x3def,%eax
  3f:	b9 cd cc cc cc       	mov    $0xcccccccd,%ecx
  44:	48 0f af c8          	imul   %rax,%rcx
  48:	48 c1 e9 22          	shr    $0x22,%rcx
  4c:	8d 0c 89             	lea    (%rcx,%rcx,4),%ecx
  4f:	29 c8                	sub    %ecx,%eax
  51:	c3                   	retq   
```

We have a couple of jumps here, but no loop. This code looks at only 1 byte from the target string to determine the corresponding case index. We can see that more clearly with the `@compileLog` snippet from earlier:

```
| @TagType(Op).XorShiftMultiply
| @TagType(Op).Index
```

So it initializes the hash with a constant value of predetermined random bits, then adds the byte from a randomly chosen index of the string. Using `@compileLog(plan[1].Index)` I determined that it is choosing the value 34, which means that:

- For `one`, 34 % 3 == 1, it looks at the 'n'
- For `two`, 34 % 3 == 1, it looks at the 'w'
- For `three`, 34 % 5 == 4, it looks at the 'e'
- For `four`, 34 % 4 == 2, it looks at the 'u'
- For `five`, 34 % 4 == 2, it looks at the 'i'

So the perfect hashing exploited that these bytes are all different, and when combined with the random constant that it found, the final modulus spreads out the value across the full range of 0, 1, 2, 3, 4.

### Can we do better?

There are lots of ways this can be improved - this is just a proof of concept. For example, we could start by looking for a perfect hash in the subset of bytes that are within the smallest string length. For example, if we used index 1 in the above example, we could avoid the jumps and remainder division instructions in the hash function.

Another way this can be improved, to reduce compile times, is to have the `perfectHash` function accept the RNG seed as a parameter. If the seed worked first try, great. Otherwise the function would find the seed that does work, and emit a compile error instructing the programmer to switch the seed argument to the good one, saving time in future compilations.

### Conclusion

If you like this, you should check out [Zig](https://ziglang.org/). Consider [becoming a sponsor](https://github.com/users/andrewrk/sponsorship).

Thanks for reading my blog post.
