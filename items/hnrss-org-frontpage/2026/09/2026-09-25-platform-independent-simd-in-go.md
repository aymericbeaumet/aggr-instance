---
title: Platform-Independent SIMD in Go
link: https://go.dev/blog/simd-experiment
source: hnrss-org-frontpage
published: 2026-09-25T11:47:06Z
updated: 2026-09-25T11:47:06Z
first_seen: 2026-09-25T18:30:37.122772579Z
authors:
- yurivish
summary: 'Article URL: https://go.dev/blog/simd-experiment Comments URL: https://news.ycombinator.com/item?id=49843269 Points: 264 # Comments: 95'
content: extracted
html: 2026-09-25-platform-independent-simd-in-go.html
preview:
  file: 2026-09-25-platform-independent-simd-in-go.preview-f0dc582029e6.webp
  width: 256
  height: 149
  color: '#a7aa9a'
images:
- source: https://go.dev/doc/gopher/gopher5logo.jpg
  original:
    file: 2026-09-25-platform-independent-simd-in-go.image-b8c8eb669827.jpg
    width: 1262
    height: 733
  color: '#bac5b7'
---

Go 1.26 and 1.27 include experimental APIs for Single Instruction Multiple Data (SIMD) operations. SIMD is a native feature of many modern CPUs that allows software to perform uniform operations across vectors of data very quickly, such as adding 8 pairs of `float64` values in a single instruction. It can significantly speed up many computationally-intensive tasks, ranging from cryptography to data processing to AI. In fact, Go’s [Green Tea garbage collector](https://go.dev/blog/greenteagc) even makes use of SIMD to accelerate scanning memory for live objects.

Prior to these new experimental APIs, the only way to access this functionality from Go was by writing Go assembly. This was only worth it for truly performance-critical compute kernels, which meant plenty of software that could benefit from SIMD simply left a lot of the CPU unused.

Go 1.26 introduced a SIMD API for amd64, and Go 1.27 added APIs for arm64 (specifically NEON) and wasm. However, a basic challenge for a SIMD API is the enormous variation between platforms, not simply in what operations they support, but even in how vectors are represented. Some platforms provide fixed-size vectors, typically between 128 bits and 512 bits, while on others the vector size isn’t known at build time and must be queried when the program starts. To provide full access to the breadth of these platforms, these APIs live in an architecture-dependent `archsimd` package.

But Go 1.27 goes beyond these architecture-dependent APIs and introduces an experimental, fully portable, platform- and size-agnostic SIMD interface, loosely based on [Highway](https://google.github.io/highway/en/master/index.html) for C++. The goal is to support write-once near-asm-performance “simd” code on platforms with SIMD support, and to provide a competent emulation on those platforms that do not (yet) have SIMD support. The `simd` package currently supports AVX, AVX2, and AVX512 on amd64, NEON on arm64, and wasm’s SIMD instructions.

## Motivation: variation among SIMD architectures

SIMD architectures vary in several dimensions. Some provide a single fixed vector size (wasm, PowerPC, and s390x, 128 bits). Some provide several fixed vector sizes (amd64, with 128, 256, and 512; loong64 with 128 and 256). Riscv64 supports vectors of unspecified size between 128 and 65536 bits, though the length is limited to powers of 2. Arm64 supports one fixed size (128 bits, NEON), and one variable size (128-2048 bits, powers of two only, SVE). On a given instance of a particular architecture, determining what sizes that particular instance happens to support requires feature checks: amd64, but is it AVX, AVX2, or AVX512? Arm64, but is it NEON or SVE? If SVE, how large? Which variant of SVE: SVE, SVE2, or SVE2.1?

Different SIMD architectures vary in how they handle vector masking. For vectors, if-then-else across a vector can be implemented with masks; do the operation, but only assign the result (or load, or store) where the mask is “true”. Some SIMD variants do not provide masks; all operations work across all elements, and “masking” is done with vector bitmasks and vector boolean operations (wasm, AVX, AVX2, NEON). Some provide special mask registers, with one bit governing operations on one vector element (AVX512 and RVV). Others (SVE) allocate one bit per vector byte, but the least-significant bit of each element’s mask bits governs masked operations. AVX2 also supports masked loads and stores, but using a plain vector as the mask, and with the most-significant bit governing the operation.

A third source of variation is in the operations themselves. Each architecture provides its own primitives for rearranging vector elements; some require constant inputs, others support variable inputs. Different SIMD architectures support different crypto-related operations. Even basic arithmetic can have varying support; for example wasm lacks comparisons for vectors of 64-bit integers. Even for a given vector length on a particular architecture, instruction support depends on “features” that must be checked.

Even though Go’s architecture-dependent `archsimd` package was designed to be as uniform as possible across architectures, many of these quirks remain, and make designing, writing, and testing code for multiplatform SIMD onerous. We could do more in the `archsimd` package to make the different architectures appear more similar, but we can only go so far without compromising efficiency.

## Overview

The new `simd` package hides these differences by removing fixed-size vectors from the type system, and by only supporting those operations that are in the intersection of all the different platforms, and fills gaps in the intersection with efficient emulation in terms of other SIMD instructions. The goal is a set of operations that is

1. adequate to support many data processing algorithms that benefit from a vectorized implementation (but are not tied to a particular vector size),
2. is as efficient as assembly language when the source code operations match the underlying hardware,
3. is otherwise emulated as well as possible,
4. and is easy to read and understand (even/especially if an LLM ends up writing the code).

On platforms that lack SIMD instructions or that lack support in `archsimd`, all of the operations are emulated, so that code written using the `simd` package will always run.

To use this experimental package, set `GOEXPERIMENT=simd` at build time, just like using the experimental `archsimd` package.

The `simd` vector types are just capitalized, plural, primitive types, for example `simd.Uint8s` or `simd.Float32s`. Vectors are loaded from and stored to slices, for example:

```
// innerProduct returns the inner product of x and y.
func innerProduct(x, y []float32) float32 {
    var a simd.Float32s
    var i int
    for i = 0; i < len(x)-a.Len()+1; i += a.Len() {
        u := simd.LoadFloat32s(x[i : i+a.Len()])
        v := simd.LoadFloat32s(y[i : i+a.Len()])
        a = u.MulAdd(v, a)
    }
    if i < len(x) {
        u, _ := simd.LoadFloat32sPart(x[i:])
        v, _ := simd.LoadFloat32sPart(y[i:])
        a = u.MulAdd(v, a)
    }
    return sum(a)
}
// sum returns scalar sum of elements of x.
func sum(x simd.Float32s) float32 {
    s := make([]float32, x.Len())
    x.Store(s)
    var r float32
    for _, e := range s {
        r += e
    }
    return r
}
```

This example also shows one of the limitations of the first experimental release of this package; because there’s no common way to sum across all the elements of a vector, it’s not supported by `simd` in Go 1.27, though `ReduceSum` will appear in the next release so `sum` can be replaced with just `simd.ReduceSum`.

SIMD comparisons produce mask values, which are specific to the corresponding vector element width, so that comparisons of `Int8s` produce `Mask8s`, etc., and mask values can be used to select and filter vectors.

## Supported `simd` package operations as of Go 1.27

In this table, `V` and `U` are vector types, `M` is a mask type, `E` is a scalar type, and `W` is a width.

### Package-Level Load / Broadcast Functions

| Function                  | `Int8s` | `Int16s` | `Int32s` | `Int64s` | `Uint8s` | `Uint16s` | `Uint32s` | `Uint64s` | `Float32s` | `Float64s` |
| ------------------------- | ------- | -------- | -------- | -------- | -------- | --------- | --------- | --------- | ---------- | ---------- |
| `LoadV([]E) V`            | Y       | Y        | Y        | Y        | Y        | Y         | Y         | Y         | Y          | Y          |
| `LoadVPart([]E) (V, int)` | Y       | Y        | Y        | Y        | Y        | Y         | Y         | Y         | Y          | Y          |
| `BroadcastV(E) V`         | Y       | Y        | Y        | Y        | Y        | Y         | Y         | Y         | Y          | Y          |

### Store/String operations

| `(x V).Method(...)`    | `Int8s` | `Int16s` | `Int32s` | `Int64s` | `Uint8s` | `Uint16s` | `Uint32s` | `Uint64s` | `Float32s` | `Float64s` |
| ---------------------- | ------- | -------- | -------- | -------- | -------- | --------- | --------- | --------- | ---------- | ---------- |
| `Store(s []E)`         | Y       | Y        | Y        | Y        | Y        | Y         | Y         | Y         | Y          | Y          |
| `StorePart(s []E) int` | Y       | Y        | Y        | Y        | Y        | Y         | Y         | Y         | Y          | Y          |
| `String() string`      | Y       | Y        | Y        | Y        | Y        | Y         | Y         | Y         | Y          | Y          |

### Arithmetic operations

| `(x V).Method(...) V`        | `Int8s` | `Int16s` | `Int32s` | `Int64s` | `Uint8s` | `Uint16s` | `Uint32s` | `Uint64s` | `Float32s` | `Float64s` |
| ---------------------------- | ------- | -------- | -------- | -------- | -------- | --------- | --------- | --------- | ---------- | ---------- |
| `Abs() V`                    | Y       | Y        | Y        |          |          |           |           |           | Y          | Y          |
| `Add(y V) V`                 | Y       | Y        | Y        | Y        | Y        | Y         | Y         | Y         | Y          | Y          |
| `AddSaturated(y V) V`        | Y       | Y        |          |          | Y        | Y         |           |           |            |            |
| `Average(y V) V`             |         |          |          |          | Y        | Y         |           |           |            |            |
| `Div(y V) V`                 |         |          |          |          |          |           |           |           | Y          | Y          |
| `IfElse(mask MaskWs, y V) V` | Y       | Y        | Y        | Y        | Y        | Y         | Y         | Y         | Y          | Y          |
| `Len() int`                  | Y       | Y        | Y        | Y        | Y        | Y         | Y         | Y         | Y          | Y          |
| `Masked(mask MaskWs) V`      | Y       | Y        | Y        | Y        | Y        | Y         | Y         | Y         | Y          | Y          |
| `Max(y V) V`                 | Y       | Y        | Y        |          | Y        | Y         | Y         |           | Y          | Y          |
| `Min(y V) V`                 | Y       | Y        | Y        |          | Y        | Y         | Y         |           | Y          | Y          |
| `Mul(y V) V`                 | Y       | Y        | Y        |          | Y        | Y         | Y         |           | Y          | Y          |
| `MulAdd(y V, z V) V`         |         |          |          |          |          |           |           |           | Y          | Y          |
| `Neg() V`                    | Y       | Y        | Y        | Y        |          |           |           |           | Y          | Y          |
| `Not() V`                    | Y       | Y        | Y        | Y        | Y        | Y         | Y         | Y         |            |            |
| `Or(y V) V`                  | Y       | Y        | Y        | Y        | Y        | Y         | Y         | Y         |            |            |
| `Sqrt() V`                   |         |          |          |          |          |           |           |           | Y          | Y          |
| `Sub(y V) V`                 | Y       | Y        | Y        | Y        | Y        | Y         | Y         | Y         | Y          | Y          |
| `SubSaturated(y V) V`        | Y       | Y        |          |          | Y        | Y         |           |           |            |            |
| `Xor(y V) V`                 | Y       | Y        | Y        | Y        | Y        | Y         | Y         | Y         |            |            |

### Boolean and vector masking operations

| `(x V).Method(...) V`          | `Int8s` | `Int16s` | `Int32s` | `Int64s` | `Uint8s` | `Uint16s` | `Uint32s` | `Uint64s` | `Float32s` | `Float64s` |
| ------------------------------ | ------- | -------- | -------- | -------- | -------- | --------- | --------- | --------- | ---------- | ---------- |
| `And(y V) V`                   | Y       | Y        | Y        | Y        | Y        | Y         | Y         | Y         |            |            |
| `AndNot(y V) V`                | Y       | Y        | Y        | Y        | Y        | Y         | Y         | Y         |            |            |
| `CarrylessMultiplyEven(y V) V` |         |          |          |          |          |           |           | Y         |            |            |
| `CarrylessMultiplyOdd(y V) V`  |         |          |          |          |          |           |           | Y         |            |            |
| `IfElse(mask MaskWs, y V) V`   | Y       | Y        | Y        | Y        | Y        | Y         | Y         | Y         | Y          | Y          |
| `Masked(mask MaskWs) V`        | Y       | Y        | Y        | Y        | Y        | Y         | Y         | Y         | Y          | Y          |
| `Not() V`                      | Y       | Y        | Y        | Y        | Y        | Y         | Y         | Y         |            |            |
| `Or(y V) V`                    | Y       | Y        | Y        | Y        | Y        | Y         | Y         | Y         |            |            |
| `Xor(y V) V`                   | Y       | Y        | Y        | Y        | Y        | Y         | Y         | Y         |            |            |

### Comparison operations

| `(x V).Method(...) M`      | `Int8s` | `Int16s` | `Int32s` | `Int64s` | `Uint8s` | `Uint16s` | `Uint32s` | `Uint64s` | `Float32s` | `Float64s` |
| -------------------------- | ------- | -------- | -------- | -------- | -------- | --------- | --------- | --------- | ---------- | ---------- |
| `Equal(y V) MaskWs`        | Y       | Y        | Y        | Y        | Y        | Y         | Y         | Y         | Y          | Y          |
| `Greater(y V) MaskWs`      | Y       | Y        | Y        | Y        |          | Y         | Y         | Y         | Y          | Y          |
| `GreaterEqual(y V) MaskWs` | Y       | Y        | Y        | Y        |          | Y         | Y         | Y         | Y          | Y          |
| `Less(y V) MaskWs`         | Y       | Y        | Y        | Y        |          | Y         | Y         | Y         | Y          | Y          |
| `LessEqual(y V) MaskWs`    | Y       | Y        | Y        | Y        |          | Y         | Y         | Y         | Y          | Y          |
| `NotEqual(y V) MaskWs`     | Y       | Y        | Y        | Y        | Y        | Y         | Y         | Y         | Y          | Y          |

### Conversion operations

| `(x V).Method(...) U`       | `Int8s` | `Int16s` | `Int32s` | `Int64s` | `Uint8s` | `Uint16s` | `Uint32s` | `Uint64s` | `Float32s` | `Float64s` |
| --------------------------- | ------- | -------- | -------- | -------- | -------- | --------- | --------- | --------- | ---------- | ---------- |
| `ConvertToFloatW() FloatWs` |         |          | Y        |          |          |           |           |           |            |            |
| `ConvertToIntW() IntWs`     |         |          |          |          | Y        | Y         | Y         | Y         | Y          |            |
| `ConvertToUintW() UintWs`   | Y       | Y        | Y        | Y        |          |           |           |           |            |            |
| `ToMask() (to MaskWs)`      | Y       | Y        | Y        | Y        |          |           |           |           |            |            |

### Mask Methods

| `(m M).Method(...) M)` | `Mask8s` | `Mask16s` | `Mask32s` | `Mask64s` |
| ---------------------- | -------- | --------- | --------- | --------- |
| `And(y M) M`           | Y        | Y         | Y         | Y         |
| `Or(y V) V`            | Y        | Y         | Y         | Y         |
| `String() string`      | Y        | Y         | Y         | Y         |
| `ToIntWs() (to IntWs)` | Y        | Y         | Y         | Y         |

### Shift and rotate operations

| `(x V).Method() V`              | `Int8s` | `Int16s` | `Int32s` | `Int64s` | `Uint8s` | `Uint16s` | `Uint32s` | `Uint64s` | `Float32s` | `Float64s` |
| ------------------------------- | ------- | -------- | -------- | -------- | -------- | --------- | --------- | --------- | ---------- | ---------- |
| `RotateAllLeft(dist uint64) V`  |         | Y        | Y        | Y        |          | Y         | Y         | Y         |            |            |
| `RotateAllRight(dist uint64) V` |         | Y        | Y        | Y        |          | Y         | Y         | Y         |            |            |
| `ShiftAllLeft(dist uint64) V`   |         | Y        | Y        | Y        |          | Y         | Y         | Y         |            |            |
| `ShiftAllRight(dist uint64) V`  |         | Y        | Y        |          |          | Y         | Y         | Y         |            |            |

### Zero-cost reshaping operations

| `(x V).Method(...) U`        | `Int8s` | `Int16s` | `Int32s` | `Int64s` | `Uint8s` | `Uint16s` | `Uint32s` | `Uint64s` | `Float32s` | `Float64s` |
| ---------------------------- | ------- | -------- | -------- | -------- | -------- | --------- | --------- | --------- | ---------- | ---------- |
| `ToBits() UintWs`            | Y       | Y        | Y        | Y        |          |           |           |           | Y          | Y          |
| `ReshapeToUint8s() Uint8s`   |         |          |          |          |          | Y         | Y         | Y         |            |            |
| `ReshapeToUint16s() Uint16s` |         |          |          |          | Y        |           | Y         | Y         |            |            |
| `ReshapeToUint32s() Uint32s` |         |          |          |          | Y        | Y         |           | Y         |            |            |
| `ReshapeToUint64s() Uint64s` |         |          |          |          | Y        | Y         | Y         |           |            |            |
| `BitsToFloatW() FloatWs`     |         |          |          |          |          |           | Y         | Y         |            |            |
| `BitsToIntW() IntWs`         |         |          |          |          | Y        | Y         | Y         | Y         |            |            |

## Transition to/from platform-specific code

It may happen that the `simd` package is too limited for all parts of a particular application, or that we have not yet provided an adequate emulation for some necessary feature. For that case, the `simd` package supports transition to and from architecture-specific SIMD. Each vector type in the `simd` package has a conversion method `ToArch()` returning an `any`. That `any` can be type-asserted to one of the architecture-specific types for a platform. To convert back, use one of the `simd.<SimdType>FromArch` functions. For portable code this creates an obligation to write architecture-specific code for each of the platforms, including an emulation.

Here’s a complete example for a method/function that is currently missing, but should be added in Go 1.28. Suppose your algorithm needs `Int8s.OnesCount()` (which `simd` in Go 1.27 lacks). Rather than rewriting the entire algorithm for each platform, it’s possible to just implement the missing operation.

First, for amd64, which lacks the instruction for AVX and AVX2, but not AVX512:

```
//go:build goexperiment.simd && amd64
package simd_test
import (
    "simd"
    "simd/archsimd"
)
var popcnt4x16 = [16]int8{0, 1, 1, 2, 1, 2, 2, 3, 1, 2, 2, 3, 2, 3, 3, 4}
var popcnt4x32 = [32]int8{
    0, 1, 1, 2, 1, 2, 2, 3, 1, 2, 2, 3, 2, 3, 3, 4,
    0, 1, 1, 2, 1, 2, 2, 3, 1, 2, 2, 3, 2, 3, 3, 4,
}
// OnesCount returns the number of one bits for each element.
func OnesCount(v simd.Int8s) simd.Int8s {
    switch x := v.ToArch().(type) {
    case archsimd.Int8x16:
        lut := archsimd.LoadInt8x16Array(&popcnt4x16)
        mask0f := archsimd.BroadcastInt8x16(0x0f)
        lo := x.And(mask0f)
        hi := x.ToBits().ReshapeToUint16s().ShiftAllRight(4).
                ReshapeToUint8s().BitsToInt8().And(mask0f)
        return simd.Int8sFromArch(lut.PermuteOrZero(lo).
                Add(lut.PermuteOrZero(hi)))
    case archsimd.Int8x32:
        lut := archsimd.LoadInt8x32Array(&popcnt4x32)
        mask0f := archsimd.BroadcastInt8x32(0x0f)
        lo := x.And(mask0f)
        hi := x.ToBits().ReshapeToUint16s().ShiftAllRight(4).
                ReshapeToUint8s().BitsToInt8().And(mask0f)
        return simd.Int8sFromArch(lut.PermuteOrZeroGrouped(lo).
                Add(lut.PermuteOrZeroGrouped(hi)))
    case archsimd.Int8x64:
        return simd.Int8sFromArch(x.OnesCount())
    default:
        // GODEBUG=simd=0 emulation
        return OnesCountEmulated(v)
    }
}
```

The interface conversion and type switch look like they should be inefficient, but the compiler-side implementation of `simd` specializes code and optimizes away the type switch.

NEON and Wasm both support `Int8s.OnesCount()`, so their implementation is much simpler, though it still uses `Int8s.ToArch` and `Int8sFromArch`.

```
//go:build goexperiment.simd && (wasm || arm64)
package simd_test
import (
    "simd"
    "simd/archsimd"
)
// OnesCount returns the number of one bits for each element.
func OnesCount(v simd.Int8s) simd.Int8s {
    // TODO when SVE is added, this won't work
    switch x := v.ToArch().(type) {
    case archsimd.Int8x16:
        return simd.Int8sFromArch(x.OnesCount())
    default:
        // GODEBUG=simd=0 emulation
        return OnesCountEmulated(v)
    }
}
```

Don’t forget that some people don’t have hardware SIMD support:

```
//go:build goexperiment.simd && !(amd64 || wasm || arm64)

package simd_test
import (
    "simd"
)
// OnesCount returns the number of one bits for each element.
func OnesCount(v simd.Int8s) simd.Int8s {
    return OnesCountEmulated(v)
}
```

And to complete the exercise, a separate emulation function shared as a fallback across all implementations:

```
//go:build goexperiment.simd
package simd_test
import (
    "simd"
)
// OnesCountEmulated returns the number of one bits for each element.
func OnesCountEmulated(v simd.Int8s) simd.Int8s {
    a := [2]uint64{}
    v.ToBits().ReshapeToUint64s().Store(a[:])
    a0, a1 := a[0], a[1]
    m1 := uint64(0x5555555555555555)
    m2 := uint64(0x3333333333333333)
    m4 := uint64(0x0f0f0f0f0f0f0f0f)
    a0 = (a0 & m1) + ((a0 >> 1) & m1)
    a1 = (a1 & m1) + ((a1 >> 1) & m1)
    a0 = (a0 & m2) + ((a0 >> 2) & m2)
    a1 = (a1 & m2) + ((a1 >> 2) & m2)
    a0 = (a0 & m4) + ((a0 >> 4) & m4)
    a1 = (a1 & m4) + ((a1 >> 4) & m4)
    a[0], a[1] = a0, a1
    return simd.LoadUint64s(a[:]).ReshapeToUint8s().BitsToInt8()
}
```

## API intersection and method emulation

Whatever operations the `simd` package offers need to run acceptably well on most architectures. As a first step, any operation that is supported everywhere, can easily be supported on `simd`. This tends to include loads, stores, arithmetic, and comparisons (but not all comparisons!).

A naive intersection across SIMD methods from different architectures still leaves plenty of holes. These are filled by adding emulations to the various architecture-specific `archsimd` APIs. These APIs already contain many trivial emulations to simplify life for Go programmers; signed and unsigned integer addition use the same instruction, but in the same way that Go supports the `+` operator for both `int` and `uint`, the `archsimd` package provides both `Int8x16.Add(Int8x16)` and `Uint8x16.Add(Uint8x16)`, even though those compile to the same instruction. Modern programming languages also don’t expect programmers to know how to implement floating point negation and absolute value with bit fiddling, so `archsimd` implements that where necessary, or “emulates” if you look at it just so.

There are many emulations that require just 2 or 3 instructions; for example, some architectures support only a same-value shift distance across vector elements, while others support a different shift distance for each vector element. To support scalar shifting in `simd`, we emulate scalar shift with vector shift. Some architectures lack some unsigned comparisons–these are just signed comparison, plus two XORs with a constant.

Not all missing instructions are that simple. The “carryless multiply” instruction is important to cryptography and CRC checksumming, but it isn’t always supported. Leaving that out of the `simd` API would prevent its use for some important algorithms. Therefore, we provide an emulation, and because one important use is in crypto, its run time does not vary depending on its inputs.

In other cases, rather than implement a primitive instruction like “add pairs” (also called “horizontal addition”), for the `simd` package in the next release we will provide the higher level operation that add pairs is usually used for, which is sum reduction. This also helps insulate users from vector-length dependence; even given the hardware instruction for adding pairs, the number of reduction steps depends on the vector length.

The constraint of supporting all platforms, including ones that we predict will appear in `archsimd` within the next year or so, forces a somewhat conservative approach to which methods we add to `simd`. Riscv64, ppc64, s390x, and loong64 all have their own SIMD extensions.

## GODEBUG settings

On platforms where there is some hardware support, behavior can be modified with `GODEBUG`, to make it easier to test `simd`-using code with various hardware configurations. You can set the `GODEBUG` environment variable prior to executing your program.

In Go 1.27, levels of SIMD support are roughly described by vector length:

- `GODEBUG=simd=0` means use emulation for SIMD operations even if the hardware support is available.
- `GODEBUG=simd=128` means use 128-bit vectors and their features. If the features aren’t available, panic immediately.
- `GODEBUG=simd=256` means use 256-bit vectors and their features, if possible.
- `GODEBUG=simd=512` means use 512-bit vectors and their features, if possible.
- `GODEBUG=simd=+128` means use 128-bit vectors and their features even if some features are not supported. If unsupported instructions are used, the code will panic, but if they are not it may still run. An example of this is Raspberry Pi, which supports NEON but lacks PMULL (carryless multiply).
- `GODEBUG=simd=+256` means use 256-bit vectors and their features even if some features are not supported. If unsupported instructions are used, the code will panic, but if they are not it may still run. An example of this is Apple Silicon’s amd64 emulation, which supports AVX2 but not VPCLMULQDQ (again, carryless multiply).
- `GODEBUG=simd=+512` means use 512-bit vectors, even if some features are not supported.

## Implementation details

If you are debugging code that uses `simd`, or even just look at a stack trace, you will notice some weird extra types and methods. The reason is that `simd` is both a package, an internal implementation package, and some AST rewriting in the front end of the compiler.

The AST rewrite creates multiple specialized copies of functions, variables, and types that mention `simd` types, where `simd` types are replaced with references to size-specialized types in `simd/internal/bridge`. Each of these bridge types is defined as an `archsimd` type, but with a restricted set of methods. The specialized functions, variables, and types acquire a suffix of the form `@simdNNN`, where `NNN` is either a vector length (128, 256, or 512) or 0, indicating emulation. Functions that mention `simd` internally, but not in their signature, are converted to wrappers that switch on the SIMD level detected at program start, and call the appropriate specialized version of that function. Specialized functions call other specialized functions directly without dispatch overhead (and perhaps with inlining). This rewrite strategy was chosen as a compromise between code duplication and SIMD performance; the overhead is hoisted as high as necessary to avoid dispatch within SIMD computations, but not higher. If SIMD dispatch appears “too low” in a computation, a gratuitous mention of a `simd` type will move it upwards, as in this example:

```
func BenchmarkVpsumdSIMD(b *testing.B) {
    // mention "simd" so the benchmark loop calls specialized vpsumd3 directly
    var _ simd.Uint64s
    var w, x, y, z uint64 = ... // magic constants omitted.
    var lo, hi uint64
    for b.Loop() {
        // vpsumd3 does simd stuff, but lacks a simd signature,
        // so that it can be compared with non-SIMD emulations.
        lo, hi = vpsumd3(w, x, y, z)
    }
    sinkLo, sinkHi = lo, hi
}
```

## What’s coming

We plan to publish a blog post describing `archsimd` in greater detail soon.

For Go 1.28, we intend to add SVE support to `archsimd`, and also hope to add that to `simd`. More importantly, we hope to add additional SIMD operations to those that the `simd` package already supports (e.g., OnesCount, mask operations, reduction operations, vector shuffling operations). Go 1.28 will also include a small number of “feature variants” to avoid downgrading all the way to full emulation for platforms that have a hardware vector implementation but just lack one or a few operations, such as Raspberry Pi.
