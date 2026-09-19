---
title: Three Steps from Scalar to SIMD
link: https://www.computerenhance.com/p/three-steps-from-scalar-to-simd
source: computerenhance-com
published: 2026-08-03T17:35:21Z
updated: 2026-08-03T17:35:21Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: When we want to change the interior of a loop (with complex control flow) from processing one thing to processing several at a time, it's best to tackle the problem in three sequential steps.
content: extracted
html: 2026-08-03-three-steps-from-scalar-to-simd.html
preview:
  file: 2026-08-03-three-steps-from-scalar-to-simd.preview-c15aaec1bd37.webp
  width: 256
  height: 144
  color: '#2b272c'
images:
- source: https://substack-video.s3.amazonaws.com/video_upload/post/209667660/608e4b6d-02a2-42c0-987c-d4664c46a147/transcoded-44145.png
  original:
    file: 2026-08-03-three-steps-from-scalar-to-simd.image-83c513c9c2ad.png
    width: 1920
    height: 1080
  variants:
  - file: 2026-08-03-three-steps-from-scalar-to-simd.image-aaec51eec4f1.webp
    width: 320
    height: 180
  - file: 2026-08-03-three-steps-from-scalar-to-simd.image-f3c519b22a09.webp
    width: 640
    height: 360
  - file: 2026-08-03-three-steps-from-scalar-to-simd.image-b3e96f6f1535.webp
    width: 960
    height: 540
  - file: 2026-08-03-three-steps-from-scalar-to-simd.image-1cc9170850ae.webp
    width: 1280
    height: 720
  - file: 2026-08-03-three-steps-from-scalar-to-simd.image-60b112c42c34.webp
    width: 1600
    height: 900
  - file: 2026-08-03-three-steps-from-scalar-to-simd.image-3bbfd6e99674.webp
    width: 1920
    height: 1080
  color: '#141414'
- source: https://substackcdn.com/image/fetch/$s_!SESu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd43570a2-1431-41d8-a8e6-26df51f9de46_3840x2160.jpeg
  original:
    file: 2026-08-03-three-steps-from-scalar-to-simd.image-9cef84ca35be.jpg
    width: 1456
    height: 819
  color: '#171512'
---

Playback speed

×

Share post

Share post at current time

0:00

/

### Paid episode

The full episode is only available to paid subscribers of Computer, Enhance!

When we want to change the interior of a loop (with complex control flow) from processing one thing to processing several at a time, it's best to tackle the problem in three sequential steps.

Aug 03, 2026

∙ Paid

[![](https://substackcdn.com/image/fetch/$s_!SESu!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd43570a2-1431-41d8-a8e6-26df51f9de46_3840x2160.jpeg)](https://substackcdn.com/image/fetch/$s_!SESu!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd43570a2-1431-41d8-a8e6-26df51f9de46_3840x2160.jpeg)

*This is the thirteenth video in Part 5 of the Performance-Aware Programming series. Please see the [Table of Contents](https://www.computerenhance.com/p/table-of-contents) to quickly navigate through the rest of the course as it is updated, and [the code repository](https://github.com/cmuratori/computer_enhance) for downloadable code listings.*
