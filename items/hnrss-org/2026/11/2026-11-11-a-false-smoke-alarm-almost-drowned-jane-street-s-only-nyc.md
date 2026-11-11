---
title: A false smoke alarm almost drowned Jane Street's only NYC datacenter
link: https://michael.bacarella.com/2026/11/10/false-smoke-alarm-jane-street-datacenter/
source: hnrss-org
published: 2026-11-11T16:38:44Z
updated: 2026-11-11T16:38:44Z
first_seen: 2026-11-11T21:51:32.649893059Z
authors:
- mbac32768
content: extracted
html: 2026-11-11-a-false-smoke-alarm-almost-drowned-jane-street-s-only-nyc.html
remote_preview:
  url: https://michael.bacarella.com/assets/mbacface.jpg
---

Published on 10 Nov 2026 by mbacarella

A war story from Jane Street's systems team: a false smoke alarm, a datacenter with no cooling, and a sprint to pull every power cord before the sprinklers went off

AI disclosure: this story was written entirely by human hands, with AI doing proof-reading and light edits. Some tasteful imperfections kept to heighten Wabi-Sabi effect.

Everyone’s heard of how awesome Jane Street is. Fewer people have heard about the times something stupid nearly put it out of business and it was saved by grit and dumb luck. I was on the systems team for some of those. Jane Street didn’t do titles, and we on the systems team weren’t above getting our hands dirty. You could think of us as the firm’s blue-collar OCaml programmers: when something broke, we were the ones running through the corridors to fix it. Call this story a Star Trek: Lower Decks view of Jane Street.

For the longest time Jane Street insisted on having its machine room close to its trading floor. When it was 10 people it was cute. When it was 150 people it was starting to feel… odd. Part of the reason for this was intellectual property concern: they didn’t want to risk having their servers stolen by spies who would learn their precious trading strategies. Another reason was technology risk: they wanted to make sure out-of-control automated trading systems could always be shut down by running across the hall and pulling power cables.

Yes, tools existed to mitigate both of these fears. But Jane Street moved irritatingly slowly on this stuff, and they still wanted the last-resort option of yanking power cords. Their risk culture didn’t exclusively come from reading heady thinkpieces. A lot of it was instilled by a partner who’d served on a nuclear submarine, and it showed, mostly in a good way! On a sub, you don’t trust the automation unless there’s a big red-painted, mechanically connected lever you can pull to turn it off.

As the company grew this humble machine room was built out into a respectably sizable datacenter, complete with an online UPS, Computer Room Air Conditioner (CRAC) units, refrigerator-sized network switches, a subterranean diesel backup generator and about 10 rows of server cabinets, 10 cabinets deep. Oh, and it was all crammed into the middle of a skyscraper called [1 New York Plaza](https://en.wikipedia.org/wiki/1_New_York_Plaza), which was otherwise just a normal office building.

When I first landed my job at Jane Street I was commuting to the southern tip of Manhattan all of the way from Queens, an hour or more each way. I’d gone to a magnet high school in the city, so I’d been doing commutes like that since I was a teenager, and I was tired of it.

Now that I was making big Wall Street money I decided I’d get an apartment. A real estate agent kept trying to show me what they called “panty droppers”, but I was only interested in one thing: proximity to the office. I ended up in a 200 square foot studio that was a 9 minute commute on foot from my apartment door to my desk on the trading floor. I would weirdly boast about it to the rest of the systems team, but it also meant everyone at the office knew I was a stone’s throw away in case there was an emergency.

And that day came soon enough. One evening after work, just as I was about to climb into the elevator at my apartment building, my phone rang. It was our Hong Kong office, and when Hong Kong had a New York problem, mine was (now) the number they called. The smoke alarm in our NYC datacenter had gone off.

We had contractors working on a prep room next to the datacenter. While cutting into stuff, some dust was kicked up and it set off the smoke alarm. This is bad, but not for the reason you might think. Office buildings have sprinklers in them for fire suppression. You may have seen it go off in movies where someone just holds a lit cigarette up to the sprinkler on the ceiling and it floods the entire room. In reality they’re not quite that dumb, but it’s close.

Firstly, the sprinklers over a machine room like ours usually aren’t kept full of water all of the time. This saves you from having to deal with any leaks that might develop overhead over the years. Secondly, the sprinkler heads don’t have smoke sensors. Instead, a different system detects the smoke, and that’s when the pipes fill with pressurized water, but they don’t spray yet. What triggers the spray is heat: each sprinkler head has its own heat-sensitive plug, which opens once the air around it becomes hot enough. The logic is that the smoke alarm must be real, whatever fire triggered it hasn’t been brought under control since it’s gotten very hot, and the sprinklers should douse it with water.

Of course, the ceiling of our datacenter was full of sprinklers set up exactly this way. We’re still fine because even though contractors might have set the smoke detector off by kicking up dust, there’s no actual fire producing heat, so the sprinklers wouldn’t spray, right? Well, no. One final piece of this contraption is that if the smoke alarm detects smoke, it would be absolutely terrible to keep circulating air through the room since that would help feed any fires, so the smoke alarm is wired to shut down the CRAC units(!)

Can you see the problem now? Rooms full of computer racks have high cooling demands. If you shut down the cooling, the temperature starts climbing. If you have a bunch of sprinklers that have just become pressurized because of a smoke alarm, you now have a serious, potentially business-ending problem on your hands.

And so my mission was made clear: as fast as I possibly could, get back to Jane Street, get into the machine room, and pull the power cord on every device in that room before the temperature climbed past 150 F and the sprinklers destroyed everything.

I ran down the (ironically named) Water Street in the Financial District faster than probably anyone has ever run down it, mashed the elevator button in the lobby of 1 NYP, waited very impatiently for the elevator to climb the 33 floors, slammed my keycard against the reader, booked through the main office door, took a hard right turn to the machine room door, slammed my keycard against that, stepped into what felt like a sauna and went row by row, cabinet by cabinet, yanking power cables out with both hands.

Then I watched the temperature monitor. 141 F. 142. 143. 143.5. 143.6. Finally it stopped. A quiet I’d never heard before set in on the machine room.

The machine room had been kept across the hall so that someone could always run in and pull the power cords. I doubt anyone had pictured doing it to save the servers from the goddamn sprinklers.

Since the major NYC markets were closed for the day, the downtime from this fiasco was minimal, and we were up and running the next morning.

There were a couple of meetings after that. You might picture Gordon Gekko coldly deciding whose career was over, but meetings at Jane Street were never like that. They were matter-of-fact, level-headed and honestly kind of fun, closer to engineering students in a college dorm arguing over what went wrong. The outcome of all that level-headed discussion: the sprinklers would stay, we would tighten up procedures and surveillance, and we would continue hosting servers in our office building right next to our trading floor. That is, until Hurricane Sandy smacked us upside the head, but that’s a story for another day.

As for me, when the next morning came around I knew what had to be done. Our Hong Kong team had only caught the problem by luck: someone happened to see on a display that server after server was popping up a climbing temperature condition. No one server was configured to alert just because it was a little hot, since that happens from time to time. But an across the board increase in temperature on many servers means there’s a generalized cooling failure.

What we needed was a system monitor built for this exact purpose, one we could trust to be ultimately, unfailingly reliable at the task of asking each Linux server for its temperature and computing a simple mean. Bash wouldn’t cut it for this. Nor would Perl. No, a job of this magnitude called for the most reliable and type-safe programming language imaginable. So I fired up emacs and wrote it in good old OCaml: every minute, ask every server in each location directly for the temperature from its onboard sensors, take the average, and if it’s elevated, blast an alert out to everyone on the systems team to check for datacenter overheat.

Yes, we could have also alerted if the CRAC units shut down or if a dedicated temperature sensor in the room was high, but the telemetry for this stuff was nowhere near as reliable, and unknown unknowns can still ruin you, so the most durable thing sometimes really is to ask your fleet of Linux boxes for their temperature and freak out if the average is just a little too high.

Our team didn’t interact much with the partner with the nuclear sub experience, but I’d like to think he would have approved.

*Got a better tale of woe? Let me know at [X](https://x.com/mbacarella/status/2120554818486104552), [Bluesky](https://bsky.app/profile/michael.bacarella.com/post/3n22khjlui223), [Hacker News](https://news.ycombinator.com/item?id=50455394) or [Lobsters](https://lobste.rs/s/ii1gzp/how_false_smoke_alarm_almost_drowned_jane).*
