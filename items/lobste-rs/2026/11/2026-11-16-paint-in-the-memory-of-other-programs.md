---
title: Paint in the memory of other programs
link: https://citrons.xyz/journal/2026-04-13
source: lobste-rs
published: 2026-11-16T03:37:36Z
updated: 2026-11-16T03:37:36Z
first_seen: 2026-11-17T01:00:47.489879523Z
authors:
- citrons.xyz via gavinmorrow
labels:
- programming
- visualization
summary: Comments
content: extracted
html: 2026-11-16-paint-in-the-memory-of-other-programs.html
remote_preview:
  url: https://citrons.xyz/files/LIKxVNQU.png
  alt: the cool S, a heart, and a cat scribbled above a program's stack
---

april 13, 2026

[![the cool S, a heart, and a cat scribbled above a program's stack](https://citrons.xyz/files/LIKxVNQU.png)](https://citrons.xyz/files/LIKxVNQU.png)

*core* is [a program that I wrote](https://git.citrons.xyz/core/about) which spills the viscera of other programs onto the screen and allows you to prod them. it could be called an “esoteric debugger” or a “paint program.”

the visual content of the program is constructed as follows:

- pages of memory (hardcoded for now as 4096 bytes) are arranged on a Z-order curve.
- each page of memory is displayed as a 256x128 rectangle.
- each pixel represents a bit. a white pixel represents one, and a black pixel represents zero.
- the bits are arranged in rows, and the bytes are arranged in columns

[![the text and data segment of a program](https://citrons.xyz/files/j_ODSJFQ.png)](https://citrons.xyz/files/j_ODSJFQ.png)

as well as the display of program memory, core allows for its manipulation. if core shows you a picture of a program’s insides, then it is only natural that it allows you to scribble on it. the overcommitted empty space below the stack stack provides an inviting empty space to scribble in. it’s a perfect place to hide your terrible secrets in some poor, innocent process.

WARNING: scribbling on programs may cause loss of data!

core is the logical conclusion of blinkenlights, “Allblinkenlights,” if you will. the intended experience is evocative aimlessness. the human visual system is highly parallel and very good at recognizing patterns. you are encouraged to allow these patterns to wash over you.

many textures arise from different kinds of data in memory. ASCII text is incredibly recognizable, by virtue of being a 7-bit encoding, as well as having a biased distribution of values. 64-bit pointers are similarly easy to recognize because their last two bytes will either be zero or one.

[![a view of the stack of a program. there are text and arrows scribbled in empty spaces, pointing to ASCII data, *envp and *argv, and return addresses](https://citrons.xyz/files/PzvOTBbx.png)](https://citrons.xyz/files/PzvOTBbx.png)

in the mesmerizing undulations of complex programs, these textures may blink or move around or stay boringly still. if you are keen, you may catch some large data structure being allocated or filled in. or you may see the first generation of a garbage collector fly by as short-lived objects are created at a rapid rate. in games, the stack can be seen flickering rapidly and changing every frame.

core can make stark the immensity of memory and the way that programs use it. data structures and programming patterns appear in the physical reality of which bits are toggled where.

I was very keen to see what a program would look like that uses continuation passing style implemented as in the famous paper, [CONS should not CONS its arguments, part II: Cheney on the M.T.A](https://dl.acm.org/doi/10.1145/214448.214454). I compiled a game programmed in CHICKEN scheme and was delighted to see a stack that moved like a waterfall.

[![a stack continuously filled with pointers](https://citrons.xyz/files/UvkDiSvw.png)](https://citrons.xyz/files/UvkDiSvw.png)

graphical programs can be the most interesting. visual patterns in graphics in memory can emerge vividly.

[![sprites from super mario brothers are visible in the memory](https://citrons.xyz/files/eSPOP7T+.png)](https://citrons.xyz/files/eSPOP7T+.png)

when attached to virtual machines or emulators, core gains omniscience over an entire operating system.

[![many, many pages filling the VM memory porously](https://citrons.xyz/files/M_ujZTMA.png)](https://citrons.xyz/files/M_ujZTMA.png)

it is astounding to see an immense creature flicker to life. pages fill empty space in speckled patterns. panning over a chasm, the page tables can be seen vibrating and filling in with new allocations. it is reminiscent, perhaps, of insects, like a beehive.

one could not hope to wholly understand the way that physical memory is arranged, but they may peer voyeuristically into random pages of process memory, abound with text strings and recognizable graphics.

[![windows XP icons are visible in the memory: two Xs, an information symbol, and a question mark bubble](https://citrons.xyz/files/5_iWtVQQ.png)](https://citrons.xyz/files/5_iWtVQQ.png)

[![more XP icons: more Xs, an alert symbol](https://citrons.xyz/files/dNhSc6rG.png)](https://citrons.xyz/files/dNhSc6rG.png)

programs often exist in the mind as something ineffable, intangible, and abstract. even their physical existence is locked inside black plastic squares, harboring impossibly tiny transistors. in my opinion, to lay visceral the individual bits in a computer program is an idea that despite being simple, is immensely potent.

peer into the core.

![ferrite core memory](https://citrons.xyz/files/BIUqMjSM.png)

## journal

- 2026-09-13 [Lou Sou Gales ie la Ta](https://citrons.xyz/journal/2026-09-13)
- 2026-09-09 [type declaration syntax](https://citrons.xyz/journal/2026-09-09)
- 2026-06-01 [bootstrapping the latest go on 9front](https://citrons.xyz/journal/2026-06-01)
- 2026-05-20 [flight recorder](https://citrons.xyz/journal/2026-05-20)
- 2026-04-13 [paint in the memory of other programs](https://citrons.xyz/journal/2026-04-13)
- 2025-12-29 [life without scotch tape](https://citrons.xyz/journal/2025-12-29)
- 2025-11-23 [have you heard of this website called Google? part II](https://citrons.xyz/journal/2025-11-23)
- 2025-10-24 [have you heard of this website called Google?](https://citrons.xyz/journal/2025-10-24)
- 2025-09-12 [distributions are good!](https://citrons.xyz/journal/2025-09-12)
- [more...](https://citrons.xyz/journal)
