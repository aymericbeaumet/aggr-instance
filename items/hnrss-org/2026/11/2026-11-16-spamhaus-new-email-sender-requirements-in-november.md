---
title: Spamhaus new email sender requirements in November
link: https://www.spamhaus.org/sender-guidelines/
source: hnrss-org
published: 2026-11-16T21:47:19Z
updated: 2026-11-16T21:47:19Z
first_seen: 2026-11-17T09:02:46.426369911Z
authors:
- computer23
content: extracted
html: 2026-11-16-spamhaus-new-email-sender-requirements-in-november.html
remote_preview:
  url: https://www.spamhaus.org/_nuxt/logo.tPE1gnpS.svg
  alt: The Spamhaus Project
---

Note

Starting in **November 2026**, we are gradually rolling out a requirement for every sending IP to have a valid HELO, a reverse DNS (PTR) record, and a matching A record. Use the HELO checker below to see whether the DNS meets our best practices, and follow the guidelines to avoid being listed.

Guidance for senders, mail admins, and postmasters on meeting Spamhaus's sending requirements. All senders must meet these minimum email sending requirements to avoid being listed.

1. Use confirmed opt-in (COI) for all subscribers

   Each recipient should confirm their address by clicking a link in a confirmation email. If no confirmation is received, do not send email to that address. Purchased, rented and harvested lists are the most common source of spam trap hits, and hitting traps is one of the fastest routes to a listing. [See section](https://www.spamhaus.org/sender-guidelines/#3-permission-and-list-hygiene--permission)

2. Publish forward-confirmed reverse DNS for every sending IP

   Every sending IP must have a reverse DNS entry pointing to a fully qualified domain name (FQDN). That hostname must, in turn, resolve back to the same sending IP address. This is known as forward-confirmed reverse DNS (FCrDNS). An IP with missing or mismatched reverse DNS is treated as suspicious and is one of the most common reasons for an IP to be considered for listing. [See section](https://www.spamhaus.org/sender-guidelines/#1-infrastructure-and-dns--reverse-dns-rdns)

3. Use a HELO/EHLO hostname that resolves to the sending IP

   The hostname the server announces in the HELO/EHLO command must resolve to the IP it is connecting from, and must stay consistent across connections from that IP. Placeholder values, hosting-provider defaults, and hostnames that vary between connections are treated as signals of misconfiguration or compromise and can result in a listing. [See section](https://www.spamhaus.org/sender-guidelines/#1-infrastructure-and-dns--helo-ehlo)

4. Meet the IPv6 allocation and reverse DNS minimum

   If sending over IPv6, the sending address must fall within an allocation of /64 or larger (a /64 is the standard block size we recommend assigning), and that space must have valid reverse DNS. IPv6 ranges without reverse DNS or with allocations smaller than /64 are often misused by spammers. [See section](https://www.spamhaus.org/sender-guidelines/#1-infrastructure-and-dns--ip-space-and-routing)

5. Authenticate mail with [SPF](https://www.spamhaus.org/glossary/#spf-sender-policy-framework), [DKIM](https://www.spamhaus.org/glossary/#dkim-domainkeys-identified-mail) and [DMARC](https://www.spamhaus.org/glossary/#dmarc-domain-based-message-authentication-reporting-and-conformance)

   Publish SPF and DMARC records for every sending domain, sign every outbound message with a signature, and align the DKIM sending domain with the domain in the message's [RFC 5322](https://www.rfc-editor.org/info/rfc5322/) From address. Authentication is one of the signals that shapes reputation, so gaps here make a listing more likely when other signals are weak. [See section](https://www.spamhaus.org/sender-guidelines/#2-authentication)

[Operating an Email Server Provider (ESP) or other shared sending infrastructure? **See the ESPs and Large Senders guidelines**](https://www.spamhaus.org/sender-guidelines/esp-large-senders/)

Check your HELO and DNS setup

Enter your sending IP to check its HELO and DNS setup against our guidelines. Results may not be available for all IPs and some queries might not provide results for internal policy reasons. You can also check the current HELO by sending an email to [helocheck@abuseat.org](mailto:helocheck@abuseat.org).

## 1\. Infrastructure and DNS

All sending infrastructure must have the following DNS and mailserver configuration. Failure to do so can result in IPs being listed on the Email Spam IPs (CSS) or compromised IPs (XBL) zones.

How to check

Run `dig TXT <envelope sender domain>` and confirm an SPF record is returned and includes the sending IPs.

* * *

## 2\. Authentication

Authentication is how receiving mail servers verify that emails are authorized to send from the domain. Without SPF, DKIM, and DMARC properly configured, the domain is exposed to spoofing and phishing, and emails are more likely to be rejected or flagged by Spamhaus.

How to check

Run `dig TXT <selector>._domainkey.<domain>` (the selector is set in the ESP's DKIM setup page) and confirm a DKIM key, starting `v=DKIM1`, is returned.

* * *

## 3\. Permission and List Hygiene

Send email only to people who want to receive it. Once the technical infrastructure is in place, the next step is building a legitimate list of email addresses from people who have provided explicit permission to receive the email. Lack of permission is one of the most common reasons for being listed by Spamhaus.

* * *

## 4\. Sending best practices

How email is sent matters as much as what is sent. Warming the IP and domain, maintaining a consistent volume, using trusted links and domains within emails, and providing clear, legitimate ways to opt-out of emails will help improve overall deliverability.

* * *

## 5\. IP and Domain Reputation

The reputation of the [IP](https://www.spamhaus.org/ip-reputation/) and [domain](https://www.spamhaus.org/domain-reputation/) as seen by Spamhaus can be checked via our [reputation checker](https://check.spamhaus.org).

Spamhaus looks at multiple signals before assigning a reputation score, and a strong negative signal can outweigh the absence of positive ones — so following all email best practices consistently is essential.

Factors that influence reputation include: the service provider, the IP neighbourhood, the infrastructure the IPs are associated with, the upstream networks that connect to internet backbones, and the IP or domain's history — when it was first seen, when it was last used, and how it was used.

How to check

Run the domain through the [Reputation Checker](https://www.spamhaus.org/domain-reputation/) to get more insights.

Note: We do not accept requests for explanations of the attributes shown here or requests to reset reputation.

* * *

## 6\. ESPs & Large Senders

ESPs and shared infrastructure hosts are responsible for the email sent via their infrastructure. Hosting abusive senders will negatively impact the reputation of shared domains and infrastructure.

[Read the ESP & Large Sender guidelines](https://www.spamhaus.org/sender-guidelines/esp-large-senders/)

* * *

## Spamhaus Tools & Resources

[DNS Tools Check your HELO and DNS setup](https://www.spamhaus.org/sender-guidelines/#dns-checker)\
[IP Reputation Check if an IP is listed](https://www.spamhaus.org/ip-reputation/)\
[Domain Reputation Check if a domain is listed](https://www.spamhaus.org/domain-reputation/)
