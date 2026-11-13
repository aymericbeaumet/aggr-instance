---
title: How Not to Build a Music Player
link: https://alichraghi.github.io/blog/how-not-to-build-a-music-player
source: lobste-rs
published: 2026-11-13T09:16:28Z
updated: 2026-11-13T09:16:28Z
first_seen: 2026-11-13T21:55:15.639657374Z
authors:
- alichraghi.github.io by alichraghi
labels:
- person
- zig
summary: Comments
content: extracted
html: 2026-11-13-how-not-to-build-a-music-player.html
remote_preview:
  url: https://alichraghi.github.io/blog/how-not-to-build-a-music-player/public-release.avif
  alt: An early dark-themed AMP interface showing a music library and synchronized lyrics.
---

The [Twelve-Day War](https://en.wikipedia.org/wiki/Twelve-Day_War) made me realize online music players aren’t a viable option for those who live under an oppressive regime that [disconnects the internet](https://en.wikipedia.org/wiki/2026_Internet_blackout_in_Iran) across the country as a way to silence public opinion.

So, as soon as the connection came back, I started archiving tracks and looking for offline alternatives and discovered [Strawberry](https://www.strawberrymusicplayer.org/). It’s a feature-rich music player and kept me happy for quite a while, but I have this habit of looking at song lyrics and singing along! Strawberry fetches lyrics from the internet when it recognizes a song, but sometimes the lyrics weren’t line-synced and worse, it displayed the timestamps next to the lyrics which I found pretty annoying to look at. I wasn’t aware of other music players such as [foobar2000](https://www.foobar2000.org/) which my friend just pointed me to while writing this post.

I decided to write my own music player and call it [AMP](https://codeberg.org/alichraghi/amp) with a few goals in mind: minimal feature-set, no C++ dependencies, proper system integration to make it feel like a native app, and of course, write it in Zig (including the shaders!). I also wanted to roll my own UI because of the fun memories of working on [V’s UI](https://github.com/vlang/ui) during my first days of being introduced into system programming. But before I could make any meaningful progress, a new wave of protests started. Following the tragedy of [January 8 and 9](https://en.wikipedia.org/wiki/2025%E2%80%932026_Iranian_protests), another internet blackout began, lasting more than four months.

During the blackout, there were these anonymous people on Telegram selling internet access through personal [White SIMs](https://en.wikipedia.org/wiki/White_SIM_Card), whitelisted servers, Starlink dishes, etc. A black market had formed for the freaking internet! You’d get charged $5 or more for just 1GB of traffic. The prices dropped slightly after a while, but it was still hella expensive. The regime surely made a lot of money out of it. I ended up spending hundreds of dollars on these services and other methods just to stay in touch with my friends and keep my hobbies alive. Killing time was becoming a problem. Especially for someone like me who built their entire social life online.

I carefully used this slow connection to download repositories, documention, etc. to continue working on my music player. It really brought me immense joy during this hard time.

![An early dark-themed AMP interface showing a music library and synchronized lyrics.](https://alichraghi.github.io/blog/how-not-to-build-a-music-player/public-release.avif)\
Screenshot from 2026/5/25

Once the connection was mostly restored to the pre-blackout days (UDP traffic is still blocked, though, which naturally also affects QUIC), my attention was shifted towards LLMs. It had become very hard for me to ignore the hype and progress around frontier LLMs. As I really never thought about using them seriously for coding. They are still everywhere in the news, and I was just reminded that I owned a *Claude Max 20x* subscription gift from my boss back in 2025. Eating dust and maybe soon expiring. So I decided to give it a try and experiment on my music player. It was a very addictive experience. I literally couldn’t hit the plan limits no matter how hard I tried, so after a few non-stop days of prompting, a good chunk of [AMP](https://codeberg.org/alichraghi/amp) was vibecoded..

At the time, I thought this was an okay approach for a hobby project and I could just un-slopify it later. Little did I know I’m mentally incapable of reviewing tens of thousands lines of code. Shortly after, Anthropic figured that I live in Iran and instantly banned my account. Then Codeberg announced its [no-AI policy](https://blog.codeberg.org/protecting-our-floss-commons-from-llms.html), which gave me two choices. Either get rid of all that slop or move the repository elsewhere. Going with the former option, I started reading every single line of code, rewriting them. I was surprised by my own ignorance and how much LLMs like to complicate stuff. (I wish I could show an example, but my memory is extremely fragile and I’ve got no pre-rewrite history of the repo to check).

Four months later, nearly half the codebase is gone, and somehow it’s more robust with the same set of features (and more). I regret relying on LLMs in the first place, but hey, lesson learned. That said I think it’s still possible to create a healthy relationship with LLMs as long as you look at them as a fancy IDE feature. A lot of times they are just running long sed or regex commands that are a no-brainer act for humans when done by hand but still consumes a good time. For now and hopefully forever I think I’ll be going the old school way. Never rush your hobbies!

Btw this is how [AMP](https://codeberg.org/alichraghi/amp) looks like right now!

![AMP v0.1 showing a music library, playback controls, and lyrics.](https://alichraghi.github.io/blog/how-not-to-build-a-music-player/v0.1-release.avif)\
v0.1

Worth mentioning that the blur shader is written in Zig based on [this excellent blog post!](https://fgiesen.wordpress.com/2012/07/30/fast-blurs-1/).

Roadmap to v1.0:

- Music server
- Android and Windows support
- Packaging for major Linux distros
- [Any suggestions?](https://codeberg.org/alichraghi/amp/issues/new)
