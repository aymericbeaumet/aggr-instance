---
title: The GitHub wiki is an anti-pattern (2022)
link: https://michaelheap.com/github-wiki-is-an-antipattern/
source: hnrss-org-frontpage
published: 2026-09-23T13:05:49Z
updated: 2026-09-23T13:05:49Z
first_seen: 2026-09-23T19:00:13.987032799Z
authors:
- ibobev
summary: 'Article URL: https://michaelheap.com/github-wiki-is-an-antipattern/ Comments URL: https://news.ycombinator.com/item?id=49815484 Points: 142 # Comments: 84'
content: extracted
html: 2026-09-23-the-github-wiki-is-an-anti-pattern-2022.html
preview:
  file: 2026-09-23-the-github-wiki-is-an-anti-pattern-2022.preview-2dfb52723a43.webp
  width: 256
  height: 128
  color: '#acaaaa'
images:
- source: https://michaelheapcom-og-image.s3.amazonaws.com/custom/github-wiki-antipattern.png
  original:
    file: 2026-09-23-the-github-wiki-is-an-anti-pattern-2022.image-188bba0434af.png
    width: 1024
    height: 512
  variants:
  - file: 2026-09-23-the-github-wiki-is-an-anti-pattern-2022.image-1825a05c82bf.webp
    width: 320
    height: 160
  - file: 2026-09-23-the-github-wiki-is-an-anti-pattern-2022.image-9f936a4d43ea.webp
    width: 640
    height: 320
  - file: 2026-09-23-the-github-wiki-is-an-anti-pattern-2022.image-6edebee29b46.webp
    width: 960
    height: 480
  - file: 2026-09-23-the-github-wiki-is-an-anti-pattern-2022.image-686eca5500f9.webp
    width: 1024
    height: 512
  color: '#fefefe'
---

The “[should I use the wiki or a docs folder on GitHub?](https://twitter.com/joemasilotti/status/1483124554843058180)” discussion comes up every 6 months or so, and following Shawn Wang’s [three strikes rule](https://www.swyx.io/three-strikes/) I thought it was about time I wrote something down about it.

The initial version of this post opened with “You can use the wiki or a `docs` folder for your GitHub project, both are valid choices” but as I wrote more, I realised that there is a single reason to use a wiki, and many more reasons *not* to use the wiki. So many in fact, that I consider **using the wiki on GitHub is an anti-pattern**.

Let’s start with the benefits of using a wiki:

1. You can get to the wiki contents in a single click from anywhere in the repo
2. There is no 2.

Really, the only benefit that I’ve been able to find to using the wiki is that it’s always there.

How about the reasons **not** to use the wiki?

1. Documentation is versioned alongside your code when using the `/docs` folder. If you need to use an old version, the docs are easy to find
2. The documentation isn’t available locally when someone clones your repo (you can clone the wiki separately, but this is a hidden feature)
3. Documentation edits get the same treatment as code. They get a full peer review through the pull request process
4. You can use GitHub Actions to lint your docs using tools such as [Vale](https://github.com/errata-ai/vale-action)
5. People can work with tooling that they already know (e.g. `vscode` with spellcheck)
6. Wikis provide limited branding opportunities. They all look pretty much the same
7. The wiki doesn’t support image uploads, so you have to put images somewhere else anyway

Now that you’re sold on the idea of keeping docs alongside your code, how do you make it easy for people to view them?

1. Add your docs to your repository in the `/docs` folder. Do *not* use the `gh-pages` branch as this prevents docs being versioned alongside code
2. Set up a GitHub pages build to publish the docs
   - If you’re just getting started, I recommend using the `just-the-docs` theme and letting GitHub build and publish your docs
   - If you prefer to build your own workflow (e.g. using Hugo), you can use [this](https://github.com/peaceiris/actions-gh-pages/) GitHub Action to publish the docs
3. Add a single wiki page directing people to the hosted documentation

Using the `/docs` folder is the highest effort-to-reward ratio option whilst you’re building out a new product. At some point your docs will outgrow a single folder, and then all bets are off. You’ll want a separate repo with its own build process, pull request review guidelines and a whole host of other things. At that point people are already used to working with docs in a repository, and the migration from `/docs` to its own repo should be seamless for your contributors.

Whether you agree or disagree, I’d love to hear your thoughts [on Twitter](https://twitter.com/mheap/)
