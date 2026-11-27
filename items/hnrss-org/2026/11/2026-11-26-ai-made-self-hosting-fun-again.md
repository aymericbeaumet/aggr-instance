---
title: AI made self-hosting fun again
link: https://janlukas.blog/posts/ai-self-hosting
source: hnrss-org
published: 2026-11-26T21:47:45Z
updated: 2026-11-26T21:47:45Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- jlelse
labels:
- ai
- selfhosting
- thoughts
content: extracted
html: 2026-11-26-ai-made-self-hosting-fun-again.html
---

I’ve been self-hosting things for many years. There were times when I was quite enthusiastic about it, constantly trying out new software and tinkering with my infrastructure. But at some point, that enthusiasm faded a bit.

I never really stopped self-hosting, but I became less inclined to experiment with new things. Maintaining what I already had running was enough for me.

Over the last few months, however, I’ve found myself getting increasingly excited about self-hosting again. And, somewhat ironically, AI is a big reason for that.

## Less friction

Most of my infrastructure configuration lives in Git repositories, mainly in the form of Docker Compose files, scripts, and other configuration files. This makes it possible to use AI coding agents to work on my infrastructure in much the same way as they would work on any other software project.

Whenever I want to try a new service, adjust an existing configuration, or automate something, I can now do so with considerably less effort.

In the past, I would sometimes put off certain changes simply because I didn’t feel like spending an evening reading changelogs and documentation or troubleshooting issues. It’s not that I couldn’t have done these things before. But often, the amount of effort required just wasn’t worth it to me.

Now, even relatively spontaneous ideas often end up becoming reality.

## Trying things I always found interesting

There are also technologies that I’ve always found interesting but never felt knowledgeable enough about to really make use of them.

NixOS is a good example. I’ve always liked the idea of declaratively configuring an entire system; I [even used it on my laptop](https://janlukas.blog/dev/back-to-ubuntu) during my Linux honeymoon phase. But getting everything configured the way I wanted was quite a hurdle. With the help of AI, I’m now making much more extensive use of NixOS on my home server.

For my VPS infrastructure, I’ve also started building my own Debian-based bootc image (after [thinking about it](https://janlukas.blog/micro/2026/09/2026-09-08-xjhfc) for some time).

Having my entire operating system configuration defined in a Git repository and automatically building a bootable container image from it is something I find really cool. But it’s also something I probably wouldn’t have invested enough time in to get working on my own before. Even with the help of AI, it still took me half a day when I set it up a few months ago. Without AI, it would probably have taken me at least a week.

AI makes experimenting with unfamiliar technologies much more approachable. I don’t necessarily need to understand every little detail before I can get started. I can learn about things along the way, ask questions, and gradually improve my configurations.

## Automating almost everything

Something I’ve also finally managed to do is automate most of the build, update, and deployment processes for my infrastructure.

A lot of the work involved in self-hosting isn’t particularly exciting. Keeping track of new releases, updating container image tags, building images, and deploying configuration changes are all things that need to be done regularly.

I still remember what updating a service used to look like: bumping the version and adjusting the Docker Compose file on my laptop, pushing the changes to the Git remote, SSHing into the server, pulling the changes, and finally deploying them. Now, a single Git push is enough. If a push is even necessary.

With Renovate, I no longer need to actively keep track of most software updates. It creates pull requests for me, and often, all I need to do is review and merge them. I can even do that on my phone while sitting on the sofa.

Pushing changes or merging a pull request triggers the corresponding GitHub Actions or Forgejo Actions workflows, which take care of building and deploying everything.

Of course, there’s nothing particularly groundbreaking about any of this. DevOps has been around for a long time, and none of these automation tools require AI. But AI has helped me put all these pieces together in a way that I previously never quite got around to doing.

And the nice thing about automating these tasks is that the benefits extend well beyond the initial setup. I now have less tedious work to do every time I want to change something or update a service.

## Back to more powerful hardware

As I’ve started hosting more things again, I’ve also moved more of my services from my VPS to my home server.

For a while, I was using an HP t640 thin client as my home server. However, with more things I wanted to run, I decided to switch back to my ASRock DeskMini A300, which had been sitting unused for some time. I’m thinking about repurposing the t640 as a “router on a stick”, though.

I even took the opportunity to upgrade its CPU from an AMD Ryzen 5 2400G to a Ryzen 5 PRO 4650G and replaced the stock AMD cooler with a quieter Noctua cooler. Noise was the main reason I switched to the t640 in the first place.

It’s nice to put some hardware I already owned back to use rather than buying an entirely new machine. I also managed to get both the CPU and the cooler for a good price through a classifieds site.

Having some additional computing power available gives me more room to experiment. And there’s something quite satisfying about having a small box at home running a growing collection of services that I use in my everyday life.

## Building my own tools

But it’s not just about self-hosting existing software. AI has also changed how I approach problems for which there isn’t an existing piece of software that does exactly what I want.

Previously, I would often spend quite some time searching for a suitable open-source project, comparing different alternatives, and figuring out how to make one of them work for my particular use case.

These days, for smaller problems, building my own solution has become a much more attractive option.

I’ve recently built tools to [automatically download invoices](https://janlukas.blog/posts/get-me-the-invoices), [e-papers](https://janlukas.blog/posts/vibe-anything) or *Linux ISOs*.

I can build (or have AI build) these tools in Go, my preferred programming language, and tailor them to my exact needs.

They don’t need to support every possible use case, have an elaborate user interface, or be particularly interesting to anyone other than me. I don’t need to turn every little tool into a proper open-source project with extensive documentation and support for other people’s use cases. They just need to solve my problem.

What I particularly enjoy is how well this complements my increasingly automated infrastructure. Building a small tool has become easier, and so has building a container image for it, deploying it, and keeping it running.

The entire process, from having an idea to having something actually running on my home server, requires much less effort than it used to.

## Just because I can doesn’t mean I should

Of course, there are downsides to all of this.

Every additional service is another thing that needs to be maintained. AI-generated software isn’t exempt from bugs, security issues, or technical debt. And having an AI agent make changes to my infrastructure doesn’t absolve me of the responsibility of understanding those changes.

There’s also the temptation to build or deploy things simply because I can. When implementing an idea becomes so easy, it’s perhaps more important than ever to occasionally ask myself whether it’s actually worth implementing in the first place.

On the other hand, self-hosting is a hobby for me. Not everything I do needs to be economically sensible or particularly useful. Sometimes, experimenting with something new is the whole point.

And with much of the routine maintenance automated, adding another service doesn’t necessarily mean adding as much recurring work as it used to.

## A somewhat ironic development?

I find it somewhat ironic that AI, a technology largely dominated by big tech companies, is helping me become a little less dependent on those very companies in other areas of my digital life.

I’m aware that this also creates a new kind of dependency. But the software and infrastructure configurations I create with the help of AI can continue to exist independently of the AI services I used to create them. And since I mostly use open-weight models, I have a wide range of providers to choose from.

What I appreciate most, however, isn’t necessarily the time I’m saving. It’s that I’m more inclined to actually pursue the little ideas I have.

I’m trying out technologies I’ve been curious about for years. I’m automating things that I’ve wanted to automate for a long time. I’m building little tools for myself. I’m tinkering with hardware again.

AI hasn’t fundamentally changed why I like self-hosting. I still enjoy having control over my digital life, learning new things, and tinkering with technology.

It’s just removed a lot of the friction that made me lose some of my enthusiasm for it over the years.

And now, I’m having a lot of fun with it again.
