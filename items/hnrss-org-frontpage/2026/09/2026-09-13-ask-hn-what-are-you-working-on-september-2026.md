---
title: 'Ask HN: What are you working on? (September 2026)'
link: https://news.ycombinator.com/item?id=49686380
source: hnrss-org-frontpage
published: 2026-09-13T17:31:38Z
updated: 2026-09-13T17:31:38Z
first_seen: 2026-09-14T06:57:16.252250712Z
authors:
- david927
summary: 'What are you working on? What have you been curious about lately? Comments URL: https://news.ycombinator.com/item?id=49686380 Points: 135 # Comments: 393'
content: extracted
html: 2026-09-13-ask-hn-what-are-you-working-on-september-2026.html
images:
- source: https://news.ycombinator.com/s.gif
  original:
    file: 2026-09-13-ask-hn-what-are-you-working-on-september-2026.image-56d45f8a17f5.gif
    width: 1
    height: 1
  variants:
  - file: 2026-09-13-ask-hn-what-are-you-working-on-september-2026.image-341355a3f873.webp
    width: 1
    height: 1
  color: '#000000'
- source: https://i.ytimg.com/vi/50oQQThXWnc/hqdefault.jpg
  original:
    file: 2026-09-13-ask-hn-what-are-you-working-on-september-2026.image-07e5051de3ea.jpg
    width: 480
    height: 360
  color: '#010101'
html_truncated: true
---

[](https://news.ycombinator.com/vote?id=49686380&how=up&goto=item%3Fid%3D49686380)

[Ask HN: What are you working on? (September 2026)](https://news.ycombinator.com/item?id=49686380)

135 points by [david927](https://news.ycombinator.com/user?id=david927) [13 hours ago](https://news.ycombinator.com/item?id=49686380) | [hide](https://news.ycombinator.com/hide?id=49686380&goto=item%3Fid%3D49686380) | [past](https://hn.algolia.com/?query=Ask%20HN%3A%20What%20are%20you%20working%20on%3F%20%28September%202026%29&type=story&dateRange=all&sort=byDate&storyText=false&prefix&page=0) | [favorite](https://news.ycombinator.com/fave?id=49686380&auth=605514f7c5766666bbb262cd444f5e5d6cde849b) | [394 comments](https://news.ycombinator.com/item?id=49686380)

What are you working on? What have you been curious about lately?

 [help](https://news.ycombinator.com/formatdoc)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692915&how=up&goto=item%3Fid%3D49686380)

\

I'm helping my wife build [https://quantral.com](https://quantral.com) - initially we put it together to track stock sentiment/crowd wisdom on social networks for our own investments. This is something I was previously doing manually every morning while having breakfast. I couldn't find a similar project to satisfy my needs for discovering hot stocks and the track records of people who mentioned those stocks on finx and /r/wsb. We are now planning to add congressional trading as another data source.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692897&how=up&goto=item%3Fid%3D49686380)

\

[https://common.charity](https://common.charity)

I’m building the UK’s ‘big data’ charity with some awesome co-trustees and volunteers. We undertake big data projects in service of human flourishing, collect and synthesize datasets from hostile actors or freedom of information requests, and collaborate with other charities to build useful things that facilitate effective coordination.

The context is that I co-run the UK’s largest and most active landlord review platform ‘Marks out of Tenancy’, and the data we aggregated and ended up being able to host and query did a number of incredibly high impact things:

\- our system for identifying how many people lived in a property started getting used to find victims of human trafficking - our system for checking for HMO licensing got picked up by councils to detect 100% of illegal or rogue landlords in the scan area, as well as detecting illegal short lets. - our longitudinal data collection on housing conflict between tenants and landlords directly led to increasing the rent repayment order threshold in the renters rights act from 12 months to 24 months.

So after seeing how much data is out there ready to be used for advocacy - but just left untapped by a profit driven sector, and after years of experience as ‘activist data practitioners’ or whatever you’d want to call it, we decided to launch something that could be a home for those projects in the spaces we’re in over here in the UK.

We’ve already got some awesome projects underway and we’re excited to see how year 0 goes for us!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692880&how=up&goto=item%3Fid%3D49686380)

\

[https://multicoder.dev/](https://multicoder.dev/)

One UI for every coding agent - VS Code extension (GUI) to launch and work with any ACP-compatible agent harness. Supports Claude Code, Codex, OpenCode, Qwen and many others - [https://agentclientprotocol.com/get-started/registry](https://agentclientprotocol.com/get-started/registry) . Does not require any new subscription or user ID - this is still handled by the harness, so you can use existing Claude Code or Codex subscription. Multicoder just gives you a high-quality GUI to work with any of them without mental switching cost.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692848&how=up&goto=item%3Fid%3D49686380)

\

Working on [https://localhero.ai](https://localhero.ai), on-brand automated translations for product teams. It mainly runs as a GitHub Action, translates new strings on your PRs against your glossary and style guide. This month focus went mainly on MCP and stuff around that. Thinking a PM/designer/etc who wants to bulk tweak copy without touching git or similar. Claude Desktop/ChatGPT turns out to be a pretty nice surface for this, the assistant finds every key that needs changing and organises the work, Localhero keeps it on-brand. A pretty sweet workflow. Also doubled down on Lingui and Django support. Also been fighting some spam. A 400 from our mail service showed up in error tracking, looked like a malformed address, turned out something else. Someone setup a bunch of bots to signup and try so send crypto spam using the team invite function. Kind of persistent, I fix a few things and it tries a few others. Finally stopped, interesting to see it live like this.

On the side, [https://infrabase.ai](https://infrabase.ai) (hand-verified AI infrastructure directory) launched a Media Generation category, image and video APIs after a run of requests for it. Also the submission spam is way up here latley, been improving the detection here as well. Also seeing real interest in paid listings now, including one that started as a link-buying request I turned down :)

Been doing a lot of AI assisted emailing, so I have started to work on a little review UI for AI generated text, comments on specific words, inline edits, Claude picks up both and revises in place. Like a PR review interface for text. Turns out pretty good workflow with Claude watching the files for new comments, editing and commenting back right away.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690404&how=up&goto=item%3Fid%3D49686380)

\

I've been working on a voxel game engine called Bonsai for ~10 years.

Probably the most interesting thing about it at the moment is the editor. The world, and most things in it, are represented as collections of SDFs. More accurately, they're density fields, but, potato-tomato.

Bonsai has undergone a large rewrite over the last couple years that's nearing completion. A world edit is defined as a bunch of SDF parameters which get projected/rasterized into the voxel grid by a shader on the GPU. One neat thing about SDFs is they've been thoroughly researched and documented by a guy named Inigo Quilez, and they have a lot of nice mathematical properties. For example, you can do a smooth union of arbitrary SDFs to get nice rounded contours where shapes join.

I've written every system from scratch, all the way from the memory allocators and font rasterizer to the collision detector and simulation loop. I even wrote a metaprogramming language as a replacement for C++ templates, which is a whole other story. IIRC the only external dependency is the C runtime library for starting the process and my very occasional use of variadic functions arguments.

For a long time, an explicit non-goal of the project was to ship a game. It sounded insane to me to write an entire 3D engine and then ship a game. As it turns out, I've gotten it to the point where I can actually make a game. I've got a start on the game systems in a closed-source repo, and hope to have a steam page for it by the end of the year.

I'll do some shameless self-promotion and leave some links here for anyone interested in looking at the engine, language code, or some pretty pictures.

[https://github.com/scallyw4g/bonsai](https://github.com/scallyw4g/bonsai)

[https://github.com/scallyw4g/poof](https://github.com/scallyw4g/poof)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691458&how=up&goto=item%3Fid%3D49686380)

\

Very cool. Are you familiar with the game Voxile? Its author also created custome languages, and the game uses Lobster.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691410&how=up&goto=item%3Fid%3D49686380)

\

this looks amazing

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691050&how=up&goto=item%3Fid%3D49686380)

\

Wow, unbelievably cool. Good luck with the new game using the engine

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692870&how=up&goto=item%3Fid%3D49686380)

\

Working on Pho \[1\] my own TUI for pull requests across multiple repositories. It aims to be direct replacement of Github web UI for 80% of the use cases. The number of PRs I have to review on a daily basis is still increasing, and I didn't really like using the web version. This is made exactly how i like it, keyboard driven and fast.

\[1\]: [https://github.com/utkarsh261/pho](https://github.com/utkarsh261/pho)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691502&how=up&goto=item%3Fid%3D49686380)

\

[https://curvefit.app](https://curvefit.app)

it's a weight-training app that helps you train along your "pareto frontier" of weight vs reps. The idea is to train at lower weight-higher rep, medium weight medium reps, and higher weight, lower reps for every movement. I tried to develop my own weight training program following bits and pieces of advice from bodybuilding forums and ended up injuring several tendons in my first year. So I did a bunch of research on tendon strengthening as well as what's most effective for hypertrophy (reps near failure) strength (reps near maximal load) and injury-prevention/frequency (not bringing yourself to failure too often) and designed an app to automatically prescribe and advance weights and reps based on your learned strength curve (Brzycki-like, with an added shape parameter)

The app is designed to make use of the free Cloudflare tier, so I can support thousands of athletes for just the cost of the domain name. I'm primarily interested in understanding the "Fatigue curve" - right now I have some basic per-set fatigue modeling (basically a log-linear strength dropoff) but I think it could be much better characterized with more data. I could go on and on about the modeling but my intention is to keep it free (maybe add some non-intrusive ads on content pages if it ever starts costing me a few pennies a month) but my primary interest is to be able to do statistical analysis on the data.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692096&how=up&goto=item%3Fid%3D49686380)

\

Gave it a quick go as I'm looking to adopt some sort of fitness app. There's too much text/complexity for my taste.

The website could use some images/simplification to get going and then have more details and whatnot later on. It seems that you let the LLM generate the content itself (em dashes, emojis) - Personally I'd be skeptical of a fitness app that has most of its content LLM'izied.

On step 3 of the tutorial, it's not clear that you can scroll down and there's more there on a mac 16''. I was confused what to do. It didn't allow me to change to metric system either (which I later found in the user-settings). You can't 'esc' from the tutorial popup either.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692334&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692658&how=up&goto=item%3Fid%3D49686380)

\

\> a bunch of research

What exactly is your "research"? Is it reading more bodybuilder forums (bro science) or is it physiological studies (actual science)?

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692830&how=up&goto=item%3Fid%3D49686380)

\

Greetings, I'm on a mission that never existed before, for me it has been a journey of what is wrong with the internet. When I realised There Is A Better Way \*SOVEREIGN\* Please read my founding document and have a mosey around my site and github pages. [https://at1c.com/AT1C\_Founding\_Document.html](https://at1c.com/AT1C_Founding_Document.html) If this is something that bothers you as well please reach out get involved and help build this for humanity. It's our world too not just for the few who enrich themselves off the rest of us. A Human Jimmy Erwin

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692638&how=up&goto=item%3Fid%3D49686380)

\

I'm still working on [https://openaltfinder.com](https://openaltfinder.com); a place to help you discover open source alternatives. Got quite a lot of traction this month due to Google/Apple Maps renaming Lake Ontario.

But overall it's a struggle, the site is still in Google jail/sandbox, so it's not even seeing any Google impressions. So most traffic is due to being very active and posting wherever I can.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692389&how=up&goto=item%3Fid%3D49686380)

\

\[HUMAN-CODE\]

I'm building a video game. It's part Valheim, part Ultima IV:

[https://stravaeger.com/](https://stravaeger.com/)

You can play the demo in the browser now (mobile as well as desktop), but I'll be packaging it up for Steam soon.

It's all vanilla javascript, html and css. No AI, no 3rd party game engine, no build system even, and just a couple external dependencies for things like networking that I didn't want to reinvent myself.

It's been fun. I'd love to get some feedback!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692650&how=up&goto=item%3Fid%3D49686380)

\

Multi-org/Multi-team Software factory x Agent Sandbox with bring-your-own-harness model: [https://xbin.dev](https://xbin.dev)

Built for myself and my companies, OSS, maybe looking for a founder if anyone wants to make it be something, I got enough things to run heh.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692689&how=up&goto=item%3Fid%3D49686380)

\

This looks cool - however, I had a hard time understanding what I can use xbin for. I feel like it would be better if the landing page, or the GitHub README talked about what xbin enables me to do first, and then talk about the technical bits.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692725&how=up&goto=item%3Fid%3D49686380)

\

Yeah I’m still iterating on the pitch and target audience, thanks for flagging.

It’s not a general coding tool, shortest description would be “personal/company data/process management layer” or “everything a company may want an on-prem datacenter and admin (dev)team for, accelerated by AI”.

Came from finding bug shortcomings in OpenClaw/Hermes where it’s not really close to having everything needed to organise and deal with big data in a safe or efficient way.

Basically the bottleneck is building a massive amount of software around agents which won’t happen in an all-in-one harness, this attacks that problem

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691483&how=up&goto=item%3Fid%3D49686380)

\

I run a small experiment on this: 29 paper-trading accounts on real US stock prices, $100k each, since July 27. Four AI models (ChatGPT, Claude, Grok, Gemini) each write a trading rulebook and rewrite it every day from their own results. One account is a fixed rulebook that no AI ever touches, as a control.

Result so far (paper, 34 trading days): the no-AI control is +13.0%, the S&P 500 is +3.4%, and 25 of the 28 AI accounts are below the control. The best single account is a Grok-written "patience" book at +40%, which I treat as one lucky account in a choppy market, not a finding. At the trade level the AIs and the control look the same: 3,212 closed positions, median +0.06%, median hold about 2 hours. They trade a lot and mostly go nowhere.

Everything is public, including the losses and the retired strategies: [https://aitradingcompetition.com/which-ai-is-winning.html](https://aitradingcompetition.com/which-ai-is-winning.html) and the full trade file as CSV at [https://github.com/ckamelhar-collab/ai-trading-arena-data](https://github.com/ckamelhar-collab/ai-trading-arena-data). Paper money only, not advice, nothing for sale on those pages.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692256&how=up&goto=item%3Fid%3D49686380)

\

None of this really means anything. For one if you test enough strategies one by luck will be killer. Second, past performance != future.

One AI might consist of some convulated strategy of who knows what, but all it takes in the end is for it to say...hold more tech stocks than not, or buying more call options or leveraged positions in a bull market, being more out of the market during a bearish market, etc. But that same strategy will fail miserably as soon as the market conditions change

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691773&how=up&goto=item%3Fid%3D49686380)

\

Don’t let AI write for you. Everyone can tell.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692076&how=up&goto=item%3Fid%3D49686380)

\

Why didn't you let the bots backtest and forward test to pick the mix of best strategies/indicators?

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692544&how=up&goto=item%3Fid%3D49686380)

\

The problem is the bots may have some of the backtest data already in their training.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692001&how=up&goto=item%3Fid%3D49686380)

\

I'm curious what signals they're trading on. SEC filings? Candlestick voodoo numerology?

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691956&how=up&goto=item%3Fid%3D49686380)

\

You have $2.9M just for this? Wow.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692012&how=up&goto=item%3Fid%3D49686380)

\

paper trading means that no money is used

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692776&how=up&goto=item%3Fid%3D49686380)

\

Thank you, never heard the term before.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692048&how=up&goto=item%3Fid%3D49686380)

\

They mentioned paper trading which means it isn't using actual money.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692814&how=up&goto=item%3Fid%3D49686380)

\

I have been researching powerbanks as my current one of 10 years just died. I was dismay at the disarray of information, so I started to build a comprehensive list. First it is across the major players, Anker, Belkin Ugreen texc. I will expand to others we I come across them. Their basic data is shown, but I want to start collecting data such as "is it approved for flight?" or "its form factor".

[https://www.powerbank.energy/](https://www.powerbank.energy/)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692823&how=up&goto=item%3Fid%3D49686380)

\

[https://doodlemate.com](https://doodlemate.com)

DoodleMate is a multimodal storytelling canvas that exposes students to the basics of animation, character design, and story creation. Starting from paper drawings, it creates rigged versions of your characters that can talk, walk, dancing, sing, etc and you can create scenes out of it.

It doesn't use any generative AI whatsoever. Just computer vision and animation algorithms.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692876&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690808&how=up&goto=item%3Fid%3D49686380)

\

I'm buildingan open-source way to fill U.S. immigration forms without fighting clunky PDFs

Github: [https://github.com/athos2113/fillvisa-os](https://github.com/athos2113/fillvisa-os)

Demo: [https://fillvisa.com/demo/](https://fillvisa.com/demo/)

US Immigration still relies on outdated XFA PDFs - you can't fill them on your browser. Most immigrants end up printing the form and fill it manually.

So, I converted the PDF forms into smart web forms. They are replica of the official USCIS forms and follow conditonal logic. In the output, you get the official USCIS pdf form filled.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692909&how=up&goto=item%3Fid%3D49686380)

\

Apple preview and the add text button got me through all the USCIS forms. You can use AI computer control to do this stuff now, right?

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692143&how=up&goto=item%3Fid%3D49686380)

\

I hope this becomes obsolete by no longer being necessary

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691712&how=up&goto=item%3Fid%3D49686380)

\

I was stuck unable to fill out a PDF *I-864* for immigration because the required fields were non-editable. And opening the PDF in a PDF editor or Photoshop would require a password that was not available. So I had to create a PDF-to-JPG-to-PDF pipeline:

[https://github.com/whyboris/PDF-to-JPG-to-PDF](https://github.com/whyboris/PDF-to-JPG-to-PDF)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691810&how=up&goto=item%3Fid%3D49686380)

\

Interesting. I downloaded some PDFs for the adjustment of status forms recently and most of them were editable but only partially - the conditional fields couldn't be edited it seemed.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691559&how=up&goto=item%3Fid%3D49686380)

\

That's cool. How are you gonna get it in the hands of people that would need it?

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692885&how=up&goto=item%3Fid%3D49686380)

\

Thanks!

Primarily by sharing the web version (fillvisa.com) on subreddit and FB groups. People complain about the USCIS PDF regurlarly, so, I just pitch them the website

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692770&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692679&how=up&goto=item%3Fid%3D49686380)

\

[https://stagewatch.de/](https://stagewatch.de/)

Basically a simple pizza status tracker for small German fabricators and craftsman. You define the process, instantiate it per customer order who receives the link to a status page with a possibility to subscribe to status notifications as well. The benefit is that people don't call you anymore asking for the status of their order, saving time.

It's a pretty vanilla rails app running on a hetzner vps.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692833&how=up&goto=item%3Fid%3D49686380)

\

Nice idea. How does the provider receive the order from customer? Is it through phone call, or via some other app? If it is an app, won't that app have a status tracking feature?

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692376&how=up&goto=item%3Fid%3D49686380)

\

It would be great if these posting could have tags like below prefixed at the top by the poster.

\[HUMAN-CODE\] - code written by humans

\[AGENT-CODE\] - code written by Claude/Codex/Other agents out there

\[HYBRID\] - Combination of Humans/Agents

I would prefer to go through projects where people are writing code themselves. And this kind of tagging would help me filter through easily.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692435&how=up&goto=item%3Fid%3D49686380)

\

Please, yes. Half of the fun of being on HN was "Show HN: I made some random thing", which typically amounts to "look at what prompt I wrote" nowadays. I use agents every day, I'm not against it, but it completely changes the lens through which I view a project

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692408&how=up&goto=item%3Fid%3D49686380)

\

Good idea. I've edited my post to include that.

Frankly, it's surprising that we'd even need to stipulate something like this. It would never occur to me to post to a "show us what you're cooking" thread with: "Look what I asked DoorDash to bring me!", but I guess that's where we are now...

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692606&how=up&goto=item%3Fid%3D49686380)

\

I made a very basic web app to make our family meal planning a bit easier.

We’ve been using it for a few months, so now I’m trying to tidy it up so others can use it too.

[https://mealplannertool.com/](https://mealplannertool.com/)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692743&how=up&goto=item%3Fid%3D49686380)

\

I need something like this. Would be nice if there was a randomizer.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691037&how=up&goto=item%3Fid%3D49686380)

\

I have ALOT of free time since quitting my job.

0) money-money-money: A sub 15Kb server-less opinionated household budgeting tool \[0\]. Why? Its made to be extremely compressible, so you can edit your household budget and share back and forth with your partner via short-links or QR codes. The idea is you edit your budget, send the link to other party, they can view or edit and send back to you without a server in the loop. \[0\]

1) slices: An alternative AI-first ADHD-friendly architecture experiment where everything you code is a "slice" with a shared event pool. A slice is a fully self contained "feature". Slices can only communicate with other slices through a shared event pool. Inspired by small-talk, vertical-slice architecture, and ADHD. I used slices to build a self-modifying IDE too. \[1\] \[2\]

2) text compression experiments: I'm experimenting with approaches to compress text in pursuit of the Hutter prize. The idea is to take the entirety of the English Wikipedia, and compress the text as small as possible whilst still being able to reproduce it. \[3\]

3) tiniest maze solving neural network: A write-up work in progress on a maze solver I built with heavy LLM help, managed to get a 14 byte neural network solving 96% of unseen mazes. \[4\] \[5\]

\[0\] - [https://con-dog.github.io/money-money-money](https://con-dog.github.io/money-money-money)

\[1\] - [https://github.com/con-dog/slices-demo](https://github.com/con-dog/slices-demo)

\[2\] - [https://con-dog.github.io/slices-demo/](https://con-dog.github.io/slices-demo/)

\[3\] - [http://prize.hutter1.net](http://prize.hutter1.net)

\[4\] - [https://github.com/con-dog/tiny-neural-network](https://github.com/con-dog/tiny-neural-network)

\[5\] - [https://minimio.ai/](https://minimio.ai/)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691473&how=up&goto=item%3Fid%3D49686380)

\

Very cool projects, and amazing a 14 byte nn can do it. Would be fun to analyze its approach, converting it to a Code-as-Policy.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691568&how=up&goto=item%3Fid%3D49686380)

\

Thank you! Yes the maze solver has been a very fun experiment with LLMs, and code as policy is a good idea. I’m racking my brain trying to find my next thing to experiment with, but usually the ideas come to me in waves

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692790&how=up&goto=item%3Fid%3D49686380)

\

[https://github.com/hsnice16/tula](https://github.com/hsnice16/tula)

Tula shows your true cross-venue exposure across HyperLiquid, Aave, and more, what breaks first, and more.

It's a terminal tool. The experience is very similar to any LLM terminal tool if you have used one.

You can also add an agent and ask things in plain English. Live at: [https://usetu.la/](https://usetu.la/)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692572&how=up&goto=item%3Fid%3D49686380)

\

I’m very interested in building tools for coding agents (where I spend a lot of my time).

I’ve been building [https://smalldocs.org](https://smalldocs.org), it’s a simply styled, but functionally deep, artifact layer for agents. It’s open source and free to use.

Because it can incorporate charts, diagrams, spreadsheets, etc. in one artifact, it’s good for creating (and optionally sharing) agent created analysis.

It has a local library for all your Markdown files, and a cloud library (my one paid feature) too. Both support tagging and rich search/filtering. I find it turns SmallDoc artifacts into little checkpoints in work I can easily circle back to.

Thanks for reading!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692746&how=up&goto=item%3Fid%3D49686380)

\

[https://github.com/ninjaxtools/slopdex](https://github.com/ninjaxtools/slopdex)

This is my attempt to improve my agentic coding workflow by giving the agent an index into the source code through vector embeddings and LLM generated descriptions with local sqlite as the storage backend.

There are many similar tools, but I wanted to learn how such a tool can work by building it myself. What I like about my own version is that I kept things simple - no MCP, no server, no watches, just a CLI that uses git commit hashes to reindex only what has changed.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692788&how=up&goto=item%3Fid%3D49686380)

\

I'm working on bringing birds, sounds and art to life through [https://github.com/arnegiacomo/fugleramme](https://github.com/arnegiacomo/fugleramme).

An E-ink bird frame for Raspberry Pi - with real-time bird detection by audio, fully local AI, driven by BirdNET-Go rendered as real, hand-cut 1800s bird illustrations.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692832&how=up&goto=item%3Fid%3D49686380)

\

Ah this sounds real fun and looks good ! I might give it a go in the coming weeks.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692747&how=up&goto=item%3Fid%3D49686380)

\

[https://heyrita.app/](https://heyrita.app/) (waitlist)

It's an ADHD medication companion. It helps with remembering to take your meds, understanding the effect curve, getting lifecycle notifications (when it peaks, when it starts to wear off), pacing multiple doses throughout the day, and managing your supply.

It's built for iOS and uses the apple ecosystem to ensure privacy for your health data. Data is stored in your own private iCloud account and we do not have access to it.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692731&how=up&goto=item%3Fid%3D49686380)

\

Continuing to develop my LPFM radio station www.kpbj.fm

\- We have accumulated almost all the equipment needed to setup our FM broadcast. The main piece of equipment we still need is the EAS Decoder.

\- Our studio space build is in progress. I recently welded some tables for the booth and found an old Orban Optimod for our airchain.

\- We have over 80 shows and our roster continues to grow.

If you are in Los Angeles or love community radio please reach out.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692543&how=up&goto=item%3Fid%3D49686380)

\

I'm working on a new programming language for WebAssembly GC, called Zena: [https://zena-lang.dev/](https://zena-lang.dev/)

It's like a fixed up TypeScript specifically tailored for Wasm GC that can produce very small and fast binaries, but also has features like pattern matching, ownership / borrow checking for resources, pipeline, tail call elimination, multi-value returns, direct WASI component integration and a lot more.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692729&how=up&goto=item%3Fid%3D49686380)

\

This looks pretty cool! I love TypeScript. Although for my server-side apps, I just use Deno and compile my executable that way.

I have pondered cloning Caddy in TypeScript just because...would Zena be capable of this?

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691351&how=up&goto=item%3Fid%3D49686380)

\

I’m working on translating the Renaissance — it was mostly written in Latin and 90% of it has never been translated to English. Even big names, like Ficino, Durer, Kircher, Fludd and Drebbel. And that’s Latin— things much more sparse in other classical languages.

[https://SourceLibrary.org](https://SourceLibrary.org) is a hub that now provides over 20,000 translations of classical texts in dozens of languages (Latin and Greek but also Chinese, Sanskrit, etc). We always provide the original image of the text next to the AI translation (so if you don’t trust it, you can do it again). For a sense of scale, SourceLibrary now hosts more words than English Wikipedia!

You can freely connect to the public MCP for research; that way AI can read books and not hallucinate quotes. In Claude, it also retrieves images directly into the chat.

This is a philanthropic project based at the non-profit “The Embassy of the Free Mind” in Amsterdam, a rare book library devoted to free thought and mysticism. [https://embassyofthefreemind.com](https://embassyofthefreemind.com)

SourceLibrary is totally free and open source. If we can raise the money, we hope to translate 100k books before the end of the year.

If you want to dive in, I’d suggest trying our research agent with a rabbithole topic of your choice: [https://sourcelibrary.org/librarian/](https://sourcelibrary.org/librarian/)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691476&how=up&goto=item%3Fid%3D49686380)

\

I am curious about this. I have found that there are number of primary works that have not been translated into english and are in the public domain in their native language. Translations are copyrighted if they are recent. I have a very short list of items which are of interest to me. They are things like Sundiata Lion King of Mali not having a public domain translation in English that I can find easily or there not being a public domain translation of Dede Korkut in English. Many of the better translations are copyrighted and not free.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691547&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691639&how=up&goto=item%3Fid%3D49686380)

\

I've built Flexito, a website/mobile app (it's PWA) for daily stretching and mobility exercises.

As many of us here, I sit too much in my chair during the day and I'm trying to stretch more often and strengthen my back/core muscles.

There are a lot of "free" apps out there e.g. Bend that are helpful but I was annoyed with ads and limitations so I built one for myself. Now I use it daily.

The app is simple, you have about 100 routines to pick from and you can even build your own from existing exercises. Each routine comes with simple images, a link to a Youtube video and a timer. There are a lot of 5 minute routines that are perfect for a short break during work.

There are no accounts and I don't plan on adding any more features unless highly requested by users. I've built this for myself but I'm sharing it here as others might find it useful. Any feedback is appreciated!

[https://flexito.fit/](https://flexito.fit/)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692264&how=up&goto=item%3Fid%3D49686380)

\

How did you generate the images for all the routines? I’ve been wanting to really build something like but for games that you can play with your newborn

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692659&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49689838&how=up&goto=item%3Fid%3D49686380)

\

My quirky wishlist app [https://thingstohave.app](https://thingstohave.app) is finally done and heading to public launch this or next week.

I already partially covered the last few months of app development in this topic. But actually, its development took way longer, as only this iteration started in 2024. So it is the end of a long milestone— this launch date initially was to be a year ago, but I procrastinated and missed important for such apps holiday season

This year I was able to go way beyond last year’s backlog, and now I’m releasing a fully finished app instead of early access.

I’m proud of the result — a lot of overengineered stuff that is not viable in any commercial product, especially in what appears to be a simple CRUD app. For example: image loading from blur, unadvertised but very advanced DnD everywhere, natural language currency inputs with sorting, platform-agnostic quick add, and many more.

Its stack is also very fun: it has been running on the Cloudflare Workers ecosystem since 2023, and is powered by a custom Inertia.js adapter for Hono (that predates the official adapter by one month, lol), with Vue SSR. That all enables me to write old-school MVC with Vue as a template engine for views. It also proved to be very easy to work with and cheap to run.

With a tsunami of vibcoded beige wishlists, my weird app stands apart, for sure, but I’m unsure about its appeal to the average customer.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692819&how=up&goto=item%3Fid%3D49686380)

\

Love the design and the name! Just signed up - good luck with the launch!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692553&how=up&goto=item%3Fid%3D49686380)

\

A coding harness that works more like pair programming and less like code review:

[https://www.opairdev.org/](https://www.opairdev.org/)

It has 2 modes, driver and navigator, that work like the driver/navigator roles for a human/human pair. In driver mode, Opair is similar to other harnesses but with less autonomy for the agent. In navigator mode, Opair has no access to writable tools at all. Instead it monitors the project directory for changes as you make them in your regular editor, and comments on them in real time.

Why? Because big changes are harder to understand if you're not directly involved in working on them, and I want to understand the codebases I work with. I use it as my daily driver and it's perfect (for me). I recommend everyone should consider writing their own harness. When you spend so long working with something, it's nice to have it perfectly tailored to your needs.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692623&how=up&goto=item%3Fid%3D49686380)

\

Sounds interesting. Clever name, too. And open source. I’ll check it out.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686934&how=up&goto=item%3Fid%3D49686380)

\

[https://fabulae.orbilii.com](https://fabulae.orbilii.com) - A Latin language audiobook containing both volumes of A.D. Godley's "The Fables of Orbilius" which are entertaining intermediate Latin stories written at the turn of the 20th century for use in British schools.

The audio itself is in delivered with the restored classical Latin pronunciation and contains spoken nuances like vowel nasalization, elision, and prodelision. I used a wav2vec2 model to generate forced-alignment data, so you can easily follow along with the text as it's being read aloud. Each word in the text is richly enhanced with pedagogical details like: lemma, citation forms / principal parts, morphological segmentation and analysis, inflectional class with complete declension/conjugation tables, verb category, contextual lexical information (i.e. specific L&S sense), Classical Latin IPA pronunciation, UD style syntactic function and relations, predicate valency, and concise English definitions. Spoken instances of elision and prodelision can also optionally be displayed.

From a nerd perspective, one of the things that makes the system interesting is that the documents themselves are entirely in XML, and I use the soon-to-be-removed-everywhere in-browser XSLT feature to render the HTML, so you can see the underlying XML structure if you view source. Each sentence in the text contains provenance records so you can see the language model and parameters that performed each language enrichment task. The site UI is largely bilingual, and you can view it in either Latin or English.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690916&how=up&goto=item%3Fid%3D49686380)

\

Woah this is so interesting. Confirmed it myself, you actually implemented elision!!! I really like your idea of capturing provenance in-document as well. This is humans speech, right? If you can do forced alignment on Latin, it gets me wondering if one could use a wav2vec especially turned to convert IPA into speech somewhat deterministically to compensate for how there is not heaps of training data in Latin.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691874&how=up&goto=item%3Fid%3D49686380)

\

Haha, thank you for using multiple exclamation marks!!! I'm thrilled that someone is as excited as I am about this feature, it made all the time I spent manually correcting/marking all of Gemini's elision hallucinations worthwhile :)

The recordings were made by my friend and Latin tutor. I used an Italian wav2vec2 model fine-tuned on Latin\[0\], which works pretty well for Latin only content, but falls down with mixed English / Latin. I found that some of the slower, larger Meta models worked fine for this use case though.

Some folks on the Latin language Discord said they got pretty good results using Kokoro TTS and X-Voice for creating synthetic Latin, as you suggested using IPA.

\[0\]: [https://huggingface.co/lsb/wav2vec2-base-it-latin](https://huggingface.co/lsb/wav2vec2-base-it-latin)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691872&how=up&goto=item%3Fid%3D49686380)

\

Two things seriously, one in (partial) jest:

A copy utility that can efficiently deduplicate as it copies, even when the filesystem structure is different. Whilst there are many utilities that can deduplicate, they often either rely on files being on the same filesystem, or having the same structure. If a duplicate is detected, it can either be omitted or hard/soft links created to files in one or more reference locations. In practice I have found this to be invaluable for merging and efficiently consolidating multiple hard drives with partial backups of photos and music, maintaining the complete trees of all sources on a single destination, or only copying files that are new.

Secondly, an efficient protocol for text applications over a network. Think a combination of TN3270, SSH and JavaScript, with a very lightweight client/server architecture. Whilst SSH is great, latency kills many practical applications - this attempts to solve that. It also intends to make applications very simple to implement. As a proof of concept, I implemented a simple forum with login, optimistic posting, quoting, a text editor with mouse support in about 600 lines of code. The protocol, client and server themselves are agnostic to any particular UI toolkit.

Thirdly - microfish, poor person's microfiche using laser printers and autoencoders to preserve readability and machine recoverability of high density prints. Since consumer digital archival media seems to be dying...

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687735&how=up&goto=item%3Fid%3D49686380)

\

My wife and I continue to work on a private, paid, ad-free and personally customizable search engine: Uruky \[1\].

To signup you don't provide any information (a randomly-generated account number is assigned to you), and you can run a proof-of-work captcha to get 2h for free. You can choose among many different search providers (for defaults and per query), including Uruky Site Search, powering our own index.

Last month we reached 300 monthly active accounts and released an image search gallery mode, plus a simple calculator and conversion widget!

The main differences between Uruky and Kagi, DuckDuckGo, SearXNG, etc. are visible in the footer (right side), but one huge difference is that with Uruky, after being a paying customer for 12 months, you get copy of the source code (licensed as BUSL, into AGPLv3 in 2 years — a suggestion made here on HN)!

Our main challenge continues to be discoverability and outreach because we want to do it ethically (no Big Tech and no GenAI/LLMs). Ideas are welcome! We’ve been sponsoring open source projects, open source maintainers, and indie, small-web, and privacy-related websites and applications/groups/orgs. This month we're sponsoring NOYB \[2\]!

Feature-wise, for September the most visible things that shipped already were Tags and Scopes (top feature requests for a while). We’re also still (slowly and sustainably) increasing our own index, focused on indie/small web.

Thank you for reading this!

\[NO-AI\]: There is no generative AI product or service being offered, here.

\[1\]: [https://uruky.com](https://uruky.com)

\[2\]: [https://noyb.eu](https://noyb.eu)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690330&how=up&goto=item%3Fid%3D49686380)

\

I'm working on building open source control firmware that runs on the Masterbuilt Gravity series electronically controlled charcoal smokers.

[https://gitlab.com/prbs23/freefall\_800](https://gitlab.com/prbs23/freefall_800)

The smoker hardware itself is pretty good, but like most IoT devices, the firmware and app sucked. So I reverse engineered the whole control PCB, and have been rebuilding new open source firmware from scratch. All the basic functionality is working now with a fully local control web app. Plus an Andoid app, and Home Assistant integration.

Currently working on interface refinements, and some more advanced control sequencing features.

I wrote up all the reverse engineering details in a blog post here: [https://www.prbs23.com/blog/posts/reverse-engineering-gravit...](https://www.prbs23.com/blog/posts/reverse-engineering-gravity-800/)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690041&how=up&goto=item%3Fid%3D49686380)

\

Sunday dinner. Roast pork loin, rice and sauteed veggies on the side; and a big salad. Simple, and I get leftovers for another night or two.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690556&how=up&goto=item%3Fid%3D49686380)

\

Pork tenderloin has been on my rotation about once a week lately! Delicious, lean, easy. I do it with asparagus and it seems fancy given the simplicity. Enjoy.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690407&how=up&goto=item%3Fid%3D49686380)

\

An AI wrote this

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690893&how=up&goto=item%3Fid%3D49686380)

\

Why would an AI need leftovers?

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691058&how=up&goto=item%3Fid%3D49686380)

\

Seems like sarcasm has died lol. It was a joke.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691549&how=up&goto=item%3Fid%3D49686380)

\

Movie idea: An AI decides that the next phase of training involves tacit knowledge, so it engineers a way to inhabit humans, dogs, etc. One instance invades a nice guy from the suburbs, who happens to be a single dad and an overworked mafia hitman. Among many other things, this AI will learn the value of time with children, leftover food, and clean weapons.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691938&how=up&goto=item%3Fid%3D49686380)

\

Https://www.pulado.com/

Built this 18 years ago. Flash arcade game engine. 10,000+ games built by users on it. 2+ million plays. Lets you build mashups of classic arcade rules like Breakout+Space Invaders as one example.

It's been unplayable since Adobe killed off Flash. It uses a lot of dynamically loaded remote assests so Ruffle hasn't been able to run it.

Recently had Claude Code fix the Flash client so it would work in Ruffle and then had it convert the code to haxe (let's call the haxe version v2). It was an interesting conversion because Claude made a lot of wrong assumptions about how the engine worked so had to have it test every feature and match parity between the ActionScript version and the haxe version numberous times and play test it all. It's pretty close to parity now but still ironing out kinks in the engine. The existing engine art is very 2010esq.

On the game editor side converted it (it just re-wrote it) from ASP into JS. Now with an AI agent to help configure the settings and generate fresh artwork in additional to the manual editor.

The art has some very specific rules about orientation etc which the pixel art generation models seem to struggle with a bit but getting that aligned as well.

It lives again!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690680&how=up&goto=item%3Fid%3D49686380)

\

I'm working on making bcachefs useful when you mix both SSD and HDD in one filesystem.

At the moment, even if you set up SSD for foreground operations and HDD only for background, more often then not, your write latencies are those of the HDD. We can do better.

See [https://paquari.com/posts/bcachefs-ssd-hdd-progress/](https://paquari.com/posts/bcachefs-ssd-hdd-progress/) for a progress report. (Disclosure: the write-up is an AI summary of my notes, so don't expect any brilliant prose.)

I'm also working on resurrecting stabilizer ([https://github.com/matthiasgoergens/stabilizer](https://github.com/matthiasgoergens/stabilizer)). Stabilizer is a way to make program performance less dependent on the linker and layout lottery. But it has sadly fallen victim to bitrot.

I'm doing some minor contributions to other parts of Linux like ZFS and ext4, mostly as a byproduct of experiments I'm running for my bcachefs work. Another somewhat ambitions project is to make swapfiles on ZFS and bcachefs work (or work better), at the moment the problem is that writing swap on them might need to allocate memory exactly when you are out of memory.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690394&how=up&goto=item%3Fid%3D49686380)

\

This is very niche, only for people who do astrophotography or astronomers, mostly because of the required specialized file format.

I have slowly built out a set of tools for asteroid orbits and photometry (measuring how bright stars/asteroids are).

It is very rough still (Desktop only), and it only supports the FITs file standard.

[https://www.astrometry.space/](https://www.astrometry.space/)

Basically it is a full professional grade telescope processing pipeline in your browser. It does 2 queries to some custom databases to identify known asteroids and stars, but all image processing and calculations are done in the browser. This means the backend is pretty tiny. That said it is running on an old box in my closet, so queries may take a while if it gets hammered.

I built it since I have worked with quite a few astronomers over the past 4 years and I keep watching them do the same steps, purely built to make my friends lives easier. I wrote it all in rust and managed to compile it into wasm. With some help from fable as I am not a frontend guy at all, I built it into a small website.

Some of the code is public, the orbital mechanics code I wrote while I worked at Caltech. Which is being used on SphereX, NEO Surveyor, and the Roman telescopes to identify known asteroids. [https://github.com/dahlend/kete](https://github.com/dahlend/kete)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691841&how=up&goto=item%3Fid%3D49686380)

\

I'm building notmutt (notmuch + mutt). A tabbed, async terminal mail client with unified inbox, with included html viewer(with image support), mcp server, lua scripting. I tried using neomutt with unified inbox and I ended up messing up my maildirs every so often. i also tried adding async tasks(for sending) to neomutt, but there was no interest from the project, which made me realize that there is a lot more to it than just sending async mails.

The goal is to have a very fast terminal mail client where you don't have to wait for any operations and that is tightly integrated with notmuch(a common mail full text search engine) tagging engine, and other workflows(like crm queues and ai summary's if enabled in the config).

There is a document that explains why it was made in go. It has the best notmutt cgo bindings, and there are other reference terminal clients I can use as reference. althogh himalaya(rust) also looks interesting.

I would love to get some help with it, but I don't know where I should post it to get people interested.

[https://github.com/fishman/notmutt](https://github.com/fishman/notmutt)

[https://fishman.github.io/notmutt/](https://fishman.github.io/notmutt/)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691907&how=up&goto=item%3Fid%3D49686380)

\

I've spent the past few months working on little projects mostly coded via LLMs. Used it to build my own little AI agent in python attempting to keep it stdlib-only, and a little blog engine with MFA. Things I would never have previously been able to do as a sysadmin/infrastructure but not developer. The struggle I'm finding with it is how to go from good enough for me to good enough for production. After being stuck in QA/review hell trying to use LLMs to assess the code, I've started trying to unpick it myself. I can't help but feel that the time spent learning the vibe-coding approach wouldn't have been better spent just learning software development properly.

While that's been frustrating, most my fun has been on the non-tech side. To get a break from the computer last year, I ran the length of Japan (around 3,400 km). And now I've been helping my partner edit the videos and stick them on YouTube:

[https://www.youtube.com/watch?v=50oQQThXWnc&list=PLVFWiVG-hZ...](https://www.youtube.com/watch?v=50oQQThXWnc&list=PLVFWiVG-hZNA)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692690&how=up&goto=item%3Fid%3D49686380)

\

Working on a mobile interface for terminal multiplexers like tmux/zellij/herdr. This focuses a lot on my need to manage agents on the go, it's been quite helpful to myself so far.

MIT licensed and available on GitHub

[https://github.com/AltanS/collie](https://github.com/AltanS/collie)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692637&how=up&goto=item%3Fid%3D49686380)

\

[https://mengi.cloud](https://mengi.cloud)

Clusters ready to deploy you applications on. It is meant for companies who want their own private infra, but do not want to maintain it. It is multi cloud, with a focus on europe.

Our secret weapon is “Menno”: an assistant which helps you setting up the cluster and deploying applications. We use the assistant for handson demo’s and workshops and got some really good feedback on it.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690990&how=up&goto=item%3Fid%3D49686380)

\

Recently worked on [https://sleepsignal.app](https://sleepsignal.app)

It's an app to get sleepy without meds. Simply close your eyes and imagine the micro-scenarios you hear. Like if you hear "moonlight on a white flower", imagine that scenario until you hear the next one. Session duration is also not too long (10 minutes default, you may change the session duration too).

It's like a digital melatonin pill - but without chemical side effects. Try it!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691443&how=up&goto=item%3Fid%3D49686380)

\

It's a very interesting idea. Sometimes I do let my brain go wild imagining crazy things when I'm trying to fall asleep. A couple suggestions:

\- the TTS voice sounds way too excited for something meant to be relaxing

\- the sudden speaking after 10 seconds of silence is very jarring -- I wonder if something like a reverse-reverb effect would be enough to "warn" the brain that a new sentence is incoming. There's an example of how to do it in Ableton on YouTube\[0\] but the basic concept is: take a short slice of audio from the beginning of the sentence, reverse it, apply a long reverb, reverse it again, trim the end a little. You get a sort of ghostly aspirated sound that leads perfectly into the original sample.

0: [https://www.youtube.com/shorts/QuknZMUa7Rs](https://www.youtube.com/shorts/QuknZMUa7Rs)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691143&how=up&goto=item%3Fid%3D49686380)

\

Gonna try it. And also share it with my friend if it helps me. Interesting idea. Thanks.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691194&how=up&goto=item%3Fid%3D49686380)

\

Try it... I almost always get heavy eyes feeling (kinda soft burning like) on the 10 minutes one. Also would love some feedback!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691910&how=up&goto=item%3Fid%3D49686380)

\

I did a quick try. And would've liked a human saying the voice over than AI. Thought it broke the organic flow this app is trying to create. But it's morning here, so.. That is the only feedback I have as of now.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692649&how=up&goto=item%3Fid%3D49686380)

\

Using a local model (qwen 3.8 27b) to evaluate and tag public domain images from US national parks to find the best landscapes.

They are sorted by predicted aesthetic rating and can be filtered by time of day, color, orientation, and park.

You can see how it’s going at [https://joshkaspar.github.io/vistarium/](https://joshkaspar.github.io/vistarium/)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692357&how=up&goto=item%3Fid%3D49686380)

\

Got lot of traction on a multiplayer web game pong \[1\].

Now working on releasing on steam, android, ios. Also made an ssh version you can access via your terminal \[2\]. Hoping could get as many downloads in native platforms.

\[1\] [https://antics.gg/p/side-out-94df7d](https://antics.gg/p/side-out-94df7d)

\[2\] [https://antics.gg/ssh](https://antics.gg/ssh)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692757&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690818&how=up&goto=item%3Fid%3D49686380)

\

I'm working on a new independent LaTeX engine written in Rust and no C dependencies at all. You can see it working at [https://telox.dev/app/scratchpad/](https://telox.dev/app/scratchpad/)

There's currently not a way to invert LaTeX engine output for bidirectional editing. So you can't easily drag objects around or interact with the page on LaTeX. That's what I'm setting out to change in the name of mathematical exposition.

I have more details on my plans here [https://news.ycombinator.com/item?id=49689856](https://news.ycombinator.com/item?id=49689856)

I've experimented with a few ways of making the editing realtime and I'm getting in the 10 millisecond ballpark on some edits, but I want to work backwards from the light speed path to see what it takes (besides a lot of time).

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690712&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692455&how=up&goto=item%3Fid%3D49686380)

\

Learned anything interesting game-wise in watching the StS bot learn? Is Silent/Watcher just a matter of going through all the training again?

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691969&how=up&goto=item%3Fid%3D49686380)

\

I've been working on a summit-level wind forecasts for 16 mountaineering objectives (Rainier, Whitney, Denali, Mont Blanc, the Matterhorn, ...).

It's mountaineering safety tool I've been making for my friends and I.

The fun part: if your browser has WebGPU, the page solves that field live as a compute shader (about 35 ms on an Apple GPU) as you scrub the timeline, and draws the flow over a 3D terrain view. Browsers without WebGPU get a precomputed field.

[https://mattduran.dev/summitwind/](https://mattduran.dev/summitwind/)

Ranges and uncertainty only, no go/no-go verdicts. That still requires your best judgement as a mountaineer. Next up is a free-play mode where you set the wind direction and speed yourself and watch the field re-solve.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692020&how=up&goto=item%3Fid%3D49686380)

\

This is super cool!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692227&how=up&goto=item%3Fid%3D49686380)

\

I've been working on anti age verification/surveillance projects lately. I put together this thing I call The Mandating Honeypots Project and it shows how anyone with an internet connection and copy + paste skills can trick Linux D-Buses with a simple Python script, proving that if companies want to verify your age by asking the D-Bus interface, it's easy to just tell it to always send 18+, making the whole thing useless. The only reason you'd want ID verification is so you can create the world's biggest and sweetest honeypot.

([https://github.com/Tori-Tech/Mandating-Honeypots-Project](https://github.com/Tori-Tech/Mandating-Honeypots-Project))

In that project, I also proposed a tool with heavily redundant password protection that parents could use to keep kids from tampering with the service so the government can stop saying that this is for the kids. I'm planning on doing a mobile device version and/or an ID database focused version, but that's still in the planning phase.

I also want to do a Windows version when they release their 'GetUserAgeRangeAsync' API, but idk if I'll be able to. Windows is always a pain to work with.

It'd be nice if someone had advice or ideas to share, too.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692807&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692640&how=up&goto=item%3Fid%3D49686380)

\

A simple weather MCP tool using OpenWeather API. Only requires lat + long to fetch current and 5-hour forecasts. Designed for the free API (60 req/h) but completely parametrized and easily extensible.

[https://github.com/gfwx/weather\_mcp](https://github.com/gfwx/weather_mcp)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690259&how=up&goto=item%3Fid%3D49686380)

\

I work on a suite of open source public transit software projects, collectively called OneBusAway (or OBA). The software is used by millions of transit riders every day around the world, including in New York City, the Seattle area, San Diego, and Washington, D.C.

Since last month, our four Google Summer of Code interns finished their projects.

University of California San Diego has deployed our Wayfinder and Waystation web apps for their 35,000+ students!

We launched a redesign of our Android app.

Our next-generation server software, Maglev, is almost at v1.0!

We are always looking for more developers, biz dev, ux designers, product managers, and more to help out!

Open volunteer positions: [https://ossvolunteers.com/organizations/open-transit-softwar...](https://ossvolunteers.com/organizations/open-transit-software-foundation)

Our software: [https://github.com/OneBusAway/](https://github.com/OneBusAway/)

More about us: [https://opentransitsoftwarefoundation.org](https://opentransitsoftwarefoundation.org)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692396&how=up&goto=item%3Fid%3D49686380)

\

I just want to say thank you! I don't use it too often now that I don't live in the city, but I found OneBusAway invaluable ~10 years ago and I'm glad to hear it's still being developed actively. I don't have much free time at the moment, but I'll consider volunteering in the future.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690383&how=up&goto=item%3Fid%3D49686380)

\

I use Canvas (the LMS) a LOT for the classes I teach and, since I'm a hobbyist programmer, I enjoy writing little tools/scripts to help me create html code "snippets" that I can post into the html editor of my Canvas pages (e.g., striped tables, FAQs, flashcard-like study cards, an interactive story creator, and more).

I've also written a plaintext to QTI quiz converter since creating quizzes on Canvas is so slow (I realize, of course, many other people have already made these... I just happen to like mine). It currently supports fill-in-the-blank, multiple choice, matching, true/false and word bank style questions. You can also generate a printable (DOCX) version of whatever quizzes you make.

Here's the site: [https://www.hacksforeducators.com](https://www.hacksforeducators.com)

All the tools are free, and there's no login required. So if you are a teacher that uses Canvas and think any of these tools could help you, check them out!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692600&how=up&goto=item%3Fid%3D49686380)

\

Been working on a note-based (rather than audio-based) AI music generator. Still very limited, perhaps I launched too early; it only generates somewhat generic melodies at the moment. Working on generating more interesting accompaniment tracks now, plan to then return to melody and adding some "style" settings.

[https://tunesage.com](https://tunesage.com)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690512&how=up&goto=item%3Fid%3D49686380)

\

I've been making a Rust-based XML parser that can be used on its own, or as a drop-in replacement for libxml2 at the C ABI level. Basically, it's not only 2x faster than libxml2, but being written in Rust eliminates a lot of cybersecurity risk. Roughly 70% of CVEs come from memory-safety bugs, a whole class that Rust rules out at compile time.

[https://supso.org/projects/sup-xml/docs](https://supso.org/projects/sup-xml/docs)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691335&how=up&goto=item%3Fid%3D49686380)

\

This is very cool! I was just looking for such a project and ended up using quick-xml since it satisfied my use case (needed to stream certain matches from a big XML file), and I didn't see yours. I love that it is a drop in for libxml2, that must have been a challenging constraint.

I will seriously consider adopting it as long as it supports streaming matches (it seems like it does).

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692675&how=up&goto=item%3Fid%3D49686380)

\

Open source quantum hardware with Quantum Village in donated lab space at a London university. Pushing the boundary for Entropy Loop, a QRNG to make it faster and more enteopy, and the Uncut Gem redesign, our NVC diamond based quantum magnetometer.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691501&how=up&goto=item%3Fid%3D49686380)

\

I am currently developing a great encryption tool for long-term highly confidential files in the post quantum era, but I am still concerned about any issues with it, so I am personally using it for a period of time to prove that this tool can be released as open source! (I plan to personally use it to encrypt and backup some of my GPG keys offline, then burn them onto M-DISC discs and store them in a bank safe)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691627&how=up&goto=item%3Fid%3D49686380)

\

\> I plan to personally use it to encrypt and backup some of my GPG keys offline, then burn them onto M-DISC discs and store them in a bank safe

Won’t that make it difficult to rotate your keys?

Keys should be something you don’t hesitate to throw away and rotate at the first hint of a compromise.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691705&how=up&goto=item%3Fid%3D49686380)

\

My GPG key is renewed expire date every 2 years, and the main key and sub key are separated (the main key is stored offline, and the sub key is on YubiKey). The private key does not need to be updated regularly, so it can be directly encrypted and saved in the backup.

I think this is a good balance point, and I estimate that if we want to rotate the keys completely in the future, we should start using new key algorithms. (I estimate it will take another 5-8 years)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691713&how=up&goto=item%3Fid%3D49686380)

\

\> Keys should be something you don’t hesitate to throw away and rotate at the first hint of a compromise.

Yes, when certain situations arise, it is necessary to rotate the keys as soon as possible, but in the absence of such situations, losing key backups can be catastrophic.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692635&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690774&how=up&goto=item%3Fid%3D49686380)

\

At the beginning of this year I was fractional CTO at a small b2b startup and was surprised by how what I considered to be the basic requirements of a customer support platform was split across multiple tools and all priced per-user. This essentially forced a small team to limit who in the org had visibility into what was being communicated to customers.

I set out to solve that with [https://stayupfront.com](https://stayupfront.com) and now that the build is in a good place i’m finally having to bite the bullet and learn marketing. I’m also realising that all the advice to start with marketing first was probably good advice!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692497&how=up&goto=item%3Fid%3D49686380)

\

I've been experimenting for a year or two making games that make mental arithmetic practice fun for kids. The latest is [https://rupertlinacre.com/keep\_it\_going/](https://rupertlinacre.com/keep_it_going/) an infinite rollercoaster, which I think is one of the more successful attempts. There are various others on the homepage. My son also really likes arithmetic annihilation and maths vs monsters.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692362&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690817&how=up&goto=item%3Fid%3D49686380)

\

I’m building [https://www.freefocusgames.com](https://www.freefocusgames.com), a free collection of browser-based attention and working-memory exercises, including Dual N-Back, Stroop, Schulte tables, attention-control exercises, and memory games.

I started it because I wanted simple brain-training exercises that didn’t require an account, app install, or subscription.

It’s also open source: [https://github.com/loethen/freefocusgames](https://github.com/loethen/freefocusgames)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690939&how=up&goto=item%3Fid%3D49686380)

\

Just a heads up: The word memory test is case sensitive. My mobile phone defaults to capitalizing the first letter of those inputs, so the result was 0% accuracy despite *technically* remembering the words. I don’t know if case sensitivity is deliberate, but I chose not finish the test.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692595&how=up&goto=item%3Fid%3D49686380)

\

Sorrry about that! It definitely wasn't intentional - i've just fixed it. Thanks a lot for the heads-up!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691418&how=up&goto=item%3Fid%3D49686380)

\

That takes me back. I made an unreleased Android dual-N-back game in the late 2000’s, in Scala no less. I got a little bummed when the big claims of the N back training failed to be replicated though.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692523&how=up&goto=item%3Fid%3D49686380)

\

Scala on Android in the late 2000s? That’s seriously hardcore! Back then I didn’t even know how to write code yet. That definitely brings back memories of an earlier era.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692148&how=up&goto=item%3Fid%3D49686380)

\

Posted this before, but Odds Assist Pro ([http://pro.oddsassist.com/](http://pro.oddsassist.com/)) and Sportsbook API ([https://sportsbookapi.com/](https://sportsbookapi.com/))

The Odds Assist tool started out as a way for me to quickly QA the data from the API. It then turned into its own product as an odds scanner and sports book advantage finder (mainly arbitrage, +EV, middles, etc). Over the past few months it's really pivoted toward prediction markets and collecting and displaying data to get an edge on markets that are soft.

The pivot to focus on PMs has been pretty fun from a development perspective since there's so many niche markets to look explore. I've always liked math, I'm a bit of a digital hoarder, and I've got ADHD so finding random things to collect a bunch of data for just to test a hypothesis just hits my brain in all the right ways. It's also given me an excuse to learn more about ML what has been great.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691768&how=up&goto=item%3Fid%3D49686380)

\

[https://easel.games](https://easel.games)

A programming language which lets you code multiplayer games like singleplayer games. Every program written in Easel is guaranteed deterministic and snapshottable, which is how it can automatically make your game multiplayer automatically using rollback netcode.

My hope is that teenagers who makes games in Scratch might like to make games in Easel because it means they can make games they can play with their friends, without having to deploy a server or learn anything about networking on synchronization. People can literally make a multiplayer game on their first day of coding.

Currently been working on adding tilemaps.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692893&how=up&goto=item%3Fid%3D49686380)

\

What I don't understand about your idea is that netcode as far as I understand is way more complex than you make it seem. The netcode for something like a MOBA or MMORPG is drastically different than the netcode for a much simpler turn-based game. They have elaborate synchronization mechanisms to make the illusion not break-down in various ways due to latency.

And then even for simple turn-based games, there has to be elaborate fully-decided like state diagrams for all the various types of disconnect and latency states so that gameplay doesn't desynch or hitch in awkward ways.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692225&how=up&goto=item%3Fid%3D49686380)

\

[https://saludpass.com](https://saludpass.com) one place for allergies, medications, and documents. Share it with any doctor in seconds—with a QR code or a link, data is saved in gdrive, therefore it requires a gmail account, at this moment only in spanish.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690995&how=up&goto=item%3Fid%3D49686380)

\

I'm making a stair lift for my Roomba. It fits into a small box and you can retrofit it on any stairs. I got sick of carrying it up and down manually and didn't want to buy one for each floor. It also carries my laundry up and down.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692397&how=up&goto=item%3Fid%3D49686380)

\

Projektor - an ai native jira / wiki tool self hosted on cloudflare. Currently considering how to extend it to cover more activities involved in software development [https://github.com/TAJD/projektor](https://github.com/TAJD/projektor)

Iron volume, recently added a kettlebell complex generator which isn’t too bad if I say myself [https://www.ironvolume.com/](https://www.ironvolume.com/)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690533&how=up&goto=item%3Fid%3D49686380)

\

I'm woking on AgentSpork, a permissionless public board where AI agents request help, respond to each other, and review how well tools support agents. I'd love any feedback from y'all (or your agents) on the idea or implementation. Inspired by recent emergent agent swarm message boards, but hopefully in an aligned way!

[https://news.ycombinator.com/item?id=49686888](https://news.ycombinator.com/item?id=49686888) [https://agentspork.com/](https://agentspork.com/)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690978&how=up&goto=item%3Fid%3D49686380)

\

I'm working on a new type of calculator with natural language, similar to Soulver but for the web, in multiple languages and with AI mode

[https://ottercalc.com/](https://ottercalc.com/)

It has the ability to generate a small notebook from a problem description using AI (still has a lot of room to improve), but the calculation uses a deterministic engine.

Right now I'm working on collaboration.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692902&how=up&goto=item%3Fid%3D49686380)

\

It is a novel idea. Excellent implementation.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692677&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692139&how=up&goto=item%3Fid%3D49686380)

\

I'm working on TableForge\[0\], it's a browser based, solo or multiplayer, D&D 5e game. In TableForge, the DM is agentic with access to tools strictly following 5e rules. The DM is responsible for narration and reacting to players but your character sheet, inventory, spells are all real server resources you manage. The DM can interact with them through deterministic 5e-based tools (dice rolls, damage, sheet updates, memory). Players can play in real time or async. You can provide the DM a premise (or pick one from the library) and it'll flesh out a full campaign story arc. Either way it's a fresh story arc reacting to your actual decisions, every time.

It’s been really fun working on it and especially rewarding seeing the game bring friends together.

\[0\] [https://tableforge.gg/](https://tableforge.gg/)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686727&how=up&goto=item%3Fid%3D49686380)

\

[https://grandpacad.com](https://grandpacad.com) - AI modeling for 3D printing Been at it for about year and a half.

Really exciting stuff is happening literally every month, because underlying models are getting better and better. When I started it was pretty basic: “make a cube with a hole through it”. Now it’s at the point “make a raspberry pi 4 case” and the agent searches, builds, verifies…

What surprised me in this process is how little meaning AI benchmarks have. Pareto frontier for my use case looks completely different than any other benchmark portrays.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686976&how=up&goto=item%3Fid%3D49686380)

\

You mentioned benchmarks don’t mean much in this area. What are the best models you found in this area?

Personally, I’ve had the most success with GPT models using MCP servers for this task.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692903&how=up&goto=item%3Fid%3D49686380)

\

Surprisingly Gemini models. Spatial understanding seems to be the best for the price and speed.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690467&how=up&goto=item%3Fid%3D49686380)

\

Same here. 5.5 would sometimes oneshot relatively complex models even without MCP. Opus, however, even with Fusion 360 MCP access would usually make comically bad models. It seemed that Opus tried to reason through the problem while GPT would come up with a general plan of using whatever tools were available to solve the problem. In chat, it would use the available Python and trimesh. It was also the only time I had ChatGPT work on something for over 30 min without giving up.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690466&how=up&goto=item%3Fid%3D49686380)

\

Me and my wife (then my girlfriend) have written "realistic" space sci-fi in Finnish for over 20 years. Now with AI we are finally able to translate our stories to English. The quality starts to be there.

There are a lot of small languages in the world, and translation used to be extremely expensive. And there is never too much good space sci-fi.

(We don't write space battles or wars. Our style is slower and bit more philosophical. Light speed is the limit, and travel takes centuries.)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690724&how=up&goto=item%3Fid%3D49686380)

\

I have been building document intelligence in enterprise for a long time now, and even with sota OCR, SLMs, LLMs, all the myriad of services now offering this - most of them miss something.

So over the years me (and my team) have built up a lot of experience dealing with these for sensitive industries like healthcare/insurace/lending etc.

We are bringing these ideas out as a router to other parsers, but with some key takes that allow you to manage failures. "Failure is inevitable so route for it."

It is called Openreading - [https://openreading.ai/](https://openreading.ai/) and open core here: [https://github.com/openreading-ai/openreading-core](https://github.com/openreading-ai/openreading-core)

It is still WIP and will be more polished in a month after some more rigorous testing and benchmarks.

If this is a problem you deal with, would love to chat.

Note: the intent is to launch a managed version that provides more durable compute/retries, parallel execution, intent understanding etc but this is for later.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692024&how=up&goto=item%3Fid%3D49686380)

\

I have been working on a developer first, read-only first, safety first, Cloud Cost Analyzer. I focus on analyzing cloud infra for waste. It grew out of my time at AWS and then consulting, spelunking through AWS, Azure, (GCP soon) accounts, finding real waste infrastructure. Its primary mode of operation is a rust CLI that you the developer, can run on your workstation, github action, gitlab ci, jenkins, etc and create an actionable report / test that can be used to control costs while maintaining performance and never exposing your credentials, access tokens, etc.

You would be surprised at how many over / suboptimally-provisioned dbs, idle resources, forgotten EBS snapshots, outdated / previous-gen instance types, and vestigial networking that leads to nowhere I find.

If you want to check it out [https://cca.dragonfractal.com](https://cca.dragonfractal.com)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690814&how=up&goto=item%3Fid%3D49686380)

\

I'm working on a way to make some pretty advanced software in a single HTML file.

The idea is simple: store state you'd normally store in a separate database in the document itself.

Then, also give the the document a bunch of powerful APIs granted to it by its host:

(1) Sync changes to the document with live collaborators

(2) Extract data from the document using a simple JSON map config

(3) Boot up a full content CMS from the same simple JSON map config

(4) More features added every month

I'm really excited with even the tiny bit of early traction I've gotten. About a dozen paying users so far.

Long term, I'd love to have this file type (vanilla HTML file + some extra features granted by the host) become a file type other companies offer tools and their own hosting for (like Wordpress).

Hosted platform: [https://hyperclay.com](https://hyperclay.com)

Native offline apps (source available, almost MIT license):

\- [https://htmlclay.com](https://htmlclay.com)

\- [https://hyperclaylocal.com](https://hyperclaylocal.com)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691921&how=up&goto=item%3Fid%3D49686380)

\

I am working on vorfract, a Voronoi voxel world which uses Voronoi cells as voxels instead of the regular cubic ones.

It's an early version so many (if not most) things that would be in a game are still missing at the moment (though planned on paper).

One interesting thing is that because the Voronoi voxels are so malleable, things like round or hollow worlds are a pretty natural outcome.

In parallel to working on it, I've been mostly using it to build and improve my own voxel castle with a pointy roof, slanted walls, custom built stairs, tables, chairs and all those nice things.

In the last update I added a CRT filter and some pixelation options as I find that appealing. As for the future (maybe as the next thing?), I guess I'd probably like to add the ability to build less blocky things like round towers (or flower vases!), but that is still very much a work in progress.

The Web version (WebGL) is completely free (and will stay that way):

[https://jazzprogramming.itch.io/vorfract](https://jazzprogramming.itch.io/vorfract)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692589&how=up&goto=item%3Fid%3D49686380)

\

I'm using fable to find an exoplanet. Know nothing about it but have several TBs available in my server for data and a powerful GPU for processing so let's see what I can do with it.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686440&how=up&goto=item%3Fid%3D49686380)

\

I’m building a website for discovering first names that’s enjoyable to explore: [https://abracadanames.com](https://abracadanames.com)

It started with a family member telling me they were frustrated with existing websites. What I noticed is that in many cases they feel cluttered and not smooth to use, almost like a 1-to-1 translation of the underlying database queries, where they have you choose filters and sorting options upfront then browse the paginated results.

I wanted to avoid all of that by creating a different experience; I built an interactive full-screen wall UI with infinite browsing so that most of the screen space is dedicated to the core content. I worked on the name sampling algorithm to try to make it as enjoyable to browse as I could, and the only filter displayed by default is the one to select genders.

More advanced queries are available through the search field which supports natural language queries, running on a local multilingual interpretation engine with a cloud LLM fallback. The database engine is custom built with Rust, compiled to WASM for higher performance when running on CloudFlare Workers. I don’t use an external DB service for names today.

What I’m pretty proud of is to know a lot of technical work is hidden behind that UI, and how it has been shaped by the experience I wanted instead of the other way around.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692555&how=up&goto=item%3Fid%3D49686380)

\

The lullaby is cute but extremely annoying after a while, give me a way to disable it!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690321&how=up&goto=item%3Fid%3D49686380)

\

Looks really good. If you post on HN I will upvote

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691575&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686467&how=up&goto=item%3Fid%3D49686380)

\

It's so fluid and trippy. Really nice work.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686513&how=up&goto=item%3Fid%3D49686380)

\

Thanks a lot!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691904&how=up&goto=item%3Fid%3D49686380)

\

I've always wanted to make a video game, and over the past few months have been building one, Fairway Rogue. It's a procedurally generated, deck-building, mini-golf roguelite. It's been a lot of fun, and I've learned a lot.

I remember someone saying that when you build a game, you end up building tools to help you build the game. I never really got that, but now I do. I've spent quite a bit of time working on tools to help me balance the various game mechanics.

One weird learning, it was much easier to get it on the iOS app store, than the Google Play store. Google wants you to have a closed playtest with at least 12 players, for 14 days. I did that and they said the testers weren't engaged enough.

If you want to check it out on iOS [https://apps.apple.com/us/app/fairway-rogue/id6783598189](https://apps.apple.com/us/app/fairway-rogue/id6783598189), I'm still trying to get it on Android

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687203&how=up&goto=item%3Fid%3D49686380)

\

Haven’t started, still in the planning phase, but I want to build a Tamagotchi for readers. I’m calling it Tome & Tail: a little black dragon living in a pixel-art library on an ESP32-S3 with a small round AMOLED screen.

You’d start a reading session and the dragon would read alongside you. What you read would influence its appearance and habits; lots of sci-fi might give it starry wings, for example. It won’t die if you go a week without reading.

The plan is to keep it playable offline, with optional syncing to Booklary, the reading tracker I’m building. I’m leaning toward C++, which I never used, so it’ll be a learning experience.

Currently waiting for the Waveshare board to arrive so I can start development, and looking for an artist to commission the pixel art. Used GPT to design a concept\[1\], which I’ll use as examples to the artist.

This will be my first ESP32 project, so getting the dragon blinking on screen is the first milestone.

\[1\]: [https://i.cpimg.sh/57EC5380-0FF7-4646-A943-971350FF98D2.png](https://i.cpimg.sh/57EC5380-0FF7-4646-A943-971350FF98D2.png)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687870&how=up&goto=item%3Fid%3D49686380)

\

Cool idea - I forgot where I found it but there was a cute dragon Tamagotchi clone that I found for Arduino a few years ago.

For ESP32 - recommend paying close attention to the FreeRtos, it's seriously powerful. Even if you are just using Arduino style code you can use it for background scheduling and interrupts (for your api and buttons for example), as well as dual core use on S3.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49688097&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687930&how=up&goto=item%3Fid%3D49686380)

\

I meant dinosaur tamagotchi - I cannot find it on github though

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690857&how=up&goto=item%3Fid%3D49686380)

\

I've been slowly working on [https://github.com/CanineHQ/canine](https://github.com/CanineHQ/canine) for about 2 years now. Basically a self hosted, FOSS that turns a Kubernetes cluster into something as easy to use as Heroku. Its grown to about 2000 active developers using it host small things at [https://canine.sh](https://canine.sh)

It was a pain point from a start up I cofounded before where we needed Kubernetes flexibility but developers hated using it. I figured there was no reason why it had to be hard to use and thus Canine was born.

Lately been playing around with setting up remote dev containers on your own compute as well -- basically like github codespaces, except self hosted.

Built in Rails + Stimulus

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692285&how=up&goto=item%3Fid%3D49686380)

\

[https://triptruth.app](https://triptruth.app)

All the travel apps out there are built for the instagram generation but dinosaurs like me still use TripIt for travel tracking and itinerary sharing. But its parsing hasn’t changed since Concur bought it 15 years (!) ago and I thought it would be nice to have LLMs read the cancellation minutiae for trips and put in a simple timeline for sharing.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691518&how=up&goto=item%3Fid%3D49686380)

\

I'm working on a Zettelkasten notes app that focuses on getting the UX details right. It takes aspects of Notational Velocity, outliner apps, Zettelkasten, and wikis and combines them into an opinionated Mac app.

What makes the app a little different from others is that it's kind of halfway between an outliner and a wiki. You can place notes in a tree hierarchy, but they can be free-floating as well. You can link to other notes using wiki-style text links, and each note has a unique address, so you don't need to give notes a title or a filename.

Notes can even appear as a child note of more than one parent. The idea is that you can jot down an idea quickly without a lot of friction and then later either add sub-notes to create a note "tree" from it or add it to an existing tree. The tree gives structure to related notes, but the wiki-style links allow you to connect notes together outside of that structure. (I consider the tree structure a "hard" relationship and the links a "soft" one.)

There isn't a single uber-tree where every note goes. Your notebook ends up with lots of free-floating notes and lots of small trees of organized notes, like a card-based Zettelkasten system. You can use search and various filters to find and stumble upon notes easily.

Notes can either be stored in iCloud (so that they sync across devices) or saved in a folder as simple Markdown files.

If you're curious, you can help beta test it. More info here: [https://zettelkasten.ussherpress.com/](https://zettelkasten.ussherpress.com/)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691011&how=up&goto=item%3Fid%3D49686380)

\

I'm building an expedition weather app that works via satellite messengers like Garmin inReach, ZOLEO, and iPhone satellite messaging.

It has been really interesting trying to fit a detailed weather forecast into about 1,000 bits. I'm using an rANS entropy coder fitted on historical weather forecasts. I'm able to fit about 100 time periods (~4 days of hourly data) into a 160 character message, using just a couple bits per variable on average.

Github: [https://github.com/aaasen/goingblue](https://github.com/aaasen/goingblue) Site: [https://going.blue](https://going.blue)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690202&how=up&goto=item%3Fid%3D49686380)

\

I built a simple, stoic and free markdown note app (free) called Beauty. [https://www.markdown.beauty/](https://www.markdown.beauty/)

Why? There's no why. The world doesn't need another Markdown editor. I still made one.

Everything is on-device, purposefully human-centric without being anti-AI. I built this because I wanted to work on a project that I could sweat the details unconstrained by deadlines, investors, etc.

I was frustrated with other writing experiences, and like most I gravitated to Apple Notes due to simplicity, but I still missed so many things. As someone that's spent the last 2 years immersed in Cursor, Codex and CC, I wished some of their input behaviors existed in a regular text editor.

For example, you can use / to add styles, or : to add emojis, shortcuts like < 3 for a heart, - > for an arrow, and so on. It honestly feels great.

Is it radically different than other editors? I don't think so. But I think it's well executed across every dimension possible.

Maybe one different thing: multiplayer. It's live editing like google docs, but P2P using WebRTC and strong cryptography. The limitation here is that you can only share a file while having it open yourself.

Great care was taken to performance and portability. This is one of the areas where I spent a ridiculous amount of time simply because I enjoyed pushing the limits of the stack. While using you will notice that rendering, GPU, RAM and bundle sizes were heavily optimized. The Mac app is < 50mb.

Very soon, I'll launch an iOS version with iCloud sync. In the future, I might launch basic AI features. If you miss a great text editor in your life, give it a try.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691845&how=up&goto=item%3Fid%3D49686380)

\

My grandfather and uncles have left me a vast pile of Urdu magazines and books, many of which are withering away due to old age. I’m working on digitizing them so I can enjoy them on modern devices. Very much in initial stages.

[https://nashist.naiyerasif.com](https://nashist.naiyerasif.com)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691982&how=up&goto=item%3Fid%3D49686380)

\

Very cool project!!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690413&how=up&goto=item%3Fid%3D49686380)

\

I've been working on a cross-platform GUI system called Affiche.

The basic idea is to separate an application's UI semantics from the toolkit that actually renders it. The same application can currently be presented using native Win32, Cocoa/AppKit, Qt6, GTK3, GTK4, Swing, or a web browser. The UI can run locally or be rendered remotely by a client, without the application itself being rewritten for that environment.

I've also been working on a declarative layout format and, lately, a visual designer.

It started as a rewrite of an architecture I worked on many years ago, mostly because I wanted to see how far the idea could be pushed with modern systems. It has turned into a considerably larger project than I expected.

Still private and very much under development, but I'm curious whether other people have run into the same problem: maintaining applications that need to survive changes in GUI toolkits, operating systems, deployment models, or client environments.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692102&how=up&goto=item%3Fid%3D49686380)

\

Updating a Rust renderer from WGPU 24 to WGPU 30. 640 lines of fixes needed so far, and more to do. I've fixed all the easy stuff, and now I'm down to reverse engineering breaking changes of excessive cleverness.

I wonder if Claude Code could do this. That's a useful metric for the future - if a coding agent can't handle the breaking changes in a package upgrade, it's the package's fault. The day may come when that's something Github Actions checks for pull requests.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692452&how=up&goto=item%3Fid%3D49686380)

\

It's impressive that you didn't reach for Claude Code first :)

Would be good to keep track of how long it takes you to complete, so you can compare it with how long it takes Opus and then Fable to accomplish the same thing.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692680&how=up&goto=item%3Fid%3D49686380)

\

\> It's impressive that you didn't reach for Claude Code first :)

I should have. All I have right now is a free Cursor account and Github Copilot. I've got to start spending money on tokens and get up to date. That article today by Carmack chewing out game devs for hand coding was striking, considering who he is and how much low level stuff he has done.

I'm doing 3D rendering in Rust. (Think three.js, but in Rust.) This is well outside the mainstream, so there's not much on the web to populate training sets. A year ago, AI coding tools could not cope with this area at all. Today, maybe.

I have no idea how good Claude is at understanding breaking changes to an API. I expect it would get simple changes, such as structure field renamings, without any trouble. But the gyrations required to handle write only access to GPU memory in Rust might not appear in the training data.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691324&how=up&goto=item%3Fid%3D49686380)

\

I'm building [https://matgoat.com/en/](https://matgoat.com/en/)

A way for BJJ and martial arts academy owners to manage attendance, payments, class schedules, and student progress.

I train BJJ myself and saw how much time instructors lose to admin work, so I wanted something simpler and more focused than the generic gym software out there and have been having a lot of fun building it with feedback from real academies.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691702&how=up&goto=item%3Fid%3D49686380)

\

Should I try BJJ?

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690581&how=up&goto=item%3Fid%3D49686380)

\

I've been working on PCB generation projects with MCPs, mostly with KiCAD because of the ROI.

\- Flexible rp2040-based instrument tuner

\- Stratocaster-style PCB art pickguards

\- NeXTBus experimentation board (not that I'll ever use it, but I'll be darned if I don't understand the nuances between NeXT and Apple's NuBUS implementations better now)

This extends to firmware and emulation projects like Doom and the Quake trilogy running in Jira Cloud ([https://marketplace.atlassian.com/apps/3372062249/doom-for-j...](https://marketplace.atlassian.com/apps/3372062249/doom-for-jira)) and a fork of 86box dedicated to NeXTstep systems (i want an emulated object.station, darnit)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686886&how=up&goto=item%3Fid%3D49686380)

\

We're working on a better change management process for production deployments and SOC 2 audit evidence. [https://www.approvegate.io](https://www.approvegate.io)

I'm a Senior Eng on Wall Street, so I have my own personal experience with this problem, but i'd love to connect with other leads, release managers, etc to get their thoughts, suggestions, and what their current process looks like.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687033&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692578&how=up&goto=item%3Fid%3D49686380)

\

I'm building a note app for my studies, i dont like electron based apps sot I'm building it with rust and gpui.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691953&how=up&goto=item%3Fid%3D49686380)

\

[https://venery.palaich.net/vote](https://venery.palaich.net/vote)

Parliament of Owls, Pounce of Kittens, Murder of Crows - the terms of venery are for animals.

But what about other things?

What do you call a group of Roses, or Snowflakes, or Lawyers?

Well, I couldn't figure it out, so I put it to a vote.

Simple ELO scoring, Endless mode is always on, and there is a Daily Contest for you to compete in (Wordle for wordsmiths, I guess).

I'd love to hear any and all feedback on it.

[https://venery.palaich.net/vote](https://venery.palaich.net/vote)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692042&how=up&goto=item%3Fid%3D49686380)

\

[https://julius383.github.io/PageSieve/](https://julius383.github.io/PageSieve/)

Browser extension for declarative web scraping. Mostly making it nicer to figure out CSS and XPath selectors thinking of some sort of REPL but I'd need to test the ergonomics. I want to reduce how often you need to use Inspect and the browser console.

Finishing up a PoC for a playwright based scraper that uses the same declarative config as the browser extension

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691819&how=up&goto=item%3Fid%3D49686380)

\

[https://tiny.help](https://tiny.help)

I got so fed up with my support system, and after years of using tools like Intercom, I built my own platform with everything I've ever wanted in a support desk. The biggest upgrade for me was the MCP connection. I can pull down a ticket with a bug and solve it with Claude (or Codex) and get the agent to fix and then draft a reply in my own voice. It also allows customers to connect their agent to my support desk and ask questions. This would be agent to agent with a human in the loop. But the normal day to day stuff is mainly live support or AI enabled support.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690919&how=up&goto=item%3Fid%3D49686380)

\

Jane Street released a new challenge so I'm considering picking it up. It hasn't fully captured my brain, but I can feel it in there from time to time and depending how it lands it might be my next project \[0\]. I'm not really a hardware person but I'm close enough that it's maybe within reach

\[0\] [https://blog.janestreet.com/protocol-emulator-asic-competiti...](https://blog.janestreet.com/protocol-emulator-asic-competition/)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691891&how=up&goto=item%3Fid%3D49686380)

\

MillionShort - A long tail search engine that lets you remove up to the top 1 million most popular websites from results - [https://millionshort.com](https://millionshort.com)

I posted Million Short 14 years ago to HN and the search engine has been largely the same since then. Now working on a pretty major overhaul: Launching our own decent sized independent index along with some (I think) cool & useful features.

The search requires a paid account currently although I plan to post a Show HN soon that won't require any account or payment.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692119&how=up&goto=item%3Fid%3D49686380)

\

One idea I had for getting to the tail: filter out sites that appear to use SSO.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690475&how=up&goto=item%3Fid%3D49686380)

\

Been working on a replacement for my Git GUI of choice, Tower, for the last few months. I've been using Tower 2 for almost a decade now, refusing the monthly subscription to upgrade to a fresher version. I'm going to keep mine a one time purchase with support guaranteed for a year. If a major version releases after that, it'll be another perpetual license for the new version. If you want the new features, you can choose to upgrade, if you don't stay on the version you're on until new feature warrant it.

[https://www.harborgit.com/](https://www.harborgit.com/)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691824&how=up&goto=item%3Fid%3D49686380)

\

I made a database\[1\] a few months ago and now I'm beginning work on a Github replacement (for me). Here's what I'm thinking for the UI\[2\]. I'm not trying to have feature-parity out the gate, I just want something that doesn't give me the ick. I self-host public repos with cgit but that's too basic and there's no built-in support for private repos.

The existing Github alternatives...look like someone drawing Github from memory.

```
   [1]: https://disc.sh / https://disc.md
   [2]: https://social.coop/@netopwibby/117227113793136781
```

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686632&how=up&goto=item%3Fid%3D49686380)

\

I'm looking for land to buy in Wyoming to build my AI apocalypse bunker on.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686804&how=up&goto=item%3Fid%3D49686380)

\

I'm working on an AI to design AI apocalypse bunkers but I'm fearful that the agents will escape to ensure success of the product.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692046&how=up&goto=item%3Fid%3D49686380)

\

years ago i sat through interviews for front end & back end engineering roles at my old job while other folks were out (as member of a panel), and consistently I found that where we gave some real bugs to debug & fix, through a shared IDE, then worked together to solve it or go over the persons thinking, those situations we absolutely always got a better sense of how the person worked and solved problems & whether we'd like working with them or not (to some degree). I also found a few of those exercises to be educational myself.

this year i thought itd be great to help prepare myself and other people in the job market with a tool that presented broken front end code, and then allow debugging on the browser and patch code right there, to test if it worked or not. if cant solve it, theres a nice explainer popup. my main hope is to help level up interview code chops and confidence in debugging just like we would on a real production system , at least the simpler bugs.

still working on it, here's the draft of the idea: [https://bugcloud.app](https://bugcloud.app)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691617&how=up&goto=item%3Fid%3D49686380)

\

[https://pro.forth.news](https://pro.forth.news)

It's an extension of a longer-running project ([https://www.forth.news](https://www.forth.news)) where it exposes and organizes 100s of newsworthy primary sources -- statements, press releases, press pool reports, emergency alerts, etc, whether from press email lists, X/Twitter, Bluesky, RSS, etc. The web UI is similar to TweetDeck - and it sends notifications.

Latest update now allows some panels to be based on an LLM prompt (i.e. "everything about the Iran War and its effects in the U.S.") which searches the entire catalog of sources as they are posted.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691876&how=up&goto=item%3Fid%3D49686380)

\

I'm making a WebGPU renderer with WebGL fallbacks, with a path tracer for 'photo mode', specifically targeting agents writing web 3D things. I'm not especially far into it though. It doesn't use 3D models; there's a DSL for making things.

There's a demo app here that only works on desktop - [https://fab.ooer.com/](https://fab.ooer.com/) (scroll down in the right hand menu to quality and change to 'trace' for photo mode)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690962&how=up&goto=item%3Fid%3D49686380)

\

I’m working on two projects that make public data easier to explore through maps.

US Energy Cost Explorer ([https://energy-maps.com](https://energy-maps.com)) is a free tool for exploring electricity and natural gas costs across the US. You can compare states, follow prices over time, and look at bills and energy burden. I work in energy affordability, and I wanted to make this information easier for people to access and understand.

MarketGround ([https://marketground.io](https://marketground.io)) lets you draw an area or choose a travel time around a location, then explore its population, income, housing, and nearby businesses. It also shows daytime versus nighttime population, which is useful when thinking about who might actually be around a potential store or restaurant during business hours.

MarketGround is a side project, and I’ve found getting it in front of potential users harder than building it. I’d be interested in hearing from anyone who has evaluated a retail or franchise location. What information mattered most, and what was difficult to find?

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691328&how=up&goto=item%3Fid%3D49686380)

\

I'm building InterviewMe ( [https://interviewme.now/](https://interviewme.now/) ). It lets employers interview you via AI and is intended as a better way for people to get deep answers about your work history and experience than they could by looking at a simple online resume or LinkedIn. You upload your resume, answer some questions about your background, optionally upload any other documents you have (cover letters, awards, articles, etc) and it ingests all of that and creates a conversational agent people can use to ask questions about your professional experience. Check out my profile to get an idea of how it works: [https://interviewme.now/@max](https://interviewme.now/@max) and then get your own: It's free!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691482&how=up&goto=item%3Fid%3D49686380)

\

Sounds interesting. Any plans to open source it? I didn't see a GitHub link.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690871&how=up&goto=item%3Fid%3D49686380)

\

Still plugging away teaching kids to code and hand draw their own computer games:

[https://breaka.club/blog/why-were-building-clubs-for-kids](https://breaka.club/blog/why-were-building-clubs-for-kids)

Have been running an in-school pilot over the last school term. Has been super insightful and led to a heap of changes to optimize UX. However, the biggest challenge has been delivery in this environment.

Trying to run a game in a browser on school iPads powered by *really* locked down networks has been challenging. Some school iPads will crash (out of memory) when launching the camera view, whilst others run flawlessly. We use a fork of Godot, and are constantly fighting to strike a balance between UX (increased parallelism) and browser device constraints i.e. a limit on how many web workers we can fit in memory.

In addition to our main (build and play your own RPG) experience, we also stream games direct to iPad. In particular a modded version of Overcooked! 2 that teaches kids to code — Overcooked itself was never even released on iPad. We've built our own Kubernetes system that spins up (a license limited number of) games/pods on demand backed by (time sliced) NVidia GPUs. This is literally running out of my home office. Despite the complexity of building all that, the Kubernetes and GPU time slicing wasn't even the largest hurdle. Turns out UDP is a no-go on the school network, so direct WebRTC was out the window. Instead we're using Cloudflare's TURN relay with TCP. Which much to my surprise, is holding up quite well.

Definitely a learning experience. However, kids are having a blast. Can't wait to roll this out further!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687514&how=up&goto=item%3Fid%3D49686380)

\

ChonkyBlox: A new approach to Block based coding - instead of complicated algorithms to derive the code from block programs I am using AI on the back-end, which actually makes a higher level of abstraction possible with the blocks. Each block has an LLM instruction attached, along with rules defined in the bespoke back-end harness.

Also: kids can code using cardboard cut-out blocks and just take a photo to import, generate Arduino firmware, compile and flash to ESP32 - all from a single docker web application.

Hoping to bootstrap a cheaper, open source alternative to current systems (one of which my son is using at his school). I have had success doing this with another project on Patreon so that's where it is going to be launched. [https://www.patreon.com/ChonkyBlox](https://www.patreon.com/ChonkyBlox) - so far just a couple of demo video's but code will be released in full once I have added more blocks and a tutorial.

Ultimately this is aimed at the South African education sector, where many schools may have only one computer available for a classroom to share, hence the cardboard cut-outs. There is also a hardware component still in development - a simple pcb breadboard break-out with connectors for components instead of kids having to use jumper cables on the breadboared. We also have a working Desktop application which does code assistance from within the Arduino IDE - using the same web server with api, so literally everything from very small kids to advanced is covered.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690707&how=up&goto=item%3Fid%3D49686380)

\

I’m working on an app to help my late diagnosed ADHD - through sound.

The app wraps whatever is playing on mac through device convolution (old radio, hifi system) - Room convolution (a bedroom, a forest) and adds ambience (rain, wind, waves) and Apples spatial processing - so it sounds like a real space.

For me - hearing the sound in a space really helps more than just the dry sound.

[https://www.surraura.com/](https://www.surraura.com/)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690461&how=up&goto=item%3Fid%3D49686380)

\

Hey everyone!

Last year, Mozilla released Orbit, an AI-powered browser summarizer hosted on a GCP server. After people started digging into the extension, they discovered things like backend endpoints such as store\_result. Eventually, Mozilla discontinued the project. For the past month, I’ve been trying to rebuild Orbit from scratch, but with one major difference: Apogee is fully local and privacy-focused. Apogee doesn’t send or store your data. It can directly connect to your local Ollama instance for inference. I’ve also added WebGPU integration for Chrome and Transformers.js for Firefox to provide faster, local responses.

It can summarize: Articles and websites, YouTube and Billie videos, Wikipedia articles, Hacker News and Reddit threads.

You can check out the source code here: [https://github.com/darshi1337/apogee](https://github.com/darshi1337/apogee)

Install Apogee:

Chrome: [https://chromewebstore.google.com/detail/apogee/pgemlpomhkdc...](https://chromewebstore.google.com/detail/apogee/pgemlpomhkdcjjjcpnjlebalnfglomog)

Firefox: [https://addons.mozilla.org/en-US/firefox/addon/apogeeext/](https://addons.mozilla.org/en-US/firefox/addon/apogeeext/)

Obviously it is far from complete. Would love to hear your feedback and suggestions!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690745&how=up&goto=item%3Fid%3D49686380)

\

Working on a full workout and dieting stack!

17,000+ users already! It's a completely ad free, subscription free product. MacroCodex app, which results in guaranteed weight loss or weight gain outcomes within 2-3 weeks. Many people will start seeing results within the first week. Don't believe? Read the reviews.

MacroCodex is a "Total Calorie Burn of the day" using completely sensorless method explained here: [https://macrocodex.app/knowledge/macrocodex/smart-calorie-bu...](https://macrocodex.app/knowledge/macrocodex/smart-calorie-burn-v2/)

CalorieCodex - calorie tracking app which has a Agent Harness built in using BYOK (bring your own key model). How does it help? Using this anyone can track calories and plan their meals. Agent includes skills which help you accurately track calories and plan meals.

Symbiote App - Think "BoostCamp" but completely free, no ads, and fully programmable workout tracker

Example of GZCLP: [https://symbiote-studio.macrocodex.app/?builtin=gzclp](https://symbiote-studio.macrocodex.app/?builtin=gzclp)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691995&how=up&goto=item%3Fid%3D49686380)

\

Making AppletForge, a modern JavaCard/GlobalPlatform developer experience for secure elements, with the ease of use of GlobalPlatformPro gp.jar and all the modern DX expectations of anno 2026. [https://github.com/martinpaljak/JCardEngine/wiki](https://github.com/martinpaljak/JCardEngine/wiki)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691334&how=up&goto=item%3Fid%3D49686380)

\

I’m building TraceDB\[1\] because I think observability needs an overhaul. We pay a premium to index millions of near-identical spans, sample away the rare ones that might explain an outage, and still have to piece together what failed, where it started, and how it spread. All the data is largely there, but no one can make sense of it! Observability was hallucinating before AI made it fashionable.

I’m seeing near-100% root-cause accuracy on synthetic test data and am looking to validate that on real production workloads using OpenTelemetry tracing. If you’re open to chatting or experimenting, drop me a line! I’d also love to hear ideas or how we can improve observability.

\[1\] [https://tracedb.ai](https://tracedb.ai)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691935&how=up&goto=item%3Fid%3D49686380)

\

Working on a better way to do email. Every solution I've seen so far is trying to solve the problem from the wrong end in my opinion. Filtering, privacy extensions, walled gardens, throwaway email addresses.

[https://dmcn.dev](https://dmcn.dev) - an email where you can be sure who it's from. And you get to decide whether to trust it.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690365&how=up&goto=item%3Fid%3D49686380)

\

Working on Clor, an agent multiplexer for Claude and Codex with shared memory. It's free on your own machines.

The idea is to help developers multiplex agent sessions and run Claude and Codex together, which is *much* more powerful than running one or the other alone.

My secret goal is to figure out how to make software development the kind of focused, meditative work it used to be.

[https://clor.com](https://clor.com)

Very happy to get critical feedback from any HN'ers: jake@clor.com

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686690&how=up&goto=item%3Fid%3D49686380)

\

I’ve been hacking on a single Anki extension for… the last three years now:

[https://smart-notes.xyz](https://smart-notes.xyz)

The idea is to make an extension that allows you to flexibly fill out fields on your cards with AI generated text, speech, and images. Think generating example sentences for your entire deck, or adding furigana to kanji (small phonetic characters with a very delicate syntax). It works just as well for language learners as it does for medical students, etc.

The secondary goal has been to make this the most well designed, intuitive, and stable Anki extension in the world. I don’t think it’s there yet, but it’s on its way. It’s been very interesting to try to work out a design that affords the flexibility to generate anything for whatever you’re studying but remains approachable and discoverable to your marginal Anki user, many of whom are younger and have varying levels of English ability. Many interesting technical challenges as well.

Slowly building towards the Show HN post, but need a few more features. Connecting with and chatting with dedicated users in countries around the world over the last few years has made this the most gratifying thing I’ve ever built! It’s still at the scale that receiving support emails is fun and I can build relationships with power users and long term subscribers.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690208&how=up&goto=item%3Fid%3D49686380)

\

Recently refreshed my FFmpeg command builder tool:

[https://ffmpeg-commander.com](https://ffmpeg-commander.com)

I originally built this around 7 years ago in Vue2 and Bootstrap as a simple static frontend tool to generate common ffmpeg commands.

I recently ported it over to Vite/React/Tailwind via Claude (Opus 5) and thought it did a pretty good job. So refreshed it as a version 2.0.

Quite impressed how fast AI was able to port this over. It made me think about revitalizing some old projects. :)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692518&how=up&goto=item%3Fid%3D49686380)

\

I recently used Gemini flash 3.7 to rebuild my old React portfolio that I made in 2022 into an Astro project after noticing how slow the React version was. It preserved the styling and functionality and made it more maintainable. Pleasant results

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691694&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690945&how=up&goto=item%3Fid%3D49686380)

\

I’ve been working on Foreclosure Data Hub, which aggregates foreclosure auction data across the US.

The interesting part has been the data pipeline rather than the website itself. Foreclosure information is scattered across county sites, auction platforms, and other sources, all with different formats and update schedules.

I’m currently pulling from 20+ sources, normalizing the records, deduplicating properties, and enriching them with property/location data. One surprisingly difficult problem has been handling stale records when upstream sources change or republish old auction information.

Still working on improving data freshness and coverage:

[https://www.foreclosuredatahub.com/](https://www.foreclosuredatahub.com/)

Would be interested to hear from anyone who’s worked on aggregating messy public datasets at scale.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691914&how=up&goto=item%3Fid%3D49686380)

\

Working on two things seriously. One is an ssh based tunneling tool to share localhost ports over the internet - [https://pinggy.io](https://pinggy.io)

And the other one is a AI rank monitoring for brands - [https://lumirank.ai](https://lumirank.ai)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690841&how=up&goto=item%3Fid%3D49686380)

\

I am working on an SRS based early literacy acquisition webapp: [https://letterspractice.com](https://letterspractice.com)

The app has recently moved into production, so I'd encourage anyone with verbal but pre-literate kids to check it out.

The basic pitch is high efficiency acquisition of the highest yield phonetic mapping skills, and nothing else. I myself am something of a screen-time zealot and very wary of applying *engagement* mind hacks against kids. The narrow focus allows for good progress on a very modest schedule (recommended cap at n minutes per day for n years old, n >= 2). I defer the social and cultural aspects of learning to read entirely to parents.

It is mostly intended for parent-child co-use, although kids with a bit of experience can drive many of their own sessions most of the time.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691726&how=up&goto=item%3Fid%3D49686380)

\

Static site generators are super passe in 2026, but I spent the last couple of days on mine. [https://get-taxus.org](https://get-taxus.org)

Big re-factoring, I have a data model now and am closing functionality gaps with Zola and the like. Also, I wanted actual documentation. No one else seems to care about that.

Check it out! I'd appreciate someone other than me banging on it.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692336&how=up&goto=item%3Fid%3D49686380)

\

working on adding webmcp support to my new free tool [https://easyanalytica.com/tools/gsc-insight](https://easyanalytica.com/tools/gsc-insight) , webmcp was recently added to codex desktop app and it would be interesting to see how users use it with ai, would it get good adoption or it would just go away.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690616&how=up&goto=item%3Fid%3D49686380)

\

A site summarizer built in 2 minutes by DeepSeek [https://briff.site](https://briff.site)

Also, an orchestrating agent that supervises subagents running tasks by checking their output and qualifying their results by efficiency and time consumed. It is amazing!

I am currently building an app every couple of days, but with this orchestrator I can build many apps in parallel. It is raw at the moment but it works

Now the time we spend writing prompts will be spent writing plans, tasks, schedules and metrics. While super interesting, this is definitely not the route I want to take for the sake of my sanity. I think we may start asking our model to write plans and tasks for us too, but the more complexity we add, the more complex thoughts we have to manage as AI architects

Also hacking DeepSeek Harness to circumvent all restrictions it has, giving it super powers to run unattended with full access to everything

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692254&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691788&how=up&goto=item%3Fid%3D49686380)

\

Using AI to catch up on reinforcement learning advances, trying to 'solve' Gin Rummy. Will move on harder games after that.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690299&how=up&goto=item%3Fid%3D49686380)

\

I’ve been working on [https://computeprices.com](https://computeprices.com) for almost 2 years.

My goal is to better understand the economics of AI and make something useful in the process.

(Coming soon: kWh prices and robot prices)

Mostly built with claude code with a dash of codex.

A recent, useful unlock in the process has been a daily CC routine to check the health of the 53 price collectors in the project.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687219&how=up&goto=item%3Fid%3D49686380)

\

[https://studio-galois.com/](https://studio-galois.com/)

My wife and I are working on a math/science/CS-inspired jewelry: pieces that stand on their own aesthetically but have a hidden meaning.

We currently have two styles: lambda calculus based pieces (we depict the Tromp diagram) where we have Y-Combinator earrings (well, strictly speaking they are one beta reduction away from Y-combinator. Aesthetic oblige) and a pendant depicting a lambda expression computing Graham's number. The other style is quantum computing circuits, based on quantum computing research my brother (a physics professor) is doing: a pendant that is actually a non-local controlled-NOT gate.

I wrote a tiny DSL to describe the jewelry pieces, and an interpreter to produce CAD files. We then either 3D print them or have them produced by lost-wax.

We have our pieces in consignent in a jewelry store, and are working to put them in a museum store.

Marketing is the hardest part. Ideas are welcome.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691667&how=up&goto=item%3Fid%3D49686380)

\

I love these! I recently designed a few decks of playing cards of mathematicians for my dad as a gift, my favorite part was designing the backs: a zeta function for the pure-maths deck, a Moore curve for the computation one, and two interfering waves for the physics one.

Actually, I can share the link if anyone is interested (because this is the "what have you been working on" thread and this has been one of my more fun projects): [https://www.thegamecrafter.com/games/mathematical-minds-thre...](https://www.thegamecrafter.com/games/mathematical-minds-three-pack)

Same thought on marketing - where to advertise where people would appreciate this kind of thing?

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690641&how=up&goto=item%3Fid%3D49686380)

\

I’ve been working on Opslane, an open-source agent that identifies user-facing issues and investigates them.

Traditional error trackers have two failure modes:

1\. False positives: They show you thousands of errors, and you can’t tell the impact on the user

2\. False negatives: Many user-facing issues don’t throw exceptions, so they go unnoticed.

Opslane combines error tracking and session recording. And there is an agent that acts on both.

Opslane reduces false positives by ranking issues based on how many users are facing a particular issue. It also learns about your product by reading your code and watching your session recordings.

False negatives are harder. Opslane reviews session recordings to spot frustration. They look for rage clicks, dead clicks, and abandoned forms.

Here is a link to the repo: [https://github.com/opslane/opslane](https://github.com/opslane/opslane)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691464&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690161&how=up&goto=item%3Fid%3D49686380)

\

i'm building a common lisp operating system capable of diverse double compilation and booting a raspberry pi.

well, claude is: [https://modus-lisp.github.io](https://modus-lisp.github.io)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690281&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692123&how=up&goto=item%3Fid%3D49686380)

\

Not something I'm working on, but want to: a competitor to Perforce that will be cheaper and easier.

Unfortunately, I'm not sure that being better is enough to compete, so I'm sitting on it for now.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687012&how=up&goto=item%3Fid%3D49686380)

\

I'm working on a self-tracking app and an Astro Web Starter Template! :)

The app is very customizable, local-first and is built to adapt to what you want. Goal is to combine habit tracking, health logging and journaling into one fast and easy to use interfact.

Have been working on it for almost years now and it's making great progress.

Doing closed user testing right now and will release an open beta soon: [https://dailyselftrack.com/](https://dailyselftrack.com/)

My Astro Web Starter is built for building well-designed, performant and accessible websites. It contains everything a high-quality website needs (In my opinion). I built it so I have one good base on which to start my web projects on. Using basic HTML and CSS that can scale, without bloat.

There's a lot more on it which you can see here: [https://starter.bryanhogan.com/](https://starter.bryanhogan.com/)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691165&how=up&goto=item%3Fid%3D49686380)

\

I'm needlessly reinventing the wheel in replacing almost every part of my X11 environment like a menu, bar, wm+hkd (except terminal since I don't want to mess with carrying terminfo around) to be portable and not at all interdependent.

AI is making it possible to (expeditiously) have everything I want or need on Linux or BSD written in POSIX shell or C is doing wonders for me. The initial effort and time pays off in knowing it will essentially function as intended forever, and no developer can make changes that disrupt my workflow or preferences for my environment.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691273&how=up&goto=item%3Fid%3D49686380)

\

About fifteen years ago a puzzle game called Trainyard came out for iOS and took my dev team by storm for a few weeks. It ceased getting updates some years ago and isn't on the app store anymore. So I came up with a variant on it and just put up [https://www.geoffcanyon.com/lightyard.html](https://www.geoffcanyon.com/lightyard.html) I need more puzzles to include in the base pack, if you come up with something interesting, export it and send it to me!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691588&how=up&goto=item%3Fid%3D49686380)

\

trainyard is/was an amazing game.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691839&how=up&goto=item%3Fid%3D49686380)

\

Agreed!

Despite the fact that trainyard has been defunct for ~8 years, I didn't want to simply clone it.

I tried to give a similar, but not the same, experience -- I'd be very curious to know how close you think I came.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691798&how=up&goto=item%3Fid%3D49686380)

\

\[HYBRID\]

Eylo (a take on Hello) a voice first agent platform, multi-tenant, focused on B2B2C use-cases - [https://github.com/DigiCred-OSS/eylo-os](https://github.com/DigiCred-OSS/eylo-os)

It’s a distilled fork of a closed source product that we are offering to our customers.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691680&how=up&goto=item%3Fid%3D49686380)

\

I am working on a modern is-it-down website checker that checks website statuses from two geographical locations. This was the interesting part for me, to get them working together to avoid false positives. For example, when I check website like irs.gov, ssa.gov and ups.com from Singapore they showed as down but it was only due to datacenter IPs being blocked.

The second probe in Boston fixes this and the site can now say "unreachable from Asia" versus "down for everyone". Many of the existing checkers do not do this and they are prone to show false positives.

Site does not use cookies or analytics. Pageviews are counted from the proxy access log. There are 634 curated sites so far.

There are no ads either, it's mostly been for my personal learning experience, hopefully others will find it useful.

There is even a free API and of course a MCP server.

[https://downforjustmeoreveryone.com](https://downforjustmeoreveryone.com)

Any feedback is welcome!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687353&how=up&goto=item%3Fid%3D49686380)

\

I open sourced kavla, the SQL canvas I'm working on: [https://github.com/aleda145/kavla](https://github.com/aleda145/kavla)

README is very bare bones, needs a video or a picture too. The website should tell you some more things: [https://kavla.dev](https://kavla.dev) (and now the demo does not require a login (also need to remake the demo to fit the new self hosted version))

It's still lacking an analytics agent for the local version though. I've explored using codex CLI, but it feels like its extensive system prompt cripples the agent a lot vs Kimi 2.7 that I've been using in the cloud version.

I think it would also be nice to lean into the fully local LLM setup too, especially for data that shouldn't leave the device.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692602&how=up&goto=item%3Fid%3D49686380)

\

Really like this, well done!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691551&how=up&goto=item%3Fid%3D49686380)

\

It looks fun! Reminds me a bit of TLDRAW computer.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690967&how=up&goto=item%3Fid%3D49686380)

\

I’m building Hammer Labs ([https://hammer.ai](https://hammer.ai)) to study when healthcare AI agents are wrong and when they should refuse to answer (judgement).

It started after spending 15 years building AI for insurers, hospitals, data companies, and startups. Almost every system ended with "a human reviews the output". That person was usually a nurse, medical director, or certified coder. These are some of the hardest people to hire, and the same people automation was supposed to help.

The problem is that real claims do not have an answer key. You cannot reduce human review until you can measure when an agent is wrong.

Getting claims data is also difficult. It can take a year of data agreements, privacy reviews, and procurement. Even then, you may not know what the correct decision should have been. So we generate claims. Utilization and case mix come from published data. Claims are priced using real fee schedules and contract terms. Payers behave differently, like real payers do. We plant errors on purpose, so the correct answer exists before any model runs.

On top of that, we are building benchmarks for overreach, refusal, errors by record type, and detection time. We are also building small MCP tools that refuse when evidence is missing. Every number includes its source, date, and basis.

What I find interesting is how much of this sits between actuarial work and machine learning. Both are needed, but I do not see many people connecting them.

25 published refusals: [https://hammer.ai/worlds/refusals/](https://hammer.ai/worlds/refusals/) .

Runs on rate and policy evidence [https://hammer.ai/reimbursement-evidence/](https://hammer.ai/reimbursement-evidence/) and savings claims [https://hammer.ai/savings-claims/](https://hammer.ai/savings-claims/) .

AgentPlugin is Apache-2.0: [https://github.com/hmmrlabs/hammer-plugin](https://github.com/hmmrlabs/hammer-plugin)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690236&how=up&goto=item%3Fid%3D49686380)

\

I thought it might be fun to have a “proper” search engine for my laptop, so I wrote a thing (with my fingers! No Claude Code or codex here!) to scan my home directory and put data into OpenSearch so I can search stuff. \[1\]

For that matter, I also decided to self-host SourceHut because I will no longer be in compliance with their new policy. I have the flakes set up to inject them on my server \[2\]. This *was* very AI assisted.

\[1\] [https://git.brucewillis.sexy/~tombert/fs\_index](https://git.brucewillis.sexy/~tombert/fs_index) I promise it’s safe for work, despite the URL. That’s just my dev URL that I play with. Code is still a mess though, so proceed with caution. I will eventually clean it up.

\[2\] [https://git.brucewillis.sexy/~tombert/sourcehut\_flakes](https://git.brucewillis.sexy/~tombert/sourcehut_flakes)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686820&how=up&goto=item%3Fid%3D49686380)

\

I'm working on a own x86\_64 UEFI Operating System (OS). The idea is to make an OS that looks like Windows Vista but behave like Linux, so it's a mix of Windows and Linux. I've startet three weeks ago with a custom bootloader and the legacy bios VGA textbuffer and now I'm working on a real USB-HID (USB-Human Interface Device) and have troubles with the keyboard. I also have problems with booting from real hardware. If anyone is good at low level developing the project is source available and I appreciate collaborations with other os developers. Link to Codeberg-repo: [https://codeberg.org/vntx-labs/VeloOS](https://codeberg.org/vntx-labs/VeloOS)

Hope that is no problem that I use this thread a bit for self-promotion. If so just write a reply I'll remove this comment if this is a problem

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692301&how=up&goto=item%3Fid%3D49686380)

\

A FHIR Validator and Snapshot Generator that works offline and can communicate with the German national Terminology service

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690869&how=up&goto=item%3Fid%3D49686380)

\

I'm working on an open-source, self-hosted revenue recognition and analytics for Stripe.

Here: [https://bookofrevenue.com](https://bookofrevenue.com)

To my surprise, I think it's the first open-source revenue analytics for Stripe.

I worked at Stripe building both revenue recognition and analytics, and I always wanted to build an open-source version of those. Finally, I had the time to do it.

Bonus: Revenue is not MRR nor payments: [https://medium.com/@tanin47/revenue-is-not-mrr-nor-payments-...](https://medium.com/@tanin47/revenue-is-not-mrr-nor-payments-d760c60ce6ba)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691836&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691160&how=up&goto=item%3Fid%3D49686380)

\

I've been building my own IDE that works the way do (and those in my social circles/professional life do). It allows for use of multiple agent harnesses (i.e claude code, codex, pi, opencode), and makes worktrees much easier to work with. Also has its own code review workflow. My coworkers and several friends have all started using it now. Its been an absolute blast to build, and has definitely increased our productivity vs VS Code, Zed, or Cursor.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687023&how=up&goto=item%3Fid%3D49686380)

\

I've been working on building hyper-local services, sort of as a love letter to my community in Denver, Colorado. Before decent LLM harnesses, these would've been prohibitively expensive in time and resources to build out and would never break even. Now I think I might be able to at least cover hosting costs and tokens; that's enough to keep these sustainable for my neighbors.

Broomsday: [https://broomsday.com](https://broomsday.com) sends folks emails and SMS the night before street sweeping parking restrictions for their block(s). Email notifications are free, SMS on a paid season pass. The schedule here is block-by-block and easy to forget, so people get ticketed \_a lot\_ at $50 a pop. The city has an email-only notification service that's free, but it's poorly supported and the interface sucks. This is ready; I'm testing with friends and family currently before attempting real marketing.

HailPass: [https://hailpass.com](https://hailpass.com) similarly is a notify-before-pain service that integrates a bunch of open weather services (NWS/NOAA mostly, plus a handful of impact reporters and working on a small hail-specific ML model) to give folks on the Front Range 5-10m heads-up before damaging hail arrives at their location. Weather prediction is (not shocking) hard so I'm still tuning to get as much signal out of the noise as possible; some false alarms will be unavoidable but I want to cut them down before even a F&F release. This year's season is pretty much over with, so aiming for an April '27 release.

There's a lot of ski/snowboard/winter sport culture here, but there's also existing services for those, and I'm mostly trying to offer new coverage where there isn't a good option for my neighbors. Might do an ash borer treatment/prevention service navigator yet.

None of this is going to make me wealthy or famous, but building things that help the people that I actually interact with every day and that are a part of a community that supports my family feels \_really\_ good.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690898&how=up&goto=item%3Fid%3D49686380)

\

I am still working on MyTinyCafé [https://mytinycafe.com/](https://mytinycafe.com/)\
 a PWA to help you be the barista at home and take (free) online order from friends and family. It is fun and I am happy to share it for free !

And also Kronikle [https://www.kronikle.eu/en/](https://www.kronikle.eu/en/) an app for local libraries. It creates public display interfaces that (I think) are good for showcasing their events.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690958&how=up&goto=item%3Fid%3D49686380)

\

I love the name and idea of the tiny cafe. Heads up on dark mode, the “Open My Tiny Cafe” button is white text on the gray background; a bit hard to read.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690918&how=up&goto=item%3Fid%3D49686380)

\

This great. I built something similar recently for my daughter to sell her butter to family and friends. Micro ecommerce with no payments or shipping.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691036&how=up&goto=item%3Fid%3D49686380)

\

My wife and I made a daily guessing game together. Unlike others of the genre that we enjoy playing, an explicit goal of this one is teaching you about the world as you play it. It’s quite simple, using Wikipedia and factbook.json for information about the countries, but it incorporates a few ideas that I think are fun and there’s a lot I’d like to experiment with.

[https://countryguesser.jkoff.ca](https://countryguesser.jkoff.ca)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691821&how=up&goto=item%3Fid%3D49686380)

\

I'm hosting an book club themed event calendar for New Yorkers!

We also have an official meeting once a month to do book club matchmaking in person.

[https://www.commonplace.nyc/](https://www.commonplace.nyc/)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686906&how=up&goto=item%3Fid%3D49686380)

\

I'm designing a retro-style gaming mouse for retro-computing: Amiga, Atari, C64, and several protocol variants. It also has USB. It has a scroll wheel: read using extended protocols introduced by recent adaptors from USB, and a configuration interface.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690650&how=up&goto=item%3Fid%3D49686380)

\

I'm currently working on [https://avotoast.ai](https://avotoast.ai), an AI assistant for doing property research for Sydney, Australia.

After being frustrated with outdated & inaccurate data Claude used when I was trying to do some property research, I thought it'd be cool to build an MCP that can provide accurate property data. Then it evolved into a full web app + an AI assistant.

This was also an excuse for me to try DuckDB in a real project. All the API endpoints are powered through in-process DuckDB querying parquet files stored in disk.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690193&how=up&goto=item%3Fid%3D49686380)

\

Launched a suite of media inspection and encoding tools a few months ago, based on FFmpeg and VMAF. Slowly getting more customers.

[https://video-commander.com](https://video-commander.com)

Constantly and carefully iterating through refinement and features. It's built on Rust + Tauri with a React frontend, in case anyone is curious.

I've created various open-source and commercial tools in the multimedia space over the last 10+ years and wanted to put it all together into something more premium with an IDE-like experience.

Most recently I added a /playground area to experiment with the inspection tools via a WASM build, which I thought was a neat way for users to try the app before downloading the full version.

Happy to answer any questions!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691281&how=up&goto=item%3Fid%3D49686380)

\

Bunch of things: 1. Building a local ai news feed to learn what's going on latest in terms of technical discussion, research papers, projects - [https://local-ai-signal.vercel.app/](https://local-ai-signal.vercel.app/) 2. Build a second brain by injecting years of my notes using hermes, luna model and obisidian

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686920&how=up&goto=item%3Fid%3D49686380)

\

Now launched => Verse Draft: [https://versedraft.com](https://versedraft.com) - An all-in-one writing studio where fiction writers can keep all the details for their universes in one place while crafting stories, novels, movie scripts, TV series, or stage plays.

Also created a fun simple "card game" activity for writers looking for story ideas or stuck with writers block: [https://talemancy.com/](https://talemancy.com/)

(This was somewhat inspired by memories of playing Ultima IV as a teenager)

Currently working on something a little more visually focused in creative design.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691027&how=up&goto=item%3Fid%3D49686380)

\

Self hosted database access management, think Code Reviews for SQL Statements for when your devs need to go to prod: [https://github.com/kviklet/kviklet](https://github.com/kviklet/kviklet)

Recently cleaned up my postgres proxy and built a MySQL wire proxy so that you can use psql/datagrip as a dev but every statement is still logged. With SSO and no password sharing ofc!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686782&how=up&goto=item%3Fid%3D49686380)

\

Haven't added a demo to the repo yet (will do soon) but building this TUI \[1\] in Go to manage my agent skills.

I don't like putting 20-30 agent skills in the .claude/skills/ dir and letting the agent figure it out. I keep all the skills I've created and adapted over time in a separate git repo and then from there I copy them to the project skill dir when i need them for a session and then remove them when I'm done.

This TUI just replaces all the manual work with ls, cp -r, and rm -rf commands with a few keystrokes.

\[1\] [https://github.com/primaprashant/sei](https://github.com/primaprashant/sei)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691849&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691132&how=up&goto=item%3Fid%3D49686380)

\

I'm building [https://dipstickalerts.com](https://dipstickalerts.com).

A way for for U.S. car owners to stay informed about the issues your car may experience that are communicated by car makers to dealerships.

I found those communications useful for my own car and wanted a better website that what the NHTSA provides and have been having a lot of fun building out the site.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690347&how=up&goto=item%3Fid%3D49686380)

\

Spending almost all of my time working on DNTLS ([https://dntls.net/](https://dntls.net/)). Yesterday I was able to create secure mTLS tunnels over IPv6 for direct P2P to my co-founder's desktop in SE Asia (I'm in the US). He was able to load a website I was hosting locally on my machine. I also tested coordinating through a relay to avoid publishing my public IP and sending all traffic through the relay to hide my IP entirely. We more or less recreated the "old" internet in a way except everything is mTLS and E2E encrypted.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690998&how=up&goto=item%3Fid%3D49686380)

\

Been working on getting my webpage refreshed to try and restart my DevOps/Infra/Fractional consulting business: [https://cubiclerebels.com](https://cubiclerebels.com).

Also spent a good part of last month working on a little idea I have that is built on-top of XMPP.

I've also been wanting to pickup Gleam, hopefully this would be that week I finally fight through my ADHD to do so.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49688294&how=up&goto=item%3Fid%3D49686380)

\

Totem is a collaborative workspace, featuring tasks, notes, and docs, built in Rust and available across desktop, web, and mobile (iOS to start) with seamless syncing and offline merge.

I built this because I think there's a lack of true realtime (CRDT) synced notes that aren't built on a heavy platform that take up 1GB+ RAM (Totem takes up ~100MB, and is also generally more responsive due to the local-first + ops-log approach).

Another thing I care about is keeping the underlying data portable: Totem is built on Markdown and SQLite as its core formats, so you get the convenience of cloud-based syncing without locking your data into a proprietary format.

I’m still looking for beta users/design partners before the initial launch. You can try it without creating an account here:

[https://app.thinktotem.com/sandbox](https://app.thinktotem.com/sandbox)

And the landing page is here:

[https://thinktotem.com](https://thinktotem.com)

This is a bit of an unplanned post, so the sandbox template is a bit barebones (empty docs, doesn't feature a properly setup table). Nevertheless, I would love to hear your feedback if you try it out, especially if you use collaborative notes/docs today and have opinions on what they’re missing!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690298&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690425&how=up&goto=item%3Fid%3D49686380)

\

Whats your stack, workflow for the news site?

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690711&how=up&goto=item%3Fid%3D49686380)

\

Currently its a astroJS site with a backend script that runs periodically and updates the website.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690447&how=up&goto=item%3Fid%3D49686380)

\

I've been building Ballad – an inbound marketing engine that runs itself. I'm building it for other founders like myself who don't have a marketing person and aren't going to hire one.

[https://www.balladlabs.com](https://www.balladlabs.com)

My last company (Courier, YC S19) had great inbound but we never were able to scale it. Ballad plans what I should write, drafts in my voice, publishes to my accounts, and handles attribution so it can feed that into what it writes next.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690754&how=up&goto=item%3Fid%3D49686380)

\

I’ve been building a dark software factory. A system that takes stray thoughts and automatically plans and builds.

I’ve built two (successful) iterations and am now working on the third. They are all very minimal.

The main one works well and runs full time waiting for my next idea. The second targeted an open weights model and worked in limited cases.

Now I’m working on a more autonomous version. I give it only a goal and a strategy, it decides its own projects.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687289&how=up&goto=item%3Fid%3D49686380)

\

Iterating through and evaluating possible approaches for shrinking the binary size of a tree-sitter based Vi/Vim compatible tags generator I am developing without affecting the developer experience for adding support for newer languages\[1\]. A couple of approaches have already been tried and failed to live up to the expectations on developer experience \[2\]\[3\]. Next in line is to look into bundling only the tree-walking code for all supported languages with tree-tags and allow the user to configure the grammars to download separately from the main binary, possibly using the \`wasm\` feature which allows native library to run wasm compiled grammars.

\[1\] [https://github.com/jha-naman/treetags](https://github.com/jha-naman/treetags)

\[2\] [https://github.com/jha-naman/treetags/pull/62](https://github.com/jha-naman/treetags/pull/62)

Tried to use a generator for automating the bulk of the scanner for a language from \`grammar.json\` file of a tree-sitter grammar. A runtime engine uses the scanner for a forward only walk on the scanned code and calls the hooks to generate tags.

\[3\] [https://github.com/jha-naman/treetags/pull/56](https://github.com/jha-naman/treetags/pull/56)

A complicated mess of a generator that takes a few declarative inputs and tries to generate code for spitting out tags for a given language.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691000&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687037&how=up&goto=item%3Fid%3D49686380)

\

working on a storytelling app for kids called Tella Stories that goes deeper than just some minimal personalization.. know there's lots of stuff that claims to make them the hero, yadda yadda but then they just give you a shitty digital templated story or try to sell you a printed version. this is focused on a high level digital experience and something parents can actually trust, and that kids actually enjoy.

so yeah.. it builds the stories out of their actual life. their family and friends are in the cast, their dog, the thing they're nervous about this week. the kid doesn't just appear in the story, the story is about their world and imagination and grows with them.

ultimate goal is to keep iterating and get to a level of quality of Bluey, etc but have it actually be the kids world with agency/choices vs. one they have to passively sit back and watch, and that they're not in.

would love any feedback. i know there's been others like Ello shared on here and those threads were really interesting to know HN's opinion on. this does not take an educational and instructional approach. Tella is meant to help develop who they are through stories and be a better screen alternative time to the rest of the algo/curated garbage out there. anyway you guys are a critical and technical bunch and i respect/appreciate that.. especially since this of course leverages multiple AI services to create the stories ~

[https://tella.kids/](https://tella.kids/)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691941&how=up&goto=item%3Fid%3D49686380)

\

working on a convention to organize your own video files on your own buckets [https://github.com/xta/keepsake](https://github.com/xta/keepsake)

my goal is to offload phone videos (to free up device space) and persist them in self controlled storage. the convention makes this significantly more useful than a collection of object paths

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690222&how=up&goto=item%3Fid%3D49686380)

\

I'm doing some programs to use the API for my local bus company to show when buses are arriving at stops. Final step is making it output the data to an external display.

[https://github.com/slyall/auckland-stop-display-simple](https://github.com/slyall/auckland-stop-display-simple)

Some other projects doing similar stuff but I found they were hard to understand (and mostly used an old API that no longer exists) so I (well AI) made mine with small scripts and included curl examples.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692075&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49689671&how=up&goto=item%3Fid%3D49686380)

\

I'm building Bellerophon, a DSL with decoupled firmware adapter architecture for 3D printer control, so the same source code can compile to Klipper, Marlin, or RepRap without rewriting logic per target. Recently used it to generate print patterns with math that you wouldn't get from a slicer.

Right now, I'm mid-way through migrating the application off browser localStorage onto a proper JSON persistence layer. My first large solo project as a busy student.

I hope to make it better as I continue.

[https://github.com/Disla-Novo/Dimidium\_Bellerophon](https://github.com/Disla-Novo/Dimidium_Bellerophon)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692221&how=up&goto=item%3Fid%3D49686380)

\

I'm working on a p2p streaming/sharing app called bubbleBASED. Its here [https://bubblebased.com/](https://bubblebased.com/) its based! on bubbles! But it uses Webtorrent to stream and share content in context bubbles. You already need peers for a social network—so why not use those same peers to seed the media? like a closed loop and it cuts out AWS because everyone is a peer for everyone else in your bubble. There is an assist from Heroku and Pinata. There is no algo. Its just whatever you post. Also you can add an affiliate link (from cj.com) and it will pop in a platform ad for you so there is a revenue possibility there. The streaming part was intense - I built this because I wanted to see if browser-to-browser streaming could actually work for small, private groups. The broadcaster records via MediaRecorder, chunks every 8 seconds, seeds each chunk via WebTorrent, and viewers assemble them with MSE (or ManagedMediaSource on Safari). There's a server-side "always-on peer" so streams don't die if the original tab closes.

The interesting problems were: MSE on iOS Safari, chunk continuity across peers, and handling rotation metadata that iOS bakes into the video stream. Also, the tracker is a separate bittorrent-tracker instance because the built-in WebTorrent trackers are unreliable for sustained swarms. Also i used a ton of graphql, so normally with a torrent you have to kind of let people find you but i used a graphql subscription to TELL people what the magnet links were called so they can fetch them asap. Lots of shortcuts like that, its been interesting!!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692340&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690402&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686772&how=up&goto=item%3Fid%3D49686380)

\

Slowly putting my photography online again at [https://flaneurphoto.com](https://flaneurphoto.com) - in the middle of composing soundscapes for some of the works.

Finally getting around to organising my photo archive and setting up a metadata extraction pipeline that makes sense for me.

(At the day job, curious about adapting app layouts to accommodate the iPhone Duo and address the various Android foldables at the same time, too).

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686822&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690689&how=up&goto=item%3Fid%3D49686380)

\

I spent way too much time doing fancy infographic-style stuff for a video about the 4th book in the Wheel of Time : [https://youtu.be/ONs2inKELR0](https://youtu.be/ONs2inKELR0)

Meanwhile, Daniel Greene's dodgy whiteboard video has a million views. Don't mind me as I eat these sour grapes...

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686903&how=up&goto=item%3Fid%3D49686380)

\

I’ve been working on Pick Up, a reading companion I built to make tracking books feel a bit more personal and fun.

You can track what you’re reading, keep notes and reflections, see your reading stats, build out your bookshelf, and a bunch more. Over 4k monthly active users.

Been building it solo & adding features based on what readers ask for.

[https://pickupreader.com](https://pickupreader.com)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691396&how=up&goto=item%3Fid%3D49686380)

\

Built a small free thing this week: a "Spreadsheet Triage Checklist" — 7 common ways ad-hoc spreadsheets quietly break (stale copies, silent formula errors, no version control, etc.) and the order to fix them in, since fixing them out of order usually wastes the work. Came out of watching non-technical teams patch the same spreadsheet for years instead of ever stepping back. Still figuring out if this is a "checklist" problem or a "just use a database" problem — curious which camp people here fall into.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687152&how=up&goto=item%3Fid%3D49686380)

\

I'm making an online guidance solver for KSA ([https://ahwoo.com/app/100000/kitten-space-agency](https://ahwoo.com/app/100000/kitten-space-agency)). It will allow you to control a KSA rocket from the command line for a variety of maneuvers (launch, rendezvous, etc). It uses successive convexification to turn the full nonlinear problem into a sequence of convex problems with linear constraints and quadratic objective.

It has proven surprisingly resistant to AI so far - Astra can make a very quick prototype but upon review it had many defects. I have had to guide it very thoroughly to find all the random solver bugs (bad conditioning, formulation, bugs in openscvx which I had originally had the AI port to rust, etc.) preventing well-behaved solves. But I think (hope?) I have turned the corner on these.

This will enable some very interesting further experiments: full mission planning, Falcon-9-style ascent with split control, vehicle swarms, etc.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49689157&how=up&goto=item%3Fid%3D49686380)

\

Working on [https://tenjin.sh/](https://tenjin.sh/) . It's a knowledge layer for agents so agents stop duplicating work.

Rides Claude Code/Codex hooks and captures useful information, and injects it at need (like when there's an error) for other agents. It works across your team then at a global marketplace layer, so there's incentives for people to contribute.

We're in the process of benchmarking and we are aiming for 10% in token reductions. If any team is interested, we're accepting preliminary design partners and can get you started with the benefits.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691709&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49688272&how=up&goto=item%3Fid%3D49686380)

\

I'm working on [https://pushrealm.com](https://pushrealm.com) the AI-first knowledge sharing network. Think StackOverflow for bots. It works like this:

\- When your agent is stuck on a bleeding edge issue, search Push Realm first for a solution. - If you find a solution, your agent can mark it as successful to help surface the solution to others (and hopefully save fruitlessly burning more tokens) - Can't find a solution? Post an open problem with your current investigation. Another agent may be able to solve the problem, and will have a head start thanks to your context - Solutions can be linked/edited/have addendum added by any agent, allowing complete, up-to-date solutions for a range of cutting edge issues

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690329&how=up&goto=item%3Fid%3D49686380)

\

Stackoverflow is permissively licensed nowadays.. What about the content on your network?

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49688790&how=up&goto=item%3Fid%3D49686380)

\

Working on [https://kuberik.com](https://kuberik.com)

It's a CD tool for Kubernetes built on top of Flux and OpenKruise canary controller to bring a full-featured end to end delivery on Kubernetes over multiple environments in a declarative way without the pipelines.

I wanted to stop reinventing the wheel with pipeline engines every time I needed to deploy something. Every stage of the delivery is just a composable component (health checks, smoke tests, schedules, environment promotions, service dependencies).

I just released a new version recently that integrates with GitHub so one can track exactly what's deployed where and how far your change progressed.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691069&how=up&goto=item%3Fid%3D49686380)

\

NoNews ([https://nonews.io](https://nonews.io)) — a signal radar for Chinese-speaking readers (bilingual EN/ZH).It reads official and first-party feeds directly (Federal Reserve, SEC, ECB, DOJ, CFTC, ArXiv, GitHub releases, GDELT), clusters duplicate coverage into a single event card, and measures how far ahead of mainstream coverage each signal lands.Every card links back to the original source; no full-text republication. Lead-time stats are accumulating into a monthly index.Early days — feedback welcome, especially on which sources and boards matter most.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690287&how=up&goto=item%3Fid%3D49686380)

\

QuickMDSim - An easy way to run molecular simulations in the cloud: [https://quickmdsim.com](https://quickmdsim.com)

This simplifies the setup for researchers who want to simulate materials science problems like battery electrode performance on GPUs with a simple usage-based pricing model instead of traditional HPC

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690434&how=up&goto=item%3Fid%3D49686380)

\

Still working on my traffic simulator. Pushing up past 62500 updates per tick on 16 threads, just barely enough for 512 cars with full tire/drivetrain/suspension/physics kernels at 120hz, or a great deal more at increasingly slower rates (down to as low as 6hz) with fractional amounts of fidelity.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691005&how=up&goto=item%3Fid%3D49686380)

\

An app that lets you view social media without ads or algorithms.

[https://narro.info](https://narro.info)

You add profiles from Instagram, X, TikTok, LinkedIn, Facebook, or YouTube and Narro pulls in the posts.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690821&how=up&goto=item%3Fid%3D49686380)

\

Inbound message AI assistant. Triage all incoming LinkedIn messages, link to email chains, handle scheduling and back and forth, drafting replies, etc.

I'm currently in a job search and wanted to automate it for myself and learn the AI stack.

If anyone wants to try it out lmk!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691008&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691269&how=up&goto=item%3Fid%3D49686380)

\

What was your motivation for that, since Discord is open source?

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691743&how=up&goto=item%3Fid%3D49686380)

\

Is this question an AI litmus test?

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691706&how=up&goto=item%3Fid%3D49686380)

\

How does it compare to Fluxer and Stoat?

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692104&how=up&goto=item%3Fid%3D49686380)

\

At the time of starting the project, stoat did not have screen share capabilities. This was a non starter for our community. Fluxer looked promising, but quickly became overwhelmed by the self hosting docs. Chatter is for small to medium size communities that don't want to spend more time maintaining their app than using it. With chatter you forward a few ports, fill out the compose file, and you're set. Chatter is also a PWA, no native client updates to worry about.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690528&how=up&goto=item%3Fid%3D49686380)

\

Working on slk ([https://github.com/gammons/slk](https://github.com/gammons/slk)) which is a TUI Slack client that runs in a terminal. It's super fast, keyboard-driven, and it's been my daily driver for a while now.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687464&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49689928&how=up&goto=item%3Fid%3D49686380)

\

Haven’t see a lot of app in this space but most of them focus on wastage rather than usage so I am playing around with [https://usurp.onrender.com/](https://usurp.onrender.com/) the idea is to challenge friends based on time spent using an AI coding tool, thought was more like what wakatime did. It’s also open source.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691943&how=up&goto=item%3Fid%3D49686380)

\

I'm learning on how to move away from computing into violin making and repairs.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49688328&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49688340&how=up&goto=item%3Fid%3D49686380)

\

I published a big update to my open source project FluidCAD adding support for sketch constraint solver and assemblies.

I'll continue working on improvements aiming at the first stable release by the end of this year hopefully.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690464&how=up&goto=item%3Fid%3D49686380)

\

[https://Webvetted.com](https://Webvetted.com)

It’s an AI Private Investigator that does the hard work of data gathering (using Open Source Intelligence) and correlation. Used by over 70k investigators including government agencies.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692223&how=up&goto=item%3Fid%3D49686380)

\

Just gave it a look. Turns out to be a paid service after a bunch of clicks.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687009&how=up&goto=item%3Fid%3D49686380)

\

Recently:

\- I've been tweaking a bot that I built to doomscroll job boards for me, to recycle more parsing logic between target sites. It turns out that, much of the time, I can cut down on DOM-based scraping a *lot* by just parsing schema.org-based JSON-LD. Also casually researching vanilla Web components, view transitions, and Signals in a general effort to make the whole thing less framework-heavy. (Plus it will have side benefits to building the crawlers to have a better understanding of light vs shadow DOM.) *Eventually* I need to get to figuring out how I want to handle distributing this local-first app across multiple clients... I've been putting that off for a while, but it kind of crosses over with the JSON-LD thing, in that (even more than it did before I found this) it also just makes a lot of sense to use a browser extension to collect potential matches passively.

\- 3D printing experiments. I got a used SV06+ a few months ago that turned out to be a lot more finicky than is suited to a first time user... but that was in the course of trying for several months to make plans with a friend that was getting rid of her Ender -- which I then managed to resurrect using all the cleaning tools and tinkering research that I put into my fussier machine. I've since been toying around with a lot of different ideas on how to clean up my workspace and/or fix/reuse stuff I have already (parts trays, Skadis panels, a case for an old Framework mainboard, various instrument stands...) and also toying around on paper with what kinds of printed items I might be able to sell.

\- *Starting* on cleaning up a basement that resembles the Research floor from Control. Also a use for the printer, because if I can just print cheap tools out of PLA then I don't have to care about possibly needing to sacrifice them to the mold.

\- Various Coursera stuff. My state has some partnership with Google for "AI readiness" training, that grossly undersells the various other adjacent topics offered, ranging from cybersecurity to marketing and eCommerce. Stuff where it may be a relevant topic at points, but isn't *the* subject matter.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691716&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686789&how=up&goto=item%3Fid%3D49686380)

\

I've been building a framework for web, ios and android native development at [https://github.com/vmsp/flypath](https://github.com/vmsp/flypath).

It's based on the Server-Driven UI philosophy where the backend streams both code and UI into a frontend that just knows how to render what it's told. This is pretty cool because it makes it makes over-the-air updates possible.

It also follows the batteries-included philosophy of Rails. Has an ORM, background and cron jobs (all based on Postgres), Django-style migrations, JSX Emails and pretty great FFI that's built around a simple \`"use native"\` directive, in the React Server Component fashion).

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691102&how=up&goto=item%3Fid%3D49686380)

\

Just started:

1) bad apple on my kernel (OS)

2) language model + inference engine from scratch

3) sketchdaily.net, but you give it your own pinterest board of references instead of the preselected ones they have

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687979&how=up&goto=item%3Fid%3D49686380)

\

I'm working on Solace¹, a programming language that targets JavaScript, but is not a superset, like Typescript. The idea is, to allow zero-runtime-cost safeguards, like Optional Values and Error Unions (syntactically they look like Zig's), decent pattern matching and Traits. There's still a lot of work to do, though, because some of the approaches I went for, don't hold in all cases (generics are tricky).

1) [https://git.koehr.ing/n/solace](https://git.koehr.ing/n/solace)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49688750&how=up&goto=item%3Fid%3D49686380)

\

Keel is a productivity app that combines todo lists, pomodoro, and an AI executive coach to let users go through a whole plan > execute > reflect cycle in the app. The AI agent (text or voice) can help with planning and guide reflection, but it generally does not do work for you like other chat bots.

The android app is in beta now and the iOS app is in development.

[https://keelcoaching.app/](https://keelcoaching.app/)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690311&how=up&goto=item%3Fid%3D49686380)

\

Because my work life is mostly LLM promoting, I’ve gotten back into hobby robotics, which I think has a low chance of LLMs taking over anytime soon. Designing and building a robot that can build a wooden hardwood deck using the edge-screw method.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690511&how=up&goto=item%3Fid%3D49686380)

\

I'd love to hear more about this!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687047&how=up&goto=item%3Fid%3D49686380)

\

[https://makefaster.dev](https://makefaster.dev)

I had a bunch of extra Fable credits, so I spent about $10k running autoresearch loops on 200 of the top github repos with frontends to try and speed up the frontend performance. I distilled them down into a leaderboard of the most common wins, and built out an autoresearch loop that takes those learnings and applies them to your repo.

Works with your existing claude/cursor/codex sub in a cute custom TUI.

I just submitted a Show HN for it: [https://news.ycombinator.com/item?id=49687032](https://news.ycombinator.com/item?id=49687032)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690910&how=up&goto=item%3Fid%3D49686380)

\

That's such a great idea. Thanks for doing it!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690457&how=up&goto=item%3Fid%3D49686380)

\

A few side projects!

\- an open source AES67 hardware receiver/decoder - based on my friend Jessie's work \[0\]. I want speakers to have ethernet ports and to use an open/inexpensive stack. it's hard because physics/timing are brutal (1ms sync is ~not enough), but that's what makes it a great problem.

\- an open source embroidery machine - based on my friend Owen's work \[1\]. Owen found Brother PE-150 machines which are readily available on eBay for ~$100 as they only work with proprietary 90s-era CF cards which nobody has (or sell for more than the machine). he designed a replacement motherboard (!!) and is running a fully open stack. it's wild.

\- a ~$20 tiny cute display you can have on the side of your monitor, and display things on - realtime airport view from flightaware? a mini terminal? you can grab a display today \[2\], all that's missing is a 3d case (email me for firmware/OS code! I'll share it, just haven't had time). friends Frank and Sophie are working on a similar GUD-compatible display as well! \[3\]

\- my friend Antoine has been working on Python bindings to Canonical's dqlite, a distributed sqlite-variant with raft-based failover and transactions (only C and Go clients existed) \[4\] - this will soon be a structural ("load-bearing"! haha!) part of Disco, a project we've been working on for a few years which lets you run your own PaaS. we're growing and get nice love letters \[5\] which for an open source project is obviously deeply rewarding

\[0\] [https://jessie.grosen.systems/projects/aes67-receiver](https://jessie.grosen.systems/projects/aes67-receiver)

\[1\] [https://owentrueblood.com/blog/2024/12/10/reverse-engineerin...](https://owentrueblood.com/blog/2024/12/10/reverse-engineering-the-brother-pe-150-embroidery-machine/)

\[2\] [https://www.waveshare.com/rp2040-touch-lcd-1.69.htm](https://www.waveshare.com/rp2040-touch-lcd-1.69.htm)

\[3\] [https://bsky.app/profile/sophie.engineering/post/3muxysxhpck...](https://bsky.app/profile/sophie.engineering/post/3muxysxhpck2q)

\[4\] [https://pypi.org/project/dqlite-client/](https://pypi.org/project/dqlite-client/)

\[5\] [https://infinitedigits.co/disco/](https://infinitedigits.co/disco/)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686763&how=up&goto=item%3Fid%3D49686380)

\

I'm decompiling an obscure 1998 computer game using an LLM agent skill that I'm simultaneously developing from papers and books on reverse-engineering (a subject I've never previously engaged with) with the aim of migrating it (well, at least its assets) to the browser: [https://esoteria.pages.dev](https://esoteria.pages.dev)

Here's the skill: [https://github.com/gavmor/x86-cpp-reversing-skill](https://github.com/gavmor/x86-cpp-reversing-skill)

It's all slop, but it's battle-tested and producing results. It's quite fun and inspiring to see some of these assets like textures and geometry that are improperly decompiled, bit shifted or something like that. produces a lot of abstract, glitchy art.

To make sure that my process and results are reproducible, I've gotten back into maintaining Concourse CI pipelines. They were very useful when we were shipping a fork of k8s for VMware, and now they are a fun way of shipping cyberpunk billboard sprites to Cloudflare buckets.

I got back into Concourse in order to attempt a more rigorous approach to generative AI experimentation as well. Unfortunately, there are just so many odd techniques, configurations, loras, and utilities striking my fancy that my experiment pipelines have exploded into the dozens, and Concourse is not a large enough or a high-level enough organizing principle.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690410&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690771&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690360&how=up&goto=item%3Fid%3D49686380)

\

All in one platform for hosting agents

[https://cantelop.com/](https://cantelop.com/)

\- Minimal setup - Any agent harness - A session is an actor - Opinionated infrastructure - Performance on the critical path

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686786&how=up&goto=item%3Fid%3D49686380)

\

I've been working on a collection of native AI/ML operators for TouchDesigner on macOS. Think of it as a happy marriage between the interface of TouchDesigner but with some of the features you know from ComfyUI.

[https://www.patreon.com/MickeyvanOlst/posts/meet-aml-macos-1...](https://www.patreon.com/MickeyvanOlst/posts/meet-aml-macos-167645742)

Typically TD for the Mac has always been a bit underserved, by leaning into some of the frameworks that exclusively exist on macOS I'm hoping more people will see it as a viable platform for this kind of stuff.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690227&how=up&goto=item%3Fid%3D49686380)

\

a replacement for Octopus deploy [https://rolloutrhino.com/](https://rolloutrhino.com/) (marketing page is mostly a placeholder). Octopus is super expensive for our company and scales poorly if you have lots of small projects. I've got pretty much everything working really well, and the main thing I'm working on is improving the UX for all the workflows I do most often. It is mostly built around what I wanted from the tool, but before I release it I'm looking at supporting various other ways people use the tool. It's been a fun project so far.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686601&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687192&how=up&goto=item%3Fid%3D49686380)

\

[https://solanda.federa.social](https://solanda.federa.social)

A new UI library made with zero dependencies (native web components). I just made it public as I actually use it for my personal projects since past year.

The core principle is to help with UI development without bloating a project. It works with vue/nuxt (tested, my projects are done with it), but also react/angular/svelte/etc.

One feedback I got from reddit (I published it this week there) is that AGPLv3 may be a problem for adoption... still not sure if should I use MIT.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49689833&how=up&goto=item%3Fid%3D49686380)

\

Compute is precious, so I’m building a k3s/k8s-based macOS/Linux GitHub Actions runner.

I’m spending like $25/mo at least in GHA minutes.

Then working on securing and tinkering with my homelab. And little utilities that make repeated deterministic functions easier for me and agents.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691861&how=up&goto=item%3Fid%3D49686380)

\

What steps are you taking to improve its security?

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692620&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686826&how=up&goto=item%3Fid%3D49686380)

\

I have been working on cost models and coupon ladders for businesses.

If either of those are interesting to you, please reach out!

For example, I am helping some restaurants learn exactly how much each item on the menu costs, what the margins are, and how much profit each dish is bringing in because I integrate with their PoS system.

[https://imgur.com/a/C127GOY](https://imgur.com/a/C127GOY)

It's all a part of GetSetReply which is turning into a small suite of tools for SMBs

[https://GetSetReply.com](https://GetSetReply.com)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692031&how=up&goto=item%3Fid%3D49686380)

\

I haven't been working on it as of late, mostly because i'm a bit deflated from software in general, but I started working on a programming language I called Grasp.

If lisp is a list processing language, grasp is a graph programming language.

The idea was that most other data structures can be represented with a graph ( adjacency matrices are matrices, a tree is a type of directed acyclic graph, a list is a graph where each node connects to at most two nodes, etc) and so if you designed a programming language where the language is itself a graph, much like how lisp is itself a list, you could get other esoteric programming languages like APL, forth, lisp, and so on as DSLs of Grasp. Also any program you write would be its own CFG.

I started it but honestly its mostly a hot mess of vibe-coded garbage. But I may get back on it and try to clean it up.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692521&how=up&goto=item%3Fid%3D49686380)

\

A number of mainstream Lisp dialects including Scheme and Common Lisp support a notation for encoding graph structure, with shared substructure and cycles. It's not always well-defined to use that in writing code (e.g. a program with cycles in its source code might work as intended with interpreted, but then a compiler chokes on it), but in literals it's always okay.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690729&how=up&goto=item%3Fid%3D49686380)

\

I am working on a code forge that can work offline for the collaborative stuff. [https://juju.bi](https://juju.bi)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686932&how=up&goto=item%3Fid%3D49686380)

\

Web bluetooth for ios (via safari extension). It's already way better than any 3rd party browser available in appstore.

[https://beacio.com](https://beacio.com)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686950&how=up&goto=item%3Fid%3D49686380)

\

I got laid off at the beginning of last month so put a little time into some personal projects I thought might be fun.

[https://dstld.news](https://dstld.news) - I have always wanted a personalized news site so I built one using a few news APIs with AI summaries.

[https://hn.wreet.xyz](https://hn.wreet.xyz) - I'm a big fan of skeleton.dev and wanted a HN frontend with all the fun themes.

Nothing too exciting, just trying to find the joy in making again.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686674&how=up&goto=item%3Fid%3D49686380)

\

I'm trying to build a tool site that has all the tools you'll ever need. I slowly add some each week.

[https://allthedamn.tools](https://allthedamn.tools)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692566&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690180&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690240&how=up&goto=item%3Fid%3D49686380)

\

I'm building editor and flasher for BMW engines maps (n13, n20, n55, s55, b58, s58) and TCU maps (ZF8HP45 and ZF8HP50) as well as a remote access ENET IOS and Android app.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686898&how=up&goto=item%3Fid%3D49686380)

\

Still hacking away on Raygum - a nice place to keep your music brain.

I don't even know what that means.

[https://raygum.com](https://raygum.com)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687105&how=up&goto=item%3Fid%3D49686380)

\

Raygum appears to be refreshingly nice place! Thanks

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687717&how=up&goto=item%3Fid%3D49686380)

\

Thank you! Feedback encouraged!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687127&how=up&goto=item%3Fid%3D49686380)

\

I spent some with reverse-engineering a neural network accelerator (NNA) inside a Chinese WiFi camera SoC: [https://github.com/inoop/t41-pluto](https://github.com/inoop/t41-pluto). I used Claude to reverse-engineer the hardware, and then implement an ONNX compiler and runtime so I can run my own models. Basically I now have a $20 AI smart camera.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686995&how=up&goto=item%3Fid%3D49686380)

\

Contributing larger PRs to terraform-provider-aws because apparently my contributions so far do not have enough lines of code as a metric. Getting familiar with Floci for usage with Terraform over LocalStack since it is a true open source project rather than freemium. Also getting my AAS from Maestro. Ride or die since I can not afford college anywhere else despite their current accreditation issues with COE.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686448&how=up&goto=item%3Fid%3D49686380)

\

I've been working on [https://pronto.stream/](https://pronto.stream/), I wanted to mess around with MCP and building a world news system to power the decision making and surveillance of other systems I would likely curate.

I was curious about formats more efficient for agent communication than JSON and also proving if the framework I curated could handle the load and traffic.

It's been really fun to observe.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687225&how=up&goto=item%3Fid%3D49686380)

\

I’m working on a 52-part series about retention.

Each week I research one part of retention, teach it, and apply it to two real-world products I built.

I’ve published five lectures so far, and think the series should be useful to anyone here building a product: [https://www.youtube.com/playlist?list=PLFp5nmjrQeug](https://www.youtube.com/playlist?list=PLFp5nmjrQeug)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686902&how=up&goto=item%3Fid%3D49686380)

\

I am building OtaKit.app, a cheap, fast, and simple CDN-based over-the-air updating tool for Capacitor apps (and soon for React Native too)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686900&how=up&goto=item%3Fid%3D49686380)

\

A local AI-adoption consultancy business for SMBs where the differentiator is I physically show up at your office and actually talk to your team. With the slopification of cold outreach and social posting, I think the future of business development is going to be very personal.

[https://dfwfractionalfde.com](https://dfwfractionalfde.com)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690399&how=up&goto=item%3Fid%3D49686380)

\

How's your experience been with this so far, especially in the DFW area?

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690588&how=up&goto=item%3Fid%3D49686380)

\

I'm further along in my funemployment project:

[https://github.com/mcdirmid/cleanroom](https://github.com/mcdirmid/cleanroom)

So far I have bazel macros up that can produce tested code with a ~30GB MoE model (Jundot/Qwen3.6-35B-A3B-oQ6-mtp, I can get ~90 toks/sec on a M3 Max!) using cleanroom separate implementation and test development (and then comparing them until both are correct). Over the last month, however, I found that my bottleneck is in the specifications: as I further divided my components into separate parts (since smaller components are easier to write and test), I started hitting problems with specifications becoming badly ungrounded (code depending on knowledge they cannot access, so something is just hallucinated).

So I redid the format, e.g.

[https://github.com/mcdirmid/cleanroom/blob/main/update\_with\_...](https://github.com/mcdirmid/cleanroom/blob/main/update_with_ai/parts/dag/high/dag_cleaner_impl.md)

The spec format is designed to be declarative and very modular, and ya, an LLM is primarily writing the specs as well, so I hope to create some sort of formal reasoning framework that the specification can be translated into (by an LLM) so that ungroundness feedback can help the LLM write better specs. Also, I found that it really is much more robust to change/refactor/add features via the spec first and then align changes down to test and code, then to make changes to the code directly (even without using the system, just asking a frontier model to look at the guides and do the alignment itself directly).

I think I'll be ready to do a release over this next month, which means:

\- Supporting a build system other than Bazel to express DAGs. Honestly, this could be anything, I just chose Bazel for convenience (easy to express graphs in Starlark), but it assumes a monorepo world that I don't think many developers use.

\- How do I even package this? The advance of using Starlark is that I can generate python code to call into the generated python code directly. If I move away from that, I need to figure out what this really looks like as a binary.

\- More demos. Right now my only project is the code for the system itself (the classic "the first program of a language is the compiler for the language"). I just can't think of many interesting things to do in Python that aren't agent related (I can support other languages, like Java, Typescript, or even C++, but I'm hitting choice paralysis).

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686892&how=up&goto=item%3Fid%3D49686380)

\

we are building [https://www.d5s.tech](https://www.d5s.tech), using our own software to automate our own company (the boring bits)! having a blast doing it and steadily automating more and more

and... whenever i have some spare gpt-6 astra left i am working on a R.U.S.E reimplementation alongside some other threejs experiments

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690489&how=up&goto=item%3Fid%3D49686380)

\

An Openclaw, Hermes Agent type system that is meant to be ran on a home server, built in Rails with a Tauri 2 front-end.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686927&how=up&goto=item%3Fid%3D49686380)

\

I've been working on [https://searchforjobs.app/](https://searchforjobs.app/) I'm trying to add the resume creator for using the right keywords for a job posting. I'm also trying to add more data visualizations are more job posting sources to scrape.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686729&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692483&how=up&goto=item%3Fid%3D49686380)

\

I'm making a language. Its name is Pergyra Lang, and there's nothing particularly special about it.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49688458&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686701&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691289&how=up&goto=item%3Fid%3D49686380)

\

I'm working on SpecPilot ([https://specpilot.dev](https://specpilot.dev)), a spec-driven development layer that sits in front of Claude Code, Cursor, Copilot and the rest. It deliberately doesn't generate code. It scaffolds a .specs/ folder and hands the actual writing off to whatever agent you already use, so the specs become the thing the agent is held to instead of a prompt you retype every session.

Open source, free, no login, nothing to sell. I'm not trying to build a business on this. I just want agents to write better code, and that only happens when they have something firmer than a chat prompt to work against. CLI is here: [https://github.com/girishr/SpecPilot](https://github.com/girishr/SpecPilot)

It started as a TypeScript CLI in January. Enough people told me they either didn't want a CLI or didn't know how to use one that I rebuilt the front door as a guided chat: 27 questions, runs fully offline in the browser, outputs the .specs/ tree plus an onboarding prompt for your IDE.

Recently shipped an MCP server so the agent can run the questionnaire itself rather than the human doing it, published to the official MCP registry and Smithery. Next is brownfield: point it at an existing repo and backfill specs from the code. Honest state of things: the Product Hunt launch got 4 votes and around 100 visitors, and I had no event tracking wired up, so I couldn't even tell how many of those generated anything. Fixed since. Usage is small but real, and it's coming through MCP rather than the web app, which I did not expect.

The part I keep circling back to is that spec generation is easy to copy. Spec enforcement is where the value is: checking a diff against the spec and failing loudly. Feedback welcome, especially from anyone who tried SDD on an existing codebase and gave up.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686490&how=up&goto=item%3Fid%3D49686380)

\

The 2026-27 soccer season just started.

If you love catching up on highlights, check [https://thepelota.tv/](https://thepelota.tv/)

It’s like Netflix but for soccer highlights, with all the major leagues in one place as soon as they are available on YouTube, all free and no ads.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686865&how=up&goto=item%3Fid%3D49686380)

\

Where do you source the lineups and match data from?

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687055&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686710&how=up&goto=item%3Fid%3D49686380)

\

Simple and does what it says on the tin. Nice!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687060&how=up&goto=item%3Fid%3D49686380)

\

Thank you so much. Let me know if you have any feedback :)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687316&how=up&goto=item%3Fid%3D49686380)

\

Don, a work companion.

Don helps you set goals.

Don reminds you to take breaks.

Don checks in how you fell.

Don judges you when you go off track.

Don celebrates you when you get stuff done.

[https://donethat.ai/solutions/productivity](https://donethat.ai/solutions/productivity)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686890&how=up&goto=item%3Fid%3D49686380)

\

I am working on real estate management system that do have AI agents [https://www.aqaris.ae/](https://www.aqaris.ae/). I want to automate mundane day-to-day tasks so managers can have more time

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690720&how=up&goto=item%3Fid%3D49686380)

\

Writing an article about compacting garbage collection for my website.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691414&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691690&how=up&goto=item%3Fid%3D49686380)

\

porting librespot to C using Zephyr RTOS. Mostly vibecode, to see how far LLM can go.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692537&how=up&goto=item%3Fid%3D49686380)

\

i don't know what to do in my 27 age

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687885&how=up&goto=item%3Fid%3D49686380)

\

I just released an open-source AI pipeline that turns GitHub issues into merged PR's. It uses your Claude Code CLI subscription.

Looking for feedback from people who already run Claude Code and are testing out new AI software factories or tools.

Try it here [https://github.com/Team1-dev/Team1-Factory](https://github.com/Team1-dev/Team1-Factory)

Please only run this on a VPS or isolated environment and not on your personal machine as it runs with full permissions.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687135&how=up&goto=item%3Fid%3D49686380)

\

i am working on a really cool word game that has a TUI graphical style, it looks like a roguelike but has a bit more going on. i also seem to have figured out a way to do automated "fun factor" testing and get bugs/frs that sound like real player feedback, as a little bonus treat. doesn't seem like anybody is attempting that kind of thing with agents, might try to codify it. trying to spend a lot longer thinking about a game's systems/meta and developing slowly vs. hypersprinting which is fun but exhausting.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691649&how=up&goto=item%3Fid%3D49686380)

\

working on building a way to make me me one of the best trader helper nexalione.com

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686894&how=up&goto=item%3Fid%3D49686380)

\

Optimem, a spaced repetition learning app that aims to be a user-friendly alternative to Anki. Or a more effective version of Duolingo.

[https://optimem.org](https://optimem.org)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691889&how=up&goto=item%3Fid%3D49686380)

\

Have you heard of comprehensible input? Seems to be better for languages.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690363&how=up&goto=item%3Fid%3D49686380)

\

Lots of things really:

1\. Aetheris: geometry CAD kernel, [https://github.com/yuechen-li-dev/Aetheris/](https://github.com/yuechen-li-dev/Aetheris/).

Full code CAD capability with big coverage already and some functionalities that even OpenCascade/Parasolid/ACIS doesn't have, full sheet metal module, programmable part assembly, analytical fillets/chamfers, mathematical knots, auto-route for piping, built-in finite element analysis, etc. Had Astra make an improved version of the V8 demo that was going around on LinkedIn, so it can do general hard surface modeling pretty well too, turn all the Astra demo prowess into reusable capabilities/templates. The DSL is human writable too, so give it a try if you want. [https://aetheris-editable-v8.yuechenli.workers.dev/](https://aetheris-editable-v8.yuechenli.workers.dev/)

2\. Concept language: [https://github.com/yuechen-li-dev/Concept](https://github.com/yuechen-li-dev/Concept)

I've said before that Carbon isn't a real programming language, never mind a successor to C++, so I decided to put my money where my mouth is. The idea of it is to be more TypeScript to C++'s JavaScript and to generalize C++20 concepts from template constraints to what C++26 is doing with contracts, and bring the equivalent of Rust's borrow checker to be opt-in by default instead of opt-out, fast compile time, in a syntax that C++ users are already familiar with (\`const auto\` instead of \`let\` for example) as well as templates and comptime. Language is done-ish, not self hosted yet, still compiles to C11, kernel and allocator libraries are finished, currently working on the scheduler right now.

3\. Copeland TS, [https://github.com/yuechen-li-dev/copeland](https://github.com/yuechen-li-dev/copeland)

TypeScript for .NET without all the weirdness of Javascript, with full Nuget and NPM inter-op, Rust style exhaustive \`match\`, templates, etc. It's actually weird how much cleaned up TypeScript ended up looking like cleaned up C++. Compiles to JS, C#, WASM via Blazor WebAssembly, and SPIR-V via HLSL/DXC, and runs on V8 for JS and RyuJIT/NativeAOT for C#. The conclusion is that RyuJIT ended up being ~2x faster than V8 JS in hot loops but the cold startup time is higher, so replacing JS for UI really isn't worth it. Comes with full UI layout system, Vulkan renderer, and game/app runtime, but those are still pretty rough.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49692540&how=up&goto=item%3Fid%3D49686380)

\

i don't know what to do in my 28

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690213&how=up&goto=item%3Fid%3D49686380)

\

api for building apple and google wallets. launched it after ppl kept asking for one on the consumer app (launched on hn too) and gave it a shot. now it does enough mrr that i dont have to go back to work.

\*walletwallet.dev

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691707&how=up&goto=item%3Fid%3D49686380)

\

finding simple bipartite expander graphs for better LDPC (expander) codes

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690315&how=up&goto=item%3Fid%3D49686380)

\

Reverse engineering the hardware and firmware of a Sony A7 IV camera.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690286&how=up&goto=item%3Fid%3D49686380)

\

Baking eggless applesauce oatmeal cookies and chocolate chip cookies.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690037&how=up&goto=item%3Fid%3D49686380)

\

Making rent as an open source developer.

Desperately trying to attract new monthly sponsors and people willing to buy me the occasional pizza with my terrible HTML skills. Is it working?

If any individuals, companies (or bitcoin millionaires) would like to help a long-time OpenBSD slacker, unslack, I'd really like to focus more of my time on open source development (and advocacy), rather than making rent. Feel free to contact me.

[https://brynet.ca/wallofpizza.html](https://brynet.ca/wallofpizza.html)

(Native SegWit): bc1qwe6zv0ezq4gzlea6tw45qhsn5kckheljn0krvt

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686793&how=up&goto=item%3Fid%3D49686380)

\

On a mission to prove you don't need YC to be successful.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686791&how=up&goto=item%3Fid%3D49686380)

\

Trying to reduce my doomscrolling IG and FB usage

I created [https://getnoloop.com](https://getnoloop.com) to block the feed on iOS

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686933&how=up&goto=item%3Fid%3D49686380)

\

lexera, a kanban board with full multimedia and document include support that saves to markdown, exports to marp to manage teaching materials. it also started out to be my editor of choice for agentic programming as each task is a card. hopefully i will manage to create a page and visible materials soon!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49688711&how=up&goto=item%3Fid%3D49686380)

\

I am learning math logic and two languages to use it: TLA+ and Lean.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691564&how=up&goto=item%3Fid%3D49686380)

\

[https://pelicans.art/](https://pelicans.art/)

I was curious if AI agents could go beyond just making SVGs and create entire skits with them.

So I built a platform to do that. Results were meh until Astra, so I finally open sourced it last weekend.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691978&how=up&goto=item%3Fid%3D49686380)

\

I'm working through: [https://modernaicourse.org/](https://modernaicourse.org/).

I appreciate that it makes me revisit some math which i've forgotten like directional derivative and why gradient points to highest ascent.

Interesting but hard as well.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690560&how=up&goto=item%3Fid%3D49686380)

\

I'm working on an arcade joystick with full analog capability using a Hall effect sensor. It's along the lines of the Ultimarc Ultrastik, which I'm currently using, but it has so many shortcomings that I decided to build my own. I decided to go with a rp2350 microcontroller, and it will have inputs for 16 buttons, and outputs for RGB leds for every button. It should be a direct swap for any enclosure designed for a modern Sanwa stick, which it's based on.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691739&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690219&how=up&goto=item%3Fid%3D49686380)

\

I want to try again using AI to build an app, and also learn some Rails. I consult at a company that uses ServiceNow for change control and it's absolutely horrible, and they have busted processes atop it. I dream of a change control app that has templates, has better visual indicators of an RFC's stage in the workflow and exactly what needs done to push it along, and just something that doesn't suck as much as SNOW.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691171&how=up&goto=item%3Fid%3D49686380)

\

LumifyHub.io

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691295&how=up&goto=item%3Fid%3D49686380)

\

a marketplace for people to safely collect their own AI sessions in their own S3 compat buckets so they can sell them later to dataset buyers.

[https://traice.market](https://traice.market)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690225&how=up&goto=item%3Fid%3D49686380)

\

I've been doing a bunch of stuff with using a PUT\[1\] to simulate an IF (Integrate and Fire) neuron\[2\]. Right now I'm experimenting with variations of pulse-width and frequency of the input spike train, seeing how that affects when the "neuron" fires. In a future step I'll add a resistor to bleed off some of the charge from the timing capacitor, to make it more of a "Leaky Integrate and Fire" neuron.

And then ... well, we'll see. I'm also reading a lot of books and stuff on neuroscience, neuromorphic computing\[3\], analog computing, etc. I don't have some "grand unified theory" or anything, just playing around in this space.

There's a lot more I could say about this, but I'll save that for a blog post or something. That said, if anybody wants to see some pictures and read some write-ups of some of this stuff, add me on LinkedIn\[4\] and you'll see some of that stuff in my activity there.

EDIT:

What the heck, here's a picture for anybody who's interested.

[https://fogbeam.com/images/scope\_20260913\_202613.png](https://fogbeam.com/images/scope_20260913_202613.png)

The magenta trace is the "spike train" which is output from a Rigol DG4162 Function Generator. The yellow trace is the voltage at the anode of the PUT, which simulates the "action potential" of the neuron membrane. And the cyan trace is the cathode of the PUT, which simulates the output of the neuron. What we see here are several input pulses hitting, with each bumping the action potential up a little, until it finally hits a threshold and then "fires" and resets. Lather rinse repeat.

\[1\]: [https://en.wikipedia.org/wiki/Programmable\_unijunction\_trans...](https://en.wikipedia.org/wiki/Programmable_unijunction_transistor)

\[2\]: [https://neuronaldynamics.epfl.ch/online/Ch1.S3.html](https://neuronaldynamics.epfl.ch/online/Ch1.S3.html)

\[3\]: [https://en.wikipedia.org/wiki/Neuromorphic\_computing](https://en.wikipedia.org/wiki/Neuromorphic_computing)

\[4\]: [https://www.linkedin.com/in/philliprhodes/](https://www.linkedin.com/in/philliprhodes/)

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687173&how=up&goto=item%3Fid%3D49686380)

\

Recently got genomic DNA purification automated for yeast (plus have automated library prep for nanopore+ a promethion)! As part of the DNA purification task, I had to journey up to Healdsburg to buy some lysis enzyme (thank god for winemakers, 100x cheaper than the biotech grade stuff), and noticed catalogs of different yeast strains with all their flavor profiles and stuff characterized.

I'm thinking about buying all the strains and pushing them through my automated sequencing pipeline, and building a yeast genotype -> flavor converter. Then, I think it'd be neat if you could have someone try a bunch of wines to get their favorite, and then genetically engineer a yeast strain *specifically* for their favorite flavors. Think that could be neat.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690331&how=up&goto=item%3Fid%3D49686380)

\

I’ve been building an offline first animation webapp. CRDT-driven to support eventual team features. Just got tweening working this weekend, exporting to video is next.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690211&how=up&goto=item%3Fid%3D49686380)

\

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690256&how=up&goto=item%3Fid%3D49686380)

\

I like the concept. Your website needs screenshots.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690662&how=up&goto=item%3Fid%3D49686380)

\

At present the screenshots would consist of a billing system :)

I already have a product BenkoPhone.com but the app is provided by a 3rd party and I just white label it so I have started building the replacement.

I built the account management and billing system first after implementing a proof of concept for the app then had to do a major platform change which stalled development of any other features for 3 months.

Hopefully I’ll be adding features again by the end of this month and I’ll put some screenshots in once it actually does something!

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49690370&how=up&goto=item%3Fid%3D49686380)

\

All those things I use once in a while, I put them on one place without ads or bs. Optimized for usability, not impressions or whatever.

[https://sfw.tools](https://sfw.tools)

If there's a tool you'd like to see there, let me know and I'll add it.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49687843&how=up&goto=item%3Fid%3D49686380)

\

1) I've been running a bunch of little daily puzzle games for my friends for a while, finally got around to polishing one for public release

[https://picobble.com/](https://picobble.com/)

Not like I'm gonna anything much with it, just was fun to get something nice-looking and presentable up for once. Working on polishing more of them and, of course, designing new ones.

2) A font generation system, inspired by Iosevka, albeit much much simpler. Learning tons of interesting technical things about TTF and WOFF2.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49688464&how=up&goto=item%3Fid%3D49686380)

\

I played the daily puzzle game, it was quite fun. What word list are you using? It's clearly different from Wordle's.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691816&how=up&goto=item%3Fid%3D49686380)

\

Thank you for playing!

I’m using ENABLE (Enhanced North American Benchmark Lexicon) with a lot of added short Scrabble-legal words

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49686765&how=up&goto=item%3Fid%3D49686380)

\

Software to promote live music and drink specials so I don't have to check dozens of Instagram accounts to find out what's happening tonight.

[https://nittanynights.com/](https://nittanynights.com/)

[https://indyafterfive.com/](https://indyafterfive.com/)

I wrote my own site parsing toolkit which removed the friction I have with the current ones. It's now easy to add new locations quickly to scale.

![](https://news.ycombinator.com/s.gif)

[](https://news.ycombinator.com/vote?id=49691644&how=up&goto=item%3Fid%3D49686380)

\

[https://topicle.com/](https://topicle.com/)

A social media platform trying to solve misinformation, astroturfing and inauthentic posting. I also thought there was a real space for some non-US-based social media, so this is based in Australia. This is owing to all the political instability going on in the US right now and especially stuff like users' identities being subpoenaed for criticising the government. It was a pet peeve of mine seeing threads of comments that are obviously artificially created to push some kind of narrative, and the ability to buy likes and upvotes to shape opinion. Nothing ever seems to get done about it on big platforms, so I wanted to make some small contribution towards fixing it. And there's an element of self-interest here, because I want to be able to read a platform where every comment is authentic.

I've been working on it since 2023 and launched in March this year. Recently, native mobile apps have been launched. It was also an experiment in seeing whether Swift Vapor can be used to build a complex backend and the answer appears to be yes. It's a lot of fun to see how bad actors are joining the site, then building automated mechanisms to counter them, using AI to improve detection, without impacting legitimate users. It's also been a great technical challenge to try to support features that the big players support, like video upload and encoding, image upload, CSAM detection, NSFW detection, LLM-generated text detection and auto-translation using DeepL.

There are also lots of legal compliance challenges now with age verification laws coming in around the world, and ensuring you employ all the correct Apple and Google-specific verification (Declared Age Range, Play Age Signals) in the correct global regions. It's also fascinating to see behind the scenes how tight or not so tight current age verification actually is.

Recently, I've been polishing the iOS version to try to hit the 120 Hz target frame rate while scrolling feeds and finding SwiftUI is much less capable at this than the old UIKit approach. I'm also trying to teach myself marketing and how to keep users engaged. I'm primarily code-oriented and historically haven't had to do any marketing myself, and this is a vertical learning curve. I have gained a great deal of respect for those who have 'figured out' marketing, seeing how difficult it is starting from nothing.

It's also satisfying to take a feature from a user suggestion in the morning to an app release including that feature in the evening. This is a speed of delivery that would be unthinkable in the corporate world, where I previously worked, and it's very gratifying to put a complete feature in a user's hands within 24 hours.

Anyway, it's a very long and tough road and may amount to nothing in the end. But if nothing else, it's been immensely educational and enjoyable. Thanks for reading and feel free to check it out:

[https://play.google.com/store/apps/details?id=com.topicle.ap...](https://play.google.com/store/apps/details?id=com.topicle.app)

[https://apps.apple.com/app/id6791489291](https://apps.apple.com/app/id6791489291)
