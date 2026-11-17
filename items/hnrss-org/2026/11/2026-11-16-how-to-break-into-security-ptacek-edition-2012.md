---
title: How to Break into Security, Ptacek Edition (2012)
link: https://krebsonsecurity.com/2012/06/how-to-break-into-security-ptacek-edition/
source: hnrss-org
published: 2026-11-16T22:19:23Z
updated: 2026-11-16T22:19:23Z
first_seen: 2026-11-17T09:02:46.426369911Z
authors:
- ronfriedhaber
labels:
- a little sunshine
- burp suite free edition
- how to break into security
- linkedin
- matasano security
- owasp webscarab
- security career advice
- sneakers
- the web application hacker's handbook
- thomas ptacek
content: extracted
html: 2026-11-16-how-to-break-into-security-ptacek-edition-2012.html
remote_preview:
  url: https://krebsonsecurity.com/wp-content/uploads/2012/06/howtobreakintosecurity.png
---

At least once a month, sometimes more, readers write in to ask how they can break into the field of computer security. Some of the emails are from people in jobs that have nothing to do with security, but who are fascinated enough by the field to contemplate a career change. Others are already in an information technology position but are itching to segue into security. I always respond with my own set of stock answers, but each time I do this, I can’t help but feel my advice is incomplete, or at least not terribly well-rounded.

[![](https://krebsonsecurity.com/wp-content/uploads/2012/06/howtobreakintosecurity.png "howtobreakintosecurity")](https://krebsonsecurity.com/wp-content/uploads/2012/06/howtobreakintosecurity.png)I decided to ask some of the brightest minds in the security industry today what advice they’d give. Almost everyone I asked said they, too, frequently get asked the very same question, but each had surprisingly different takes on the subject. Today is the first installment in a series of responses to this question. When the last of the advice columns have run, I’ll create an archive of them all that will be anchored somewhere prominently on the home page. That way, the next time someone asks how they can break into security, I’ll have more to offer than just my admittedly narrow perspectives on the matter.

Last month, I interviewed **Thomas Ptacek**, founder of [Matasano Security](http://www.matasano.com "matasano.com"), about how companies could beef up password security in the wake of a week full of news about password leaks at **LinkedIn** and other online businesses. [Ptacek’s provocative advice](http://krebsonsecurity.com/2012/06/how-companies-can-beef-up-password-security/ "How Companies Can Beef Up Password Security") generated such a huge amount of reader interest and further discussion that I thought it made sense to begin this series with his thoughts:

**Ptacek:** “Information security is one of the most interesting, challenging, and, if you do it carefully, rewarding fields in the technology industry. It’s one of the few technology jobs where the most fun roles are well compensated. If you grew up dreaming of developing games, the laws of supply and demand teach a harsh lesson early in your career: game development jobs are often tedious and usually pay badly. But if you watched “Sneakers” and ideated a life spent breaking or defending software, great news: infosec can be more fun in real life, and it’s fairly lucrative.

I’m a software developer. I try to look at security through the lens of computer science. To me, the most attractive thing about the field is the opportunity it provides to work with lots of different concepts at lots of different levels. Computer security might be the best way to work professionally with compiler theory, or to spend time understanding computer microarchitecture, or to apply advanced mathematics.

Other people get involved in security for different reasons, and those reasons are probably equally valid. Some people really like the “good guys / bad guys” narrative that comes with security. Some people see security as an opportunity to save the world. Other people are drawn to the competitive nature of the field: at the higher levels, it really is a cat-and-mouse game. It also comes closer to meritocracy than most of technology: you can either break in or you can’t; your defenses either work or they don’t.

But for me, it’s just one of the best development jobs you can get, and with that in mind, here’s my advice for people interested in pursuing a career in it.

*First: I want you to learn how to program.* Clearly, you can get through college and get a good-paying steady job without ever learning to love programming. But no one factor gives you as much control over your career, as much of an ability to write your own ticket, as the ability to solve problems using programming languages. A lot of very smart technology professionals have concluded that they just don’t enjoy writing software. Those people should reconsider. Try different languages, or different application domains, but find a way to make programming stick in your head.

*Second: the best jobs in our field are in application security.* You can get good steady work designing firewall deployments, setting up desktop agents, or responding to incidents. But all those roles are fundamentally reacting to what’s happening in appsec. The next generation of security products — along with the org charts of security teams at the savviest companies — are being designed now, by accident, by application security practitioners.

Appsec roles are roles that involve attacking software or devising fixes and countermeasures for those attacks. Two good ways to “come up” in appsec: become a penetration tester, or contribute to (or start) a secure software development practice in your company.

A good way to move into penetration testing: grab some industry standard tools and use an [Amazon EC2](http://aws.amazon.com/ec2/ "aws.amazon.com/ec2/") account to set up a “shooting range” to attack. Some of the best-known tools are available for free: the [Nessus scanner](http://www.tenable.com/products/nessus "http://www.tenable.com/products/nessus"), for instance, while not an application security tool, is free and can land you a network penetration testing role that you can use as a springboard to breaking applications.

When people reach out to Matasano asking how to get a foot in the door attacking software, we have a few simple steps to offer:

0\. Learn to love programming in at least one language. The C Programming Language has the most cachet in application security, but for this step-by-step list, Java or Python or Ruby will do fine.

1\. Grab a copy of  [The Web Application Hacker’s Handbook](http://www.amazon.com/The-Web-Application-Hackers-Handbook/dp/1118026470/ref=pd_sim_b_1/191-9509128-5792759 "http://www.amazon.com/The-Web-Application-Hackers-Handbook/dp/1118026470/ref=pd_sim_b_1/191-9509128-5792759").

2\. Go to the “previous releases” archive at **WordPress.org** and grab very old versions of WordPress; install them at EC2.

3\. Download [OWASP WebScarab](https://www.owasp.org/index.php/WebScarab_Getting_Started "https://www.owasp.org/index.php/WebScarab_Getting_Started") or [Burp Suite Free Edition](http://portswigger.net/burp/download.html "http://portswigger.net/burp/download.html"), both of which are free, and use them to find bugs in ancient WordPress.

If you’re already in an IT role, and want to come up on the defensive side of appsec, try to position yourself near custom software development. Most large firms build “line of business” applications. As a rule, building “line of business” isn’t particularly fun. But defending those apps can be; sometimes, the most boring applications turn out to be surprisingly sensitive.

And the good news for doing appsec in BigCo’s: most companies have very immature security programs. If you can get a role in QA, or in what the cool kids are calling “DevOps”, you can end up with a lot of influence in security. At Matasano, we’ve watched lots of ops people successfully transition to appsec and senior security management just by instituting basic security testing processes on their company’s software.

To me, the whole field boils down to studying, understanding, and manipulating technology. Like most software security practices, we’ve gotten our hands dirty with a fascinating cross-section of the whole technology industry. We’ve written attack tools that target the control registers of device chipsets, we’ve done custom RF work, we’ve written low-level debuggers for different CPU architectures, and we’ve seen and beaten up on products built in almost every programming language and every platform you can imagine. If crawling through the ventilation ducts of the world’s most important technology is something you think might make you tick like we do… well, appsec! Learn to code. Spend a couple days Starbucks money on an EC2 account and deploy some broken apps to break. Find opportunities to practice in your job.

And, uh, come talk to us? People like you are hard to find!”
