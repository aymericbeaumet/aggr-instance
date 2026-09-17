---
title: Nvidia announces native GPU programming in Rust
link: https://developer.nvidia.com/blog/introducing-cuda-rust-two-tracks-for-writing-gpu-kernels/
source: hnrss-org-frontpage
published: 2026-09-16T11:15:53Z
updated: 2026-09-16T11:15:53Z
first_seen: 2026-09-17T01:40:53.278229139Z
authors:
- nonmaskable
summary: 'Article URL: https://developer.nvidia.com/blog/introducing-cuda-rust-two-tracks-for-writing-gpu-kernels/ Comments URL: https://news.ycombinator.com/item?id=49724881 Points: 250 # Comments: 92'
content: extracted
html: 2026-09-16-nvidia-announces-native-gpu-programming-in-rust.html
preview:
  file: 2026-09-16-nvidia-announces-native-gpu-programming-in-rust.preview-2ced73f88d35.webp
  width: 256
  height: 144
  color: '#0a1116'
images:
- source: https://developer-blogs.nvidia.com/wp-content/uploads/2026/09/image1-1.webp
  original:
    file: 2026-09-16-nvidia-announces-native-gpu-programming-in-rust.image-590ba519c63f.webp
    width: 1920
    height: 1080
  variants:
  - file: 2026-09-16-nvidia-announces-native-gpu-programming-in-rust.image-2aaaa48910d1.webp
    width: 320
    height: 180
  color: '#020508'
---

*In September 2026, NVIDIA announced it is leaning into native GPU programming in Rust. CUDA C++ and CUDA Python are mature, enterprise-grade toolchains, and NVIDIA will be growing and maturing CUDA Rust into 2027 and beyond*

The systems layer of AI spans inference engines, serving infrastructure, drivers, and agent runtimes, and it churns constantly as models and techniques change. More and more of it is written in Rust, which catches whole classes of bugs at compile time without giving up performance.

NVIDIA is part of that shift for the same reason. The Nova Linux driver is written in Rust. [NVIDIA Dynamo](https://www.nvidia.com/en-us/ai/dynamo/) is built on a Rust core. NVTX has Rust bindings.

The GPU kernel is the exception. You can launch kernels from Rust, but the kernel itself often has to be written in another language.

NVIDIA CUDA Rust closes that gap. GPU kernels can be written in Rust, compiled natively to PTX, rather than a wrapper around code from somewhere else.

There are two tracks to use Rust, matching the two tracks CUDA itself has. **SIMT** is the model you already write in CUDA C++ or [numba-cuda](https://nvidia.github.io/numba-cuda/). You indicate what one thread does, and launch thousands of them. **Tile** is a newer programming model, which is also available in [C++](https://docs.nvidia.com/cuda/cuda-tile-cpp-api-reference/) and [Python](https://docs.nvidia.com/cuda/cutile-python/). All of these frontends let you say what one *tile* of data does, and the [Tile IR compiler](https://docs.nvidia.com/cuda/tile-ir/latest/index.html) does the rest.

When you are picking one to build on, reach for Tile first. The compiler decides how tiles map onto each architecture, so your source doesn’t encode architecture-specific choices, and you drop to SIMT when you need that control or want to manage memory and threads yourself.

Which language you reach for is a separate question from which model. Use the CUDA exposure that best fits the stack you already have. The two projects below are for when that stack is Rust. We plan to support inter-language interop, so the choice does not lock you out of the others.

Below is the same kernel on each track, which performs elementwise addition over 1,024 floats. Both are complete programs, both run, and both print the same line, so you can read them side by side and see what changes.

## The SIMT track: cuda-oxide [](https://developer.nvidia.com/blog/introducing-cuda-rust-two-tracks-for-writing-gpu-kernels/#the_simt_track_cuda-oxide)

[cuda-oxide](https://github.com/NVlabs/cuda-oxide) is a custom `rustc` codegen backend. It intercepts compilation, routes `#[kernel]` functions through Rust MIR, the community [Pliron](https://github.com/pliron-org/pliron) IR framework, and LLVM IR down to PTX, and hands everything else to the standard backend. The GPU dialects on top of Pliron are ours. The dialects and every transform stay in Rust until the standard LLVM backend takes over.

You will need Linux, a GPU with compute capability 8.0 or later, a CUDA toolkit (12.x or newer), clang with its libclang headers, and the pinned nightly toolchain. `cargo oxide doctor` checks all of it, including the optional system LLVM. Install `cargo-oxide`, the Cargo subcommand that drives the build:

```
cargo +nightly-2026-04-03 install --git https://github.com/NVlabs/cuda-oxide.git cargo-oxide
```

Then scaffold a project and run it. The template is a complete vector addition program:

```
cargo oxide new vecadd_demo
cd vecadd_demo
cargo oxide doctor
cargo oxide run
```

The first `cargo oxide run` builds the codegen backend, so expect it to take a while. Later runs reuse the cache.

It prints `PASSED: all 1024 elements correct`. This is the whole program that did it, exactly what `cargo oxide new` wrote, with comments added here:

```
use cuda_device::{kernel, launch_bounds, launch_contract, thread, DisjointSlice};
use cuda_host::cuda_module;
use cuda_core::{CudaContext, DeviceBuffer, LaunchConfig1D};
 
// === DEVICE CODE - everything in here is compiled to PTX ===
// The macro also generates the host-side API used further down:
// `load`, `prepare_vecadd`, and the safe `vecadd` launch method.
#[cuda_module]
mod kernels {
    use super::*;
 
    #[kernel] // GPU entry point
    #[launch_bounds(256)] // max threads per block; lets the compiler budget registers
    #[launch_contract(domain = 1, block = (256, 1, 1))] // indexes in 1-D, 256-thread blocks
    pub fn vecadd(a: &[f32], b: &[f32], mut c: DisjointSlice<f32>) {
        let idx = thread::index_1d();
        let idx_raw = idx.get(); // the plain usize, for reading the inputs
        if let Some(c_elem) = c.get_mut(idx) {
            *c_elem = a[idx_raw] + b[idx_raw];
        }
    }
}
 
fn main() -> Result<(), Box<dyn std::error::Error>> {
    // === HOST SETUP - device, stream, and buffers ===
    let ctx = CudaContext::new(0)?;
    let stream = ctx.default_stream();
 
    const N: usize = 1024;
    let a_host: Vec<f32> = (0..N).map(|i| i as f32).collect();
    let b_host: Vec<f32> = (0..N).map(|i| (i * 2) as f32).collect();
 
    let a_dev = DeviceBuffer::from_host(&stream, &a_host)?;
    let b_dev = DeviceBuffer::from_host(&stream, &b_host)?;
    let mut c_dev = DeviceBuffer::<f32>::zeroed(&stream, N)?;
 
    // === LOAD, PREPARE, LAUNCH ===
    // SAFETY: this package owns the embedded device bundle produced for the
    // kernels module above.
    let module = unsafe { kernels::load(&ctx)? };
 
    // 4 blocks of 256 threads, 0 bytes of dynamic shared memory. `prepare_vecadd`
    // checks that against the contract above and against the live device limits.
    // The safe `vecadd` below takes that token where a raw config would go.
    let prepared = module.prepare_vecadd(LaunchConfig1D::new((N as u32).div_ceil(256), 256, 0))?;
    module.vecadd(&stream, &prepared, &a_dev, &b_dev, &mut c_dev)?;
 
    // === READ BACK AND VERIFY ===
    // Copies down and synchronizes, so the launch has finished by the time
    // `c_host` can be read.
    let c_host = c_dev.to_host_vec(&stream)?;
    let errors = (0..N)
        .filter(|&i| (c_host[i] - (a_host[i] + b_host[i])).abs() > 1e-5)
        .count();
 
    if errors == 0 {
        println!("PASSED: all {} elements correct", N);
    } else {
        eprintln!("FAILED: {} errors", errors);
        std::process::exit(1);
    }
    Ok(())
}
```

Host and device code live in one file, build with one command, and need no separate kernel crate.

Read the kernel signature first, because it carries the whole safety argument. `a` and `b` are ordinary shared slices, readable by every thread. `c` is a `DisjointSlice<f32>`, a type that hands each thread exclusive access to its own element and nothing else. It exists because `&mut [f32]` is the wrong shape for the job. Every thread would need the same `&mut`, which Rust correctly refuses. `DisjointSlice` splits that one mutable borrow into per-thread pieces.

`thread::index_1d()` returns an index type, not a bare integer, and `c.get_mut(idx)` only accepts that type. You get back an `Option`, so the out-of-bounds case is a branch you handle rather than a memory error you find later.

The launch is checked rather than trusted. `#[launch_contract]` declares that this kernel indexes in one dimension with 256-thread blocks. `prepare_vecadd` validates your `LaunchConfig1D` against that declaration and the live device limits, and hands back a proof that the safe `vecadd` method requires. Kernels without a contract expose only raw unsafe launch methods, because a bare `LaunchConfig` says nothing about the kernel it is launching.

## The Tile track: cutile-rs [](https://developer.nvidia.com/blog/introducing-cuda-rust-two-tracks-for-writing-gpu-kernels/#the_tile_track_cutile-rs)

[cutile-rs](https://github.com/NVlabs/cutile-rs) works one level higher. You perform computations on tiles rather than scalars. Each tile block runs the kernel body once as a single logical thread over one sub-tensor of data, and the compiler decides how many real GPU threads back it. The `#[cutile::module]` macro embeds the kernel’s AST in the host binary and JIT-compiles it through CUDA Tile IR (the NVIDIA tile-level compiler IR) when the kernel is first needed.

Requirements are lighter than the SIMT track. You need a GPU with compute capability 8.0 or later, CUDA 13.3, stable Rust 1.89 or newer, and Linux, but no nightly toolchain and no LLVM of your own.

`cutile` is published, so there is nothing to clone:

```
cargo new vecadd_demo
cd vecadd_demo
cargo add cutile
```

Here is the same elementwise addition, written for tiles. Paste it into `src/main.rs` and `cargo run`:

```
use cutile::prelude::*;
 
// The macro captures this module's AST into the host binary. The kernel is
// JIT-compiled through CUDA Tile IR the first time it is actually launched.
#[cutile::module]
mod kernel {
    use cutile::core::*;
 
    #[cutile::entry()]
    fn add<const B: i32>(
        // B is the tile width, a static dimension. A different B produces a
        // different specialization.
        z: &mut Tensor<f32, { [B] }>, // exclusive output, one sub-tensor of B elements
        x: &Tensor<f32, { [-1] }>,    // shared input; -1 is a dynamic dimension, resolved at launch
        y: &Tensor<f32, { [-1] }>,
    ) {
        // This body runs once per mut sub-tensor, as a single logical thread.
        // Tile kernels load tiles, not scalars, from x and y.
        let tx = load_tile_like(x, z); // the slice of x lining up with this sub-tensor of z
        let ty = load_tile_like(y, z);
        z.store(tx + ty); // elementwise across the whole tile
    }
}
 
fn main() -> Result<(), Error> {
    let device = Device::new(0)?;
    let stream = device.new_stream()?;
 
    // These are lazy. Nothing has touched the GPU yet.
    let x = api::ones::<f32>(&[1024]);
    let y = api::ones::<f32>(&[1024]);
 
    // Partitioning does three things at once: gives each tile exclusive
    // ownership of its own 128-element chunk, fixes the grid at 1024/128 = 8
    // tiles, and supplies B.
    let z = api::zeros::<f32>(&[1024]).partition([128]);
 
    let c: Vec<f32> = kernel::add(z, x, y) // takes ownership of all three tensors
        .first()                           // ...and returns them; pick the output back out
        .unpartition()                     // drop the host-side partition wrapper; no data moves
        .to_host_vec()                     // record the copy back
        .sync_on(&stream)?;                // and only now does any of it run
 
    let errors = c.iter().filter(|&&v| (v - 2.0).abs() > 1e-5).count();
    if errors == 0 {
        println!("PASSED: all {} elements correct", c.len());
    } else {
        eprintln!("FAILED: {errors} errors");
    }
    Ok(())
}
```

`PASSED: all 1024 elements correct`

The Tile track reaches the same answer on stable Rust, and its signature makes the same safety argument. There is no `DisjointSlice` this time. Partitioning on the host is only needed for mutable tensors, and it hands each tile block one writable sub-tensor that no other tile block can overlap. That exclusivity is what `&mut` already guarantees.

The `-1` in the input shapes is a sentinel rather than a size. That dimension is read off the tensor at launch, so the shape can vary without recompiling.

The interesting line on the host is `.partition([128])`, and it is doing three jobs at once. It makes the exclusivity real. Each tile owns its 128-element chunk and no other tile can touch it. It fixes the launch geometry, since 1,024 divided by 128 is a grid of 8 tiles.

The grid follows from the partition instead of being computed separately and checked against the kernel’s indexing. It also supplies `B`, which is never written at the call site because the launcher reads the tile width off the partition. That is why a `&mut` output has to be partitioned before it can be passed at all.

Then look at what the launch returns. The `add` you call on the host is a macro-generated launcher, not the device function above. It takes ownership of all three tensors and hands them back as a tuple when the GPU is done. That is what `.first()` is for, picking the output back out of it.

Nothing runs until `.sync_on(&stream)`. Everything before it is a lazy description, recorded rather than submitted. That includes the `ones`, the `zeros`, the kernel call, and even the copy back to the host. The whole program is one chain with a single synchronization point.

## What the compiler catches [](https://developer.nvidia.com/blog/introducing-cuda-rust-two-tracks-for-writing-gpu-kernels/#what_the_compiler_catches)

Both kernels make the same claim about memory. Their inputs are shared, and their output belongs to one writer alone. They differ only in the level at which they make it, and in whether a purpose-built type is needed to make it at all.

That matters because thousands of threads reach the same buffers in no guaranteed order. When two of them hit the same address and one is writing, the ordering decides the result. Those bugs rarely reproduce on demand, and they pass tests before failing in production.

Passing the SIMT kernel’s output buffer as one of its own inputs does not compile, whether or not that kernel would actually race:

```
module.vecadd(&stream, &prepared, &c_dev, &b_dev, &mut c_dev)?;
```

``error[E0502]: cannot borrow `c_dev` as mutable because it is also borrowed as immutable``

The same aliasing on the Tile side does not compile either:

```
let z = api::zeros::<f32>(&[1024]);
kernel::add(z.partition([128]), z, y)
```

`` error[E0382]: use of moved value: `z` ``

Both examples catch the classic aliasing mistake at compile time, and they draw the line in different places. cuda-oxide checks each launch call. cutile-rs’s ownership follows the tensors across the launch boundary, which is the stronger of the two claims.

Tile gives you no shared memory or thread indexing to get wrong, because the compiler owns both. A tile block is a single logical thread, so there are no threads for you to race. That is what makes it safe by construction, and it is also what you trade away. SIMT keeps that control, and today shared memory there requires `unsafe`. Shared memory is the bedrock of fast SIMT kernels, so making that path safe is active work.

## Where the projects stand [](https://developer.nvidia.com/blog/introducing-cuda-rust-two-tracks-for-writing-gpu-kernels/#where_the_projects_stand)

Both projects are early-stage and neither is production-ready. cuda-oxide is early alpha. cutile-rs is further along, published on crates.io and already used outside NVIDIA in HuggingFace’s [Grout](https://github.com/huggingface/grout) inference engine and in [mistral.rs](https://github.com/EricLBuehler/mistral.rs). Coverage is incomplete and APIs will move. Where you find rough edges, we want to hear about them.

Cargo and crates set an expectation that getting started is easy. GPU programming has historically been the opposite, and closing that distance is part of the work. The SIMT track still needs a pinned nightly toolchain, which is exactly the kind of thing we would like to stop asking you for.

Rust on GPUs is not new. There is good work in this space that predates ours and continues alongside it. The [ecosystem appendix](https://nvlabs.github.io/cuda-oxide/appendix/ecosystem.html) in the cuda-oxide book maps where we sit relative to Rust-GPU, rust-cuda, CubeCL, and the rest, and we have been working with the rust-cuda maintainers as both projects mature.

What is new is the engineering we are putting behind it, and a clear sense of where it is going.

## What you can do today [](https://developer.nvidia.com/blog/introducing-cuda-rust-two-tracks-for-writing-gpu-kernels/#what_you_can_do_today)

- **Run the SIMT example.** `cargo oxide new`, then `cargo oxide run`, in [cuda-oxide](https://github.com/NVlabs/cuda-oxide).
- **Run the Tile example.** Clone [cutile-rs](https://github.com/NVlabs/cutile-rs), then `cargo run -p cutile-examples --example hello_world`.
- **Read the docs.** The [cuda-oxide book](https://nvlabs.github.io/cuda-oxide/) and the [cuTile Rust documentation](https://nvlabs.github.io/cutile-rs/main/).
- **Read the paper.** *[Fearless Concurrency on the GPU](https://arxiv.org/abs/2606.15991)*.
- **File issues.** Tell us what broke and what was missing, on [cuda-oxide](https://github.com/NVlabs/cuda-oxide/issues) or [cutile-rs](https://github.com/NVlabs/cutile-rs/issues).
- **Join the conversation.** GitHub Discussions on both repos, or the [cuda-oxide Discord](https://discord.gg/ZUEr4AhH5C).
- **Come to the talk.** Melih Elibol is presenting “Fearless Concurrency on the GPU*”* at [RustConf 2026](https://rustconf.com/), Sept. 8 to 11 in Montréal. NVIDIA will have other staff attending too, so come find us if you’re there!

Tinker with what is here and come work on it with us. It is early, it is open, and what you build now will shape what comes next.

NVIDIA is excited to be leaning in with the Rust community as we elevate native Rust GPU programming. Projects like rust-cuda, rust-gpu, and cudarc pioneered the marriage of GPUs and Rust, and the people behind them, including the team at VectorWare, continue to shape how we think about our own work as we build with the Rust community.
