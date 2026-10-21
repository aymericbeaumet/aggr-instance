---
title: Apple routed a graphite pipe through the iPhone Duo's hinge to dissipate heat
link: https://www.pcmag.com/news/apple-routed-a-graphite-pipe-through-the-iphone-duos-hinge
source: hnrss-org
published: 2026-10-13T07:52:54Z
updated: 2026-10-13T07:52:54Z
first_seen: 2026-10-16T09:40:32.304738087Z
authors:
- giuliomagnifico
labels:
- apple
- computers electronics
- mobile phones
content: extracted
html: 2026-10-13-apple-routed-a-graphite-pipe-through-the-iphone-duo-s-hinge.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50068203
  num_comments: 94
  points: 135
---

The [iPhone 18 Pro](https://www.pcmag.com/reviews/apple-iphone-18-pro) and [18 Pro Max](https://www.pcmag.com/reviews/apple-iphone-18-pro-max) push the boundaries of what mobile phones can do, such as handling on-device Siri AI calculations and running computational photography algorithms to deliver the best possible images. When it's released on Oct. 23, the [iPhone Duo](https://www.pcmag.com/news/hands-on-apples-iphone-duo-refines-the-foldable-formula-apple-event) promises similar capabilities, but in Apple's first-ever foldable. It's all made possible by the [A20 Pro processor](https://www.pcmag.com/news/a20-pro-iphone-chip-explained-apple-event) inside the latest iPhone line, a new system-on-a-chip (SoC) from Apple's engineers that brings laptop-class power to your pocket.

Developing the SoC posed a tough question for Apple. Would the same chip work in the slab-style iPhone 18 Pros *and* the iPhone Duo? Each has its own set of requirements and constraints, not only for the chip and its position inside the phone's chassis, but also for effective heat management. The less room there is, the more difficult it is to move heat away from the components while maintaining performance. Managing thermal conditions plays a vital role in any device, and the limited internal space of slimmer-than-ever phones—including one that folds in half—makes it particularly tricky.

I spoke with Dan Murray, Apple's vice president of silicon engineering, and Banafsheh Barabadi, senior manager of thermal product design, to learn how Apple’s engineers navigated these new challenges and squeezed even more performance from the A20 Pro across two very different iPhone designs.

![Confessions of a Fold-aholic: Can the iPhone Duo Break My Samsung Addiction?](https://cdn.ex.co/video-uploads/production/0010J00001l1J64QAE/a0b49779-3886-59e0-a0b0-a2d9b17eaae2-thumbnail.jpeg?cb=1789172456188)

![PCMag Logo](https://www.pcmag.com/images/pcmag.svg)

Confessions of a Fold-aholic: Can the iPhone Duo Break My Samsung Addiction?

* * *

## Solving the Engineering Puzzle: Pack Laptop-Class Power Into an iPhone

Apple says the A20 Pro has an "M-class-inspired packaging design." Macs run on the company's own M-branded processors, the most current being the M5 Max and M6. These chips power Apple's MacBook Pros and [Mac mini](https://www.pcmag.com/reviews/apple-mac-mini-2026-m6), respectively. The A20 Pro draws some of its basic design elements from the M-line chips—specifically, their layouts.

Chips designed for phones are often stacked vertically to save space in the X-Y dimensions. Real estate on the logic board is a premium. Although this method saves side-to-side space, it introduces its own issues with data flow and heat. For example, the DRAM module is often placed on top of the processor, which can slow things down.

"With all of our iPhone SoCs before A20 Pro, we used a packaging technology called package-on-package or POP," Murray told me. "And with POP packaging, the SoC and DRAM are placed in a vertical stack with the DRAM directly on top of the SoC. But it creates challenges from a thermal perspective. Since the DRAM is physically on top of the SoC, it limits the ability to extract heat from the SoC into the system. So for A20 Pro, we brought a new packaging technology to the iPhone."

iPhone 18 Pro internal view of the rear panel (Credit: Apple)

Because laptop chassis have more internal space, the DRAM module can be placed next to the processor, facilitating quicker data speeds and more effective heat management. Apple follows this configuration with the A20 Pro, unstacking the DRAM module, and placing it next to the chip rather than on top of it. This is called a wafer-level multichip module, or WMCM. Though this increases the X-Y dimensions, Apple worked closely with the system team to ensure the phones' internals could support this slightly larger package.

Qualcomm [made a similar design change](https://www.pcmag.com/news/qualcomm-splits-its-first-2nm-snapdragon-flagship-chip-into-two) to the placement of the DRAM module with its recently announced Snapdragon 8 Elite Extreme Gen 6 and 8 Elite Gen 6 chips. The chipmaker's claims about its performance are lofty, but we won't be able to fully test this chip until phones built around it arrive later this year.

Apple says CPU cores and other elements are often developed once within the silicon team and then customized for different devices, such as MacBooks, iPads, and iPhones. The A20 Pro is the company's first 2nm design for phones, which Murray says delivered greater transistor density, improved power efficiency, and higher performance.

The chip has two super-cores for short, bursty workloads and four efficiency cores for background tasks and sustained workloads, which Apple says equate to a 20% performance improvement. The A20 Pro adds another GPU core, bringing the total to seven and delivering a 40% gain in graphics performance. The GPU also benefits from a second-generation neural accelerator, which improves throughput by 50%.

The A20 Pro has twice as many neural engine cores as the A19 Pro, jumping from a single bank of 16 cores to two banks of 16, for a total of 32. Apple says these two engines can be used either independently or in a bonded mode that uses all 32 at once. This is critical for on-device AI calculations.

iPhone 18 Pro internal view, display side (Credit: Apple)

The iPhone Duo's A20 Pro variant is just slightly different. "For iPhone Duo, we added a second display engine to support simultaneous operation of the inner and outer displays," says Murray, "and with the increased CPU and GPU performance, we ensure we can deliver a smooth user experience across the displays. For the inner display, we also added support for placing the FaceTime camera behind the display."

Put this all together, and you have a very powerful chip that runs fast, handles graphics with ease, churns through algorithms in a blink—and puts out a lot of heat. Apple had to rethink its thermal management system to dissipate the high temperatures generated by the processor.

* * *

## Keeping Things Cool With Completely Redesigned Thermals

The [iPhone 17 Pro](https://www.pcmag.com/reviews/apple-iphone-17-pro) and [Pro Max](https://www.pcmag.com/reviews/apple-iphone-17-pro-max) were the first iPhones to employ vapor chambers. These assisted the phones' A19 Pro chip and moved heat from the SoC out to the rear panel, where it could dissipate. Other phone makers have turned to vapor chambers, too. RedMagic, for example, uses a 3D vapor chamber with a composite liquid metal and a fan to keep the [11S Pro gaming phone](https://www.pcmag.com/reviews/redmagic-11s-pro) running cool.

Apple took the basics of the iPhone 17 Pros' vapor chamber design and reengineered it not only for the higher temperatures generated by the A20 Pro but also for the different chassis designs of the iPhone 18 Pros and iPhone Duo.

The phones and processor together "represent a fundamental state change in the thermal design, and it starts with a deep collaboration with the silicon team and the WMCM packaging," Barabadi told me. "For the first time, we were able to couple silicon directly to our thermal solution, and we designed the entire system around that opportunity."

iPhone 18 Pro vapor chamber and battery (Credit: Apple)

Barabadi explains that Apple followed four pillars in its approach to heat management. The first pillar is the direct silicon access. Because Apple removed the DRAM from the vertical stack, "that \[gave\] us a direct thermal path to our next generation vapor chamber. It basically eliminates the thermal bottleneck that previously existed on all POP packaging and allows us to extract the heat from the processor far more efficiently."

The second pillar is a new material that bridges the gap between the A20 Pro and the vapor chamber. "We call it metal over foam," says Barabadi, and it "basically creates a direct thermal and mechanically compliant coupling between the SoC and the vapor chamber. It is highly effective in removing the heat, meeting the strict mechanical requirements of the system. It took us months of development to get the material properties exactly right to be able to use this material."

The third pillar is the vapor chamber itself, which has a surface area three times larger than that of the one in the iPhone 17 Pro. It also has more de-ionized water inside to cycle heat throughout the structure. In the iPhone 18 Pro and Pro Max, the vapor chamber does more than simply reroute heat. "It serves as the central structural plane for the device," explains Barabadi. The battery and the logic board are mounted directly to it. "So what it does is simultaneously \[act as\] a primary thermal solution and the backbone of the mechanical system. And we use 80% recycled stainless steel for the strength that was required to support the internal components."

Apple didn't stop there. The fourth pillar is a new layer applied to the back of the screen to assist. "We introduced nanotwin copper as our new display metal plate material. So this piece ... is the back display plate, and what makes this unique is that it's a new type of material that provides a unique combination of mechanical strength that is required for the display and the high thermal conductivity that hasn't previously been available in any smartphone. It combines those two together." A high-conductivity graphite layer is also applied.

iPhone 18 Pro graphite layer (Credit: Apple)

Combining the nanotube copper display plate with high-conductivity graphite improves heat spread laterally across the display. It reduces hotspots and improves the overall thermal profile.

"When we combine these four pillars together, it meaningfully improves the thermal performance across every metric, and these gains are realized across sustained workloads such as gaming and video recording, and also the short-duration bursty workloads, like on-device AI," notes Barabadi. This is also why the iPhone 18 Pro models can charge faster than older iPhones.

* * *

## A Clever Use for the iPhone Duo's Hinge

Although most of the engineering described above applies to all three phones, the team had to tackle a different set of thermal challenges with the iPhone Duo.

iPhone Duo internal view (Credit: Apple)

For the Duo, the team created a different vapor chamber that's thinner than the one in the iPhone 18 Pro. It's still integrated into the A20 Pro, but rather than serve as a central piece of the support structure, it floats in the middle. The Duo may be ultra thin when open, but because it's a folding device, its thickness doubles when closed. This is a problem when it comes to heat.

"This was one of the most technically challenging aspects of our design," says Barabadi. To surmount the issue, Apple routed a flexible graphite pipe through the hinge itself to transfer heat between the two halves, thereby increasing the surface area available for heat dissipation. "This part need\[ed\] to be co-engineered between our thermal team and the mechanical team. It's a flexible material that needs to survive, as you can imagine, many hinge cycles while maintaining its thermal connectivity, and also its structural integrity throughout the product life cycle."

* * *

## Cooler Systems Yield Improved Performance

Apple's goal for the latest iPhones was to enable the "best possible performance," says Barabadi. "We \[are\] able to achieve 35% higher sustained GPU performance than iPhone 17 Pro in a form factor that people didn't think was possible. All of it exists so that the experiences that the people care about most can run harder, longer, and more efficiently than before, such as gaming, camera, and Apple Intelligence. What I find most compelling about this work is that none of it is visible to the user."

We've seen the results already. In our own testing, the A20 Pro delivered some of the highest benchmark tests we've ever recorded. The efficiency gains mean longer-lasting batteries, with the iPhone 18 Pro Max [winning a PCMag Lab Award](https://www.pcmag.com/articles/after-hundreds-of-battery-tests-ive-never-seen-a-phone-last-this-long) for its record-setting longevity. In my testing, I found Siri AI runs smoothly and delivers more accurate results quicker than ever while the cameras take breathtaking photos and record sharp video, surpassing the performance of last year's models.

We haven't tested the A20 Pro in the iPhone Duo just yet, but we expect to in the coming weeks. Can Apple's first folding phone stay cool under pressure? Will it provide the same seamless experiences without suffering performance gaps? According to Apple's Murray and Barabadi, it should run just fine, without overheating.

We look forward to finding out.

## About Our Expert

![Eric Zeman](https://i.pcmag.com/imagery/authors/01yCxdcXQrL8HzDTtSt6RNi.fit_lim.size_100x100.v1760122065.jpg)

Eric Zeman

Managing Editor, Consumer Electronics

eric.zeman@pcmag.com

* * *

Experience

I’m PCMag’s Managing Editor for Consumer Electronics content, overseeing an experienced team of reviewers and product testers. I’ve been covering technology for more than 25 years. Prior to PCMag, I worked at outlets such as Android Authority, Fortune, InformationWeek, and Phonescoop.

Latest By Eric Zeman

[Read Full Bio](https://www.pcmag.com/authors/eric-zeman)
