---
title: A Spark of the Anti-AI Backlash (on Bluesky)
link: https://eugeneyan.com/writing/anti
source: eugeneyan-com
published: 2024-12-08T00:00:00Z
updated: 2024-12-08T00:00:00Z
first_seen: 2026-11-14T16:02:04.620496262Z
summary: How the sharing of 1M Bluesky posts uncovered the strong anti-AI sentiment on Bluesky.
content: extracted
html: 2024-12-08-a-spark-of-the-anti-ai-backlash-on-bluesky.html
remote_preview:
  url: https://eugeneyan.com/assets/og_image/anti-ai2.png
---

Recently, a dataset of 1M Bluesky posts unexpectedly sparked backlash from the Bluesky community. This incident uncovered strong anti-AI sentiment among Bluesky accounts, leaving the AI community feeling unwelcome on Bluesky. This write-up reflects on what happened, hypotheses on why it happened, and how the data/AI community responded.

* * *

Bluesky, 26th November 2024: A developer shared about the [one-million-bluesky-posts](https://huggingface.co/datasets/bluesky-community/one-million-bluesky-posts) they had collected from Bluesky’s public firehose API. It contained post text, metadata, and information about media attachments like alt text, and was hosted on Hugging Face.

**However, many Bluesky accounts reacted with aggression.** There were profanity-laced comments. There were accusations of theft, privacy violations, and unethical behavior. There were calls for legal repercussions. And there were death threats.

![Negative reactions on the dataset, many of which have since been removed](https://eugeneyan.com/img/backlash_hu_49ecb386c1272817.webp) *Negative reactions on the dataset, many of which have since been removed ([source](https://bsky.app/profile/altryne.bsky.social/post/3lbxe673zs22v))*

**Within 12 hours of sharing the dataset, the developer took it down.** They followed up with a post on Bluesky acknowledging that the dataset collection lacked sufficient transparency and consent, and apologized.

IMHO, the intentions were positive and the transparency was commendable. Such datasets enable research on social media patterns, content moderation, and training language models. The dataset description also explicitly prohibited problematic uses like building bots, generating fake content, or extracting user’s personal information. (To be frank, any well-intentioned data/ML/AI practitioner could have compiled a similar dataset.)

Regardless, **the dataset’s release gave us a glimpse of a potential Anti-AI [Butlerian Backlash](https://dune.fandom.com/wiki/Butlerian_Jihad)** on Bluesky. Because of the developer’s affiliation with Hugging Face, many of their researchers and developers found themselves added to Anti-AI blocklists. On Bluesky, users can create [lists](https://docs.bsky.app/docs/tutorials/user-lists) to curate feeds or mass-mute/block accounts. It’s likely many accounts discussing ML/AI topics were added to such lists and blocked; I know I was.

![Blocklists targeting AI and Hugging Face accounts](https://eugeneyan.com/img/blocklists_hu_6a2bee1bfdf16d4f.webp) *Blocklists targeting AI and Hugging Face accounts*

**In a somewhat cheeky response, new datasets emerged within 24 hours.** One developer posted (on Bluesky) about releasing [two-million-bluesky-posts](https://huggingface.co/datasets/alpindale/two-million-bluesky-posts); as a result, they had their account [suspended](https://x.com/AlpinDale/status/1862069301106065842) and then [reactivated](https://x.com/AlpinDale/status/1862229176620613896) by Bluesky’s moderation team. Others shared [20-million-bluesky-posts](https://huggingface.co/datasets/informatiker/20-million-bluesky-posts), and [bluesky-1M-metaposts](https://huggingface.co/datasets/noteuclaise/bluesky_1M_metaposts) that discussed the original dataset. Overall, this demonstrated the ease of collecting bulk data from Bluesky’s API.

Aside: It’s worth noting that an [even larger dataset](https://zenodo.org/records/11082879) containing “the complete post history of over 4M users (81% of all registered accounts), totaling 235M posts” was published six months ago. This makes the 1M dataset look tiny in comparison. The 235M dataset work was supported by the EU, and organizations funded by the EU.

* * *

To better understand the intense backlash, I dived deeper into the critical comments and examined the accounts that made those comments.

**At first glance, the primary issue *seemed* to be the lack of consent in data collection.** Some comments expressed anger at having their data “scraped” or “stolen” without permission. (Note: The accusation is incorrect—there was no scraping or stealing involved. Posts on Bluesky are public and the Bluesky firehose API is open for all to consume.)

![You still did this without people’s consent in the first place and that kind of makes you an awful person.](https://eugeneyan.com/img/consent2_hu_fdfb76188696588b.webp)

![Apology not accepted This is why consent & TOS matter, & you disgusting techbros don’t care GFY](https://eugeneyan.com/img/consent3_hu_723199a3e329020e.webp)

![This was incredibly gross. Not caring if users opted in or not was exactly the kind of thing that would be a case study in a computer science ethics class.](https://eugeneyan.com/img/consent4_hu_16f1661f3e6ac39a.webp)

![You just stole all our data and set it loose thanks a lot. I hope the data commissioner buries hugging face.](https://eugeneyan.com/img/consent5_hu_bb7de85a7d2cb63e.webp) *Responses objecting to the dataset due to lack of consent*

However, as I explored further, two clusters of concerned accounts took shape:

- Accounts that explicitly object to their data being used to train AI models
- Accounts that post NSFW/mature content

The first group’s comments often mention not wanting their data in “LLM training sets” or not having their creative work used to train AI without consent. Some demanded the deletion of the dataset and any AI models trained on it. Notably, one comment raised that many artists joined Bluesky specifically to avoid having their work “scraped” for training AI, hinting at the larger tension around AI’s impact on creative professions.

![keep my data out of your damn LLM training set now and forever. i don’t want to be in it. we’re not just data to be scraped without consent. i’m never going to give you my consent.](https://eugeneyan.com/img/training1_hu_86248720242d44ad.webp)

![To clarify, did it get used for training? And if yes, are you scrapping that round of training then? Because apologies are nice but it’s largely performative if this is being done after that and you don’t throw out all that’s derived from the act.](https://eugeneyan.com/img/training2_hu_8a3a8768f7f4cdaf.webp)

![You should delete the whole thing. And then go take an ethics class. Just because it’s possible to steal whatever you want from the Internet with impunity doesn’t make it right. I don’t want my work to be used to train any AI. It’s mine until 50 years after I’m dead. And, I’m not dead yet.](https://eugeneyan.com/img/training3_hu_93c7cac0be6b436a.webp)

![Good call. Keep it removed. Consent to the data collection is very important, and there are so many artists that have joined bsky for the sole purpose of avoiding their hard work being scraped for other uses left right and center. It’s only natural you’re seeing massive pushback against this here](https://eugeneyan.com/img/training4_hu_9fb29742aff5cf8d.webp) *Responses objecting to the dataset as training data for AI models*

To validate this, I visited a sample of the critique accounts and found at least a dozen profiles of writers, artists, musicians, and other creatives, **some of whom added explicit anti-AI declarations to their bios**.

![Druzy 👻 21 👻 Art student 👻 comic illustrator, concept design. Video game stuff & general tomfoolery 👻 🌸 No AI 🌸 No NFT 🌸 do not edit or repost 🌸 No scrubs 🌸 🏳️🌈](https://eugeneyan.com/img/artist1_hu_ea736678ed1e6a50.webp)

![3D Animator & Graphic Designer #catmadethat | Anti AI/NFT 🚫 Portfolio: https://www.artstation.com/](https://eugeneyan.com/img/artist4_hu_6cf3ff2240e7ca62.webp)

![3 commission slots open! indie game composer + digital fusion guy | scoring Nagayami Nights, Viral Reload EX, MDF + more | no nfts no ai 🚫](https://eugeneyan.com/img/artist8_hu_359aa8f7db4b56f4.webp)

![They/them. Artist just doing book covers, email info@debutart.com for inquiries. Upcoming TTRPG: Sometimes rant about industry and AI stuff, no worries if you want to mute me bc my posts r stressful, I completely understand](https://eugeneyan.com/img/artist2_hu_6c6e0dcec5d4b59d.webp)

![Artist & Writer, occasionally a bit academic (PhD student - Digital Humanities - Resisting AI). Also a living donor (kidney). 📍 South East London](https://eugeneyan.com/img/artist3_hu_996fd8fbc62561d6.webp) *Profiles of writers, artists, musicians, and other creatives with explicit anti-AI declarations*

I also discovered a second cluster of accounts posting adult-oriented content, typically identified by the 🔞 emoji in their profiles. For this group, the concern may have been having their sensitive posts stored externally without the ability to delete them.

![‼️ Scottish Bi-Ace ‼️ Polar Bear ‼️ NB/Trans (Femme To Be) ‼️ Some NSFW inside ‼️ Undergoing Metamorphosis ‼️ Compliant 🔞 If you follow this account you’re going to see me naked 🔞](https://eugeneyan.com/img/nsfw8_hu_e477dcd834ef46db.webp)

![🔞 | 32/M/US Gulf Coast | Adult Diagnosed Autistic | Sci-Fi #Writer | #Gay | I’m a bull on the internet. Links: Community: Discord:](https://eugeneyan.com/img/nsfw6_hu_4bedfb83f0262518.webp)

![am a awkward gay | She/Her | 37 | 🏴󠁧󠁢󠁷󠁬󠁳󠁿 🔞 Minors DNI HRT: 7/10/21 Nerd | Queeirdo | ASD | ADHD | T4T | Demi | Poly Lesbian (with a hint of bi) 💘 💘 💘 discord: Queer merch](https://eugeneyan.com/img/nsfw7_hu_d60d85482e8e0733.webp)

![he/Her/Its 🏳️⚧️ 25 | AuDHD+ Aceflux Trans Hacker, Software engineer, nsfw poster, and basically a puppy Chicago’s resident Bluey girl I bully cops 🇵🇸 Minors dni 🔞](https://eugeneyan.com/img/nsfw3_hu_fec47a16a89a8c77.webp)

![🔞 | They/them | Non-binary Gen-X Greymuzzle | Furry artist drawing SFW and #NSFW art: ABDL/babyfur, macro/micro, other kinks/fetishes, and wistful nostalgia. Please don’t confront me with my failures. I have not forgotten them](https://eugeneyan.com/img/nsfw12_hu_2fa99b9dba180680.webp) *Profiles of accounts posting NSFW/mature content with 🔞 emoji*

Note: These two clusters only accounted for 10 - 20% of the accounts criticizing the dataset. [Most accounts](https://bsky.app/profile/did:plc:ilhs5ksfpze2l5rqgig4ycl6/lists/3lbxx6amyfk2j) had more generic profiles with posts about everyday life content.

In response to the backlash, many in the data, ML, and AI community shared thoughtful perspectives on the complex issues of consent, using AI for good, and why it happened.

- The nuances of consent for public social media data ([source](https://bsky.app/profile/cfiesler.bsky.social/post/3lbwurkbfcs2w))
- How consent may not apply to online data ([source](https://bsky.app/profile/hdevalence.bsky.social/post/3lbxz3c3ne22w))
- The inherent public nature of the Bluesky firehose ([source](https://bsky.app/profile/tedunderwood.me/post/3lc3qjuz4as2l))
- The potential benefits of AI, such as combating spam ([source](https://bsky.app/profile/howard.fm/post/3lbzo5oarxc2s))
- The inevitability of Bluesky needing to train its own AI models as it scales ([source](https://bsky.app/profile/dorialexander.bsky.social/post/3lc4av4e7qs2o))
- The relative insignificance of 1 - 2M posts for training LLMs ([source](https://bsky.app/profile/stellaathena.bsky.social/post/3lbzqnqmo4k27))
- Concerns about the original developer being bullied ([source](https://bsky.app/profile/vgel.me/post/3lbw5vvycm22e))
- The hypothesis that early accounts were disproportionately anti-AI ([source](https://bsky.app/profile/aidanclark.bsky.social/post/3lbyezan52c2l))

Several folks in the AI community also (publicly and privately) shared their concerns about the potential for escalating anti-AI sentiment on Bluesky, with some unfortunately expressing reluctance towards using Bluesky for now.

> I really liked the community before this all blew up. Less crap cluttering my feed, just quality posts from HF employees and researchers (the only people who really moved there)

> All the examples of Bluesky is pure toxicity from what I have seen. I wanted to check it out in the beginning but feel no desire to do so after seeing the people on there

> This sort of mentality has been going on in Tübingen (Germany) for many years. Despite there being some of the brightest AI minds in Europe, locals strongly oppose any development.

> Everything that happened on bluesky is making me put a pause on my own participation. I thought it was going to be the next data twitter but now I think I don’t need that anti-ai hate in my life.

> anyway if you need someone to talk to about it - just let me know. And I’m not saying this randomly, Me and various EleutherAI folks & gang has been hit on this, multiple times. Also, I know with the direction I’m heading towards, I will be on the target board as well, for more of it. Inevitably.

Despite the unpleasant events, I understand it **comes from a place of concern and fear for many. To alleviate this, we need open, empathetic dialog to build mutual understanding.** Even now, several AI researchers and practitioners continue to engage in [good](https://bsky.app/profile/stellaathena.bsky.social/post/3lc6lxhadq22y)-[faith](https://bsky.app/profile/tedunderwood.me/post/3lc4jxepvv22k) [education](https://bsky.app/profile/dylancastillo.co/post/3lc3jfi564k2r) [about](https://bsky.app/profile/chrisoffner3d.bsky.social/post/3lbyh4ubx6c22) [AI](https://bsky.app/profile/howard.fm/post/3lbzo5oarxc2s) on Bluesky. Nonetheless, it’s unclear if critics are open to engage, especially with the widespread blocking of AI-associated accounts.

For now, I plan to hang out where the community is, and since the community’s on both Twitter and Bluesky, that’s where I’ll be. Given recent events, if you do want to discuss AI on Bluesky, [it’s dangerous to go alone](https://knowyourmeme.com/memes/its-dangerous-to-go-alone-take-this)—take [this 📜](https://bsky.app/profile/did:plc:ilhs5ksfpze2l5rqgig4ycl6/lists/3lbxx6amyfk2j). You can subscribe to it and either mute or block accounts known to be anti-AI and potentially abusive.

* * *

P.S., Unfortunately, I too became a target in this kerfuffle. To reduce the negativity on my feed, I started curating a [moderation list](https://bsky.app/profile/did:plc:ilhs5ksfpze2l5rqgig4ycl6/lists/3lbxx6amyfk2j) of accounts that displayed explicitly anti-AI and abusive behavior. To my surprise, one of the large accounts on the list [shared it](https://bsky.app/profile/dennisbhooper.bsky.social/post/3lc2gxo4yu222) with their ~20k followers (as a badge of honor?), putting me squarely in the spotlight.

![A large account lol-ing about their presence on the moderation list](https://eugeneyan.com/img/shared-list_hu_9bf67204086078bd.webp) *A large account lol-ing about their presence on the moderation list*

Soon, I started receiving anti-AI comments, even on unrelated posts like a [Thanksgiving message](https://bsky.app/profile/eugeneyan.com/post/3lbzrviuv522x). There were also accounts [asking](https://bsky.app/profile/danmcdaid.bsky.social/post/3lc2hpr5cjk2a) [to](https://bsky.app/profile/pfish.zone/post/3lc2hhw6cdk2i) [be](https://bsky.app/profile/suspiraserhead.bsky.social/post/3lc2ivlks7c23) [put](https://bsky.app/profile/politosway.bsky.social/post/3lc2k6rg5m22d) [on](https://bsky.app/profile/indefinitearticles.bsky.social/post/3lc2qkv4tdc2v) the list 🤷

![Anti-AI comments and requests to be added to the moderation list](https://eugeneyan.com/img/personal_hu_f517e51aea37eef7.webp) *Anti-AI comments and requests to be added to the moderation list*

As someone whose writing occasionally hits the front page of Hacker News (aka trolling as a service), this negativity doesn’t faze me much. Nonetheless, it has me thinking about how to turn the list into a self-serving web app that allows accounts to opt themselves in.

Thanks to Xinyi Yang, Hamel Husain, Vibhu Sapra, Eugene Cheah, Zach Mueller, Alex Volkov, and Nathan Lambert for reading drafts and encouraging me to publish this.
