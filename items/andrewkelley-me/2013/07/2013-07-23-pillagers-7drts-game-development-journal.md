---
title: Pillagers! 7dRTS Game Development Journal
link: https://andrewkelley.me/post/pillagers-7drts-game-dev-journal.html
source: andrewkelley-me
published: 2013-07-23T10:09:53Z
updated: 2013-07-23T10:09:53Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'Pillagers! 7dRTS Game Development Journal I have decided to participate in the 7-Day Real Time Strategy Challenge, July 2013 edition. I am working with Michael Weber who will create sound effects, music, and be in charge of the "atmosphere" of the game. Day 1 Cross-posted on ludumdare.com. I am using chem, a canvas-based game engine I made for rapid development occasions such as this. It has been working out quite well and I think it has been playing a large role in my productivity today. The game is codenamed "pillagers". The idea is to mix space physics with real time strategy and see what comes out. Instead of creating a carefully planned out base, you will be campaigning through levels, pillaging for resources. That''s the idea, anyway. We''ll see how it pans out. Here''s a list of the things that work right now: Selecting squads and telling them to move around. Ships shoot enemy ships, destroying them when their health reaches 0. Scrolling around the map. Auto generated parallax background with stars and a planet. The code is open source, hosted on GitHub. It''s 941 lines of JavaScript: 46 src/bullet.js 33 src/explosion.js 432 src/main.js 19 src/militia_ship.js 241 src/ship_ai.js 165 src/ship.js 5 src/uuid.js 941 total Screenshot of a squad of ships under attack: Short video of me playing around with some elements of the game It was a fun challenge to write the AI to get the ships to stop at the intended destinations. As is it''s not optimal, but I think that''s okay. Maybe later classes of ships will have better AI. You can actually play this game right now. Since it''s web based, you don''t even have to download anything. Here''s what the TODO list looks like currently: Edit the main militia ship to have short range. Auto targeting enemies should only work when close enough. Ability to right click an enemy ship to tell squad to target it. Add enemy turrets and ships to level 1. Add enemy flag which grants victory when destroyed. Add 2nd class of ship which you get some of at beginning of level 1. Instead of giving the player ships at the beginning, give them cash and buildings which they can use to create the ships they want. The user will thus be able to choose how many of class 1 and class 2 ships they want. Put instruction label text in level 1 to explain the controls. Start planning level 2. Well, I''m off to bed to get some rest. Looking forward to Day 2! Day 2 Cross-posted on ludumdare.com. opengameart.org has been very good to me. I think this will be my new go-to game dev art supply. So far I''ve found everything I''ve wanted on that site, except for a meteor sprite. Maybe I''ll make one and contribute back. Michael made a lazer sound and a ship thrusting sound, and I added the sound effects to the game. In addition to that, I made a ton of progress today: 3pm - draw flags only if selected 5pm - add another class of ship 8pm - found out I was using t * a ^ 2 + t * v for acceleration instead of a * t ^ 2 + v * t. I fixed all the math and made ships arriving at gather points nice and smooth. 9pm - added team colors overlayed on ships 10pm - gave the Militia ship an electric melee attack 11pm - gave Militia ships smart attacking AI 12am - made Ranger ships pursue targets 2am - added TurretShip and FlagShip 3am - added support for ships with backwards thrusters 3am - added move & engage command 3am - improved ship selection capabilities 5am - added title screen, credits screen, and game over screen. I made it possible to win and lose the game. Here''s the title screen: Here''s a screenshot of a battle underway: Battles can get hectic and chaotic, but it is fun to watch it play out. Here''s a video of me demoing a battle: Again, you can playtest this game in your browser right now using the same url as Day 1. Feel free to give me feedback or advice. I came up with some ideas on what bigger picture gameplay will look like which I am pretty pleased with. There will be a bunch of different classes of ships, for example: Militia Basic cheap unit. Has short range. Agile. Actually doesn''t shoot lazers, shoots a short range but powerful lightning attack out the front. It must resort to charging other ships to attack them. Ranger Short range weak lazers. Less agile. Less defense. In a level, a Militia should be able to overtake a Ranger and kill it. Artillery Slower, shoots big lazers. Slow moving, weak defense. FlagShip Does not attack. Has high defense. Moves very slowly. Turret Stationary lazer shooter. Medi Heals other ships. There will be some obvious deficiencies with the ships targeting systems and AI. These can be helped with upgrades, such as: Targeting Target the first enemy found (default) Target the closest enemy. Communicate with others to divide up the targets evenly. Ranger Upgrade range. Update bullet count to 3. Evasive maneuvers 1. Evasive maneuvers 2. Smarter aiming. Backwards thrusters. Militia Evade lazers. Backwards thrusters. Smarter aiming. You will start with only a flagship, and as you progress throughout the campaign, you will start to build up a convoy that gets ever bigger and more powerful. You''ll need it to be bigger and more powerful to get through later levels, in fact. Here''s what''s next on the TODO list: Build simpler level 1 where you only have to navigate your flag ship around meteors. Give the player some cash and let them choose to build Ranger ships or Militia ships using the Flagship. Create level 2 where you have to destroy the enemy flagship and then fly your flagship through the created portal. I''m at a pretty good checkpoint right now. I''m calling it a night. Day 3 Cross-posted on ludumdare.com. Today Michael gave me the first music track he composed for the game and I am impressed. I am excited to have professional sounding music for a change. That being said, the first thing I did after inserting the music into the game was program a mute button so that I could keep listening to techno while I coded. Michael also delivered an electric explosion sound effect that works perfectly. I mentioned yesterday that I could not find a meteor graphic to use on opengameart.org. I actually did end up finding art that works really well by searching for "rock". You can see the three rock types in the current spritesheet for the game which is autogenerated by chem: The first thing I did today was create a electrical disintegration animation, and I''m pretty pleased with the result: I wonder if this is considered good enough to submit to opengameart.org. After I finished that animation, I worked hard and had a productive day: 3pm - Finished electrical disintegration animation and added sound effect to game. 3pm - Added in Michael''s background music and made it so you can toggle it with M key. 5pm - Added meteors with collision detection. 5pm - Fixed "ships rotating for no reason" bug. 6pm - Inserted a new Level 1 - meteor field that you have to navigate through. 7pm - Created a spastic portal graphic and added it to the level. 7pm - Fixed navigation bug for ships equipped with backwards thrusters. 9pm - Fixed scrolling when manual overriding. 11pm - Added a UI pane when ships are selected. 12am - Added a mini-map. 1am - Made it so you can send ships into a portal. 3am - Made it so selected portal shows what''s inside it. 4am - Added ability to send ships out of a portal. 5am - Added announcement support. 6am - Made your convoy show up on the level complete screen. 6am - Added stats to the level complete screen. 8am - Finished level complete screen so that you can get to the next level. The source tree has grown quite a bit since last time I checked, up to 2,704 lines: 49 src/bullet.js 77 src/credits_screen.js 43 src/flag_ship.js 30 src/fx.js 73 src/game.js 34 src/game_over_screen.js 178 src/level_complete_screen.js 20 src/main.js 81 src/meteor.js 58 src/militia_ship.js 91 src/physics_object.js 82 src/portal.js 60 src/ranger_ship.js 37 src/sfx.js 449 src/ship_ai.js 177 src/ship.js 7 src/ship_types.js 57 src/squad.js 933 src/state.js 23 src/team.js 105 src/title_screen.js 35 src/turret_ship.js 5 src/uuid.js 2704 total I actually feel pretty good about the code organization right now. I have only had to pause progress and refactor 2-3 times and each time it was mostly painless. Enough of the technical stuff. Let''s see some screenshots. Here''s one of the meteor field level you start out in: Here''s what it looks like when you finish the level: And here''s me giving a walkthrough of the progress made today: My TODO list is getting a bit unruly. I''ve divided it into "next steps" and "nice-to-have"s: Next steps: Add text instructions in Level 1 to explain the controls and how to beat the level. Make it so that you start out the next level with the same fleet that you exited with. Show your cash in the UI Allow you to create ships that you unlock by spending cash. Insert a level between 1 and 2 with some attackers. You''ll have to build some Ranger ships to defend your Flagship. Nice-to-haves: Figure out why the game slows to a crawl when many ships are added and then deleted. Experiment with speed cap on ships. (I''m reluctant to do this one - I like the idea of high-velocity dogfighting.) If Ranger is in range, don''t accelerate toward target. Fix the thruster sound glitchiness Each command should draw something on the screen to indicate that something is commanded. (Some commands do this already.) When forming a squad, don''t assume all ships have the same radius. And now I must rest. I am exhausted. Day 4 Cross-posted on ludumdare.com. It has been wonderful relying only upon circles in this physics engine. The math is simple and beautiful, and it''s easy to write fast code. Who needs polygons anyway? Today was a good day. Pillagers is now actually a game. What I got done: 5pm - Added more of Michael''s sound effects into the game. 5pm - Tweaked the physics. 5pm - Added some cheats to help speed up testing. 6pm - Added text in Level 1 to explain controls. 7pm - The game shows how much cash you have. 10pm - You keep your same fleet when progressing to the next level. 12am - Added a new Level 2. 12am - Made unlocking ships work. 12am - Made it so you get cash from killing enemy ships. 12am - Tweak the money system. 1am - Made it so you can scroll and give orders while paused. 1am - Fixed some game crashes and bugs. 2am - Modified attacking AI to stay within certain speed limits. Makes the game less chaotic. 3am - Added ability to skip tutorial levels. 4am - Better squad formation. 4am - Updated the Move & Engage command to be more effective. 6am - Added Civilian ships and plan out Level 4. 7am - Add Artillery ship and finish implementing Level 4. 8am - Fix the crash which happened at the end of the demo below. Screenshot: Screencast demo: Bed. Day 5 Cross-posted on ludumdare.com. I spent a good chunk of the day trying to solve a performance problem. After approximately 100,000 bullets were fired, the framerate would drop to an excruciating 16 FPS. I was able to figure it out by using Google Chrome''s heap profiling tool. Here''s what the heap snapshot looked like: I found out that there was a memory leak in the game engine - every time a sprite was created it called setInterval but it did not call clearInterval when the sprite was deleted. Ever since I fixed the issue, FPS has stayed at a nice and smooth 60, no matter how many objects are created and destroyed. Here are some other things I got done today: 9pm - Miscellanous small enhancements. 11pm - Fixed the performance issue. 12am - Added the new thruster sound that Michael gave me. 3am - Added Sandbox Mode. 4am - Added more features to Sandbox Mode. 6am - Added Ctrl+A to select all your ships. Also made WASD and J work the same as arrow keys and space. 7am - Added dogfighting mode. 8am - Added graphics so that ships display their targets when selected. 8am - Added 2 more dogfighting levels. Enhanced manual piloting of Militia ship so that you can use your melee attack. Here''s a screenshot of Sandbox mode: Here''s me playing through Dogfighting Mode and messing around in Sandbox Mode a bit: I have only 2 days left to work. The last day will be primarily reserved for gameplay tweaking, bugfixes, touchups, and testing. That leaves only 1 more day to make actual progress. These are my goals for tomorrow: Add a rotating turret to the flagship. Add a civilian to Level 1 and use that to explain the Move command vs the Engage command. Add a dogfighting level where there is an ongoing battle and you have to rack up a certain number of kills to win. Add more campaign levels. I reserve the right to ramp up the difficulty in the later levels! Pan sound effects and volume depending on scroll location Add some more ship classes and implement upgrades. Thanks to the folks in the #7dRTS IRC channel for helping me playtest today. Specifically Zapa and Orava. Day 6 Cross-posted on ludumdare.com. Today I worked on making the level format and core engine more robust. I added a bunch more dogfighting levels. There are 11 now, and they get pretty crazy at the end. The cool thing about them is that the core game engine does not even know that you are in "Dogfighting Mode". There''s no dogfightingMode variable that tells the engine that''s what you''re doing. Instead, the level format supports trigger conditions, events, and groups, and the logic of dogfighting is done in the level JSON file. Michael gave me the battle music today as well. It sounds amazing. I made it so that the game automatically detects when you are entering or exiting a battle and transitions the music appropriately. I''m pleased with the effect. Here''s a list of what I got done today: 12am - Added battle music. The game automatically detects when a battle is happening and fades in the battle music, then fades back to normal music once the battle is over. 1am - Added more dogfighting levels. 3am - Worked on making dogfighting levels more fun. You have to try again if you die rather than respawning. 4am - Made the Militia ship show the area of attack when you are manually piloting it. 6am - Sandbox Mode: Ability to load and save. I was able to use this to build some dogfighting levels. 7am - Added more dogfighting levels. I did not accomplish most of what I set out to today. Regardless, I am really happy with Dogfighting Mode right now. It''s almost certainly more fun than Campaign Mode currently. Here''s a video of me playing through it: This version of the game is going to be pretty close to the final one that I submit for 7dRTS. I will probably only spend about 6-8 more hours on this debugging on Firefox and bugfixing. I will not be supporting Internet Explorer. Day 7 Today was short. I added more sound effects that Michael provided, and lowered the difficulty on several levels. The game is in a stable state and I don''t want to screw it up before submitting. I experimented with adding aiming hints when manually piloting a ship, but I found that it actually made the game more difficult because it tricks the player into aiming directly at the enemy ships instead of compensating for relative velocity. I tried to make my girlfriend play the game, but she said it was too hard. It probably is too hard. I like hard games. Either way, I''m submitting. View my entry on ludumdare.com. Play the game in your web browser.'
content: extracted
html: 2013-07-23-pillagers-7drts-game-development-journal.html
preview:
  file: 2013-07-23-pillagers-7drts-game-development-journal.preview-9f2ba0c7dcc4.webp
  width: 256
  height: 186
  color: '#070504'
images:
- source: https://s3.amazonaws.com/superjoe/blog-files/pillagers-7drts-game-dev-journal/screenshot-day-1.png
  original:
    file: 2013-07-23-pillagers-7drts-game-development-journal.image-7413a38ec5bd.png
    width: 790
    height: 573
  variants:
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-4f483649ccf6.webp
    width: 320
    height: 232
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-9c7669904f17.webp
    width: 640
    height: 464
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-c74ace3fb8c2.webp
    width: 790
    height: 573
  color: '#000000'
- source: https://s3.amazonaws.com/superjoe/blog-files/pillagers-7drts-game-dev-journal/day-2-titlescreen.png
  original:
    file: 2013-07-23-pillagers-7drts-game-development-journal.image-3ed6dc243110.png
    width: 1064
    height: 597
  variants:
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-7fc85d27d478.webp
    width: 320
    height: 180
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-d4b8db5f1cec.webp
    width: 640
    height: 359
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-0c668f0ba388.webp
    width: 960
    height: 539
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-4072d66a62f5.webp
    width: 1064
    height: 597
  color: '#000000'
- source: https://s3.amazonaws.com/superjoe/blog-files/pillagers-7drts-game-dev-journal/day-2-screenshot.png
  original:
    file: 2013-07-23-pillagers-7drts-game-development-journal.image-ced40a71a004.png
    width: 1063
    height: 598
  variants:
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-dfeb1e2695ed.webp
    width: 320
    height: 180
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-4d3c7ab3ca0e.webp
    width: 640
    height: 360
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-805160b8451a.webp
    width: 960
    height: 540
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-8d50a13a02c8.webp
    width: 1063
    height: 598
  color: '#000000'
- source: https://s3.amazonaws.com/superjoe/blog-files/pillagers-7drts-game-dev-journal/day-3-spritesheet.png
  original:
    file: 2013-07-23-pillagers-7drts-game-development-journal.image-faedaa1375e5.png
    width: 3431
    height: 200
  variants:
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-794ad9d147d5.webp
    width: 320
    height: 19
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-0a0b7d8e1d00.webp
    width: 640
    height: 37
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-dbbda83781c0.webp
    width: 960
    height: 56
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-fab21dbcbe2f.webp
    width: 1280
    height: 75
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-ebc02a480ffc.webp
    width: 1600
    height: 93
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-572905ff4406.webp
    width: 3431
    height: 200
  color: '#2a231c'
- source: https://s3.amazonaws.com/superjoe/blog-files/pillagers-7drts-game-dev-journal/day-3-disintegrate.gif
  original:
    file: 2013-07-23-pillagers-7drts-game-development-journal.image-d72a3c654581.gif
    width: 63
    height: 83
  color: '#f7f7d9'
- source: https://s3.amazonaws.com/superjoe/blog-files/pillagers-7drts-game-dev-journal/day-3-screenshot.png
  original:
    file: 2013-07-23-pillagers-7drts-game-development-journal.image-aad4718cf153.png
    width: 1065
    height: 599
  variants:
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-88fde9c29b82.webp
    width: 320
    height: 180
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-13cc19aa60a1.webp
    width: 640
    height: 360
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-55be76139a75.webp
    width: 960
    height: 540
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-82dcf8a508c6.webp
    width: 1065
    height: 599
  color: '#000000'
- source: https://s3.amazonaws.com/superjoe/blog-files/pillagers-7drts-game-dev-journal/day-3-level-complete.png
  original:
    file: 2013-07-23-pillagers-7drts-game-development-journal.image-fc1bd334a478.png
    width: 1065
    height: 598
  variants:
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-5e50c46b1b85.webp
    width: 320
    height: 180
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-5028f035095c.webp
    width: 640
    height: 359
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-f4a47e455fef.webp
    width: 960
    height: 539
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-a739ac4aebdc.webp
    width: 1065
    height: 598
  color: '#000000'
- source: https://s3.amazonaws.com/superjoe/blog-files/pillagers-7drts-game-dev-journal/day-4-screenshot.png
  original:
    file: 2013-07-23-pillagers-7drts-game-development-journal.image-0c973dd82cf5.png
    width: 1066
    height: 600
  color: '#000000'
- source: https://s3.amazonaws.com/superjoe/blog-files/pillagers-7drts-game-dev-journal/day-5-memory-leak.png
  original:
    file: 2013-07-23-pillagers-7drts-game-development-journal.image-945aa95877f1.png
    width: 1359
    height: 520
  color: '#f3f7fc'
- source: https://s3.amazonaws.com/superjoe/blog-files/pillagers-7drts-game-dev-journal/day-5-sandbox-mode.png
  original:
    file: 2013-07-23-pillagers-7drts-game-development-journal.image-3e980e086a8b.png
    width: 1066
    height: 599
  variants:
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-bbff195d114b.webp
    width: 320
    height: 180
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-65b0ec5ba43e.webp
    width: 640
    height: 360
  - file: 2013-07-23-pillagers-7drts-game-development-journal.image-7d4936295619.webp
    width: 1066
    height: 599
  color: '#010000'
---

I have decided to participate in the [7-Day Real Time Strategy Challenge, July 2013 edition](http://www.ludumdare.com/compo/2013/07/05/minild-44-announcement/).

I am working with [Michael Weber](http://brokensounds.com/) who will create sound effects, music, and be in charge of the "atmosphere" of the game.

## [Day 1](https://andrewkelley.me/post/pillagers-7drts-game-dev-journal.html#day-1)

[Cross-posted on ludumdare.com](http://www.ludumdare.com/compo/2013/07/23/pillagers-7drts-game-development-journal-day-1/).

I am using [chem](https://github.com/andrewrk/chem/), a canvas-based game engine I made for rapid development occasions such as this. It has been working out quite well and I think it has been playing a large role in my productivity today.

The game is codenamed "pillagers". The idea is to mix space physics with real time strategy and see what comes out. Instead of creating a carefully planned out base, you will be campaigning through levels, pillaging for resources.

That's the idea, anyway. We'll see how it pans out.

Here's a list of the things that work right now:

- Selecting squads and telling them to move around.
- Ships shoot enemy ships, destroying them when their health reaches 0.
- Scrolling around the map.
- Auto generated parallax background with stars and a planet.

The code is [open source, hosted on GitHub](https://github.com/andrewrk/pillagers). It's 941 lines of JavaScript:

```
   46 src/bullet.js
   33 src/explosion.js
  432 src/main.js
   19 src/militia_ship.js
  241 src/ship_ai.js
  165 src/ship.js
    5 src/uuid.js
  941 total
```

Screenshot of a squad of ships under attack:

![](https://s3.amazonaws.com/superjoe/blog-files/pillagers-7drts-game-dev-journal/screenshot-day-1.png)

Short video of me playing around with some elements of the game

It was a fun challenge to write the AI to get the ships to stop at the intended destinations. As is it's not optimal, but I think that's okay. Maybe later classes of ships will have better AI.

You can actually [play this game right now](http://s3.amazonaws.com/superjoe/temp/pillagers/index.html). Since it's web based, you don't even have to download anything.

Here's what the TODO list looks like currently:

- Edit the main militia ship to have short range.
- Auto targeting enemies should only work when close enough.
- Ability to right click an enemy ship to tell squad to target it.
- Add enemy turrets and ships to level 1.
- Add enemy flag which grants victory when destroyed.
- Add 2nd class of ship which you get some of at beginning of level 1.
- Instead of giving the player ships at the beginning, give them cash and buildings which they can use to create the ships they want. The user will thus be able to choose how many of class 1 and class 2 ships they want.
- Put instruction label text in level 1 to explain the controls.
- Start planning level 2.

Well, I'm off to bed to get some rest. Looking forward to Day 2!

## [Day 2](https://andrewkelley.me/post/pillagers-7drts-game-dev-journal.html#day-2)

[Cross-posted on ludumdare.com](http://www.ludumdare.com/compo/2013/07/24/pillagers-7drts-game-development-journal-day-2/).

[opengameart.org](http://opengameart.org/) has been very good to me. I think this will be my new go-to game dev art supply. So far I've found everything I've wanted on that site, except for a meteor sprite. Maybe I'll make one and contribute back.

Michael made a lazer sound and a ship thrusting sound, and I added the sound effects to the game. In addition to that, I made a ton of progress today:

- 3pm - draw flags only if selected
- 5pm - add another class of ship
- 8pm - found out I was using `t * a ^ 2 + t * v` for acceleration instead of `a * t ^ 2 + v * t`. I fixed all the math and made ships arriving at gather points nice and smooth.
- 9pm - added team colors overlayed on ships
- 10pm - gave the Militia ship an electric melee attack
- 11pm - gave Militia ships smart attacking AI
- 12am - made Ranger ships pursue targets
- 2am - added TurretShip and FlagShip
- 3am - added support for ships with backwards thrusters
- 3am - added move & engage command
- 3am - improved ship selection capabilities
- 5am - added title screen, credits screen, and game over screen. I made it possible to win and lose the game.

Here's the title screen:

![](https://s3.amazonaws.com/superjoe/blog-files/pillagers-7drts-game-dev-journal/day-2-titlescreen.png)

Here's a screenshot of a battle underway:

![](https://s3.amazonaws.com/superjoe/blog-files/pillagers-7drts-game-dev-journal/day-2-screenshot.png)

Battles can get hectic and chaotic, but it is fun to watch it play out. Here's a video of me demoing a battle:

Again, you can playtest this game in your browser right now using the [same url](http://s3.amazonaws.com/superjoe/temp/pillagers/index.html) as Day 1. Feel free to give me feedback or advice.

I came up with some ideas on what bigger picture gameplay will look like which I am pretty pleased with. There will be a bunch of different classes of ships, for example:

Militia

Basic cheap unit. Has short range. Agile. Actually doesn't shoot lazers, shoots a short range but powerful lightning attack out the front. It must resort to charging other ships to attack them.

Ranger

Short range weak lazers. Less agile. Less defense. In a level, a Militia should be able to overtake a Ranger and kill it.

Artillery

Slower, shoots big lazers. Slow moving, weak defense.

FlagShip

Does not attack. Has high defense. Moves very slowly.

Turret

Stationary lazer shooter.

Medi

Heals other ships.

There will be some obvious deficiencies with the ships targeting systems and AI. These can be helped with upgrades, such as:

- Targeting
  - Target the first enemy found (default)
  - Target the closest enemy.
  - Communicate with others to divide up the targets evenly.
- Ranger
  - Upgrade range.
  - Update bullet count to 3.
  - Evasive maneuvers 1.
  - Evasive maneuvers 2.
  - Smarter aiming.
  - Backwards thrusters.
- Militia
  - Evade lazers.
  - Backwards thrusters.
  - Smarter aiming.

You will start with only a flagship, and as you progress throughout the campaign, you will start to build up a convoy that gets ever bigger and more powerful. You'll need it to be bigger and more powerful to get through later levels, in fact.

Here's what's next on the TODO list:

- Build simpler level 1 where you only have to navigate your flag ship around meteors.
- Give the player some cash and let them choose to build Ranger ships or Militia ships using the Flagship.
- Create level 2 where you have to destroy the enemy flagship and then fly your flagship through the created portal.

I'm at a pretty good checkpoint right now. I'm calling it a night.

## [Day 3](https://andrewkelley.me/post/pillagers-7drts-game-dev-journal.html#day-3)

[Cross-posted on ludumdare.com](http://www.ludumdare.com/compo/2013/07/25/pillagers-7drts-game-development-journal-day-3/).

Today Michael gave me the first music track he composed for the game and I am impressed. I am excited to have professional sounding music for a change.

That being said, the first thing I did after inserting the music into the game was program a mute button so that I could keep listening to techno while I coded.

Michael also delivered an electric explosion sound effect that works perfectly.

I mentioned yesterday that I could not find a meteor graphic to use on [opengameart.org](http://opengameart.org/). I actually did end up finding [art that works really well](http://opengameart.org/content/rocks) by searching for "rock".

You can see the three rock types in the current spritesheet for the game which is autogenerated by [chem](https://github.com/andrewrk/chem/):

![](https://s3.amazonaws.com/superjoe/blog-files/pillagers-7drts-game-dev-journal/day-3-spritesheet.png)

The first thing I did today was create a electrical disintegration animation, and I'm pretty pleased with the result:

![](https://s3.amazonaws.com/superjoe/blog-files/pillagers-7drts-game-dev-journal/day-3-disintegrate.gif)

I wonder if this is considered good enough to submit to [opengameart.org](http://opengameart.org/).

After I finished that animation, I worked hard and had a productive day:

- 3pm - Finished electrical disintegration animation and added sound effect to game.
- 3pm - Added in Michael's background music and made it so you can toggle it with M key.
- 5pm - Added meteors with collision detection.
- 5pm - Fixed "ships rotating for no reason" bug.
- 6pm - Inserted a new Level 1 - meteor field that you have to navigate through.
- 7pm - Created a spastic portal graphic and added it to the level.
- 7pm - Fixed navigation bug for ships equipped with backwards thrusters.
- 9pm - Fixed scrolling when manual overriding.
- 11pm - Added a UI pane when ships are selected.
- 12am - Added a mini-map.
- 1am - Made it so you can send ships into a portal.
- 3am - Made it so selected portal shows what's inside it.
- 4am - Added ability to send ships out of a portal.
- 5am - Added announcement support.
- 6am - Made your convoy show up on the level complete screen.
- 6am - Added stats to the level complete screen.
- 8am - Finished level complete screen so that you can get to the next level.

The source tree has grown quite a bit since last time I checked, up to 2,704 lines:

```
   49 src/bullet.js
   77 src/credits_screen.js
   43 src/flag_ship.js
   30 src/fx.js
   73 src/game.js
   34 src/game_over_screen.js
  178 src/level_complete_screen.js
   20 src/main.js
   81 src/meteor.js
   58 src/militia_ship.js
   91 src/physics_object.js
   82 src/portal.js
   60 src/ranger_ship.js
   37 src/sfx.js
  449 src/ship_ai.js
  177 src/ship.js
    7 src/ship_types.js
   57 src/squad.js
  933 src/state.js
   23 src/team.js
  105 src/title_screen.js
   35 src/turret_ship.js
    5 src/uuid.js
 2704 total
```

I actually feel pretty good about the code organization right now. I have only had to pause progress and refactor 2-3 times and each time it was mostly painless.

Enough of the technical stuff. Let's see some screenshots.

Here's one of the meteor field level you start out in:

![](https://s3.amazonaws.com/superjoe/blog-files/pillagers-7drts-game-dev-journal/day-3-screenshot.png)

Here's what it looks like when you finish the level:

![](https://s3.amazonaws.com/superjoe/blog-files/pillagers-7drts-game-dev-journal/day-3-level-complete.png)

And here's me giving a walkthrough of the progress made today:

My TODO list is getting a bit unruly. I've divided it into "next steps" and "nice-to-have"s:

Next steps:

- Add text instructions in Level 1 to explain the controls and how to beat the level.
- Make it so that you start out the next level with the same fleet that you exited with.
- Show your cash in the UI
- Allow you to create ships that you unlock by spending cash.
- Insert a level between 1 and 2 with some attackers. You'll have to build some Ranger ships to defend your Flagship.

Nice-to-haves:

- Figure out why the game slows to a crawl when many ships are added and then deleted.
- Experiment with speed cap on ships. (I'm reluctant to do this one - I like the idea of high-velocity dogfighting.)
- If Ranger is in range, don't accelerate toward target.
- Fix the thruster sound glitchiness
- Each command should draw something on the screen to indicate that something is commanded. (Some commands do this already.)
- When forming a squad, don't assume all ships have the same radius.

And now I must rest. I am exhausted.

## [Day 4](https://andrewkelley.me/post/pillagers-7drts-game-dev-journal.html#day-4)

[Cross-posted on ludumdare.com](http://www.ludumdare.com/compo/2013/07/26/pillagers-7drts-game-development-journal-day-4/).

It has been wonderful relying only upon circles in this physics engine. The math is simple and beautiful, and it's easy to write fast code. Who needs polygons anyway?

Today was a good day. Pillagers is now [actually a game](http://s3.amazonaws.com/superjoe/temp/pillagers/index.html). What I got done:

- 5pm - Added more of Michael's sound effects into the game.
- 5pm - Tweaked the physics.
- 5pm - Added some cheats to help speed up testing.
- 6pm - Added text in Level 1 to explain controls.
- 7pm - The game shows how much cash you have.
- 10pm - You keep your same fleet when progressing to the next level.
- 12am - Added a new Level 2.
- 12am - Made unlocking ships work.
- 12am - Made it so you get cash from killing enemy ships.
- 12am - Tweak the money system.
- 1am - Made it so you can scroll and give orders while paused.
- 1am - Fixed some game crashes and bugs.
- 2am - Modified attacking AI to stay within certain speed limits. Makes the game less chaotic.
- 3am - Added ability to skip tutorial levels.
- 4am - Better squad formation.
- 4am - Updated the Move & Engage command to be more effective.
- 6am - Added Civilian ships and plan out Level 4.
- 7am - Add Artillery ship and finish implementing Level 4.
- 8am - Fix the crash which happened at the end of the demo below.

Screenshot:

![](https://s3.amazonaws.com/superjoe/blog-files/pillagers-7drts-game-dev-journal/day-4-screenshot.png)

Screencast demo:

Bed.

## [Day 5](https://andrewkelley.me/post/pillagers-7drts-game-dev-journal.html#day-5)

[Cross-posted on ludumdare.com](http://www.ludumdare.com/compo/2013/07/27/pillagers-7drts-game-development-journal-day-5/).

I spent a good chunk of the day trying to solve a performance problem. After approximately 100,000 bullets were fired, the framerate would drop to an excruciating 16 FPS.

I was able to figure it out by using Google Chrome's [heap profiling tool](https://developers.google.com/chrome-developer-tools/docs/heap-profiling). Here's what the heap snapshot looked like:

![](https://s3.amazonaws.com/superjoe/blog-files/pillagers-7drts-game-dev-journal/day-5-memory-leak.png)

I found out that there was a memory leak in the game engine - every time a sprite was created it called `setInterval` but it did not call `clearInterval` when the sprite was deleted. Ever since I [fixed the issue](https://github.com/andrewrk/chem/commit/900ba7c6f5c27617d9917653ee4d63e4f043374d), FPS has stayed at a nice and smooth 60, no matter how many objects are created and destroyed.

Here are some other things I got done today:

- 9pm - Miscellanous small enhancements.
- 11pm - Fixed the performance issue.
- 12am - Added the new thruster sound that Michael gave me.
- 3am - Added Sandbox Mode.
- 4am - Added more features to Sandbox Mode.
- 6am - Added Ctrl+A to select all your ships. Also made WASD and J work the same as arrow keys and space.
- 7am - Added dogfighting mode.
- 8am - Added graphics so that ships display their targets when selected.
- 8am - Added 2 more dogfighting levels. Enhanced manual piloting of Militia ship so that you can use your melee attack.

Here's a screenshot of Sandbox mode:

![](https://s3.amazonaws.com/superjoe/blog-files/pillagers-7drts-game-dev-journal/day-5-sandbox-mode.png)

Here's me playing through Dogfighting Mode and messing around in Sandbox Mode a bit:

I have only 2 days left to work. The last day will be primarily reserved for gameplay tweaking, bugfixes, touchups, and testing. That leaves only 1 more day to make actual progress.

These are my goals for tomorrow:

- Add a rotating turret to the flagship.
- Add a civilian to Level 1 and use that to explain the Move command vs the Engage command.
- Add a dogfighting level where there is an ongoing battle and you have to rack up a certain number of kills to win.
- Add more campaign levels. I reserve the right to ramp up the difficulty in the later levels!
- Pan sound effects and volume depending on scroll location
- Add some more ship classes and implement upgrades.

Thanks to the folks in the #7dRTS IRC channel for helping me playtest today. Specifically Zapa and Orava.

## [Day 6](https://andrewkelley.me/post/pillagers-7drts-game-dev-journal.html#day-6)

[Cross-posted on ludumdare.com](http://www.ludumdare.com/compo/2013/07/28/pillagers-7drts-game-development-journal-day-6/).

Today I worked on making the level format and core engine more robust. I added a bunch more dogfighting levels. There are 11 now, and they get pretty crazy at the end.

The cool thing about them is that the core game engine does not even know that you are in "Dogfighting Mode". There's no `dogfightingMode` variable that tells the engine that's what you're doing. Instead, the [level format](https://github.com/andrewrk/pillagers/blob/7d09b2f0842e8962e059ebaa7c05fbf47cff890a/public/text/dogfight0.json) supports trigger conditions, events, and groups, and the logic of dogfighting is done in the level JSON file.

Michael gave me the battle music today as well. It sounds amazing. I made it so that the game automatically detects when you are entering or exiting a battle and transitions the music appropriately. I'm pleased with the effect.

Here's a list of what I got done today:

- 12am - Added battle music. The game automatically detects when a battle is happening and fades in the battle music, then fades back to normal music once the battle is over.
- 1am - Added more dogfighting levels.
- 3am - Worked on making dogfighting levels more fun. You have to try again if you die rather than respawning.
- 4am - Made the Militia ship show the area of attack when you are manually piloting it.
- 6am - Sandbox Mode: Ability to load and save. I was able to use this to build some dogfighting levels.
- 7am - Added more dogfighting levels.

I did not accomplish most of what I set out to today. Regardless, I am really happy with Dogfighting Mode right now. It's almost certainly more fun than Campaign Mode currently.

Here's a video of me playing through it:

This version of the game is going to be pretty close to the final one that I submit for 7dRTS. I will probably only spend about 6-8 more hours on this debugging on Firefox and bugfixing. I will not be supporting Internet Explorer.

## [Day 7](https://andrewkelley.me/post/pillagers-7drts-game-dev-journal.html#day-7)

Today was short. I added more sound effects that Michael provided, and lowered the difficulty on several levels. The game is in a stable state and I don't want to screw it up before submitting.

I experimented with adding aiming hints when manually piloting a ship, but I found that it actually made the game more difficult because it tricks the player into aiming directly at the enemy ships instead of compensating for relative velocity.

I tried to make my girlfriend play the game, but she said it was too hard. It probably is too hard. I like hard games.

Either way, I'm submitting.

[View my entry on ludumdare.com](http://www.ludumdare.com/compo/minild-44/?action=preview&uid=24697).

[Play the game in your web browser](http://s3.amazonaws.com/superjoe/temp/pillagers/index.html).

Thanks for reading my blog post.
