---
title: Janet Reno was talking about my friend Coolio (computer criminal)
link: https://michael.bacarella.com/2026/11/16/janet-reno-vs-my-friend-coolio/
source: lobste-rs
published: 2026-11-16T16:43:58Z
updated: 2026-11-16T16:43:58Z
first_seen: 2026-11-20T16:02:15.998933044Z
authors:
- michael.bacarella.com by scanner_brightly
labels:
- culture
- security
summary: Comments
content: extracted
html: 2026-11-16-janet-reno-was-talking-about-my-friend-coolio-computer.html
remote_preview:
  url: https://michael.bacarella.com/assets/mbacface.jpg
---

Published on 16 Nov 2026 by mbacarella

A war story from EFNet #goonies: the February 2000 DDoS attacks on Yahoo, eBay and buy.com, the teenager who launched them, and what happened to him afterwards

AI disclosure: this story was written entirely by human hands, with AI doing proof-reading and light edits.

No, not the rapper Coolio, but rather [Dennis M. Moran](https://en.wikipedia.org/wiki/Dennis_Moran_%28computer_criminal%29). No dishonor intended for the real [Coolio](https://en.wikipedia.org/wiki/Coolio), the rapper, though arguably mine was harder.

There I was, standing on the couch in my basement, pointing at the TV. Janet Reno is giving a press conference on CNN, vowing to bring these cyber terrorists to justice. My jaw is open, I’m laughing. I can’t believe it. She’s talking about my friend Dennis “Coolio” Moran, a 17-year-old.

But let’s back up.

We were the #goonies on EFNet. We broke off from #doom, following our friend chunkk, the [legendary Doom player](https://www.doom2.net/history/history.html) (I still haven’t seen The Goonies movie though). Some of these people are still very close friends and at the time they were much closer friends than anyone I had IRL since they were computer elite and my local friends were all llamas. Above all, we were still very teenage and very dumb.

They were fond of robotripping, since it was all the drugs teenagers could afford. As a suburban kid from Queens from a loving supportive family I knew drugs were bad and would sit this stuff out. I had a job at a dialup ISP in Manhattan called nyct.net as a FreeBSD sys admin. I was moving up in the world.

Others, not so much. The most extreme robotripper and psychonaut on #goonies was Coolio. He was also a lot of fun. He basically never spoke ill of anyone. Everyone loved him. Girls would fall for him just watching him say stuff on IRC over my shoulder.

He was also the poorest friend I’ve ever had. He grew up in Wolfeboro, New Hampshire. His mom wasn’t in his life and his dad was, as far as I knew, a decent fellow, though they struggled to make ends meet and he had lived out of a tent for a period of his life. People mostly sort their friends and they get a picture of the world that’s very curated. Dennis (and indeed the rest of IRC) was my window out of my Queens NY bubble.

We did eventually hang out IRL. Dennis came to visit me in NYC and we went to Mindless Self Indulgence shows together, which were insanely fun. After one of them the (freakishly tall) lead singer Little Jimmy Urine was offering kisses for $5 (“VD free”). Dennis offered $20 over the heads of a bunch of clawing girls and he and Jimmy Urine made out.

When he wasn’t talking about drugs or girls, he was talking about hacking. We were all dabbling in computer stuff. We loved Doom and Quake and we had aspirations about becoming game developers. I bought a book on C and installed DJGPP and was trying my best to learn, but lost interest in games and pivoted to networks and coding on Linux. And also hacking. Though it had been out for years, I had just caught up with that Phrack article about Smashing the Stack for Fun and Profit and was curious about how to apply it. Dennis was interested too, and we thought about how cool it’d be to write a PoC exploit together.

Dennis found out about a BIND vulnerability with no PoC. We wrote one together. It worked! We were pumped. I thought we should promote internet health by publishing the PoC, since that was how you got people to fix bugs in those days. Dennis goes quiet for a few days. Then he comes back. Bro had been busy, he hacked into all of the vulnerable servers and recruited them into an insane botnet. I was shocked. WTF was he going to do with this? I thought we were just learning and there he goes hacking into half of the DNS servers on the internet! At least he patched BIND for them afterwards.

Dennis reads about the [CD Universe extortion attempt](https://time.com/archive/6740163/extortion-on-the-internet) and sees a business idea. What if he combines his botnet with [smurfing](https://en.wikipedia.org/wiki/Smurf_attack) to do massive DDoS attacks? Then he could threaten to knock sites offline if they didn’t pay. I thought this sounded crazy but he insisted on trying. His reasoning was you can’t just cold email executives out of nowhere and try to extort them. You gotta build up a reputation first. So he flexed his muscle and took down Yahoo!

He would say “watch this” and the sites would go down. First Yahoo, then buy.com and eBay, then etrade.com.

He picked buy.com because it was the day of their IPO. What better way to show his power than ruin a company during the dot com bubble on their IPO day.

In our teenage chat room this all made sense. Like a lot of teenagers (and adults), Dennis thought wealth was often illegitimately acquired, and that it was only fair to take from the rich in the bullshit days of the dot com bubble. Thug life, kind of. Definitely worked on the girls, which was probably not healthy for him.

To learn more about his views on wealth maybe we should go back to Wolfeboro. I visited twice, once while he was in jail and once just to hang out and party, and it was absolutely nothing like where I was from in Queens. According to Google it’s a small town of 6,000 known as being full of lakefront summer homes. The part I saw was very much not summer homes. It was houses in the sticks. The rich, such as Mitt Romney, were vacationing in his town while his family was, at times, living in a tent.

Which brings us back to the basement. I’m standing on the couch, pointing at Janet Reno on the TV, laughing. She says they’ll find whoever did this. I know who did this. I know what he used, too, because I helped write it. Are we all going to jail??

Dennis wasn’t done though. He had previously rooted DNS servers and defaced [dare.com](https://attrition.org/mirror/attrition/1999/11/14/www.dare.com/) and [cwc.gov](https://attrition.org/mirror/attrition/1999/11/14/www.cwc.gov/), but took the opportunity to deface [rsa.com](https://attrition.org/mirror/attrition/2000/02/12/www.rsa.com/) after the DDoS attacks. He did recruit his friends on IRC to help him write ad copy for the defacements. I am sad to admit some of those are my words.

He was especially tickled by defacing RSA since they had just put out a press release advertising that they could help defend against his DDoS attacks. Rekt.

This played out over the course of a couple of days. Being teenagers on IRC, we didn’t exactly all keep his secret.

We were “infiltrated” by an assistant security officer at Stanford named David Brumley. He came on calling himself “aforce” and he reeked of How Do You Do Fellow Kids energy, but it still took us way too long to kick him out. It was all for naught anyway: he was tapping the ircd at Stanford, an EFNet server, and could read absolutely everything, private messages included. Coming into the channel was probably just to provoke us into revealing even more. Oh, uh, it also didn’t help that Dennis’s RSA defacement specifically taunted aforce, which directly linked someone in #goonies to the crime.

Once word got out that US digital infrastructure was being brought to its knees by a robotripping 17-year-old, the intensity died down. They ultimately did arrest Dennis, and showed him a thick stack of IRC logs of teenagers bullshitting, which is as hilarious as it sounds. He admitted to defacing the web sites, and to getting into Army and Air Force computers at four military bases, but denied launching the DDoS attacks.

As it turned out, we were not all going to jail. Nothing happened to me. No FBI visit. No emails from third parties. Nothing. It was as if I had never been in the room.

Meanwhile, in Canada, a copycat named [Mafiaboy](https://en.wikipedia.org/wiki/Michael_Calce) took credit for all of them. I’m not saying he did none of them. Dennis didn’t launch DDoS attacks against news sites since he thought they would help spread the story about his underworld reputation. So, it’s possible Mafiaboy did do those. No dishonor intended for Mafiaboy either. I’m sure he was a legit cyber criminal in his own way.

Dennis didn’t exactly get off scot-free. He was sentenced to a year in county jail and was released after nine months. He was also ordered to pay $15,000 in restitution, $5,000 of it to RSA. A security company collecting damages for getting hacked. IMO that situation is a wash.

When I visited him in jail, the Carroll County House of Corrections, I was expecting him to be depressed but he seemed really upbeat about it. He was almost having fun. He was treating it like a sleepaway camp, and the other inmates seemed to love him. While we were talking I overheard an extremely loud, very powerful toilet flush. He said something nuts like “Oh, yeah. Some kid joined our block the other day. We tried to flush him down that toilet but he wouldn’t fit so we decided he was cool.” (Years later he would tell me another story, this time about prison. Dennis was a not-big white guy, and a very large black man who had just moved to his cell wrapped a towel around his head and yanked it so hard that Dennis flipped off the bed and hit the ground. Terrifying. Dennis got up laughing his ass off, and the guy ended up being his best friend in there.)

And don’t just take it from his IRC friends that he was lovable. Paul Zimmerman, a local businessman who wrote to Dennis in jail and then visited him there, [told a reporter in 2001](https://www.foxnews.com/story/teen-hacker-coolio-gets-fresh-start-as-head-of-computer-services-company): “I found him to be a most delightful person.” And: “He was not angry or vindictive or mad at the system. He was taking his licks in an honorable way.”

Anyway, at that age you don’t usually get such an early view of law enforcement, but here it was: they didn’t really care about the truth of the story. They knew full well Dennis did it. The stack of IRC logs printed out screamed that he did it. The FBI has them. They know. But Mafiaboy was already willing to take the rap so why bother indicting Dennis.

Dennis’s story has not fully been told to this day because he did not have a rehabilitation arc. He didn’t turn it into a career the way Mafiaboy did. Not because he never had chances: Dennis was offered a security engineering gig at a local firm but one day just walked off the job, bored.

The years after that went in cycles. He’d get straight and hold down an honest job in a sandwich shop for a year or two. Then he’d lose touch with reality and do some nonsensical crime. He’d sober up in jail, which he seemed not too upset about, and then he’d be back to normal.

One time he dropped LSD and started going through people’s mailboxes. He was convicted of that. Soon after starting his jail sentence he came on IRC, and he told us he had secured work release, helping a local strip club with their web site. Doing his time meant he’d leave jail, go to the strip club, hang out with strippers and smoke weed, then go back to his cell in jail at night. When society needed Dennis to repay his debts this is usually what it had in mind.

He spent a while bouncing around the country. He had lots of friends from IRC who welcomed him into their homes, often romantically. Ultimately he settled in the Bay Area.

I wish the story ended there. We thought he’d eventually figure it out.

This story is a secret we’ve been keeping for Dennis all of this time: he did launch those DDoS attacks, and we watched him do it. There’s really no need anymore. Dennis had a secret of his own: he was addicted to heroin, and he hid that from us for years. It seems he found a community of heroin addicts out there, and it got out of control.

I have some guilt about all this. Dennis went to jail, which is probably where he learned about heroin. A few of the rest of us on IRC got questioned in person by the FBI. Absolutely nothing happened to me.

I really loved Dennis. We all did. We tried to help Dennis but couldn’t. He threatened to kill himself if we ever said the word “rehab” around him. He dabbled in dealing research chemicals in the Bay Area, and on the darkweb for Bitcoin, but was eventually busted and the walls began to close in around him as his court date loomed. He became paranoid and started lashing out. He was using all of the time. The last thing that happened between us was that I joined a different chat room I knew he was in. It was on a different platform, so I picked a different nick, as a joke, but one I thought he would easily grok as me. He didn’t recognize the nick and freaked out. He accused me of being a narc. I told him I was sorry and that I wasn’t trying to stress him out, but he just quit in a huff. I never talked to him again. He disappeared from IRC for several weeks. I ended up hearing from mutual IRC friends that he had been found dead in his room, in 2013 of a heroin overdose. Age 30.

There was never a proper goodbye. On some level I still don’t accept that he’s gone.

I still have dreams sometimes where Coolio connects to IRC and joins the chat, and we all shoot the shit like nothing had ever happened.

*Did you know Coolio? Did he DDoS attack you? :P Let’s talk about it on [X](https://x.com/mbacarella/status/2122355335885422599), [Bluesky](https://bsky.app/profile/michael.bacarella.com/post/3n2gutmtxos2f), [Mastodon](https://infosec.exchange/@mbacarella/117626904151960541) or [Hacker News](https://news.ycombinator.com/item?id=50518786).*
