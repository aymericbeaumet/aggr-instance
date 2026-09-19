---
title: Fast and Hard Code
link: https://lucumr.pocoo.org/2026/8/22/fast-hard-code/
source: lucumr-pocoo-org
published: 2026-08-22T00:00:00Z
updated: 2026-08-22T00:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Armin Ronacher
content: extracted
html: 2026-08-22-fast-and-hard-code.html
preview:
  file: 2026-08-22-fast-and-hard-code.preview-cdb79af1dd86.webp
  width: 256
  height: 134
  color: '#1f3559'
images:
- source: https://lucumr.pocoo.org/social/2026-08-22-fast-hard-code-social.png
  original:
    file: 2026-08-22-fast-and-hard-code.image-68aa6dfd1238.png
    width: 1200
    height: 630
  variants:
  - file: 2026-08-22-fast-and-hard-code.image-425929cd7f56.webp
    width: 320
    height: 168
  - file: 2026-08-22-fast-and-hard-code.image-70481b1fbd92.webp
    width: 640
    height: 336
  - file: 2026-08-22-fast-and-hard-code.image-342cc7e8749a.webp
    width: 1200
    height: 630
  color: '#1b3156'
---

One of the memes on Twitter is that “programming is solved now.” I’m not sure to what degree it is, but one thing is pretty clear: the act of familiarizing yourself with a language no longer matters and some of the friction that mattered for humans does not matter for agents.

As a result, LLMs make language choice much less consequential than it used to be. If you don’t like the choice, you can seemingly rewrite it in another language and you can make it pick a language that you, as a programmer, are entirely unfamiliar with.

Which in turn means that people can, and do, choose based on the marketing of languages much more. As a long-term Rust programmer I found it quite fascinating to see people now ship Rust code who previously might not have chosen it. I attribute at least one part of this to two recent vibe shifts: there is a lot more talk about wanting fast software, and about LLMs being exceptional at optimizing code without regressing behavior.

Folks like Mitchell Hashimoto, Charlie Marsh, Jarred Sumner, Daniel Lemire and quite a few others always carried a certain level of obsession with fast and performant software and they also all happen to be receptive to agents writing code. Maybe as a result, or unrelated others are now joining in. That’s because with things like [autoresearch](https://github.com/davebcn87/pi-autoresearch) you don’t even necessarily need to know all the tricks: you just need to put an agent on it — though knowledge greatly helps!

If you look around, there are plenty of projects that want to be fast and small, and they increasingly pick “hard languages”. And it’s not just Rust that is benefiting. Even Zig — despite the fact that the creators and parts of the core community are pretty negative on the whole AI thing — is too. For instance Cloudflare’s new [Artifacts](https://blog.cloudflare.com/artifacts-git-for-agents-beta/) service uses a pure-Zig Git-protocol engine, compiled to a roughly 100 KB WebAssembly module and Vercel released [fx](https://github.com/vercel-labs/fx), a Zig coding agent advertised to be small and fast. From what I can tell, all these projects are largely LLM-assisted.

But it’s not just people picking less common languages but also that they are increasingly working with “much harder” technologies. All of a sudden I have seen people do some really impressive stuff with DWARF files, eBPF, custom network drivers, custom crypto and really old computing hardware. Many of these things were previously off-limits for lots of developers. In some cases (eg: crypto) you were even pushed away because those things were intentionally gatekept by the people in the know.

So maybe the world will have more slop, but it might also have more developers in it, that want things to be fast and small.

This entry was tagged [ai](https://lucumr.pocoo.org/tags/ai/), [programming](https://lucumr.pocoo.org/tags/programming/) and [thoughts](https://lucumr.pocoo.org/tags/thoughts/)

[copy as](https://lucumr.pocoo.org/2026/8/22/fast-hard-code.md) / [view](https://lucumr.pocoo.org/2026/8/22/fast-hard-code.md) markdown
