---
title: HERMES radio enables voice and data communication over vast distances
link: https://spectrum.ieee.org/hermes-shortwave-radio-digital-data
source: hnrss-org-frontpage
published: 2026-09-21T16:14:16Z
updated: 2026-09-21T16:14:16Z
first_seen: 2026-09-22T04:43:15.388238719Z
authors:
- SamuraiLion
summary: 'https://hermes.radio/ https://www.rhizomatica.org/ Comments URL: https://news.ycombinator.com/item?id=49789228 Points: 112 # Comments: 49'
content: extracted
html: 2026-09-21-hermes-radio-enables-voice-and-data-communication-over-vast.html
preview:
  file: 2026-09-21-hermes-radio-enables-voice-and-data-communication-over-vast.preview-19e6f6858bfb.webp
  width: 256
  height: 128
  color: '#939186'
images:
- source: https://spectrum.ieee.org/media-library/image.png?id=67783301&width=1200&height=600&coordinates=0%2C167%2C0%2C168
  original:
    file: 2026-09-21-hermes-radio-enables-voice-and-data-communication-over-vast.image-3e2b88bec0ed.png
    width: 1200
    height: 600
  color: '#c8c5c2'
---

Shortwave radios offer a way to connect one location on Earth to practically anywhere else with minimal infrastructure. But these radios come with some drawbacks—a significant one being that, unlike [satellite communications](https://spectrum.ieee.org/tag/satellite-communications), their transmission rates for digital data are typically measured in just [hundreds of bits per second](https://pretalx.sysmocom.de/osmodevcon2019/talk/BZX338/).

### Peter Bloom

[Peter Bloom](https://www.rhizomatica.org/team/peter-bloom/) is the founder of Rhizomatica, a nonprofit that works with remote, indigenous, and off-grid communities around the world to build shortwave and cellular-communication infrastructure.

Peter Bloom is the founder of [Rhizomatica](https://www.rhizomatica.org/), a Philadelphia-based nonprofit that has open-sourced a [digital shortwave-radio](https://spectrum.ieee.org/the-consumer-electronics-hall-of-fame-grundig-satellit-650-radio) set called the [High-frequency Emergency and Rural Multimedia Exchange System](https://hermes.radio/), or HERMES. The set operates in the high-frequency (HF) band from 3 to 30 megahertz, as does [Mercury](https://mercury.hermes.radio/), its digital modem. Rhizomatica staff travel around the globe to remote locations in countries like Bangladesh, Brazil, and Ecuador. Wherever they go, they use HERMES to help connect locals to the rest of the world.

Bloom spoke with **IEEE Spectrum** about how HERMES brings better data rates and [encryption](https://spectrum.ieee.org/tag/encryption) to shortwave radios.

**How does HERMES connect remote locations?**

**Peter Bloom:** We use the [ionosphere](https://www.noaa.gov/jetstream/ionosphere-max) as our satellite—or mirror—which helps us move information, voice, and data over really long distances. We’re using small radios that put out about 20 watts of power, and we can pretty reliably do 400- to 600-kilometer links between two radios. We’re talking about places that are not easy to reach, where it’s not simple to put terrestrial infrastructure.

**What can HERMES send that a basic voice radio can’t?**

**Bloom:** HERMES is a software stack—it’s a set of different programs that all work together in order to be able to send data over HF. HERMES allows you to send pretty much any file. Depending on what the file is, whether it’s a photo or an email or a voice memo, it just sends it as a file. It’s like a data pipeline over HF.

**Why does sending files and data matter more than just voice?**

**Bloom:** In emergency situations, people send their latitude and longitude over HF to say, “Hey, I’m here at this place.” People need to be able to send data over HF if there’s a manifest, a parts list, [telemedicine](https://spectrum.ieee.org/digital-health)—here’s what we have, here’s what we need. Instead of trying to read that out over the air, it’s much easier to just send the file. Same with a photo—if we need evidence that an area was logged illegally, we can just have someone send that over HF, rather than spending days getting down the river to get the photo where it needs to go.

**Why did you build in encryption that amateur-radio regulations in many countries don’t allow?**

**Bloom:** Encryption \[regulations\] for [ham radio operators](https://spectrum.ieee.org/tag/amateur-radio) are different in each country. So it’s all optional—you turn it on, you turn it off. The reason we built the encryption is that some of the partners we work with are in very sensitive areas and don’t want to be sending out information that can be easily captured and used against them.

**How has HERMES made an impact?**

**Bloom:** We’ve been working with artisanal fishers in Bangladesh on a pilot project. There’s 10 or 11 boats that have HERMES systems on them. Pretty soon after we installed those, one of the boats had a mechanical issue in the Bay of Bengal, 100 or 200 kilometers offshore. They were able to send their [GPS](https://spectrum.ieee.org/tag/gps) position and an SOS that they were having trouble. They were able to coordinate the rescue of the crew and the boat. So that was a really cool moment of HERMES in action that we’re super happy about.

*This article appears in the October 2026 print issue as “Peter Bloom.”*
