---
title: Stalwart is now a drop-in replacement for Microsoft Exchange
link: https://stalw.art/blog/microsoft-exchange-replacement/
source: hnrss-org
published: 2026-11-23T22:32:29Z
updated: 2026-11-23T22:32:29Z
first_seen: 2026-11-24T05:54:25.686228927Z
authors:
- shaunpud
content: extracted
html: 2026-11-23-stalwart-is-now-a-drop-in-replacement-for-microsoft-exchange.html
remote_preview:
  url: https://stalw.art/og/blog/microsoft-exchange-replacement.png
---

For more than twenty years, “the mail server” in most companies meant Microsoft Exchange, and “the mail client” meant Outlook. Today we are releasing Stalwart 1.2.0, and with it **Stalwart becomes a drop-in replacement for Microsoft Exchange**.

Stalwart now implements the protocols that Exchange clients speak: MAPI over HTTP, Exchange Web Services (EWS), Exchange ActiveSync and Autodiscover. Outlook, Apple Mail and the mail apps on phones and tablets connect to Stalwart as they would to an Exchange server, with mail, calendars, contacts, tasks and notes in one account. Users type their email address and password, Autodiscover does the rest, and they keep working in the client they already know.

Exchange support is part of the open source Community edition, under the AGPL.

## Exchange on premises in 2026

[Section titled “Exchange on premises in 2026”](https://stalw.art/blog/microsoft-exchange-replacement/#exchange-on-premises-in-2026)

Before going into what Stalwart now does, it is worth looking at where self-hosted Exchange stands today, because it explains why so many organisations are looking for a way out.

Microsoft has not announced the end of Exchange Server. On paper, on-premises Exchange still exists. In practice, every road Microsoft is building leads to Exchange Online:

- **Exchange 2016 and 2019 are out of support.** Both [reached end of support](https://learn.microsoft.com/en-us/exchange/plan-and-deploy/supportability-matrix) on 14 October 2025. Microsoft sold a paid Extended Security Update programme for a while, and according to [press reports](https://www.bleepingcomputer.com/news/microsoft/microsoft-exchange-2016-and-2019-esu-program-ends-in-october/amp/) of an Exchange team statement, it ended in October 2026 with no further extension. Servers still running those versions receive no fixes of any kind.
- **No new features.** [Exchange Server Subscription Edition](https://learn.microsoft.com/en-us/lifecycle/products/exchange-server-subscription-edition) (SE) is now the only supported on-premises Exchange, and there will be [no more major versions](https://techcommunity.microsoft.com/blog/exchange/top-3-myths-about-exchange-server-subscription-edition/4483187) after it. SE does not receive the features developed for the cloud. Even its first cumulative update has slipped from late 2025 to [an undetermined date](https://windowsforum.com/news/exchange-server-se-cu1-has-no-release-date-yet-megathread.442861/) in 2026.
- **The new Outlook does not support it.** Microsoft’s own [documentation for the new Outlook for Windows](https://learn.microsoft.com/en-us/microsoft-365-apps/outlook/get-started/supported-account-types) says it plainly: “On-premises Exchange accounts aren’t supported.”
- **The APIs are moving to the cloud.** New functionality ships only on Microsoft Graph, a cloud API. Exchange Web Services in Exchange Online [started being switched off](https://learn.microsoft.com/en-us/exchange/clients-and-mobile-in-exchange-online/deprecation-of-ews-exchange-online) in October 2026 and will be gone entirely in April 2027.

None of these steps forces anyone to move on its own. Together, they leave an Exchange administrator with a shrinking set of choices: keep running a server that will not grow, or move the organisation’s mail into Microsoft’s cloud.

For many organisations the second option is not acceptable. Email is the record of a company’s contracts, negotiations, legal advice, HR matters and customer data. Placing it with a US provider means placing it within reach of US law, including the CLOUD Act, which allows US authorities to compel American providers to hand over data they control wherever it is stored. Public administrations, healthcare providers, law firms, defence contractors and many European companies cannot accept that, or no longer want to. Data sovereignty means that you decide where your data lives, under which jurisdiction, and who can read it. A mail service in someone else’s cloud cannot offer that, and an on-premises server whose vendor is steering its customers toward that cloud offers less of it every year.

## Outlook without Exchange

[Section titled “Outlook without Exchange”](https://stalw.art/blog/microsoft-exchange-replacement/#outlook-without-exchange)

The obstacle to leaving Exchange has rarely been the server. It has been the client.

Many people have spent their whole working life in Outlook. They have years of muscle memory in it: their folder layout, their rules, their calendar views, their shared mailboxes, the way they book meeting rooms. Many organisations also have workflows built around it, such as delegated calendars managed by an assistant. Replacing Exchange with a standard IMAP and CalDAV server has always meant telling every one of those users to switch to a different mail client, or to reconfigure Outlook into a reduced IMAP mode where calendars and contacts no longer sync. For a migration project, that is often where the plan dies.

With Stalwart 1.2.0 the users do not need to change anything. The administrator moves the mailboxes and points the DNS records at Stalwart, and users connect with the same Outlook they used before.

### Supported protocols and clients

[Section titled “Supported protocols and clients”](https://stalw.art/blog/microsoft-exchange-replacement/#supported-protocols-and-clients)

Stalwart implements the following Microsoft protocols, all served on the same HTTPS port as JMAP and the WebUI:

- **MAPI over HTTP** (EMSMDB and NSPI), the native protocol of classic Outlook for Windows, in both cached and online mode. It covers mail, calendars, contacts, tasks and notes, incremental synchronisation, the global address list and the offline address book, delegate access, shared mailboxes, rules, junk email settings, reminders, search folders and read receipts.
- **Exchange Web Services**, schema versions Exchange 2010 SP2 to Exchange 2016: mail, calendars, contacts, tasks, folders, sharing and delegates, free/busy, inbox rules, out of office, name resolution, distribution lists, and streaming and pull notifications.
- **Exchange ActiveSync** 14.1 and 16.1: mail, calendars, contacts, tasks and notes, with push through Ping, meeting responses, search and out of office.
- **Autodiscover** in its POX, SOAP and JSON dialects, so clients configure themselves from an email address and a password.

That set of protocols is what the following clients use to talk to an Exchange server, and they can connect to Stalwart the same way:

- **Classic Outlook for Windows** (Outlook 2016 and later, including Microsoft 365 Apps), over MAPI over HTTP and EWS.
- **Outlook for Mac**, over EWS.
- **Apple Mail, Calendar and Contacts on macOS**, through an Exchange account over EWS.
- **Mail, Calendar and Contacts on iPhone and iPad**, through an Exchange account over ActiveSync.
- **Android mail apps** with Exchange account support, such as Gmail, Samsung Email and Nine, over ActiveSync.
- **Thunderbird**, with its built-in Exchange support for mail over EWS, or with TbSync for calendars, contacts and tasks over ActiveSync.
- **Evolution** (through evolution-ews) and **eM Client**, over EWS.
- Libraries and tools built on EWS or ActiveSync, such as exchangelib.

The Exchange protocols are not a separate silo inside Stalwart. They are another door into the same accounts. A message filed in Outlook is in the same mailbox an IMAP or JMAP client sees, a meeting accepted on a phone appears in the calendar that CalDAV clients read, and a contact added in Apple Contacts shows up in the CardDAV address book. A user can keep Outlook on the office desktop, the native mail app on a phone and a JMAP webmail in the browser, all on one account.

Rules created in Outlook’s **Rules and Alerts** are stored on the server as [mail rules](https://stalw.art/docs/sieve/mail-rules), which run as Sieve when the message is delivered. They apply when Outlook is closed and whatever client the user opens next. Delegates and folder permissions set in Outlook become regular Stalwart shares, so they apply to every protocol. Shared mailboxes open automatically in Outlook from group membership, as they do with Exchange.

Not every part of an Exchange deployment has a counterpart. Stalwart does not offer public folders, archive mailboxes, mobile device management, RPC over HTTP (Outlook 2010 and earlier) or NTLM and Kerberos sign-in: clients authenticate with a password, an [app password](https://stalw.art/docs/auth/authentication/app-password) or OAuth. The [Exchange documentation](https://stalw.art/docs/email/exchange/) covers configuration, DNS and Autodiscover, each family of clients, and the [differences from Exchange](https://stalw.art/docs/email/exchange/limitations) that administrators should know about before a migration.

One Outlook is missing from the list above: the new Outlook for Windows, the one that Windows now installs by default and that classic Outlook keeps offering through a toggle in its title bar.

The new Outlook does not work with Stalwart, and it does not work with any self-hosted server, Exchange included. It is built to work through Microsoft’s cloud. Microsoft [does not support](https://learn.microsoft.com/en-us/microsoft-365-apps/outlook/get-started/supported-account-types) on-premises Exchange accounts in it at all.

The new Outlook does accept IMAP accounts, and that might look like a way around the problem. It is not. When an IMAP account is added to the new Outlook, the connection to the mail server is not made from the user’s computer. As heise and c’t [reported](https://www.heise.de/en/news/Microsoft-lays-hands-on-login-data-Beware-of-the-new-Outlook-9608798.html) in November 2023, the new Outlook sends the IMAP server name, the user name and the password to Microsoft, and Microsoft’s cloud connects to the mail server on the user’s behalf. Every message then passes through Microsoft’s infrastructure on its way to the screen. Microsoft does not only see the mail; it holds the password to the mailbox.

For an organisation that runs its own mail server to keep its data under its own control, this defeats the purpose of the whole exercise. **We advise against using the new Outlook with any self-hosted email deployment**, whether it runs Stalwart or anything else. Users who see the “Try the new Outlook” toggle in classic Outlook should leave it off.

The same applies on phones. Outlook for iOS and Android reach on-premises mailboxes [through a Microsoft cloud service](https://learn.microsoft.com/en-us/exchange/clients/outlook-for-ios-and-android/use-hybrid-modern-auth) as well, which requires a hybrid Exchange Online setup. On mobile devices, the native mail, calendar and contacts apps, which connect directly over ActiveSync, are the better choice.

The obvious question is why Stalwart is adding Exchange protocols now. [EWS is being removed](https://learn.microsoft.com/en-us/exchange/clients-and-mobile-in-exchange-online/deprecation-of-ews-exchange-online) from Exchange Online, Microsoft’s own future for Outlook is a cloud client, and classic Outlook is [reportedly supported until 2029](https://www.directionsonmicrosoft.com/microsoft-to-support-classic-outlook-for-windows-for-some-until-at-least-2029/). Why build support for protocols that will be obsolete in a few years?

Because the companies that run Exchange today cannot wait a few years, and because those protocols are the bridge they need to cross.

An organisation with a few hundred Outlook users cannot replace its mail server and its mail client in the same weekend. It can replace the server, though, if nobody notices. With Stalwart speaking the Exchange protocols, the server moves first: mailboxes are migrated, DNS records change, and on Monday every user opens the same Outlook and the same phone apps they used on Friday. The organisation’s mail is back under its own control from day one, and no one’s work is interrupted to get there.

The client can then change at the pace that suits the users. Over time, as classic Outlook reaches the end of its life and the new Outlook remains tied to Microsoft’s cloud, we expect users to move gradually to third-party email clients built on open standards, and in particular on [JMAP](https://jmap.io). Outlook will stop being an option for anyone who cares about where their email lives, and by then the people who used it will have had the time to choose something else without a deadline over their heads.

Exchange support is not where Stalwart is going. It is how organisations get from where they are to where Stalwart is.

## Footprint and modularity

[Section titled “Footprint and modularity”](https://stalw.art/blog/microsoft-exchange-replacement/#footprint-and-modularity)

Exchange compatibility is not a small piece of work. It adds about 200,000 lines of code to Stalwart, and we know that some of you would rather not carry that weight if you never plan to see an Outlook client. We built it with that in mind, so that it costs as little as possible for those who do not use it, and so that it can be removed cleanly once the Exchange protocols are obsolete:

- **It is a separate module.** All of the Exchange code lives in its own crates, behind a single `exchange` Cargo feature. Build Stalwart without that feature and every line of Exchange code is left out of the binary. The [compilation guide](https://stalw.art/docs/development/compile) shows how. The day the Exchange protocols have no users left, removing them from the codebase is largely a matter of deleting those crates and that feature.
- **It is disabled by default.** Even in the official builds, which include the module, the Exchange protocols are off until an administrator [enables them](https://stalw.art/docs/email/exchange/configuration). While they are disabled, the server behaves as if they did not exist.
- **It stores nothing until it is used.** Outlook and the other Exchange clients keep properties that no standard protocol has a place for: extended properties, reminder state, client settings. Stalwart stores them as metadata attached to the item they belong to, using the [JMAP Object Metadata](https://datatracker.ietf.org/doc/draft-ietf-jmap-metadata/) extension, and only writes them when an Exchange client connects and sets them. Accounts that are never opened by an Exchange client carry no Exchange data at all.
- **Its data can be removed.** Because the Exchange-specific properties live in that metadata and not in the messages, events and contacts themselves, deleting the metadata removes every trace of them while the items stay intact for IMAP, JMAP, CalDAV and CardDAV clients.

## Migration from Exchange and Microsoft 365

[Section titled “Migration from Exchange and Microsoft 365”](https://stalw.art/blog/microsoft-exchange-replacement/#migration-from-exchange-and-microsoft-365)

A drop-in replacement is only useful if the data can follow. [Vandelay](https://stalw.art/docs/migration/import-export/), our account import and export tool, migrates mailboxes from both sides of Microsoft’s offering:

- **Exchange Server on premises**, through the [EWS importer](https://stalw.art/docs/migration/import-export/import/exchange-ews), which reads mail, calendars and contacts.
- **Microsoft 365 (Exchange Online)**, through the [Microsoft Graph importer](https://stalw.art/docs/migration/import-export/import/exchange-graph), which reads mail, calendars, contacts and OneDrive files. The EWS importer also works against Exchange Online, for as long as Microsoft keeps its EWS endpoint running.

Vandelay copies each account into Stalwart over JMAP, and the domain moves to Stalwart when its MX records and client settings change. The [migration guide for hosted services](https://stalw.art/docs/migration/guide/hosted) walks through the whole process, from the first test account to the cutover. With the Exchange protocols enabled on Stalwart, Outlook and the other Exchange clients then connect through Autodiscover and find the migrated mail, calendars and contacts.

## Other changes in 1.2.0

[Section titled “Other changes in 1.2.0”](https://stalw.art/blog/microsoft-exchange-replacement/#other-changes-in-120)

Exchange support is the headline, but 1.2.0 brings several other features, most of them developed alongside it because an Exchange replacement needs them:

- **Mail rules.** Server-side rules in the familiar “when a message matches these conditions, do this” form, managed in the WebUI, over JMAP or from Outlook. Conditions cover senders and recipients, subject and body, headers, size, attachments, importance, message type, mailing lists, spam score, date ranges and weekly schedules. Actions include moving and copying, marking as read or as junk, forwarding, replying with a template and rejecting. Rules are compiled into Sieve together with the vacation response. Each account also gets junk email lists: blocked senders, safe senders and safe recipients. See [Mail rules](https://stalw.art/docs/sieve/mail-rules).
- **Read receipts.** Message Disposition Notifications ([RFC 8098](https://www.rfc-editor.org/rfc/rfc8098)), including the JMAP methods for sending and parsing them ([RFC 9007](https://www.rfc-editor.org/rfc/rfc9007)).
- **Working hours.** Per-account working hours, managed in the WebUI, over JMAP or through CalDAV. Time outside working hours appears as unavailable in free/busy lookups, and a new setting lets users of the same organisation see each other’s free/busy time without sharing calendars explicitly. See [Availability](https://stalw.art/docs/collaboration/availability).
- **Rooms and equipment.** Resource accounts with a capacity, a location and delegates who manage the calendar, with invitations booked automatically or approved by hand. See [Resources](https://stalw.art/docs/collaboration/resources).
- **Native FoundationDB client.** A new asynchronous Rust client replaces Apple’s `libfdb_c` library. It needs no client library on the host and is now part of the standard build and Docker image. In our benchmarks it delivers between 4.5 and 6.9 times the throughput of the previous client on point reads, with 5 to 7 times lower median latency and about half the CPU.

The [changelog](https://github.com/stalwartlabs/stalwart/blob/main/CHANGELOG.md) lists every change and fix, along with the upgrade steps: existing servers need to add the new permissions to their default roles.

Last week we also released version 2.0.0 of `stalwart-cli`, and its biggest addition is an [interactive console](https://stalw.art/docs/management/cli/console). Run `stalwart-cli` with no arguments in a terminal and it opens a full-screen interface that covers the same ground as the WebUI: browsing and editing accounts, domains and every other configuration object, watching the dashboard, following live traces and running actions. It is aimed at administrators who live in SSH sessions, or who work on hosts where opening a browser is impractical.

The console is built with [Ratatui](https://ratatui.rs), the excellent Rust library for terminal user interfaces, and it is schema-driven like the rest of the CLI: its screens, forms and field help come from the schema that the server publishes, so it picks up new objects without being updated. The CLI also learned to sign in with the OAuth device flow and to remember the session, so a single approval in the browser is enough for both the console and every subsequent command.

Stalwart will be at [FOSDEM 2027](https://fosdem.org/) in Brussels. We will have a Stalwart stand, and on Saturday, 30 January 2027, we will give a talk about moving from Microsoft Exchange to Stalwart in the Modern Email devroom. If you are planning a migration away from Exchange, or are curious about what it takes, come and talk to us. We will post the time and title of the talk once the schedule is published.

Until then, the [Exchange documentation](https://stalw.art/docs/email/exchange/) has everything needed to try it, and we would love to hear how your Outlook clients get on with Stalwart.
