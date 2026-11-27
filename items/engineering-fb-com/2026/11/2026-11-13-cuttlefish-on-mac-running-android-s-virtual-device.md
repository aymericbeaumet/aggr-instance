---
title: 'Cuttlefish on Mac: Running Android’s Virtual Device Framework Natively on Apple Silicon'
link: https://engineering.fb.com/2026/11/13/android/cuttlefish-on-mac/
source: engineering-fb-com
published: 2026-11-13T17:00:39Z
updated: 2026-11-13T17:00:39Z
first_seen: 2026-11-27T00:01:05.630507547Z
labels:
- android
summary: 'How Meta''s Reality Labs brought Cuttlefish and its QEMU backend to macOS and Apple silicon. [...] Read More... The post Cuttlefish on Mac: Running Android’s Virtual Device Framework Natively on Apple Silicon appeared first on Engineering at Meta.'
content: extracted
html: 2026-11-13-cuttlefish-on-mac-running-android-s-virtual-device.html
remote_preview:
  url: https://engineering.fb.com/wp-content/uploads/2026/11/Cuttlefish-on-Mac-lead.png
---

How Meta's Reality Labs brought Cuttlefish and its QEMU backend to macOS and Apple silicon.

Meta’s Reality Labs builds products such as [AI glasses](https://www.meta.com/smart-glasses/). Emulators provide us with a way to scale development and testing without relying on scarce physical hardware.

Testing software for non-phone Android devices depends on how closely a virtual device reproduces the software behavior of the physical product. While a lightweight emulator like Goldfish gets you part of the way, differences from the real device can hide bugs until later in development. This lack of fidelity also erodes trust in device emulation, increasing reliance on scarce physical hardware.

[Cuttlefish](https://source.android.com/docs/devices/cuttlefish) is the Android Open Source Project’s (AOSP’s) virtual-device framework for testing Android software against virtual hardware. It boots the same system images as real hardware and runs a realistic software stack against virtual hardware. This enables Cuttlefish to more closely track a physical device’s behavior than a lightweight emulator does.

Unfortunately, Cuttlefish is only supported on Linux, and our application and library developers work on Macs with Apple silicon. This forced us to support Goldfish and Cuttlefish side by side.

We knew if we could consolidate onto a single Cuttlefish-based platform, we could give all of our developers the benefit of the same high-fidelity virtual device. The challenge was porting the stack to make Cuttlefish run natively on macOS.

## The Impact of Fragmentation

Maintaining dual emulator platforms imposed a heavy operational burden: It drained computing resources, created confusion for developers, and slowed down the emulator team. Every tool required support for two separate platforms, while infrastructure and engineering capacity were divided between them. Furthermore, developers had to choose an emulator target based on how they worked: Goldfish ran everywhere but had less fidelity while Cuttlefish needed Linux.

![](https://engineering.fb.com/wp-content/uploads/2026/11/Cuttlefish-on-Mac-image-1.png)

The AOSP mirrors this exact divide: Goldfish targets application developers, whereas Cuttlefish serves Android platform developers. So long as we maintained that division, we would suffer the effects of fragmentation.

## The Stack and the macOS Gaps

Cuttlefish is the orchestration layer, while the component that actually runs the guest OS is a virtual machine monitor (VMM). On Linux, Cuttlefish normally uses [crosvm](https://crosvm.dev/book/) but previously included support for [QEMU](https://www.qemu.org/) as an alternative VMM. [That integration had become inactive over time](https://android.googlesource.com/device/google/cuttlefish/+/20895e84134be01f7ee787012c11de1ae68aac7f). To enable native Apple silicon support on macOS, we revived QEMU support.

Many of Cuttlefish’s virtual devices use [vhost-user](https://qemu-project.gitlab.io/qemu/interop/vhost-user.html) backends, separate processes that provide [virtio](https://docs.oasis-open.org/virtio/virtio/v1.2/virtio-v1.2.html) devices to QEMU over a socket. The architecture is clean, but large parts of it were written with a hard Linux assumption. Three of those assumptions break on macOS. The vhost-user machinery is gated to Linux and won’t build or run. The Apple hypervisor imposes resource limits QEMU didn’t expect. Finally, the native display, build, and signing paths didn’t exist. We tackled all three.

![](https://engineering.fb.com/wp-content/uploads/2026/11/Cuttlefish-on-Mac-image-2.png)\
*The Cuttlefish-on-Mac stack. Cuttlefish orchestrates virtual devices backed by QEMU. The highlighted orange blocks are the Mac-specific work: adding macOS support to vhost-user, increasing memory-region tracking slots in QEMU’s HVF backend, and using the native Cocoa display and code-signing paths.*

### Making vhost-user Portable Beyond Linux

The most structurally important changes were in QEMU’s vhost-user path. The frontends, libvhost-user, and the contrib backends such as vhost-user-blk were all gated to Linux. We relaxed those build restrictions so the vhost-user  frontends and libraries can build on macOS as well as Linux. This lets Cuttlefish’s device backends hand off to QEMU on a Mac the same way they do on Linux. Without this, the higher-fidelity device model that makes Cuttlefish worth using isn’t available outside Linux.

### Living Within Hypervisor.framework

On Apple silicon, QEMU accelerates through HVF (Hypervisor.framework) instead of [KVM](https://linux-kvm.org/page/Main_Page). QEMU’s HVF backend did not track enough memory regions for larger Cuttlefish configurations. We increased its tracking-slot count so those configurations could run correctly.

### A Usable Native Display

Local application development requires an interactive display allowing developers to view and engage directly with the virtual device. To enable direct interaction and bypass reliance on the WebRTC interface, we transitioned to QEMU’s native Cocoa display. The changes include:

- **BGRA Pixel Format Support**: Enables accurate display rendering to eliminate visual artifacts on Apple silicon.
- **Mouse-Click-as-Touch**: Translates pointer actions into touch input for interacting with touchscreen devices.
- **Custom Geometry Options**: Adds the -g flag to graphic\_console\_init for display configuration.

We also enabled the unstable [Rutabaga](https://github.com/magma-gpu/rutabaga_gfx) structs needed to match the Rust-side graphics ABI in [gfxstream](https://github.com/google/gfxstream) /rutabaga configurations.

### Building and Signing on Apple Silicon

Finally, there was the build and packaging work: initial Darwin build support, building QEMU for arm64 on macOS, and toolchain fixes. The macOS builds also needed the JIT entitlement in addition to support for code signing and notarization. On the Cuttlefish side, we added macOS host support so the framework’s orchestration builds and runs on a Mac instead of assuming a Linux host.

## The Benefits

By moving to a unified platform, we cut maintenance requirements, reduced infrastructure needs, eliminated redundant tooling, enhanced fidelity, and ensured developers run the exact same Cuttlefish virtual devices used across CI.

![](https://engineering.fb.com/wp-content/uploads/2026/11/Cuttlefish-on-Mac-image-3.png)

A single platform also made it easier to support capabilities on a shared stack, including:

- **Bluetooth** pairing and connectivity.
- **Wi-Fi.**
- **Cellular.**
- **3D graphics.**
- **Over-the-air (OTA) update testing** to exercise over-the-air update flows against a virtual device.
- **Fastboot.**
- **Snapshotting** to save and restore device state in CI and development flows.

Beyond supporting Reality Labs engineers, this identical virtual device is shared with external developers building for our ecosystem. This alignment guarantees that third-party teams test against the exact same Cuttlefish-based platform we use to develop and validate the OS itself.

With AI agents playing a growing role in Android development, validation execution and runtime behavior is just as essential as code generation. Virtual devices empower both engineers and automated agents to operate concurrently within isolated environments, facilitating application execution and rapid iteration. By enabling instant state resets and repeated test runs, teams eliminate dependencies on shared physical hardware and prevent workflow interruptions. Ultimately, high-fidelity emulation ensures these continuous feedback loops are both rapid and meaningful.

## Giving Back to the Community

Although we built this for development within Reality Labs, the macOS support is useful beyond our products. We’re sharing our work with the community so other developers can build on the QEMU and AOSP foundations that enabled our work.

We’re distributing the work in two parts:

- [**qemu-wearables**](https://github.com/facebookincubator/qemu-wearables)**:** a Meta-maintained fork of QEMU carrying our commits.
- [**cuttlefish-wearables**](https://github.com/facebookincubator/cuttlefish-wearables)**:** a set of patch files, with instructions for patching Android/AOSP.

The repositories contain our QEMU changes and the Cuttlefish patch series, along with build and patch-application instructions.

We’re also exploring ways to contribute these changes upstream to QEMU and AOSP. In the meantime, developers can use our fork and patch set.

## Acknowledgements

*This project relies on the foundations laid by the QEMU and Cuttlefish/AOSP communities. We are particularly grateful for the* [*vsock-user*](https://gitlab.com/sgarzarella/qemu/-/tree/macos-vhost-user) *implementation created by* [*Stefano Garzarella*](https://gitlab.com/sgarzarella)*, which directly enabled our macOS integration.*

*Special thanks also go to Olya Arkhangelskaia and Tom Care at Meta for their foundational work bringing Cuttlefish to macOS, and to Jeffrey Effendy for improvements to the native Mac developer experience and subsequent productization work.*
