---
title: Taking Email Back
link: http://www.linux-magazine.com/Online/Features/Taking-Email-Back
source: lobste-rs
published: 2026-11-26T19:51:21Z
updated: 2026-11-26T19:51:21Z
first_seen: 2026-11-27T22:38:26.539383534Z
authors:
- linux-magazine.com via lim
labels:
- email
summary: Comments
content: extracted
html: 2026-11-26-taking-email-back.html
remote_preview:
  url: http://www.linux-magazine.com/design/linux_magazin_en/images/OGLogo.png
---

Author(s): Jeffrey A. McGuire

*The fourth in a five-part series on European-funded open source projects in hardware and software.*

#### Three Homes, No Data Center

Deuxfleurs, a French nonprofit, keeps its servers in members' homes in Lille, Nantes, and Massy: two or three second-hand Lenovo ThinkCentre mini PCs each, with a copy of the same data so its websites stay up when one of them loses power or fiber line. Quentin Dufour, one of the founders, says some major French websites already serve their pages from this setup. Next, the collective wants its email to work the same way.

The software for self-hosting email in this manner is called Aerogramme, and it isn't finished. Deuxfleurs still runs its own mail on Dovecot, a long-established open source mail server, and plans to move to Aerogramme around January 2027.

The collective is heading the opposite way from almost everyone else. According to the independent daily DNS measurements published by RIPE Labs, email for the million busiest domains on the web has consolidated dramatically: In 2016, 44.6 percent of these domains used small or self-hosted servers, and that share halved to 22.4 percent by July 2026. Small regional hosting companies and independents are losing accounts to corporate providers like Google and Microsoft, who together handled 38.6 percent of these busy domains.

Email is older than the Internet itself, and it still runs on open standards anyone can implement. Any server can deliver to any other, and any standards-compliant client can read any mailbox. It’s the hosting that has concentrated into a very few hands, eroding autonomy and giving hosting providers power over the data that travels through digital correspondence.

Two cases illustrate how this can limit operational independence. In May 2025, staff at the International Criminal Court (ICC) told the Associated Press that the court's chief prosecutor lost access to his Microsoft email account after he was named in US sanctions; Microsoft denies suspending services but hasn’t explained what happened to his account. In February 2026, the Dutch government told its parliament that the tax administration was moving about 40,000 workplaces to Microsoft 365 for lack of a short-term alternative, while acknowledging that Microsoft could be forced under state pressure to stop delivering services.

The Dutch government reversed course in October: The tax administration, customs, and the benefits agency will run mail and calendar on their own servers from 2027, with European open source tools for storage and collaboration to follow. The question now: How are developers working to decentralize email hosting on a wider scale? Each obstacle pushing organizations off of small or self-hosted email servers has open source projects working on a solution. Three of them show how the pieces fit; all of them supported by NGI Zero, the EU-funded programs that enable open source research and development.

#### How Email Got Away from Us

In the 1990s, running your own mail server was ordinary. Universities and small companies did it, and so did Mauro De Gennaro. In 1993, as a teenager, he fetched mail for users of his bulletin board over UUCP: the dial-up protocol Unix machines used to exchange mail. He has worked on mail software for most of the three decades since. When he looked at the open source stack again about six years ago, he saw "it's the same software that companies and servers have been using for over 20 years."

He counts 12 separate programs to run a conventional self-hosted setup. Each has its own configuration format and logs, written by different people in different styles. The software works well, he says, but was "designed and documented for other developers." Bundles such as Mailcow and Mail-in-a-Box install the whole set at once: “They abstract the complexity," De Gennaro says, "but they do not remove it." Since all of it was designed for a single machine, running it as a cluster server is possible in the same way it's possible to take a Fiat Cinquecento and turn it electric: "It was not designed for that." Google launched Gmail into that friction in 2004, with a gigabyte of free storage, and let companies run their own domain's mail on it. "Then they started billing," De Gennaro says. The users were captive, and moving email is disruptive enough that few of them left.

Ben Bucksch joined the Mozilla mail team in the late 1990s, where his first contribution made email links clickable; he also served in leadership for the Thunderbird Council and invented AutoConfig, which lets a mail client set up an account from nothing but an email address. In his telling, the lock-in started within the client.

IT departments think about email as servers; “what users want is to keep their workflow," Bucksch says. Microsoft shaped those workflows with Outlook's calendar and corporate features: Outlook works properly only with Microsoft Exchange, and Microsoft is pushing Exchange customers into its cloud. By some estimates, about 75 percent of professional mailboxes are on Office 365. "If we want to free the lock-in," he says, "we need to start with the client."

Dufour and Armaël Guéneau of Deuxfleurs locate the problem in operations. IMAP is the protocol most clients use to read mail. An IMAP server has to serve webmail that queries the server for every view, desktop clients such as Thunderbird that synchronize whole mailboxes, and phones that do some of each. Implementing it is hard; the reliability people expect is harder. When the tax office sends you an email, Dufour says, "it must work." The expertise to run mail at that level, Guéneau says, "tends to be captured by hyperscalers."

As Dufour tells it, Google didn't adapt existing mail software; it built Gmail on top of its distributed storage. Open source mail software was designed for one machine first, so adding distribution later, he says, requires "really huge trade-offs."

Reclaiming email is more complex than deciding to run your own server and navigating the software. Your service has to fluently speak with the large providers and support migration without disrupting service. And, of course, it needs sustainable funding.

Today, publicly funded teams are working on solving each of those three problems.

#### Rebuilding Server, Client, and Storage

Stalwart, the server from De Gennaro’s team, focuses on streamlining the software to replace 12 programs with one: Installation takes two commands and a six-screen web wizard. It speaks every common mail and collaboration protocol: SMTP for sending, IMAP and POP3 for reading, JMAP for web-based mail, and CalDAV and CardDAV for calendars and contacts. Spam filtering, sender authentication, and TLS encryption are built in. Give it a token for a DNS provider such as Cloudflare or Route 53, and the server publishes its own mail records. Behind the wizard are nearly 3,000 settings across 176 screens, which most operators never touch. De Gennaro calls Stalwart "the software at the other end of your email client or calendar client."

Stalwart 1.0 was released in October 2026 after five and a half years of development. NGI Zero funded the project with two grants: one for the mail server, and one for calendars, contacts, and file storage.

For users who can’t leave Outlook features behind, there’s Parula. Keeping the application logic strictly separate from a thin interface layer, and a well thought-out common logic, so a new protocol takes “just a few thousand lines of code,” Bucksch and his team at Beonex GmbH built an Outlook-like interface alongside a modern interface, a chat-style view and a classic three-pane view. It runs on a JMAP server or any other mail server and offers shared team mailboxes, delegated access so an assistant can run a manager’s calendar, and free/busy lookups for team meetings. It speaks IMAP, JMAP, and POP3, plus four from Microsoft: Exchange Web Services (EWS), Exchange ActiveSync, Microsoft Graph, and Outlook Web Access.

Partially funded by NGI Zero, Parula also removes a dependence most people never notice. Multi-factor login typically runs through Google's or Microsoft's identity services; an AutoConfig extension allows a client to discover and log in directly to an organization's own identity server. Bucksch says that Parula is the first desktop client that does this.

Aerogramme, an IMAP server that doesn't keep any state of its own, answers the operations problem in the storage. It doesn’t send or receive mail, so it needs a separate mail transfer agent such as Postfix; then it encrypts each mailbox on Garage, compatible with Amazon’s S3 interface. Garage replicates every piece of data in three locations, so users can still read and write their mail if a site goes offline. This design for power cuts and residential Internet connections gave up perfect synchronization, but it spreads maintenance work among five to ten members rather than requiring one big server host to give up all his weekends.

The research problem, Dufour says, is mapping the mail, calendar, and contact protocols cleanly onto object storage. Aerogramme is still on the way, with internal deployment planned for January 2027 and a public beta a few months later. To get there, a second grant funded through NGI Zero is supporting a hunt for edge cases where a network delay or a failure halfway through writing could lose a message.

The three projects already interoperate with each other and with other email programs on the NLnet list funded by NGI Zero. Parula lists Stalwart as a supported server; Stalwart can keep its messages in Garage. Bucksch and De Gennaro both name Mox as another server built as a single package. Different teams, working separately, have split the email problem and built solutions that fit back together.
