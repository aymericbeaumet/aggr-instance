---
title: 'Breaking Up with Google Play: Why Conversations Is Now Free'
link: https://gultsch.de/posts/breaking-up-with-google-play/
source: lobste-rs-top-1w
published: 2026-09-24T14:57:57Z
updated: 2026-09-24T14:57:57Z
first_seen: 2026-09-24T23:11:42.252098928Z
authors:
- gultsch.de via tusharhero
labels:
- android
summary: Comments
content: extracted
html: 2026-09-24-breaking-up-with-google-play-why-conversations-is-now-free.html
preview:
  file: 2026-09-24-breaking-up-with-google-play-why-conversations-is-now-free.preview-477d34847b75.webp
  width: 256
  height: 169
  alt: Gross revenue of Conversations 2014-2026 (before Google’s fee and sales tax)
  color: '#f1f5f7'
images:
- source: https://gultsch.de/posts/conversations_revenue.png
  original:
    file: 2026-09-24-breaking-up-with-google-play-why-conversations-is-now-free.image-fa0324448698.png
    width: 903
    height: 595
  variants:
  - file: 2026-09-24-breaking-up-with-google-play-why-conversations-is-now-free.image-a99922a11bcf.webp
    width: 320
    height: 211
  - file: 2026-09-24-breaking-up-with-google-play-why-conversations-is-now-free.image-cff5e7ae8f79.webp
    width: 903
    height: 595
  color: '#fefefe'
---

[Conversations](https://codeberg.org/iNPUTmice/Conversations), my federated instant messaging client for Android, started out as many traditional open-source projects do: as an attempt to scratch my own itch. Development started in January 2014 in my student dormitory, and within weeks I started dogfooding and using the client as the primary means of communicating with my friends. However, when it came to releasing the app to the public on March 24, 2014—exactly twelve and a half years ago today—it was immediately clear to me that I would at least try to turn my open-source project into a business. While I didn’t invent the business model of making the source code publicly available but charging for the convenience of a compiled binary, it was certainly unusual in 2014.

Fast forward a decade, and I did manage to turn Conversations into a sustainable business. Ever since March 2014, Conversations—or other related activities—have been my primary source of income. Admittedly, sustaining life as a student in a tiny dormitory doesn’t take much, but luckily revenue has steadily increased as I grew older.

The exact sources of income have shifted over the years. In the beginning, it was a lot of paid development for companies that wanted to use Conversations. Some paid for features that made it into mainline Conversations; others wanted custom features so specific to their workflow that they never made sense to merge upstream. This was occasionally supplemented with providing server setup or even some consulting on instant messaging and security-related topics. Later on, grants and funding opportunities played a more and more important role.

One surprisingly steady source of income, however, has always been the Play Store revenue. I used to say that it pays my rent. Every freelancer knows the feeling of uncertainty that comes with only being able to send out invoices every few months or receiving payment for funded projects only at the end of the funding period. Any form of regular income—especially in the early stages, when you have not yet built up any savings—is a blessing.

![Gross revenue of Conversations 2014-2026 (before Google’s fee and sales tax)](https://gultsch.de/posts/conversations_revenue.png)

Gross revenue of Conversations 2014-2026 (before Google’s fee and sales tax)

My relationship with Google was never good. App updates have been rejected more times than I can count for incomprehensible reasons. Conversations has been removed twice from the Play Store. Once, Google just randomly accused me of uploading users’ contacts[^1]—which simply wasn’t true and was also not triggered by a specific update. Countless times I wished I could just talk to an actual human for five minutes. So many misunderstandings could have been cleared up in no time if I wasn’t going up against AIs and click workers. At the time of writing this blog post, I’ve been waiting 14 days for Google to review an app update. Review times were never good or anything close to what I would deem acceptable, but they have been getting a lot worse over the last year or so. One can imagine that part of the problem is an avalanche of AI-generated slop apps—something Google played no small part in creating in the first place. But Google should have the responsibility to prioritize long-standing, non-AI-generated apps with infrequent updates. Waiting a little longer for new features might not sound like a big deal to some, but Google makes no distinction between feature updates and security updates. Delaying security updates by days or weeks is outright dangerous.

At this point, I should add some context. Google takes a 15% cut on my app sales. This effectively means I’m paying Google more than 1000 Euro per year for their services. 1000 Euro per year is 1.5x what I’m paying for my internet access. It’s roughly what I’m paying for my notebook per year if you assume I use it for three to four years. When my internet breaks, someone drives to my house and fixes it. When my notebook breaks, someone drives to my house and fixes it. When Google fucks up, there is absolutely nothing I can do. Apparently that amount of money doesn’t give me the privilege to talk to a fucking human for five minutes once per year.

For years I’ve felt like I was in a toxic relationship with Google, and the only reason I stayed was economic dependency.

Over time, the source of my income has shifted more and more towards grants. Sometimes via NLnet[^2][^3][^4], sometimes more directly from the European Commission[^5]. I have secure funding via various grants until the end of 2029 and I’m fairly confident that other funding opportunities will come up for the time after that.

Conversations was always available on F-Droid, but in the beginning, I didn’t advertise the option of downloading it for free. Initially, the F-Droid package maintainers asked for my permission, knowing that Conversations was a paid app on Google Play. I didn’t refuse, but I also didn’t link to F-Droid from the official website because I wanted to steer users towards the paid version. Over time, as my sentiment toward Google shifted from bad to worse, I did start linking to F-Droid. Now F-Droid has become the primary method of distributing the app. The APK distributed over F-Droid is now built reproducibly and signed with my personal signing key.

Fortunately, I’m no longer economically dependent on Google Play Store revenue. Google doesn’t deserve me and my money anymore. I’m done. Fuck the gatekeepers.

[^1]: [https://gultsch.social/@daniel/111929074071688694](https://gultsch.social/@daniel/111929074071688694)

[^2]: [https://nlnet.nl/project/Conversations/](https://nlnet.nl/project/Conversations/)

[^3]: [https://nlnet.nl/project/Conversations-3.0/](https://nlnet.nl/project/Conversations-3.0/)

[^4]: [https://nlnet.nl/project/Conversations-OpenPGP-refresh/](https://nlnet.nl/project/Conversations-OpenPGP-refresh/)

[^5]: [https://mobifree.org](https://mobifree.org)
