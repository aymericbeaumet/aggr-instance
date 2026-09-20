---
title: Pirate Face Rescues LLM Models from Deletion
link: https://pirateface.co/
source: hnrss-org-frontpage
published: 2026-09-20T15:16:07Z
updated: 2026-09-20T15:16:07Z
first_seen: 2026-09-20T17:28:39.170422635Z
authors:
- skepticalgenius
summary: 'Article URL: https://pirateface.co/ Comments URL: https://news.ycombinator.com/item?id=49776699 Points: 142 # Comments: 48'
content: extracted
html: 2026-09-20-pirate-face-rescues-llm-models-from-deletion.html
preview:
  file: 2026-09-20-pirate-face-rescues-llm-models-from-deletion.preview-79c620623e84.webp
  width: 256
  height: 134
  color: '#e4d9c1'
images:
- source: https://pirateface.co/og/home
  original:
    file: 2026-09-20-pirate-face-rescues-llm-models-from-deletion.image-581a5d1b3bc2.png
    width: 1200
    height: 630
  variants:
  - file: 2026-09-20-pirate-face-rescues-llm-models-from-deletion.image-7b36fb475ea7.webp
    width: 320
    height: 168
  - file: 2026-09-20-pirate-face-rescues-llm-models-from-deletion.image-828bc10e752c.webp
    width: 640
    height: 336
  - file: 2026-09-20-pirate-face-rescues-llm-models-from-deletion.image-17974b41cf0d.webp
    width: 960
    height: 504
  - file: 2026-09-20-pirate-face-rescues-llm-models-from-deletion.image-caef0e99792b.webp
    width: 1200
    height: 630
  color: '#f3ead8'
---

Pirate Face is **decentralized infrastructure** for sovereign AI. Every open model is mirrored from Hugging Face as a torrent, held **peer-to-peer** instead of by a single company.

[Why models should never die →](https://pirateface.co/mission)

Censorship-resistant

Every model is a torrent that also downloads straight from Hugging Face. The day it's gone, the swarm keeps it alive - there's no single host to shut down.

huggingface.co/model removed

fallback ↓

pirateface.co/model 1,240 seeding

Checksum-verified

Every file carries its official Hugging Face **SHA-256** - so you verify every byte. Download it anywhere and the hash still has to match: the real weights, never a tampered copy.

model-00001.safetensors 6.7 GB

SHA-256 9f86d081884c7d659a2feaa0c55ad015a3bf4f1b2b0b822cd15d6c15b0f00a08

✓ Matches Hugging Face - bit-for-bit, untampered

Drop-in API soon

Point your existing pipeline at Pirate Face - same paths, same API, just a different endpoint. Zero code changes.

pirateface - zsh

$export HF\_ENDPOINT\=https://pirateface.co

$python train.py

✓pulling meta-llama/Llama-4 from the swarm

## FAQ

[What is Pirate Face?](https://pirateface.co/#what-is-pirate-face)

A decentralized, peer-to-peer layer for sovereign AI. Open models from Hugging Face become checksum-verified torrents, held by a global swarm so they survive any single host taking them down. The goal is permanence for open-source AI.

[Do I need an account to use Pirate Face?](https://pirateface.co/#why-register)

No. You can browse, download, and seed without an account.

- **Claiming is optional.** A public claim reserves your Pirate Face handle and helps the network spread. Only your first eligible handle earns welcome points.
- **Hugging Face creator claims require verification.** To receive the badge for a current Hugging Face username or organization, you must verify the matching Hugging Face account. Until then, the handle is only reserved and does not prove identity.
- **Direct publishing is planned.** Accounts will eventually support publishing and managing models directly on Pirate Face, without first uploading them to Hugging Face. That is not live yet.

Today, submitting a model requires a Hugging Face account and the model must already be on Hugging Face.

Accounts also support upcoming community benefits. [See account benefits](https://pirateface.co/#account-benefits).

[Why create a Pirate Face account?](https://pirateface.co/#account-benefits)

An account gives you a place to manage your claimed handles and track your contributions. Claiming reserves a public handle; verifying the matching Hugging Face identity adds the creator badge and helps prevent impersonation.

Pirate Face is preparing community benefits for account holders, including **free compute credits** and **exclusive model releases**.

You can browse, download and seed without an account.

[Why use Hugging Face verification if the goal is independence from centralized hosting?](https://pirateface.co/#why-hf-verification)

Hugging Face verification confirms that someone claiming a creator’s handle controls the matching HF account or organization. It helps prevent impersonation and preserves attribution as models spread beyond their original host.

You don’t need verification, or an account, to browse, download or seed.

Pirate Face’s short-term goal is for model availability to outlive any single hosting platform. Today, parts of Pirate Face depend on Hugging Face and its community. Pirate Face is working to allow direct model publishing soon, without first uploading to HF.

[Does my Pirate Face handle have to match my Hugging Face username?](https://pirateface.co/#handle-verification)

No. You can reserve an available name, but the verified badge requires a matching Hugging Face identity.

For example, a Hugging Face account named **john123** can verify **john123**, not **john**.

You can still reserve **john**, but it stays unverified. The matching Hugging Face owner could reclaim that handle without deleting your Pirate Face account.

Manage your connected accounts and reserved handles in [Account](https://pirateface.co/account).

[Could someone impersonate a creator here?](https://pirateface.co/#creator-impersonation)

Both are protected:

- **The weights** can't be faked - every file is checksum-verified against Hugging Face's official SHA-256.
- **The handle** - anyone can reserve a name (it shows **reserved**, no ), but only proving the matching Hugging Face identity earns the . A squatter can't lock you out: the real owner reclaims a reserved name by verifying.

[What does “checksum-verified” mean?](https://pirateface.co/#checksum-verified)

Every weight file carries its official Hugging Face SHA-256 - a unique fingerprint of the exact bytes. Download it from anywhere and the hash still has to match, so you always know it's the real weights, never a tampered copy. It's the #1 fear with mirrored models, so it's a first-class feature here.

[How do downloads work, and what if Hugging Face removes a model?](https://pirateface.co/#model-downloads)

Every model is a magnet link (a torrent) with a Hugging Face web-seed built in - so it works even with zero peers.

- **While it's on Hugging Face:** you pull the bytes straight from HF - same weights, checksum-verified, same speed.
- **The day HF removes it:** the web-seed dies and the download falls back to the peer-to-peer swarm. We mark it **Rescued** - still reachable, kept alive by whoever's seeding.

That fallback is the whole point: the same bytes as downloading from Hugging Face directly, but with no single point of failure (and the swarm can serve far-from-HF regions faster).

[Can I add my own model?](https://pirateface.co/#add-a-model)

Yes - if you have a surviving copy, [submit a magnet](https://pirateface.co/submit) with source evidence. Opening a model page already records Hugging Face checksums when they exist. A listing is not a download, and listing does not start seeding.

- Sign in and submit a peer-only magnet, pinned revision, file checksums, and license evidence. MIT and Apache-2.0 only, plus the approved Kimi-K3 exception.
- Matching Hugging Face LFS hashes at the pinned revision lists immediately. Mismatches or a gone source stay in the review queue.

Pirate Face mirrors *from* Hugging Face, so the model has to live there first. If yours isn't on HF yet, [upload it there](https://huggingface.co/new) (free) and then submit it here.

[What's a “web-seed”?](https://pirateface.co/#web-seed)

The one bit of jargon worth knowing. A **web-seed** (BitTorrent spec BEP-19) is a plain HTTPS URL built into a torrent - here, the model's file on Hugging Face. It's really just **the download link**, carried inside the torrent as a guaranteed source - which is why models download even with zero peers, and why the swarm takes over the moment that link dies.

[What's the drop-in API?](https://pirateface.co/#drop-in-api)

Set HF\_ENDPOINT=https://pirateface.co and your existing pipeline resolves models through us - straight from Hugging Face while it's up, from the swarm the moment it isn't. Same paths, same API, zero code changes. soon

[What are points?](https://pirateface.co/#points-and-benefits)

Points recognise participation on the [leaderboard](https://pirateface.co/leaderboard). They are not money or a compute-credit balance.

- **Welcome points:** one qualifying handle per account. Extra reservations and linking HF do not earn another award. Connect your claim to an account to qualify.
- **Referrals:** 50 points for a new account's first handle, up to 10 credited referrals per referring account. Self-referrals and extra handles do not count.
- **Rescued models:** 25 points per distinct rescued model, credited to its first verified handle.
- **Seeding rewards:** planned, not active. We need attributable seeding evidence before awarding them.

Pirate Face is building an ecosystem of contributors, with planned community benefits including free compute credits and exclusive model releases. Eligibility, limits and launch dates will be announced before benefits become available. Points do not automatically convert to credits, and reserving more handles does not increase your benefits.

There is no token. Watch [our official X](https://x.com/thepirateface) for announcements.
