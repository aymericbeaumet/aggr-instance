---
title: How API changes flow into Stripe's developer products
link: https://stripe.dev/blog/how-api-changes-flow-into-stripes-developer-products
source: stripe-com
published: 2026-06-15T00:00:00Z
updated: 2026-06-15T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Mikael Mello
summary: 'When a Stripe engineer makes changes to an API, merging the PR is just the beginning. Stripe maintains a vast developer product suite that needs to stay in sync with the API changes we frequently ship:'
content: extracted
html: 2026-06-15-how-api-changes-flow-into-stripe-s-developer-products.html
remote_preview:
  url: https://stripe.dev/og/blog/how-api-changes-flow-into-stripes-developer-products.png
  alt: 'Stripe Developer Blog: How API changes flow into Stripe''s developer products'
extra:
  content:interactive: true
---

When a Stripe engineer makes changes to an API, merging the PR is just the beginning. Stripe maintains a vast developer product suite that needs to stay in sync with the API changes we [frequently ship](https://docs.stripe.com/changelog):

- The [official SDKs](https://docs.stripe.com/sdks) need to have new versions published across seven different languages and three release channels.
- The [CLI](https://docs.stripe.com/stripe-cli), [Shell and API Explorer](https://docs.stripe.com/workbench/shell) need to be updated to include the new functionality.
- The [API Reference](https://docs.stripe.com/api) needs to reflect the new API shape, and have meaningful descriptions to assist developers in integrating with it.
- The [public changelog](https://docs.stripe.com/changelog) needs to list the new change, why it matters, and how it affects users.
- Internal tools need to reflect these changes to help Stripe engineers (and AI agents) manage and maintain their APIs.
- … and more.

Keeping all of this in sync is a significant challenge, and building the infrastructure to do it reliably is a long-term investment that started a few years back.

## Enter OpenAPI

The [OpenAPI Specification](https://www.openapis.org/what-is-openapi) defines a standard format for describing HTTP APIs. It’s widely used across the industry to generate documentation, client libraries, and to build out a company’s [Developer Experience Infrastructure](https://kenneth.io/post/developer-experience-infrastructure-dxi) (DXI). [Stainless](https://www.stainless.com/) (founded by a former Stripe engineer and recently acquired by Anthropic) and many other companies are built around solving the challenge of DXI.

OpenAPI has been part of Stripe’s toolbox since 2016, initially powering internal tooling like validations and test suites. In 2017, we published a [public OpenAPI description of our API](https://github.com/stripe/openapi) and kept it up-to-date as our APIs evolved. In 2019, we used OpenAPI to auto-generate a Stripe SDK for the first time, releasing stripe-java@8.0.

But OpenAPI wasn’t universally adopted across Stripe. Some tools relied on the OpenAPI descriptions we generated. Others, like the API Reference, bypassed OpenAPI entirely and parsed APIs straight from our internal Ruby-based [DSL](https://en.wikipedia.org/wiki/Domain-specific_language).

### A breaking point

In 2024, Stripe introduced the [v2 namespace](https://docs.stripe.com/api-v2-overview) to bring improved domain modeling and updated API semantics to our users. Internally, this meant an architectural shift in how we define and implement APIs, leveraging Protocol Buffers. Unlike v1, v2 APIs are defined by a separate internal DSL and serialized into snapshots using a Stripe-built internal API spec. Both are protobuf-based, which lets us share primitives and represent the DSL, API shape, and version differences (patch files) in the same format.

```
class AccountRetrieveMethod < APICore::AbstractAPIMethod
  description "Retrieves the details of an account."

  resource AccountAPIResource
  permission Permission.fetch(:account_retrieve)
  # ...

  client_metadata(
    in_class: "AccountAPIResource",
    method_type: :retrieve,
    endpoint_url: "/v1/accounts/:id"
  )

  def execute
    # ...
  end
end
```

```
service AccountsApi {
  // Retrieves the details of an Account.
  rpc RetrieveAccount(RetrieveAccountRequest) returns (RetrieveAccountResponse) {
    option (v2.method) = {
      get: "/v2/core/accounts/:id"
      documented: PUBLIC
      permission: "v2_account_read"
      error_code: "not_found"
      error_code: "account_rate_limit_exceeded"
      // ...
    };
  }
}
```

Now there were two formats: OpenAPI (or parsing the Ruby-based DSL) for v1, and our internal API spec for v2.

The consequences of the new internal API spec were immediate. Teams responsible for a developer product now had to support two formats instead of one. SDK and API Reference generators had to be duplicated. Preview support for v2 APIs in the CLI was shut down entirely because the maintenance burden was too high. Stripe Shell didn’t support v2 APIs at all.

The situation was already unsustainable, and we hadn't even launched v2 publicly yet. With new developer products like [Stripe Workflows](https://docs.stripe.com/workflows) and an improved changelog on the way, things were only going to get worse. It was clear we needed to converge on a single way of representing Stripe’s APIs.

Between the different DSLs, the internal API spec, and OpenAPI, the choice was clear: converge on OpenAPI. OpenAPI was, and still is, the industry standard for representing HTTP APIs and a path already proven within Stripe. Most importantly, OpenAPI is extensible. Anything we needed to represent that wasn’t natively supported could be added as an extension.

This decision to use OpenAPI enabled us to further expand a pipeline that ships developer products at scale, keeping them in sync with every API change we make.

## How the pipeline works

The pipeline starts with generating OpenAPI descriptions for each API namespace. But not every consumer of those descriptions needs the same thing. The Stripe CLI doesn't need the same information as the public API Reference. Internal tools can operate on APIs that are internal or still in development, which have no place in public-facing SDKs. To handle this, the generator’s output is controlled by three main parameters: variant, phase, and version. Together, they determine which API definitions (methods, event types, resource types, etc.) and what metadata is included in each artifact.

![](https://stripe.dev/images/how-api-changes-flow-into-stripes-developer-products/image2.png)

> The OpenAPI Generator takes inputs from both v1 and v2 DSLs and produces tailored artifacts per consumer.

The variant controls which APIs and metadata are included for a given consumer. Think of variants as "surfaces." Each developer product gets its own tailored artifact.

- Public artifacts, like our [stripe/openapi](https://github.com/stripe/openapi) repository, exclude metadata that describe private or sensitive information.
- Some APIs are omitted entirely from certain artifacts. For example, methods that power internal integrations like the Stripe Dashboard don't appear in the descriptions powering public SDKs.

The phase parameter filters APIs based on their [release phase](https://docs.stripe.com/release-phases):

| Phase                 | What's included                            |
| --------------------- | ------------------------------------------ |
| `generally_available` | GA APIs only                               |
| `public_preview`      | GA + public preview APIs                   |
| `private_preview`     | GA + public preview + private preview APIs |

For example, our public API Reference is powered by two different artifacts chosen based on whether the Preview toggle is enabled–one for GA APIs and one for preview. That distinction is controlled at the DSL level, so that a method marked undocumented won't appear in any public spec and one marked private\_preview won't appear in GA artifacts.

The version parameter dictates what version of Stripe's API is being described. Products like [API Reference](https://docs.stripe.com/api) or [Stripe Database](https://docs.stripe.com/data/database) can support different views of the API depending on the API version chosen by the customer, and this parameter allows these teams to fetch the appropriate OpenAPI description.

In the v2 namespace, all API changes are stored on disk and easily reproducible from the beginning of history. This allows us to generate snapshots of the API surface, and their OpenAPI representation, for any point in time. With that capability, we can provide a canonical OpenAPI description for any past or current API version, and propagate improvements to the spec (e.g. richer metadata about an API) at will.

Things are a little trickier for v1. Due to the historical architecture of the v1 platform, it’s not trivial to regenerate descriptions for non-current API versions. Today, when a new API version is released, we store the current OpenAPI spec as the canonical description of v1 APIs for that version. It’s not perfect: new variants or metadata we add to the spec can't be backfilled into older versions. This is a problem we still have to solve in v1: how to support older API versions and leverage new OpenAPI features we add.

## Making OpenAPI our own

Stripe uses [vendor extensions](https://swagger.io/docs/specification/v3_0/openapi-extensions/) to include extra information about the APIs and communicate to consumers how the spec should be interpreted. The extensions cover gaps in what the OpenAPI spec natively supports, helping us bridge the two API namespaces and their DSLs when possible, and signal to consumer products about special behavior that applies to some APIs but not others.

For example, we define Stripe's resources as reusable JSON schemas (under [#/components/schemas](https://learn.openapis.org/specification/components.html)) and include an extension called x-stripeOperations. This extension lists which API methods operate on a given resource and, with some additional metadata, directly powers the methods available on an SDK object and the subcommands available in the Stripe CLI.

```
components:
  schemas:
    v2.core.account:
      type: object
      properties: [...]
      x-stripeOperations:
      - method_name: list
        method_type: list
        operation: get
        path: /v2/core/accounts
      # ...
      - method_name: close
        method_type: custom
        operation: post
        path: "/v2/core/accounts/{id}/close"
```

Extensions helped us turn a standard spec into something purpose-built for Stripe's needs. Here are a few others we have added over the years:

| Extension Name       | Applies to              | Description                                                                                                                                                                                  |
| -------------------- | ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `x-resourceId`       | Resource schemas        | String that represents the canonical name for that resource.                                                                                                                                 |
| `x-stripeResource`   | Resource schemas        | Object containing type hints for consumers, e.g. the package location where the type should be added.                                                                                        |
| `x-stripeEvent`      | Event schemas           | Object containing additional metadata about an event, e.g. its type identifier or whether it represents a [thin event](https://docs.stripe.com/event-destinations#benefits-of-thin-events). |
| `x-stripeAccess`     | Operations or schemas   | Object containing metadata about the access and visibility of that API definition, used by the API Reference to conditionally render documentation for APIs that are not publicly available. |
| `x-expandableFields` | v1 API Resource schemas | Contains a list of names of fields that are expandable via the `expand` parameter. See [expanding objects](https://docs.stripe.com/api#expanding_objects).                                  |

## Drawing the rest of the owl

The above is just a small piece of the work we do to be able to deliver consistently world-class developer experiences. Over Stripe's lifetime, we’ve invested significant care into understanding how API changes impact our entire product suite. Along the way, we’ve codified a set of design patterns that all APIs must follow.

Before a PR can merge, a set of automated tests validate API changes against those patterns: API naming, field types, versioning considerations such as backwards compatibility, or constraints specific to products (e.g. reserved words in SDKs). Documentation and code samples follow the same model, they're authored and validated alongside the API definition itself and threaded through to the API Reference generator via OpenAPI artifacts.

```
warn[empty_doc_string]: Every public API method, event, error, field, & enum value should have a doc string. See https://go/v2-api-ref for more details.
  --> /path/to/accounts_api_service.proto:71
70 |   
71 |   rpc CreateAccount (CreateAccountRequest) returns (CreateAccountResponse) {
72 |     option (v2.method) = {
73 |       stable_id: "v2_method_create_account"
74 |       post: "/v2/core/accounts"
   | ...
  help: see https://go/api-v2-lint/empty_doc_string for more information
```

In the PR view, we display an OpenAPI-based diff of the changes for visual (or much more common now: agent-assisted) spot-checking. In the v2 namespace, we also detect and display changes to past API versions (if applicable), a feature that has been especially helpful for catching inadvertent changes to older API versions.

![](https://stripe.dev/images/how-api-changes-flow-into-stripes-developer-products/image4.png)

Once the changes get merged, a pre-release flow is kicked off: the changes make their way into our internal API versioning tool, where the OpenAPI description of the previous and upcoming API versions are compared. Each change is listed and enriched with extra information such as previews of how SDKs will be affected, the affected product, suggested changelog descriptions, and more, providing one more place for product architects to review and refine upcoming changes.

![](https://stripe.dev/images/how-api-changes-flow-into-stripes-developer-products/image3.png)

At release time, the canonical OpenAPI description for the new version is snapshotted and propagated to downstream consumers for their subsequent releases. The new artifact is made available to internal services via direct dependencies or build artifacts. It’s published to our CDN and fetched by workflows in our open-source repos such as [stripe/openapi](https://github.com/stripe/openapi) and [stripe/stripe-cli](https://github.com/stripe/stripe-cli). The API Reference is updated with the new API version, up-to-date code snippets are generated, new versions of our SDKs and the CLI are released, and so on.

![](https://stripe.dev/images/how-api-changes-flow-into-stripes-developer-products/image1.png)

All of those improvements–and many others not mentioned here–have helped Stripe maintain a high bar for developer products while reducing manual work for engineers and the chance of mistakes being made.

## What comes next

The pipeline’s foundation is solid, but there’s a lot more we want to build on top of it.

Smoothing the differences between defining v1 and v2 APIs is an area of active work as we incorporate the learnings of both stacks to provide a more unified experience to Stripe engineers building on either namespace (or most likely, both!). We are also extending our DSLs so that product teams can more easily define custom integration points targeting any developer product: some functionality may not really fit Stripe's API, but make a lot of sense in the Stripe CLI or as an action in a Stripe Workflow.

As Stripe launches new developer products, such as [Stripe Database](https://docs.stripe.com/data/database), we constantly find new use-cases that fit into this pipeline and managing that growth becomes its own challenge. It takes a coordinated effort maintaining parity between the DSLs, the OpenAPI generators of both v1 and v2 stacks, and keeping consumer teams in the loop when further extending the spec.

—

There's no shortage of interesting problems left to solve. If that sounds like your kind of challenge, and you want your work to reach every developer who builds on Stripe, [we're hiring](https://stripe.com/jobs).
