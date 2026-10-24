---
title: Teal – A statically-typed dialect of Lua
link: https://teal-language.org/
source: hnrss-org
published: 2026-10-16T21:07:24Z
updated: 2026-10-16T21:07:24Z
first_seen: 2026-10-18T18:36:03.647050480Z
authors:
- Lyngbakr
content: extracted
html: 2026-10-16-teal-a-statically-typed-dialect-of-lua.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50124730
  num_comments: 56
  points: 128
---

## About Teal

Teal is a statically-typed dialect of [Lua](https://www.lua.org). It extends Lua with type annotations, allowing you to specify arrays, maps and records, as well as interfaces, union types and generics.

It aims to fill a niche similar to that of TypeScript in the JavaScript world, but adhering to Lua's spirit of minimalism, portability and embeddability.

Is it implemented as a compiler, [tl](https://github.com/teal-language/tl), which compiles `.tl` source code into `.lua` files.

## A taste of Teal

Here is a quick taste of what Teal code looks like:

```
local function add(a: number, b: number): number
   return a + b
end

local s = add(1, 2)
print(s)
```

...or check out this page's source code!

You can also play with the Teal compiler right from your browser, using the [Teal Playground](https://teal-playground.netlify.app/).

## Install Teal

If you're using [LuaRocks](https://luarocks.org), you can install the compiler with:

```
luarocks install tl
```

You can also find [pre-compiled binaries](https://github.com/teal-language/tl/releases) for Linux and Windows.

To build larger projects, you probably won't want to run `tl` on each file individually. We recommend using [Cyan](https://github.com/teal-language/cyan), the build tool designed for Teal.

You can also install [vscode-teal](https://github.com/teal-language/vscode-teal/) for Visual Studio Code integration, [teal-language-server](https://github.com/teal-language/teal-language-server/) for NeoVim and others, and [more](https://github.com/teal-language/awesome-teal).

## Documentation

You can browse the Teal documentation [online](https://teal-language.org/book/latest/index.html).

Here are some recorded talks discussing the history of Lua and types, outlining the motivations behind Teal and talking about the project's progress:

- [Minimalism versus types (2019)](https://www.youtube.com/watch?v=OPyBQRndLUk)
- [Minimalistic typed Lua is here (2020)](https://www.youtube.com/watch?v=HfnjUCRzRKU)
- [What's next for Teal (2021)](https://www.youtube.com/watch?v=OqXbnaDR8QY)
- [Five years of Teal: minimalism versus growth in language design (2024)](https://www.youtube.com/watch?v=68F5Qs91izQ)
- [What should Teal be? - musings on FOSS project directions (2025)](https://www.youtube.com/watch?v=Uq_8bckDxaU)

## Community

Development happens on [GitHub](https://github.com/teal-language).

We have a [community forum](https://github.com/teal-language/tl/discussions) hosted by GitHub. You can also chat with us via Matrix at [#teal-language\_community:gitter.im](https://matrix.to/#/#teal-language_community:gitter.im).

Teal is a project started by [Hisham Muhammad](https://hisham.hm), developed by a [growing number of contributors](https://github.com/teal-language/tl/graphs/contributors) and is written using Teal itself!

## License

Teal is free and open source software.\
Its [license](https://github.com/teal-language/tl/blob/master/LICENSE) is MIT, the same as Lua.
