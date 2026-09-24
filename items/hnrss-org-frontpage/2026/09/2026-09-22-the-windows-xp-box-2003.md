---
title: The "Windows XP Box" (2003)
link: https://www.mini-itx.com/projects/windowsxpbox/
source: hnrss-org-frontpage
published: 2026-09-22T03:09:04Z
updated: 2026-09-22T03:09:04Z
first_seen: 2026-09-24T06:05:26.292611038Z
authors:
- doubletwoyou
summary: 'Article URL: https://www.mini-itx.com/projects/windowsxpbox/ Comments URL: https://news.ycombinator.com/item?id=49796372 Points: 104 # Comments: 16'
content: extracted
html: 2026-09-22-the-windows-xp-box-2003.html
preview:
  file: 2026-09-22-the-windows-xp-box-2003.preview-d5947117ee04.webp
  width: 256
  height: 48
  color: '#4e321e'
images:
- source: https://www.mini-itx.com/images/mini-itx-logotype-with-m-256px.png
  original:
    file: 2026-09-22-the-windows-xp-box-2003.image-d41e77cd17f8.png
    width: 256
    height: 48
  variants:
  - file: 2026-09-22-the-windows-xp-box-2003.image-d5947117ee04.webp
    width: 256
    height: 48
  color: '#f27720'
- source: https://www.mini-itx.com/projects/windowsxpbox/images/windowsxpbox0001.jpg
  original:
    file: 2026-09-22-the-windows-xp-box-2003.image-f9700fec41ef.jpg
    width: 320
    height: 240
  color: '#cba36a'
- source: https://www.mini-itx.com/projects/windowsxpbox/images/windowsxpbox0002.jpg
  original:
    file: 2026-09-22-the-windows-xp-box-2003.image-b6693df262ed.jpg
    width: 320
    height: 240
  color: '#aa8435'
- source: https://www.mini-itx.com/projects/windowsxpbox/images/windowsxpbox0003.jpg
  original:
    file: 2026-09-22-the-windows-xp-box-2003.image-57c79bf558c3.jpg
    width: 320
    height: 240
  color: '#aa8435'
- source: https://www.mini-itx.com/projects/windowsxpbox/images/windowsxpbox0004.jpg
  original:
    file: 2026-09-22-the-windows-xp-box-2003.image-71f0147d5813.jpg
    width: 320
    height: 240
  color: '#b9953b'
- source: https://www.mini-itx.com/projects/windowsxpbox/images/windowsxpbox0005.jpg
  original:
    file: 2026-09-22-the-windows-xp-box-2003.image-f5f28c95209d.jpg
    width: 320
    height: 240
  color: '#231b06'
- source: https://www.mini-itx.com/projects/windowsxpbox/images/windowsxpbox0006.jpg
  original:
    file: 2026-09-22-the-windows-xp-box-2003.image-0515bc05516a.jpg
    width: 320
    height: 240
  color: '#866628'
---

The "Windows XP Box"

Introduction

I needed a small Windows XP machine and a Mini-ITX board was the obvious choice. So I decided to build my "Windows XP Box" in a Windows XP box. The external dimensions of the box are a tiny 243mm x 200mm x 48mm.

![Click image to magnify](https://www.mini-itx.com/projects/windowsxpbox/images/windowsxpbox0001.jpg)

My requirements were:

- Use as powerful a motherboard as possible for the size i.e. a Nehemiah EPIA M10000.
- It must have an internal CD drive (this size eating requirement turned out be be very hard to meet).
- No bits could be cut off the motherboard to make it fit.
- The box must not bulge in an unsightly way, nor can the box be made bigger.
- It must not burst into flames when working hard (this should always be a requirement of a computer project).

 Fortunately there is no longer any requirement for an internal floppy drive. That would have have defeated me.

Construction

The bits arrive and it looks like an impossible task, with too many bits to fit in a small space.

![Click image to magnify](https://www.mini-itx.com/projects/windowsxpbox/images/windowsxpbox0002.jpg)

I nearly gave up and decided it was an impossible task. The Windows XP box was 3mm thinner and 12 mm narrower than the Adobe Acrobat box I had measured up when first deciding if the project was going to be possible. The challenge was to arrange the components into a 3D jigsaw, then decide how to build enough of an internal support case to get everything to stay in place.

Eventually it looked like I might have a possible layout, but the tolerances were tight. I had 6mm to spare on the long internal dimension of the box and only 3mm to spare on the thickness of the box, and this was not allowing for any thickness for the internal support case that holds everything in place.

![Click image to magnify](https://www.mini-itx.com/projects/windowsxpbox/images/windowsxpbox0003.jpg)

In order to mount all the bits I was going to have to make an inner support case that would tightly slide into the cardboard box. I chose Wonderboard plastic as my construction material because it is reasonably strong and very easy to work with (it cuts with a Stanley knife). It would have been nice to use aluminium, but the cramped design made the chances of a short circuit too great.

The first construction step was to cut out a base plate the exact size of the inside of the cardboard box and double check where the bits will fit.

As the Wonderboard was 3mm thick this reduced my tolerance in two dimensions to zero. The CD drive would touch one side of the inner support case. The deep part of the CD drive would touch the heat sink on the motherboard, with the narrow bit being able to overlap it, and the far side of the motherboard touches the other side of the Wonderboard case. In the other dimension it was even harder. The top of the sound connector would touch the support case, and the underside of the motherboard would touch the cardboard box. Fortunately the hard drive can slide under the motherboard as this is above (below?) the unused PCI slot. The only place left for the PSU was above the hard drive with the bulky connectors facing down towards the CD both to the front and the back of the hard drive.

![Click image to magnify](https://www.mini-itx.com/projects/windowsxpbox/images/windowsxpbox0004.jpg)

Now I could position the CD drive hard against the side and start assembling the support case. In the next picture you can see the step up between the thin part of the CD drive and the thicker part of the main body of the drive. The heat sink on the Mini-ITX board touches this step.

![Click image to magnify](https://www.mini-itx.com/projects/windowsxpbox/images/windowsxpbox0005.jpg)

After much cutting and half a tube of glue the case was finished. I built pillars to support three corners of the motherboard and the power supply and added brackets to support the CD and the hard drive. In such a compact design cooling was a concern so I made fan mounting points in opposite corners of the case. To keep the CPU nice and cool I cut a hole for it it the side of the case and glued in a couple of plates to act as ducting so the CPU fan will only suck in cold outside air. The other two fans are the exhaust points. The fan guards were cut out of a metal speaker grill using an angle grinder as neatly drilling that many holes is just not fun. Angle grinders are almost as much fun to use as chain saws.

![Click image to magnify](https://www.mini-itx.com/projects/windowsxpbox/images/windowsxpbox0006.jpg)
