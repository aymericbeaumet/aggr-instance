---
title: Raspberry Pi Zero 3W and Pi 4 Model A spotted in device trees
link: https://bret.dk/raspberry-pi-zero-3-w-pi-4-model-a/
source: hnrss-org
published: 2026-10-27T20:48:49Z
updated: 2026-10-27T20:48:49Z
first_seen: 2026-10-28T05:25:47.188168527Z
authors:
- sthlmb
labels:
- raspberry pi
- single board computers
content: extracted
html: 2026-10-27-raspberry-pi-zero-3w-and-pi-4-model-a-spotted-in-device.html
remote_preview:
  url: https://bret.dk/wp-content/uploads/2026/10/image.png
  alt: raspberry pi zero 3 w
---

Two new Raspberry Pi boards have quietly turned up in Raspberry Pi’s Linux kernel tree, and neither of them is quite what I’d have guessed. Device tree files for a Raspberry Pi Zero 3 W and a Raspberry Pi 4 Model A have landed ([Phoronix spotted it first](https://www.phoronix.com/news/Raspberry-Pi-Zero-3-W-Pi-4-A)), and whilst there’s no official announcement just yet, device tree files tend to give away a surprising amount if you can decipher them and I gave it my best shot.

## Raspberry Pi Zero 3 W: A Pi 4 Shrunk Down?

The Raspberry Pi Zero 2 W launched almost exactly five years ago (tomorrow, in fact), so a successor has been a loooooong time coming. If you want a refresher on where things stood back then, my [Raspberry Pi Zero 2 W vs Zero W benchmarks](https://bret.dk/raspberry-pi-zero-2-w-benchmarks-comparison/) piece covers the jump from the original pretty well.

The headline from the [Zero 3 W device tree](https://raw.githubusercontent.com/martamomotko/linux/1afad193173c7e17fd1d4e802e5e2d7c8ee3fc09/arch/arm/boot/dts/broadcom/bcm2711-rpi-zero-3-w.dts) is the SoC. It’s the [Broadcom BCM2711](https://sbc.compare/broadcom-bcm2711), the same quad-core Cortex-A72 chip that powers the [Raspberry Pi 4](https://sbc.compare/raspberry-pi-4), replacing the [Zero 2 W’s](https://sbc.compare/raspberry-pi-zero-2-w) quad-core 1GHz Cortex-A53 RP3A0. If you want an idea of the sort of gap we’re talking about, it’s worth having a look at [how those two compare on sbc.compare](https://sbc.compare/raspberry-pi-4/raspberry-pi-zero-2-w). That’s a big step up in per-core performance, and if they can get anything close to Pi 4 clocks in that tiny footprint, it’ll be a hell of a lot of compute for a Zero. I do wonder how toasty it’s going to get though, as the BCM2711 isn’t exactly known for running cool, and there’s not a lot of board for the heat to spread into. The device tree doesn’t tell us anything about clock speeds, so we’ll have to wait and see whether it’s been dialled back.

![image](https://bret.dk/wp-content/uploads/2026/10/image.png "Raspberry Pi Zero 3 W and Pi 4 Model A on the Way? 17")

Beyond the SoC, here’s what else stood out to me:

- **eMMC on a Zero?** The storage controller is configured with an 8-bit bus width and a comment describing it as driving “the eMMC/SD card”, which only really makes sense for eMMC. I’d love to see it, but I’m not expecting it to make the final board, as it’d push the price up on a range that lives and dies by its price tag. My money is on this being a leftover or an option kept open during development, and microSD being what we get.
- **An SPI flash bootloader.** The GPIO names include SPI flash lines, which suggests the Zero 3 W gets the same EEPROM-based bootloader as the Pi 4 (the Zero 2 W never had one). That should bring the Pi 4’s boot options along with it, and the ability to [update the bootloader](https://bret.dk/update-raspberry-pi-bootloader/) in the usual way.
- **A single HDMI output**, with the second HDMI controller permanently disabled. No surprise there given the form factor.
- **A power button** defined on GPIO 46 and mapped to KEY\_POWER, so we may be getting a physical button (or at least a header for one) much like the Pi 5.
- **Wi-Fi and Bluetooth** wired up in the same way as the rest of the BCM2711 family, with a single ACT LED for status.
- **The usual 40-pin GPIO layout** (GPIO 0-27 with the ID EEPROM pins), so your existing HATs should be fine.

There are a couple of more speculative bits too. The PCIe controller is enabled and there are lines named PCIE\_PWR\_EN and PCIE\_DET\_WAKE, and the camera side pulls in a 4-lane CSI configuration rather than the 2-lane one the Pi 4 Model B uses. It’s worth noting that both device trees share a lot of copy-and-paste DNA (the Zero 3 W even has a duplicated “PWR\_GPIO” line name), so I wouldn’t read too much into either of those until we see an actual board. I’d love to see PCIe exposed on a Zero, but I’m not holding my breath.

## Raspberry Pi 4 Model A: The Return of the A

The second surprise is a [Raspberry Pi 4 Model A](https://raw.githubusercontent.com/martamomotko/linux/c17dd5f6e7accd9b62a7f272c6e69e40efb7563c/arch/arm/boot/dts/broadcom/bcm2711-rpi-4-a.dts). We haven’t had a Model A since the Raspberry Pi 3 Model A+ back in 2018, and the formula has always been the same: take the Model B, strip out the bits most people can live without (RJ45, some USB ports etc), and knock a chunk off the price. The Pi 4 Model A looks to follow that recipe pretty closely, though the price comment may not quite hold. Let’s see!

- **No Ethernet.** The Ethernet controller isn’t enabled anywhere, and the GPIO lines that drive the Ethernet PHY on the Model B have been repurposed. Very much in keeping with previous Model A boards.
- **A single HDMI output** rather than the Model B’s dual micro HDMI.
- **No 3.5mm audio/composite jack.** The analogue audio pins are empty and the composite video encoder is disabled, so audio will be over HDMI (or USB/I2S if you’re that way inclined).
- **microSD only**, with the same 1.8V signalling support as the Model B for the faster UHS card modes.
- **External antenna support.** This is the one I find most interesting. There are two antenna control lines (ANT\_CTRL1 and ANT\_CTRL2) and overlay parameters named `ant1`, `ant2` and `noant`, which is exactly how the Compute Module 4 switches between its onboard antenna and an external U.FL connector. If that carries over to the final board, it’d be a lovely addition for anyone sticking a Pi in a metal enclosure.
- **PWR and ACT LEDs**, the same SPI flash bootloader setup as the Model B, and the same power button definition as the Zero 3 W.

The device tree doesn’t tell us anything about USB. On the Model B, the USB 3 ports made use of the PCIe bus, and the PCIe controller is enabled here too, so USB 3 is possible, but the 3 Model A+ only had a single USB-A port and I’d probably expect the same here.

![image 1](https://bret.dk/wp-content/uploads/2026/10/image-1.png "Raspberry Pi Zero 3 W and Pi 4 Model A on the Way? 18")\
The previous Model A, the Raspberry Pi 3 Model A from 2017

## How Do They Compare?

Here’s a quick side-by-side of what the device trees tell us, with the Pi 4 Model B thrown in for good measure. Anything marked with an asterisk is my interpretation of the device tree rather than something it states outright.

|                              | Raspberry Pi Zero 3 W | Raspberry Pi 4 Model A        | Raspberry Pi 4 Model B |
| ---------------------------- | --------------------- | ----------------------------- | ---------------------- |
| **SoC**                      | Broadcom BCM2711      | Broadcom BCM2711              | Broadcom BCM2711       |
| **CPU**                      | Quad-core Cortex-A72  | Quad-core Cortex-A72          | Quad-core Cortex-A72   |
| **HDMI**                     | 1x                    | 1x                            | 2x micro HDMI          |
| **Ethernet**                 | No                    | No                            | Gigabit                |
| **Wi-Fi/Bluetooth**          | Yes                   | Yes, with antenna switching\* | Yes                    |
| **Storage**                  | microSD (unless..)    | microSD                       | microSD                |
| **Analogue Audio/Composite** | No                    | No                            | Yes (3.5mm)            |
| **SPI Flash Bootloader**     | Yes\*                 | Yes\*                         | Yes                    |
| **Power Button**             | Yes\*                 | Yes\*                         | No                     |
| **LEDs**                     | ACT                   | PWR, ACT                      | PWR, ACT               |

## What We Don’t Know (Yet)

Quite a lot, I’m afraid. Device trees don’t describe RAM, so there’s no word on memory configurations, and there’s nothing on pricing, clock speeds, connector types or a release date either.

Pricing is the big one for me. Memory costs haven’t exactly been kind lately (I covered the last round of increases alongside the [1GB Raspberry Pi 5 launch](https://bret.dk/1gb-raspberry-pi-5-launched-alongside-price-increases/)), and a lower-cost Pi 4 variant makes a lot of sense in that context. The BCM2711 is a mature chip at this point, it’s well supported in software, and a cut-down board gives people who don’t need a Pi 5 something cheaper to reach for. The Zero 3 W is a tougher one to call though, as the Zero range has always lived and died by its price, and I can’t imagine the BCM2711 being as cheap to build around as the RP3A0.

## Thoughts

On the whole, I like what I’m seeing here. The Zero 3 W looks like the upgrade a lot of people have been asking for since the Zero 2 W landed, and the 4 Model A feels like a sensible way of squeezing more life out of the Pi 4 platform at a time when components aren’t getting any cheaper. The external antenna support on the 4 Model A is a really nice touch if it makes it to the final board.

Saying that, everything above comes from device tree files rather than an official spec sheet, so mileage may vary when the real thing turns up. Kernel support landing ahead of launch is usually a good sign that it’s not far off, and you can bet I’ll be throwing both the Zero 3 W and the 4 Model A onto [sbc.compare](https://sbc.compare) as soon as I can get one in my hands.

Which of the two are you more excited about? Do let me know in the comments!

**Sources**\
[bcm2711-rpi-zero-3-w.dts](https://raw.githubusercontent.com/martamomotko/linux/1afad193173c7e17fd1d4e802e5e2d7c8ee3fc09/arch/arm/boot/dts/broadcom/bcm2711-rpi-zero-3-w.dts)\
[bcm2711-rpi-4-a.dts](https://raw.githubusercontent.com/martamomotko/linux/c17dd5f6e7accd9b62a7f272c6e69e40efb7563c/arch/arm/boot/dts/broadcom/bcm2711-rpi-4-a.dts)

**Related posts:** [Raspberry Pi Zero 2 W vs Zero W Benchmarks – Pi Zero Showdown](https://bret.dk/raspberry-pi-zero-2-w-benchmarks-comparison/) – [1GB Raspberry Pi 5 Launched Alongside Price Increases](https://bret.dk/1gb-raspberry-pi-5-launched-alongside-price-increases/)

![](https://secure.gravatar.com/avatar/a8cf0b10aa80a9ceaf517d3a6c6a95798383ac954ba8c351d57ae9617e581b8a?s=200&d=mm&r=g)

##### [Bret](https://bret.dk/author/bret/ "Author Bret")

Bret has worked with Raspberry Pi computers for almost 10 years now and in that time he's benchmarked and tested over 30 Single Board computers. In his day job, he's a systems administrator for a large cloud computing provider.
