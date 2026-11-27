---
title: 'How we built it: Jurisdiction resolution for Stripe Tax'
link: https://stripe.dev/blog/how-we-built-it-jurisdiction-resolution-for-stripe-tax
source: stripe-com
published: 2025-07-10T00:00:00Z
updated: 2025-07-10T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Erich Rentz
summary: Our new jurisdiction resolution system (JRS) is a faster, less resource-intensive solution to the challenging problem of determining tax obligations in places with complicated, overlapping tax jurisdictions.
content: extracted
html: 2025-07-10-how-we-built-it-jurisdiction-resolution-for-stripe-tax.html
remote_preview:
  url: https://stripe.dev/og/blog/how-we-built-it-jurisdiction-resolution-for-stripe-tax.png
  alt: 'Stripe Developer Blog: How we built it: Jurisdiction resolution for Stripe Tax'
---

![Guides > JRS > Header image](https://stripe.dev/images/how-we-built-it-jurisdiction-resolution-for-stripe-tax/image-0.jpg)

Illustration by Cynthia Alfonso

Stripe’s users rely on us to calculate tax correctly and quickly, no matter where a transaction is happening in [100+ supported countries](https://docs.stripe.com/tax/supported-countries#supported-countries). This is a technical challenge particularly in the US, where there are more than 16,000 different combinations of sales tax rates and rules that can apply to an internet purchase depending on where you pay. To make things even harder, the geographic boundaries that dictate which rules apply where are intricate and constantly changing. As a result, determining which taxes apply to which transactions—quickly and accurately—is a surprisingly complex data engineering and algorithmic challenge.

To address it, we’ve introduced the patent-pending jurisdiction resolution system (JRS). When presented with a transaction in the US, the JRS finds the right taxing jurisdictions within a few thousandths of a second. This system runs internally, rather than relying on outsourced processes, which keeps [Stripe Tax](https://docs.stripe.com/tax) fully in control of reliability and accuracy even as tax rates change.

The JRS has offline and online components. Offline, it splits the US into nonoverlapping regions defined by unique combinations of taxing authorities. Every transaction within a region is subject to the same state, county, and local taxes. We call these regions Stripe places of taxation (SPOTs). The JRS figures out the geographic boundaries of each SPOT offline, and then it stores that data to be called on later. At calculation time, the online part of the system calls the data and figures out which SPOT applies. Since the data is precomputed, the online computation is completed with minimal latency. Defining SPOTs offline, and determining which ones apply online, required us to reinvent how we organize and query geospatial data.

### Postal addresses aren’t enough

You might ask, “Why not use postal addresses to determine which taxes apply to which transactions?” That would be a simple solution, but it doesn’t work since postal addresses do not align neatly with taxing jurisdictions. These two houses in Drexel, Missouri, are in the same city, with the same ZIP code, with very close house numbers—but they’re in different counties, and therefore subject to different sales tax. Postal addresses don’t contain enough information to pick up on that distinction.

![Blog > JRS > Postal addresses image](https://stripe.dev/images/how-we-built-it-jurisdiction-resolution-for-stripe-tax/image-1.png)

Given postal addresses don’t work, we had to develop a more exact way of locating addresses within tax zones. To do this, we collected public data on jurisdiction boundaries—states, districts, counties, and so on. This data is essentially a series of points on a map that define the outline of a polygon. At this point, we could then situate postal addresses inside these polygons to determine the relevant taxes.

However, there were two barriers to taking this step directly:

1. Publicly available geospatial data just isn’t that accurate. It varies by quality and consistency across state and local authorities. This is a problem because a deviator of just a few inches could mean addresses are incorrectly assigned and charged the wrong sales tax. Moreover, neighboring jurisdictions must be consistent with one another, meaning they border gaplessly and without overlaps. But the state and local resources that maintain this data are of varying quality and consistency.
2. The polygons are very complex. They can have hundreds—or in some cases, even thousands—of sides, reflecting the intricacy of the legal boundaries of counties, cities, and states. The more complex the polygon, the more algorithmically intensive it is to determine if an address lies within it (especially along the boundaries). For this reason, applying tax via computations on polygons threatened to add latency to our transaction processing, which we didn’t want.

![Blog > JRS > Sandy, UT SPOT outline](https://stripe.dev/images/how-we-built-it-jurisdiction-resolution-for-stripe-tax/image-2.png)

This map of Sandy, Utah, shows how complex SPOTs can get even for a small city in the Salt Lake City metro area. Notice the irregular shape of the boundary and the presence of “islands” within the polygon that are not part of the SPOT.

### Offline processing: Defining SPOTs accurately

To tackle the problem of low-quality jurisdictional definitions, we developed a geographic information system (GIS), which cleans, centralizes, and standardizes jurisdiction data. The GIS stores boundaries for 53 states (Puerto Rico, Virgin Islands, and Washington, DC are considered states for simplicity), 3,225 counties, approximately 10,000 cities, and around 3,000 districts. (The exact numbers change as new areas enact and repeal taxes.)

Once we had upgraded the jurisdictional data, we overlaid it on a central map. We first overlaid state boundaries, which are easy to handle because they never overlap. Then we overlaid county boundaries, which are also straightforward because they never stretch across state lines. Last, we added jurisdictional data for cities and districts; these are more complicated because cities can cross county boundaries while districts can cross both city, county, and other district boundaries. That completed, we identified SPOTs (i.e., unique tax jurisdictions) as regions where a specific combination of jurisdictions overlap.

The GIF below illustrates this process for Utah. We begin with the state boundary, then overlay the boundaries for county, city, and district. This results in the SPOT boundaries in yellow.

![Blog > JRS > Utah SPOT gif](https://stripe.dev/images/how-we-built-it-jurisdiction-resolution-for-stripe-tax/image-3.gif)

This process gives us accurate SPOTs, but only at a moment in time, because tax jurisdictions change often. To keep the GIS up-to-date, the Stripe Tax team carefully tracks any changes in the taxing landscape that impact SPOT boundaries. Each time jurisdiction boundaries change, we recompute the SPOTs for the relevant state. New and updated SPOT boundaries are added to the GIS and tagged with the time the change occurs. Old SPOTs are retained and similarly tagged with the time the change occurs. This makes our SPOTs time-aware, meaning we can calculate tax accurately on transactions that occur right now, as well as recalculate tax accurately on transactions that occurred in the past.

### Online processing: Matching addresses to SPOTs

With the SPOT polygons in place, the next step in applying the correct taxes occurs at the time of transaction. This requires matching the customer’s postal address with a SPOT polygon. This process needs to be fast—so customers don’t notice a lag—and extremely accurate. If we match an [address](https://docs.stripe.com/tax/customer-locations) to the wrong SPOT, we’ll get the sales tax wrong. It also can’t easily be done offline given scale: with more than [166 million mailing addresses](https://facts.usps.com/size-and-scope/) and 16,000 SPOTs in the United States, it isn’t practical to create a database that pairs every address with a SPOT.

Instead, we need to match addresses to SPOTs on demand, as efficiently as possible. Typically this kind of matching is done through the application of “point-in-polygon” algorithms, which compare the coordinates of the target point (i.e., a customer’s geocoded address) with the coordinates of the vertices of the polygon (the places where two edges meet). The more vertices a polygon has, the more calculations the algorithms need to perform—with the total number of calculations scaling geometrically with the number of vertices. For complex SPOTs with many vertices, the total number of calculations can grow into the millions, which is too many to perform in a timely manner while a customer is waiting for their online transaction to process.

Providing fast and accurate tax analysis meant we had to find a way to speed up traditional methods.

### Simplifying SPOTs

The determining factor on algorithmic run-time is the complexity of the SPOTs. To achieve the speedups we needed, we decided to replace complex SPOTs with “bounding boxes”—rectangles formed by connecting the minimum and maximum X and Y coordinates on the original SPOT. In the image below, you can see the generation of the bounding box for the earlier SPOT example.

![Blog > JRS > SPOT bounding box gif](https://stripe.dev/images/how-we-built-it-jurisdiction-resolution-for-stripe-tax/image-4.gif)

Replacing complex SPOTs with bounding boxes simplifies the problem: in place of thousands of vertices, we only have to consider four. Of course, a single crudely defined bounding box might encompass multiple tax jurisdictions, essentially recreating the problem we started out with. So, from there, we create bounding boxes within bounding boxes, stored in a balanced R-tree that is rebuilt—using the Sort-Tile-Recursive (STR) algorithm—each time the data is updated.

We can navigate these bounding boxes by navigating this R-tree. We zoom in on the geocoded address a little bit at a time, starting from a box that bounds an entire state (in red) and moving to a smaller box at each step. At the final step, we are left with a bounding box (in white) that might span a handful of candidate SPOTs.

![Blog > JRS > Utah and R-tree](https://stripe.dev/images/how-we-built-it-jurisdiction-resolution-for-stripe-tax/image-5.png)

At this point, we implement a point-in-polygon algorithm to determine which tax jurisdictions apply. This is the kind of calculation that was prohibitive when we started—there were too many SPOTs to consider. But by reducing the scope of candidates, it becomes feasible.

While the approach resulted in significant latency gains, there were still some problematic outlier areas. The first major optimization was prioritizing smaller bounding boxes within each level. These smaller boxes are typically associated with more densely populated areas, which are more likely to be making requests. Second, we disaggregated complicated SPOTs that reflected disjoint areas. Disjoint SPOTs are ones consisting of noncontiguous islands (imagine Hawaii). Instead of maintaining a bounding box for the entire SPOT (i.e., draw a box around Hawaii), we split the SPOT into component parts (each Hawaiian island), meaning each bounding box was more discrete and targeted.

For most states, we can match addresses to SPOT polygons in just a few milliseconds. The graph below shows the 95th percentile latency time for the 53 states, territories, and Washington, DC. All are under 10 milliseconds, except for South Carolina. That’s because tax jurisdictions in the Charleston area are unusually complicated. Charleston alone consists of 73 disjoint areas.

![Blog > JRS > JRS latency graph](https://stripe.dev/images/how-we-built-it-jurisdiction-resolution-for-stripe-tax/image-6.png)

### Looking ahead

Our dual approach of offline SPOT creation and online bounding boxes allows us to make faster tax calculations at the time of a transaction—and allows us to precisely update jurisdictions as tax regulations change. If a sales tax update is enacted at, say, midnight in the relevant time zone, a JRS-like strategy helps us correctly identify which time zone that is and update the rules at the right time.

We’re still working to perfect the JRS. One remaining issue is memory—when SPOT polygon boundaries change, we keep the out-of-date SPOTs, so they can be retrieved later if necessary. But as time goes on, the memory load of this data piles up. We’re working on techniques that ease this memory burden while maintaining focus on the core value [Stripe Tax](https://stripe.com/tax) offers to users: delivering fast, accurate tax calculations. By employing the most cutting-edge techniques in geospatial data management and processing, we aim to continue to be the most reliable sales tax solution for businesses around the world.

To learn more, [read our docs](https://docs.stripe.com/tax) or [get in touch](https://stripe.com/contact/sales).

Already a Stripe user? [Get set up](https://dashboard.stripe.com/tax/overview) in the Stripe Dashboard.
