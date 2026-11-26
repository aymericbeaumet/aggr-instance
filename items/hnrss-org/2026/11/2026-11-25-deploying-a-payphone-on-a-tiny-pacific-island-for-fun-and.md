---
title: Deploying a payphone on a tiny Pacific Island for fun and zero profit
link: https://nickvsnetworking.com/deploying-a-payphone-on-a-tiny-pacific-island-for-fun-and-zero-profit/
source: hnrss-org
published: 2026-11-25T22:12:25Z
updated: 2026-11-25T22:12:25Z
first_seen: 2026-11-26T12:13:26.788658858Z
authors:
- qzervaas
labels:
- australian telco
- history
- payphones
- voip
content: extracted
html: 2026-11-25-deploying-a-payphone-on-a-tiny-pacific-island-for-fun-and.html
remote_preview:
  url: https://nickvsnetworking.com/wp-content/uploads/2026/11/WhatsApp-Image-2026-11-20-at-07.59.37-2-604x270.jpeg
---

For the past few years Australia’s Telstra has run a campaign of “Free calls to Santa” from the universal service obligation funded payphones across the country – It’s a really fun idea and we wanted to replicate it on our network on Norfolk Island where we run the mobile network, but the’s no payphones.\*

\* There is now a payphone – You probably already guessed this.

## Getting a Payphone to a small island in the South Pacific

Step 1 was getting a payphone.

Those who have seen my office / shed / studio / the HackHouse, know that I may have one or two payphones… But this needed a payphone that could stand the elements and be modern ish.

The answer, is of course, to use the sexiest payphone of all time; the *Nortel Millennium*. Featuring VFD display, durable keypad, card service, coin service, no fancy metering line required.

Unfortunately Nortel’s bankruptcy in 2008 meant I couldn’t phone up an account manager, but with the hookup of some guy with a U-Haul van full of Millennium’s in the US and a bit of a road trip, Cody from our team managed to pickup some really clean GTE branded payphones for the job.

Then Cody flew to Sydney where we drove to the house of someone who was heading to Norfolk the next day to explain they’d need to leave all their baggage at home as all of their baggage allowance was to be taken up with the weight of a payphone – Thanks Paul.

## Programming

Eventually with the Payphone on Norfolk, we needed to get it programmed; for that there’s Howard Harte’s great `mm_manager`, which, with the aid of a USB modem is able to program the phones.

Sidebar: I also got this working with a softmodem (A fork of [D-Modem](https://github.com/cryan209/D-Modem) with [some minor tweaks I’ve documented here](https://github.com/nickvsnetworking/D-Modem/commit/4b0f1a6c2586ac2eef08912653b472253d9832e8)), so we could do it via straight SIP without the USB modem required – but the USB modem was the easier path.

The [Philtel blog post covers how to setup your Millennium](https://philtel.org/2023/09/24/resurrecting-a-nortel-millennium.html) if you’re looking to play along at home yourself.

![](https://nickvsnetworking.com/wp-content/uploads/2026/11/image-4.png)

I hit a bit of a stumbling block with `mm_manager` as the software assumed you were part of the North American Numbering Plan (NANP) – as one of the literally *dozens* of people who live outside of America, having to put a `1` in front of the number wasn’t going to fly.

After some trial and error I sussed out what bits on the config files that get pushed to the phone controls this and [submitted a PR](https://github.com/hharte/mm_manager/pull/32) to `mm_manager` which is now merged.

We’re also able to set the messaging that appears on the screen, which I’m very happy with the branding on.

We set it for free-on-net calls, so anyone can call any Norfone number without charge and the Payphone tag number.

## Talking to Santa

Once this would’ve been the hard part, [I did some work a long time ago with IBM Watson](https://nickvsnetworking.com/tag/ibm-watson/) and built some stuff to annoy voters via phone with automated phone calls and capture their human responses and sort them and play the appropriate response. But tech has come a long way since 2019, and this was the easy part of the project.

We setup the payphone to make HOHOHO (464646) a destination that did not require coins when we did the rating table setup when programming the phone.

The ATA the payphone is connected to just comes through to our IMS like any other fixed line subscriber; the TAS is setup to zero-rate the destination and then it’s dialplan forwards it to “Santa Bridge” listening for SIP on port 5090.

I’ve got 4 pre-recorded “Norfolk Specific” greetings to buy us time to get everything setup in the background, once these greetings play (each ends asking the caller their name) then we feed into the OpenAI API.

One of the members from our team attempted to ask Santa to write a Python script to iterate over a `for loop`, while another team member asked Santa for firearms. In both cases it did a good job of redirecting.

[All the code for this is on Github.](https://github.com/nickvsnetworking/santa-bridge)

## Branding

The phone came GTE branded, so we needed to redo the card with the branding on it that goes around the screen. Two screws and some wiggling later we pulled out the card, and then I badgered my partner into using their day off to design a new one, with a retro style I really like.

![](https://nickvsnetworking.com/wp-content/uploads/2026/11/image-1.png)

![](https://nickvsnetworking.com/wp-content/uploads/2026/11/image-2.png)

![](https://nickvsnetworking.com/wp-content/uploads/2026/11/image-5.png)\
This one is on the wall in my house, but gave it the same branding – And this phone number does make the one on Norfolk ring if you want to speak to someone confused as to why there’s a payphone ringing

## Installation

The install was beautifully completed by long-time-reader-of-the-blog Minas from Shield Electrical and Air.

He knocked up a housing to *Shield* it from the rain and elements and installed it outside the office entrance.

![](https://nickvsnetworking.com/wp-content/uploads/2026/11/WhatsApp-Image-2026-11-20-at-07.59.37-1.jpeg)

## Getting it on Payphone Tag

The last step of the puzzle was making the phone so it could be captured by cult game [Payphone Tag](https://beta.payphonetag.com/news/a-phone-on-norfolk-island) – I reached out to Alex and gave him the info, [and now it’s there in the map for anyone to capture](https://beta.payphonetag.com/?payphone=14070).

## Future payphone adventures

So with this now in place, we’ve still got to give it some tinsel and Christmas cheer, and I’ll get some more pics of it in place.

We’re waiting for someone to claim it on Payphone tag, and we’ve shipped 3 of these to another even more remote Island I’ll hopefully be able to share more info on later.

If you’re ever on Norfolk, give it a call.
