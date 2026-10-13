---
title: 'Launch HN: Agent37 (YC F26) – Low-cost agent sandboxes for running agents'
link: https://news.ycombinator.com/item?id=50059096
source: hnrss-org
published: 2026-10-12T16:46:34Z
updated: 2026-10-12T16:46:34Z
first_seen: 2026-10-13T03:09:55.529316228Z
authors:
- vishnukool
summary: 'Hey HN, I’m Vishnu from Agent37 (https://agent37.com). We build sandboxes for running AI agents at under 10% of the cost of most sandbox providers. Our customers spend an average of $0.37/month per sandbox. You can view a video walkthrough of the product here: https://youtu.be/DK5ISR2tbNo We started as one of those OpenClaw wrapper companies back in February (yup.. judge me). In time we added more and more harnesses. One of the goals I had at the time was to offer a free plan to users, so as to gain more market share. But it was too cost prohibitive to do it right away. Especially since we were bootstrapped and servers were running on my savings. Today we’re actually able to offer the free plan thanks to the optimizations we’ve made since. Over 90% of the new signups we have are just free plan users and they host their personal Hermes/Openclaw/Claude Code etc on our cloud. Our infrastructure cost for the free sandboxes averages around $0.05 to $0.10 per user per month. Here are some techniques that have worked for lowering cost: * When you look at agent harnesses, they have certain patterns in the way they consume CPU and RAM. They need CPU in bursts when they’re initializing or performing certain tasks, but beyond that, they’re often not peaking. They need more RAM for things like browser and computer-use tasks. Once the task is done, usage often comes back down. This is a key insight in lowering cost, so by efficiently managing the CPU/RAM utilization we could lower the cost. * Secondly, you have to skip the hyperscalers when renting the Hosts. I worked at Amazon before and they have 80-90% margins on VMs. So it was an obvious choice once I looked at the prices to go one layer down to vendors like Hetzner and OVH. This alone was probably one of the biggest cost levers in reducing the cost. * Thirdly, you also want to pick a technology stack that lets you efficiently cache and share resources across the host. We use gVisor for better utilization and fast boot times. It lets us use available host memory across sandboxes without reserving a full VM’s memory for each one. There are tradeoffs: our gVisor sandboxes don’t support workloads like Docker-in-Docker or Android emulators. * And finally the size and shape of the hosts you buy also makes a big difference. RAM is typically the bottleneck when it comes to cost and not the CPU. This is specifically true for agent harness workloads, so when buying servers, we optimize for packing more RAM into a given host. We also host full VM sandboxes for workloads that need them, including running Docker at a slightly higher price, but they are still pretty low cost given the rest of the techniques. Overall, my learning has been that just by focusing on a niche of sandbox usecases, and optimizing for it you can give a much better cost/performance tradeoff. I was previously a Senior Software Engineer at Amazon and an Engineering Manager at Quantcast. I built Amazon''s internal AI chatbot, Diya, and their internal coding harness, Ziya. If you’re someone who’s looking to run agents on sandbox and are exploring a sandbox vendor, come check us out. We’re particularly looking for developers building messaging agents to try their workloads and challenge the limits and economics of our platform. Thanks! https://agent37.com Comments URL: https://news.ycombinator.com/item?id=50059096 Points: 112 # Comments: 74'
content: feed
html: 2026-10-12-launch-hn-agent37-yc-f26-low-cost-agent-sandboxes-for.html
---

Hey HN, I’m Vishnu from Agent37 ([https://agent37.com](https://agent37.com)). We build sandboxes for running AI agents at under 10% of the cost of most sandbox providers. Our customers spend an average of $0.37/month per sandbox.

You can view a video walkthrough of the product here: [https://youtu.be/DK5ISR2tbNo](https://youtu.be/DK5ISR2tbNo)

We started as one of those OpenClaw wrapper companies back in February (yup.. judge me). In time we added more and more harnesses.

One of the goals I had at the time was to offer a free plan to users, so as to gain more market share. But it was too cost prohibitive to do it right away. Especially since we were bootstrapped and servers were running on my savings.

Today we’re actually able to offer the free plan thanks to the optimizations we’ve made since. Over 90% of the new signups we have are just free plan users and they host their personal Hermes/Openclaw/Claude Code etc on our cloud. Our infrastructure cost for the free sandboxes averages around $0.05 to $0.10 per user per month.

Here are some techniques that have worked for lowering cost:

\* When you look at agent harnesses, they have certain patterns in the way they consume CPU and RAM. They need CPU in bursts when they’re initializing or performing certain tasks, but beyond that, they’re often not peaking. They need more RAM for things like browser and computer-use tasks. Once the task is done, usage often comes back down. This is a key insight in lowering cost, so by efficiently managing the CPU/RAM utilization we could lower the cost.

\* Secondly, you have to skip the hyperscalers when renting the Hosts. I worked at Amazon before and they have 80-90% margins on VMs. So it was an obvious choice once I looked at the prices to go one layer down to vendors like Hetzner and OVH. This alone was probably one of the biggest cost levers in reducing the cost.

\* Thirdly, you also want to pick a technology stack that lets you efficiently cache and share resources across the host. We use gVisor for better utilization and fast boot times. It lets us use available host memory across sandboxes without reserving a full VM’s memory for each one. There are tradeoffs: our gVisor sandboxes don’t support workloads like Docker-in-Docker or Android emulators.

\* And finally the size and shape of the hosts you buy also makes a big difference. RAM is typically the bottleneck when it comes to cost and not the CPU. This is specifically true for agent harness workloads, so when buying servers, we optimize for packing more RAM into a given host.

We also host full VM sandboxes for workloads that need them, including running Docker at a slightly higher price, but they are still pretty low cost given the rest of the techniques.

Overall, my learning has been that just by focusing on a niche of sandbox usecases, and optimizing for it you can give a much better cost/performance tradeoff.

I was previously a Senior Software Engineer at Amazon and an Engineering Manager at Quantcast. I built Amazon's internal AI chatbot, Diya, and their internal coding harness, Ziya.

If you’re someone who’s looking to run agents on sandbox and are exploring a sandbox vendor, come check us out. We’re particularly looking for developers building messaging agents to try their workloads and challenge the limits and economics of our platform.

Thanks!

[https://agent37.com](https://agent37.com)

* * *

Comments URL: [https://news.ycombinator.com/item?id=50059096](https://news.ycombinator.com/item?id=50059096)

Points: 112

\# Comments: 74
