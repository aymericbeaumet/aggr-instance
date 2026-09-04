---
title: Automating .NET Framework → .NET Core with Devin
link: https://cognition.com/blog/dotnet-migration-with-devin
source: cognition-com-blog
published: 2025-10-28T00:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2025-10-28-automating-net-framework-net-core-with-devin.html
---

Engineering teams dread migrating from .NET Framework to .NET Core. However, autonomous coding agents are rapidly changing the previously known timelines. What once took months, teams are now finishing in as little as [two weeks](https://devin.ai/customers/thecitationgroup) — with Devin.

## Overview

Every engineering team has a migration story — late nights, brittle builds, endless dependency errors. Moving from .NET Framework to .NET Core is one of the most notorious of them all. But autonomous coding agents are rewriting the story entirely.

We’ve adapted [Jimmy Bogard’s canonical migration guide](https://www.jimmybogard.com/tales-from-the-net-migration-trenches/) — originally written for humans — into one for agents. The sections below detail how Devin automates each stage of a .NET Strangler Fig migration, step by step.

For clarity, we’ve simplified the example prompts and Playbooks; production usage should reference the Devin documentation on [prompting](https://docs.devin.ai/essential-guidelines/instructing-devin-effectively) and [Playbooks](https://docs.devin.ai/product-guides/creating-playbooks) for robust patterns.

## Accelerating with Devin

![Automating .NET Framework → .NET Core with Devin](https://cdn.sanity.io/images/2mc9cv2v/production/19653f8ef97357448e0553df5b195d003d189432-1966x1132.png?w=1600&fit=max)

The Strangler Fig migration relies on four things: visibility, planning, execution, and validation. Devin makes this possible through four core capabilities:

**[DeepWiki](https://docs.devin.ai/work-with-devin/deepwiki)*:*** Devin uses DeepWiki to generate always-up-to-date documentation and architecture diagrams, giving engineers instant visibility into controllers, dependencies, and framework-specific components.

**[AskDevin](https://docs.devin.ai/work-with-devin/ask-devin)*:*** Devin queries its own knowledge through Ask Devin to produce context-rich migration plans, ranking controllers by complexity and mapping framework-specific dependencies automatically.

**Local Machine:** Devin executes migrations inside its own fully configured environment, providing a production-like runtime that’s completely isolated from developer machines.

**Testing Setup:** Devin validates every pull request in real time with built-in test harnesses, ensuring functional parity and stability at each stage of the Strangler Fig migration.

![Automating .NET Framework → .NET Core with Devin](https://cdn.sanity.io/images/2mc9cv2v/production/5fcdcc84f1ad4b5fbb29573b64df640a84d3befd-1012x646.png?w=1600&fit=max)

## Planning

*In Bogard's Guide original guide: [Planning](https://www.jimmybogard.com/tales-from-the-net-migration-trenches-catalog/)*

The first step in the migration is to take inventory of all controllers and their actions, then estimate the complexity of moving each one to .NET Core by considering two factors: the number of action methods and the number of dependencies in each controller. This helps prioritize the simplest cases first.

A single Ask Devin prompt completes this task in seconds.

Cataloging dependencies in the framework can be similarly automated by querying the .config files directly with Ask Devin.

## Sharing Dependencies

In Bogard's Guide original guide:[Sharing Dependencies](https://www.jimmybogard.com/tales-from-the-net-migration-trenches-shared-library/)

The next step is to build a shared library that both the legacy framework and the new Core app can use. This involves upgrading dependencies in the old framework and moving them into a shared project accessible to both environments.

Devin thrives on repetitive tasks like upgrading dependencies, refactoring structure, and moving code where it belongs. In just a few Sessions, Devin upgrades and relocates dependencies into a shared directory.

## **Controllers & Business Logic**

*In Bogard's Guide original guide: [Controllers](https://www.jimmybogard.com/tales-from-the-net-migration-trenches-our-first-controller) &[Business Logic](https://www.jimmybogard.com/tales-from-the-net-migration-trenches-migrating-business-logic)*

A .NET Framework application contains many controllers, each tied to business logic. Part of the challenge is that .NET Core requires different built-in bundling and minification from Framework.

The process isn’t identical every time, but it’s repeatable; this is where coding agents shine*.* [Devin Playbooks](https://docs.devin.ai/product-guides/creating-playbooks) help automate these repetitive tasks.

## **Views**

*In Bogard's Guide original guide**:** [Views](https://www.jimmybogard.com/tales-from-the-net-migration-trenches-our-first-views/)*

.NET Core uses a different bundler from Framework. Devin can help adapt our code to the new bundler. Devin automates this switch to WebOptimizer in just a few Sessions.

## **Session State**

*In Bogard's Guide original guide: [Session State](https://www.jimmybogard.com/tales-from-the-net-migration-trenches-session-state/)*

During a Strangler Fig migration, the new Core app often needs to talk to the legacy Framework app. The .NET Core and .NET session states aren’t compatible (the old system handled locking and serialization automatically, but the new one doesn’t), so we use a remote app to facilitate communication amongst the two. This remote is also useful for the [migration of the authentication service](https://www.jimmybogard.com/tales-from-the-net-migration-trenches-authentication/).

First, pinpoint every Session State usage across the .NET Framework codebase instantly using a Devin Session.

Next, use a Devin Session to register each key and serializer.

Then, register the adapters and client in .NET Core and make the remote session available to controllers.

Finally, update all usages of session state in the new core to use the System.Web adapters.

## Conclusion

What once took months of manual work now takes weeks, accelerated by a coordinated fleet of Devins. If your team is staring down a migration like this, or just needs more hands on deck, [consider hiring Devin](https://cognition.ai/contact).
