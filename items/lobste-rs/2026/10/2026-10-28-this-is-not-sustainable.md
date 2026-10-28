---
title: This is not sustainable
link: https://www.netmeister.org/blog/defense-ddos.html
source: lobste-rs
published: 2026-10-28T03:56:49Z
updated: 2026-10-28T03:56:49Z
first_seen: 2026-10-28T17:21:45.580541782Z
authors:
- netmeister.org via binjip978
labels:
- security
summary: Comments
content: extracted
html: 2026-10-28-this-is-not-sustainable.html
remote_preview:
  url: https://www.netmeister.org/blog/images/captain-haddock.jpg
---

What a week, huh? High fives all around—October is "Cybersecurity Awareness Month", and I think we've all done a stellar job. If both the [New York Times](https://www.nytimes.com/2026/10/22/world/php-vulnerability-websites-cybersecurity-ai.html) and [NPR](https://www.npr.org/2026/10/23/g-s1-149503/hacking-php-flaws) report on one of the at least four or five [concurrent fires](https://istheinternetonfire.com/) I'm fighting, then I think we can say that yeah, people are aware of cybersecurity. Mission accomplished, so... could we maybe turn it down a bit?

The current situation is not sustainable: 0-days are dropping left and right, pre-notifications and embargoes are [broken for marketing purposes](https://www.akamai.com/blog/security-research/tlp-is-not-a-suggestion), AI companies compete first on who commits the most crimes and then pretend to sell you protection in the form of a DoS on the Open Source community. "*Nice project you got there. Be a shame if somebody dropped an 0-day on it."*[1](https://www.netmeister.org/blog/defense-ddos.html#1)

Companies are not staffed for this, and no matter what the industry wants to tell you, you can't AI your way out of this. Vulnerability discovery has fundamentally changed, disclosure processes are effectively broken, and patch management has been a losing proposition for years now. Severe vulnerabilities are identified and disclosed at a pace that defenders are unable to keep up with.

This is because it still takes significant resources to accurately assess the feasibility of a given vulnerability within your environment; it takes time to discover your attack surface relevant to the given attack vector, and it takes even more time to deploy a fix. As I've [said before](https://www.netmeister.org/blog/everybodys-lost-their-minds.html#sisyphus), development of the fix is not the bottleneck. Let me illustrate with examples from the last couple of days:

### "[PwnEscape](https://pwn.ai/blog/pwnescape)" (CVE-2026-99999)

Initially disclosed as a "KVM escape", this vulnerability *also* applies to other virtualization as well as containers, although that angle hasn't really gotten much attention. You now need to assess which possible mitigations might be in place across your fleet of systems, then determine whether you can [livepatch](https://www.kernel.org/doc/html/latest/livepatch/livepatch.html) your kernels with the custom patches you pull from upstream or if you want to wait for your upstream OS provider to deliver a new kernel [2](https://www.netmeister.org/blog/defense-ddos.html#2) and then all you have to do is reboot a few thousand systems.

How does AI help you here? Maybe you spend a few thousand dollars on tokens to use the most advanced cyber cyber models to develop your own exploit, but in the end, that doesn't really tell you anything you didn't already know, and your defensive position and tasks at hand remain the same.

### [eMudhra compromise](https://emudhra.we-have-a-problem.com/)

This one's fun, isn't it? Sure, browsers were all set quickly, but guess what, there's a few other things that might use TLS in your environment. Like... *everything*. So again, having the fix is not the bottleneck. Waiting for upstream packages to release an update only blocks you if you actually know that you need to pull in that update.

Oh, you have a detailed package inventory or, dare I say it, SBOMs? That's really nice. Do show me where that lists the various [vendored or hard-coded CA trust bundles](https://github.com/NixOS/nixpkgs/issues/576475). Yeah, I didn't think so.[3](https://www.netmeister.org/blog/defense-ddos.html#3)

(At least this incident was not AI generated, detected, or triggered. Just one of your wholesome, classical, [a-human-going-"huh"](https://bugzilla.mozilla.org/show_bug.cgi?id=2083449#c2) kind of discoveries. It's almost reassuring that we still get these, too.)

### "[php2shell](https://www.php.net/archive/2026.php#2026-10-21-5)" (CVE-2026-109618, CVE-2026-109619)

The PHP vulnerability that supposedly affects "over 70% of websites" (a widely quoted, but never attributed statistic) was disclosed under embargo to several organizations. If you were part of this inner circle, then this should have reduced your panic-mode somewhat, buying you at least *some* time to deploy protections for your customers and do the tedious work of sifting through your inventory and, again, identify the attack surface, affected systems, possible local mitigations, etc. etc. The usual.

Unless the embargo is broken. Then you get to scramble again. And you need to keep up to date with all the rapidly published (and increasingly verbosely AI polluted) breathless blog posts driven by cyber security vendors' marketing teams eager to beat each other as the first to detect [4](https://www.netmeister.org/blog/defense-ddos.html#4), first to exploit, first to brag, all of which adds to the distraction.

And of course you *still* need to do the actual work of determining the accuracy of the various exploits floating around the internet, some put together by people with a clue and many, many slopped together by those lacking a full understanding of the bug [5](https://www.netmeister.org/blog/defense-ddos.html#5). (More on that below.)

### The [brotli RCE](https://github.com/advisories/GHSA-9v8h-qqgm-4m35) (CVE-2026-111497)

And then, by the time Thursday rolls around, people start to see a [NodeJS security advisory](https://nodejs.org/en/blog/vulnerability/october-2026-security-releases), which includes as the third "high" severity vulnerability an issue with the "Node.js Brotli decoder". Alright, attack surface enumeration, software inventory check, start to deploy updated packages,... the usual.

Except, of course, once you take a closer look, it begins to [dawn on you](https://www.openwall.com/lists/oss-security/2026/10/23/25) that this goes well beyond just Node.js [6](https://www.netmeister.org/blog/defense-ddos.html#6). And so once again, having the [upstream fix](https://github.com/advisories/GHSA-9v8h-qqgm-4m35) available is absolutely not your bottleneck, because you still need to find the plethora of frankenstances of brotli across *all* of your stacks first.

Of course at that point in time OpenAI [starts to go](https://twitterviewer.net/OpenAI/status/2113766354386084338) "Us! It was us! We found it!" even though the issue [was fixed back in February](https://github.com/google/brotli/commit/0ed4268a4401abcb4472096340477cf4af7d1a2b) and Mozilla [noted the critical security impact](https://bugzilla.mozilla.org/show_bug.cgi?id=2029458) as far back as April. It seems there's no vulnerability, no compromise, no cyber crime AI companies are not willing to claim as their credit.

### (More) Winter is Coming

All of this was going on over the course of one week, and doesn't even account for the previous set of weekly Linux local privilege escalation vulnerabilities [7](https://www.netmeister.org/blog/defense-ddos.html#7). And no doubt there will be more such disclosures—many more.

AI companies are incentivized to continue the onslaught, with the most benevolent interpretation of mass disclosures of findings being a canonical example of "[verschlimmbessern](https://www.studygermanonline.com/blog/the-verschlimmbessern-in-german)", with less friendly interpretations observing that it's profitable for a company to create the disease for which they're selling you to treatment (and never the cure). They've got a new toy, it's a hammer, and by golly they're gonna swing it.

Millions of dollars have been spent on identifying vulnerabilities using so-called "frontier AI models", and we know that adversaries are feeding, e.g., the Linux kernel commit messages into their models to analyze them for exploitable vulnerabilities on the fly. Today, vulnerabilities are found independently by multiple parties on a regular basis, so you already have to assume that a disclosure embargo only contains the *massive* exploitation by opportunistic adversaries at best.

At the same time, I'm observing a worrying trend on the defensive side of cyber security, which mirrors the same de-skilling trend across the industry: AI generated vulnerability reports are often much, much too verbose to be digested easily by human readers. They drown you in details, overwhelm your attention with unnecessary information. Because defenders don't fully understand the vulnerabilities described here, they tend to believe the generally exaggerated severity claims, and feed the full report into *their* AI to reproduce or generate a proof of concept.

Several thousand tokens later, you end up with a binary handed to you by a meat-proxy that's supposed to trigger the vulnerability, but nobody actually understands why it works. Which, more often than not, it actually *doesn't*, because your environment is different, your AI was missing context the meat-proxy didn't (know to) provide, because the code base differs from upstream, or who knows what else. When you ask for clear instructions on how to run the exploit, what pre-conditions need to be met, and how it works—you know, the kind of one-pager that offensive engineers used to be able to produce—you get back, you guessed it, an AI-vomited 7,000 word `README.md`.[8](https://www.netmeister.org/blog/defense-ddos.html#8)

### Incident Fog

Fighting one fire at a time is taxing enough. Each one of the above examples would have been a once-a-year incident not too long ago. Having multiple such incidents at the same time is burning out all your incident response staff, your engineers, your comms and legal teams.

(And it always *is* more or less the same set of people driving each incident. I have not seen organizations properly staffed and not relying on the same set of ghostbusters—the heroes and company-elders with the required tribal knowledge to make up for the gaps in your software inventory, attack surface enumeration, and documentation, because who else are you gonna call?)

Context switching between each of these incidents means you can't focus on any one of them, making you more likely to miss important information, to fall for sensationalistic headlines, to succumb to pressure and to rush fixes you don't understand for vulnerabilities that might not apply in your environment. Which, in turn, makes more people more susceptible to the appealing lure of quick and confident answers from the bot, and subject-matter non-experts start to paste their prompt results into incident response channels.

This is not sutainable.

### How it gets better

Oh, lol, sorry, I mislead you. When you ramble and rant, you're supposed to end on an upbeat note, provide a solution [9](https://www.netmeister.org/blog/defense-ddos.html#9), proffer suggestions on how we can all roll up our sleeves and work together and solve the problem. Alas, I have no idea.

Ah, but surely all of this is just *temporary*, right? AI will find all the vulns, we slog through this dark winter of disclosure chaos, and then come out in a world where all software vulnerabilities have been found and fixed.

If you're able to believe that, I envy you. I don't see it happening. If you try to tell me that software developers should just use AI to generate patches, remember that [over 50% of the time](https://1password.com/blog/why-ai-generated-patches-still-require-human-review), AI either doesn't fix the issue, introduces a new vulnerability, or changes the application's behavior in the process. Reducing our own understanding of complex software while simultaneously [producing more and more code](https://mstdn.social/@jschauma/117439759243067966) is a recipe for *more* vulnerabilities, not fewer. And, at the risk of sounding like a broken record, *getting the vulnerability fixed is not the bottleneck*.

Even if the current status were temporary—for how long? A year? Two? And what are we supposed to do in the meantime? Hiring more infosec staff isn't likely to happen [in this economy](https://www.apollo.com/wealth/insights-news/insights/daily-spark/no-signs-of-ai-payoff-outside-the-tech-sector). Investing into overhauling your entire infrastructure such that you can at least pull in and deploy updates automatically and reboot your fleet and do all that *safely* is equally unlikely: companies have gotten away with *not* doing that for decades, and who wants to be the VP or executive who shifts funding away from "moar code moar faster"? Not to mention that such a project would take several years as well.

The last option left is to simply accept the status quo. There will be more vulnerabilities, including RCEs, and you won't be able to keep up with them. Your only option is to limit the blast radius. Assume compromise, detect exploitation, limit exposure, lateral movement, privilege escalation, etc. If that sounds like an old hat, well, yes, that's what sane infosec practitioners have been asking for funding for for decades. (We used to call it "Zero Trust", before that became an empty marketing term.) But that funding instead goes to the next singing and dancing, AI-based vulnerability management SIEM dashboardmajing, which will only miss 40% of your infrastructure and assets. Hooray!

Well, good night and good luck, indeed.

October 27th, 2026

* * *

Footnotes:

 \[1\] The same racket works even better with businesses: "*Nice supply chain you got there. Be a shame if somebody dropped hundreds of 0-days for most of your dependencies. Better subscribe to our service to get pre-notification.*"

 \[2\] As of 2026-10-27, 12 days after initial disclosure, Canonical, to name one example, still [doesn't ship a fixed kernel](https://ubuntu.com/security/CVE-2026-99999), considering the issue of "medium" severity.

 \[3\] Your best bet in some cases is to just run [a simple fetch](https://github.com/jschauma/emudhra) using the language/framework in question. But that doesn't scale, either. Good luck, tho!

 \[4\] Detection of exploit attempts is a critical signal to help you assess the urgency of your response and can influence disclosure timelines. [Announcing you've observed per-disclosure exploitation](https://aws.amazon.com/blogs/security/amazon-threat-intelligence-identifies-php2shell-exploitation-prior-to-public-disclosure-in-madpot/) without sufficient confidence [is harmful](https://twitterviewer.net/naderman/status/2114146087699087548) to all responders.

 \[5\] And don't get me started on using `setjmp(3)/longjmp(3)` for error handling in any even remotely complex program. Sure, it's *clever*, but writing *clever* is rarely the same as writing *safe* code. I'm not convinced you can use this mechanism safely in all but the most trivial cases, and wouldn't be surprised if additional vulnerabilities are lurking here. And now that the Eye of Sauron is trained on this codebase...

 \[6\] By the way, does anybody know why Google Docs insists on hyperlinking `node.js`? `.js` is not a TLD. Is it really stupid enough to just go by `[a-z0-9.-]+\.[a-z]{2}`? Oh, it totally is. Brilliant.

 \[7\] Disclosures like "[Linux kernel LPEs: ZcopyReaper (CVE-2026-43502) *and 20 more*](https://www.openwall.com/lists/oss-security/2026/09/08/1) are pretty much par for the course after we got "[DirtyFrag](https://www.dirtyfrag.tech/)", "[GhostLock](https://nebusec.ai/research/ionstack-part-2/)", "[FragGap](https://securityonline.info/fraggap-linux-kernel-cve-2026-53362/)", "[ZapScape](https://github.com/V4bel/Zapscape)", "[Januscape](https://github.com/V4bel/Januscape)", and I forgot what else in a constant trickle earlier this year.

 \[8\] Granted, that's only marginally more than some dude's ranting all-over-the-place blog post. But at least *that* was 100% human generated.

 \[9\] I would like to say that one way forward here is *accountability*. To hold companies accountable for what they do—including what they enable or allow their software, their models, their bots to do. That certainly is necessary for the various reckless AI companies as they continue to fail to restrict their malware. I'd also like to hold vendors accountable for shipping software with known vulnerabilities, but I don't know how that is supposed to work for Open Source, nor do I think it solves the disclosure-as-ddos problem.

* * *

Links:

- [Discussion on Lobsters](https://lobste.rs/s/cpaibp/this_is_not_sustainable)
- [Discussion on HackerNews](https://news.ycombinator.com/item?id=50266602)
