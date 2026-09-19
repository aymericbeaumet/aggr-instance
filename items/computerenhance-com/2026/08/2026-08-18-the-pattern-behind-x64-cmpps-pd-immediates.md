---
title: The Pattern Behind x64 CMPPS/PD Immediates
link: https://www.computerenhance.com/p/the-pattern-behind-x64-cmppspd-immediates
source: computerenhance-com
published: 2026-08-18T23:21:28Z
updated: 2026-08-18T23:21:28Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Casey Muratori
summary: If you're wondering how the 5 bits used in the CMPPD/PS immediate got assigned the way they did, Pete Cawley's got your back.
content: extracted
html: 2026-08-18-the-pattern-behind-x64-cmpps-pd-immediates.html
preview:
  file: 2026-08-18-the-pattern-behind-x64-cmpps-pd-immediates.preview-5bc606bd7796.webp
  width: 217
  height: 256
  color: '#e7e7e7'
images:
- source: https://substackcdn.com/image/fetch/$s_!yQYq!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4ab4a228-d21c-4864-b6cc-b3329621e785_2006x2370.png
  original:
    file: 2026-08-18-the-pattern-behind-x64-cmpps-pd-immediates.image-078a52778c3d.png
    width: 2006
    height: 2370
  variants:
  - file: 2026-08-18-the-pattern-behind-x64-cmpps-pd-immediates.image-3ce57c3dbe99.webp
    width: 320
    height: 378
  - file: 2026-08-18-the-pattern-behind-x64-cmpps-pd-immediates.image-4d71181f5d05.webp
    width: 640
    height: 756
  - file: 2026-08-18-the-pattern-behind-x64-cmpps-pd-immediates.image-acc1fc877bca.webp
    width: 960
    height: 1134
  - file: 2026-08-18-the-pattern-behind-x64-cmpps-pd-immediates.image-b48864c9465e.webp
    width: 1280
    height: 1512
  - file: 2026-08-18-the-pattern-behind-x64-cmpps-pd-immediates.image-f0c6db776500.webp
    width: 2006
    height: 2370
  color: '#f8f8f8'
- source: https://substackcdn.com/image/fetch/$s_!yQYq!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4ab4a228-d21c-4864-b6cc-b3329621e785_2006x2370.png
  original:
    file: 2026-08-18-the-pattern-behind-x64-cmpps-pd-immediates.image-245484fff718.jpg
    width: 1456
    height: 1720
  color: '#f7f7f7'
- source: https://substackcdn.com/image/fetch/$s_!uUJy!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa8dc40ab-1977-44f7-adb4-81ab32b3745f_1574x666.png
  original:
    file: 2026-08-18-the-pattern-behind-x64-cmpps-pd-immediates.image-fe6c8331c89f.jpg
    width: 1456
    height: 616
  color: '#e4cdbb'
---

While I can’t say it matters for any practical purpose, I have more-than-once wondered how Intel came up with this table:

[![](https://substackcdn.com/image/fetch/$s_!yQYq!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4ab4a228-d21c-4864-b6cc-b3329621e785_2006x2370.png)](https://substackcdn.com/image/fetch/$s_!yQYq!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F4ab4a228-d21c-4864-b6cc-b3329621e785_2006x2370.png)

Table 1-2 from the CMPPD entry in Volume 2, Chapter 3 of the Intel 64 and IA-32 Architectures Software Developer’s Manual

The meaning of these immediate values is straightforward enough if you understand the `cmpps` and `cmppd` instructions. First, you pick one of eight different comparisons: equal, less-than, less-than-or-equal, always false, not equal, greater-than-or-equal, greater-than, and always true. Then, you pick whether or not you want the comparison to be signaling [1](https://www.computerenhance.com/p/the-pattern-behind-x64-cmppspd-immediates#footnote-1) . Finally, you pick whether you want NaN’s to pass through as true values or false values (called “unordered” vs. “ordered” comparisons in this context).

Once you’ve made those three decisions, you combine them together and you pick the immediate value that corresponds to your choice. Eight possible comparisons, times two for signaling-or-not, times another two for ordered-or-not, equals thirty-two different values. Great! That’s exactly how many are in the table. Everything checks out.

But how did Intel choose which combination of immediate bits mapped to which combination of choices? Naively, one might think that a bit would be set whenever the comparison is signaling. Another might be set if the comparison is unordered. And so on.

To some extent, this does seem to be true. 0H, 8H, 10H, and 18H code for the four different possible equals comparisons. This would imply that the 4th and 5th bits must code for ordering and signaling, respectively, right?

Well, 0H, where the 4th bit is unset, is an ordered equals. 8H, where the 4th bit is set, is an unordered equals. So the 4th bit must code for an unordered comparison.

Except wait - 4H, which does *not* have the 4th bit set, is an unordered not-equals. CH, which *does* have the 4th bit set, is the ordered version! So the bit seems to code for the opposite in this case.

This kind of pattern continues whenever you look at the values. You can find *something* like a relationship between bits, but never quite what you would expect.

Recently, I asked The Internet if anyone knew what was actually going on here. While it unfortunately rewards my lazy behavior, I am delighted to report that [Pete Cawley](https://x.com/corsix) worked out what was likely going on, and wrote it up:

[![](https://substackcdn.com/image/fetch/$s_!uUJy!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa8dc40ab-1977-44f7-adb4-81ab32b3745f_1574x666.png)](https://www.corsix.org/content/encoding-vcmpps-vcmppd)

It’s a quick read, and covers the entire immediate encoding. So, if you’re like me and are curious as to how these sorts of things end up the way they do, I highly recommend you [head over to Pete’s writeup](https://www.corsix.org/content/encoding-vcmpps-vcmppd) and check it out!

[1](https://www.computerenhance.com/p/the-pattern-behind-x64-cmppspd-immediates#footnote-anchor-1)

Which, unless you’ve unmasked them yourself, usually won’t signal either way, since NaN exceptions are masked out by default on the most x64-based OSes.
