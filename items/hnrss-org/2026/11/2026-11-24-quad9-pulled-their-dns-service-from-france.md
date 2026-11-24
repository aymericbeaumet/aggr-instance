---
title: Quad9 pulled their DNS service from France
link: https://quad9.net/news/blog/quad9-service-deactivation-in-france/
source: hnrss-org
published: 2026-11-24T08:33:14Z
updated: 2026-11-24T08:33:14Z
first_seen: 2026-11-24T17:30:21.295672362Z
authors:
- farrokhi
content: extracted
html: 2026-11-24-quad9-pulled-their-dns-service-from-france.html
remote_preview:
  url: https://quad9.net/images/cover_c2f081e020.png
---

[Back to Blog](https://quad9.net/news/blog/)

 November 24, 2026 blog

## Quad9 Service Deactivation in France

![](https://quad9.net/news/blog/quad9-service-deactivation-in-france/french-announcement-image.png)

###### ([en français](https://quad9.net/fr/news/blog/quad9-service-deactivation-in-france/))

Today, November 24 2026, Quad9 will disable services for French users. This means any query reaching any Quad9 service address will be refused, and no DNS resolution will be successful. Our planned deactivation will be at 09:00 Central European (Paris) Time, to try to give users who are unaware a chance to respond quickly instead of having services down outside of normal business hours.

We are unhappy to have reached this impasse. For those of you seeing this for the first time, see [our announcement of this deactivation](https://quad9.net/news/blog/au-revoir-france/) from one month ago.

## The Summary

We have received demands from beIn (a sports rightsholder company) to block various domains they claim to be related to piracy streaming sites, so users in France cannot resolve those DNS names. Currently, the courts have upheld beIn’s demands, and we have faced fines for non-compliance. We are entirely opposed to piracy, but Quad9 has nothing to do with the piracy sites. This is an attempt to force us to be part of an enforcement regime against third parties.

We have a fundamental opposition to using recursive DNS providers as a tool in this or any other content blocking campaign. Putting demands on recursive resolvers has no effect on the origin servers that deliver objectionable content. Attacking recursive resolvers is a futile and [counter-productive effort](https://torrentfreak.com/inside-a-french-piracy-blocklist-expired-domains-a-seized-site-and-hong-kong-servers/), but that explanation is not understood. The policymakers and courts of France may of course make any determination they wish, even those that seem ineffective or damaging in our view. We wish to remain within legal requirements where it is necessary. We have our own strict views of what we do and what services we deliver. Because there is a mismatch between the case outcome and our mission, we must withdraw from France to remain in compliance with both law and our internal guidelines.

This is a terrible outcome, but it imposes the least amount of harm viewed over time longer than the duration of this case. We understand this will cause harm in the short term: users who do not know of our withdrawal will find our services unexpectedly unavailable. Users who have to move to different resolvers will not be protected by our cybersecurity capabilities, and they may choose to use resolvers that are less privacy-focused than Quad9.

## Repeating the Obvious

We object to the core premise of the legal arguments, which is the false assertion that recursive DNS can or should be used as a method to prevent piracy or any other type of content access. Our position is this is not effective or useful, as we have no monopoly on recursive DNS, and anyone can set up their own resolver in just a few seconds (though without the privacy and malicious content protection that Quad9 offers). The courts and plaintiff have not considered this argument and ignore it entirely in the judgement. Their interest is focused on the possibility for us to do what is demanded, even if it has results outside of France, and their interest is not in why that is unjust or unwise or ineffective. So, we will continue to argue with a refutation of our ability to do this accurately without applying French law to all other European nations and the world.

## How Our Withdrawal Will Work

There is an understandable focus on how Quad9 is going to block “French users” when we do not collect user information and have also claimed that geoIP blocking is insufficient for selectively denying resolution for certain names. We understand this seemingly apparent paradox, so we will expand on it as it is a question that is frequently repeated.

Our geoIP-based method to identify and deny service to French users is inexact. We are aware that our methods to geographically locate users and declare they are in France is quite imprecise - this is inevitable. By using these methods, we will create conditions of overblocking, where we accidentally classify some queries as being from France when they are not from France, and it will also result in conditions of underblocking, where we do not correctly identify some queries as being from France and inappropriately complete those requests even though the user is in French jurisdiction. This imprecision is a core component of our case: there is no legally accurate way for us to determine what queries are from users located in France versus what queries are from users not in France. But we have no other mechanism - it simply does not exist.

These geoIP lists are inaccurate in two dimensions: firstly, IP geolocation data are often imprecise or just plain wrong, as IP geolocation is not an exact science. Mobile operators span national boundaries, corporate WANs split networks across nations, IP addresses are bought and sold frequently, and network operators shift allocations across borders. These are just a few of many other examples of how geoIP association may not be accurate. Secondly, **IP location does not necessarily equate to user location,** geoIP data only is a guess as to the location of the IP address, not the location of the end user. VPNs may exit in France but have the majority of their users in other nations; corporate users in one nation may egress in another due to proxies or internal DNS service distribution - there is *nothing* about a query that has any indication of the jurisdiction of the origin of the query itself.

The data is imprecise, but we do not know to what level it is imprecise. So we must work with what we have.

## Why Not Block With This Method?

We could apply this same geoIP filter database to selectively block French users from reaching the sites we have been told to make unavailable, but this would corrupt our core premise which is that the DNS is not the right mechanism for this blocking to happen. If we comply within France to meet the requirement then we accept the worst eventual outcome for users - we lose our integrity and still do not have legally perfect coverage, plus we will inevitably apply French rulings across borders into other nations. We wish to first remain consistent with our core mission which is to only block cyber risks to end users and not content. Secondarily, we must remain as close to compliance as we can to the legal constraints put upon us.

By accepting even the smallest of the demands for blocking based on geoIP data, we would admit that such demands are permissible within our mission - that we accept to play a complicit role in aiding companies (or others) to create ineffective and mis-directed regimes of content constraint. We would be admitting that DNS blocking is a valid instrument for the piracy problem - *it is not*. We would be admitting we will do this for an ever-increasing number of names - *we will not*. We would be permitting these methods to be applied for more areas of content filtering such as moral, religious, social, or political reasons - *we must not*.

So we accept there will be egregious overblocking/underblocking errors in our blockade of French users but would rather remain steadfast to our stated mission and principles to uphold the integrity of the DNS. We would rather there be a clear distinction made for users of our services: you receive the DNS as you (the user) expect, or if Quad9 is forced to pollute the service with unexpected and arbitrary number of fragmentation demands, then we will not offer the services at all.

## Doesn’t Quad9 Already Block Names?

Quad9 blocks a set of cyber-threats on 9.9.9.9 (and secondary IPs) for end users, where the outcome would be an unexpected and harmful result to the end user (i.e., malicious software). Content blocking is explicitly not in that set. Quad9 has subjective editing on that set, as well - if we believe something is inappropriately categorized by our threat intelligence partners, then we can remove it at our discretion. Additionally, Quad9 also offers 9.9.9.10, which has no blocking of any kind - it is entirely open. The demands we have been given would enforce mandatory restrictions on *all our service* addresses, which then makes our statements of their functionality inconsistent with the reality that would be forced upon us. We will not accept that condition.

## The Future Prospects

We take a long range and principled view to the outcome of this case and the others in line. There are unmistakable long-term risks to Quad9, to user privacy, and to potential fragmentation of the internet that are far beyond the current moment. To give in now would start ‘the beginning of the end’ and allow motion towards internet fragmentation that cannot be undone. We accept the risks and the pain that come with fighting where others in similar circumstances to ourselves must bend and compromise in this invisible card game where end users are not even at the table. We represent those users, and our nonprofit mission of public service is aptly unique by not representing any shareholders, products, or other interests other than the end user.

It is especially disturbing to us to be doing this for an issue (piracy) which we believe is not slowed at all by any involvement of recursive DNS, though the damage to the integrity and trust of the global namespace is significant.

We have seen [recent articles](https://www.article19.org/resources/france-dns-blocking-proposal-poses-a-threat-to-free-expression/) about extended blocking demands given to ISPs in France of social media or instant messaging sites, which we anticipate would be demanded of us as well if continued. It is quite clear to us the path that is being taken with these efforts: first create the mechanism, then expand the volumes of the mechanism (unconstrained rightsholder editing of the DNS), then expand the scope of prohibitions to other groups and for other reasons until there is effectively no limit to the number of editors or the reasons for DNS names to be deleted from recursive resolution. This contradicts our mission of delivery of trustworthy DNS results, and we believe this potential outcome to be antithetical to a free and open internet.

This is the second legal assault we have faced to see if the DNS can be slowly corrupted - we stood our ground and eventually [won in Germany](https://quad9.net/news/blog/quad9-turns-the-sony-case-around-in-dresden/); with the continued community sponsorship and support we hope to win here, too. We refuse to take even the smallest step towards building the machinery of mandatory DNS restriction, and we will not abandon our obligation to the security, privacy, and integrity we have said we will deliver to users worldwide. We deeply regret having to step back from France and hope we will be able to return some day and offer the same unfiltered services that the rest of the world enjoys.

Legal battles like this drain time and resources away from building a better, safer internet. Your financial support is what keeps us in the fight—and keeps our services free and open for everyone. If you stand with us, please consider [donating](http://www.quad9.net/donate) or becoming a sponsor today.
