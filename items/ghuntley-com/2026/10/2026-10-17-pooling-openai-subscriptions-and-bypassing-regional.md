---
title: pooling OpenAI subscriptions and bypassing regional inference capacity limits
link: https://ghuntley.com/underclass/
source: ghuntley-com
published: 2026-10-17T08:01:21Z
updated: 2026-10-17T08:01:21Z
first_seen: 2026-10-17T09:13:01.651988366Z
authors:
- Geoffrey Huntley
labels:
- ai
summary: by now, most people should be aware that we're in a cost of compute crisis. The cost of CPU and memory is through the roof. There just isn't enough. Meanwhile, there definitely aren't enough GPUs in the world. If you've been using
content: extracted
html: 2026-10-17-pooling-openai-subscriptions-and-bypassing-regional.html
remote_preview:
  url: https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/2026/10/Traditional-tattoo-art-print-from-a-bird---s-eye-view--depicting-pooled-OpenAI-subscriptions-as-a-collaborative-network-overcoming-regional-inference-limits.-Interwoven-streams-of-glowing-data-form-a-swirling-ornamental-tapestry--with-each-.jpg
  alt: pooling OpenAI subscriptions and bypassing regional inference capacity limits
---

by now, most people should be aware that we're in a cost of compute crisis. The cost of CPU and memory is through the roof. There just isn't enough. Meanwhile, there definitely aren't enough GPUs in the world.

If you've been using ChatGPT lately, you might have seen the meme circulating in the space that OpenAI just doesn't have enough inferencing capacity. Heck, even Tibo admits publicly that they're having compute issues.

> It’s the compute that is cooked
>
> — Tibo (@thsottiaux) [October 6, 2026](https://x.com/thsottiaux/status/2107586339860631980?ref_src=twsrc%5Etfw&ref=ghuntley.com)

In the last couple of weeks, I have lost so much time babysitting agents because of inferencing capacity issues, and it's got me wondering: what if inferencing capacity constraints were region-based? Last year, before this crunch, even with Anthropic models, inferencing was noticeably faster in Australia because everyone else in America was sleeping.

Day to day, I do all my open-source software development on bare-metal machines in Germany, and I've started to notice similar patterns when Europe wakes up: all of a sudden, inferencing performance plummets and I'm being greeted with walls of 'model is at capacity, try again' errors.

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/2026/10/CleanShot-2026-10-15-at-6.45.19-PM.gif)

Above is what I woke up to this morning. Imagine my dismay at losing time on a strict get-overalls-done-and-released-before-my-conference-talk timeframe. You can complain online all you want, but we're software engineers. We can solve these problems.

## underclass

The first piece of the puzzle is a piece of software that I wrote last month called Underclass. Underclass pools many ChatGPT subscriptions together and works as a proxy. It can automatically claim resets and correctly handles the prompt cache key to ensure usage isn't wasted.

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/size/w2400/2026/10/CleanShot-2026-10-17-at-6.20.12-PM@2x.png)

I haven't written about it before, but it is now a month old, and I consider it stable. It's available on GitHub, and I use it with six ChatGPT 500 accounts, but there's no real upper limit on how many accounts you can configure.

[GitHub - ghuntley/underclass: underclass: an OpenAI-compatible pooling proxy that pins sessions to one account (prompt cache stays warm), cools quota-exhausted subscriptions until their window resets, and fails fast with the earliest Retry-After when the whole pool is dry.](https://github.com/ghuntley/underclass?ref=ghuntley.com)

underclass: an OpenAI-compatible pooling proxy that pins sessions to one account (prompt cache stays warm), cools quota-exhausted subscriptions until their window resets, and fails fast with the ea…

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/icon/favicon-aea76f9c-44c9-49d3-a1a5-a9a9ba062525.png)GitHub

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/thumbnail/underclass-f93661a6-b274-450d-b3b6-20a3ef696782.png)

Installation is easy, and it's well documented. It's also available as a Nix Flake.

## preflight

Another piece of software I wrote last month is a local LLM proxy that scans requests and attachments for secrets and proactively scrubs and redacts them before they hit the model. It's on GitHub (below)

[GitHub - ghuntley/preflight: A local proxy that scans LLM requests and attachments for secrets before they reach the model.](https://github.com/ghuntley/preflight?ref=ghuntley.com)

A local proxy that scans LLM requests and attachments for secrets before they reach the model. - ghuntley/preflight

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/icon/favicon-0b2813e7-01d6-4378-8639-e44757c71541.png)GitHub

![](https://opengraph.githubassets.com/5ddbfa2cc8cf8a0f2809a19284b5243fbb8b9e37ee29f7507467413916460ac9/ghuntley/preflight)

To use Preflight, run a single instance of Preflight and one instance of Underclass, then point Underclass to use Preflight. Preflight scrubs requests and sends them out for inference, ensuring sensitive material never reaches the inference providers.

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/2026/10/CleanShot-2026-10-17-at-6.28.31-PM@2x.png)

## unburying the lede

By now you're probably wondering either, "Wow, that's two really cool projects," or why I opened with geographic routing. It's because... today I'm releasing a new *experimental prototype* called `geoclass` which routes your requests to different inferencing regions around the world. Like underclass and preflight, it is composable. You can use all of them, none of them, or any combination you can imagine, since they are separate pieces of software that compose well together.

How it works is simple.

Set up a Tailscale network and deploy virtual machines around the world, then tag those virtual machines with a Tailscale tag; GeoClass can then identify these machines via the tag. From that point forward, all you need to do is point UnderClass to GeoClass, and the rest just works (in theory!).

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/size/w2400/2026/10/Screenshot-2026-10-17-at-6.55.27---PM.png)

I don't know *yet* if `geoclass` will solve my inferencing woes, but there's also no way in hell I'm going to sit here and stew when the performance of OpenAI in Europe has been miserable over the last 30 days.

So I'm going to try experimenting with geo-routing. If the below works for you, let me know; I'm keen to catch up with folks thinking along the same wavelength. You never know until you try!

[GitHub - ghuntley/geoclass: \[experimental\] Geo-aware egress router for underclass: retries OpenAI capacity errors in another region over a Tailscale mesh, keeping prompt caches warm](https://github.com/ghuntley/geoclass?ref=ghuntley.com)

\[experimental\] Geo-aware egress router for underclass: retries OpenAI capacity errors in another region over a Tailscale mesh, keeping prompt caches warm - ghuntley/geoclass

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/icon/favicon-a6233f77-bb91-4153-ad02-6d008cf5526c.png)GitHub

![](https://opengraph.githubassets.com/1abfa8d880b75475bfbabdb698c328de58526a92b7c02633e9dfb6d0309dc83a/ghuntley/geoclass)

ps. socials

> 🗞️ pooling OpenAI subscriptions and bypassing regional inference capacity limits [https://t.co/s10AD5O5Vb](https://t.co/s10AD5O5Vb?ref=ghuntley.com)
>
> today I'm releasing a new experimental prototype called geoclass, which routes your requests to different inference regions around the world.
>
> set up a Tailscale network… [pic.twitter.com/2htIMiUdD3](https://t.co/2htIMiUdD3?ref=ghuntley.com)
>
> — geoff (@GeoffreyHuntley) [October 17, 2026](https://x.com/GeoffreyHuntley/status/2111367787629195767?ref_src=twsrc%5Etfw&ref=ghuntley.com)
