---
title: 'Show HN: A Truly Modular Keyboard'
link: https://github.com/notaroomba/keybytes
source: hnrss-org
published: 2026-10-25T03:47:56Z
updated: 2026-10-25T03:47:56Z
first_seen: 2026-10-26T14:07:38.108821132Z
authors:
- NotARoomba
content: extracted
html: 2026-10-25-show-hn-a-truly-modular-keyboard.html
remote_preview:
  url: https://opengraph.githubassets.com/9e32dbd4208688f89406ded4c864e37232861c7ab2dbf09aa696f33bedc776d8/notaroomba/keybytes
  alt: A truly modular keyboard. Contribute to notaroomba/keybytes development by creating an account on GitHub.
---

#### A truly modular keyboard with rearrangeable keys, magnetic cases and Bluetooth!

[![KiCad](https://camo.githubusercontent.com/83adf67ca538892e0271a7f4c152208ec1ce975b4297334153ea3e447158b46e/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6b696361642d2532333030353738462e7376673f7374796c653d666f722d7468652d6261646765266c6f676f3d6b69636164266c6f676f436f6c6f723d7768697465)](https://camo.githubusercontent.com/83adf67ca538892e0271a7f4c152208ec1ce975b4297334153ea3e447158b46e/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6b696361642d2532333030353738462e7376673f7374796c653d666f722d7468652d6261646765266c6f676f3d6b69636164266c6f676f436f6c6f723d7768697465) [![STM32](https://camo.githubusercontent.com/d12df1f5373a066f32ff73486cae7523e229b796239f0eec6b3fca1cfffb6400/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f53544d33322d3033323334423f7374796c653d666f722d7468652d6261646765266c6f676f3d73746d6963726f656c656374726f6e696373266c6f676f436f6c6f723d7768697465)](https://camo.githubusercontent.com/d12df1f5373a066f32ff73486cae7523e229b796239f0eec6b3fca1cfffb6400/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f53544d33322d3033323334423f7374796c653d666f722d7468652d6261646765266c6f676f3d73746d6963726f656c656374726f6e696373266c6f676f436f6c6f723d7768697465) [![RISC-V](https://camo.githubusercontent.com/63e1997538f93ca538171cc3f91e8b1e13e2aa3aef3a9a19616dc209242e6d3b/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f524953432d2d562d3238333237323f7374796c653d666f722d7468652d6261646765266c6f676f3d7269736376266c6f676f436f6c6f723d7768697465)](https://camo.githubusercontent.com/63e1997538f93ca538171cc3f91e8b1e13e2aa3aef3a9a19616dc209242e6d3b/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f524953432d2d562d3238333237323f7374796c653d666f722d7468652d6261646765266c6f676f3d7269736376266c6f676f436f6c6f723d7768697465) [![Onshape](https://camo.githubusercontent.com/2ac2bd5a44e3b913f7489b5f06fbd92d6dfe7a7ee3101531b7352454d727b6b6/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6f6e73686170652d2532333231373334362e7376673f7374796c653d666f722d7468652d6261646765266c6f676f3d6f6e7368617065266c6f676f436f6c6f723d7768697465)](https://camo.githubusercontent.com/2ac2bd5a44e3b913f7489b5f06fbd92d6dfe7a7ee3101531b7352454d727b6b6/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f6f6e73686170652d2532333231373334362e7376673f7374796c653d666f722d7468652d6261646765266c6f676f3d6f6e7368617065266c6f676f436f6c6f723d7768697465) [![Blender](https://camo.githubusercontent.com/febea57e27e9e082d838240aa6829e18bb87068a141fd4e0406cd088328919be/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f626c656e6465722d2532334635373932412e7376673f7374796c653d666f722d7468652d6261646765266c6f676f3d626c656e646572266c6f676f436f6c6f723d7768697465)](https://camo.githubusercontent.com/febea57e27e9e082d838240aa6829e18bb87068a141fd4e0406cd088328919be/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f626c656e6465722d2532334635373932412e7376673f7374796c653d666f722d7468652d6261646765266c6f676f3d626c656e646572266c6f676f436f6c6f723d7768697465)

[Key Features](https://github.com/notaroomba/keybytes#key-features) • [Video Demo](https://github.com/notaroomba/keybytes#video-demo) • [PCB](https://github.com/notaroomba/keybytes#pcb) • [Case](https://github.com/notaroomba/keybytes#case) • [Credits](https://github.com/notaroomba/keybytes#credits) • [License](https://github.com/notaroomba/keybytes#license)

[![Keybytes](https://github.com/notaroomba/keybytes/raw/main/assets/keybytes.jpg)](https://github.com/notaroomba/keybytes/blob/main/assets/keybytes.jpg)

## Video Demo

keybytes-demo.mp4

## Key Features

- **Rearrangeable layout** where every key is its own 1u PCB
- **CH32V003** RISC-V microcontroller on every key
- **STM32WB55RGV6** main board with Bluetooth and USB
- **Pogo pins on all 4 sides** of every key for power and I2C
- **Magnetic cases** to hold the modules together
- **Key programming** from the main board over pogo pins
- **2MB QSPI flash** (W25Q16JV) to store the key firmware
- **Battery charging** with the BQ25895 and solder pads for a 1S LiPo
- **Buck-Boost converter** (TPS631000)
- **RGB status LED**
- **USB-C** with ESD protection
- **Ceramic Bluetooth antenna**

## PCB

Designed in KiCad! There are two boards, the Keybit and the Keyword, and both are 1u (19.05mm x 19.05mm).

### Keybit

A single key with a Cherry MX switch and a CH32V003. Every side has pogo pins for 3.3V, GND and I2C so it can connect to the keyword or to other keybits, and the pads on the corners are for programming.

**Schematic:**

[![Keybit Schematic](https://github.com/notaroomba/keybytes/raw/main/assets/journal/keybit_schematic.png)](https://github.com/notaroomba/keybytes/blob/main/assets/journal/keybit_schematic.png)

**Layout:**

[![Keybit Layout](https://github.com/notaroomba/keybytes/raw/main/assets/readme/keybit-layout.png)](https://github.com/notaroomba/keybytes/blob/main/assets/readme/keybit-layout.png)

### Keyword

The main board that the keybits connect to. It is split into two PCBs that stack on top of each other. The top one has the STM32WB55RGV6, the antenna and the flash, and the bottom one has the USB-C, the charger and the buck-boost converter. The 3 pogo pins on the top are for programming the keybits.

**Schematic:**

[![Keyword Schematic](https://github.com/notaroomba/keybytes/raw/main/assets/readme/keyword-schematic.png)](https://github.com/notaroomba/keybytes/blob/main/assets/readme/keyword-schematic.png)

**Layout:**

[![Keyword Layout](https://github.com/notaroomba/keybytes/raw/main/assets/readme/keyword-layout.png)](https://github.com/notaroomba/keybytes/blob/main/assets/readme/keyword-layout.png)

**3D View:**

[![Keyword 3D View](https://github.com/notaroomba/keybytes/raw/main/assets/readme/keyword-3d-angle.png)](https://github.com/notaroomba/keybytes/blob/main/assets/readme/keyword-3d-angle.png)

[![Keyword 3D View](https://github.com/notaroomba/keybytes/raw/main/assets/readme/keyword-3d-bottom.png)](https://github.com/notaroomba/keybytes/blob/main/assets/readme/keyword-3d-bottom.png)

## Case

Custom designed cases in [Onshape](https://cad.onshape.com/documents/16fd731b2222007142e5340e/w/1583a55f05f5e1a2f8aaf622/e/787b8ffe055f15084c3845af), the STEP files are in the [`cad`](https://github.com/notaroomba/keybytes/blob/main/cad) folder.

[![Keybit and Keyword cases assembled in Onshape](https://github.com/notaroomba/keybytes/raw/main/assets/readme/keyboard.png)](https://github.com/notaroomba/keybytes/blob/main/assets/readme/keyboard.png)

## Credits

This project uses:

- [KiCad](https://www.kicad.org/)
- [OnShape](https://www.onshape.com/) for case design
- [Blender](https://www.blender.org/) for 3D renders
- [STM32CubeMX](https://www.st.com/en/development-tools/stm32cubemx.html)
- [Hack Club Forge](https://forge.hackclub.com/)

## You may also like...

- [Ember](https://github.com/NotARoomba/ember) - A USB-C powered reflow hotplate with Bluetooth
- [Cyberboard](https://github.com/NotARoomba/Cyberboard) - A Raspberry Pi Pico-sized STM32 development board with Bluetooth
- [Trace](https://github.com/NotARoomba/Trace) - A comprehensive PCB ruler with reference footprints
- [Linea](https://github.com/NotARoomba/Linea) - An EMR tablet

## License

MIT

* * *

> [notaroomba.dev](https://notaroomba.dev)  ·  GitHub [@NotARoomba](https://github.com/NotARoomba)  ·
