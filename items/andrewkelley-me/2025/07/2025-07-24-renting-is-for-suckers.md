---
title: Renting is for Suckers
link: https://andrewkelley.me/post/renting-is-for-suckers.html
source: andrewkelley-me
published: 2025-07-24T19:04:14Z
updated: 2025-07-24T19:04:14Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'Renting is for Suckers A genie offers you a choice: 50% chance to gain 10 million USD 100% chance to gain 1 million USD Which do you pick? Your answer, of course, depends on how much capital you have. If you are struggling to pay rent, then taking the less risky gamble is quite attractive, while if you already have 10 million sitting in the bank, it''s a no-brainer to press the button with the higher expected value (1 mil vs 5 mil). Despite living in a world without genies, we are presented with choices like this constantly - having more capital absorbs risk, offering choices with higher expected value. For instance, getting a payday loan is insane from a financial perspective, unless you won''t be able to feed your kids this week without begging for help from an abusive relative, in which case it starts to become tempting. Paying interest on a car is equally ridiculous, unless you live in a city like my hometown where you really can''t get by without a car, and you lack the capital to buy one. There''s no excuse for renting a couch. Just sit on the floor. The pattern here is a reverse Robin Hood effect: those without capital borrow stuff and then pay interest to the owners. This is managing your personal finances 101, really basic stuff. But what if I told you that we are in the midst of an entire upper-middle-class not only willingly giving up ownership, but gleefully bragging about it, while transferring massive amounts of wealth from smaller companies and individuals to approximately three large companies? Manufactured Consent The section title here is of course a reference to the famous book by Edward S. Herman and Noam Chomsky but I''m going to take it in a little bit of a different direction. The introduction of this blog post is intended to be completely obvious, uncontroversial stuff. Why is it, then, that people think "moving to the cloud" is a good idea? You know what I mean, right? If you ask your colleagues, or random people on the Internet, there will be a general consensus that moving to the cloud is Good. "Oh yeah," someone will say, "we''ve been working on moving to the cloud, you know." "We gotta move more of our stuff to the cloud!" - totally normal thing to say. Why is something that flies in the face of basic financial planning considered good? You''re switching from owning a computer to renting one, and paying two orders of magnitude more money for it. For example let''s look at Microsoft pricing. Using a single macOS runner for 1 year costs 0.08 * 60 * 24 * 365 = 42 thousand USD. Meanwhile, for Zig''s CI testing we purchased a beefy mac mini for $3000 two years ago. So why is everybody moving to the cloud?! My hypothesis is that this is clever strategy from cloud provider companies. Since, as demonstrated above, there is so much potential money to be gained for them, given that they are starting with a high amount of capital, they have an enormous budget, which they spend in order to capture more of the market. And what they''re up against is basic math. In the words of Drew DeVault, "cloud computing is like taking money, and lighting it on fire." So how do you convince people to ignore basic math and give you their money? It''s a tall order, isn''t it? They do conferences. Ever notice how major software conferences are often sponsored by cloud companies? Developers are inundated with astroturfed talks which take as a premise that moving to the cloud is a sound decision. Entire careers are built upon this. Nobody is immune to propaganda. Eventually, the Illusory Truth Effect kicks in and we all start to believe it, at least to some degree, and then we start participating in doing the marketing for them, by virtue of trying to keep up to date with the industry 1 2 . When I worked at OkCupid, I was tasked with migrating from a data center full of computers, to Kubernetes in the cloud. Something always felt off about it but I couldn''t quite place my finger on it at the time. I didn''t get very far, and I don''t know where they landed with that stuff, but I would bet you anything it ended up being nothing but a colossal waste of time and money. Renting is for suckers. It''s Happening Again Personal computers today are magnificent beasts. You could run one of the top 100 most popular websites from a 5 year old laptop. And yet, people are using LLMs to code when they have a powerful machine that they actually own sitting there, idle. Consider where we are now. We own the devices that we use to program. The act of programming uses a trickle of electricity and is effectively free. It''s one of the cheapest hobbies you can get into. Programming has no dependency on the Internet. Maybe you download some documentation, search for some troubleshooting tips, participate in a community, fetch some dependencies. But being offline does not compromise one''s ability to code. It''s happening again. We''re being told the industry will leave us behind if we don''t move programming itself to the cloud. So let''s get this straight by doing some basic math. My monthly costs as a programmer are $0. I use free software to create free software. I can program on the train, I can program on a plane, I can program when my ISP goes down. I never have to trust any third party with my private information, nor trust them to run arbitrary commands on my computer. Now I switch to agentic coding. First of all, the costs are difficult to calculate, and that''s part of the problem. Same thing with cloud computing - funny how it''s the cloud that does the accounting and tells you how much you owe, isn''t it? In this case it''s even more suspicious because the company that bills you not only counts how much you owe them, it also controls the agent''s behavior in terms of how many requests it tries to make. So they could easily insert into their system prompt something like, "our earnings this quarter are a little short so try to pick strategies when doing agentic coding that end up earning us more API requests, but keep it subtle." There''s no oversight. They could even make it target specific companies. They could also change the model so that it favors trying to solve problems using their own cloud solutions or pushes their marketing agenda, and it would be basically impossible to detect. I''d be surprised if they weren''t already doing this. Anyway let''s try to compare one month of programming 8 hours per day. I''m going to estimate 1 prompt per minute, for a total of 60 * 8 * 30 = 14400 prompts in a given month. Based on Zed''s pricing page, that comes out anywhere from $576/month to $2,880/month to $90,000/month depending on which model you use and whether you enable burn mode/max mode. So on one hand we have $0 per day for a total of $0 per month, programming in total privacy and security, while on the other hand we have to gamble and pay rent, while also trusting OpenAI, Google, Microsoft, Amazon, or Meta - all companies which have already proven their untrustworthiness time and time again. Did you read their Terms of Service Agreement before you clicked "auto-accept all"? How about the email in your spam folder that says "Updated Terms of Service Agreement"? Don''t be a sucker. Use your own computer.'
content: extracted
html: 2025-07-24-renting-is-for-suckers.html
---

A genie offers you a choice:

- 50% chance to gain 10 million USD
- 100% chance to gain 1 million USD

Which do you pick?

Your answer, of course, depends on how much **capital** you have. If you are struggling to pay rent, then taking the less risky gamble is quite attractive, while if you already have 10 million sitting in the bank, it's a no-brainer to press the button with the higher expected value (1 mil vs 5 mil).

Despite living in a world without genies, we are presented with choices like this constantly - having more capital absorbs risk, offering choices with higher expected value.

For instance, getting a payday loan is insane from a financial perspective, unless you won't be able to feed your kids this week without begging for help from an abusive relative, in which case it starts to become tempting.

Paying interest on a car is equally ridiculous, unless you live in a city like [my hometown](https://en.wikipedia.org/wiki/Phoenix,_Arizona) where you really can't get by without a car, and you lack the capital to buy one.

There's no excuse for [renting a couch](https://www.youtube.com/watch?v=gMt-Opo1Ovw). Just sit on the floor.

The pattern here is a reverse Robin Hood effect: those without capital borrow stuff and then pay interest to the owners.

This is managing your personal finances 101, really basic stuff.

But what if I told you that we are in the midst of an entire upper-middle-class not only willingly giving up ownership, but gleefully bragging about it, while transferring massive amounts of wealth from smaller companies and individuals to approximately three large companies?

## Manufactured Consent

The section title here is of course a reference to [the famous book by Edward S. Herman and Noam Chomsky](https://en.wikipedia.org/wiki/Manufacturing_Consent) but I'm going to take it in a little bit of a different direction.

The introduction of this blog post is intended to be completely obvious, uncontroversial stuff.

Why is it, then, that people think "moving to the cloud" is a good idea?

You know what I mean, right? If you ask your colleagues, or random people on the Internet, there will be a general consensus that moving to the cloud is Good. "Oh yeah," someone will say, "we've been working on moving to the cloud, you know." "We gotta move more of our stuff to the cloud!" - totally normal thing to say.

Why is something that flies in the face of basic financial planning considered good? You're switching from owning a computer to renting one, and paying *two orders of magnitude* more money for it.

For example let's look at [Microsoft pricing](https://docs.github.com/en/billing/managing-billing-for-your-products/about-billing-for-github-actions). Using a single macOS runner for 1 year costs 0.08 \* 60 \* 24 \* 365 = **42 thousand USD**. Meanwhile, for [Zig](https://ziglang.org/)'s CI testing we purchased a beefy mac mini for $3000 two years ago.

So why is everybody moving to the cloud?!

My hypothesis is that this is clever strategy from cloud provider companies. Since, as demonstrated above, there is so much potential money to be gained for them, given that they are starting with a high amount of capital, they have an *enormous* budget, which they spend in order to capture more of the market.

And what they're up against is basic math. In the words of [Drew DeVault](https://drewdevault.com/), "cloud computing is like taking money, and lighting it on fire."

So how do you convince people to ignore basic math and give you their money? It's a tall order, isn't it?

They do conferences. Ever notice how major software conferences are often sponsored by cloud companies? Developers are inundated with astroturfed talks which take as a premise that moving to the cloud is a sound decision. [Entire careers are built upon this](https://en.wikipedia.org/wiki/Technology_evangelist). Nobody is immune to propaganda. Eventually, the [Illusory Truth Effect](https://en.wikipedia.org/wiki/Illusory_truth_effect) kicks in and we all start to believe it, at least to some degree, and then we start participating in doing the marketing for them, by virtue of trying to keep up to date with the industry [1](https://xeiaso.net/blog/2024/overengineering-preview-site/) [2](https://sharnoff.io/blog/why-rust-compiler-slow) .

When I [worked at OkCupid](https://andrewkelley.me/post/full-time-zig.html), I was tasked with migrating from a data center full of computers, to Kubernetes in the cloud. Something always felt off about it but I couldn't quite place my finger on it at the time. I didn't get very far, and I don't know where they landed with that stuff, but I would bet you anything it ended up being nothing but a colossal waste of time and money.

Renting is for suckers.

## It's Happening Again

Personal computers today are magnificent beasts. You could run one of the top 100 most popular websites from a 5 year old laptop.

And yet, people are [using LLMs to code](https://antirez.com/news/154) when they have a powerful machine that they actually own sitting there, idle.

Consider where we are now. We own the devices that we use to program. The act of programming uses a trickle of electricity and is effectively free. It's one of the cheapest hobbies you can get into.

Programming has no dependency on the Internet. Maybe you download some documentation, search for some troubleshooting tips, participate in a community, fetch some dependencies. But being offline does not compromise one's ability to code.

It's happening again. We're [being told the industry will leave us behind](https://tomrenner.com/posts/llm-inevitabilism/) if we don't move *programming itself* to the cloud.

So let's get this straight by doing some basic math.

My monthly costs as a programmer are $0. I use free software to create free software. I can program on the train, I can program on a plane, I can program when my ISP goes down. I never have to trust any third party with my private information, nor trust them to run arbitrary commands on my computer.

Now I switch to agentic coding.

First of all, the costs are difficult to calculate, and that's part of the problem. Same thing with cloud computing - funny how it's the cloud that does the accounting and tells you how much you owe, isn't it?

In this case it's even more suspicious because the company that bills you not only counts how much you owe them, it also controls the agent's behavior in terms of how many requests it tries to make. So they could easily insert into their system prompt something like, "our earnings this quarter are a little short so try to pick strategies when doing agentic coding that end up earning us more API requests, but keep it subtle." There's no oversight. They could even make it target specific companies.

They could also change the model so that it favors trying to solve problems using their own cloud solutions or pushes their marketing agenda, and it would be basically impossible to detect. I'd be surprised if they weren't already doing this.

Anyway let's try to compare one month of programming 8 hours per day. I'm going to estimate [1 prompt per minute](https://www.youtube.com/watch?v=WcpfyZ1yQRA), for a total of 60 \* 8 \* 30 = 14400 prompts in a given month. Based on [Zed's pricing page](https://zed.dev/docs/ai/models), that comes out anywhere from $576/month to $2,880/month to $90,000/month depending on which model you use and whether you enable [burn mode](https://zed.dev/docs/ai/models#burn-mode)/[max mode](https://docs.cursor.com/models#max-mode).

So on one hand we have $0 per day for a total of $0 per month, programming in total privacy and security, while on the other hand we have to gamble and pay rent, while also trusting OpenAI, Google, Microsoft, Amazon, or Meta - all companies which have already proven their untrustworthiness time and time again. Did you read their Terms of Service Agreement before you clicked **"auto-accept all"**? How about the email in your spam folder that says "Updated Terms of Service Agreement"?

Don't be a sucker. Use your own computer.

Thanks for reading my blog post.
