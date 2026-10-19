---
title: Let's Encrypt is adding two additional remote perspectives for domain validation
link: https://community.letsencrypt.org/t/lets-encrypt-is-adding-two-additional-remote-perspectives-for-domain-validation/252166
source: hnrss-org
published: 2026-10-19T10:53:39Z
updated: 2026-10-19T10:53:39Z
first_seen: 2026-10-19T19:14:52.283302801Z
authors:
- gslin
content: extracted
html: 2026-10-19-let-s-encrypt-is-adding-two-additional-remote-perspectives.html
remote_preview:
  url: https://global.discourse-cdn.com/letsencrypt/original/3X/4/c/4c5760509a18adfc3bc657ba8bd49b301faaa66b.png
---

October 15, 2026, 2:14pm 1

Hey everybody,

Let's Encrypt is adding two new remote perspectives for domain validation. This change is motivated by the fact that increased perspectives provide more domain validation security and is required by the [Baseline Requirements](https://github.com/cabforum/servercert/blob/3574ae809e5f909b58ee4c26b7bac4a3b9e78421/docs/BR.md#3229-multi-perspective-issuance-corroboration) which is quoted below. Increasing the number and coverage of our domain validation perspectives improves visibility and protection against BGP attacks.

> **BR 3.2.2.9 Multi Perspective Issuance Corroboration**\
>  Effective 2026-12-15, the CA MUST implement Multi-Perspective Issuance Corroboration using at least five (5) remote Network Perspectives. The CA MUST ensure that the requirements defined in Quorum Requirements Table are satisfied, and the remote Network Perspectives that corroborate the Primary Network Perspective fall within the service regions of at least two (2) distinct Regional Internet Registries. If the requirements are not satisfied, then the CA MUST NOT proceed with issuance of the Certificate.

During domain validation, this means that we will make seven total validation requests, one from the primary datacenter and six from remote perspectives [(previously four)](https://community.letsencrypt.org/t/lets-encrypt-is-adding-two-new-remote-perspectives-for-domain-validation/214123). For validation to succeed, the primary server and a quorum of remote perspectives must receive the correct challenge response. This makes it more difficult for attackers to hijack validation requests. To learn more about multi-perspective domain validation, please see [our earlier blog post](https://letsencrypt.org/2020/02/19/multi-perspective-validation.html) when Let's Encrypt first added new perspectives.

We expect little to no impact to users. Let's Encrypt will begin performing domain validation with the new regions in staging on **Tuesday, October 27th, 2026**. Assuming all goes well, we will do the production rollout on **Tuesday, November 10th, 2026**. Updates will be posted in this thread.

Happy issuance!

13 Likes
