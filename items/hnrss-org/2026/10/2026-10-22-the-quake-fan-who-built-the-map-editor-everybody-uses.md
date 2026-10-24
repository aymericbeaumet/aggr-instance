---
title: The Quake Fan who built the map editor everybody uses
link: https://funcarena.com/articles/the-quake-fan-who-built-the-map-editor-everybody-uses
source: hnrss-org
published: 2026-10-22T10:55:22Z
updated: 2026-10-22T10:55:22Z
first_seen: 2026-10-24T09:25:49.691482789Z
authors:
- klaussilveira
content: extracted
html: 2026-10-22-the-quake-fan-who-built-the-map-editor-everybody-uses.html
remote_preview:
  url: https://funcarena.com/images/logo.gif
  alt: func_arena
---

**The Quake Fan who built the map editor everybody uses**\
![](https://funcarena.com/images/spacer.gif)\
 Posted by [**webmaster**](https://funcarena.com/members/func_klaus) · Oct 20, 2026 06:11pm \
![](https://funcarena.com/images/spacer.gif)

![](https://funcarena.com/images/spacer.gif)

![](https://funcarena.com/images/spacer.gif)\
 Back in 2010, a Mac user named Kristian Duske wanted to make Quake maps again. Small problem though: nobody had built an editor Duske actually liked. ToeTag by Warren Marshall was out there, but the workflow just did not click. So Duske, who already had some 3D programming under the belt, did the only sensible thing: wrote a new one from scratch.

Fifteen years and a couple of full rewrites later, that editor is TrenchBroom. If you have played a custom Quake map in the last ten years, there's a good chance it was born inside of it. Mappers swear by its 3D first workflow. Indie teams used it to build modern retro shooters like Wrath and GRAVEN. Godot projects have been using it with Qodot and other extensions. And the Quake scene, which by all means should now be gathering dust next to the Voodoo2 in your parents garage, is having one of the best runs in its history, with TrenchBroom sitting right in the middle of the party.

The punchline? Duske never made another map. Not one. Not in TrenchBroom, not in anything else.

We caught up with Duske by email, somewhere between a full time programming job at a music company, a family, and a worrying number of hours in Control Resonant. We talked about the spark that started it all, the code that should have stayed simple, the move from wxWidgets to Qt, the CSG algorithm that refused to be tamed, and the fine art of maintaining a project of this scale. Grab your rocket launcher. Here we go.

**What was the spark that made you create TrenchBroom? Was it a small itch, or had you planned it for a while?**

I started TrenchBroom in 2010 because I wanted to make maps again, but I was (still am) a Mac user and couldn’t find a level editor that I liked. There was ToeTag by Warren Marshall, but I wasn’t super happy with the workflow. Plus, I had already experience with 3D programming and wanted to improve my skills. So I started TrenchBroom as a macOS (then called Mac OS X) app, written in Objective C and C only. [This version was released in 2012](https://celephais.net/board/view_thread.php?id=60674).

People were interested in the 3D only approach, but since most Quake modders and mappers used Windows at the time, they couldn’t use it. I decided I would start over, but this time using C++, which I also wanted to learn, and with portability in mind. TrenchBroom 1.0 came out a year later, initially for Windows and macOS: [TrenchBroom 1.0 release](https://celephais.net/board/view_thread.php?id=60908). The Linux builds came out a bit later. Since then, there has been one major rewrite (TrenchBroom 2.0 in 2018) and in 2020 we switched the UI frameworks from wxWidgets to Qt.

Ironically, I never made a single map again, using TrenchBroom or otherwise...

**It's been 15 years since April 2, 2011, the first commit on GitHub. What would the Kristian of 2011 be most surprised by in TrenchBroom today?**

That it’s so successful and that I’m still working on it. I wasn’t expecting that at all when I started it. And maybe the 2D views, because I originally didn’t want to add those at all.

**Any design decision from the early days you would take back?**

Lots! TrenchBroom has a lot of over engineered code that I’m still cleaning up to this day. I wish I had kept things simpler back then and used much less object orientation. I have spent enormous efforts in undoing past mistakes.

**How has your relationship with the project been all those years? Any moments you stepped away or nearly quit? What made you come back?**

There have been fluctuations, which you can see on my [github profile](https://github.com/kduske?tab=overview) if you go through the years, but overall it has been relatively constant. I never quit the project, so I never had to come back either. Lately I have been more motivated to work on it because AI support allows me to take on tasks that I couldn’t take on alone before. For example, I was able to reorganize the project into smaller libraries and improve the build system using AI, which I had been wanting to do for a long time, but doing that manually in the evenings would have taken too long. These quality of life improvements make working with the code much easier, so now I can focus on the more interesting features and contributions.

**The Quake mapping scene has had a real renaissance in recent years. How much of an impact do you think your technical decisions in TrenchBroom had?**

 That’s a tough one to answer, but I think a few factors may have contributed here:

- The 3D centric workflow allows for more direct control and quicker iteration. I also believe that using 3D views predominantly instead of 2D views influences how mappers think about their designs.
- Linked groups make it much more pleasant to establish and evolve a design language. Some designs in recent maps have clearly been influenced by this feature (look for symmetry).
- TrenchBroom supports large maps relatively well, so mappers can create such maps and work comfortably with them.
- We use the textual map format instead of a custom format for storage, so maps can be edited in other tools. This makes it easier to use other tools for tasks that TB doesn’t support so well.

I hope that the sum of my technical decisions (and UX decisions) have enabled mappers to reach flow state easily. Flow state is fun and rewarding, so people spend more time using TB to make maps.

**What was the hardest feature you had to implement in TrenchBroom?**

The hardest feature I never got right was a nicer CSG subtraction algorithm. I wanted it to produce the same output that a mapper would produce, with no unnecessary added vertices and nice symmetry features etc., but it’s a very hard problem to solve. The problem is NP complete, which is a comp sci term that means “proven to be untractable for large inputs”. I tried this for months with many approaches, but they all failed, so I gave up and did the simple approach that we have now in TB.

The hardest feature that did make it into TB was linked groups. Linked groups affected almost everything else, and it took quite a while until the feature became stable and performant, in particular with the supported protected entity properties. But I think the effort paid off handsomely because if used well, linked groups can transform the entire mapping workflow and make it much more efficient and fun!

**Was the switch to Qt worth it? Do you think it's still relevant?**

Yep, that was worth it. It was almost fully driven by Eric Wasylishen, who was my co-maintainer at the time. wxWidgets forced us to make many platform specific work arounds, and Qt abstracts most of these away, so the code is much nicer to work with.

Making code nice to work with is quite important for a hobby project like this. I really don’t want to spend my free time working on ugly and hard to maintain software, so I invest a lot of time into making the code nice to work with for me. Opinions vary of course, so what I find hard to work with might seem terrible to others...

**What was it like supporting several Quake-derived engines? What kind of brush quirks did you have to battle with?**

Daikatana has the most quirks of them all, and TB still doesn’t support them all 100%. Other than that, it wasn’t too hard, although we are still lacking full support of a bunch of engines. For example, Quake 3 support is still not finished, but I’m working on it.

**What about the different texture projection formats?**

The original Quake format only has axis aligned texture projection, which distorts the textures on slanted surfaces, making texture lock impossible. That’s why we need the Valve 220 map format. I don’t think anybody is still using the original Quake format anymore. Linked groups also wouldn’t work very well without exact texture lock. There’s a third option that TrenchBroom doesn’t support yet: Brush primitives from Q3Radiant. Brush primitives don’t offer anything else than Valve 220 maps that I’m aware of, so it’s not exactly a high priority for me to add support for them.

**How did you get into gaming? And what made you cross the line between playing games and wanting to learn more about them?**

I started playing games on our home computer back in the 80ies. I played point and click adventures and the early flight simulators like Secret Weapons of the Luftwaffe, or X-Wing by Lucasarts. One day my brother got ahold of Wolfenstein 3D and from then on I was hooked on FPS games. I played Doom, mail ordered a Doom level editor and fell in love with mapping. Then came Quake and Quake 2 and online deathmatch. I downloaded and played lots of custom deathmatch maps for Quake and Quake 2 and released a few myself using WorldCraft and the original QERadiant. I became a part of that scene and made some life long friends. It felt like a natural transition from being a player to making my own content. I was quite young, maybe around 20, the internet was still new and the possibilities were endless.

I also started my first Quake related programming projects back then, starting with a converter tool that allowed me to use QERadiant, a Quake 2 editor, for Quake 1 maps. Later I ported the FitzQuake engine to SDL so that I could play Quake on my Mac. But I originally started programming even earlier when I was around 16 or so. Back then I was very much into electronic music and bought a couple of synthesizers and samplers to make Techno, but since I spent all my money on hardware, I couldn’t afford the software. Being naive, I decided to write my own sequencer and MIDI utilities. I had taken some basic programming class in school and learned everything else from books and offline manuals. Later I studied computer science and now I work as a programmer for a music company, so I somehow ended up where I started, only with slightly more understanding of what I’m doing.

These days I mostly play games on my Playstation 5. I love The Last of Us, Death Stranding and Control. Currently I’m playing Control Resonant, which is why TrenchBroom progress has taken a bit of a nose dive. I still play Quake, but I don’t play every released map like I used to in the past.

**How do you maintain the vision? How do you keep motivated?**

I don’t have a super clear vision, just a few guiding principles. These have become very ingrained into my thinking over the years so it’s essentially second nature and what appears as a vision maybe just arises from these decisions. I try to keep TB’s UX and workflow simple. Sacrifice some control for ease of use. Don’t introduce too many options or preferences. Don’t overwhelm users. Don’t surprise them.

Of course I don’t always succeed. The TB user base has grown a lot in recent years, and I’m not a part of the other communities that use TB as much as I am a part of the Quake community, so I don’t understand their needs and requirements as well. For me, TrenchBroom is still a Quake level editor first and foremost, so that informs my decisions and priorities when working on features, but sometimes these decisions and priorities are at odds with the requirements and wishes of other communities. But TrenchBroom is open source, so people can fork it and with AI tools, they can adapt it to their workflows.

What motivates me is seeing what the Quake community creates with TrenchBroom, and how TrenchBroom enables people to be creative. That really makes me happy. You don’t need much to make Quake levels. You don’t need a super beefy computer or additional hardware. Almost any laptop will do. The tools are available for every major platform. So Quake mapping is sort of democratic and inclusive by design, although not all communities haven’t always been so welcoming to everybody. TrenchBroom has also been used by indie developer teams to make retro shooters such as Wrath, and that makes me proud, too.

**Is it hard to say no to contributors?**

In the beginning, it was more difficult to say no because there was much less interest, let alone contributions. To me it makes a big difference whether a feature request would benefit a lot of people, or whether it just benefits a vocal minority, in which case I’ll usually deny it nowadays. I try to be graceful but I know I don’t always succeed. I answer issue reports and pull requests during work breaks or in the evenings before dinner, and sometimes my answers will be brief. Some things have been discussed to death years ago, and when such a topic comes up again, I’lll just say “I don’t want to discuss this again”. That doesn’t seem very graceful on the receiving end, but I’m just one guy with a family and full time job. I simply don’t have the time to write a lot of prose, or I wouldn’t get any feature work done at all.
