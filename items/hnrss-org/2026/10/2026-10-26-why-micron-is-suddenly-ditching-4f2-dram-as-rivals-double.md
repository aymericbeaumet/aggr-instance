---
title: Why Micron Is Suddenly Ditching 4F² DRAM as Rivals Double Down
link: https://hothardware.com/news/why-micron-is-suddenly-ditching-4f-dram
source: hnrss-org
published: 2026-10-26T15:05:04Z
updated: 2026-10-26T15:05:04Z
first_seen: 2026-10-27T20:37:14.562886086Z
authors:
- rbanffy
labels:
- (nasdaq:mu)
- memory
- micron
content: extracted
html: 2026-10-26-why-micron-is-suddenly-ditching-4f2-dram-as-rivals-double.html
remote_preview:
  url: https://images.hothardware.com/contentimages/newsitem/71935/content/16x9_2133x1200_highres-micron-boise-fab-photo.jpg
---

[![hero micron boise fab photo](https://hothardware.com/image/resize/?width=360&height=360&imageFile=%2fcontentimages%2fnewsitem%2f71935%2fcontent%2fsmall_hero-micron-boise-fab-photo.jpg&fileFormat=webp)](https://hothardware.com/photo-gallery/NewsItem/71935?image=big_hero-micron-boise-fab-photo.jpg&tag=popup) \
Micron's Boise campus, recently the site of a $50B expansion, will produce 1δ memory. - Image: Micron

DRAM is fundamentally different from logic. Logic cells are like those in your processor, and in that form, they're all the same sort of thing: switching transistors. DRAM is different because each cell is made up of a transistor and a capacitor, and [that complicates scaling](https://hothardware.com/news/backyard-scientist-is-making-ram-at-home-in-a-memory-shed-fab). For a long time now, we've been making those cells with a geometry known as "6F²", and the 'holy grail' was the ultimate move to 4F², offering a theoretical 33% density improvement. Now, Micron says that despite putting "over two decades" of research into the idea, it's throwing in the towel and sticking to 6F² for the foreseeable future.

This might seem absurd, but you have to understand that these are cell geometries, not process nodes. In essence, these terms describe the shape and layout of the DRAM cell, not its physical size directly; that is determined by the combination of layout and process node. DRAM doesn't use the same node nomenclature as logic; because of the mixed cell type (transistors and capacitors), it doesn't scale the same way; to mark node changes, DRAM vendors [currently use terminology](https://hothardware.com/news/micron-1-gamma-dram) like "1γ" and "1δ"—those being the Greek letters gamma and delta, used in this case to signify serial improvements over time.

[![micron roadmap 2026](https://images.hothardware.com/contentimages/newsitem/71935/content/small_micron-roadmap-2026.png)](https://hothardware.com/photo-gallery/NewsItem/71935?image=big_micron-roadmap-2026.png&tag=popup)\
Micron roadmap, revealed this Wednesday. Image: Micron via [Tom's Hardware](https://www.tomshardware.com/pc-components/dram/micron-pulls-the-plug-on-future-4f2-dram-after-two-decades-of-research-in-dramatic-shift-company-says-architecture-offers-no-meaningful-density-advantages-increases-manufacturing-complexity-and-costs-and-has-fundamental-scaling-limitations)

Micron's ultimate decision not to bring 4F² memory products to market is fascinating in the context that all three of its competitors, those being Samsung, SK hynix, and CXMT, are moving ahead with 4F² designs, with CXMT in particular claiming it will have 4F² RAM in server DDR5 RDIMMs [by the end of the year](https://hothardware.com/news/chinas-cxmt-eyes-second-beijing-fab-to-fight-memory-shortage). The US firm held a webinar on Wednesday that announced its future roadmap, above, and talked about a variety of upcoming products, not just this decision to skip 4F². That's arguably the most surprising news, though.

Scott DeBoer, Micron's President and CTO, says DRAM density is ultimately set by the spacing required by the capacitors, not by the nominal cell footprint. While shrinking the geometry from 6F² to 4F² seems like an easy win on paper, according to DeBoer, it doesn't actually gain you much in terms of density, and it drastically [increases cost and complexity](https://hothardware.com/news/samsung-sk-hynix-chip-plant-expansion). He noted that Micron "invented" 4F² and that the firm has been trying to figure out how to commercialize the technology for "over two decades" before finally discarding it.

[![micron ddr5 mrdimm](https://images.hothardware.com/contentimages/newsitem/71935/content/small_micron-ddr5-mrdimm.jpg)](https://hothardware.com/photo-gallery/NewsItem/71935?image=big_micron-ddr5-mrdimm.jpg&tag=popup)\
DDR5 MRDIMMs represent the current pinnacle of DRAM tech. - Image: Micron

What is Micron doing instead, then? Well, it's aggressively scaling 6F² DRAM, for one thing. The firm's roadmap notes that high-density 1δ (1-delta) memory is ramping yields, 1-ε (1-epsilon) featuring advanced wafer bonding is in development, and looking ahead to the future, it sees "ζÅ" (zeta-Angstrom) and "ηÅ" (eta-Angstrom) as its future for planar DRAM. But the real future, according to Micron, [will be in 3D DRAM](https://hothardware.com/news/micron-pours-10-billion-us-hub-invent-post-dram-ai-tech). Rather than simply stacking DRAM on top of logic—this is the "wafer bonding" Micron mentioned—3D DRAM describes memory devices where multiple physical layers of DRAM storage cells are stacked vertically on top of each other.

Flash memory has been doing the 3D thing [for more than a decade](https://hothardware.com/news/samsung-starts-volume-production-of-worlds-first-3d-vertical-nand-flash-memory-), of course, but flash memory relies on simple charge-trap transistors that can be stacked like floors in a skyscraper. DRAM cells, by contrast, are dynamic; each bit requires a delicate, high-aspect-ratio capacitor paired with a transistor that inevitably leaks charge. Flipping those tall capacitors sideways and scaling them vertically without thermal crosstalk or signal destruction requires a total overhaul of the basic DRAM architecture. That transition won't happen overnight, but as 2D scaling hits its absolute physical limit, 3D DRAM is where the industry is inevitably headed.

[In the short term](https://www.vlsi.kr/en/micron-technology-webinar-roadmap-deep-dive-en/), Micron could face challenges if its competitors are able to economically manufacture 4F² memory at scale, but it is placing a massive strategic wager that 4F² will prove to be a costly, short-lived detour. If Samsung, SK hynix, or CXMT end up burning billions in capital expenditure to adopt vertical-channel transistors only to hit an early brick wall with capacitor scaling, Micron will look brilliant for skipping the headache. By wringing every last drop of efficiency out of established 6F² lithography with High-NA EUV, advanced materials, and wafer-on-wafer bonding through the zeta and eta eras, Micron hopes to preserve its cash and engineering bandwidth for the real prize: being first to cross the finish line when true monolithic 3D DRAM finally becomes economically viable.

![Zak Killian](https://hothardware.com/contentimages/author/144/thumbnail/default/arpt.jpg)

[Zak Killian](https://hothardware.com/author/zak-killian)

A 30-year PC building veteran, Zak is a modern-day Renaissance man who may not be an expert on anything, but knows just a little about nearly everything.
