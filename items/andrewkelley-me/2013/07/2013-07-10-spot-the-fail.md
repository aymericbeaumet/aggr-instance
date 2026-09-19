---
title: Spot the Fail
link: https://andrewkelley.me/post/spot-the-fail.html
source: andrewkelley-me
published: 2013-07-10T21:47:20Z
updated: 2013-07-10T21:47:20Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'Spot the Fail It''s time to have a little fun. Sometimes when I''m programming, I do something so comically stupid that I feel the need to screenshot the code and share my facepalm moment with someone else. There are a couple guidelines for my flavor of Spot the Fail: Guidelines The fail should be spottable even if you do not know the context of the code. For trickier ones, the mouse or text cursor can optionally be nearby the fail to give a hint. I have collected a few of these screenshots and explanations for your enjoyment. 1. Labyrinth South, South. Should be North, South. 2. jax Classic. Missing break on one of the cases. 3. jax Bytes are not 4 bits long. 4. "Code from work" Case 8 should be [0-7] not [0-8]. 5. motrs It''s the cliché break statement again. Seriously, gotta watch out for that. 6. motrs new T[newSizeX + m_sizeY * m_sizeZ] - the addition should be multiplication. 7. stinkomanlevels.com D''oh! I asked for 1 more character than I should have. 8. motrs m_tileCountY = value; instead of value should be tileCount. 9. menu item Tried to press the "OK" button with ALt+O; instead messed up the shortcut. 10. solidcomposer.com It''s MIME TIME!! 11. solidcomposer.com vim spotted the fail for me. Good job vim. 12. solidcomposer.com state.user !============== null 13. repatriator It''s highlighted in red. Pretty silly. 14. repatriator X and Y are swapped 15. Some Rails Code syntax error: enr 16. motrs "C:\out.bin" - accidentally escaping the ''o'' but more importantly it''s not even running on windows. Fin. Well, hope that was fun. On a relevant note, remember when Quixey in an act of hiring PR offered $100 if you could spot the fail in 1 minute? Good times.'
content: extracted
html: 2013-07-10-spot-the-fail.html
preview:
  file: 2013-07-10-spot-the-fail.preview-80452e21177a.webp
  width: 256
  height: 178
  color: '#ececee'
images:
- source: https://s3.amazonaws.com/superjoe/blog-files/spot-the-fail/labyrinth.png
  original:
    file: 2013-07-10-spot-the-fail.image-d34a80c2147d.png
    width: 665
    height: 463
  variants:
  - file: 2013-07-10-spot-the-fail.image-1c6c49432ab7.webp
    width: 320
    height: 223
  - file: 2013-07-10-spot-the-fail.image-a22b9606b15e.webp
    width: 665
    height: 463
  color: '#fdfdfd'
- source: https://s3.amazonaws.com/superjoe/blog-files/spot-the-fail/jax.png
  original:
    file: 2013-07-10-spot-the-fail.image-a093b724c5ab.png
    width: 909
    height: 662
  color: '#fdfdfd'
- source: https://s3.amazonaws.com/superjoe/blog-files/spot-the-fail/jax-2.png
  original:
    file: 2013-07-10-spot-the-fail.image-b0e488b2ec2a.png
    width: 743
    height: 648
  variants:
  - file: 2013-07-10-spot-the-fail.image-c4958e5aa574.webp
    width: 743
    height: 648
  color: '#fdfdfd'
- source: https://s3.amazonaws.com/superjoe/blog-files/spot-the-fail/work.png
  original:
    file: 2013-07-10-spot-the-fail.image-13e149e657ff.png
    width: 440
    height: 273
  variants:
  - file: 2013-07-10-spot-the-fail.image-1c3ac930848e.webp
    width: 440
    height: 273
  color: '#fdfdfe'
- source: https://s3.amazonaws.com/superjoe/blog-files/spot-the-fail/motrs.png
  original:
    file: 2013-07-10-spot-the-fail.image-30168f06921b.png
    width: 962
    height: 659
  color: '#fdfdfd'
- source: https://s3.amazonaws.com/superjoe/blog-files/spot-the-fail/motrs-2.png
  original:
    file: 2013-07-10-spot-the-fail.image-db37512c93c4.png
    width: 713
    height: 550
  color: '#fcfdfc'
- source: https://s3.amazonaws.com/superjoe/blog-files/spot-the-fail/motrs-3.png
  original:
    file: 2013-07-10-spot-the-fail.image-04f8c93aacf3.png
    width: 1366
    height: 743
  color: '#fbfbfb'
- source: https://s3.amazonaws.com/superjoe/blog-files/spot-the-fail/shortcut.png
  original:
    file: 2013-07-10-spot-the-fail.image-a2aeb00ca3d6.png
    width: 378
    height: 281
  color: '#eae9e8'
- source: https://s3.amazonaws.com/superjoe/blog-files/spot-the-fail/ruby.png
  original:
    file: 2013-07-10-spot-the-fail.image-58de1457635e.png
    width: 737
    height: 864
  color: '#300a24'
- source: https://s3.amazonaws.com/superjoe/blog-files/spot-the-fail/motrs-4.png
  original:
    file: 2013-07-10-spot-the-fail.image-3605b7e1bdfe.png
    width: 717
    height: 768
  color: '#fdfdfd'
---

It's time to have a little fun.

Sometimes when I'm programming, I do something so comically stupid that I feel the need to screenshot the code and share my facepalm moment with someone else.

There are a couple guidelines for my flavor of Spot the Fail:

### Guidelines

- The fail should be spottable *even if you do not know the context of the code.*
- For trickier ones, the mouse or text cursor can optionally be nearby the fail to give a hint.

I have collected a few of these screenshots and explanations for your enjoyment.

## 1\. [Labyrinth](https://github.com/andrewrk/labyrinth)

![](https://s3.amazonaws.com/superjoe/blog-files/spot-the-fail/labyrinth.png)

South, South. Should be North, South.

## 2\. [jax](https://github.com/thejoshwolfe/jax)

![](https://s3.amazonaws.com/superjoe/blog-files/spot-the-fail/jax.png)

Classic. Missing break on one of the cases.

## 3\. [jax](https://github.com/thejoshwolfe/jax)

![](https://s3.amazonaws.com/superjoe/blog-files/spot-the-fail/jax-2.png)

Bytes are not 4 bits long.

## 4\. "Code from work"

![](https://s3.amazonaws.com/superjoe/blog-files/spot-the-fail/work.png)

Case 8 should be \[0-7\] not \[0-8\].

## 5\. [motrs](https://github.com/andrewrk/motrs)

![](https://s3.amazonaws.com/superjoe/blog-files/spot-the-fail/motrs.png)

It's the cliché break statement again. Seriously, gotta watch out for that.

## 6\. [motrs](https://github.com/andrewrk/motrs)

![](https://s3.amazonaws.com/superjoe/blog-files/spot-the-fail/motrs-2.png)

new T\[newSizeX + m\_sizeY \* m\_sizeZ\] - the addition should be multiplication.

## 8\. [motrs](https://github.com/andrewrk/motrs)

![](https://s3.amazonaws.com/superjoe/blog-files/spot-the-fail/motrs-3.png)

m\_tileCountY = value; instead of value should be tileCount.

## 9\. menu item

![](https://s3.amazonaws.com/superjoe/blog-files/spot-the-fail/shortcut.png)

Tried to press the "OK" button with ALt+O; instead messed up the shortcut.

## 15\. Some Rails Code

![](https://s3.amazonaws.com/superjoe/blog-files/spot-the-fail/ruby.png)

syntax error: enr

## 16\. [motrs](https://github.com/andrewrk/motrs)

![](https://s3.amazonaws.com/superjoe/blog-files/spot-the-fail/motrs-4.png)

"C:\\out.bin" - accidentally escaping the 'o' but more importantly it's not even running on windows.

### Fin.

Well, hope that was fun.

On a relevant note, remember when Quixey in an act of hiring PR [offered $100 if you could spot the fail in 1 minute](http://blog.quixey.com/2011/10/03/quixey-challenge/)? Good times.

Thanks for reading my blog post.
