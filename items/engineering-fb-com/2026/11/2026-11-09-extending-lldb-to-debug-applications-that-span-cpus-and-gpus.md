---
title: Extending LLDB to Debug Applications That Span CPUs and GPUs
link: https://engineering.fb.com/2026/11/09/developer-tools/extending-lldb-to-debug-applications-that-span-cpus-and-gpus/
source: engineering-fb-com
published: 2026-11-09T17:00:12Z
updated: 2026-11-09T17:00:12Z
first_seen: 2026-11-27T00:01:05.630507547Z
labels:
- devinfra
summary: We built a vendor-neutral accelerator plugin architecture in lldb-server for debugging applications that span CPUs and GPUs. It separates shared debugger infrastructure from hardware-specific device control. The design represents the CPU and GPU as separate debugger processes while presenting them in one debugging session. Modern AI applications rarely execute in one place. A workload can [...] Read More... The post Extending LLDB to Debug Applications That Span CPUs and GPUs appeared first on Engineering at Meta.
content: extracted
html: 2026-11-09-extending-lldb-to-debug-applications-that-span-cpus-and-gpus.html
remote_preview:
  url: https://engineering.fb.com/wp-content/uploads/2026/11/Extending-LLDB-Hero.png
---

- We built a vendor-neutral accelerator plugin architecture in lldb-server for debugging applications that span CPUs and GPUs. It separates shared debugger infrastructure from hardware-specific device control.
- The design represents the CPU and GPU as separate debugger processes while presenting them in one debugging session.

Modern AI applications rarely execute in one place. A workload can begin in a high-level framework and move back and forth between native CPU code and kernels running on a GPU. When a bug crosses those boundaries, developers need to understand the host application and the device together.

That is harder than attaching a conventional debugger to another process. The CPU and GPU may use different instruction sets, registers, memory spaces, and execution models. The GPU is also controlled through a runtime and driver inside the host process, so debugging the device often requires careful coordination with the CPU.

Historically, GPU vendors built device-specific debuggers, including AMD’s ROCgdb and NVIDIA’s CUDA-GDB. These tools provide deep support for their respective platforms, but common debugger capabilities must often be implemented and maintained separately.

At Meta, we have been developing a different approach for LLDB: a common accelerator plugin architecture in lldb-server. In this post, “GPU” refers to the concrete devices and examples, while “accelerator” refers to LLDB’s broader, vendor-neutral plugin abstraction. The architecture has evolved through contributions from engineers at Meta and across the accelerator ecosystem. It keeps the shared debugger independent of any one GPU while giving hardware vendors a focused place to implement device-specific behavior.

## Why GPU Debugging Challenges Traditional Debugger Models

A traditional debugger generally assumes that one process has one architecture and a consistent view of threads, registers, and memory. GPU programs challenge each of those assumptions.

A host process might run on x86-64 while its kernels use a completely different instruction set. A CPU thread has a familiar register context, while a GPU may execute groups of lanes in lockstep. Host and device pointers can refer to different address spaces. A GPU can expose thousands of execution lanes, which do not map cleanly onto the thread model that most command-line and graphical debuggers present.

Combining all of this state into one artificial process model makes the debugger more complicated. Every operation must first determine whether it applies to the CPU or the GPU. Register handling, memory reads, breakpoints, stepping, and stop events all become conditional on the selected hardware. A single process model also forces the CPU and GPU into one unified memory map, even though their address spaces can be independent: The same address, such as 0x1000, can refer to different locations in the CPU and GPU processes.

Instead of hiding those differences, the LLDB accelerator architecture gives them a clear boundary.

## Plugin Support in lldb-server for GPU Debugging

LLDB uses a companion process called lldb-server  to launch or attach to a program and perform low-level debugging operations. The LLDB client communicates with the server through the GDB remote protocol, a command protocol for operations such as reading memory, inspecting registers, controlling execution, and setting breakpoints.

Because lldb-server  already controls the native process, it is also the right place to coordinate with an accelerator runtime. The new plugin interface lets a hardware-specific backend work alongside the normal CPU debugger. The backend translates a vendor’s device debugging API into concepts LLDB already understands, such as processes, threads, registers, memory, modules, and stop reasons.

This separation keeps the LLDB client vendor-neutral. Common improvements to the command line, remote protocol, symbol handling, and IDE integration can benefit every accelerator. At the same time, each vendor remains responsible for the details that are unique to its hardware and driver stack.

GPU debugging support is activated only when the debugged application uses a supported GPU runtime. For CPU-only applications, LLDB and lldb-server   continue through the existing native debugging path without incurring runtime computational overhead.

## Two Targets, One Debugging Experience

The architecture represents the CPU and accelerator as separate LLDB targets.\
 Each target has its own architecture and is associated with a debugger process that maintains its threads, registers, address space, and execution state. The CPU continues to use LLDB’s established native debugging path. When the accelerator becomes available, the plugin provides a second connection that LLDB uses to create the device target.

To the debugger, the two targets remain technically independent. To the developer, they can appear in one LLDB or IDE session. This avoids forcing incompatible CPU and GPU concepts into a single process while still supporting a unified workflow.

![](https://engineering.fb.com/wp-content/uploads/2026/11/Extending-LLDB.png)\
The CPU and GPU remain separate LLDB targets while a plugin in lldb-server coordinates host and device debugging.

## Coordinating the Host and Device

A typical debugging session follows a small set of steps:

1. LLDB connects to lldb-server  and discovers which accelerator plugins are available.
2. A plugin can ask LLDB to set internal breakpoints in the host process. These breakpoints identify important runtime events, such as the point at which a GPU debugging session can begin.
3. When the host reaches one of those points, lldb-server  notifies the plugin. The plugin initializes the vendor debugging API and prepares an accelerator debugging endpoint.
4. LLDB creates a second target for the accelerator and connects to it. Device threads, registers, memory, modules, and breakpoints can then use LLDB’s existing abstractions.

Structured actions coordinate the two sides. A plugin can request CPU breakpoints, provide connection information, report device modules, and synchronize host and device execution. The common protocol describes what the debugger needs to do without exposing vendor-specific API details to the LLDB client.

We also built a mock accelerator plugin that exercises this flow without specialized hardware. It lets the LLVM test suite verify discovery, breakpoint coordination, connection creation, and basic device state. This has been important for developing shared infrastructure that can be reviewed and tested independently of any particular GPU.

## A Typical GPU Workflow

The [vector dot-product HIP sample](https://gist.github.com/satyajanga/3c629abbc05a9596195ac59820a12d16) produced this abridged session:

```none
(lldb) target list
Current targets:
  target #0: gpu_demo (x86_64, stopped)
* target #1: amd_memory_kernel[...] (amdgcn, stopped)

(lldb) b compute_element
Breakpoint 1: no locations (pending).
(lldb) c
(lldb) target select 0
(lldb) c
Launching kernel: 2 blocks x 32 threads
1 location added to breakpoint 1
Target 1 stopped.

(lldb) target select 1
(lldb) bt
* frame #0: compute_element(..., idx=32) at gpu_demo.hip:26
  frame #1: vector_dot_kernel(..., n=64) at gpu_demo.hip:35
```

*The pending breakpoint resolves when the GPU module loads, and the backtrace comes from the GPU target.*

## A Common Plugin Framework Across Hardware Vendors

The plugin boundary is intentionally based on debugger operations rather than concepts from a specific GPU platform. That makes it useful across devices with very different instruction sets, runtime APIs, and execution models.

The framework supports postmortem GPU debugging as well as live sessions. GPU core-dump plugins use a common base class for their LLDB integration, while vendor implementations provide the device-specific thread, register, memory, and module state.

GPU debugging efforts at Meta are already leveraging the architecture. Because we operate a [heterogeneous fleet of GPUs](https://ai.meta.com/blog/meta-mtia-scale-ai-chips-for-billions/) with different hardware and software stacks, the fact that these efforts use the same model is an important validation of the design: the common LLDB layer can remain stable while each backend represents its own device accurately.

At the time of writing, we have upstreamed most of the common framework to LLVM. This includes the lldb-server plugin interface; GDB-remote packets for plugin discovery, initialization, runtime-event coordination, and GPU connection creation; support for associating CPU and GPU debugger processes while preserving their independent architecture; support for multiple address spaces, and hardware-independent mock tests. We expect to upstream the remaining pieces soon.

## Looking Ahead

Next, we plan to finish upstreaming the remaining common infrastructure and expand the vendor integrations built on it. Our goal is that integrating new hardware with LLDB should require only a vendor-specific plugin, not a separate debugger to build and maintain.

## Acknowledgements

*We are grateful to Meta’s Developer Infrastructure leadership team, especially Shane Nay, Todd Fiala, and Ale Contenti, whose direct support made this work possible.*

*We also thank our Debugging & Tooling teammates Jacob Lalonde, David Young, Alexandre Perez, Zhiyuan Li, Piyush Jaiswal, and Philip DePetro for their collaboration and support.*

*Finally, we thank the open source community, especially Jonas Devlieghere, Jim Ingham, David Spickett, and Jason Molenda, for reviewing the RFCs and upstream pull requests.*
