---
title: Apple broke an essential presentation feature of iPads
link: https://teddysvoronos.com/2026-10-19-apple-broke-an-essential-presentation-feature-of-ipads/
source: hnrss-org
published: 2026-10-20T12:17:28Z
updated: 2026-10-20T12:17:28Z
first_seen: 2026-10-20T17:55:58.869157456Z
authors:
- evanb
content: extracted
html: 2026-10-20-apple-broke-an-essential-presentation-feature-of-ipads.html
remote_preview:
  url: https://teddysvoronos.com/assets/img/headshot-nav.jpg
---

A brief digression from my usual teaching posts to complain about a devastating regression in iPadOS 27.

If you’ve followed my work up until now, you’ll know that I rely heavily on writing on handouts from my iPad, [projected via AirPlay to an external screen](https://teddysvoronos.com/2015-11-19-in-praise-of-goodnotes/) that my students can see. When I’m doing this, my iPad is also doing many other things: [controlling a PowerPoint presentation on my Mac](https://teddysvoronos.com/2017-11-26-my-setup/), showing me notes that I’ve taken on points I want to mention and students I want to call on, and showing me the results of polls before I reveal them to the class. This ability to do other things on your iPad while the audience just sees your presentation or handout is an essential part of a variety of pedagogical moves that I use in my teaching.

Unfortunately, Apple just completely broke it.

## The problem

As of iPadOS 27, if you have multiple windows open while AirPlaying (whether or not Stage Manager is on), tapping on anything other than the presenting app will snap the external display into showing your whole iPad screen. This means that my students see all my notes on who I want to call on, or poll results that should be hidden from them. They also get distracted from what we’re learning by all these jarring windows popping up and disappearing. If you’re presenting at a big conference, it’s extremely embarrassing.

In GoodNotes, the presenter view will return once you tap back on the app. In Keynote, it will just exit the presentation entirely. In PowerPoint, it will put the app in a limbo where you have to restart the presentation to proceed. [Here’s a Reddit post illustrating the behavior in GoodNotes](https://www.reddit.com/r/iPadOS/comments/1wc7sa2/ipad_os_27_rc_still_breaks_presentation_mode_in/). But don’t just take my word for it; try it at home!

I *sort of* understand why Apple did this. In iPadOS 26, which app could “control” the external display was hard to identify. Essentially, you had to swipe up as though going to the home screen, then tap on the app you wanted in the dock, and that app would take over the external display if it had a presentation mode. This was weird and hard to understand, but Apple’s solution is to just make it so that whatever is the most recently tapped app takes over, which is much, much worse.

## Ask what you can do

As for what the behavior should be, I’m not a developer, but here’s a proposal: if you’re using an app that is in presenter mode (e.g., PowerPoint playing a presentation, or a GoodNotes document open), it should *always* get precedence on the external display, until that presentation gets stopped, the document is closed, or the app is quit. Period.

I talked/griped/whined/begged about this on an episode of Mac Power Users in early September ([here’s a link to the video](https://youtu.be/CGZ2EhB48SY?t=453)), and as of now this behavior hasn’t changed, including in the latest betas. I’m hoping that this post can get the word out, or that maybe someone has figured out a workaround. I’ve also filed a feedback report with Apple and encourage you to do the same ([click here](https://www.apple.com/feedback/ipad/) and mention my feedback code FB23615646).

I’ll update this post if anything changes, but for now, I can’t recommend this operating system to anyone using their iPad to teach or present.
