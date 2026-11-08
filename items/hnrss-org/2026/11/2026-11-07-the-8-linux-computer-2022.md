---
title: The $8 Linux Computer (2022)
link: http://thelittleengineerthatcould.blogspot.com/2022/12/the-8-linux-computer.html
source: hnrss-org
published: 2026-11-07T14:00:20Z
updated: 2026-11-07T14:00:20Z
first_seen: 2026-11-07T20:48:07.979401525Z
authors:
- zenincognito
content: extracted
html: 2026-11-07-the-8-linux-computer-2022.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50404599
  num_comments: 76
  points: 171
---

Yes, you read that right... for [$8](<https://pine64.com/product/128mb-ox64-sbc-available-on-december-2-2022/
    >) you can run linux on a RISC-V computer. And not just a stripped down kernel without an MMU. No, the Ox64 comes with capable RISC-V cores. Heck, and not just one core, but three cores (all different.) Exciting times!

**Caveat Emptor!** At the time of writing, ethernet, wifi and SD-Card are not working. It's good to realize this before jumping into this.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiMR6WHmKku5veDN9XI3ZTSOw9dSsNTbCsuJ6LSSxejLaYPTO-V71zPoc_-fpF0QYx260DGx5RSEsx58z-xxsujTZLugdjAs_7uaG6YZMdHBxrGaxlx9Q-H-NYszKY7-pvv1bfxEtmH1C3HZSoVBlkm5kdangQQyXUyRGFX8ja04tWg6ZzwsNs_9DuJJA/s600/ox64.jpeg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEiMR6WHmKku5veDN9XI3ZTSOw9dSsNTbCsuJ6LSSxejLaYPTO-V71zPoc_-fpF0QYx260DGx5RSEsx58z-xxsujTZLugdjAs_7uaG6YZMdHBxrGaxlx9Q-H-NYszKY7-pvv1bfxEtmH1C3HZSoVBlkm5kdangQQyXUyRGFX8ja04tWg6ZzwsNs_9DuJJA/s3114/ox64.jpeg)

It looks like a microcontroller, but behaves like a UNIX machine. Sweet, or what? Getting it up and running, though, is far from trivial. There are so many hurdles to take!

**Hurdle 1**: You cannot program it using USB, like you would an Arduino, ESP32, Stamp-C3 or rPi Pico. Nope... this needs to be programmed via the UART TX/RX pins. In the picture above, you can see them hooked up to GPIO14 / GPIO15 and GND.

**Hudle 2**: The flashing tool Dev Cube will crash on launch when using Ubuntu on Wayland: you have to switch to Xorg instead.

```
$ ./BLDevCube-ubuntu 
Warning: Ignoring XDG_SESSION_TYPE=wayland on Gnome. Use QT_QPA_PLATFORM=wayland to run on Wayland anyway.
/usr/lib/x86_64-linux-gnu/gio/modules/libdconfsettings.so: undefined symbol: g_log_structured_standard
Failed to load module: /usr/lib/x86_64-linux-gnu/gio/modules/libdconfsettings.so
GLib-GIO-Message: Using the 'memory' GSettings backend.  Your settings will not be saved or shared with other applications.

(BLDevCube-ubuntu:54646): GLib-GIO-ERROR **: Settings schema 'org.gnome.settings-daemon.plugins.xsettings' does not contain a key named 'antialiasing'
Trace/breakpoint trap (core dumped)
  
```

So, UART programming then... fine, we'll just use one of those little CH340G dongles. I got [one](https://www.amazon.ca/gp/product/B07V556Q82) off Amazon, but it would not work... every time the handshake would fail in Dev Cube (**hurdle 3**).

Luckily, [some people](https://discord.com/channels/463237927984693259/771032441971802142) found out that you can program it with an RP2040 device, like the rPi Pico. I had a [KB2040](https://learn.adafruit.com/adafruit-kb2040) from Adafruit lying around, so used that.

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhouPPAyy5QKp275xjz4VaBCd191CAfVXivIQuqxSTve9OUbJwR7grqbrIPH1_GwGg6hCsev-2UL4hHoH3kECmDA-k4my1h4_Tbq_qc0I-h8xDG1U6HPCoyXmhfM0Kutb-rT3xyBM5-MwIvEEpIxzTjHyJd54uagXfUMMCpiRZLPRU-aHsJOCggKwi1Pg/s600/kb2040.jpeg)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhouPPAyy5QKp275xjz4VaBCd191CAfVXivIQuqxSTve9OUbJwR7grqbrIPH1_GwGg6hCsev-2UL4hHoH3kECmDA-k4my1h4_Tbq_qc0I-h8xDG1U6HPCoyXmhfM0Kutb-rT3xyBM5-MwIvEEpIxzTjHyJd54uagXfUMMCpiRZLPRU-aHsJOCggKwi1Pg/s1618/kb2040.jpeg)

Which puts us at **hurdle 4**: How to turn a KB2040 into a serial bridge between PC and Ox64? Hook up GPIO-4 of the RP2040 to GPIO-15 of the Ox64. Hook up GPIO-5 of the RP2040 to GPIO-14 of the Ox64. Also, connect the grounds, of course. See below how to hook up for flashing the M0.\\

[![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhM9Xafi7eeiz0tNQWMUX4pULaTq4VGeupNLwmk-ox-bT6ESlBtjhC3ZxsI72G3c74a85T5lHmr96pv_wlREMl1u7Lqz2mlReGCn5AZ4Y7VXfv6UfdvutWR0wTUPW_6WRktiA_kXWlSAQ-zRkcV9620lXUIZmMTihFAKWqOllqFfIbvdVvKN_-7Uh0s7Q/s600/wiring-kb2040-ox64.png)](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEhM9Xafi7eeiz0tNQWMUX4pULaTq4VGeupNLwmk-ox-bT6ESlBtjhC3ZxsI72G3c74a85T5lHmr96pv_wlREMl1u7Lqz2mlReGCn5AZ4Y7VXfv6UfdvutWR0wTUPW_6WRktiA_kXWlSAQ-zRkcV9620lXUIZmMTihFAKWqOllqFfIbvdVvKN_-7Uh0s7Q/s1463/wiring-kb2040-ox64.png)

Then we need to load our RP2040 with [picoprobe](https://github.com/raspberrypi/picoprobe) which we can do by holding BOOT while powering up, and feeding it the uf2 file with picoprobe.

Ok, so now we can flash it then? Unfortunately, no, **hurdle 5**: DevCube is still not able to flash the Ox64, but we can use [bflb-mcu-tool](https://pypi.org/project/bflb-mcu-tool/) instead.

Ok, but with bflb-mcu-tool, we are all set! Are we? Introducing **hurdle 6**: we need to modify the tool before it will work for us. This means stripping DTR and RTS stuff from the tool by editing the ~/.local/lib/python3.10/site-packages/bflb\_mcu\_tool/libs/bflb\_interface\_uart.py file. Here is [my copy](https://gist.github.com/stolk/46228d2b7ab0ca5b0ff98a4ce3b2c6fe).

So with the Ox64 powered via MicroUSB, and the RP2040 connected to my PC using USB-C, I can finally flash something. For that, I took the low\_load\_bl808\_m0.bin from the [bl808 linux guide](https://github.com/bouffalolab/bl808_linux) and flashed it with:

```
$ bflb-mcu-tool --chipname=bl808 --port=/dev/ttyACM2 --baudrate=2000000 --firmware=out/low_load_bl808_m0.bin
    
```

After which I now get output from my Ox64, via my KB2040 to my PC:

```
dynamic memory init success,heap size = 156 Kbyte 
E907 start...
mtimer clk:1000000
psram clk init ok!
    
```

Ok, a modest start, but the Ox64 is talking to my PC now! Yay! Come back to my blog at a later time to see if I will manage to run Linux on the Ox64! Because as I understand it, I also need to flash something to the D0 and then flash the actual OS as well.

**UPDATE:** Here is [PART 2](https://thelittleengineerthatcould.blogspot.com/2022/12/the-8-linux-computer-part-2.html).
