---
title: 'Zig: December 2017 in Review'
link: https://andrewkelley.me/post/zig-december-2017-in-review.html
source: andrewkelley-me
published: 2018-01-03T07:23:11Z
updated: 2018-01-03T07:23:11Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'Zig: December 2017 in Review I figured since I ask people to donate monthly, I will start giving a monthly progress report to provide accountability. So here''s everything that happened in Zig land in December 2017: enum tag types You can now specify the tag type for an enum (#305): const Small2 = enum (u2) { One, Two, }; If you specify the tag type for an enum, you can put it in a packed struct: const A = enum (u3) { One, Two, Three, Four, One2, Two2, Three2, Four2, }; const B = enum (u3) { One3, Two3, Three3, Four3, One23, Two23, Three23, Four23, }; const C = enum (u2) { One4, Two4, Three4, Four4, }; const BitFieldOfEnums = packed struct { a: A, b: B, c: C, }; const bit_field_1 = BitFieldOfEnums { .a = A.Two, .b = B.Three3, .c = C.Four4, }; You can no longer cast from an enum to an arbitrary integer. Instead you must cast to the enum tag type and vice versa: const Small2 = enum (u2) { One, Two, }; test "casting enum to its tag type" { testCastEnumToTagType(Small2.Two); } fn testCastEnumToTagType(value: Small2) { assert(u2(value) == 1); } enum tag values Now you can set the tag values of enums: const MultipleChoice = enum(u32) { A = 20, B = 40, C = 60, D = 1000, }; Complete enum and union overhaul Related issue: #618 Enums are now a simple mapping between a symbol and a number. They can no longer contain payloads. Unions have been upgraded and can now accept an enum as an argument: const TheTag = enum {A, B, C}; const TheUnion = union(TheTag) { A: i32, B: i32, C: i32 }; test "union field access gives the enum values" { assert(TheUnion.A == TheTag.A); assert(TheUnion.B == TheTag.B); assert(TheUnion.C == TheTag.C); } If you want to auto-create an enum for a union, you can use the enum keyword like this: const TheUnion2 = union(enum) { Item1, Item2: i32, }; You can switch on a union-enum just like you could previously with an enum: const SwitchProngWithVarEnum = union(enum) { One: i32, Two: f32, Meh: void, }; fn switchProngWithVarFn(a: &const SwitchProngWithVarEnum) { switch(*a) { SwitchProngWithVarEnum.One => |x| { assert(x == 13); }, SwitchProngWithVarEnum.Two => |x| { assert(x == 13.0); }, SwitchProngWithVarEnum.Meh => |x| { const v: void = x; }, } } However, if you do not give an enum to a union, the tag value is not visible to the programmer: const Payload = union { A: i32, B: f64, C: bool, }; export fn entry() { const a = Payload { .A = 1234 }; foo(a); } fn foo(a: &const Payload) { switch (*a) { Payload.A => {}, else => unreachable, } } test.zig:11:13: error: switch on union which has no attached enum switch (*a) { ^ test.zig:1:17: note: consider ''union(enum)'' here const Payload = union { ^ test.zig:12:16: error: container ''Payload'' has no member called ''A'' Payload.A => {}, ^ There is still debug safety though! const Foo = union { float: f32, int: u32, }; pub fn main() -> %void { var f = Foo { .int = 42 }; bar(&f); } fn bar(f: &Foo) { f.float = 12.34; } access of inactive union field lib/zig/std/special/panic.zig:12:35: 0x0000000000203674 in ??? (test) @import("std").debug.panic("{}", msg); ^ test.zig:12:6: 0x0000000000217bd7 in ??? (test) f.float = 12.34; ^ test.zig:8:8: 0x0000000000217b7c in ??? (test) bar(&f); ^ Aborted However, if you make an extern union to be compatible with C code, there is no debug safety, just like a C union. Other tidbits: @enumTagName is renamed to @tagName @EnumTagType is renamed to @TagType, and it works on both enums and union-enums. There is no longer an EnumTag type It is now an error for enums and unions to have 0 fields. However you can still have a struct with 0 fields. union values can implicitly cast to enum values when the enum type is the tag type of the union and the union value tag is comptime known to have a void field type. likewise, enum values can implicitly cast to union values. See #642. test "cast tag type of union to union" { var x: Value2 = Letter2.B; assert(Letter2(x) == Letter2.B); } const Letter2 = enum { A, B, C }; const Value2 = union(Letter2) { A: i32, B, C, }; test "implicit cast union to its tag type" { var x: Value2 = Letter2.B; assert(x == Letter2.B); giveMeLetterB(x); } fn giveMeLetterB(x: Letter2) { assert(x == Value2.B); } Update LLD fork to 5.0.1rc2 We have a fork of LLD in the zig project because of several upstream issues, all of which I have filed bugs for: LDD calls exit after successful link. Patch to fix sent upstream and accepted. LDD crashes on a linker script with empty sections. Fixed upstream. Buggy MACH-O code causes assertion failure in simple object. We have a hacky workaround for this bug in zig''s fork of LLD, but the workaround is not good enough to send upstream. See #662 for more details. MACH-O: Bad ASM code generated for __stub_helpers section Thanks to Patricio V. for sending the fix upstream, which has been accepted. When LLVM 6.0.0 comes out, Zig will have to keep its fork because of the one issue, but we can drop all the other patches since they have been accepted upstream. Self-hosted compiler progress The self-hosted compiler effort has begun. So far we have a tokenizer, and an incomplete parser and formatter. The code uses no recursion and therefore has compile-time known stack space usage. See #157 The self-hosted compiler works on every supported platform, is built using the zig build system, tested with zig test, links against LLVM, and can import 100% of the LLVM symbols from the LLVM C-API .h files - even the inline functions. There is one C++ file in Zig which uses the more powerful LLVM C++ API (for example to create debug information) and exposes a C API. This file is now shared between the C++ self-hosted compiler and the self-hosted compiler. In stage1, we create a static library with this one file in it, and then use that library in both the C++ compiler and the self-hosted compiler. Higher level arg-parsing API It''s really a shame that Windows command line parsing requires you to allocate memory. This means that to have a cross-platform API for command line arguments, even though in POSIX it can never fail, we have to handle the possibility because of Windows. This lead to a command line args API like this: pub fn main() -> %void { var arg_it = os.args(); // skip my own exe name _ = arg_it.skip(); while (arg_it.next(allocator)) |err_or_arg| { const arg = %return err_or_arg; defer allocator.free(arg); // use the arg... } } Yikes, a bit cumbersome. I added a higher level API. Now you can call std.os.argsAlloc and get a %[]const []u8, and you just have to call std.os.argsFree when you''re done with it. pub fn main() -> %void { const allocator = std.heap.c_allocator; const args = %return os.argsAlloc(allocator); defer os.argsFree(allocator, args); var arg_i: usize = 1; while (arg_i < args.len) : (arg_i += 1) { const arg = args[arg_i]; // do something with arg... } } Better! Single point of failure. For now this uses the other API under the hood, but it could be reimplemented with the same API to do a single allocation. I added a new kind of test to make sure command line argument parsing works. Automatic C-to-Zig translation Translation now understands enum tag types. I refactored the C macro parsing, and made it understand pointer casting. Now some kinds of int-to-ptr code in .h files for embedded programming just work: #define NRF_GPIO ((NRF_GPIO_Type *) NRF_GPIO_BASE) Zig now understands this C macro. std.mem add aligned functions to Allocator interface mem.Allocator initializes bytes to undefined. This does nothing in ReleaseFast mode. In Debug and ReleaseSafe modes, it initializes bytes to 0xaa which helps catch memory errors. add mem.FixedBufferAllocator std.os.ChildProcess I added std.os.ChildProcess.exec for when you want to spawn a child process, wait for it to complete, and then capture the stdandard output into a buffer. pub fn exec(self: &Builder, argv: []const []const u8) -> []u8 { const max_output_size = 100 * 1024; const result = os.ChildProcess.exec(self.allocator, argv, null, null, max_output_size) %% |err| { std.debug.panic("Unable to spawn {}: {}", argv[0], @errorName(err)); }; switch (result.term) { os.ChildProcess.Term.Exited => |code| { if (code != 0) { warn("The following command exited with error code {}:\n", code); printCmd(null, argv); warn("stderr:{}\n", result.stderr); std.debug.panic("command failed"); } return result.stdout; }, else => { warn("The following command terminated unexpectedly:\n"); printCmd(null, argv); warn("stderr:{}\n", result.stderr); std.debug.panic("command failed"); }, } } std.sort Hejsil pointed out that the quicksort implementation in the standard library failed a simple test case. There was another problem with the implementation of sort in the standard library, which is that it used O(n) stack space via recursion. This is fundamentally insecure, especially if you consider that the length of an array you might want to sort could be user input. It prevents #157 from working as well. I had a look at Wikipedia''s Comparison of Sorting Algorithms and only 1 sorting algorithm checked all the boxes: Best case O(n) complexity (adaptive sort) Average case O(n * log(n)) complexity Worst case O(n * log(n)) complexity O(1) memory Stable sort And that algorithm is Block sort. I found a high quality implementation of block sort in C, which is licensed under the public domain. I ported the code from C to Zig, integrated it into the standard library, and it passed all tests first try. Amazing. Surely, I thought, there must be some edge case. So I created a simple fuzz tester: test "sort fuzz testing" { var rng = std.rand.Rand.init(0x12345678); const test_case_count = 10; var i: usize = 0; while (i < test_case_count) : (i += 1) { fuzzTest(&rng); } } var fixed_buffer_mem: [100 * 1024]u8 = undefined; fn fuzzTest(rng: &std.rand.Rand) { const array_size = rng.range(usize, 0, 1000); var fixed_allocator = mem.FixedBufferAllocator.init(fixed_buffer_mem[0..]); var array = %%fixed_allocator.allocator.alloc(IdAndValue, array_size); // populate with random data for (array) |*item, index| { item.id = index; item.value = rng.range(i32, 0, 100); } sort(IdAndValue, array, cmpByValue); var index: usize = 1; while (index < array.len) : (index += 1) { if (array[index].value == array[index - 1].value) { assert(array[index].id > array[index - 1].id); } else { assert(array[index].value > array[index - 1].value); } } } This test passed as well. And so I think this problem is solved. @export There is now an @export builtin function which can be used in a comptime block to conditionally export a function: const builtin = @import("builtin"); comptime { const strong_linkage = builtin.GlobalLinkage.Strong; if (builtin.link_libc) { @export("main", main, strong_linkage); } else if (builtin.os == builtin.Os.windows) { @export("WinMainCRTStartup", WinMainCRTStartup, strong_linkage); } else { @export("_start", _start, strong_linkage); } } It can also be used to create aliases: const builtin = @import("builtin"); const is_test = builtin.is_test; comptime { const linkage = if (is_test) builtin.GlobalLinkage.Internal else builtin.GlobalLinkage.Weak; const strong_linkage = if (is_test) builtin.GlobalLinkage.Internal else builtin.GlobalLinkage.Strong; @export("__letf2", @import("comparetf2.zig").__letf2, linkage); @export("__getf2", @import("comparetf2.zig").__getf2, linkage); if (!is_test) { // only create these aliases when not testing @export("__cmptf2", @import("comparetf2.zig").__letf2, linkage); @export("__eqtf2", @import("comparetf2.zig").__letf2, linkage); @export("__lttf2", @import("comparetf2.zig").__letf2, linkage); @export("__netf2", @import("comparetf2.zig").__letf2, linkage); @export("__gttf2", @import("comparetf2.zig").__getf2, linkage); } } Previous export syntax is still allowed. See #462 and #420. Labeled loops, blocks, break, and continue, and R.I.P. goto We used to have labels and goto like this: export fn entry() { label: goto label; } Now this does not work, because goto is gone. test.zig:2:10: error: expected token '';'', found '':'' label: ^ There are a few reasons to use goto, but all of the use cases are better served with other zig control flow features: cleanup pattern. Use defer and %defer instead. goto backward goto forward goto backward export fn entry() { start_over: while (some_condition) { // do something... goto start_over; } } Instead, use a loop! export fn entry() { outer: while (true) { while (some_condition) { // do something... continue :outer; } break; } } goto forward pub fn findSection(elf: &Elf, name: []const u8) -> %?&SectionHeader { var file_stream = io.FileInStream.init(elf.in_file); const in = &file_stream.stream; section_loop: for (elf.section_headers) |*elf_section| { if (elf_section.sh_type == SHT_NULL) continue; const name_offset = elf.string_section.offset + elf_section.name; %return elf.in_file.seekTo(name_offset); for (name) |expected_c| { const target_c = %return in.readByte(); if (target_c == 0 or expected_c != target_c) goto next_section; } { const null_byte = %return in.readByte(); if (null_byte == 0) return elf_section; } next_section: } return null; } Looks like the use case is breaking out of an outer loop: pub fn findSection(elf: &Elf, name: []const u8) -> %?&SectionHeader { var file_stream = io.FileInStream.init(elf.in_file); const in = &file_stream.stream; section_loop: for (elf.section_headers) |*elf_section| { if (elf_section.sh_type == SHT_NULL) continue; const name_offset = elf.string_section.offset + elf_section.name; %return elf.in_file.seekTo(name_offset); for (name) |expected_c| { const target_c = %return in.readByte(); if (target_c == 0 or expected_c != target_c) continue :section_loop; } { const null_byte = %return in.readByte(); if (null_byte == 0) return elf_section; } } return null; } You can also break out of arbitrary blocks: export fn entry() { outer: { while (some_condition) { // do something... break :outer; } } } This can be used to return a value from a block in the same way you can return a value from a function: export fn entry() { const value = init: { for (slice) |item| { if (item > 100) break :init item; } break :init 0; }; } Omitting a semicolon no longer causes the value to be returned by the block. Instead you must use explicit block labels to return a value from a block. I''m considering a keyword such as result which defaults to the current block. Removal of goto caused a regression in C-to-Zig translation: Switch statements no longer can be translated. However this code will be resurrected soon using labeled loops and labeled break instead of goto. See #346, #630, and #629. New IR pass iteration strategy Before: IR basic blocks are in arbitrary order when doing an IR pass, when a block is encountered, code must look at all the instructions in the old basic block, determine what blocks are referenced, and queue up those old basic blocks first. This had a bug while (cond) { if (false) { } break; } Pretty crazy right? Something as simple as this would crash the compiler. Now: IR basic blocks are required to be in an order that guarantees they will be referenced by a branch, before any instructions within are referenced. ir pass1 is updated to meet this constraint. hen doing an IR pass, we iterate over old basic blocks in the order they appear. Blocks which have not been referenced are discarded. fter the pass is complete, we must iterate again to look for old basic blocks which now point to incomplete new basic blocks, due to comptime code generation. his last part can probably be optimized - most of the time we don''t need to iterate over the basic block again. This improvement deletes a lot of messy code: 5 files changed, 288 insertions(+), 1243 deletions(-) And it also fixes comptime branches not being respected sometimes: export fn entry() { while (false) { @compileError("bad"); } } Before, this would cause a compile error. Now the while loop respects the implicit compile-time. See #667. Bug Fixes fix const and volatile qualifiers being dropped sometimes. in the expression &const a.b, the const (and/or volatile) qualifiers would be incorrectly dropped. See #655. fix compiler crash in a nullable if after an if in a switch prong of a switch with 2 prongs in an else. See #656. fix assert when wrapping zero bit type in nullable. See #659. fix crash when implicitly casting array of len 0 to slice. See #660. fix endianness of sub-byte integer fields in packed structs. In the future packed structs will require specifying endianness. See #307. fix std.os.path.resolve when the drive is missing. fix automatically C-translated functions not having debug information. fix crash when passing union enum with sub-byte field to const slice parameter. See #664. Miscellaneous changes Rename builtin.is_big_endian to builtin.endian. This is in preparation for having endianness be a pointer property, which is related to packed structs. See #307. Tested Zig with LLVM debug mode and fixed some bugs that were causing LLVM assertions. Add @noInlineCall. See #640. This fixes a crash in --release-safe and --release-fast modes where the optimizer inlines everything into _start and clobbers the command line argument data. If we were able to verify that the user''s code never reads command line args, we could leave off this "no inline" attribute. This might call for a patch to LLVM. It seems like inlining into a naked function should correctly bump the stack pointer. add i29 and u29 primitive types. u29 is the type of alignment, so it makes sense to be a primitive. probably in the future we''ll make any i or u followed by digits into a primitive. add implicit cast from enum tag type of union to const ptr to the union. closes #654 ELF stack traces support DW_AT_ranges, so sometimes when you would see "???" you now get a useful stack trace instead. add std.sort.min and std.sort.max functions std.fmt.bufPrint returns a possible error.BufferTooSmall instead of asserting that the buffer is large enough. Remove unnecessary inline calls in std.math. zig build now has a --search-prefix option. Any number of search prefixes can be specified. add some utf8 parsing utilities to the standard library. Thank you contributors! MIURA Masahiro fixed the color of compiler messages for light-themed terminals. (See #644) Peter Rönnquist added format for floating point numbers. {.x} where x is the number of decimals. (See #668) Thank you financial supporters! Special thanks to those who donate monthly: Lauren Chavis Andrea Orru Adrian Sinclair David Joseph jeff kelley Hasan Abdul-Rahman Wesley Kelley Jordan Torbiak Richard Ohnemus Martin Schwaighofer Matthew Mirek Rusin Brendon Scheinman Pyry Kontio Thomas Ballinger Peter Ronnquist Robert Paul Herman Audun Wilhelmsen Marko Mikulicic Anthony J. Benik Caius Tyler Philbrick Jeremy Larkin Rasmus Rønn Nielsen'
content: extracted
html: 2018-01-03-zig-december-2017-in-review.html
---

I figured since I [ask people to donate monthly](https://github.com/users/andrewrk/sponsorship), I will start giving a monthly progress report to provide accountability.

So here's everything that happened in [Zig land](http://ziglang.org/) in December 2017:

## enum tag types

You can now specify the tag type for an enum ([#305](https://github.com/zig-lang/zig/issues/305)):

```
const Small2 = enum (u2) {
    One,
    Two,
};
```

If you specify the tag type for an enum, you can put it in a packed struct:

```
const A = enum (u3) {
    One,
    Two,
    Three,
    Four,
    One2,
    Two2,
    Three2,
    Four2,
};

const B = enum (u3) {
    One3,
    Two3,
    Three3,
    Four3,
    One23,
    Two23,
    Three23,
    Four23,
};

const C = enum (u2) {
    One4,
    Two4,
    Three4,
    Four4,
};

const BitFieldOfEnums = packed struct {
    a: A,
    b: B,
    c: C,
};

const bit_field_1 = BitFieldOfEnums {
    .a = A.Two,
    .b = B.Three3,
    .c = C.Four4,
};
```

You can no longer cast from an enum to an arbitrary integer. Instead you must cast to the enum tag type and vice versa:

```
const Small2 = enum (u2) {
    One,
    Two,
};
test "casting enum to its tag type" {
    testCastEnumToTagType(Small2.Two);
}

fn testCastEnumToTagType(value: Small2) {
    assert(u2(value) == 1);
}
```

## enum tag values

Now you can set the tag values of enums:

```
const MultipleChoice = enum(u32) {
    A = 20,
    B = 40,
    C = 60,
    D = 1000,
};
```

## Complete enum and union overhaul

Related issue: [#618](https://github.com/zig-lang/zig/issues/618)

Enums are now a simple mapping between a symbol and a number. They can no longer contain payloads.

Unions have been upgraded and can now accept an enum as an argument:

```
const TheTag = enum {A, B, C};
const TheUnion = union(TheTag) { A: i32, B: i32, C: i32 };
test "union field access gives the enum values" {
    assert(TheUnion.A == TheTag.A);
    assert(TheUnion.B == TheTag.B);
    assert(TheUnion.C == TheTag.C);
}
```

If you want to auto-create an enum for a union, you can use the `enum` keyword like this:

```
const TheUnion2 = union(enum) {
    Item1,
    Item2: i32,
};
```

You can switch on a union-enum just like you could previously with an enum:

```
const SwitchProngWithVarEnum = union(enum) {
    One: i32,
    Two: f32,
    Meh: void,
};
fn switchProngWithVarFn(a: &const SwitchProngWithVarEnum) {
    switch(*a) {
        SwitchProngWithVarEnum.One => |x| {
            assert(x == 13);
        },
        SwitchProngWithVarEnum.Two => |x| {
            assert(x == 13.0);
        },
        SwitchProngWithVarEnum.Meh => |x| {
            const v: void = x;
        },
    }
}
```

However, if you do not give an enum to a union, the tag value is not visible to the programmer:

```
const Payload = union {
    A: i32,
    B: f64,
    C: bool,
};
export fn entry() {
    const a = Payload { .A = 1234 };
    foo(a);
}
fn foo(a: &const Payload) {
    switch (*a) {
        Payload.A => {},
        else => unreachable,
    }
}
```

```
test.zig:11:13: error: switch on union which has no attached enum
    switch (*a) {
            ^
test.zig:1:17: note: consider 'union(enum)' here
const Payload = union {
                ^
test.zig:12:16: error: container 'Payload' has no member called 'A'
        Payload.A => {},
               ^
```

There is still debug safety though!

```
const Foo = union {
    float: f32,
    int: u32,
};

pub fn main() -> %void {
    var f = Foo { .int = 42 };
    bar(&f);
}

fn bar(f: &Foo) {
    f.float = 12.34;
}
```

```
access of inactive union field
lib/zig/std/special/panic.zig:12:35: 0x0000000000203674 in ??? (test)
        @import("std").debug.panic("{}", msg);
                                  ^
test.zig:12:6: 0x0000000000217bd7 in ??? (test)
    f.float = 12.34;
     ^
test.zig:8:8: 0x0000000000217b7c in ??? (test)
    bar(&f);
       ^
Aborted
```

However, if you make an `extern union` to be compatible with C code, there is no debug safety, just like a C union.

Other tidbits:

- `@enumTagName` is renamed to [@tagName](http://ziglang.org/documentation/master/#builtin-tagName)
- `@EnumTagType` is renamed to [@TagType](http://ziglang.org/documentation/master/#builtin-TagType), and it works on both enums and union-enums.
- There is no longer an `EnumTag` type
- It is now an error for enums and unions to have 0 fields. However you can still have a struct with 0 fields.
- union values can implicitly cast to enum values when the enum type is the tag type of the union and the union value tag is comptime known to have a void field type. likewise, enum values can implicitly cast to union values. See [#642](https://github.com/zig-lang/zig/issues/642).

```
test "cast tag type of union to union" {
    var x: Value2 = Letter2.B;
    assert(Letter2(x) == Letter2.B);
}
const Letter2 = enum { A, B, C };
const Value2 = union(Letter2) { A: i32, B, C, };

test "implicit cast union to its tag type" {
    var x: Value2 = Letter2.B;
    assert(x == Letter2.B);
    giveMeLetterB(x);
}
fn giveMeLetterB(x: Letter2) {
    assert(x == Value2.B);
}
```

## Update LLD fork to 5.0.1rc2

We have a fork of LLD in the zig project because of several upstream issues, all of which I have filed bugs for:

- LDD calls exit after successful link. Patch to fix sent upstream and accepted.
- LDD crashes on a linker script with empty sections. Fixed upstream.
- Buggy MACH-O code causes assertion failure in simple object. We have a hacky workaround for this bug in zig's fork of LLD, but the workaround is not good enough to send upstream. See [#662](https://github.com/zig-lang/zig/issues/662) for more details.
- MACH-O: Bad ASM code generated for \_\_stub\_helpers section Thanks to Patricio V. for sending the fix upstream, which has been accepted.

When LLVM 6.0.0 comes out, Zig will have to keep its fork because of the one issue, but we can drop all the other patches since they have been accepted upstream.

## Self-hosted compiler progress

The self-hosted compiler effort has begun.

So far we have a tokenizer, and an incomplete parser and formatter. The code uses no recursion and therefore has compile-time known stack space usage. See [#157](https://github.com/zig-lang/zig/issues/157)

The self-hosted compiler works on every supported platform, is built using the zig build system, tested with `zig test`, links against LLVM, and can import **100%** of the LLVM symbols from the LLVM C-API .h files - *even the inline functions*.

There is one C++ file in Zig which uses the more powerful LLVM C++ API (for example to create debug information) and exposes a C API. This file is now shared between the C++ self-hosted compiler and the self-hosted compiler. In stage1, we create a static library with this one file in it, and then use that library in both the C++ compiler and the self-hosted compiler.

## Higher level arg-parsing API

It's really a shame that Windows command line parsing requires you to allocate memory. This means that to have a cross-platform API for command line arguments, even though in POSIX it can never fail, we have to handle the possibility because of Windows. This lead to a command line args API like this:

```
pub fn main() -> %void {
    var arg_it = os.args();
    // skip my own exe name
    _ = arg_it.skip();
    while (arg_it.next(allocator)) |err_or_arg| {
        const arg = %return err_or_arg;
        defer allocator.free(arg);
        // use the arg...
    }
}
```

Yikes, a bit cumbersome. I added a higher level API. Now you can call `std.os.argsAlloc` and get a `%[]const []u8`, and you just have to call `std.os.argsFree` when you're done with it.

```
pub fn main() -> %void {
    const allocator = std.heap.c_allocator;

    const args = %return os.argsAlloc(allocator);
    defer os.argsFree(allocator, args);

    var arg_i: usize = 1;
    while (arg_i < args.len) : (arg_i += 1) {
        const arg = args[arg_i];
        // do something with arg...
    }
}
```

Better! Single point of failure.

For now this uses the other API under the hood, but it could be reimplemented with the same API to do a single allocation.

I added a new kind of test to make sure command line argument parsing works.

## Automatic C-to-Zig translation

- Translation now understands enum tag types.
- I refactored the C macro parsing, and made it understand pointer casting. Now some kinds of int-to-ptr code in .h files for embedded programming just work:

```
#define NRF_GPIO ((NRF_GPIO_Type *) NRF_GPIO_BASE)
```

Zig now understands this C macro.

## std.mem

- add `aligned` functions to Allocator interface
- `mem.Allocator` initializes bytes to undefined. This does nothing in ReleaseFast mode. In Debug and ReleaseSafe modes, it initializes bytes to `0xaa` which helps catch memory errors.
- add `mem.FixedBufferAllocator`

## std.os.ChildProcess

I added `std.os.ChildProcess.exec` for when you want to spawn a child process, wait for it to complete, and then capture the stdandard output into a buffer.

```
pub fn exec(self: &Builder, argv: []const []const u8) -> []u8 {
    const max_output_size = 100 * 1024;
    const result = os.ChildProcess.exec(self.allocator, argv, null, null, max_output_size) %% |err| {
        std.debug.panic("Unable to spawn {}: {}", argv[0], @errorName(err));
    };
    switch (result.term) {
        os.ChildProcess.Term.Exited => |code| {
            if (code != 0) {
                warn("The following command exited with error code {}:\n", code);
                printCmd(null, argv);
                warn("stderr:{}\n", result.stderr);
                std.debug.panic("command failed");
            }
            return result.stdout;
        },
        else => {
            warn("The following command terminated unexpectedly:\n");
            printCmd(null, argv);
            warn("stderr:{}\n", result.stderr);
            std.debug.panic("command failed");
        },
    }
}
```

## std.sort

[Hejsil](https://github.com/Hejsil) [pointed out](https://github.com/zig-lang/zig/issues/657) that the quicksort implementation in the standard library failed a simple test case.

There was another problem with the implementation of sort in the standard library, which is that it used `O(n)` stack space via recursion. This is fundamentally insecure, especially if you consider that the length of an array you might want to sort could be user input. It prevents [#157](https://github.com/zig-lang/zig/issues/157) from working as well.

I had a look at [Wikipedia's Comparison of Sorting Algorithms](https://en.wikipedia.org/wiki/Sorting_algorithm#Comparison_of_algorithms) and only 1 sorting algorithm checked all the boxes:

- Best case `O(n)` complexity (adaptive sort)
- Average case `O(n * log(n))` complexity
- Worst case `O(n * log(n))` complexity
- `O(1)` memory
- Stable sort

And that algorithm is [Block sort](https://en.wikipedia.org/wiki/Block_sort).

I found a [high quality implementation of block sort in C](https://github.com/BonzaiThePenguin/WikiSort/blob/master/WikiSort.c), which is licensed under the public domain.

I ported the code from C to Zig, integrated it into the standard library, and it passed all tests first try. Amazing.

Surely, I thought, there must be some edge case. So I created a simple fuzz tester:

```
test "sort fuzz testing" {
    var rng = std.rand.Rand.init(0x12345678);
    const test_case_count = 10;
    var i: usize = 0;
    while (i < test_case_count) : (i += 1) {
        fuzzTest(&rng);
    }
}

var fixed_buffer_mem: [100 * 1024]u8 = undefined;

fn fuzzTest(rng: &std.rand.Rand) {
    const array_size = rng.range(usize, 0, 1000);
    var fixed_allocator = mem.FixedBufferAllocator.init(fixed_buffer_mem[0..]);
    var array = %%fixed_allocator.allocator.alloc(IdAndValue, array_size);
    // populate with random data
    for (array) |*item, index| {
        item.id = index;
        item.value = rng.range(i32, 0, 100);
    }
    sort(IdAndValue, array, cmpByValue);

    var index: usize = 1;
    while (index < array.len) : (index += 1) {
        if (array[index].value == array[index - 1].value) {
            assert(array[index].id > array[index - 1].id);
        } else {
            assert(array[index].value > array[index - 1].value);
        }
    }
}
```

This test passed as well. And so I think this problem is solved.

## @export

There is now an [@export](http://ziglang.org/documentation/master/#builtin-export) builtin function which can be used in a comptime block to conditionally export a function:

```
const builtin = @import("builtin");

comptime {
    const strong_linkage = builtin.GlobalLinkage.Strong;
    if (builtin.link_libc) {
        @export("main", main, strong_linkage);
    } else if (builtin.os == builtin.Os.windows) {
        @export("WinMainCRTStartup", WinMainCRTStartup, strong_linkage);
    } else {
        @export("_start", _start, strong_linkage);
    }
}
```

It can also be used to create aliases:

```
const builtin = @import("builtin");
const is_test = builtin.is_test;

comptime {
    const linkage = if (is_test) builtin.GlobalLinkage.Internal else builtin.GlobalLinkage.Weak;
    const strong_linkage = if (is_test) builtin.GlobalLinkage.Internal else builtin.GlobalLinkage.Strong;

    @export("__letf2", @import("comparetf2.zig").__letf2, linkage);
    @export("__getf2", @import("comparetf2.zig").__getf2, linkage);

    if (!is_test) {
        // only create these aliases when not testing
        @export("__cmptf2", @import("comparetf2.zig").__letf2, linkage);
        @export("__eqtf2", @import("comparetf2.zig").__letf2, linkage);
        @export("__lttf2", @import("comparetf2.zig").__letf2, linkage);
        @export("__netf2", @import("comparetf2.zig").__letf2, linkage);
        @export("__gttf2", @import("comparetf2.zig").__getf2, linkage);
    }
}
```

Previous export syntax is still allowed. See [#462](<https://github.com/zig-lang/zig/issues/462 >) and [#420](https://github.com/zig-lang-zig/issues/420).

## Labeled loops, blocks, break, and continue, and R.I.P. goto

We used to have labels and goto like this:

```
export fn entry() {
    label:
    goto label;
}
```

Now this does not work, because goto is gone.

```
test.zig:2:10: error: expected token ';', found ':'
    label:
         ^
```

There are a few reasons to use goto, but all of the use cases are better served with other zig control flow features:

- cleanup pattern. Use `defer` and `%defer` instead.
- goto backward
- goto forward

### goto backward

```
export fn entry() {
    start_over:

    while (some_condition) {
        // do something...
        goto start_over;
    }
}
```

Instead, use a loop!

```
export fn entry() {
    outer: while (true) {

        while (some_condition) {
            // do something...
            continue :outer;
        }

        break;
    }
}
```

### goto forward

```
pub fn findSection(elf: &Elf, name: []const u8) -> %?&SectionHeader {
    var file_stream = io.FileInStream.init(elf.in_file);
    const in = &file_stream.stream;

    section_loop: for (elf.section_headers) |*elf_section| {
        if (elf_section.sh_type == SHT_NULL) continue;

        const name_offset = elf.string_section.offset + elf_section.name;
        %return elf.in_file.seekTo(name_offset);

        for (name) |expected_c| {
            const target_c = %return in.readByte();
            if (target_c == 0 or expected_c != target_c) goto next_section;
        }

        {
            const null_byte = %return in.readByte();
            if (null_byte == 0) return elf_section;
        }
next_section:
    }

    return null;
}
```

Looks like the use case is breaking out of an outer loop:

```
pub fn findSection(elf: &Elf, name: []const u8) -> %?&SectionHeader {
    var file_stream = io.FileInStream.init(elf.in_file);
    const in = &file_stream.stream;

    section_loop: for (elf.section_headers) |*elf_section| {
        if (elf_section.sh_type == SHT_NULL) continue;

        const name_offset = elf.string_section.offset + elf_section.name;
        %return elf.in_file.seekTo(name_offset);

        for (name) |expected_c| {
            const target_c = %return in.readByte();
            if (target_c == 0 or expected_c != target_c) continue :section_loop;
        }

        {
            const null_byte = %return in.readByte();
            if (null_byte == 0) return elf_section;
        }
    }

    return null;
}
```

You can also break out of arbitrary blocks:

```
export fn entry() {
    outer: {

        while (some_condition) {
            // do something...
            break :outer;
        }
    }
}
```

This can be used to return a value from a block in the same way you can return a value from a function:

```
export fn entry() {
    const value = init: {
        for (slice) |item| {
            if (item > 100)
                break :init item;
        }
        break :init 0;
    };
}
```

Omitting a semicolon no longer causes the value to be returned by the block. Instead you must use explicit block labels to return a value from a block. I'm considering a keyword such as `result` which defaults to the current block.

Removal of goto caused a regression in C-to-Zig translation: Switch statements no longer can be translated. However this code will be resurrected soon using labeled loops and labeled break instead of goto.

See [#346](https://github.com/zig-lang/zig/issues/346), [#630](https://github.com/zig-lang-zig/issues/630), and [#629](https://github.com/zig-lang-zig/issues/629).

## New IR pass iteration strategy

Before:

- IR basic blocks are in arbitrary order
- when doing an IR pass, when a block is encountered, code must look at all the instructions in the old basic block, determine what blocks are referenced, and queue up those old basic blocks first.
- This had a bug

```
while (cond) {
    if (false) { }
    break;
}
```

Pretty crazy right? Something as simple as this would crash the compiler.

Now:

- IR basic blocks are required to be in an order that guarantees they will be referenced by a branch, before any instructions within are referenced. ir pass1 is updated to meet this constraint.
- hen doing an IR pass, we iterate over old basic blocks in the order they appear. Blocks which have not been referenced are discarded.
- fter the pass is complete, we must iterate again to look for old basic blocks which now point to incomplete new basic blocks, due to comptime code generation.
- his last part can probably be optimized - most of the time we don't need to iterate over the basic block again.

This improvement deletes a lot of messy code:

```
 5 files changed, 288 insertions(+), 1243 deletions(-) 
```

And it also fixes comptime branches not being respected sometimes:

```
export fn entry() {
    while (false) {
        @compileError("bad");
    }
}
```

Before, this would cause a compile error. Now the while loop respects the implicit compile-time.

See [#667](https://github.com/zig-lang/zig/issues/667).

## Bug Fixes

- fix const and volatile qualifiers being dropped sometimes. in the expression `&const a.b`, the const (and/or volatile) qualifiers would be incorrectly dropped. See [#655](https://github.com/zig-lang/zig/issues/655).
- fix compiler crash in a nullable if after an if in a switch prong of a switch with 2 prongs in an else. See [#656](https://github.com/zig-lang/zig/issues/656).
- fix assert when wrapping zero bit type in nullable. See [#659](https://github.com/zig-lang/zig/issues/659).
- fix crash when implicitly casting array of len 0 to slice. See [#660](https://github.com/zig-lang/zig/issues/660).
- fix endianness of sub-byte integer fields in packed structs. In the future packed structs will require specifying endianness. See [#307](https://github.com/zig-lang/zig/issues/307).
- fix `std.os.path.resolve` when the drive is missing.
- fix automatically C-translated functions not having debug information.
- fix crash when passing union enum with sub-byte field to const slice parameter. See [#664](https://github.com/zig-lang/zig/issues/664).

## Miscellaneous changes

- Rename `builtin.is_big_endian` to `builtin.endian`. This is in preparation for having endianness be a pointer property, which is related to packed structs. See [#307](https://github.com/zig-lang/zig/issues/307).
- Tested Zig with LLVM debug mode and fixed some bugs that were causing LLVM assertions.
- Add [@noInlineCall](http://ziglang.org/documentation/master/#builtin-noInlineCall). See [#640](https://github.com/zig-lang/zig/issues/640). This fixes a crash in `--release-safe` and `--release-fast` modes where the optimizer inlines everything into `_start` and clobbers the command line argument data. If we were able to verify that the user's code never reads command line args, we could leave off this "no inline" attribute. This might call for a patch to LLVM. It seems like inlining into a naked function should correctly bump the stack pointer.
- add `i29` and `u29` primitive types. `u29` is the type of alignment, so it makes sense to be a primitive. probably in the future we'll make any `i` or `u` followed by digits into a primitive.
- add implicit cast from enum tag type of union to const ptr to the union. closes [#654](https://github.com/zig-lang/zig/issues/654)
- ELF stack traces support `DW_AT_ranges`, so sometimes when you would see "???" you now get a useful stack trace instead.
- add `std.sort.min` and `std.sort.max` functions
- `std.fmt.bufPrint` returns a possible `error.BufferTooSmall` instead of asserting that the buffer is large enough.
- Remove unnecessary inline calls in `std.math`.
- `zig build` now has a `--search-prefix` option. Any number of search prefixes can be specified.
- add some utf8 parsing utilities to the standard library.

## Thank you contributors!

- **MIURA Masahiro** fixed the color of compiler messages for light-themed terminals. (See [#644](https://github.com/zig-lang/zig/issues/644))
- **Peter Rönnquist** added format for floating point numbers. `{.x}` where `x` is the number of decimals. (See [#668](https://github.com/zig-lang/zig/issues/668))

## Thank you financial supporters!

Special thanks to those who [donate monthly](https://github.com/users/andrewrk/sponsorship):

- Lauren Chavis
- Andrea Orru
- Adrian Sinclair
- David Joseph
- jeff kelley
- Hasan Abdul-Rahman
- Wesley Kelley
- Jordan Torbiak
- Richard Ohnemus
- Martin Schwaighofer
- Matthew
- Mirek Rusin
- Brendon Scheinman
- Pyry Kontio
- Thomas Ballinger
- Peter Ronnquist
- Robert Paul Herman
- Audun Wilhelmsen
- Marko Mikulicic
- Anthony J. Benik
- Caius
- Tyler Philbrick
- Jeremy Larkin
- Rasmus Rønn Nielsen

Thanks for reading my blog post.
