---
title: JetKVM Mini
link: https://jetkvm.com/blog/introducing-jetkvm-mini
source: hnrss-org-frontpage
published: 2026-09-13T07:49:46Z
updated: 2026-09-13T07:49:46Z
first_seen: 2026-09-13T12:42:01.306673055Z
authors:
- taubek
summary: 'Article URL: https://jetkvm.com/blog/introducing-jetkvm-mini Comments URL: https://news.ycombinator.com/item?id=49681152 Points: 212 # Comments: 83'
content: extracted
html: 2026-09-13-jetkvm-mini.html
preview:
  file: 2026-09-13-jetkvm-mini.preview-c2d13e8233e0.webp
  width: 256
  height: 134
  color: '#d7d8da'
images:
- source: https://jetkvm.com/blog/jetkvm-mini/og-v2.jpg
  original:
    file: 2026-09-13-jetkvm-mini.image-03119187bdab.jpg
    width: 1200
    height: 630
  variants:
  - file: 2026-09-13-jetkvm-mini.image-1820c6fbc379.webp
    width: 48
    height: 25
  color: '#f9f9f9'
- source: https://jetkvm.com/blog/jetkvm-mini/angle.webp
  original:
    file: 2026-09-13-jetkvm-mini.image-4d93610e3d3d.webp
    width: 1959
    height: 1545
  variants:
  - file: 2026-09-13-jetkvm-mini.image-31e0f62f32d9.webp
    width: 48
    height: 38
  - file: 2026-09-13-jetkvm-mini.image-653d6026993d.webp
    width: 320
    height: 252
  color: '#28292a'
- source: https://jetkvm.com/blog/jetkvm-mini/back-ethernet.webp
  original:
    file: 2026-09-13-jetkvm-mini.image-9dad9ef7cfa1.webp
    width: 1600
    height: 983
  variants:
  - file: 2026-09-13-jetkvm-mini.image-bf5a1a41d9c6.webp
    width: 48
    height: 29
  - file: 2026-09-13-jetkvm-mini.image-9872e94c885d.webp
    width: 320
    height: 197
  color: '#555758'
- source: https://jetkvm.com/blog/jetkvm-mini/back-wifi.webp
  original:
    file: 2026-09-13-jetkvm-mini.image-a6f1737d8c3b.webp
    width: 1600
    height: 957
  variants:
  - file: 2026-09-13-jetkvm-mini.image-26dfaeed0357.webp
    width: 48
    height: 29
  - file: 2026-09-13-jetkvm-mini.image-64f87a648cf6.webp
    width: 320
    height: 191
  color: '#77797a'
- source: https://jetkvm.com/blog/jetkvm-mini/front.webp
  original:
    file: 2026-09-13-jetkvm-mini.image-23d1c2dad6d0.webp
    width: 2000
    height: 1502
  variants:
  - file: 2026-09-13-jetkvm-mini.image-3aad130349c6.webp
    width: 48
    height: 36
  - file: 2026-09-13-jetkvm-mini.image-3f31991a3f83.webp
    width: 320
    height: 240
  - file: 2026-09-13-jetkvm-mini.image-8476dbff55f5.webp
    width: 640
    height: 481
  color: '#737576'
- source: https://jetkvm.com/blog/jetkvm-mini/bios-ui.webp
  original:
    file: 2026-09-13-jetkvm-mini.image-57df7eba8c93.webp
    width: 1600
    height: 930
  variants:
  - file: 2026-09-13-jetkvm-mini.image-f51922972fc9.webp
    width: 48
    height: 28
  - file: 2026-09-13-jetkvm-mini.image-6f2ad86eec88.webp
    width: 320
    height: 186
  color: '#a2a2a2'
---

JetKVM Mini is a smaller, more affordable JetKVM. It comes in two models: **JetKVM Mini** with Ethernet for **$39** and **JetKVM Mini W**, wireless, for **$42**. In packs of three, they become even more affordable and drop to **$33** and **$36** per unit.

The case is aluminium, **42 × 42 × 23 mm** (1.7 × 1.7 × 0.9 in), about the footprint of a matchbox. It does what a JetKVM does: native video capture at 1080p ([up to 4K with JetKVM OS Services](https://jetkvm.com/blog/introducing-jetkvm-mini#os-services)), keyboard and mouse over USB, the same web interface, the same cloud, the same updates.

![JetKVM Mini from above, showing the display on top and the buttons on the side](https://jetkvm.com/blog/jetkvm-mini/angle.webp)

JetKVM Mini. The display is on top, the buttons on the side.

## Wired or wireless

**JetKVM Mini** has an RJ45 port. Plug in Ethernet, video, and the USB cable to the target computer, read the IP address off the display, and open it in a browser. Setup is the same as on every JetKVM.

**JetKVM Mini W** drops the RJ45 for a **2.4 and 5 GHz** radio. It is for machines a cable does not reach: a PC in another room, a box under the TV, a rack with no free switch ports. Video and USB are the only cables. Setup happens over Bluetooth from a browser on your phone or laptop: pick a network, enter the password, and the Mini W joins it.

![Back of JetKVM Mini with RJ45 and two USB ports](https://jetkvm.com/blog/jetkvm-mini/back-ethernet.webp)

JetKVM Mini · Ethernet

![Back of JetKVM Mini W with two USB ports](https://jetkvm.com/blog/jetkvm-mini/back-wifi.webp)

JetKVM Mini W · Wireless

The port face of JetKVM Mini and JetKVM Mini W.

## Why a Mini

We wanted a JetKVM affordable enough to put on every machine, and getting there meant starting over. **The Mini isn't a stripped-down JetKVM. It's a JetKVM re-engineered around the things a KVM needs to do well:** capture video, control keyboard and mouse, mount virtual media, and stay reachable when the machine it manages is not.

The result is a much simpler architecture: an ESP32-P4X handles video capture, H.264 encoding, USB, and the JetKVM firmware without the separate DRAM and eMMC required by a Linux system. Around it, we built a compact aluminium enclosure, a simple status display with physical controls, and the same web interface, cloud, extensions, and update system as JetKVM.

**It does the job of a JetKVM, in a smaller design that starts at $33.**

![JetKVM Mini from the front, the display showing the IP address and USB and video status](https://jetkvm.com/blog/jetkvm-mini/front.webp)

IP address, USB, and video status on the display.

## The hardware

**The Mini runs on an ESP32-P4X**, a microcontroller with a hardware H.264 encoder. Video is captured at **1080p at 30 fps** or 720p at 60 fps, encoded on the chip, and streamed over WebRTC to your browser. The Mini W adds an ESP32-C5 for wireless. It has IEEE 802.11a/b/g/n/ac/ax on 2.4 and 5 GHz for networking, Bluetooth LE for setup, and IEEE 802.15.4 for Zigbee and Thread.

The Mini has two USB ports. One goes to the target computer: it runs at USB 2.0 High Speed (480 Mbps), presents keyboard, mouse, and virtual media to the machine, and draws power from it. Virtual media runs off a TF Card in the slot on the side — drop in your own card and load it with the ISOs you need.

**The other is a general-purpose USB port.** It runs at USB 2.0 Full Speed (12 Mbps) and can also serve as a backup or secondary power input. It is where our extensions connect: we're reworking the ATX, DC power, and serial extensions to use standard USB, with new versions coming in a later release. With an external power supply, the port can even operate as a USB host, opening it up to essentially any USB device you choose to support in your own firmware.

## The software

**We rewrote the firmware for the ESP32-P4X**, and it will be open source from day one. It speaks the same protocols, so the web interface and the cloud carry over unchanged.

**That means the interface you already know**: keyboard and mouse control, text paste, keyboard layouts, virtual media from an ISO on the TF Card, JetKVM Cloud for access from outside your network, Wake-on-LAN, MQTT and Home Assistant, OIDC login, and over-the-air updates with automatic rollback.

**It also gets JetKVM OS Services.** The OS service for the machine your KVM is plugged into comes out in the next few weeks, and it supports the Mini from day one. It captures the target computer's screen at up to 4K, and adds a shared clipboard, a guest terminal, and file transfer. [Read more.](https://jetkvm.com/blog/introducing-jetkvm-os-services)

![The JetKVM web interface showing the AMI BIOS setup screen of the connected machine](https://jetkvm.com/blog/jetkvm-mini/bios-ui.webp)

The JetKVM interface on the Mini, in the BIOS of the connected machine.

**Secure boot, when you want it.** JetKVM Mini comes ready for secure boot. A digest of JetKVM's public signing key is preinstalled in the chip's eFuse, so you can permanently lock the Mini to JetKVM-signed firmware with a single setting in the web interface.

## Pricing and Availability

Both models are available **October 26, 2026** through our authorized resellers.

Model 1x 3x

JetKVM Mini Ethernet $39 $99

JetKVM Mini W Wireless $42 $108

MSRP in USD.
