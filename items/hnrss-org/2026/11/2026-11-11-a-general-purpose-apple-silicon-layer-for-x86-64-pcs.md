---
title: A general purpose Apple Silicon layer for x86-64 PCs
link: https://github.com/tylertms/Stela
source: hnrss-org
published: 2026-11-11T23:02:38Z
updated: 2026-11-11T23:02:38Z
first_seen: 2026-11-12T10:54:03.804437502Z
authors:
- iceboundrock
content: extracted
html: 2026-11-11-a-general-purpose-apple-silicon-layer-for-x86-64-pcs.html
remote_preview:
  url: https://opengraph.githubassets.com/2b66e1b7138d1c82fb25963e8be9290b8d8c384a7c64fa1456865628e1a1fcbb/tylertms/Stela
  alt: A general purpose Apple Silicon layer for x86-64 PCs - tylertms/Stela
---

A general purpose Apple Silicon layer for x86-64 PCs. It is a UEFI boot program with an arm64-to-x86-64 translator, a Metal driver for NVIDIA GPUs and a model of Apple silicon hardware. macOS and most apps start, but it is slow and needs much more testing, as of v0.1.0

Warning

Stela is very much still in development. Your current chances of it booting are low, major hardware support/fixes are on the way.

## Translator

An ahead-of-time and just-in-time compiler translates arm64e code to x86-64. It covers more than 3,000 AArch64 instruction encodings, maps guest registers to host registers and caches translated code. It runs on any x86-64-v2 CPU, Intel or AMD.

## NVIDIA driver

The Metal driver supports GTX 16 and RTX 20 series GPUs and newer. It compiles AIR, Metal's shader IR, directly to NIR, Mesa's IR, and supports the Metal3 and Apple7 GPU families.

## Requirements

- An x86-64-v2 CPU (about 2011 or newer), UEFI with Secure Boot off, 8 GB of RAM.
- An NVIDIA GTX 16 or RTX 20 series GPU or newer.
- A USB stick of 8 GB or more.
- An empty GPT partition of at least 60 GB.
- Network access during the install, which downloads macOS.

Stela currently models the Mac mini (M2) and runs macOS 27.

## Install

1. Download `stela` from the [releases page](https://github.com/tylertms/Stela/releases).
2. Run `sudo ./stela setup` (Linux, macOS) or double-click `stela.exe` (Windows).
3. Choose the USB stick, the macOS version and the Mac model, then confirm. Setup erases the USB stick.
4. Boot the PC from the USB stick.

To update a USB stick made with an older version, download the new `stela`, plug the stick in and run `sudo ./stela setup update stela` (Linux, macOS) or `stela.exe setup update stela` (Windows).

## Build

You need [Rust](https://rustup.rs) and LLVM with Clang 21 or newer.

```shell
git clone https://github.com/tylertms/Stela
cd Stela
cargo xtask dist
```

This writes `target/release/stela`. The first build fetches and compiles Linux, Mesa and LLVM sources and takes a long time.

## Legal

Stela contains no Apple software. You are responsible for following Apple's software license agreement and terms of use.

Stela's code is GPL-2.0-or-later. See [LICENSE](https://github.com/tylertms/Stela/blob/main/LICENSE) and [NOTICE](https://github.com/tylertms/Stela/blob/main/NOTICE).

## Community

[Discord Server](https://discord.gg/aSkwrJEMYc)
