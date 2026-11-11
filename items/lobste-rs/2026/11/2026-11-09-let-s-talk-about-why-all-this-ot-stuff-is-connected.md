---
title: Let’s Talk About Why all This OT Stuff is Connected
link: https://tisiphone.net/2026/11/08/lets-talk-about-why-all-this-ot-stuff-is-connected/
source: lobste-rs
published: 2026-11-09T12:22:39Z
updated: 2026-11-09T12:22:39Z
first_seen: 2026-11-11T00:34:12.222309031Z
authors:
- tisiphone.net via polywolf
labels:
- security
summary: Comments
content: extracted
html: 2026-11-09-let-s-talk-about-why-all-this-ot-stuff-is-connected.html
remote_preview:
  url: https://i0.wp.com/tisiphone.net/wp-content/uploads/2026/11/pexels-photo-17546504.jpeg?fit=818%2C1200&ssl=1
---

It’s been a really bad month (and year) for OT cybersecurity. From a wave of new, high-impact vulnerabilities, to [attacks on supermarket refrigerators](https://www.theguardian.com/world/2026/nov/02/supermarket-freezers-across-europe-hit-by-second-suspected-cyber-attack), to [attacks on blood banks](https://www.theguardian.com/technology/2026/nov/04/france-national-blood-service-hack-cyber-attack-europe), we’ve been left shorthanded and reeling. And that’s on top of [months of attacks against water utilities](https://techcrunch.com/2026/08/26/cisa-confirms-hackers-targeted-over-100-us-water-systems-during-july/).

Through all of this, I keep seeing the same comment from seasoned enterprise cybersecurity people, and as someone dealing with these cases on the front line, I find it very frustrating. They express intense shock that these systems are accessible via networks at all, then ask why they can’t just be disconnected. It goes something like this:

[![Quote post - From an IT guy and cybersecurity fellow, why aren't those critical things airgapped? Outside of remote observation for temperatures (which you could do with non-connected thermometers), I can't imagine why I would want a refrigerator that contains temperature-sensitive cultures of supplies to be connected to the internet.](https://tisiphone.net/wp-content/uploads/2026/11/image-1.png)](https://tisiphone.net/wp-content/uploads/2026/11/image-1.png)

This is just one example, and I’m genuinely not trying to bash the OP. I’m certain they’re a very competent, experienced IT person, and I understand their frustration. But the situation is far more complicated than it looks, and calling the people who support and purchase these systems incompetent won’t fix it. So let me try to explain why so much OT, ICS, and IIoT is exposed, and which systems are in the worst state.

## How We Got Here

Before we can talk about the current state of OT, we need to go back 15 to 20 years to discuss IT/OT convergence. Before then, industrial devices and network infrastructure were often highly bespoke, heterogeneous technologies engineered by vendors. Most communication was serial-based, protocols were custom, and even device firmware was developed in-house. A Rockwell system looked nothing like a Siemens system. When you purchased a system, it was designed, deployed, and supported by that specific OEM. They supplied the replacement parts. They trained the technicians and installers.

This got expensive, and it slowed down logistics, not just for parts but also for labour. So clever engineers and businesspeople looked at the IT landscape and had an idea. Using Windows computers as human-machine interfaces, and commercial Ethernet to encapsulate serial traffic, would be much cheaper to buy, replace, and train technicians to support.

Over the years that followed, systems became a mix: lower-level PLCs and RTUs down at Level 1, and above them, numerous devices that were far more familiar to IT staff. Unfortunately, they were also familiar to hackers and hacking tools. This was the first change in exposure, and it was really just a loss of security by obscurity.

The next change was the homogenisation of OEM deployments. To deploy and support large-scale systems more efficiently, most industrial vendors started producing very uniform deployments of technology across the [Purdue Model](https://en.wikipedia.org/wiki/Purdue_Enterprise_Reference_Architecture), in very static architectures. Network maps and addressing can look incredibly similar between deployments, even across verticals and countries. Once again, this was the most financially and logistically efficient solution.

Unfortunately, any savvy cybersecurity professional can see how this lends itself to more scalable attacks, and even to automating parts of those attacks with tools and agents.

## The Age of Remote Connectivity

Over the last ten years, we’ve lived through the rapid adoption of remote connectivity in industrial environments around the world.

There was a time when many industrial systems were effectively “air-gapped”. This certainly coincided with the serial era of industrial technology.

As IT/OT convergence took hold, a mix of tactics was used. In some flat corporate network environments, industrial control systems were simply plugged into existing cabling for the cheapest possible network access. This included both industrial and building automation systems. We still see this today in low-margin verticals, especially manufacturing. Those flat (or relatively flat) networks have been a thorn in the side of industrial cybersecurity people for decades.

The more acceptable solution from an industry perspective was implementing a DMZ at Level 3.5 of the Purdue Model. Enterprise IT would live above it, and the industrial OT environment would live below it. Note that this is still some level of connectivity. Traditionally, it was accomplished with two firewalls and some kind of jump host for traversing between the two environments when needed.

Then the scope expanded, faster than practitioners could adapt.

## Everything Wants Access

Networking industrial systems beyond single factory floors made them far more robust and efficient. Ethernet and routing allowed for distributed control and telemetry not just inside a building, but across regional, national, and then international deployments. A single control centre could make real-time changes to power transmission across mountains and multiple substations via SCADA. A manufacturer could match production to demand down to the minute, second, or millisecond. Parts could be monitored for failure tolerances, and replacements planned months in advance.

But humans weren’t the limit! Computers could make these decisions on a massive scale, far faster than human operators alone. Raise production, lower production. Our society came to rely on this, with a massive push towards just-in-time logistics. Of course, that meant central SCADA servers needed bidirectional control of industrial devices across huge regions, via networks.

Global power grids are a significant example, and they’ve changed enormously over the past ten years. Consider the advent of renewable energy. Firstly, wind and solar farms are made up of numerous generation devices, which must be coordinated and monitored individually across geographically disparate locations. Secondly, home generation necessitates smart meters to monitor usage and buy-back. Finally, there’s the problem of inertia. Traditional generation sources like nuclear, gas, and hydroelectric are high-inertia. Their output can be readily controlled through fuel, and they provide a lot of grid frequency stability during sudden spikes and drops in demand. Renewables provide far less inertia and are far less predictable. We can’t turn the wind on and off. That makes careful monitoring of detailed telemetry, and adjustments to load and generation at machine speed, essential.

Add centralised dispatch and repair with far fewer staff, plus mass consumer demand for exact, real-time electricity billing and status checking. The result? Many power companies around the world have little to no ability to switch to manual operations for more than a few days, if at all.

All of this requires many inputs and outputs: telemetry from multiple industrial devices via fibre, copper, LTE, and radio; billing systems; customer portals; even computers in maintenance trucks.

And bulk electricity is just one example. Connectivity and centralisation have been huge cost and labour savers for every vertical around the world. Big industrial companies rely on central DCS and SCADA control, and the more geographically dispersed their systems are, the more connectivity they require.

Take those supermarket refrigerators. Many countries hold supermarkets to regulatory standards for the temperature at which food is kept, and supermarkets don’t staff to have humans check this in real time. So most of it is done through mobile apps or central monitoring, which saves money, ensures regulations are met, and keeps food from spoiling. A thermometer that isn’t connected to anything is only useful when someone is standing in front of it.

## Everyone Wants Access

System efficiency is only part of the story, though. Human engineers and operators are expensive, and the 2020 pandemic forced remote support on many businesses that had never considered it. The cost savings were immediately clear to industrial operators and OEMs.

It costs an OEM far less to train and employ ten technicians in a central, regional office than to have one in every city or at every customer site. Those savings are passed on to customers in very tangible ways. While there have been holdouts who insist on local support at significant extra expense, the industry has shifted heavily to remote support for routine system monitoring and fault analysis.

Assumptions about security controls are one of the most dangerous problems I encounter as a full-time incident responder in industrial environments. One of the most common is cybersecurity personnel assuming the only connectivity to OT is still the DMZ at Level 3.5 with a jump host. When I perform forensics, I typically find five to seven other remote access mechanisms. Some are software-based remote access tools. Others are extraneous modems, VPN concentrators, and wireless bridges installed by local staff and a multitude of vendors and subcontractors.

This isn’t even necessarily a contract violation. I’ve seen many major OEMs and specialist device suppliers add contractual language requiring some level of remote access in order to keep receiving support, or support within a reasonable SLA. I talk to numerous asset owners around the world, and even Fortune 50 and government clients struggle to change these terms. Some OEMs will simply only provide timely support via remote connection.

So, as an OT cybersecurity industry, we have to work around this. We have to identify and evaluate these remote connections. We have to try to build segmentation and microsegmentation around them, if the OEMs and operations permit. If that fails, we monitor and log to the very best of our ability. But all changes to industrial environments are costly, and fighting OEM requirements isn’t just expensive. It can cause a life-safety incident.

## What’s Getting Hacked and Why?

Now that we understand the chaos of connectivity in modern industrial environments, we can talk about what’s getting hacked right now, and why. Why water? Why refrigerators?

A few unfortunate, incredibly predictable factors have led us here, and my colleagues and I have been screaming about them for years:

- **Distributed industrial systems tend to be far more exposed than those in a single building.** Whether it’s water pumps, wind turbines, refrigerators, or solar panels, if you have a bunch of standalone industrial environments in a field that need connectivity, the most cost-effective solution is a modem per unit or some other wireless link. Putting firewalls or concentrators in front of every modem is expensive. Keeping them updated and monitored is even more expensive. We’ve seen a two-week delay in updating them end in catastrophe. Which leads us to:
- **The organisations you see repeatedly hit have next to no money for IT, let alone cybersecurity.** Manufacturing runs on thin margins, while water and some transport systems are municipally funded, with almost no budget for system updates or cybersecurity programs. It’s very easy to say “why don’t you just” put a firewall and fibre connection on every refrigerator. The cost of doing that is days of operational shutdown, and orders of magnitude more than a simple mobile router. Water has kept my colleagues and me awake, and at bars, far more than any other vertical. It’s essential to society, with next to no investment across large parts of the world.
- **The devices you see repeatedly hit are fairly homogeneous and well documented.** Complex industrial systems of systems are, as a rule, hard to break in a way that’s both reliable and dangerous. They’re full of redundancies and analogue and digital safety controls. Reliable attacks used to require engineering expertise. However, AI, and LLMs specifically, are certainly lowering that barrier to entry. Without controls, they can reveal which verticals are least defended and most exposed, and which devices are popular globally for a given function. Since vendors have trended towards homogenisation, the most commonly exposed devices, and their interfaces and vulnerabilities, can be understood and automated against.
- **Fixing the vulnerability isn’t easy, even if the organisation is aware of it.** Industrial systems have long lifecycles, sometimes 10 to 30 years. They’re engineered and tested by the OEM to work in a very specific technology stack and topology. Any alteration can, at best, disrupt the process and, at worst, lead to injury or death. Either outcome can be far more damaging than a casual hacker. Because of these lifecycles, and the inability to replace systems, I still deal with Windows 2000, CE, and XP every week. They can’t be removed from production or modified without express authorisation from the OEM and engineers, plus a significant outage and testing window. All of this adds up to huge expenses for the asset owner.

As you can see, my colleagues and I have our work cut out for us. These systems are more connected than they have ever been. They contain out-of-support operating systems that can’t be replaced. Many of the most exposed systems are similar, well documented, and lack monitoring and basic cybersecurity controls because of budgets. And we’re now seeing the result first-hand in our cases: adversaries, enabled by LLM data gathering, are pinpointing these systems en masse and automating simple, disruptive attacks against the most exposed devices. That’s a very different scenario from the well-resourced, long-term state and criminal campaigns we’ve dealt with until now.

## So, Why Don’t We Just Disconnect It?

The next time a headline lands about hacked pumps or freezers, and you feel the urge to ask why they don’t just disconnect it, remember that there’s rarely a single plug left to pull. These connections were built one sensible business decision at a time, and they now help keep the lights on, the water running, and the food cold.

None of this makes it acceptable to leave systems directly connected where a regulation prohibits it, or where any other feasible security solution exists that doesn’t pose a safety risk. There should absolutely be a best effort to rearchitect these environments and add essential security controls, like the [SANS Five ICS Cybersecurity Critical Controls](https://www.sans.org/white-papers/five-ics-cybersecurity-critical-controls). But these things take time, and a little empathy would go a long way towards understanding the uphill battle low-resourced organisations are fighting in the middle of the current onslaught.

If you’re an enterprise security professional who’s angry about all this, good. We could use your help. Bring your frustration, and bring your curiosity too.

Acronyms used in this post

- **OT** – Operational Technology: hardware and software that monitors and controls physical equipment and processes
- **ICS** – Industrial Control Systems: the computers, networks and devices that run industrial processes
- **IIoT** – Industrial Internet of Things: networked sensors and devices used in industrial settings
- **PLC** – Programmable Logic Controller: a rugged industrial computer that directly controls machinery
- **RTU** – Remote Terminal Unit: a field device that collects data and carries out commands from a central system
- **SCADA** – Supervisory Control and Data Acquisition: systems for monitoring and controlling equipment across wide areas
- **DCS** – Distributed Control System: a plant-level control system with controllers spread throughout a process
