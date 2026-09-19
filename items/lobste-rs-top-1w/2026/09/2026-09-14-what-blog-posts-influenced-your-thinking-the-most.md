---
title: What blog posts influenced your thinking the most?
link: https://lobste.rs/s/lbavmm/what_blog_posts_influenced_your_thinking
source: lobste-rs-top-1w
published: 2026-09-14T15:27:32Z
updated: 2026-09-14T15:27:32Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- by mt
labels:
- ask
- practices
summary: Interested in hearing your opinions, I usually learn a lot from it. For me it has to be Parse, Don’t Validate and early joelonsoftware.
content: extracted
html: 2026-09-14-what-blog-posts-influenced-your-thinking-the-most.html
preview:
  file: 2026-09-14-what-blog-posts-influenced-your-thinking-the-most.preview-ff083f62c7ee.webp
  width: 144
  height: 144
  color: '#bb3d38'
images:
- source: https://lobste.rs/touch-icon-144.png
  original:
    file: 2026-09-14-what-blog-posts-influenced-your-thinking-the-most.image-108366a6ff08.png
    width: 144
    height: 144
  color: '#ab120c'
---

A Recipe For a Functional App and especially its middle post, Railway-Oriented Programming [https://fsharpforfunandprofit.com/posts/recipe-part1/#series-toc](https://fsharpforfunandprofit.com/posts/recipe-part1/#series-toc)

It was my first exposure to the Result type, and to the notion of passing the ok-or-error result forward through the pipeline as a value instead of 'return if ok, raise if error' was mind-expanding to me.

A few years later I discovered Elm, and experienced for the first time the joy of sum types and a language that does not let you fall. That brings me to the second blog post on this list:

Compiler Errors for Humans, by Evan Czaplicki, 2015 [https://elm-lang.org/news/compiler-errors-for-humans](https://elm-lang.org/news/compiler-errors-for-humans)

An error message should contain what went wrong, and where, and what you should do to fix it --- that trifecta gave my colleagues and me the words to demand better of each other's error messages.

It's hard to overstate how much Elm's error messages outshone the rest of the field in 2015, and how much they influenced the newer programming languages -- especially Rust -- to have excellent error messages of their own.
