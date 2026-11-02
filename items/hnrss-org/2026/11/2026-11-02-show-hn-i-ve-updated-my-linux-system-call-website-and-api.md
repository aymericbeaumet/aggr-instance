---
title: 'Show HN: I''ve updated my Linux System Call website and API'
link: https://syscall.sh/
source: hnrss-org
published: 2026-11-02T09:29:16Z
updated: 2026-11-02T09:29:16Z
first_seen: 2026-11-02T16:59:20.971254334Z
authors:
- guitmz
summary: 'I first released those in back in 2020 and now I finally added more architectures, Kernel selector, syscall descriptions and a few other niceties. It should work a bit better on mobile devices too but I am no CSS expert. The API is at https://api.syscall.sh Comments URL: https://news.ycombinator.com/item?id=50334704 Points: 159 # Comments: 45'
content: extracted
html: 2026-11-02-show-hn-i-ve-updated-my-linux-system-call-website-and-api.html
remote_preview:
  url: https://syscall.sh/og.png
  alt: The syscall(2) man page
---

## Linux syscall tables

Every Linux syscall table in one place, grouped by architecture: system calls for 27 architectures with their numbers, arguments and registers, from Linux 3.3 to 7.3.

## Architectures

- [**x64** x86\_64 369 syscalls · since 3.3](https://syscall.sh/x64/)
- [**x86** i386, 32 bit x86 441 syscalls · since 3.3](https://syscall.sh/x86/)
- [**arm64** AArch64 326 syscalls · since 3.7](https://syscall.sh/arm64/)
- [**arm** ARM EABI, 32 bit 426 syscalls · since 4.10](https://syscall.sh/arm/)
- [**riscv64** RISC-V, 64 bit 327 syscalls · since 4.15](https://syscall.sh/riscv64/)
- [**ia32** IA-32 compat on x86\_64 439 syscalls · since 3.3](https://syscall.sh/ia32/)
- [**x32** x32 ABI on x86\_64 369 syscalls · since 3.4](https://syscall.sh/x32/)
- [**loongarch64** LoongArch, 64 bit 323 syscalls · since 5.19](https://syscall.sh/loongarch64/)
- [**ppc64** PowerPC, 64 bit 383 syscalls · since 5.0](https://syscall.sh/ppc64/)
- [**ppc** PowerPC, 32 bit 425 syscalls · since 5.0](https://syscall.sh/ppc/)
- [**s390x** IBM Z 378 syscalls · since 4.16](https://syscall.sh/s390x/)
- [**mips64** MIPS n64 ABI 363 syscalls · since 5.0](https://syscall.sh/mips64/)
- [**mipsn32** MIPS n32 ABI 387 syscalls · since 5.0](https://syscall.sh/mipsn32/)
- [**mipso32** MIPS o32 ABI 413 syscalls · since 5.0](https://syscall.sh/mipso32/)
- [**alpha** DEC Alpha 403 syscalls · since 5.0](https://syscall.sh/alpha/)
- [**arc** Synopsys ARC 350 syscalls · since 3.9](https://syscall.sh/arc/)
- [**csky** C-SKY 346 syscalls · since 4.20](https://syscall.sh/csky/)
- [**hexagon** Qualcomm Hexagon 345 syscalls · since 3.7](https://syscall.sh/hexagon/)
- [**m68k** Motorola 68000 435 syscalls · since 5.0](https://syscall.sh/m68k/)
- [**microblaze** Xilinx MicroBlaze 423 syscalls · since 5.0](https://syscall.sh/microblaze/)
- [**nios2** Intel Nios II 346 syscalls · since 3.19](https://syscall.sh/nios2/)
- [**openrisc** OpenRISC 346 syscalls · since 3.7](https://syscall.sh/openrisc/)
- [**parisc** HP PA-RISC 404 syscalls · since 5.0](https://syscall.sh/parisc/)
- [**sh** SuperH 433 syscalls · since 5.0](https://syscall.sh/sh/)
- [**sparc** SPARC, 32 bit 434 syscalls · since 5.0](https://syscall.sh/sparc/)
- [**sparc64** SPARC, 64 bit 399 syscalls · since 5.0](https://syscall.sh/sparc64/)
- [**xtensa** Tensilica Xtensa 388 syscalls · since 5.0](https://syscall.sh/xtensa/)

Every architecture's table, with the full history of when each syscall appeared and when it was dropped, is also a plain JSON file at `/<arch>/syscalls.json`, for example [/x64/syscalls.json](https://syscall.sh/x64/syscalls.json).

## Calling conventions

Number in *NR*, arguments in *ARG0* to *ARG5*, then the instruction. The result comes back in *RETURN*.

| ARCH                                            | INSTRUCTION            | NR  | RETURN | ARG0 | ARG1 | ARG2 | ARG3 | ARG4  | ARG5  |
| ----------------------------------------------- | ---------------------- | --- | ------ | ---- | ---- | ---- | ---- | ----- | ----- |
| [x64](https://syscall.sh/x64/)                 | `syscall`              | rax | rax    | rdi  | rsi  | rdx  | r10  | r8    | r9    |
| [x86](https://syscall.sh/x86/)                 | `int 0x80`             | eax | eax    | ebx  | ecx  | edx  | esi  | edi   | ebp   |
| [arm64](https://syscall.sh/arm64/)             | `svc #0`               | x8  | x0     | x0   | x1   | x2   | x3   | x4    | x5    |
| [arm](https://syscall.sh/arm/)                 | `svc #0`               | r7  | r0     | r0   | r1   | r2   | r3   | r4    | r5    |
| [riscv64](https://syscall.sh/riscv64/)         | `ecall`                | a7  | a0     | a0   | a1   | a2   | a3   | a4    | a5    |
| [ia32](https://syscall.sh/ia32/)               | `int 0x80`             | eax | eax    | ebx  | ecx  | edx  | esi  | edi   | ebp   |
| [x32](https://syscall.sh/x32/)                 | `syscall`              | rax | rax    | rdi  | rsi  | rdx  | r10  | r8    | r9    |
| [loongarch64](https://syscall.sh/loongarch64/) | `syscall 0`            | a7  | a0     | a0   | a1   | a2   | a3   | a4    | a5    |
| [ppc64](https://syscall.sh/ppc64/)             | `sc`                   | r0  | r3     | r3   | r4   | r5   | r6   | r7    | r8    |
| [ppc](https://syscall.sh/ppc/)                 | `sc`                   | r0  | r3     | r3   | r4   | r5   | r6   | r7    | r8    |
| [s390x](https://syscall.sh/s390x/)             | `svc 0`                | r1  | r2     | r2   | r3   | r4   | r5   | r6    | r7    |
| [mips64](https://syscall.sh/mips64/)           | `syscall`              | v0  | v0     | a0   | a1   | a2   | a3   | a4    | a5    |
| [mipsn32](https://syscall.sh/mipsn32/)         | `syscall`              | v0  | v0     | a0   | a1   | a2   | a3   | a4    | a5    |
| [mipso32](https://syscall.sh/mipso32/)         | `syscall`              | v0  | v0     | a0   | a1   | a2   | a3   | sp+16 | sp+20 |
| [alpha](https://syscall.sh/alpha/)             | `callsys`              | v0  | v0     | a0   | a1   | a2   | a3   | a4    | a5    |
| [arc](https://syscall.sh/arc/)                 | `trap0`                | r8  | r0     | r0   | r1   | r2   | r3   | r4    | r5    |
| [csky](https://syscall.sh/csky/)               | `trap 0`               | r7  | a0     | a0   | a1   | a2   | a3   | r4    | r5    |
| [hexagon](https://syscall.sh/hexagon/)         | `trap0(#1)`            | r6  | r0     | r0   | r1   | r2   | r3   | r4    | r5    |
| [m68k](https://syscall.sh/m68k/)               | `trap #0`              | d0  | d0     | d1   | d2   | d3   | d4   | d5    | a0    |
| [microblaze](https://syscall.sh/microblaze/)   | `brki r14,8`           | r12 | r3     | r5   | r6   | r7   | r8   | r9    | r10   |
| [nios2](https://syscall.sh/nios2/)             | `trap`                 | r2  | r2     | r4   | r5   | r6   | r7   | r8    | r9    |
| [openrisc](https://syscall.sh/openrisc/)       | `l.sys 1`              | r11 | r11    | r3   | r4   | r5   | r6   | r7    | r8    |
| [parisc](https://syscall.sh/parisc/)           | `ble 0x100(%sr2, %r0)` | r20 | r28    | r26  | r25  | r24  | r23  | r22   | r21   |
| [sh](https://syscall.sh/sh/)                   | `trapa #31`            | r3  | r0     | r4   | r5   | r6   | r7   | r0    | r1    |
| [sparc](https://syscall.sh/sparc/)             | `t 0x10`               | g1  | o0     | o0   | o1   | o2   | o3   | o4    | o5    |
| [sparc64](https://syscall.sh/sparc64/)         | `t 0x6d`               | g1  | o0     | o0   | o1   | o2   | o3   | o4    | o5    |
| [xtensa](https://syscall.sh/xtensa/)           | `syscall`              | a2  | a2     | a6   | a3   | a4   | a5   | a8    | a9    |

## API

```
curl https://api.syscall.sh/v2/syscalls/x64/read
```

Any kernel release, diffs between releases and more in the [API docs](https://api.syscall.sh/docs).

## Syscall tables across kernel versions

Entries get added and, rarely, removed between releases. Each architecture page has a kernel selector, and its JSON download records when each entry first appeared (`since`) and when it was dropped (`removed_in`).

Removed entries stay visible: pick "all, with removed" in the kernel selector and each dropped syscall is marked with the release that dropped it.

## Where the data comes from

Everything on this site is read from the kernel source itself. For each architecture, the generator walks the syscall table and the `SYSCALL_DEFINE` macros of every mainline release it scans, so these are the numbers the kernel actually dispatches on.

It also records each architecture's calling convention, the instruction that enters the kernel, so every syscall table shows exactly where the number and the arguments go.

A syscall whose `since` equals the first release scanned for its architecture was already there back then, and may be much older.

### Descriptions and full history

The short descriptions come from the [Linux man pages project](https://www.kernel.org/doc/man-pages/), and the `io_uring` ones from [liburing](https://github.com/axboe/liburing). Whenever the data changes, the site is rebuilt from the [syscall API](https://api.syscall.sh/docs).
