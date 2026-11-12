---
title: A live tree of systemd jobs on the boot console
link: https://codeberg.org/wvhulle/bootvine
source: hnrss-org
published: 2026-11-09T13:02:44Z
updated: 2026-11-09T13:02:44Z
first_seen: 2026-11-12T18:49:11.379952736Z
authors:
- orhunp_
content: extracted
html: 2026-11-09-a-live-tree-of-systemd-jobs-on-the-boot-console.html
remote_preview:
  url: https://codeberg.org/wvhulle/bootvine/-/summary-card
  alt: 'Summary card of repository wvhulle/bootvine, described as: A boot animation showing stages of systemd as a tree'
---

A live tree of systemd jobs on the boot console, in the spirit of [nix-output-monitor](https://github.com/maralorn/nix-output-monitor). It replaces the scrolling kernel and systemd lines with the units still pending and, in the room the tree leaves, a timeline of the slowest ones that ran.

I started bootvine out of frustration with the boot screen of most Linux distributions: a wall of log lines scrolling past too fast to read, which never gave me any idea of what was going on.

![A boot through the initrd into stage 2, with one failed unit](https://codeberg.org/wvhulle/bootvine/raw/branch/recordings/boot.gif)

It also follows shutdowns, answers passphrase prompts such as a [LUKS](https://gitlab.com/cryptsetup/cryptsetup) unlock in place, and shows why a unit failed with its last journal lines. The [guide](https://codeberg.org/wvhulle/bootvine/src/branch/main/docs/guide.md) walks through each part of the screen.

## Requirements

- systemd ≥ 246 as PID 1
- [libdbus](https://www.freedesktop.org/wiki/Software/dbus/) and libsystemd
- a virtual terminal
- a systemd initrd, to show the boot from the initrd as well as stage 2

See [Install](https://codeberg.org/wvhulle/bootvine/src/branch/main/docs/install.md) for each distribution.

## Try it without installing

With [Nix](https://nixos.org/download/), boot a sample [NixOS](https://nixos.org) machine with slow and failing units in a [QEMU](https://www.qemu.org) window, without touching your system's boot:

```sh
nix run git+https://codeberg.org/wvhulle/bootvine#sample-vm-slow-services
```

It logs in as root; `poweroff` shows the shutdown. The [guide](https://codeberg.org/wvhulle/bootvine/src/branch/main/docs/guide.md#trying-it) lists the other sample machines and the scripted demos that play in a terminal.

## Documentation

- [Guide](https://codeberg.org/wvhulle/bootvine/src/branch/main/docs/guide.md) to the screen: the tree, the timeline, failures, shutdown and passphrase prompts.
- [Install](https://codeberg.org/wvhulle/bootvine/src/branch/main/docs/install.md) on NixOS, Ubuntu and other distributions.
- [Configuration](https://codeberg.org/wvhulle/bootvine/src/branch/main/docs/configuration.md) of colours, timing and the console.
- [Architecture](https://codeberg.org/wvhulle/bootvine/src/branch/main/docs/architecture.md): how bootvine follows the boot and takes the console.
- [Development](https://codeberg.org/wvhulle/bootvine/src/branch/main/docs/development.md) and the [code style](https://codeberg.org/wvhulle/bootvine/src/branch/main/docs/style.md).

## License

[GPL-3.0-or-later](https://codeberg.org/wvhulle/bootvine/src/branch/main/LICENSE).
