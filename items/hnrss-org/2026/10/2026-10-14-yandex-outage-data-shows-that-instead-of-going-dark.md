---
title: 'Yandex outage: data shows that instead of going dark, malicious bots moved'
link: https://blog.cloudflare.com/yandex-outage/
source: hnrss-org
published: 2026-10-14T20:29:13Z
updated: 2026-10-14T20:29:13Z
first_seen: 2026-10-15T00:11:15.830933315Z
authors:
- coloneltcb
content: extracted
html: 2026-10-14-yandex-outage-data-shows-that-instead-of-going-dark.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50094673
  num_comments: 60
  points: 192
---

Following reported strikes on Yandex data centers, claims circulated on the Internet that Russian bot farms and influence operations had been knocked offline. HTTP traffic charts showed steep drops, fueling speculation that the infrastructure damage had stopped these malicious actors. However, aggregate traffic metrics can easily mislead. There are two autonomous system numbers ([ASNs](https://www.cloudflare.com/learning/network-layer/what-is-an-autonomous-system/)) associated with Yandex: AS13238 is used by Yandex’s search engine, while AS200350 runs its cloud services.

Using Cloudflare Radar and Bot Management data, we analyzed how these two ASNs were affected — and while both saw drops in traffic, they saw differing impact.

- Yandex Search (AS13238)**:** The drop in traffic consisted almost entirely of verified search crawlers indexing web pages, images, and site resources.
- Yandex.Cloud (AS200350): There was a decline in malicious activities like vulnerability scanning and probing, but high-volume automation did not vanish — it simply migrated to other hosting providers.

While malicious traffic may originate from a particular provider, it does not necessarily tell us who is responsible for the traffic. Understanding why the strikes had little lasting effect on malicious traffic requires analyzing request behavior, rather than just overall traffic patterns.

## What happened: the effect of the strikes and pre-event baseline

Between October 7 and 11, reported Ukrainian drone strikes hit three Yandex facilities in Russia, progressively knocking data centers and network capacity offline. As seen on [Cloudflare Radar](https://radar.cloudflare.com/), this culminated in a near-total collapse of the Yandex network on October 11 when traffic plummeted to 99% below baseline for Yandex Search (AS13238) and 90% for Yandex.Cloud (AS200350). Despite a brief partial recovery, both networks remained disrupted through October 13, settling at 86-90% below normal baselines. (As of October 14 at 18:00 UTC, traffic is at 85% of normal.)

![](https://blog.cloudflare.com/_image?href=https%3A%2F%2Fblog.cloudflare.com%2F_emdash%2Fapi%2Fmedia%2Ffile%2F01M4Y1445Y2D8SEE4V03F4DW7Z.png&w=1430&h=810&f=webp&fit=cover&position=center)

A drop in overall HTTP traffic only confirms that the network went down. It does not reveal whether malicious or evasive bots were disrupted. Looking at the pre-event automation baseline, we can see that before the strikes, automated requests accounted for 36.1% [of all HTTP traffic observed globally](https://radar.cloudflare.com/traffic) across Cloudflare's network, and 35.8% [of traffic from Russian IP addresses](https://radar.cloudflare.com/traffic/ru#bot-vs-human).

The outage had different effects on automated activity on the two affected ASNs because they served different roles.

**Yandex Search ASN (AS13238)** is used for search index crawling. Cloudflare classified 99.5% of its requests as automated, and 93.6% of its bot requests as [Verified](https://developers.cloudflare.com/bots/concepts/bot/verified-bots/). Our analysis showed that traffic from this ASN indexed pages, rendered pages, and rendered images, which is consistent behavior for a search crawler. The Yandex Search ASN supplied about 7% of Russia-origin bot requests, and just 0.05% of the global total.

**Yandex.Cloud ASN (AS200350)** is a cloud-hosting network and carries a very different mix of automation. Approximately 95.0% of its scored requests were bots, but none of its observed bot requests were verified by Cloudflare. Yandex.Cloud’s bots looked more like software apps: nearly two-thirds of their requests sent data to websites, some identified themselves as JavaScript, Python, or Go programs, and a larger share went to software companies than Yandex’s search bots. The Yandex.Cloud ASN supplied about 2% of Russia-origin bot requests, and just 0.01% of the global total.

## Impact of the event on overall bot traffic

"Bot traffic" refers to automated requests to websites on Cloudflare's network, such as crawlers and scripts, not necessarily activity on social media platforms. As Yandex’s networks lost capacity, their bot requests fell too. That was expected: nearly all traffic from Yandex Search and Yandex.Cloud was automated. While we saw the obvious drop in total traffic, we did not see any evidence linking a global disruption in malicious traffic to the drop in traffic from Yandex.

Russia-origin bot requests fell roughly in proportion to the Yandex traffic that went offline, since most requests from those networks were automated.

|                                      |                                                                                                               |                                                                                                   |
| ------------------------------------ | ------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| Date UTC+3                           | Global bot requests (% of all HTTP requests)\*                                                                | Russia-origin bot requests (% of Russia-origin HTTP requests)                                     |
| October 1–6: pre-event baseline      | [39.4%](https://radar.cloudflare.com/traffic?dateStart=2026-10-01&dateEnd=2026-10-06#traffic-volume)         | [50.4%](https://radar.cloudflare.com/traffic/ru?dateStart=2026-10-01&dateEnd=2026-10-06)         |
| October 7–8: drone strike on Sasovo  | [39.2%](https://radar.cloudflare.com/traffic?dateStart=2026-10-07&dateEnd=2026-10-08#traffic-volume) (−0.2%) | [48%](https://radar.cloudflare.com/traffic/ru?dateStart=2026-10-07&dateEnd=2026-10-08) (-2.4%)   |
| October 9: drone strike on Kaluga    | [39.4%](https://radar.cloudflare.com/traffic?dateStart=2026-10-09&dateEnd=2026-10-09#traffic-volume) (+0%)   | [42.5%](https://radar.cloudflare.com/traffic/ru?dateStart=2026-10-09&dateEnd=2026-10-09) (-7.9%) |
| October 11: drone strike on Vladimir | [39.5%](https://radar.cloudflare.com/traffic?dateStart=2026-10-11&dateEnd=2026-10-11#traffic-volume) (+0.1%) | [42.2%](https://radar.cloudflare.com/traffic/ru?dateStart=2026-10-11&dateEnd=2026-10-11) (−8.2%) |
| October 12                           | [39.6%](https://radar.cloudflare.com/traffic?dateStart=2026-10-12&dateEnd=2026-10-12#traffic-volume) (+0.2%) | [43.6%](https://radar.cloudflare.com/traffic/ru?dateStart=2026-10-12&dateEnd=2026-10-12) (−6.8%) |
| October 13                           | [39.5%](https://radar.cloudflare.com/traffic?dateStart=2026-10-13&dateEnd=2026-10-13#traffic-volume) (+0.1%) | [42.1%](https://radar.cloudflare.com/traffic/ru?dateStart=2026-10-13&dateEnd=2026-10-13) (−8.3%) |

*\* Radar reports global traffic by both total HTTP requests, and by HTML requests (an approximation of a page view). The above table reflects total request volume.*

## Impact on Yandex Search ASN (AS13238): less search crawling

The decline on Yandex Search ASN was almost entirely search crawling. Sampled requests showed that 78.1% belonged to verified YandexBot, 11.5% to YandexRenderResourcesBot, and 1.9% to YandexImages.

Among websites and services, the industry segment that saw the steepest drop was Publishing and Newspapers, where traffic from Yandex networks fell 44%. Approximately 90% of pre-event traffic to news sites from this ASN was YandexBot indexing articles, with 40.5% of Cloudflare customers returning HTTP 403 Forbidden responses because the site owner blocked the crawler. When the Yandex Search ASN went offline, customers simply saw less indexing from Yandex.

## Impact on Yandex.Cloud ASN (AS200350): legitimate use and migrating malicious traffic

Yandex.Cloud displayed a completely different profile. Following the reported strikes, bot traffic from Yandex.Cloud fell about 40%. Several attack-like categories showed especially sharp declines. For example, WordPress plugin probing fell 90% and scanner-identified requests fell 93%. Interestingly (and tellingly), Cloudflare customer blocks on Yandex.Cloud traffic also fell 70% during the event period, suggesting that proportionately less malicious traffic was even hitting the Cloudflare WAF from this ASN.

### Impact on business verticals

Yandex.Cloud’s bot traffic is also interesting when segmented by top request destination industries:

|                         |                       |                                     |        |                                                            |
| ----------------------- | --------------------- | ----------------------------------- | ------ | ---------------------------------------------------------- |
| Destination industry    | Bot traffic pre-event | Bot traffic during event (Oct 7-13) | Change | 4xx (client error) HTTP response status share: pre → event |
| Computer software       | 24.24M                | 15.18M                              | −37.4% | 18.5% → 15.9%                                              |
| Gambling                | 23.01M                | 18.80M                              | −18.3% | 99.1% → 99.3%                                              |
| Media incl. adult       | 1.33M                 | 223.1K                              | −83.3% | 82.0% → 61.8%                                              |
| Internet and telecom    | 524.2K                | 206.5K                              | −60.6% | 71.4% → 51.1%                                              |
| Travel / tourism        | 363.9K                | 158.1K                              | −56.6% | 27.0% → 12.3%                                              |
| Retail / wholesale      | 316.0K                | 167.7K                              | −46.9% | 71.0% → 55.9%                                              |
| Gaming / computer games | 226.3K                | 63.2K                               | −72.1% | 51.4% → 9.2%                                               |
| Publishing / newspapers | 90.8K                 | 8.8K                                | −90.3% | 90.2% → 61.4%                                              |

The majority of traffic from the Yandex.Cloud ASN fell into two industry buckets: computer software and gambling. The computer software bucket was predictable for an ASN meant to host third-party applications and software integrations. In fact, we even saw that [api.cloudflare.com](http://api.cloudflare.com) is a top hostname for Yandex.Cloud request traffic. An overall majority of that traffic succeeded (requests returned 200s) both before and during the event, pointing to legitimate usage.

The gambling sector offers the clearest evidence that malicious automation relocated rather than disappeared. Nearly all of Yandex.Cloud’s bot requests were targeted at one gambling customer account, with traffic shifting among its hostnames. Immediately following the October 8 disruption, bot requests from Yandex.Cloud to this account dropped to zero. Simultaneously, identical requests began appearing from other Russian hosting ASNs, including Radist and Selectel. The incoming traffic maintained an identical fingerprint across providers:

- The same JA4 TLS client fingerprint
- The same uncommon request paths and HTTP headers
- The same custom User-Agent strings

Overall, Russian bot traffic targeting gambling accounts remained essentially flat over the week, moving from 167.7 million to 170.1 million requests (+1.4%). The automation never stopped running; the operators simply migrated their workloads to alternate cloud providers once Yandex.Cloud became unavailable.

Across most industries, the share of bot requests from Yandex.Cloud returning any HTTP 4xx response fell. This is largely due to a drop off in the attack-like traffic mentioned above. This, in combination with the gambling customer analysis, shows that malicious traffic that was sending requests through Yandex.Cloud simply shifted its “services” to other providers after the service disruption.

Finally, when we analyzed social media traffic, we found no unexpected changes in traffic either from Russian-origin IPs (0.6% decrease in total traffic over the event period) or globally (1.2% increase in total traffic over the event period). We repeated the analysis on bot traffic hitting the “post”, “reply”, and “like” endpoints for a specific social media platform and similarly did not find an unexpected shift before or during the event period from either Russian-origin IPs (5.7% decrease in total traffic over the event period) or globally (1.1% increase in total traffic over the event period). Based on our assessment, the data does not appear to support the claims that the strikes crippled social media manipulation, though without further analysis with data outside of Cloudflare, we cannot conclusively say there is no connection.

## What the data told us — and what it didn’t

When traffic from Yandex networks collapsed, speculation spread across the Internet that Russian influence operations and malicious botnets had suffered a crippling blow. Cloudflare data tells a different story: across Cloudflare's network, there was no evidence that the strikes knocked social media manipulation offline or otherwise disrupted malicious activity on the Internet. Instead, the data demonstrated:

- **Search indexing dropped.** The overwhelming majority of the traffic that dropped from Yandex Search ASN consisted of verified YandexBot crawlers fetching news articles and web assets. The main impact was reduced content discovery in Yandex Search.
- **Malicious automation migrated instead of stopping.** While vulnerability scanning and plugin probing temporarily dropped on Yandex.Cloud, high-volume operations quickly moved to alternative hosting networks.

A network disruption means that specific servers stopped sending requests, but it does not mean an automated threat disappeared. Cloud workloads are portable, so when a data center goes dark or an IP prefix gets blocked, operators can migrate their scripts to another provider in hours.

Our analysis here is a reminder that client ASN traffic volume is an unreliable proxy for interpreting automated bot behavior. Defending against automated threats requires inspecting request behavior, such as identifying shared JA4 fingerprints, monitoring anomalous request rates, and applying WAF rules to sensitive endpoints, regardless of which network the request originates from. Physical infrastructure is static, but malicious automation is built to move.
