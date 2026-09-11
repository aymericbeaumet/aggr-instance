---
title: I spent $220 on Google app ads and 60% of the installs were robots
link: https://dayzlegame.com/blog/google-ads-bot-farm/
source: hnrss-org-frontpage
published: 2026-09-11T18:24:55Z
updated: 2026-09-11T18:24:55Z
first_seen: 2026-09-11T23:11:51.018809841Z
authors:
- nickabe
summary: 'Article URL: https://dayzlegame.com/blog/google-ads-bot-farm/ Comments URL: https://news.ycombinator.com/item?id=49662990 Points: 185 # Comments: 89'
content: extracted
html: 2026-09-11-i-spent-220-on-google-app-ads-and-60-of-the-installs-were.html
preview:
  file: 2026-09-11-i-spent-220-on-google-app-ads-and-60-of-the-installs-were.preview-7ee70ca29035.webp
  width: 256
  height: 134
  alt: Dayzle — five calm puzzles, every day
  color: '#d1d6d1'
images:
- source: https://dayzlegame.com/og.png
  original:
    file: 2026-09-11-i-spent-220-on-google-app-ads-and-60-of-the-installs-were.image-91ac2bf8dfc2.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-11-i-spent-220-on-google-app-ads-and-60-of-the-installs-were.image-9e3c3f1a5d4d.webp
    width: 48
    height: 25
  - file: 2026-09-11-i-spent-220-on-google-app-ads-and-60-of-the-installs-were.image-f6399fac2a9c.webp
    width: 320
    height: 168
  - file: 2026-09-11-i-spent-220-on-google-app-ads-and-60-of-the-installs-were.image-597f8a6fb814.webp
    width: 640
    height: 336
  - file: 2026-09-11-i-spent-220-on-google-app-ads-and-60-of-the-installs-were.image-437330ac6668.webp
    width: 960
    height: 504
  - file: 2026-09-11-i-spent-220-on-google-app-ads-and-60-of-the-installs-were.image-07344d505b14.webp
    width: 1200
    height: 630
  color: '#e9ece8'
---

I run a small puzzle app called Dayzle. I’m a numbers guy, so a marketing optimization problem is right up my alley. Two weeks ago I turned on a Google Ads campaign for Android at CA$40 a day, with the goal set to installs.

For the first few days it barely spent anything. I had a target cost per install of $1.50, and Google couldn’t find installs at that price. So, as a test, I removed the target. It immediately spent double my daily budget, CA$80, and reported 21 installs. I was excited. Then I checked my admin panel, which said 1. Turns out I didn’t need to be a numbers guy to see something didn’t add up.

The panel had missed them because old versions of the app don’t report an install date. When I went into the raw analytics there were 21 new Android devices that day, and 20 of them were running an old version of the app that the Play Store had stopped serving days earlier. You can’t get an old version from Play, so these phones got the app from somewhere else, even though every one of them said Google Play was the installer. Each opened the app once, spent zero seconds on any screen, and never came back. Twenty-eight phone models across nineteen states, which is a lot of variety for twenty phones that all did exactly the same thing.

Over the whole two weeks: 56 installs billed, 33 with that pattern, 7 more from countries the campaign wasn’t targeting, and 13 people. The 13 people finished 92 games between them, which is a nice signal that real people enjoyed what we’ve built.

The 33 weren’t behaving like people, so I suspected a bot farm, and the analytics export bears it out. Google optimizes for whatever goal you give it, and my goal was installs. This farm would watch the shortest video in my ad group, not click it, and then install our app from a saved copy of the file instead of from the store, because that’s faster and Play might notice. Google counts a view followed by an install as a conversion, so the irony is that the more the farm “installed” our app, the better it looked to Google’s algorithm, which sent more of my ads to the farm, which installed it more. A loop that guaranteed my ad spend was wasted.

Where I am now: waiting on Google’s answer to the invalid-traffic form, and the campaign’s goal is now “won a puzzle” instead of “opened the app”. It’s low effort to make a script open an app and click around; it’s higher effort to make one solve a Sudoku. The idea is just to make us more expensive to farm than the next app. That’s probably decent protection for an app my size. Larger apps are worth the extra effort, and I’d guess they see a lot more of this than they know. I’ll report back on the refund.

So I guess this is my PSA: if you’re relying on Google’s install count for your ads, it’s a real number, but it’s definitely worth digging into. If a bot farm can find my tiny ad budget, it can definitely find yours.
