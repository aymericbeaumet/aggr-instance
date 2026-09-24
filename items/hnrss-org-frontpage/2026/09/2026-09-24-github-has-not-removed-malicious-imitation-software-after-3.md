---
title: GitHub has not removed malicious imitation software after 3 weeks
link: https://successfulsoftware.net/2026/09/24/github-has-not-removed-malicious-imitation-software-after-3-weeks/
source: hnrss-org-frontpage
published: 2026-09-24T15:50:26Z
updated: 2026-09-24T15:50:26Z
first_seen: 2026-09-24T20:00:20.682990735Z
authors:
- hermitcrab
summary: 'Article URL: https://successfulsoftware.net/2026/09/24/github-has-not-removed-malicious-imitation-software-after-3-weeks/ Comments URL: https://news.ycombinator.com/item?id=49832406 Points: 210 # Comments: 86'
content: extracted
html: 2026-09-24-github-has-not-removed-malicious-imitation-software-after-3.html
preview:
  file: 2026-09-24-github-has-not-removed-malicious-imitation-software-after-3.preview-6380cbbe68cf.webp
  width: 256
  height: 124
  color: '#d9e1d6'
images:
- source: https://successfulsoftware.net/wp-content/uploads/2026/09/github.png
  original:
    file: 2026-09-24-github-has-not-removed-malicious-imitation-software-after-3.image-10faef4b43e9.png
    width: 1330
    height: 643
  variants:
  - file: 2026-09-24-github-has-not-removed-malicious-imitation-software-after-3.image-5be98b29fa1f.webp
    width: 320
    height: 155
  - file: 2026-09-24-github-has-not-removed-malicious-imitation-software-after-3.image-7daeea59ccf0.webp
    width: 640
    height: 309
  - file: 2026-09-24-github-has-not-removed-malicious-imitation-software-after-3.image-83376df7a47b.webp
    width: 960
    height: 464
  - file: 2026-09-24-github-has-not-removed-malicious-imitation-software-after-3.image-3ef7ae1de0fb.webp
    width: 1330
    height: 643
  color: '#fcfcfc'
- source: https://successfulsoftware.net/wp-content/uploads/2026/09/github2.png
  original:
    file: 2026-09-24-github-has-not-removed-malicious-imitation-software-after-3.image-194133b1a613.png
    width: 712
    height: 493
  variants:
  - file: 2026-09-24-github-has-not-removed-malicious-imitation-software-after-3.image-22026eef3863.webp
    width: 320
    height: 222
  - file: 2026-09-24-github-has-not-removed-malicious-imitation-software-after-3.image-a925272ab5ea.webp
    width: 712
    height: 493
  color: '#fcfcfc'
- source: https://successfulsoftware.net/wp-content/uploads/2026/09/virustotal.png
  original:
    file: 2026-09-24-github-has-not-removed-malicious-imitation-software-after-3.image-9e0811092662.png
    width: 1086
    height: 933
  variants:
  - file: 2026-09-24-github-has-not-removed-malicious-imitation-software-after-3.image-b91370e3642f.webp
    width: 320
    height: 275
  - file: 2026-09-24-github-has-not-removed-malicious-imitation-software-after-3.image-42feda39a04a.webp
    width: 640
    height: 550
  - file: 2026-09-24-github-has-not-removed-malicious-imitation-software-after-3.image-352c1b4159e6.webp
    width: 960
    height: 825
  - file: 2026-09-24-github-has-not-removed-malicious-imitation-software-after-3.image-4bf8721fe5f2.webp
    width: 1086
    height: 933
  color: '#171726'
- source: https://successfulsoftware.net/wp-content/uploads/2026/09/isobuster.png
  original:
    file: 2026-09-24-github-has-not-removed-malicious-imitation-software-after-3.image-15bc2c49609e.png
    width: 1168
    height: 631
  color: '#fcfcfc'
- source: https://successfulsoftware.net/wp-content/uploads/2026/09/malware-background-image.png
  original:
    file: 2026-09-24-github-has-not-removed-malicious-imitation-software-after-3.image-93edef89b451.png
    width: 781
    height: 445
  variants:
  - file: 2026-09-24-github-has-not-removed-malicious-imitation-software-after-3.image-60f26caa949a.webp
    width: 320
    height: 182
  - file: 2026-09-24-github-has-not-removed-malicious-imitation-software-after-3.image-70e879f1a46c.webp
    width: 640
    height: 365
  - file: 2026-09-24-github-has-not-removed-malicious-imitation-software-after-3.image-e07162b51619.webp
    width: 781
    height: 445
  color: '#fefefe'
- source: https://successfulsoftware.net/wp-content/uploads/2026/09/github3.png
  original:
    file: 2026-09-24-github-has-not-removed-malicious-imitation-software-after-3.image-a8a7d4afaaef.png
    width: 1330
    height: 832
  color: '#fdfdfd'
---

On the 31st August I got an email from a customer, telling me that they had found an imitation of my [data wrangling software](https://www.easydatatransform.com/) on Github. I’m not linking to it, but here is a screenshot:

[![](https://successfulsoftware.net/wp-content/uploads/2026/09/github.png)](https://successfulsoftware.net/wp-content/uploads/2026/09/github.png)

It is using our product name and logo, without permission. I reported it to Github as an imitation on the same day. I got this reply:

[![](https://successfulsoftware.net/wp-content/uploads/2026/09/github2.png)](https://successfulsoftware.net/wp-content/uploads/2026/09/github2.png)

A colleague scanned the Mac .dmg file from the repository using [virustotal.com](https://virustotal.com) and got a whole load of malware warnings:

[![](https://successfulsoftware.net/wp-content/uploads/2026/09/virustotal.png)](https://successfulsoftware.net/wp-content/uploads/2026/09/virustotal.png)

Using [Isobuster](https://www.isobuster.com/) he found out that they have also changed the background image of the .dmg:

[![](https://successfulsoftware.net/wp-content/uploads/2026/09/isobuster.png)](https://successfulsoftware.net/wp-content/uploads/2026/09/isobuster.png)

The new image encourages downloaders to ignore any warnings about the malware!

[![](https://successfulsoftware.net/wp-content/uploads/2026/09/malware-background-image.png)](https://successfulsoftware.net/wp-content/uploads/2026/09/malware-background-image.png)

I reported this additional information on the 10th September.

As of the 23rd September, I have had no response from Github support beyond the original automated email. 23 days without a reponse. This is pisspoor. Do better Github.

I’m not sure what my next line of attack is. A DCMA takedown request to Github?

Realistically the only people likely to download the .dmg are those trying to avoid paying for a license for Easy Data Transform. I don’t have a huge amount of sympathy for them if they get their computers compromised. But I really don’t like bad guys taking advantage of my hard work.

Ps/ Always download software from the vendor, where possible.

**\*\* Update 24-Sep-2026 \*\***

Github finally took the offending page down approximately 10 minutes after this post appeared on the front page of [Hacker News](https://news.ycombinator.com/item?id=49832406). Total coincidence. I’m sure!

[![](https://successfulsoftware.net/wp-content/uploads/2026/09/github3.png)](https://successfulsoftware.net/wp-content/uploads/2026/09/github3.png)

Moral of the story. If you want even the most basic level of support from Github, you need to get on the front page of Hacker News.

And it seems they are able to do things very quickly, when they want to.
