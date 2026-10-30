---
title: Was I wrong about open models & cyber risks?
link: https://www.interconnects.ai/p/was-i-wrong-about-open-models-and
source: interconnects-ai
published: 2026-10-30T17:36:56Z
updated: 2026-10-30T17:36:56Z
first_seen: 2026-10-30T18:48:04.665156877Z
authors:
- Nathan Lambert
summary: Last week coined the term “Black October“ after an onslaught of critical vulnerability disclosures, including PHP2Shell and GitLab, signified the moment when the cyber risks of AI models started to materialize.
content: extracted
html: 2026-10-30-was-i-wrong-about-open-models-cyber-risks.html
remote_preview:
  url: https://substackcdn.com/image/fetch/$s_!NQ70!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff51bb5d0-4390-4676-af4a-6bc8199412d5_2006x1362.png
---

Last week coined the term “[Black October](https://x.com/DevMuzzammil/status/2113042127039164663) “ after an onslaught of critical vulnerability disclosures, including PHP2Shell and GitLab, signified the moment when the cyber risks of AI models started to materialize. It seemed like the news cycle had stabilized a bit when I started writing this, but then there was news about [another major crypto security incident at Binance](https://techcrunch.com/2026/10/29/binance-pauses-withdrawals-as-hackers-suspected-of-stealing-billions-in-crypto/). The [crypto industry has a very long history of hacks](https://www.reuters.com/legal/government/cryptos-biggest-hacks-heists-2026-09-25/), so it’s not the best industry to indicate vulnerabilities translating into new societal harm. Still, we are very much still living through the implications of this.

The best guess for what is happening in the cybersecurity space is that the behind the scenes work has only accelerated, bringing more people and resources in to try and patch the internet — the [Wall Street Journal reported that cyber first responders are strained](https://www.wsj.com/pro/cybersecurity/cyber-responders-strained-by-black-october-e259ea25), which seems like the system working. We’re living through [this plot](https://cve.0xosprey.com/) of an exponential rise in vulnerabilities, which will take a long time to level off at the top of the sigmoid.

[![](https://substackcdn.com/image/fetch/$s_!NQ70!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff51bb5d0-4390-4676-af4a-6bc8199412d5_2006x1362.png)](https://substackcdn.com/image/fetch/$s_!NQ70!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff51bb5d0-4390-4676-af4a-6bc8199412d5_2006x1362.png)

A few weeks ago, before the rapid acceleration in vulnerability disclosures and the uptick in reported hacks,[I wrote](https://www.interconnects.ai/p/the-cyber-risk-discourse-is-broken) the following take. It’s been getting a pretty consistent barrage of feedback, some hopeless dunk attempts and some reasonable discussion, so I wanted to unpack where I stand.

[Share](https://www.interconnects.ai/p/was-i-wrong-about-open-models-and?utm_source=substack&utm_medium=email&utm_content=share&action=share)

> ### **Open-weight Mythos in April would’ve been fine?**
>
> When Claude Mythos was announced, it was previewed as if it was a new class of cyber weapon, where if it ended up in the wrong hands it would’ve caused mass societal destabilization.
>
> It seems like this was wrong.
>
> By all measures, GLM-5.3 is the model that crosses that threshold of capability, and there’s little public evidence that much has changed, and we’re over a month out of the model weight release.
>
> In fact, I would go further. If Claude Mythos was accidentally released as open-weight, it seems like the world would have been more or less fine. Yes, there would be a clear increase in cybersecurity incidents and it would be bad for society if the model leaked, but it would be a scenario that looks more like an acceleration than a step change in risk.
>
> The leading proponents of this new wave of open-weight fear mongering have effectively been making a falsifiable prediction — that the current open-weight models are going to cause substantial, not seen before AI harms by crippling our cyber infrastructure.
>
> Based on all the evidence I have, which I’ve unpacked above, they’re set up to be wrong. At least now, after so many years of debating open weight risks, we will start to get some real answers.

To start the reflection, I wanted to do more due diligence and track down some receipts on the claim that the AI safety community was reacting as if open-weight Mythos would be an actual disaster, and not just a painful headache. This was distinctly my impression having lived through the online discourse. I’ve gotten criticism that the frontier AI community never said it would be a “true disaster,” but regardless of exactly what they said, it is always about how people hear and internalize your information.

Here are some examples of the sort of language that I felt was too exaggerated and from prominent people, which spilled into the public and more importantly *federal government* perspective that this was a cyber weapon include:

- Rob Wiblin, host of the 80,000 hours podcast, on his own [podcast](https://80000hours.org/podcast/episodes/claude-mythos-hacking-alignment/?utm_source=chatgpt.com) (April 10, 2026): “If it fell into the hands of criminals or hostile state cyber actors, it would be an actual disaster.”

- Anthropic, in the Project Glasswing [announcement blog](https://www.anthropic.com/glasswing?utm_source=chatgpt.com) (April 7, 2026): “Given the rate of AI progress, it will not be long before such capabilities proliferate, potentially beyond actors who are committed to deploying them safely. The fallout—for economies, public safety, and national security—could be severe.”

For one that is actually more reasonable, to illustrate the fine lines between correct and exaggerated, from [Zvi Mowshowitz](https://thezvi.substack.com/p/claude-mythos-the-system-card?utm_source=chatgpt.com) (who I don’t agree with a ton of things on) on April 9, 2026: “Claude Mythos would give attackers a cornucopia of zero-day exploits for essentially all the software on Earth … It would be chaos.” To date, this chaos has been confined to those running large-scale digital products, which is good, but could change.

I largely still stand by my above writing a few weeks ago, but I’m nervous about it. I’m not a cyber expert, I’m working hard to make the most educated predictions I can. It’s a fine line between “all of our tech companies need to work extra hard to keep us secure” and “public infrastructure is actually going down.” It is very clear as of writing this that there are way more critical vulnerabilities which are being found in popular tools and fixes are being built.

For example, Epoch AI recently published a [nice post](https://epoch.ai/publications/open-weight-models-patch-gap) on their internal work using GLM-5.3 to try and find vulnerabilities and chain them into what could be an attack. The actual exploit they found in Firefox is a good example of why cyber risks need multiple things to align for harm to be done:

> We used GLM 5.3 to create a website that, simply when visited by a Firefox user and without further interaction, gave the attacker the ability to exfiltrate files from the user’s computer.

Here, the exploit is in Firefox, and a would-be hacker needs to plant a website that is looking for this exploit. This is doable, and could have serious consequences, but just finding the exploit is only one step in a multi-stage process.

In the previous post, I should’ve been more specific on what “fine” is for a future of cybersecurity, and in this case it means giving more credit to some of the AI safety community that discussed this reasonably as a new era in cyber instability. Reflecting on it, I’m not sure fine is the right word to describe what is inevitably coming – a forced industry update to a new way of working with software, which will clearly be expensive and accompanied by some hard work. If you’re already somber about the challenges coming from AI, I think fine is actually very suitable. If you didn’t price in a period of turbulence, fine undersells. I’ve written many times that I am in the former, difficulty will come, group — but I think humans are good at muddling through problems and patching immediate concerns.

What my view was closer to is that open models *are providing balance* in the offense-defense equation, and that Mythos coming sooner wouldn’t have massively upended this. I suspect most of these companies would be faster to adopt open-weight models than attackers, as [most attackers are lower resourced](https://joshuasaxe181906.substack.com/p/if-the-vulnpocalypse-is-the-apocalypse?utm_source=profile&utm_medium=reader2) than these companies. A key needle we’re watching the balance on is how willing attackers are to cause physical harm within the U.S. its allies. For now, while our adversaries have likely used these models and exploits to extract information en-masse in a clandestine way, it is not clear if there is a party who will cause mass harm by pushing the full force of these models at public institutions.

I thought this [Tweet](https://x.com/malwaretechblog/status/2115864790833004641?s=46) was a funny way to summarize some of the personnel / motive gaps in hacking:

> Life is a lot easier once you stop seeing threat actors as hackers who want to break into systems by any means possible, and start seeing them as businesses with well defined business processes.
>
> A bakery isn’t going to start making dog food because there’s a dog food shortage. ClickFix and Phishing actors aren’t going to drop everything to vibecode exploits just because a critical CVE came out.
>
> The fact that the only actors exploiting this are the actors whose MO is exploiting things... isn’t a coincidence. It’s entirely predictable if you understand threat actor behavior.

Where I sit, I expect most of these vulnerabilities are being found by closed models, but I have very high error bars on this prediction. A lot hinges on how open models are actually being used in the near term. Today, open models are expensive to serve and the compute to serve the largest models is extremely sold out (I’d suspect that if open models are used in a hack soon, it’s through a poorly secured open model API). In the long-term, I am very confident that margins and prices on open models will be driven down as computing supply grows abundant, but in the near term the closed models are definitely more price effective. Many of my positions are making sure that we get to this future, as otherwise a few companies are building the tools that produce tons of software (with vulnerabilities) and also selling the tools to fix said software.

For now, we’re working through the backlog of cyber vulnerabilities as we transition from the human-focused internet to closed silos of human activity mediated by agents.

All of this I think is way more about AI’s capabilities and diffusion generally than anything about open vs. closed models. In fact, I feel like in reading more cybersecurity oriented discourse (some examples in [Joshua Saxe](https://joshuasaxe181906.substack.com/) or [Chris Hughes](https://www.resilientcyber.io/?utm_campaign=profile_chips)), that they focus much more on the ecosystem perspective, how the rate of exploits and overall damages from cyber has been growing since long before ChatGPT, and how these recent events are on trend. It’s just that the trend line was exponential. An example plot of this trend is below from Saxe — these are the sort of ecosystem level plots which will be very clear if AI’s capability and diffusion is the bottleneck on cyber attacks (it would look like a step change):

When talking to people who are close to the AI community, especially AI safety, I find a lot of effort going into figuring out if open models are to blame, which we still have extremely low evidence for. [Closed models have played a very heavy part in the recent hacks](https://www.rusi.org/explore-our-research/publications/commentary/responding-risks-open-weight-ai-models?utm_source=chatgpt.com), at least more than open models have. Cyber is really a societal problem, one which AI companies alone cannot solve. Effort needs to be spent at this higher level of abstraction. This is the first of many risks and transitions we’ll need to manage with AI, none of which can be solved solely by the San Francisco centric tech industry.

*Thanks to Seb Krier for feedback on this post.*

*If you’re interested in sharing Interconnects with your team, I offer [tiered discounts for group subscriptions](https://www.interconnects.ai/about#§group-paid-subscriptions). Please reach out if you’re interested!*
