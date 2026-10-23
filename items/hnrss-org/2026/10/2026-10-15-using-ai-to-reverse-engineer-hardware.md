---
title: Using AI to reverse engineer hardware
link: https://blogs.gnome.org/uraeus/2026/10/15/aiandreverseengineerhardware/
source: hnrss-org
published: 2026-10-15T14:35:15Z
updated: 2026-10-15T14:35:15Z
first_seen: 2026-10-16T04:55:12.775129010Z
authors:
- ErenayDev
labels:
- fedora
- general
- gnome
- multimedia
content: extracted
html: 2026-10-15-using-ai-to-reverse-engineer-hardware.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50104299
  num_comments: 127
  points: 147
---

After my mother passed away I ended up with a big box full of old film negatives containing photos from childhood family trips and similar. So I decided to buy a negative photo scanner and found that the Plustek 8200i was supported under Linux so I bought that. Only to discover that the chipset in the scanner had changed to one that wasn’t supported under Linux anymore. So I had some success making Linux configuration tools for some of my hardware using Claude code, like my [Dell Camera controller app](https://gitlab.com/cschalle/appfordellwebcam) so I thought, maybe Claude can help me get a working SANE driver. I basically got stuck and got nowhere for the longest time and even basically gave up on the project for a few Months.

Then I saw a [post on X from Tom Crider aka Glorious Eggroll](https://x.com/GloriousEggroll/status/2103816580966830314) talking about how he had managed to fix a mouse driver using AI tooling. He basically described how he set it up so that the AI agent had full control of a Windows instance to capture data needed to fix the driver. So I pasted his tweet into Claude and asked it to help me set up a windows virtual machine to do the same things Tom as doing for my scanner. I think the combination of having the ability to run scans under windows and capture the USB data while doing so worked wonders, probably combined with improvements to Claude code made a huge difference. Within a few hours I had my first ever working scan under Linux, sure with some quality issues, but 100% recognizable as the image in the negative. Some days later I had a near perfect driver, including support for Infrared scanning ready to go, only to discover that in the time between me giving up and then trying again someone had committed code to SANE to support the scanner already…..it happens sometimes. I also used Claude to write an application for doing negative scanning, which I named [Negative Vibes](https://github.com/cschalle/negative-vibes), thought that was a cute name.

Anyway, with the success of my scanner effort I thought, I wonder if I can use this to finally fix the last issues with MIPI camera support on my laptop. So I told claude to re-use the harness I had created and try to fix the MIPI camera on my laptop by updating the kernel driver and libcamera as needed. I started the job in the morning and by the end of the day I had [a much improved situation](https://cschalle.github.io/mipi-fixes-x1-aura/) with patches for both the kernel and libcamera ready.

So I thought that this might be worthwhile sharing with the world, and while anyone could start from scratch I did ask Claude to make the harness generic and document it to make the pathway for anyone else who wants to try this on their hardware a little easier. Writing a driver for undocumented hardware has always been tedious. You capture bus traffic, stare at hex dumps, and form hypotheses about what each register or control transfer does. You then test them one at a time, usually for weeks. None of it is conceptually hard, but it takes a lot of patience and skills.

That kind of work suits an AI model well. It can read thousands of lines of USB capture, notice that a sequence repeats with one byte changing, and propose what that byte means. It can write the test to confirm it, and it doesn’t get bored. The human still decides what to build and checks that it works on real hardware, but the slog shrinks a lot.

**[hw-harness](https://github.com/cschalle/hw-harness)**

Be aware that I did not use hw-harness in this form for my projects as this was basically bespoke code at that time, so this project is an attempt to make the harness more generic and shareable from the scripts I was using while doing the work. What hw-harness is meant to help your Claude or Codex session with:

1\. Record a reference session. Run the vendor’s Windows driver in a KVM virtual machine, with the real device passed through to the guest.\
 2. Capture from outside the VM. Record the traffic on the Linux host with usbmon. It sits below the passthrough layer, so the guest can’t see it.\
 3. Repeat the same operation on Linux with your own driver, and capture that the same way.\
 4. Diff the two traces. Both come from the same instrument, so they compare directly. The differences are your list of driver bugs.

For devices with no bus to tap, such as an internal MIPI camera, the harness uses Windows-side inspection instead. That means INF files, registry data, ACPI tables and device properties.

The approach should be fine in the sense it doesn’t rely on copying any code instead it observes what the hardware does and only extract configuration data from the Windows drivers, so every constant in the new driver traces back to something recorded.

So for those interested I have 3 short descriptions of the projects I did and what was needed.

**Project 1: [a SANE backend for the Plustek OpticFilm 8200i](https://htmlpreview.github.io/?https://github.com/cschalle/OpticFilm8200i-sane/blob/main/docs/comparison/drivers-comparison.html)**

[![comparison of two scanner drivers on same negative-](https://blogs.gnome.org/uraeus/files/2026/10/drivercompare.png)](https://blogs.gnome.org/uraeus/files/2026/10/drivercompare.png)

Comparing the driver output of two drivers available.

The first project is a SANE backend for the newer Plustek OpticFilm 8200i. It uses the Genesys GL128 controller (USB 07b3:1825) and had no Linux support. The older model with the same name uses a GL845 and is already handled by SANE.

The backend supports:

- Color, gray, infrared and color+infrared modes
- Resolutions from 300 to 7200 dpi, at 8 or 16 bits per channel
- Dust and scratch removal from the infrared channel
- An exposure multiplier for dense negatives

It’s packaged as a Fedora RPM, built automatically on every push. I mostly tested it with my own Negative Vibes application as there are not that many options under Linux for negative scanning.

Upstream SANE’s genesys backend also gained support for this scanner in September 2026, so I compared the two. Both resolve the same fine detail at 3600 dpi. Mine is faster at full resolution (34.6 s against 54.4 s) and supports infrared dust removal. Upstream offers more resolution steps. The webpage has an interactive side-by-side comparison you can check out. These are single-unit, single-frame results, so treat them as one data point. [Check out the interactive page I made](https://htmlpreview.github.io/?https://github.com/cschalle/OpticFilm8200i-sane/blob/main/docs/comparison/drivers-comparison.html) showing the current state of the two drivers, hopefully the discoveries I made can be used by the SANE project to fix some smaller items in the upstream driver. Anyway, I put this project up more as a proof of concept due to that since I expect everyone including myself will use the upstream driver once it reaches parity with this one.

**Project 2: [Negative Vibes](https://github.com/cschalle/negative-vibes)**

[![](https://blogs.gnome.org/uraeus/files/2026/10/afterscan-2048x1309.png)](https://blogs.gnome.org/uraeus/files/2026/10/afterscan.png)

Full scan of negative completed including infrared cleaning of image.

A driver alone isn’t much use if scanning negatives is still painful, so I built Negative Vibes. It’s a GNOME app, written in Rust, for scanning colour and black-and-white negatives and slides on any film-capable SANE scanner.

The scan is kept exactly as the scanner delivered it: 16-bit, linear, with the infrared plane. Everything else is a stack of non-destructive settings on top, so you can change the look and export again. It includes:

- Density-space negative inversion with film base and dye-crosstalk correction
- Roll-wide settings with per-frame overrides
- Infrared and software dust removal
- IT8 calibration through ArgyllCMS
- Export to TIFF, JPEG, PNG and DNG, with ICC profiles and metadata

It ships as a Flatpak that includes the scanner backend. It’s tested on the 8200i so far. The goal is to support any film scanner SANE supports. For that, SANE needs a standard way to expose infrared channels, which it doesn’t have yet.

**Project 3: [Fixing the MIPI camera on my ThinkPad](https://cschalle.github.io/mipi-fixes-x1-aura/)**

[![Showing MIPI output before and after fixes. Colors are better than they look as I was sitting in a relatively dark room by the end of the session.](https://blogs.gnome.org/uraeus/files/2026/10/mipifixes.png)](https://blogs.gnome.org/uraeus/files/2026/10/mipifixes.png)

Showing MIPI output before and after fixes. Colors are better than they look as I was sitting in a relatively dark room by the end of the session.

The camera in my Lenovo ThinkPad X1 Carbon Gen 14 (Aura Edition) uses a Sony IMX471 sensor behind an Intel IPU7. On Linux it produced dark images with heavy vignetting and a strong yellow-green cast. mipi-fixes-x1-aura collects the fixes. It’s only relevant to this laptop, which is why it isn’t one of the bigger packaged projects, but I have already submitted the kernel patches to Kate Hsuan and the libcamera patches is being reviewed by Wim Taymans for submission to Libcamera.

Kernel (IMX471 driver):

- A crop-selection bug returned an error before storing the crop rectangle, so libcamera assumed the full pixel array.
- The test-pattern register was declared with the wrong width.
- The analogue gain cap was too low, which cost about 1.8 stops of sensitivity.

libcamera software ISP:

- Lens shading correction was missing entirely, leaving the corners about 2.7 stops darker.
- White-balance statistics were summed at the sensor’s native bit depth against an 8-bit black level. That skewed white balance, and the fix applies to any RAW10/RAW12 sensor.

In a controlled low-light test, frame brightness rose from 31.1 to 90.9. The red/green and blue/green ratios moved from 0.890 and 0.672 to 1.047 and 0.823, close to a USB webcam reference. Blue is still about 17% short in low light, which I put down to grey-world white balance. Fedora 44 RPMs are on the releases page.

What I learned

- Reference captures are a big step. Having the AI compare a known-good trace against a failing one turns a vague “it doesn’t work” into a concrete list of differences. For example for the MIPI camera having a USB camera also connected so Claude could compare the two captures was critical.
- The AI is good at the tedium. That covers decoding captures, writing probes and cross-checking constants. I still have to be the one holding the physical hardware.
- Real hardware is the only judge. Every claim above was checked on the actual scanner or laptop.

If you have hardware with no Linux driver, try hw-harness. Thanks again to Glorious Eggroll for the inspiration.

**NOTE: I am not claiming that my scans or MIPI camera is perfect now, but the scanner gives me decent looking scans and the MIPI camera looks a lot better than before. So over time I am sure we be able to tweak and polish further, but I am happy that it allowed me to move the needle despite lacking the in-depth knowledge for this type of work.**

—
