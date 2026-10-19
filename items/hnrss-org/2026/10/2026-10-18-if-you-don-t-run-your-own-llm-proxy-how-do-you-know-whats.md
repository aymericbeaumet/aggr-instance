---
title: If you don't run your own LLM proxy, how do you know whats going on?
link: https://jrdsgl.com/if-you-dont-run-your-own-llm-proxy-how-the-fuck-do-you-even-know-whats-going-on/
source: hnrss-org
published: 2026-10-18T21:09:12Z
updated: 2026-10-18T21:09:12Z
first_seen: 2026-10-19T12:57:26.464953148Z
authors:
- amrrs
content: extracted
html: 2026-10-18-if-you-don-t-run-your-own-llm-proxy-how-do-you-know-whats.html
remote_preview:
  url: https://jrdsgl.com/content/images/2026/10/Adorable-Plush-Creature-with-Iridescent-Orb-2.png
---

I think the title captures the idea fully but I'll dig into it because this ain't Twitter.

You're working an idea, a piece of code, or a multibillion a year SaaS app which is probably saving the world. Maybe you're automating the cloning process of grabbing new Labubu release images and taking the 2D image and using a ComfyUI to translate it into a 3D CAD to get the replica production started faster because CNY is on the horizon and global manufacturing halts around that time. Maybe you like to use your tokens to search against the most powerful computer software ever developed for macronutrient information for a McRib or some shit.

You're most likely chatting with some type of frontend thats tied into a paid subscription from the provider of your choice. TUI, GUI, Voice call, whatever. It's your data going in and their data coming back for you. That's what they're selling, I'm happy to buy that right now. They'll show you something like this as proof of work happening:

![](https://jrdsgl.com/content/images/2026/10/Adorable-Plush-Creature-with-Iridescent-Orb-2.png)

![](https://jrdsgl.com/content/images/2026/10/Fluffy-Orange-Mascot-Jumping-in-a-Bowler-Hat.png)

yikes y'all

That is a distraction. That is not token usage statistics with time series data meant for engineers. I can't make a decision with this bullshit. Surely the big labs will have valuable insight to share with the curious user who digs into the Analytics panel.

![](https://jrdsgl.com/content/images/2026/10/Screenshot-2026-10-17-at-11.36.38---PM.png)

I can't make a decision with this bullshit. I hear you already, "there's OTel export options!!1!". Cool, I guess. This is one reason, here's 5 more off of the top of my head right now:

- multi provider, multi models, at a single endpoint you control
- logs of which key called what model with what client at what time and its status
- use virtual api keys to stop littering your single main key over your work, agents, and harnesses
- its infra you control
- you can pivot model and provider extremely fast without resetting everything up (again, probably, for like the 3rd time)

I'm not going to suggest a proxy provider for you to use, only that you run it yourself somewhere you control. When you pick one, don't forget you can modify it for your actual needs. I've modded mine for better insight, for custom MCP server endpoints, additional features. Whatever you choose, you should be getting something along these lines for data.

![](https://jrdsgl.com/content/images/2026/10/Screenshot-2026-10-17-at-11.44.14---PM.png)

When I see a cache miss during a coding session, I can pinpoint why that happened in 20 seconds by hand. I haven't automated it but the proxy I use has an API, so it's possible to automate.

Own your infrastructure, your application visibility, data pathing, and storage. I do this at home because it's fun and it's how I like to learn. I do this at work because I'm not doing my job right if I don't. I may never fully understand how the fuck a large language model works well enough to white board it in an interview. But I can wrassle it down and make sure it's instrumented while serving inference.
