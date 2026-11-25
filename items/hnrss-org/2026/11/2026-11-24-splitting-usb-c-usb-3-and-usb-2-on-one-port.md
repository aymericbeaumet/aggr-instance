---
title: 'Splitting USB-C: USB 3 and USB 2 on one port'
link: https://www.frankchiarulli.com/blog/splitting-usb-c/
source: hnrss-org
published: 2026-11-24T14:54:10Z
updated: 2026-11-24T14:54:10Z
first_seen: 2026-11-25T01:32:55.949141972Z
authors:
- fcjr
content: extracted
html: 2026-11-24-splitting-usb-c-usb-3-and-usb-2-on-one-port.html
remote_preview:
  url: https://frankchiarulli.com/blog/splitting-usb-c/opengraph-image?1da773d5ba88c69c
  alt: frankchiarulli.com
---

I started building [restorekit.org](https://restorekit.org) dongles a few months ago at the [Recurse Center](https://recurse.com). They are small inline USB-C boards that sit between a host computer and a target mac. They set the target mac into DFU Device Firmware Update: Apple's lowest-level recovery mode built into the boot ROM, so even a bricked Mac can be reflashed. mode over USB-PD USB Power Delivery: the communication protocol over the CC wire used to negotiate power, roles, and vendor-defined commands. without having to touch the target at all, and work with an open-source toolkit for restoring macs without the need for another mac.

The original board, the **Dongle-Lite**, is USB 2 only. A single USB 2 hub ([`CH334F`](https://www.lcsc.com/product-detail/C5187527.html)) connects both the dongle’s MCU and the target mac through a single cable, while a PD PHY Physical-layer transceiver chip that encodes and decodes USB Power Delivery packets on the CC wire. ([`FUSB302B`](https://www.lcsc.com/product-detail/C132291.html)) sends Apple’s DFU commands on the CC line.

I recently learned from timo at [Twocanoes](https://twocanoes.com/) that newer Macs support USB 3 speeds for recovery. I obviously immediately wanted to support this on the hub for faster recovery, but the problem was cost. At first I designed a “proper” USB 3 board using a [`GL3510`](https://www.lcsc.com/product-detail/C7501408.html) USB 3 hub and a pair of TI SuperSpeed muxes ([`HD3SS3220`](https://www.lcsc.com/product-detail/C165155.html) and [`HD3SS3212`](https://www.lcsc.com/product-detail/C544517.html)). It basically doubled the parts count, and BOM Bill of Materials: the full list of components and quantities required to assemble one board. cost, required me to increase the size of PCB to ~100 mm, and topped out at 5 Gbps transfers between host and target (USB 3.1 Gen 1).

I really didn’t want to make a board that cost more than twice the Dongle-Lite just to get USB 3 speeds.

So with the 2nd version **Dongle-Pro**, I tried a kinda crazy USB-C experiment: drop the USB 3 hub completely, keep a cheap USB 2 hub on `D+`/`D-`, and route the USB 3 SuperSpeed USB 3.x's dedicated high-speed TX/RX differential pairs, physically separate from USB 2's D+/D- wires. lanes as a straight passthrough from host to target on the same USB-C port. The idea being that *in theory* the USB 3 and 2 controllers on a USB-C are completely independent, and should work simultaneously.

Since high-speed USB 3 is super prone to interference I didn’t really have a choice other than to purchase a view boards and test it out.

Well… I just got the boards in from JLC and… THEY WORK! They establish a full **10 Gbps USB 3.2 Gen 2** connection while connecting the dongle’s USB 2 hub and MCU over the exact same port. After some aggressive cost optimization, the 10 Gbps board also ended up **cheaper and smaller than the USB 2 Dongle-Lite**.

![Front and back 3D renders of the Dongle-Pro v2 PCB](https://www.frankchiarulli.com/splitting-usb-c/dongle-pro-v2-renders.png)\
Dongle-Pro v2 (21 x 48 mm)

Like I mentioned earlier, USB-C is not a single data bus. Inside a standard USB-C port, USB 2 and USB 3 sit on completely different sets of pins:

USB-C 24-Pin Spec

Every USB-C port has two mirrored 12-pin rows: `A1` through `A12` on top, and `B12` through `B1` on the bottom, so a cable works rotated either way.

USB 3 SuperSpeed USB 2 (D+/D-) CC / USB-PD SBU (Apple UART) Power / GND

- **USB 2 (`D+` / `D-`):** a single 480 Mbps pair in the center (pins `A6/A7` and `B6/B7` are shorted together).
- **USB 3 (`TX/RX`):** 2 separate high-speed lanes on the outer edge. Only 1 set is active depending on orientation.
- **Control & sideband (`CC`, `SBU`):** `CC1/CC2` detect cable orientation and negotiate USB-PD; `SBU1/SBU2` (`A8/B8`) are used by Apple’s 1.2 V debug UART.

This continues to the computer’s xHCI Extensible Host Controller Interface: the USB controller inside your computer that manages both USB 2 and USB 3.x root ports. controller: a single physical USB-C port is actually wired to two independent root ports: a USB 2 port and a USB 3 port.

A standard USB 3 hub chip like the [`GL3510`](https://www.lcsc.com/product-detail/C7501408.html) is actually just two hubs in 1 package (a USB 2 hub and a USB 3 hub). On my first iteration of the Dongle-Pro, everything ran through that chip even though the dongle’s own MCU only speaks USB 2 Full Speed (12 Mbps) and has no need for the USB 3 speed.

On the Dongle-Pro v2, the two buses are split apart:

- **USB 2 (`D+`/`D-`)** runs into a [`CH334P`](https://www.lcsc.com/product-detail/C5373042.html) USB 2 hub. One downstream port goes to the target’s `D+`/`D-` (so DFU mode and any USB 2 device behave just like the Lite), and another goes to the dongle’s MCU ([`CH32X035G8U6`](https://www.lcsc.com/product-detail/C7437027.html)).
- **USB 3 (`TX`/`RX`)** skips the hub completely. Each port’s two lane sets go into a [`CH9441P`](https://www.lcsc.com/product-detail/C54300797.html) 2:1 mux Multiplexer: a high-speed analog switch that routes whichever side of a reversible USB-C plug is active onto one pair of board traces.. Each mux reads its own port’s `CC1`/`CC2` lines to handle cable flip and connects the active lane set to its common side. The two muxes sit in the center of the board, wired straight together through a short corridor with no hub and no redriver An active signal-conditioning chip that boosts and equalizes high-speed USB 3 traces to overcome cable and PCB insertion loss..

Pro v1 (hubbed 5 Gbps) vs. Pro v2 (split-bus 10 Gbps)

### [What Could Go Wrong](https://www.frankchiarulli.com/blog/splitting-usb-c/#what-could-go-wrong)

This sounded like it would work to me on paper, but there were a few things I thought could go wrong:

1. **Two passive muxes in series with no redriver.** Every connector, ESD array, via, and switch adds insertion loss and reflections. I was hoping two [`CH9441P`](https://www.lcsc.com/product-detail/C54300797.html) chips back-to-back would pass 5 Gbps. But hitting 10 Gbps is even better.
2. **Sharing the target CC lines.** On the target port, the [`HUSB311`](https://www.lcsc.com/product-detail/C7471922.html) PD PHY actively drives the CC lines to send Apple’s SOP” Debug A USB-PD packet framing addressed to a debug accessory rather than a normal device, required for Apple's DFU commands. VDMs Vendor-Defined Messages: custom USB-PD packets. Apple uses them to trigger DFU, reboot, and serial debug without touching the keyboard.. The target’s [`CH9441P`](https://www.lcsc.com/product-detail/C54300797.html) is also reading the lines to steer its lanes.
3. **Host OS behavior.** Even though it made sense in theory, I really wasn’t sure if a machine would happily enumerate a USB 2 hub on a port’s `D+`/`D-` lines while a separate USB 3.2 Gen 2 device negotiated directly with the same port’s SuperSpeed lanes.

## [On the Bench](https://www.frankchiarulli.com/blog/splitting-usb-c/#on-the-bench)

When the boards arrived, I immediately plugged one into my mac, plugged an iPhone into the target port, and inspected the bus with `system_profiler SPUSBHostDataType` and `ioreg -p IOUSB`.

Here is what macOS sees on a **single physical USB-C port** (`AppleT8132USBXHCI@01000000`):

One USB-C port on macOS: USB 2 hub on 0x01100000, 10 Gb/s target on 0x01200000

The raw `system_profiler SPUSBHostDataType` output shows both sides active at once:

```yaml
USB 3.1 Bus:
  Location ID: 0x01000000
  Driver: AppleT8132USBXHCI
 
    USB HUB:
      Location ID: 0x01100000
      Link Speed: 480 Mb/s
      USB Vendor ID: 0x1a86
      USB Product ID: 0x8091
 
        Dongle-Pro-v2:
          Location ID: 0x01120000
          Manufacturer: RestoreKit
          Serial Number: DP2-C2BDF224
          Link Speed: 12 Mb/s
          USB Vendor ID: 0x16d0
          USB Product ID: 0x14f0
 
    iPhone:
      Location ID: 0x01200000
      Manufacturer: Apple Inc.
      Link Speed: 10 Gb/s
      USB Vendor ID: 0x05ac
      USB Product ID: 0x12a8
```

`0x01100000` is the USB 2 half of the physical port, and `0x01200000` is the SuperSpeed half of the same port. The iPhone enumerated straight through both passive [`CH9441P`](https://www.lcsc.com/product-detail/C54300797.html) muxes at **10 Gb/s (USB 3.2 Gen 2)**, while the [`CH334P`](https://www.lcsc.com/product-detail/C5373042.html) hub and `Dongle-Pro-v2` MCU enumerate at 480 Mb/s and 12 Mb/s on `D+`/`D-`.

### [Companion Ports](https://www.frankchiarulli.com/blog/splitting-usb-c/#companion-ports)

Splitting the bus required one interesting change in the host SDK (`restorekit`).

On the Dongle-Lite, whatever device you plugged into the dongle always showed up as a **child of the dongle’s hub**. That made it easy to tell which target Mac belonged to which dongle when you plugged in several at once.

On the Dongle-Pro v2, that breaks whenever the target connects at USB 3. Just like `iPhone@01200000` above, a SuperSpeed target bypasses the [`CH334P`](https://www.lcsc.com/product-detail/C5373042.html) hub completely and shows up as a **sibling** directly on the host’s root hub.

Turns out the OS already tracks which USB 2 and USB 3 root ports share the same physical port and it exposes it directly:

- On **macOS**, the USB 2 port’s IORegistry macOS's live kernel tree of hardware devices, ports, and drivers, which you can inspect with ioreg. entry has a `UsbProtocolCompanion (3.x)` property containing the path to its SuperSpeed twin.
- On **Linux**, the USB 2 port’s sysfs Linux's virtual filesystem at /sys that exposes kernel devices, USB hubs, and physical port peer links. directory has a `port/peer` symlink pointing straight to the SuperSpeed port.

```rust
// On Linux, finding the SuperSpeed twin of the port our USB 2 hub sits on:
let peer = fs::canonicalize(hub.sysfs_path().join("port/peer")).ok()?;
let port = fs::canonicalize(dev.sysfs_path().join("port")).ok()?;
Some(peer == port)
```

So `restorekit` can still know with 100% certainty which 10 Gbps device is plugged into which dongle.

## [Cutting the Cost](https://www.frankchiarulli.com/blog/splitting-usb-c/#cutting-the-cost)

I originally planned to have a Dongle-Lite and significantly more expensive USB 3 Pro, but that means having to maintain two hardware lines, so if I could cut the cost of the USB 3 version to be comparably priced to the original Dongle-Lite it felt like a no brainer.

After I removed the [`GL3510`](https://www.lcsc.com/product-detail/C7501408.html) USB 3 hub from the original design, I went through the rest of the board to see what else I could cut:

- **SuperSpeed path:** [`HD3SS3220`](https://www.lcsc.com/product-detail/C165155.html) ($1.66) + [`HD3SS3212`](https://www.lcsc.com/product-detail/C544517.html) ($1.55) + [`GL3510`](https://www.lcsc.com/product-detail/C7501408.html) ($1.23) came to **$4.44** on Pro v1 for 5 Gbps. Two [`CH9441P`](https://www.lcsc.com/product-detail/C54300797.html) muxes cost **$0.47** total ($0.24 each) on Pro v2 for **10 Gbps**. Pro v1’s SuperSpeed chips alone cost more than the entire Pro v2 BOM.
- **MCU:** Swapped the [`RP2354A`](https://www.lcsc.com/product-detail/C41378174.html) + 3.3 uH inductor + crystal ($1.62) with a [`CH32X035G8U6`](https://www.lcsc.com/product-detail/C7437027.html) ($0.42), running off its internal 48 MHz HSI High-Speed Internal oscillator: an on-chip clock source that lets an MCU run without an external quartz crystal. with no crystal, no external inductor, and a built-in ROM USB bootloader.
- **PD PHY:** Swapped the [`FUSB302BMPX`](https://www.lcsc.com/product-detail/C132291.html) ($0.68) with a [`HUSB311BLA`](https://www.lcsc.com/product-detail/C7471922.html) ($0.51).
- **USB 2 hub:** Swapped the [`CH334F`](https://www.lcsc.com/product-detail/C5187527.html) ($0.53) with a [`CH334P`](https://www.lcsc.com/product-detail/C5373042.html) ($0.31), which integrates the crystal load capacitors and drops two more passives.
- **1.2 V SBU UART level shifters:** two [`74AVC1T45`](https://www.lcsc.com/product-detail/C282330.html) translators plus a dedicated [`TLV70212`](https://www.lcsc.com/product-detail/C81462.html) 1.2 V LDO ($0.79) became two [`HL2302`](https://www.lcsc.com/product-detail/C28646268.html) MOSFETs and a few resistors ($0.05) wired as common-gate level shifters.
- **Unified ESD and passive discipline:** swapped mixed [`USBLC6`](https://www.lcsc.com/product-detail/C7519.html) + [`TPD4E05U06`](https://www.lcsc.com/product-detail/C138714.html) parts for six [`H5VU25U`](https://www.lcsc.com/product-detail/C7420374.html) flow-through arrays (JLCPCB Preferred Extended = $0 feeder fee), and limited the whole board to just **three resistor values** and **three capacitor values**.

Running the three BOMs against JLCPCB pricing:

| Metric                      | Dongle-Lite (USB 2)       | Dongle-Pro v1 (5 Gbps)    | Dongle-Pro v2 (10 Gbps)       |
| --------------------------- | ------------------------- | ------------------------- | ----------------------------- |
| **Board size**              | 22.1 x 77.1 mm (17.0 cm²) | 26.0 x 98.0 mm (25.5 cm²) | **21.0 x 48.0 mm (10.1 cm²)** |
| **Unique BOM lines**        | 24 (11 Extended)          | 31 (15 Extended)          | **18 (8 Extended)**           |
| **Total placements**        | 56                        | 99                        | **52**                        |
| **JLCPCB feeder fees**      | $33                       | $45                       | **$24**                       |
| **Parts / board @ qty 15**  | $5.03 ($7.23 w/ fees)     | $10.44 ($13.44 w/ fees)   | **$2.93 ($4.53 w/ fees)**     |
| **Parts / board @ qty 100** | $4.06 ($4.39 w/ fees)     | $8.24 ($8.69 w/ fees)     | **$2.31 ($2.55 w/ fees)**     |

*(Parts and Extended feeder fees from JLCPCB’s catalog, excluding PCB fab and placement fees. V2 wins on fab cost too since it is 40% smaller than the Lite and 60% smaller than Pro v1, packing three boards into one 81 x 48 mm panel.)*

These swaps resulted in a **72% BOM reduction** from original Dongle-Pro while **doubling** the SuperSpeed link rate from 5 Gbps to 10 Gbps, and a **42% BOM reduction** compared to the Dongle-Lite.

## [Conclusion](https://www.frankchiarulli.com/blog/splitting-usb-c/#conclusion)

If you are building an inline USB-C tool where your MCU only needs a slow control channel and the host just needs fast passthrough to the downstream device, you might not need a USB 3 hub. USB-C gives you two independent buses in the same port.

Next up: A flash drive with USB 3.2 Gen 2 + 2 speeds?

* * *

[Edit on GitHub](https://github.com/fcjr/frankchiarulli.com/edit/main/public/splitting-usb-c/index.md)
