---
title: OccupySaaS
link: https://occupysaas.com/
source: hnrss-org
published: 2026-11-02T00:17:27Z
updated: 2026-11-02T00:17:27Z
first_seen: 2026-11-02T06:43:52.954701233Z
authors:
- misswaterfairy
content: extracted
html: 2026-11-02-occupysaas.html
remote_preview:
  url: https://occupysaas.com/assets/img/og.jpg
---

Est. October 2026 · Apache-2.0

OccupySaaS is a protest movement written in code. Laid-off engineers, armed with AI and public documentation, are building **clean-room, open-source, drop-in replacements** for the platforms that rent our own industry back to us — one seat at a time.

![OccupySaaS logo: a raised fist holds a laptop showing a code symbol above a crowd of protesters with placards, in front of SaaS company skyscrapers. Below, OCCUPY in red brush letters and SAAS in black.](https://occupysaas.com/assets/img/logo.png)

We are the\
1,000,000

The grievances

The software built\
by the 99% belongs\
to the 99%.
---

In 2011, Occupy Wall Street asked who the economy was really working for. In 2026 we're asking the same question about the cloud. The tools every company runs on are now toll booths — and the people who built them were shown the door.

Per-seat pricing is a *tax* on growth

Every hire, every intern, every contractor — another line item. Infrastructure shouldn't charge rent by the head.

Lock-in is not a *feature*

Proprietary APIs, export walls and surprise price hikes keep customers captive. Compatible open alternatives set them free.

They cut us. We *commit*.

A million engineers were let go while the platforms kept growing. We still know how to ship. Now we ship it in the open.

The method

Clean room.\
AI rebuild.\
Drop-in.
---

We don't leak, decompile or copy anyone's code. Every project is rebuilt from public documentation and open standards, with AI doing the heavy lifting on the build side of a strict firewall — then proven compatible against the vendors' own public SDKs.

1. ### Read the public record

   Public API references, open standards (OAuth 2.0, OIDC, REST) and published SDK behaviour become a written specification. Nothing confidential, nothing leaked, nothing from former employers.

2. ### Raise the firewall

   Spec writers and builders stay separated. Each repo carries a `CLEAN_ROOM.md` policy, and provenance is recorded for every fixture and reference.

3. ### Rebuild with AI

   AI agents implement from the spec alone — contracts first (`openapi.yaml`), then code, then tests — under human review at every merge.

4. ### Prove compatibility

   A compatibility laboratory runs the vendors' own public client SDKs against our server. If your existing code doesn't work unchanged, it isn't done.

5. ### Release it to everyone

   Apache-2.0. Self-hostable. Pluggable storage. No telemetry you didn't ask for, and no seat counter.

#### Allowed in →

- Public API docs
- Open standards & RFCs
- Public SDK behaviour
- Black-box test results

Clean room

#### × Never allowed

- Leaked or proprietary source
- Decompiled binaries
- Former-employer confidential info
- Copied UI assets or trademarks

First releases

Two towers\
occupied.
---

Identity and CRM: the two subscriptions almost every company pays for and almost nobody loves. Both are early, both are public, and both welcome contributors today.

Early · in the open Go

### ZeroAuthy

Replaces

Auth0

A self-hostable identity server that speaks the Auth0 Authentication API. Point an existing Auth0 SDK at a ZeroAuthy tenant domain and it works with no code changes.

- OIDC discovery, JWKS & multi-tenant domains
- Hosted login, authorization code + PKCE
- `client_credentials`, rotating refresh tokens, revoke & logout
- Verified with auth0-spa-js, auth0-react & nextjs-auth0

```
# build & run
$ go build -o bin/zeroauthy ./cmd/zeroauthy
$ ./bin/zeroauthy serve --config config.yaml
$ curl https://localhost:8443/.well-known/openid-configuration
```

SQLite PostgreSQL MySQL gRPC plugins Apache-2.0

Early · in the open Go · Vue · TypeScript

### SalesCompel

Replaces

Salesforce

A free, open-source Salesforce replacement with a tenant-isolated Go/PostgreSQL core, a Vue record editor, and a compatibility laboratory that checks behaviour against Salesforce's public API contract.

- Persistent Account & Contact CRUD
- Object `describe` and limited queries
- External-ID upsert & bearer authentication
- OpenAPI 3.2 REST contract & offline test lab

```
# run the compatibility laboratory
$ npm ci --prefix compat/clients/node --ignore-scripts
$ npm --prefix compat/clients/node run check
```

PostgreSQL Multi-tenant REST i18n Apache-2.0

The occupation map

## Who's next?

Every category on this board is a monthly invoice somewhere. The movement picks the next target in public — bring a spec, a use case, or a grudge.

● Occupied

### Identity & Auth

ZeroAuthy — Auth0-compatible login, tokens and tenants.

● Occupied

### CRM

SalesCompel — Salesforce-compatible records and REST API.

○ Proposed

### Docs & Wikis

Team knowledge bases that don't lock your writing behind a seat licence.

○ Proposed

### Issue Tracking

Tickets, sprints and boards with a compatible API for existing integrations.

○ Proposed

### Feature Flags

SDK-compatible flag evaluation you can run next to your own services.

○ Proposed

### Observability

Logs, metrics and traces without per-gigabyte ransom notes.

○ Proposed

### Customer Support

Help desk, inbox and knowledge base, priced at zero agents.

[\+ Your pick](https://github.com/occupysaas/website/issues/new?title=Target%20proposal%3A%20&body=Which%20SaaS%20should%20we%20occupy%20next%2C%20and%20why%3F)

### Propose a target

Open an issue and make the case.

Proposed categories are open for discussion, not commitments. Names of existing products are used only to describe compatibility.

Lineage

From Zuccotti Park\
to git push.
---

[Occupy Wall Street](https://en.wikipedia.org/wiki/Occupy_Wall_Street) began on 17 September 2011 in Zuccotti Park, New York City, with a simple frame: the 1% and the 99%. It had no leaders, made decisions in open assemblies, and spread to hundreds of cities.

OccupySaaS borrows that spirit and swaps tents for terminals. No marches, no property damage — just working software, published under an open licence, that anyone can run.

> “We are the 99%.”

1. 17 Sep 2011 **Occupy Wall Street begins**

   Protesters set up camp in Zuccotti Park in Manhattan's Financial District.

2. Oct 2011 **The movement spreads**

   Occupy camps and general assemblies appear in cities around the world.

3. 15 Nov 2011 **Zuccotti Park is cleared**

   The camp ends; the language of the 1% and the 99% doesn't.

4. 2022 – 2025 **The great tech layoffs**

   Wave after wave of cuts across the industry while subscription prices keep climbing.

5. 14 Oct 2026 **ZeroAuthy goes public**

   The first clean-room rebuild opens its repo under Apache-2.0.

6. 15 Oct 2026 **SalesCompel joins the occupation**

   The second tower. More to come — decided in the open.

Join

## Occupy everything.

You don't need permission, a badge or a manager. You need a laptop and an afternoon.

### Write code

Pick an open issue, follow the clean-room policy, open a pull request.

### Self-host it

Run a release against your real SDKs and tell us exactly where it breaks.

### Write specs

Turn public docs into precise, testable specifications for the builders.

### Make noise

Star the repos, share the site, and tell your CFO what you're paying per seat.

Questions

## Fair questions.

Is this legal?

Clean-room reimplementation is a long-established way to build compatible software: you work only from public information and independently write your own code. Interoperating with an API is not the same as copying a product. Each project publishes its clean-room policy and source provenance so anyone can check the work. This page isn't legal advice; if you're adopting a project for a business, talk to your own counsel.

Are you affiliated with Auth0, Okta or Salesforce?

No. OccupySaaS and its projects are independent and not affiliated with, endorsed by, or sponsored by any vendor named on this site. Product names are used only to describe compatibility.

Can former employees of those companies contribute?

Yes, but never with confidential knowledge from a former employer. Contributors work only from public material, and anyone with relevant inside knowledge should stay on the public-spec side of the firewall or sit that project out. When in doubt, read the project's `CLEAN_ROOM.md` and ask first.

Why use AI to build it?

Because the giants had thousands of engineers and we have a movement. AI agents let a small group turn a precise spec into working code quickly. Humans write the specs, review every change, and own the result, and compatibility tests against real SDKs decide what counts as done.

Why Go?

Portability. A Go project compiles to a single self-contained binary, so self-hosting means dropping one file onto a server or into a Docker or Podman image — no installer, no runtime to set up, no tree of shared libraries to keep in sync. The same toolchain cross-compiles for Linux, macOS, Windows, x86 and ARM, and the result fits in a minimal container image. If you're leaving a SaaS vendor, the replacement should be this easy to carry.

Why not Rust?

Nothing is wrong with Rust, and we're not against it. Go was picked for the first projects because it makes the one-file, container-friendly drop-in the default with very little build setup. Rust can get there too; Go just gets there more simply for the kind of servers we're building.

Rust recreations of these projects are welcome — they just haven't been made yet. The specs, API contracts and compatibility tests are language-neutral, so a Rust port can be checked against the same SDKs. If you want to start one, [open an issue](https://github.com/occupysaas/website/issues/new?title=Rust%20port%3A%20) and say which project.

Is it production-ready?

Not yet. Both first projects are early and say so plainly in their READMEs, along with exactly which features work today. Try them, break them, and file issues.

What does it cost?

Nothing. Every project is released under the Apache License 2.0. Run it, fork it, sell services around it. The only thing we ask is that you contribute back what you can.
