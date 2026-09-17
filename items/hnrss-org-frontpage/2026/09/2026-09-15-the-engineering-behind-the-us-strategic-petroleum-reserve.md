---
title: The engineering behind the US Strategic Petroleum Reserve
link: https://johnjwang.com/post/2026/09/15/engineering-behind-us-strategic-petroleum-reserve
source: hnrss-org-frontpage
published: 2026-09-15T22:15:25Z
updated: 2026-09-15T22:15:25Z
first_seen: 2026-09-17T07:35:27.698192978Z
authors:
- johnjwang
summary: 'Article URL: https://johnjwang.com/post/2026/09/15/engineering-behind-us-strategic-petroleum-reserve Comments URL: https://news.ycombinator.com/item?id=49719596 Points: 180 # Comments: 71'
content: extracted
html: 2026-09-15-the-engineering-behind-the-us-strategic-petroleum-reserve.html
preview:
  file: 2026-09-15-the-engineering-behind-the-us-strategic-petroleum-reserve.preview-cdca716d31cd.webp
  width: 256
  height: 134
  alt: John Wang, co-founder and CTO of Assembled
  color: '#d4c5ba'
images:
- source: https://johnjwang.com/og-image.jpg
  original:
    file: 2026-09-15-the-engineering-behind-the-us-strategic-petroleum-reserve.image-289258658cb0.jpg
    width: 1200
    height: 630
  color: '#f9f8f4'
- source: https://johnjwang.com/images/cushing-tank-farm.jpg
  original:
    file: 2026-09-15-the-engineering-behind-the-us-strategic-petroleum-reserve.image-4a544c6764c1.jpg
    width: 4752
    height: 3168
  color: '#682914'
---

One of the most fascinating things I’ve learned about recently is the engineering behind the US Strategic Petroleum Reserve. Here are the key requirements it’s designed to meet:

- Store hundreds of millions of barrels of crude oil for long periods of time to [respond to disruptions in petroleum supplies](https://www.energy.gov/hgeo/opr/strategic-petroleum-reserve).
- Keep the oil secure against attacks by our adversaries. This is an especially hard challenge because petroleum has this tendency to light on fire, so a huge concentration of it is especially difficult to keep safe.
- Release oil quickly so it can reach the market during a supply disruption.
- Keep maintenance costs low and last for decades.

The actual solution, like many great engineering solutions, is incredibly elegant and simple. But before we talk about it, let’s start with how you might go about solving this:

## The naive solution: External floating roof tanks

![A large storage tank with an exterior staircase at the Enbridge tank farm in Cushing, Oklahoma.](https://johnjwang.com/images/cushing-tank-farm.jpg)

Enbridge tank farm, Cushing, Oklahoma, April 2010. Photo: [roy.luck](https://commons.wikimedia.org/wiki/File:Enbridge_tank_farm,_Cushing_OK.jpg), [CC BY 2.0](https://creativecommons.org/licenses/by/2.0/).

Looking at the requirements, the first thing most people would probably think to do is to just take what we do commercially for storing petroleum and scale it up. [External floating roof tanks (EFRTs)](https://en.wikipedia.org/wiki/External_floating_roof_tank) are the common solution for storing petroleum. Most tanks are confined to about 50 feet tall and 300 feet in diameter – larger than that and you start to have engineering problems with the foundation and drainage systems. This gives a volume of $V = \\pi (150 \\mathrm{ft})^2(50 \\mathrm{ft})$, or $3{,}532{,}500 \\mathrm{ft}^3$, equivalent to about 630,000 barrels of oil.

To hold 714 million barrels of oil (the full capacity of the Strategic Petroleum Reserve), you’d need about 1,130 of these tanks. If you use the standard capacity of the largest commercial petroleum farms (e.g., in Cushing, Oklahoma), you need about 45,000 acres to store these tanks. That’s basically the size of Washington, D.C., which means you’d need to acquire a lot of land.

However, the biggest downside to using tanks is that they’re incredibly vulnerable to attack. Damage can cause spills and fires, and there’s a particular weak point at the seal between a floating roof and the tank shell. [Lightning-caused fires have been documented in the seal space of open floating-roof tanks](https://law.resource.org/pub/us/cfr/ibr/002/api.2003.1998.pdf#page=38), so a deliberate ignition source (shrapnel, incendiary) could be particularly bad.

## Putting tanks underground

Next up, one might think about putting the petroleum underground. The Navy actually did this at the [Red Hill Facility](https://en.wikipedia.org/wiki/Red_Hill_Underground_Fuel_Storage_Facility) near Pearl Harbor. Built in 1943, it housed 20 enormous steel-lined concrete tanks inside excavated volcanic rock. Each tank was about 100 feet across and 250 feet tall, making it comparable in volume to a large EFRT. Altogether, the facility housed 6 million barrels of fuel. The surrounding rock provided protection from aerial attack, which was a major reason for building the facility. But this was a substantial construction project, and thousands of workers had to excavate the tunnels, install steel liners, and pour concrete. The original construction cost was [$42.2 million (~$820M in 2026 dollars)](https://ascelibrary.com/doi/abs/10.1061/JLADAH.LADR-1005).

Also, groundwater protection became a major challenge. A tank released about 27,000 gallons of fuel in 2014. Separate releases in 2021 contaminated the Navy’s drinking-water system, causing the Navy to defuel and [permanently close the facility](https://www.epa.gov/red-hill/about-fuel-releases).

There’s also the question of scale. To get to 714 million barrels, you’d need roughly 119 Red Hill-sized facilities’ worth of capacity and enough space to actually put these tanks in the ground. It would be an absolutely enormous construction project that would cost hundreds of billions of dollars (which, even for the government, is extremely expensive).

## The actual solution: Salt caverns

So how did the US solve this? The crux was using salt domes at four sites in Texas and Louisiana along the Gulf of Mexico. The US created massive caverns underground in these salt domes that hold about 10 million barrels each (more than the entire Red Hill facility). The DOE currently lists 60 caverns with a combined authorized storage capacity of about [714 million barrels](https://www.energy.gov/hgeo/opr/spr-storage-sites).

A few properties make this work:

- Cylindrical caverns are excavated using water. Engineers drill into a salt dome and inject fresh water. The salt dissolves in the water, and then pumps are used to remove the resulting brine, leaving a cavern that can be used to store petroleum.

- Salt contains the oil and helps seal small fractures. The rock salt surrounding the SPR’s caverns has extremely low permeability, meaning fluids have very little ability to pass through it. It also doesn’t react with petroleum. Under enormous pressures underground, salt also slowly deforms, which helps close small fractures. The salt itself can therefore contain the oil without a steel-and-concrete tank lining the cavern.

- Oil floats on water, which means pumping water into the bottom of the cavern pushes the oil out. As fresh water is pumped into the bottom of the cavern, the oil gets displaced upwards into a delivery system.

- As a bonus, the location helps get oil to market. The Gulf Coast puts the reserve near refineries, pipelines, and marine terminals, which is particularly useful when the whole point is to deliver oil during a supply disruption.

This storage solution is relatively inexpensive. DOE’s historical capital-cost estimate is about \[$3.50 per barrel\](https://www.energy.gov/hgeo/opr/spr-faqs) of cavern storage capacity, compared with $15-$18 for aboveground tanks. Storing the oil deep underground also helps protect it from aerial attack.

There are still tradeoffs, though. Creating caverns requires a water supply and a way to dispose of the brine. And fresh water introduced during withdrawals dissolves additional salt, gradually enlarging the caverns. That limits repeated cycling and makes cavern monitoring and maintenance quite important. The wells, pumps, and pipelines also need continued upkeep, so frequent withdrawals can degrade the infrastructure.
