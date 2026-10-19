---
title: Laptop Based on 6502 CPU
link: https://www.maddox.pro/?p=448
source: hnrss-org
published: 2026-10-15T22:56:23Z
updated: 2026-10-15T22:56:23Z
first_seen: 2026-10-19T12:57:26.464953148Z
authors:
- nanochess
labels:
- '6502'
- retro computer
content: extracted
html: 2026-10-15-laptop-based-on-6502-cpu.html
remote_preview:
  url: https://www.maddox.pro/wp-content/uploads/2026/08/IMG8369-2048x1536.jpg
---

Well, I had a chance to get my head sorted and I finally finished the “b” version of the LT6502

![](https://www.maddox.pro/wp-content/uploads/2026/08/IMG8369-2048x1536.jpg)

Here you can see it next to the first version, I changed my mind about the case and went for translucent orange, it’s way more vivid in real life. I really do like the translucent material, it’s really different 🙂

The first big thing that’s different, it’s half the thickness of the old one;

![](https://www.maddox.pro/wp-content/uploads/2026/08/IMG8365-2048x1536.jpg)

It also has a larger, higher resolution screen. The screen is 1024×600 and is 10.1″, the previous one was 800×480 and 9″.\
It has a similar battery capacity, so should last about the same amount of time.\
It’s a little faster, 10MHz instead of 8MHz.\
It has a SID emulator on board and a WiFi Modem.\
It doesn’t have an expansion slot though. The aim was to make it more “sleek”.

![](https://www.maddox.pro/wp-content/uploads/2026/08/IMG8370s-2048x1287.jpg)

The screen also has a better contrast ratio being IPS rather than regular LCD, and the higher resolution really does show.

Here is a good example of that, and also the extra rows of text you can fit on the screen!

![](https://www.maddox.pro/wp-content/uploads/2026/08/IMG8375-2048x1257.jpg)

You can find the code/PCB Files/etc all on Codeberg here – [https://codeberg.org/TechPaula/LT6502b](https://codeberg.org/TechPaula/LT6502b)

There are, of course, somethings I’d change

- The holes and support for the heat inserts need making bigger and stronger, they’re VERY weak.
- CAPSLOCK led under the capslock key
- Find a better WiFi Modem, this is too slow and a bit flakey for real use.
- better speaker, it works, but it’s a bit “anemic”
- Faster, start with speed in mind (i.e. figure out how to copy ROM to RAM and switch out ROM)
- Better battery solution, I’m thinking something 18650 and on the back as part of the “hinge”
- USB PD support that doesn’t need preprogramming of the USB-PD chip to limit it at 5V, would be nice to have a buck/boost convertor to take anything USB-PD can throw at it and make it 5V
- Better sound chip, the SID is ok, but it’s a bit limited by modern standards.
- I really want to be able to read a FAT16 file system on the CFCard rather than my own custom one.
