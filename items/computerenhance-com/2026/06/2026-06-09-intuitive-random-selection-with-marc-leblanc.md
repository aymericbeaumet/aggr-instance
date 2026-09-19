---
title: Intuitive Random Selection (with Marc LeBlanc)
link: https://www.computerenhance.com/p/intuitive-random-selection-with-marc
source: computerenhance-com
published: 2026-06-09T17:19:31Z
updated: 2026-06-09T17:19:31Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Casey Muratori
summary: After the ECS interview, Mahk shared a new random selection that's much more intuitive than the one typically used in game development.
content: extracted
html: 2026-06-09-intuitive-random-selection-with-marc-leblanc.html
preview:
  file: 2026-06-09-intuitive-random-selection-with-marc-leblanc.preview-2e60a2aba118.webp
  width: 256
  height: 144
  color: '#6b5970'
images:
- source: https://substackcdn.com/image/youtube/w_728,c_limit/SL4MzRbNdmk
  original:
    file: 2026-06-09-intuitive-random-selection-with-marc-leblanc.image-2d140e1d20c4.jpg
    width: 728
    height: 410
  color: '#0c0526'
---

Suppose you want to pick a random element from a set that is difficult to iterate. Perhaps the candidate elements are embedded in a spatial hierarchy, and only determined by a complex predicate.

One obvious way to solve this problem is to first build a “filtered” array of all the candidate elements, then pick a random index into that array. This always works, and is fair (assuming the RNG is fair) — but it requires reserving space for a potentially-large array, and uses up bandwidth and cache space each time a random selection is made.

A non-obvious-but-common alternative is to use a statistical method where random numbers are chosen during the initial iteration to continuously “retain or replace” a running selection. The random numbers must be carefully constructed, and typically require a modulus on each step. Proof of the fairness of the method is not obvious, and requires knowledge of the relevant underlying math.

In short, while it works, and is likely more efficient than the array method, it has drawbacks.

Inspired by the way you might write such a query in SQL, Marc “Mahk” LeBlanc shared with me a new arrayless picking method that has none of these drawbacks. It’s very similar to the statistical methods version, but without any of the drawbacks.

In this video, Mahk presents the algorithm in a series of steps, starting from a “worst” random selector, and ending with his new one.
