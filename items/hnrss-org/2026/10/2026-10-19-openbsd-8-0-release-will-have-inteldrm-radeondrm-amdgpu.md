---
title: OpenBSD 8.0 release will have inteldrm/radeondrm/amdgpu drivers from Linux 6.18
link: https://bsd.network/@brynet/117407035545900963
source: hnrss-org
published: 2026-10-19T21:11:45Z
updated: 2026-10-19T21:11:45Z
first_seen: 2026-10-20T07:25:43.733405278Z
authors:
- doener
content: extracted
html: 2026-10-19-openbsd-8-0-release-will-have-inteldrm-radeondrm-amdgpu.html
remote_preview:
  url: https://media.bsd.network/accounts/avatars/000/001/413/original/d113a02fb84e4f5a.jpg
---

I think it's about time for some [#OpenBSD](https://bsd.network/tags/OpenBSD) [#80HYPE](https://bsd.network/tags/80HYPE) ![:flan_hacker:](https://media.bsd.network/custom_emojis/images/000/011/234/original/flan_hacker.png ":flan_hacker:") ![:flan_cool:](https://media.bsd.network/custom_emojis/images/000/011/235/original/flan_cool.png ":flan_cool:")

> Clang/LLVM 22 is now OpenBSD's base compiler on clang architectures, including updated libc++{,abi}, compiler-rt & libunwind support libraries.

[https:// bsd.network/@brynet/1166580624 79940402](https://bsd.network/@brynet/116658062479940402)

> OpenBSD/sparc64 switches to Clang by default! This was the last significant architecture to switch. This makes OpenBSD the only operating system to support sparc64 with a modern toolchain.

[https:// bsd.network/@brynet/1171301305 36792528](https://bsd.network/@brynet/117130130536792528)

> Since 7.9, Jonathan Gray (jsg@) has continued to update the drm graphics stack (inteldrm/radeondrm/amdgpu) bringing OpenBSD 8.0 to Linux 6.18.y/6.18.53, adding driver support for Intel Arc A-Series (DG2/Alchemist) cards.
>
> Increased KVA (kernel virtual addresses) on amd64 (to 512G) now allows mapping more than 4G of VRAM (for example, GPU compute w/ Vulkan)

[https:// freshbsd.org/openbsd/src?q=drm &committer\[\]=jsg](https://freshbsd.org/openbsd/src?q=drm&committer[]=jsg)

[https:// bsd.network/@brynet/1167944616 42940154](https://bsd.network/@brynet/116794461642940154)

> After a ton of work in Edmonton by Mike Larkin (mlarkin@) and Dave Voutila (dv@), OpenBSD vmm/vmd(8) now supports SMP guests up to 64 VCPUs per VM! ![:flan_cool:](https://media.bsd.network/custom_emojis/images/000/011/235/original/flan_cool.png ":flan_cool:") 🎉

[https:// bsd.network/@brynet/1172950441 09301163](https://bsd.network/@brynet/117295044109301163)

[https:// bsd.network/@brynet/1172997536 68348206](https://bsd.network/@brynet/117299753668348206)

> OpenBSD 8.0 adds qwz(4), new WiFi driver for Qualcomm WCN7850 "ath12k" devices.

[https:// bsd.network/@brynet/1165743693 82033904](https://bsd.network/@brynet/116574369382033904)

> A new driver, sambat(4), for monitoring battery status, cycle count/charge info on Samsung Galaxy Book4 Edge (Snapdragon arm64) machines.

[https:// bsd.network/@brynet/1166006745 74234896](https://bsd.network/@brynet/116600674574234896)

> Accelerated swap encryption on amd64 and arm64 by using AES CPU instructions.

[https:// bsd.network/@brynet/1170908422 36460135](https://bsd.network/@brynet/117090842236460135)

> OpenBSD/arm64 and OpenBSD/riscv64 boards can now heartbeat blinking LEDs to show system load averages.
>
> ![:flan_hurrah:](https://media.bsd.network/custom_emojis/images/000/024/443/original/flan_hurrah.png ":flan_hurrah:") Das Blinkenlights!! ![:flan_hurrah:](https://media.bsd.network/custom_emojis/images/000/024/443/original/flan_hurrah.png ":flan_hurrah:")

[https:// bsd.network/@brynet/1168794791 68132932](https://bsd.network/@brynet/116879479168132932)

> [#OpenBSD](https://bsd.network/tags/OpenBSD) httpd(8) now supports many new features:
>
> - Setting custom HTTP response headers.
> - New blocking/filtering rules based on received HTTP headers (e.g: User-Agent: )

[https:// bsd.network/@brynet/1173716044 63587328](https://bsd.network/@brynet/117371604463587328)

[https:// bsd.network/@brynet/1169791332 75422995](https://bsd.network/@brynet/116979133275422995)

See also rsadowski@'s report: [https:// rsadowski.de/posts/2026/w2k26- edmonton-openbsd-hackathon/](https://rsadowski.de/posts/2026/w2k26-edmonton-openbsd-hackathon/)

.. and finally, ![:flan_peek:](https://media.bsd.network/custom_emojis/images/000/011/828/original/flan_peek.png ":flan_peek:")

> I made my first commit to [#OpenSSH](https://bsd.network/tags/OpenSSH) this release cycle!

[https:// bsd.network/@brynet/1170078335 31799959](https://bsd.network/@brynet/117007833531799959)

And lots more to see in 8.0! ![:flan_thumbs:](https://media.bsd.network/custom_emojis/images/000/011/812/original/flan_thumbs.png ":flan_thumbs:")![:flan_hacker:](https://media.bsd.network/custom_emojis/images/000/011/234/original/flan_hacker.png ":flan_hacker:")

EDIT: 8.0 is released! [https:// bsd.network/@brynet/1174706613 42565419](https://bsd.network/@brynet/117470661342565419)
