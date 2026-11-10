---
title: Locating the source of a mysterious beep with Claude
link: https://simonwillison.net/2026/Nov/9/mysterious-beeps/
source: simonwillison-net
published: 2026-11-09T23:57:21Z
updated: 2026-11-09T23:57:21Z
first_seen: 2026-11-10T03:25:21.974411074Z
labels:
- ai
- claude
- claude-code
- claude-mythos-fable
- coding-agents
- generative-ai
- llms
- projects
summary: 'Our house has a box room, full of boxes that we still haven''t unpacked from our last move. A couple of weeks ago something in that room started intermittently beeping. The beeps were far enough apart that standing in the room and trying to determine the source of the noise was frustrating, and I really didn''t want to dig through every one of those boxes. So I had Claude Code build Beep Locator, a software system for triangulating the source of a mysterious beep using multiple devices scattered around the room. Here''s what Beep Locator looks like after it has been running for a while: You run Python web server to show the primary UI, and then use the QR code or URL to connect as many devices as you can spare to act as listeners - I was using two iPads, a phone, a MacBook Pro and a Surface Ultra. The devices then play noises to each other to help estimate their locations. This didn''t work particularly well, so I added a "Rough mode" where you can drag their icons around in the space to provide rough locations which are then adjusted based on those sounds. Once that''s set up, the devices all listen for the beep! I added a clapping mode too so I could test it by clapping my hands in different locations around the room - displayed as diamonds on the map. This helped me gain confidence that it was capable of locating a sound. Beeps show up on the map as an X, plus a line in the "Sounds located" log. You can replay these sounds (from each device in turn) to confirm they are the mysterious beep you are looking for, then click "This is the mystery beep" to mark them as such. Once you''ve confirmed the mysterious beep frequency the system will listen out for it, measure the interval between beeps, and use the triangulated locations to lock down the most likely location on the map. The system worked as intended. It gave me the position of the beep to within about 40cm, then I dug through that specific pile of boxes and found an ancient carbon monoxide detector that desperately wanted its batteries replaced. How it works When I started working through this problem with Claude it quickly dug up BeepBeep: A High-Accuracy Acoustic-Based System for Ranging and Localization Using COTS Devices, a 2007 paper by Chunyi Peng, Guobin Shen, Yongguang Zhang, Yanlin Li, and Kun Tan. Here''s that paper''s abstract: We present the design, implementation, and evaluation of BeepBeep, a high-accuracy acoustic-based ranging system. It operates in a spontaneous, ad-hoc, and device-to-device context without leveraging any pre-planned infrastructure. It is a pure software-based solution and uses only the most basic set of commodity hardware – a speaker, a microphone, and some form of device-to-device communication – so that it is readily applicable to many low-cost sensor platforms and to most commercial-off-the-shelf mobile devices like cell phones and PDAs. It achieves high accuracy through a combination of three techniques: two-way sensing, self-recording, and sample counting. That sounded exactly like what we needed to build! Beep Locator runs a Python web application server that does most of the work. The listening devices then connect to that server via WebSockets and present a UI which requests browser microphone access. The listeners mainly work by recording a 60 second buffer of recent audio. They also run a simple beep detection algorithm (see detector.js), which uses Fast Fourier Transform and a high-pass filter to identify potential beeps. It sends these events to the main server. When a beep is reported, the server has a device play a short chirp (the device with the lowest round-trip time to the server), then asks every listener upload a clip from about 0.6s before the beep to 0.8s after that chirp. This chirp allows the clips to be aligned with each other, a crucial step in locating the beep. How I built it I started this project using Claude Code Cloud via my iPhone, with Opus 5.5 running in Extra thinking effort mode. Here''s that shared starter session, which began with this prompt against a newly created empty GitHub repository: Write out a problem.md file describing the problem we want to solve There is a device beeping in a room and I cannot find it I want to set up several devices - laptops, phones, iPads - around the room all listening at the same time and have them help triangulate where the beep comes from There needs to be a Python web app which coordinates it all - one device shows a web page for the app, other devices join a URL which turns them into listeners - all via browser WebAudio and WebSockets back to the Python server app Ideally I wouldn''t need to manually input the relative locations of the devices - it would be amazing if they could play sounds to each other in order to locate each other in 2D space - is that even possible though? That got me this problem.md file. As part of writing that up Claude found and read the BeepBeep paper, which is described in that file. The research looked good, so I told it: Ok try and build it, add a mechanism so you can send new code to the listeners which they will then run automatically - that way I can set everything up in the room and then leave you alone to continue hacking and experiment while I do something else That got me to this point - most of the system was built, but nothing had actually tried running it yet. I cloned the new repository to my laptop and fired up Claude Code... and then Fable 5.2 was released, and this project seemed like an excellent opportunity to put that through its paces. I had Claude Code on my laptop start the server running, then made sure it could both see what was happening and make live changes to the software as it ran. This was already baked into the software: Opus 5.5 had built an installModule() method on the listeners that could be triggered over the WebSocket connection, meaning the admin server could execute arbitrary new code on any of its clients. On some devices, web pages that listen on the microphone request user permission any time the page reloads. The hot-swap module mechanism meant that we could deploy new code without having to manually interact with each of the listening devices in turn. From that point on my involvement was mainly to sit by the laptop chatting with Fable 5.2 about what was going on, and occasionally requesting new features. Here are some of the feature prompts I ran: I think we need to make it so I can drag things around on the map in “rough mode”, where my job is just to put them roughly where they are, then switch out of rough mode and it attempts to get them accurate This gave me the ability to drag listeners around the map. what’s a good way for me to test this I wonder? Can we have it listen for claps, then I could clap and it could identify the location of the clap? [...] Claps should use a different symbol on the map than beeps That added the thing where any time I clapped it would show on the map as a different symbol from the beeps. each symbol on the map should include a HH:MM:SS timestamp That made it easier to spot which symbol likely reflected which real-world sound. The log should show beeps and claps with their timestamps AND have a little play button which, when clicked, replays the recorded noise from each of the listeners in sequence (with clear text on screen showing which listener recorded which snippet) while temporarily turning off microphone recording while that replay plays Now I could replay sounds, making it much easier to identify which recorded clip was the mysterious beep I was looking for. There should be a button next to a recording for “This is the mystery beep” - then I can hit replay to see and then confirm it was the mystery beep if that’s the one I am looking for Then the system can categorize noises in the future to see if they are likely that same mysterious beep and use that information to build a more accurate idea of where it is At this point I was getting close - I had a solid idea of where the beep was coming from, but wouldn''t it be neat if I could reposition the listeners around that point to get an even finer location? I want a feature for this, I don’t want to have to restart each listening device etc, I want to be able to click a “Reposition the devices” button which lets me switch into repositioning, then I click “Repositioned” and it puts me into Rough mode and asks me to roughly locate them, then I do that and click, then it starts up again. The previous session can be read-only viewed through the app. And since I had one spare device, I then said: add a feature where the map includes a spot that is a suggestion for a location that it would be useful to add a listener at - based on data collected so far Giving Claude Code ears I think the most interesting part of this project is that I demonstrated it''s possible to give Claude Code ears. Claude doesn''t have native audio input (yet), but it can run JavaScript and Python code that can analyze audio captured by a microphone. I told it to run a web server with WebSocket connections to multiple listening devices, then had it both run a WebAudio microphone on those devices and hook them up so they could be sent fresh JavaScript code to execute via that WebSocket. Then I could pretty much leave Claude alone, hacking away at the problem, querying the audio logs, and reconfiguring its ears to help it solve the problem. I''m reminded of a similar hack I heard about at GitHub Universe a few weeks ago, where Martin Woodward told me he had been debugging the GitHub Universe Badge computers by plugging them into a computer via USB-C and then pointing a webcam at their screen, so his coding agent could tell if updates were working correctly or not. Figuring out ways to let coding agents perceive the world around them is a whole lot of fun. Tags: projects, ai, generative-ai, llms, claude, coding-agents, claude-code, claude-mythos-fable'
content: extracted
html: 2026-11-09-locating-the-source-of-a-mysterious-beep-with-claude.html
remote_preview:
  url: https://static.simonwillison.net/static/2026/beep-locator-card.jpg
---

Our house has a box room, full of boxes that we still haven’t unpacked from our last move. A couple of weeks ago something in that room started *intermittently beeping*. The beeps were far enough apart that standing in the room and trying to determine the source of the noise was frustrating, and I really didn’t want to dig through every one of those boxes. So I had Claude Code build [Beep Locator](https://github.com/simonw/beep-locator), a software system for triangulating the source of a mysterious beep using multiple devices scattered around the room.

Here’s what Beep Locator looks like after it has been running for a while:

![Screenshot of a web app that uses several phones, tablets and laptops as microphones to locate a mystery beeping sound in a room. The left column has an \"Add a listener\" panel with a QR code and join link, a \"Controls\" panel with buttons including \"Calibrate\", \"Test beep\" and \"It just beeped!\" plus a \"Listen for beeps\" checkbox, and a \"Mystery beep\" note reading \"3345 Hz · 3 confirmed · remembered across sessions\". Below that, a \"Devices\" list shows five color-coded listeners (Simon iPhone, New iPad, Old iPad, MacBook Pro, Surface Ultra), each marked \"✓ mic working · ✓ speaker working\". The main area is a gridded map with a 1 m scale bar that shows the five devices as colored dots and earlier sounds as small timestamped markers. A pink shaded circle near the MacBook Pro is labeled \"mystery beep · 2 hearings · within ~41 cm\", and beside it a dashed plus marker suggests \"add a listener here? could tighten the mystery beep's position by ~12%\". Below the map, a \"Mystery beep loudness warmer / colder\" panel shows a bar for each device: Surface Ultra is loudest at \"-6.0 dB ▲ +11.6 dB warmer\", with the note \"about 6 dB louder means roughly half the distance\". At the bottom, a \"Sounds located\" table lists recent claps and beeps with coordinates, or the warning \"arrival times are not physically possible (echo or a different sound?)\".](https://static.simonwillison.net/static/2026/beep-locator-ui.webp)

You run Python web server to show the primary UI, and then use the QR code or URL to connect as many devices as you can spare to act as listeners—I was using two iPads, a phone, a MacBook Pro and a Surface Ultra.

The devices then play noises to each other to help estimate their locations. This didn’t work particularly well, so I added a “Rough mode” where you can drag their icons around in the space to provide rough locations which are then adjusted based on those sounds.

Once that’s set up, the devices all listen for the beep!

I added a clapping mode too so I could test it by clapping my hands in different locations around the room—displayed as diamonds on the map. This helped me gain confidence that it was capable of locating a sound.

Beeps show up on the map as an X, plus a line in the “Sounds located” log. You can replay these sounds (from each device in turn) to confirm they are the mysterious beep you are looking for, then click “This is the mystery beep” to mark them as such.

Once you’ve confirmed the mysterious beep frequency the system will listen out for it, measure the interval between beeps, and use the triangulated locations to lock down the most likely location on the map.

The system worked as intended. It gave me the position of the beep to within about 40cm, then I dug through that specific pile of boxes and found an ancient carbon monoxide detector that desperately wanted its batteries replaced.

#### How it works

When I started working through this problem with Claude it quickly dug up [BeepBeep: A High-Accuracy Acoustic-Based System for Ranging and Localization Using COTS Devices](https://www.cs.purdue.edu/homes/chunyi/pubs/sensys106-beepbeep.pdf), a 2007 paper by Chunyi Peng, Guobin Shen, Yongguang Zhang, Yanlin Li, and Kun Tan. Here’s that paper’s abstract:

> We present the design, implementation, and evaluation of BeepBeep, a high-accuracy acoustic-based ranging system. It operates in a spontaneous, ad-hoc, and device-to-device context without leveraging any pre-planned infrastructure. It is a pure software-based solution and uses only the most basic set of commodity hardware – a speaker, a microphone, and some form of device-to-device communication – so that it is readily applicable to many low-cost sensor platforms and to most commercial-off-the-shelf mobile devices like cell phones and PDAs. It achieves high accuracy through a combination of three techniques: two-way sensing, self-recording, and sample counting.

That sounded exactly like what we needed to build!

Beep Locator runs a Python web application server that does most of the work. The listening devices then connect to that server via WebSockets and present a UI which requests browser microphone access. The listeners mainly work by recording a 60 second buffer of recent audio. They also run a simple beep detection algorithm (see [detector.js](https://github.com/simonw/beep-locator/blob/main/beep_locator/modules/listener/detector.js)), which uses Fast Fourier Transform and a high-pass filter to identify potential beeps. It sends these events to the main server.

When a beep is reported, the server has a device play a short chirp (the device with the lowest round-trip time to the server), then asks every listener upload a clip from about 0.6s before the beep to 0.8s after that chirp. This chirp allows the clips to be aligned with each other, a crucial step in locating the beep.

#### How I built it

I started this project using Claude Code Cloud via my iPhone, with Opus 5.5 running in Extra thinking effort mode. Here’s [that shared starter session](https://claude.ai/code/cshk_016BvfqAWf7MeGZKeYCrUnN2MmRfCSAX9Gu8DiptXMuH9P), which began with this prompt against a newly created empty GitHub repository:

> `Write out a problem.md file describing the problem we want to solve`
>
> `There is a device beeping in a room and I cannot find it`
>
> `I want to set up several devices - laptops, phones, iPads - around the room all listening at the same time and have them help triangulate where the beep comes from`
>
> `There needs to be a Python web app which coordinates it all - one device shows a web page for the app, other devices join a URL which turns them into listeners - all via browser WebAudio and WebSockets back to the Python server app`
>
> `Ideally I wouldn't need to manually input the relative locations of the devices - it would be amazing if they could play sounds to each other in order to locate each other in 2D space - is that even possible though?`

That got me [this problem.md file](https://github.com/simonw/beep-locator/blob/main/problem.md). As part of writing that up Claude found and read the BeepBeep paper, which is described in that file.

The research looked good, so I told it:

> `Ok try and build it, add a mechanism so you can send new code to the listeners which they will then run automatically - that way I can set everything up in the room and then leave you alone to continue hacking and experiment while I do something else`

That got me [to this point](https://github.com/simonw/beep-locator/commits/e36ac93a81091a246403c074e34c0041c6972fac)—most of the system was built, but nothing had actually tried running it yet.

I cloned the new repository to my laptop and fired up Claude Code... and then [Fable 5.2 was released](https://www.anthropic.com/claude-fable-and-mythos-5-2), and this project seemed like an excellent opportunity to put that through its paces.

I had Claude Code on my laptop start the server running, then made sure it could both see what was happening *and* make live changes to the software as it ran.

This was already baked into the software: Opus 5.5 had built an [installModule()](https://github.com/simonw/beep-locator/blob/44fea65c918fb31a7d2c0e17797ee1a106860e23/beep_locator/static/runtime.js#L710) method on the listeners that could be triggered over the WebSocket connection, meaning the admin server could execute arbitrary new code on any of its clients.

On some devices, web pages that listen on the microphone request user permission any time the page reloads. The hot-swap module mechanism meant that we could deploy new code without having to manually interact with each of the listening devices in turn.

From that point on my involvement was mainly to sit by the laptop chatting with Fable 5.2 about what was going on, and occasionally requesting new features. Here are some of the feature prompts I ran:

> `I think we need to make it so I can drag things around on the map in “rough mode”, where my job is just to put them roughly where they are, then switch out of rough mode and it attempts to get them accurate`

This gave me the ability to drag listeners around the map.

> `what’s a good way for me to test this I wonder? Can we have it listen for claps, then I could clap and it could identify the location of the clap? [...]`
>
> `Claps should use a different symbol on the map than beeps`

That added the thing where any time I clapped it would show on the map as a different symbol from the beeps.

> `each symbol on the map should include a HH:MM:SS timestamp`

That made it easier to spot which symbol likely reflected which real-world sound.

> `The log should show beeps and claps with their timestamps AND have a little play button which, when clicked, replays the recorded noise from each of the listeners in sequence (with clear text on screen showing which listener recorded which snippet) while temporarily turning off microphone recording while that replay plays`

Now I could replay sounds, making it much easier to identify which recorded clip was the mysterious beep I was looking for.

> `There should be a button next to a recording for “This is the mystery beep” - then I can hit replay to see and then confirm it was the mystery beep if that’s the one I am looking for`
>
> `Then the system can categorize noises in the future to see if they are likely that same mysterious beep and use that information to build a more accurate idea of where it is`

At this point I was getting close—I had a solid idea of where the beep was coming from, but wouldn’t it be neat if I could reposition the listeners around that point to get an even finer location?

> `I want a feature for this, I don’t want to have to restart each listening device etc, I want to be able to click a “Reposition the devices” button which lets me switch into repositioning, then I click “Repositioned” and it puts me into Rough mode and asks me to roughly locate them, then I do that and click, then it starts up again. The previous session can be read-only viewed through the app.`

And since I had one spare device, I then said:

> `add a feature where the map includes a spot that is a suggestion for a location that it would be useful to add a listener at - based on data collected so far`

#### Giving Claude Code ears

I think the most interesting part of this project is that I demonstrated it’s possible to *give Claude Code ears*.

Claude doesn’t have native audio input (yet), but it can run JavaScript and Python code that can analyze audio captured by a microphone.

I told it to run a web server with WebSocket connections to multiple listening devices, then had it both run a WebAudio microphone on those devices and hook them up so they could be sent fresh JavaScript code to execute via that WebSocket.

Then I could pretty much leave Claude alone, hacking away at the problem, querying the audio logs, and reconfiguring its ears to help it solve the problem.

I’m reminded of a similar hack I heard about at GitHub Universe a few weeks ago, where [Martin Woodward](https://github.com/martinwoodward) told me he had been debugging the [GitHub Universe Badge computers](https://simonwillison.net/2026/Oct/27/game-boy-color-on-my-conference-badge/) by plugging them into a computer via USB-C and then pointing a webcam at their screen, so his coding agent could tell if updates were working correctly or not.

Figuring out ways to let coding agents perceive the world around them is a whole lot of fun.
