---
title: Microsoft blames monster email outage on software defect introduced in July fix
link: https://www.theregister.com/saas/2026/11/16/microsoft-blames-monster-email-outage-on-software-defect-introduced-in-july-fix/5310026
source: hnrss-org
published: 2026-11-16T13:18:22Z
updated: 2026-11-16T13:18:22Z
first_seen: 2026-11-17T01:00:47.489879523Z
authors:
- Bender
labels:
- microsoft
- outage
- outlook
- saas
content: extracted
html: 2026-11-16-microsoft-blames-monster-email-outage-on-software-defect.html
remote_preview:
  url: https://image.theregister.com/254046.jpg?imageId=254046&x=0&y=0&cropw=100&croph=100&panox=0&panoy=0&panow=100&panoh=100&width=1200&height=683
---

REG AD

[saas](https://www.theregister.com/tag/saas)

Summer code change caused a Fall fail – frantic engineers had to patch the patch instead of rolling back

Microsoft has blamed last week's [epic outage](https://www.theregister.com/saas/2026/11/13/microsoft-services-restored-to-all-customers-company-says-its-deeply-sorry/5309609) on a change it made in July that lurked in the network gateway servers until a surge in load and a drop in capacity triggered the defect, giving the Microsoft 365 world a day off from email.

According to a preliminary post incident report [published](https://x.com/clamanna/status/2122209697226948729) by Microsoft on November 15, the defect was introduced in July 2026. The update looked fine at the time, Microsoft says, having passed load testing and been deployed without any impact.

However, the testing wasn't good enough. Microsoft's report states: "Our load testing tended to focus on weaker hardware with fewer cores." The defect caused "lock contention inside an internal Windows process which caused the machine's resources (CPU & memory) to be exhausted."

REG AD

"The contention issue is worsened on machines with more cores and newer hardware."

REG AD

The fix containing the defect was prepared specifically for Microsoft 365 network gateway servers. Then two things happened at once. Teams failed over to the "primary" gateway that supports Exchange Online, creating a surge in load that was not, in itself, particularly unusual. At the same time, almost 14 percent of server capacity for the networking gateway was out of service, which wasn't a problem by itself either: it was on its way to being restored and, according to Microsoft, "the service had sufficient capacity headroom." Together, they pushed the gateway into the defect and chaos ensued.

It was a classic cascading failure scenario. When a server loses connectivity, traffic gets routed to the next healthy endpoint. Which encountered the same problem. So traffic was rerouted again and… you can see where this was going. Within two hours, the defect was encountered across Microsoft's fleet. "At its lowest," the report states, "more than 80 percent of servers were unresponsive."

So, what to do? Bringing servers back to service didn't help – they were immediately overwhelmed and fell over again.

"Our normally automated recovery processes were not designed to restore service from this broad unresponsive state." A straightforward rollback was also not possible because the defect was months old.

Instead, engineers developed patch for the patch or, in Microsoft's words, "an update that returned the code path to the previously known good state in Windows." The patch was applied to groups of servers and service was gradually restored.

The timings show the pain customers experienced and the floundering in Redmond as Microsoft tried to work out why Exchange Online and other cloud services were so poorly. The Teams failover, which turned out to be a factor in the outage, happened at 0107 UTC on November 12. During the day, the load began to ramp up until, at 1603 UTC, Microsoft's connectivity anomaly detection monitor identified an increase in connectivity issues.

At 1633 UTC, the first support case came in.

It is not difficult to imagine the stress engineers felt as, over the next 15 hours, nothing they tried resolved the issue. All ongoing and planned deployments and configuration changes were paused while the team attempted to resurrect the service. Extra capacity was added. Traffic was rerouted. Yet servers kept falling over.

REG AD

"Those services that were able – like Teams, OneDrive, SharePoint, and Office apps – failed out of the faulty network gateway infrastructure," the report says. "But not all services, like Exchange Online, can fail out of this network gateway for scale reasons."

Hence the length of time for which Exchange Online (or, from the perspective of many users, Outlook) was out of action.

By 0030 UTC on November 13, Microsoft had identified connection negotiation as the main culprit for the access failures and concluded that competing processes were preventing connections from being handled.

However, it took until 0720 UTC before "We identified a mitigation and applied the mitigation –  an update that reverted the defective change."

At 1610 UTC, Microsoft confirmed that things were once again looking healthy and, at 0415 on November 14, issued its final customer update.

A final post incident report is due on November 20. However, the preliminary report will do little to calm jangling nerves. An update applied three months ago managed to cause Exchange Online (and other services) disruption on a global scale? With load testing that didn't reflect scenarios under Microsoft's control?

The engineers who worked feverishly to restore services deserve commendation and will hopefully never have to buy their own beverages in Redmond again (or at least escape the next round of firings). However, the processes that led to an update with a latent defect reaching production deserve a long, hard look. ®
