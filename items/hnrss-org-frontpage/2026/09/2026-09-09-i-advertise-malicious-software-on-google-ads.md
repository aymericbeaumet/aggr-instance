---
title: I advertise malicious software on Google Ads
link: https://xlii.space/eng/malicious-software-on-google-ads/
source: hnrss-org-frontpage
published: 2026-09-09T11:43:21Z
updated: 2026-09-09T11:43:21Z
first_seen: 2026-09-09T16:31:40.482436471Z
authors:
- xlii
summary: 'Article URL: https://xlii.space/eng/malicious-software-on-google-ads/ Comments URL: https://news.ycombinator.com/item?id=49624856 Points: 239 # Comments: 140'
content: extracted
html: 2026-09-09-i-advertise-malicious-software-on-google-ads.html
preview:
  file: 2026-09-09-i-advertise-malicious-software-on-google-ads.preview-268486411221.webp
  width: 256
  height: 156
  alt: screenshot of Google Safe Browsing saying that race-term.com site is safe
  color: '#f2f3f3'
images:
- source: https://xlii.space/images/race-screen-safe_hu_101a2ba0f58f761d.png
  original:
    file: 2026-09-09-i-advertise-malicious-software-on-google-ads.image-70a4b80a6008.png
    width: 574
    height: 350
  variants:
  - file: 2026-09-09-i-advertise-malicious-software-on-google-ads.image-7f5c8129ff73.webp
    width: 48
    height: 29
  - file: 2026-09-09-i-advertise-malicious-software-on-google-ads.image-5f59c4f5313c.webp
    width: 320
    height: 195
  - file: 2026-09-09-i-advertise-malicious-software-on-google-ads.image-f1d7b769f63f.webp
    width: 574
    height: 350
  color: '#fcfcfc'
- source: https://xlii.space/images/race-screen-console_hu_ba44cd83ef7428bc.png
  original:
    file: 2026-09-09-i-advertise-malicious-software-on-google-ads.image-434dee5871c2.png
    width: 677
    height: 350
  variants:
  - file: 2026-09-09-i-advertise-malicious-software-on-google-ads.image-2530c69bc1db.webp
    width: 48
    height: 25
  - file: 2026-09-09-i-advertise-malicious-software-on-google-ads.image-71c3fb49a928.webp
    width: 320
    height: 165
  - file: 2026-09-09-i-advertise-malicious-software-on-google-ads.image-3eafa5e1a2bc.webp
    width: 640
    height: 331
  - file: 2026-09-09-i-advertise-malicious-software-on-google-ads.image-177456dda5ce.webp
    width: 677
    height: 350
  color: '#f0f4f8'
- source: https://xlii.space/images/race-screen-virustotal_hu_dba72959fe458fc8.png
  original:
    file: 2026-09-09-i-advertise-malicious-software-on-google-ads.image-00407a4f1df3.png
    width: 608
    height: 850
  variants:
  - file: 2026-09-09-i-advertise-malicious-software-on-google-ads.image-f41c1736b55e.webp
    width: 48
    height: 67
  - file: 2026-09-09-i-advertise-malicious-software-on-google-ads.image-f5e2fd47c6db.webp
    width: 320
    height: 447
  - file: 2026-09-09-i-advertise-malicious-software-on-google-ads.image-a59a74482274.webp
    width: 608
    height: 850
  color: '#171827'
- source: https://xlii.space/images/evil-google-ads_hu_c5070ed34434d7e4.png
  original:
    file: 2026-09-09-i-advertise-malicious-software-on-google-ads.image-77632da76876.png
    width: 615
    height: 350
  variants:
  - file: 2026-09-09-i-advertise-malicious-software-on-google-ads.image-e453097d7b1c.webp
    width: 48
    height: 27
  - file: 2026-09-09-i-advertise-malicious-software-on-google-ads.image-ba683d9410de.webp
    width: 320
    height: 182
  - file: 2026-09-09-i-advertise-malicious-software-on-google-ads.image-32b71b13d87c.webp
    width: 615
    height: 350
  color: '#fdfdf9'
---

> A caller phones Radio Erywan.
>
> “Is it true that in Moscow, on Red Square, they give out cars?”
>
> “True. Not in Moscow, however, but in St. Petersburg. Not on Red Square, but on Revolution Square. Not cars, but bikes. And they don’t give them out, but steal them”.

* * *

Edit: Through the apparent magic of Hacker News, my Google Ads account has been reinstated. ✨.

Still no explanation of what triggered the suspension, but thank you to everyone who helped make it visible (and/or fixed).

* * *

RACE is a native macOS terminal multiplexer written in Rust. You can position and resize terminals freely instead of squeezing everything into a uniform grid: Keep a large editor beside a small shell, spread out build logs, arrange the workspace around the work, mark with colors etc. I built it, I use it, and I maintain it. Since it’s multiplexer it manages terminals and lets shell sessions survive application restarts (good for development and hooking off the work).

In action it looks like this:

I also built the website for it - [https://race-term.com](https://race-term.com). Static page built with Bridgetown. JavaScript changes the DOM to provide interactive visual features and that’s it. No custom server-side application behind it. At the submission time (that changed recently) the only third-party JavaScript ws Cloudflare Analytics. No Google Analytics or advertising tracking - to keep it light. Served from Cloudflare, Cloudflare R2 keeps the downloads.

Then I tried advertising for the first time.

I set up a Google Ads campaign. Spent 500$ and… Google suspended the account for “Malicious software”.

Unexpected addition to the feature list.

## Google

To be precise - Google said “Malicious software” and “Compromised Site”. Sounded like bullshit. Application was signed and notarized. Website was clear and had no attack surface, but sure I’ll check. Zero. Nothing. Nada. Clean like a whistle. So I appealed.

It rejected my appeal, told me to submit new information, and suggested deleting my account if I had none. It also pointed me toward EU redress options.

It did not say what was malicious, what was compromised, or why the evidence I supplied failed to address either accusation. I received instructions for appealing the decision, but no explanation that would help me challenge.

I appealed, and got rejected.

Then I appealed, and got rejected.

And I appealed with even more information, and got rejected.

And again, and got blocked for a week.

And again…

> We have completed a full security review of the RACE website, download infrastructure, JavaScript assets, and distributed macOS application.
>
> ### Security verification
>
> - **Google Safe Browsing:** no issues reported for the distributed DMG:\
>    [https://transparencyreport.google.com/safe-browsing/search?url=https:%2F%2Fdownloads.race-term.com%2FRACE.dmg&hl=en](https://transparencyreport.google.com/safe-browsing/search?url=https:%2F%2Fdownloads.race-term.com%2FRACE.dmg&hl=en)
> - **VirusTotal:** the distributed DMG/binary is clean:\
>    [https://www.virustotal.com/gui/file/84d082f6a5d3ab75b30e532bf60b406ce12ecc3550bc3f2c6904f34d08ba5e9f?nocache=1](https://www.virustotal.com/gui/file/84d082f6a5d3ab75b30e532bf60b406ce12ecc3550bc3f2c6904f34d08ba5e9f?nocache=1)
> - The DMG and application signatures were independently verified and found valid and clean.
> - All JavaScript source files and generated bundles used by the website were reviewed. Bundle outputs were analyzed and no malicious, injected, obfuscated, or unexpected code was found.
> - **Google Search Console — race-term.com:** no security issues reported:\
>    [https://search.google.com/search-console/security-issues?resource\_id=sc-domain%3Arace-term.com](https://search.google.com/search-console/security-issues?resource_id=sc-domain%3Arace-term.com)
> - **Google Search Console — downloads.race-term.com:** no security issues reported:\
>    [https://search.google.com/search-console/security-issues?resource\_id=sc-domain%3Adownloads.race-term.com](https://search.google.com/search-console/security-issues?resource_id=sc-domain%3Adownloads.race-term.com)
>
> ### Application behavior
>
> RACE is a native application written in Rust. It is a terminal multiplexer, so by design it launches and manages terminal processes in the background.
>
> This subprocess-management behavior is an essential part of the application’s functionality and may resemble behavior sometimes associated with security-sensitive software, but it is neither hidden nor malicious.
>
> The process-multiplexing mechanism is documented in the application. Users can also configure RACE to use `dtach` as an alternative multiplexer. This behavior and the relevant configuration are disclosed in the application’s About/configuration documentation.
>
> The application does not install malware, inject code into other applications, modify browser behavior, or attempt to conceal its process activity.
>
> ### Request for review
>
> We have investigated all plausible causes of the policy/security flag, verified the website and distributed application using both Google and independent security tools, and found no security issue.
>
> We therefore believe the suspension may be the result of a false positive, potentially related to the legitimate subprocess-management behavior inherent to a terminal multiplexer.
>
> Please perform a manual re-review of the account, website, and application based on the evidence above.

## Checklist

But it’s not like I just responded. I checked!

**1\. Google Safe Browsing**

I checked both `race-term.com` and `downloads.race-term.com`. Both reported “No unsafe content found”. The screenshots show the status updated on 9 September 2026.

![screenshot of Google Safe Browsing saying that race-term.com site is safe](https://xlii.space/images/race-screen-safe_hu_101a2ba0f58f761d.png)

This establishes what Safe Browsing reported for those addresses. It does not establish what Google Ads detected, or whether the two systems use the same criteria.

**2\. Google Search Console**

I opened the Security Issues report for each domain separately. Both reported “No issues detected”.

There was no listed infected page, malicious download, or injected resource to investigate. Again, this is a result from Search Console, not a clearance certificate from Google Ads.

![screenshot of Google Search Console saying that race-term.com site is safe](https://xlii.space/images/race-screen-console_hu_ba44cd83ef7428bc.png)

**3\. The File**

Maybe something got into the file? Worth checking the file, right? And so I did. Of course it came out clean! The fun thing is that even Google’s scan identifies NO MALWARE inside.

![screenshot of VirusTotal dash saying that RACE.dmg is clean](https://xlii.space/images/race-screen-virustotal_hu_dba72959fe458fc8.png)

**4\. Signatures and website code**

And so I checked:

- File signatures
- Notarization status
- Raw JavaScript output
- Bundled JavaScript output (who knows maybe something’s in the bundler?!)
- Cloudflare logs (maybe Google bounced of some anti-LLM guard or captcha)
- Accessibility with different User Agents

Nothing. Nothing. Nothing.

**5\. Persistent terminal processes**

There is one thing though.

RACE deliberately starts and manages background shell processes. Its configuration documents three persistence backends: its native PTY host, external `dtach`, or no persistence.

Ok, so I thought - maybe that’s the case. That you can start app, and it stays behind. So I made version 1.0.39 that makes a hacky cleanup after application is deleted. Submitted it and do you know the results?

(Note: hackiness is out, right now user is being asked about it, though it’s fucking stupid from UX perspective)

## What now?

Who knows. That road might be out forever.

What is the most enraging in the whole situation is Catch 22 I’m in:

![](https://xlii.space/images/evil-google-ads_hu_c5070ed34434d7e4.png)

No idea what I can do now, I’ll appeal until oblivion, …or maybe I’ll try EU court case. Cause it seems like that’s the only road forward.

Radio Erywan at least supplied the corrections.
