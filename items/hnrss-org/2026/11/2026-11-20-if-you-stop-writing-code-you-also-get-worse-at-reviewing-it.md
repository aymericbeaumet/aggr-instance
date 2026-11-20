---
title: If you stop writing code, you also get worse at reviewing it
link: https://cekrem.github.io/posts/if-you-stop-writing-code-you-also-get-worse-at-reviewing-it/
source: hnrss-org
published: 2026-11-20T12:41:38Z
updated: 2026-11-20T12:41:38Z
first_seen: 2026-11-20T16:02:15.998933044Z
authors:
- theanonymousone
labels:
- ai
- coding-as-craft
- software craftsmanship
content: extracted
html: 2026-11-20-if-you-stop-writing-code-you-also-get-worse-at-reviewing-it.html
remote_preview:
  url: https://codimg.alwaysdata.net/code.svg?input=%7B-%0A+++++++++++_+++++++++++++++++++++++++++++++++++_+_+++_+++++++++++_+++++++++++_+++++++%0A++++++++++%7C+%7C+++++++++++++++++++++++++++++++++%28_%29+%7C+%7C+%7C+++++++++%7C+%7C+++++++++%28_%29++++++%0A+++___+___%7C+%7C+___+__+___+_+__+___+++++++++__+_+_%7C+%7C_%7C+%7C__++_+++_%7C+%7C__++++++++_++___++%0A++%2F+__%2F+_+%5C+%7C%2F+%2F+%27__%2F+_+%5C+%27_+%60+_+%5C+++++++%2F+_%60+%7C+%7C+__%7C+%27_+%5C%7C+%7C+%7C+%7C+%27_+%5C++++++%7C+%7C%2F+_+%5C+%0A+%7C+%28_%7C++__%2F+++%3C%7C+%7C+%7C++__%2F+%7C+%7C+%7C+%7C+%7C++_++%7C+%28_%7C+%7C+%7C+%7C_%7C+%7C+%7C+%7C+%7C_%7C+%7C+%7C_%29+%7C++_++%7C+%7C+%28_%29+%7C%0A++%5C___%5C___%7C_%7C%5C_%5C_%7C++%5C___%7C_%7C+%7C_%7C+%7C_%7C+%28_%29++%5C__%2C+%7C_%7C%5C__%7C_%7C+%7C_%7C%5C__%2C_%7C_.__%2F++%28_%29+%7C_%7C%5C___%2F+%0A++++++++++++++++++++++++++++++++++++++++++__%2F+%7C++++++++++++++++++++++++++++++++++++++%0A+++++++++++++++++++++++++++++++++++++++++%7C___%2F+++++++++++++++++++++++++++++++++++++++%0A%0A+++cekrem.github.io%2Fposts%2Fif-you-stop-writing-code-you-also-get-worse-at-reviewing-it%0A-%7D%0A%0Atitle+%3A+String%0Atitle+%3D+%22If+you+stop+writing+code%2C+you+also+get+worse+at+reviewing+it%22%0A%0Adescription+%3A+String%0Adescription+%3D%0A++++%22Before+2022+10%2F10+engineers+would+agree+wholeheartedly.+%22%0A++++++++%2B%2B+%22Somehow+it+changed.%28%3F%21%29%22&lang=elm
---

Whatever skill you’re learning, it won’t take long before someone with experience in that particular field tells you to get your hands dirty. It’s fine to read “Born to Run”, but you won’t be running an ultra marathon any time soon if you don’t start running. Whether it’s muscle memory, technique, physical fitness or problem solving you need to work on to reach your goals, you need to *practice*. **This isn’t a controversial statement in *any field at all*, and has never really been.**

Except in software engineering, somehow.

When it comes to software engineering, we somehow lost our way, thinking that *our skills won’t stop improving if we stop practicing*. Making such a claim in any other field would be sneered at at best. Imagine John Scofield[^1] saying he stopped practicing his guitar because “he was at a good enough level, musically, to maintain (and even improve!) his expertise” simply by 1) instructing others on how to play (either in vibe general terms, or with a strict spec musical notation), and then 2) listening to what they played and iterating until it sounded good enough for his next record. “My music is better”, he would claim, and moreover “actually *playing the guitar* was never the point anyway, there’s more to making music.”

Mind you, my point in this post is not to discuss the short term quality of code output (or recorded guitar playing, to continue the proverbial thread), but rather the *skill of the craftsman practicing his craft* vs the one who doesn’t practice at all.

To make another comparison that’s arguably closer to software engineering, or at least the “writing and reasoning about code” part of it, let’s look at authors in other fields. I have no doubt that reading great prose has the potential to nudge a writer towards better writing of his/her own; in fact it’s super important to have creative references to draw upon - no doubt at all. But, imagine an author who 1) stopped writing, and only pursued reading and giving feedback to other writers, and worse yet 2) *mostly read AI generated prose*!? To be perfectly honest, I would expect a quite steady (if not exponential) decline of this author’s writing skills.

## “There’s more to software engineering than writing code” [Link to heading](https://cekrem.github.io/posts/if-you-stop-writing-code-you-also-get-worse-at-reviewing-it/#theres-more-to-software-engineering-than-writing-code)

Sure. But if we can at least agree that **the code that runs our software is a non-trivial part of it** (though, granted, [not all of it by far: The other parts of developing and maintaining software speak even louder against using generative AI as a significant tool service!](https://cekrem.github.io/posts/programming-as-theory-building-naur/)), we should aspire to be proficient in understanding how code works, what good code looks like, what to watch out for and how software architecture plays a vital part in making code Easy To Change.

Simply “reviewing all the code Claude produced” won’t quite cut it. And who are you trying to fool, after all; at this point you’re not really reading half of it, are you?

So I propose something that used to be a no-brainer: **Keep practicing!**

Which brings me to my next point:

## All engineers(!) need to be good at solving problems [Link to heading](https://cekrem.github.io/posts/if-you-stop-writing-code-you-also-get-worse-at-reviewing-it/#all-engineers-need-to-be-good-at-solving-problems)

`if` || `while` you can argue that the need for being hands-on on your software’s *code* varies, I propose you *can’t* argue that we’re still *solving problems*. And the moment you stop *practicing solving problems* is the moment you stop getting better at it, just like everything else. Simple as that. This is hardly new, btw: you could always tell who beat Doom using cheat codes and who finished the game the real way! If the end goal is to “beat the game”, then by all means cheat; if the end goal is *getting good at it*, then I’d advice you not to. Some lessons are only learned the hard way (trial *and error*), and very few are learned *without effort at all*. Repeated LLM usage for problem solving pounds the following premise into our brain’s wiring: If something is hard, tricky or requires effort, you shouldn’t even try. And *right there* goes our learning, craft and joy, right out the window.

> In a world where [people are using AI to summarise books instead of reading them](https://www.reddit.com/r/Bard/comments/1j1g8bo/which_ai_is_best_for_summarizing_books/) and [turning to AI for therapy](https://www.abc.net.au/news/2025-05-19/young-australians-using-ai-bots-for-therapy/105296348), I don’t want to keep using it. It feels like such a slippery slope. [As literacy and numeracy rates decline](https://www.ceda.com.au/research-and-policy/research/education/the-state-of-australian-literacy-and-numeracy), **I can’t help but question whether encouraging us to rely on AI so heavily is a good idea**.
>
> It makes me wonder: are we still thinking for ourselves at all?
>
> — Em’s blog[^2], emphasis mine.

Please, John Scofield, don’t stop playing your guitar!

[^1]: ![John scofield playing at the Moers Festival in 2021](https://upload.wikimedia.org/wikipedia/commons/9/99/John_Scofield-8615.jpg)

[^2]: I’m not sure who Em is, [but their blog](https://em-online.bearblog.dev/blog/) is great!
