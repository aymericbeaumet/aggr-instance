---
title: Making any integer with four 2s (2025)
link: https://eli.thegreenplace.net/2025/making-any-integer-with-four-2s/
source: hnrss-org
published: 2026-11-07T22:38:58Z
updated: 2026-11-07T22:38:58Z
first_seen: 2026-11-08T06:37:14.114093134Z
authors:
- germanrabbit
content: extracted
html: 2026-11-07-making-any-integer-with-four-2s-2025.html
remote_preview:
  url: https://eli.thegreenplace.net/images/logosmall.png
---

There's a cute math puzzle that can be interesting to folks on very different levels:

> Given exactly four instances of the digit 2 and some target natural number, use any mathematical operations to generate the target number with these 2s, using no other digits.

Some examples can be done by elementary school kids:

In middle school, kids learn about exponents, factorials, etc. which expands the range considerably:

Then come the tricks; for example, the number 22 (twenty two) can be seen as a valid use of two 2s, and so on; so we can have:

Getting to 7 is notoriously difficult, but if you allow even more mathematical tools like the [Gamma function](https://en.wikipedia.org/wiki/Gamma_function), it becomes easy:

The more math skill people have, the more numbers they can make. See [this thread](https://math.stackexchange.com/questions/1034122/get-the-numbers-from-0-30-by-using-the-number-2-four-times) for some fun concoctions using integrals, repeating fractions and combinatorial operators. One of my favorite examples involves complex numbers:

So the fun doesn't end even after one graduates from university! In fact, this seems to have been a favorite pastime for mathematicians in the 1920s. Until [Paul Dirac](https://en.wikipedia.org/wiki/Paul_Dirac) ruined it for everyone by finding a general solution for every number.

It's all about nested square roots:

If the square root is applied *n* times:

All that's left now is some base-2 logarithms:

And another:

This leads to the general formula:

There's just one small wrinkle: it uses *three* instances of the digit 2, not four. This is easy to amend, however; since , we can replace any single digit with that and get exactly four:

One may claim this is cheating, but it seems to be in line with the rules of the puzzle! Note that the entity *n* doesn't actually appear anywhere - it's just a helper to count the number of repeated square roots. For example, another way to express 7 is:

There are exactly four 2s, and this uses only reasonable, elemental math operations to do the calculation. It's clear that *any* number can be expressed this way; the only challenge is properly drawing all those square roots!

## Credits

I've read about this story in Graham Farmelo's book *The Strangest Man: The Hidden Life of Paul Dirac, Quantum Genius*. I'm enjoying this book so far.

* * *

For comments, please send me [an email](mailto:eliben@gmail.com).
