---
title: Bare Metal Qwen
link: https://github.com/alesha-pro/llm.asm/tree/main/02-baremetal-os
source: hnrss-org
published: 2026-11-25T22:15:34Z
updated: 2026-11-25T22:15:34Z
first_seen: 2026-11-26T12:13:26.788658858Z
authors:
- miohtama
content: extracted
html: 2026-11-25-bare-metal-qwen.html
remote_preview:
  url: https://opengraph.githubassets.com/d94115cdfce79235ecc6c6a9ba6df3c7a266789703e204f8492171d598ad8387/alesha-pro/llm.asm
  alt: Qwen3-0.6B inference (12 KB), bare-metal bootable OS (22 KB), and from-scratch Mini-GPT trainer with live Braille TUI (32 KB) in 100% pure x86-64 NASM assembly. Zero C, zero libc, zero dependencies...
---

## Phase 2: Bare-Metal Bootable Qwen3-0.6B OS in 24 KB of Pure Assembly

A standalone bootable x86-64 operating system that runs **Qwen/Qwen3-0.6B** directly on hardware or KVM with **no Linux, no Windows, no C, no Rust, and no libc**.

- **24,252 bytes** stripped static Multiboot 1 kernel (`FAST_Q8=1`, default: `.text` = 17,143 B, `.rodata` = 2,772 B, `.bss` = 2.1 MiB `NOBITS`, zero dynamic dependencies), or **23,612 bytes** (`FAST_Q8=0`, legacy Q8\_0xFP32).
- **74–94 ms cold boot** to interactive `asm-llm>` multi-turn chat with persistent KV-cache across turns, all SMP cores online, and the `.asmbin` v2 (`614 MiB` FP16 scales) or v1 (`649 MiB` FP32 scales) model bound zero-copy from physical RAM on 2 MiB huge pages.
- **100% token-exact and byte-for-byte parity** with [Phase 1](https://github.com/alesha-pro/llm.asm/blob/main/01-qwen3-inference) across `1..32` vCPUs.
- **134.9 tok/s** decode on QEMU KVM (AMD EPYC 7642, 16 vCPUs; **23.7 tok/s** at 1 vCPU, **118.1 tok/s** at 8 vCPUs, **136.6 tok/s** peak) and **55.4 tok/s** live on a **0 GB disk Proxmox VE VM** (Intel Xeon E5-2670 v3, 12 cores).

[![Bare-Metal Qwen3-0.6B OS VGA Console](https://github.com/alesha-pro/llm.asm/raw/main/assets/baremetal-vga.png)](https://github.com/alesha-pro/llm.asm/blob/main/assets/baremetal-vga.png)

*Running live on a diskless (`0 GB` storage) Proxmox VE VM booted from the hybrid BIOS+UEFI ISO (Intel Xeon E5-2670 v3, 12 cores, 2 GiB RAM):*

[![Running on Real Proxmox VE VM (0 GB Disk, Intel Xeon KVM)](https://github.com/alesha-pro/llm.asm/raw/main/assets/proxmox-vm250-vga.png)](https://github.com/alesha-pro/llm.asm/blob/main/assets/proxmox-vm250-vga.png)

* * *

## Prerequisites

- x86-64 CPU with **AVX2 + FMA + F16C**.
- Core build & QEMU KVM:

  ```shell
  sudo apt install nasm binutils qemu-system-x86
  ```

- Optional, for building a bootable BIOS+UEFI hybrid ISO (`make os-iso`):

  ```shell
  sudo apt install grub-pc-bin grub-efi-amd64-bin grub-common mtools xorriso
  ```

- An exported `models/qwen3-0.6b-q8_0.asmbin` file (`--scale-dtype fp16` v2 recommended, or legacy v1 `--scale-dtype fp32`; see [Phase 1 setup](https://github.com/alesha-pro/llm.asm/blob/main/01-qwen3-inference/README.md#1-download-qwenqwen3-06b)).

* * *

## Build

From the repository root (`FAST_Q8=1` is enabled by default; pass `FAST_Q8=0` for legacy FP32-activation mode):

```shell
make os FAST_Q8=1
make os-check
```

Expected output (`FAST_Q8=1`):

```
-rwxr-xr-x 1 user user 24252 build/asm-llm-os.elf
build/asm-llm-os.elf: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), statically linked, stripped
There is no dynamic section in this file.
```

*(The kernel is linked as 64-bit ELF and wrapped into ELF32 via `objcopy -O elf32-i386` for standard Multiboot 1 loaders like QEMU `-kernel` and GRUB2; `boot.asm` transitions immediately into 64-bit Long Mode.)*

* * *

## Running & Deploying

### Option A: Instant QEMU KVM Boot (~85 ms)

Boot directly in the terminal over COM1 serial (`-nographic`):

```shell
qemu-system-x86_64 -enable-kvm -cpu host -m 2G -smp 16 \
  -kernel build/asm-llm-os.elf \
  -initrd models/qwen3-0.6b-q8_0.asmbin \
  -nographic
```

Or use the Makefile shortcut:

```shell
make os-run MODEL=models/qwen3-0.6b-q8_0.asmbin SMP=16
```

Run a one-shot batch prompt with clean ACPI shutdown:

```shell
make os-run MODEL=models/qwen3-0.6b-q8_0.asmbin SMP=16 \
  CMDLINE='prompt="Explain how a CPU cache works in 3 concise sentences." max_tokens=64 think=0 shutdown=1'
```

Open a live **80x25 VGA GUI window** with PS/2 keyboard input:

```shell
qemu-system-x86_64 -enable-kvm -cpu host -m 2G -smp 16 \
  -kernel build/asm-llm-os.elf \
  -initrd models/qwen3-0.6b-q8_0.asmbin \
  -vga std -serial stdio
```

### Option B: Bootable Hybrid BIOS+UEFI ISO

Packages the 23 KB kernel, GRUB2 Multiboot 1 config, and the 649 MiB `.asmbin` model into a single self-contained `~661 MiB` bootable ISO (`build/asm-llm-os.iso`):

```shell
make os-iso MODEL=models/qwen3-0.6b-q8_0.asmbin
```

Boot the ISO in QEMU KVM (SeaBIOS or UEFI OVMF):

```shell
# Legacy BIOS (SeaBIOS)
qemu-system-x86_64 -enable-kvm -cpu host -m 2G -smp 16 \
  -cdrom build/asm-llm-os.iso -nographic

# UEFI (OVMF)
qemu-system-x86_64 -enable-kvm -cpu host -m 2G -smp 16 \
  -bios /usr/share/ovmf/OVMF.fd \
  -cdrom build/asm-llm-os.iso -nographic
```

### Option C: Deploying on Proxmox VE (0 GB Disk VM)

Because the entire OS and model live inside the ISO and run out of RAM, the VM needs **zero hard disks**.

1. Upload `build/asm-llm-os.iso` to your Proxmox node's ISO storage (`local` -> **ISO Images** -> **Upload**, or `scp` into `/var/lib/vz/template/iso/asm-llm-os.iso`).
2. Create and start a diskless VM on the Proxmox host (pick an unused `<vmid>`, e.g. `250`):

```shell
qm create 250 \
  --name asm-llm-os \
  --machine pc \
  --bios seabios \
  --cpu host \
  --cores 12 \
  --memory 2048 \
  --balloon 0 \
  --vga std \
  --serial0 socket \
  --ide2 local:iso/asm-llm-os.iso,media=cdrom \
  --boot order=ide2

qm start 250
```

3. Interact either through the Proxmox web UI (**noVNC Console**, VGA + PS/2 keyboard) or over the virtual serial port:

```shell
qm terminal 250
```

*(Note: `--cpu host` is required so the VM exposes AVX2 + FMA, and `--balloon 0` ensures 2 GiB of contiguous guest RAM for the 649 MiB model + 449 MiB KV-cache arena.)*

* * *

## Interactive Multi-Turn Chat & Kernel Cmdline

### Interactive Console (`asm-llm>`)

By default, the bare-metal console runs in **multi-turn chat mode with persistent KV-cache**: each new user turn prefills only the new Qwen3 chat tokens starting at `g_chat_pos` without recomputing earlier turns, closes every assistant turn in the KV-cache with `<|im_end|>\n`, and displays live context usage (`ctx=X/2048`) in the decode stats line. When the KV-cache approaches `max_seq` (`pos + n_prompt + 64 >= max_seq`), it prints `[chat] context full, resetting KV-cache` and resets cleanly. If booted with `prompt="..."` and `shutdown=0`, the initial batch turn stays in the KV-cache so you can continue the same conversation in the REPL.

Both PS/2 keyboard (VGA) and COM1 serial (`115200 8N1`) are polled simultaneously. Type any message and press Enter, or use a built-in command:

| Command                    | Action                                                              |
| -------------------------- | ------------------------------------------------------------------- |
| `/clear`, `/new`, `/reset` | Clear conversation history (`g_chat_pos = 0`, prints `ctx=0/2048`)  |
| `/think`                   | Enable Qwen3 `<think>...</think>` reasoning mode                    |
| `/nothink`                 | Disable reasoning mode (direct answers, default)                    |
| `/bench`                   | Reset KV-cache and run built-in latency/throughput benchmark prompt |
| `/halt` or `/quit`         | Trigger clean ACPI S5 poweroff                                      |
| `/help`                    | Show available console commands                                     |

### Multiboot Kernel Cmdline Options

Pass via QEMU `-append "..."`, `make os-run CMDLINE='...'`, or `make os-iso CMDLINE='...'`:

| Option             | Default  | Description                                                                       |
| ------------------ | -------- | --------------------------------------------------------------------------------- |
| `prompt="..."`     | *(none)* | Run a batch prompt immediately after boot (preserved in KV-cache if `shutdown=0`) |
| `max_tokens=N`     | `128`    | Maximum tokens to generate per prompt                                             |
| `ctx=N`            | `2048`   | KV-cache context length                                                           |
| `threads=N`        | `0`      | Limit active SMP cores (`0` = all online cores)                                   |
| `think=0&#124;1`   | `0`      | Enable or disable Qwen3 reasoning mode                                            |
| `raw=0&#124;1`     | `0`      | Skip Qwen3 chat template                                                          |
| `single_turn=0&#124;1` | `0`      | Disable multi-turn KV-cache retention (`0` = multi-turn chat by default)          |
| `temp=F`           | `0.0`    | Sampling temperature (`0.0` = greedy)                                             |
| `topp=F`           | `0.95`   | Top-p threshold                                                                   |
| `seed=N`           | `42`     | PRNG seed                                                                         |
| `print_tokens=0&#124;1` | `0`      | Print `PROMPT_TOKENS:` and `GEN_TOKENS:` markers                                  |
| `shutdown=0&#124;1` | `0`      | Power off via ACPI after batch prompt completes                                   |

* * *

## Architecture Overview

Phase 2 compiles the exact same `shared/math.asm`, `shared/matmul.asm`, `shared/tokenizer.asm`, and `shared/transformer.asm` with `-DBARE_METAL -DFAST_Q8=$(FAST_Q8)` and pairs them with 4,167 lines of bare-metal OS code:

- **Boot & Long Mode (`boot.asm`, `linker.ld`):** verifies Multiboot 1 magic (`0x2BADB002`), zeroes `.bss`, builds 4-level 2 MiB huge-page identity tables covering `0..4 GiB` (with `0xFE000000..0xFFFFFFFF` marked Strong Uncacheable for Local APIC MMIO), enters 64-bit Long Mode, installs a 32-entry exception IDT, verifies `XSAVE + AVX + AVX2 + FMA + F16C` via CPUID, and initializes `CR0`/`CR4`/`XCR0=7`/`MXCSR`.
- **Zero-Copy Weights & Physical Arena (`runtime.asm`):** binds the `.asmbin` (v2 FP16 scales or v1 FP32 scales) Multiboot module directly at its physical address (validating `"ASML"` magic) and carves the 449 MiB KV-cache + scratch arena from the largest usable Multiboot memory-map region, zeroing it with AVX2.
- **Bare-Metal SMP (`smp.asm`):** parses ACPI RSDP -> RSDT/XSDT -> MADT (`"APIC"`) with a Local APIC broadcast fallback, installs a position-independent 16->32->64-bit trampoline at `0x8000`, wakes all AP cores via Local APIC `INIT-SIPI-SIPI`, enables AVX2/FMA/MXCSR identically on every core, and dispatches both `matmul_q8_0` and `gqa_attn_range` (`FAST_Q8=1`) with a lock-free cache-line-isolated spin-barrier.
- **Hardware Drivers (`drivers.asm`):**
  - **COM1 Serial (`0x3F8`, 115200 8N1):** raw UTF-8 TX and RX.
  - **80x25 Color VGA (`0xB8000`):** colored boot banner, hardware scrolling, batched CRTC cursor updates, and UTF-8 decoding with Cyrillic-to-ASCII fallback on VGA while streaming raw UTF-8 to COM1.
  - **PS/2 Keyboard (`0x60`/`0x64`):** scancode set 1 to ASCII (Shift, Backspace, Enter).
  - **PIT Channel 2 + RDTSC:** 20 ms hardware PIT calibration for accurate nanosecond boot/prefill/decode profiling.
  - **ACPI Poweroff:** QEMU/Bochs ACPI S5 ports (`0x604`, `0xB004`, `0x4004`) plus `isa-debug-exit` fallback.

* * *

## Verification & Benchmarks

### Automated Verification Suite

```shell
python3 scripts/verify_os.py --model models/qwen3-0.6b-q8_0.asmbin --run-dir runs/os-verify
```

All 5 parity prompts (English technical, English concise, Russian UTF-8, arithmetic word problem, `--think` mode) match Phase 1 (`build/asm-llm`) **100% exact** in prompt tokens, generated tokens, and raw UTF-8 bytes.

### KVM SMP Scaling (`FAST_Q8=1`, `.asmbin` v2 FP16 Scales, AMD EPYC 7642, 34 prompt tok, 64 gen tok, greedy)

| vCPUs (KVM) | SMP Online | Boot Ready (ms) | Boot -> 1st Tok (ms) | Prefill (ms) | Prefill (`tok/s`) | Decode (ms) | Decode (`tok/s`)           | Parity vs Phase 1 |
| ----------- | ---------- | --------------- | -------------------- | ------------ | ----------------- | ----------- | -------------------------- | ----------------- |
| 1           | 1/1        | 73.47           | 1161.48              | 1071.98      | 31.72             | 2685.04     | **23.46** *(peak 23.67)*   | **Exact (64/64)** |
| 4           | 4/4        | 84.10           | 425.11               | 324.82       | 104.67            | 805.95      | **78.17** *(peak 78.89)*   | **Exact (64/64)** |
| 8           | 8/8        | 84.54           | 316.71               | 215.93       | 157.46            | 529.53      | **118.97**                 | **Exact (64/64)** |
| **16**      | **16/16**  | **84.88**       | **312.87**           | **211.89**   | **160.46**        | **466.88**  | **134.94** *(peak 136.63)* | **Exact (64/64)** |
| 24          | 24/24      | 94.27           | 288.75               | 178.32       | **190.67**        | 473.44      | **133.07**                 | **Exact (64/64)** |
| 32          | 32/32      | 89.84           | 313.82               | 203.53       | 167.05            | 475.72      | **132.43** *(peak 133.75)* | **Exact (64/64)** |

*(Because `boot.asm` identity-maps guest RAM with 2 MiB huge pages, the bare-metal kernel avoids 4 KiB page-table walks across the 614 MiB model and outperforms both Linux `mmap()` Phase 1 and `llama.cpp` CPU `Q8_0` at every thread count.)*

### Live Proxmox VE VM (Intel Xeon E5-2670 v3, 12 Cores, 2 GiB RAM, 0 GB Disk, BIOS GRUB ISO)

| Prompt                              | Prompt Tokens | Generated Tokens | Prefill                     | Decode                       |
| ----------------------------------- | ------------- | ---------------- | --------------------------- | ---------------------------- |
| CPU cache explanation (3 sentences) | 25            | 52               | 286.20 ms (**87.35 tok/s**) | 920.15 ms (**55.43 tok/s**)  |
| Alice & Bob apples word problem     | 39            | 96               | 446.86 ms (**87.28 tok/s**) | 1770.49 ms (**53.66 tok/s**) |
