---
title: Building Software Is Learning
link: https://registerspill.thorstenball.com/p/building-software-is-learning
source: registerspill-thorstenball-com
published: 2026-06-02T15:33:52Z
updated: 2026-06-02T15:33:52Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Thorsten Ball
summary: An internal note to the Amp team on feedback and shipping faster
content: extracted
html: 2026-06-02-building-software-is-learning.html
preview:
  file: 2026-06-02-building-software-is-learning.preview-76249af1c7a5.webp
  width: 256
  height: 113
  color: '#e3e3e3'
images:
- source: https://substack-post-media.s3.amazonaws.com/public/images/377243b1-c488-4d95-9143-9d5c676809df_920x406.png
  original:
    file: 2026-06-02-building-software-is-learning.image-c821eb68c618.png
    width: 920
    height: 406
  color: '#f6f6f6'
- source: https://substackcdn.com/image/fetch/$s_!hrv7!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F96e29137-aa72-415c-a9e3-95232387f055.tif
  original:
    file: 2026-06-02-building-software-is-learning.image-d1fbc4b2c30e.png
    width: 154
    height: 12
  variants:
  - file: 2026-06-02-building-software-is-learning.image-430de7918a25.webp
    width: 154
    height: 12
  color: '#000000'
- source: https://substackcdn.com/image/fetch/$s_!P9dJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F49914c6a-58ad-4eac-a261-fbff1bb8f1c2.tif
  original:
    file: 2026-06-02-building-software-is-learning.image-f56ccb6839c5.png
    width: 177
    height: 13
  variants:
  - file: 2026-06-02-building-software-is-learning.image-476f5ca1e8af.webp
    width: 177
    height: 13
  color: '#000000'
---

*A few weeks ago I shared the following as an internal message with the Amp team. I showed it to a friend while talking about feedback loops and he told me to post this publicly. So here we go. Unedited, straight up copy & pasted from our Slack.*

[![](https://substackcdn.com/image/fetch/$s_!hrv7!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F96e29137-aa72-415c-a9e3-95232387f055.tif)](https://substackcdn.com/image/fetch/$s_!hrv7!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F96e29137-aa72-415c-a9e3-95232387f055.tif)

You know what’s rare?

```
person a: “we need this feature”
person b: “yes, let me build it”
...
person b: “done.”
person a: “fantastic, exactly what I wanted.”
```

That’s basically *NEVER* what happens. At least not when you’re building something *new*. It might happen when you fix a bug or when you port something that already exists in another app to a new language or framework, ... Or when you’re building after a spec.

But when there’s no spec, and when you’re building something new?

Here’s how that works:

```
person a: “we need this feature”
person b: “yes, let me build it”
[...]
person b: “done.”
person a: “hmm, actually, that’s not what I meant. what I meant is: [...]”
```

Or this:

```
person a: “we need this feature”
person b: “yes, let me build it”
[...]
person b: “you know what... There’s 3 ways to do this, actually, and I’m not sure what the best way to do this is?”
person a: “ah, I see, I think given that we want to ship this tomorrow, let’s go with way 1”
```

Or:

```
person a: “we need this feature”
person b: “yes, let me build it”
[...]
person b: “done.”
person a: “Don’t like it”
```

Now why does that happen, again and again and again?

Because *building new software is learning*! If you’re building something new and you don’t yet fully know how exactly it’s supposed to work, you will learn what exactly it is that you’re building as you’re doing it. Let me repeat: building new software is learning.

So far, so good, right? But here’s the very important bit, the one bit I want you to take with you into this week: there is *no way in hell*, absolutely zero chance, that you can build something new *and* avoid bumping into “that’s not what I meant”, or “now that I’m working on it I’m not actually sure”, or “hmm, now that I use it, I don’t like it”. Because the only way you could avoid that would be to fully specify what you want up front and, well, guess what programming is? It’s fully specifying what you want. You can’t avoid it, because you can’t define it yet, because *building software is learning*!

Now that was the bad news. Here’s the good ones. You *can* reduce the time the `...` from the examples above takes — the time between the confident “yes, let me build this” and the humbled “oh, I see”.

And that, in turns out, is **the most important thing you can do** when you’re building something new: reducing the time it takes you to go from “let me try something” to getting your ass whooped by reality.

If someone says “we need this feature”, don’t go “yes, let me build it” and hack on something for 4 weeks only for the other person to ultimately go “that’s not what I meant.” No. Instead, embrace that *we need to learn*, that we need to try and play around with this idea as fast as possible, in a way that lets us learn. To embrace that idea means that you try to figure out “what is it they mean” *as fast as possible*, with the minimal effort required, so you can LEARN what it is you’re building.

Instead of going away for 4 weeks and hacking on something, you instead can do stuff like this:

- ... build a prototype, in 1hr, and show it to them, and they go “no, that’s not what I meant, you should change this part here”

- ... write down a spec of how you’d approach it, in 30min, and show it to them, and they go “no that’s not what I meant”

- ... cut up the thing in multiple things and ship one every day, so that every day what you built hits reality and you get to learn, because on day 2 someone says “know what, we should change...”

- ... reduce the scope, figure out what the things are that we’re already sure about and skip those, and instead focus on the bits that we don’t know yet — that’s where we need to learn. don’t add 5 ways to login, if all we need to learn right now is 1.

- ... fake a demo video, show that around, get input on that - Quinn’s done it many times

- ... write the news post that explains the idea — why waste effort building something for a week if the idea can be captured in 3 paragraphs?

- ... write the example code that would go into the README, show that around, does that look like a good API? People don’t need an SDK built if they dislike the API in the readme.

- ...

There’s more options that I didn’t list here. And you don’t have to pick only one. In fact, for something big, you should probably do a few of these things. Or you vary them, or combine them, or ...

What *exactly* you do doesn’t matter as much as constantly asking: how can I get feedback on what I’m trying to build as soon as possible? And “feedback” here is used in the widest sense possible. Feedback comes in all shapes and sizes: feedback from the CI system on main, feedback from colleagues, feedback from users, feedback from *you* once you actually use it.

And if you follow that question — how can I get feedback as soon as possible, so I can learn? — you’ll also find out how to chop things up and how to ship them:

- you won’t get good feedback if you ship an “MVP” of an idea that’s so obviously buggy that all you’ll get is bug reports for 3 days, not actual feedback on how useful it is

- you won’t get good feedback if the people supposed to give you feedback have to jump through 8 hurdles to test it

- you won’t get good feedback if you keep your changes on a branch for 3 weeks, because by the time you merge your 27 commits and CI blows up you have 27 possible causes, vs. 1 if you had merged them one by one

- want feedback on the design of your skateboard? sure, show them the deck, no need for wheels. want feedback on how your skateboard feels? you can’t ship it to testers without wheels on it.

- ...

So. Here’s what I want you to think about going into this week: how can I get feedback on what I’m building on as fast as possible? when is the last time I got valuable feedback on what I’m building? in what frequency do I get feedback on what I’m building? why is that frequency so low?

Because we’re building something new, in a time when software is changing (background vocals: *everything is changing*), and no one has a clue what the fuck is going on - so **the most important thing is to embrace that and** ***as soon as possible, as often as possible, ship things on which we can get feedback on, in a way that gives us valuable feedback — by CI, by production, by our teammates, by select users, by select customers, by all of our users**.*

Yes, italics ***and*** bold. Because building software is learning and we want to learn as much as possible.

[![](https://substackcdn.com/image/fetch/$s_!P9dJ!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F49914c6a-58ad-4eac-a261-fbff1bb8f1c2.tif)](https://substackcdn.com/image/fetch/$s_!P9dJ!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F49914c6a-58ad-4eac-a261-fbff1bb8f1c2.tif)

No posts
