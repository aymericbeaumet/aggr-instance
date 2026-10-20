---
title: Writing a Demoscene Intro for the ZX Spectrum
link: https://www.markround.com/blog/2026/zx-spectrum-intro
source: hnrss-org
published: 2026-10-12T19:40:00Z
updated: 2026-10-12T19:40:00Z
first_seen: 2026-10-13T11:00:50.474439149Z
authors:
- zdw
labels:
- spectrum
content: extracted
html: 2026-10-12-writing-a-demoscene-intro-for-the-zx-spectrum.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50061859
  num_comments: 26
  points: 136
---

*A look at the tools, techniques and challenges of creating a retro-style 'crack intro'*

[![](https://www.markround.com/assets/images/cassette.svg)Download Now](https://www.markround.com/downloads/mdr-tnfstro.zip) [View on pouet.net](https://www.pouet.net/prod.php?which=107200) [Discussion on Hacker News](https://news.ycombinator.com/item?id=50061859)

## Intro

[![My +3 running my intro](https://www.markround.com/assets/generated/intro/speccy-800-1f31c3383.jpg)](https://www.markround.com/assets/images/intro/speccy.jpg)

In the late 80s, I spent an embarrassing number of hours hunched over my ZX Spectrum, typing in Sinclair BASIC programs and rudimentary assembler routines copied from magazines, trying to re-create the Amiga crack intros I’d seen round at a friend’s house. Obviously, they never came even slightly close - but now, many decades later, I’ve finally managed it! I’m now the proud author of a hand-rolled retro demoscene intro that runs on a stock 128Kb Spectrum. It features all those late 80s tropes: A smooth scrolltext (with ‘leet custom fonts!), a three-channel AY soundtrack, bouncing VU meters and the essential parallax starfield. It’s currently 10287 bytes of code, or 19552 bytes as a complete tape image - which, given I’ve been working on this since December 13th last year, means my average rate of code-production is a paltry ~ 455 bytes per week!

This blog post is a technical overview of how I got there, some cool tricks I picked up, and everything I had to learn (and re-learn) about the Speccy and Z80 assembly along the way. It simultaneously rates as one of my favourite side-projects ever, and also the one that most often nearly triggered a foot put through my monitor in frustration. I’ll reflect on all that at [the end](https://www.markround.com/blog/2026/zx-spectrum-intro#the-zen-of-z80), but for now if you just want to see the finished product, the download button is at the top of the article and the included `.tap` cassette tape image should load into any emulator. There’s also a [YouTube capture](https://youtu.be/ALbjMMgmAXo), links to the production on [pouet.net](https://www.pouet.net/prod.php?which=107200), and you can run it from within a web browser by visiting my [TNFS site portal](https://jsspeccy.markround.com) - check option 4 for recent uploads.

## Be Kind, Rewind

But let’s skip back briefly to the late 80s. Coming from the world of an 8-bit, cassette tape loading computer, seeing an Amiga for the first time was like encountering something from outer space. Like everyone who saw one back in the day, I was blown away by the amazing graphics and sound on display, not to mention the library of [ground-breaking games](https://www.lemonamiga.com/games/votes_list.php). But I found myself even more captivated by the mysterious (and totally badass) “intros” displayed at the beginning of pirated games.

Created by the [software piracy groups](https://www.kickstarter.com/projects/bitman/bootblock-rebels/) that had cracked the copy protection and distributed the floppy-disk based games, they were mini works of art in their own right - music, graphics and usually some cool-looking effect to show off the group’s prowess. There’s a great collection of these intros (or “cracktros”) [archived on YouTube](https://www.youtube.com/watch?v=50WWFEBsgfk).

Watch a few of those videos and you’ll quickly notice the “design” tropes that almost every cracktro shares: a cool, edgy logo at the top of the screen (bonus points if it’s dripping blood or in any way “metal”), some boastful text displayed on top of an effect of some sort - usually a scrolling parallax starfield, or rotating 3d object - and a further scroll-text full of shout outs, greetings to fellow pseudonymous crackers and adverts for illegal BBS systems galore. Needless to say, to a kid in the 80s/early 90s this was possibly the *coolest thing ever*.

[![I thought this one was the absolute bomb when I first saw it. Sine-scroller, pirate BBS adverts, cool logo with a sword - and of course, a starfield. There’s something for everyone here.](https://www.markround.com/assets/generated/intro/cracktros/horizon-720-692dc21a9.png)](https://www.markround.com/assets/images/intro/cracktros/horizon.png)

I thought this one was the absolute *bomb* when I first saw it. Sine-scroller, pirate BBS adverts, cool logo with a sword - and of course, a starfield. There’s something for everyone here.

[![The music from this is burned into my brain. I think pretty much everyone of my classmates knew this intro given the popularity of Lemmings in the early 90s. Skid Row as a group are still active to this day and this is probably the quintessential Amiga cracktro of the period.](https://www.markround.com/assets/generated/intro/cracktros/lemmings-720-38b76b747.png)](https://www.markround.com/assets/images/intro/cracktros/lemmings.png)

The music from this is burned into my brain. I think pretty much everyone of my classmates knew this intro given the popularity of Lemmings in the early 90s. Skid Row as a group are still active to this day and this is probably the quintessential Amiga cracktro of the period.

[![Endless Piracy. Edgy. Another cool logo, sine-wave scroller, starfield and boasting text. The classics never go out of style.](https://www.markround.com/assets/generated/intro/cracktros/piracy-720-e745a7f08.png)](https://www.markround.com/assets/images/intro/cracktros/piracy.png)

Endless Piracy. Edgy. Another cool logo, sine-wave scroller, starfield and boasting text. The classics never go out of style.

[![Another great combination of all the essential ingredients. Cool logo, even cooler text font, scrolling starfield and a scroll-text running along the bottom. Vintage stuff.](https://www.markround.com/assets/generated/intro/cracktros/spirit-720-271697994.png)](https://www.markround.com/assets/images/intro/cracktros/spirit.png)

Another great combination of all the essential ingredients. Cool logo, even cooler text font, scrolling starfield and a scroll-text running along the bottom. Vintage stuff.

[![Logo. Text. Scroller. But no starfield - otherwise a solid effort from Anthrox here :)](https://www.markround.com/assets/generated/intro/cracktros/anthrox-720-b8a1eac02.png)](https://www.markround.com/assets/images/intro/cracktros/anthrox.png)

Logo. Text. Scroller. But no starfield - otherwise a solid effort from Anthrox here :)

The [demoscene](https://en.wikipedia.org/wiki/Demoscene) grew out of these intros. The cracking eventually faded into the background, and coders, “graphicians” and musicians [all contributed](https://www.datagubbe.se/scenearch/) to feature-length productions that showcased their abilities, competed with other groups and pushed the machine to its limits. In amazing feats of ingenuity and pure “code as an art form”, the scene lives on today, with groups producing breathtaking productions like [this](https://youtu.be/2jciCr8zEhw) and [this](https://youtu.be/pYtleuGV7ok) - bear in mind, both of those are running on a 7Mhz home computer equipped with a floppy disk, 1Mb of memory and a sound chip from 1985.

But much as I admire the sheer skill and dedication that takes, it’s the early “single screen” scrolly-text-and-starfield cracktros I’ve always found captivating. They took on a sort of mystical quality for me, hinting at a scene and a culture I could only ever experience from the outside - which is exactly why I spent all those hours in Sinclair BASIC, trying in vain to re-create them, and dreaming of the day I’d eventually have an Amiga of my own.

## The Jump Back

I eventually did trade up to a range of Amiga systems, joined a few “scene” groups myself and threw myself into a [variety of activities](https://www.markround.com/blog/2026/06/17/25-its-only-when-you-look-back/) until exams and the end of my teenage years beckoned. I dabbled with Z80 code back then, and at one point knew enough 68k assembly to bluff my way through - partly from the Amiga’s heyday, partly from a few “embedded systems” modules at University where I had to program a set of traffic lights or lift-controllers. Given there’s not much call for that sort of thing at my $DAYJOB, though, it had all pretty much faded from my memory.

[![TNFS site screenshot](https://www.markround.com/assets/generated/intro/tnfs-800-01940097c.png)](https://www.markround.com/assets/images/intro/tnfs.png)

I have kept working on a bunch of [projects](https://www.markround.com/projects) in my spare time, and one of my favourites is my [TNFS site](https://tnfs.markround.com) - sort of an old-school BBS system or interactive “website” for the Spectrum, which grew out of a [fun experiment](https://www.markround.com/blog/2021/12/21/devops-for-the-sinclair-spectrum-part-1) to see how far I could push Sinclair BASIC. It relies on a modern network card fitted to the Spectrum, which is how I came to be chatting to “Desertkun”, another TNFS-protocol enthusiast. He’s now producing a great new [WiFi network card](https://spectranext.net) for the Speccy, and was kind enough to send me a few of the prototypes to test out for him.

I was massively inspired by his hardware and low-level coding chops, and during one of our video calls where we’d been comparing our development setups, he asked why I didn’t write any Z80 assembly myself. A throw-away comment, but the challenge was set! Now, I couldn’t face re-writing the whole site in asm, but I figured I could produce a self-contained intro to promote it in the style of the early “BBStros”, and *finally* be one of the cool kids with a scene production of my own :)

I mean, how hard could it be ? (*ooooh - foreshadowing!*)

## Dev Environment

My development environment through this was pretty straightforward, and is the same stack I use for developing my TNFS site. I just used a Makefile-driven build system which ran my code through the [Pasmo](https://pasmo.speccy.org) assembler, my fork of `zmakebas` to build BASIC files, and the [FUSE](https://fuse-emulator.sourceforge.net) emulator for testing. For final testing on the real hardware, I uploaded my `.tap` files to my local TNFS server where I could run them on my +3.

[![ASM Editor screenshot](https://www.markround.com/assets/generated/intro/asm-editor-800-85804483e.png)](https://www.markround.com/assets/images/intro/asm-editor.png)

A few additional tools I have to call out because they ended up making a big difference - one is the amazing web-based “asm-editor” I discovered while looking up Z80 instructions. Not only does this site have a great reference of all the Z80 instructions, but it also has an editor where you can run example code and see how the memory and/or various registers change state. Take for example, the [djnz](https://asm-editor.specy.app/documentation/z80/instruction/djnz) instruction - you can build it and step through the example loop construct one instruction at a time. Or try my example of an [add](https://asm-editor.specy.app/documentation/z80/instruction/add) instruction in the screenshot to see how the carry flag works when a bit gets shifted out of a register. Although it uses a slightly different syntax to Pasmo (so I couldn’t drop my code directly in it), it massively helped when manually stepping through things.

[![Best debugging agents ever.](https://www.markround.com/assets/generated/intro/dogs-800-e1a0c22d8.jpg)](https://www.markround.com/assets/images/intro/dogs.jpg)

And lastly, probably the most significant debugging tools I used on this project - a notepad and pen, and my two dogs (say “Hi!” to Biscuit & Pebble…) I’ll write some more about this in the [wrap-up](https://www.markround.com/blog/2026/zx-spectrum-intro#the-zen-of-z80), but this really was a highly unusual project for me, coming from decades of much higher-level languages and constructs. Most of my z80 code I only ever really understood after drawing out the bytes and bitmasks, keeping a table of register flags and drawing chunks of screen memory out by hand. And I found my evening walks with the two dogs provided ideal [rubber duck](https://en.wikipedia.org/wiki/Rubber_duck_debugging) exercises, although I’m not sure what they both made of my protracted mutterings about the `hl` register pair. Either way, debugging-by-dog is highly recommended!

## The Starting Point

So, let’s get started. I followed a great “Hello, world!” example from [futurice.com](https://www.futurice.com/blog/build-environment-like-its-1986) to validate my build environment. Beyond that, the first thing I wanted to do was load in my logo and background image I’d use as a “stage” for the remainder of the planned intro. I wasn’t really sure what I was going to do at this point, just that I wanted my logo at the top, and a scrolltext along the bottom row of the screen. As my TNFS site header already looked kinda demoscene-y, I made a quick edit putting in place two blue guide-bars inside which I’d put further content.

[![TNFS site screenshot](https://www.markround.com/assets/generated/intro/memory_map-452-cba2ad4fc.png)](https://www.markround.com/assets/images/intro/memory_map.png)

A Spectrum `SCREEN$` file (usually displayed as loading screens) is just a dump of screen memory, starting from byte 16384, and running for 6912 bytes. The weird layout of this is very relevant later, but in order to display it all you need to do is load your file into that memory location. After a few hour’s work of reviewing Speccy internals and following various guides, I had a very simple starting point for my demo that served as the basis for everything else that came after.

```z80
org $8000

screen  EQU $4000 ; address of the first byte of screen memory, 16384 in dec
scrlen  EQU 6912  ; total length of the screen memory inc. attributes
ula     EQU $FE   ; I/O port for the ULA to control the border
black   EQU 0     ; Spectrum colour number, the exact same ones BASIC uses

start:
  ld a,black      ; Set the border colour to black using the ULA
  out (ula),a

  ; Copy my logo header picture to screen memory
  ld hl,header    ; HL = location of my screen file in this code
  ld de,screen    ; DE = start of screen memory
  ld bc,scrlen    ; BC = 6912 bytes to copy
  ldir            ; block copy, see https://jnz.dk/z80/ldir.html

; busy loop - rest of intro goes here :P                    
hold:
  halt            ; sleep until the next interrupt (1/50 second)
  jr hold         ; then jump back and sleep again, for ever

header:
  incbin "assets/header.scr"  ; my logo header/background

end start
```

[![Not much to look at now, but this simple starting point introduced me to important concepts like interfacing with the Spectrum’s hardware ( ULA chip to start with, and AY sound chip later) via the out instruction, how to pull in binary files using Pasmo’s INCBIN directive, using the Z80’s block copy ldir instruction, the use of register pairs for 16-bit addressing, and simple branching to labels.](https://www.markround.com/assets/generated/intro/screen-800-6ec15b2c9.png)](https://www.markround.com/assets/images/intro/screen.png)

Not much to look at now, but this simple starting point introduced me to important concepts like interfacing with the Spectrum’s hardware ([ULA](https://sinclair.wiki.zxnet.co.uk/wiki/ZX_Spectrum_ULA) chip to start with, and AY sound chip later) via the `out` instruction, how to pull in binary files using Pasmo’s [INCBIN](https://pasmo.speccy.org/pasmodoc.html#dirincbin) directive, using the Z80’s block copy `ldir` instruction, the use of [register pairs](https://jnz.dk/z80/registers.html) for 16-bit addressing, and simple branching to labels.

### Hello World

[![](https://www.markround.com/assets/generated/intro/hello-800-d0d7c965a.png)](https://www.markround.com/assets/images/intro/hello.png)

The next step was to print something on the screen and explore some of the Speccy’s built-in ROM routines, which are somewhat analagous to syscalls on modern operating systems. There are plenty of disassemblies and guides to the Spectrum ROM available on the internet, one of the best commented and linked one is at [https://skoolkid.github.io/rom/](https://skoolkid.github.io/rom/). Using this as a reference alongside the various “Hello, world!” tutorials showed me how this sort of thing is built up:

```z80
  at      EQU 22 ; "at" control character used to position the cursor
  row     EQU 11 ; coordinates where to print the message
  col     EQU 9

  prstring EQU $203C ; where the pr_string routine lives in ROM

  ld de,msg        ; DE = address of the first character
  ld bc,msglen     ; BC = how many characters to print
  call prstring    ; print the lot

; busy loop is the same...

msg:
  db at,row,col            ; equivalent of "PRINT AT 11,9" in BASIC
  db "Hello, world!"
msglen  EQU $-msg          ; 16 bytes in all, pasmo shortcut, $=program counter
```

I ended up mostly not using the ROM routines in my code as they dragged in all sorts of things I didn’t need, but it was massively useful to understand *how* they worked as they introduced me to yet more commonly-used constructs. For example, the `PR_STRING` routine defined [here](https://skoolkid.github.io/rom/asm/1FFC.html) shows how you can build a loop over each character. If you’re just starting out, I highly recommend going over the 8 lines in that routine with a notepad and pen, working out the contents of the registers and following the loop constructs.

[![I also made a version of it in asm-editor you can try. If you click that link, you’ll see a version of the code loaded into the web editor. Click “Build”, and then step through the code using the “Step” button - although no text will be actually displayed (I skipped over the call to the Spectrum ROM “restart” vector that does this), you’ll see the various states of the registers and for example the BC register counting down as each byte of the string is processed.](https://www.markround.com/assets/generated/intro/asm-prtstring-800-50a5d8c2a.png)](https://www.markround.com/assets/images/intro/asm-prtstring.png)

I also made a version of it in [asm-editor](https://asm-editor.specy.app/projects/share?project=N4IgZglgNgpgziAXKAtgQwgOwHQC8AcADEqDJgMYD2AJlgOZIgAOUGmIANCFZgC5m9GAAhGiR2SgCc6QwgA8ihQgB1MqgNxCACgCUAkgDkAKkICCJ8gAs0ktOX6S4QtJmpCqU2pjT84qn2LiMACOAK5CAEwRqpKUAO6B2CHhAIwpqlRQiclCAJyqKHB0sJhBYUIpAGxCmiV0vJZClGBCDTBCKPBwaHTtWORQoV4yPLyxWVY2dg5+aphwvDa8iKqBolBu1DAcHUVimgAiAKJCALzO1NSSXU0tbUKQjrzu1rb2MJKra0IbQgBG5A4hWKZBEmgAQgBhM5CSzxDouACeLym70crUoQiYkiwvC+a3IaCgWWxCxxmBkmmxuNalnaUEoeNK32sUCZqlJY3oiFE+MCvzQHD+Qj5Yik7lFoi25H+5ElImuz1w8p+bkFAAotgBKFX9IRbFWaOnXWntaahIlY8nPJ5COKUUK-P7tQnEmDUFUAK0kVrJ9BFcwKRRWzOw1GFPg4sTiHEy30CmnMFRSHFyAfE4ZFIAAEjBiZQdvbJBsAITKECqEAAXyrXAEkkRjHQWDwRE4IDgMF4vHoCGQte41x87tMgkQKQA7LlJxEAKyEKIAZlyi64oSY1GH1FHSEn04nc8Ii4iE4PXG8nUYugA+gBlIz6AwAcSEMDkaBQLHplEoTHbrApC1ekYAAtNsuC2OByBxJge0odhEBAUwfh-JgMX1GAUHgslh1heF7hve9HxfW8mBgexJFCFAhFiUIe0wdoiwAawQLhfF4QlOz7ABtABdLgIGoRhr2vOBXhgETqyAA) you can try. If you click that link, you’ll see a version of the code loaded into the web editor. Click “Build”, and then step through the code using the “Step” button - although no text will be actually displayed (I skipped over the call to the Spectrum ROM “restart” vector that does this), you’ll see the various states of the registers and for example the BC register counting down as each byte of the string is processed.

### Adding Some Style

Problem is, the Speccy’s built-in font is not exactly dripping with demoscene credibility. I wanted to use some stylish fonts and this turned out to be super easy - a quick search turned up Jim Blimey’s awesome [font collection](https://www.jimblimey.com/blog/24-zx-spectrum-fonts/), which included a ready-to-use set of assembler files and some instructions which soon led me to these additions in my code:

```z80
; CHARS is the address the ROM's printing routine reads to find the font. 
; Picking our own font is simply a matter of over-writing this
chars   EQU $5C36 ; from Jim's docs - POKE 23607=5C36 in hex

  ld hl,myfont-256 ; HL = font address - 256 as per docs
  ld (chars),hl    ; from now on the ROM draws with it!

; rest of code is the same

myfont:
  include "textfont.asm" ; "Cartoon" font from jimblimey.com
```

[![](https://www.markround.com/assets/generated/intro/font-800-13482085f.png)](https://www.markround.com/assets/images/intro/font.png)

There we go! Now that’s looking a little more elite. Sorry, I mean `31337`. Strap in folks, we’re leaving lamer city… So at this point, a month or so in to my dabblings and I’d started to get a reasonable understanding of common idioms and was starting to feel like this was a project that might actually go somewhere. The problem started when I moved on to trying to implement actual demo effects: It became very apparent, very quickly, just how much I *didn’t* know about how this all worked. Not to mention the fact that at this level, you’re really closely exposed to the physical hardware intricacies, including the design decisions and trade-offs that the hardware designers had to make when working with the extremely limited specs of the early 1980s.

## Cursed Screen Layout

[![The spectrum's screen memory layout is cursed.](https://www.markround.com/assets/generated/intro/normal-480-1de2f92b1.jpg)](https://www.markround.com/assets/images/intro/normal.jpg)

The first such problem - and the cause of probably 90% of my pain - is the Speccy’s extremely *weird* screen layout. Rather than standard bitmapped display you might expect on more modern hardware, it is instead split into 3 sections, each with a set of interleaved rows (so if you keep adding 1 to a screen address, eventually instead of reaching the next row, you’ll instead end up 8 rows down, and so on until you hit the bottom of the section and then loop back to the row underneath where you started!) followed by a block of “attribute” bytes which determine the colours of each 8x8 block. The attribute blocks are the reason why each 8x8 character cell can only have two colours (a `PAPER` and `INK` attribute) which led to the garish “colour-clash” of spectrum games and distinctive style where artists try to keep inside 8x8 blocks as much as possible. But it was the layout of the bitmap portion that is such a mind twister.

You can see this illustrated whenever a game reads in a loading screen from tape. You can see each section built up with the interleaved rows one at a time, and then finally the colour and other attributes (like flashing ). There’s a lot of detail at the fantastic [Break Into Program](http://www.breakintoprogram.co.uk/hardware/computers/zx-spectrum/screen-memory-layout) site. In the defense of the designers, they were working with extremely limited hardware and I don’t think the Speccy was ever imagined as a games machine. When you start shuffling bytes around, you realise that this approach is really optimised for displaying text which is neatly arranged into 8x8 character boundaries. For example, using this “scrambled” bit order, if you have a screen address in the `hl` register pair, you can quickly get to the next pixel line of a given address by simply incrementing the “high” byte. There’s a bunch of other “cheap moves” you can make by simply shifting bits around:

| Move                 | Instruction   |
| -------------------- | ------------- |
| Right 8 pixels       | `inc l`       |
| Left 8 pixels        | `dec l`       |
| Down 1 pixel line    | `inc h`       |
| Up 1 pixel line      | `dec h`       |
| Down 1 character row | add 32 to `l` |

It was a reasonable choice for a machine that would probably spend a lot of it’s time displaying BASIC program listings - it’s only when you start trying to write demo or sprite routines that it starts to feel really weird. This also means you’ll pretty quickly need a routine to convert X,Y addresses to the relevant memory addresses. Again, L Break Into Program has a useful routine and if you disassemble more or less any spectrum game or demo, you’ll find a version of this procedure somewhere. And yes, this all became horribly relevant later on…

[Download the video](https://www.markround.com/assets/images/intro/dot.mp4)

Admittedly, not the most interesting demonstration - but this was the first time I had made something move on the Spectrum’s screen with assembly! I watched this with pride more times than I care to admit :)

I had a snazzy logo and “Hello World” text, but that doesn’t make for a very interesting intro! The first effect I wanted was the scrolltext - without that, it’s not really an intro. Now, the Amiga had all kinds of hardware sprites, blitter operations, custom chips but the Spectrum had none of that. I was going to have to work out how to do things the hard way - “banging on the metal”, literally by shifting bits around the screen memory by hand and having the CPU do all the work.

This is where understanding the Speccy’s weird screen and character cell layout is so important. It turns out there’s a neat trick as long as I stay inside one character row - there’s a [RL](https://jnz.dk/z80/rl_r.html) instruction, which takes a byte and “rotates” it left. It inserts whatever is in the carry flag into the rightmost bit, and whatever gets shifted out ends up in the carry bit again. That means if you string a bunch of them together, you can move a set of pixels across the screen - the moment they “fall off” one character cell, they end up in the carry flag and then the next `rl` instruction inserts them at the beginning of the next character cell to the left. Repeat 32 times, and you’ve just moved a pixel (or set of pixels) across a line of the screen:

```z80
linestart EQU $4880         ; leftmost byte of the line 12 rows down
lineend   EQU linestart+31  ; rightmost byte column ($489F) 
linelen   EQU 32            ; one whole line is 32 bytes (character cells) across
dot       EQU %00000001     ; the dot "shape", just one pixel lit at the far right end

newdot:
  ld a,dot                 ; load the dot shape (only rightmost pixel lit) into a
  ld (lineend),a           ; store it in the rightmost byte of the line, pixel is now lit

frame:
  halt                     ; wait for the next frame, otherwise it whizzes across too fast to see
  ld hl,lineend            ; hl = last (rightmost) byte of the line
  ld b,linelen             ; loop counter, 32 bytes to work through
  or a                     ; trick to clear the carry flag, or against itself leaves a register 
                           ; unchanged but clears various bits including the carry flag
                           ; see https://jnz.dk/z80/or_r.html
shift:
  rl (hl)                  ; rotate this byte left (00000001 -> 00000010)
  dec hl                   ; point hl at the next byte to the left
  djnz shift               ; loop construct: b=b-1 and then jump back to shift label until b=0
  jr c,newdot              ; now we've shifted the whole line, so if the carry flag is set now it means
                           ; the dot has just left the screen, so we jump back to the start
  jr frame                 ; otherwise it is still somewhere on the screen, so keep shifting!
```

Wrapping this in another loop that increments `h` each time (remember - that gets us moving down a scanline) gets us moving an entire character cell. Here you can see it moving an entire row of my “Hello world” message. There was however a *lot* more work needed to make a proper scrolltext, with each letter appearing smoothly. I started off by just printing an entire character at a time on the right-hand edge of the screen - it worked, but was pretty ugly as you saw each new character appear at once in the right-hand side of the screen, scrolling 8 pixels over and then the next character flashed up. Instead, I wanted to make each character appear pixel by pixel and that required a bit more bit juggling. The routine I ended up with effectively streams each character in, line by line, pixel by pixel using the font data directly instead of calling the ROM print character routine.

It does this by looking up the ASCII code for a character, and then fetching the corresponding font representation into a 8-byte buffer, one byte for each line of data. The font data starts with the space character (character code 32), so a lookup is simply `font code base address + (ascii code - 32) * 8` to get to the bits for that letter.

[![Font Letter H](https://www.markround.com/assets/generated/intro/h-684-04a1c5537.png)](https://www.markround.com/assets/images/intro/h.png)

Arranged into an 8x8 grid, the bits for the letter H in the font data look like this. For each row of bits, I shift the line left using a neat trick. The byte is loaded into the `a` register, and then by adding itself to itself - `add a,a` we shift the value left (in binary, multiplying by 2 moves everything over one bit) so that the pixel I want to draw ends up dropping into the carry flag. This is an idiomatic way of doing it in Z80 code as it’s faster and smaller than the `sla` instruction. And every cycle quickly adds up when you’re re-drawing 256 bytes every frame!

[![Font Letter H](https://www.markround.com/assets/generated/intro/h-shift-684-724e9877d.png)](https://www.markround.com/assets/images/intro/h-shift.png)

The character buffer then looks like this, with the top line shifted. Now, with the `hl` register pair pointing at the correct location on the screen, I can then use the `rl` trick discussed above. This shifts a scanline from the bottom character row of the screen left by one pixel and draws my pixel in the rightmost position by [moving it in](https://jnz.dk/z80/rl_r.html) from the carry flag. Looping 32 times over this moves the entire row by 1 pixel with my new pixel inserted at the right.

[![Font Letter H](https://www.markround.com/assets/generated/intro/h-col-shift-684-2a97c8926.png)](https://www.markround.com/assets/images/intro/h-col-shift.png)

I then repeat this step again using `inc h` to move down one scan line until the entire character column has been drawn. At this point, the character buffer looks like this, and all 8 lines of the bottom screen row have all been shifted left by one bit by looping over all 32 character columns. The whole thing then repeats another 7 times until the entire character has been smoothly scrolled onto the screen.

So just to re-state this because I think it’s worth the emphasis - just to scroll a message across the bottom of the screen, we’re having to loop over and re-draw 256 pixels every frame! It shows how much faster assembly is than the BASIC interpreter built into the spectrum’s ROM. I found it amazing to see my code finally working and imagining all the bits flying around and registers flapping away. It really makes me appreciate just how much effort went into creating the games I played back then.

## Music

[Sounds of Gnome demo, playing the Milkman track](https://www.youtube.com/watch?v=JdRB_mUKeBA)

The original Amiga module I converted. A proper 1980s ear-worm!

Of course, any good intro needs some music. On the Amiga, the standard was the “tracker” module format, which has a long and [involved history](https://youtu.be/roBkg-iPrbw) of different forks, versions and tweaks. A song is built up from a series of patterns, each made up of rows of instructions. Each instruction usually includes note or pitch information, which sample to play and various parameters such as volume, effects - or in the case of some instructions, which position in the song to jump to. In many ways, it’s almost like a programming language for music. Using [ProTracker](https://en.wikipedia.org/wiki/Protracker) as an example, a typical note cell is 4 bytes, shown on-screen as something like `C-2 01 C20`. That breaks down into three fields: the note and octave (`C-2`), the sample number (`01`), and the effect and its parameter (`C20` - in this case, “set the volume to `$20`”, which is half of the maximum `$40`).

[![Closeup of Protracker notes](https://www.markround.com/assets/generated/intro/tracker-800-3e81550c3.png)](https://www.markround.com/assets/images/intro/tracker.png)

A close-up of Protracker playing a tune. Note the VUmeters - another classic demo effect I would pinch later on!

For an impressive demonstration of what’s possible with a tracker and to see what the note data looks like on-screen, check out the work of h0ffman, and in particular his winning “[Professional Tracker](https://www.youtube.com/watch?v=wkuj7r9zJ_k)” entry from the [Revision 2014 competition](https://youtu.be/WAJZLRXCtz4?t=2044). He’s an undisputed master of the science (or art!) of tracking, and his “[Secrets of Protracker](https://youtu.be/IdbgeFAus0k)” talk gives a great overview of what’s possible.

The Amiga’s sound hardware far eclipsed anything else on the market at the time, and the ZX Spectrum is paltry by comparison. The original 48K models only offered a rudimentary “beeper”, but the later 128K models included a 3-channel AY-3-8912 sound chip. This was very popular in 8-bit micros of the time, and a more-or-less identical variant was also used in the Atari ST. While it only offers basic waveforms and sound processing, it does enable multi-channel “chip music”, and so it became the mainstay of the ZX Spectrum demoscene. It also spawned a variety of SoundTracker clones on the Spectrum itself, although most musicians these days prefer to work in modern tools like Vortex Tracker II, which runs natively under Windows.

## The Conversion

[Pex's retrospective talk on his demos](https://www.youtube.com/watch?v=AGN9Hv2xQAc)

Pex looking back on his demos - this starts at the point where he features my letter and plays a bit of my cover version!

For my intro, I wanted to port an Amiga module - and I had the perfect one in mind. One of my favourite Amiga music disks was “Sounds Of Gnome”, released in 1989 by the Swedish duo “Mahoney & Kaktus”. Mahoney also wrote NoiseTracker, one of the first really widely used trackers on the Amiga, and the program that served as my teenage introduction to making computer music of my own. Back in 2018 I made a [rock cover](https://www.markround.com/blog/2018/07/24/flashback/) of one of the other tunes on that disk, and wrote to Mahoney (real name “[Pex](https://livet.se/mahoney/)”) about it. He ended up showing my “music video” at a demo conference, which I am eternally grateful for! So it seemed only fitting that I mangle another song from that collection with a Spectrum conversion… Sorry, guys! I picked the track “Milkman” which was written by “Glue Master”, an associate of Mahoney’s who had a few of his tracks featured on the compilation.

It’s an ideal track for converting: It’s short (only 7 patterns in total), most of those patterns share the same chord sequence, and it looked fairly easy to squeeze the original 4 channels down into the Spectrum’s 3. Plus, it’s an incredibly catchy tune and I knew it well - I’ve been listening to it for 35+ years and often jam along to it on guitar or bass. The conversion started off pretty easily: all I really had to do was copy the patterns note-for-note from the Amiga mod, which I had open in [OpenMPT](https://openmpt.org) as my Windows tracker. Below is a session with both trackers open on the same pattern - the original Amiga mod on the left, and my 3-channel Spectrum conversion on the right.

[![Note the 11.7 in the 3rd channel of the Spectrum version. This is a “pitch slide down” effect in Vortex Tracker syntax, which drops the pitch by about a quarter-tone. It’s repeated on the next line, so the overall effect is a slide down from the A# that leads in to the next bass note. The original Amiga mod does the same thing using its own syntax - those are the 202 s you can see on the same notes, at the bottom of the 4th track.](https://www.markround.com/assets/generated/intro/trackers-800-5c22a89b8.png)](https://www.markround.com/assets/images/intro/trackers.png)

Note the `11.7` in the 3rd channel of the Spectrum version. This is a “pitch slide down” effect in Vortex Tracker syntax, which drops the pitch by about a quarter-tone. It’s repeated on the next line, so the overall effect is a slide down from the `A#` that leads in to the next bass note. The original Amiga mod does the same thing using its [own syntax](https://youtu.be/IQr8X1Yv0ao) - those are the `202`s you can see on the same notes, at the bottom of the 4th track.

Using just a single “beep” sound for everything was enough to check everything was working, and after a few weeks of hacking on it and re-arranging things in the evenings, I had a basic version which sounded surprisingly good for a first draft:

I have to give a special mention to [this](https://chipmusic.org/forums/topic/27/vortex-tracker-ii/) forum post which was a massive help getting me started. It’s probably the best introduction to Vortex Tracker I’ve found - many, many thanks to the original author for taking the time to write it all up!

[![Vortex Tracker Sample](https://www.markround.com/assets/generated/intro/sample-800-0460ecdd9.png)](https://www.markround.com/assets/images/intro/sample.png)

The real challenge was making it sound as close to the original as possible. That meant building up a library of better-sounding samples for the different parts, and making extensive use of Vortex Tracker’s “ornaments”. Vortex Tracker comes with a great selection of sounds, and fortunately I’m also a weirdo who likes to listen to [chiptunes](https://zxart.ee/music) in the background while I work! So whenever I found something particularly enticing, I filed it away for later pilfering inspiration. A Vortex Tracker sample isn’t really a “sample” in the Amiga sense of a recorded sound - it’s a definition of how a note sounds, one line per frame. Each line can mix in a tone or the white noise generator (useful for snare effects and suchlike), along with various other parameters.

Ornaments, on the other hand, give you extra control over what pitch a note plays - and this enables *really* useful tricks like “arpeggio chords”. For example, an ornament of `0,4` makes the channel switch rapidly (25 times a second) between the note being played and the one 4 semitones above it. It happens so quickly that it sounds much like two notes played at the same time, which means you can build up full-sounding triad chords using only two channels. Take the opening chord in my fully converted version, Cm: It plays C in the first channel and the minor 3rd (D#) in the second, then I apply an ornament to the second channel to bring it up 4 semitones to the 5th (G). That gives all three notes of the chord, while keeping the 3rd channel free for other parts.

As well as making much more efficient use of the limited 3 channels, this also gives Spectrum music that characteristic “warbling” sound, which I find really appealing in a retro kind of way! Between tricks like this and samples that mix tones with white noise (to make a combined “bass-snare” sound, for example), I managed to condense the full track down to 3 channels. Many months later, I have to say I’m really pleased with the [final result](https://youtu.be/ALbjMMgmAXo).

## Playback

The original site that was home to Vortex Tracker, documentation and other resources appears to be offline at the moment. I managed to grab a copy of everything I needed from [archive.org](https://web.archive.org/web/20220612040410/http://bulba.untergrund.net/progr_e.htm) - you’ll want the `PTxTools.7z` file for the playback routines discussed here.

Update - a user on Hacker News added this useful bit of information: “Sergey Bulba’s WWW site is still around and the relevant stuff can be found today at [https://ay.strangled.net/progr_e.htm](https://ay.strangled.net/progr_e.htm)”. Thanks, JdeBP!

The playback was simple in theory: As [this overview](https://github.com/alfishe/speccy-bootcamp/blob/main/06_sound/trackers_and_formats/vortex_tracker.md#player-integration--embedding-pt3-in-a-game-or-demo) notes, you basically just pull in the player routine, include your module file and call the playback routine once per frame in your main loop. The catch is, none of the source code versions supplied in the archive worked with my Pasmo assembler. Fortunately, there is a pre-compiled binary object shipped alongside the source (`VTII10bG` in the `PTxTools/PT3Play` directory). This just has to be loaded in at address `$C000` - this cannot be changed, as it has lots of absolute addresses baked in.

My code to handle module playback now looks something like this, with the binary module loaded in place:

```z80
; Define the start address of the player routine and functions we need
player EQU $C000      ; hard-coded org address of binary player
plinit EQU player+3   ; init function in docs, module address in hl
plplay EQU player+5   ; play and move on one frame

; from the docs - this "analyzes PT3 module header and prepares 
; corresponding note and volume tables"
ld hl,moduleimg
call plinit

; main loop
hold:
  halt
  call plplay
  call scroll
  ; other effects / routines go here...
  jr hold

moduleimg:
  incbin "assets/music.pt3"
```

## Starfield

This brings me to the main event. No self-respecting cracktro is complete without a starfield!

My first, naive approach to the classic starfield effect was to scatter random stars across the screen, then move down each scan-line applying the same `rl` trick I’d used for the scroll-text. I figured I could get parallax by shifting each row a different amount: One pixel for the first row, two for the second, three for the third, then back to one again, all the way down the screen to give the impression of several “fields” of stars moving at different speeds.

It turned out there were two major problems with that approach. Firstly, shifting an entire row smeared whatever else was on it along with the stars - which I’d expected, and had filed away as a problem for future me. The bigger issue was speed: by the time I’d finished the first three rows I’d already performed hundreds of shift operations, and it was nowhere near fast enough to fit inside a single frame. Everything crawled across the screen, and the music playback slowed down so much that you could actually hear the arpeggio ornaments warbling away as separate notes.

Clearly I was going to have to keep track of the individual stars instead. I found [this routine](https://github.com/falsovsky/z80/blob/master/starfield/main.asm), which was more-or-less what I had in mind and started me off with the idea of a structure to hold the state of each star. It also uses a [screen address lookup](https://github.com/falsovsky/z80/blob/master/starfield/main.asm#L353-L382) much like the one from “Break Into Program”.

That got things moving, but the co-ordinate lookup and tracking proved expensive to do (it has to run for every star, every frame as well as keep track of the old position for erasing) and I’d been ignoring the bigger question how to display stuff on top of the starfield. My original idea was simply to draw the stars and then paint text over them each frame, which blew the frame budget just as comprehensively as my first attempt.

If I wanted something more interesting than an empty starfield, I’d have to keep track of which cells were occupied by text and avoid drawing stars in them in the first place, rather than over-painting them every time. After lots of scribbling in my notepad and visualising rows of bits, I eventually solved both challenges by “cheating” fairly liberally.

So now, I don’t generate a random starfield at all. Instead, I built a table of pre-calculated Y and X position pairs:

```z80
stdata:
  defb 109, 25,  88,182,  42, 96, 155,226
  defb  59,254, 137,169, 160, 84, 125,193
  ; ... and so on, for 32 stars ...
```

This saves faffing about with an RNG, and once I have a layout that looks good I know it’ll always look good. I can also “fudge” things and ensure no two stars share the same y co-ordinate which sidesteps a whole class of problems like stars sharing a scanline, and having to deal with them overtaking each other.

The “random” layout is generated by a simple Ruby script:

```ruby
#!/usr/bin/env ruby

PAIRS    = 32
PER_LINE = 4          # pairs per line
Y_RANGE  = 40..175    # exclude rows taken up by my logo & bars
X_RANGE  = 0..255

raise "y range too small" if Y_RANGE.size < PAIRS

# no duplicate y values
ys    = Y_RANGE.to_a.sample(PAIRS)
pairs = ys.map { |y| [y, rand(X_RANGE)] }

pairs.each_slice(PER_LINE) do |row|
  body = row.map { |y, x| format('%3d,%3d,', y, x) }.join(' ')
  puts "defb #{body.chomp(',')}"
end
```

When the routine initialises, I convert those co-ordinate pairs into a table that tracks the state of each star. So rather than storing a position and translating it to a screen address every frame, each of my star records now looks like this (offsets from the base address):

```z80
STSCR   EQU 0  ; screen address of the byte the star is in, e.g. $4123 
STOCC   EQU 2  ; a pointer to the "occupancy check" table cell
STMASK  EQU 4  ; the star's bit pattern, e.g. %00010000 
STSTEP  EQU 5  ; "speed" - num. of pixels this star travels per frame
```

The key thing is that there are no X/Y co-ordinates at all. For each star I store the screen byte it sits in and the bit mask within that byte, so it’s already in the form the screen writes need. That means no address conversion per frame, just simple bit shifting and rotation.

Moving a star is then dead simple. I rotate its mask left with [rlca](https://jnz.dk/z80/rlca.html), and when the bit rotates out of bit 7, it lands back in bit 0 with the carry flag set. That’s the signal I need to move the star one character cell to the left, which is one of the “cheap moves” from earlier.

Because I’m tracking which cell each star is in, the collision check comes almost for free. That’s what the second pointer is for: `STOCC` (Star Occupancy) moves in step with `STSCR` through a table holding one byte per character cell. The “typewriter” routine sets a byte to 1 for every cell containing a text character or the cursor, so checking whether a star is hidden behind text is a single `or` instruction. If the cell is occupied, I skip drawing the star but carry on rotating and stepping it along until it reaches a cell where it can be displayed:

```z80
ld l,(ix+STOCC)
ld h,(ix+STOCC+1)
ld a,(hl)                ; A = the occupancy check for this cell
or a                     ; test A against zero
jr nz,stmove             ; it's occupied, so leave the screen alone
```

Parallax is still handled by moving each star a different number of pixels per frame, but the cost is a **lot** lower this time as I’m only touching the cells the stars actually occupy, rather than every byte of every row.

[Download the video](https://www.markround.com/assets/images/intro/typewriter.mp4)

That’s looking more like it! The typewriter routine is a simple copy of the font data from memory, and is very cheap as we can use the “cheap moves” to quickly fetch the 8x8 block.

I also skip drawing any space character so it leaves the “occupancy cell” unmarked. This means the stars keep drifting through the gaps in the text - you can see here the odd star passing in between the words. I switched to `ALL CAPS` writing for the final version which helps the effect even more as the font is quite blocky, so you don’t really notice stars skipping the spaces inside a glyph at all.

## Misc Notes

A few odds and ends that didn’t warrant a section of their own…

## VU Meters

Some sort of graphic display bouncing along with the audio playback was another common early demo trope. I did originally consider a real-time graphic equaliser, but that would have been far too complex, probably in terms of spare cycles per frame, and *definitely* in terms of my limited mental capacity at this point! So I settled for a simple set of bouncing meter bars instead.

It’s worth a mention mostly because it gives me an excuse to link to the awesome [chibiakumas](https://www.chibiakumas.com) website! I just love the late 90s web design on display - one can argue that readability and accessibility take a hit, but it’s just so much *fun* to encounter a site that still looks like this. It’s a clear labour of love and there’s a wealth of content there as well, if your eyeballs are up to it.

Their guide on [Making Sound with the AY-3-8910](https://www.chibiakumas.com/z80/platform2.php#LessonP18) was what showed me how to get the data I needed to draw the meter bars. The examples write a register number to `$FFFD` to select a register, and I realised that once a register is selected, I could use the `in` instruction to read its value back out instead of writing a new one. As the page says, R8, R9 and R10 hold the channel amplitudes, so they feed nicely into the height parameter for drawing a bar. Getting the amplitude for channel A is then as simple as:

```z80
  ld a,8        ; Channel A register
  call ayread

; ... rest of code here ...

ayread:
  ld bc,$FFFD   ; Register select port
  out (c),a
  in a,(c)
  ret
```

## Twinkle, Twinkle, Little Star

This is a simple effect that makes the header logo a little more interesting. I’m not sure where I originally got the idea - I’ve definitely seen something like it on a few late 80s/early 90s demos, though. All I wanted was to pick random star dots in the logo background and animate them.

[![Header graphics](https://www.markround.com/assets/generated/intro/twinkle-800-33097ce0d.png)](https://www.markround.com/assets/images/intro/twinkle.png)

Here’s the header image loaded in ZXPaintbrush with the character cell grid showing. By happy chance, there are four stars which each sit neatly inside a single character cell with enough surrounding space, so they can be used to give an animated “twinkle” effect by drawing a cross out from the centre point and back in again.

These dots map to the following co-ordinates, which target the top scanline of each fully-grown star (two pixels up from the centre):

```z80
twkdata:
  defb 1,5    ; char row 0, col 5,  centre y=3  ink 5
  defb 1,20   ; char row 0, col 20, centre y=3  ink 1 bright
  defb 26,7   ; char row 3, col 7,  centre y=4  ink 4
  defb 25,22  ; char row 3, col 22, centre y=3  ink 6 bright
```

The “twinkle” routine itself is simple enough: it adds a pixel on either axis to grow the star, waits a few frames, then shrinks it back down again. The interesting bit was how I made the twinkle order pseudo-random. Remember that my starfield was entirely pre-calculated from a data table generated by a Ruby script, which meant I’d never actually needed a random number generator in assembly. From Jonathan Cauldwell’s “[How To Write Spectrum Games](https://jonathan-cauldwell.itch.io/how-to-write-spectrum-games)” e-book, I found another great “hack” which simply walks through the start of the Spectrum’s ROM, picking out random-looking bytes as it goes. As he says, he uses it in most of his games, and it does the job in a really simple way - as long as you stay below the block of structured, repeating data that sits 8Kb into the ROM.

```z80
twkrand:
  push hl
  ld hl,(twkseed)
  ld a,h
  and 31                   ; keep it within the first 8k of ROM
  ld h,a
  ld a,(hl)                ; the "random" byte
  inc hl
  ld (twkseed),hl
  pop hl
  ret
```

## Out Of Space

About halfway through writing the intro, I hit a fairly predictable problem: I had run out of memory. In an attempt to keep everything self-contained in one block of code, I was pulling all of the assets (the screen background, the player binary, the music file etc.) into the program itself, and then using the `ldir` block copy instruction to move each one into its correct location. Since that meant effectively storing every block twice, and the player routine had a hard-coded start address of `$C000`, I quickly ran out of room and my code started crashing into the playback routine. The simplest option was to split things out into separate code blocks, load each one directly to the address it needed to live at, and wrap the lot in a multi-stage BASIC tape loader:

```zxbasic
10 CLEAR 32767
20 BORDER 0: PAPER 0: INK 0: CLS
30 LOAD "" CODE
40 LOAD "" CODE
50 LOAD "" CODE
60 RANDOMIZE USR 32768
```

This loads the main program, then the player routine directly into `$C000`, and finally the background screen straight into screen memory. It does mean the background starts appearing before the demo has fully loaded, but as I imagine most people will be seeing this running in an emulator or on my TNFS site, it doesn’t really matter. Plus, at this point I was in danger of disappearing down yet more rabbit holes (I still *really* want to work out how to do multi-colour border effects!) and I’d never have got this thing released.

## The Zen Of Z80

[![My Coding Notebook](https://www.markround.com/assets/generated/intro/notebook-800-6e765cfcb.jpg)](https://www.markround.com/assets/images/intro/notebook.jpg)

This whole project is a really weird one to look back at. On the one hand, I’ve fulfilled a childhood ambition and have something I’m very proud of. It’s also pretty unusual these days to end up with a binary where you can say pretty much what every single byte is doing and the story of how it got there. But on the other hand - I now have an answer for why I don’t code in assembly: It’s because I value (what’s left) of my sanity! The thing is, in order to write code like this you have to start thinking like the machine itself. You have to visualise the registers and the status flags flapping away, graphics as lines of bits slotted into strange orders, loops and conditionals as binary logic and so on. Getting this close to the hardware definitely brought about a greater sense of attachment to the hardware of my childhood, and it’s easy to see why assembly coders and sceners get so passionate about it. That said, I’m in no hurry to start re-writing my body of work in assembly!

The projects I really enjoy are those that give me an excuse to learn something for the sheer hell of it, and to slow down a little from the ever-increasing velocity and demands of the modern work world. Going back to Z80 code is probably about the furthest I could have taken this, short of going into hardware hacking. I found it at times an intriguing puzzle, often a frustrating exercise that nearly had me throwing in the towel at several stages. This project has certainly taken the longest to go from the “idea” stage of any given feature to any kind of meaningful output.

True, I didn’t spend every day of the last year working on it - my actual coding time was probably only a few hours over a couple of evenings a week. But I’ve certainly been thinking about it and pondering it most days (as my dogs will attest). The biggest breakthroughs and “aha!” moments usually didn’t come from behind the keyboard; they came from scribbles in my notebook and thinking about processor states while doing something unrelated.

It has however given me massive appreciation and respect for the coders who pull this stuff off and make it look easy. And that goes double for those kids and bedroom coding superstars who managed all of this back in the 80s, and had to work it all out by experiment themselves. I only managed to get this far because of numerous books, online tutorials, GitHub repositories with sample code, YouTube videos… I can’t imagine how much longer this would have all taken me if the only resources I had were my Spectrum, a cassette tape recorder and a notepad.

So, as I draw a line under this project - for now! - I raise a glass, and in the words of h0ffman: “Shout out to the hackers, graphicians and protrackers.”

[![MDR October 2026](https://www.markround.com/assets/generated/intro/out-800-de99f2650.png)](https://www.markround.com/assets/images/intro/out.png)

*The opinions and views expressed on this website are my own and do not necessarily reflect the views of my employer, past or present.*
