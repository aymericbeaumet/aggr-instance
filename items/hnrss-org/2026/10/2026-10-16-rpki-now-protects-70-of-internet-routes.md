---
title: RPKI now protects 70% of internet routes
link: https://ipinfo.io/blog/rpki-protects-70-internet-routes
source: hnrss-org
published: 2026-10-16T00:52:35Z
updated: 2026-10-16T00:52:35Z
first_seen: 2026-10-16T09:40:32.304738087Z
authors:
- emot
content: extracted
html: 2026-10-16-rpki-now-protects-70-of-internet-routes.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50112330
  num_comments: 48
  points: 155
---

The Internet relies on networks telling each other which IP addresses they can deliver traffic to, using [BGP](https://community.ipinfo.io/t/border-gateway-protocol-bgp-explained/5546?ref=ipinfo.io) (Border Gateway Protocol). These routing announcements have historically depended heavily on trust. When a network announces addresses it shouldn’t, accidentally or deliberately, traffic can be sent to the wrong place and services can become unreachable.

On February 24, 2008, Pakistan Telecom (AS17557) tried to block YouTube inside Pakistan by announcing a more specific route for YouTube's addresses. Its upstream provider, PCCW Global (AS3491), accepted the announcement and passed it to the rest of the Internet, redirecting YouTube traffic to Pakistan globally for roughly two hours ([RIPE NCC's case study](https://www.ripe.net/about-us/news/youtube-hijacking-a-ripe-ncc-ris-case-study/?ref=ipinfo.io)). Work on RPKI, which lets networks reject announcements like that one, had started at the IETF two years earlier, and the regional Internet registries began issuing RPKI certificates in 2011.

[RPKI](https://ipinfo.io/data/rpki?ref=ipinfo.io) (Resource Public Key Infrastructure) lets the holder of a block of IP addresses publish a signed record, a Route Origin Authorization (ROA), naming the networks (Autonomous Systems, or ASes) allowed to announce it. Network operators that check RPKI compare announcements against those records and can reject the ones that fail.

We looked at a year of IPinfo’s daily RPKI data to see how much of the Internet it now protects, and where it falls short. What we found (TL;DR):

- 70.3% of routed prefixes (blocks of IP addresses) are now RPKI-valid, up from 54.0% a year ago, a 30% rise. That’s 16.4 points, against 3.9 and 3.2 in the two years before.
- China drove most of the jump: its IPv4 addresses in valid routes went from 3.7% to 83.0%, almost 60% of the global gain counted by addresses. Outside China, the share of addresses rose 5.2 points.
- Invalid routes barely travel: a typical valid route is seen by 596 vantage points, an invalid one by two.
- Loose ROAs are the biggest hole left: 16.2% of covered IPv4 routes leave room for a forged, RPKI-valid route, the setup used against Hetzner on August 28.
- Protection can disappear quietly: SEACOM, Saudi Telecom and Orange Maroc each lost ROAs for months.
- ASPA is arriving fast, from 131 to 3,272 networks, but it verifies only about 3.5% of routing, and one operator’s incomplete record causes about a quarter of the mid-path failures.

![](https://storage.ghost.io/c/b1/57/b157bf7f-0fe2-447f-a945-12d7951755b3/content/images/2026/10/70-percent-RPKI.png)\
RPKI grew more in the past year than in the two years before it combined: 16.4 points, against 3.9 and 3.2. Data pulled on September 28 of each year.

## Invalid Routes Barely Propagate

In August, the [Internet Society Pulse](https://pulse.internetsociety.org/en/blog/2026/08/do-rpki-updates-affect-the-internets-stability/?ref=ipinfo.io) found RPKI-related activity accounts for under 1% of daily BGP update volume, a share that is rising but still small, so RPKI is not itself destabilizing routing. We wanted to know whether RPKI was actually protecting Internet routing.

![](https://storage.ghost.io/c/b1/57/b157bf7f-0fe2-447f-a945-12d7951755b3/content/images/2026/10/propagation-by-RPKI.png)\
Number of BGP vantage points (peers of the RIPE RIS and RouteViews collectors) seeing each prefix, by RPKI status. September 28, 2026, across 1,614,288 routed prefixes.

Routes with an RPKI record and routes with none travel equally far: a typical one is seen by 596 and 595 vantage points. Routes that fail the check are seen by just two. Now that enough networks check, they are dropping these invalid routes. Nearly half of all invalid routes reach only a single vantage point. When the wrong network announces the addresses, the clearest kind of hijack or mistake, 71% reach only one.

This changes how country-level RPKI statistics should be read. A country can look bad because of announcements almost nobody sees.

Simply counting invalid routes can badly exaggerate how much of a country is affected. Spain has the highest share of invalid IPv4 prefixes at 28.2%. Count the addresses in those routes instead, weighted by how many vantage points see each one, and it falls to 0.057%. Slovakia falls from 17.4% to almost zero. Italy has the highest figure of any country with at least a million IPv4 addresses, 0.121%, up from 0.075% a year ago, and 90% of it is one network.

WIND TRE (AS1267) announces 34 routes in its own address space that its own ROAs don't allow: 19 are more specific than the ROAs permit, and 15 come from AS1267 while the ROA names WIND TRE's other network, AS24608. None reaches more than 77 vantage points, against about 617 for the signed blocks around them.

![](https://storage.ghost.io/c/b1/57/b157bf7f-0fe2-447f-a945-12d7951755b3/content/images/2026/10/invalid-share-by-country.png)\
Gray: share of a country's routed IPv4 prefixes that are RPKI-invalid. Blue: share of its IPv4 addresses in invalid routes, weighted by how many vantage points see each route. The 12 countries with the highest gray share and at least a million IPv4 addresses, September 28, 2026. The number after each country is its rank by gray share.

## China Drove Most of the 2026 Jump

Counted by IPv4 addresses on September 28 each year, the share in RPKI-valid routes rose from 42.7% in 2023 to 47.9% in 2024, 53.1% in 2025 and 65.0% in 2026. The last jump was 11.9 points, against about five in each previous year.

Removing China shows how much of that was one country. Outside China, the share rose from 58.0% to 63.2%, a 5.2-point gain, a little less than the year before.

China’s change was also sudden: 3.7% of its IPv4 addresses were in valid routes in September 2025, and 83.0% a year later. Most of that happened this year: at the end of December 2025, just 2.9% of China's routed prefixes were valid. It also changed which way it moves the world figure. In 2025 China pulled the global average down: 53.1% worldwide against 58.0% without China. In 2026 it pushed it up: 65.0% against 63.2%. Others moved a long way too: Egypt from 23% to 99%, Kazakhstan from 16% to 84%, Mexico from 37% to 78%, and South Africa from 38% to 60%.

![](https://storage.ghost.io/c/b1/57/b157bf7f-0fe2-447f-a945-12d7951755b3/content/images/2026/10/global-china.png)\
Share of IPv4 addresses in RPKI-valid routes, September 28 of each year. Outside China the share rose about five points in each of the last two years. The lines cross in 2026, when China moved from lowering the global figure to raising it.

![](https://storage.ghost.io/c/b1/57/b157bf7f-0fe2-447f-a945-12d7951755b3/content/images/2026/10/RPKI-validity-by-country.png)\
Share of each country's IPv4 addresses in RPKI-valid routes, September 28, 2026. Gray means no data.

Where the unprotected addresses are is just as concentrated. Of all routed IPv4 space not in RPKI-valid routes on September 28, nearly 60% is in the United States, and about one in four addresses belongs to the US Department of Defense (more on who holds IPv4 in an upcoming post). Most of that is space the Department began announcing in 2021 to watch for misuse, not to run services.

## IPv6 Has Moved Ahead

In early 2025 there were concerns that IPv6 adoption of RPKI was slowing. Instead, it accelerated and has now overtaken IPv4. In [RPKI's 2024 year in review](https://labs.ripe.net/author/job_snijders/rpkis-2024-year-in-review/?ref=ipinfo.io) for RIPE Labs, Job Snijders noted IPv6 ROA growth "seems to be stalling." Over our window, IPv6 routed prefixes marked valid rose from 56% to 79% while IPv4 prefixes rose from 54% to 68%, putting IPv6 roughly ten points ahead. The [Hurricane Electric](https://bgp.he.net/report/rpki_and_aspa?ref=ipinfo.io) RPKI report shows the same ordering.

Two other metrics he flagged also changed: the average number of address blocks per ROA climbed back from 2.31 at the end of December to 2.62, and publication points, which he counted falling from 63 to 53 and read as consolidation, rose from 73 to 91 as more organizations began hosting their own repositories instead of relying on their registry.

![](https://storage.ghost.io/c/b1/57/b157bf7f-0fe2-447f-a945-12d7951755b3/content/images/2026/10/ipv6-vs-ipv4.png)\
Share of routed prefixes marked RPKI-valid, by address family. Daily, September 28, 2025 to September 28, 2026.

## What RPKI Does Not Do

RPKI validates who may originate a prefix. It does not validate the path, and it cannot catch an attacker who stays inside the permission you published, so it's not bulletproof.

On August 28, 2026, a route for 162.55.80.0/24 (a block of 256 addresses) appeared with Hetzner's AS24940 forged as the origin. Hetzner's ROA covered the surrounding /16, a block 256 times larger, and allowed announcements as small as a /24, so the forged route satisfied it. The route was RPKI-valid, no legitimate /24 existed to compete with it, and longest-prefix-match meant everyone preferred it. That was enough to obtain a genuine TLS certificate and serve a malicious Virtualizor update to hypervisors, as [documented by Kentik](https://www.kentik.com/blog/latest-bgp-hijack-targets-hosting-software-vendor/?ref=ipinfo.io).

We measured how common that posture is. Of 855,217 RPKI-covered IPv4 routes, 138,698, or 16.2%, have a ROA looser than the route announced and no more-specific route of their own filling the gap. For IPv6 it is 20.9%. That leaves roughly 18 million more-specific prefixes an attacker could announce today, forge the origin, and come out RPKI-valid and uncontested.

Operators in the same business make very different choices here. Among hosting providers, Contabo has an open window on 540 of 553 covered routes and DigitalOcean on 780 of 863, while OVH sits at 11.5%. Access providers are worse still: Cablevision at 98.1%, Shaw at 96.5%, Bharti Airtel at 85.0%.

[RFC 9319](https://www.rfc-editor.org/rfc/rfc9319.html?ref=ipinfo.io) already says the best practice is to leave maxLength unset. maxLength is the setting that lets a ROA also cover smaller blocks inside it, so leaving it unset means a ROA matches only the block actually announced. Hetzner tightened three prefixes after the incident, including the one that was hijacked, though 49 of its 63 covered routes remain open.

![](https://storage.ghost.io/c/b1/57/b157bf7f-0fe2-447f-a945-12d7951755b3/content/images/2026/10/maxlength-exposure.png)\
Share of RPKI-covered IPv4 routes with an open more-specific window, by hosting provider, against the 16.2% global average. (September 28, 2026)

## Where RPKI Coverage Declined

RPKI coverage can also disappear without causing an outage. A missing ROA simply removes the protection, and nobody notices unless they look. We found several cases where that happened over the past year.

Unlike China, Mauritius saw its RPKI coverage fall. The share of its IPv4 addresses in valid routes fell from 94.0% to 69.0% between September 2025 and September 2026. SEACOM (AS37100) lost four of its six ROAs on January 1. One covered 105.16.0.0/12, a block of about a million addresses, 46% of which are in Mauritius. That block alone explains Mauritius's drop, and it still had no ROA on September 28.

Two other operators show the same thing happening and then being fixed. Saudi Telecom (AS25019) lost RPKI protection on 19 routes, including six large blocks totaling about 1.6 million addresses, when the ROAs for those six blocks disappeared between March 30 and April 13. Saudi Arabia's coverage recovered when the ROAs came back on September 21 and 22.

In Morocco, Orange Maroc (AS36925) is the only large operator that publishes ROAs, and most of them went missing twice: from July 2025 to July 2026, and again from July 25 to September 3. Morocco went from 20% covered to almost none, and back.

![](https://storage.ghost.io/c/b1/57/b157bf7f-0fe2-447f-a945-12d7951755b3/content/images/2026/10/operators-missing-roas.png)\
Days with no ROA for the largest address blocks of Saudi Telecom (AS25019) and SEACOM (AS37100), and for most of Orange Maroc's (AS36925) ROAs. The routes stayed reachable but lost RPKI protection. SEACOM's block still had no ROA on September 28, 2026.

## One Measurement Trap

Depending on how you check, an RPKI record can look broken while it is still protecting its routes. We ran into this while counting.

Every signed record carries a link to the certificate above it. When an operator moves its repository, that link can go stale, and a check that inspects records one by one will report a hard failure. But the validation that actually produces the list routers use never follows that link, it walks down from the trust anchors instead.

We found thousands of records flagged as failed that were present in every independently validated set we checked, with their routes still RPKI-valid. So if you are reading anyone's RPKI numbers, including ours, it is worth knowing which of the two they counted. A count of failing records is not a measure of lost protection.

## Path Validation Is Starting to Arrive

ROAs secure the origin of a route. They say nothing about whether the path used to reach that origin makes sense.

ASPA, or Autonomous System Provider Authorization, addresses that. A network publishes a signed record naming its authorized upstream providers, which lets others check observed AS paths against them and catch route leaks that origin validation misses.

Deployment is early but fast. On September 28, 2025, 131 networks had published ASPA records. By September 28, 2026 there were 3,272, 25 times as many.

The growth comes in steps rather than a curve. RIPE NCC sat at about 85 networks from September to late November, then climbed to 470 by the end of December. ARIN stepped up from January 21, APNIC at the end of June and again in September, when its count nearly doubled. Each jump follows a registry making ASPA signing available, so tooling has been the main driver. One detail worth highlighting: [APNIC announced its support on July 9](https://blog.apnic.net/2026/07/09/aspa-at-apnic-strengthening-bgp-path-validation/?ref=ipinfo.io), and its curve started climbing on June 26.

![](https://storage.ghost.io/c/b1/57/b157bf7f-0fe2-447f-a945-12d7951755b3/content/images/2026/10/aspa-adoption.png)\
Networks publishing ASPA records, stacked by registry, September 28, 2025 to September 28, 2026. The December, January, June, and September increases line up with registries shipping signing tools.

That coverage is now enough to ask the better question: how much of observed routing is ASPA-valid?

On September 28, 2026, 9.7% of observed routing passed ASPA checks, up from 6.2% a year earlier. Most of that is automatic: under the draft rules, a path through only one or two networks always passes, and those short paths were about 6% of routing in both years.

Take them out and the records networks have published verify about 3.5% of routing, up from roughly 0.1%.

Almost all the rest is not failing. It is unknown, because too few networks along those paths have published records to tell. In practice, ASPA can check only a small slice of routing today, and that slice grows with every network that publishes a record.

Of the paths that can be judged, those breaking ASPA’s rules mid-path, the signal a collector can read with most confidence, rose from effectively zero to 0.048% of observations. That is not necessarily routing getting worse: a path can only be called invalid once enough of it has declared providers, so as publication grew, paths that were previously unjudgeable became testable and some failed.

![](https://storage.ghost.io/c/b1/57/b157bf7f-0fe2-447f-a945-12d7951755b3/content/images/2026/10/aspa-validation.png)\
Share of observed routing that is ASPA-valid, weighted by route observations. The headline rose from 6.2% to 9.7%, but about 6 points of that is a structural floor; published records account for about 3.5 points, up from 0.1.

Attributing those failures needs care, because the gap sits mid-path: one provider with a stale record makes every one of its customers' routes look invalid, so ranking by origin blames the wrong party.

Attribute to the hop instead and the networks at the top are the ones doing it right. Cogent, Arelion, and GTT each declare AS0, which says they buy transit from nobody. That is true, and it is exactly what stops a stale record further down the path from hiding as "no attestation." The real gaps are smaller: of 3,272 networks publishing ASPA records, 479 have upstream providers visible in our routing data that they never declared, across 658 missing relationships.

The largest single cause is one operator. Syrian Telecom (AS29386) publishes a record naming four carriers. Three of them carry just 16% of the transit we observe through it, and the fourth carries none. The four carriers that handle the other 84% are missing from the record, so about 40% of paths arriving through them fail the check, and the rest come out unknown.

Through Syrian Telecom's own routes and those of two Syrian networks that reach the Internet through it, that record accounts for 21% of all mid-path ASPA failures on the Internet. Another 3 points come from AS201550, a second Syrian Telecom network whose record copies the same four carriers instead of naming the Syrian Telecom networks it connects through. So the global ASPA failure rate is not yet a measure of the Internet’s health: about a quarter of it comes from one operator’s incomplete records.

Eighteen years after PCCW Global passed on Pakistan Telecom's announcement for YouTube's addresses, PCCW is a gap in the newer system. ASPA would not have stopped the 2008 incident; RPKI's origin check is what covers that. But ASPA lets others check PCCW's place in a path, and AS3491 has not published a record. That breaks nothing on its own. Still, nearly 6,000 paths crossing PCCW read ASPA-invalid today, because networks below it publish records that leave PCCW out.

## What Operators Can Do

RPKI now protects most Internet routes, and invalid routes barely travel. The gaps that remain are mostly in the hands of the networks that own the addresses:

- **Publish ROAs for everything you announce.** Three in ten routed prefixes are still not RPKI-valid.
- **Leave maxLength unset.** One in six covered IPv4 routes has a window like the one used against Hetzner.
- **Check that your ROAs are still there.** Saudi Telecom, SEACOM, and Orange Maroc each lost some of theirs, and since nothing broke, it was easy to miss.
- **Publish ASPA, and list every provider.** Syrian Telecom's record leaves out the four carriers that handle most of the traffic we see reaching it, so many routes through them fail the check.

IPinfo tracks the RPKI status of every routed prefix daily: [ipinfo.io/data/rpki](https://ipinfo.io/data/rpki?ref=ipinfo.io).

* * *

***Methodology***: *All figures come from IPinfo’s daily routing and RPKI data, comparing September 28 of each year unless stated. Routing data comes from the public RIPE RIS and RouteViews collectors. A route’s reach is the number of their BGP peers (vantage points) that carry it, and reach figures are medians. “RPKI-valid” means a route matches the validated set of ROAs that routers use, not the status of individual records. Country figures use IPv4 only and count addresses: each route is split across countries using IPinfo geolocation (the current mapping for every year), each address is counted once, and it counts as covered if any valid route includes it. In Chart 3, the prefix share uses each prefix’s first address, and the weighted share multiplies each route’s addresses by its reach. The maxLength figures cover routes seen by at least two vantage points. ASPA figures are weighted by how often each route is observed, and the 0.048% counts failures in the middle of a path only.*
