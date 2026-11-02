---
title: Copper – A minimal low-level programming language with OOP and generics
link: https://tibleiz.net/copper/
source: hnrss-org
published: 2026-10-30T19:09:33Z
updated: 2026-10-30T19:09:33Z
first_seen: 2026-11-02T00:33:20.199752031Z
authors:
- bogdanoff_2
content: extracted
html: 2026-10-30-copper-a-minimal-low-level-programming-language-with-oop.html
remote_preview:
  url: https://tibleiz.net/images/home-black.png
---

Copper is a programming language that I use as a replacement to C. The main differences are:

Minimalism is the most important feature, the language is intended to be simple so that someone with limited brain power such as me could fully understand it. The syntax is simple, almost everything is explicit and most constructs are common.

A simple example beyond the pointless "hello world".

```
class Point
    attr x: Int
    attr y: Int

    def init(x: Int, y: Int)
        self.x = x
        self.y = y
    end

    def translate(dx: Int, dy: Int)
        self.x += dx
        self.y += dy
    end
end

func main: Int
    var pt: Point!
    pt.init(3, 5)
    pt.translate(1, 0)
    println("x = \a, y = \a", pt.x, pt.y)
    return 0
end

import "std"
```

Copper is released under the BSD license.
