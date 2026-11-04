---
title: Preparing for the Flipper One Kickstarter and opening pre-orders
link: https://flipper.net/pages/flipper-one
source: hnrss-org
published: 2026-11-03T19:32:13Z
updated: 2026-11-03T19:32:13Z
first_seen: 2026-11-04T00:52:13.198927099Z
authors:
- user_7832
summary: 'https://docs.flipper.net/one Comments URL: https://news.ycombinator.com/item?id=50356994 Points: 167 # Comments: 77'
content: extracted
html: 2026-11-03-preparing-for-the-flipper-one-kickstarter-and-opening-pre.html
remote_preview:
  url: http://flipper.net/cdn/shop/files/page_flipper-one_og-image.png?v=1785249769
---

![](https://flipper.net/cdn/shop/files/page_flipper-one_body.png?v=1793720883&width=3840) ![](https://flipper.net/cdn/shop/files/page_flipper-one_nmap-animation.gif?v=1779205141&width=1706) ![](https://flipper.net/cdn/shop/files/page_flipper-one_body.png?v=1793720883&width=886) ![](https://flipper.net/cdn/shop/files/page_flipper-one_body-shadow.png?v=1779205140&width=886) ![](https://flipper.net/cdn/shop/files/page_flipper-one_nmap-animation.gif?v=1779205141&width=804)

Open Linux computer based on a powerful ARM processor. Network multi-tool — built-in Wi-Fi 6E and Gigabit Ethernet. Expandable via M.2 modules: 5G cellular modem, SDR radio and more.

[PRE-ORDER NOW](https://flipper.net/products/flipper-one-preorder)

Follow progress at the [Developer Portal >>](https://docs.flipper.net/one "https://docs.flipper.net/one")

![](https://flipper.net/cdn/shop/files/page_flipper-one_console.png?v=1785164297&width=1706) ![](https://flipper.net/cdn/shop/files/page_flipper-one_antenna-desktop.png?v=1785164297&width=916) ![](https://flipper.net/cdn/shop/files/page_flipper-one_hero-terminal-mobile.png?v=1779205143&width=650) ![](https://flipper.net/cdn/shop/files/page_flipper-one_hero-switch-mobile.png?v=1779205140&width=784) ![](https://flipper.net/cdn/shop/files/page_flipper-one_hero-tower-mobile.png?v=1779205141&width=628)

## OPEN LINUX PLATFORM

Flipper One runs on completely open-source Linux firmware with a mainline kernel. This means ultimate compatibility with bleeding-edge kernel versions, free from vendor-specific BSP limitations and binary blobs. Our goal is to create a fully upstream-supported ARM computer that never becomes outdated, and a well-documented platform giving the community maximum freedom to customize:

- Full mainline Linux kernel support
- No binary blobs
- No vendor-locked BSP

![](https://flipper.net/cdn/shop/files/page_flipper-one_linux-illustration.png?v=1793720974&width=2832) ![](https://flipper.net/cdn/shop/files/page_flipper-one_linux-illustration-mobile.png?v=1793720965&width=1194)

We have partnered with the Collabora team, who are actively working on bringing the RK3576 SoC into the mainline kernel, to make Flipper One fully upstream-supported and free from binary blobs. [Read Collabora blog post](https://www.collabora.com/news-and-blog/news-and-events/collabora-flipper-opening-up-the-rk3576.html)

## CO-PROCESSOR ARCHITECTURE

Two independent processors work together in parallel, each controlling different sub-systems of Flipper One. \
 This architecture allows you to **control the Linux sub-system directly from the built-in LCD display**, without losing access to the device even when the Linux kernel is not yet ready or turned off.

![](https://flipper.net/cdn/shop/files/page_flipper-one_copro.png?v=1793720908&width=2896) ![](https://flipper.net/cdn/shop/files/page_flipper-one_copro-medium.png?v=1793720892&width=1436) ![](https://flipper.net/cdn/shop/files/page_flipper-one_copro-mobile.png?v=1793720899&width=804)

### Why 2 processors?

- **Low-power MCU (Raspberry Pi RP2350)** — an energy-efficient microcontroller responsible for core device control. It drives the LCD display, physical buttons, and status LEDs. It manages the power sub-system and controls the Linux OS boot parameters when the Linux sub-system is off. [More about MCU Firmware >>](https://docs.flipper.net/one/mcu-firmware/about "https://docs.flipper.net/one/mcu-firmware/about")

- **High-performance CPU (Rockchip RK3576)** — a powerful 8-core ARM system-on-chip that runs the Linux operating system. It executes user applications, controls all networking interfaces such as Wi-Fi and Ethernet, and manages high-speed peripherals including USB3 and PCIe. It also integrates an NPU for AI acceleration and a GPU for graphics output via HDMI and DisplayPort. [More about Linux sub-system >>](https://docs.flipper.net/one/cpu-software/about "https://docs.flipper.net/one/cpu-software/about")

## ONE CABLE FOR EVERYTHING

Get SSH access and charge the device via a single USB cable! No extra adapters needed for Linux console access or powering the device — Flipper One can simultaneously emulate a virtual network adapter, a serial adapter (COM port), and a mass-storage device.

No drivers required on macOS, Linux, Windows, iOS, or Android.

![](https://flipper.net/cdn/shop/files/page_flipper-one_cable.png?v=1785164299&width=5760) ![](https://flipper.net/cdn/shop/files/page_flipper-one_cable-medium.png?v=1779205180&width=2998)

![](https://flipper.net/cdn/shop/files/page_flipper-one_cable-mobile.png?v=1782219750&width=804)

## NETWORK MULTI‑TOOL

Multi-WAN network toolkit — connect up to 5 uplinks. Configure dynamic routing, failover, load balancing, and firewall rules. Use it simultaneously as a Wi‑Fi hotspot and client. Tunnel traffic through a VPN and multiple gateways.

![](https://flipper.net/cdn/shop/files/page_flipper-one_multitool.png?v=1793721010&width=3586) ![](https://flipper.net/cdn/shop/files/page_flipper-one_multitool-mobile.png?v=1793721002&width=784)

## NETWORK SCANNER

Analyze wired and wireless IP networks. Flipper One helps you map network topology, discover active hosts, and scan for open ports and services. It can also act as a passive sensor to detect network attacks and report anomalies.

![](https://flipper.net/cdn/shop/files/page_flipper-one_scanner-desktop.png?v=1785164298&width=5760) ![](https://flipper.net/cdn/shop/files/page_flipper-one_scanner-mobile.png?v=1779205148&width=1024)

## POWER METER

The power consumption meter works like a wattmeter, showing in real time how much power the entire system uses, including the CPU and peripherals. It helps you understand what is happening under the hood: whether the system is idle or actively working.

## M.2 MODULES

The core idea of Flipper One is an expandable hardware platform. Create your own specialized tool with a high-speed M.2 expansion module that installs inside the device, under the back plate. It supports almost any type of module — cellular or satellite modems, SDR modules, AI accelerators, SSDs (NVMe or SATA), and Wi-Fi cards via adapters.

![](https://flipper.net/cdn/shop/files/page_flipper-one_modules-desktop.png?v=1793720985&width=2732) ![](https://flipper.net/cdn/shop/files/page_flipper-one_modules-mobile.png?v=1793720991&width=738)

### M.2 tech specs

![](https://flipper.net/cdn/shop/files/page_flipper-one_modules-specs.png?v=1785164297&width=1366)

- M.2 type: Key-B
- Supported sizes: 2230 (via extender), 2242, 3042, 3052
- Supported thickness: up to D3 (double-sided modules)
- Interfaces: PCIe 2.1 ×1 / USB 2.0 / USB 3.1 / SATA3 / SIM card + eSIM
- Extra interfaces: Serial Audio / UART / I²C

### Open source mounting system

All mechanical parts for module mounting are [completely open source.](https://docs.flipper.net/one/mechanics/module-mounting-system) You can design your own antennas, cooling system, and mounting solutions for additional accessories.

![](https://flipper.net/cdn/shop/files/page_flipper-one_modules-block-two.png?v=1782219685&width=1366)

## GPIO MODULES

DIY modules can be connected via the GPIO connector using standard 2.54 mm pin headers. To make the assembled device easier to carry around, the GPIO module is positioned along the device body and can be covered with a protective cover.

Check out [GPIO technical specification](https://docs.flipper.net/one/hardware/gpio-port) and [examples of GPIO modules](https://docs.flipper.net/one/hardware/gpio-port/modules).

![](https://flipper.net/cdn/shop/files/page_flipper-one_gpio-desktop.png?v=1793720944&width=2896) ![](https://flipper.net/cdn/shop/files/page_flipper-one_gpio-mobile.png?v=1793720952&width=740)

## Desktop Mode

Connect Flipper One to an external display with a single USB-C cable for a full desktop experience. Support for DisplayPort Alt Mode allows one cable to deliver power, transmit video, and connect USB peripherals such as a keyboard and mouse simultaneously.

![](https://flipper.net/cdn/shop/files/page_flipper-one_display.png?v=1785164299&width=5760) ![](https://flipper.net/cdn/shop/files/page_flipper-one_display-mobile.png?v=1785164297&width=740)

## Satellite NTN Modem

## Satellite modem

NTN [(Non-Terrestrial Networks)](https://www.3gpp.org/technologies/ntn-overview) — is a satellite communication technology, a low-speed connection designed for IoT devices, standardized by 3GPP as part of the 5G and LTE specifications. It uses the standard cellular stack, including SIM/eSIM authentication, roaming, and regular IP traffic.

![](https://flipper.net/cdn/shop/files/page_flipper-one_satellite-desktop.png?v=1785164298&width=2896) ![](https://flipper.net/cdn/shop/files/page_flipper-one_satellite-mobile.png?v=1785164297&width=740)

We want to add NTN satellite connectivity support to Flipper One to help popularize this technology and give engineers and enthusiasts a chance to work with real satellite infrastructure. \
To make this happen, we're looking for a partner company like [Skylo](https://www.skylo.tech/) that's ready to work with us to add support for their satellite network to our eSIM module and help us choose a specific [NTN M.2 module](https://www.skylo.tech/certified-devices#modules) that we can officially support. \
 Read more on the [Modules → Satellite modem](https://docs.flipper.net/one/hardware/m2-modules/satellite-modem?_gl=1*1wsjoy*_ga*MjAwNDk3OTU4LjE3NzAxMzUwODk.*_ga_GM78S6JK0K*czE3NzkxOTc3NDgkbzM1JGcxJHQxNzc5MjAxNjM4JGo0MiRsMCRoMA..) page.

## Built-in Advanced Wi-Fi Chip

## Built-in Wi-Fi Chip

We want to build the most versatile integrated Wi-Fi into Flipper One — it needs to support all the features required for Wi-Fi network analysis, including monitor mode and packet injection. For now, we've settled on the popular Mediatek MT7921U chipset. It's modern enough, supports three frequency bands, and ticks just about every box. We're actively testing this chip right now and [we'd love you to join in](https://docs.flipper.net/one/testing/network/wifi).

![](https://flipper.net/cdn/shop/files/page_flipper-one_wifi.png?v=1785164297&width=2896) ![](https://flipper.net/cdn/shop/files/page_flipper-one_wifi-chip-mobile.png?v=1785164297&width=740)

### Why MT7921AUN?

The same chipset is used in the popular ALFA AWUS036AXML adapter, and there's a sizeable community around the open-source MT7921U driver. The main strengths of this chipset:

![](https://flipper.net/cdn/shop/files/page_flipper-one_wifi-chip-illustration.png?v=1785164296&width=424)

- Modern Wi-Fi 6E: 802.11ax, 2x2 MIMO
- Frequencies: 2.4 / 5 / 6 GHz bands
- Open mainline Linux driver: mt7921u
- Monitor mode support
- Integrated Bluetooth 5.2

## BUILT-IN eSIM

Flipper One has a built-in eSIM chip that comes pre‑provisioned with a Flipper SIM data plan.

To use the eSIM data plan, you need to install a 5G modem in the M.2 slot. You can also switch between the built-in eSIM and a physical SIM card.

![](https://flipper.net/cdn/shop/t/5/assets/one-esim-sim-icon.svg?v=142576394257534669311793721441)

### What is Flipper SIM?

We're working on an experimental connectivity project for network geeks. Flipper SIM is a universal SIM card for routers, phones, and IoT devices, with advanced features such as always-on connectivity, outage-proof, traffic filtering, and network management tools. We'll share more details soon!

Coming soon

![](https://flipper.net/cdn/shop/files/page_flipper-one_esim.png?v=1793720928&width=1990) ![](https://flipper.net/cdn/shop/files/page_flipper-one_esim-mobile.png?v=1793720918&width=804)

## Tech Specs

![](https://flipper.net/cdn/shop/files/page_flipper-one_specs-1.png?v=1793721042&width=5760) ![](https://flipper.net/cdn/shop/files/page_flipper-one_specs-1-mobile.png?v=1793721031&width=1200)

![](https://flipper.net/cdn/shop/files/page_flipper-one_specs-2.png?v=1793721055&width=5760) ![](https://flipper.net/cdn/shop/files/page_flipper-one_specs-2-mobile.png?v=1793721048&width=1200)

![](https://flipper.net/cdn/shop/files/page_flipper-one_specs-3.png?v=1793721078&width=5760) ![](https://flipper.net/cdn/shop/files/page_flipper-one_specs-3-mobile.png?v=1793721067&width=1200)

![](https://flipper.net/cdn/shop/files/page_flipper-one_specs-4.png?v=1793721095&width=5760) ![](https://flipper.net/cdn/shop/files/page_flipper-one_specs-4-mobile.png?v=1793721088&width=1200)

![](https://flipper.net/cdn/shop/files/page_flipper-one_specs-5.png?v=1793721116&width=5760) ![](https://flipper.net/cdn/shop/files/page_flipper-one_specs-5-mobile.png?v=1793721109&width=1200)

![](https://flipper.net/cdn/shop/files/page_flipper-one_specs-6-mobile.png?v=1793721121&width=1200)
