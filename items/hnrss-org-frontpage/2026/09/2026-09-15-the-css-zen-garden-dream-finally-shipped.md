---
title: The CSS Zen Garden dream, finally shipped
link: https://josprague.com/blog/the-css-zen-garden-dream-finally-shipped/
source: hnrss-org-frontpage
published: 2026-09-15T14:40:08Z
updated: 2026-09-15T14:40:08Z
first_seen: 2026-09-15T23:09:31.935147691Z
authors:
- yosito
summary: 'Article URL: https://josprague.com/blog/the-css-zen-garden-dream-finally-shipped/ Comments URL: https://news.ycombinator.com/item?id=49713262 Points: 112 # Comments: 56'
content: extracted
html: 2026-09-15-the-css-zen-garden-dream-finally-shipped.html
preview:
  file: 2026-09-15-the-css-zen-garden-dream-finally-shipped.preview-7bc2c7234919.webp
  width: 256
  height: 134
  color: '#0b0a08'
images:
- source: https://josprague.com/og.png
  original:
    file: 2026-09-15-the-css-zen-garden-dream-finally-shipped.image-347c86aabc83.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-15-the-css-zen-garden-dream-finally-shipped.image-0a8c294fd916.webp
    width: 320
    height: 168
  - file: 2026-09-15-the-css-zen-garden-dream-finally-shipped.image-15ed5f2bc400.webp
    width: 640
    height: 336
  - file: 2026-09-15-the-css-zen-garden-dream-finally-shipped.image-a6e2db3090ab.webp
    width: 960
    height: 504
  - file: 2026-09-15-the-css-zen-garden-dream-finally-shipped.image-2b4f9f8cab50.webp
    width: 1200
    height: 630
  color: '#000000'
---

Rebuilding Firefox.com with Mozilla and Lincoln Loop on modern, native CSS with no preprocessors, and what that means for design systems today.

Back in 2008, fresh out of college, I discovered [CSS Zen Garden](https://csszengarden.com/), Dave Shea’s project where one HTML file could be restyled into something completely different using nothing but CSS. It was a glimpse of the dream: clean, reusable design, fully separated from content.

Then you tried to ship real client work, and the dream fell apart. There were no CSS variables. The properties available couldn’t express a full-fidelity design, so we leaned on server-side processing, images, table-based layouts, and endless hacks. Every browser rendered things differently, so much of the work was just making one design behave across all of them. Zen Garden showed what was possible in theory. Production was another story.

That gap took the better part of two decades to close, and most of the closing happened in the last few years. Custom properties gave us variables the browser understands. Grid and Flexbox gave us layout that doesn’t fight the medium. The properties we lacked in 2008 now exist, and they’re implemented consistently enough that you can design against them instead of around them.

## Firefox.com

That gap is finally closed. Modern CSS now does natively what we used to need preprocessors and hacks for. On the rebuild of [Firefox.com](https://firefox.com), working with Mozilla and the team at Lincoln Loop, I built the whole system in modern, native CSS with no preprocessors. Custom properties are exported straight from design files, native CSS is written once with no hacks, and it works in every modern browser, with a minimal branded stylesheet giving legacy browsers basic, accessible branding.

Together we built a design system of more than 70 components and 25 page templates, implemented as Wagtail components so the site’s content team can assemble pages without engineering help. The site currently supports 19 locales.

There’s one honest footnote. We did end up with PostCSS in production, used for a single job: inlining `@import` statements. Native `@import` still has terrible performance characteristics in some browsers, and on a site like this one that matters more than architectural purity. The authored CSS is still plain, native CSS. Nothing in it depends on a build step to be valid or to make sense. The build only flattens what the browser would otherwise fetch serially.

That distinction is worth keeping in mind when someone tells you a project is “no build step.” What matters is whether the source you write is the language the browser speaks, or a dialect that only exists until compilation.

## Why this mattered to me

The Zen Garden dream, finally realized in production. Doing that with Mozilla, one of the leaders of web standards, meant a lot. Firefox.com is a site about the browser, made by the organization that spent 20 years arguing for the platform being used to build it. It’s hard to think of a better place to find out whether the platform is really ready.

It is.

I owe a debt to the people who shaped how I think about this craft: [Nicole Sullivan](https://www.linkedin.com/in/nicolesullivan/), [Rachel Andrew](https://www.linkedin.com/in/rachelandrew/), [Jen Simmons](https://www.linkedin.com/in/jensimmons/), [Chris Coyier](https://www.linkedin.com/in/chris-coyier-1aa843100/), [Kasey Kelly](https://www.linkedin.com/in/kkellydesign/), who connected me to the Mozilla project, and [Eric Meyer](https://www.linkedin.com/in/meyerweb/), my hometown hero. Their work taught a whole generation of us to think about CSS as a system.

## The thread

This is the thread running through all my work: design systems that let teams move fast and stay consistent, whether the building blocks are pure CSS, Tailwind, or shadcn/ui. The tools change every few years. What doesn’t change is the value of a system where the right thing to do is also the easy thing to do, and where a designer’s decision travels to production without being translated three times along the way.

If your team is rethinking its front-end foundation, [get in touch](https://josprague.com/contact/?topic=contract).
