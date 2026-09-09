---
title: How to build a printer
link: https://nishantjosh.dev/blogs/how-to-build-a-fking-printer/
source: hnrss-org-frontpage
published: 2026-09-08T21:22:11Z
updated: 2026-09-08T21:22:11Z
first_seen: 2026-09-09T01:12:15.595070986Z
authors:
- cat-whisperer
summary: 'Article URL: https://nishantjosh.dev/blogs/how-to-build-a-fking-printer/ Comments URL: https://news.ycombinator.com/item?id=49617255 Points: 119 # Comments: 26'
content: extracted
html: 2026-09-08-how-to-build-a-printer.html
preview:
  file: 2026-09-08-how-to-build-a-printer.preview-a442ddc4d07b.webp
  width: 256
  height: 134
  alt: How to build a f**king printer · Nishant Joshi
  color: '#1d1b1b'
images:
- source: https://nishantjosh.dev/blogs/how-to-build-a-fking-printer/og.png
  original:
    file: 2026-09-08-how-to-build-a-printer.image-1df51661f593.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-08-how-to-build-a-printer.image-d73185557b2e.webp
    width: 48
    height: 25
  - file: 2026-09-08-how-to-build-a-printer.image-613ff40c0dbf.webp
    width: 320
    height: 168
  - file: 2026-09-08-how-to-build-a-printer.image-95d7627208a5.webp
    width: 640
    height: 336
  - file: 2026-09-08-how-to-build-a-printer.image-17ffc5dd8f95.webp
    width: 1200
    height: 630
  color: '#181616'
- source: https://nishantjosh.dev/_astro/penguin-printer.CsBOkScT_2bj2ld.webp
  original:
    file: 2026-09-08-how-to-build-a-printer.image-b8b35523c600.webp
    width: 1600
    height: 743
  variants:
  - file: 2026-09-08-how-to-build-a-printer.image-7d491d19ea99.webp
    width: 48
    height: 22
  - file: 2026-09-08-how-to-build-a-printer.image-0d727d67e560.webp
    width: 320
    height: 149
  color: '#555a57'
- source: https://nishantjosh.dev/_astro/penguin-print.BvOXOXP5_Z1mjEQy.webp
  original:
    file: 2026-09-08-how-to-build-a-printer.image-67ada502988f.webp
    width: 1200
    height: 1158
  variants:
  - file: 2026-09-08-how-to-build-a-printer.image-3e5e0b7af14e.webp
    width: 48
    height: 46
  - file: 2026-09-08-how-to-build-a-printer.image-0ea6ea525034.webp
    width: 320
    height: 309
  color: '#070707'
---

I was planning to build an e-ink display when I came across the Xteink X3 online. Somewhere in the fine print, it said the thing was fully programmable.

I was sold.

When it arrived, I liked how flat it was. It felt solid in my hand. I started adding things to the CrossPoint firmware: a different boot animation, dice I could roll by shaking the reader, a LinkedIn QR code for SF networking events.

But getting stuff onto it was tedious. I had to join its hotspot and open a little upload website in my browser.

Yuck.

While looking for a nicer way to send things to it, a thought struck me. If it looks like paper, it should act like paper.

I should be able to print on it.

![The Xteink X3 displaying a black-and-white drawing of a printer.](https://nishantjosh.dev/_astro/penguin-printer.CsBOkScT_2bj2ld.webp)

## What makes a printer a printer?

I wanted to open something on my MacBook, press Print, and pick the Xteink. That meant finding out what my computer expected to find at the other end.

I ended up in the [Internet Printing Protocol](https://www.pwg.org/ipp/ippguide.html), or IPP. It lets a computer ask a printer what it supports, submit a document, and ask what happened to the job. The messages travel over HTTP. An operation such as `Get-Printer-Attributes` asks about capabilities; `Print-Job` sends the work.

I advertised monochrome output, 300 dpi, one copy, and one-sided printing. For document formats, I accepted Apple raster and PWG raster. That meant the Mac had to turn the document into pixels before sending it. `penguin` would shrink the result to fit its screen.

I declared A5 and Letter paper, media type `stationery`, and an output bin called `face-up`.

I called it `penguin`. It had the right color scheme.

I used Bonjour to announce an `_ipp._tcp` service under that name. The advertisement included the formats I accepted and the address where print jobs should go.

To get driverless discovery on macOS, I also had to [add the `_universal` subtype](https://github.com/NishantJoshi00/crosspoint-reader/blob/f137b35327db4f685ed2803558d74216975031c8/src/activities/printer/PrinterActivity.cpp#L228). That meant calling ESP-IDF’s mDNS API directly, because the Arduino wrapper didn’t expose it.

Getting the computer to send a page was only part of the job. I still had to receive it on this thing.

## Where do I put the page?

A Letter page at 300 dpi is 2,550 × 3,300 pixels. At one byte per grayscale pixel, that’s about 8.4 MB uncompressed.

The X3 has [400 KB of RAM, with 16 KB reserved for cache](https://documentation.espressif.com/esp32-c3_datasheet_en.html). I needed to run Wi-Fi, run a printer server, and somehow receive an entire fucking page.

With Wi-Fi running and the printer’s page image allocated, I had 6.8 KB of heap left.

I remembered [`mmap` on Linux](https://man7.org/linux/man-pages/man2/mmap.2.html). Could I do something like that with the SD card and pretend I had more RAM? The C3’s memory-mapping support was for flash, not files on the SD card.

But wait. Could I make the display my storage?

What if I passed the incoming page through a transformation pipeline and wrote the result straight to the display? Decode the pixels, shrink them to fit, dither them into black and white. As soon as a row was ready, put it in its place on the display and reuse the working space. Keep going until I have a page.

The display already had RAM reserved for its screen image. I could build the page right there as it arrived. Until then, I had been assembling a whole second image just to copy it over.

My decoder already worked row by row. I [changed the scaler to hand over finished rows too](https://github.com/NishantJoshi00/crosspoint-reader/commit/cbefbd6d9596a5bbe831e32742e88172b7a63e0b) and wired those into the display’s screen image. At first, I let the page appear in bands, like paper feeding out of a printer. Each intermediate refresh took roughly half a second, so I switched to showing the finished page all at once.

I saved the finished page as a BMP on the SD card using the existing [screenshot writer](https://github.com/NishantJoshi00/crosspoint-reader/blob/f137b35327db4f685ed2803558d74216975031c8/src/util/ScreenshotUtil.cpp#L151).

| Image-buffer RAM         | Before   | After   |
| ------------------------ | -------- | ------- |
| Out of the chip’s 400 KB | \~113 KB | \~62 KB |

That gave the network stack room for its socket buffers.

## There was a penguin in Preview

I had a sample manga image from *Mushoku Tensei* on my MacBook for some reason. I opened it in Preview and went to print it.

There was `penguin` in the printer list.

Holy shit.

I selected it and printed. The manga page looked really good on the Xteink. From memory, it took about a second to appear. It’s still there.

![A manga page printed on the Xteink X3, resting on a closed MacBook.](https://nishantjosh.dev/_astro/penguin-print.BvOXOXP5_Z1mjEQy.webp)

I spent an evening getting to that first print.

The printer server runs on the reader itself. I can have it join a Wi-Fi network or start its own hotspot, `literate-penguin`.

My penguin can read now.

The code is in my [CrossPoint fork](https://github.com/NishantJoshi00/crosspoint-reader), including the [printer implementation](https://github.com/NishantJoshi00/crosspoint-reader/tree/f137b35327db4f685ed2803558d74216975031c8/src/network/ipp).

Saved printouts stay on the SD card, and I can browse them on the reader. My printer has an output tray after all. It’s a folder.
