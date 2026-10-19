---
title: The people holding up the internet
link: https://sheets.works/data-viz/holding-up-the-internet
source: lobste-rs
published: 2026-10-08T08:18:39Z
updated: 2026-10-08T08:18:39Z
first_seen: 2026-10-08T23:55:34.578128200Z
authors:
- sheets.works via eaj
labels:
- historical
- web
summary: Comments
content: extracted
html: 2026-10-08-the-people-holding-up-the-internet.html
---

Billions of phones run on code looked after by a handful of people. We counted them from the code itself.

[Read it as a plain page](https://sheets.works/data-viz/holding-up-the-internet/read)

Your alarm, a boarding pass, a meeting in another country, the day your salary lands: anything on your phone that shows a time has to know where you are. Underneath sit every Android phone, every iPhone and most of the servers on the internet. They all get local time from one text file, the time zone database, which holds every clock rule a government has ever announced. Paul Eggert, a lecturer at UCLA, keeps that file up to date in his spare time.

![Paul Eggert on a video call, wearing headphones, at home](https://sheets.works/assets/holding-up-the-internet/img/eggert.png)\
Paul Eggert giving a talk on the time zone database, LibrePlanet 2022. Free Software Foundation, CC BY-SA 3.0.

## Paul Eggert

Teaches computer science at UCLA, and has been the official coordinator of the time zone file since 2012.

4,000,000,000

Android phones and iPhones use his file. We didn't count servers, Macs or laptops, which would push the number higher.

Your phone's time zone, as the file has it today

On 29 September Paul put out version 2026e of the file. The first line of its notes says that Manitoba moves to permanent -05 on 31 October, which tells phones in Winnipeg not to set their clocks back on 1 November, and it was typed in by two volunteers a month ahead. Of the 251 changes made to the file in the last year, 218 were his and 28 were Tim Parenti's.

In 2011 an astrology software company sued Paul and Arthur Olson, who started the file in 1986 at the National Institutes of Health, claiming some of its history came from an atlas the company owned. The file's mailing list and download site were switched off until IANA, which keeps the internet's master lists, took them over later that month. The Electronic Frontier Foundation defended them for free, and the company dropped the case in February 2012.

> “In case I am no longer available due to retirement or whatever.”
>
> Paul Eggert, asking the tz mailing list to make Tim Parenti his backup, 20 May 2020

I also make a daily word game. [Play today’s sentence](https://sheets.works/long-story-short?r=hold_eggert&utm_source=holding-up-the-internet&utm_medium=drop&utm_content=eggert). About two minutes.

[![xkcd 2347, Dependency: a tower of blocks labelled all modern digital infrastructure, resting on one small block labelled a project some random person in Nebraska has been thanklessly maintaining since 2003](https://imgs.xkcd.com/comics/dependency.png)](https://xkcd.com/2347/)\
[Dependency](https://xkcd.com/2347/), Randall Munroe, xkcd 2347. CC BY-NC 2.5, colours inverted.

People post this comic whenever something breaks. The tower is labelled "all modern digital infrastructure" and it stands on one small block, "a project some random person in Nebraska has been thanklessly maintaining since 2003".

We wanted to know how close to true it is. We downloaded the full history of 23 pieces of software that phones, browsers and servers depend on, and counted everyone who made ten or more changes to each one between October 2025 and October 2026. Then we listed what is actually installed on an Android phone, an iPhone and a Windows PC.

11 of 23

projects had one or two people doing the regular work.

> “It's also good to keep in mind that this is an unpaid hobby project.”
>
> Lasse Collin, xz mailing list, 8 June 2022

## Lasse Collin

Lives in Finland and looks after xz, the compression tool on almost every Linux server and inside every iPhone.

He wrote that line in the same email where he said his ability to care had been "fairly limited mostly due to longterm mental health issues". For months, accounts calling themselves Jigar Kumar and Dennis Ens had been posting to the list about how slowly things were moving, and a contributor called Jia Tan had been sending useful fixes.

Lasse gave Jia Tan more access. In 2023 Jia Tan made 304 changes to xz and Lasse made 172, which is the red on the wall below. In February 2024 the versions Jia Tan released carried a hidden way into Linux servers. Andres Freund, an engineer at Microsoft, found it on 29 March because his logins were using more processor time than they should, before most Linux systems had shipped it. Nobody has found out who Jia Tan is.

Lasse Collin Jia Tan everyone else

Every change to xz since January 2021, one square each. In 2021 almost all of them are Lasse's. 28 January 2022: the first change written by Jia Tan goes in. By 2023 Jia Tan is making more changes than Lasse, 304 to his 172. February 2024: Jia Tan releases two versions with the backdoor inside. Andres Freund finds it on 29 March. After that it is Lasse on his own again, and in 2025 he wrote 97 percent of the changes.

$57 a month

was what Denis Pushkarev raised when he asked for donations to keep core-js going.

## Denis Pushkarev

Wrote core-js, which lets new JavaScript work in old browsers. By his own count it runs on about half of the thousand busiest websites in the world.

In 2019 he was working on core-js full time without pay, living in Russia because it was cheaper. By his own account, three weeks after a big release he was driving home at 3 a.m. when two young women on a dark road ended up under his car, and one of them died. The families asked for about $80,000, and the prosecutor asked for seven years.

He added a message that appeared in the terminal every time anyone installed core-js, asking for help or a job. Millions of developers saw it, and many of them complained about the message. He went to prison in January 2020 and was released early, about ten months later.

Denis Pushkarev everyone else

Every change to core-js, week by week, from June 2019. Nearly all of them are his. December 2019: 101 changes in a month. In January 2020 he goes to prison, and the weeks after that stay almost empty. October 2020: he is out and back at work. Downloads nearly doubled while he was inside. This year he wrote 95 percent of the changes, and he has two sponsors on GitHub.

5,408 of 5,409

changes to sudo between 2008 and 2018 were made by Todd Miller.

> “I'm currently in search of a sponsor to fund continued sudo maintenance and development.”
>
> Todd C. Miller, millert.dev, February 2026

## Todd Miller

Has maintained sudo, the command that gives you admin rights on a Mac or a Linux server, since the early 1990s.

Sudo was first written around 1980 at SUNY Buffalo, and Todd has looked after it for longer than Linux has existed. After The Register wrote about his note in February, the project's Open Collective budget reached about $61,700 a year and 30 people sponsored it on GitHub, which makes sudo the best funded one-person project in our count.

Text inside libjpeg.so, read off an Android 16 phone

Copyright (C) 1991-2022 The libjpeg-turbo Project and many others

## DRC

Runs libjpeg-turbo, which opens every JPEG on Android phones and in Chrome and Edge, and signs his emails with his initials.

He started it in 2010 by taking the JPEG code from the 1990s and making it two to six times faster, and in 2019 it became the reference version of JPEG for the ISO and the ITU. He runs it on his own as a small business, which the project describes as "sustained solely through patronage and funded development". At one point he wrote that it had general funding "for about 8-10 hours of labor per month". This year he wrote 98 percent of the changes.

A B C D

## The SQLite team

Four people changed SQLite in the last year. It is in every Android phone, every iPhone, every Mac, every copy of Windows 10 and 11 and every major browser.

> “There are a lot of crazy people in the world who might misuse that information.”
>
> sqlite.org, on why the page with the developers' names and photos was taken down

D. Richard Hipp wrote SQLite in 2000 after working on software for a US Navy destroyer, where the database kept failing whenever a separate server went down, so he made one that lives in a single file. When you open WhatsApp, your chats load from one. The project's own estimate is over a trillion SQLite databases in use, and the team pays for the work by selling support through Hipp's company.

If you want two minutes away from the squares, [play today’s Long Story Short](https://sheets.works/long-story-short?r=hold_sqlite&utm_source=holding-up-the-internet&utm_medium=drop&utm_content=sqlite), one true story cut to five of its own words.

![Mark Adler at the Jet Propulsion Laboratory, wearing a lab lanyard](https://sheets.works/assets/holding-up-the-internet/img/adler.jpg)\
Mark Adler at the Jet Propulsion Laboratory, 2002. NASA, public domain.

## Mark Adler

Wrote zlib with Jean-loup Gailly in 1995. In his other job he managed NASA's Spirit rover on its way to Mars.

zlib makes files smaller. It is inside PNG images, web pages, Git, Android, iPhones and Chrome, and Debian counts it on 291,615 of the machines that report to it, effectively every one. In the last year two people did most of the work, Mark and a contributor who goes by Vollstrecker. Mark has no sponsor page, and zlib is not on any public funding list we checked.

25 years

between the line that became Shellshock going into bash and anyone reporting it.

## Chet Ramey

Has maintained bash, the shell on Linux and on Macs from 2003 to 2019, since about 1990, alongside his job in the network group at Case Western Reserve University in Ohio.

In September 2014 Stéphane Chazelas reported a bug in bash to Chet. Anyone could use it to run commands on a server by sending it a specially shaped piece of text, and on 24 September it went public as Shellshock, on hundreds of millions of machines. The line behind it had gone into bash on 5 August 1989. Every change in bash's public history is Chet's, including the official fixes.

![Behdad Esfahbod, black and white portrait](https://sheets.works/assets/holding-up-the-internet/img/behdad.jpg)\
Behdad Esfahbod, 2016. CC0.

## Behdad Esfahbod

Rewrote HarfBuzz, which decides how letters join and sit for Android, Chrome, Firefox, Edge, the Kindle and Figma.

हिन्दी

with HarfBuzz

the same letters, unshaped

مرحبا

with HarfBuzz

the same letters, unshaped

For English the job is mostly simple. In Hindi, Arabic, Tamil and most of the world's writing, letters change shape depending on their neighbours, and without shaping a word comes out as a row of pieces. Behdad started the rewrite around 2012 while he was at Google. This year he wrote 85 percent of the changes, with five other people doing regular work.

![Daniel Stenberg, portrait](https://sheets.works/assets/holding-up-the-internet/img/stenberg.jpg)\
Daniel Stenberg, 2015. Photo by Daniel Stenberg, CC BY 4.0.

## Daniel Stenberg

Started curl in Sweden in 1996. It moves data for phones, cars, TVs and Windows, which has shipped it since 2018.

This year eleven people did regular work on curl. Viktor Szakats made the most changes, then Daniel, then Stefan Eissing with 445. In his review of 2025 Daniel wrote that everyone else has now added more lines to curl than he has. He works on it full time because companies pay for support, the project takes in about $89,700 a year through Open Collective, and Germany's public fund for open source paid €195,000 for work on it.

Every change to curl in the last twelve months, by week, one colour per person who made ten or more. Hover a square to read the change.

### Heartbleed

OpenSSL puts the padlock on a large share of the web. In April 2014 a bug called Heartbleed let anyone read passwords and private keys out of the memory of about 17 percent of trusted secure servers, by Netcraft's count. That week the OpenSSL foundation's president, Steve Marquess, wrote that it received about $2,000 a year in donations, and he told NPR that one person worked on it full time.

Within two months the Linux Foundation raised $5.4 million from technology companies, and OpenSSL got two paid developers and an audit. It had six people doing regular work in 2013 and fourteen in 2014.

People who made ten or more changes that year OpenSSL xz

Before 2014 OpenSSL has three to seven people a year doing regular work, and xz has one. After Heartbleed in April 2014 the money arrives, and OpenSSL goes from six people to fourteen. March 2024: the xz backdoor is found, and no new money follows that we could find. In 2026 OpenSSL has 32 people doing regular work, and xz has one.

### The money

Germany's Sovereign Tech Agency has funded about ninety open source projects since 2022, and the Alpha-Omega fund gave out nearly $6 million last year, much of it to security engineers at foundations like Python's and Ruby's. Both give money to organisations that can apply for it and report on it.

Funded by the Sovereign Tech Agency

log4j€**596,160**

FFmpeg€**437,930**

OpenSSL€**405,888**

OpenSSH€**200,000**

curl€**195,000**

No public grant we could find

The time zone database€**0**

SQLite€**0**

zlib€**0**

libjpeg-turbo€**0**

HarfBuzz€**0**

xz€**0**

bash€**0**

nghttp2€**0**

Some projects found money elsewhere. Nick Wellnhofer raised a low six-figure sum over the ten years he maintained libxml2, and since August 2026 the City of Munich has been paying Sebastian Pipping to work on expat for up to six months. On GitHub Sponsors, Daniel Stenberg has 64 sponsors. Paul Eggert, Lasse Collin, DRC and Mark Adler do not have a sponsor page.

From libxml2's README, until December 2025

“This is open-source software written by hobbyists, maintained by a single volunteer, badly tested, written in a memory-unsafe language and full of security bugs.”

libxml2 is on 5.6 billion phones and computers.

libxml2 reads XML, the format behind a lot of documents, feeds and settings, and it is on Android phones, iPhones and in Chrome. Nick Wellnhofer maintained it for about ten years. In September 2025 he announced that he was stepping down, kept fixing regressions until he took himself off the maintainers list in December, and about twelve hours later new maintainers were added. Daniel Garcia Moreno has done most of the work since.

Nick Wellnhofer Daniel Garcia Moreno everyone else

Every change to libxml2 from June 2025, week by week. September 2025: Nick Wellnhofer announces he is stepping down, and keeps fixing regressions until December. December: Nick takes himself off the maintainers list, new maintainers are added within a day, and Daniel Garcia Moreno takes over the work. libxml2 is on 5.6 billion phones and computers.

The first line of the notes for version 2026e of the file, released by Paul Eggert on 29 September 2026

Our daily game

### Long Story Short

Every day there is one true news story, and you cut its sentence down to five of its own words. It takes about two minutes.

[Play today’s](https://sheets.works/long-story-short?r=hold_plug&utm_source=holding-up-the-internet&utm_medium=drop&utm_content=plug)

### Who is inside the device you are holding

We guessed your device from your browser. Nothing is sent anywhere.

### How we know

Each dot is one project. Across: phones and computers it ships on by default, a lower bound. Up: people who made ten or more changes in the last twelve months. Hover a dot.

We took the full public history of each project and kept the changes written between 7 October 2025 and 7 October 2026, leaving out merges and bots. A person counts if they made ten or more changes in that time. A change's author is not always the maintainer, and some of these projects publish their history as a copy of another system.

For devices we counted a project on a platform only when we could see it there. On Android we listed the system libraries of an Android 16 image and read inside them, which is how we found that Android's "liblzma" is 7-Zip's code and not xz. For the iPhone we read the system libraries listed in Apple's iOS 26.2 developer kit. For Windows we used Microsoft's announcement for curl and the code inside Edge, which comes with every copy. Android has over three billion active devices (Google, 2022), iPhones over one billion (Apple, 2021) and Windows 1.6 billion a month (Microsoft, 2026). Macs, iPads, servers, cars and TVs are left out, so every device number here is lower than the real one.

"No public grant" means none from the Sovereign Tech Agency, Alpha-Omega, Open Collective or GitHub Sponsors, not that nobody has ever paid these people. If we got your project wrong, tell us and we will fix it.

Data: [the 23-project table](https://sheets.works/assets/holding-up-the-internet/data/dataset_v2.json) · [every change shown](https://sheets.works/assets/holding-up-the-internet/data/walls.json) · [time zone lines with who last changed them](https://sheets.works/assets/holding-up-the-internet/data/zones.json) · [the script](https://sheets.works/assets/holding-up-the-internet/data/build_data.py).\
Sources: tz mailing list and RFC 6557; IANA tzdb NEWS 2026e; EFF, Astrolabe v. Olson; xz-devel, 8 June 2022; Russ Cox's xz timeline; Andres Freund on oss-security, 29 March 2024; Denis Pushkarev, "So, what's next?" (2023); millert.dev and The Register, 3 February 2026; libjpeg-turbo.org; sqlite.org/mostdeployed and /crew; Wikipedia on Mark Adler; David A. Wheeler's Shellshock timeline; HarfBuzz README; daniel.haxx.se, 2025 review; Steve Marquess, "Of Money, Responsibility, and Pride" (2014); Netcraft, 7 April 2014; sovereign.tech; Alpha-Omega 2025 report; GNOME Discourse, 15 September 2025; libxml2's commit history and Nick Wellnhofer's corrections (October 2026); Sebastian Pipping, "libexpat now funded by the City of Munich", 4 August 2026.\
Photos: Paul Eggert, FSF / LibrePlanet 2022, CC BY-SA 3.0. Mark Adler, NASA, public domain. Behdad Esfahbod, CC0. Daniel Stenberg, CC BY 4.0. Comic: xkcd 2347, CC BY-NC 2.5.
