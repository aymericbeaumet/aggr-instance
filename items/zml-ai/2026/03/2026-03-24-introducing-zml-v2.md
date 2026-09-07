---
title: Introducing ZML/v2
link: https://zml.ai/posts/zml-v2/
source: zml-ai
published: 2026-03-24T15:00:00Z
updated: 2026-03-24T15:00:00Z
first_seen: 2026-09-07T17:03:44.343711062Z
summary: 'ZML is an inference stack built close to the hardware. It lowers models directly onto NVIDIA, AMD, TPU, and Trainium targets from a single codebase, without depending on and suffering from the Python-heavy runtime layers that most of the ecosystem is built around. The guiding idea behind zml/v1 was simplicity: give ZML a model and its weights, and the system would take care of compilation, placement, and execution for you. That made the first version approachable and effective for standard deployments, but it also baked too much behavior into implicit global state. As the project pushed into partial compilation, custom passes, sharding, quantization, and more backend-specific execution paths, those implicit shortcuts became constraints. ZML/v2 is the rewrite that makes those concepts explicit: platform ownership, compilation, memory, IO, and placement are now first-class, so advanced use cases can be expressed directly instead of forced through workarounds.'
content: extracted
html: 2026-03-24-introducing-zml-v2.html
---

ZML is an inference stack built close to the hardware. It lowers models directly onto NVIDIA, AMD, TPU, and Trainium targets from a single codebase, without depending on and suffering from the Python-heavy runtime layers that most of the ecosystem is built around.

The guiding idea behind zml/v1 was simplicity: give ZML a model and its weights, and the system would take care of compilation, placement, and execution for you. That made the first version approachable and effective for standard deployments, but it also baked too much behavior into implicit global state. As the project pushed into partial compilation, custom passes, sharding, quantization, and more backend-specific execution paths, those implicit shortcuts became constraints. ZML/v2 is the rewrite that makes those concepts explicit: platform ownership, compilation, memory, IO, and placement are now first-class, so advanced use cases can be expressed directly instead of forced through workarounds.

## Composability as a core principle

ZML/v2 is a complete rewrite of the framework with a focus on developer experience, performance and most importantly **composability**. This is a fundamental departure from the original design which was a lot more turnkey but could make more complex programs a bit harder than necessary.

So we’ve decided to strongly orient the design towards composability while also offering an uplift in performance.

## Explicit over implicit

The new API centers around `zml.Platform`s, which is the main abstraction across accelerators, runtimes, IO etc. In ZML, a platform is instantiated and from then on used to transfer data, build, compile and execute programs.

This is how it all starts:

```zig
const platform: *zml.Platform = try .auto(allocator, io, .{});
```

ZML offers various platforms selection mechanisms: compile-time, runtime and manual. The automatic runtime selection is the default and will pick the best available platform on the system, but users can also choose to compile for a specific platform or manually create a platform with custom options.

The then built program can now run transparently on all comptime-time enabled platforms, which can be enabled via the `--@zml//platforms:{cuda,rocm,tpu,neuron}=true` build flags. Doing so will download the runtime, strip it to its bare minimum and embed it inside the binary sandbox.

For instance, here is the **entire** CUDA sandbox:

```
$ tree --du -h bazel-bin/examples/llm/llm.runfiles/+cuda_packages+libpjrt_cuda/sandbox/
[2.9G]  bazel-bin/examples/llm/llm.runfiles/+cuda_packages+libpjrt_cuda/sandbox/
├── [ 82M]  bin
│   ├── [3.0M]  compat_probe
│   ├── [ 40M]  nvlink
│   └── [ 39M]  ptxas
├── [2.8G]  lib
│   ├── [409M]  compat
│   │   ├── [ 11M]  libcudadebugger.so.1
│   │   ├── [ 94M]  libcuda.so.1
│   │   ├── [ 86M]  libnvidia-gpucomp.so.590.48.01
│   │   ├── [ 24M]  libnvidia-nvvm70.so.4
│   │   ├── [ 75M]  libnvidia-nvvm.so.4
│   │   ├── [ 14K]  libnvidia-pkcs11-openssl3.so.590.48.01
│   │   ├── [ 31M]  libnvidia-ptxjitcompiler.so.1
│   │   └── [ 90M]  libnvidia-tileiras.so.590.48.01
│   ├── [479M]  libcublasLt.so.13
│   ├── [ 52M]  libcublas.so.13
│   ├── [740K]  libcudart.so.13
│   ├── [104M]  libcudnn_adv.so.9
│   ├── [2.3M]  libcudnn_cnn.so.9
│   ├── [233M]  libcudnn_engines_precompiled.so.9
│   ├── [ 26M]  libcudnn_engines_runtime_compiled.so.9
│   ├── [3.5M]  libcudnn_graph.so.9
│   ├── [ 56M]  libcudnn_heuristic.so.9
│   ├── [ 37M]  libcudnn_ops.so.9
│   ├── [126K]  libcudnn.so.9
│   ├── [286M]  libcufft.so.12
│   ├── [4.4M]  libcupti.so.13
│   ├── [136M]  libcusolver.so.12
│   ├── [162M]  libcusparse.so.12
│   ├── [218M]  libnccl.so.2
│   ├── [ 95M]  libnvJitLink.so.13
│   ├── [4.2M]  libnvrtc-builtins.so.13.1
│   ├── [109M]  libnvrtc.so.13
│   ├── [ 40M]  libnvshmem_host.so.3
│   ├── [ 39K]  libnvtx3interop.so
│   ├── [383M]  libpjrt_cuda.so
│   ├── [2.9M]  libzmlxcuda.so.0
│   ├── [118K]  libz.so.1
│   ├── [ 72K]  nvshmem_bootstrap_uid.so.3
│   └── [1023K]  nvshmem_transport_ibrc.so.4
└── [ 77M]  nvvm
    ├── [ 77M]  bin
    │   └── [ 77M]  cicc
    └── [458K]  libdevice
        └── [454K]  libdevice.10.bc
```

## Pinned Memory Allocators

ZML/v2 enables the explicit allocation and use of pinned memory. Pinned memory is memory that is not movable nor swappable and thus can be directly accessed by the DMA engine of the accelerators. This functionality is exposed via the `zml.mem.DmaAllocator` which, combined with Zig’s explicit allocator model, enables unique optimizations and removes extraneous copies found in other frameworks.

## Zero and overlapped copy primitives

Data transfers between host and accelerator now compose with `std.Io.Writer` via `zml.io.MemoryWriter`, enabling zero-copy and overlapped data movement across the board.

For instance, loading a model from disk to GPU memory now happens at SSD speed when using `O_DIRECT`, even with staging through the system memory, since transfers are overlapped. This enables NVIDIA GPUDirect-like performance on raw POSIX.

```
$ bazel build --config=release //examples/llm --@zml//platforms:cuda=true
$ bazel-bin/examples/llm/llm --model=/var/models/meta-llama/Llama-3.1-8B-Instruct/
...
info(llm): Selected backend: .cuda_fa2
info(llm): Loaded tokenizer [181.256ms]
info(llama): Loaded weights [14.96GiB, 1.165s, 12.83GiB/s]
```

## Userland Virtual File System

Thanks to injectable `std.Io`, ZML implements a full userland VFS built into the program. It supports local files, HTTP endpoints, S3 buckets and Hugging Face repositories. Concretely, this means that models can be loaded directly from each of those sources without downloading to disk first, regardless of the underlying FS implementation.

Let’s try to register an HF implementation and load the tokenizer directly from Hugging Face:

```zig
var vfs: zml.io.VFS = try .init(init.gpa, init.io);
defer vfs.deinit();

var hf_vfs: zml.io.VFS.HF = try .auto(init.gpa, init.io, &http_client, init.environ_map);
defer hf_vfs.deinit();
try vfs.register("hf", hf_vfs.io());

// Use the VFS IO from now on
const io = vfs.io();

const tokenizer: zml.Tokenizer = blk: {
    const file = try std.Io.Dir.openFile(.cwd(), io, "hf://Qwen/Qwen3-8B/tokenizer.json", .{});
    defer file.close(io);
    var reader = file.reader(init.gpa, &.{});
    const bytes = try reader.interface.readAlloc(init.gpa, try file.length(io));
    defer init.gpa.free(bytes);
    break :blk try .fromBytes(init.gpa, io, bytes);
};
```

Or load the model directly from Hugging Face:

```
$ bazel-bin/examples/llm/llm --model=hf://meta-llama/Llama-3.1-8B-Instruct
```

## Powerful sharding primitives

Sharding is now a first-class citizen of the programming model instead of an implicit backend detail. We introduce a new public `zml.sharding` API with `PhysicalMesh`, `LogicalMesh`, `Strategy`, `Sharding`, and `Placement`. Compile / load / buffer APIs now take explicit sharding information and finally `zml.ops.manualComputation` expresses a shard-local primitive similar to JAX’s `shard_map`.

It also fully leverages [Shardy](https://openxla.org/shardy) or [GSPMD](https://research.google/blog/general-and-scalable-parallelization-for-neural-networks/) based on the target platform transparently.

## Pluggable Attention backend

ZML/v2 provides a pluggable attention backend system, giving a clean way to select and configure different attention implementations without rewriting model code around each kernel.

Instead of baking backend-specific logic into every call site, `zml.attention` exposes an explicit `Backend` plus backend-typed `Parameters`, `Metadata`, and paged-attention `Context`, so the same high-level attention path can target vanilla behavior or optimized CUDA FlashAttention variants like Tri-Dao’s FA2 and FA3.

The backend is selected automatically based on the platform and accelerator.

In this release, we took the opportunity to integrate [FlashAttention 2 and 3 built on CUDA for `sm80` up to `sm121`](https://github.com/zml/flash-attention/releases/tag/v2026.3.23).

## Complete build sandboxing

Our build now runs entirely on the 100% hermetic LLVM toolchain [@llvm](https://github.com/cerisier/toolchains_llvm_bootstrapped). This enables full reproducibility, better cross-compilation support, and a more consistent development experience across platforms. This means that ZML supports remote execution on services such as [BuildBuddy](https://www.buildbuddy.io/) or [NativeLink](https://www.nativelink.com/).

On macOS, we no longer rely on Xcode nor Command Line Tools to build ZML. The macOS SDK is downloaded as part of the build.

Cross compilation is also fully supported, enabling fully working containers for any supported ZML platform to be built right from a Mac.

## //examples/llm

Try it today via the `llm` CLI:

```
$ bazel build --config=release //examples/llm --@zml//platforms:cuda=true
$ bazel-bin/examples/llm/llm --model=hf://Qwen/Qwen3.5-9B
```
