---
title: 'Modern Java at Stripe: Language upgrades as a service'
link: https://stripe.dev/blog/modern-java-at-stripe-language-upgrades-as-a-service
source: stripe-com
published: 2026-05-27T00:00:00Z
updated: 2026-05-27T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Mahib Hosain
summary: 'We all know the stereotypes of working at enterprise tech companies: endless bureaucracy and behemoth codebases running on a years-old language version. Most organizations know they should modernize quickly, but haven’t figured out how to build an upgrade path compatible with a large codebase. The recurring cost of upgrading keeps most large JVM codebases trailing behind the latest Java versions.'
content: extracted
html: 2026-05-27-modern-java-at-stripe-language-upgrades-as-a-service.html
remote_preview:
  url: https://stripe.dev/og/blog/modern-java-at-stripe-language-upgrades-as-a-service.png
  alt: 'Stripe Developer Blog: Modern Java at Stripe: Language upgrades as a service'
---

We all know the stereotypes of working at enterprise tech companies: endless bureaucracy and behemoth codebases running on a years-old language version. Most organizations know they should modernize quickly, but haven’t figured out how to build an upgrade path compatible with a large codebase. The recurring cost of upgrading keeps most large JVM codebases trailing behind the latest Java versions.

As services slip further and further behind the cutting edge, they miss out on the cost savings and performance gains provided by years of JVM improvements. Developer experience degrades gradually enough that it can be hard to notice, but over time the lack of new language features eats into the velocity of shipping products. At Stripe, we weren’t willing to accept this, so we invested in a better way to stay current. Our Java team created a platform that could continuously evolve without burdensome migrations, fundamentally changing our approach from quarter-long, often unfinished migration projects to continuous, automated updates.

## Why Java upgrades stall in large codebases

On paper, upgrading Java can look straightforward: just move the toolchain, update a few dependencies, fix a handful of compiler errors, and move on. In reality, there are myriad build-graph constraints to navigate. They require coordinated dependency and runtime moves across hundreds or even thousands of owners. This complexity is why Java upgrades are often left to languish in a backlog.

Previously, our Java migrations looked a lot like other large organizations. We relied on bespoke scripts, stale inventory data, and lots of manual reasoning. While there have been some novel approaches to this (e.g., AST rewriting via [OpenRewrite](https://github.com/openrewrite/rewrite), a tool used by PayPal, Apple, and Walmart), the status quo has never shifted to a fully hands-free approach. A migration would begin with a burst of attention, make progress through the easy parts of the graph, and then slow down once it hit the hardest sections.

Even when the migration itself was useful, the machinery didn’t survive the migration. That meant every new JDK move started by rediscovering the same facts:

- What was safe to upgrade?
- What was blocked (permanently or temporarily)?
- What required coordination with teams owning external frameworks?
- What needed special handling because it lived in an awkward part of the build graph/was a fork?

We kept wasting our time solving the same problem over and over again. We wanted to stop re-running long migration projects and instead build a durable and future-proof upgrade process. And so, AutoJDK was built.

Instead of treating each Java upgrade as a recurring quarter-long maintenance project, we built a system that continuously computes upgrade eligibility from the live build graph, applies safe migrations with reviewable PRs, leverages AI to fix certain classes of errors, and highlights blockers for code that can’t be upgraded yet.

## Automatic JDK upgrades across a massive JVM monorepo

Before getting into the technical details of how AutoJDK works, it’s useful to understand a few of the nuances of version upgrades in a Java codebase.

You can compile Java code and run Java code on two different versions of the JDK; however, the runtime version must be greater than or equal to that of the compiled code. Compilation (upgrading language levels) is more restricted—it’s a matter of mass-modifying language version attributes directly in our JVM codebase in such a way that all transitive and reverse dependencies (parents and subtrees in the build graph) are satisfied with a version bump.

This means that, with some caveats, it is relatively easy to move services onto the latest Java runtime (and this is a logical first step). But, to handle the challenges around upgrading language versions, we built an internal system to automatically upgrade JDK versions across our JVM monorepo.

At a high level, the system:

1. Analyzes the live build graph to determine upgrade eligibility
2. Generates bounded, reviewable PRs instead of giant opaque rewrites
3. Tracks adoption and blockers so the remaining hard parts are visible

This allowed us to change our operating model from “How do we run a giant Java migration this quarter?” to “What did we upgrade today, and what is still blocked?”

### 1\. Reasoning over the build graph

The language-version management problem in large codebases is fundamentally about build-graph structure and platform policy. If JDK upgrades are primarily a build-graph and compatibility problem, then the solution should start from the live (most recently cached) build graph.

Our upgrader is build-graph driven. It uses graph analysis plus compatibility heuristics to determine which Java and Scala targets are eligible for upgrade.

Eligibility is not based on a flat list of targets. It takes into account things like:

- Reverse dependencies—do consumers of this library allow for a higher Java version?
- Runtime JDK compatibility caps
- Dependency-ordering constraints
- Framework-specific guards
- Coordination between source and test targets
- Bazel macros that pin language versions or have brittle logic, which should be skipped

### 2\. Generating reviewable changes

Once the system identifies eligible targets, it generates target\_jdk upgrades in bounded partitions.

From an operational standpoint, this is huge. Large upgrade waves can create merge conflicts, overwhelm reviewers, and become hard to rerun safely. We wanted the output to remain readable to humans even when the system was operating quickly at monorepo scale.

The upgrader is designed around:

- Partitioned change generation
- Automated PR creation
- Rerun-friendly, idempotent behavior
- The ability to run continuously in the background or ad hoc for a specific package or service

### 3\. Make blockers legible

One of the hardest parts of large-scale language upgrades is that blocked code tends to become folklore. Everyone knows some subsystem is “special” or some library is “still pinned,” but nobody has a crisp, live picture of how much is blocked, why it is blocked, and what would unblock it.

A big part of our system is visibility for:

- Adoption trends over time
- Blocker classification
- A clearer view of where legacy JDKs still live in the graph
- Better prioritization signals for infra teams and service owners

Some upgrade failures are genuinely deep compatibility issues, but others are more mechanical like CI cleanup, obvious fixes, or small changes needed to satisfy the new toolchain.

We use AI assistance as part of that remediation loop, especially for fixable classes of failures that would otherwise create a lot of manual cleanup toil. [Minions at Stripe](https://stripe.dev/blog/minions-stripes-one-shot-end-to-end-coding-agents) are very well-suited for this task since they can fix minor failures and use fresh git checkouts in sandboxes to do their work.

## How we use it

[Minions](https://stripe.dev/blog/minions-stripes-one-shot-end-to-end-coding-agents-part-2) are used to easily launch upgrades directly from Slack:

![](https://stripe.dev/images/modern-java-at-stripe-language-upgrades-as-a-service/image4.png)

> Note: Zoolander is the name of our JVM codebase. It was named after the [movie](https://en.wikipedia.org/wiki/Zoolander) because it originally housed Stripe’s (machine learning) models.

For large upgrades, we created an agentic skill that can run upgrades across all codebase partitions we defined, monitor and fix CI, and cherry-pick commits together into massive (~10K targets upgraded) PRs:

![](https://stripe.dev/images/modern-java-at-stripe-language-upgrades-as-a-service/image1.png)

> Most build-time risk is caught by our CI system. We set up LogScale alerts for anticipated runtime failures like UnsupportedClassVersionError, which we can then issue a targeted rollback for.

Some compatibility knowledge still needs to be kept current. Some targets remain intentionally skipped because the upgrade risk is higher than we want to absorb automatically, though the automation progress has encouraged efforts to unblock the risky migration targets.

## Impact

In under 1 week of rollout progress, service-level adoption reached 93%+ on JDK 17 while following constraints around framework and runtime compatibility. More broadly, the system made JDK 17 the most common target\_jdk configuration in our monorepo, eliminated Java 8 services, and built the foundation to move much faster to Java 21, 25, and beyond.

![](https://stripe.dev/images/modern-java-at-stripe-language-upgrades-as-a-service/image3.png)

![](https://stripe.dev/images/modern-java-at-stripe-language-upgrades-as-a-service/image2.png)

Since going live 3 months ago, we:

- Merged 160+ PRs using the automated upgrader
- Made JDK 17 the most widely used target\_jdk configuration in the monorepo
- Eliminated all Java 8 services and most Java 11 services
- Shrunk the legacy-JDK footprint, specifically JDK 8 and 11, by roughly 25K targets

## The infrastructure decisions that made this possible

The automation only works because JDK selection is centralized and enforced by the build system. A build-graph-aware upgrader is powerful, but it becomes much more powerful when version policy lives in the platform rather than in thousands of individual service configurations.

In our case, several infrastructure choices made that possible.

- Supported Java language (JDK) and runtime versions (JRE) are controlled centrally.
- Build-time checks in CI enforce dependency-ordering constraints. That prevents invalid mixed-version edges from slipping through silently.
- Our build system supports multiple JDKs at once, which allows the graph to transition gradually instead of requiring an all-at-once cutover.
- We utilize static analysis tools and custom allowlists to safely roll out new language versions while controlling available features.
- We own the underlying infrastructure with respect to Bazel Java and Scala rules, Java version manifests, and base containers.
- Our packaging model removes a common source of runtime drift by bundling a platform-specific JDK directly into deployable artifacts alongside the application artifact. This eliminates the need to rely on a matching JDK being pre-installed in a variety of deployment environments.

Taken together, these decisions mean that raising the JDK floor repo-wide can be driven by a small number of centralized definitions plus automation, rather than a scavenger hunt across thousands of targets.

A lot of upgrade pain disappears once the language-version policy lives in the platform.

## What’s next

We now have infrastructure that can do much of the heavy lifting for Java 21, 25, and beyond. This frees up our Java team to focus on solving complex problems such as enabling Java support outside of IntelliJ, making Bazel fast in our monorepo, and ensuring Stripe’s most latency-sensitive services are performing optimally. If you’re interested in improving developer infrastructure at Stripe, [we’re hiring](https://stripe.com/jobs).
