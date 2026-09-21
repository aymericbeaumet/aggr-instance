---
title: Amiga Unix, Again
link: https://amigaux.org/
source: hnrss-org-frontpage
published: 2026-09-20T23:57:10Z
updated: 2026-09-20T23:57:10Z
first_seen: 2026-09-21T11:04:21.981356257Z
authors:
- doener
summary: 'Article URL: https://amigaux.org/ Comments URL: https://news.ycombinator.com/item?id=49781436 Points: 108 # Comments: 37'
content: extracted
html: 2026-09-20-amiga-unix-again.html
preview:
  file: 2026-09-20-amiga-unix-again.preview-e981165ab0fb.webp
  width: 256
  height: 85
  alt: Amiga Unix — 68040 · 68060
  color: '#574e4e'
images:
- source: https://amigaux.org/img/amiga-unix-boxcover.svg
  original:
    file: 2026-09-20-amiga-unix-again.image-6b11eb3a6543.png
    width: 636
    height: 212
  variants:
  - file: 2026-09-20-amiga-unix-again.image-10c7f9e9504d.webp
    width: 320
    height: 107
  - file: 2026-09-20-amiga-unix-again.image-909540daae5d.webp
    width: 636
    height: 212
  color: '#f7f7f7'
- source: https://amigaux.org/video/install.gif
  original:
    file: 2026-09-20-amiga-unix-again.image-22660bf5301c.gif
    width: 720
    height: 568
  color: '#111111'
- source: https://amigaux.org/video/first-boot.gif
  original:
    file: 2026-09-20-amiga-unix-again.image-17168a9b6dc0.gif
    width: 720
    height: 568
  color: '#111111'
---

![Amiga Unix — 68040 · 68060](https://amigaux.org/img/amiga-unix-boxcover.svg)

Year of the Amiga Unix Desktop 2026

## Amiga Unix, again

Amiga Unix — Amix — was Commodore's System V Release 4 for the Amiga: shipped in 1990–92 for the A2500UX and A3000UX, then left where it stood. amigaux.org is an unofficial community project that picks it up again: Amix on 68040 and 68060 machines and on today's accelerator hardware, with a modern toolchain, a package manager, and drivers for cards that never had any. The work is done in the open, and written down as it happens in the [grimoire](https://jusii.github.io/grimoire-amix/).

### Soon: install it yourself

The install medium boots from floppy and CD-ROM on real hardware — floppy image and ISO under emulation — and takes care of the whole installation, interactively or unattended. With supported hardware, getting Amix onto a machine comes down to following the prompts. The classic tape install follows later.

From there the network takes over: apkg installs packages straight from **pkg.amigaux.org** — grep, gzip, less, patch and zlib to start with, and more GNU and BSD tools as they are built and tested.

\# apkg update catalog updated: 39 packages \# apkg install less downloading extra/less-704.pkg ... installed less-704

![Installing Amiga Unix on a 68060: the installer asks for keymap, disk, sizes and package set, then summarises what it will install.](https://amigaux.org/video/install.gif) \
**Installing on a real 68060.** Keymap, disk, partition sizes, package set — then it shows you what it is about to do. Real time, with the long waits taken out.

![First boot: Amiga Unix asks for a nodename and domain, writes its hosts file and comes up at a login prompt.](https://amigaux.org/video/first-boot.gif) \
**First boot.** It names itself, sets the clock, writes its hosts file — including the package repository — and comes up at a login prompt.

## Where we are

### Working today

- **68040/68060 support** — Amix 2.1 on a real 68060, with or without FPU; an MMU is mandatory. The kernel: [asokero/amix-040-060-port](https://github.com/asokero/amix-040-060-port)
- **Z3660 accelerator** — native SCSI and ethernet drivers, proven on real hardware
- **A4091 / A4092** — a Zorro III SCSI driver and an auto-detecting kernel
- **Installation** — from floppy + CD-ROM on a real machine, or floppy image + ISO image under emulation, interactive or unattended; the classic tape install follows later
- **apkg** — a remote package client with a hosted repository (catalog, dependencies, upgrades), and the first modern GNU tools packaged: grep, gzip, less, patch, zlib
- **A cross toolchain** — Linux-hosted `m68k-cbm-sysv4`, building all of the above
- **An OpenLook desktop** that comes up ready on a fresh install
- **Quake** — runs; a benchmark more than a game, for now

### In progress

- RTG graphics for the Z3660 (ZZ9000-compatible) — running under emulation, real hardware next
- Ethernet throughput in the accelerator firmware
- X11R6.3 and Mesa as packages, from the community ports; the wider userland after that
- A read-only CD filesystem (ODFileSystem port)
- Install-media polish: a repair path, guard rails; the tape version

### Next

- A native, modern gcc on the box
- RTG on the real accelerator
- The packaged X11R6.3 / Mesa stack
- The firmware ethernet fixes
- Then the full launch here: packages, source, instructions and manuals

## How it's built

Part of this work is done with generative AI in the loop — models reading the old kernels in binary, as no source is available, and writing drivers and notes — with people setting direction, reviewing every change and testing on real hardware, where it either boots or it doesn't. Other parts are done the classic way. The grimoire records both, confidence-tagged, so you can see what is verified and what is still a guess.

## The code

### The kernel

- [asokero/amix-040-060-port](https://github.com/asokero/amix-040-060-port) — Amix 2.1 kernel for the 68040 and 68060
- [asokero/amix-unix-boot](https://github.com/asokero/amix-unix-boot) — patches that make Markus Wild's `unix_boot` load an Amix kernel on a 68040 or 68060

### Drivers and hardware

- [jusii/amix-z3660scsi](https://github.com/jusii/amix-z3660scsi) — SCSI driver for the Z3660's PiStorm piscsi mailbox
- [jusii/amix-z3660net](https://github.com/jusii/amix-z3660net) — STREAMS/DLPI ethernet driver for the Z3660
- [jusii/Z3660-amix](https://github.com/jusii/Z3660-amix) — Amix-aware fork of the Z3660 firmware
- [jusii/amix-a4091](https://github.com/jusii/amix-a4091) — SCSI driver for the Commodore A4091/A4092 (NCR 53C710)
- [isoriano1968/zz9000-amix](https://github.com/isoriano1968/zz9000-amix) — ZZ9000 RTG driver
- [isoriano1968/hydra-amix](https://github.com/isoriano1968/hydra-amix) — Hydra ethernet driver
- [asokero/va2000-amix](https://github.com/asokero/va2000-amix) — MNT VA2000 RTG driver

### Graphics and X11

- [asokero/xrtg-amix](https://github.com/asokero/xrtg-amix) — Xrtg, an X11R5 RTG server
- [isoriano1968/x11r6.3-amix](https://github.com/isoriano1968/x11r6.3-amix) — X11R6.3 for Amix
- [isoriano1968/mesa-amix](https://github.com/isoriano1968/mesa-amix) — Mesa for Amix

### Built on

- [shanshe/Z3660](https://github.com/shanshe/Z3660) — the Z3660 accelerator firmware
- [reinauer/ODFileSystem](https://github.com/reinauer/ODFileSystem) — the CD filesystem we are porting
- [BlitterStudio/amiberry](https://github.com/BlitterStudio/amiberry) — the emulator on our bench (our fork: [jusii/amiberry](https://github.com/jusii/amiberry))

### Tools and docs

- [isoriano1968/gcc-cross-amix](https://github.com/isoriano1968/gcc-cross-amix) — the `m68k-cbm-sysv4` cross toolchain
- [jusii/amix-kerntools](https://github.com/jusii/amix-kerntools) — kernel build hub: patches your own Amix tree, driver.conf-driven
- [jusii/grimoire-amix](https://github.com/jusii/grimoire-amix) — the documentation, as a repo
- [asokero/quake-amix](https://github.com/asokero/quake-amix) — Quake for Amix
