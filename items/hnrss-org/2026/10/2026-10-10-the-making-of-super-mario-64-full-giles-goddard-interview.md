---
title: The Making Of Super Mario 64 – full Giles Goddard interview (2001)
link: https://pixelatron.com/blog/the-making-of-super-mario-64-full-giles-goddard-interview-ngc/
source: hnrss-org
published: 2026-10-10T19:28:41Z
updated: 2026-10-10T19:28:41Z
first_seen: 2026-10-12T22:35:36.108955126Z
authors:
- gregsadetsky
labels:
- giles goddard
- making of
- super mario 64
- writing
content: extracted
html: 2026-10-10-the-making-of-super-mario-64-full-giles-goddard-interview.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50036287
  num_comments: 77
  points: 205
---

![](https://pixelatron.com/blog/media/Super-Mario-64-U-snap0035.jpg "Super Mario 64 (U)  snap0035")

*Seeing as it’s Mario’s 25th birthday and all that, seems like a good time for this!*

*This is the unpublished, unedited version of a 2001 interview with Nintendo coder Giles Goddard about the making of the N64’s Super Mario 64. Parts of this appeared in NGC magazine (that article is [referenced on the Super Mario 64 Wikipedia page](http://en.wikipedia.org/wiki/Super_Mario_64#cite_note-goddard_interview-24)), but this much bigger version has lots of juicy behind-the-scenes ’90s-era Nintendo stuff that didn’t make it into the article.*

*The notes are a bit rough. And the ‘questions’ are all guesses as to what I originally asked – for some reason I didn’t keep a record of what I actually asked at the time. Fool!*

**What was your official job title?**\
 I can’t remember what my official title was, but I think I was front-end programmer or something like that. I basically just did the Mario face. It was supposed to be something like it was loading – even though it wasn’t on CD there was still lots of stuff going on in the background, sorting itself out.

**How many people were on the Super Mario 64 team?**\
 Probably about 15 people in the end. Probably not as many as people think. But then again there was some other people doing stuff behind the scenes. Compared with Zelda *\[Ocarina Of Time\]*, it was a very small team.

**What was the Nintendo office like?**\
 It was an open-plan office, but everybody had their own little cubicle. Depending on what projects they’re working on, they shift around. It is all open plan, in as much as you have rows of people sitting there working. But then where’s there a project going on, the core team would basically grab hold of these programmers and artists around them, and you’d get the Super Mario group expanding outwards from the centre.

I think at that time the artists and the programmers were actually separated out, so you had an artist group and a programmer group. I was actually right behind the Mario programmers team who were working with SGI on the N64 APIs and stuff. That part was quite small, so you could stand up and shout at someone who was working on Wave Race just away from someone working on Mario 64. Quite a good little set-up.

**Was it a bustling office?**\
 No talking! Particularly in Japanese companies, it’s very very quiet. Occasionally you’d get little groups of programmers or artists getting together for a chat, and somebody higher up would walk over and give them the eye, and they’d sit down and shut up. It’s nothing like English or US companies.

**What kickstarted the Super Mario 64 project?**\
 Well, obviously it was the first game, another Mario game was the most obvious game to do. It started way before there was any hardware, it started on these Onyx emulators, the old SGI Onyx. They had an N64 emulator, the first thing we got from SGI. It was an emulation of the API, not the hardware. At that time, Nishida-san – main programmer who started the entire project off – he was working on a GL and this emulator.

The emulator was quite close to the GL, so he had these little Mario characters running around on the Onyx, or API Indys later.

![](https://pixelatron.com/blog/media/Super-Mario-64-U-snap0031.jpg "Super Mario 64 (U)  snap0031")

**What role did Miyamoto play?**\
 There was only 3 or 4 projects going on at EAD by that time, he was in charge of all of them. Three of those weren’t high priority, so he was 100% Mario 64, he was always there. Sitting down with a machine and playing with the various demos. One of the things he loved doing was just playing down with experiments.

That’s the way the Mario face came about. At the time, I was thinking of things to make that would show off what N64 could do. One of the things was real-time vertex calculations. In games, you usually have a fixed set of vertices that you simply move around in a matrix – you just move a shape around. We wanted to move around individual points, morph it. No games used it at the time, we didn’t know what to call it, so I named it meshes or something like that. I was playing around with this thing with a GL program on the Indy, playing around with this Mario face with an Indy cam, a cam you get with Indy – two ping-pong balls for a face. Miyamoto said: can you make a Mario face out of that for the N64?

It was one of the first games that let you play around before you’d even started – and I don’t think it was written anywhere in the manual that you could actually play around with Mario’s face. Another Nintendo secret!

**So did Miyamoto start with Mario running around on his screen?**\
 Usually that’s the first thing to go in, some kind of map system where the characters can figure out what height they are, the camera can figure out what it can see and how much it can see, figure out how to split up the scenery so it only draws as much as it needs to draw. That was the first thing, we were just playing around with Mario. He was wandering around a simple grid to start with, just picking stuff up, dropping stuff.

The animation was quite central to what he was doing. A lot of the animation was actually in there before any of the game. The Mario that Miyamoto had running around basically looked as he did in the final version.

**The camera system was revolutionary for the time.**\
 One of the reasons for that was the Miyamoto didn’t know how to extend the Super Mario Bros from the SNES. Wasn’t really sure how to give that same sort of Mario Bros feeling in 3D. Quite a few months were spent around just playing around with different camera views, animations, ways of looking at the map. At some point, the game had a fixed path, almost like an isometric type of look. That didn’t represent that much of a jump from the original 2D Mario.

There was a lot of criticism of the camera originally. At the time, Miyamoto thought the camera was really good – the way it tried to avoid the scenery. It was very well done – it was done by *\[Takumi*\] Kawagoe, who worked on Starfox 2 for the SNES, which got canned. He was a very very good programmer. I think his only job was the camera, so quite a major thing to do. He was there on the entire project – the whole time.

**Your name is on some Nintendo patents.**\
 There was a lot of stuff being patented. I think I had my name on one of them – something like 1080 or something like that, where you draw the background with a cube, rather than have a simple flat background. That was fair enough. SGI had a demo for GL a few years before that, I came up with a way of drawing bitmap information and colour information – so rather than just having a static view, you could rotate the camera around, you could also move objects into the background and they’d put themselves behind the other objects. I think they used it on Zelda *\[Ocarina Of Time\]*. So they patented it!

Half the patents that come out have been techniques that people have been using for years. The software patents just don’t work.

This was about the time that Sega patented using different 3D camera views in games. So, suddenly, halfway through the Super Mario 64 project, one of the people from downstairs came up and said you realise Sega has the patent on being able to switch cameras. Jaws dropped. We investigated whether we could use them and whether anything would happen. But there was also a patent on putting a 3D object on a bitmapped plane from a coin-op about 15-20 years ago.

**\[Asked something about Bowser levels\]**\
 The 2D Bowser levels weren’t really a result of that. I would imagine it mainly came about because you had a boss there, there’s a lot of Mario maps you can take various routes to get to the end, whereas this one you’ve gotta have a fixed route going on. You know where you stand – you know you can’t run off into a corner and find another exit. It might be nice to do that, but you’ve gotta meet that boss!

**How quickly did the maps come together?**\
 The 3D worlds formed quite quickly. Bob-Omb’s Battlefield was the first map developed. It can’t have been more than 6-7 months before it was ready and working, with enemies running around the map.

At that time, NCL hadn’t done a 3D game before, so no-one really knew what they were doing, to be honest. The artists didn’t really know how they should turn their Softimage objects into objects into the game. There was a lot of experimenting, a lot of improvisation going on. It was quite difficult to pinpoint when, where and how much Mario came into being.

**Were you able to play with the N64 controller from the start?**\
 The original Mario was running around with the keyboard, on the Onyx server. We had no controllers for six months, we had a simple serial port on the back of the emulator boards. The entire game was done with these Indy emulator boards, hardware boards basically emulating the N64. These serial ports – we plugged modified Sega controllers in – most of us were using these modified Sega joypads. We had various prototypes – there were lots of them, probably at least 100 prototypes, mostly based around the central stick, how that moved, how well it moved, what shape the thing around it should be – it ended up seven-sided, but we tried many, circles etc.

**Was Mario 64 designed around the controller?**\
 The first time Miyamoto played with the controller, because he’s working most of the time on Mario 64, he would have seen Mario 64 with it. It wasn’t so much that controller dictated Mario 64, it was just that was the game he was working on. Mario was the way of testing it out. Probably more the other way around.

The actual movement of Mario came from the N64 controller, the way you move the central stick. There was a lot of thought about how the camera moved with the yellow buttons – I don’t think Miyamoto even liked them. I remember talking to him a couple of years ago, he said it’d have been better to have two D-pads, it would’ve been a better balance to have the same on the left and the right.

**How much thought went into Mario’s movement?**\
 99% of the game is concerned with that. most of Miyamoto’s time is spent on that, and the movement of the camera. The majority of the other characters and animation are done with Yamada-san and \[inaudible\], these design the movement of the bosses and the levels and so on, whereas Miyamoto just stands in the background, obviously making suggestions. But his main job is to sit down with the programmers and play with controls and camera and shape the way that the way the game \*feels\*. That is fundamental to the entire game.

**Was the camera system a lot of work?**\
 I think this probably went through 1000s of different systems – having it locked, having it moving, having it locked again, player could control it 100%, etc. That was one of the problems – Miyamoto wasn’t sure how to control the camera in 3D as he hadn’t worked with them before, so it went through with various stages, being controlled by the different stages.

**How does the Super Mario Club testing team work?**\
 The way that Super Mario Club is set up its like a hobbyist club, it’s gamers, so its a snapshot of your target audience in-house. So you did get a bit of feedback towards the end – is this too hard, etc. That might have changed now, but back then you didn’t get any ongoing feedback.

**Do you think Mario 64 was too easy in retrospect?**\
 Gamecube games are really tough – that’s mainly because Nintendo have realised that people are more accustomed to 3D now, either that or Nintendo is more used to 3D. Because they’ve been doing 3D for five years now.

![](https://pixelatron.com/blog/media/Super-Mario-64-U-snap0018.jpg "Super Mario 64 (U)  snap0018")

**Was much changed during development?**\
 Apart from the number of polygons use, the characters were first-off – once they were designed, they went straight into the game. There wasn’t really a lot of changing, artists just kept supplying stuff that just got put in the game, conveyor-belt style. That was more or less because of a very, very tight deadline – it had to be out right when the N64 came out.

**Was the N64 delayed to give time for Super Mario 64 to be finished?**\
 N64 wasn’t delayed because of software. Whatever rumours you believe – some games had problems with the final lock check, which might have caused slight delays, but not the big delays that the N64 went through.

They have a very good project management system – the director spends most of his time thinking what needs to be done when, and by who. He’ll go and talk to people and go off and do it, and they’ll supply feedback and say whether schedules need changing. There was a definite someone supposed to be doing something at a certain time. Because NCL have these directors whose main job is to organise stuff, you’re allowed a certain amount of leeway, so you can go to them and request 2 or 3 weeks for what you’re working on. That was one good thing about EAD work – they appreciated that projects were dynamic like that, not saying here’s your schedule stick to it. So, fairly relaxed – but as you’re Nintendo, you’re setting an example to all the other developers, that was quite a bit of pressure.

**Do you remember the reaction to Mario 64 [first being shown at Shoshinkai](http://www.youtube.com/watch?v=Hl6MZqhjt84) *\[1995 Nintendo consumer show\]*?**\
 I can’t remember. There was a lot of attention to the way the camera moved, and Mario walking around wherever he looked. That was what public drew most attention, too – a whole new outlook on games. That was the main buzz. It was something so much graphic-wise, just the moving around.

**Did the positive reaction at Shoshinkai bolster the team?**\
 Being a Japanese company, you get very shielded from any external feedback or anything. I don’t think anyone from the Mario team actually went to Shoshinkai. That’s mainly because Nintendo are very secretive, they don’t like giving out too much info, they don’t like doing it the other way, they don’t like giving inside people access to the outside. Even now, you can’t get internet access inside the building.

They have their own set ways that work. There’s no point changing them.

**\[Asked something about graphics\]**\
 The thing about the N64 was that it wasn’t particularly fast. SGI said that the ‘quality of our pixels are much better than anyone else’s’. Not a lot of people got that – for every pixel it drew, it put a lot of time and effort into it. They were nice pixels. Nicely-textured, nicely-coloured, nicely-lit, nicely anti-aliased. The PlayStation, speed-wise it was much faster, but the pixels were dreadful, there was no texturing, anti-aliasing. Blindingly fast, but the pixels just looked crap. That was SGI aimed for from the outset – the ‘Reality System’ graphics pipeline for Onyx and the Indy stuff – they were trying to compress that all down into the N64, and did a really good job. Quality of pixels over speed.

![](https://pixelatron.com/blog/media/Super-Mario-64-U-snap0033.jpg "Super Mario 64 (U)  snap0033")

**What’s the secret behind that Mario ‘feel’?**\
 I think that is fine-tuning. Many, many, many hours and days spent actually sitting down and playing the game. If you don’t play the game, you don’t pick up on the little nuances, and the effects that you think should be there or shouldn’t be there. EAD spend a lot of time playing the games they make, half the team are playing while the other half are developing. That’s why they end up so good.

**What was the atmosphere like during the final days of coding?**\
 I think there was a lot of panicking going on. But it was still very organised, there was lots of people working very hard. I think it was quite laid back at the very end, not many bugs, gameplay was sorted out on time. One of the programmers had quite a hard time of it – two of them decided not to make games anymore because of Mario 64. Not because they didn’t enjoy it, but because they’d burnt themselves out.

*I asked Giles to comment about some specific ‘best bits’ in Super Mario 64.*

**Castle**\
 I think the idea of having a 3D version of the world map, avoiding a menu system, is a novel way of doing that. I think a lot of time went into designing it, but not much time actually implementing it – just three or four shapes plonked together at the end, really.

**Yoshi**\
 EAD always do this in their games. There’s a connection between every single game that EAD do. The characters from one game are always in another. I think there was a plan to ride Yoshi around – the suggestion came up. With more time, it would have been taken more seriously.

**Water**\
 The guy who did the sound was very meticulous, very interesting in having – rather than just having a soundtrack, having it change depending on where you are. With more time, he would have spent more time on that. To make it as much like a film as possible, sounds follow what you’re doing. That was a big thing – they spent a lot of time working on it. They spent a lot of time on the swimming – I think it’s a lot harder than just running, to get the feeling right, there are actually a few tricks you can do while swimming. They wanted to make it not frustrating, so you don’t want to avoid the water, they wanted to make an advantage to going into the water, fun going into the water.

There’s no actual water texture below – it’s empty. You can actually use a water texture – it looks nice, moves around, but then you think it gets in the way, you can’t see. They tried that. They tried various overlays, but realised it would detract from the fun of the game. That’s the trademark of Nintendo games – they’re not afraid to take huge artistic liberties on stuff they shouldn’t do with the laws of physics. If it plays right and looks ok then Ninty go with it.

The physics in Mario 64 are quite realistic in some ways, but there’s a lot of stuff that went in to give a twist so you could defy the laws of physics for gameplay’s sake. Wave Race and 1080 did the same thing – based on very fundamnetally good physics, but you have bits on top that you plug in so you can do things you can’t actually do. That’s where the excitement comes from.

**The ‘Mario sleeping’ easter egg**\
 Voices in games were new. That was good about N64 – 16-bit digital sound you could play with. That did add a lot to the game. It took a lot of room on the cart, though, very much a balancing act at the time.

**Wobbly level gates**\
 These show the N64 could play with verteses – like the Mario face. We wanted to do something different from 3D on PC. Wave Race did the same, about the same sort of time. Ways of playing with vertices and reflection mapping, this whole buzz going round EAD at the time. I was amazed by the reflection-mapping on the Onyx, only the Onyx can do that, £1,000,000 machine, but when I looked at maths of it at the time, realised that when we got N64 emulators, we can actually do it on that. Very exciting.

**The never-ending staircase**\
 This was simply putting you back at an exact point when you reached a certain spot – it just happens so fast.

**The size of the levels**\
 You’ll notice that many of the objects that far away in the distance disappear to make this possible. The reason that Mario could do such expansive levels without fogging was partly by being quite compact, despite the impression they were big. Turok were very wide, Mario was quite narrow and convoluted. On some of the maps, there were places where you could swap between map shapes – give an impression of a huge map when in fact it was split up into small sub-sections. There are very few places where you can see the entire map.
