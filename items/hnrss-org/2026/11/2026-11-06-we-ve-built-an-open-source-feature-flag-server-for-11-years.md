---
title: We've built an open-source feature flag server for 11 years
link: https://github.com/Unleash/unleash
source: hnrss-org
published: 2026-11-06T11:40:06Z
updated: 2026-11-06T11:40:06Z
first_seen: 2026-11-06T18:21:10.909146084Z
authors:
- ivarconr
content: extracted
html: 2026-11-06-we-ve-built-an-open-source-feature-flag-server-for-11-years.html
remote_preview:
  url: https://repository-images.githubusercontent.com/24594933/0ee96080-d406-11eb-856d-7df51ff2efbb
  alt: Open-source feature management platform. Contribute to Unleash/unleash development by creating an account on GitHub.
---

## What is Unleash?

Unleash is a powerful open-source solution for feature management. It streamlines your development workflow, accelerates software delivery, and empowers teams to control how and when they roll out new features to end users. With Unleash, you can deploy code to production in smaller, more manageable releases at your own pace.

Feature flags in Unleash let you test your code with real production data, reducing the risk of negatively impacting your users' experience. It also enables your team to work on multiple features simultaneously without the need for separate feature branches.

Unleash is the most popular open-source solution for feature flagging on GitHub. It supports 15 official client and server SDKs and over 15 community SDKs. You can even create your own SDK if you wish. Unleash is compatible with any language and framework.

## Get started with Unleash

### Set up Unleash

To get started with Unleash, you can either explore [Unleash Enterprise](https://github.com/Unleash/unleash#unleash-enterprise) with a free trial or [get started locally](https://github.com/Unleash/unleash#unleash-open-source) with our open-source solution.

#### Unleash Enterprise

To start with Unleash Enterprise, [request a free trial](https://www.getunleash.io/plans/enterprise-payg?utm_source=oss&utm_medium=readme&utm_content=unleash-enterprise-start). This gives you access to a hosted instance with unlimited projects and environments and features such as [role-based access control](https://docs.getunleash.io/concepts/rbac), [change requests](https://docs.getunleash.io/concepts/change-requests), [single sign-on](https://docs.getunleash.io/concepts/sso), and [SCIM](https://docs.getunleash.io/concepts/scim) for automatic user provisioning.

#### Unleash Open Source

To set up Unleash locally, you'll need [`git`](https://git-scm.com/) and [`docker`](https://www.docker.com/) installed on your machine.

Execute the following commands:

```shell
git clone git@github.com:Unleash/unleash.git
cd unleash
docker compose up -d
```

Then point your browser to `localhost:4242` and log in using:

- username: `admin`
- password: `unleash4all`

If you'd rather run the source code in this repo directly via Node.js, see the [step-by-step instructions to get up and running in the contributing guide](https://github.com/Unleash/unleash/blob/main/contributing/CONTRIBUTING.md#how-to-run-the-project).

### Connect your SDK

Find your preferred SDK in [our list of official SDKs](https://github.com/Unleash/unleash#unleash-sdks) and import it into your project. Follow the setup guides for your specific SDK.

If you use the docker compose file from the previous step, here's the configuration details you'll need to get going:

- For frontend SDKs, use:
  - URL: `http://localhost:4242/api/frontend/`
  - `clientKey`: `default:development.unleash-insecure-frontend-api-token`
- For backend SDKs, use:
  - Unleash API URL: `http://localhost:4242/api/`
  - API token: `default:development.unleash-insecure-api-token`

If you use a different setup, your configuration details will most likely also be different.

### Check a feature flag

Checking the state of a feature flag in your code is easy! The syntax will vary depending on your language, but all you need is a simple function call to check whether a flag is available. Here's how it might look in Java:

```java
if (unleash.isEnabled("AwesomeFeature")) {
  // do new, flashy thing
} else {
  // do old, boring stuff
}
```

## Try Unleash in the cloud

Want to explore Unleash without setting it up locally? Here are two easy ways to see it in action.

### Live demo

Open a running Unleash environment in your browser with no signup or setup required. This shared demo instance includes the core functionality so you can quickly get a feel for how Unleash works.

**→ [Launch the live demo](https://www.getunleash.io/interactive-demo?utm_source=readme&utm_medium=oss&utm_content=demo-section-cta)**

[![Launch the live demo](https://github.com/Unleash/unleash/raw/main/.github/github_online_demo.svg)](https://www.getunleash.io/interactive-demo?utm_source=readme&utm_medium=oss&utm_content=demo-section-cta)

### Free Enterprise trial

Get your own dedicated instance with a free 14-day cloud trial. This includes the full Unleash Enterprise feature set; ideal when evaluating Unleash for production use.

**→ [Start your free trial](https://www.getunleash.io/plans/enterprise-payg?utm_source=readme&utm_medium=oss&utm_content=demo-section-cta)**

## Deploying to a production server

To self-host Unleash in a production environment, you need to deploy it to a persistent server.

We provide guides for deploying to services like Heroku and DigitalOcean:

[![Deploy to Heroku](https://github.com/Unleash/unleash/raw/main/.github/deploy-heroku-20.png)](https://www.heroku.com/deploy/?template=https://github.com/Unleash/unleash) [![Deploy to DigitalOcean](https://github.com/Unleash/unleash/raw/main/.github/deploy-digital.svg)](https://cloud.digitalocean.com/apps/new?repo=https://github.com/Unleash/unleash/tree/main&refcode=0e1d75187044)

For more advanced configurations options, check out our documentation on:

- [Getting started with self-hosting](https://docs.getunleash.io/deploy/getting-started)
- [Unleash configuration options](https://docs.getunleash.io/deploy/configuring-unleash)

## Community and help

We know that learning a new tool can be hard and time-consuming. We have a growing community that loves to help out. Please don't hesitate to reach out for help.

[![Join Unleash on Slack](https://camo.githubusercontent.com/a01e9d30c69503f32b4b632430fc14ae76f5ddfac3cf9aaa668a3b092122c7db/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f736c61636b2d6a6f696e2d3633356463353f6c6f676f3d736c61636b)](https://slack.unleash.run)

💬 [Join Unleash on Slack](https://slack.unleash.run) if you want ask open questions about Unleash, feature toggling or discuss these topics in general.

💻 [Create a GitHub issue](https://github.com/Unleash/unleash/issues/new) if you have found a bug or have ideas on how to improve Unleash.

📚 [Visit the documentation](https://docs.getunleash.io/) for more in-depth descriptions, how-to guides, and more.

📖 Learn more about the principles of building and scaling [feature flag](https://docs.getunleash.io/guides/feature-flag-best-practices) solutions.

## Contribute to Unleash

Unleash is the largest [open-source feature flag solution](https://www.getunleash.io/) on GitHub. Building Unleash is a collaborative effort, and we owe a lot of gratitude to many smart and talented individuals. Building it together with the community ensures that we build a product that solves real problems for real people. We'd love to have your help too: Please feel free to open issues or provide pull requests.

Check out [the CONTRIBUTING.md file](https://github.com/Unleash/unleash/blob/main/contributing/CONTRIBUTING.md) for contribution guidelines and the [Unleash developer guide](https://docs.getunleash.io/contribute) for tips on environment setup, running the tests, and running Unleash from source. Before a pull request can be merged, contributors must read and agree to the [Unleash Contributor License Agreement](https://github.com/Unleash/unleash/blob/main/CLA.md).

### Contributors

[![The Unleash contributors](https://camo.githubusercontent.com/4bf33cbe02e58c562e1b5bf2e73308548a99b75a04199d9ddd7739e0051f48b5/68747470733a2f2f63646e2e676574756e6c656173682e696f2f646f63732d6173736574732f636f6e7472696275746f72732e737667)](https://github.com/Unleash/unleash/graphs/contributors)

## Documentation

You can visit the Unleash documentation at [https://docs.getunleash.io](https://docs.getunleash.io). As of 2 February 2026, the Unleash documentation source code lives in its own repository: [Unleash/unleash-documentation](https://github.com/Unleash/unleash-documentation).

## Features our users love

### Flexibility and adaptability

- Get an overview of all feature flags across all your environments, applications and services
- Targeted releases using [activation strategies](https://docs.getunleash.io/concepts/activation-strategies) to enable and disable features for certain users or segments without having to redeploy your application.
- [Canary releases / gradual rollouts](https://docs.getunleash.io/concepts/activation-strategies)
- [Kill switches](https://docs.getunleash.io/concepts/feature-flags#feature-flag-types)
- [A/B testing](https://docs.getunleash.io/guides/a-b-testing)
- 2 [environments](https://docs.getunleash.io/concepts/environments)
- Organize feature flags using [tags](https://docs.getunleash.io/concepts/feature-flags#tags)
- Out-of-the-box integrations with popular tools ([Slack](https://docs.getunleash.io/integrate/slack-app), [Microsoft Teams](https://docs.getunleash.io/integrate/teams), [Datadog](https://docs.getunleash.io/integrate/datadog)) + integrate with anything with [webhooks](https://docs.getunleash.io/integrate/webhook)
- [Insights for managing technical debt](https://docs.getunleash.io/concepts/technical-debt) and [stale flags](https://docs.getunleash.io/concepts/technical-debt#stale-and-potentially-stale-flags)
- API-first: *everything* can be automated. No exceptions.
- [12 official SDKs](https://docs.getunleash.io/sdks#official-sdks), and 10 [community-contributed SDKs](https://docs.getunleash.io/sdks#community-sdks)
- Run it via Docker with the [official Docker image](https://hub.docker.com/r/unleashorg/unleash-server) or as a pure Node.js application

### Security and performance

- Privacy by design (GDPR and Schrems II). End-user data never leaves your application.
- [Audit logs](https://docs.getunleash.io/concepts/events)
- Enforce [OWASP's secure headers](https://owasp.org/www-project-secure-headers/) via the strict HTTPS-only mode
- Flexible hosting options: host it on premise or in the cloud (*any* cloud)
- Scale with [Unleash Edge](https://docs.getunleash.io/unleash-edge) independently of the Unleash server to support any number of frontend clients without overloading your Unleash instance

### Looking for more features?

If you're looking for one of the following features, please take a look at our [Pro and Enterprise plans](https://www.getunleash.io/plans):

- [role-based access control (RBAC)](https://docs.getunleash.io/concepts/rbac)
- [single sign-on (SSO)](https://docs.getunleash.io/concepts/sso)
- more environments
- [feature flags project support](https://docs.getunleash.io/concepts/projects)
- [advanced segmentation](https://docs.getunleash.io/concepts/segments)
- [additional strategy constraints](https://docs.getunleash.io/concepts/activation-strategies#constraints)
- tighter security
- more hosting options (we can even host it for you!)

## Architecture

[![](https://github.com/Unleash/unleash/raw/main/unleash-architecture.png "Unleash System Overview")](https://github.com/Unleash/unleash/blob/main/unleash-architecture.png)

Read more in the [*system overview* section of the Unleash documentation](https://docs.getunleash.io/get-started/unleash-overview#system-overview).

## Unleash SDKs

To connect your application to Unleash you'll need to use a client SDK for your programming language.

**Official backend SDKs:**

- [Go SDK](https://docs.getunleash.io/sdks/go)
- [Java SDK](https://docs.getunleash.io/sdks/java)
- [Node.js SDK](https://docs.getunleash.io/sdks/node)
- [PHP SDK](https://docs.getunleash.io/sdks/php)
- [Python SDK](https://docs.getunleash.io/sdks/python)
- [Ruby SDK](https://docs.getunleash.io/sdks/ruby)
- [Rust SDK](https://github.com/unleash/unleash-client-rust)
- [.NET SDK](https://docs.getunleash.io/sdks/dotnet)

**Official frontend SDKs:**

- [Android SDK](https://docs.getunleash.io/sdks/android)
- [Flutter SDK](https://docs.getunleash.io/sdks/flutter)
- [iOS SDK](https://docs.getunleash.io/sdks/ios)
- [JavaScript SDK](https://docs.getunleash.io/sdks/javascript-browser)
- [React SDK](https://docs.getunleash.io/sdks/react)
- [Svelte SDK](https://docs.getunleash.io/sdks/svelte)
- [Vue SDK](https://docs.getunleash.io/sdks/vue)

**Community SDKs:**

If none of the official SDKs fit your need, there's also a number of [community-developed SDKs](https://docs.getunleash.io/sdks#community-sdks) where you might find an implementation for your preferred language (such as [Elixir](https://gitlab.com/afontaine/unleash_ex), [Dart](https://pub.dev/packages/unleash), [Clojure](https://github.com/AppsFlyer/unleash-client-clojure), and more).

## Users of Unleash

**Unleash is trusted by thousands of companies all over the world**.

**Proud Open-Source users:** (send us a message if you want to add your logo here)

[![The Unleash logo encircled by logos for Finn.no, nav (the Norwegian Labour and Welfare Administration), Budgets, Otovo, and Amedia. The encircling logos are all connected to the Unleash logo.](https://github.com/Unleash/unleash/raw/main/.github/github_unleash_users.svg)](https://github.com/Unleash/unleash/blob/main/.github/github_unleash_users.svg)

## Migration guides

Unleash has evolved significantly over the past few years, and we know how hard it can be to keep software up to date. If you're using the current major version, upgrading shouldn't be an issue. If you're on a previous major version, check out the [Unleash migration guide](https://docs.getunleash.io/deploy/upgrading-unleash)!

## Want to know more about Unleash?

### Videos and podcasts

- [The Unleash YouTube channel](https://www.youtube.com/channel/UCJjGVOc5QBbEje-r7nZEa4A)
- [*Feature toggles — Why and how to add to your software* — freeCodeCamp (YouTube)](https://www.youtube.com/watch?v=-yHZ9uLVSp4&t=0s)
- [*Feature Flags og Unleash med Fredrik Oseberg* — Utviklerpodden (podcast; Norwegian)](https://pod.space/utviklerpodden/feature-flags-og-unleash-med-fredrik-oseberg)

### Articles and more

- [The Unleash Blog](https://www.getunleash.io/blog)
- [*Designing the Rust Unleash API client* — Medium](https://medium.com/cognite/designing-the-rust-unleash-api-client-6809c95aa568)
- [*FeatureToggle* by Martin Fowler](http://martinfowler.com/bliki/FeatureToggle.html)
- [*Feature toggling transient errors in load tests* — nrkbeta](https://nrkbeta.no/2021/08/23/feature-toggling-transient-errors-in-load-tests/)
- [*An Interview with Ivar of Unleash* — Console](https://console.substack.com/p/console-42)
- [*Unleash your features gradually*](http://ivarconr.github.io/feature-toggles-presentation/sch-dev-lunch-2017/#1 ""), slideshow/presentation by Ivar, the creator of Unleash
