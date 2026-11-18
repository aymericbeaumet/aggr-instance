---
title: Improving the Wii U's Wii Mode
link: https://garyodernichts.blogspot.com/2026/11/improving-wii-us-wii-mode.html
source: lobste-rs
published: 2026-11-16T22:39:54Z
updated: 2026-11-16T22:39:54Z
first_seen: 2026-11-18T17:57:41.978411823Z
authors:
- garyodernichts.blogspot.com via calvin
labels:
- graphics
- hardware
- reversing
summary: Comments
content: extracted
html: 2026-11-16-improving-the-wii-u-s-wii-mode.html
remote_preview:
  url: https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgddutNwjGyoPAqVySy66feWh7iohqv11w75McVPWIoAW49DaClcX26EhS-ULyPcYmLRoOySk_6BUjpls-O_qcAzf2qrrxyG9sjFrFxvz0yUbd5r0kh-7rAaXDxWnSwwaV-FfKwkXyikJpF-w-IaoGLmwWYlQzrVHadty8c4DZzAjmZUupxrFxXS9ReUjo/w1200-h630-p-k-no-nu/before.png
---

One feature of the Wii U is the backwards compatibility with Wii games through Wii Mode, often referred to as vWii. In theory, this is a really nice way to enjoy Wii games on a modern TV, since the Wii U has HDMI output. Unfortunately, Wii Mode does not look great. Ironically, the output of an original Wii can look better.\
 The two most prominent issues are the force-cropped output and color bleed issues.

|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgddutNwjGyoPAqVySy66feWh7iohqv11w75McVPWIoAW49DaClcX26EhS-ULyPcYmLRoOySk_6BUjpls-O_qcAzf2qrrxyG9sjFrFxvz0yUbd5r0kh-7rAaXDxWnSwwaV-FfKwkXyikJpF-w-IaoGLmwWYlQzrVHadty8c4DZzAjmZUupxrFxXS9ReUjo/w640-h360/before.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEgddutNwjGyoPAqVySy66feWh7iohqv11w75McVPWIoAW49DaClcX26EhS-ULyPcYmLRoOySk_6BUjpls-O_qcAzf2qrrxyG9sjFrFxvz0yUbd5r0kh-7rAaXDxWnSwwaV-FfKwkXyikJpF-w-IaoGLmwWYlQzrVHadty8c4DZzAjmZUupxrFxXS9ReUjo/s1280/before.png) |
| Scaled up image of the chroma bleed issue in Super Mario Sunshine and The Legend of Zelda: Skyward Sword. You can see the green tint on the left side of the characters. (Image credit: Rulumi)                                                                                                                                                                                                                                                                                                                                                                  |

Reports of these issues go back to 2013[^1], shortly after the Wii U was released, but a fix was never provided.

In this blog post we'll take a closer look at the technical architecture of Wii Mode graphics, how two simple typos in the firmware caused these issues, and what modern AMD GPUs have in common with the Wii U.

## The DMCU

So who can we blame for these issues? One candidate is the DMCU. To get a quick overview of the DMCU, I'm quoting fail0verflow's description from their 2014 Wii U blog post[^2]:

> You may have noticed that good old VI (the display controller / Video Interface of ye olde GameCube and Wii) is gone. Huh? How does that work? Turns out they’re emulating that in software and translating its configuration to the R7xx’s CRTC registers. But where does this emulation software run? Sneaky: they added a special microcontroller to the Latte just for this purpose. The DMCU is a [68HC11](http://en.wikipedia.org/wiki/HC11) compatible 8-bit CPU whose sole purpose is to perform VI emulation. It has a frontend hardware shim that looks like VI to the Wii software, and behind the scenes it translates those registers to the Radeon’s, including upscaling configuration. cafe2wii loads the DMCU’s firmware (the DMCU doesn’t seem to be active in Wii U mode). It has its own dedicated RAM and access to both the faux-VI back side and the Radeon’s register area. \[...\]

To get to the bottom of these issues, I decided to reverse-engineer the firmware of the DMCU. The DMCU firmware is located in the Wii U's storage and is loaded into the DMCU's RAM before launching cafe2wii. Unfortunately, my favorite reverse-engineering tool Ghidra does not support the HC11, so in 2024 I ended up writing my own processor specification for it[^3]. Luckily, Ghidra already supported the HCS08 and HCS12, which I could base my specification on.

I then spent several days reverse-engineering the DMCU firmware. The firmware itself is relatively straightforward. The DMCU has an interface to access the 32-bit registers of the display controller. The faux-VI hardware shim triggers interrupts in the DMCU firmware, which then configures the display controller accordingly.

Reverse engineering the DMCU quickly reveals the reason for the cropped image. The DMCU configures the display controller's viewport to simulate overscan. This crops off up to 16 pixels, which does not look great in several titles. Luckily, this is an easy fix; I can simply patch the bytes of the firmware directly to change the values written to the viewport registers.

## AMD's DMCU

What fail0verflow didn't know at the time, or at least didn't mention, is that the DMCU is not a Wii U-specific microcontroller, but is found in other AMD GPUs too. A patch submitted by an AMD employee in 2018 to the amd-gfx mailing list[^4] mentions the following:

> DMCU (Display Microcontroller Unit) is a GPU chip involved in eDP features like Adaptive Backlight Modulation and Panel Self Refresh.

At least now we know what DMCU stands for. Could this really be the same chip design as used by the Wii U? The linux-firmware repository has several DMCU binary blobs for different GPUs[^5]. After downloading `raven_dmcu.bin` and `navi12_dmcu.bin`, I wrote a quick tool to parse the headers of the firmware blobs and extract the part that actually gets loaded into ERAM[^6].

After loading the AMD Raven and Navi 12 DMCU firmware into Ghidra, I was able to confirm that the newer AMD GPUs still use an M68HC11-based DMCU. Even the display controller interface is almost identical to the one used on the Wii U.

## Decompiling the DMCU

Since I grew tired of having to apply byte patches to the existing DMCU firmware, I decided to create a C decompilation of the firmware. This would also allow me to get a deeper understanding of how everything works.

At first, I needed a proper toolchain to compile my project. GCC used to have upstream support for the M68HC11 and M68HC12[^7]. The last upstream release with support is GCC 3.3.5 from 2005. Later versions broke it before it was removed entirely. I eventually stumbled across releases from 2006 on the personal website of Stéphane Carrez[^8], the author of the original port. This leaves me with GCC 3.3.6 and binutils 2.15, which will have to do for now.

To my surprise, I was able to byte-match entire functions by hand from the original Wii U DMCU firmware using this compiler. To help with matching I created a modified version of [decomp.me](https://github.com/decompme/decomp.me) with support for the previously mentioned compiler.

For example, the following function (compiled with `gcc3.3.6-m68hc1x-20060122 -Os -m68hc11 -fomit-frame-pointer -mshort`) results in an exact byte-match in the Wii U DMCU firmware.

void FUN\_0e3b (FVIVideoMode videoMode, FVIState \* state)

{

 uint32\_t postBlanking = 0;

 uint32\_t preBlanking = 0;

 uint32\_t uVar2 = 0;

 uint32\_t uVar3 = 0;

 if (videoMode == FVI\_VIDEO\_MODE\_288P\_312 ||

 videoMode == FVI\_VIDEO\_MODE\_240P\_262) {

 preBlanking = state ->preBlanking;

 postBlanking = state ->postBlanking + 1;

 uVar2 = state ->unk0x0C - 1;

 uVar3 = state ->unk0x14 + 2;

 } else if (videoMode == FVI\_VIDEO\_MODE\_288P\_313 ||

 videoMode == FVI\_VIDEO\_MODE\_240P\_263) {

 preBlanking = state ->preBlanking;

 postBlanking = state ->postBlanking - 1;

 uVar2 = state ->unk0x0C + 1;

 uVar3 = state ->unk0x14 - 2;

 }

 DEINT\_Lock ();

 FUN\_1292 ();

 DC\_Write32 (0x10A6, postBlanking | (preBlanking << 16) | 0x80008000ul);

 DC\_Write32 (0x10A7, uVar3 | (uVar2 << 16) | 0x80008000ul);

 DAT\_0120 = 1;

 FUN\_133f ();

 DEINT\_Unlock ();

 DEBUG\_Write0x6 (0x40);

}

Looks like AMD or Nintendo (I'm not sure who was responsible for writing the Wii U's DMCU firmware) might have used Carrez's GCC port from 2006 when they were writing the Wii U's DMCU firmware. Are they still using it? Unfortunately, I wasn't able to get an exact match in the Raven or Navi 12 DMCU. They might have used some convoluted compiler flags that I couldn't figure out, or a different compiler entirely.

One interesting thing I noticed during reverse engineering is that the `nn_cmpt.rpl` library responsible for loading the DMCU firmware ends up writing the current display resolution into the middle of the DMCU's ERAM. These resolutions are unused by the DMCU firmware, but end up overwriting instructions in the middle of the firmware. By sheer luck, the overwritten functions happen to be unused, resulting in no issues being caused by this. Since I'm loading my custom firmware in the same way as the original one, this required some special attention in my decompilation. I had to create a carveout section for the resolutions and place the text section before these resolutions, manually placing larger functions into a section behind this carveout.

After manually decompiling the entire DMCU firmware to C, I had a functionally equivalent firmware. Not every function is byte-identical, but it seems good enough for me. I ended up publishing this firmware under the name [OpenDMCUFirm](https://github.com/GaryOderNichts/OpenDMCUFirm/tree/main) in 2025, in the hope that others could help with taking a closer look.

## The Cause of Color Bleed

Several months after releasing OpenDMCUFirm, I noticed two bugs in the DMCU firmware, which led to the color bleed issues in Wii mode.

The DMCU firmware processes events in the main event loop. These events are triggered based on interrupts from the faux-VI hardware shim. In the IRQ handler, status bits are mapped from an FVI register to events, which are then processed in the main loop. The code looks roughly like this[^9]:

struct {

 uint32\_t inStatus;

 uint16\_t outEvent;

} irqEventMasks \[4\] = {

 { 0x00000003, DMCU\_EVENT\_0, },

 { 0x08007B1C, DMCU\_EVENT\_1, },

 { 0x07C084E0, DMCU\_EVENT\_2, },

 { 0x003C0000, DMCU\_EVENT\_3, },

};

uint32\_t irqStatus = DC\_Read32 (0x1061);

uint32\_t eventFlags = gDmcuState.eventFlags;

for (uint32\_t i = 0; i < 4; i ++) {

 if (! (irqStatus & irqEventMasks \[i\].inStatus)) {

 continue;

 }

 handledFlags |= irqStatus & irqEventMasks \[i\].inStatus;

 flags = irqStatus >> DMCU\_EVENT\_ACTIVE\_SHIFT;

 if ((flags & irqEventMasks \[i\].outEvent & 0xf) != 0) {

 eventFlags |= irqEventMasks \[i\].outEvent << DMCU\_EVENT\_RETRIGGER\_SHIFT;

 } else {

 eventFlags |= irqEventMasks \[i\].outEvent;

 }

}

gDmcuState.eventFlags = eventFlags;

If an event is currently already being handled by the main loop, it is marked as "active". The IRQ handler checks for that and marks it to be "retriggered" instead. This causes the event loop to immediately run the event handler again after the handler returns[^10]:

getEventHandler (& eventHandler, & eventMask, gDmcuState.eventFlags & 0xF);

// ...

IRQ\_Lock ();

gDmcuState.eventFlags = (gDmcuState.eventFlags | (eventMask << DMCU\_EVENT\_ACTIVE\_SHIFT)) & ~ eventMask;

IRQ\_Unlock ();

do {

 eventHandler (& gDmcuState);

 if (((gDmcuState.eventFlags >> DMCU\_EVENT\_RERUN\_SHIFT) & eventMask) != 0) {

 gDmcuState.eventFlags &= ~ (eventMask << DMCU\_EVENT\_RERUN\_SHIFT);

 } else if ((gDmcuState.eventFlags >> DMCU\_EVENT\_RETRIGGER\_SHIFT) & eventMask & 0xf) {

 gDmcuState.eventFlags &= ~ (eventMask << DMCU\_EVENT\_RETRIGGER\_SHIFT);

 } else {

 IRQ\_Lock ();

 gDmcuState.eventFlags &= ~ (eventMask << DMCU\_EVENT\_ACTIVE\_SHIFT);

 IRQ\_Unlock ();

 goto loop\_start;

 }

} while (1);

Looks like someone made a typo in the IRQ handler, though. Instead of checking if the event active bit is set in `eventFlags`, they're checking if the bit is set in `irqStatus`, which is the hardware status register. If the bit for the active event happens to be set in the IRQ status register, the event is now "retriggered" and never marked as pending. This means the event is never actually processed by the main event loop. The fix for this is really simple:

-flags = irqStatus >> DMCU\_EVENT\_ACTIVE\_SHIFT;

+flags = eventFlags >> DMCU\_EVENT\_ACTIVE\_SHIFT;

I tested this on hardware and confirmed that this occurs for events 2 and 3. They are executed once and are then always stuck in the "retrigger" state and are never processed again. All because of a simple typo.

So what are events 2 and 3 used for? Event handler 2 configures a bit in the `DEINT` register block called `CBCR_SWAP`, based on the horizontal origin of the image[^11]:

// This register consists of (HBS << 16) | HBE

// See YAGCD 5.3 VI - Video Interface: HTR1 - Horizontal Timing 1

DC\_PerformRead (0x1082);

uint8\_t hbe = DC\_Get8 (0);

DEINT\_Lock ();

if ((hbe & 0x1) == 0) {

 DEINT\_EnableCBCRSwap ();

 \* outEnabled = TRUE;

} else {

 DEINT\_DisableCBCRSwap ();

 \* outEnabled = FALSE;

}

DEINT\_Unlock ();

There's actually another typo in this function, which was probably never noticed since this event handler is never called: `CBCR_SWAP` should be enabled for odd origins, not even origins. The fix for this is again really simple:

-if ((hbe & 0x1) == 0) {

+if ((hbe & 0x1) != 0) {

**Edit 2026-11-17:** It turns out this part actually used to be correct, but Nintendo had updated the DMCU firmware before, where they inverted the `CBCR_SWAP` logic. I assume they knew something was wrong, but couldn't figure out the root cause in the IRQ handler, so they tried inverting the logic here. Credits to [Rairii](https://labyrinth.zone/users/Rairii) for noticing this.

Making these two changes fixes the color bleed issues.

I'm not entirely sure yet what event handler 3 is used for. There don't seem to be any visual differences without it. It appears to be similar to event handler 1, so maybe they moved event handler 3's functionality there after noticing it wasn't working.

## Try it out now!

|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg-ntlrNV3ObkHTV_xIwGdjeff9I6VwhqoFGL2oO6zVCGv_8DiZAG5Pa1ktPzlKwPrw0vvHkWkJLwH2WrcnXZ5XC3OYEagGrVFoMS-A8t0b7JjfQRXdqlO87293UQS3bYI3D_x5uLzEIhTBHPXPar6HQ7q2i0DMUKxPxjyQeF5JtkTLXajDwn6UFm1EVcQ/w640-h360/before_after_sunshine.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEg-ntlrNV3ObkHTV_xIwGdjeff9I6VwhqoFGL2oO6zVCGv_8DiZAG5Pa1ktPzlKwPrw0vvHkWkJLwH2WrcnXZ5XC3OYEagGrVFoMS-A8t0b7JjfQRXdqlO87293UQS3bYI3D_x5uLzEIhTBHPXPar6HQ7q2i0DMUKxPxjyQeF5JtkTLXajDwn6UFm1EVcQ/s1280/before_after_sunshine.png) |
| Scaled up before and after image in Super Mario Sunshine. (Image credit: Rulumi)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |

|                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh9xrTcpmzqDzLteyNuqYYhPOEIkx0dTYprXKSaO-Th03nyeY3vhhC93Zu8B8yb34VvNhyphenhyphenHIQ5e5lsAScfDvNkkVPlWGLgiIBgjp2_X5SxBYuYv_C71ckH6mYZc15Fg-DRJ4GqM627qiQBKri51hSZDenEC8tgIOUjHDPep1hlrRLoW_mebk7-jN-r4QL4/w640-h360/before_after_skyward.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh9xrTcpmzqDzLteyNuqYYhPOEIkx0dTYprXKSaO-Th03nyeY3vhhC93Zu8B8yb34VvNhyphenhyphenHIQ5e5lsAScfDvNkkVPlWGLgiIBgjp2_X5SxBYuYv_C71ckH6mYZc15Fg-DRJ4GqM627qiQBKri51hSZDenEC8tgIOUjHDPep1hlrRLoW_mebk7-jN-r4QL4/s1280/before_after_skyward.png) |
| Scaled up before and after image in The Legend of Zelda: Skyward Sword. (Image credit: Rulumi)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |

These fixes are part of the [custom branch](https://github.com/GaryOderNichts/OpenDMCUFirm/tree/custom) in OpenDMCUFirm. You can load this version of OpenDMCUFirm with the latest release of [evWii](https://github.com/GaryOderNichts/evwii). Let me know how it looks!

You can also help me pay for my Wiimote batteries here: [https://ko-fi.com/garyodernichts](https://ko-fi.com/garyodernichts)

[^1]: [https://gamefaqs.gamespot.com/boards/631516-wii-u/66227322](https://gamefaqs.gamespot.com/boards/631516-wii-u/66227322)

[^2]: [https://fail0verflow.com/blog/2014/console-hacking-2013-omake/](https://fail0verflow.com/blog/2014/console-hacking-2013-omake/)

[^3]: [https://github.com/GaryOderNichts/ghidra-hc11-lang](https://github.com/GaryOderNichts/ghidra-hc11-lang)

[^4]: [https://lists.freedesktop.org/archives/amd-gfx/2018-September/026550.html](https://lists.freedesktop.org/archives/amd-gfx/2018-September/026550.html)

[^5]: [https://gitlab.com/kernel-firmware/linux-firmware/-/blob/main/amdgpu/raven_dmcu.bin](https://gitlab.com/kernel-firmware/linux-firmware/-/blob/main/amdgpu/raven_dmcu.bin)

[^6]: [https://gist.github.com/GaryOderNichts/ab38b340ea077daae8c4d58cd367c87b](https://gist.github.com/GaryOderNichts/ab38b340ea077daae8c4d58cd367c87b)

[^7]: [https://www.gnu.org/software/m68hc11/m68hc11_gcc.html](https://www.gnu.org/software/m68hc11/m68hc11_gcc.html)

[^8]: [https://web.archive.org/web/20240530035034/http://gnu-m68hc11.vacs.fr/blog/](https://web.archive.org/web/20240530035034/http://gnu-m68hc11.vacs.fr/blog/)

[^9]: [https://github.com/GaryOderNichts/OpenDMCUFirm/blob/ee11a04c536825de35e19996ea030d065afcae8e/source/dmcu.c#L91-L111](https://github.com/GaryOderNichts/OpenDMCUFirm/blob/ee11a04c536825de35e19996ea030d065afcae8e/source/dmcu.c#L91-L111)

[^10]: [https://github.com/GaryOderNichts/OpenDMCUFirm/blob/ee11a04c536825de35e19996ea030d065afcae8e/source/dmcu.c#L848-L865](https://github.com/GaryOderNichts/OpenDMCUFirm/blob/ee11a04c536825de35e19996ea030d065afcae8e/source/dmcu.c#L848-L865)

[^11]: [https://github.com/GaryOderNichts/OpenDMCUFirm/blob/ee11a04c536825de35e19996ea030d065afcae8e/source/deint.c#L350-L365](https://github.com/GaryOderNichts/OpenDMCUFirm/blob/ee11a04c536825de35e19996ea030d065afcae8e/source/deint.c#L350-L365)
