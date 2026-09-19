---
title: SIMD Masking
link: https://www.computerenhance.com/p/simd-masking
source: computerenhance-com
published: 2026-08-31T20:51:15Z
updated: 2026-08-31T20:51:15Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: Once we execute both sides of an if/else, we need some way of merging the results.
content: extracted
html: 2026-08-31-simd-masking.html
preview:
  file: 2026-08-31-simd-masking.preview-8205bc6a580b.webp
  width: 256
  height: 144
  color: '#2d2b30'
images:
- source: https://substack-video.s3.amazonaws.com/video_upload/post/213610664/30ec8d6b-4c6e-46bf-865c-f5b2bb0151db/transcoded-47959.png
  original:
    file: 2026-08-31-simd-masking.image-a76bc97d05c3.png
    width: 1920
    height: 1080
  variants:
  - file: 2026-08-31-simd-masking.image-11dae0f27c6e.webp
    width: 320
    height: 180
  - file: 2026-08-31-simd-masking.image-2f3c9d87b547.webp
    width: 640
    height: 360
  - file: 2026-08-31-simd-masking.image-85bbab73aace.webp
    width: 960
    height: 540
  - file: 2026-08-31-simd-masking.image-4994d409322e.webp
    width: 1280
    height: 720
  - file: 2026-08-31-simd-masking.image-0338cb2d7298.webp
    width: 1600
    height: 900
  - file: 2026-08-31-simd-masking.image-feab8b4fc34b.webp
    width: 1920
    height: 1080
  color: '#141414'
- source: https://substackcdn.com/image/fetch/$s_!JIxq!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb3b2f687-f82d-4bc0-8bd0-b5e0c8a4fa97_3840x2160.jpeg
  original:
    file: 2026-08-31-simd-masking.image-f7d773105b3d.jpg
    width: 1456
    height: 819
  color: '#282319'
- source: https://substackcdn.com/image/fetch/$s_!fGm8!,w_96,h_96,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdecce376-01ed-4022-bb6a-ed1d1322183e_800x800.png
  original:
    file: 2026-08-31-simd-masking.image-a24e39d775c3.jpg
    width: 96
    height: 96
  color: '#040506'
---

Playback speed

×

Share post

Share post at current time

0:00

/

### Paid episode

The full episode is only available to paid subscribers of Computer, Enhance!

Once we execute both sides of an if/else, we need some way of merging the results.

Aug 31, 2026

∙ Paid

[![](https://substackcdn.com/image/fetch/$s_!JIxq!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb3b2f687-f82d-4bc0-8bd0-b5e0c8a4fa97_3840x2160.jpeg)](https://substackcdn.com/image/fetch/$s_!JIxq!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb3b2f687-f82d-4bc0-8bd0-b5e0c8a4fa97_3840x2160.jpeg)

*This is the sixteenth video in Part 5 of the Performance-Aware Programming series. Please see the [Table of Contents](https://www.computerenhance.com/p/table-of-contents) to quickly navigate through the rest of the course as it is updated, and [the code repository](https://github.com/cmuratori/computer_enhance) for downloadable code listings.*

[![Computer, Enhance!](https://substackcdn.com/image/fetch/$s_!fGm8!,w_96,h_96,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdecce376-01ed-4022-bb6a-ed1d1322183e_800x800.png)](https://www.computerenhance.com/s/programming-courses)

Programming Courses

A series of courses on programming topics.
