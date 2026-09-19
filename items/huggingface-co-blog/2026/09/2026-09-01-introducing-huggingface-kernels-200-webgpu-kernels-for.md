---
title: 'Introducing @huggingface/kernels: 200+ WebGPU Kernels for Local AI'
link: https://huggingface.co/blog/webgpu-kernels
source: huggingface-co-blog
published: 2026-09-01T00:00:00Z
updated: 2026-09-01T00:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2026-09-01-introducing-huggingface-kernels-200-webgpu-kernels-for.html
preview:
  file: 2026-09-01-introducing-huggingface-kernels-200-webgpu-kernels-for.preview-86139f0b99a4.webp
  width: 256
  height: 128
  color: '#282a34'
images:
- source: https://huggingface.co/blog/assets/webgpu-kernels/thumbnail.png
  original:
    file: 2026-09-01-introducing-huggingface-kernels-200-webgpu-kernels-for.image-cb9ef8fc9c86.png
    width: 1300
    height: 650
  color: '#00000a'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/webgpu-kernels/ai-onnx-add.png
  original:
    file: 2026-09-01-introducing-huggingface-kernels-200-webgpu-kernels-for.image-8637b95eaf14.png
    width: 1285
    height: 408
  variants:
  - file: 2026-09-01-introducing-huggingface-kernels-200-webgpu-kernels-for.image-fdbff8a39e1c.webp
    width: 320
    height: 102
  - file: 2026-09-01-introducing-huggingface-kernels-200-webgpu-kernels-for.image-98066aa9d6ea.webp
    width: 640
    height: 203
  - file: 2026-09-01-introducing-huggingface-kernels-200-webgpu-kernels-for.image-a70a60d6518f.webp
    width: 960
    height: 305
  - file: 2026-09-01-introducing-huggingface-kernels-200-webgpu-kernels-for.image-3b53f47ea166.webp
    width: 1285
    height: 408
  color: '#111928'
- source: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/webgpu-kernels/kernels.png
  original:
    file: 2026-09-01-introducing-huggingface-kernels-200-webgpu-kernels-for.image-0a22fee182d7.png
    width: 1579
    height: 894
  variants:
  - file: 2026-09-01-introducing-huggingface-kernels-200-webgpu-kernels-for.image-a87a7de8a622.webp
    width: 320
    height: 181
  - file: 2026-09-01-introducing-huggingface-kernels-200-webgpu-kernels-for.image-ff6800376d08.webp
    width: 640
    height: 362
  - file: 2026-09-01-introducing-huggingface-kernels-200-webgpu-kernels-for.image-70aabeb2bc6c.webp
    width: 960
    height: 544
  - file: 2026-09-01-introducing-huggingface-kernels-200-webgpu-kernels-for.image-cabc46a11c73.webp
    width: 1280
    height: 725
  - file: 2026-09-01-introducing-huggingface-kernels-200-webgpu-kernels-for.image-5b93fd3a3895.webp
    width: 1579
    height: 894
  color: '#0c101b'
---

One of our biggest goals on the WebAI team at Hugging Face is to make browser inference as fast and as user-friendly as possible. Getting there is a multi-layer effort: models need browser-friendly representations, runtimes need to build efficient execution plans, and the individual GPU operations at the bottom of the stack need to make the most of many different devices and browser implementations.

Today, we are releasing the first layer of that effort: [`@huggingface/kernels`](https://www.npmjs.com/package/@huggingface/kernels), a minimal library for loading and running optimized WebGPU kernels from the Hugging Face Hub, together with an initial collection of **207 kernels** at [huggingface.co/webgpu-kernels](https://huggingface.co/webgpu-kernels).

The collection covers operations used across a wide variety of machine learning architectures and workloads. More importantly, each kernel is published as a complete, versioned package: its interface, shader templates, correctness cases, benchmark cases, and usage instructions all live together on the Hub.

We are also launching [Fleet](https://webgpu-kernels-fleet.hf.space/), an in-browser GPU benchmarking and testing suite that runs and scores the kernels on your hardware. Beyond the results for your own machine, Fleet gives the community a way to contribute performance and correctness evidence from devices we could never cover in a conventional test lab. With your consent, every run adds private evidence that can help us find failures (incorrect results, pathologically slow cases, etc.), improve kernel variants, and make better optimization decisions across real-world hardware.

## [](https://huggingface.co/blog/webgpu-kernels#tldr) TL;DR

- **207 WebGPU kernels**, published as individual repositories in the [`webgpu-kernels`](https://huggingface.co/webgpu-kernels) organization. Apache-2.0 licensed.
- **A JavaScript loader**, `@huggingface/kernels`, which downloads, prepares, and runs kernels directly from the Hub.
- **Explicit contracts and reproducible evidence** for every kernel, including manifests, correctness tests, benchmark cases, and WGSL shader templates.
- **Fleet**, a browser-based benchmarking tool that crowdsources correctness and performance evidence across real-world GPUs to help us improve kernels and their variants.

## [](https://huggingface.co/blog/webgpu-kernels#why-start-with-kernels) Why start with kernels?

A model running in the browser eventually becomes a sequence of GPU operations: matrix multiplications, normalizations, convolutions, attention primitives, quantization operations, data-layout transformations, and many more. WebGPU makes these operations available across modern browsers through a portable API, while WGSL provides a common language for the shaders that execute them.

Portability, however, does not automatically mean performance. Two shaders can implement the same operation and produce the same output while behaving completely differently across different accelerators. Workgroup sizes, memory access patterns, vectorization, data types, and fusion strategies can all affect performance. The best choice can also change with the input shape, device, browser, and available WebGPU features.

This is why kernels form a foundational layer of fast browser inference. Higher-level runtimes can only be as efficient as the operations they dispatch. By making those operations individually discoverable, testable, benchmarkable, and versioned, we can improve the foundation independently while keeping a stable contract for the layers above it.

## [](https://huggingface.co/blog/webgpu-kernels#a-kernel-repository-not-just-a-shader) A kernel repository, not just a shader

Each kernel in the collection has its own repository and kernel card. The card documents the operation's semantics, inputs, outputs, attributes, supported data types, source files, and a ready-to-run `@huggingface/kernels` example.

For example, [`ai.onnx.Add`](https://huggingface.co/webgpu-kernels/ai.onnx.Add) implements elementwise addition with multidirectional broadcasting. It is one of the simplest operations in a neural network, used everywhere from residual connections to adding a bias. Its card documents the two inputs, the broadcasted output shape, supported data types, and the variants available for different shapes and devices.

![Files in the ai.onnx.Add WebGPU kernel repository](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/webgpu-kernels/ai-onnx-add.png)

The `ai.onnx.Add` repository packages its manifest, correctness and benchmark cases, and WGSL shader templates together.

Behind the card, the repository contains the artifacts needed to understand and evaluate the implementation:

- **`manifest.json`** is the source of truth for the operation contract. It defines inputs, outputs, attributes, type constraints, and shape derivation rules.
- **`metadata.json`** records the kernel identifier, digests, and provenance.
- **`test.json`** contains correctness cases, so an implementation can be checked against expected behavior.
- **`bench.json`** contains benchmark and tuning cases that represent the workloads used to evaluate the kernel.
- **`*.wgsl.jinja`** files contain the parameterized WGSL implementations used to produce shaders for a particular request and device.

This structure turns a shader into a reusable software artifact. The interface is inspectable without reading WGSL, correctness and performance cases travel with the implementation, and published versions can be loaded explicitly rather than depending on an unversioned file URL. Our kernels can also serve as reference implementations for developers building custom WebGPU kernels or integrating these operations into their own runtimes.

## [](https://huggingface.co/blog/webgpu-kernels#loading-a-kernel-from-the-hub) Loading a kernel from the Hub

Install the package from npm:

```
npm install @huggingface/kernels@preview
```

> Running these kernels requires a browser with [WebGPU support](https://developer.mozilla.org/en-US/docs/Web/API/WebGPU_API). WebGPU availability depends on the browser, operating system, GPU, and driver. You can check for it in JavaScript with `"gpu" in navigator`.

`@huggingface/kernels` provides the bridge between a kernel repository and your application. Call `getKernel` with a Hub repository ID and a contract version, then invoke the returned function with typed input data and tensor shapes. Here is a small bias-add example:

```
import { getKernel } from "@huggingface/kernels";

const add = await getKernel("webgpu-kernels/ai.onnx.Add", { version: 1 });

const { c } = await add({
  a: {
    data: new Float32Array([1, 2, 3, 4, 5, 6]),
    shape: [2, 3],
  },
  b: {
    data: new Float32Array([10, 20, 30]),
    shape: [3],
  },
});
```

The second input is broadcast across the first dimension, producing an output with shape `[2, 3]`. The loader derives that output shape and logical data type from the manifest contract and the inputs, then allocates `c` automatically.

Addition on six floats is deliberately the smallest possible demo. At this size, the GPU round trip costs far more than the math. The point is the call pattern: it stays exactly the same for the heavyweight operations where optimized kernels actually pay off, such as matrix multiplication (`ai.onnx.MatMul`). Only the repository ID and the inputs change.

Even this elementary operation illustrates why kernels need variants. Equal-shape addition can use a direct vectorized path, while broadcasted inputs need different indexing logic. The published Add kernel includes variants for equal shapes, vectorized broadcasting, scalar processing, and general broadcasting. The runtime can select an implementation that fits the current call and device without changing the application-facing API.

The `version: 1` option selects version 1 of the published **kernel contract**. It is separate from an ONNX opset, an operator's `since_version`, or a model revision. Keeping those concepts separate lets applications depend on a stable JavaScript-facing contract while kernel implementations evolve behind it.

## [](https://huggingface.co/blog/webgpu-kernels#how-fast-are-the-kernels) How fast are the kernels?

So, how much of a difference do optimized kernels actually make? We put our collection head-to-head with ORT WebGPU on an Apple M4 GPU, using ONNX Runtime Web `1.30.0-dev.20260826-b1f76d586a`. We started with 1,756 test cases across all 207 operations and kept the 809 cases where both sides produced matching outputs and reliable timings.

Across those comparisons, our kernels were **2.57x faster by geometric mean** and **1.90x faster at the median**, with 629 wins, 176 losses, and 4 ties. Here is a closer look at four familiar operations:

| Operation          | Compared cases | Our WebGPU Kernel | ORT WebGPU | Speedup   |
| ------------------ | -------------- | ----------------- | ---------- | --------- |
| Add                | 5              | 0.064 ms          | 0.227 ms   | **3.52x** |
| MatMul             | 29             | 0.115 ms          | 0.131 ms   | **1.14x** |
| Softmax            | 12             | 0.114 ms          | 0.240 ms   | **2.11x** |
| LayerNormalization | 6              | 0.061 ms          | 0.135 ms   | **2.22x** |

Some individual wins were much bigger. A particularly difficult bilinear Einsum case (`i,ij,j` with size 4096) ran in 0.136 ms with our kernel versus 1,396 ms with ORT WebGPU: more than **10,000x faster**. A row-wise CumSum over `[256, 4096]` was **301x faster**, at 0.016 ms versus 4.784 ms. These are unusual cases rather than the speedups you should expect everywhere, but they show how much a specialized kernel can help when a general implementation hits a slow path.

We timed the work done on the GPU itself, leaving out setup such as loading kernels, creating sessions, uploading inputs, compiling shaders, and reading outputs back. Very short workloads are naturally harder to measure, and small cases can benefit from the GPU cache, so these numbers are best read as a useful comparison rather than a promise for every application.

They are also results for individual operations, not complete models. Exact performance will change across GPUs and browsers, which is why Fleet is so important for building a broader picture.

We are also working with the ONNX Runtime team to upstream these improvements so they can benefit the broader ONNX Runtime Web ecosystem.

## [](https://huggingface.co/blog/webgpu-kernels#from-one-device-to-a-fleet) From one device to a fleet

WebGPU performance varies across GPUs, browsers, and drivers, so results from one machine only tell part of the story. [Fleet](https://webgpu-kernels-fleet.hf.space/) lets anyone run correctness and performance checks in the browser and see how the kernels behave on their hardware.

With consent, each run privately contributes evidence that helps us spot device-specific failures, compare variants, and improve selection rules. The goal is simple: use broad, real-world coverage to make the kernels faster and more reliable for everyone.

## [](https://huggingface.co/blog/webgpu-kernels#building-a-shared-foundation-for-webai) Building a shared foundation for WebAI

The initial 207 kernels are a starting point, not the end state. Publishing kernels independently on the Hub gives us a common place to inspect contracts, compare implementations, reproduce correctness checks, and improve performance without embedding every shader directly into every runtime.

The collection is also part of the Hub's broader kernel ecosystem: on the [Kernels page](https://huggingface.co/kernels?platform=webgpu&sort=trending), the WebGPU kernels sit alongside kernels for CUDA, ROCm, Metal, and other platforms, and can be filtered, sorted, and explored like any other artifact on the Hub.

![The Hub Kernels page filtered to the WebGPU platform, listing the 207 published kernels](https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/blog/webgpu-kernels/kernels.png)

All 207 WebGPU kernels on the Hub's [Kernels page](https://huggingface.co/kernels?platform=webgpu&sort=trending), filtered by platform.

The pieces reinforce one another:

1. Kernel repositories define transparent, versioned operation contracts.
2. `@huggingface/kernels` makes those operations straightforward to load and run from JavaScript.
3. Fleet crowdsources real-world evidence across a much broader range of devices than a conventional benchmark lab can cover.
4. Every contributed run can reveal failures, guide tuning, improve variant selection, and help validate future kernel versions.

This is the low-level foundation for the next steps in our browser inference stack. We are excited to connect these kernels to higher-level model tooling, continue expanding operation coverage, and make fast local inference easier to use across the WebAI ecosystem.

Explore the [WebGPU kernel collection](https://huggingface.co/webgpu-kernels), try [`@huggingface/kernels`](https://www.npmjs.com/package/@huggingface/kernels), and [join the Fleet](https://webgpu-kernels-fleet.hf.space/) to contribute evidence from your device and help us make the kernels better for everyone.
