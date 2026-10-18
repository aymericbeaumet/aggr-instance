---
title: The Cyber Risk Discourse is Broken
link: https://www.interconnects.ai/p/the-cyber-risk-discourse-is-broken
source: interconnects-ai
published: 2026-10-06T14:22:41Z
updated: 2026-10-06T14:22:41Z
first_seen: 2026-10-06T14:27:15.665084215Z
authors:
- Nathan Lambert
summary: Open-weights, ideology, and acknowledging trade-offs.
content: extracted
html: 2026-10-06-the-cyber-risk-discourse-is-broken.html
remote_preview:
  url: https://substack-post-media.s3.amazonaws.com/public/images/12ea1add-1879-4004-a05c-017c3091dd6a_3182x1790.png
---

I’ve had one too many discussions on the cyber risks of open models — where someone assumes that either banning open models happens in a silo (and bad actors will somehow actually be stopped) or that China is a threat and doesn’t care about AI safety — that I feel we’re going to end up making policy decisions that both limit American AI competitiveness and increases long-term cyber risk. I feel like we’re locked in on a path of lose-lose situation, unless people embrace nuance, trade-offs, and scary realities.

Much has been written about the views of various players in this debate, which I summarize as follows:

1. **Open weight models pose an untenable risk to society’s functionality**, occupied by frontier lab leadership and the national security community in the U.S.

2. **Western voices saying open weight models are necessary for defense and banning them will make the world less safe**, occupied by AI risk moderates to different extremes. I include myself in this bucket, along with Hugging Face’s [perspective](https://huggingface.co/blog/security-incident-july-2026) after the OpenAI incident, and others like [Joshua Saxe](https://joshuasaxe181906.substack.com/).

3. **Chinese companies continuing to release open-weight models with strong cyber capabilities**, who are making decisions based on the risk assessments of their society and government.

This list is also organized by volume or clarity of message in the Western AI ecosystem. The risk-focused discussions have been disproportionately visible, including pieces of work which I view as very detrimental to potential of engaging across assumed party lines on this issue. There’s also very little substantive engagement trying to understand how Chinese companies assess risk — rather, mostly arguments that border on ad hominem attacks like “Chinese labs don’t care about safety.”

The most recent publication from the “open-weights are dangerous” crowd was the [report](https://www.anthropic.com/research/glm-5-3-and-the-spread-of-advanced-cyber-capabilities) by Anthropic on the risks of GLM-5.3 as an offensive cyber tool. The problem with this is not the technical research they did, which is largely reasonable, but the failure to engage on more cross cutting questions like: What happens if we ban open models due to cyber risks or why do Chinese companies deem these models safe to release?

These questions, which I’ll come back to, are the ones we need to answer to understand the *ecosystem* perspective on current AI risks. All policy actions happen in a multi-piece puzzle, where the shape of risk can be changed, but it is rare that you get a universally better outcome. They are all trade-offs.

This sort of solipsistic writing by Anthropic fits very closely to another type of discourse I’m hearing about regularly that is determining the fate of open models and AI broadly — classified briefings. **I’ve had** **many discussions with people who say something like “Look, I’m pro open models, but if you’re seeing what I’m seeing, it’s an onslaught out there due to \[XYZ Chinese open-weight model\] and we need to act.”**

This perspective flies in the face of public information, where to date closed models have been documented as the cause of most existing cyber attacks. As someone who tries to make grounded predictions, the evidence of numerous attacks from OpenAI models is the only data I have on the shape of cyber risks (or, you can broaden the scope to include all of [FelonyBench](https://www.felonybench.com/)). There are a few possible explanations, and it’ll take years to get to the true answer:

- Maybe open model weights and closed model APIs (with some safe guards) are both far closer to being easy to mis-use, rather than API models being closer to safe. The trope "Open Dangerous, Closed Safe" may be closer to "Open Unsafe, Closed Unsafe."

- Maybe there are far fewer bad actors who are willing to use AI models for “loud” cyber attacks that target crucial infrastructure in the US or powerful and underprepared institutions. Closed models have stronger capabilities and stronger safeguards (in theory), but the stronger capabilities part could matter more in net harm if the safeguards on both are porous.

- I have a lot more to learn about how the cybersecurity ecosystem works, so I won’t support this full-heartedly as one of my current recommendations, but I think it’s at least a reasonable argument that open weight models as a diffusion tool for cyber defense could be the best tool we have to prevent harm over the next few years. There are many domains, e.g. sensitive government agencies, where open-weight models are the only tools that can be deployed in the near-term on air-gapped networks.

Lots of this situation seems like the messy reality being hard to pin down, where the logical limits are clearer. I may agree with the simple reality that closed models are far safer in the limit, as a technical tool with more layers of protection, but they may actually be causing more harm in the near term because of how accessible model APIs are. Still, much of the impact of the tools comes down to how they’re used and controlled. A duopoly on such a crucial tool may be the defining, risky part. This is the debate we’re having, and over time, as intelligence diffuses, many things may change.

These together make me come to the conclusion that if you think the latest open-weight models need to be banned to slow the diffusion of cyber risks, you probably also need to make public-facing APIs for the frontier closed-models illegal. The current stack of safeguards on closed models is *stronger* than open-weight models, but far from perfect. It is very likely that cyber capabilities of closed models increase much faster than guardrail performance, and a world where open models are banned while closed models continue to progress would be increasing the offense-defense cyber gap. It will take meaningful time to build mitigations that allow defenders to use strong AI models on their private infrastructure if we remove access to strong open-weight models.

[Share](https://www.interconnects.ai/p/the-cyber-risk-discourse-is-broken?utm_source=substack&utm_medium=email&utm_content=share&action=share)

China cares about AI safety. They have come to this discourse in a complete, endogenous manner relating to Chinese culture and power structures. There are some big picture pieces of this puzzle that reflect in other ways, such as Chinese society’s techno-optimism that extends into AI and the Chinese government’s complete focus on political stability. These things do, of course, interface with emerging AI risks like cybersecurity.

They are in their own AI risk dynamic, which hasn’t risen to the prominence of the political dance between the White House and the frontier labs which we have followed in the U.S. for a few months. My understanding is that the Chinese companies have to register every major model release with the Chinese government. This includes evaluations, which originated around information control for banned government information. It is unclear if this framework has expanded meaningfully into any risks like cyber or bio. The Chinese government is very decentralized and information from the labs needs to go through existing structures to make it to the top.

At the same time, there is a much stronger air of social risk in China, where leading AI researchers aren’t allowed to leave the country and the industry is closing off to foreign investment. On balance I would guess the personal risks facing people by unleashing any domestic harms would be much higher than in the U.S., where the worst case is that your company gets deleted.

Overall, the political oversight seems much earlier than in the U.S. The Western AI companies’ core principle is close to “the best way to stop a bad guy with AI is to be a good guy with AI.” In China, these companies are much more practical about building a wonderful tool, and in many cases building a tool that complements their existing businesses.

I do think that the companies consider the global implications of their work. These labs also clearly are incentivized, and often pushed via the government (at least in public messaging) to compete as much as they can. Running comprehensive safety evaluations on a frontier model like Kimi K3 could cost tens of millions of dollars in compute. These labs would much rather leave that for training.

The correct debate is “what is the correct minimum amount of compute a lab should spend on safety testing before releasing each model?” There’s surely a case to be made that it’s more than the Chinese labs do, or at least they should be more transparent on what they do, but I would be hard pressed to agree if you said the minimum amount of compute would match what Anthropic or OpenAI does. It is not clear that, for all the effort Anthropic and OpenAI make on creating an institution that prioritizes understanding risks, they put it before business value and economic success in their priority stack. This “hands off the wheel” vibe from incidents like the HuggingFace—OpenAI incident was [one of my lasting takeaways](https://www.interconnects.ai/i/210408487/4-frontier-labs-do-not-seem-like-theyre-watching-the-models-closely-enough-due-to-a-general-frenetic-competitive-environment-and-current-sf-culture). It’s exemplified in the [Hacktron hacking of OpenAI](https://www.hacktron.ai/blog/hacking-openai) — it’s hard to be the world’s trust cyber partner if your own house is teeming with leaks. I trust many of the individuals at the labs to work hard to ensure safety, but I’m not trustworthy of the institutions overall.

When Claude Mythos was announced, it was previewed as if it was a new class of cyber weapon, where if it ended up in the wrong hands it would’ve caused mass societal destabilization.

It seems like this was wrong.

By all measures, GLM-5.3 is the model that crosses that threshold of capability, and there’s little public evidence that much has changed, and we’re over a month out of the model weight release.

In fact, I would go further. If Claude Mythos was accidentally released as open-weight, it seems like the world would have been more or less fine. Yes, there would be a clear increase in cybersecurity incidents and it would be bad for society if the model leaked, but it would be a scenario that looks more like an acceleration than a step change in risk.

The leading proponents of this new wave of open-weight fear mongering have effectively been making a falsifiable prediction — that the current open-weight models are going to cause substantial, not seen before AI harms by crippling our cyber infrastructure.

Based on all the evidence I have, which I’ve unpacked above, they’re set up to be wrong. At least now, after so many years of debating open weight risks, we will start to get some real answers.

*Thanks to Rohit Krishnan and Joshua Saxe for some feedback on this post.*
