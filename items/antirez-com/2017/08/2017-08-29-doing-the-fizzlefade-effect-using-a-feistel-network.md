---
title: Doing the FizzleFade effect using a Feistel network
link: http://antirez.com/news/113
source: antirez-com
published: 2017-08-29T14:35:14Z
updated: 2017-08-29T14:35:14Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'Today I read an interesting article about how the Wolfenstein 3D game implemented a fade effect using a Linear Feedback Shift Register. Every pixel of the screen is set red in a pseudo random way, till all the screen turns red (or other colors depending on the event happening in the game). The blog post describing the implementation is here and is a nice read: http://fabiensanglard.net/fizzlefade/index.php You may wonder why the original code used a LFSR or why I''m proposing a different approach, instead of the vanilla setPixel(rand(),rand()): doing this with a pseudo random generator, as noted in the blog post, is slow, but is also visually very unpleasant, since the more red pixels you have on the screen already, the less likely is that you hit a new yet-not-red pixel, so the final pixels take forever to turn red (I *bet* that many readers of this blog post tried it in the old times of the Spectum, C64, or later with QBASIC or GWBasic). In the final part of the blog post the author writes: "Because the effect works by plotting pixels individually, it was hard to replicate when developers tried to port the game to hardware accelerated GPU. None of the ports managed to replicate the fizzlefade except Wolf4SDL, which found a LFSR taps configuration to reach resolution higher than 320x200.” While not rocket science, it was possibly hard for other resolutions to find a suitable LFSR. However regardless of the real complexity of finding an appropriate LFSR for other resolutions, the authors of the port could use another technique, called a Feistel Network, to get exactly the same result in a trivial way. What is a Feistel Network? === It’s a building block typically used in cryptography: it creates a transformation between a sequence of bits and another sequence of bits, so that the transformation is always invertible, even if you use all the kind of non linear transformations inside the Feistel network. In practical terms the Feistel network can, for example, translate a 32 bit number A into another 32 bit number B, according to some function F(), so that you can always go from B to A later. Because the function is invertible, it implies that for every input value the Feistel network generates *a different* output value. This is a simple Feistel network in pseudo code: Split the input into L and R halves (Example: L = INPUT & 0xFF, R = INPUT >> 8) REPEAT for N rounds: next_L = R R = L XOR F(R) L = next_L END RETURN the value composing L and R again into a single sequence of bits: R<<8 | L So we basically split a (for example) 16 bit integer into two 8 bit integers L and R, perform some transformation for N rounds, and recompose them back into a 16 bit integer, which is our output. But how is this useful for our problem of implementing FizzleFade? Well you can imagine your 2D screen like a linear array of pixels. If the resolution is 320x200 like in the original game you have from pixel 0 to pixel 63999. So for every integer from 0 to 63999 we can generate a random looking pixel position just by counting and setting the pixel in the position returned by the Feistel network. The problem is that the Feistel network works in bits, so we can’t have exactly from 0 to 63999, we have to pick a power of two which is large enough. The nearest is 16 in this case: with 16 bits we have 65536 integer-to-integer transformations, a few cycles will not be used to set an actual pixel but is not a big waste. So, this is how our Feistel network looks like, in Javascript: /* Transforms the 16 bit input into another seemingly psenduo random number * in the same range. Every input 16 bit input will generate a different * 16 bit output. This is called a Feistel network. */ function feistelNet(input) { var l = input & 0xff; var r = input >> 8; for (var i = 0; i < 8; i++) { var nl = r; var F = (((r * 11) + (r >> 5) + 7 * 127) ^ r) & 0xff; r = l ^ F; l = nl; } return ((r<<8)|l)&0xffff; } The non linear transformation “F” I’m using is just a few random multiplications and shifts, picked mostly at random. I’m using 8 rounds even if it is probably not needed with a better F function, but I want the effect to look random (coincidentally drawing random pixels is a decent way to visually spot trivial bad distribution properties). Implementing this using a Javascript canvas we need a few more functions, to get a 2D context and set a pixel. The final code is in this Gist: https://gist.github.com/antirez/6d58860b221a6ae5622ced8ccdddbe47 You can see the result here: http://antirez.com/misc/fizzlefade.html The original problem to explore in this article was to find a way to implement the effect in different resolutions, so even if it is a trivial extension of the 320x200 case, just to make an example, imagine you want to implement the same with 1024*768. There are 786432 pixels, so 2^20 will fit quite well with 1048576 possible integers. We’ll have to modify the Feistel network to have 20 bits input/output by using 10 bits L and R variables, otherwise everything is pretty much the same, but remember to also change the stop condition (that checks the number of frames). Actually Feistel networks one-to-one pseudo random mapping properties are very useful in other contexts as well. For instance I used it in my radix tree implementation tests (https://github.com/antirez/rax in case you are curious). A good tool to have in a programmer mental box. Comments'
content: extracted
html: 2017-08-29-doing-the-fizzlefade-effect-using-a-feistel-network.html
---

## [Doing the FizzleFade effect using a Feistel network](https://antirez.com/news/113)

[antirez](https://antirez.com/user/antirez) 3308 days ago. 218403 views.

```
Today I read an interesting article about how the Wolfenstein 3D game implemented a fade effect using a Linear Feedback Shift Register. Every pixel of the screen is set red in a pseudo random way, till all the screen turns red (or other colors depending on the event happening in the game). The blog post describing the implementation is here and is a nice read: http://fabiensanglard.net/fizzlefade/index.php

You  may wonder why the original code used a LFSR or why I'm proposing a different approach, instead of the vanilla setPixel(rand(),rand()): doing this with a pseudo random generator, as noted in the blog post, is slow, but is also visually very unpleasant, since the more red pixels you have on the screen already, the less likely is that you hit a new yet-not-red pixel, so the final pixels take forever to turn red (I *bet* that many readers of this blog post tried it in the old times of the Spectum, C64, or later with QBASIC or GWBasic). In the final part of the blog post the author writes:

 "Because the effect works by plotting pixels individually, it was hard to replicate when developers tried to port the game to hardware accelerated GPU. None of the ports managed to replicate the fizzlefade except Wolf4SDL, which found a LFSR taps configuration to reach resolution higher than 320x200.”

While not rocket science, it was possibly hard for other resolutions to find a suitable LFSR. However regardless of the real complexity of finding an appropriate LFSR for other resolutions, the authors of the port could use another technique, called a Feistel Network, to get exactly the same result in a trivial way.

What is a Feistel Network?
===

It’s a building block typically used in cryptography: it creates a transformation between a sequence of bits and another sequence of bits, so that the transformation is always invertible, even if you use all the kind of non linear transformations inside the Feistel network. In practical terms the Feistel network can, for example, translate a 32 bit number A into another 32 bit number B, according to some function F(), so that you can always go from B to A later. Because the function is invertible, it implies that for every input value the Feistel network generates *a different* output value.

This is a simple Feistel network in pseudo code:

    Split the input into L and R halves (Example: L = INPUT & 0xFF, R = INPUT >> 8)
    REPEAT for N rounds:
        next_L = R
        R = L XOR F(R)
        L = next_L
    END
    RETURN the value composing L and R again into a single sequence of bits: R<<8 | L

So we basically split a (for example) 16 bit integer into two 8 bit integers L and R, perform some transformation for N rounds, and recompose them back into a 16 bit integer, which is our output.

But how is this useful for our problem of implementing FizzleFade? Well you can imagine your 2D screen like a linear array of pixels. If the resolution is 320x200 like in the original game you have from pixel 0 to pixel 63999. So for every integer from 0 to 63999 we can generate a random looking pixel position just by counting and setting the pixel in the position returned by the Feistel network. The problem is that the Feistel network works in bits, so we can’t have exactly from 0 to 63999, we have to pick a power of two which is large enough. The nearest is 16 in this case: with 16 bits we have 65536 integer-to-integer transformations, a few cycles will not be used to set an actual pixel but is not a big waste.

So, this is how our Feistel network looks like, in Javascript:

/* Transforms the 16 bit input into another seemingly psenduo random number
 * in the same range. Every input 16 bit input will generate a different
 * 16 bit output. This is called a Feistel network. */
function feistelNet(input) {
    var l = input & 0xff;
    var r = input >> 8;
    for (var i = 0; i < 8; i++) {
        var nl = r;
        var F = (((r * 11) + (r >> 5) + 7 * 127) ^ r) & 0xff;
        r = l ^ F;
        l = nl;
    }
    return ((r<<8)|l)&0xffff;
}

The non linear transformation “F” I’m using is just a few random multiplications and shifts, picked mostly at random.
I’m using 8 rounds even if it is probably not needed with a better F function, but I want the effect to look random (coincidentally drawing random pixels is a decent way to visually spot trivial bad distribution properties).

Implementing this using a Javascript canvas we need a few more functions, to get a 2D context and set a pixel.

The final code is in this Gist: https://gist.github.com/antirez/6d58860b221a6ae5622ced8ccdddbe47

You can see the result here: http://antirez.com/misc/fizzlefade.html

The original problem to explore in this article was to find a way to implement the effect in different resolutions, so even if it is a trivial extension of the 320x200 case, just to make an example, imagine you want to implement the same with 1024*768. There are 786432 pixels, so 2^20 will fit quite well with 1048576 possible integers. We’ll have to modify the Feistel network to have 20 bits input/output by using 10 bits L and R variables, otherwise everything is pretty much the same, but remember to also change the stop condition (that checks the number of frames).

Actually Feistel networks one-to-one pseudo random mapping properties are very useful in other contexts as well. For instance I used it in my radix tree implementation tests (https://github.com/antirez/rax in case you are curious). A good tool to have in a programmer mental box.
```

[blog comments powered by Disqus](https://disqus.com)
