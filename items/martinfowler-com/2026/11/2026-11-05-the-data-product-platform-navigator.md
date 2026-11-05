---
title: The Data Product Platform Navigator
link: https://martinfowler.com/articles/data-product-platform-nav.html
source: martinfowler-com
published: 2026-11-05T14:11:00Z
updated: 2026-11-05T14:11:00Z
first_seen: 2026-11-05T17:57:26.867095589Z
authors:
- Martin Fowler
content: extracted
html: 2026-11-05-the-data-product-platform-navigator.html
remote_preview:
  url: https://martinfowler.com/articles/data-product-platform-nav/card.png
---

Guiding Your Data Mesh Journey using the Platform Engineering Maturity Model

## Data Mesh and Data Products need a Self-Service Platform

One of the core principles of Data Mesh is the need for a self-service data platform. To achieve decentralized ownership, domain teams must be empowered to build, deploy, and manage their own data products without friction or cognitive overload. A self-service platform provides the necessary abstractions, automation, and tools to make this autonomy possible.

A well-designed platform should incorporate key data mesh concepts like data contracts and data products. Data contracts ensure a controlled and stable handover of data between teams. Data products act as the unit of reusability for productized data and can be connected to each other to form the network or 'mesh' in which decentralized ownership can be assigned.

![](https://martinfowler.com/articles/data-product-platform-nav/self_service_platform_need.svg)

Figure 1: A high-level overview of a self-service data platform empowering the creation and lifecycle of data products across domains.

## The Platform Gap

To build a complete data product, we need various technologies such as compute to run data transformations, data storage, data ingestion tools, data catalogs, query engines, quality tools, and more. When we examine these technologies, we often find a significant gap because they typically lack the necessary abstractions for concepts like data contracts or data products.

This “platform gap” arises because a single data product requires a combination of these technologies to function as an integrated whole. Thus, no single underlying technology can cover the whole data product either. The absence of built-in abstractions often leads to confusion about what a data product is and how it manifests within the technology stack. This complexity can make it difficult for teams to use the available technologies directly, hindering the adoption of data mesh principles.

![](https://martinfowler.com/articles/data-product-platform-nav/platform_gap.svg)

Figure 2: The Platform Gap, illustrating a disconnect between high-level Data Mesh concepts and the underlying data technologies.

## Platform Engineering to the Rescue

The obvious solution to the platform gap is to engineer a platform that sits on top of the existing data technologies. This new layer should raise the level of abstraction, reduce cognitive load for developers, and provide a user experience centered around data contracts and data products. The goal is to automate and integrate the experience, creating a truly self-service platform.

However, building such a platform is not without its challenges. Platform teams often struggle to articulate their progress, prioritize their work, and demonstrate the value of their investments. This is where a structured approach, guided by a maturity model, provides clear guidance.

![](https://martinfowler.com/articles/data-product-platform-nav/platform-engineering-rescue.svg)

Figure 3: Platform engineering bridging the platform gap by creating a new layer of abstraction on top of existing data technologies.

## The Platform Navigator: CNCF Maturity Model

The platform engineering community, through the Cloud Native Computing Foundation (CNCF), has published the [Platform Engineering Maturity Model](https://tag-app-delivery.cncf.io/whitepapers/platform-eng-maturity-model/). This model provides a framework for assessing and advancing platform capabilities, making it an excellent navigator for a data mesh journey.

### Understanding the Maturity Model

The CNCF Platform Engineering Maturity Model is defined by several maturity levels and a set of aspects that evolve as the platform matures.

The maturity levels are:

- *Provisional:* An initial, ad-hoc platform.
- *Operational:* A more standardized and centrally managed platform.
- *Scalable:* A self-service platform that encourages adoption.
- *Optimizing:* A platform that is continuously improving and deeply integrated into the development workflow.

The aspects of the platform that are assessed at each level include:

- *Investment:* How staffing and funds are allocated to the platform.
- *Adoption:* How and why users discover and use the platform.
- *Interfaces:* How users interact with and consume platform capabilities.
- *Operations:* How platform capabilities are planned, prioritized, developed, and maintained.
- *Measurement:* How feedback is gathered and incorporated to improve the platform.

![](https://martinfowler.com/articles/data-product-platform-nav/cncf-maturity-model.svg)

Figure 4: The CNCF Platform Engineering Maturity Model, showing the four levels Provisional, Operational, Scalable, Optimizing and the five aspects (Investment, Adoption, Interfaces, Operations, Measurement).

### How to Use the Maturity Model

![](https://martinfowler.com/articles/data-product-platform-nav/maturity_model_process.svg)

Figure 5: The three-step process for using the maturity model: 1. Know your current platform maturity, 2. Focus on high friction aspects, 3. Iterate.

Using the maturity model is a straightforward process:

1. *Know your current state:* The first step is to map your current platform situation against the different aspects and maturity levels. For example, we might have a dedicated team for platform investment (Level 2), but operations and measurement might still be ad-hoc (Level 1).

![](https://martinfowler.com/articles/data-product-platform-nav/current_state_mapping.svg)

Figure 6: Mapping the current state of a platform on the maturity model grid, and indicating planned improvements on aspects with high friction.

2. *Focus on a specific aspect:* Once we have a clear picture of the current state, we can focus on a specific aspect to improve. For instance, if we want to ensure we are building the right features, we might decide to invest in a platform product team and start collecting insights on the biggest pain points for our users.
3. *Iterate:* The final step is to apply our improvements and continuously adapt.

As we iterate, we must remember to balance the different aspects of the maturity model against each other. Some aspects, such as Investment and Adoption, heavily revolve around people, policies, and processes, while others, like Interfaces and Operations, are more focused on technology. By using the maturity model grid as our compass, we can consciously guide our next steps. For example, we might realize we need to balance our investments by focusing on self-service solutions and automated central enablement (technology) only after we have established a dedicated product team and collected insights (people, policies, and processes). This balance prevents us from over-investing in one area while leaving others behind, and the model helps us clearly communicate these strategic decisions to stakeholders.

## The Four Levels of Platform Maturity for Data Products and Data Mesh

### Level 1: Provisional Platform - The “No Platform” Platform

A provisional platform for data products and data mesh is characterized by an ad-hoc and chaotic approach to technology and provisioning. To illustrate this, let's use a bit of domain storytelling.

Figure 7: Domain storytelling for a provisional platform, showing developers making ad-hoc requests to different engineers for various components, leading to a chaotic mix of technologies.

Consider a first data product developer who sets out to create a new data product. Lacking a central self-service platform, they must navigate the organization through personal networks. They start by talking to a DevOps engineer \[1\] to request a source control repository, and that engineer creates a Git repository \[2\] on the Git platform. Next, the developer needs a build and deployment automation pipeline, so they talk to a second DevOps engineer \[3\], who creates a CI pipeline \[4\] on the CI system. For data storage, the developer talks to a cloud engineer \[5\], who creates an S3 bucket \[6\] on AWS Cloud. With the basic infrastructure in place, the developer sets up their own data processing tools: they create an Iceberg database \[7\] on an Iceberg catalog and query engine, and create a Spark job \[8\] running on a Spark cluster. By manually assembling these disparate pieces, they produce something that resembles a data product.

Sometime later, a second data product developer is tasked with building another data product. Having no platform documentation or self-service interface to guide them, they talk to the first developer \[9\] to ask how to get started. The first developer refers them to the DevOps engineer who helped them earlier. The second developer then talks to that same DevOps engineer \[10\], who creates another Git repository \[11\] on the Git platform. Next, the developer talks to the second DevOps engineer \[12\], who creates another CI pipeline \[13\] on the CI system.

When choosing their analytical technology stack, however, the second developer takes an entirely different direction. Rather than using Iceberg and Spark, they create a dataset \[14\] on Google BigQuery, run a dbt model \[15\] on dbt Cloud for transformations, and create a view \[16\] on a data virtualization tool to serve the data.

This scenario highlights the core failure modes of a provisional platform. Data technology is requested and provisioned entirely ad-hoc, with human bottlenecks at every step where developers must “know someone who knows someone that knows how to do something” to make progress. There is no explicit concept of a data product or data contract. There are only isolated, manually stitched pieces of technical infrastructure. The result is too high a cognitive load, deep technological fragmentation and complexity, and increasing shadow IT across the organization.

### Level 2: Operational Platform - The “Fragmented” Platform Experience

At the operational level, there is an effort to standardize technology and track requests centrally. However, the experience for data product developers remains fragmented and complex.

To introduce order into the chaos, organizations typically establish dedicated infrastructure teams (e.g. a DevOps Team, a Cloud Team, and a Data Platform Team). Requests are then channeled through a centralized ticketing system like Jira.

Figure 8: Domain storytelling for an operational platform, showing developers using a ticketing system, but still having to manually glue the components together.

Continuing the story, the data product developer now has to request every component through formal tickets:

The developer creates a Git Repository Ticket \[1\] in the ticketing system. Sometime later, the DevOps Team reads the ticket \[2\] and provisions a Git repository \[3\] on the Git platform. The developer then creates a CI Pipeline Ticket \[4\], which the DevOps Team reads \[5\] to provision a CI pipeline \[6\] on the CI System. For storage, the developer creates an S3 Bucket Ticket \[7\], which the Cloud Team reads \[8\] to provision an S3 bucket \[9\] on AWS Cloud. Turning to data infrastructure, the developer creates an Iceberg Database Ticket \[10\], which the Data Platform Team reads \[11\] to provision an Iceberg database \[12\] on the Iceberg Catalog & Query Engine. Finally, the developer creates a Spark Job Ticket \[13\], which the Data Platform Team reads \[14\] to provision a Spark job \[15\] on the Spark Cluster.

While this introduces central visibility and standardizes the technology catalog, it replaces personal tapping-on-shoulders with queues and handoffs. Waiting for multiple teams to fulfill tickets takes significant time. More critically, once all these individual components are provisioned, the developer is still left entirely alone to glue them together into a functioning data product. Frustrated by the delays and integration friction, developers are often tempted to bypass the platform altogether, managing \[16\] their own shadow IT solution (“My Data Platform”) to deliver results faster.

At the operational level, automation can be introduced through typical Infrastructure-as-Code (IaC) scripts (e.g. Terraform) to speed up the provisioning of individual components. This will avoid ticketing systems and human bottlenecks.

Figure 9: Domain storytelling for an operational platform, showing developers using limited Infrastructure-as-Code automation, but still having to manually glue the components together.

In this alternative setup, each specialized team provides reusable scripts that developers can execute independently. The data product developer executes the `createGitRepository` script \[1\], which automatically provisions a Git repository \[2\] on Git. The developer then executes the `createCIPipeline` script \[3\], which provisions a CI pipeline \[4\] on the CI System. For cloud storage, the developer executes the `createS3Bucket` script \[5\], provisioning an S3 bucket \[6\] on AWS Cloud. For data assets, the developer executes the `createIcebergDatabase` script \[7\], provisioning an Iceberg database \[8\] on the Iceberg Catalog & Query Engine, and executes the `createSparkJob` script \[9\], provisioning a Spark job \[10\] on the Spark Cluster.

While this script-based automation eliminates ticketing delays, the developer is still left with the complex task of gluing everything together to create a data product. The cognitive load remains high, and there is a knowledge bottleneck, as developers need to know how to compose data products from these different components (e.g. configure and integrate networking, security roles, storage, pipelines, secrets, etc). When errors occur, developers run into knowledge gaps and still need \[11\] direct human help from the DevOps Team, Cloud Team, and Data Platform Team.

Ultimately, this operational approach leads to a lack of uniformity: organizations end up with “Frankenstein” data products that look similar on the surface because they use the same underlying technologies, but are architecturally inconsistent under the hood. Exhausted by the complexity of stitching everything together, developers frequently retreat to shadow IT, continuing to manage \[12\] their own local platforms (“My Data Platform”).

### Level 3: Scalable Platform - Platform as a Product

A scalable data mesh platform represents a major shift in the developer experience. The interface changes completely, moving from individual technologies to a higher-level, data product-centric abstraction.

Figure 10: Domain storytelling for a scalable platform, showing a developer interacting with a self-service portal that handles the provisioning and integration of data products.

In this scenario, the organization establishes a dedicated Data Product & Data Mesh Platform Team that treats the platform as a product. Rather than handling tickets or providing disconnected scripts, this team provides a self-service developer experience consisting of a Data Product Developer Portal and an underlying Data Mesh Platform Backend.

The domain story is much simpler. In summary, a data product developer just clicks 'Create Data Product', and the platform wires the technology and infrastructure together as a Data Product. In the domain storytelling diagram above, the developer creates a Data Product \[1\] on the Data Product Developer Portal by defining its architectural specification such as input and output ports, data transformation code setup, and data contracts for the structure and guarantees on the data. The portal captures this intent and instructs the Data Mesh Platform Backend to do the provisioning and integration of components.

From this single declaration, the Data Mesh Platform Backend automatically orchestrates the underlying technical resources across all infrastructure domains: it provisions a Git repository \[2\] on Git, provisions a CI pipeline \[3\] on the CI System, and provisions an S3 bucket \[4\] on AWS Cloud. In the data infrastructure domain, the backend provisions an Iceberg database \[5\] on the Iceberg Catalog & Query Engine, and provisions a Spark job \[6\] on the Spark Cluster.

Instead of just spinning up isolated components, the backend automatically wires and glues them together into a cohesive, standardized data product.

This integrated self-service tooling transforms the developer experience. The cognitive load on developers drops significantly because they interact with the abstraction of a data product rather than wrestling with underlying technologies. It removes the knowledge bottleneck: developers no longer need to be infrastructure plumbing experts who know how to hand-stitch security permissions, pipelines, and storage configurations. Furthermore, because the platform backend enforces architectural standards, every data product exhibits more technical uniformity, which prevents “Frankenstein” data products. When every data product adheres to the same standardized architectural structure, the platform can enable automated governance by injecting automated policy checks, audit logging, access controls, and data quality verification across all data products. By making data product creation easy, straightforward and frictionless, it encourages voluntary adoption, naturally discouraging shadow IT.

By removing the knowledge bottleneck, the platform empowers data product developers with more autonomy, which is crucial for scaling a data mesh and data product management practice across multiple domain teams.

### Level 4: Optimizing Platform - Platform as an Ecosystem

At the optimizing level, the platform becomes deeply integrated into the daily developer workflow. Rather than requiring developers to switch contexts and go to a platform portal, the platform comes directly to the developer's working environment and context.

Figure 11: Domain storytelling for an optimizing platform, showing the platform integrated into the developer's local environment IDE/CLI and an ecosystem of teams contributing to the platform.

This shift is achieved in the developer's local context by making the tools they live in every day (e.g. IDE or CLI) “data product aware.” The domain story illustrates how this streamlines the entire data product lifecycle:

The data product developer creates \[1\] a local Data Product directly from their day-to-day workspace. Through an IDE Plugin or a dedicated CLI, the developer can create, develop, deploy, and monitor the data products from within their familiar environment without disruptive context switching.

The local tools communicate with the platform and create \[2\] the Data Product on the centralized Data Mesh Platform Backend (which is maintained by the Data Mesh Platform Team).

From there, the Data Mesh Platform Backend automatically provisions and integrates the necessary technologies needed for the data product. A Git Repository is provisioned on Git \[3\] and a CI Pipeline on the CI System \[4\]. In AWS Cloud, an S3 Bucket is created \[5\] to store data in the data product. Structure is given to the data using an Iceberg Database \[6\] on an Iceberg Catalog & Query Engine. And finally, the data product comes alive when a Spark Job is running in the Spark Cluster \[7\].

Just as in Level 3, the platform backend automatically handles all the wiring and integration so the provisioned resources are glued together to function as a single, cohesive data product.

Another defining dimension of the optimizing level is the emergence of a federated platform ecosystem. The central platform team no longer acts as a bottleneck for new capabilities. Instead, other teams across the organization can contribute directly to the platform backend:

- An engineer from a Domain Team can contribute a domain-specific extension, such as Platform Capability X (e.g. specialized data product templates), making it available to the entire organization \[8\].
- An engineer from a Specialized Team (such as security, compliance, or data governance) can contribute platform capabilities, such as Platform Capability Y (e.g. automated PII masking, fine-grained access policies, or audit logging) \[9\].

An optimizing platform for data products and data mesh minimizes context switching for developers, manages the entire data product lifecycle, and enables an ecosystem of contributors. Managing the entire data product lifecycle empowers the organization to execute migrations of data products when underlying technologies evolve (e.g. upgrading an Iceberg or Spark version, breaking changes in data contract formats, etc), without requiring every team to rewrite their data products. Actively tracking which platform capabilities and which data products are actually consumed allows teams to prune unused data products and decommission obsolete platform features, continually refining and evolving the platform to meet organizational needs.

## Minimal Maturity Level for Data Mesh Success

Given these different levels of maturity, what is the minimum level required for a successful data mesh implementation?

![](https://martinfowler.com/articles/data-product-platform-nav/minimal_levels_for_success.svg)

Figure 12: Minimal maturity levels for Data Mesh success, highlighting the Scalable level as the minimum requirement.

The *provisional* level is essentially chaos. There is no real platform, and domain teams are left waiting for resources or building their own solutions. It's impossible to build a decentralized, interoperable mesh with disconnected and unmanaged tools.

The *operational* level, while more organized, still presents significant challenges. The high cognitive load and fragmented developer experience mean that teams are still responsible for gluing the pieces together themselves. This is a common trap for organizations that focus on automating the provisioning of individual technologies without addressing the overall developer experience. The complexity and friction often lead to low adoption rates and a continuation of shadow IT. After having invested only in standardizing and automating individual technologies, many data mesh journeys get stuck at this level, because it is too complex and costly for domain teams to participate.

This brings us to the *scalable* platform for data products and data mesh. This is the minimal level for success. At this stage, the platform team transitions from an IT helpdesk to a product team focused on developer experience and bridging the platform gap. By allowing developers to interact with the abstraction of a data product, the platform removes the complexity of the underlying technologies, encouraging voluntary adoption and enabling the decentralized ownership that is at the heart of data mesh.

## The Platform Adoption Curve

There is an interesting relationship between platform maturity and user adoption. Up to and including the operational level, leadership has to push the platform by mandating its use through policies and architectural review boards. In large organizations, engineers will find their way around painful platform tools and find the path of least resistance through shadow IT. So the platform struggles with adoption. The platform is simply too complex and difficult to use.

It is only when a platform reaches the scalable maturity level that we begin to see a significant increase in adoption because using the platform is just easier than doing it yourself (i.e. “Intrinsic Pull”). Even then, adoption doesn't skyrocket overnight. The platform team needs to evangelize the platform and educate users to drive adoption. However, the scalable level is the point where voluntary adoption becomes possible, which is a prerequisite for a successful data mesh that needs to be adopted by many domain teams.

![](https://martinfowler.com/articles/data-product-platform-nav/adoption_curve.svg)

Figure 13: The Platform Adoption Curve, showing low adoption for provisional and operational levels, with adoption taking off at the scalable level.

## Apply a Targeted Investment Strategy

The Platform Engineering Maturity Model serves as a compass for the self-service platform in a data mesh journey. It allows to make targeted investments to improve platform maturity and drive adoption. By assessing the current maturity level, we can identify the aspects causing the most friction and invest in improving them. This iterative approach allows to pull up the overall maturity of the platform and ensure that all aspects are taken care of.

As illustrated in the sequence above, a journey rarely moves all five aspects forward in lockstep:

- *Step 1:* A potential starting point is where all five aspects sit at the provisional level: ad-hoc technology, fragmented requests, and no dedicated platform team.
- *Step 2:* A common trap is where teams push into technology only and build self-service interfaces and automate operations (Level 3). Adoption still lags behind at Level 2 because nobody is spreading the word on how easy to use the platform is. There is also the risk of building capabilities nobody needs as measurement for feedback remains ad-hoc at Level 1. Organizational enablement and feedback gathering need to get priority urgently.
- *Step 3:* Using the model as a compass, the organization makes a course correction. Rather than adding more technical features, the team directs its next investment cycle into the lagging aspects. They transition to a platform-as-a-product mindset, collect actionable usage insights, and drive voluntary adoption through evangelizing the platform, embedding with domain teams, and refining the developer experience. This brings all five aspects to the scalable level, reaching the minimum for data mesh success.
- *Step 4:* With a foundation in place, subsequent investment cycles pull the platform forward into Level 4 by integrating capabilities into local IDEs and CLIs, enabling contributions from domain and specialized teams, and leveraging measurements and feedback for continuous optimization.

This is only one example of a scenario where investments are bringing the self-service data product platform to an increasing level of maturity. Every organizational context might be different and focus on different aspects first, which is completely fine. We can use the maturity model to detect what is lagging behind and keep balancing people, processes, and technology.

## Conclusion

Building a self-service platform for data products and data mesh requires balancing different aspects. On a technical level the platform should lower cognitive load significantly and be as easy-to-use as possible. At the same time it is not a purely technical project, because we want people to adopt the platform without pushing them to do so.

By using the Platform Engineering Maturity Model as our navigator, we can assess where our platform stands today, uncover the aspects causing the most friction, plan small steps for improvement, and iterate on this. The model allows us to explain our progress, what priorities we want to invest in, and the value of our investments to stakeholders to get buy-in and funding.

If you are wondering where to begin on your own journey, start small: bring this maturity model to your next platform or architecture discussion. Map where your capabilities sit today, identify the single area creating the most friction for your domain teams, and focus your next investment cycle right there.

The end goal is to bridge the platform gap and move from a fragmented and mandated toolchain to a data product and user-centric self-service platform that truly empowers teams to succeed with data mesh, data products and data contracts.

[![](https://martinfowler.com/articles/data-product-platform-nav/talk-thumbnail.png)](https://www.youtube.com/watch?v=fOjWN1jU9R0)

Watch the presentation on YouTube (20 minutes).
