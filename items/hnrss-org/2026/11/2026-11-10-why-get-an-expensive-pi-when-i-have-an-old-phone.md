---
title: Why get an expensive Pi when I have an old phone?
link: https://so.long.thanks.fish/why-pi-when-i-have-an-old-phone/
source: hnrss-org
published: 2026-11-10T14:16:05Z
updated: 2026-11-10T14:16:05Z
first_seen: 2026-11-14T16:02:04.620496262Z
authors:
- casi
content: extracted
html: 2026-11-10-why-get-an-expensive-pi-when-i-have-an-old-phone.html
remote_preview:
  url: https://so.long.thanks.fish/why-pi-when-i-have-an-old-phone/redmi-leds-1200.webp
  alt: Rooted Redmi 13C 5G running the Moth Rig app, connected by USB-C to a controller for an LED strip
---

![Rooted Redmi 13C 5G running the Moth Rig app, connected by USB-C to a controller for an LED strip](https://so.long.thanks.fish/why-pi-when-i-have-an-old-phone/redmi-leds-1200.webp)\
The rooted Redmi talking over USB-C to the LED controller.

I’m working on a project that needs a high resolution camera, GPS, storage, and a microcontroller to control some lights. (More on the project soon!) I’ve been designing PCB adapters around a Raspberry Pi, an Arducam 64MP camera, and a GPS module. Every time I add another part, the bill grows.

I’m also trying an ESP32-P4 board, which will get its own post soon. But the alternative I keep coming back to is an old smartphone.

I have a Redmi 13C 5G and a Huawei Mate 30 5G. Phones like these turn up second hand on Taobao for roughly **¥300–500 (£33–56 / €40–67 / $45–75)**. The Redmi gives me a camera that saves 50MP JPEGs; the Mate 30 has a 40MP main camera and a telephoto lens. Both already contain GPS, storage, a clock, a battery, mobile data, and a touchscreen. Their USB-C ports give me a route to the PCB that will handle the lights and other sensors.

Here is the rough parts comparison I made while deciding what to build next:

| What I need               | Used Redmi or Mate 30               | Raspberry Pi 4 approach                                                                                                              |
| ------------------------- | ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| Computer                  | Included in phone price             | [Pi 4, 4GB](https://mall.dfrobot.com.cn/goods-1977.html): ¥880 / £98 / €118 / $132                                                  |
| High resolution camera    | Redmi 50MP JPEG; Mate 30 40MP       | [Arducam 64MP](https://www.arducam.com/arducam-1-1-32-64mp-autofocus-camera-module-for-raspebrry-pi.html): \~¥400 / £45 / €53 / $60 |
| GPS                       | Included                            | [GPS HAT](https://www.waveshare.com/product/ai/expansions/l76k-gps-hat.htm): \~¥153 / £17 / €20 / $23                               |
| Storage                   | Built in                            | [64GB microSD](https://www.microsoftstore.com.cn/hidden/sandisk-tf-card-64gb): \~¥50 / £6 / €7 / $7                                 |
| Core total                | ¥300–500 / £33–56 / €40–67 / $45–75 | \~¥1,483 / £165 / €198 / $222                                                                                                        |
| Clock                     | Included                            | [RTC HAT](https://www.waveshare.com/product/modules/others/rtc.htm): \~¥80 / £9 / €11 / $12                                         |
| Battery backup            | Included                            | [UPS HAT](https://www.waveshare.com/product/ups-hat.htm): \~¥133 / £15 / €18 / $20, plus cells                                      |
| Touch display             | Included                            | [5″ touchscreen](https://www.waveshare.com/product/displays/5-dsi-touch-b.htm): \~¥247 / £28 / €33 / $37                            |
| Mobile data               | Built in, with a SIM                | [4G HAT](https://www.waveshare.com/sim7600ce-4g-hat.htm): \~¥527 / £59 / €70 / $79                                                  |
| More fully equipped total | ¥300–500 / £33–56 / €40–67 / $45–75 | \~¥2,317 / £258 / €310 / $347, plus batteries                                                                                        |

These are illustrative November 2026 listings. The more fully equipped Pi total uses the cellular HAT’s GPS instead of buying the separate GPS HAT; its modem is 4G, while the phones are 5G models. Neither side includes my custom light controller PCB, a SIM or data plan, or an enclosure. The conversions use 10 November 2026 mid market rates of approximately ¥1 = [£0.111531](https://www.xe.com/currencyconverter/convert/?Amount=1&From=CNY&To=GBP) / [€0.133643](https://www.xe.com/currencyconverter/convert/?Amount=1&From=CNY&To=EUR) / [$0.14991](https://www.xe.com/currencyconverter/convert/?Amount=1&From=CNY&To=USD), rounded to whole units in the table.

The phone is already becoming more than a price exercise. I’ve unlocked and rooted the Redmi, tested its camera, and got it sleeping and waking for scheduled work. I can plug it into my computer and have Codex configure and test the pieces I need. The Huawei is harder to root, but I can still run a kiosk app on it. I’ve made a stand to photograph the same target with the Redmi and Arducam side by side; I want to see what detail each camera actually captures. A 50MP JPEG file does not, by itself, settle that question.

The open question is supply. Finding two good used phones is easy; finding a few hundred matching ones, with healthy batteries and predictable camera behaviour, would take more work. I feel increasingly confident that it’s possible.

This could be useful well beyond the current project. For digital artworks and installations, we’ve often used a Pi or a Mac mini. Imagine instead posting a client a configured phone: plug it into the display and it runs. With mobile data and remote access, I can fix something without asking a gallery for Wi-Fi privileges or an open port.

There’s something fun about taking a device made in enormous numbers, giving it a second life, and finding that most of the hardware I was about to assemble is already inside it.
