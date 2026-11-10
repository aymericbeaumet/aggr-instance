---
title: Microsoft are rejecting your mail
link: https://www.mythic-beasts.com/blog/2026/11/09/microsoft-are-rejecting-your-mail/
source: lobste-rs
published: 2026-11-09T18:36:47Z
updated: 2026-11-09T18:36:47Z
first_seen: 2026-11-10T11:40:21.505980627Z
authors:
- mythic-beasts.com via robalex
labels:
- email
summary: Comments
content: extracted
html: 2026-11-09-microsoft-are-rejecting-your-mail.html
remote_preview:
  url: https://www.mythic-beasts.com/static/img/beasts-256h.png
  alt: Mythic Beasts logo
---

November 9th, 2026 by [pdw](https://www.mythic-beasts.com/blog/author/pdw/)

On Friday morning, we became aware that a significant amount of email to Microsoft-run mail systems was being rejected. This included mail to hotmail.com and outlook.com.

The messages were being rejected with the following message:

`550 5.7.1 Message rejected: Message lacks SPF/DKIM/DMARC Auth Records`

We checked our logs, and found that we’d seen this message over 500 times on Thursday, but not once before that.

The affected messages all had [completely valid SPF and DKIM](https://www.mythic-beasts.com/support/hosting/mail/spf), but did not have a DMARC record. We added a DMARC record with a “none” policy to some of the affected domains, and sure enough, mail was then accepted again. A quick [fedi-survey](https://social.mythic-beasts.com/@beasts/117569497236204746) confirmed that we were not alone.

DMARC records are a way of publicly stating what you want recipients to do with email that claims to be from your domain, but which doesn’t pass verification checks (DKIM or SPF) and thus is potentially forged. Your DMARC policy can be one of the following:

- `reject` – recipients should reject mail that doesn’t pass DKIM or SPF. This is the real purpose of DMARC: if you’re confident that all legitimate outbound mail is properly authenticated, you can tell recipients to assume that anything else is forged, and bin it.
- `quarantine` – recipients should treat such mail as suspicious.
- `none` – your domain has no policy on what to do with mail that doesn’t pass DKIM or SPF.

Publishing a policy of `none` is effectively the same as not publishing a policy at all.

Microsoft are now rejecting mail without a DMARC policy, but accepting mail with a policy of `none`. This is a massive and pointless change in their policy for incoming mail, made with no notice whatsoever.

Last year Microsoft, introduced a policy that required domains *sending more than 5,000 messages per day* to have a DMARC record. This change was [announced in advance](https://techcommunity.microsoft.com/blog/microsoftdefenderforoffice365blog/strengthening-email-ecosystem-outlook%E2%80%99s-new-requirements-for-high%E2%80%90volume-senders/4399730). This latest change appears to be completely unannounced, which makes us wonder if it was even intentional. Of course, trying to contact Microsoft in order to resolve mail issues such as this is futile.

*Update: [apparently](https://www.theregister.com/applications/2026/11/09/outlookcom-bounces-legit-emails-as-microsofts-authentication-crackdown-goes-sideways/5308428) it wasn’t intentional, but they haven’t figured out how to roll it back yet.*

We’ve just applied `p=none` DMARC records to a large number of our customers’ domains. We generally try to avoid making unsolicited changes to DNS zones, but in this case we feel that it is the best option.

If you’re in contact with Microsoft users affected by this, please encourage them to complain to Microsoft; it’s very likely that they are losing other legitimate email from other providers because of this unannounced change. Better still, encourage them to move to [an email provider that is pro-active in investigating email deliverability issues](https://www.mythic-beasts.com).

Posted in [Email](https://www.mythic-beasts.com/blog/category/email/), [Services](https://www.mythic-beasts.com/blog/category/services/)
