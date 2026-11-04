---
title: Nvidia RTX Spark encrypted firmware slows inference speed to 35% on Linux
link: https://www.mindstudio.ai/blog/linux-nvidia-rtx-spark-laptop
source: hnrss-org
published: 2026-11-04T08:46:29Z
updated: 2026-11-04T08:46:29Z
first_seen: 2026-11-04T15:43:16.719955117Z
authors:
- novaRom
labels:
- linux dual boot laptop
- nvidia dgx spark
- nvidia gb10 linux
- rtx spark linux
- ubuntu omarchy rtx spark
- wsl ai performance
content: extracted
html: 2026-11-04-nvidia-rtx-spark-encrypted-firmware-slows-inference-speed.html
remote_preview:
  url: https://i.mscdn.ai/o/iZl0kkZU2R9KXyy7/a/3V2ICO3oSq2QZPii/_published/generated-images/0d8c9668-cbfe-4ecf-bb31-1a7fff7102be.png?fm=auto&w=1200&h=630&fit=crop
---

Yes, but it comes with a serious catch. Linux installs and runs on RTX Spark laptops like the MSI Prestige N16 Flip AI Plus, including GPU acceleration through Nvidia’s open kernel driver. The problem is clock speed: once the laptop is plugged into wall power, the GPU gets locked to roughly 780 MHz on Linux instead of the 2 GHz-plus it hits on Windows. That turns a chip capable of around 65 tokens per second on a 120 billion parameter model into one struggling to break 14 tokens per second, for reasons tied to power management firmware that Linux currently can’t read.

## TL;DR

- **Dual-booting Linux works** on the MSI Prestige RTX Spark laptop, with a full desktop, GPU acceleration via Nvidia’s open driver, 4K output at 120Hz, and working audio, keyboard, and touchpad once configured correctly.
- **A hidden Windows-only component called the Nvidia Platform Controllers and Framework** manages power delivery between CPU and GPU, and it has no Linux equivalent, which is the root cause of the slowdown.
- **GPU clock speed collapses to about 780 MHz on Linux when plugged in**, compared to roughly 2.2 GHz on Windows, because the firmware’s power table is encrypted and only the Windows driver can decrypt it.
- **Benchmarks on GPT-OSS-120B show the gap clearly**: Windows hit 65.4 tokens per second, WSL hit 65.1, the Nvidia DGX Spark hit 61, and native Linux on the same laptop managed only 13.9.
- **Memory allocation is a separate headache**, since the laptop reserves 96GB of unified memory for the GPU by default and only Windows exposes a setting to change that split, which is required before Linux can see more than a sliver of RAM.
- **WSL (Windows Subsystem for Linux) is the practical middle ground today**, since it rides on the Windows driver stack and keeps full GPU boost whether the laptop is plugged in or on battery, while still running Linux-first AI tools like vLLM.
- **A real fix depends on Nvidia**, specifically on porting the platform controller’s power management logic into its Linux driver, which would effectively turn the laptop into a portable version of the DGX Spark.

![](https://i.mscdn.ai/o/iZl0kkZU2R9KXyy7/a/3V2ICO3oSq2QZPii/_published/generated-images/198a4092-7568-45db-8dce-64458e46e9b0.png?fm=webp&w=1400)

Self-host\
Remy.

On-prem or in your private cloud. Remy and every app you build stay inside your walls.

## What makes the RTX Spark laptop different from a normal Windows PC?

The RTX Spark platform, which ships in machines like the MSI Prestige, the Asus ProArt, and the Dell XPS, uses the same GPU design as Nvidia’s DGX Spark desktop unit. But where the DGX Spark ships with Nvidia’s own Linux build out of the box, every one of these laptop variants ships with Windows on ARM.

That distinction matters because Windows on ARM with CUDA support is new. ARM-based Windows machines have existed for a few years, but pairing that architecture with Nvidia’s CUDA stack only became possible with this chip. The laptops also support unified memory configurations up to 128GB, shared between CPU and GPU, which is unusual for a laptop and central to why they’re being marketed at AI developers in the first place.

Because so much of the AI tooling ecosystem, including vLLM and SGLang, is built Linux-first, developers who buy these machines naturally want to know whether they can ditch Windows, or at least dual-boot, without losing performance.

## How do you actually install Linux on it?

The process is not plug-and-play. A first attempt using Omarchy (a Linux distribution with Arch roots) failed because, while it already supported the chip on Asus and Dell hardware, the MSI Prestige behaved differently, producing a flickering, unstable display during boot.

Switching to Debian worked more reliably, but still needed workarounds. The installer couldn’t connect to Wi-Fi, so installation required a USB Ethernet adapter. Out of the box, Debian also didn’t recognize the keyboard, touchpad, or GPU, because none of the laptop-specific hardware support was present in a vanilla kernel.

Going back to Omarchy solved the driver problem, since its kernel already bundled Nvidia’s open-source driver. At that point Linux booted properly, but could only see 28GB of the laptop’s 128GB of unified memory. That’s because the laptop’s firmware reserves 96GB for GPU use, and that split is only adjustable from Windows. Dropping the Windows-side GPU memory reservation down to 4GB freed up 120GB for Linux to see once booted. After that adjustment, the laptop ran a full desktop at 4K and 120Hz, with working audio, touchpad, keyboard, and CUDA acceleration.

Getting video output over HDMI during the install process was its own obstacle. Early in the process, the laptop would only display video on its own internal screen, not over HDMI, which required pointing an external camera at the laptop’s screen just to see what was happening during remote-controlled installation steps.

## How does Linux performance compare to Windows and WSL?

This is where the story turns. With GPT-OSS-120B running on the GPU, Windows produced 65.4 tokens per second. WSL, which runs a Linux environment inside Windows, hit 65.1 tokens per second, nearly identical. The DGX Spark desktop unit, running Nvidia’s native Linux build, came in at 61 tokens per second. Native Linux on the laptop, however, dropped to 13.9 tokens per second, roughly five times slower.

![](https://i.mscdn.ai/o/iZl0kkZU2R9KXyy7/a/3V2ICO3oSq2QZPii/_published/generated-images/6777a823-6890-41fa-b8c2-f5ab8409315b.png?fm=webp&w=1400)

Every team builds with AI.\
*Nothing gets lost.*

The gap showed up again with vLLM running eight concurrent clients: WSL reached 127 tokens per second while native Linux reached 39.5. A longer benchmark run told the same story in time rather than throughput. Windows finished in about 8 minutes while holding the GPU near 2.2 GHz. Linux took 37 minutes, with the GPU parked around 790 MHz the entire time. Temperatures stayed low, between 40 and 47°C, so this isn’t a thermal throttling issue. The GPU simply isn’t being allowed to run at full speed.

Oddly, the direction of the problem flips depending on power source. Unplugged, Linux hit around 61 to 67 tokens per second on LM Studio, comparable to Windows. Plugged in, that same test dropped to about 15 tokens per second. Clock speed told the same story: around 1,800 MHz unplugged under load, and down to roughly 780 MHz when connected to wall power.

## Why does plugging in the laptop slow Linux down?

The cause traces back to a component visible in Windows Device Manager called the Nvidia Platform Controllers and Framework. This piece of firmware and driver logic manages the shared power budget between CPU and GPU, since a laptop (unlike the desktop DGX Spark) has to balance power delivery between both chips rather than feeding the GPU on its own line.

Disabling that controller in Windows reproduced the slowdown directly: tokens per second dropped from around 83 to about 31, and clock speed fell from roughly 2 GHz down to the same 786 MHz seen on Linux when plugged in. Re-enabling the controller restored normal speed immediately. That confirms the controller, not the kernel, the distribution, or the Nvidia GPU driver itself, is what’s holding Linux back.

The deeper issue is that the firmware hands this controller a table of power settings that is encrypted. Windows has the key to decrypt it. Linux currently does not, and no software fix attempted during testing could enable it, including one attempt that triggered a kernel panic. Extracting the decryption key from the Windows driver was floated as a theoretical workaround, but that would amount to a fragile, single-machine hack rather than something that works broadly. A real fix needs to come from Nvidia shipping equivalent power management logic in its Linux driver.

## Is WSL a good substitute for native Linux right now?

For most developers, yes, at least for now. WSL runs on top of the Windows driver stack, so it inherits Windows’ full GPU boost behavior whether the laptop is plugged in or running on battery. It also runs Linux-first AI tools like vLLM and SGLang without the clock throttling problem that affects native Linux installs.

The one adjustment needed is memory. WSL reserves only 14GB by default, which isn’t enough for large model workloads on a 128GB unified memory machine, so that allocation needs to be increased manually. Once that’s done, WSL benchmarks land close to native Windows performance, making it the more practical choice until Nvidia addresses the Linux driver gap directly.

## Frequently Asked Questions

### Can the Nvidia RTX Spark laptop dual-boot Windows and Linux?

Yes. Dual-booting was demonstrated on the MSI Prestige RTX Spark laptop using Omarchy, a Linux distribution that already included Nvidia’s open kernel driver for this chip, after an initial attempt with Debian required manual driver and networking workarounds.

### Why is Linux so much slower than Windows on this laptop when plugged in?

Developers have their tools.Enterprises have Remy.

Developers

\>

add rate limiting to the API\
**●** Reading src/server.ts\
**●** Editing 3 files

\+ import

{ limit }

from

'rate'\
*\- app.use(cors())*\
**✓** Tests passed · ready for PR

Enterprises

Loan Approvals● *Live*

SOC 2 Role-based access Self-hosted

Mortgage · $412,000*Approved*

Small business · $85,000*Approved*

Auto · $31,500*Approved*

A Windows-only firmware component called the Nvidia Platform Controllers and Framework manages power sharing between the CPU and GPU. Without a Linux equivalent, the GPU’s clock speed gets locked around 780 MHz when the laptop is connected to power, compared to roughly 2.2 GHz on Windows.

### Does unplugging the laptop fix the Linux performance problem?

It helps significantly. On battery power, Linux GPU clocks and token throughput come much closer to Windows levels. The severe slowdown specifically appears when the laptop is plugged into wall power, which is the opposite of what most users would expect.

### Is WSL the same as running native Linux on the RTX Spark?

Not exactly, but it’s close in practice. WSL runs a Linux environment on top of the Windows kernel and driver stack, so it retains Windows’ GPU clock boost behavior and avoids the throttling issue, while still supporting Linux-native AI tools like vLLM.

### Will Nvidia fix Linux GPU performance on RTX Spark laptops?

That depends on Nvidia shipping the platform controller’s power management logic in its Linux driver. The encrypted firmware power table currently can only be decrypted by Windows, and no workaround found so far restores full clock speed on Linux without it.
