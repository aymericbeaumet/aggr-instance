---
title: Pangram Image 1.0
link: https://www.pangram.com/blog/introducing-pangram-image-1-0
source: hnrss-org
published: 2026-11-05T19:27:52Z
updated: 2026-11-05T19:27:52Z
first_seen: 2026-11-06T11:27:16.888185679Z
authors:
- gmays
content: extracted
html: 2026-11-05-pangram-image-1-0.html
remote_preview:
  url: https://cdn.sanity.io/images/6vunltag/production/d9e0f2c5b335fbb748927abe54503f2daf0c132b-2400x1350.png
  alt: Introducing Pangram Image 1.0
---

![A real photograph containing an AI image. Pangram Image 1.0 is able to locate the AI content with its heatmap.](https://www.pangram.com/_next/image?url=https%3A%2F%2Fcdn.sanity.io%2Fimages%2F6vunltag%2Fproduction%2Fd7c9d0ad9cc0fcf85fcc18a8842d2ab8171b8593-2653x1196.png%3Fw%3D1200%26q%3D85%26auto%3Dformat&w=3840&q=75)A real photograph containing an AI image. Pangram Image 1.0 is able to locate the AI content with its heatmap.

A few months ago, we released Pangram Image as a research preview. We are now proud to announce Pangram Image 1.0: The state-of-the-art in both AI image and AI video detection. We would like to thank the whole community for taking the time to try it and find its weaknesses, and for holding us to a high standard to make a better detector for everyone.

### **What’s new**

- Our new flagship AI image detection model, **Pangram Image 1.0**, with:
  - State-of-the-art performance at detecting frontier AI image generators
  - Heatmap output for localizing exactly where AI appears in images
  - Robustness to edits, compression, and low resolutions
- A smaller, faster model, **Pangram Image Flash 1.0,** with:
  - State-of-the-art performance at detecting frontier AI video generators
  - Frame by frame analysis of AI edited videos
- Chrome Extension support for detecting AI in your feed
- API support for image and video

### [Try it here](https://www.pangram.com/image-detection)

## Pangram Image 1.0

### Frontier Image Detection

We tested our Pangram Image 1.0 model against several commercial and academic AI image detectors. We created synthetic mirrors of 100 real photos using 12 frontier state of the art AI image generators, using prompts that closely matched the original photo for a total of 1,200 synthetic images. Our model achieves 99.96% accuracy on this evaluation set.

![](https://www.pangram.com/_next/image?url=https%3A%2F%2Fcdn.sanity.io%2Fimages%2F6vunltag%2Fproduction%2Fd873e792c55421888b2262bb07f7d82383fb33df-2400x1464.png%3Fw%3D1200%26q%3D85%26auto%3Dformat&w=3840&q=75)

### Academic AI Image Detection Benchmarks

We are committed to providing the highest quality detection to our users. To test our model robustly, we compared it against several academic benchmarks for AI image detection.

![](https://www.pangram.com/_next/image?url=https%3A%2F%2Fcdn.sanity.io%2Fimages%2F6vunltag%2Fproduction%2F44f1b1e638742b1bb241e981eeaf2d86288372bc-2400x1464.png%3Fw%3D1200%26q%3D85%26auto%3Dformat&w=3840&q=75)

Sources: [X-AIGD](https://arxiv.org/abs/2601.19430) · [Mirage-Test](https://arxiv.org/abs/2511.08423) · [AIGenImages2026](https://arxiv.org/abs/2605.02567) · [Synthbuster + RAISE comparison](https://openaccess.thecvf.com/content/CVPR2026/supplemental/Qin_Scaling_Up_AI-Generated_CVPR_2026_supplemental.pdf) · [Community Forensics](https://arxiv.org/abs/2411.04125)

### Robustness

Many social media platforms compress and downscale images, and malicious actors can also add blur, noise, and other artifacts to try to trick AI detectors into thinking AI images are real. We specifically train to detect these kinds of transformations, and we have found our model to be robust to these edits.

![](https://www.pangram.com/_next/image?url=https%3A%2F%2Fcdn.sanity.io%2Fimages%2F6vunltag%2Fproduction%2Fcc015d1f349b1bf404f92586185adcf6c684b228-2216x1892.png%3Fw%3D1200%26q%3D85%26auto%3Dformat&w=3840&q=75)

### Low Resolution Detection

Our initial release limited image input to the minimum size of 512x512, as our accuracy dropped off at lower resolutions. We are now lowering this limit to 256x256, as we find our new model has improved accuracy at lower resolutions. At resolutions below 512, image detection results will come with a reduced confidence disclaimer.

![](https://www.pangram.com/_next/image?url=https%3A%2F%2Fcdn.sanity.io%2Fimages%2F6vunltag%2Fproduction%2F2d19c49ae8fda0f693550902715e37c560416223-2400x1526.png%3Fw%3D1200%26q%3D85%26auto%3Dformat&w=3840&q=75)

## Video Detection

### Pangram Image Flash 1.0

A recent academic study, [DF26](https://arxiv.org/abs/2609.07369), showed that humans are barely better than random when it comes to telling real videos from AI videos of people talking. Modern detectors were not much better. This is why we are very excited to release Pangram Image Flash 1.0, a faster model that particularly excels at frontier AI video detection. On DF26 it scores 98.7%, a 29-point leap over the next best detector.

![](https://www.pangram.com/_next/image?url=https%3A%2F%2Fcdn.sanity.io%2Fimages%2F6vunltag%2Fproduction%2F06b995c96492c188092df98cb5ebac7523ee2a7b-2160x1303.png%3Fw%3D1200%26q%3D85%26auto%3Dformat&w=3840&q=75)

### Industry Comparison

To compare against the industry, we took 129 real social media videos and generated AI mirrors using 14 different frontier video generators (100 videos each, sampled from the 129 real videos). The generators were each seeded with the first real frame from the input video and then prompted with a VLM-generated description of what happens in the video to make a difficult synthetic mirror. We then compared our model against multiple major AI video detection services across the 1,529 total videos:

![](https://www.pangram.com/_next/image?url=https%3A%2F%2Fcdn.sanity.io%2Fimages%2F6vunltag%2Fproduction%2F6096d4a93191aa9fe11f5bc71d5ef8f93d66b268-2160x1620.png%3Fw%3D1200%26q%3D85%26auto%3Dformat&w=3840&q=75)

![An example of a real video (left) and its synthetic mirror (right). Could you tell which is real?](https://www.pangram.com/_next/image?url=https%3A%2F%2Fcdn.sanity.io%2Fimages%2F6vunltag%2Fproduction%2F5c56eb507c3b49ada5a78fa0137a740bb801eb8e-720x640.gif%3Fw%3D1200%26q%3D85%26auto%3Dformat&w=3840&q=75)An example of a real video (left) and its synthetic mirror (right). Could you tell which is real?

By starting from real video frames and creating a synthetic mirror, we can evaluate the model’s ability to distinguish between two very realistic looking videos. This prevents the model from introducing bias towards calling certain types of content AI, since each real video and its mirror share the same scene and subject.

### Academic AI Video Detection Benchmarks

We compare our Image Flash model against several academic benchmarks for frontier AI video detection. We achieve state-of-the-art results across multiple benchmarks, which together include almost every frontier generation model for AI video.

![](https://www.pangram.com/_next/image?url=https%3A%2F%2Fcdn.sanity.io%2Fimages%2F6vunltag%2Fproduction%2Fc19ab0a62f416d4d60a5b3fe9179e4fa58d69cc6-2400x1360.png%3Fw%3D1200%26q%3D85%26auto%3Dformat&w=3840&q=75)

Sources: [AIGVDBench](https://arxiv.org/abs/2601.11035) · [DF26](https://arxiv.org/abs/2609.07369) · [Ivy-Fake](https://arxiv.org/abs/2506.00979)

### Native Video Support

Pangram now natively supports video upload. [Upload a video](https://www.pangram.com/video-detection) and Pangram Image Flash 1.0 will analyze it frame by frame to give you a complete analysis of which parts of the video come back as AI detected.

## Chrome Extension

We have integrated Pangram Image 1.0 into our Chrome extension. This will flag AI-generated content while you scroll social media, online shopping, rental listings, and any other site where knowing that what you’re seeing is real matters.

[Try it here](https://www.pangram.com/solutions/chrome-extension)

## API

With the release of Pangram Image 1.0 and Flash 1.0, we updated our API to support both image and video detection. We are excited to put the capability of these models into the hands of the community.

[API Documentation](https://www.pangram.com/solutions/api)
