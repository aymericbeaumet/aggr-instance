---
title: Samsung running a TensorFlow lite model on 960x540 image to track you
link: https://twitter.com/mike64_t/status/2114150399406202896
source: hnrss-org
published: 2026-10-25T10:42:01Z
updated: 2026-10-25T10:42:01Z
first_seen: 2026-10-25T14:18:35.600292421Z
authors:
- mashally
content: extracted
html: 2026-10-25-samsung-running-a-tensorflow-lite-model-on-960x540-image-to.html
remote_preview:
  url: https://pbs.twimg.com/media/HVb1282bgAAg6jY?format=webp&name=large
---

[![@mike64_t](https://pbs.twimg.com/profile_images/1594187845408899072/xzTgGoI6_reasonably_small.jpg)](https://x.com/mike64_t)

Ok, I just checked this, and its literally running a freaking TensorFlow Lite model on 960×540 images with INT8 quantization every couple of seconds in an attempt to figure out what game you are playing. It's literally called "alex.tflite" and its literally AlexNet and seems to follow torchvision.models.alexnet. As far as I can tell this does some strange nearest neighbor feature comparisons against a DB and literally doesn't work 99% of the time, you love to see your CPU get wasted every other second by this thing though. It also just seems to dump its results into the void because the consent gate comes later. Anyways, here's the tf model graph and some dumped captures.

![](https://pbs.twimg.com/media/HVb1282bgAAg6jY?format=webp&name=large)

![](https://pbs.twimg.com/media/HVb4a-6a8AIVTc8?format=webp&name=large)

[![@log4j](https://pbs.twimg.com/profile_images/2103214939112448000/ejp3VPpq_bigger.jpg)](https://x.com/log4j)

live-tweeting on samsung findings were fun. i'll let my agents give it a bit more time, but this document is already the most comprehensive breakdown of some key concerns of tizen i've seen run on a live device. it takes like a good while for me just to scroll through it

![](https://pbs.twimg.com/media/HVXTdOeWAAEBM3j?format=webp&name=large)
