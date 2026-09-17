---
title: PCB is brought to you by Fable 5
link: https://a6mzero.com/posts/this-pcb-is-brought-to-you-by-fable-5/
source: hnrss-org-frontpage
published: 2026-09-14T12:25:24Z
updated: 2026-09-14T12:25:24Z
first_seen: 2026-09-17T12:46:46.439381799Z
authors:
- jasonpeacock
summary: 'Article URL: https://a6mzero.com/posts/this-pcb-is-brought-to-you-by-fable-5/ Comments URL: https://news.ycombinator.com/item?id=49695689 Points: 129 # Comments: 59'
content: extracted
html: 2026-09-14-pcb-is-brought-to-you-by-fable-5.html
preview:
  file: 2026-09-14-pcb-is-brought-to-you-by-fable-5.preview-f588c2f994bf.webp
  width: 256
  height: 256
  color: '#c3a45e'
images:
- source: https://a6mzero.com/static/images/a6m-zero-plate-2.jpg
  original:
    file: 2026-09-14-pcb-is-brought-to-you-by-fable-5.image-209603479acc.jpg
    width: 1200
    height: 1200
  color: '#f7cc72'
- source: https://a6mzero.com/static/images/board-cropped.jpeg
  original:
    file: 2026-09-14-pcb-is-brought-to-you-by-fable-5.image-4a6a8e7155ba.jpg
    width: 1400
    height: 1459
  color: '#656729'
- source: https://a6mzero.com/static/images/pcb-router-compare.png
  original:
    file: 2026-09-14-pcb-is-brought-to-you-by-fable-5.image-7db63135fe84.png
    width: 1600
    height: 900
  variants:
  - file: 2026-09-14-pcb-is-brought-to-you-by-fable-5.image-23134fa1de89.webp
    width: 320
    height: 180
  - file: 2026-09-14-pcb-is-brought-to-you-by-fable-5.image-29e24c1355f2.webp
    width: 640
    height: 360
  - file: 2026-09-14-pcb-is-brought-to-you-by-fable-5.image-2ac318525f73.webp
    width: 960
    height: 540
  - file: 2026-09-14-pcb-is-brought-to-you-by-fable-5.image-6afba799b824.webp
    width: 1280
    height: 720
  - file: 2026-09-14-pcb-is-brought-to-you-by-fable-5.image-9176c9c5af2c.webp
    width: 1600
    height: 900
  color: '#0e1116'
- source: https://a6mzero.com/static/images/factory-2.jpeg
  original:
    file: 2026-09-14-pcb-is-brought-to-you-by-fable-5.image-b015e6466973.jpg
    width: 1400
    height: 1867
  color: '#848849'
- source: https://a6mzero.com/static/images/orin-tablet.png
  original:
    file: 2026-09-14-pcb-is-brought-to-you-by-fable-5.image-30ed3b68161d.png
    width: 1400
    height: 902
  variants:
  - file: 2026-09-14-pcb-is-brought-to-you-by-fable-5.image-a9c31e27910e.webp
    width: 320
    height: 206
  - file: 2026-09-14-pcb-is-brought-to-you-by-fable-5.image-2ff4c6a03f5e.webp
    width: 640
    height: 412
  - file: 2026-09-14-pcb-is-brought-to-you-by-fable-5.image-d8f71e137aa4.webp
    width: 960
    height: 619
  - file: 2026-09-14-pcb-is-brought-to-you-by-fable-5.image-73ec679e265f.webp
    width: 1280
    height: 825
  - file: 2026-09-14-pcb-is-brought-to-you-by-fable-5.image-526ed3e951c1.webp
    width: 1400
    height: 902
  color: '#1d3b2a'
---

![](https://a6mzero.com/static/images/board-cropped.jpeg)

The board I will be journaling about.

I have been wanting to design a simple PCB for the last couple of years now. The thought of converting a design idea into a physical board and programming it to do things is fascinating to me. Long story short, I procrastinated until I tried to vibe-generate a dead simple PCB with Claude Opus 4.8 and it was terrible ! It had no idea about the orientation of the components, it did not do any proper routing. I was disappointed and accepted the fact that these tools were not there yet.

Then arrived the Fable 5. At first I was not that hopeful. One Thursday evening, around 10 hours before my weekly reset for Claude, I decided to give it another try; this time with Fable 5.

I had two rules:

1. No manual edits or verification of the board.
2. Every problem I face before manufacturing will be solved by Fable.

This meant I was going to trust Fable with my wallet. I decided to describe the board I want it to generate, and I was not going to be involved in the design phase. I was the end customer.

This is the prompt I gave it:

That was it, a short description of a RPI 2350 based development board which can drive an E-ink display. After working autonomously for a couple of hours it came up with the design shown in the video below.

The board as it freshly came out of Kicad ⃰.

Board

31.8 × 37.32 mm, 4 layer

MCU

RP2350A

Display

1.54" E-ink, 200×200

Flash

8 MB QSPI

Cost

26€ per board

## A closer look to some errors

The design process was not error free of course. When I showed the initial design (Claude was still working on it) of the PCB to my colleague, the first thing he wanted me to check (after recovering from the pain of seeing them tracks) was the DRC (Design Rule Checking) errors. I did not know what it was, and when we looked at it, there were indeed 65 DRC errors. Following the rule, I only mentioned the errors to Fable and did nothing else.

Fable was amazing at component selection, except for the two components I highlighted above. The big one on the top left is the SPI flash which stores the firmware and other data that you want to save. The SPI flash memory Claude chose was W25Q128JVS. It comes with the SOIC-8 wide package, but the pads designed for the memory was for a SOP-8 package, meaning the chip is too big for the pads. The bottom left component on the other hand is the transistor that switches the boost converter for the E-ink driver circuitry. As you can see it also chose the wrong package, as it is too small for the pads. I did not realise these until I uploaded the required files to JLCPCB. There I could see the issues, and I discussed it with Claude. For the W25Q128JVS it insisted that there was a SOP-8 package but I could not find it in LCSC's library. Eventually we settled at the P25Q64SH chip.

## But wait a minute, how did it even route ?

The design had 65 footprints, 54 nets and 118 unconnected lines. It is not a complex PCB by any means :P. Fable decided to use the Freerouting open-source project. The tool worked for 2 minutes, and after seventeen passes it plateaued at sixty nine connections, leaving 49 disconnected, and it could not finish the job. The remaining connections were hand-routed by Claude.

Freerouting doing its job

After I ordered the board my colleague mentioned to me the KiCadRoutingTools open-source project. It ran for 1.25 seconds and it could route all the connections with no problem. I will try this tool out for my upcoming hardware projects.

![Freerouting versus KiCadRoutingTools](https://a6mzero.com/static/images/pcb-router-compare.png)

Same placement, two routers

## Ordering with JLCPCB

I had never ordered anything from a PCB manufacturer before. It seemed complex and I was reluctant to take the first step. Upon Claude's compilation of the project, I asked it to prepare the required files for JLCPCB, and tell me what to select on their GUI. Man am I satisfied with JLCPCB. It was so straight forward, easy to interact with and there was no bloat. I uploaded the files, some components Claude selected were not available, we did a back and forth and voila we were done. For five fully assembled boards I paid 130 Euros, ordered the E-ink displays from a local shop and now it was the waiting game.

## Some cool animations while we are waiting for the PCBs

The four layers, pulled apart.

Assembly of our board

## The boards have arrived !!!

I received the PCBs and the first thing I wanted to do was to plug it in to my laptop. I have broken multiple USB modules for my Framework earlier, hence I thought checking for a short between 3v3 and ground was a no-brainer, although my colleague was suggesting me to just yeet it since this was a fully vibe-generated board. There was no short and I just plugged it in. There it was, the board was recognized and it was ready to be used.

![](https://a6mzero.com/static/images/factory-2.jpeg)

## What do I do with it ?

I already wrote some proof-of-concept apps, and they worked perfectly fine. I can read on that beautiful 1.54 inch display :D Stopwatch is quite handy if i need some focusing, and the album is my favourite feature since even after powering the board off, the images stay on the display thanks to E-ink.

Hands on with the finished board.

## How do I feel all about this ?

Great and meh. I love how I was able to just describe the board I want in plain English, send the files overseas and then receive a fully functional board without knowing any proper PCB design knowledge. The possibilities are limitless here, and I will certainly continue doing this in the future.

That said, I was not feeling much of an accomplishment, rightfully so. I used to enjoy the learning and the struggle that came with it. Although we are in the best era to learn about something, the fact that you can make things without knowing anything about a subject puts you in an uncomfortable spot.

## The future I wish to have

Yes it does suck that the joy we had while building has been sucked out of us and now we are told to enjoy building from a higher abstraction level. I am trying to adjust to this, especially at work. At work I can't just YOLO stuff so I meticulously review all the time. It is tiring but knowing the fact that my input still matters, is rewarding. For my hobby projects tho, I will continue to YOLO it and build stuff fast without necessarily knowing about the details.

I hope one day JLCPCB or PCBWAY will have a chat-box where I can dump all my ideas and some of my illustrations, and two days later they will ship me the board. I want them to remove the middle man, and make PCB generation so much simpler and safer.

## Sneak Peek

I already started working on my next project. Using Fable 5.1 with KiCAD and KiCADRoutingTools I am building an NVIDIA Jetson Orin Nano based tablet. The same rules I mentioned earlier will apply and I will let you know about the results(if I get to order it :P) .

![](https://a6mzero.com/static/images/orin-tablet.png)

NVIDIA Jetson Orin Nano based Tablet

Thank you for reading my journal, sharing is the most fun part of tinkering and building. I appreciate that you are part of this fun journey :)
