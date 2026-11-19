---
title: UK government forced to admit it issued Apple a secret encryption order
link: https://macanorak.com/uk-government-forced-to-admit-it-issued-apple-a-secret-encryption-order/
source: hnrss-org
published: 2026-11-19T16:21:03Z
updated: 2026-11-19T16:21:03Z
first_seen: 2026-11-19T23:46:32.431702950Z
authors:
- Terretta
labels:
- field notes
- storm warning
content: extracted
html: 2026-11-19-uk-government-forced-to-admit-it-issued-apple-a-secret.html
remote_preview:
  url: https://macanorak.com/content/images/2026/11/Apple.jpg
---

There's something pretty extraordinary about a government being able to demand changes to the security of devices used by millions of people, prevent a company that makes those devices from discussing those demands, and then refuse to even acknowledge that the demands exist at all.

That has been the sordid situation surrounding the UK governement's ongoing dispute with Apple over encrypted iCloud data. Up until today, the UK government's position has essentially been that it would neither confirm nor deny whether it issued Apple with a [Technical Capability Notice](https://fipr.org/20250721-AppleTCN.pdf?ref=macanorak.com) (TCN). These are secret orders issued under Section 253 of the [Investigatory Powers Act 2016](https://www.legislation.gov.uk/ukpga/2016/25/contents?ref=macanorak.com), which gives the UK government powers to require technology companies to make changes to their services to facilitate the exercise of investigatory powers. Today, the [Investigatory Powers Tribunal](https://www.gov.uk/government/organisations/investigatory-powers-tribunal?ref=macanorak.com) has ruled that the UK government *must acknowledge that it issued Apple with such a notice*.

This is a big deal. Whilst it doesn't mean we suddenly know everything the UK government wants Apple to do, the government's attempt to keep even the existence of the order secret has now suffered a major defeat.

I covered the background to this extensively in my piece, [*Two-Tier Encryption in the UK*](https://macanorak.com/two-tier-encryption-in-the-uk/). The story goes back to February 2025 when the [Washington Post](https://www.washingtonpost.com/technology/2025/02/07/apple-encryption-backdoor-uk/?ref=macanorak.com) exposed that the government had ordered Apple to provide ‘backdoor’ access to encrypted iCloud data, belonging not just to users in the UK, but around the world. The order was reportedly issued in January 2025 under the Investigatory Powers Act — the centrepiece of the UK’s modern surveillance law.

Rather than weaken its security provisions, Apple took the quite extraordinary step of withdrawing [Advanced Data Protection](https://support.apple.com/en-gb/102651?ref=macanorak.com) (ADP) from new UK users. ADP is an optional feature that extends end-to-end encryption to additional categories of iCloud data — when enabled, even Apple can't decrypt the protected information.[^1] Apple subsequently made clear that it would [not build a backdoor or a master key into any of its products or services](https://support.apple.com/en-us/122234?ref=macanorak.com). They stated this without publicly referencing the Technical Capability Notice, as they were forbidden from doing so.

The original order's scope extended beyond British users — it potentially implicated the encrypted data of Apple's customers all around the world, including the United States. Following months of diplomatic wrangling involving the Trump administration, the [UK government withdrew that original notice](https://www.cnn.com/2025/08/19/tech/uk-retreats-apple-back-door-vance-gabbard?ref=macanorak.com), but that wasn't the end of it. The UK government issued a [second Technical Capability Notice](https://www.eff.org/deeplinks/2025/10/uk-still-trying-backdoor-encryption-apple-users?ref=macanorak.com), this time reportedly restricted to British users. Apple's original legal challenge was subsequently dismissed following with the withdrawal of the first notice, so the company launched a [fresh legal challenge in 2026](https://9to5mac.com/2026/08/03/apple-launches-second-legal-challenge-to-uk-icloud-backdoor-order-per-report/?ref=macanorak.com).

The difficulty for the UK government has always been that the existence of the original order was already widely reported. The Washington Post had published its exposé, and many other news outlets subsequently reported on it. The UK government nevertheless maintained its position of neither confirming nor denying the existence of individual TCNs.

[At a hearing](https://www.telegraph.co.uk/business/2026/09/17/apple-demands-home-office-admit-to-secret-iphone-backdoor-o/??ref=macanorak.com) at the Investigatory Powers Tribunal on 17 September this year, lawyers representing civil rights organisations Liberty and Privacy International challenged that position. Ben Jaffey KC, representing the organisations, argued that the UK government's insistence on maintaining secrecy had become increasingly difficult to justify. Jaffey compared the secrecy order to the *“emperor’s new clothes… and brings the administration of justice into disrepute”*.

The existence of the order was already public knowledge and continuing to pretend otherwise was obstructing the proper consideration of the legal challenge. The UK government disagreed, arguing that abandoning its neither-confirm-nor-deny policy could damage national security by assisting hostile actors in building a picture of which companies were subject to such notices. Today the tribunal rejected that argument.

As [Reuters reports](https://www.reuters.com/business/retail-consumer/uk-tribunal-confirms-government-order-demanding-access-apple-data-2026-11-19/?utm_source=chatgpt.com), the Tribunal concluded:

> *“The respondent has not established that it would be damaging to national security to reveal in these proceedings, as is the case, that the respondent has given Apple a Technical Capability Notice.”*

The Tribunal hasn't ruled that the underlying order was unlawful; it hasn't made a determination whether the UK government can legitimately require Apple to make changes to its encryption systems; it hasn't disclosed the precise technical requirements of the notice. But the government has lost an important argument about keeping the proceedings secret.

In my Two-Tier Encryption in the UK piece, I used the idea of “Alice and Bill”, two fictional people living in the UK, both using identical iPhones and both paying for iCloud, to illustrate a fundamental problem that had arisen from the dispute between Apple and the UK government.

Alice had enabled Advanced Data Protection before Apple [withdrew the feature for new UK users](https://support.apple.com/en-us/122234?ref=macanorak.com), Bill hadn't. Alice continued to benefit from Apple's strongest available iCloud encryption protections, Bill couldn't even enable them. The result was a peculiar situation in which two customers using the same service had materially different levels of protection.

Because Apple withdrew ADP for new UK users in February 2025, existing users who had already enabled the feature have not since been required to turn it off — Apple’s [security documentation](https://support.apple.com/en-gb/guide/security/sec973254c5f/?ref=macanorak.com) says its servers can’t modify the ADP setting, or roll it back on a user’s behalf. This has created a ‘two-tier’ system of protection based on when somebody enabled a particular security feature.

Our iCloud accounts contain a lot of private information — photos, documents, backups, other material that most of us reasonably expect to remain fully private. Encryption exists so that the service provider shouldn't have the ability to access that information. If a government can compel a technology company to undermine its own protections, the consequences deserve considerably more scrutiny than secret exchanges between officials and lawyers. Today's ruling makes scrutiny more possible.

The Investigatory Powers Act doesn't only exist to regulate Apple's relationship with the UK government. Its provisions establish powers that can potentially be applied to other communications and technology providers. What happens when these powers are used against another company? What happens when the next notice concerns a different service or a different encryption system, or a different category of personal information? How would customers even know about it anyway? That's one of the most troubling aspects of the TCN regime - the *secrecy*. Companies receiving TCNs can be legally prohibited from disclosing that they have received them.

The UK government’s position, of course, is that these powers are necessary to support lawful investigations and protect public safety. [Liberty](https://www.libertyhumanrights.org.uk/issue/why-were-challenging-the-uk-governments-attempts-to-break-encryption/?ref=macanorak.com) and [Privacy International](https://privacyinternational.org/legal-action/pi-apple-tcn-challenge?ref=macanorak.com) have thankfully been challenging the lawfulness, necessity and secrecy of this regime. They argue it lacks adequate safeguards and threatens fundamental rights. Apple is pursuing its own legal challenge. These cases are now proceeding alongside one another with a substantive hearing expected sometime in 2027. Today's decision doesn't resolve the wider questions, but it does remove one obstacle to examining the dispute in public.

Apple has [consistently argued](https://www.cbsnews.com/amp/news/fbi-director-james-comey-apple-ceo-tim-cook-talk-privacy-concerns-on-60-minutes/?ref=macanorak.com) that it will not build ‘backdoors’ into its products. The UK government, meanwhile, has relied on powers that allow it to issue orders to technology companies in secret. For now, the legal process has at least established that the government issued this particular order, but my guess is that we're still some distance from fully understanding what it actually wants Apple to do, and whether it has the legal authority to demand it.

I will continue to follow this one very closely indeed.

[^1]: Advanced Data Protection is not synonymous with all iCloud data encryption. Apple already encrypts various categories of iCloud information without ADP, but some of those categories remain accessible to Apple and can potentially be provided in response to valid legal demands. ADP changes that arrangement by extending end-to-end encryption to additional categories of data.
