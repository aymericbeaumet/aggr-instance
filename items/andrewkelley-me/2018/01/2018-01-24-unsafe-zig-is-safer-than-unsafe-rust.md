---
title: Unsafe Zig is Safer Than Unsafe Rust
link: https://andrewkelley.me/post/unsafe-zig-safer-than-unsafe-rust.html
source: andrewkelley-me
published: 2018-01-24T20:17:36Z
updated: 2018-01-24T20:17:36Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'Unsafe Zig is Safer than Unsafe Rust Consider the following Rust code: struct Foo { a: i32, b: i32, } fn main() { unsafe { let mut array: [u8; 1024] = [1; 1024]; let foo = std::mem::transmute::<&mut u8, &mut Foo>(&mut array[0]); foo.a += 1; } } This pattern is pretty common if you are interacting with Operating System APIs. Another example. Can you spot the problem with the code? It''s pretty subtle, but there is actually undefined behavior going on here. Let''s take a look at the LLVM IR: define internal void @_ZN4test4main17h916a53db53ad90a1E() unnamed_addr #0 { start: %transmute_temp = alloca %Foo* %array = alloca [1024 x i8] %0 = getelementptr inbounds [1024 x i8], [1024 x i8]* %array, i32 0, i32 0 call void @llvm.memset.p0i8.i64(i8* %0, i8 1, i64 1024, i32 1, i1 false) br label %bb1 bb1: ; preds = %start %1 = getelementptr inbounds [1024 x i8], [1024 x i8]* %array, i64 0, i64 0 %2 = bitcast %Foo** %transmute_temp to i8** store i8* %1, i8** %2, align 8 %3 = load %Foo*, %Foo** %transmute_temp, !nonnull !1 br label %bb2 bb2: ; preds = %bb1 %4 = getelementptr inbounds %Foo, %Foo* %3, i32 0, i32 0 %5 = load i32, i32* %4 %6 = call { i32, i1 } @llvm.sadd.with.overflow.i32(i32 %5, i32 1) %7 = extractvalue { i32, i1 } %6, 0 %8 = extractvalue { i32, i1 } %6, 1 %9 = call i1 @llvm.expect.i1(i1 %8, i1 false) br i1 %9, label %panic, label %bb3 bb3: ; preds = %bb2 %10 = getelementptr inbounds %Foo, %Foo* %3, i32 0, i32 0 store i32 %7, i32* %10 ret void panic: ; preds = %bb2 ; call core::panicking::panic call void @_ZN4core9panicking5panic17hfecc01813e436969E({ %str_slice, [0 x i8], %str_slice, [0 x i8], i32, [0 x i8], i32, [0 x i8] }* noalias readonly dereferenceable(40) bitcast ({ %str_slice, %str_slice, i32, i32 }* @panic_loc.2 to { %str_slice, [0 x i8], %str_slice, [0 x i8], i32, [0 x i8], i32, [0 x i8] }*)) unreachable } That''s the code for the main function. This is using rustc version 1.21.0. Let''s zoom in on the problematic parts: %array = alloca [1024 x i8] ; loading foo.a in order to do + 1 %5 = load i32, i32* %4 ; storing the result of + 1 into foo.a store i32 %7, i32* %10 None of these alloca, load, or store instructions have alignment attributes on them, so they use the ABI alignment of the respective types. That means the i8 array gets alignment of 1, since the ABI alignment of i8 is 1, and the load and store instructions get alignment 4, since the ABI alignment of i32 is 4. This is undefined behavior: the store has undefined behavior if the alignment is not set to a value which is at least the size in bytes of the pointee the load has undefined behavior if the alignment is not set to a value which is at least the size in bytes of the pointee It''s a nasty bug, because besides being an easy mistake to make, on some architectures it will only cause mysterious slowness, while on others it can cause an illegal instruction exception on the CPU. Regardless, it''s undefined behavior, and we are professionals, and so we do not accept undefined behavior. Let''s try writing the equivalent code in Zig: const Foo = struct { a: i32, b: i32, }; pub fn main() { var array = []u8{1} ** 1024; const foo = @ptrCast(&Foo, &array[0]); foo.a += 1; } And now we compile it: /home/andy/tmp/test.zig:8:17: error: cast increases pointer alignment const foo = @ptrCast(&Foo, &array[0]); ^ /home/andy/tmp/test.zig:8:38: note: ''&u8'' has alignment 1 const foo = @ptrCast(&Foo, &array[0]); ^ /home/andy/tmp/test.zig:8:27: note: ''&Foo'' has alignment 4 const foo = @ptrCast(&Foo, &array[0]); ^ Zig knows not to compile this code. Here''s how to fix it: @@ -4,7 +4,7 @@ }; pub fn main() { - var array = []u8{1} ** 1024; + var array align(@alignOf(Foo)) = []u8{1} ** 1024; const foo = @ptrCast(&Foo, &array[0]); foo.a += 1; } Now it compiles fine. Let''s have a look at the LLVM IR: define internal fastcc void @main() unnamed_addr #0 !dbg !8911 { Entry: %array = alloca [1024 x i8], align 4 %foo = alloca %Foo*, align 8 %0 = bitcast [1024 x i8]* %array to i8*, !dbg !8923 call void @llvm.memcpy.p0i8.p0i8.i64(i8* %0, i8* getelementptr inbounds ([1024 x i8], [1024 x i8]* @266, i32 0, i32 0), i64 1024, i32 4, i1 false), !dbg !8923 call void @llvm.dbg.declare(metadata [1024 x i8]* %array, metadata !8914, metadata !529), !dbg !8923 %1 = getelementptr inbounds [1024 x i8], [1024 x i8]* %array, i64 0, i64 0, !dbg !8924 %2 = bitcast i8* %1 to %Foo*, !dbg !8925 store %Foo* %2, %Foo** %foo, align 8, !dbg !8926 call void @llvm.dbg.declare(metadata %Foo** %foo, metadata !8916, metadata !529), !dbg !8926 %3 = load %Foo*, %Foo** %foo, align 8, !dbg !8927 %4 = getelementptr inbounds %Foo, %Foo* %3, i32 0, i32 0, !dbg !8927 %5 = load i32, i32* %4, align 4, !dbg !8927 %6 = call { i32, i1 } @llvm.sadd.with.overflow.i32(i32 %5, i32 1), !dbg !8929 %7 = extractvalue { i32, i1 } %6, 0, !dbg !8929 %8 = extractvalue { i32, i1 } %6, 1, !dbg !8929 br i1 %8, label %OverflowFail, label %OverflowOk, !dbg !8929 OverflowFail: ; preds = %Entry tail call fastcc void @panic(%"[]u8"* @88, %StackTrace* null), !dbg !8929 unreachable, !dbg !8929 OverflowOk: ; preds = %Entry store i32 %7, i32* %4, align 4, !dbg !8929 ret void, !dbg !8930 } Zooming in on the relevant parts: %array = alloca [1024 x i8], align 4 %5 = load i32, i32* %4, align 4, !dbg !8927 store i32 %7, i32* %4, align 4, !dbg !8929 Notice that the alloca, load, and store all agree on the alignment. In Zig the problem of alignment is solved completely; the compiler catches all possible alignment issues. In the situation where you need to assert to the compiler that something is more aligned than Zig thinks it is, you can use @alignCast. This inserts a cheap safety check in debug mode to make sure the alignment assertion is correct.'
content: extracted
html: 2018-01-24-unsafe-zig-is-safer-than-unsafe-rust.html
---

Consider the following Rust code:

```
struct Foo {
    a: i32,
    b: i32,
}

fn main() {
    unsafe {
        let mut array: [u8; 1024] = [1; 1024];
        let foo = std::mem::transmute::<&mut u8, &mut Foo>(&mut array[0]);
        foo.a += 1;
    }
}
```

This pattern is pretty common if you are [interacting with Operating System APIs](https://gist.github.com/andrewrk/182ace5dee6c4025d8c4b0ca22ca98ca). [Another example](https://github.com/andrewrk/libsoundio/blob/fc96baf8130b52ba6fe928e5f629afd55ecc7321/src/alsa.c#L802).

Can you spot the problem with the code?

It's pretty subtle, but there is actually undefined behavior going on here. Let's take a look at the LLVM IR:

```
define internal void @_ZN4test4main17h916a53db53ad90a1E() unnamed_addr #0 {
start:
  %transmute_temp = alloca %Foo*
  %array = alloca [1024 x i8]
  %0 = getelementptr inbounds [1024 x i8], [1024 x i8]* %array, i32 0, i32 0
  call void @llvm.memset.p0i8.i64(i8* %0, i8 1, i64 1024, i32 1, i1 false)
  br label %bb1

bb1:                                              ; preds = %start
  %1 = getelementptr inbounds [1024 x i8], [1024 x i8]* %array, i64 0, i64 0
  %2 = bitcast %Foo** %transmute_temp to i8**
  store i8* %1, i8** %2, align 8
  %3 = load %Foo*, %Foo** %transmute_temp, !nonnull !1
  br label %bb2

bb2:                                              ; preds = %bb1
  %4 = getelementptr inbounds %Foo, %Foo* %3, i32 0, i32 0
  %5 = load i32, i32* %4
  %6 = call { i32, i1 } @llvm.sadd.with.overflow.i32(i32 %5, i32 1)
  %7 = extractvalue { i32, i1 } %6, 0
  %8 = extractvalue { i32, i1 } %6, 1
  %9 = call i1 @llvm.expect.i1(i1 %8, i1 false)
  br i1 %9, label %panic, label %bb3

bb3:                                              ; preds = %bb2
  %10 = getelementptr inbounds %Foo, %Foo* %3, i32 0, i32 0
  store i32 %7, i32* %10
  ret void

panic:                                            ; preds = %bb2
; call core::panicking::panic
  call void @_ZN4core9panicking5panic17hfecc01813e436969E({ %str_slice, [0 x i8], %str_slice, [0 x i8], i32, [0 x i8], i32, [0 x i8] }* noalias readonly dereferenceable(40) bitcast ({ %str_slice, %str_slice, i32, i32 }* @panic_loc.2 to { %str_slice, [0 x i8], %str_slice, [0 x i8], i32, [0 x i8], i32, [0 x i8] }*))
  unreachable
}
```

That's the code for the main function. This is using rustc version 1.21.0. Let's zoom in on the problematic parts:

```
  %array = alloca [1024 x i8]
  ; loading foo.a in order to do + 1
  %5 = load i32, i32* %4
  ; storing the result of + 1 into foo.a
  store i32 %7, i32* %10
```

None of these [alloca](http://llvm.org/docs/LangRef.html#alloca-instruction), [load](http://llvm.org/docs/LangRef.html#load-instruction), or [store](http://llvm.org/docs/LangRef.html#store-instruction) instructions have alignment attributes on them, so they use the ABI alignment of the respective types.

That means the i8 array gets alignment of 1, since the ABI alignment of i8 is 1, and the load and store instructions get alignment 4, since the ABI alignment of i32 is 4. This is undefined behavior:

> the store has undefined behavior if the alignment is not set to a value which is at least the size in bytes of the pointee

> the load has undefined behavior if the alignment is not set to a value which is at least the size in bytes of the pointee

It's a nasty bug, because besides being an easy mistake to make, on some architectures it will only cause mysterious slowness, while on others it can cause an illegal instruction exception on the CPU. Regardless, it's undefined behavior, and we are professionals, and so we do not accept undefined behavior.

Let's try writing the equivalent code in [Zig](http://ziglang.org/):

```
const Foo = struct {
    a: i32,
    b: i32,
};

pub fn main() {
    var array = []u8{1} ** 1024;
    const foo = @ptrCast(&Foo, &array[0]);
    foo.a += 1;
}
```

And now we compile it:

```
/home/andy/tmp/test.zig:8:17: error: cast increases pointer alignment
    const foo = @ptrCast(&Foo, &array[0]);
                ^
/home/andy/tmp/test.zig:8:38: note: '&u8' has alignment 1
    const foo = @ptrCast(&Foo, &array[0]);
                                     ^
/home/andy/tmp/test.zig:8:27: note: '&Foo' has alignment 4
    const foo = @ptrCast(&Foo, &array[0]);
                          ^
```

Zig knows not to compile this code. Here's how to fix it:

```
@@ -4,7 +4,7 @@
 };
 
 pub fn main() {
-    var array = []u8{1} ** 1024;
+    var array align(@alignOf(Foo)) = []u8{1} ** 1024;
     const foo = @ptrCast(&Foo, &array[0]);
     foo.a += 1;
 }
```

Now it compiles fine. Let's have a look at the LLVM IR:

```
define internal fastcc void @main() unnamed_addr #0 !dbg !8911 {
Entry:
  %array = alloca [1024 x i8], align 4
  %foo = alloca %Foo*, align 8
  %0 = bitcast [1024 x i8]* %array to i8*, !dbg !8923
  call void @llvm.memcpy.p0i8.p0i8.i64(i8* %0, i8* getelementptr inbounds ([1024 x i8], [1024 x i8]* @266, i32 0, i32 0), i64 1024, i32 4, i1 false), !dbg !8923
  call void @llvm.dbg.declare(metadata [1024 x i8]* %array, metadata !8914, metadata !529), !dbg !8923
  %1 = getelementptr inbounds [1024 x i8], [1024 x i8]* %array, i64 0, i64 0, !dbg !8924
  %2 = bitcast i8* %1 to %Foo*, !dbg !8925
  store %Foo* %2, %Foo** %foo, align 8, !dbg !8926
  call void @llvm.dbg.declare(metadata %Foo** %foo, metadata !8916, metadata !529), !dbg !8926
  %3 = load %Foo*, %Foo** %foo, align 8, !dbg !8927
  %4 = getelementptr inbounds %Foo, %Foo* %3, i32 0, i32 0, !dbg !8927
  %5 = load i32, i32* %4, align 4, !dbg !8927
  %6 = call { i32, i1 } @llvm.sadd.with.overflow.i32(i32 %5, i32 1), !dbg !8929
  %7 = extractvalue { i32, i1 } %6, 0, !dbg !8929
  %8 = extractvalue { i32, i1 } %6, 1, !dbg !8929
  br i1 %8, label %OverflowFail, label %OverflowOk, !dbg !8929

OverflowFail:                                     ; preds = %Entry
  tail call fastcc void @panic(%"[]u8"* @88, %StackTrace* null), !dbg !8929
  unreachable, !dbg !8929

OverflowOk:                                       ; preds = %Entry
  store i32 %7, i32* %4, align 4, !dbg !8929
  ret void, !dbg !8930
}
```

Zooming in on the relevant parts:

```
  %array = alloca [1024 x i8], align 4
  %5 = load i32, i32* %4, align 4, !dbg !8927
  store i32 %7, i32* %4, align 4, !dbg !8929
```

Notice that the alloca, load, and store all agree on the alignment.

In Zig the problem of alignment is solved completely; the compiler catches all possible alignment issues. In the situation where you need to assert to the compiler that something is more aligned than Zig thinks it is, you can use [@alignCast](http://ziglang.org/documentation/master/#alignCast). This inserts a cheap safety check in debug mode to make sure the alignment assertion is correct.

Thanks for reading my blog post.
