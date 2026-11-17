---
title: Building Visual Intelligence — Andreas Blattmann & Dustin Podell, Black Forest Labs
link: https://latent.space/p/bfl/
source: latent-space
published: 2026-11-16T21:38:03Z
updated: 2026-11-16T21:38:03Z
first_seen: 2026-11-17T01:00:47.489879523Z
authors:
- swyx .ai
summary: Black Forest Labs’ case for learning AGI from pixels, motion and sound.
content: extracted
html: 2026-11-16-building-visual-intelligence-andreas-blattmann-dustin.html
remote_preview:
  url: https://i.ytimg.com/vi/8cGfJOU3AlQ/maxresdefault.jpg
  alt: Building Visual Intelligence — Andreas Blattmann & Dustin Podell, Black Forest Labs
extra:
  audio_type: audio/mpeg
  audio_url: https://podcasts.beehiiv.com/3967c1f2-cb97-4eb0-a5c0-21a1d2579408/01a15fdf-0e93-7e1a-880d-d73752a1f915/01a1e632-c02d-7e1d-84cb-b3d9453f63eb/01a1e632-c136-7f19-ba30-c3bc167811bd.mp3
  duration_seconds: 5903
  thumbnail: https://i.ytimg.com/vi/8cGfJOU3AlQ/maxresdefault.jpg
---

Black Forest Labs’ case for learning AGI from pixels, motion and sound.

Not many people know that Stable Diffusion is why I went fulltime into AI (I aptly titled [Multiverse, not Metaverse](https://latent.space/p/multiverse-not-metaverse/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) to call out Zuck’s mistaken investment). It turns out that **Dustin Podell** also had the same aha moment - quitting his day job when the model came out. But we took very different paths… I stuck to what I knew in human content creation, and Dustin immersed deep into AI content creation with **Andreas Blattmann**, co-inventor of Latent Diffusion and the rest of the world-leading imagegen crew at Stability AI. Stability fell apart, but [Anj Midha](https://www.youtube.com/watch?v=dCX4PE2HxMs&t=1s&utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) helped the core group spin out to BFL, which immediately built an incredible new imagegen model in the Flux series. **But why are they now going into video and world models?**

Andreas gives an example you can try with one hand. If somebody tosses you a ball, your hand starts moving before the ball reaches you. You can do that without being able to write down the trajectory. When he talks about building **visual intelligence**, that’s the kind of knowledge he means. A still can show you where the ball is. A video shows you how it got there. The sound of impact can tell you when it hit something, even if the camera missed the contact. That’s BFL’s argument for training FLUX 3 on the three signals together. [BFL’s FLUX 3 release](https://bfl.ai/blog/flux-3?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv)

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/349354cd-819f-4893-a1cf-c7bae8662f05/flux3-strip.jpg?t=1794863695)

*The horse in motion is one of the FLUX 3 examples we show in the intro. The audio examples are worth playing with the sound on.* [FLUX 3 Video](https://bfl.ai/blog/flux-3-video?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv)

## What a moving camera teaches you

Suppose the camera starts in front of a person and pans around them. At some point the model owes you the back of their head. Andreas thinks ordinary two-dimensional video contains a great deal of evidence about what objects look like from other angles. That doesn’t settle whether a model “understands physics,” a point we go back and forth on. It does give you a much better test than asking whether one frame looks convincing.

There’s a good engineering example late in the episode. Sharpen each frame of a clip independently and each frame might look terrific. Hit play and the extra detail can shimmer, because the upscaler made a slightly different decision every time. BFL derives its video upscaler from the same FLUX 3 backbone. Temporal consistency matters even when all the user asked for was a higher-resolution file.

## What’s worth spending training compute on?

One of the questions I had for them was whether autoregression was taking over image generation. Andreas gives the other side its due. An autoregressive model can learn from all the tokens in a training example at once. On the diffusion side, the team can distill a long sampling process down to a handful of steps. Which advantage matters depends on whether you’re paying to train the model or waiting for an image to finish.

Dustin’s early Kontext experiment is a nice example of how they decide where to invest. He suspected an image model that could *draw* a dog wearing a hat already knew something about where hats go. He and a colleague collected a small set of photos labeled with relationships. A training run of a few hours got the model to put a logo on a shirt. It was rough, but it was enough to take to the next offsite. The team then spent months on the data and the model. [FLUX Kontext examples](https://bfl.ai/models/flux-kontext?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv)

My favorite user story comes near the end. People were feeding FLUX old, degraded family photos, restoring them, then showing the pictures to their families. Dustin says he hadn’t predicted that use. After talking about preserving somebody’s identity across edits and across frames, it’s a lovely example of who actually notices if you get it right.
