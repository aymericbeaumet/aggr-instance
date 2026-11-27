---
title: 'Petsem: Sometimes Dead Is Better'
link: https://fly.io/blog/petsem/
source: fly-io
published: 2026-11-20T00:00:00Z
updated: 2026-11-21T04:03:54Z
first_seen: 2026-11-27T00:01:05.630507547Z
content: extracted
html: 2026-11-20-petsem-sometimes-dead-is-better.html
remote_preview:
  url: https://fly.io/blog/petsem/assets/figure-carrying-a-cat-through-a-graveyard-f95b.webp
  alt: A silhouetted figure carrying a limp cat across a hilltop graveyard at dusk
extra:
  thumbnail: https://fly.io/blog/petsem/assets/figure-carrying-a-cat-through-a-graveyard-f95b.webp
---

Author

![Matt Braun](https://fly.io/static/images/placeholder.webp)

Name

Matt Braun

![A silhouetted figure carrying a limp cat across a hilltop graveyard at dusk](https://fly.io/blog/petsem/assets/figure-carrying-a-cat-through-a-graveyard-f95b.webp)\
 Image by [Annie Ruygt](https://annieruygtillustration.com/)

It’s a hard time to be in infosec. Not that it was ever easy, mind you, but nowadays it seems particularly bad, what with the speed at which LLM-powered attackers can explore, iterate, and exploit their victims. The balance of power has tipped in favor of attackers, and while as defenders we may be on the back foot, we can take some steps to try and mitigate risks.

The risks, they are legion, but the first-and-foremost risk for any neocloud is an attacker hoovering up every cred and API key they can get their hands on. This isn’t a new risk, though it does have an interesting modern-day twist: it used to be that attackers were looking for keys to, say, payment systems or other crown jewels. Now keys for AI inference providers are of particular interest.

So, protecting keys: the best case is to not have a key to protect in the first place (think: ambient identity or API gateways). But, if that’s not an option, generally folks know not to hardcode keys in source or write them to config files (right?) and instead pull sensitive values from an external source and park them as environment variables. Of course, that shifts the responsibility elsewhere, but that’s a fair tradeoff: one centralized authority that hosts can pull secrets from vs. having to manage pushing the secrets to N running hosts. And so we have API-driven secrets managers like CyberArk, Delinea (née Thycotic) Secret Server, and HashiCorp’s Vault. Sure, attackers still can pull creds from a running or suspended system, that much is unchanged, but now operators can roll creds quickly, log access, and not have to deal with deployment headaches.

## Outgrowing Hashistack

We like Hashistack. It’s good, battle-hardened software. Fly.io was built on Consul for service discovery, Nomad for fleet orchestration, and Vault for secrets storage. But, as we scaled, we hit the edges of what those services could do for us and we had to start building out our own partial-or-full replacements: first Consul in 2022, then Nomad in 2023, and finally Vault in 2025.

A bit about how Vault works. Picture, if you will, a simple disk-backed key-value store running on a single node. Give the store an API with auth, split the single node into a cluster and make the cluster consistent and fault-tolerant, perhaps using a storage backend that uses Raft for consensus. That’s Vault, more or less. Park the cluster in a single region, since Raft needs low latency between the members for consensus and propagation, and now you can point your global fleet of readers at the cluster’s active read replica to pull down their values (i.e. secrets).

## Ten seconds on the boot path

But, alas, nothing comes for free, and in order for Raft to work well, the cluster nodes need to be “close” to one another. We were seeing a p99 of 10 seconds for apps to load secrets when booting, not to mention that app boot had a hard dependency on IAD being available (and never mind adding in latency for apps reading from the other side of the globe). You put a 10s+ secrets read phase and a single-region dependency on the boot path for apps and you’re talking a huge performance and reliability challenge at scale.

## Finding OK

We’re not running a bank here: in the context of secrets storage it’s fine-ish if a read returns an out-of-date value and we’re OK with giving up hard consistency guarantees to get back performance. We could deploy a multi-region Vault under an enterprise agreement backed by enterprise Consul, but that would run us roughly the amount of a fully loaded engineer’s salary per year. And that was the main impetus behind Petsem(atary), our home-grown Vault replacement (all the other cool crypt/coffin/vault/sepulcher names were taken). While there is still a write master in IAD, we now have read replicas scattered across regions and we dropped p99 from 10s to 300ms, solving both our availability and latency problems in one go.

## A use case for macaroons

Something else about Petsem is it’s tailored to us. Vault provided no easy way to guarantee, given a read request for app secrets from a worker, that the worker is hosting that app and has the rights to read those secrets. Given a fleet of thousands of worker servers, where only a handful of them are running instances of a particular app (roughly 80% of apps on Fly.io live on a single worker), only those few servers should be able to request the app’s secrets and pass them to the app.

Sure, Vault supports client policies to lock down secrets to particular clients, but the challenge of keeping that list accurate meant an orchestrator would need to update the list for every app placement or migration (and timing issues made this fragile as policy updates could arrive after Machines were placed on a worker). But this is a use case for [macaroons](https://fly.io/blog/operationalizing-macaroons/ ""): a worker requesting access to a secret can’t just present its credentials; it also needs a third-party discharge token from a separate service that says “this caller has been granted access to this particular app.” The worker can request the discharge token by presenting the app’s service token, which the worker receives from the orchestrator when the app is placed on the worker. Only if the worker’s credential AND the 3p discharge are valid does the read go through.

## Vault is not gone

We began the migration in March of 2024. The platform started writing new secrets to both stores, and shortly thereafter we backfilled existing secrets into Petsem. By May, we were running Petsem and Vault side by side (Vault as authority, Petsem as fallback). We let the system run for some time to season, and we flipped the final cutover switch in July 2025 to make Petsem the source for flyd secrets reads. But Vault is not gone: we still use it in various places in our infrastructure; it’s just no longer load-bearing for customer secret reads.

## A speed bump, not a roadblock

Let’s be clear here: Petsem is a speed bump, not a roadblock. If someone compromises a worker host, they have access to guest memory and can nab whatever’s in there. What Petsem does protect against is a smash-and-grab of all secrets at once. An attacker who compromises a worker is scoped down to only secrets corresponding to apps that have been deployed on that worker (because of the discharge check). They’ll have to engineer a lateral move to other workers or attract new deployments to the worker they’re on in order to grab more secrets, and denied reads are logged and trigger alerts in our observability pipeline.

## The secret your app never holds

Remember how I said it’s ideal to not have secrets in the app in the first place? If what you need is the output of an operation using a secret, as opposed to the secret itself (e.g. computing a signature over a hash), we built a nifty feature on top of Petsem: [Fly KMS](https://community.fly.io/t/fly-kms/21825 ""). You can write a secret to Petsem and it exposes encrypt, decrypt, sign, and verify operations that use that secret, authenticated using the same mechanisms as other Petsem read requests (except not supporting read operations). No Fly KMS secret leaves Petsem or lives in your app where it can be leaked or stolen. It’s one of our nifty-and-yet-not-widely-known features and more folks should check it out.

Next post ↑

[Holding the Door for Agents](https://fly.io/blog/holding-the-door-for-agents/)

Previous post ↓

[Throwing Out The Phonebook](https://fly.io/blog/throwing-out-the-phonebook/)
