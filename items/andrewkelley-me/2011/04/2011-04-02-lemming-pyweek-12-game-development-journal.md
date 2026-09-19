---
title: 'Lemming - PyWeek #12 Game Development Journal'
link: https://andrewkelley.me/post/lemming-pyweek-12.html
source: andrewkelley-me
published: 2011-04-02T12:00:00Z
updated: 2011-04-02T12:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'Lemming - PyWeek #12 Game Development Journal PyWeek #12 challenge starts in 2 days, 16 hours, and 11 minutes. And I have 23 hours of homework to do in order to clear the Week of Py for development. I''ve plotted out THIS week in Google Calendar (the week before PyWeek) - an entry for homework time, break time, and even sleep time. I''m following a strict schedule for optimum time management during PyWeek. The start date is 5pm on Saturday for me, so I have rotated my sleep schedule so that I will go to sleep at 9am on Saturday in order to wake up just as PyWeek starts. I''m getting pumped up. I can''t wait to start! End of Day 1 Status: Tired, Hungry, Have to Pee Idea: Side scrolling platformer. You''re an odd-shaped lemming-like creature leading a stampede of 9 clones. When you die control is instantly transferred to the next one behind you. Your death affects the environment, so a large part of the gameplay will be suiciding yourself in order to get the next guy behind you to safety. Dying should be fun and comedic. Screenshot: Video Source code on GitHub Lines of code so far: 612 Decided to go with a tile-based level format. I got hung up for a while on pyglet''s inverted Y axis but I''m getting used to it now. Having some trouble keeping the frame rate up. I have it up to about 24 but that''s not even with a complete level. I''m hoping I won''t have to mess with OpenGL directly, as I always encounter bugs that take hours upon hours to find when I do. I''m using pyglet''s batch sprite API currently. Next up on the TODO list is to create more interesting tile types and implement the physics for them. Maybe then getting a parallax background going (hard part is finding/creating the art). I think I''ll save animation for later. I''d rather focus on my strengths (programming) and get some good gameplay going, and then if I have time I can make the animation happen. Really wishing I had an artist on board at this point. Well, time for an 8-hour nap and then it''s back to work! pyglet bug I ran into a rather unfortunate pyglet bug regarding animation. This is going to cost me a an hour or two of dev time to use sprite sheets instead of .gif''s :-( In other news, new screenshot: Also, when I tried to upload my character art () to Facebook, I got this: LOL! End of Day 2 Here I am at the end of day 2. According to git log, here''s what I''ve done today: 6:24pm: scrolling 2-layer background 8:23pm: ability to detatch leader and give control to the next guy 10:44pm: suicide bomb control works 11:57pm: +1 dude powerup 12:06am: +infinite dudes powerup 12:21am: land mines 12:35am: spikes 9:20am: totally rewrite display engine and change level format. 9:38am: use pyglet resources rather than the skelington data.py module Lines of code so far: 542. This is interesting because yesterdays LOC count was 612. After a full day''s work and several new features, I ended up with less code. Three cheers for good design! I''m now using Tiled for editing, which is way nicer than my own hacked up piece of crap level editor. I had fun deleting that. Thanks to the display engine rewrite (I simply use glTranslatef instead of manually positioning every sprite), all my FPS worries are gone. I''m well above 60. However I have a new problem: weird line artifacts appearing on sprites. It seems almost as if the bottom row of pixels of every sprite is displaying on the top of the sprite instead. You can see it in the screenshot - there isn''t supposed to be a flickering line on the spikes. Video Next up on the TODO list: use the nifty level editor to create more tiles and a fun and legit Level 1. Then begin to do whatever game upgrades necessary to make Level 1 work. This may involve doing some (shudder) animations. I should begin to think about music and sound effects too. I am considering making the background music myself. I have been known to make some music in FL Studio. I think it would be fun if some parts of the level pulsated to the beat of the music. End of Day 3 Video Lines of code so far: 653 What I''ve done: 7:27pm: fixed the sub-pixel access issue with glTranslatef. thanks richard! 12:13am: created animation assets for the main character 1:05am: implement animation support in the game logic 3:27am: smooth camera movement - follows the character at a damped acceleration curve 6:50am: created level 1 in map editor with some unimplemented game ideas and tiles Here is some new concept art: Problem: I have a mid-term coming up in 7 hours, and I have not been to class or read the book since the last test. Also I worked through the night so I will be in sleepy-and-not-motivated mode all day while studying and test-taking. Bleh. Overview of what my TODO list looks like right now: try to not fail college implement in game logic all the stuff I just pretended would work when I made level 1 make another level with even more new ideas and then implement them. repeat until satisfied. sound effects bg music HUD menu system / title screen death / game over test on windows and mac End of Day 4 Good news and bad news. The bad news is that I got at best 55% on my statistics test yesterday. The good news is that I have all kinds of new stuff done for PyWeek. I think I have my priorities in the right place :-) Video #1 Video #2 3:20am: more advanced collision detection which makes walls work 4:18am: ramps work 4:33am: text boxes work. (that was dead simple, thanks to pyglet!) 5:40am: belly flops work. 6:16am: ladders work 6:52am: decorations work; added house decoration 7:18am: add ladder animation 8:15am: add monster animations 8:51am: added monster who throws you 9:23am: explosions will break breakable stuff 9:39am: you can explode monsters 9:54am: add some tiles to make the level a little nicer 11:33am: implement buttons and bridges. level 1 works completely 12:21am: support reverse direction for monsters I talked to my friend Tyler and he wants to make the music for the game, if only he can whip his computing situation into shape in time. I''m crossing my fingers. I''ve formulated a plan for the rest of the days I have left: Day 5: sound effects HUD menu system and title screen game over Day 6: implement as many gameplay ideas as possible create as many levels as possible exploring those concepts bg music if it doesn''t work out with tyler Day 7: Dinner with family Party with friends Test on Mac and Windows and smooth out any glitches/bugs/problems with the experience. bug fixing only. no new features End of Day 5 I''m really fighting sleep-deprivation right now. Going to crash like a rock as soon as I''m done with this diary entry. Like a rock? Is this how a person crashes? I can''t remember. Anyways: 1:42pm (yes I cheated and worked more after yesterday''s diary entry before sleeping): maps support background music and the game engine will stream, play, and loop the background music. I''m really liking how simple this stuff is in pyglet. 2:11pm: test on Windows with py2exe. It worked after I added another entry to the resource search path. It''s not safe to rely on the .py file location - with py2exe it moves. I better warn people who used the skellington. 12:46am (after a nap and Poker Night at Tyler''s across town): record, create, and find sound effects 2:04am: create a short background music track 3:58am: support sound effects in game 4:13am: if you don''t belly flop, you don''t cover as many spikes up 4:35am: a HUD which gives you hints as to what buttons you can press 7:07am: slim down tiledtmxloader and make it use pyglet.resource 8:57am: huge code refactor enabling me to switch scenes from gameplay to title screen or restart gameplay when you die. I took my own advice from Hugoagogo''s stack overflow help post and it worked pretty nicely. Hopefully there isn''t a memory leak. 9:47am: title screen works and has sound 9:53am: FPS display is a command line argument, off by default 10:10am: on game over, starts level over 10:28am: if you beat the level you go on to the next one 10:41am: automatically save progress so player can continue from where they left off. 11:04am: credits scene when you win Video Music I''m waiting on confirmation to make sure I don''t have to swap out my explosion graphic. That would suck. TODO: Day 6: implement as many gameplay ideas as possible create as many levels as possible exploring those concepts fix some bugs/nice-to-haves Day 7 (half-day): test on windows and mac complete the README and make sure all files are in place on pyweek.org download the final submission and play through it on each operating system Bugs / nice to haves that I probably don''t have time for: credits song physics bugs the "beat level" jingle is obnoxious. re-do it don''t call game over until belly flop is done. he might pick up a +1 animate the title screen End of Day 6 Lines of code: 1939 I''m not going to spoil the surprise by posting any more commit log, videos, pictures, or sound. You''ll just have to play it to find out! Things are progressing on track. I already have something playable and working right now, all that needs doing is some more levels and bug fixes. Good news - Tyler managed to get his computer working and make an awesome track. I really need sleep but I''m going to be out and about for another 10 hours. I can do this. TODO: Day 7 (half-day): create at least 3 more levels critical bug: if you die several times, memory keeps expanding and sound stops working important bug: belly flop stuck in wall, and don''t call it game over until all belly flops are turned to dead bodies test on windows and mac I''d really like to, but probably not going to get to these, unfortunately: add a gory explosion of bloody organs when you die on spikes bad physics glitch when you jump against a vertical wall physics glitch with belly flops and spikes death shouldn''t interrupt the music flow the sound that plays when you win is obnoxious and horrible End of Day 7 Well here I am. 32 minutes to go and I am submitted. That was a gruelling but satisfying experience. I tried to make as many levels as possible and ironically ended up with 9. 2021 lines of python. Tyler''s having fun with the level editor - I think we may continue to work on this past the competition. Also I''m going to contribute to Tiled. Having relied on it heavily, I know how to make it better. I also discovered a few pyglet bugs. I''ll submit patches to fix them. Postmortem A lot of people are getting stuck on Level 2. And 3, and 4, and 5, and... I realize now that I made the levels way too hard. I should have very slowly increased the difficulty. Each new level shows off more of the game''s gameplay elements, so it would be a shame for you to give up and judge without at least checking out all the levels. If you edit a file called "save_game" (this will be created if you beat level 1) in the directory you run the game from, it has the number of the level you start on when you press Continue from the title screen. You can edit this to skip around and see all 9 levels. Alternately, there is a YouTube video of me speedrunning through the entire game (only 7 minutes long):'
content: extracted
html: 2011-04-02-lemming-pyweek-12-game-development-journal.html
preview:
  file: 2011-04-02-lemming-pyweek-12-game-development-journal.preview-9cd849802dfa.webp
  width: 256
  height: 149
  color: '#0e0b08'
images:
- source: http://s3.amazonaws.com/superjoe/temp/pyweek-screenshot-1.png
  original:
    file: 2011-04-02-lemming-pyweek-12-game-development-journal.image-35cadd422e5a.png
    width: 1160
    height: 676
  color: '#000000'
- source: http://s3.amazonaws.com/superjoe/temp/pyweek-screenshot-2.png
  original:
    file: 2011-04-02-lemming-pyweek-12-game-development-journal.image-d9d72bdf092b.png
    width: 860
    height: 508
  color: '#00cbfe'
- source: http://s3.amazonaws.com/superjoe/temp/pyweek-char-art.png
  original:
    file: 2011-04-02-lemming-pyweek-12-game-development-journal.image-361cba275208.png
    width: 70
    height: 64
  variants:
  - file: 2011-04-02-lemming-pyweek-12-game-development-journal.image-80317accb8e1.webp
    width: 70
    height: 64
  color: '#fefefe'
- source: http://s3.amazonaws.com/superjoe/temp/pyweek-character-art-facebook.png
  original:
    file: 2011-04-02-lemming-pyweek-12-game-development-journal.image-fa6cfc93dda9.png
    width: 462
    height: 126
  color: '#f5f5f5'
- source: http://s3.amazonaws.com/superjoe/temp/pyweek-day-2-screenshot.png
  original:
    file: 2011-04-02-lemming-pyweek-12-game-development-journal.image-37a5285e65bb.png
    width: 1366
    height: 768
  color: '#818181'
- source: http://s3.amazonaws.com/superjoe/temp/pyweek-screenshot-day-3.png
  original:
    file: 2011-04-02-lemming-pyweek-12-game-development-journal.image-e8f852f19827.png
    width: 861
    height: 508
  color: '#00cbfd'
- source: http://s3.amazonaws.com/superjoe/temp/winnar.png
  original:
    file: 2011-04-02-lemming-pyweek-12-game-development-journal.image-1bdb4e80902f.png
    width: 80
    height: 48
  color: '#804000'
- source: http://s3.amazonaws.com/superjoe/temp/sadface.png
  original:
    file: 2011-04-02-lemming-pyweek-12-game-development-journal.image-eda593ebab3e.png
    width: 64
    height: 64
  color: '#008000'
- source: http://s3.amazonaws.com/superjoe/temp/pyweek-day-4-screenshot.png
  original:
    file: 2011-04-02-lemming-pyweek-12-game-development-journal.image-87f5c023ec75.png
    width: 861
    height: 508
  color: '#00cbfd'
- source: http://s3.amazonaws.com/superjoe/temp/pyweek-screenshot-day-5.png
  original:
    file: 2011-04-02-lemming-pyweek-12-game-development-journal.image-97c25fa6fa90.png
    width: 861
    height: 508
  color: '#00cafd'
---

[PyWeek #12](http://pyweek.org/12/) challenge starts in 2 days, 16 hours, and 11 minutes.

And I have 23 hours of homework to do in order to clear the Week of Py for development.

I've plotted out THIS week in Google Calendar (the week before PyWeek) - an entry for homework time, break time, and even sleep time. I'm following a strict schedule for optimum time management during PyWeek. The start date is 5pm on Saturday for me, so I have rotated my sleep schedule so that I will go to sleep at 9am on Saturday in order to wake up just as PyWeek starts.

I'm getting pumped up. I can't wait to start!

## [End of Day 1](https://andrewkelley.me/post/lemming-pyweek-12.html#day-1)

Status: Tired, Hungry, Have to Pee

Idea: Side scrolling platformer. You're an odd-shaped lemming-like creature leading a stampede of 9 clones. When you die control is instantly transferred to the next one behind you. Your death affects the environment, so a large part of the gameplay will be suiciding yourself in order to get the next guy behind you to safety. Dying should be fun and comedic.

Screenshot:

![](http://s3.amazonaws.com/superjoe/temp/pyweek-screenshot-1.png)

[Video](http://s3.amazonaws.com/superjoe/temp/pyweek-day-1.mpg)

[Source code on GitHub](https://github.com/andrewrk/lemming)

Lines of code so far: 612

Decided to go with a tile-based level format.

I got hung up for a while on pyglet's inverted Y axis but I'm getting used to it now.

Having some trouble keeping the frame rate up. I have it up to about 24 but that's not even with a complete level. I'm hoping I won't have to mess with OpenGL directly, as I always encounter bugs that take hours upon hours to find when I do. I'm using pyglet's batch sprite API currently.

Next up on the TODO list is to create more interesting tile types and implement the physics for them. Maybe then getting a parallax background going (hard part is finding/creating the art). I think I'll save animation for later. I'd rather focus on my strengths (programming) and get some good gameplay going, and then if I have time I can make the animation happen. Really wishing I had an artist on board at this point.

Well, time for an 8-hour nap and then it's back to work!

## [pyglet bug](https://andrewkelley.me/post/lemming-pyweek-12.html#pyglet-bug)

I ran into a [rather unfortunate pyglet bug](http://code.google.com/p/pyglet/issues/detail?id=411) regarding animation.

This is going to cost me a an hour or two of dev time to use sprite sheets instead of .gif's :-(

In other news, new screenshot:

![](http://s3.amazonaws.com/superjoe/temp/pyweek-screenshot-2.png)

Also, when I tried to upload my character art (![](http://s3.amazonaws.com/superjoe/temp/pyweek-char-art.png)) to Facebook, I got this:

![](http://s3.amazonaws.com/superjoe/temp/pyweek-character-art-facebook.png)

LOL!

## [End of Day 2](https://andrewkelley.me/post/lemming-pyweek-12.html#day-2)

Here I am at the end of day 2. According to git log, here's what I've done today:

6:24pm: scrolling 2-layer background 8:23pm: ability to detatch leader and give control to the next guy 10:44pm: suicide bomb control works 11:57pm: +1 dude powerup 12:06am: +infinite dudes powerup 12:21am: land mines 12:35am: spikes 9:20am: totally rewrite display engine and change level format. 9:38am: use pyglet resources rather than the skelington data.py module

Lines of code so far: 542. This is interesting because yesterdays LOC count was 612. After a full day's work and several new features, I ended up with less code. Three cheers for good design!

I'm now using [Tiled](http://www.mapeditor.org/) for editing, which is way nicer than my own hacked up piece of crap level editor. I had fun deleting that.

Thanks to the display engine rewrite (I simply use glTranslatef instead of manually positioning every sprite), all my FPS worries are gone. I'm well above 60. However I have a new problem: weird line artifacts appearing on sprites. It seems almost as if the bottom row of pixels of every sprite is displaying on the top of the sprite instead. You can see it in the screenshot - there isn't supposed to be a flickering line on the spikes.

[Video](http://s3.amazonaws.com/superjoe/temp/pyweek-video-day-2.mpg)

![](http://s3.amazonaws.com/superjoe/temp/pyweek-day-2-screenshot.png)

Next up on the TODO list: use the nifty level editor to create more tiles and a fun and legit Level 1. Then begin to do whatever game upgrades necessary to make Level 1 work. This may involve doing some (shudder) animations.

I should begin to think about music and sound effects too. I am considering making the background music myself. I have been known to make [some music](http://theburningawesome.bandcamp.com/) in [FL Studio](http://www.image-line.com/documents/flstudio.html). I think it would be fun if some parts of the level pulsated to the beat of the music.

## [End of Day 3](https://andrewkelley.me/post/lemming-pyweek-12.html#day-3)

[Video](http://s3.amazonaws.com/superjoe/temp/pyweek-video-day-3.mpg)

![](http://s3.amazonaws.com/superjoe/temp/pyweek-screenshot-day-3.png)

Lines of code so far: 653

What I've done:

- 7:27pm: fixed the sub-pixel access issue with glTranslatef. thanks richard!
- 12:13am: created animation assets for the main character
- 1:05am: implement animation support in the game logic
- 3:27am: smooth camera movement - follows the character at a damped acceleration curve
- 6:50am: created level 1 in map editor with some unimplemented game ideas and tiles

Here is some new concept art:

![](http://s3.amazonaws.com/superjoe/temp/winnar.png) ![](http://s3.amazonaws.com/superjoe/temp/sadface.png)

Problem: I have a mid-term coming up in 7 hours, and I have not been to class or read the book since the last test. Also I worked through the night so I will be in sleepy-and-not-motivated mode all day while studying and test-taking. Bleh.

Overview of what my TODO list looks like right now:

- try to not fail college
- implement in game logic all the stuff I just pretended would work when I made level 1
- make another level with even more new ideas and then implement them. repeat until satisfied.
- sound effects
- bg music
- HUD
- menu system / title screen
- death / game over
- test on windows and mac

## [End of Day 4](https://andrewkelley.me/post/lemming-pyweek-12.html#day-4)

Good news and bad news. The bad news is that I got at best 55% on my statistics test yesterday. The good news is that I have all kinds of new stuff done for PyWeek. I think I have my priorities in the right place :-)

![](http://s3.amazonaws.com/superjoe/temp/pyweek-day-4-screenshot.png)

[Video #1](http://s3.amazonaws.com/superjoe/temp/pyweek-day-4.mpg)

[Video #2](http://s3.amazonaws.com/superjoe/temp/pyweek-day-4-#2.mpg)

- 3:20am: more advanced collision detection which makes walls work
- 4:18am: ramps work
- 4:33am: text boxes work. (that was dead simple, thanks to pyglet!)
- 5:40am: belly flops work.
- 6:16am: ladders work
- 6:52am: decorations work; added house decoration
- 7:18am: add ladder animation
- 8:15am: add monster animations
- 8:51am: added monster who throws you
- 9:23am: explosions will break breakable stuff
- 9:39am: you can explode monsters
- 9:54am: add some tiles to make the level a little nicer
- 11:33am: implement buttons and bridges. level 1 works completely
- 12:21am: support reverse direction for monsters

I talked to my friend [Tyler](http://www.soundclick.com/bands/default.cfm?bandID=501445) and he wants to make the music for the game, if only he can whip his computing situation into shape in time. I'm crossing my fingers.

I've formulated a plan for the rest of the days I have left:

Day 5:

- sound effects
- HUD
- menu system and title screen
- game over

Day 6:

- implement as many gameplay ideas as possible
- create as many levels as possible exploring those concepts
- bg music if it doesn't work out with tyler

Day 7:

- Dinner with family
- Party with friends
- Test on Mac and Windows and smooth out any glitches/bugs/problems with the experience.
- bug fixing only. no new features

## [End of Day 5](https://andrewkelley.me/post/lemming-pyweek-12.html#day-5)

I'm really fighting sleep-deprivation right now. Going to crash like a rock as soon as I'm done with this diary entry. Like a rock? Is this how a person crashes? I can't remember. Anyways:

- 1:42pm (yes I cheated and worked more after yesterday's diary entry before sleeping): maps support background music and the game engine will stream, play, and loop the background music. I'm really liking how simple this stuff is in pyglet.
- 2:11pm: test on Windows with py2exe. It worked after I added another entry to the resource search path. It's not safe to rely on the .py file location - with py2exe it moves. I better warn people who used the skellington.
- 12:46am (after a nap and Poker Night at Tyler's across town): record, create, and find sound effects
- 2:04am: create a short background music track
- 3:58am: support sound effects in game
- 4:13am: if you don't belly flop, you don't cover as many spikes up
- 4:35am: a HUD which gives you hints as to what buttons you can press
- 7:07am: slim down tiledtmxloader and make it use pyglet.resource
- 8:57am: huge code refactor enabling me to switch scenes from gameplay to title screen or restart gameplay when you die. I took my own advice from [Hugoagogo's stack overflow help post](http://stackoverflow.com/questions/5581447/switching-scenes-with-pyglet) and it worked pretty nicely. Hopefully there isn't a memory leak.
- 9:47am: title screen works and has sound
- 9:53am: FPS display is a command line argument, off by default
- 10:10am: on game over, starts level over
- 10:28am: if you beat the level you go on to the next one
- 10:41am: automatically save progress so player can continue from where they left off.
- 11:04am: credits scene when you win

[Video](http://s3.amazonaws.com/superjoe/temp/pyweek-day-5.mpg)

![](http://s3.amazonaws.com/superjoe/temp/pyweek-screenshot-day-5.png)

[Music](http://s3.amazonaws.com/superjoe/temp/silly.mp3)

I'm [waiting on confirmation](http://www.pyweek.org/d/3787/#comment-8584) to make sure I don't have to swap out my explosion graphic. That would suck.

TODO:

Day 6:

- implement as many gameplay ideas as possible
- create as many levels as possible exploring those concepts
- fix some bugs/nice-to-haves

Day 7 (half-day):

- test on windows and mac
- complete the README and make sure all files are in place on pyweek.org
- download the final submission and play through it on each operating system

Bugs / nice to haves that I probably don't have time for:

- credits song
- physics bugs
- the "beat level" jingle is obnoxious. re-do it
- don't call game over until belly flop is done. he might pick up a +1
- animate the title screen

## [End of Day 6](https://andrewkelley.me/post/lemming-pyweek-12.html#day-6)

Lines of code: 1939

I'm not going to spoil the surprise by posting any more commit log, videos, pictures, or sound. You'll just have to play it to find out!

Things are progressing on track. I already have something playable and working right now, all that needs doing is some more levels and bug fixes.

Good news - Tyler managed to get his computer working and make an awesome track.

I really need sleep but I'm going to be out and about for another 10 hours. I can do this.

TODO:

Day 7 (half-day):

- create at least 3 more levels
- critical bug: if you die several times, memory keeps expanding and sound stops working
- important bug: belly flop stuck in wall, and don't call it game over until all belly flops are turned to dead bodies
- test on windows and mac

I'd really like to, but probably not going to get to these, unfortunately:

- add a gory explosion of bloody organs when you die on spikes
- bad physics glitch when you jump against a vertical wall
- physics glitch with belly flops and spikes
- death shouldn't interrupt the music flow
- the sound that plays when you win is obnoxious and horrible

## [End of Day 7](https://andrewkelley.me/post/lemming-pyweek-12.html#day-7)

Well here I am. 32 minutes to go and I am submitted. That was a gruelling but satisfying experience.

I tried to make as many levels as possible and ironically ended up with 9.

2021 lines of python.

Tyler's having fun with the level editor - I think we may continue to work on this past the competition.

Also I'm going to contribute to Tiled. Having relied on it heavily, I know how to make it better.

I also discovered a few pyglet bugs. I'll submit patches to fix them.

## [Postmortem](https://andrewkelley.me/post/lemming-pyweek-12.html#postmortem)

A lot of people are getting stuck on Level 2.

And 3, and 4, and 5, and...

I realize now that I made the levels way too hard. I should have very slowly increased the difficulty.

**Each new level shows off more** of the game's gameplay elements, so it would be a shame for you to give up and judge without at least checking out all the levels.

If you edit a file called "save\_game" (this will be created if you beat level 1) in the directory you run the game from, it has the number of the level you start on when you press Continue from the title screen. You can edit this to skip around and see all 9 levels.

Alternately, there is a YouTube video of me speedrunning through the entire game (only 7 minutes long):

Thanks for reading my blog post.
