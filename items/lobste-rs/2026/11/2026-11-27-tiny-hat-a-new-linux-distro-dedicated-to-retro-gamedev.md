---
title: 'Tiny Hat: a new Linux distro dedicated to retro gamedev'
link: https://roboticoverlords.org/tinyhat
source: lobste-rs
published: 2026-11-27T09:11:13Z
updated: 2026-11-27T09:11:13Z
first_seen: 2026-11-28T14:03:39.806524745Z
authors:
- roboticoverlords.org by xyproto
labels:
- games
- linux
- retrocomputing
summary: Tiny Hat uses only around 100MB of RAM after booting, and can be booted from a >= 1GB memory stick. It comes with a few small games, an easy way to install and run more games and several example programs for getting into retro game development. It also uses a modern stack of libraries, with dwl / Wayland, SDL3 and GTK4. One possible use is to buy a cheap x86 machine for younger members of a family, to completely brain wash inspire them into getting into graphics programming and appreciating retro games, perhaps as a Christmas present. At least a few people have found it fun so far, so I wanted to share it. Comments
content: extracted
html: 2026-11-27-tiny-hat-a-new-linux-distro-dedicated-to-retro-gamedev.html
remote_preview:
  url: https://roboticoverlords.org/tinyhat/tinyhat.svg
  alt: Tiny Hat logo
---

![Tiny Hat logo](https://roboticoverlords.org/tinyhat/tinyhat.svg)

- A Linux distro for 32-bit and 64-bit x86 computers.
- Intended for retro gaming and retro game development (DOS, GBA, SNES, NES, TIC-80, CHIP-8, C64 and ZX Spectrum).
- The image is signed and fully reproducible.
- Uses less than 100M RAM after booting (87M on the 32-bit version, right over 100M on the 64-bit version).
- Uses just a bit more than 600M disk space, and is fully usable on a 1G memory stick.
- Works better than MS-DOS and FreeDOS for playing games on the old x86 retro gaming computer I created the distro for (because of hardware support).
- Comes with modern libraries, like SDL3, Pipewire and Raylib. Uses dwl and Wayland.
- Comes with the Orbiton editor and an easy way to compile and run C programs, DOS Assembly programs, GBA programs and also LÖVE+Fennel programs.
- Some applications that comes with the i686 version requires SSE right now, but that will be fixed in the next release.
- There is support for aarch64 and rpi4, but it is experimental.

Release page: [https://github.com/xyproto/tinyhat/releases](https://github.com/xyproto/tinyhat/releases)
