---
title: 'Show HN: Esp32sim – Run ESP32 firmware in the browser'
link: https://github.com/mikroverk/esp32sim
source: hnrss-org
published: 2026-10-17T13:51:08Z
updated: 2026-10-17T13:51:08Z
first_seen: 2026-10-18T06:32:49.699637230Z
authors:
- joakimeriksson
summary: 'Hi HN, I’ve been working on esp32sim, an open-source ESP32 emulator that runs in the browser. The fun part of the demo is that there is no ESP32 involved — the firmware is executing in the emulator. The goal is to make ESP32 software easier to experiment with, test and simulate without always needing physical hardware connected. Source: https://github.com/mikroverk/esp32sim I made a short video showing it in action: https://youtu.be/waoWFilTWEc I’m particularly interested in feedback from embedded developers: what would an emulator like this need to support before you’d actually use it for development, testing or CI? Comments URL: https://news.ycombinator.com/item?id=50131189 Points: 149 # Comments: 15'
content: extracted
html: 2026-10-17-show-hn-esp32sim-run-esp32-firmware-in-the-browser.html
---

[![esp32sim logo: a smiling chip in an emulator window](https://github.com/mikroverk/esp32sim/raw/main/docs/assets/esp32sim-logo-icon.png)](https://github.com/mikroverk/esp32sim/blob/main/docs/assets/esp32sim-logo-icon.png)

esp32sim is an ESP32 emulator written in Rust. It runs real ESP32 firmware on your computer or in a browser tab, without the hardware.

It boots the chip's own mask ROM, the real second-stage bootloader and your unmodified application. It also models the board around the chip: displays, touch, LEDs, buttons, audio, camera and WiFi.

Try it in your browser, nothing to install: [https://mikroverk.github.io/esp32sim/](https://mikroverk.github.io/esp32sim/)

## Supported chips

| Chip               | CPU                  | Status | Boards                                                                    |
| ------------------ | -------------------- | ------ | ------------------------------------------------------------------------- |
| ESP32-S3           | 2 x Xtensa LX7       | mature | Atech 14-port kit, Waveshare Touch-LCD-4B, Touch-AMOLED-1.8, ESP32-S3-CAM |
| ESP32-C3           | RISC-V RV32IMC       | draft  | console only                                                              |
| ESP32-C6           | RISC-V RV32IMAC      | draft  | Waveshare ESP32-C6-LCD-1.47, with the 802.15.4 radio                      |
| ESP32 (classic)    | 2 x Xtensa LX6       | early  | LilyGO TTGO T-Display                                                     |
| ESP32-P4 (rev < 3) | 2 x RISC-V RV32IMAFC | draft  | console only                                                              |

## Features

- The full boot chain: mask ROM, bootloader, FreeRTOS, then your app. No patched firmware.
- WiFi with the unmodified Espressif WiFi library. Firmware joins a virtual access point, gets DHCP, DNS and NTP, and can reach the real network, HTTPS included.
- Board models, so firmware can draw on a display, read touch, light LEDs and play sound.
- Speed: the S3 has a block cache and a JIT, and runs hello\_world about 30 times faster than real time.
- A WebAssembly build of the same emulator for the browser.
- Comparisons with real chips: the S3 in JTAG lock-step, the C3 and C6 by console diff. See [hardware testing](https://github.com/mikroverk/esp32sim/blob/main/docs/hardware-testing.md).

## Demos

These run on the [demo site](https://mikroverk.github.io/esp32sim/):

- Linux 6.11 on the ESP32-S3 ([svermigo/Linux-on-esp32-S3](https://github.com/svermigo/Linux-on-esp32-S3)), with a login shell.
- The Atech kit's Pocket Synth and its C64 SID jukebox ([atech-firmware](https://github.com/joakimeriksson/atech-firmware)).
- An LVGL touch panel at 480x480 with a SID player.
- [pocket-tank](https://github.com/mediacutlet/pocket-tank), where a small transformer model steers the fish.
- An 802.15.4 energy scanner, and two Contiki-NG motes running RPL/IPv6, on the ESP32-C6.
- MicroPython on a classic ESP32 T-Display.

You can also load your own firmware for any of the chips.

## Quick start

No Rust needed: the [releases](https://github.com/mikroverk/esp32sim/releases) carry prebuilt binaries for macOS and Linux, and [docs/quick-start.md](https://github.com/mikroverk/esp32sim/blob/main/docs/quick-start.md) takes your own build from the tarball to a run in three steps. From a checkout:

```shell
cargo build --release
tools/fetch-demo-assets.sh --no-linux       # downloads the mask ROMs, which are not in the repo

FW=web/wasm/fw/public                       # demo firmware included in the repo
./target/release/esp32sim --boot rom --rom web/wasm/fw/esp32s3_rev0_rom.elf \
    --bootloader $FW/atech-bootloader.bin --ptable $FW/atech-ptable.bin --app $FW/atech-firmware.bin \
    --script $FW/atech-script1.txt --wav out.wav --tft-png tft.png --max-seconds 5
```

This boots the Atech synth firmware, plays a short script, and saves the audio to `out.wav` and the display to `tft.png`. Use `esp32sim-c3`, `esp32sim-c6`, `esp32sim-p4` or `--chip esp32` for the other chips.

[Getting started](https://github.com/mikroverk/esp32sim/blob/main/docs/getting-started.md) covers the rest: your own ESP-IDF builds, each chip, WiFi, scripts, the browser build and debugging.

## Documentation

- [Quick start](https://github.com/mikroverk/esp32sim/blob/main/docs/quick-start.md): install the release, fetch the mask ROM, run your own build, read the result
- [Getting started](https://github.com/mikroverk/esp32sim/blob/main/docs/getting-started.md): running firmware on each chip, WiFi, scripts, the browser build
- [CLI reference](https://github.com/mikroverk/esp32sim/blob/main/docs/cli.md): every flag, environment variable and output file
- [Boards](https://github.com/mikroverk/esp32sim/blob/main/docs/boards.md): what each board models, and how to add one
- [Architecture](https://github.com/mikroverk/esp32sim/blob/main/docs/architecture.md): how the emulator is put together
- [Peripherals](https://github.com/mikroverk/esp32sim/blob/main/docs/peripherals.md): which hardware blocks are modelled, and how completely
- [Hardware testing](https://github.com/mikroverk/esp32sim/blob/main/docs/hardware-testing.md): how the emulator is compared with real chips
- [All documents](https://github.com/mikroverk/esp32sim/blob/main/docs/README.md): chip notes, design decisions and plans

## Contributing

Bug fixes, peripherals, boards and new chips are welcome. [CONTRIBUTING.md](https://github.com/mikroverk/esp32sim/blob/main/CONTRIBUTING.md) lists what a pull request needs; these are the same checks CI runs. Security problems go through [SECURITY.md](https://github.com/mikroverk/esp32sim/blob/main/SECURITY.md), not the public issue tracker.

## Credits and license

esp32sim is released under the [MIT License](https://github.com/mikroverk/esp32sim/blob/main/LICENSE).

Authors: Joakim Eriksson, RISE ([@joakimeriksson](https://github.com/joakimeriksson)), Alice ([@aliceisjustplaying](https://github.com/aliceisjustplaying)) and Mattia Fiumara, Schematik ([@mfiumara](https://github.com/mfiumara)), together with [other contributors](https://github.com/mikroverk/esp32sim/graphs/contributors).

The emulator was written from the ESP-IDF register headers, the Xtensa core configuration that ships with ESP-IDF, the RISC-V specification, the chips' technical reference manuals and observed firmware behaviour. QEMU was consulted only to confirm instruction semantics; no code was copied.

esp32sim is unofficial and not affiliated with Espressif Systems.

Third-party code, data and demo files keep their own licenses:

- pocket-tank ([mediacutlet/pocket-tank](https://github.com/mediacutlet/pocket-tank), MIT). Its bootloader, partition table and app are in `web/wasm/fw/public/` with `pocket-tank-LICENSE.txt`; its model is downloaded.
- MicroPython ([micropython.org](https://micropython.org/download/ESP32_GENERIC/), MIT): the official ESP32\_GENERIC v1.29.0 image used by the T-Display demo, downloaded by `tools/fetch-micropython.sh` and not in the repo. The demo's app and filesystem are in `examples/ttgo-tdisplay`, with Russ Hughes's st7789py driver and fonts (MIT); the notice is in `web/wasm/fw/public/tdisplay-NOTICE.txt`.
- Mask ROM ELFs ([espressif/esp-rom-elfs](https://github.com/espressif/esp-rom-elfs), Apache-2.0), downloaded, not in the repo.
- xterm.js ([xtermjs/xterm.js](https://github.com/xtermjs/xterm.js), MIT), downloaded, not in the repo.
- The Linux image ([svermigo/Linux-on-esp32-S3](https://github.com/svermigo/Linux-on-esp32-S3), GPL-3.0), downloaded, not in the repo.
- ESP-IDF ADC calibration data (Espressif, Apache-2.0): the curve-fitting coefficients and S3 ADC1 calibration from #165, in `esp-periph/src/sar_adc.rs`; see `esp-periph/LICENSE-ADC`.
