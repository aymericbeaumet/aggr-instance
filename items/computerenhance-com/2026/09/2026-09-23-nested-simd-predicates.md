---
title: Nested SIMD Predicates
link: https://www.computerenhance.com/p/nested-simd-predicates
source: computerenhance-com
published: 2026-09-23T19:29:51Z
updated: 2026-09-23T19:29:51Z
first_seen: 2026-09-23T22:13:44.124112615Z
summary: Before moving on, it’s worth looking at how we would handle more complex predicates than the ones present in the haversine loop.
content: extracted
html: 2026-09-23-nested-simd-predicates.html
preview:
  file: 2026-09-23-nested-simd-predicates.preview-468770e9221b.webp
  width: 256
  height: 144
  color: '#29262a'
images:
- source: https://substack-video.s3.amazonaws.com/video_upload/post/217124887/8977a831-ab97-4461-89ed-ed614e33357e/transcoded-47832.png
  original:
    file: 2026-09-23-nested-simd-predicates.image-15959cf7330e.png
    width: 1920
    height: 1080
  variants:
  - file: 2026-09-23-nested-simd-predicates.image-0936a6f7f2df.webp
    width: 320
    height: 180
  - file: 2026-09-23-nested-simd-predicates.image-3bf12329f3f6.webp
    width: 640
    height: 360
  - file: 2026-09-23-nested-simd-predicates.image-9620c45ad7c5.webp
    width: 960
    height: 540
  - file: 2026-09-23-nested-simd-predicates.image-50c049432591.webp
    width: 1280
    height: 720
  - file: 2026-09-23-nested-simd-predicates.image-25911f922f54.webp
    width: 1600
    height: 900
  - file: 2026-09-23-nested-simd-predicates.image-c05118e9f343.webp
    width: 1920
    height: 1080
  color: '#141414'
- source: https://substackcdn.com/image/fetch/$s_!g9TJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3bbd5b12-065d-4250-ac02-be3a277fcf7a_3840x2160.jpeg
  original:
    file: 2026-09-23-nested-simd-predicates.image-099047e40c07.jpg
    width: 1456
    height: 819
  color: '#3b3733'
- source: https://substackcdn.com/image/fetch/$s_!FgJW!,w_96,h_96,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack.com%2Fimg%2Fpodcast%2Fgeneric.png
  original:
    file: 2026-09-23-nested-simd-predicates.image-b04d0f20b684.png
    width: 96
    height: 96
  color: '#0a6354'
---

Before moving on, it’s worth looking at how we would handle more complex predicates than the ones present in the haversine loop.

Sep 23, 2026

∙ Paid

[![](https://substackcdn.com/image/fetch/$s_!g9TJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3bbd5b12-065d-4250-ac02-be3a277fcf7a_3840x2160.jpeg)](https://substackcdn.com/image/fetch/$s_!g9TJ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3bbd5b12-065d-4250-ac02-be3a277fcf7a_3840x2160.jpeg)

*This is the eighteenth video in Part 5 of the Performance-Aware Programming series. Please see the [Table of Contents](https://www.computerenhance.com/p/table-of-contents) to quickly navigate through the rest of the course as it is updated, and [the code repository](https://github.com/cmuratori/computer_enhance) for downloadable code listings.*

## The full video is for paid subscribers

[Already a paid subscriber? **Sign in**](https://substack.com/sign-in?redirect=%2Fp%2Fnested-simd-predicates&for_pub=computerenhance&change_user=false)

[![Computer, Enhance!](https://substackcdn.com/image/fetch/$s_!FgJW!,w_96,h_96,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack.com%2Fimg%2Fpodcast%2Fgeneric.png)](https://www.computerenhance.com/s/programming-courses)

Programming Courses

A series of courses on programming topics.
