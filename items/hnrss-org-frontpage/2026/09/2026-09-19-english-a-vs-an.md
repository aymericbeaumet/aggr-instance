---
title: 'English: A vs. An'
link: https://www.redblobgames.com/blog/2026-09-16-english-a-vs-an/
source: hnrss-org-frontpage
published: 2026-09-19T20:41:26Z
updated: 2026-09-19T20:41:26Z
first_seen: 2026-09-20T04:48:43.602226688Z
authors:
- azhenley
summary: 'Article URL: https://www.redblobgames.com/blog/2026-09-16-english-a-vs-an/ Comments URL: https://news.ycombinator.com/item?id=49769944 Points: 136 # Comments: 174'
content: extracted
html: 2026-09-19-english-a-vs-an.html
preview:
  file: 2026-09-19-english-a-vs-an.preview-9394775402f1.webp
  width: 256
  height: 256
  alt: Tree style visualization of the first two letters of a word
  color: '#f4f4f4'
images:
- source: https://www.redblobgames.com/x/2635-a-vs-an/blog/level-2f.png?2026-08-25-11-10-15
  original:
    file: 2026-09-19-english-a-vs-an.image-f303872534bb.png
    width: 2275
    height: 2276
  color: '#fcfbfb'
---

Blog post: 16 Sep 2026

In English, there is an “indefinite” article a that can go before a word. For example, a raccoon. But for some words, we use an. For example, an apple.

When procedurally generating text, I want a function `a_or_an("apple")` that tells me which article to use. That seems like it’d be easy. We can check the first letter to see if it’s a vowel. But that would mean we output an unicorn, not a unicorn.

The actual rule is not whether the *written* word starts with a vowel letter, but whether the *spoken* word starts with a vowel sound. The word \<unicorn> starts with vowel letter (\<u>) but a consonant sound (cmudict `Y`, ipa `/j/`). The word \<hour> starts with a consonant letter (\<h>) but a vowel sound (cmudict `AW`, ipa `/aʊ/`).

[![Tree style visualization of the first two letters of a word](https://www.redblobgames.com/x/2635-a-vs-an/blog/level-2f.png?2026-08-25-11-10-15)](https://www.redblobgames.com/x/2635-a-vs-an/blog/level-2f.png)\
Visualization showing whether the first two letters of a word are enough to determine whether it should have “a” or “an”

I was curious how often these exceptions occurred, and whether they can be grouped together, so [I spent a day looking at the data and building some visualizations and wrote up the results.](https://www.redblobgames.com/x/2635-a-vs-an/) I was surprised that only 129 of the 32,455 words in my list needed exceptions.

\[LLM note: I did *not* use LLMs to write any of this code, but in hindsight, I should have. This is one-off code to answer a question. It doesn’t need to be clean or maintainable. It only needs to be correct. I would’ve spent more time on the trie simplification algorithm and less time on parsing cmudict and re-learning d3.js.\]
