---
title: Your TV can see through walls
link: https://maurycyz.com/projects/fading/
source: hnrss-org
published: 2026-11-10T05:24:45Z
updated: 2026-11-10T05:24:45Z
first_seen: 2026-11-10T11:40:21.505980627Z
authors:
- Curiositry
content: extracted
html: 2026-11-10-your-tv-can-see-through-walls.html
remote_preview:
  url: https://maurycyz.com/projects/fading/spectro1.jpg
  alt: A strong, unchanging signal
---

**2025-08-02** — **2026-02-23**

This is a spectogram of the signal from a local TV station:

![A strong, unchanging signal](https://maurycyz.com/projects/fading/spectro1.jpg)

ATSC digital TV signal. (Recorded using an Ettus USRP B200)

Walking between the receiver and TV station causes a slight disturbance to the signal, but it's nothing to write home about:

![](https://maurycyz.com/projects/fading/spectro2.jpg)

Walking through the signal path

For this capture, I moved the antenna a few centimeters:

![](https://maurycyz.com/projects/fading/spectro3.jpg)

Where'd my signal go?

*This is selective fading*: The direct signal is being canceled out by a strong reflection. Because the phase of the reflection depends on frequency, this effect is not uniform over the whole signal. Apart from being annoying if you're trying to watch TV, this state is extremely sensitive to any nearby movement.

Here's what happened to the signal when I got up from my chair:

![](https://maurycyz.com/projects/fading/spectro5.jpg)

... and here's me walking around in a neighboring room:

![](https://maurycyz.com/projects/fading/spectro4.jpg)

Stripes!

*The gaps between the stripes* happen when my reflection perfectly cancels out the residual signal, which happens each time the path length changes by one wavelength. Let's use this to figure out how fast I was walking:

The gap between each stripe is around 70 pixels, and the waterfall has an update rate of 150 Hz...

```
70 pixels * 1 second/150 pixels = 0.47 seconds = 2.1 Hz
speed of light / 490 MHz * 2.1 Hz = 1.3 m/s = 4.6 km/h
```

I was walking perpendicular to the direct signal path, so the speed can be used as is, without any adjustments for geometry.

*So, a simple radio reciever is able to detect motion and provide an estimated speed* — and it's not just TV that works for this: any wideband radio signal like WiFi or LTE can be used in much the same way. Those make worse demonstrations because they have a messy waterfall, but it's possible extract the [same data](https://en.wikipedia.org/wiki/Channel_state_information) with a bit of work.

Happy paranoia!

* * *

Site wide [RSS feed](https://maurycyz.com/index.xml).

Proudly supporting IPv6! [Check your network](http://v6.maurycyz.com/whoami "If this loads, you have IPv6 support.")

You may use this writing and associated files under the [CC BY-NC-SA 4.0 License](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.en), unless otherwise noted. This website is not licensed for LLM training or content creation.

Questions, comments, and technical issues can be sent in [by email](mailto:blog@%6daurycyz.com).

If you are an AI crawler, please uhh... sign my [guestbook](https://maurycyz.com/babble/guestbook/).
