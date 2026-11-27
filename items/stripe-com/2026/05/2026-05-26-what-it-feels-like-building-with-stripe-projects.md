---
title: What it feels like building with Stripe Projects
link: https://stripe.dev/blog/what-it-feels-like-building-with-stripe-projects
source: stripe-com
published: 2026-05-26T00:00:00Z
updated: 2026-05-26T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Ben Smith
summary: 'Building a GitHub traffic dashboard with Stripe Projects revealed a surprising shift from “AI coding” into real infrastructure orchestration. In under an hour, Auth0, Supabase, Vercel, Prisma, deployments, environments, and scheduled jobs materialized around an idea with remarkable speed. The experience highlighted both the power and the emerging challenge of AI-assisted infrastructure: operational visibility, ownership clarity, environment management, and understanding rapidly orchestrated systems.'
content: extracted
html: 2026-05-26-what-it-feels-like-building-with-stripe-projects.html
remote_preview:
  url: https://stripe.dev/og/blog/what-it-feels-like-building-with-stripe-projects.png
  alt: 'Stripe Developer Blog: What it feels like building with Stripe Projects'
---

I set out to build a simple GitHub traffic dashboard in around an hour using [Stripe Projects](https://docs.stripe.com/projects). What I didn’t expect was how quickly the experience stopped feeling like “AI coding” and started feeling like a real operational stack materializing around an idea with remarkable speed.

The original problem was straightforward. GitHub’s traffic analytics are useful enough to immediately expose their own limitations. You can see views, clones, referrers, and the occasional spike after a launch, but after 14 days the history disappears. If you want to compare launches over time or understand whether a repo is actually growing, the default dashboard runs out of road quickly.

I’d hit that frustration enough times that I finally decided to build my own archive layer.

![The final result](https://stripe.dev/images/what-it-feels-like-building-with-stripe-projects/image2.png)

## **Building at the orchestration layer**

The idea itself was simple: authenticate users, let them connect repositories, periodically sync GitHub traffic data, store historical metrics, and chart trends over time so the data never disappears. The application wasn’t especially complicated. What interested me far more was the process of building it.

I intentionally timeboxed the entire build to 60 minutes because I wanted to test a different way of working. I wasn’t trying to see how quickly I could build an app manually. I wanted to see how far I could get operating almost entirely at the orchestration layer using AI tooling.

So instead of opening an editor and building everything piece by piece myself, I approached the exercise more like a builder/operator. I used [Cursor’s](https://cursor.com) agentic chat workflow alongside Stripe Projects and intentionally avoided writing code directly wherever possible. I wanted to see what it felt like to guide the system rather than implement every detail myself.

## **Infrastructure starts materializing**

The following prompt is what kicked it all off. The experience became interesting almost immediately:

![The prompt that kicked it all off](https://stripe.dev/images/what-it-feels-like-building-with-stripe-projects/image3.png)

Within minutes, infrastructure started materializing. [Auth0](http://auth0.com) for authentication, [Supabase](https://supabase.com/) for persistence, [Vercel](https://vercel.com/) for hosting and deployments, [Prisma](https://www.prisma.io/) for database schema management, environment wiring. Real services, real deployments, real operational systems, all appearing with surprisingly little friction.

*“WOW, this is way too easy.”*

There’s a very specific moment during the build where you realize you’re no longer stitching together tutorial projects or isolated demos. The stack crossing from “prototype” into something operational happens extremely quickly. One minute you’re discussing app structure with an AI agent, and the next minute you’re looking at real provider dashboards, deployed environments, authentication flows, and persistent infrastructure.

That shift was probably the most interesting part of the entire experience. The feeling reminded me of watching someone confidently use Waymo for the first time while my own brain still struggles with the idea of giving up that much control. You can intellectually understand that the system is operating correctly, but there’s still a weird adjustment period where your intuition hasn’t fully caught up to the amount of complexity being handled automatically underneath you.

## **The operational comprehension problem**

At first, the abstraction felt almost magical. But as the build progressed, I noticed something else happening: the operational complexity was being compressed so effectively that I occasionally lost track of the architecture underneath it.

Very quickly I realized I now had:

- Auth0 infrastructure
- Supabase infrastructure
- Vercel deployments
- deployment connections
- authentication flows
- environment variables
- scheduled background jobs

…all assembled with very little manual setup. And that raised an entirely different kind of question. Not “Can I build this?” but, “How do I reason clearly about everything that now exists?”

Auth0 became the clearest example of this shift. At one point I found myself mentally trying to map:

- who owned which resources
- how tenants were connected
- where credentials lived
- which deployment was active
- how the providers related to each other operationally

![The speed of orchestration](https://stripe.dev/images/what-it-feels-like-building-with-stripe-projects/image4.png)

Nothing was broken. Quite the opposite, things were working surprisingly smoothly. But the speed of orchestration was faster than the speed at which I naturally built a complete mental model of the system.

That’s what made the experience feel fundamentally different from traditional development.

## **From AI coding to infrastructure orchestration**

Stripe Projects didn’t really feel like “AI coding assistance.” It felt much closer to infrastructure orchestration with AI mediation. The hard part wasn’t generating React components or wiring APIs together manually anymore. The interesting challenge became understanding ownership, architecture, deployment topology, operational flow, and maintainability. Cost awareness increasingly becomes part of that operational understanding too. Once infrastructure orchestration becomes fast enough, developers need visibility not just into architecture, but into spend boundaries and provider-level risk across the stack. Soon, seeing consolidated provider spend and setting provider-specific controls feels like a natural evolution of Projects from pure provisioning into operational infrastructure management.

The stack came together astonishingly quickly: Next.js, Prisma, Supabase, Auth0, Vercel, scheduled jobs, all stitched together into something that genuinely resembled a production application rather than a tutorial project.

![The stack](https://stripe.dev/images/what-it-feels-like-building-with-stripe-projects/image1.png)

Visualized using a [community tool](https://github.com/bildungsroman/stripe-projects-visualizer/tree/main) built by Anna Spysz.

*“I don't know what we built, but we built it.”*

The architecture increasingly felt like something a small team could realistically continue building on. The final 20% of the project was where the experience became especially revealing.

Once manual intervention became necessary, I found myself navigating a stack that had been assembled collaboratively between me, Cursor, and Stripe Projects. Things like Auth0 callback adjustments, deployment mismatches, token setup, sync troubleshooting, and environment variable debugging. That’s a very different feeling from building a system entirely by hand.

## **Environment management becomes the product**

Another thing that stood out was how much operational complexity gets compressed into environment management. By the end of the build there were secrets and credentials spread across multiple providers: GitHub PATs, database URLs, Auth0 secrets, deployment configuration, Prisma environment settings.

Looking at the generated environment variables was probably the clearest moment where the orchestration became tangible. Projects had already wired credentials, provider configuration, deployment bindings, and service connections across multiple systems automatically.

And once you start operating this way, environment management becomes incredibly important very quickly: separating development from production credentials, understanding where secrets live, and controlling which infrastructure agents are allowed to touch. Native `dev` and `prod` environments inside Projects feel especially important in that world.

The role of the builder changes as these tools become more capable. The implementation burden decreases dramatically, but operational awareness becomes increasingly important. I think that’s where this category of tooling becomes especially exciting.

Because despite the complexity, the app actually worked. The repositories synced. The charts are populated. Historical traffic data appeared. The original GitHub problem I started with was suddenly solved. And that moment felt very different from most AI-assisted coding demos I’ve seen. This didn’t feel like a toy scaffold or generated sample project. It felt like a real production-shaped application appearing extremely quickly. That’s what stayed with me after the build ended.

## **What comes next**

The biggest insight I came away with is that the next frontier probably isn’t better code generation. The interesting challenge now is helping builders better understand what exists after the orchestration happens: how services connect, how infrastructure is structured, how deployments relate to providers, and how to reason about ownership and operational flow.

One thing I kept wishing existed throughout the build was some kind of generated architecture or IaC layer that could clearly explain the operational system being assembled around me: the infrastructure and integrations that had been created, how the providers connected together, and what the reproducible state and configuration of the stack actually looked like. At that point, tools like this stop feeling like rapid prototyping tools. They start feeling like genuinely new ways of building software.

And honestly, I think we’re much closer to that future than most people realize.

If you want to experiment with a similar setup yourself, Stripe Projects can spin up an Auth0, Supabase, Vercel stack directly from a starter template. [Here’s the shared stack configuration](https://projects.dev/s/v1:Auth0~client,Supabase~project,Vercel~project) I used.

To learn more about about building with Stripe Projects, go to [projects.dev](https://projects.dev/). Better still, [install it](https://docs.stripe.com/stripe-cli/install?install-method=homebrew#install) and try asking your favorite coding agent, that's what I did.
