---
title: Outlook.com Is Quietly Rejecting Email from Domains Without DMARC
link: https://mailroute.net/blog/outlook-rejecting-email-without-dmarc
source: hnrss-org
published: 2026-11-08T19:20:32Z
updated: 2026-11-08T19:20:32Z
first_seen: 2026-11-08T22:46:45.931966426Z
authors:
- xmddmx
content: extracted
html: 2026-11-08-outlook-com-is-quietly-rejecting-email-from-domains-without.html
remote_preview:
  url: https://mailroute.net/og-default.png
  alt: MailRoute — Cloud Email Security
---

Since November 5, 2026, Microsoft's consumer mail service has been turning away some of the email from domains that don't publish a DMARC record. If your company emails customers, vendors or applicants with outlook.com, hotmail.com, live.com or msn.com addresses, some of that mail may be bouncing right now. Nobody announced it. The fix is one DNS record.

**Key takeaways**

- Outlook.com, Hotmail, Live and MSN now refuse part of the mail from domains that have no DMARC record.
- It hits ordinary, low-volume senders, not only the bulk senders Microsoft's published rules cover.
- Only some messages bounce, so "my mail is getting through" doesn't mean your domain is safe.
- The fix is a single TXT record, `v=DMARC1; p=none`, which changes nothing about how other receivers treat your mail.

## What the bounce looks like

The sender gets a bounce like this one, followed by a bracketed server name and ID that differ per message:

```
550 5.7.1 Message rejected: Message lacks SPF/DKIM/DMARC Auth Records (AS9031519)
```

On the first day, the same rejection came back without the `(AS9031519)` code:

```
550 5.7.1 Message rejected: Message lacks SPF/DKIM/DMARC Auth Records
```

The wording suggests something is badly broken. Usually it isn't. The domains we have seen bounced mostly had working SPF, and many had DKIM too. What they were missing was the DMARC record that ties the two to the From address.

## What changed

Microsoft does have published sender rules. Since May 2025, domains that send [more than 5,000 messages a day](https://techcommunity.microsoft.com/blog/microsoftdefenderforoffice365blog/strengthening-email-ecosystem-outlook%E2%80%99s-new-requirements-for-high%E2%80%90volume-senders/4399730) to Outlook.com have had to pass SPF, DKIM and DMARC, or get `550 5.7.515 Access denied`. That rule is aimed at marketers and bulk senders, and most small businesses rightly assumed it didn't apply to them.

This new rejection is different in the ways that matter:

- **It reaches small senders.** A one-line reply from a small office can bounce.
- **It wasn't announced.** As of November 6, Microsoft has published nothing about it: no postmaster notice, no blog post, no service-health entry.
- **It doesn't say what failed.** A 5.7.515 bounce lists the SPF, DKIM and DMARC results. This one only says records are missing.

We first saw these bounces in our own outbound logs on November 5. Public reports from other administrators started appearing the same day.

## Why it's easy to miss

**Microsoft refuses only part of the mail** from an affected domain. One message to a Hotmail address goes through, the next one bounces, and a resend may succeed. That inconsistency is what makes this one dangerous: the person who sent the message sees a bounce full of acronyms, assumes a glitch, and moves on. The customer on the other end never knows a message was sent.

If your business depends on reaching people at personal addresses (patients, homeowners, applicants, parents, members), the cost is quiet. Invoices that don't arrive, appointment confirmations that never land, replies that look like you ignored someone.

## Is your domain affected?

Your domain is exposed if it has no DMARC record. You can check in a few seconds from any terminal. Replace `example.com` with your domain:

```
dig +short TXT _dmarc.example.com
```

A line starting with `"v=DMARC1` means you have a record. No output means you don't. That's the case this rejection targets.

## The fix is one DNS record

Add a TXT record at your DNS host:

- **Name:** `_dmarc`
- **Value:** `v=DMARC1; p=none`

**`p=none` is the safe starting point.** It declares that your domain uses DMARC and asks receivers to send you reports, but it tells them to take no action on failures. Nothing about how Gmail, Yahoo or anyone else treats your mail changes.

There is one condition. DMARC passes only when SPF or DKIM passes *for the domain in your From address*. If neither lines up, a DMARC record alone won't help. We covered how alignment works, and why it's where most setups go wrong, in [SPF, DKIM, and DMARC Are No Longer Optional](https://mailroute.net/blog/email-authentication-now-mandatory).

Step-by-step instructions, including the checks for all three records, are in our [guide to the Outlook.com 5.7.1 error](https://mailroute.net/docs/admin/outlook-message-lacks-spf-dkim-dmarc-auth-records).

## If your mail goes through MailRoute

Mail you send through the MailRoute outbound relay already passes both checks DMARC needs:

- **SPF.** Your mail keeps your own domain as its envelope sender and leaves from servers that [`include:spf.mailroute.net`](https://mailroute.net/docs/setup/implementing-spf-for-the-mailroute-outbound-smarthost-service) authorizes.
- **DKIM.** We [sign every outbound message as your domain](https://mailroute.net/docs/setup/implementing-dkim-for-the-mailroute-outbound-smarthost-service), using the keys your `mr01`, `mr02` and `mr03` CNAME records point to. Publish all three, so we can rotate keys without your signatures ever breaking.

For MailRoute senders, the DMARC record is the whole fix. Once it's in place, Outlook.com sees a domain that authenticates, and DMARC passes on either check. Our guide to [publishing DMARC for the MailRoute outbound relay](https://mailroute.net/docs/setup/implementing-dmarc-for-the-mailroute-outbound-smarthost-service) takes you from `p=none` to enforcement when you're ready.

## If your mail doesn't go through MailRoute

The DMARC record is still the fix, but the service that sends your mail has to pass SPF or DKIM for your domain. Microsoft 365, for example, signs with its own `onmicrosoft.com` domain until you turn on DKIM for your own domain, and that signature doesn't count toward DMARC. Our sister company mxio has a [guide to this error for any sender](https://mxio.io/learn/errors/outlook-lacks-spf-dkim-dmarc-auth-records-5-7-1), with a checker that tests your domain from the page.

## Where this is heading

This is the pattern every major mailbox provider is following. Requirements start with the bulk senders, get announced with a deadline, and then quietly become the baseline for everyone. DMARC at `p=none` is the floor now, not a best practice.

That's not a bad thing. Spoofing is how much of today's phishing and business email compromise starts, and a domain that authenticates is a domain that's much harder to impersonate. The `p=none` record that gets your mail delivered to Outlook.com today is also the first step to `p=reject`, which tells receivers to refuse mail that forges your domain.

If you'd like a relay that signs your outbound mail correctly from day one, our [setup documentation](https://mailroute.net/docs/setup/set-up-spf-dkim-and-dmarc) walks through SPF, DKIM and DMARC, and you can [start a trial](https://mailroute.net/trial-request) whenever you're ready. Add the record today. It takes a few minutes, and it's one less reason for your mail to disappear.
