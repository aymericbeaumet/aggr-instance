---
title: Why I'm donating $150/month (10% of my income) to the musl libc project
link: https://andrewkelley.me/post/why-donating-to-musl-libc-project.html
source: andrewkelley-me
published: 2019-06-24T20:15:06Z
updated: 2019-06-24T20:15:06Z
first_seen: 2026-09-19T21:30:23.395188495Z
summary: 'Why I''m donating $150/month (10% of my income) to the musl libc project One year ago, I quit my day job to work on Zig full time. Since then, the project has seen a steady growth in funding. Thanks to the people donating, Zig is on track to become fully sustainable before my savings run out. This support has allowed me to focus on steady improvements to the language and tooling. In the last year, Zig has released two versions: 0.3.0 (release notes) 0.4.0 (release notes) These release notes serve as my accountability to people who donate, and if you have a look at them, I hope you can agree with me that they speak for themselves. The V language drama I know that open-source project funding has been on people''s minds lately, as we''ve watched the Internet''s reaction to the bizarre open-sourcing of the V language unfold. As to how this relates to musl libc, stick with me - let me give you the talking points of this situation: The V language website was published several months ago, claiming that it could already do incredible things, such as automatically translate C++ code to V code, and compile 1.2 million lines of code per second per CPU core by directly outputting x64 machine code. These features already work today, it said, and they will be released soon. Please donate. Meanwhile, the project was closed-source. Binaries and release notes were behind a paywall. Over the next few months, people saw these amazing things it was claimed that V could do, and started donating money. It even looked like the language was already available, with download links including file sizes and file names, but if you clicked the download link, it did an alert() pop-up box saying "June 20". The project climbed up to $927/month in donations. On 2019-06-20, the V author released a macOS binary of the V language, and it immediately became apparent that the many fantastic claims on the V website were unsubstantiated. People were quick to point this out, including myself. In response to the backlash, the V author redacted the macOS binary, and said that he would do a full source release in 2 days. On 2019-06-22, he updated the V language website with "WIP" labels to point out which features of the project are not available, and within an hour after that, published the V project source code. At this point, I made the following statement: Now that the website has the "WIP" label to communicate which features are not available, and the source is released, I no longer consider this project to be fraudulent. The information is available to everyone, and people who donate on Patreon are making an informed choice. I''m genuinely glad it turned out this way. Good luck on your endeavor, and welcome to the programming languages club. At this point, the Internet discovered the source release. Shitposters, trolls, and genuine people all started paying attention to the project at once. Most people were unaware that the V website had been modified to retract the false claims, which is quite understandable given that they had been there just yesterday. Now that it was crystal clear that the V project had been intentionally misleading, the backlash intensified. In 2 days, V lost $100/month in donations. Finally, there was a backlash on the backlash. To some people, $927/month was "table scraps". From this perspective, people were piling on a hate-train over a paltry sum of cash. Maybe other open-source projects should take a hint from V and do a little bit more marketing. Donations to open-source projects are not a zero sum game. One thing that is crystal clear is that the V author succeeded in creating hype. He got people excited about ambitious features - so excited that they were willing to donate some cash. This made me think of another open source project in the opposite category. musl libc: a project with no hype but huge impact musl libc is an alternative to GNU libc for Linux, created by Rich Felker, and with a healthy community of high-quality contributors. It''s been around for years, yet making less than V in donations. The Zig project owes a lot to musl, for many reasons: Mentorship Especially in the early stages of the Zig project, but still to this day, I asked question after question in the #musl IRC channel, and the musl community patiently and expertly indulged them. Sometimes my questions were not even musl-related, but just more about how the Linux kernel works. I didn''t start off as an expert systems programmer when I started Zig, but the musl community has mentored me over the years. Zig bundles musl Zig ships with musl source code. This allows Zig projects to cross compile for Linux. For example, on Windows, you can: > zig.exe build-exe --c-source hello.c -target x86_64-linux-musl Copy the resulting ELF binary to a Linux machine and it will run. (Or run that in the Windows Subsystem for Linux). This works because Zig lazily builds musl from source on demand, for the selected target. It is in no small part thanks to musl''s simplicity and well-designed nature that this is possible. It''s also useful to create static builds on Linux where the native libc is glibc. Much of the Zig standard library is ported from musl musl is such a high quality codebase, that most of the Zig standard library''s interface to Linux is a direct port of musl code. This has prevented countless bugs and made things "just work" in general. Without this head start, the Zig project would have had to spend more time on Linux system interface and less on everything else. For example, thanks to Marc Tiehuis contributions, the Zig standard library has all the math functions you would expect to find in libm, and they are available at compile-time as well as runtime. Float literal parsing adapted and ported from musl Zig has 128-bit floating point literals, so that compile-time computations can be done in a higher level of precision, before being casted to the floating point type of choice. This is tricky to implement in the C++ non-self-hosted compiler, however, because libc does not have a 128-bit strtod function. So I took musl''s strtold function, which works with the type long double, and then ported the code into Zig, making all the #ifdefs assume that long double is 128-bits (which is only true on some architectures), and replacing all the math with SoftFloat so that it would work on any architecture, no matter what long double maps to. Alpine Linux is based on musl Many people are aware of Alpine Linux because it has become a popular Linux distribution to use with Docker, mainly due to its simplicity and small binary size. This is in large part thanks to the fact that the system uses musl as its libc rather than glibc. The Zig project uses Alpine Linux to create the static Linux builds on ziglang.org/download. And so I have decided to donate $150/month to the musl project, even though that represents 10% of my income. I''m putting my money where my mouth is. But there''s more - please read on... This is a marketing stunt Now if I''m being honest about my motivations for this blog post, it''s that I want to prove that open source funding is not a zero-sum game. If there''s anything we''ve learned from the V language Internet drama that has unfolded over the past few days, it''s that open source projects have to do marketing if they want to get financial support. But I want to set a better example of what that might look like. I don''t think you have to trick people. This blog post is a marketing stunt intended to show that you can do things to get attention and funding, without being dishonest. Will you help me prove my point? If enough people start pledging donations to Zig when reading this, and the $150/month is recovered, then this little stunt will have been a proof-of-concept of one open source project running a fund-raiser for another one. (To be clear my donations to musl are not conditional; regardless of the outcome I will continue to donate.) Worth a shot, right? Since making news headlines, several generous people have taken me up on this experiment: Marko Mikulicic - $4 Tristan Hume - $5 Christine Dodrill - $15 komu - $1 Dylan Baker - $3 Dave Voutila - $6.25 Tyler Philbrick - $3 Ben - $1 Tanner Schultz - $3 Martin DeMello - $5 Thomas Ballinger - $2 Steven Branson - $5 Zachary Feldman - $10 Dan Gallagher - $10 Quetzal Bradley - $10 Brian Orr - $5 YVT - $5 Dexter Haslem - $5 Wes - $2.40 Aeron Avery - $10 Carsten Dreesbach - $5 Ville Tuulos - $5 Curtis Fenner - $3 Nathan Sculli - $5 Alec Nunn - $5 Gurpreet Singh - $5 Haze Booth - $20 Don Harris - $20 That''s a total of 178.65, well beyond the $150 that I started donating to musl. 100% Thank you so much! I''m really grateful for all your support. This experiment was a success! Today, the Zig project ran a fundraiser for the musl libc project, and it worked!'
content: extracted
html: 2019-06-24-why-i-m-donating-150-month-10-of-my-income-to-the-musl-libc.html
---

[Andrew Kelley](https://andrewkelley.me/) - Why I'm donating $150/month (10% of my income) to the musl libc project (2019 Jun 24)

One year ago, [I quit my day job to work on Zig full time](https://andrewkelley.me/post/full-time-zig.html). Since then, the project has seen a steady growth in funding. Thanks to the people donating, Zig is on track to become fully sustainable before my savings run out.

This support has allowed me to focus on steady improvements to the language and tooling. In the last year, Zig has released two versions:

- 0.3.0 ([release notes](https://ziglang.org/download/0.3.0/release-notes.html))
- 0.4.0 ([release notes](https://ziglang.org/download/0.4.0/release-notes.html))

These release notes serve as my accountability to people who donate, and if you have a look at them, I hope you can agree with me that they speak for themselves.

## The V language drama

I know that open-source project funding has been on people's minds lately, as we've watched the Internet's reaction to the bizarre open-sourcing of the V language unfold. As to how this relates to musl libc, stick with me - let me give you the talking points of this situation:

1. The V language website was published several months ago, claiming that it could already do incredible things, such as automatically translate C++ code to V code, and compile 1.2 million lines of code per second per CPU core by directly outputting x64 machine code. These features already work today, it said, and they will be released soon. Please donate. Meanwhile, the project was closed-source. Binaries and release notes were behind a paywall.
2. Over the next few months, people saw these amazing things it was claimed that V could do, and started donating money. It even looked like the language was already available, with download links including file sizes and file names, but if you clicked the download link, it did an alert() pop-up box saying "June 20". The project climbed up to $927/month in donations.
3. On 2019-06-20, the V author released a macOS binary of the V language, and it immediately became apparent that the many fantastic claims on the V website were unsubstantiated. People were quick to point this out, [including myself](https://news.ycombinator.com/item?id=20230351).
4. In response to the backlash, the V author redacted the macOS binary, and said that he would do a full source release in 2 days.
5. On 2019-06-22, he updated the V language website with "WIP" labels to point out which features of the project are not available, and within an hour after that, published the V project source code. At this point, I made the [following statement](https://github.com/vlang/v/issues/292#issuecomment-504689104):

   > Now that the website has the "WIP" label to communicate which features are not available, and the source is released, I no longer consider this project to be fraudulent. The information is available to everyone, and people who donate on Patreon are making an informed choice.
   >
   > I'm genuinely glad it turned out this way. Good luck on your endeavor, and welcome to the programming languages club.

6. At this point, the Internet discovered the source release. [Shitposters, trolls, and genuine people all started paying attention to the project at once](http://web.archive.org/web/20190623042334/https://github.com/vlang/v/issues/319). Most people were unaware that the V website had been modified to retract the false claims, which is quite understandable given that they had been there just yesterday. Now that it was crystal clear that the V project had been intentionally misleading, the backlash intensified. In 2 days, V lost $100/month in donations.
7. Finally, there was a backlash on the backlash. To some people, $927/month was "table scraps". From this perspective, people were piling on a hate-train over a paltry sum of cash. Maybe other open-source projects should take a hint from V and do a little bit more marketing. Donations to open-source projects are not a [zero sum game](https://en.wikipedia.org/wiki/Zero-sum_game).

One thing that is crystal clear is that the V author succeeded in creating hype. He got people excited about ambitious features - so excited that they were willing to donate some cash. This [made me think](https://lobste.rs/s/rh1pbo/v_source_code_released#c_jyakpy) of another open source project in the opposite category.

## musl libc: a project with no hype but huge impact

[musl libc](https://www.musl-libc.org/) is an alternative to GNU libc for Linux, created by [Rich Felker](http://ewontfix.com/), and with a healthy community of high-quality contributors. It's been around for years, yet making less than V in donations.

The Zig project owes a lot to musl, for many reasons:

### Mentorship

Especially in the early stages of the Zig project, but still to this day, I asked question after question in the `#musl` IRC channel, and the musl community patiently and *expertly* indulged them. Sometimes my questions were not even musl-related, but just more about how the Linux kernel works.

I didn't start off as an expert systems programmer when I started Zig, but the musl community has mentored me over the years.

### Zig bundles musl

Zig ships with musl source code. This allows Zig projects to cross compile for Linux. For example, on Windows, you can:

```
> zig.exe build-exe --c-source hello.c -target x86_64-linux-musl
```

Copy the resulting ELF binary to a Linux machine and it will run. (Or run that in the Windows Subsystem for Linux).

This works because Zig lazily builds musl from source on demand, for the selected target. It is in no small part thanks to musl's simplicity and well-designed nature that this is possible.

It's also useful to create static builds on Linux where the native libc is glibc.

### Much of the Zig standard library is ported from musl

musl is such a high quality codebase, that most of the Zig standard library's interface to Linux is a direct port of musl code.

This has prevented countless bugs and made things "just work" in general. Without this head start, the Zig project would have had to spend more time on Linux system interface and less on everything else.

For example, thanks to [Marc Tiehuis](https://tiehu.is/) contributions, the Zig standard library has all the math functions you would expect to find in libm, and they are available [at compile-time](https://ziglang.org/documentation/master/#Introducing-the-Compile-Time-Concept) as well as runtime.

### Float literal parsing adapted and ported from musl

Zig has 128-bit floating point literals, so that compile-time computations can be done in a higher level of precision, before being casted to the floating point type of choice.

This is tricky to implement in the C++ non-self-hosted compiler, however, because libc does not have a 128-bit `strtod` function. So I took musl's `strtold` function, which works with the type `long double`, and then [ported the code into Zig](https://github.com/ziglang/zig/commit/4615ed5ea003516c8235728ac3f5f0ee2ccea8a7), making all the #ifdefs assume that `long double` is 128-bits (which is only true on some architectures), and replacing all the math with [SoftFloat](http://www.jhauser.us/arithmetic/SoftFloat.html) so that it would work on any architecture, no matter what `long double` maps to.

### Alpine Linux is based on musl

Many people are aware of [Alpine Linux](https://www.alpinelinux.org/) because it has become a popular Linux distribution to use with [Docker](https://www.docker.com/), mainly due to its simplicity and small binary size. This is in large part thanks to the fact that the system uses musl as its libc rather than glibc.

The Zig project uses Alpine Linux to create the static Linux builds on [ziglang.org/download](https://ziglang.org/download/).

* * *

And so I have decided to [donate $150/month to the musl project](https://www.patreon.com/musl), even though that represents 10% of my income. I'm putting my money where my mouth is. But there's more - please read on...

## This is a marketing stunt

Now if I'm being honest about my motivations for this blog post, it's that I want to prove that **open source funding is not a zero-sum game**. If there's anything we've learned from the V language Internet drama that has unfolded over the past few days, it's that open source projects have to do marketing if they want to get financial support.

But I want to set a better example of what that might look like. I don't think you have to trick people. This blog post is a marketing stunt intended to show that you can do things to get attention and funding, without being dishonest.

**Will you help me prove my point?**

If enough people start [pledging donations to Zig](https://github.com/users/andrewrk/sponsorship) when reading this, and the $150/month is recovered, then this little stunt will have been a proof-of-concept of one open source project running a fund-raiser for another one. (To be clear my donations to musl are not conditional; regardless of the outcome I will continue to donate.)

Worth a shot, right?

Since making news headlines, several generous people have taken me up on this experiment:

- Marko Mikulicic - $4
- Tristan Hume - $5
- Christine Dodrill - $15
- komu - $1
- Dylan Baker - $3
- Dave Voutila - $6.25
- Tyler Philbrick - $3
- Ben - $1
- Tanner Schultz - $3
- Martin DeMello - $5
- Thomas Ballinger - $2
- Steven Branson - $5
- Zachary Feldman - $10
- Dan Gallagher - $10
- Quetzal Bradley - $10
- Brian Orr - $5
- YVT - $5
- Dexter Haslem - $5
- Wes - $2.40
- Aeron Avery - $10
- Carsten Dreesbach - $5
- Ville Tuulos - $5
- Curtis Fenner - $3
- Nathan Sculli - $5
- Alec Nunn - $5
- Gurpreet Singh - $5
- Haze Booth - $20
- Don Harris - $20

That's a total of 178.65, well beyond the $150 that I started donating to musl.

100%

Thank you so much! I'm really grateful for all your support. This experiment was a success! Today, the Zig project ran a fundraiser for the musl libc project, and it worked!

Thanks for reading my blog post.
