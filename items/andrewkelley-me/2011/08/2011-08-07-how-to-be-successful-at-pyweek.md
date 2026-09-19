---
title: How to be Successful at PyWeek
link: https://andrewkelley.me/post/pyweek-success.html
source: andrewkelley-me
published: 2011-08-07T12:00:00Z
updated: 2011-08-07T12:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'How to be Successful at PyWeek A short series to help you win. Covers these topics: What PyWeek is, and Why You Should Participate Video Games in Python Speed Developing Game Theory - How to Create Fun Getting the Most Out of the Community The Psychology of Judging Conclusion What PyWeek is, and Why You Should Participate PyWeek is a challenge. It asks you to spend one week of your life developing a new video game, using Python. PyWeek is difficult. It is not easy to create a new video game in just one week, especially one that is fun, innovative, and polished. But it is fun to try, especially with peers from all around the world working alongside you toward the same goal. PyWeek will strengthen you as a game developer. If you go solo it will strengthen you as a generalist, and if you join a team it will strengthen you as a team member. As of this writing, the next competition is the week of September 11, 2011 to September 18, 2011. Registration opens August 12, 2011. You can check out the competition''s Rules and FAQ. Video Games in Python Python is a great language for rapid video game development: Large selection of libraries to use, for example PyGame and Pyglet Python is known for being a prototyping language due to its ease of use. It is simple to create arbitrary one-off data structures that you often need in game programming. You can use objects and inheritance if you need it. Cross platform There are some drawbacks though: Deployment is cumbersome. For PyWeek this isn''t too much of an issue since everyone will run from source, but for an official game, it would be an issue. Code is about 10x less efficient than in C or C++. This will not matter for graphics on screen, since you will likely use a game library which uses the GPU for graphics, but it might make your main loop or physics slow. Just keep these things in mind when developing. Speed Developing The biggest thing preventing you from success during PyWeek is lack of time. Therefore, the more time you conserve, and the more time-consuming things you can do before the development week, the better chances you have for success. Allocate as much time as possible. PyWeek is inspired by the Ludum Dare competition[1], which only lasts 48 hours. In that competition, you pretty much have to use all 48 hours if you want to be successful. PyWeek is one week long to make it easier on people who have commitments to things other than the Internet. Regardless, your success will be directly proportional to the amount of time you spend on PyWeek. Get ahead at work and/or school so that you can spend as little time on them as possible. Make sure your significant other knows you don''t want to be bothered this particular week. You''ll make up for it later. Remember not to agree to go to any social activities that will take place during PyWeek. If someone asks you to hang out, try to change the time to before or after. Use version control. You may be tempted to forgo version control, in the interest of time. One of two things are true. Either, You are new to version control, and you honestly would spend more time struggling with the version control software than developing your game, or You are comfortable with version control, but think that you can save time by not using it since this is a speed-developing contest. In the first case, you should use version control anyways, because that skill should take priority over game development, and this is a great opportunity to learn. GitHub makes it dead simple if you are okay with making your project open source. In the second case, let me assure you that version control will actually save you time: When you''re tired and you forgot what you were working on, you can use the diff tool to quickly remember what you were working on. You can spend a few minutes working on a feature or experimenting with a different style of gameplay to see if it''s fun. If not, it''s one command to scrap it and try again. If you''re used to using version control and you forgo it for the competition, it will throw off your groove to not use it, messing with your psychology in a time-wasting manner. Practice beforehand. Development is all about solving problems. There are some problems you are bound to run into that are simply a function of Python, the game library you are relying on, and the fact that you are writing a game. You would run into these problems no matter what game you decided to write. Therefore, it makes sense to solve these problems before the actual competition. Practice writing a simple game engine with the library of your choice before the competition. You will run into and solve some issues that you would have otherwise had to waste time solving during the competition. Examples of things you may want to practice: Basic game engine / main loop Displaying graphics Animating things on the screen Responding to user input Sound playback Transitioning state between a title screen or menu system and gameplay. 3D graphics Networking Deploying on Windows, Mac, and Linux Practice using the tools you are going to use as well. Before the competition starts, you should feel comfortable creating assets, such as: Art Animations Sound effects Music Often you can make at least half of your sound effects using only a microphone, Audacity, and some creativity. PyWeek allows you to use artwork, music, and sounds whose copyrights explicitly allow you to use them. If you are going to use other people''s artwork, find the databases and websites where you are going to get it from and make sure the citation process is efficient and will work. Have your game idea ready before the theme is decided. You have one week between finding out what the 5 possible themes are, and the start of development. Use this week to brainstorm several game ideas for each theme, before you even know which one will win. Brainstorming is a lengthy process. Sometimes it can help to give yourself fake limitations in order to get your brain to think of ideas. For each possible theme, try to think of a game that fits each category: Action Adventure Puzzle Role-Playing Simulation Other - a crazy wacky idea that probably will be stupid Sometimes trying to think of an idea for one category will make you think of an idea for another category. That''s great, write it down! And sometimes trying to think of an idea for one theme will make you think of an idea for a different theme. That''s great, write it down! At this point you want as many ideas as possible. A friend or room mate can be very useful when brainstorming ideas. Bouncing ideas off each other is a great way to come up with material. Once the theme is decided, you can often pick your best idea, regardless of what theme you thought of it for, and adapt it to fit the real theme. My winning entry, Lemming, was originally an idea inspired by the "Fry Cook on Venus" theme, but I adapted it to the "Nine Times" theme when it was chosen. [2] Rely on other projects as much as possible. As a general rule, do not try to write a level editor during PyWeek. You need as much time as possible to spend on the game itself. Consider using Tiled and DR0ID''s tiledtmxloader. Get enough sleep. Your body needs sleep. You cannot cheat around that fact. The longer you sleep-deprive yourself, the less clearly you will think, the more mistakes you will make, and the more slowly you will work. When you rest, you give your unconscious mind a chance to sort things out and surface solutions and ideas that you wouldn''t have thought of consciously. Sleep is directly linked to creativity.[3] Game Theory - How to Create Fun It is easy to start programming and create a simple platformer, without thinking about what makes platformers fun. It is easy, but not nearly as rewarding as it is to actually think about Game Theory - a fascinating subject that philosophers have been thinking about for a very long time. Think about game theory. If you have never read that Wikipedia page, do. Be sure to check out the simple example, Prisoner''s Dilemna, and especially the See Also section of that page for other great examples. In short, you should be constantly asking yourself these questions while designing your game: What do I want the player to try to do? What is the player actually motivated to try to do? Is what I want the player to try to do the same as what the player is actually motivated to try to do? Why or why not, exactly? What elements naturally drive the player to act in a certain way? Is what I want the player to try to do fun? What exactly makes it so, or not so? Are the player''s short term goals conflicting with the player''s long term goals? (This is one way to create good gameplay.) What compels the player to keep playing? Is the player compelled to keep playing compulsively, or because they are having fun? Do I want the experience of playing to be the same every time, or have random elements that change up the gameplay? Are there any parts of the gameplay that are definitely not fun? Is it possible to make it fun to lose? What forces is the player working against? Does the player feel like the forces working against him are acting fairly? There is a plethora of reading (and watching) material on creating interesting gameplay. Here are some materials to get you started thinking about game design: Extra Credits: The Skinner Box Extra Credits: Narrative Mechanics Get your game into a playable state as soon as possible. Developing a game is an evolutionary process. The sooner you have a playable game, the sooner you can begin to morph it and mold it into a better and better game. Ask yourself if it is fun. Try to figure out what makes it fun, and expand on those elements of gameplay. Try out your gameplay on friends and see what they do. Try to not talk to them while they are playing. Figure out what parts your friends get frustrated at and change those parts. Sketch your levels on paper before coding anything. This will give you an idea of the world you''re trying to create, giving you focus and direction when you code. Getting the Most Out of the Community PyWeek is only as fun as the people who participate. After all, there are no prizes, no reason to compete, other than for personal development and community interaction. You get what you give. Keep a journal. One great way to participate is to write a diary entry after each day of work. Write about: Any progress made Things that are impeding progress Lessons learned that you want to remember for next time Teaser screenshots, art, videos, sounds, or music Decisions that you are pondering about in your game design It is fun for others to follow along with your progress and for you to follow along with others. It is customary to be positive, uplifting, and encouraging in response to diary entries, but don''t be afraid to tactfully include constructive criticism. On the flip side, be ready to accept critique, using it to improve your design. Join the IRC channel. You can feel the camaraderie by hanging out in #pyweek on Freenode. Live chat is personal and you can get to know the other participants fairly well. This place is especially fun during judging time. At one point we broke out into a spontaneous one-hour game development competition. There is almost always someone ready to give instant feedback about a game design decision or to give tips on development in general. One thing you want to be careful of, however, is spending too much time distracted by IRC during development time. You should be spending almost all your development time developing. Follow along with the forum discussion. The forum is the main communication facility of PyWeek. While it may take longer for someone to respond than on IRC, this is a great place to get questions answered and to participate in general in the community. Diary entries are posted to the forum so that everyone sees them and can comment on them. Play and judge as many games as you can. As developers, we thrive on feedback. When it is time to judge, give as much valuable and honest feedback as you can to as many games as you can stand. Giving a silly award can be a fun way to give praise to a weird aspect of a game. The Psychology of Judging Realize that in the end you are creating a demo. Your project is not going to be a polished, ready-to-ship game in one week. Your goal is to make sure judges play your entire game experience and do not get distracted by nuances. Test your game on several people. Find a few friends who have not been following along with your development and shove the game in front of their face. Set them up with the title screen, but say nothing else. Watch their face, watch their hands, and watch the screen. You are now getting an accurate preview of how judges will react to your game. Pay attention to what parts of your gameplay they don''t understand. Notice when they ignore instructions you didn''t want them to. Take note of what parts they get stuck at. All of these things are what you need to work on. If you don''t, this is everything that judges will talk about in your game''s feedback. One mistake I made was testing my game on someone who is exceptionally good at video games. He didn''t have too much of a problem with the levels that the real judges simply got stuck at and gave up. Remember that judges have hundreds of games to play. If at any time during your game they get stuck, they are likely to shrug and move on to a different game. The first few minutes of your game should be ridiculously easy. It is critical that you gradually ramp up the difficulty, starting off stupidly easy. Nobody likes a game that treats the player like an idiot. However, a player is far more willing to tolerate a Level 1 that they can pass without thinking, than they are to try to disassemble a masterpiece puzzle at the very beginning. Judges are much more likely to give up and move on. As long as each new section, or level, is at least a little tiny bit more challenging than the last, the player will be intrigued and want to keep playing. There is a time and a place for ridiculously hard challenging bonus levels, and that time and place is after the player has beaten the normal game, as a reward for their success. The test level that you use for 90% of your debugging will soon become boring and easy to you, but ridiculously hard for a new player. It has no place in the normal game; it should be in bonus-land. Make stupidly easy levels come first, and then slowly introduce your more complex gameplay elements as the player progresses and gains confidence. Again, realize that your PyWeek game is essentially a demo. You might want to include a level select on the title screen, so that a frustrated judge can skip levels that were hard in ways that you did not expect. The easier you make it for fellow PyWeekers to experience the fullness of your creation, the better feedback you will get. Post a walkthrough with your final submission. Once again, the goal here is to get each judge to experience all of your content, not to stump them with your enigmatic gameplay. Post a clear and detailed walkthrough that explains how to get through and beat each level. Reading instructions is boring. No matter how complex your gameplay is, there is no excuse for making your players read a wall of text. There should be no instructions necessary. The player should be able to learn the gameplay by immediately beginning to play, and then being gradually given small bits of learning to digest. Tutorial levels are highly encouraged. Remember that one of the categories you are being rated on is Fun[4], and reading instructions is not fun. Remember that not everyone has your setup. Some people use alternate keyboard layouts. In PyWeek 12, 11% of participants used a keyboard other than QWERTY.[5] Be polite to alternate keyboard users. Defaulting to WASD is acceptable, but the player should be able to remap the keys. A config file is an acceptable method for remapping keys, as long as it is obvious how to do it without a lot of instruction-reading. However, alternate keyboard users are often proactive, and willing to put forth a bit of extra effort to compensate for their questionable life choices*. If you are running low on development time, it would be better to focus on gameplay rather than being nice to alternate keyboard users. *Note: I am a Dvorak user. Most operating systems are case sensitive. Time to pick on Windows users. It is unacceptable to rely on your file system''s case insensitivity in your code. It is a huge pain to rename all media files in someone else''s project and then grep through their code to find the filenames in the effort to get the program to not crash. Make sure you use the same case in your filenames as you do in your code. Everyone''s screen situation is different. The simplest thing to do is use a fixed sized windowed application - that way you don''t have to take into account different screen resolutions and ratios. Some people consider it rude for games to go fullscreen without explicit instruction. Do not make a tarbomb. When you create a tar file, everything should be inside of a single folder with your game name and version number. Test your final submission. Once you upload your final submission to the website, immediately download it again into a new folder and try to play it. This ensures that you didn''t make any stupid mistakes. Test on Windows, Mac, and Linux. Get a friend to test it for you if you don''t have direct access to any of these OS''s. Linux is free, so there is no excuse for not testing it on Ubuntu. You would be amazed at how many people forget to include their media folder, or make some trivial mistake that renders their entire game unplayable. Conclusion I wrote this article selfishly. I feel compelled to play every contestant''s game, and I want the games that I play to be fun. Hopefully this article is helpful enough to raise the standard of PyWeek.'
content: extracted
html: 2011-08-07-how-to-be-successful-at-pyweek.html
---

A short series to help you win. Covers these topics:

1. [What PyWeek is, and Why You Should Participate](https://andrewkelley.me/post/pyweek-success.html#intro)
2. [Video Games in Python](https://andrewkelley.me/post/pyweek-success.html#python)
3. [Speed Developing](https://andrewkelley.me/post/pyweek-success.html#speed)
4. [Game Theory - How to Create Fun](https://andrewkelley.me/post/pyweek-success.html#game)
5. [Getting the Most Out of the Community](https://andrewkelley.me/post/pyweek-success.html#community)
6. [The Psychology of Judging](https://andrewkelley.me/post/pyweek-success.html#judging)
7. [Conclusion](https://andrewkelley.me/post/pyweek-success.html#conclusion)

## What PyWeek is, and Why You Should Participate

[PyWeek](http://pyweek.org/) is a challenge. It asks you to spend one week of your life developing a new video game, using [Python](http://python.org/).

PyWeek is difficult. It is not easy to create a new video game in just one week, especially one that is fun, innovative, and polished. But it *is* fun to try, especially with peers from all around the world working alongside you toward the same goal.

PyWeek will strengthen you as a game developer. If you go solo it will strengthen you as a generalist, and if you join a team it will strengthen you as a team member.

As of this writing, the [next competition](http://www.pyweek.org/13/) is the week of **September 11, 2011** to **September 18, 2011**. Registration opens August 12, 2011.

You can check out the competition's [Rules](http://pyweek.org/s/rules/) and [FAQ](http://pyweek.org/s/help/).

## Video Games in Python

Python is a great language for rapid video game development:

- Large selection of libraries to use, for example [PyGame](http://pygame.org/) and [Pyglet](http://www.pyglet.org/)
- Python is known for being a prototyping language due to its ease of use.
- It is simple to create arbitrary one-off data structures that you often need in game programming.
- You can use objects and inheritance if you need it.
- Cross platform

There are some drawbacks though:

- Deployment is cumbersome. For PyWeek this isn't too much of an issue since everyone will run from source, but for an official game, it would be an issue.
- Code is about 10x less efficient than in C or C++. This will not matter for graphics on screen, since you will likely use a game library which uses the GPU for graphics, but it might make your main loop or physics slow.

Just keep these things in mind when developing.

## Speed Developing

The biggest thing preventing you from success during PyWeek is lack of time. Therefore, the more time you conserve, and the more time-consuming things you can do *before* the development week, the better chances you have for success.

### Allocate as much time as possible.

[PyWeek](http://pyweek.org/) is inspired by the [Ludum Dare](http://www.ludumdare.com/compo/) competition [\[1\]](http://pyweek.org/s/help/#what-s-pyweek-all-about), which only lasts 48 hours. In that competition, you pretty much have to use all 48 hours if you want to be successful. PyWeek is one week long to make it easier on people who have commitments to things other than the Internet. Regardless, **your success will be directly proportional to the amount of time you spend** on PyWeek.

- Get ahead at work and/or school so that you can spend as little time on them as possible.
- Make sure your significant other knows you don't want to be bothered this particular week. You'll make up for it later.
- Remember not to agree to go to any social activities that will take place during PyWeek. If someone asks you to hang out, try to change the time to before or after.

### Use version control.

You may be tempted to forgo version control, in the interest of time. One of two things are true. Either,

1. You are new to version control, and you honestly would spend more time struggling with the version control software than developing your game, or
2. You are comfortable with version control, but think that you can save time by not using it since this is a speed-developing contest.

In the first case, you should use version control anyways, because that skill should take priority over game development, and this is a great opportunity to learn. [GitHub](https://github.com/) makes it dead simple if you are okay with making your project open source.

In the second case, let me assure you that version control will actually save you time:

- When you're tired and you forgot what you were working on, you can use the diff tool to quickly remember what you were working on.
- You can spend a few minutes working on a feature or experimenting with a different style of gameplay to see if it's fun. If not, it's one command to scrap it and try again.
- If you're used to using version control and you forgo it for the competition, it will throw off your groove to not use it, messing with your psychology in a time-wasting manner.

### Practice beforehand.

Development is all about solving problems.

There are some problems you are bound to run into that are simply a function of Python, the game library you are relying on, and the fact that you are writing a game. You would run into these problems no matter what game you decided to write.

Therefore, it makes sense to solve these problems before the actual competition. Practice writing a simple game engine with the library of your choice before the competition. You will run into and solve some issues that you would have otherwise had to waste time solving during the competition.

Examples of things you may want to practice:

- Basic game engine / main loop
- Displaying graphics
- Animating things on the screen
- Responding to user input
- Sound playback
- Transitioning state between a title screen or menu system and gameplay.
- 3D graphics
- Networking
- Deploying on Windows, Mac, and Linux

Practice using the tools you are going to use as well. Before the competition starts, you should feel comfortable creating assets, such as:

- Art
- Animations
- Sound effects
- Music

Often you can make at least half of your sound effects using only a microphone, [Audacity](http://audacity.sourceforge.net/), and some creativity.

PyWeek allows you to use artwork, music, and sounds whose copyrights explicitly allow you to use them. If you are going to use other people's artwork, find the databases and websites where you are going to get it from and make sure the citation process is efficient and will work.

### Have your game idea ready before the theme is decided.

You have one week between finding out what the 5 possible themes are, and the start of development. Use this week to **brainstorm several game ideas for each theme**, before you even know which one will win.

Brainstorming is a lengthy process. Sometimes it can help to give yourself fake limitations in order to get your brain to think of ideas. For each possible theme, try to think of a game that fits each category:

- Action
- Adventure
- Puzzle
- Role-Playing
- Simulation
- Other - a crazy wacky idea that probably will be stupid

Sometimes trying to think of an idea for one category will make you think of an idea for another category. That's great, write it down! And sometimes trying to think of an idea for one theme will make you think of an idea for a different theme. That's great, write it down! At this point you want as many ideas as possible.

A friend or room mate can be very useful when brainstorming ideas. Bouncing ideas off each other is a great way to come up with material.

Once the theme is decided, you can often pick your best idea, regardless of what theme you thought of it for, and adapt it to fit the real theme. My winning entry, [Lemming](http://www.pyweek.org/e/superjoe/), was originally an idea inspired by the "Fry Cook on Venus" theme, but I adapted it to the "Nine Times" theme when it was chosen. [\[2\]](https://github.com/andrewrk/lemming/blob/master/ideas/theme-brainstorming)

### Rely on other projects as much as possible.

As a general rule, do not try to write a level editor during PyWeek. You need as much time as possible to spend on the game itself. Consider using [Tiled](http://www.mapeditor.org/) and [DR0ID's tiledtmxloader.](https://code.google.com/p/pytmxloader/)

### Get enough sleep.

Your body needs sleep. You cannot cheat around that fact. The longer you sleep-deprive yourself, the less clearly you will think, the more mistakes you will make, and the more slowly you will work.

When you rest, you give your unconscious mind a chance to sort things out and surface solutions and ideas that you wouldn't have thought of consciously.

Sleep is directly linked to creativity.[\[3\]](http://en.wikipedia.org/wiki/Sleep_and_creativity)

## Game Theory - How to Create Fun

It is easy to start programming and create a simple platformer, without thinking about what makes platformers fun. It is easy, but not nearly as rewarding as it is to actually think about [Game Theory](http://en.wikipedia.org/wiki/Game_theory) - a fascinating subject that philosophers have been thinking about for a very long time.

### Think about game theory.

If you have never read that Wikipedia page, do. Be sure to check out the simple example, [Prisoner's Dilemna](http://en.wikipedia.org/wiki/Prisoner's_dilemma), and especially the [See Also](http://en.wikipedia.org/wiki/Prisoner%27s_dilemma#See_also) section of that page for other great examples.

In short, you should be constantly asking yourself these questions while designing your game:

- What do I want the player to try to do?
- What is the player actually motivated to try to do?
- Is what I want the player to try to do the same as what the player is actually motivated to try to do? Why or why not, exactly? What elements naturally drive the player to act in a certain way?
- Is what I want the player to try to do fun? What exactly makes it so, or not so?
- Are the player's short term goals conflicting with the player's long term goals? (This is one way to create good gameplay.)
- What compels the player to keep playing?
- Is the player compelled to keep playing compulsively, or because they are having fun?
- Do I want the experience of playing to be the same every time, or have random elements that change up the gameplay?
- Are there any parts of the gameplay that are definitely *not* fun?
- Is it possible to make it fun to lose?
- What forces is the player working against?
- Does the player feel like the forces working against him are acting fairly?

There is a plethora of reading (and watching) material on creating interesting gameplay. Here are some materials to get you started thinking about game design:

- [Extra Credits: The Skinner Box](http://penny-arcade.com/patv/episode/the-skinner-box)
- [Extra Credits: Narrative Mechanics](http://penny-arcade.com/patv/episode/narrative-mechanics)

### Get your game into a playable state as soon as possible.

Developing a game is an evolutionary process. The sooner you have a playable game, the sooner you can begin to morph it and mold it into a better and better game.

Ask yourself if it is fun. Try to figure out what makes it fun, and expand on those elements of gameplay.

Try out your gameplay on friends and see what they do. Try to not talk to them while they are playing. Figure out what parts your friends get frustrated at and change those parts.

### Sketch your levels on paper before coding anything.

This will give you an idea of the world you're trying to create, giving you focus and direction when you code.

PyWeek is only as fun as the people who participate. After all, there are no prizes, no reason to compete, other than for personal development and community interaction.

You get what you give.

### Keep a journal.

One great way to participate is to **write a diary entry after each day of work**. Write about:

- Any progress made
- Things that are impeding progress
- Lessons learned that you want to remember for next time
- Teaser screenshots, art, videos, sounds, or music
- Decisions that you are pondering about in your game design

It is fun for others to follow along with your progress and for you to follow along with others. It is customary to be positive, uplifting, and encouraging in response to diary entries, but don't be afraid to tactfully include constructive criticism. On the flip side, be ready to accept critique, using it to improve your design.

### Join the IRC channel.

You can feel the camaraderie by hanging out in #pyweek on Freenode.

Live chat is personal and you can get to know the other participants fairly well. This place is especially fun during judging time. At one point we broke out into a [spontaneous one-hour game development competition.](http://www.pyweek.org/d/3946/) There is almost always someone ready to give instant feedback about a game design decision or to give tips on development in general.

One thing you want to be careful of, however, is spending too much time distracted by IRC during development time. You should be spending almost all your development time developing.

### Follow along with the forum discussion.

The [forum](http://www.pyweek.org/messages/) is the main communication facility of PyWeek. While it may take longer for someone to respond than on IRC, this is a great place to get questions answered and to participate in general in the community.

Diary entries are posted to the forum so that everyone sees them and can comment on them.

### Play and judge as many games as you can.

As developers, we thrive on feedback. When it is time to judge, give as much valuable and honest feedback as you can to as many games as you can stand.

Giving a silly award can be a fun way to give praise to a weird aspect of a game.

## The Psychology of Judging

Realize that in the end you are creating a demo. Your project is not going to be a polished, ready-to-ship game in one week. Your goal is to **make sure judges play your entire game experience and do not get distracted by nuances**.

### Test your game on several people.

Find a few friends who have not been following along with your development and shove the game in front of their face. Set them up with the title screen, but say nothing else.

Watch their face, watch their hands, and watch the screen. You are now getting an accurate preview of how judges will react to your game. Pay attention to what parts of your gameplay they don't understand. Notice when they ignore instructions you didn't want them to. Take note of what parts they get stuck at. All of these things are what you need to work on. If you don't, this is everything that judges will talk about in your game's feedback.

One mistake I made was testing my game on someone who is exceptionally good at video games. He didn't have too much of a problem with the levels that the real judges simply got stuck at and gave up.

Remember that judges have hundreds of games to play. If at any time during your game they get stuck, they are likely to shrug and move on to a different game.

### The first few minutes of your game should be ridiculously easy.

It is critical that you *gradually* ramp up the difficulty, starting off stupidly easy.

Nobody likes a game that treats the player like an idiot. However, a player is far more willing to tolerate a Level 1 that they can pass without thinking, than they are to try to disassemble a masterpiece puzzle at the very beginning. Judges are much more likely to give up and move on.

As long as each new section, or level, is at least a *little tiny bit* more challenging than the last, the player will be intrigued and want to keep playing.

There is a time and a place for ridiculously hard challenging bonus levels, and that time and place is after the player has beaten the normal game, as a reward for their success. The test level that you use for 90% of your debugging will soon become boring and easy to you, but ridiculously hard for a new player. It has no place in the normal game; it should be in bonus-land.

Make stupidly easy levels come first, and then **slowly introduce your more complex gameplay elements as the player progresses and gains confidence.**

Again, realize that your PyWeek game is essentially a demo. You might want to include a level select on the title screen, so that a frustrated judge can skip levels that were hard in ways that you did not expect. The easier you make it for fellow PyWeekers to experience the fullness of your creation, the better feedback you will get.

### Post a walkthrough with your final submission.

Once again, the goal here is to get each judge to experience all of your content, *not* to stump them with your enigmatic gameplay.

Post a clear and detailed walkthrough that explains how to get through and beat each level.

### Reading instructions is boring.

No matter how complex your gameplay is, there is no excuse for making your players read a wall of text.

There should be no instructions necessary. The player should be able to learn the gameplay by immediately beginning to play, and then being gradually given small bits of learning to digest. Tutorial levels are highly encouraged.

Remember that one of the categories you are being rated on is Fun [\[4\]](http://pyweek.org/s/rules/#entries-are-judged-by-peers), and reading instructions is not fun.

### Remember that not everyone has your setup.

#### Some people use alternate keyboard layouts.

In PyWeek 12, 11% of participants used a keyboard other than QWERTY.[\[5\]](https://spreadsheets.google.com/viewanalytics?hl=en_GB&formkey=dEIxVS1SRTdYTHZKbWJiTjdSS3FUYWc6MA)

Be polite to alternate keyboard users. Defaulting to WASD is acceptable, but the player should be able to remap the keys.

A config file is an acceptable method for remapping keys, as long as it is obvious how to do it without a lot of instruction-reading.

However, alternate keyboard users are often proactive, and willing to put forth a bit of extra effort to compensate for their questionable life choices\*. If you are running low on development time, it would be better to focus on gameplay rather than being nice to alternate keyboard users.

\*Note: I am a Dvorak user.

#### Most operating systems are case sensitive.

Time to pick on Windows users.

It is **unacceptable** to rely on your file system's case insensitivity in your code. It is a huge pain to rename all media files in someone else's project and then grep through their code to find the filenames in the effort to get the program to not crash.

Make sure you **use the same case in your filenames as you do in your code**.

#### Everyone's screen situation is different.

The simplest thing to do is use a fixed sized windowed application - that way you don't have to take into account different screen resolutions and ratios.

Some people consider it rude for games to go fullscreen without explicit instruction.

#### Do not make a tarbomb.

When you create a tar file, everything should be inside of a single folder with your game name and version number.

### Test your final submission.

Once you upload your final submission to the website, immediately download it again into a new folder and try to play it. This ensures that you didn't make any stupid mistakes.

Test on Windows, Mac, and Linux. Get a friend to test it for you if you don't have direct access to any of these OS's. Linux is free, so there is no excuse for not testing it on [Ubuntu](http://www.ubuntu.com/).

You would be amazed at how many people forget to include their media folder, or make some trivial mistake that renders their entire game unplayable.

## Conclusion

I wrote this article selfishly. I feel compelled to play every contestant's game, and I want the games that I play to be fun. Hopefully this article is helpful enough to raise the standard of PyWeek.

Thanks for reading my blog post.
