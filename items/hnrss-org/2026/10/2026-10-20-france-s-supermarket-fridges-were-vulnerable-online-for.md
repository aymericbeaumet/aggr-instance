---
title: France's supermarket fridges were vulnerable online for months
link: https://ipinfo.io/blog/france-fridges-hack
source: hnrss-org
published: 2026-10-20T00:00:49Z
updated: 2026-10-20T00:00:49Z
first_seen: 2026-10-20T07:25:43.733405278Z
authors:
- emot
summary: 'Related: Hackers activate automatic defrosting in supermarket fridges across Europe - https://news.ycombinator.com/item?id=50055304 - Oct 2026 (1007 comments) Comments URL: https://news.ycombinator.com/item?id=50159616 Points: 122 # Comments: 95'
content: extracted
html: 2026-10-20-france-s-supermarket-fridges-were-vulnerable-online-for.html
remote_preview:
  url: https://website-cdn.assets.ipinfo.io/images/og/og_blog.png
---

On the night of October 11 to 12, 2026, fridges and freezers in hundreds of French supermarkets started to warm up. Cyber attackers had pushed the refrigeration systems into defrost mode, which heats the cooling parts to melt ice. Some units [reached 50°C](https://thehackernews.com/2026/10/attackers-heat-french-supermarket.html?ref=ipinfo.io) (122°F), some equipment started to catch fire, [according to the Paris prosecutor](https://www.franceinfo.fr/internet/securite-sur-internet/cyberattaques/un-piratage-pas-si-sophistique-que-ca-ce-que-l-on-sait-de-la-cyberattaque-qui-a-fait-monter-en-temperature-les-frigos-de-supermarches_8235061.html?ref=ipinfo.io), and stores [threw away their fresh and frozen food](https://edition.cnn.com/2026/10/12/europe/france-supermarket-cyberattack-fridges-intl?ref=ipinfo.io). The reports pointed to one maker of refrigeration controllers, Danfoss (a Danish industrial company).

We went through IPinfo's weekly Internet scans to examine these systems before the attack. In this blog post we’ll explore some of the findings. Here’s the TL;DR:

- **France had more exposed fridge dashboards than the rest of the world combined:** 1,331, against 1,197 in the other 72 countries and territories put together.
- **85% ran software with known security flaws.** That includes 527 older controllers Danfoss no longer supports. Only 195 had every known fix.
- **Most had been exposed for at least six months.** 1,019 of the French dashboards were at the same address on March 30, and the share fully updated rose only from 14% to 24% in that time period.
- **France didn't patch worse than other countries.** The flawed share elsewhere was almost the same (84%). France just had far more of these systems online.
- **Most went offline after the attack.** 316 of the 1,331 were still online on October 14.

## What Happened

The numbers from the chains keep growing. [More than 400 stores](https://www.futura-sciences.com/tech/actualites/cyberattaque-plus-400-supermarches-pirates-aucune-rancon-dessous-cyberattaque-inquiete-jusqua-matignon-138509/?ref=ipinfo.io) were hit in total according to Futura-Sciences, and [losses could reach several million euros](https://www.franceinfo.fr/internet/securite-sur-internet/cyberattaques/un-piratage-pas-si-sophistique-que-ca-ce-que-l-on-sait-de-la-cyberattaque-qui-a-fait-monter-en-temperature-les-frigos-de-supermarches_8235061.html?ref=ipinfo.io). Carrefour said 80 of its stores were hit, while Intermarché and Netto, both part of Les Mousquetaires (a French retail group), had [more than 300 hit](https://thehackernews.com/2026/10/attackers-heat-french-supermarket.html?ref=ipinfo.io). Monoprix reported that [about 5% of its stores](https://thehackernews.com/2026/10/attackers-heat-french-supermarket.html?ref=ipinfo.io) were affected.

Leclerc and Coopérative U, two other French supermarket chains, [also confirmed attacks](https://www.franceinfo.fr/internet/securite-sur-internet/cyberattaques/un-piratage-pas-si-sophistique-que-ca-ce-que-l-on-sait-de-la-cyberattaque-qui-a-fait-monter-en-temperature-les-frigos-de-supermarches_8235061.html?ref=ipinfo.io). Coopérative U's CEO, Dominique Schelcher, [told RTL](https://www.rtl.fr/actu/justice-faits-divers/cyberattaque-contre-les-frigos-le-pdg-de-cooperative-u-affirme-sur-rtl-qu-il-n-y-a-pas-eu-de-ruptures-de-la-chaine-du-froid-dans-la-majorite-des-80-magasins-touches-7900684435?ref=ipinfo.io) that 80 of its 2,000 stores were hit. He added that the stores don't buy from Danfoss directly but use a Danfoss system supplied by their refrigeration contractors.

[Danfoss said](https://thehackernews.com/2026/10/attackers-heat-french-supermarket.html?ref=ipinfo.io) most of the affected equipment was "legacy,” naming two older controller models, the AK-SM 800 and AK-SC 355. CERT-FR (France's cybersecurity agency) [said it knew of numerous compromises](https://cert.cyber.gouv.fr/alerte/CERTFR-2026-ALE-012/?ref=ipinfo.io) of AK-SM and AK-SC controllers. It told operators to disconnect them or restrict who can reach them, and [updated its alert on October 13](https://cert.cyber.gouv.fr/alerte/CERTFR-2026-ALE-012/?ref=ipinfo.io) to add XWEB supervisors (systems that monitor a store's fridges and freezers) from Copeland, a US maker of cooling equipment, and Dixell, its Italian brand. Its alert says attack campaigns are probably still ongoing. Nobody has said who did it or how they got in, and the Paris prosecutor's office has opened an [investigation](https://www.franceinfo.fr/internet/securite-sur-internet/cyberattaques/un-piratage-pas-si-sophistique-que-ca-ce-que-l-on-sait-de-la-cyberattaque-qui-a-fait-monter-en-temperature-les-frigos-de-supermarches_8235061.html?ref=ipinfo.io).

## France Stood Out, by Far

We scan [the IPv4 Internet](https://ipinfo.io/blog/comparing-ipv4-ipv6) (the most common type of Internet address) every week, connecting to the usual ports that websites use (the numbered doors a computer answers on) and recording what answers. In the sweep that ran on October 6 and 7, five days before the attack, 2,528 addresses showed a Danfoss StoreView dashboard. That is the web page that stores and technicians use to watch and manage these controllers, the equivalent of the control panel in the back room, except that anyone who typed in the address could open it.

According to our data, France had 1,331 of them (53%). That is more than the other 72 countries and territories combined (1,197). Poland came next with 179, then the United States (101), Portugal (92), and Belgium (85).

![](https://storage.ghost.io/c/b1/57/b157bf7f-0fe2-447f-a945-12d7951755b3/content/images/2026/10/france-fridge-chart.png)\
Danfoss dashboards open to the Internet, by country, October 6–7, 2026.

Why France? Our data can't say, but it rules a few things out. French dashboards ran the same mix of software versions as everywhere else, so they weren't patched worse. They also weren't unusual in how they connect. In France, about two-thirds connect through one national provider, much like the main telecom in Portugal, Germany, or Romania. Almost all (98%) are on telecom providers' networks rather than in data centers or clouds, which fits a controller plugged into the store's own Internet connection, with access opened for a technician or head office.

[CERT-FR's advice](https://cert.cyber.gouv.fr/alerte/CERTFR-2026-ALE-012/?ref=ipinfo.io) hints at why stores keep them online: it tells them to check their insurance terms before cutting the data their controllers send to refrigeration contractors.

## Most Fridge Software Was Out of Date

The dashboards also give away how old the software behind them is. They come in two generations. The older one is a page built with Flash (an old browser plug-in, retired in 2020) and served by the AK-SM 800. Danfoss [declared these controllers end of life in 2023](https://assets.danfoss.com/documents/latest/258303/AO453240723802en-010101.pdf?ref=ipinfo.io). That same year, the security firm Synacktiv [published a login bypass](https://www.synacktiv.com/sites/default/files/2023-10/synacktiv-danfoss-sm800-sc255-sc355-auth-bypass.pdf?ref=ipinfo.io) (a way in without the password) for them, and Danfoss's advice was to restrict network access. This is the "legacy" equipment Danfoss named.

The newer one runs on the AK-SM 800A, Danfoss's current model. Its dashboard shows when its software was built, a bit like the production date stamped on a carton, which tells us roughly which version it runs: 242 of them say May 19, 2023, the build date of firmware 3.3 (firmware is the software built into the controller). Controllers older than 3.3 miss [three 2023 fixes](https://www.danfoss.com/en/service-and-support/coordinated-vulnerability-disclosure/danfoss-security-advisories/?ref=ipinfo.io), including one for [a flaw rated critical](https://nvd.nist.gov/vuln/detail/CVE-2023-25915?ref=ipinfo.io). Those older than 4.2 also miss a fix for [a 2025 login bypass](https://www.cisa.gov/news-events/ics-advisories/icsa-25-140-03?ref=ipinfo.io), and those older than 4.3.1 miss [two more 2025 fixes](https://claroty.com/team82/research/freeze-the-controller-defrost-the-food-uncovering-vulnerabilities-in-danfoss-refrigeration-controllers?ref=ipinfo.io).

Put together, 85% of the French dashboards (1,136 of 1,331) ran software with known security flaws, almost the same as the 84% elsewhere. That is 527 older controllers Danfoss no longer supports, 280 missing the 2023 fixes, and 329 missing the 2025 fixes. Only 195 had every known fix.

![](https://storage.ghost.io/c/b1/57/b157bf7f-0fe2-447f-a945-12d7951755b3/content/images/2026/10/france-open-internet.png)\
How up to date the Danfoss dashboards were, France and the rest of the world.

## The Exposure Has Existed for Months

Every scan from late March 2026 until the attack found about the same number: between 2,464 and 2,622 Danfoss dashboards open to the Internet, roughly half of them in France.

They were mostly the same devices. Of the 1,331 French dashboards found on October 6, 1,019 were open at the very same address on March 30, six months earlier. Over those months, the number of older controllers Danfoss no longer supports didn't go down in France (519 at the end of March, 527 in October). Updates came slowly. The share of newer French controllers with every known fix went from 14% to 24%, under two points a month.

![](https://storage.ghost.io/c/b1/57/b157bf7f-0fe2-447f-a945-12d7951755b3/content/images/2026/10/open-since-march-timeline.png)\
Danfoss dashboards found in each weekly scan, March 30 to October 6, 2026.

## Did Stores Disconnect After the Attack?

Most stores did disconnect after the attack. Our next weekly scan ran on October 14, two days after the attack. Of the 1,331 French dashboards from October 6–7, only 316 were still online at the same address, a 76% drop. In the weekly scans from August to early October, only 3% to 7% disappeared from one week to the next. The older controllers Danfoss no longer supports fell the most, from 527 to 64. Fully updated ones fell too, from 195 to 54, which points to stores disconnecting them rather than updating them.

Outside France, 25% disappeared, more than the usual 9% to 12%. [Censys](https://censys.com/blog/frostbitten-in-france-a-danfoss-storeview-internet-exposure-report/?ref=ipinfo.io), another Internet scanning company that counts every day, saw a similar 71% drop in France between October 11 and 14. On October 14, the technical director of Claroty, the security company that found the 2025 flaws, [told Tom's Guide](https://www.tomsguide.fr/cyberattaque-des-frigos-de-supermarches-des-appareils-danfoss-exposes-sont-toujours-en-ligne/?ref=ipinfo.io) that exposed AK-SM 800 controllers were still online. Our October 14 scan still found 64 of them.

![](https://storage.ghost.io/c/b1/57/b157bf7f-0fe2-447f-a945-12d7951755b3/content/images/2026/10/still-open-timeline.png)\
Danfoss dashboards in France before and after the attack, by software.

## Where Else These Controllers Are Open

France wasn't the only country with these systems online. Danfoss dashboards with known flaws were open in 69 other countries and territories, led by Poland (157), the United States (85), and Portugal (77). In some countries most were the older controllers Danfoss no longer supports: 80% in Iceland, 75% in Australia, and 72% in Germany.

![](https://storage.ghost.io/c/b1/57/b157bf7f-0fe2-447f-a945-12d7951755b3/content/images/2026/10/open-fridge-map.png)\
Danfoss dashboards with known security flaws, by country.

Some reports suggest other European countries may have been hit. One analyst quoted by Australia's [ABC](https://www.abc.net.au/news/2026-10-13/french-supermarkets-hacked-causing-fires-and-food-loss/107258724?ref=ipinfo.io) said Spain, Portugal, Poland, Germany, and Belgium had also been affected. Our data can't say whether they were, but it does show that all five had out-of-date Danfoss dashboards open to the Internet five days before the attack.

## What About Dixell?

CERT-FR's updated alert also names XWEB supervisors from Dixell, Copeland's Italian brand of refrigeration controllers, so we counted those too. The same scan found 1,611 XWEB dashboards in 66 countries, but only 70 in France; Italy, where Dixell is based, had 467. Judging by when their software was built, only 180 of the 752 newer XWEB PRO units had [the February 2026 fixes](https://www.cisa.gov/news-events/ics-advisories/icsa-26-057-10?ref=ipinfo.io). The other 859 were older models that [haven't had a software update](https://webapps.copeland.com/Dixell/Pages/XWEB300D_500D_500_EVO?ref=ipinfo.io) from Copeland since 2018. After the attack, French operators unplugged their XWEB dashboards too: our October 14 scan found only 20 of the 70 still online, a 71% drop, when a normal week loses 2% to 9%. Elsewhere, including Italy, the number fell only at the usual weekly rate.

## How We Counted

The dashboards were identified with two fingerprints (patterns that identify a device) built by our data team. The newer interface is matched by its page title. The older one is matched by its web server plus the Flash loader page, which identifies the AK-SM 800. AK-SC 355 controllers, which Danfoss also named, may not be in our count. Sixteen devices that share the older web server but aren't Danfoss (network cards in backup power units, building controllers, thermostats) were removed, along with two addresses that answer on almost every port, a sign of a honeypot (a decoy set up to attract attackers).

An independent count by the security researcher Baptiste Robert, [posted on X](https://x.com/fs0c131y/status/2109693461800857775?ref=ipinfo.io), found 2,752 Danfoss systems online, 1,590 of them in France, a little above ours. Censys, which scans differently, found more: 1,825 of the newer dashboards in France on October 11, 61% of its worldwide total.

The numbers are a minimum. They cover IPv4 only and the ports we scan, and an address is not always one store. We aren't publishing any of the addresses. CERT-FR told us it had already identified the affected systems, and we're in contact with CERT-EU (the EU's cybersecurity team for its institutions) to help other European cybersecurity teams in any way we can.

## What Operators Can Do

[CERT-FR's advice](https://cert.cyber.gouv.fr/alerte/CERTFR-2026-ALE-012/?ref=ipinfo.io) is to take these controllers off the public Internet and, if a contractor needs remote access, to give it through a secure VPN (a private, encrypted connection). Letting in only the contractor's addresses is a stopgap, not a fix. A factory reset alone isn't enough either: an exposed controller that no longer gets updates will likely be compromised again. For Danfoss systems, CERT-FR also says to update the firmware, set the security level to Strict, and change the default passwords.

## What the Data Can't Say

We can't tell from the scans how the attackers got in or who they were, and we don't know which chains or stores were hit. Being able to open a dashboard from the Internet doesn't mean anyone can break in through it, and nobody has said whether any of the flaws above were used.

The data does show what things looked like before and after the attack. Five days before the attack, France had more of these dashboards open to anyone than the rest of the world combined, and most were behind on updates. After the attack, most of them went offline within days, and we'll keep checking the 316 that were still online on October 14.

This is one of IPinfo's data stories, where we use our own measurements to look at what happens on the Internet. If you spot something we got wrong, have data that adds to this, or have a story you'd like us to look into, write to us at emot@ipinfo.io.

> Data: IPinfo’s Internet scans of IPv4 addresses: 18 scans from March 30 to October 6–7, 2026, about one a week, and one on October 14, after the attack. Counts are addresses and a minimum.
