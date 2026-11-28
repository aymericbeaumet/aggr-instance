---
title: 'Show HN: DragDropMath – Learn math by feel'
link: https://dragdropmath.com/
source: hnrss-org
published: 2026-11-27T17:56:28Z
updated: 2026-11-27T17:56:28Z
first_seen: 2026-11-28T04:41:26.390181955Z
authors:
- ziofill
summary: 'Hi HN! I am a professional physicist and I shuffle symbols in equations and mathematical expressions all day long. During my PhD (almost 20 years ago) I noticed that it is as if the symbols have their own ''physics'', which makes the wrong moves impossible and the right ones obvious. So I created DragDropMath, where you can learn how to simplify mathematical expressions and how to solve equations by dragging symbols around, and only correct moves are allowed, so you get to feel what''s right without experiencing (and so learning) wrong moves. I always wanted to make this, but life got in the way. Then a few weeks ago I saw declarative dragging (https://declarative-dragging.github.io/) on the front page of HN and the spark came back! It took quite a few rounds to get the feel right, but I think it''s close to what happens in my mind when I mentally manipulate these expressions. A few more points: - There are no ads, no trackers, no accounts (it''s a static website and progress is saved in the browser cache). - Teachers can create their own problem sets, encoded in the link. - I plan to add more chapters (trigonometry, complex numbers, some calculus) but I first need to figure out the ''right'' mode of interaction. - I just started working on the experience from mobile, so there may still be some rough edges there I''m curious to know what you think, if you have suggestions for content, if you spot anything that I should fix, if you have feature requests! Thank you in advance Comments URL: https://news.ycombinator.com/item?id=50669673 Points: 115 # Comments: 33'
content: extracted
html: 2026-11-27-show-hn-dragdropmath-learn-math-by-feel.html
---

(Quick hint: are you reordering siblings without IDs?)

In order to use d.between, the dragged element must move to distinct locations in each rendered state.

Here, we are dragging element . Two states put it at the point \[\]:

(This is just the first pair of overlaps – other states may also cause overlap.)

If this isn't caused by reordering siblings without IDs, you may have a genuinely ambiguous drag behavior: multiple states bringing the dragged element to the same location. (Or maybe you just repeated a state?)
