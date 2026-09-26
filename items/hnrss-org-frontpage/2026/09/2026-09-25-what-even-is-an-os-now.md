---
title: What even is an OS now?
link: https://sockpuppet.org/blog/2026/09/25/what-even-is-an-os-now/
source: hnrss-org-frontpage
published: 2026-09-25T21:36:56Z
updated: 2026-09-25T21:36:56Z
first_seen: 2026-09-26T05:24:01.732658731Z
authors:
- fratellobigio
summary: 'Article URL: https://sockpuppet.org/blog/2026/09/25/what-even-is-an-os-now/ Comments URL: https://news.ycombinator.com/item?id=49850305 Points: 136 # Comments: 228'
content: extracted
html: 2026-09-25-what-even-is-an-os-now.html
preview:
  file: 2026-09-25-what-even-is-an-os-now.preview-982a8f7fc596.webp
  width: 256
  height: 134
  alt: A Final Ward logo
  color: '#e7e4dd'
images:
- source: https://sockpuppet.org/images/og-default.png
  original:
    file: 2026-09-25-what-even-is-an-os-now.image-b82abb52146e.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-25-what-even-is-an-os-now.image-c87f043c8037.webp
    width: 1200
    height: 630
  color: '#fefefe'
---

**Crazy as it sounds to say this, since it’s all anybody’s been able to talk about for over a year, but the impact of AI on computing hasn’t yet sunk in.**

Today I’m parting company with Fly.io to work with Kurt on a new project. I’m getting that out of the way so you all know up front I’m talking my book.

When I was a little kid, a family friend sold us our first “computer” — scare quotes because I’m pretty sure it was a VTech Laser 200 clone, a chiclet keyboard Z80 that ran off cassette tapes and plugged into our TV. I was old enough to know what a computer was, and conceptually what it meant to program one, because I remember dreaming of all the dumb games I could make. Finally! We had a device that would allow me to make things like the “Hitchhiker’s Guide” text adventure I’d play at my dad’s office.

But it booted straight into BASIC. That’s all it did. I was, like, 8 years old. I wasn’t about to learn BASIC. What I learned instead: computers were not as awesome as I’d imagined.

By my mid-teens, computer security finally dragged me into programming. Coding only reinforced that crummy lesson. The more I understood how programs were constructed, the more forbidding it sounded to build an entire complicated application. I could belt out fast network servers no problem, but a word processor? You might as well have asked me to build a diesel locomotive.

Suddenly, everything has changed. Computers now work the way I thought they did when I was 8.

I’ve been writing these past several months about all the native macOS apps I’ve been churning out, and how weird it is to watch AI dissolve the boundaries between backend programming and frontend, between web and native, systems and applications. All huge stories; any one of them would land in my career top 10 big shifts.

But AI is knocking down a much more important boundary: the one between programmers and users. My Mac menu bar and task switcher are cluttered with icons for programs I conjured for myself, with English as my programming language.

What I did on my computer, any power user can do on theirs.

Will do on theirs.

It’s fucking inevitable.

❦

That’s a statement about the reality; a “prediction”, if you want a concession to your skepticism about AI, but only in the sense that I can predict that the sun will rise in the east tomorrow. So what do you do with that?

I think we’re all so bumfuzzled by the first-order impacts of AI that we haven’t considered the second-order effects.

Consider: what does computing look like in a world where many (maybe most) applications have an audience of just 1-2 people? How is software distributed? What does it run on?

When I was younger, I learned that programs were normally something you bought off the shelves at Egghead Software. Then I watched the shelves at Egghead become obsolete. Then boxed software altogether. Then software shipped on CD-ROMs — you know that’s why we call it “shipping”, right? we used to have to arrange shipping! — wiped away by the Internet. Now I think I’m about to see something else start to wash away: the entire concept of prefabricated fixed-function applications.

Here’s what’s about to happen: users will increasingly interact with stuff they themselves conjured into being. Them, or people they’re on a first-name basis with. Programs will solve their problems specifically:

- How can I get an accurate weather forecast for Roscoe Village in Chicago when the nearest weather stations are attached to O’Hare and miss the lake effect?

- Forget the map, just tell me if I should get off the Eisenhower Expressway and take Roosevelt, Madison, or Lake back home?

- Is there a meeting happening anywhere right this minute that I’m supposed to be in?

You can go on and on like this. These all sound trivial. That’s the point. They’re normal-life problems nobody would have built serious programs around, because the audience sounds too small.

Today, almost all software comes from expert strangers. But soon, strangers will stop supplying our apps, and instead ship just their building blocks. Sure, there will still be megaproject browsers and word processors. But there’ll be thousands of times more applications that pull in 1/7th of the guts of a word processor to solve some idiosyncratic work or home life problem for somebody who doesn’t know what a for-loop is.

You can disagree with me about all of this, but if you understand where I’m coming from, it’s all just straightforwardly obvious how weird this is going to make everything.

Why do we have modern operating systems? It’s not simply to “provide access to hardware resources”; plenty of embedded systems do that with runtimes that have no business calling themselves an “OS”. No, the core purpose of a modern operating system is to partition different applications off from each other, and carefully control how they can communicate.

That makes a lot of sense in a world where we’re importing all our software from strangers. It makes less sense in the world we’re heading to, where most of the software we’re carving up fiefdoms for has the same provenance. And it makes almost no sense in a world where every application is malleable, subject to growing new limbs at any moment on the whims of its creator.

❦

That’s what we’re working on: a platform that is the device we would want to have in the world I just described. So: we’re building a phone.

Every mainstream phone on the market in 2026 was planned (torturously, to death) in 2023. That’s how product cycles work. In 2023, computers were still devices designed to run deterministic fixed-function applications produced by a priesthood of professional programmers. And software was something users bought, not built.

The soul of those phones is stuck in the 1970s. Dudes with big mustaches nailed down the design while hacking on VT52 terminals with Boston’s “More Than A Feeling” blaring on the FM radio perched on their workbenches. Since then, all we’ve done is scale and miniaturize that design so it fits in your pocket. It’s beautifully machined and sports a zillion pixels and does the same job as a DEC PDP-11: running prefab applications.

We’re building a phone that isn’t designed to run fixed-function applications. I doubt anyone will ever get up on a stage and show off a AAA mobile game (that nobody plays) on it. It’s not for TikTok and short-form video, though I guess you can ask it for that. You can ask it for whatever: it’ll build apps for you, on the phone, whenever you want.

I know what you’re probably thinking about all this. That’s OK: that means you’re a reasonable person. We’re betting that AI is about to do unreasonable things.

There’s a long history of grand statements about the world attached to “and here’s my new company” buttons. It never goes well. I’m aware that no matter what I write here, I’m going to sound like yet another startup nerd talking about “the future of programming” before announcing that I’ve joined, like, Sri Lanka’s answer to Uber or something.

I have a lot to say that I’m not saying now. Not because it’s secret, but to get the “new startup announcement” stink off the technical details.

The last year in the technology industry has felt like 100 years all happening at once. Our industry is destabilized in a way nobody’s experienced since the advent of the personal computer. Every limit AI runs up against collapses within a month. Everything we do with frontier models today, in a few years we’ll be doing instantaneously and for free.

When we were kids, we thought computers could do anything, if we could just get our hands on them. We had to become professional software developers to learn how much computers suck.

Now we all have to unlearn that stuff, and think like kids again. So, here we go.
