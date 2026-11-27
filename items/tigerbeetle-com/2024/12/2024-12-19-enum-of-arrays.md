---
title: Enum of Arrays
link: https://tigerbeetle.com/blog/2024-12-19-enum-of-arrays
source: tigerbeetle-com
published: 2024-12-19T00:00:00Z
updated: 2024-12-19T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- matklad
summary: 'A popular data-oriented design pattern is a Struct of Arrays. Is an Array of Enums also useful? Yes! But let’s do a refresher on struct of arrays (SoA) first:'
content: extracted
html: 2024-12-19-enum-of-arrays.html
remote_preview:
  url: https://tigerbeetle.com/blog/2024-12-19-enum-of-arrays/banner-e7600558.webp
---

A popular data-oriented design pattern is a Struct of Arrays. Is an Array of Enums also useful? Yes! But let’s do a refresher on struct of arrays (`SoA`) first:

If you have a `Thing`,

```
const Thing = struct {
   checksum: u128,
   number: u32,
   flag: u8,
};
```

and an array of `Thing`s,

```
// Array of Structs.
const AoS = []Thing;
```

it’s possible to wrap the data inside out and use

```
// Struct of Arrays.
const SoA = struct {
   checksum: []u128
   number: []u32,
   flag: []u8,
};
```

instead. The benefits are:

- Reduced memory usage due to amortized padding. As `flag` is a byte, it requires some padding to align with larger fields like `checksum`. In `AoS`, each individual struct has this padding. In `SoA`, there’s still padding for `flag`, but it’s just one instance of padding for all the `Thing`s.
- Better memory bandwidth utilization for batched code. If a loop needs to process all things, but the processing doesn’t require all fields (at least for the majority of objects), then an array-based representation reduces the amount of data that needs to be loaded.

You can use the same trick also for a more SIMD-friendly data layout. A bad way to use SIMD is to try to vectorize the processing of an individual item. A good way to use SIMD is to process several items at once.

For example, if a `Thing` in question is a 3D vector, it’s usually not a good idea to implement the dot product of two vectors by loading `xyz` of one vector into a SIMD register `a`, `xyz` of another vector into a SIMD register `b`, and then performing a SIMD multiplication of `a * b`. The reason is that your “SIMD width” is still only three numbers:

![](https://tigerbeetle.com/blog/2024-12-19-enum-of-arrays/wasteful-simd-afe446d4.webp)

Much better to simultaneously compute the dot products of multiple pairs of vectors. Then, you can load a SIMD vector with `xxxxxxx`, which is the `x` coordinate for the first vector of multiple pairs. And so, the full width of SIMD registers is used:

![](https://tigerbeetle.com/blog/2024-12-19-enum-of-arrays/efficient-simd-2bb5244d.webp)

This post is to raise awareness that the same trick works with enums:

```
const Thing = union(enum) {
   spam: Spam,
   eggs: Eggs
}

// With a level of abstraction peeled:
const Thing = struct {
   tag: u8,
   payload: union {
       spam: Spam,
       eggs: Eggs,
   }
};
```

Here’s an array of enums:

```
const AoE = []Thing;
```

And here’s the dual, an enum of arrays:

```
const EoA = struct {
   tag: u8, // Sic!
   payload: union {
       spam: []Spam,
       eggs: []Eggs,
   }
}
```

The *key* idea is that there’s only a single `tag` per an entire batch of things. If a batch size is 1024, you can have 1024 spams or 1024 eggs, but not a mix of the two.

The benefits of this representation:

- Tags occupy less bytes in total, because a single tag can be amortized across many objects at once.

- If individual enum variants are of different sizes, then no memory is wasted when storing the gap between a small variant and a large one.

- Finally, the purpose of the tag is for the CPU to branch on it. By having only one tag per batch of objects, the amount of branching the CPU has to do is dramatically reduced, which is a very good thing in itself—unpredictable branching slows down the CPU a lot. But what’s more, with branching out of the way, the processing is opened up for SIMD optimizations!

TigerBeetle is built around this idea of `AoE => EoA` optimizations. TigerBeetle can do two things: create new accounts, and then create transfers between accounts. This *could* have been represented as a heterogeneous enum of the two kinds of operation. Instead, TigerBeetle uses homogeneous batches. TigerBeetle does eight thousands operations at a time, and there’s one tag for an entire batch. So, it’s always 8k transfers followed by 8k accounts, not a mixture of the two.

This optimization is related to the concept of data plane and control plane separation. The `tag` is control plane, a small, infrequent piece of information which informs decisions. The data plane then is more voluminous, but processing it doesn’t require many individual decisions. Just. One. Branch!

That’s all for today. Support your local branch predictor, convert your AoE to EoA!
