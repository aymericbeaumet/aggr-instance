---
title: European RISC-V Industrial Computing Initiative
link: https://erici.eu/
source: hnrss-org
published: 2026-11-02T08:58:33Z
updated: 2026-11-02T08:58:33Z
first_seen: 2026-11-03T14:36:14.641821525Z
authors:
- ymz5
content: extracted
html: 2026-11-02-european-risc-v-industrial-computing-initiative.html
remote_preview:
  url: https://erici.eu/img/og-card.png
  alt: 'ERICI: RISC-V Industrial Computers. Open, partitioned, built to last. The reference stack Linux and QSOE on gRouvr, mr-bml, HFI BIOS and an RVA23 SoC.'
---

European RISC-V Industrial Computing Initiative

RISC-V Industrial Computers.\
 Open, partitioned, built to last.
---

RVA23 silicon has arrived. The industrial computer built on it has not. ERICI coordinates a reference stack, from firmware to real-time and general-purpose operating systems: it fixes the interfaces between the layers and verifies that they boot together on real RVA23 hardware. A box that boots it is ERICI-compatible.

- Non-commercial
- RVA23
- Based in Europe, open worldwide

**Linux** operator GUI, HMI

**QSOE** real-time control

**gRouvr** partitioning hypervisor

**mr-bml** boot loader, OS selection

**HFI BIOS** firmware, Set-Up, boot control

**RVA23 RISC-V SoC** cores, memory, devices, IOMMU

The ERICI reference stack, tested together layer by layer. [See how it boots](https://erici.eu/#stack)

Why now

## Industrial computing is ready for RISC-V

Today's industrial computers run on x86 and Arm: two instruction sets, each owned and licensed by a single company. RISC-V is open, governed by a non-profit, and implemented by many vendors. Until recently it lacked what industrial systems need. That has changed.

1

### An open ISA

No license for the instruction set, no single owner, no single country. A machine expected to run for 15 years should not depend on one company's roadmap, or one government's decisions.

2

### RVA23: a real baseline

The RVA23 profile, ratified in October 2024, makes the hypervisor and vector extensions mandatory. Software can rely on them on every compliant chip.

3

### Silicon is shipping

The first RVA23 SoCs and boards are available in 2026, with eight 64-bit cores and an IOMMU. Developers can buy them today.

4

### One box instead of two

A partitioning hypervisor runs real-time control and the operator's Linux side by side, isolated from each other, on one SoC.

### From a university project to industrial computing

1. **2010** RISC-V starts at UC Berkeley
2. **2015** RISC-V Foundation founded
3. **2020** RISC-V International, based in Switzerland
4. **2021** Hypervisor and vector extensions ratified
5. **2024** RVA23 profile ratified
6. **2026** First RVA23 boards. ERICI starts.

So why are there no RISC-V industrial computers yet? Board makers wait for software, software waits for boards, and customers wait for both. ERICI breaks that loop with an open reference stack running on real hardware.

The gap

## The silicon is here. The industrial computer isn't yet.

The first RVA23 machines are on the market: eight 64-bit RISC-V cores, the hypervisor extension, an IOMMU. They are excellent development and edge platforms. An industrial computer has to do more: survive the control cabinet, talk to the field, and stay available for a decade or longer.

![Firefly AIBOX-K3, a compact black and grey mini PC](https://erici.eu/img/01-FireFly_AIBOX_K3.png)

### Firefly AIBOX-K3

T-Chip Intelligent Technology · edge mini PC

- SpacemiT K3, 8× X100 RISC-V cores up to 2.4 GHz, RVA23
- Up to 32 GB LPDDR5, UFS, M.2 NVMe
- 2× Gigabit Ethernet, 9–20 V DC input
- −20 to 60 °C operating temperature

Image © T-Chip Intelligent Technology Co., Ltd. · [product page](https://www.firefly.store/collections/ai-box/products/aibox-k3-risc-v-edge-mini-pc)

![Milk-V Jupiter2 in its black metal case with two antennas](https://erici.eu/img/02-MilkV_Jupiter2.png)

### Milk-V Jupiter2

Milk-V · single-board computer, metal case

- SpacemiT K3, 8× X100 RISC-V cores up to 2.4 GHz, RVA23
- Up to 32 GB LPDDR5, UFS, M.2 NVMe
- Gigabit and 10 GbE SFP+, Wi-Fi 6
- Real-time I/O expansion for EtherCAT and CAN-FD modules

Image © Milk-V · [product page](https://milkv.io/jupiter2)

### What a RISC-V Industrial Computer needs

| Requirement  | AIBOX-K3      | Jupiter2 |
| ------------ | ------------- | -------- |
| Yes          | Yes           |          |
| 9–20 V       | 12 V / USB-PD |          |
| −20 to 60 °C | Not stated    |          |
| Not stated   | Via modules   |          |
| Not stated   | Not stated    |          |
| Not stated   | Not stated    |          |
| Not stated   | Not stated    |          |
| Not stated   | Not stated    |          |
| Not yet      | Not yet       |          |

Partitioned stack: demonstrated on the SpacemiT K3 PicoITX board, not yet on these products. [See the video](https://erici.eu/#demo).\
 Specifications as published by the manufacturers, October 2026. Both products are shown as examples of RVA23 hardware available today. No affiliation with ERICI or endorsement by the manufacturers is implied.

The missing pieces are not the cores. They are the industrial hardware around them and an open, partitioned software stack on top. That is where ERICI starts.

Reference stack

## Defined together, verified together

A RISC-V Industrial Computer is built in layers. ERICI defines a reference stack, documents the interface between each pair of layers, and verifies the whole stack on real RVA23 hardware. The reference components are developed by QSOE Systems; Linux by its community. Each layer depends only on what the layer below hands over.

**Operator partition** GUI, HMI, networking

**Linux** a general-purpose OS for everything that is not time-critical

- devicetree
- kernel + initrd

**Real-time partition** control loops, machine I/O

**[QSOE](https://qsoe.net)** a QNX-style environment: /N on its own Skimmer microkernel, /L on seL4, same userspace

- devicetree
- kernel + initrd
- [Multiboot3](https://gitlab.com/qsoe/mr-bml) for /L, next gRouvr

**Partitioning** CPUs, memory, devices, IOMMU

**gRouvr** RVA23 resource-partitioning hypervisor; every available means of isolation, IOMMU included

- kernel + initrd, like Linux

**Loading** boot menu, OS images

**[mr-bml](https://gitlab.com/qsoe/mr-bml)** menu-driven boot loader: the operator selects which systems to start

- UEFI application

**Firmware** initialization, boot control

**[HFI BIOS](https://qsoe.net/hfi)** U-Boot-based, with POST and a BIOS-style Set-Up for industrial boards

- RVA23 profile

**Hardware** RVA23 SoC with IOMMU

**SpacemiT K3** 8× X100 cores, hypervisor extension, IOMMU

- the silicon itself

### How it boots

The firmware initializes the system, runs POST and offers its Set-Up. Then it starts a UEFI boot loader, and its job is done. The loader starts operating systems, or a hypervisor together with them.

gRouvr is loaded just like a Linux kernel with its initrd. Its boot package is a CPIO archive that holds the kernel and initramfs image of every partition. So mr-bml loads it, and so does GRUB. gRouvr then gives each partition its own CPUs, memory and devices, and a devicetree that describes them. Real-time and Linux partitions start side by side.

[![QSOE/N login prompt on a monitor, running as a gRouvr guest on the SpacemiT K3](https://erici.eu/img/03-demo-grouvr-k3.jpg)](https://www.youtube.com/watch?v=i-NjXXnTv70) \
 [▶ Watch the demonstration](https://www.youtube.com/watch?v=i-NjXXnTv70) gRouvr booting Linux and QSOE/N on the SpacemiT K3. Opens on YouTube.

### Built to outlast

### Not tied to one chip

The first RVA23 SoCs come from Chinese vendors; more will follow from other vendors and other regions. The reference stack depends on the RVA23 profile and on open interfaces, not on a particular SoC. Moving a box to another RVA23 chip means porting the firmware, adapting the hypervisor and the board's drivers. The real-time application and the operator's Linux move with it.

### Not tied to one developer

The reference stack is open source: HFI BIOS under GPL-2.0, like the U-Boot it is based on; mr-bml under GPL-3.0; gRouvr and QSOE under Apache 2.0. Anyone can build, audit and maintain it, with or without its original developers. A machine that runs for 15 years needs software that can be maintained for 15 years.

Join

## Build the RISC-V industrial computer with us

ERICI is in its founding phase. Companies and institutions that see RVA23 as the future of industrial computing are welcome to join now and shape the initiative from the start.

### Who we invite, and what they bring

- **Industrial hardware makers** the box itself: an RVA23 board in a real enclosure, with I/O and a datasheet
- **Silicon vendors** RVA23 SoCs with an IOMMU and long-term availability
- **Machine builders and plant operators** requirements from the field, and a first machine to put a box in
- **OS and driver vendors** your RTOS, Linux distribution or drivers, brought up on a named RVA23 box
- **Universities and research** teaching and theses on real ERICI systems

### How ERICI works

- **Non-commercial.** ERICI sells nothing and charges no membership fees.
- **Open interfaces.** How each layer hands over to the next is public: UEFI, kernel + initrd, devicetree. Closed-source components are welcome, as long as they keep to these interfaces.
- **Bring a real system.** Contributions are RISC-V industrial computers: a named SoC in a real box, with a datasheet and a date. Software counts when it serves such a system, such as the driver that lets the real-time partition on box X run its machine.
- **Concrete, not abstract.** No road maps without silicon, no specs without a system that boots them.
