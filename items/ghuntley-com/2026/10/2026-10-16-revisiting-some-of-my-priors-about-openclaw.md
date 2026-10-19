---
title: revisiting some of my priors about openclaw
link: https://ghuntley.com/bot/
source: ghuntley-com
published: 2026-10-16T09:26:33Z
updated: 2026-10-16T09:26:33Z
first_seen: 2026-10-16T09:40:32.304738087Z
authors:
- Geoffrey Huntley
labels:
- ai
summary: I have to say, Peter Steinberg (aka lobster man) is absolutely brilliant. He was so ahead of his time with OpenClaw. I remember when he first tweeted about it last year, and when it turned into utter lobster madness at the beginning of the year. Heck, I even ran OpenClaw
content: extracted
html: 2026-10-16-revisiting-some-of-my-priors-about-openclaw.html
remote_preview:
  url: https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/2026/10/Symbolic-traditional-tattoo-art-print-representing-a-reassessment-of-prior-beliefs-about-OpenClaw--featuring-complex-ornamental-motifs--retro-flair--luxurious-vibrant-colors--rich-deep-teal--warm-golden-hour-illumination--soft-sunrise-or-su.jpg
  alt: revisiting some of my priors about openclaw
---

I have to say, [Peter Steinberg](https://x.com/steipete?ref=ghuntley.com) (aka lobster man) is absolutely brilliant. He was so ahead of his time with OpenClaw. I remember when he first tweeted about it last year, and when it turned into utter lobster madness at the beginning of the year.

Heck, I even ran OpenClaw for a bit, but I couldn't really see the value because I didn't want to maintain it while the [lethal trifecta problem](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/?ref=ghuntley.com) remains unsolved.

Categorically, this is a billion-dollar problem, perhaps more.

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/2026/10/image-2.png)

So here's the thing. I kind of want openclaw, but I want it managed. For almost a year now, I've been using Interaction Labs Poke, because if there's going to be a problem with the lethal trifecta, I want to make that problem someone else's as much as possible. I want them losing sleep over the lethal trifecta, not me. It's not full protection, but knowing there's a product team paid to think about these issues and resolve them at scale puts me at ease.

That is, until last week, when [Poteto](https://x.com/poteto?ref=ghuntley.com) girl nerd-sniped me on X and, in [full disclosure](https://ghuntley.com/disclosures), hooked me up with enough credits to help me reevaluate. I am now using Grokbot full-time instead of Poke (which sucks), and I've revisited my [priors about the utility of an AI bot and who they are useful for](https://ghuntley.com/toronto).

I'm writing this blog post of my own free will in the hope that it will help change some other people's perspectives on the utility of these assistants and give people some ideas on how they can be used, but most importantly, to highlight the risks of using these types of tools.

In this post, I'm going to give a high-level overview of how I've automated some aspects of my life. Late to the game, I know, but with some things it's better to be cautious.

## initial experiences

Okay, Grokbot is really weird, and I guess this applies to, like, even OpenAI's dots, even though I have not used it. One of the hardest things about it is how generic it is. Once you have it installed, it becomes a question of "now what?"

You need to be curious, have lots of imagination, and when you have an idea, just ask for it. From that point forward, it starts to unfold like a pretty flower. If functionality is missing, ask for it and turn it into a skill.

## starter pack

If you're using Grok, install this skill pack first, and use it to create other bots. Every time you need a new bot, which is often, ask this bot to create one.

[dr eggbot · Grok Bot Marketplace](https://x.ai/bot/marketplace/bots/dr-eggbot-v2?ref=ghuntley.com)

Designs high-quality Grok Bots. Asks a few preference questions, then creates them with CreateAgent.

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/icon/icon-991fbc1d-baa8-4b59-a819-3a51b67f9ed7.png)SpaceXSI Lauren Tan

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/thumbnail/introducing-grok-bot-og-2-31caf52c-45db-416f-96aa-980c6471aac8.png)

## some ideas for bots

Here are the bots I've defined in the last week, which I'm getting a lot of utility out of:

**Daily briefs and reporting**

- **Daily Update** sends the 7am brief: calendar, emails that need a reply, and site traffic. Connectors: Google Calendar, Gmail, PostHog.
- **Blog Pulse** reports on the health of [https://ghuntley.com/newsletter](https://ghuntley.com/newsletter): signups, unsubscribes, churn, MRR, and traffic. Connectors: Ghost Admin API (direct read-only, no connector), Stripe (read-only), PostHog (read-only).
- **Inbox Diet** finds newsletters worth unsubscribing from and lists them for you to click yourself. This is by far one of my favorite bots. I have unsubscribed from so many newsletters via this bot, probably upwards of 300, that would be too hard to do by hand. Finally, my inbox is under control. Connector: Gmail.
- **Family** sends a digest of WhatsApp messages related to my kids, does reminders, and automatically extracts classwork and homework reminders from school emails. Connectors: Gmail, bidirectional access to my WhatsApp via the WhatsApp bot. A custom parser for Seesaw that can retrieve updates sent by my kiddos' teachers.
- **WhatsApp** answers lookups of my WhatsApp messages acts as a function pipe for bots. Connector: WhatsApp Web in a browser, with no connector behind it. The bot can tunnel to my Mac Mini, read the WhatsApp message database directly, and send messages, but it can't send messages from the Mac Mini. To do that, you need to authenticate in a browser session, which allows two-way communication.

**Money, tax and legal**

- **PocketSmith** keeps my spending categorized and my budget up to date. PocketSmith already has pretty good categorization rules built in, but they normally rely on the user to train PocketSmith. Here, I use an LLM to do the categorization automatically. It is good. Connector: none; it calls the PocketSmith REST API directly.
- **Australian Tax** continually prepares a rough first draft of my personal tax records for each financial year. Connectors: Gmail, plus the PocketSmith bot.
- **Bank Details** shares my bank details with your other bots when they need them. Connector: none; the details are stored as a secret.
- **Conference Reimbursement** is one of my favorite bots. Conferences normally work on the basis that they will pick up your flights once you've done the talk, which always carries this risk that you won't be reimbursed. If it's a professional conference, then you get paid up front, but sometimes the community conferences have cash flow problems because they haven't sold all their tickets yet. So normally it's a gentleman's handshake that they'll reimburse, but sometimes these conferences don't reimburse me. For example, right now I'm probably out close to $10,000 in travel expenses this year. When these situations arise, I ask Dr. Egg Bot to create a reimbursement reminder bot. That bot has access to my email, WhatsApp, and everything else, and it can automatically query PocketSmith to see whether the reimbursement has landed. If it hasn't landed, it will ask me for permission to send a WhatsApp message or an email to nudge people along. Connectors: Gmail, WhatsApp bot.

**Life admin**

- **Kitten Vet** tracks the kittens' vaccinations and vet admin and automatically reminds me of what I need to do for them. Connectors: Gmail, Google Calendar.
- **Desires** keeps my bucket list and "someday" list. Connector: Google Calendar and X. What's really cool is that sometimes I'll have a conversation out in public and genuinely want to catch up with someone, but I forget. So I can just prompt my desires bot to like add it to the particular category, either by searching X or dropping the exact link of the conversation.

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/2026/10/Screenshot-2026-10-16-at-8.02.11---PM.png)\
desires bot

**Dev and infrastructure**

- **Mac Mini** operates the Mac mini. It's a function pipe bot. Tailscale is installed on the Linux machine the bot runs on; it's on my Tailscale network and can tunnel its network connectivity out via my Mac Mini as needed to bypass any geo-blocks on data center IP addresses.
- **Overalls** manages my upcoming open-source project called Overalls (previously known as loom) and acts as a remote interface for dispatching agentic coding tasks in ephemeral Firecracker VMs hosted on my self-hosted infrastructure.

[Overalls: an AI-first developer platform that you own](https://overalls.dev/?ref=ghuntley.com)

Overalls is an open-source, self-hosted platform for persistent Firecracker development machines and the AI agents that work on them.

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/icon/favicon-8885c1b6-40cb-4d08-953b-bdd019d43afe.png)Overalls Geoffrey Huntley

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/thumbnail/og-53ecf474-d1d4-4006-ae97-41c3f1567524.png)

loom is shipping

## lessons learned

What you want to do is to create bots thinly. You don't want a fat bot. You want to create a bot per function. Think about the Unix command line, how composable that is. You want to create small bots that can call other small bots, then pipe them together in a function chain.

You can install software on your Grok bot. For example, I have Tailscale installed, and that's how I do all my tunneling.

## the future

If the lethal trifecta is ever solved at a fundamental level, I'll move all this stuff into a self-hosted Open Claw, but not before. I get the value of OpenClaw, and they do a [good job with security](https://openclaw.ai/blog/openclaw-trail-of-bits-engagement-recap?ref=ghuntley.com), but fundamentally, I sleep much better at night knowing there's a product team whose arse is on the line, with staff and a funded security organization with a security VDP program that loses sleep over these topics, so I don't have to.
