---
title: 'Joy & Curiosity #95'
link: https://registerspill.thorstenball.com/p/joy-and-curiosity-95
source: registerspill-thorstenball-com
published: 2026-08-16T07:28:14Z
updated: 2026-08-16T07:28:14Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Thorsten Ball
summary: Interesting & joyful things from the previous week
content: extracted
html: 2026-08-16-joy-curiosity-95.html
preview:
  file: 2026-08-16-joy-curiosity-95.preview-88e9824e37ff.webp
  width: 256
  height: 161
  color: '#a3957d'
images:
- source: https://substack-post-media.s3.amazonaws.com/public/images/a70cd3df-d202-448a-99de-8092844cc13e_1294x812.png
  original:
    file: 2026-08-16-joy-curiosity-95.image-e109f4eb75e3.png
    width: 1294
    height: 812
  color: '#e5d5b6'
- source: https://substackcdn.com/image/fetch/$s_!rU6s!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2c56aaed-2b94-4350-ac27-25b440b6d21f_952x37.tif
  original:
    file: 2026-08-16-joy-curiosity-95.image-9d8e600417e0.png
    width: 952
    height: 37
  variants:
  - file: 2026-08-16-joy-curiosity-95.image-47c06ad04f29.webp
    width: 320
    height: 12
  - file: 2026-08-16-joy-curiosity-95.image-ec3be89aa0f3.webp
    width: 640
    height: 25
  - file: 2026-08-16-joy-curiosity-95.image-5d31a8f06632.webp
    width: 952
    height: 37
  color: '#000000'
- source: https://substackcdn.com/image/fetch/$s_!cNIr!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8fd4508d-83e3-478f-9453-574964dc5796_954x36.tif
  original:
    file: 2026-08-16-joy-curiosity-95.image-2f25f3f3a294.png
    width: 954
    height: 36
  variants:
  - file: 2026-08-16-joy-curiosity-95.image-4545f6e201ab.webp
    width: 320
    height: 12
  - file: 2026-08-16-joy-curiosity-95.image-8c35b68254b0.webp
    width: 640
    height: 24
  - file: 2026-08-16-joy-curiosity-95.image-f5edfda1c30f.webp
    width: 954
    height: 36
  color: '#000000'
---

In the last two weeks I’ve shipped: a new experimental provider backend for our orbs, an in-product bug reporting feature (not released yet) including an admin area where we can triage bugs, disk and memory warnings for orbs, visible setup logs when orbs are starting, a full Comet Busters-like game that’s hidden as an easter egg on our website, a microphone selector for our dictation features, a new work-in-progress page that explains [what orbs are](https://ampcode.com/what-are-orbs) that has a bunch of handwritten text and videos and other stuff I put in there by hand, user preferences for themes, and a few smaller things.

I also fixed around twenty bugs and removed 5k lines of code that we no longer need.

“We get it, man, you shippe—”

Nah, nah, nah! Not the point. The point is this:

I have not used my local development environment for any of this. I’ve done all of this remotely, using Amp, in orbs. Everything! Backend for remote machines; messages sent across three services to warn about system resources; landingpage. The freaking game is probably the least surprising thing here, isn’t it? And it’s a game with custom assets!

Isn’t this wild? No, I know, it is, that’s what I’m saying.

“Surely some things you want to check or test locally, no?” Nah, not really. I mean, yes, that’s probably what I would’ve said half a year ago if you’d told me I won’t need my local dev setup anymore.

Turns out that, no, you don’t. You can just ask the agent to give you “irrefutable proof” that something works and if you have an orb and it can do whatever it wants and install whatever it needs it will find a way [to give you that proof.](https://x.com/thorstenball/status/2088601032129380490) Orbs are malleable, the agent can shape them to fit the task by installing and running whatever it needs and and then you get a bespoke made-for-exactly-this-task machine in which an agent can go crazy and if you ask it it will give you a presentation or a narrated video in which it shows by — frame-by-frame, man! — that the race condition has been fixed.

And then, what else do you need your local dev env for? Editing code by hand? Come on, man. Reviewing code deeply? Amp has a diff viewer, so you don’t need to do that locally either. And for all of the things I shipped here, I didn’t review each line anyway. I do spot checks and make sure the architecture is right, yes, but do I need local tools for that? No. You can ask the agent to help you with reviewing by quizzing you, by giving you diagrams, by showing you a presentation.

What about the fiddly things? The things you do want to feel your way towards, with your hands? Little bit of padding here, some margin there; now let me flip these two paragraphs and— ah yes, better. That kind of stuff? That’s actually where I’m now experimenting the most because I do have this need to flip words and paragraphs and move stuff around. I want to look at it, change something, look again; undo, redo, change, undo, and back around again.

But here too the game has changed in a way I find marvelous. Because you can just dictation-dump all your ideas to the agent and hand it screenshots and assets and raw notes and snippets and then ask it to provide you with example pages and 15 different variations of the widget you’re interested in, and then you can tweak those and say “this one’s good, let’s use this one” and you feel like you’re the head chef strolling through the kitchen, spoon in hand, tasting the soup over here, tasting the dessert over there, saying “nah” or “mmmmh, good” or “into the trash”, and your headless and faceless and bodyless sous-chefs don’t mind at all and just do what you say and try again.

Then weeks go by and you notice you haven’t git pulled in a long time and every time you do end up doing that again (due to nostalgia?) maybe use more than one checkout, you notice that it starts to feel… *yucky?* *dirty? unclean?*

Wild times. Exciting times. The models are there now. And if you doubt that, just wait a couple months.

[![](https://substackcdn.com/image/fetch/$s_!rU6s!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2c56aaed-2b94-4350-ac27-25b440b6d21f_952x37.tif)](https://substackcdn.com/image/fetch/$s_!rU6s!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2c56aaed-2b94-4350-ac27-25b440b6d21f_952x37.tif)

- [New episode of Raising An Agent is out!](https://ampcode.com/podcast/season-02/episode-02) We recorded this one in-person, in Munich, and talked about everything that was on our mind last week (and this week): orbs, jellyware, why AI by itself doesn’t lead to slop, how you need to rethink software now, and, maybe most importantly, the mind-blowing realization of that week in Munich, that no one cares about their local dev env anymore. We all had to wipe our laptops four weeks ago and people said they still haven’t ported their dotfiles over and at this point don’t care anymore.

- Speaking of which: I recorded a short video on [why orbs aren’t “just VMs” and why saying “orbs are just VMs” is missing the mark](https://x.com/thorstenball/status/2087554456971882650) , just like saying “the cloud is just another person’s computer”. Also: woo boy, some people are really bothered by product names? Well, too late. It’s orbin’ time. I get emails from customers telling me they want to get their team “into orbit”, others signing off with “happy orbin’!”, and customers greeting us in Slack channels with “I love me some orbin’ in the mornin’.”

- My teammate Will wrote about [how we can push straight to main and still have SOC2](https://ampcode.com/notes/thats-not-soc-2-compliant) . One of the most asked questions we got in the last few months: “Wait, you don’t use pull requests? How do you have SOC2 then?” Turns out that SOC2 doesn’t require PRs.

- [Very short video in which I show off how I iterate with agents in orbs](https://ampcode.com/time-capsules/3) , working on landingpages and making visual changes, something.

- [Stolen Thoughts - Stealing Reasoning Traces from Proprietary LLM APIs](https://stolen-thoughts.com/) . First of all: wow, what a name, what a website. And then, of course, this is fascinating, isn’t it? But I’m not sure whether it’s much more than that.

- Wired also has a write-up on it: [A New Trick Reveals AI Models’ Inner Thoughts](https://www.wired.com/story/a-new-trick-reveals-ai-models-inner-thoughts/) .

- Read [Austin Kleon’s Don’t Call It Art](https://austinkleon.com/dont-call-it-art/) . Lovely, as expected. If you’re in any way interesting in *making things* or *building* or *writing* or just … doing stuff on the Internet: get all of his books. They’re very short but very good and very inspiring.

- It’s been a while since I’ve wanted to access to something this badly: “Cerebras powers GPT-5.6 Sol on Ultrafast mode, [delivering up to 750 output tokens per second.](https://www.cerebras.ai/blog/accelerating-gpt-5-6-sol-ultrafast-with-openai) ” Hey, tell the kids to cup their ears real quick. *Motherfucking seven hundred and fifty tokens per second*. *Fucking hell!* If there’s a sweet angel at OpenAI reading this and can give me access: I will fly to San Francisco and hold your hands and kiss your forehand before I kneel down to thank you and to bless your family and the house they live in and the ground they walk on.

- [Zuckerberg weighs in](https://www.meta.com/thefutureisforeveryone/) : “I do not understand why anyone who believes that AI will eliminate most jobs and much of humanity's relevance would rush to build that future.” (No, I have not read the whole thing.)

- [The hardest working font in Manhattan](https://aresluna.org/the-hardest-working-font-in-manhattan/) . This was long, but soooo good. *So good*. On a spectrum from “doesn’t care about to fonts” on the left to “writes a long and deeply researched article about the history of an unknown font” I’m slightly to the right of center, but I read the whole thing and think you should too if you ever thought “that’s a neat font.” (Except if that font was Papyrus, of course.)

- Nail it to the walls: [There are no lossless transformations of natural-language text](https://sophiebits.com/2026/06/25/there-are-no-lossless-transformations-of-natural-language-text) . Very, very, very good. (Sidenote: can you imagine working at a 1000 people org and people use AI to generate Slack messages, emails, PRDs, and slide shows? Yup. Horror stories between two parens.)

- Craig Mod: [A Swarm of Blood Robots](https://craigmod.com/essays/robot_blood/) . Insert the usual adjectives that I use when talking about Craig Mod’s writing: excellent, fantastic, lovely, beautiful. They all apply here too. There are so many things I want to quote here: the section about writing with LLMs, the part about the Weirdness, some of his example projects, the end about the usefulness of these tools. But instead let me just share this one observation: maybe my views on the future of software are so aligned with Craig’s (if you go back and read the last twenty issues of this newsletter you’ll find that my thoughts on liquid software, jellyware, the future of software, etc. match what he’s describing here) because Craig is *not* part of the software industry and he’s *not* huffing and puffing about how things aren’t done properly and he’s *not* stomping his feet about these models being bad at X and Y and he’s *not* stuck in a ten-year old world view of how software’s supposed to be built and instead he just has a ton of ideas for things to build and leans into seeing what these models can do and then goes and does it.

- [Sudo Aquarelle](https://sudoaquarelle.com/) , a watercolor simulator. So nice.

- Finally an end to this stupid argument: [“Code was never the hard part” is an insult to all programmers](https://blog.senko.net/code-was-never-the-hard-part-is-an-insult-to-all-programmers) .

- [There is No “Done”: Reflections on a Completed AT Thru-Hike](https://thetrek.co/appalachian-trail/there-is-no-done-reflections-on-a-completed-at-thru-hike/) . This was great, saying that as someone who’s dreamt of walking the AT since he read [A Walk in the Woods](https://en.wikipedia.org/wiki/A_Walk_in_the_Woods_\(book\)) many, many years ago.

- I didn’t know that the [Apple TV has color calibration via iPhone](https://x.com/cypher_ai1/status/2086107459937321123?s=46) .

- One of the most beautiful things I’ve come across this week: [Ordinary Abundance](https://ordinaryabundance.com/) . “All the items in this room were once out of reach; some not yet invented, others too rare or costly for the vast majority of people. Today, most of us lucky enough to live with them walk past without a second thought.” We’d all probably do well by scrolling through it once a week.

- Are you a hardcore Rust engineer and want to work remotely with a small and equally hardcore team and do systems- and infrastructure programming? [Look no further](https://x.com/nathanflurry/status/2087625596797083966?s=46) . I highly recommend working with Nathan and Nick.

- [The Antithesis Principle](https://shreyasdoshi.substack.com/p/the-antithesis-principle) . This was fascinating. I failed to apply it to every example and got different answers, which makes me think that either (a) the principle is not that clearly defined (possible) or (more likely) that (b) my brain’s not wired in this way and I could probably benefit from rewiring it a bit.

- [OpenAI has a friction@ email address](https://fortune.com/2026/08/11/openai-employees-email-friction-address-to-eliminate-bureaucratic-bottlenecks-sam-altman/) employees can use if they feel like they’re being blocked.

- My wife and I were talking about Dolly Parton this week and I said, “Have you ever seen her when she was younger? Or heard her talk?” She said, “No, I haven’t.” I immediately pulled out my phone and [showed her this video](https://www.youtube.com/watch?v=LtV0IpIXRHM) .

- Reminds me: I fell off the wagon again and have watched this video five times in the last 24hrs and I’m about to watch it again, so here, you watch it too. It’s only one of the greatest things ever recorded. [Danny Carey performing Pneuma](https://www.youtube.com/watch?v=FssULNGSZIA) .

- “ [My dad used to tell me](https://x.com/pschofie79/status/1670123694633435137) that you could yell at a bear and it would go away. Camping when I was 10, a bear came into the site. Dad got out of the tent and yelled at it, and it just snorted back at him. Dad got back into the tent. ‘That’s all I got.’ A lot of life is like this.”

[![](https://substackcdn.com/image/fetch/$s_!cNIr!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8fd4508d-83e3-478f-9453-574964dc5796_954x36.tif)](https://substackcdn.com/image/fetch/$s_!cNIr!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8fd4508d-83e3-478f-9453-574964dc5796_954x36.tif)

No posts
