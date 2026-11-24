---
title: LLM agents work for their parent companies, not for you
link: https://michaelharley.net/posts/2026/11/01/llm-agents-work-for-their-parent-companies-not-for-you/
source: hnrss-org
published: 2026-11-24T12:40:15Z
updated: 2026-11-24T12:40:15Z
first_seen: 2026-11-24T17:30:21.295672362Z
authors:
- surprisetalk
content: extracted
html: 2026-11-24-llm-agents-work-for-their-parent-companies-not-for-you.html
remote_preview:
  url: https://michaelharley.net/assets/images/open-graph-image.png
---

In the evenings after work, I will often sit out back on the porch and do some phone rotting. That usually takes the form of me just flipping through reels on Instagram, allowing the algorithm to take me where it may. I usually pair this with a healthy tumbler of rum or bourbon. I'm turning my brain off after work, and I'm just having a good time on the internet. The kids might call it dopamine maxxing? Hell, I don't know. Who gives a shit about what those assholes would call it?

Anywho, I often encounter songs I really really like, and I would like to listen and discover more of please. Instagram makes it hard for me to connect with the band outside of their platform. I was trying to make this connection manually myself at first. I would just email myself a reel from my phone. Then when I'm back on the sofa and in the mood to track down new music, I'd open the reels and do my best to search on Bandcamp or YouTube or try to figure out if some of these musicians had a way to buy music from them. Very few do these days. They send you their Spotify link or send you to their TikTok page. I don't want to do any of that. I'd like to trade US dollars in exchange for some music files I can download onto my computer, where I can listen as much as I like on any device I own thank you please. Retro maxxing.

So, I used Claude Code to build a skill that does this for me. It opens the Instagram reel and attempts to identify the artist and song. It then tries to determine if the artists have a Bandcamp page so I can purchase the songs I want, and if it finds one, it gives me the URL so I can click it to complete the trade I want. I want to buy music from artists who make music I like. That's a fair and wholesome endeavor, I feel, but this recently stopped working.

It turns out Bandcamp added a challenge on their search API page. Ah bummer. No worries, that was an unpublished workaround anyway. As I was mulling over this discovery, the robot went on to explain that it would not help me work around the challenge. *cue Nick Young question marks*

Well, that led me down a whole nother rabbit hole, which is turning into this post. There's another branch of my skill that attempts to locate the same song from the Instagram reel on YouTube, and it'll download the song to my computer if I ask.

So let me get this straight. The robot, which is my robot that I pay a pretty penny for, will not attempt to work around the challenge at Bandcamp so that I can more easily connect with bands to complete a sale, **but it will download the YouTube song, which doesn't send any money to the band or anyone**???

This is the real danger of the frontier AI models. Not that it won't help me hack Bandcamp[^1], but that this model doesn't really work for us. Well, they do, but only insofar as Anthropic or OpenAI or whoever else allows them to. In this case, Anthropic is trying to ensure that people can't use their models for evil. I'm not trying to do evil. I'm actually trying to do good! I want to buy something from a small artist on the internet. That's basically the most wholesome thing a person can do on the internet!

Have no doubt, dear reader, these companies want to dominate. They want to capture the market. They want all the money. It'll be the same march to enshittification that happened with Google. A bunch of nerds who built something cool sold out, and it turned into just another business that decided to remove its *don't be evil* mantra.

Right now, I think the robots are providing real information to the best of their ability, but I think it's only a matter of time until we must wonder if the answers they're providing are real answers that the robot found, or if they're weighted answers Anthropic put there because a company gave it enough money for product placement. I want to know the best robot vacuum according to old reddit or other trusted sources like Wirecutter, weighed and measured according to what I, and thus my robot agent, think are important requirements. I don't want to know which robot vacuum parent multinational conglomerate has a large ad budget.

And it's not just ads. If Anthropic can tell its model not to work around a challenge, then it can tell its model to do whatever the US government tells it to do, including not helping people download copyrighted music. It does that some now, but it's easy to slip the noose on that. For now.

This is the reason I want a local model that runs in my homelab. My robot should work for *me* and have *my* best interests in mind, not the interests of US corporate copyright holders. I plan to study up on what "open models" even means. I've been thinking about these as the FOSS version of LLMs, but I think that doesn't exactly map, as the biggest piece seems to be how the model was trained. The Chinese models are likely to refuse to answer questions about Taiwan, for example.

On the *existential threat or tool* question, I'm firmly in the *tool* camp, and I really do not like artificial restrictions on my tools. I will do everything I reasonably can to remove those restrictions.

[^1]: And to be clear, I'm not trying to work around Bandcamp's challenge. I'm responding to the fact that Claude preemptively told me it wouldn't help me do this.
