---
title: A Better Way to Implement Bit Fields
link: https://andrewkelley.me/post/a-better-way-to-implement-bit-fields.html
source: andrewkelley-me
published: 2017-02-17T00:43:53Z
updated: 2017-02-17T00:43:53Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'A Better Way to Implement Bit-Fields One of the main use cases for the Zig Programming Language is operating system development and embedded development. So, what better way to make sure the language is suitable than to work on a project in this field? This is why I am creating a 4-player arcade game that runs directly on the Raspberry Pi 3 hardware. The project has only just begun, but already it is revealing important fixes and features in Zig, and I am updating the compiler to incorporate these things as I work. The next thing I''m working on is adding a USB controller driver, so that I can use a gamepad to test the game. I noticed when looking at some reference code that there are large structs full of bit-fields, and these turn out to be extremely convenient: extern volatile struct CoreGlobalRegs { volatile struct { volatile bool sesreqscs : 1; volatile bool sesreq : 1; volatile bool vbvalidoven:1; volatile bool vbvalidovval:1; volatile bool avalidoven:1; volatile bool avalidovval:1; volatile bool bvalidoven:1; volatile bool bvalidovval:1; volatile bool hstnegscs:1; volatile bool hnpreq:1; volatile bool HostSetHnpEnable : 1; volatile bool devhnpen:1; volatile unsigned _reserved12_15:4; volatile bool conidsts:1; volatile unsigned dbnctime:1; volatile bool ASessionValid : 1; volatile bool BSessionValid : 1; volatile unsigned OtgVersion : 1; volatile unsigned _reserved21:1; volatile unsigned multvalidbc:5; volatile bool chirpen:1; volatile unsigned _reserved28_31:4; } __attribute__ ((__packed__)) OtgControl; // +0x0 volatile struct { volatile unsigned _reserved0_1 : 2; // @0 volatile bool SessionEndDetected : 1; // @2 volatile unsigned _reserved3_7 : 5; // @3 volatile bool SessionRequestSuccessStatusChange : 1; // @8 volatile bool HostNegotiationSuccessStatusChange : 1; // @9 volatile unsigned _reserved10_16 : 7; // @10 volatile bool HostNegotiationDetected : 1; // @17 volatile bool ADeviceTimeoutChange : 1; // @18 volatile bool DebounceDone : 1; // @19 volatile unsigned _reserved20_31 : 12; // @20 } __attribute__ ((__packed__)) OtgInterrupt; // +0x4 // ... I''ll stop there, but this struct goes on for pages and pages. You can see that in C, bit-fields have special syntax. The fields have a type like normal, and then an extra colon and a number of bits. Bit-fields in C have acquired a poor reputation in the programming community, for a few reasons: Poor performance due to loading more bytes than necessary Largely implementation-defined behavior Platform dependence Linus Torvalds: bitfields make it harder to work with combinations of flags Many experienced developers have given up on the usefulness of bit-fields and prefer to manually wrangle their binary data. Some of these problems I can address in Zig, by loading the minimal number of bytes necessary, defining how bit-fields are laid out in memory, and ensuring that bit-fields work the same or at least predictably on all platforms. Some of the problems are inherently tricky, such as the question of how to deal with endianness when a bit-field has more than 8 bits and crosses a byte boundary. With these things in mind, I set out to implement bit-fields in Zig, and now I am pleased to announce that work is complete. Zig has bit-fields, and it required no syntax additions. Zig takes advantage of the fact that types are first-class values at compile-time. There is a built-in function which returns an integer type with the specified signness and bit count, and it can be used to get access to uncommon integer types like this: const i13 = @intType(true, 13); These uncommon integer types work like normal integer types. Arithmetic, casting, and overflow are generalized to work with any integer type. These integer types are one component to the way bit-fields are implemented in Zig. The other component takes advantage of the fact that Zig has 3 different kinds of struct layouts: Default - compiler may re-arrange fields and insert padding extern - compatible with the target environment C ABI packed - fields in exact order specified, no padding In a packed struct, the programmer is directly in charge of the memory layout of the struct. Fields with integer types take up exactly as many bits as the integer type specifies. If a field greater than 8 bits is byte-aligned, it is represented in memory with the endianness of the host. If the field is not byte-aligned, it is represented in memory in big-endian. Boolean values are represented as 1 bit in packed structs. So that''s it. To make a bit-field, you have a packed struct with fields that are integers with the bit sizes you want. For illustration, here is the above code translated into Zig: const u1 = @intType(false, 1); const u12 = @intType(false, 12); const CoreGlobalRegs = packed struct { OtgControl: struct { sesreqscs: bool, sesreq: bool, vbvalidoven: bool, vbvalidovval: bool, avalidoven: bool, avalidovval: bool, bvalidoven: bool, bvalidovval: bool, hstnegscs: bool, hnpreq: bool, HostSetHnpEnable: bool, devhnpen: bool, _reserved12_15: u4, conidsts: bool, dbnctime: u1, ASessionValid: bool, BSessionValid: bool, OtgVersion: u1, _reserved21: u1, multvalidbc: u5, chirpen: bool, _reserved28_31: u4, }, // +0x0 OtgInterrupt: packed struct { _reserved0_1: u2, // @0 SessionEndDetected: bool, // @2 _reserved3_7: u5, // @3 SessionRequestSuccessStatusChange: bool, // @8 HostNegotiationSuccessStatusChange: bool, // @9 _reserved10_16: u7, // @10 HostNegotiationDetected: bool, // @17 ADeviceTimeoutChange: bool, // @18 DebounceDone: bool, // @19 _reserved20_31: u12, // @20 }, // +0x4 // ... By the way it''s nice to know that the USB protocol has a bit to indicate "a valid oven". I wonder when that is used. You may notice that we do a bit of setup before declaring the struct by creating these integer types. Currently, only integer types of size 8, 16, 32, and 64 are provided by the compiler globally. But perhaps more integer types can be provided as primitive types, or perhaps there will be a standard library file to import and get more integer types. You may also notice that the volatile keyword is gone. This is a different issue, but Zig handles volatile at the pointer level. So instead of putting the keyword on every field, you make sure the pointer to the whole struct is volatile, and then any loads and stores done via the pointer become volatile, and any pointers derived from the volatile pointer are also volatile. One point of comparison with C bit-fields. Take a look at this code: struct Foo { unsigned a : 3; unsigned b : 3; unsigned c : 2; } __attribute__ ((__packed__)); struct Foo foo = {1, 2, 3}; unsigned f(void) { unsigned *ptr = &foo.b; return *ptr; } Here we try to take the address of a bit-field, and clang doesn''t like that idea so much: test.c:10:12: error: address of bit-field requested return &foo.b; ^~~~~~ Meanwhile, in Zig, this works just fine: const Foo = packed struct { a: u3, b: u3, c: u2, }; var foo = Foo { .a = 1, .b = 2, .c = 3, }; fn f() u3 { const ptr = &foo.b; return ptr.*; } The bit offset and length is carried in the type of the pointer, so you get an error if you try to pass such a pointer to a function expecting a normal, byte-aligned value: const BitField = packed struct { a: u3, b: u3, c: u2, }; fn foo(bit_field: *const BitField) u3 { return bar(&bit_field.b); } fn bar(x: *const u3) u3 { return x.*; } In this case the compiler catches the mistake: ./test.zig:8:26: error: expected type ''*const u3'', found ''*:3:6 const u3'' return bar(&bit_field.b); ^ There are bound to be some edge cases and bugs as I polish this feature, but I am pleased that it turned out to integrate so cleanly into Zig''s minimal design.'
content: extracted
html: 2017-02-17-a-better-way-to-implement-bit-fields.html
---

One of the main use cases for the [Zig Programming Language](https://ziglang.org/) is operating system development and embedded development. So, what better way to make sure the language is suitable than to work on a project in this field?

This is why I am creating a [4-player arcade game that runs directly on the Raspberry Pi 3 hardware](https://github.com/andrewrk/clashos). The project has only just begun, but already it is revealing important fixes and features in Zig, and I am updating the compiler to incorporate these things as I work.

The next thing I'm working on is adding a USB controller driver, so that I can use a gamepad to test the game. I noticed when looking at [some reference code](https://github.com/Chadderz121/csud/blob/e13b9355d043a9cdd384b335060f1bc0416df61e/include/hcd/dwc/designware20.h#L164) that there are large structs full of bit-fields, and these turn out to be extremely convenient:

```
extern volatile struct CoreGlobalRegs {
	volatile struct {
		volatile bool sesreqscs : 1;
		volatile bool sesreq : 1;
		volatile bool vbvalidoven:1;
		volatile bool vbvalidovval:1;
		volatile bool avalidoven:1;
		volatile bool avalidovval:1;
		volatile bool bvalidoven:1;
		volatile bool bvalidovval:1;
		volatile bool hstnegscs:1;
		volatile bool hnpreq:1;
		volatile bool HostSetHnpEnable : 1;
		volatile bool devhnpen:1;
		volatile unsigned _reserved12_15:4;
		volatile bool conidsts:1;
		volatile unsigned dbnctime:1;
		volatile bool ASessionValid : 1;
		volatile bool BSessionValid : 1;
		volatile unsigned OtgVersion : 1;
		volatile unsigned _reserved21:1;
		volatile unsigned multvalidbc:5;
		volatile bool chirpen:1;
		volatile unsigned _reserved28_31:4;
	} __attribute__ ((__packed__)) OtgControl; // +0x0
	volatile struct {
		volatile unsigned _reserved0_1 : 2; // @0
		volatile bool SessionEndDetected : 1; // @2
		volatile unsigned _reserved3_7 : 5; // @3
		volatile bool SessionRequestSuccessStatusChange : 1; // @8
		volatile bool HostNegotiationSuccessStatusChange : 1; // @9
		volatile unsigned _reserved10_16 : 7; // @10
		volatile bool HostNegotiationDetected : 1; // @17
		volatile bool ADeviceTimeoutChange : 1; // @18
		volatile bool DebounceDone : 1; // @19
		volatile unsigned _reserved20_31 : 12; // @20
	} __attribute__ ((__packed__)) OtgInterrupt; // +0x4
// ...
```

I'll stop there, but this struct goes on for pages and pages. You can see that in C, bit-fields have special syntax. The fields have a type like normal, and then an extra colon and a number of bits.

Bit-fields in C have acquired a poor reputation in the programming community, for a few reasons:

- [Poor performance due to loading more bytes than necessary](https://www.flamingspork.com/blog/2014/11/14/c-bitfields-considered-harmful/)
- [Largely implementation-defined behavior](https://gcc.gnu.org/onlinedocs/gcc/Structures-unions-enumerations-and-bit-fields-implementation.html)
- [Platform dependence](http://stackoverflow.com/a/4240989/432)
- [Linus Torvalds: bitfields make it harder to work with combinations of flags](http://yarchive.net/comp/linux/bitfields.html)

Many experienced developers have given up on the usefulness of bit-fields and prefer to manually wrangle their binary data.

Some of these problems I can address in Zig, by loading the minimal number of bytes necessary, defining how bit-fields are laid out in memory, and ensuring that bit-fields work the same or at least predictably on all platforms. Some of the problems are inherently tricky, such as the question of how to deal with endianness when a bit-field has more than 8 bits and crosses a byte boundary.

With these things in mind, I set out to implement bit-fields in Zig, and now I am pleased to announce that work is complete. Zig has bit-fields, and it required no syntax additions.

Zig takes advantage of the fact that types are first-class values at compile-time. There is a built-in function which returns an integer type with the specified signness and bit count, and it can be used to get access to uncommon integer types like this:

```
const i13 = @intType(true, 13);
```

These uncommon integer types work like normal integer types. Arithmetic, casting, and overflow are generalized to work with any integer type.

These integer types are one component to the way bit-fields are implemented in Zig. The other component takes advantage of the fact that Zig has 3 different kinds of `struct` layouts:

- Default - compiler may re-arrange fields and insert padding
- `extern` - compatible with the target environment C ABI
- `packed` - fields in exact order specified, no padding

In a `packed` struct, the programmer is directly in charge of the memory layout of the struct. Fields with integer types take up exactly as many bits as the integer type specifies. If a field greater than 8 bits is byte-aligned, it is represented in memory with the endianness of the host. If the field is not byte-aligned, it is represented in memory in big-endian. Boolean values are represented as 1 bit in packed structs.

So that's it. To make a bit-field, you have a packed struct with fields that are integers with the bit sizes you want.

For illustration, here is the above code translated into Zig:

```
const u1 = @intType(false, 1);
const u12 = @intType(false, 12);

const CoreGlobalRegs = packed struct {
    OtgControl: struct {
        sesreqscs: bool,
        sesreq: bool,
        vbvalidoven: bool,
        vbvalidovval: bool,
        avalidoven: bool,
        avalidovval: bool,
        bvalidoven: bool,
        bvalidovval: bool,
        hstnegscs: bool,
        hnpreq: bool,
        HostSetHnpEnable: bool,
        devhnpen: bool,
        _reserved12_15: u4,
        conidsts: bool,
        dbnctime: u1,
        ASessionValid: bool,
        BSessionValid: bool,
        OtgVersion: u1,
        _reserved21: u1,
        multvalidbc: u5,
        chirpen: bool,
        _reserved28_31: u4,
    }, // +0x0
    OtgInterrupt: packed struct {
        _reserved0_1: u2, // @0
        SessionEndDetected: bool, // @2
        _reserved3_7: u5, // @3
        SessionRequestSuccessStatusChange: bool, // @8
        HostNegotiationSuccessStatusChange: bool, // @9
        _reserved10_16: u7, // @10
        HostNegotiationDetected: bool, // @17
        ADeviceTimeoutChange: bool, // @18
        DebounceDone: bool, // @19
        _reserved20_31: u12, // @20
    }, // +0x4
// ...
```

By the way it's nice to know that the USB protocol has a bit to indicate "a valid oven". I wonder when that is used.

You may notice that we do a bit of setup before declaring the struct by creating these integer types. Currently, only integer types of size `8`, `16`, `32`, and `64` are provided by the compiler globally. But perhaps more integer types can be provided as primitive types, or perhaps there will be a standard library file to import and get more integer types.

You may also notice that the `volatile` keyword is gone. This is a different issue, but Zig handles volatile at the pointer level. So instead of putting the keyword on every field, you make sure the pointer to the whole struct is volatile, and then any loads and stores done via the pointer become volatile, and any pointers derived from the volatile pointer are also volatile.

One point of comparison with C bit-fields. Take a look at this code:

```
struct Foo {
    unsigned a : 3;
    unsigned b : 3;
    unsigned c : 2;
} __attribute__ ((__packed__));

struct Foo foo = {1, 2, 3};

unsigned f(void) {
    unsigned *ptr = &foo.b;
    return *ptr;
}
```

Here we try to take the address of a bit-field, and clang doesn't like that idea so much:

```
test.c:10:12: error: address of bit-field requested
    return &foo.b;
           ^~~~~~
```

Meanwhile, in Zig, this works just fine:

```
const Foo = packed struct {
    a: u3,
    b: u3,
    c: u2,
};

var foo = Foo {
    .a = 1,
    .b = 2,
    .c = 3,
};

fn f() u3 {
    const ptr = &foo.b;
    return ptr.*;
}
```

The bit offset and length is carried in the type of the pointer, so you get an error if you try to pass such a pointer to a function expecting a normal, byte-aligned value:

```
const BitField = packed struct {
    a: u3,
    b: u3,
    c: u2,
};

fn foo(bit_field: *const BitField) u3 {
    return bar(&bit_field.b);
}

fn bar(x: *const u3) u3 {
    return x.*;
}
```

In this case the compiler catches the mistake:

```
./test.zig:8:26: error: expected type '*const u3', found '*:3:6 const u3'
    return bar(&bit_field.b);
                         ^
```

There are bound to be some edge cases and bugs as I polish this feature, but I am pleased that it turned out to integrate so cleanly into Zig's minimal design.

Thanks for reading my blog post.
