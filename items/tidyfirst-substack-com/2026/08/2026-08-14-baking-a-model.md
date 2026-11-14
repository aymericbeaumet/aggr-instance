---
title: Baking a Model
link: https://newsletter.kentbeck.com/p/baking-a-model
source: tidyfirst-substack-com
published: 2026-08-14T13:35:21Z
updated: 2026-08-14T13:35:21Z
first_seen: 2026-11-14T02:15:15.124110058Z
authors:
- Kent Beck
summary: I remember walking to the bus from high school, staring at a Motorola 6800 instruction set manual.
content: extracted
html: 2026-08-14-baking-a-model.html
remote_preview:
  url: https://substackcdn.com/image/fetch/$s_!PlwL!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F961d0b32-94f1-43bf-84a9-af749bcc28c9_768x1024.jpeg
---

I remember walking to the bus from high school, staring at a Motorola 6800 instruction set manual. I didn’t really understand what I was looking at—boolean expressions, instruction encodings, timing tables—but I was obsessively fascinated by the mechanism of it all. Here was this complicated machine where if I understood it I would have power & control.

I feel the same way about AI models right now. I don’t claim to understand the details, not yet, but I’m fascinated by the mechanism of it all. I’m interested in both:

- How models work but also,

- The machinery that makes a model.

It’s this latter topic, how a model gets constructed, that I will begin to explore in this post (& possible followups).

[![](https://substackcdn.com/image/fetch/$s_!PlwL!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F961d0b32-94f1-43bf-84a9-af749bcc28c9_768x1024.jpeg)](https://substackcdn.com/image/fetch/$s_!PlwL!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F961d0b32-94f1-43bf-84a9-af749bcc28c9_768x1024.jpeg)

Saturday’s Focaccia

I love baking. You take ingredients in one form & transform them to a totally different form. The ingredients aren’t palatable in themselves but what you create from them is delicious. Oh and also baking is sensitive to initial conditions—you can make a small change early in the process & it will have a large consequence later.

I’ve been experimenting with cold proofing, where you let the yeast do its work overnight in a refrigerator. As I was working to understand models it struck me that there’s an analogy there to creating models, at least as I understand the process so far (please correct me in the comments if I’ve gotten something wrong).

First, though, a progressively revealed story about what we mean by a model. I’ll over-simplify but then reveal more complexity a little at a time.

By “model”, we mean a computer system that can converse with a human (see, told you I’d over-simplify—hope you stay with me, this won’t take long).

[![](https://substackcdn.com/image/fetch/$s_!V6B1!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2c43435e-df14-4fb6-b170-1a4bd69a47b0_2604x2657.heic)](https://substackcdn.com/image/fetch/$s_!V6B1!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2c43435e-df14-4fb6-b170-1a4bd69a47b0_2604x2657.heic)

Actually it’s a little more complicated than that. The model is split into 2 parts:

- A user interface that takes care of formatting inputs & outputs & sequencing & authentication & all that stuff.

- The model proper where the magic happens.

[![](https://substackcdn.com/image/fetch/$s_!RWhE!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc449c8cf-8d4a-4715-be23-317365b56cf0_2486x2640.heic)](https://substackcdn.com/image/fetch/$s_!RWhE!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc449c8cf-8d4a-4715-be23-317365b56cf0_2486x2640.heic)

The user interface is built using conventional programming techniques (even though “convention” has changed radically in the last 2 years). The model proper, though (our topic of exploration) is built using radically different techniques.

[![](https://substackcdn.com/image/fetch/$s_!OINh!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F58717e01-2c56-4654-9667-57d36a600ae1_1170x672.jpeg)](https://substackcdn.com/image/fetch/$s_!OINh!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F58717e01-2c56-4654-9667-57d36a600ae1_1170x672.jpeg)

A model is a bag of numbers. For today’s purposes that’s enough (maybe some day we’ll get to how those numbers are used when the model is producing useful words, but I’d have to understand it a little first).

[![](https://substackcdn.com/image/fetch/$s_!CFeV!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa8e3b4f2-e00e-4ee5-8a41-33b6cb138a49_2141x1742.heic)](https://substackcdn.com/image/fetch/$s_!CFeV!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa8e3b4f2-e00e-4ee5-8a41-33b6cb138a49_2141x1742.heic)

How did we come up with these numbers? Unlike in programming, where you lay out a sequence of statements the result of which is a program, AI models result from *training*.

[![](https://substackcdn.com/image/fetch/$s_!kKy8!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fed913c17-e213-40f6-9597-d233320b651c_826x952.jpeg)](https://substackcdn.com/image/fetch/$s_!kKy8!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fed913c17-e213-40f6-9597-d233320b651c_826x952.jpeg)

Training bears some resemblance to programming—you change an artifact to modify its future behavior—but also some huge differences.

You wouldn’t just magically guess a bunch of correct numbers in one go. Oh no, oh no. First we get some approximately correct numbers & then we tweak them to get even better numbers. The two processes are wildly different.

[![](https://substackcdn.com/image/fetch/$s_!qNW0!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6a3d6be7-1258-4cbf-afc5-de64a43a3faa_1416x556.jpeg)](https://substackcdn.com/image/fetch/$s_!qNW0!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6a3d6be7-1258-4cbf-afc5-de64a43a3faa_1416x556.jpeg)

(Near as I can tell, there’s pre-training, post-training, & mid-training (about which I know nothing), but there’s not “training” except as the composition of pre-, mid-, & post-. Here’s hoping the vocabulary evolves.)

Pre-training is a big batch. The whole team sets up the initial conditions—the data & the blank model. They run the data backwards & forwards through the model a gajillion times. They take snapshots along the way in case of crashes. They check for signs that the pre-training has driven off into the weeds & needs to be tweaked & restarted. Pre-training is a big bet—hundreds of millions of dollars & (more expensively) months of delay.

(I need to learn more about how pre-training folks collaborate.)

Pre-training is the cold proofing of model training.

- You mix some stuff together.

- You put it away somewhere where you can’t mess with it.

- You just have to let it play out.

- The result isn’t usable but it’s the precursor to the process that follows.

Post-training is lots of little batches. Folks (called “researchers” but in my naive bluntness I’d call “model engineers”) look at particular problems the raw model addresses poorly & explore possible tweaks that might improve performance. The result is lots of little chunks of code & data (the surviving experiments) that apply to the model as it currently exists.

Post-training supplements the original model. Apply enough supplements & you have a model that, when paired with a UI & a user & compute can respond to, “Give me 5 unusual focaccia toppings.”

Post-training is the shaping & cooking of model training. You take something with potential & make it delicious for humans. (The analogy doesn’t cover the collaborative, iterative, & reversible nature of post-training—le sigh.)

In a followup I’m going to explore the different teams & roles involved in this whole process. They have some interesting divergences of incentives, tools, rhythm, short-term vs long-term, feature vs future, backgrounds, & culture.

First, though, I wanted to double check my understanding of the process. Let me know if I got something wrong above.

**Most teams don’t have a strategy problem. They have an adaptation problem.**\
Your plan was never going to survive contact with reality. The question is whether your organization bends or breaks when it doesn’t.

I help teams bend. *Adapt to Thrive.*

Booking a handful of **custom talks** and **advisory engagements** now. I interview your people, measure your real software flows, and hand you the truth plus what to do about it.

Curious whether it fits? [Tell me about your team.](https://kentbeck.com/kent-beck-sponsorship#inquire)
