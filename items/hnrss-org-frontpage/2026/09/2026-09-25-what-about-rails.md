---
title: What About Rails?
link: https://jardo.dev/what-about-rails
source: hnrss-org-frontpage
published: 2026-09-25T02:50:16Z
updated: 2026-09-25T02:50:16Z
first_seen: 2026-09-25T13:39:35.685325101Z
authors:
- jrochkind1
summary: 'Article URL: https://jardo.dev/what-about-rails Comments URL: https://news.ycombinator.com/item?id=49839664 Points: 127 # Comments: 75'
content: extracted
html: 2026-09-25-what-about-rails.html
preview:
  file: 2026-09-25-what-about-rails.preview-b4f65ac9412d.webp
  width: 256
  height: 134
  alt: A preview of the page showing a JARDO wordmark, the title of the post ("What About Rails? | Jared Norman"), a short description ("DHH's opening keynote had shockingly little to say about Rails. "), and a funny face
  color: '#e9e1d8'
images:
- source: https://d1jt649tmk2is2.cloudfront.net/?url=https%3A%2F%2Fjardo.dev%2Fog-previews%2Fwhat-about-rails&w=1200&h=630&sig=d345cfdde64b4770fbc9ef64b69d5bec84e26429c4aaa6d4bbde30ec1a19ece8
  original:
    file: 2026-09-25-what-about-rails.image-4320cc0fbb38.jpg
    width: 1200
    height: 630
  color: '#f1ece6'
---

David Heinemeier Hansson is, for better or worse, still in charge of Ruby on Rails. I’d love to stop paying attention to him, but I build applications with Rails, so his actions affect me and my clients. Yesterday, he gave [the opening keynote](https://www.youtube.com/watch?v=vDjW_dRyKXY) at Rails World 2026, where he laid out his vision for the future of Rails.

Or that’s what his talk should have done. His keynote had very little to do with Rails. Here’s what he did talk about, and what it means for Rails.

## The Gist of It

> I have retired from being a professional programmer.

Yes, he said that. No, that doesn’t mean he’s stepping away from software development. He now styles himself a “maker.” He now claims that English is the best programming language (because LLMs) and that we don’t even necessarily need to read the code the LLMs produce.

> Writing code by hand is no longer an economically productive enterprise for the vast majority of programmers working at the vast majority of companies.

He’s all-in on LLM code generation, so he’s changed his stance on both native applications *and* the Rust programming language. In his eyes, products like Hey were never really meant to be web apps.

He’s argued for years that the Rails stack allows small teams to build ambitious products. Now, as 37signals are building the next version of Hey, they are going with a different stack. In his telling, the bottleneck is gone, so they’re using LLMs to build native applications for every platform they support.

On the server side, they are going with Rust. DHH maintains that the language is hideous and that humans shouldn’t be subjected to it, but that it’s great for LLMs. Since he’s not reading the code anyway, he can now appreciate the performance and stability of the language.

He claims to have written 150k lines of code in August of this year, having previously averaged about 30k lines per *year* in the pre-LLM era. (He admits much of it is “verbose” Rust.) While Ruby made up about half his work over the last two decades, it sits at only 3% of what he wrote this year.

The new strategy is rooted in the idea that humans reading code should be the exception, rather than the norm, “like seeing a bug in Sentry.”

> That’s today. By the end of the year, it will be virtually all domains, virtually all programmers, virtually all companies. So we best get used to it.

He also wants to see every service offer a CLI so that he (read: “his agents”) can interact with it without using the UI.

> We can now want everything. We can now get everything.

The tail end of his talk focused on his vision of LLMs enabling everyone to create whatever their hearts desire. He spoke about his work on Omarchy and finished by urging the audience to reject AI skepticism and doomerism:

> The black pill is for fucking losers. Don’t be a loser.

## A Rails-shaped Hole

DHH used the opening keynote of the world’s premier Rails conference to announce that a flagship Rails app was leaving Rails. The Rails content amounted to it still being a great fit for web applications (like Basecamp) *and* being great for building with AI.

For twenty years we’ve been sold Rails as the framework for “small teams, ambitious products”. I’ve been on a ton of teams that were able to do a lot with a little because of Rails. You probably have too.

> Hey was and is a web app because making web apps for small teams was how you could be productive. In the old times, that is, 5 minutes ago…

His vision for Rails has narrowed. Rails wasn’t a preference. It was a workaround. It’s now the platform of choice for “web apps of necessity”. Convention over configuration has been reframed as “token efficiency”. Evil Martians’ agent evals are simply a reassurance; AI is good at Rails, so you don’t need to leave.

There’s a more charitable framing. Rails *is* a mature, stable framework. Stability *is* good for agentic development. But he told us only 3% of his work this year was Ruby. Nothing in this talk attempts to distinguish a mature platform from one whose creator is no longer paying attention.

The CLI demands were baffling. 37signals differentiates their products with opinionated UI/UX, not novel features. They are rewriting Hey as six native apps because the web fidelity isn’t good enough. So UI matters enough to justify complete rewrites, but also everyone just wants CLIs? If every product is used by an agent driving a CLI, what’s going to differentiate Basecamp or Fizzy from the cheapest alternative? I think this strategy needs a Rework.

I’m left wondering what the vision for Rails really is now, and who’s going to drive it. While [Mosscap](https://mosscap.dev/) forked on political grounds, part of their core argument is that Rails is done. It’s stable and needs only maintenance. [Hanami](https://hanakai.org/) has a [roadmap](https://github.com/hanami/roadmap/issues) and a vision for the future of building web applications with Ruby. While much of the day-to-day work on Rails comes from Shopify and elsewhere, DHH historically drove the vision. Now, is he arguing himself out of a business, or has he already left and not told the room?

The creator of Rails is taking one of his flagship products off the stack. His Ruby output has dropped to 3%. He believes hand-written code will be history for virtually everyone by December. In the face of this, he offers flattery.

> Now maybe that’s a little scary. Like maybe we’re gonna get a little competition. Who’s afraid of a little competition? Aren’t you better? Don’t you know more? Of course you do. You’re a fucking Rails programmer. You’re the best of the best. This is goddamn Top Gun I’m looking at here. Embrace that. With gusto.

This is reassurance instead of a plan. I bet it worked in the room too; confidence always does. But it’s totally hollow. You could say the same thing to a room of Django or Laravel or fucking Spring Boot developers *word for word*. The one moment he talked directly to Rails developers, he chose to say *nothing* about Rails.

## The Hallucinated Elephant in the Room

On to the AI claims. For context, DHH runs a company that makes simple, user-friendly products. They’re so simple that even before the advent of LLMs they would periodically fully rewrite their apps to create new versions.

37signals succeeds on product and marketing, not on solving hard technical problems. I’m not hating; lots of people love their apps. I’m just saying that their new Kanban app’s success is going to be driven by product decisions and marketing. Kanban board is not one of the hard problems of computer science.

So does his approach (never looking at the output, evaluating the result from the outside) work? These tools have come a long way. They still make all kinds of mistakes, but as long as there’s a human in the loop to verify the results and reprompt, it works fine, at least for small apps and easy problems.

It’s hard to take the numbers in this talk seriously, because David keeps undermining them. Throughout he presents topics as settled, despite failing to support them coherently.

He admits that lines of code is a poor measure and grants that we can’t compare across languages fairly, then compares 150,000 lines of LLM output in August to his 30k/year average, then immediately concedes that he tolerates Rust code from LLMs that he “would never tolerate from \[his\] Ruby code”. Lines of hand-written, concise Ruby and LLM-generated Rust slop are not comparable. He seems to know this, but compares them anyway.

> In the past 20 months, I have written half as much code as I did in the previous 21 years.

Apples to oranges again, *and* he’s struggling with the definition of “to write”. He didn’t even *read* the Rust his LLM generated.

The Hey Next numbers are similarly problematic. There’s no questioning that Rust is a more performant language than Ruby, but it’s another unfair comparison.

> …we end up with a backend that requires 99% less CPU, 95% less memory, and the only reason it needs 10 hosts is for redundancy. In fact, our back-of-the-envelope calculation has led us to believe that Hey’s peak traffic could probably be served on a single Raspberry Pi.

A pure-Ruby backend with no web frontend would also be vastly cheaper to run than the existing Rails version. Which gains come from Rust and which are from dropping the web app is unknowable. And none of it is an argument for his agent thesis. A team that likes writing Rust could build the same system. But he doesn’t think humans should write Rust.

His claims about the 10x (and 100x and 1000x) programmer are equally suspect. The study in question was measuring the difference in developer *tooling* (not developer productivity) and has been [heavily critiqued](https://publikationen.bibliothek.kit.edu/72699) from a number of angles. The “average of 10x” is just folklore, not even present in the original paper.

Big productivity differences between developers are real. I’ve seen them myself. But somewhere between the paper and the stage we went from 28:1 to 1000:1, and the only place that’s settled is a keynote where only one person has a mic.

Then there’s Basecamp 5. David reports that it resulted in an architecture “like Swiss cheese”. He blamed the models. Unreviewed, uncoordinated contributions will degrade architectures whether they come from agents or people. He argues later in the talk that “the price of repetition has gone to near zero”. Basecamp 5 is what nonzero looks like. We’ve already seen this failure mode in DHH’s own circle. Tobi Lütke recently [lamented](https://fortune.com/2026/09/17/shopify-tobias-lutke-ai-slop-grenades/) that “slop grenades” are a serious hazard when doing heavy agentic development.

David asks us to learn from history, from the ATM story. People feared that ATMs would spell the end of bank tellers. Instead, we got the opposite. There’s a problem with his story, though: all the details are wrong. [The decade](https://www.history.com/this-day-in-history/september-2/first-atm-opens-for-business) is wrong. He references the [wrong](https://en.wikipedia.org/wiki/William_Stanley_Jevons) [economist](https://knowablemagazine.org/article/technology/2018/future-work-will-robots-take-my-job). The [teller numbers](https://conversableeconomist.com/2015/03/03/atms-and-a-rising-number-of-bank-tellers/) are an order of magnitude off. The ending is [already backwards](https://fortune.com/2026/08/08/vanguard-chief-economist-ai-and-jobs-atm-phase/). **Perhaps a human should have double-checked this talk.**

“Never look at the code” and “security, something’s coming, get ready” are fifteen minutes apart in this talk. That’s a hell of a gulf, and there’s no bridge. I’m being told to believe that one company’s nascent effort to (re)build a relatively simple email product extrapolates to “virtually all programmers, virtually all companies, by December.”

Finally, where a strategy should be, there’s a plea for optimism.

> I also think maybe some of \[the concerns\] are a little overstated. I mean, maybe, but probably not. I mean, some of them maybe a little more.

Optimism isn’t a strategy, and it doesn’t override facts. The facts in this talk do not justify the optimism. I tuned in curious to see what’s next for Rails. I still do not know, and I don’t think DHH does either. He didn’t even demonstrate that he’s thinking about it.

That’s what bothers me most. I’m skeptical of his AI claims, but that’s not the real issue here. I’m also not mad about the Hey rewrite. He’s allowed to build his apps with whatever tools he wants. I don’t even use Hey.

The problem is that he stood up at Rails World and told everyone that he was moving his product off Rails and the best thing he could come up with to say to people still using Rails was that we’re “the best of the best.” Thanks, I guess.

Maybe Rails is done, in the way the Mosscap project claims. Maybe it’s time to focus on stability and maintenance. If that’s the plan, someone needs to say it. If it isn’t, then let’s hear about where we’re headed.

DHH did neither. He just told us the future is going to be great and warned against AI doomerism. I’d have settled for a slide or two about Rails.
