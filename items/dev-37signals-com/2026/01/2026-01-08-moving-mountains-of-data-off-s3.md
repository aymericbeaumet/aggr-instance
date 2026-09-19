---
title: Moving Mountains of Data off S3
link: https://dev.37signals.com/moving-mountains-of-data-off-s3/
source: dev-37signals-com
published: 2026-01-08T18:00:00Z
updated: 2026-01-08T18:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
authors:
- Jeremy Daer, Kimberly Rhodes, Fernando Olivares
summary: Principal Programmer Jeremy Daer explains how we moved billions of files out of Amazon S3 with zero downtime.
content: extracted
html: 2026-01-08-moving-mountains-of-data-off-s3.html
preview:
  file: 2026-01-08-moving-mountains-of-data-off-s3.preview-baf4ebaa44e9.webp
  width: 256
  height: 134
  alt: 37signals Dev
  color: '#e72626'
images:
- source: https://dev.37signals.com/assets/images/opengraph/moving-mountains-of-data-off-s3.png
  original:
    file: 2026-01-08-moving-mountains-of-data-off-s3.image-f60b5c851dc4.png
    width: 2400
    height: 1260
  color: '#fe0000'
extra:
  thumbnail: https://dev.37signals.com/assets/images/opengraph/moving-mountains-of-data-off-s3.png
---

In this episode of [RECORDABLES](https://dev.37signals.com/series/recordables/), we talk through the final and most nerve-racking part of our cloud exit — moving massive amounts of data out of Amazon S3. Principal Programmer Jeremy Daer shares how we moved billions of files with no downtime. He covers everything from dealing with bandwidth limits and AWS constraints to building custom tooling when off-the-shelf options won’t work.

The conversation gets into the human side of a project like this, including verification, anxiety, and the moment you finally hit delete. You’ll also hear how long it actually takes to move that much data and the tools we used to make it happen seamlessly.

[Watch the full video episode on YouTube](https://youtu.be/BnhVXGZepA8).

* * *

## Timestamps

- **00:00:00** — Introduction
- **00:02:05** — Why S3 was the last (and scariest) piece
- **00:08:34** — The volume of data to move
- **00:11:11** — Bandwidth limits and AWS constraints
- **00:13:12** — The custom-built Rails tool for copying and reconciliation
- **00:21:25** — The logistics of hard drives, write speeds, and network connections
- **00:28:05** — The intentional order of moving data
- **00:49:55** — Anxiety, verification, and the fear with deleting data you can’t get back
- **00:54:13** — Was there any downtime?
- **00:58:56** — Essential tools that made the migration possible
- **01:07:03** — What happens next

* * *

## Links & Resources

- [Rclone](https://rclone.org/)
- [DuckDB](https://duckdb.org/)
- [S3 Fast List](https://github.com/aws-samples/s3-fast-list)

* * *

## Transcript

**Episode Highlights (00:00:00):** Well yeah, there’s no real difficulty. It’s just copying some stuff, right? Among those five petabytes of data was spread across hundreds of buckets and on the order of about five billion objects. The concept is straightforward, but you got to be careful and do it right. And there are easy ways to do it right, but there are a lot of easy ways to go wrong too.

**Kimberly (00:00:22):** This is for Recordables, a place where the team at 37signals shares their behind the scenes work, building Basecamp, HEY and open source projects. We’re diving deep into what we’ve done and how we’ve done it so you can learn a thing or two and learn from some of our mistakes. I’m your host, Kimberly. I’m joined to help with the technical side of discussion by Fernando from our engineering team. Hello, Fernando.

**Fernando (00:00:45):** Hello, hello.

**Kimberly (00:00:46):** Well, we are talking this week a little bit more about our move out of the cloud. If he’s been following 37signals at all, you know that we started a move out of the cloud back in 2022. We have wrapped up the big portion of moving data out of S3. To join us, we have Jeremy Daer, principal programmer here at 37signals who did much of that final work to talk about it. Jeremy, thanks for being here. Before we dive into our topic today, tell us a little bit about you and how long you’ve been at 37signals.

**Jeremy (00:01:17):** Yeah, I’m a programmer here. I’ve been around for some ages and this storage migration is the most recent of probably half a dozen over 15, 20 years. So coming to it feeling like it’s time. We need to move all the bits across the continent again.

**Kimberly (00:01:40):** Okay. Well, I’m sure there’s a lot to dive into. Fernando, you’re going to help me with all the technical aspects of it, but let’s just start kind of where we ended, if you will. We know we’ve started this move back in 2022. S3, moving that piece of data was the last part of the project as I understand it. Kind of tell me why that was the last piece and then we’ll dive into the technical side.

**Jeremy (00:02:05):** Yeah, there are two things. It’s expensive and it’s scary. We’ve got a lot of things to move and we had built a lot of trust in S3. It’s super durable, super reliable. You can’t go wrong with S3. It’s like the old school IBM thing. Nobody’s going to get fired for using IBM. Whereas storing your own stuff, it’s all on you. And so if you haven’t been shouldering that risk for a long time, you kind of forget what it feels like. And so a bunch of planning, a bunch of mitigation work, risk assessment can help prepare for that. But kind of getting over that hump and just choosing to do it, we’re going to do it solves that problem. The other is cost. And what are we doing here? It’s easy to open up our wallet and pay AWS. And it causes us to cry a little bit or a lotta bit.

**(00:03:05):** But when we want to replace it, we’re looking at spending hundreds of thousands, millions of dollars on something. And we’ve got to be pretty sure it’s going to work. Well, we’ve got to be certain it’s going to work, but also not spin our wheels for weeks, months, trying to figure out and prove it for sure. So we need to come up with some kind of framework for understanding what we need and being able to prove it before spending a bunch of money. Third thing really, S3 also has this tricky bit which motivated some of the cloud exit of you got to pay. You got to pay for everything. And one of the things you got to pay for is a bandwidth coming out of S3. So if you want to move your data, you’ve got a little bit of, again, a lot bit of a handcuff situation.

**(00:03:49):** You got to pay to get your data out. So the EU came up with some kind of regulations around this and all the major cloud providers got out ahead of the regulations saying that you got to be able to exit and you can’t kind of keep people’s data for ransom. And it was like this before, people didn’t necessarily want to leave S3 because the alternatives were not great. And you’re on the upswing of cloud adoption of look at all the things we’re not doing. You just get to send it out to this abstracted thing and pay some monthly fee rather than doing it all yourself and paying hundreds of thousands upfront for storage hardware that you’ve got to run yourself and maintain yourself. So anyway, AWS came up with this waiver program wherein you could get your data out of S3 under certain conditions. And the conditions run like this.

**(00:04:51):** You’ve got 60 days, 90 days, whatever. You’ve got a ticking clock and you got to get all your data out. You can’t half ass it. You got to get it all out. And if by the end of that time you can prove that it’s all out, then you’ll get AWS credits for the bandwidth cost. So you got to estimate how much stuff you’re storing and then how much bandwidth it should take to get out. If you do it all perfectly, which of course everybody would do, then that would be your credit. So the public messaging is like, “We’re chill with this. We’ll let you get all your data out.” The reality is you’ve got this kind of tight binding contract and you got to do it perfectly and then we’ll give you a refund.

**Kimberly (00:05:42):** Oh, wow. Do we pick the timeframe?

**Jeremy (00:05:45):** We do not pick the timeframe.

**Kimberly (00:05:46):** Do we pick the 30, 60 or 90 days?

**Jeremy (00:05:49):** It’s negotiable. So depending on … Well, the internal messaging toward customers is a limited timeframe, like 60 days or something. The external messaging is, well, we’ll work with you. We’ll be reasonable. So the reality is kind of in between. We do have great account reps, so I can’t fault that end of things. It’s been wonderful interacting with AWS, but you see both sides of things of we’re going to look to the world like we’re ready for you to leave, but in fact, we’re going to make you jump through a bunch of very tight hoops.

**Fernando (00:06:26):** Well, I was going to joke like, “Oh, it doesn’t sound that difficult. You just, I don’t know, copy a bunch of files over to your hardest drive somewhere.” What is the real difficulty there?

**Jeremy (00:06:38):** Well, yeah, there’s no real difficulty. It’s just copying some stuff, right? It’s just that if you want to copy things, well, there’s a lot of things you got to do, especially if you’ve got a lot of files stored. For most people, this copy is not technically hard. The basic process would be clone your S3 bucket. A bucket of objects is like a folder full of files. You list the bucket to see what’s in there. You copy it over to your new place and typically be doing this with a live system. So you need to have some kind of awareness of your system would need to know that you’re moving to a new system and there’s a live old system. So if you want to do it without downtime, you need to be able to store files to both places. You need to mirror your files to both storage systems.

**(00:07:23):** And then you need to do a copy. And after that first copy, maybe you’ve got some stuff that’s in one storage system, but not the other and vice versa. So you need to bring them into sync. You need to reconcile them.

**Fernando (00:07:34):** And none of this is offered by AWS. It’s all on you.

**Jeremy (00:07:37):** AWS does offer something that does something like this, but you got to pay.

**Kimberly (00:07:44):** I feel like that’s a theme.

**Jeremy (00:07:46):** Yeah. So if there’s a data transfer service that can do something like this, and in fact, it can do a lot more. It can do incremental syncing between disparate systems, but it is very expensive. You’re looking at tens of thousands of dollars to move large scale buckets of stuff.

**Fernando (00:08:06):** Was it ever an option for us?

**Jeremy (00:08:07):** We did evaluate it. It would be very nice not to have to do this job ourselves and to have somebody else do it. And there are vendors that do stuff like this, but again, you’re looking on the order of like tens to hundreds of thousands of dollars, depending on the size of your buckets. And most of them will scale with your storage. So they’re looking to take a percentage cut because the more you’ve got, the more you can pay probably.

**Kimberly (00:08:34):** Jeremy, when you’re saying the size of our buckets, how much data are we talking about that was being moved?

**Jeremy (00:08:41):** So in aggregate, we had about 10 petabytes of data across a bunch of buckets. Some of our applications were responsible for a lot more than others. Our average object size was about 1.1 megabytes. Of all that data, we’re also geographically distributed. So deduplicated, we had probably about five petabytes of unique objects. And among those five petabytes of data was spread across hundreds of buckets and on the order of about five billion objects. It’s a lot of stuff. You get into the realm where you can’t … If this is a folder on your computer and you tried to open the folder, your computer would crash. And you can’t list a bucket of that size without taking literally days.

**(00:09:36):** So there are a bunch of interesting constraints that come into play when you try to do this conceptual process of you copy, then you stop your application or whatever, storing things for a little while. Then you do a catch-up copy to reconcile, to make sure your destination has all the stuff that was in the source. And then you cut over, you start using the new system and you got to be sure that you actually got everything, that everything was copied correctly and there were no mistakes and you didn’t miss anything and nothing showed up while you were copying, et cetera, et cetera. So even at the small scale, the concept is straightforward, but you got to be careful and do it right. And there are easy ways to do it right, but there are a lot of easy ways to go wrong too. And once you get to a larger scale, there are a lot of easy ways to go wrong and a lot more easy ways to go wrong and a lot of ways to go slow.

**Kimberly (00:10:32):** Well, I feel like we should talk about all of those things. Not only the things that are easy to do well, but what are all the things people should avoid if they’re trying to do this?

**Jeremy (00:10:43):** Well, what would be wonderful is if egress were free and we weren’t stuck in the situation of needing to move quickly because when we look at the kind of bigger picture of what the job to be done is we got to move five petabytes of stuff within … We negotiated 90 days and that was based on our back of the napkin math of what size of network connection we have available and how quickly we can move stuff.

**Fernando (00:11:11):** Is there a limit on the AWS side on the connection?

**Jeremy (00:11:15):** There are plenty of limits, yes. And there are some hidden limits and some stated limits. So that comes into play when we try to figure out what our limiting factors are.

**Fernando (00:11:25):** Wait, sorry.

**Jeremy (00:11:25):** So if we do a bounds analysis of…

**Fernando (00:11:27):** Yeah, let me backtrack just a little bit. You said like, okay, we negotiated 90 days. What was that like? Oh, we have five petabytes if we have access to, I don’t know, 10 gigabytes per second and we do this twice.

**Jeremy (00:11:40):** I was chatting with our rep saying, “Here’s how fast we can possibly do it. “ So that’s what we need. And they said, “Okay.”

**Fernando (00:11:48):** Okay.

**Jeremy (00:11:50):** We built in plenty of buffer because part of this process is not just the copy. And something that anybody doing a process like this will quickly discover is that copying is, it seems like the central point and the purpose of the job, but it’s really reconciliation and verification, making sure that you did what you thought you did. And the doing what you thought you did is as costly as the copying. And it has different kinds of limitations because you need to go look and see what you’ve got. And that means listing all your files, listing all the objects in a bucket. And when this process can take days to perform, you need faster ways to do it and you need cheap ways to do it because again, you got to pay. If you want to list a bucket, you got to pay for API requests.

**(00:12:43):** And so you’ve got to be careful that you don’t get in a situation where you’re repeating work or doing unexpectedly expensive work. And in many cases, these like API requests are cheap. So it turns out we’re totally fine, but you got to do the due diligence first, so you don’t end up discovering that in order to save whatever tens of thousands of dollars on a paid service, we don’t end up spending tens of thousands of dollars on errant API requests ourselves.

**Fernando (00:13:12):** Yeah, that makes sense. How do you even … I’m trying to wrap my mind around this, where … this process must be automated somehow, right? Is it in the app? Where does this reconciliation process happen?

**Jeremy (00:13:25):** So that gets in a little bit deeper into the approach that we arrived at. So there’s more than one way to do reconciliation and something I’d recommend to people who are moving just a single application, which ideally it’s a situation you’d be in. So a little digression. Looking back, the ideal way to set up AWS accounts in the first place is around these kind of scopes of kind of responsibility radius, blast radius of if you ever wanted to leave, you got to take your whole account out. So don’t go putting a bunch of buckets from different places, don’t share an account. Like make accounts early and often so that accounts are aligned with your application. So that gives you the flexibility to move one application at a time. We were in that situation to some extent with some of our newer stuff, but our older applications, we had a shared account, so we needed to do everything all at once.

**(00:14:30):** And when you have multiple storage systems in a single account and you need to move them all, you can’t tailor the solution for the system that’s being moved. Instead, you need something that’s going to work for all of them. So that led us to, it’s got to be something that’s at a lower level and is mostly transparent to the applications we were running. So if I were doing this with a modern rails app, this is something I’d build into Active Storage at the application level. I’d build in some kind of modeling for where things are stored. So you get metadata within the application itself, knowing that I’ve got this object stored in AWS and I’ve also have it stored in my, whatever my new destination storage is. And so the process would be that I’ve got some rule that says I need to copy storage and they need to be in each of these locations.

**(00:15:23):** And so the application could do that with just a bunch of active jobs, copying things on its own, could do it lazily in the background and just let it trickle through, if you didn’t have these other constraints of you got to get it all done, you got to blast it at maximum speed and you’ve got to make it work with, and you got to make it work with older apps. So not all of our stuff is on latest Rails and we extracted active storage from our own apps. And so most of our older apps are using older abstractions that were kind of the source of the extraction. So we needed to be compatible with all these possible systems. So we needed something lower level.

**Fernando (00:16:04):** So we built…?

**Jeremy (00:16:05):** So we built something. We didn’t want to build something, but we also didn’t want to pay a lot of money. So kind of rock and hard place thing. And we are in the position of having expertise. We are a technical organization. We have programming and operations teams. We do all this stuff. So it’s natural for us to do our jobs. Maybe this isn’t the thing that we’d outsource. If there was a natural fit, we would. And in fact, we did to the greatest extent we could by picking purpose-built tools to do each of the jobs. And in fact, we went through multiple iterations. As we got closer and closer to the kickoff of we want to begin the copy, we reinvented the system that we built to do copying several times as we discovered kind of quicker, simpler, straighter paths to do that job. So we started out with something that would be dead simple, of distributing jobs over a bucket itself, would store objects and each object would be a file manifest.

**(00:17:21):** And then that file manifest would be available to a bunch of workers. We’d spin up as many workers as we needed to saturate whatever bottleneck we had. So the bottleneck could be our network connection, how fast we could pull things from S3. It could be our write rate to our destination storage. How quickly can we write objects and are we bandwith limited? Is there a metadata limit on the IO operations per second, or is it going to be the S3 read rate? So we can discover all these things.

**Fernando (00:17:54):** Read rate. Why does it have a read rate? They’re just throttling you.

**Jeremy (00:17:59):** Yeah, because S3 doesn’t technically have a cap on read rate, but they do have a cap on what they call bucket partition read rate. And so a normal bucket will only have a single partition and that’s kind of … Well, the way S3 is laid out, it looks like it’s kind of hierarchical folders, but it’s not. It’s actually key names that are separated typically with slashes. So it looks like a file path. And the natural way to partition a bucket would be, let’s take all the first file paths up to the first slash and we’re going to turn that into an internal S3 partition, which is like their own way of kind of sharding the bucket so that they can scale out. So that if you want to do things like write a ton of things to S3, it can be fanned out to multiple partitions.

**(00:18:49):** And as you write more, it’s smart about noticing that write rates are hot on certain partitions and it’ll split them automatically for you all behind the scenes. So you can’t even tell what your partitions are unless you give some kind of cues or use like a traditional slash character, which can help S3 figure out, but also you don’t need to. It’ll just do it for you, but it’ll do it based on your usage. So if you have a moderate usage app, the first time you’re going to have high usage is when you try to copy things out of it. So you’re going to hit the rate limits pretty quickly, especially if you have a lot of small objects. If you’ve got a lot of big objects, you’re going to be bandwidth constrained, probably on your own network connection. If you’ve got a lot of small objects, you can fill that pipe with tons of connections and you’re going to hit the rate limit quickly.

**(00:19:46):** The rate limit is, for the record, 5,500 GET requests per second. So that’s fetching…

**Fernando (00:19:56):** And we hit that.

**Jeremy (00:19:57):** Oh yeah. Yeah.

**Fernando (00:19:58):** Oh my God. That’s insane.

**Jeremy (00:20:01):** So out of the box tools can hit that pretty quickly, especially if you have small objects. We got a little bit lucky because when we did our migration into S3, we worked with S3 to pre-partition our buckets knowing that we’d be writing at high rates. So on the way in, we contacted S3 ahead of time saying, “Hey, here’s about how much stuff we’ve got. Here’s what the key layout looks like for the objects.” And we generally use random hash keys, so it’s like completely uniform. So on S3’s end, the job is easy because when you have a uniform key distribution, they can just say, “Let’s take whatever the first two or three characters of the key in, we’ll use that as partition keys.” So our big buckets were already partitioned, so who knows what the limit could be because when we did a load test, we didn’t hit a rate limit on those larger buckets. On smaller ones, we quickly hit a rate limit in our load testing.

**Fernando (00:21:06):** I know this is mostly about S3, but I’m also curious, did you really hit a write limit? Because David has spoken at length about this, about the hardware, about the amount of money that we spent on brand new hardware that’s blazing fast. Even that couldn’t keep up. We just went full throttle.

**Jeremy (00:21:25):** We certainly hit its limits. So it became a question of where are the limits and which one is … So we’ve got a system with a bunch of components and each component has a kind of maximum, has a cap, and which one is going to be the weakest link, which one’s going to slow us down. And it turns out that we ended up pretty similarly bound by our network connection. We got a hundred gigabit network connection dedicated to just this copying process that thanks to our data center pros at Summit, they set this up just for this job and we set up a separate VLAN for the machines that would be doing the work. So it was essentially their dedicated little network universe of, you can saturate this pipe. Turns out to not have been completely true. We were actually sharing it for a couple other things, which we discovered because as we tuned our system to eke out the maximum possible performance, we actually overshot a little bit and started interfering with other traffic, but that was what we thought would end up being our bottleneck.

**(00:22:33):** And to our wonder, it was not our destination storage. We had initially considered using MinIO and using hard drives and our read and write rate for normal application usage can easily be satisfied by spinning disk hard drives. We didn’t exactly relish the idea of maintaining a bunch of spinning hard drives because the failure rate can be notoriously poor depending on which batch you get and whether you have kind of a hot rack in your data center. And it’s just, we were not looking forward to it. And Eron, our head of ops team, had a line on a new storage system from Pure Storage Flashblade. They’ve got this fancy, super duper proprietary flashy, bashy setup where they’ve got, rather than using off the shelf flash stuff, they mounted their own flash on their own boards and did something a lot cheaper, kind of bringing the kind of flash of two years in the future back a couple of years, which made it kind of cost competitive with hard drives.

**(00:23:46):** So that was a surprise and ended up being a huge blessing, not because we need the performance for steady state usage, but because we needed the performance for the copy. So if we had been on hard drives, we definitely would have been limited by our write rate into that storage cluster.

**Fernando (00:24:06):** By physics, just how fast you can spin those.

**Jeremy (00:24:09):** Yeah. Yeah. And so, I mean, it would depend on the number of drives. And I mean, actually we probably … Yeah, thinking back, I think we probably would be able to satisfy 100 gigabit traffic on hard drives because we’d have so many of them. In any case, it was a blessing, not quite in disguise, but we’re all happy to take the other path. I’m digressing a little bit, but if you’re choosing storage systems, if you’re choosing storage systems, it can seem like make the choice based on what you can afford now, but it’s also what you’re paying over the course of five years or 10 years. And our total cost of ownership analysis was based on five years, seven year, 10 year. What it would be like to keep the system around for a long time. And the power savings alone from flash are significant. It’s a lot cheaper when your power is expensive in a costly data center that uses up less rack space.

**Fernando (00:25:13):** I hadn’t even thought of that. When you go from an actual … I’m just picturing a bunch of machines in a place, but if you’re going to clone five petabytes, it has to be a lot, right? A lot of m achines, a lot of power, a lot of network, a lot of everything.

**Jeremy (00:25:29):** Yeah. Yeah. I mean, it’s a crazy time in the storage world. There’s a bunch of new form factors for solid stage drives coming out right now. So it gave me a little bit of FOMO because I see these things coming out just as we purchase this giant system and we still made the right decision for the time, but in about a year or two, there’s going to be kind of a new generation of solid state drives coming out at 256 terabyte size per module. So you can fit … Shoot, what was it? 40 petabytes in two units of rack space.

**Fernando (00:26:07):** Wow.

**Jeremy (00:26:07):** So you could fit all of our storage into just a tiny little bit of a rack. And in the hard drive era, we’d be looking at like two full racks just for that storage. And so the shrinkage and power savings are dramatic and it’s all happening now.

**(00:26:23):** And a lot of this is driven by AI stuff of… people need a ton of data stored and super high bandwidth to it. And so new vendors are cropping up daily trying to do this kind of job. So they’re also driving the flash hardware side. So hopefully this will become just kind of a commodity storage problem. And you’ll be able to go to Super Micro or Dell or whatever and order up some servers that are packed full of these drives and you won’t need a special setup. You’ll just go to Newegg and…

**Fernando (00:26:57):** And buy one.

**Jeremy (00:26:59):** Yeah.

**Fernando (00:26:59):** Yeah.

**Jeremy (00:27:00):** The limiting factor by far is network bandwidth. So in almost any copy, that’s going to be the cap you’re going to hit. So having a good data center partner is essential there. We were able to get 100 gigabit connections set up within just a span of days. And we had plenty of lead time, et cetera, but still it’s just wonderful to be able to bring in a big pipe like that. And of all things, we have AWS direct connects already, but not allowed. You cannot use your special direct, fast connection to AWS to do the data egress. You’ve got to use the public internet for-

**Kimberly (00:27:45):** Even if you pay money, pay more?

**Fernando (00:27:47):** Oh, wow.

**Jeremy (00:27:48):** Yeah. So you pay money, but to get this other bandwidth covered, you got to use the egress cost. And I guess maybe it’s just tied up in some kind of red tape in the direct … Would they cover the cost of the direct connect? Maybe they can’t account for it. Who knows?

**Kimberly (00:28:05):** Jeremy, I do have a question because obviously we were moving multiple applications or working with multiple applications to make this move. Was there a specific order that you were moving them or was it just you just kind of picked an application and did that? What was the reasoning behind the order for the move?

**Jeremy (00:28:24):** There were two phases. So we chose some smaller applications with less storage, but that would be representative of our applications with larger storage needs that we would migrate before the egress window opened. So essentially we wanted to test the process. We wanted to be prepared that when the window opened, we’d be able to blast. Didn’t turn out to be that way. We had some delays as we optimized and restarted things and whatever, but it was crucial to do that, identify some systems that we could do real life copies and not just kind of test runs. So we chose a couple of representative systems and did those first to prove kind of a blueprint for how we do it for our other applications. And each application ended up being not quite cookie cutter, but once you’ve kind of improvised a recipe a few times, you kind of know what you need to do.

**(00:29:26):** So you come into it with a plan of attack of you need to do dual rights to multiple places, you need to do a reconciliation step, you need to have a well-defined cutover process. And so you come up with a checklist, validate the checklist by doing a live migration and iterate, fine tune, and then you’re ready to go. Then when it comes to the actual copy, we go criticality first. The things that we want to be absolutely sure and the business critical stuff, Basecamp, HEY, all our primary revenue generating apps with the big data. We want to get those started as soon as possible because there’s the most a copy and we want the greatest assurance that we did it all properly. So give us the most headroom for unknown unknowns. We’ve got our known unknowns of things that might crop up, but we also have room for just we don’t know. Who knows what would happen?

**(00:30:27):** And we came up with a bunch of those. So we’re grateful for having started them early. But yeah, definitely dive into the trickiest, biggest thing first.

**Fernando (00:30:42):** That makes sense. I’m still trying to wrap my mind around this. You go to a small app and you’re like, okay, you know what? We need to migrate this. You start building the program that you mentioned, the reconciliation program. Is that program a Rails server on its own? Is it like the modifications that you mentioned to Active Storage?

**Jeremy (00:31:08):** No. So we built a new thing that kind of scales up the basic idea I started with of I need to list things to know what’s in the source. I need to copy everything in that list over to the destination. And at a small scale, I can use a single program. We used one called Rclone. There’s one called RSync. And most folks, technical folks have used one of these before. And you just fire it up, you give it a source and a destination and it churns and it does the job. At most scales, that’ll work fine. And in fact, for AWS, their free bandwidth egress limit is pretty generous, so most people would fit within it. And you could just Rsync and do it yourself or Rclone and call it done. And it does all the bookkeeping for you. At our scale, we needed to fan out to a bunch of workers doing this job.

**(00:32:00):** So we needed to do it in parallel. That means splitting up, batching it up. And so it becomes kind of a classic like map reduce problem. You’ve got a big input, you need to spread out to a bunch of jobs, and then they’ve got all their individual outputs. In this case, it’s take a big list of files, split it up into batches, send those to workers that are going to do the copies. Have those workers have some kind of supervision that’s tracking what they’re doing, their progress, whether there’s an error, retries, all that kind of stuff. That’s where we used a Rails app for command and control, for wrapping up the jobs and the work. We arrived at a Rails app after trying what I thought might be some kind of simpler lo-fi ways of doing things. And of all things, I kind of backed my way into doing, to using Rails because I was missing some of the conveniences of home.

**(00:32:59):** One of them was secrets management, credentials. Here we’re doing something that’s copying between a bunch of AWS accounts to a bunch of destination buckets. You’ve got a ton of sensitive credentials in one application. And so as I was building this kind of script-based simple system, I realized I’m rebuilding a credentials manager. This is not the life I want to be leading right now. And how about I do something that’s already built for me? And so going essentially to Vanilla Rails because of this mildly auxiliary concern. I had a bunch of other kind of pressures swirling in my mind that were resolved by this, so who knows whether my unconscious was also kind of like, uhhhh… I can kind of feel that change is going to need to be made and here’s the thing that just triggered it. And the trigger then led to a bunch of nice outcomes like being able to use ActiveJob and Solid Queue. And a lot of things we’re familiar with for basically how do you distribute this work?

**Fernando (00:34:03):** So in the end, you have a single Rails app that you’re constantly looking doing this window, you’re constantly monitoring like, okay, did it complete everything or are there any retries errors? And then you go and fix them like you say, right? Oh, we hit this limit. Let’s try and work around it.

**Jeremy (00:34:21):** So that’s part of the approach based on the initial migrations as our kind of test runs. As we discovered in the test runs, you’re going to have new errors, kind of surprises crop up. And so we prioritized failing fast and not trying to be resilient and automated way. So not building in things like exponential back off too early because sometimes things were not things we wanted to retry. There were actual errors. So being able to identify something that was truly like a transient failure and then automate it late rather than early. So we treated it kind of like an Andon cord of like, here we’ve got a production line, we notice something’s failing, we pull it, we stop everything, we fix it, and then we proceed from there. So that drove a lot of other decisions of when you break things up into chunks, they need to be observable, they need to be retryable, they need to be supervised.

**(00:35:19):** And especially for a diagnostics and troubleshooting, you need to be able to see what’s going on. So in these cases, you’ve got a active job process that invokes another tool and you’ve got hundreds of these running. And so you’ve got this standard output and standard error from a bunch of tools. You’ve got the exit status. How do you see them? And so this, for me, this was kind of a crucial stumbling block of like, if I can’t see exactly what’s going on, I don’t know what’s going on. And so I don’t want to spend a bunch of time guessing at and troubleshooting. I want to just look at the output and I want to be able to figure it out as if I were running it on my own console. So that was a critical step early on too, of making something that was easy to supervise and just witness.

**(00:36:12):** If something’s failing, I could try it myself, I could invoke it myself, or I could pull up a transcript of what that process had done. So this was the job of the Rails app of coordinate, pulling an inventory or a catalog from the source, split that catalog or inventory up into a bunch of pieces, which was its own whole thing. If you’ve got something that’s huge and you need … Anyway, you can get into tooling in a little bit here, and then make a bunch of jobs for all that stuff. And each job, well, it has its responsibility is its chunk of files and the output for that job is a bunch of things like status and transcripts and whatnot. And those themselves are actually stored in a storage bucket as well. So every job, there’s a unique idea associated with it and you can go inspect the whole process.

**(00:37:11):** And in fact, we did have a live tail. So since these things can take a while, you can visually see what’s going on. You could just kind of snoop on any transfer.

**Fernando (00:37:25):** So you were basically Neo for 90 days?

**Jeremy (00:37:28):** Not 90 days, thank goodness. We ended up getting it down thanks to Pure Storage and the very fat pipe of bandwidth to less than 10 days of transfer.

**Kimberly (00:37:42):** What?

**Fernando (00:37:42):** Wow.

**Jeremy (00:37:44):** Which turned out to be pretty critical because that gave us lots of time to do reconciliation, verification. Yeah, we didn’t know that. And so we had built in a lot more buffer than we needed, but I’m sure I’m glad we had the buffer because I was also going to … I was headed on vacation on sabbatical right after this was going to wrap up, which was not the wisest of career choices, but …

**Kimberly (00:38:11):** Jeremy, tell us a little bit about those 10 days. Is it 10 days just nonstop? Are you breaking … Kind of walk us through…

**Jeremy (00:38:18):** Oh, it’s nonstop. Yes.

**Kimberly (00:38:19):** Yeah. Okay. Yeah.

**Jeremy (00:38:20):** So the setup is I didn’t want to trickle things into a pipe and have to be carefully tending things. I wanted to feed a pipe and have a backlog. And so I can go through some of the technology stuff just briefly. If you’re copying a bucket with billions of objects from S3, use S3 inventory reports. It’s something you can turn on S3 console. It’s easy to do. You got to pay. You got to pay.

**Kimberly (00:38:49):** It’s the theme. It’s the theme.

**Jeremy (00:38:51):** But it is the most efficient, effective way to get a large scale bucket listing without doing the work yourself. It is delayed. The most frequent you can do it is daily, and so they drop on a schedule. And so for this kind of process where you want to do a big bulk transfer, daily’s fine, and particularly if the system that you’re migrating is doing dual writes, you’re writing to both the old source and your new destination, you know that you’re already in sync, so there’s not going to be missed writes. So the thing you do here is you turn on dual writes to both places, and then you take the inventory from the day before you turned on writes. So you know all new objects are being written to both places. So the old inventory is sufficient for knowing that you’re going to get a bulk copy of, and it’s going to bring you into accord.

**(00:39:50):** Everything’s going to be the same. So you start from that snapshot of the bucket. So we used S3 inventory reports. We didn’t have that turned on everywhere. We developed … We looked for another tool that could do something like this. And there are a bunch that take a similar approach. If you try to list an S3 bucket, it’ll take literally days because you need to sequentially list files and it lists whatever, 1,000 at a time or some 10,000 at a time, something like that. Anyway, order magnitude-wise, it’s ages, but there’s some tricky, very clever ways of doing this where you can do it in parallel by estimating what the prefixes of a bucket are. And you can ask S3 for bucket listing starting from a certain prefix. So if you know your key distribution, then rather than doing a single sequential listing, you can instead do thousands of parallel listings for every prefix you’ve got.

**(00:40:50):** So you can turn a multi-day bucket listing into something that takes like 30 minutes. So I had that in my back pocket in case we needed live listings. If we discovered that we were going to be in a situation where we needed to do a downtime or we weren’t able to do dual writes in a system, so you’d need a downtime to be able to stop writes from the old system, but also not write to the new system so you wouldn’t get out of sync. And also to list the objects in the destination buckets because Pure Storage and the Flash Blade product does not have an equivalent to inventory reports. So you’ve got to do the listing yourself. So on that other side, you want to take the inventory report from S3 and then you want to develop your own report of the destination and compare them and any discrepancies need to be accounted for.

**(00:41:45):** So you need a fast way to do that, so there’s a tool called S3 Fast List that’s on our GitHub. We forked it from AWS samples and we adapted it so that it would support non-S3 storage systems so that we can use it to list flashblade buckets. Works great. Very clever approach. Really pleased to find that. We didn’t end up needing it that much, but it was a wonderful diversion and felt like kind of an insurance policy of a special built tool in the toolbox. The next thing was, how do we split these things up? And we started with, gosh, how are we going to take this S3 inventory report comes in either CSV format or Parquet format and parquet being like flooring, it’s like kind of split up and actually I don’t know how far the metaphor goes, but it’s a very efficient format for doing columnner data storage.

**(00:42:51):** So it’s great for analytical processing and whatnot where you know which columns you want to work with and you need to do some transformations on them. And it’s particularly nice for something like this because there’s tooling that can ingest it, that can stream it from a remote destination and operate on it and then emit it again. So took us a while to discover this because I was looking initially for just something like polars. There are different tools that can ingest parquet and operate on it, split it up, whatever, like using a windowing function to … We wanted to do something like split this not just into number of objects, but in total batch size, so that we would evenly distribute batches across machines so that we wouldn’t end up with uneven bandwidth demands. So you wouldn’t have one machine that is working on a batch of a bunch of small objects and it can’t fill the pipe.

**(00:43:45):** What you want is to have an even distribution size wise so that you’re maxing out the pipe on each of the worker machines. So to do that, you need a windowing function that goes through the inventory report and does a cumulative sum on the bite size of the objects. And each time it reaches 10 gigabytes, it says, “Oh, I’m going to do a split right there and I’m going to turn that into a chunk.” And this turned out to be hard, use a lot of memory and kind of work, but ended up maxing out the memory on a machine I was using. And it’s like, “Well, okay, this is probably, might be feasible, might not be feasible.” And then I discovered DuckDB, frigging awesome. DuckDB is amazing. I cannot sing its praises enough. It’s like somebody discovering SQLite for the first time, although DuckDB is like SQLite on whatever next generation steroids, because it can even do the stuff SQLite does maybe better because I’m just glowing with its capabilities, but it can do SQL, it can like connect to remote databases, it can work with a local database end process just like SQLight.

**(00:45:03):** It can work with CSV files and parquet files on the local file system. So most kind of big data, data science stuff, DuckDB can do locally on a single machine and super efficiently. It’s really smart about spreading out IOs to do things as smartly as possible to try to avoid doing things like bringing everything into memory. So I’ll sing this praises a little bit more.

**(00:45:34):** Not only can connect to everything like a Swiss Army knife of data analysis, it can also connect to remote URLs and to S3. So I had this whole system built of ingesting data from S3, downloading it, and then staging it on local file system, and then doing the splitting myself using my own tooling, and then storing those split files in another bucket as a staging area for jobs that would then be dispatched to work each of those chunks. Turns out I could skip all of that. With one DuckDB invocation, I can point it at a glob that’s referencing multiple S3 files. So the inventory report is split up into hundreds of files. It can reference all those files, stream them all in, partition them the way I like, and then write them to a remote S3 compatible file store, all streaming. So there’s no local file system, there’s no other code I need to write.

**(00:46:36):** It’s just you got to configure it properly, you got to know what you’re doing. But when you get it working, it’s like, “Oh God, yes, this is sweet.” You’re working with the remote thing, streaming it all through, not using a bunch of memory, and then writing it out to remote storage. And then I was able to take the files that had been written and I kind of wrapped Active Storage records around them. I said, I’m going to make Active Storage records that point to where those batch files had been stored. And then I distribute those active storage records out to the jobs to work.

**Fernando (00:47:13):** That’s a benefit that you were in the Rails app, right?

**Jeremy (00:47:15):** That is a benefit of the Rails app, yes. Yeah. And I had that abstraction to work with. And thankfully, Active Storage was able to accommodate this where I was kind of going behind its back because I was using DuckDB to write the files rather than using Active Storage. And I said, haha, Active Storage, I’ve got these files. Can you make use of them? And it’s like, “Yeah, of course I can do that. “ So you just feed it the key of where it’s stored and Active Storage will be happy to work with it as you bring it.

**Fernando (00:47:39):** That is so cool.

**Jeremy (00:47:39):** So any case, DuckDB, amazing, able to partition the problem and kind of eliminate a whole step of what would otherwise need to be custom code.

**Fernando (00:47:51):** It just comes on to show you that nothing is new. How did the DuckDB guys know like, “Oh, you know what? What process would be really nice if you take this, partition it and then put it this way?” That’s so, so cool. I love that.

**Jeremy (00:48:07):** Yeah. The thing in common, people’s resource constraints, everybody’s constrained in similar ways and they all have different problems, but they’re all stuck in similar ways. And here somebody comes along and solves it elegantly and does it with open source.

**Fernando (00:48:18):** Very cool. And this process, this had to happen once a day because of the limitations of the inventory. Am I right?

**Jeremy (00:48:27):** So I did end up automating it.

**Fernando (00:48:31):** Of course.

**Jeremy (00:48:32):** But it can happen at most once a day. So I made a scheduled scan for new inventory reports and I automatically process them so they’d be ready if I did choose to use them. And I only did this because it turned out that partitioning was so easy and cheap now. I had anticipated it being like a pretty slow process. And so I wouldn’t want to just be firing it off all the time. I’d want to choose which specific inventory report I used and it would take hours or who knows how long to do the partitioning, but now it’s a matter of minutes. So I’m like, well, I’m just going to do it, make it easy to choose which inventory report I want to use as a sync, as a copy source. So from the app dashboard, I could have a list for every app and every AWS account in every bucket, which things were transferred using which source manifest. I’ve got a bunch of inventory reports. For each one, I can see that I’ve kicked off a copy. I have state tracking for every part of the process of partitioning to copying to errors to reconciliation.

**Fernando (00:49:44):** Wow.

**Kimberly (00:49:44):** As a non-technical person here, I do have a question because all of this sounds very hard and scary. I’m curious, Jeremy, what was the most nerve-wracking part of this process?

**Jeremy (00:49:55):** Deletion. Yeah, the final deletion. I mean, you know, I mean, it’s like anything that’s high stakes, your brain’s got a lot of things going on. My brain’s got a lot of things going on. I’ve got feelings, sensations, I’ve got some kind of cognitive whatnot that’s blinking on and off sometimes. And some parts are telling me like, “I know that things are fine, but anxiety’s telling me,

**(00:50:17):** Maybe you should discover why it’s not. “ And those things need to work together. And I can use my anxiety as a guide that maybe I haven’t figured everything out, but I could then use my cognitive process of here’s the things I’ve worked out, I’ve ruled all these things out and I have some standards of proof. I can demonstrate conclusively in a way that is not dependent on my anxiety, that can be externally verified, that it worked and that I’m done. Nonetheless, deletion is still dicey, but when you do press delete, then it’s, I mean, oh… Somebody else take the wheel. It’s just all happening now. Yeah, right. There’s no going back. We’re now doing deletions. Yeah. Coming up with the biggest unlock feeling was adding kind of a belt and suspenders step, you know, keeping your pants up you want more than one way. When you’re verifying and reconciling, you want to be a little bit more than sure.

**(00:51:23):** And so thanks to Pure Flashblade’s extraordinary metadata read/write rates, we can do hundreds of thousands of metadata operations per second just without breaking a sweat. It made it easy to do rather than doing reconciliation against inventory reports, to do reconciliation by doing a live sync. And so with an inventory report, you’ve got lag time between the listing and kind of the live state of things. Whereas with a live synchronization, you can see exactly how many objects were needed to be copied and the size of them. And so you could essentially do repeat copies fairly cheaply until you can see that everything is done. And it really helped just psychologically and as a matter of certifiable proof that you get the final copy that says nothing needed to be copied. Everything was up to date and that typically came after, there’s another key step of when you’re doing dual writes to a previous source and the new destination, you turn off dual writes and you go to single write just to your new storage destination.

**(00:52:49):** And then at that point you know only your new one is new. And if you did need to roll back, you’re kind of like, ah, if I did need to roll back, now you’re out of sync. Now you need to copy the new stuff back to the old place. So that happens after you get your green, everything was cool, nothing new needed to be copied. Turn off the dual writes and a final sync and double verify. Nothing is changing. Nothing is accidentally writing. So I did a bunch of other, more than just belt and suspenders also, whatever else, contraptions you couldn’t imagine keeping your pants up. One of them was just changing the permissions on the S3 buckets. Some of these, we had old systems that had multiple things writing to them and you just, you kind of know, but do you really know? And one of the ways to be sure is to just turn off writes.

**(00:53:42):** So if something was writing to it, it would error. So that’s kind of the final straw of assurance. When I go to delete this or I go to turn things off, I don’t have some straggler that’s going to surprise me. And it was only writing some hours of the day or it was on a cron job or something, so I wouldn’t have caught it in the initial sync.

**Fernando (00:54:04):** My hands are sweating and I had nothing to do with this. The amount of … Okay, the million dollar question, was there any downtime?

**Jeremy (00:54:13):** There was no downtime. Everything worked and it was really quite wonderful. There were some things we broke, so I suppose-

**Fernando (00:54:24):** We don’t need to talk about that. No, I’m kidding.

**Jeremy (00:54:25):** So the 100 gigabit link was actually on a shared link with some other things that we’re using a portion of the bandwidth. And so when we pushed over about 80 gigabits, we started kind of impinging on some other stuff that needed to not be impinged on. And so we did cause some errors elsewhere, but the copy was fine.

**Fernando (00:54:53):** Suspenders worked, right?

**Jeremy (00:54:54):** Yes.

**Kimberly (00:54:58):** Jeremy, question for you. Now that this is done, like it’s tied up with a bow, looking back, are there things that you’re like, “Oh, I wish I’d done this differently?” You can say no.

**Jeremy (00:55:08):** No.

**Kimberly (00:55:09):** Okay.

**Jeremy (00:55:09):** No. Yeah, I’m pretty happy with how things worked out. I appreciated the incremental approach to starting simple and focusing on the epicenter of, what I thought was the epicenter of the problem because I didn’t build too much as I discovered that the true epicenter was in verification, reconciliation, inventory management. It’s about how do I track what’s going on and the copy itself was fairly simple. I did rabbit hole a couple of times. I built out a live view system that I didn’t actually end up using much, but in key times I did. So it’s a kind of like hindsight bias of like I didn’t need to do all that, but I kind of did to discover that I didn’t need it. It’s a little circular, but it allowed me to diagnose and troubleshoot things that were blockers that would’ve been really hard to work out otherwise because it’s things that turn into like little Heisenbugs of, if I run this myself outside of supervision, it works.

**(00:56:11):** But then in my supervision framework, something breaks. And it’s stuff like I’m opening a pipe and I’m feeding things to standard input and I got another pipe that’s reading and like a pipe could get wedged because somebody hasn’t read from it frequently enough for something like this and it can manifest as some other kind of error. Anyway, having that kind of visibility was really helpful, but looking back, I could probably delete it from the app we built.

**Fernando (00:56:43):** My question would be, I mean, you are an eminence within the Ruby on Rails community. Let’s assume I’m just an average Rails developer. What is the complexity of the project I can take on from moving out of history to our own hardware?

**Jeremy (00:57:04):** You could take on this whole project. And this is one of the … And the magic of this is that a lot of the feeling of criticality is the business criticality. It’s not technical difficulty. It’s a modeling problem. And there’s some tricky things with process supervision that Ruby doesn’t make super easy, but it’s not bad. And there’s plenty of other kind of worked examples you could start with. And otherwise, the degree to which Vanilla Rails just works is quite gratifying. And in particular, well, Kamal also, in fact, leading into this as part of figuring out our upper bounds was doing S3 load testing against our flashblade. So we got this new S3 service. What can it actually do? Well, there are load testing tools out here that can do that. And I used Kamal to deploy one of these tools out to a bunch of nodes and hammer it as much as I could. Worked great.

**(00:58:06):** And I used Kamal to deploy our copying application called Nostos and worked fantastically too. And I was able to use accessories, Kamal Accessories to stand up the database that did all the state tracking, do open telemetry observability, do logging. It was all just single system, like a single developer pushing to some VMs somewhere. So it’s all kind of bog standard stuff, but it’s being used in the employment of a kind of critical operation. But again, the criticality is all in our heads. The actual app is fairly simple.

**Fernando (00:58:50):** You mentioned DuckDB as being like, wow, an incredible tool in your arsenal.

**Jeremy (00:58:56):** I cannot sing as praises enough.

**Fernando (00:58:59):** Are there any other tools that were completely necessary in this process?

**Jeremy (00:59:03):** Yeah, I sung Rails praises a little bit. Vanilla Rails turns out to be the way to go. And I don’t mind tooting that horn a little bit. This other tool, S3 Fastlist, was like a little pleasant discovery, diamond in the rough. In our clone itself, what we did to do the heavy lifting copies was incredible. It did all the stuff we needed. It’s open source. It was easy to contribute to. In fact, as part of this, we added a flashblade, an official flashblade destination to our clone. So when you go do an R-clone of your own, and it’s got this nice kind of interactive thing where it asks about where you’re coming from, where you’re going to, and now Pure Storage Flashblade is one of the places you’re going to. And what that is, it’s essentially like a list of characteristics of the system so that Rclone knows how to best do its transfers.

**(00:59:57):** There are certain quirks and asynchronicities with different S3 compatible file stores and Pure Storage has some of them, and now it’s just all set up out of the box where you don’t need to go figure out the command line flags yourself.

**Fernando (01:00:14):** That change was upstreamed.

**Jeremy (01:00:15):** Yes, that’s part of Rclone now.

**(01:00:19):** The other key thing is that Rclone is bandwidth and metadata operation efficient and it’s oriented around resilience first. So you can operate it in a bunch of different ways. You can kind of back off on the kinds of checks it does, but it does nice things like pull the checksum from the source and check it against the destination. And it can even do a kind of extreme check where normally you would write the file to the destination, you get a checksum back and you compare that the checksum that it says was written is what you had. And you say, “Okay, cool. It looks good.” But you can also do it in kind of really be careful mode, which is write to the destination, get the checksum, then download it from the destination and actual checksum of the bits. So it’s got you covered for every degree of risk mitigation you want to have at play.

**(01:01:16):** So if you don’t trust your destination yet, because funny things can happen where like a bit gets flipped on a hard drive or a gamma ray hits something and messes something up where it says that you got the checksum you expect, but it turns out on disk, there was a modification. So depending on the level criticality of your data, how many infinite nines you’re going for after that decimal point, Rclone’’s got your back. I’m just going to sing his praises a little more. Sorry.

**Fernando (01:01:42):** Yes, go for it.

**Jeremy (01:01:43):** So there are certain things you can be very efficient with Rclone where you can skip operations that don’t matter. And if I don’t want to do a bunch of metadata operations against S3, like checking last modified time, whatever, things that are not present in the S3 file listing or the inventory report that would normally need to make a head request against S3, which you got to pay.

**(01:02:08):** I don’t want to go do that and I don’t want to bottleneck on making those calls to S3, because again, there’s just adds to the rate limit bucket. And on the flip side, if you don’t want to do excess metadata operations on the destination, you can tune Rclone to your heart’s content.

**Fernando (01:02:26):** I was going to ask, did we at any point consider doing random statistical analysis with like a full, give me the bytes back, do the checksum here.

**Jeremy (01:02:36):** We did.

**Fernando (01:02:37):** Okay.

**Jeremy (01:02:37):** We did. So we did some of the copies with the full pull and we didn’t run into any issues. We just did kind of a grab bag sample. I just manually ran some of the batches just to satisfy myself.

**Fernando (01:02:51):** Yeah, because I mean, you could do it for everything and we completely quote unquote completely sure, but it would be insanely expensive, insanely time consuming, but that makes sense.

**Jeremy (01:03:00):** Yeah. And it would suck a lot of our bandwidth. So all the bandwidth we want to be using exclusively for writes, we don’t want to be eating it up on reads, verification reads. Whereas metadata operations, they don’t use much bandwidth. They just eat up CPU time on the flashblades. So we were able to calculate all that out too, depending on the number of, kind of, compute cores in the flashblade cluster, how many metadata operations you can possibly do concurrently. So essentially we wouldn’t flood it. We would edge it just a little bit over what it ought to be possible, what ought to be able to do to keep it full utilized.

**Fernando (01:03:43):** That makes sense. I want to go back to the open sourcing. I feel it’s such a great part of this, of 37signals that we’re both contributing to open source in the patch that went out. But I’m curious if we are going to open source our tool, like this tool that you built.

**Jeremy (01:04:05):** I did build it with a mind toward open sourcing, and part of that was to … It’s almost like a kind of a design discipline that I’m not going to make something like too bespoke. And there were some decision points along the way. At one point, it’s easy for us to do an NFS mount, for example, and to use an NFS mount to share files, but it’s not terribly different to use an object store and to use Active Storage. So in a case like that, I erred on the side of using Vanilla Rails of using object storage rather than developing a different kind of file storage backend. And if I was just doing batch scripts, I would probably do an NFS mount, but as soon as I moved into Rails, it’s like, let’s just do it all the Rails way. And once you’re doing things the Rails way, it becomes almost hard to do it in a 37signals specific way.

**(01:04:58):** Of the things you hard code in, it’s easy not to. And things like where your credentials go, well, goes into a separate credentials area and things like AWS accounts and credentials are modeled in the database. I used Active Record encryption to store credentials in the database so they’re not part of the repo. It’s part of your onboarding process. You start with a blank app, you add AWS accounts, it takes your credentials and goes and scans for all the buckets you have and imports them into the app. And on a regular basis, it can go pull them and then you can start copies from that stuff. And similarly, you set up destinations and just the same way. So you can support different kinds of sources, different kinds of destinations, perfect setup for being a general purpose tool. As we got closer to the end, I did start specializing some of my design decisions based on the phase of the copying that we were in.

**(01:06:00):** So I started tuning the dashboard to reflect what I needed to know. And that sacrificed a little bit the things that are important earlier on. So as it got into like, “Okay, I just need this from me, I’m going to totally revamp things right here.” Now the UI is, it’s a little bit narrower and purpose built for my needs and not everybody else could necessarily understand it.

**(01:06:26):** So in any case, yeah, it’s very open sourceable. We’d like to do it. I would like to do it. The investment in doing it is like, okay, got to tease apart some things. And so one possibility is sharing it kind of like an artifact of here’s what this look like frozen in time at the end. And if you wish to take it and adapt it and turn it into something, because this is certainly not our line of business. We’re not going to make a product out of doing this.

**Fernando (01:06:52):** That’s right.

**Jeremy (01:06:52):** So I’m not going to spend a six-week cycle of work on polishing up something that then we end up being open source maintainers for. It’s like, no, I’m not going to … So I’ll share it, but I’m not going to maintain it.

**Fernando (01:07:03):** That’s awesome. And what do you feel are the next steps now that the full transfer is done, you deleted everything? Where do we go from here? What about backups? What about …

**Jeremy (01:07:18):** Yeah. So it’s all the other stuff. This is all the programming side of … I drove the copies and the transfers from the application and the software looking out. Our awesome ops team and Matt and in particular did all the operations side of how do we stand up these storage systems on the back end and feed them and keep them well fed. And now that we’ve got all the bits on a disk somewhere, how do we make sure we don’t lose them? It’s all the standard kind of data reliability stuff. And so we run through the cases of like, what can happen, what are the risks we need to mitigate? And things like data loss on a drive or losing a system, losing a power supply. Each of these things has its own kind of redundancy and that’s all on the system side. And there are other kinds of redundancy concerns like what happens when a truck backs into the power transformers at a data center and takes the whole thing down.

**(01:08:20):** Well, we’re not going to lose our data, but it’s going to be unavailable. So then we got the availability problem. Well, we’ve got a second site, which is our backup of the first. It’s kept in sync live and it lags in a sense that we don’t write to it directly. So we’re insulated from things like software bugs in our applications. If we accidentally delete something like crap, we wrote a bug that deleted some stuff. It’s like you need a backup. It’s not sufficient just to have like high durability. You also need to be insulated from other kinds of mistakes. And our second site is essentially that. We’ve got another storage system that similar size, similar class, similar write rate, and we replicate from the first to the second. So if there is an issue availability wise or durability wise, we’ve got a place to go and we’ll just flip the applications over to use the second one.

**(01:09:17):** Now, that’s not the whole story. For backups, there’s the kind of age old, three, two, one rule where you’ve got three copies of your data on two different kinds of media and one offsite. It’s a little bit rusty around the corners and the age of cloud stuff and particularly with modern flash because the old school thing was typically hard drives and hard drive failures and your other media would often be tape.

**(01:09:47):** And these days, that picture looks a little bit different. What is different media really? Is it other kinds of flash? Is all flash one kind of media. You need sufficiently different characteristics that if something catastrophic were to happen to one mode of file storage, your other mode would not be affected by the same risk factor. So we’re looking at doing a third site with hard drive storage or different kind of flash and acting as our insurance policy. And the other factor that we’re pulling in here is that our two sites are using the same vendor. So we’ve been very happy with Pure’s Flashblade product, and we had similar resource constraints, both in power and cooling and rack space in both data centers. So using Pure for both made sense and we’re able to smush them together into one contract, just sensible step, but that’s a single vendor and we run the same operating system.

**(01:10:47):** We do stagger upgrades, but if there’s a bug in the system and we deploy to both systems, well, we essentially have a single point of failure. We don’t have two systems anymore and we’ve got one operating system and the same bug affecting both. So what we’d like is a third system with a different vendor. So we’re looking at using MinIO, the kind of open core storage S3 combatible storage system. They’ve got a dual license where you can use MinIO free or you can get a support plan with them. And it’s what we had been considering using before we discovered Pure. So we’ll be going back a little bit and having not just a second vendor, but also going to have an open source fallback. That’s our insurance policy against things like storage vendors being acquired. So that is often the final destination that’s pushed us off of other vendors in the past of you got bought by some big company and now what do we do?

**Fernando (01:11:46):** Wow, that is a lot of things to consider.

**Jeremy (01:11:48):** I will say the other storage medium that was kind of a romantic diversion for quite some time was using tape for real. Back in the day, I operated tape libraries and there’s something that’s just kind of a retrofuture tech satisfying about seeing a literal robot going and grabbing a tape from a library and putting in a little drive and like, “Oh, I’m going to copy my stuff onto there.” And it’s super durable. You don’t need power for tapes just sitting there and I mean, their lifetime is like 30 years and you can go put them in whatever salt mine somewhere and you’re good. And you could pack up your tapes and you could carry them in luggage if you wanted to. There’s just a lot of aesthetically pleasing characteristics about it. So we really tried to make this work because you could get a whole multi-petabyte storage system going for like less than a hundred grand, which is as far as like capital outlay, it’s like fairly cheap per terabyte, but devil’s in the detail.

**(01:12:57):** So like these tape systems are built for older school systems that have a directory, like a file system that you can scroll through to see what needs to be backed up. Object storage is a little bit different. To figure out what needs to be backed up with object storage you need some kind of gateway, which pulls new objects into a scratch space and then backs those up to tape and keeps a catalog. And there’s just a whole finicky kind of additional system you need. And then you’d have the troubles of like, how do actually restore from tape? Well, the only case it would be in is like a truly end of the company kind of scenario where some kind of nuke has hit and you want to recover things and it’s going to take weeks to recover from tape. So yeah, it’s truly an insurance policy at that point.

**(01:13:49):** And like, well, it’d be an aesthetically pleasing insurance policy and one that’s fun to consider, but it’s not actually going to work. Dang it.

**Kimberly (01:13:57):** Jeremy, thank you for joining us. This has been Recordables, a production of 37signals. To learn more from our technical team, check out the developer’s blog at dev.37signals.com.

* * *

- [Other posts in the “RECORDABLES” series](https://dev.37signals.com/series/recordables/)
