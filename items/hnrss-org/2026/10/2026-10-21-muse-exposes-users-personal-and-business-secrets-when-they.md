---
title: Muse exposes users' personal and business secrets when they share conversations
link: https://arstechnica.com/security/2026/10/meta-muse-exposes-users-personal-and-business-secrets-when-they-share-conversations/
source: hnrss-org
published: 2026-10-21T00:45:59Z
updated: 2026-10-21T00:45:59Z
first_seen: 2026-10-21T09:34:18.891023861Z
authors:
- gumby
content: extracted
html: 2026-10-21-muse-exposes-users-personal-and-business-secrets-when-they.html
remote_preview:
  url: https://cdn.arstechnica.net/wp-content/uploads/2026/10/ai-robot-hand-caution-1152x648.jpg
---

Muse, the new general-purpose AI agent from Meta, can expose sensitive information when users share conversations through web links, researchers reported Monday. In response to questions from Ars about the behavior, Meta said it’s suspending the sharing feature while the company reviews it and considers making changes.

Like many online services, Muse has allowed users to share searches, conversations, images, and similar content with other people. Such share features usually involve clicking an option to create a unique URL that anyone can use to load a page hosting the content. To keep potentially sensitive information from being accessible to unintended people, the URLs typically contain a long sequence of text that can’t be easily guessed.

## A single query is all that’s required

Muse, which Meta CEO Mark Zuckerberg [said last month](https://arstechnica.com/security/2026/09/muse-metas-extraordinarily-privileged-ai-assistant-has-a-serious-0-day/) was “built from the ground up for privacy and security,” seems to take the same precautions. But a central design decision for constructing URLs has made the links easy to discover. As a result, researchers from security firm Claroc [said](https://blog.claroc.io/ghostshares-muse-passive-dns/#_) they were able to enumerate 6,800 links Muse users created for sharing conversations with others. After opening a few hundred of them, the researchers found information that was clearly not intended to be publicly available, including:

- a candidate’s notes preparing for a confidential executive-search call with a frontier AI lab, naming the lab’s internal contact and the meeting time
- a national marketing company’s internal revenue review, marked for its CEO
- a small-business owner’s loan application, with the amount requested
- a to-do list that included a bank fraud alert, with a partial account number and a transaction amount and time
- a real-estate investor’s internal performance report, with two property addresses and exact return, income, and mortgage figures
- a workplace’s full-year staff vacation calendar, listing dozens of employees against the week each one would be away

“If you had asked us beforehand, we would have guessed that gathering these would take real effort,” the researchers wrote. “It took one query.”

In an emailed statement responding to questions from Ars, Meta said:

> We opened the Muse bug bounty program so researchers could raise different scenarios and help us continue improving the product. People have the ability to share specific Muse artifacts, which creates a page that can be accessed by those with the link. However, we’ve paused this functionality while we review this report and consider changes to how these links can be discovered.

Asaf Aprozper, the co-founder and CTO of Claroc, said he reported the findings through Meta’s bug bounty program on October 8 and also tried reaching people at Meta on LinkedIn, but got no reply either way.

The easiest way to discover the URLs is to perform a passive DNS search through one of many available commercial services. [Passive DNS records](https://www.spamhaus.com/resource-center/what-is-passive-dns-a-beginners-guide/) store the historical resolution of domain names to IP addresses, as well as nameserver, and mail-server records over time. Passive DNS systems typically collect DNS responses from sensors positioned at recursive DNS resolvers or other infrastructure. A passive DNS record is generally created when a monitoring system observes a DNS response. In the case here, once the intended party clicks on the share link, the DNS information is recorded in a searchable database.

Claroc researchers were able to discover the Muse share links because all of them use the domain name cf.metaaiusercontent.com. Discovery is further made easy by the decision to add the conversation name to the URL, along with a long string of text. Simplified examples of such links are home-sale-budget-9f21c8a3.cf.metaaiusercontent.com, q3-revenue-review-7b1e42f0.cf.metaaiusercontent.com, and loan-application-draft-a4c9e2.cf.metaaiusercontent.com. It’s this last design decision, putting the conversation title in the subdomain rather than in the URL path, that makes public discovery possible.

“Plenty of AI assistants share by public link,” Claroc said. “What makes Muse’s shares easy to find is the title sitting inside the web address, where public DNS records it for anyone to search. We call these GhostShares: pages that open for people who were never handed the link, and that those people can find on their own.”

Muse quickly catapulted to one of the top spots in Apple’s App Store. The assistant autonomously fills out forms, books appointments, makes purchases, creates documents, and carries out all kinds of other actions. To work as intended, Muse requires access to most of a user’s accounts. Meta has heavily marketed the assistant as a platform that’s worthy of the extraordinary trust it needs to work as intended.

Three weeks after the Muse launch, macOS security expert Patrick Wardle [discovered a zero-day vulnerability](https://arstechnica.com/security/2026/09/muse-metas-extraordinarily-privileged-ai-assistant-has-a-serious-0-day/) that allowed any other app installed on the same device to gain complete control of the user’s Muse account. Meta downplayed the severity of the vulnerability on the grounds that exploitation wasn’t possible unless a device was already compromised. The company didn’t acknowledge Wardle’s finding that a simple [ClickFix attack](https://arstechnica.com/security/2026/09/clickfix-attacks-infecting-pcs-and-macs-are-going-viral/) was all that was needed to hijack an account. The ease in discovering share links further justifies asking if Muse is worthy of the access it requires.

With Meta’s suspension of the feature, Muse users no longer face this particular threat, at least for now.
