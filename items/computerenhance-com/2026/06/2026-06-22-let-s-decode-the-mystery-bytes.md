---
title: Let's Decode the Mystery Bytes!
link: https://www.computerenhance.com/p/lets-decode-the-mystery-bytes
source: computerenhance-com
published: 2026-06-22T17:48:57Z
updated: 2026-06-22T17:48:57Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Casey Muratori
summary: In this follow-up to "Let's Answer Ray's Question!", we go all the way down the rabbit hole to decode the mysterious eight bytes that kept showing up in-between Ray's allocations.
content: extracted
html: 2026-06-22-let-s-decode-the-mystery-bytes.html
preview:
  file: 2026-06-22-let-s-decode-the-mystery-bytes.preview-26882fd35cb3.webp
  width: 256
  height: 144
  color: '#b6b8bb'
images:
- source: https://substackcdn.com/image/youtube/w_728,c_limit/GZqB4D_Do38
  original:
    file: 2026-06-22-let-s-decode-the-mystery-bytes.image-8d3c448d5e46.jpg
    width: 728
    height: 410
  color: '#f9faf9'
---

Last month, I posted *[Let’s Answer Ray’s Question!](https://www.computerenhance.com/p/lets-answer-rays-question)* - a video where we looked at the memory behavior of two different compilations of the same program. I ended that video by saying that if you could read assembly language (like you can if you did [Part I of the](https://www.computerenhance.com/i/99218768/performance-aware-programming-series) *[Performance Aware Programming](https://www.computerenhance.com/i/99218768/performance-aware-programming-series)*[course](https://www.computerenhance.com/i/99218768/performance-aware-programming-series) !), you could go much further and determine even more specifics.

In this video, we do exactly that. Using disassembly, memory view, and heap debugging, we zero in on exactly who is setting the magic eight bytes we saw next to Ray’s allocations in the first video - and we figure the convoluted way those bytes are encoded.

*If you’d like to be emailed when the third and final video is posted, you can subscribe your email address to either our free or paid tiers using the button below.*
