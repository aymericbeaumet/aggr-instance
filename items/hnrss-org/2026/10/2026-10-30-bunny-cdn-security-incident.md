---
title: Bunny CDN Security Incident
link: https://status.bunny.net/incidents/p6gf1by9kr7m
source: hnrss-org
published: 2026-10-30T19:05:22Z
updated: 2026-10-30T19:05:22Z
first_seen: 2026-10-30T22:55:36.634700404Z
authors:
- SondreDag
content: extracted
html: 2026-10-30-bunny-cdn-security-incident.html
remote_preview:
  url: https://dka575ofm4ao0.cloudfront.net/assets/logos/favicon-2b86ed00cfa6258307d4a3d0c482fd733c7973f82de213143b24fc062c540367.png
---

We're reaching out to inform you of a security incident involving one of our infrastructure providers, the steps we've taken, and what this means for you.

\- What happened and how we responded

Last week, one of our infrastructure providers reported unauthorized access involving third-party server management software. One of our CDN nodes, located in Armenia, was hosted on a server supplied by this provider.

After receiving the alert from the affected provider, our engineering team took immediate action. Within 20 minutes, we stopped routing traffic through the server. We then reviewed and preserved the relevant security logs for investigation, securely wiped the server, and revoked its access to our other systems. The affected server has been permanently removed from our infrastructure.

Our investigation found no evidence of unauthorized access to the CDN node or any of its data. The infrastructure provider has also found no evidence that our server was accessed, but continues to investigate the incident.

\- What this means for your data

While neither investigation has found any indication of unauthorized access to our server, we cannot completely rule out the possibility.

If unauthorized access to our CDN node did occur, the potentially exposed information would have been: temporarily cached CDN content, Pull Zone configurations, and TLS certificates stored on that server.

As an additional precaution, we have revoked and reissued all TLS certificates for bunny.net platform domains (b-cdn.net and bunny.run). Reissuance of managed TLS certificates for custom domains is ongoing and is expected to be completed within the next 24 hours. No action is required from customers using managed certificates. We will post a further update on our status page once this process is complete.

If you manage your own TLS certificates, we recommend revoking and reissuing them with new private keys as a precaution.

No other customer data, including bunny.net account data, was stored on the server in question, and no internal systems were affected.

\- What happens next

We’re closely monitoring our infrastructure and working with the affected provider as the investigation continues. Should any new findings affect our customers, we will provide an update via email and on our status page.

At bunny.net, we understand the trust you place in us to keep your content secure, and we take that responsibility seriously. If you have any questions or need help with managing your TLS certificates, our support team will be happy to assist.

Posted Oct 28, 2026 - 11:40 UTC
