---
title: That Time I Worked With a Laptop Thief (2025)
link: https://blog.pipetogrep.org/2025/07/27/that-time-i-worked-with-a-laptop-thief/
source: lobste-rs
published: 2026-10-06T20:16:33Z
updated: 2026-10-06T20:16:33Z
first_seen: 2026-10-07T13:12:11.295552311Z
authors:
- blog.pipetogrep.org via jeanthomas
labels:
- security
summary: Comments
content: extracted
html: 2026-10-06-that-time-i-worked-with-a-laptop-thief-2025.html
---

Published on: 2025-07-27 19:21:39 by **Chris Jones**

Updated on: 2026-10-05 21:10:44 • 10 min read

## Preamble

I used to worked at a place where they had an IT contractor that was stealing laptops and reselling them for over a year. This post is the story of how he got caught. Real names of people and companies will not be used, but the tale and screenshots (with redactions) are all true.

## When I Went Back to IT

I was burning out on my position as a DevOps Engineer. Instead of looking for yet another job doing the same thing, I asked my boss if I could fill a different role temporarily so I could reset while still being useful to the company. He agreed and asked what I had in mind.

The company's IT manager was leaving and the whole department's staff, tools, and processes really needed some attention. I volunteered to be the interim IT manager that handled banging the department back into shape. He liked the idea so I got started.

## The Contractor

The only one left in the IT department was a single IT contractor that we will call Jim. Jim had been with the company for a little over a year. As I got to know Jim, I discovered that everything under his watch was falling apart. The IT closet was a gigantic mess. He was rarely available during working hours, he worked on who knows what at the office during the evening, and wasn't helpful when he was assisting with IT related issues.

I gave him the benefit of the doubt and figured that his department was understaffed and overworked for too long so the first order of business was to assemble a new team.

## The New Team Assembles

After a month of helping with IT tickets basically on my own (Jim was often MIA), and after cleaning out the IT closet, I hired a new IT manager and support specialist. During their first week, I gave them a tour of the office including the newly reorganized IT closet. That was when I first noticed something was amiss.

We had just placed an order for 15 new MacBooks. While some of them had already gone out, the number that remained in the closet still looked smaller than it should have. I consulted the inventory and saw that six machines were unaccounted for.

## The Lies Begin

To keep everything transparent and out in the open, I started a thread in the IT discussion Slack channel asking about the missing laptops and requested everyone on the team to speak up in the thread if they knew anything about it. My boss knew nothing. He could not since he works remotely. The new hires didn't know anything either since this was the first day they were seeing the IT closet.

Then Jim messages me, not in the thread but via DM.

[![Lies](https://blog.pipetogrep.org/images/laptop_thief1.png)](https://blog.pipetogrep.org/images/laptop_thief1.png)

He's telling me five of the laptops had dead batteries on arrival while the sixth went out to an employee. That was the first thing that didn't add up. Five out of fifteen brand new, sealed in box Apple laptops were dead on arrival? I know Apple's hardware isn't the most resilient, but that failure rate is unheard for them.

In the message he sent me five Apple repair case IDs. Upon looking them up, I see that they were suspiciously created right after I sent out the message about the missing devices. Even more suspicious, I find out that he created the repair requests under his personal Apple account rather than the company account, preventing me from gathering more information about the requests.

## The Lies Continue

The next day, two FedEx boxes arrive containing two brand new MacBooks. The FedEx label said it was from Apple's 5th Ave store in Manhattan (which was walking distance from the office). There were two things odd about these machines:

1. Neither of them were enrolled in our company's Apple Business Manager account. That means we could not autoenroll them into our MDM.
2. The FedEx label looked like it was printed on an InkJet printer from the FedEx website. This is not how Apple does things.

Later Jim claims that two of the laptops have been replaced. I thank him and ask that he not take laptops home while we get all this figured out. He gaslights me by saying he verbally told me about the dead machines and that I must have forgotten. He then states that he does not take laptops home "at all".

[![More lies](https://blog.pipetogrep.org/images/laptop_thief2.png)](https://blog.pipetogrep.org/images/laptop_thief2.png)

## Apple Get Involved. The Lies Get Discovered.

At this point, I'm still under the impression that something is fishy, but Jim still gets the benefit of the doubt. My responsibility at this point was not to accuse or speculate, but collect date and see where it led. I decided to consult Apple.

I took the two new laptops that arrived to the 5th Ave Apple store. I explained the situation I was in and asked if they could tell me anything about the origin of them. They were not purchased by the company. Instead, the name associated with both machines was Jim himself.

Meanwhile, all 5 of the Apple repair requests that he created got as far as Apple sending empty boxes to retrieve the 5 "broken" machines. Were those boxes sent to the company? Nope! They were sent to Jim's place of residence.

Finally, I asked about the FedEx shipping labels from the "replacement" machines. I asked how and from where they are normally sent. They confirmed that they do NOT ship repairs from an Apple store and instead send them from Apple Repair Centers, which in this case, was in a neighboring state.

Jim has now been caught in multiple lies:

- "I believe 2 of the defective laptops have been replaced and should be at the office right now." - Two laptops did arrive to the office, but they did not belong to the company. They belonged to him.
- "Once again, I do no take laptops home at all." - Then why did he have the Apple repair boxes sent to his home address?
- The FedEx labels were clearly a cover. The from address was spoofed to look like Apple. The FedEx tracking for them never got past the label being created which means Jim hand delivered them to the office (no cost that way).
- The Apple repair cases never updated to indicate that the machines were received, so how could we have two replacements from them?
- The "replacement" laptops had different serial numbers. Devices Apple repairs retain their serial numbers.
- "One laptop is being prepped for a new hire starting Monday." The new hire did not have the missing laptop.

## Jim Plays the Victim

While we were civil and as non-accusatory as possible, and were doing our best to only seek answers about the missing property that Jim was responsible for keeping track of, Jim decides it's best to go full victim.

[![Time to play the victim](https://blog.pipetogrep.org/images/laptop_thief3.png)](https://blog.pipetogrep.org/images/laptop_thief3.png)

## Jim Gets Let Go

At this point, we were not sure about the entirety of what was going on, but we knew that we had machines missing, and nothing that Jim was saying was adding up and that we were being lied to. Jim was given 24 hours to reproduce all of the missing machines which he failed to do. At that point we had no choice but to let him go before he had the chance to quit himself.

## We Get Theft Confirmation

My first act as interim IT manager was to fix the auto-enrollment of company machines. By this, I mean making it so the devices the company purchased know to register themselves via mobile device management (MDM) and install the company's software, settings, etc. all automatically straight out of the box. Jim was manually enrolling them and could never give a straight answer besides "it's hard" as to why he continued to do it that way.

A few days after Jim was let go, we start getting Slack notifications about new device enrollments. They were being enrolled from places like Russia, China, and Kazakhstan and registered to people that did not work for the company. We proceeded to lock the devices. Most of them were the devices that went missing. It makes sense now why Jim never bothered fixing device auto-enrollment.

These enrollments continued for another year, some new, and some the same machines being resold again by people that knew how to unlock and wipe the machines back to factory only to have them locked again on first boot. We followed some of these machines across multiple countries and geographic zones.

The real victim is not the company itself, but the people that unknowingly bought these stolen devices. They open up their sealed, in-box MacBook only to have it immediately turn itself into a paperweight when it gets locked by the company it belongs to.

## Another Mystery Laptop Appears. Things Get Stranger.

The few days after Jim's departure, another new MacBook arrives in the mail. I take it to the Apple Store again, and they tell me it was not purchased there like the other two were. Instead, it was purchased by a company called "Ingram Micro" who is a large reseller of IT equipment.

I call Ingram and they give me the name of the purchaser and the company they worked for when the purchase was made. I will not name the company, but I will say that they are a department of defense (DoD) contractor.

It took a few days but after many phone calls and messages, I got through to the DoD contractor's IT and legal teams and was able to explain what was going on. It was discovered that Jim was working for them (at the same time!) and stole one of their laptops as a replacement for one that he stole from us!

By doing that, he got himself let go from that job as well. I later found out from my boss that an FBI agent sat him down and he confessed to everything. DoD jobs do not seem to take kindly to property theft.

## Jim Gets Petty

Weeks went by and Jim still hadn't returned the company laptop that was assigned to him. After multiple reminders, a very beat up box arrived in the mail via UPS. It contains two laptops from Jim with zero protective padding. The machines were trashed inside and out.

[![UPS box in rough shape](https://blog.pipetogrep.org/images/laptop_thief4.jpg)](https://blog.pipetogrep.org/images/laptop_thief4.jpg)

[![Inside the box](https://blog.pipetogrep.org/images/laptop_thief5.jpg)](https://blog.pipetogrep.org/images/laptop_thief5.jpg)

[![Top of the returned laptops](https://blog.pipetogrep.org/images/laptop_thief6.jpg)](https://blog.pipetogrep.org/images/laptop_thief6.jpg)

[![Gross laptop interior with surprise staple](https://blog.pipetogrep.org/images/laptop_thief7.jpg)](https://blog.pipetogrep.org/images/laptop_thief7.jpg)

[![Scrapped up laptop bottom](https://blog.pipetogrep.org/images/laptop_thief8.jpg)](https://blog.pipetogrep.org/images/laptop_thief8.jpg)

## What Happened to Jim?

I don't really know. The company that I worked for dropped the ball on pressing charges. All told, he stole more than 30 laptops over the year he worked there. The monetary value of the theft was well into the range of grand larceny, but I guess they didn't care enough to pursue it. I'm not sure if he got so lucky with the DoD contractor.

## What Did I Learn?

A lot!

- Maintain a real inventory system instead of just a spreadsheet. If that was done from the beginning, this whole thing might not have happened.
- I also learned about Apple Business manager, how Apple MDM software works at a low level, and how IT departments manage large fleets of devices.
- I learned that if something feels off, it is worth investigating.
- Don't hire individual contractor. Go through a reputable recruiter and run background checks. This did not happen with Jim.
- Most importantly, I learned that an engineer's natural curiosity is applicable to more that just engineering. The more I dug into what Jim said, the more lies I found.

## What Did Jim Learn?

Something I hope. If he simply maintained that he didn't know anything, he might have gotten further with his ruse. Instead, he kept talking and kept digging himself deeper. At the most, I can hope that he learned not to pull this crap anymore in the future. At the very least, he learned not to lie to me.

Want to discuss? Email me at [blog@pipetogrep.org](mailto:blog@pipetogrep.org).
