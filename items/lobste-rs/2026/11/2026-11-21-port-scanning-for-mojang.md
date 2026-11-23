---
title: port scanning for mojang
link: https://sipacid.com/blog/port-scanning-for-mojang/
source: lobste-rs
published: 2026-11-21T23:40:51Z
updated: 2026-11-21T23:40:51Z
first_seen: 2026-11-23T01:33:27.333975467Z
authors:
- sipacid.com via lina_sh
labels:
- rant
summary: Comments
content: extracted
html: 2026-11-21-port-scanning-for-mojang.html
remote_preview:
  url: https://sipacid.com/blog/port-scanning-for-mojang/index.png
---

last christmas i was incredibly bored and decided to bring my old minecraft server scanner [slowstack](https://sipacid.com/projects/slowstack/) back to life. i originally made it to find [liveoverflow’s ctf server](https://www.youtube.com/watch?v=Ekcseve-mOg&list=PLhixgUqwRTjwvBI-hmbZ2rpkAl4lutnJG). it’s basically just a port scanner with the minecraft protocol hooked up to it: it scans the internet for minecraft servers and displays them in a pretty format on a website.

a few months into running this project, i noticed someone had registered an account with a @mojang.com email address and thought: surely that can’t be the real mojang? am i in trouble? within the next few days i received an email from mojang. to my surprise, they weren’t asking me to shut my project down, but rather asking if i could help them debug an issue with my api. attached was a screenshot of their “investigation tool”.

![a screenshot of mojang’s investigation tool](https://sipacid.com/_astro/investigation-tool.B5CzpuYH_Z12RXIP.webp)

their tool showed that mojang was also using other services like mcscans and the one from [cornbread2100](https://cornbread2100.com). at this point i still had no clue what they were using the data for, but i didn’t really care, because i had mojang, the company behind minecraft, as one of my users, which to me was really cool. so of course i helped them fix their issue.

## banned servers

though my views on mojang did eventually change. one day i stumbled upon [this reddit post](https://reddit.com/r/MinecraftServer/comments/1vwi3nb) where op talked about getting an email from mojang, forwarded by their isp, saying their minecraft server violated mojang’s guidelines.

now this wouldn’t be that weird if they were hosting a gambling server, but in this case it was just a private server for friends and this was not the only instance. there were more posts from people having a somewhat similar experience. even a popular server provider had been getting emails from mojang because their customers’ minecraft servers were violating mojang’s guidelines, and all of this started around the same time mojang registered their account on my website.

after reading all this i was convinced that mojang had been using my data to ban minecraft servers. to confirm my suspicion, i looked through the logs and indeed found mojang searching for inappropriate server descriptions, such as the n-word, “pussy” and much more.

![a screenshot of mojang searching for inappropriate text in the website logs](https://sipacid.com/_astro/inappropriate-server-logs.BKApNBeW_Z1MFr1F.webp)

now you might be thinking “but how can we trust people from reddit?” well, we don’t have to. mojang publishes [a hashed list](https://sessionserver.mojang.com/blockedservers) of banned ip addresses and domains/subdomains. cross-referencing that with my scanner’s data turned up some fun ones. mojang banned a server whose favicon was a blahaj and whose description was just “faggot”, and another that does nothing but quote lil wayne.

![a screenshot of a minecraft server with the favicon of a blahaj and “faggot” as its server description](https://sipacid.com/_astro/slowstack-banned-server-faggot.B9FugMPk_ZpfUJe.webp)\
source: [slowstack.tv/dashboard/servers/178.196.125.27/25565](https://slowstack.tv/dashboard/servers/178.196.125.27/25565)

![a screenshot of a minecraft server with the server description: “i’m in the ocean looking for shark pussy” -lil wayne](https://sipacid.com/_astro/slowstack-banned-server-lil-wayne.BPA_p00x_1dHQLe.webp)\
source: [slowstack.tv/dashboard/servers/75.128.109.134/25565](https://slowstack.tv/dashboard/servers/75.128.109.134/25565)

yes, most of the servers mojang has banned using my data were *technically* violating their guidelines. their [guidelines](https://www.minecraft.net/en-us/usage-guidelines) state that you can’t be “unlawful, deceptive, obscene, harmful, or abusive”, servers that make money have to be “suitable for audiences of all ages”, and the [community standards](https://www.minecraft.net/en-us/community-standards) have a “zero-tolerance policy towards hate speech”. none of it actually mentions motds, server descriptions or the blocklist, though.

that said, these servers could have been banned for violating the guidelines in some more serious way that we don’t know of. so, to rule that out, i asked some of my friends to set up private, whitelisted servers with minor “inappropriate” server descriptions, and within a week their servers were also banned by mojang.

## but why would they use third-party services?

great question. surely they could just use all of the telemetry that they have been collecting; after all, it’s their software. unless, of course, they never collected that data in the first place. but it’s just one claude prompt. surely they could add the telemetry to their game, unless for whatever privacy reasons they can’t do that. really makes you wonder why they are using third-party services instead of just handling this like any other company.

for a while, i just ignored the issue. i mean, yeah, kids’ minecraft servers were getting banned because of some friendly banter, but servers from nazis and racists were also getting banned, so i told myself “it’s whatever” and stopped looking at it.

that was until about two months ago, when i got an email from mojang suggesting i should also start scanning the ipv4 space for minecraft bedrock servers. up until then i’d only been scanning for the java edition, because that is what i mostly played on.

![a screenshot of mojang suggesting i should implement bedrock support for the server scanner](https://sipacid.com/_astro/bedrock-support.BPT4hZW9_1TvzdC.webp)

really mojang? have you thought about adding telemetry to your server executables instead of asking some unemployed loser to get the data for you? i obviously *love* doing charity work, so i immediately started refactoring the codebase to add bedrock support :)

## please give me all of your data

a few weeks later, after handling dozens of forwarded abuse complaints from people who don’t know what a firewall is (and the occasional upset university), i didn’t feel like dealing with them anymore and shut it all down, with 24 hours’ notice. i even emailed mojang to let them know, which they responded to by asking for data exports and (indirectly) the source code of the project.

![a screenshot of mojang asking for data exports and source code](https://sipacid.com/_astro/data-exports-source-code.3FC4NwGk_Z1CPaas.webp)

no, mojang, you can’t “snag” my data. if you want a minecraft server scanner, just use [matscan](https://github.com/mat-1/matscan) or pipe masscan output to a shitty python script. or, you know, offer to cover my hosting costs and i *might* reconsider.

## closing and final thoughts

mojang using third-party services that port scan the internet to ban private minecraft servers is insane. if the data were collected via telemetry, it’d still be insane but not as insane. instead, a company owned by microsoft is getting its data for free from random hobbyists like me, and the people getting banned just get a scary email from their isp.

*ps: i will send packets*
