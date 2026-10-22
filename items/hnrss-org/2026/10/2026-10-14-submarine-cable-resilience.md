---
title: Submarine cable resilience
link: https://blog.apnic.net/2026/10/14/submarine-cable-resilience/
source: hnrss-org
published: 2026-10-14T15:31:15Z
updated: 2026-10-14T15:31:15Z
first_seen: 2026-10-14T21:35:38.678109349Z
authors:
- speckx
labels:
- resilience
- submarine cable
content: extracted
html: 2026-10-14-submarine-cable-resilience.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50089229
  num_comments: 36
  points: 114
---

![](https://blog.apnic.net/wp-content/uploads/2026/10/2026-10-featured-submarine-cables.png?v=e11444adc77394b2bac787beba72bc68843a007d89116073f556e66a53588ecd)

The Matsu Islands are an archipelago of 36 islands in the East China Sea, alongside the southeastern coast of mainland China. The islands have been administered since 1949 by Taiwan, part of a larger set of outlying islands administered by Taiwan, which includes the Kinmen and Penghu Islands, which all lie in the Taiwan Strait.

The Matsu Islands are connected to Taiwan’s main island by two submarine cables — [Taima No.2](https://www.submarinecablemap.com/submarine-cable/taiwan-penghu-kinmen-matsu-no-2-tpkm2), [Taima No.3](https://www.submarinecablemap.com/submarine-cable/taiwan-penghu-kinmen-matsu-no-3-tpkm3), and soon [Taima No.4](https://www.submarinecablemap.com/submarine-cable/taiwan-matsu-no-4). The Taiwan Strait is a shallow body of water, with an average depth of 70m. The submarine cables that lie on this seafloor are vulnerable to damage from ship-based activity, like being snagged by fishing equipment or anchors. But these particular cables have been remarkably unlucky. Between 2018 and 2022, the submarine cables between Taiwan and the Matsu Islands were cut 27 times, and there have been four major outages in the three years from 2023 to 2025.

That’s not the only location where submarine cables have suffered many faults. The Baltic Sea is a shallow sea that has a dozen or so submarine communications cables, and undersea electricity and gas pipelines. There have been 11 disruptions to these submarine systems since 2023. Again, a very unlucky track record.

There are an estimated total of 1.5M kilometres of subsea communications cables, used by 607 in-service submarine cable systems (Figure 1) that are critical to the world’s telecommunications services. If they are put out of service for any reason, our satellite systems simply could not replace them. In such a catastrophic scenario, some communities would be totally isolated, with severe logistical and economic ramifications.

![Figure 1 — Submarine cables, October 2026. Source: Telegeography.](https://blog.apnic.net/wp-content/uploads/2026/10/Screenshot-2026-10-14-at-11.57.54-am-2048x1074.png)\
Figure 1 — Submarine cables, October 2026. Source: [Telegeography](https://www.submarinecablemap.com/).

We regard this collection of submarine cable systems as critical infrastructure for global communication services, and we take care to make them as resilient as possible. Modern warfare can deliberately disrupt communications systems, which adds another source of potential damage to the list of geological and accidental risks facing subsea cables.

On the night of 4 August 1914, days after Britain declared war against Germany, the British cable ship HMTS Alert sailed from Dover into the English Channel. The vessel dragged grappling hooks along the seabed, hauled up five German transatlantic and European telegraph cables, and cut them. This severed Germany’s direct, secure submarine cable links to the Americas, Africa, Spain, and the rest of Europe. Germany was forced to rely on wireless radio transmissions, where British intelligence could readily eavesdrop. If the German cryptography used to protect these wireless communications was broken, then it allowed the British to track military movements and capture critical messages, such as the Zimmermann Telegram.

This same action to deliberately sever undersea cables was repeated by the British in September 1939, cutting German cables to Lisbon and New York.

## Protecting the cable

These thin polyester-coated cables at the bottom of the ocean are vulnerable, including to:

- Geology in the form of submarine earthquakes or landslips
- Being snagged by dragging ship anchors or fishing equipment along the seafloor
- Deliberate attacks on the cables with grapnels and cutting equipment

How can cables defend against such risks? The inner core of the fibre optic cables and its copper conductor sheath can be wrapped in additional layers of steel cable jacketing as a form of armour (Figure 2). This is mostly used to reduce its vulnerability to shipping accidents.

[![Figure 2 — Cross-section of a submarine cable. Source: Telegeography.](https://blog.apnic.net/wp-content/uploads/2026/10/Screenshot-2026-10-14-at-11.58.57-am-1-edited.png)](https://blog.apnic.net/wp-content/uploads/2026/10/Screenshot-2026-10-14-at-11.58.57-am-1-edited.png)\
Figure 2 — Cross-section of a submarine cable. Source: [Telegeography](https://www2.telegeography.com/).

Figure 2 is somewhat misleading. What is depicted here is a cross-section of submarine cable used in the segments of the cable that lead to the surface cable station. The cable segments that are laid on the deep-sea floor use just the inner components: Optical fibre and a single thin steel thread wrapped in a silicone gel, surrounded by a copper sheath (used to power the cable repeaters), and wrapped in polyethylene. There are no external armour elements. These deep-sea cable segments are around 20mm in diameter, or the size of a garden hose.

The cable surfaces at a cable landing station. These landing stations are typically plain, secured facilities with reliable power. The preferred location is a coastal point with a low volume of fishing or other maritime traffic, and a steep rise from the deep-sea floor. Cables are at their greatest risk from coastal shipping activity, so the shorter the distance from the deep-sea floor to the cable station on dry land, the better.

![Figure 3 — Cable landing station. Source: Wikipedia, CC BY-SA 3.0.](https://blog.apnic.net/wp-content/uploads/2026/10/subsea-landing.jpg)\
Figure 3 — Cable landing station. Source: [Wikipedia](https://commons.wikimedia.org/wiki/File:Submarine_Landed_in_BD.JPG), [CC BY-SA 3.0](http://creativecommons.org/licenses/by-sa/3.0/).

Many jurisdictions also declare ‘Cable Protection Zones’ as a warning to marine vessel operators that there are cables on the seafloor in this area, and ships should refrain from fishing activity and anchoring in these areas.

## Cable routing

Cable routing is also important in terms of resilience. There are several objectives in a cable route, and not all of them can be optimized at the same time.

The cable route should be as short as possible, as additional cable length raises capital costs and increases end-to-end signal delay.

Cables should avoid areas of active geological activity where possible, such as tectonic plate boundaries and mid-plate rifts. Deeper is better, but within reason. Cables on the floor of a deep ocean section are less vulnerable to shipping activity and hostile efforts to disrupt the cable, but with extreme depths it’s more challenging to locate and raise the cable when undertaking a repair.

Sometimes there are geographic ‘choke points’ through which many cables are routed. The Bab al-Mandab Strait is not just a choke point for surface vessels passing between the Indian Ocean and the Red Sea, but it is also the conduit through which 16 cables pass (Figure 4). In September 2025, several cables were severed in the Bab al-Mandab Strait, disrupting an estimated 25% of cable capacity between Asia, Europe, and the Middle East. Houthi-controlled waters remain effectively ungovernable for cable protection.

![Figure 4 — Cable routes in the Bab al-Mandab Strait.](https://blog.apnic.net/wp-content/uploads/2026/10/Screenshot-2026-10-14-at-12.01.41-pm.png)\
Figure 4 — Cable routes in the Bab al-Mandab Strait. Source: [Telegeography](https://www.submarinecablemap.com/).

There is also the Singapore Strait, a major junction for 35 cable systems (Figure 5). This is a shallow water strait that also hosts a significant volume of surface shipping.

[![Figure 5 — Cable routes in the Singapore Strait.](https://blog.apnic.net/wp-content/uploads/2026/10/Screenshot-2026-10-14-at-12.02.30-pm.png)](https://blog.apnic.net/wp-content/uploads/2026/10/Screenshot-2026-10-14-at-12.02.30-pm.png)\
Figure 5 — Cable routes in the Singapore Strait. Source: [Telegeography](https://www.submarinecablemap.com/).

Further north, between Taiwan and the Philippines is the Luzon Strait, where 18 cables pass (Figure 6). This was the location of an earthquake and a subsequent set of submarine landslides in December 2006 that not only severed all the cables running through this strait, but also buried many of them under deep layers of sediment. It took 11 repair vessels 49 days to bring all the cable systems back into operation.

[![Figure 6 — Cable routes in the Luzon Strait.](https://blog.apnic.net/wp-content/uploads/2026/10/Screenshot-2026-10-14-at-12.03.24-pm.png)](https://blog.apnic.net/wp-content/uploads/2026/10/Screenshot-2026-10-14-at-12.03.24-pm.png)\
Figure 6 — Cable routes in the Luzon Strait. Source: [Telegeography](https://www.submarinecablemap.com/).

The Black Sea is an area with geopolitical risk and regulatory gaps. While mine risk is confirmed, no cable faults have been directly documented in the war in Ukraine. Russia’s continued presence and the conflict make this region functionally inaccessible for routine cable maintenance.

Geopolitics also plays a role in cable routing. Cable owners will prefer to avoid routes through politically unstable territorial waters if suitable alternative routes exist. There are also political considerations. If a cable lands in US territory, then the cable is subject to several Federal Communications Commission (FCC) constraints, including banning equipment or services from companies the FCC see as a threat to US national security. The FCC has also refused to permit the landing of a cable on US territory if the cable also lands in a territory that the US deems to be a foreign adversary.

Land routes are avoided in general, but there are some circumstances where a land route is the preferred option. Connectivity between Asia and Europe would normally run through the Bab al-Mandab Strait, landing in Egypt in the Gulf of Suez, then overland through Egypt to get to the Mediterranean Sea. There is an alternative route through the Persian Gulf, Iraq and Türkiye, although this entails a segment that transits the Strait of Hormuz, or a second overland segment through Oman and the United Arab Emirates.

## Cable faults

Around 200 submarine cable faults are reported worldwide each year, a figure that the International Cable Protection Committee notes has stayed broadly steady in recent years even as the total cable inventory increases. The cables themselves are remarkably reliable. It is the maritime traffic above them that is the cause of most of the problems. 86% of cable faults are likely due to fishing activity or anchoring, and fishing activity is around twice as prevalent as anchoring incidents.

Geological events, such as submarine landslides and seabed movement, account for about 7% of cable faults, cable abrasion for 4%, and component failure (in repeaters) for only 4%.

With the high correlation of faults and maritime activity, it is not surprising that just under one half of cable faults occur in territorial waters, and most of the remainder occur in exclusive economic zones. Just 2% occur on the high seas. Cables are most vulnerable close to shore, in shallow water, where fishing is concentrated, where ships anchor, and where the cable must come ashore at a landing station alongside every other cable serving that coast. The deep ocean, where there is little seabed fishing activity and few ships anchoring, is the safest location for a cable.

National jurisdictions do not stop at the low tide mark of a territory, they extend out into the sea and the seafloor. The 1982 [United Nations Convention on the Law of the Sea (PDF)](https://www.un.org/depts/los/convention_agreements/texts/unclos/unclos_e.pdf) defined sovereign territorial waters as a 12-mile zone extending out from an economy’s coast. Every coastal economy has exclusive rights to the oil, gas, and other resources in the seabed up to 200 nautical miles from shore or to the outer edge of the continental margin, whichever is the further, subject to an overall limit of 350 nautical miles (650km) from the coast or 100 nautical miles (185km) beyond the 2,500-metre isobath (a line connecting equal points of water depth).

What does this mean for submarine cables? From the point where the cable enters territorial waters, any physical activity related to the cable requires the permission of the authorities. All cable construction and repair operations carried out in these sovereign areas also require the permission of the economy that has exclusive rights to the sea and seafloor.

## Location and repair

Locating a cable break is straightforward. An optical pulse will be reflected at the point of a cable break, which can provide the distance to the fault from the start of the cable with considerable precision. At that point, it becomes a marine operation.

A specialist cable repair ship must be engaged, which, in turn, must obtain the relevant government’s permission to work at the location of the cable fault (don’t forget that only 2% of outages occur in international waters). Once at the break location, the repair vessel drags a grapnel along the seabed to hook the cable, cuts it, recovers one end, buoys it off, then recovers the other end. A new section is spliced into the cable aboard the ship in a jointing cabin, the fibre fusion-spliced and tested, the joint rebuilt and sealed against pressure, and the repaired cable laid back down with enough slack to reach the seabed.

The cost of a repair can vary greatly, depending on the location and depth of the fault, the nature of the contracted arrangements for cable repair, the amount of time the cable operator is willing to wait for a repair vessel and crew, and the weather. USD 500k to USD 1M is a representative cost of a single cable repair.

While the number of cables has grown, the time taken to obtain permission to operate repairs in territorial waters has generally increased. Industry data indicates repair response times have more than doubled over the past decade, and the longest single repair recorded in 2023 took 947 days, many of which were spent waiting for permission to undertake the repair.

There is also the issue of repair vessels. The industry has roughly 60 ships capable of repairing a submarine cable, operated under maintenance agreements that pool the cost of running this fleet across cable owners in each geographic region. These collective Submarine Cable Maintenance and Repair Agreements are used in the Atlantic Ocean, the North American Western Coastal region, the Mediterranean, South East Asia and the Indian Ocean, and Yokohama in the North Pacific (Figure 7).

![Figure 7 — Areas covered by the five ‘zone’ agreements for pooled submarine cable maintenance and repair.](https://blog.apnic.net/wp-content/uploads/2026/10/Screenshot-2026-10-14-at-12.05.01-pm.png)\
Figure 7 — Areas covered by the five ‘zone’ agreements for pooled submarine cable maintenance and repair. Source: [International Cable Protection Committee (PDF).](https://www.iscpc.org/documents/?id=203)

[Analysis by TeleGeography and Infra-Analytics](https://resources.telegeography.com/current-state-forecasts-submarine-cable-maintenance) has found that around two-thirds of cable maintenance vessels will reach the end of their service lives by 2040, while total cable kilometres are projected to grow by about 48% over the same period. Closing that gap would take USD 3B and require fifteen replacement ships plus five additional vessels simply to hold current service levels as the submarine cable network expands. The immediate problem is that nobody obviously owns this bill. Cable maintenance is funded through shared agreements that have been optimized for decades to be as cheap as possible, which worked when the fleet was young and is precisely the wrong structure for financing a fleet renewal.

This is the core of the cable resilience problem. Adding cable routes is comparatively easy, because today’s hyperscale cloud companies fund much of the new construction themselves and have strong commercial reasons to do so. However, no individual cable is so critical that it can sustain a business case to operate a dedicated repair fleet. The industry uses pooled resources, which suffer from under-investment.

## Investigating cable outages

A rundown of recent cable outages includes:

- Subsea cables were damaged in [October 2023](https://www.reuters.com/world/china/hong-kong-call-10-witnesses-case-over-baltic-sea-undersea-cable-damage-2026-01-20/) by the anchor of the Hong Kong-flagged Newnew Polar Bear operating in the Baltic Sea.

- Three cables in the Red Sea were cut in [February 2024](https://www.pbs.org/newshour/world/houthi-rebels-deny-severing-three-red-sea-underwater-cables) by the drifting Rubymar vessel after it was struck by a missile.

- The C-Lion1 and the BCS East-West Interlink cables were cut in the Baltic Sea in [November 2024](https://cepa.org/article/cable-danger-look-beyond-the-sea-to-the-shore/) with the Chinese bulk carrier Yi Peng 3 nearby.

- Four cables were damaged in [December 2024](https://www.abc.net.au/news/2024-12-31/finland-find-anchor-drag-mark-in-baltic-sea/104775072), with anchor drag marks on the Baltic seabed and the Cook Islands-registered tanker Eagle S detained.

- Taiwan recorded incidents in [January and February 2025](https://www.theguardian.com/world/2025/jan/07/taiwan-investigating-chinese-vessel-over-damage-to-undersea-cable) on the Matsu cables.

- Three major cables, including SEA-ME-WE 4 and IMEWE, were cut in the Red Sea in [September 2025](https://www.euronews.com/2025/09/08/what-do-we-know-about-internet-disruptions-from-cut-subsea-cables-in-the-red-sea).

- Finland and Estonia lost connections again in December 2025, with anchor drag confirmed by [March 2026](https://oceaneconomist.com/articles/the-cloud-has-a-seabed-problem) on Baltic cables.

- In [August 2026](https://theconversation.com/two-subsea-cables-off-perth-suffer-concerning-damage-lasers-could-help-protect-them-289601), the Indigo West and Indigo Central cables off Perth were damaged, with a UAE-managed tanker under investigation by Australian Federal Police.

The outcomes are much less uniform than this list suggests. Swedish authorities found no conclusive evidence of wrongdoing in the Yi Peng 3 case and ruled the January 2025 Latvia-to-Gotland damage accidental. A Finnish court dismissed the case against the Eagle S captain in October 2025. United States and European officials concluded that several of the Baltic incidents were accidental. The captain of the Hong Tai 58 was convicted in Taiwan and sentenced to three years over the Taiwan to Penghu No. 3 cable, and the Newnew Polar Bear captain faced criminal damage charges in Hong Kong. The April 2026 Matsu fault turned out to have been caused by the wreck of a stranded fishing boat shifting onto the cable.

Anchor dragging is a common occurrence, particularly among poorly maintained merchant vessels in bad weather, accounting for a large share of ordinary faults every year. This may explain why investigators keep reaching inconclusive findings in good faith.

## Who is responsible for cable resilience?

Cable systems have changed in recent decades.

The old telephone system regarded each cable as a self-contained system. The cable owner was responsible for its own resilience, independent of any other cables that may exist on similar routes. The telephone’s data carriage system typically used a ring architecture, where any outage in the ring caused the traffic to flow along the other half of the ring. If the ring used diverse paths, then any individual outage in the ring could be repaired immediately.

However, the implication of this design was that the actual capacity of the system was double the usable capacity. The cost of using such a cable reflected this 50% carriage efficiency.

In their search for cheaper services, Internet Service Providers (ISPs) wanted to purchase non-backed-up cable capacity, at a price that reflected this greater efficiency of cable use. They purchased this cheaper form of cable capacity on multiple cable systems. Individual outages caused a break in that cable’s service, but the ISP could reroute their traffic to use capacity that had been purchased on other cable systems which were not affected by the outage. For Internet traffic, this failover function is readily achieved in the routing system.

The overall outcome was that the responsibility for resilience shifted from the individual cable operator to the network operator. Cable outages would still cause a drop in available capacity. But as most Internet traffic is elastic, the drop in capacity would cause a drop in performance but not cause a complete shutdown.

## Resilience and critical infrastructure considerations

Submarine cable systems are built by private consortia, repaired by private contracts that collectively fund a commercial repair fleet, regulated by coastal states individually, and governed internationally by international conventions drafted long before the Internet existed.

It would be good if permissions to repair submarine cables were granted in a streamlined manner so that repairs were not stalled for months awaiting a government’s approval to operate in their waters. It would be even better if emergency procedures were agreed in advance rather than negotiated during an outage.

Collectively, the industry needs to invest in repair capacity to upgrade an ageing repair fleet. Who undertakes this investment and how the repair capability is managed as a common asset is proving to be a difficult issue. In the worst case, it will be back to every cable for itself, and the inevitable outcome of higher costs and potentially slower repairs.

Geographic choke points will persist, geology will not stop, extreme weather and changing sediment conditions will continue to pose a threat to these cables. Route diversity is an effective response to these challenges, but economic clustering works against cable route diversity. Each cable would like to run through the shortest and cheapest route, and no cable operator is specifically motivated to spend additional sums to take a diverse route to the other existing cables.

None of this is technologically difficult, but it can be institutionally and economically difficult, which is harder.

## Looking forward

The telephone company consortium model that funded and operated almost all the world’s subsea cables 30 years ago has come to an end. The service life of the entire inventory of cables and service vessels that they operated is also coming to an end.

Approximately a quarter of the world’s current subsea cable kilometres are projected to be decommissioned and retired by 2030. This massive retirement of capacity amounts to 845,000 kilometres of subsea fibre-optic cables at the end-of-life cycle for submarine infrastructure laid during the massive Internet boom of the late 1990s and early 2000s. Specifically, TAT-8 and TAT-14 in the Atlantic Ocean are being decommissioned, SEA-ME-WE-3, linking Europe and Asia, has been retired, and Japan-US and China-US have ended operations. In addition, more than half of the current repair vessel fleet will have reached their retirement point by 2040.

In their place are cable projects that are either fully funded or partially funded by the current US digital giants. This raises sovereign risk eyebrows in many economies, where the only communications conduits for an economy are owned and operated by a foreign entity. It also raises the question of sovereign control over the submarine repair vessel fleet and the existing maintenance pooling arrangements through submarine cable maintenance zones.

By 2028, Google will own the largest set of cables, measured by kilometres, in the Pacific, Atlantic and Indian Oceans, and based on current plans it will have more than 50% of the entire cable inventory under its control in the Indo-Pacific region. Similar centralization scenarios are playing out in the Atlantic Ocean as well. At this point, the sovereign risk eyebrows should be hitting the ceiling, as such a dominant position gives this single company unilateral control over the maintenance arrangements and associated repair vessel operations for the entire world’s communications network.

This is a serious issue for us, both as individuals and societally as we try to preserve the integrity and utility of our common communications infrastructure. The stability and productivity of our societies rely on our ability to communicate with each other seamlessly, efficiently and privately.

Passing the investment and operational costs of the public telecommunications services from public to private hands in the latter part of the 20th century was seen as a smart move. The move used private finance to fund the reinvigoration of national telecommunications infrastructure as it shifted from time-based voice switching to a far larger packet switching architecture to support computer-mediated communications and service provision.

However, we may see more uncomfortable outcomes looming in the near future. It was thought that the discipline of competition would rein in the potential abuses by private operators of the privileged position of operating a public communications service. That is not what happened.

Not only are public telecommunications systems now largely in private hands, but we are also seeing consolidation of the global telecommunications system to a few private entities, with no competition. Their accountability is not to the public or users of the service. Their accountability is to their institutional investors to continually improve their value and profitability. Nothing else.

So where does this leave the submarine cable system and efforts to maintain its critical function with the appropriate levels of resilience that reflect their critical role in global communications? Outside the US, few individual economies, if any, have the resources to meaningfully respond to this challenge. The European Union (EU) has taken some action to contribute to funding a fleet of repair vessels and taken measures to improve surveillance on submarine cables as a matter of regional security, but no other economies appear to be following this lead. It would certainly be ironic if this EU money were to end up simply funding the maintenance of Google’s submarine cable assets!

Back to the topic of cable resilience, viewed at a global level: Communications resilience is largely an ocean infrastructure question. Answers to this question rely on:

- Detailed and current seabed surveys
- Shipping controls to prevent anchoring and seabed fishing in declared subsea cable protection zones
- A distributed repair fleet and crews trained to perform repair tasks
- Rapid clearance to operate in territorial waters

Some coordination of action is necessary to open diverse cable routes and avoid single points of vulnerability and potential failure. The global cloud, for all its connotations of weightless air, firmly rests on the solidity of the seabed, and it can be no more reliable than conventions that we collectively adopt to look after it.

* * *

The views expressed by the authors of this blog are their own and do not necessarily reflect the views of APNIC. Please note a [Code of Conduct](https://blog.apnic.net/?p=395) applies to this blog.
