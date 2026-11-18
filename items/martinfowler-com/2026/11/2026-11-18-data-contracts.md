---
title: Data Contracts
link: https://martinfowler.com/articles/data-contracts.html
source: martinfowler-com
published: 2026-11-18T14:02:00Z
updated: 2026-11-18T14:02:00Z
first_seen: 2026-11-18T17:57:41.978411823Z
authors:
- Martin Fowler
content: extracted
html: 2026-11-18-data-contracts.html
remote_preview:
  url: https://martinfowler.com/articles/data-contracts/social-card.png
---

With today's rise of agentic AI, well-discoverable and processable business data has become the cornerstone of any company's journey towards a data-driven business. In large companies, this valuable data is created at various places within the company, with different teams being most knowledgeable about a particular topic. Therefore, creating valuable analytics or data applications is usually about pulling data together from various sources to connect them meaningfully. In this process of combining data from many sources, *trust* in the data becomes a major factor.

Imagine Alex, a data engineer at a multinational insurance company, building a customer insights application that predicts the likelihood of customers buying additional insurance products. In the company's data warehouse, Alex found a dataset that seems to contain most of the information needed to make this prediction, but Alex has several questions:

- Does the dataset have all the information I need and in what form?
- Does the data contain the information and have the meaning that I think it does?
- Is it connected to other datasets? If so, which?
- How fresh and complete is the data, and of what quality?
- Can I rely on its structure remaining stable so my application won't break in the future?
- Am I allowed to process the data for my purpose, for example under the GDPR?
- Do I have to apply special precautions, for example for personal data, when processing the data or resharing the results?
- With whom may I share the resulting predictions?

The questions above can be categorized in various ways, but they show that the matter of trust is not only about whether Alex can trust the source of the data (in terms of quality, reliability, legal issues) but also whether the recipients of the results are trustworthy to take care of the data.

Data contracts are a way to answer these questions not just once, but to ensure the trustworthy processing of data continuously.

### A Simple Example Data Contract

In its simplest form, a data contract is a description of the shape of the data that a data provider is offering, e.g., a schema. Here is an excerpt of what the description of the dataset that Alex found could look like. We will illustrate all the examples using the [Open Data Contract Standard](https://bitol-io.github.io/open-data-contract-standard) (ODCS) as it is the de facto industry standard governed by the Bitol project at the Linux Foundation (rated *Adopt* in the [Thoughtworks Technology Radar](https://www.thoughtworks.com/en-de/radar/languages-and-frameworks/summary/open-data-contract-standard-odcs)).

```
apiVersion: "v3.2.0"
kind: "DataContract"
id: "customers"
name: "Customers"
version: "1.0.0"
status: "active"

schema:
  - name: "customers"
    physicalType: "TABLE"
    description: "All customers holding a life insurance policy since 2020-01-01"
    properties:
      - name: "customer_id"
        logicalType: "string"
        primaryKey: true
      - name: "customer_name"
        businessName: "Customer Name"
        logicalType: "string"
        physicalType: "VARCHAR(100)"
        examples:
          - "Alex Doe"
        classification: "restricted"
```

Seeing this simple data contract, most likely rendered as a page in the company's data marketplace rather than as raw YAML, Alex already has a much better idea whether the data will be sufficient for implementing the prediction. But Alex is wondering when the data was last updated and if the declared fields will consistently contain correct data. In ODCS, there are specific elements to describe the quality of the data through quality rules as well as service levels that specify runtime behavior over time. Here is how the data contract can be extended to address these issues:

```
schema:
  - name: "customers"
    properties:
      - name: "customer_name"
        quality:
          - type: "sql"
            description: "Expect less than 1% empty name fields"
            query: |
              SELECT AVG(
                CASE
                  WHEN customer_name IS NULL
                    OR TRIM(customer_name) = ''
                  THEN 1
                  ELSE 0
                END
              )
              FROM customers
            mustBeLessThan: 0.01
slaProperties:
  - property: "freshness"
    value: 24
    unit: "hours"
    element: "customers.last_updated_at"
    description: "Within 24 hours of the last update"
```

Seeing these guarantees, Alex has much more confidence that building an application based on this dataset will be sustainable and not fall apart at the next update of the dataset: the active status and the 1.0.0 version indicate that this is a stable and maintained dataset; the quality and SLA descriptions allow for processing the data reliably. And actual trust comes when these guarantees are verified automatically.

### What Does This Mean for AI Agents?

Now imagine that Alex is not a human but an AI agent. A human who distrusts data hesitates, or asks the people who own it. An agent does neither: as Pramod Sadalage and Prem Chandrasekaran put it in [Making Your Data Ready for Agentic AI](https://martinfowler.com/articles/making-data-ready-for-agentic-ai.html#AgentsTreatEveryValueAsTruth), agents treat every value as truth. Where a lack of trust slows humans down, it makes agents produce confidently wrong results, and humans in turn stop trusting agents to work with data on their own. Data contracts break that loop by giving agents the explicit metadata they would otherwise have to guess.

An agent has the same questions Alex had, on every task, and nobody to ask. A data contract answers them in a form the agent can act on: published to a marketplace that agents can search, it makes the data discoverable in the first place; its schema and semantics tell the agent how to query and join the data correctly; its quality rules and service levels tell it how far to trust the result; and its terms of use tell it what it is allowed to do. That is why agentic use cases, from chatting with your data to generating whole data pipelines, are becoming one of the main reasons to adopt data contracts.

## What's in a Data Contract

Before we look at the individual sections, it helps to understand what characterizes a data contract in the first place. A data contract is:

- **Machine-readable.** A data contract is a structured document, typically YAML, not prose buried in a wiki. Tools, pipelines, and AI agents can parse it and act on it directly, without a human having to translate it first.
- **Verifiable.** Its promises are concrete enough to be checked against the actual data. A quality rule such as “less than 1% empty names” is not an aspiration but an assertion that tooling can evaluate and either pass or fail.
- **Versioned.** A data contract carries a version and evolves under a clear scheme, so consumers can tell a harmless change from a breaking one, and providers can phase new versions in and old ones out gracefully.
- **Provider-driven.** It is published one-sidedly by the party that produces the data and knows it best. They own the promises they make, which shifts responsibility for the interface to where the knowledge sits and where the promises can be kept or broken.
- **Consumer-oriented.** Even though the provider writes it, it is written for the consumer: it captures the schema, semantics, quality guarantees, and terms of use a consumer needs to decide whether the data fits their purpose and to rely on it over time.
- **The source of truth for metadata.** It is the authoritative description of the dataset. Catalogs, marketplaces, documentation, and agents all draw from it, rather than each maintaining their own divergent copy.

Such a data contract is typically defined using the Open Data Contract Standard.

### Open Data Contract Standard

The Open Data Contract Standard (ODCS) defines a YAML format for specifying data contracts in a standardized way. You've already seen a short example of that YAML in the introduction of this article. The Open Data Contract Standard is part of the [Bitol project](https://bitol.io/), a graduate project of the Linux Foundation, and thereby part of a family of metadata standards. The specification of the Open Data Contract Standard is [publicly available on GitHub](https://github.com/bitol-io/open-data-contract-standard), where it has over 1000 stars. The standard comes with a JSON schema to help validate YAML files in terms of standard compliance.

![](https://martinfowler.com/articles/data-contracts/11-data-contract-sections.svg)

Figure 1: The sections of the Open Data Contract Standard v3.2

The Open Data Contract Standard comprises many different categories of metadata, which we will cover shortly using example code snippets for each part based on ODCS 3.2, the latest version at the time of writing. An [interactive, step-by-step walkthrough of the same sections](https://datacontract.com/) is available on datacontract.com.

```
apiVersion: "v3.2.0"
kind: "DataContract"
id: "customers"
name: "Customers"
version: "1.0.0"
status: "active"
```

Every data contract starts with its fundamentals: the version of the standard, unique identifier, name, data contract version number, and status. This metadata identifies the contract and enables versioning as well as lifecycle.

### Schema and Data Quality

```
schema:
  - name: "customers"
    physicalType: "TABLE"
    description: "All customers holding a life insurance policy since 2020-01-01"
    properties:
      - name: "customer_id"
        logicalType: "string"
        primaryKey: true
      - name: "customer_name"
        businessName: "Customer Name"
        logicalType: "string"
        physicalType: "VARCHAR(100)"
        required: true
        examples:
          - "Alex Doe"
        classification: "restricted"
        tags:
          - "pii:true"
      - name: "annual_premium"
        logicalType: "integer"
        physicalType: "INTEGER"
        description: "Annual premium in cents"
        required: true
        logicalTypeOptions:
          minimum: 0
      - name: "customer_status"
        logicalType: "string"
        physicalType: "TEXT"
        examples:
          - "active"
      - name: "last_updated_at"
        logicalType: "date"
        physicalType: "TIMESTAMP"
        description: "Timestamp of the last update to the customer record"
```

The schema defines the structure and semantics of your data. In our example that is a table with its columns, but the same structure describes the messages of a Kafka topic, files in an object store, or the payload of an API. Each column includes technical details, business semantics, and governance attributes. For example, that the annual premium is stored in cents as an integer, following the company's convention on how to handle monetary values. You can also define relationships like foreign keys between columns and tables within the data contract.

```
schema:
  - name: "customers"
    quality:
      - type: "sql"
        description: "Expect more than 100K rows"
        query: "select count(*) from customers"
        mustBeGreaterThan: 100000
    properties:
      - name: "customer_name"
        quality:
          - type: "sql"
            description: "Expect less than 1% empty name fields"
            query: |
              SELECT AVG(
                CASE
                  WHEN customer_name IS NULL
                    OR TRIM(customer_name) = ''
                  THEN 1
                  ELSE 0
                END
              )
              FROM customers
            mustBeLessThan: 0.01
      - name: "customer_status"
        quality:
          - type: "library"
            description: "Only valid customer statuses"
            metric: "invalidValues"
            arguments:
              validValues:
                - "active"
                - "lapsed"
                - "cancelled"
            mustBe: 0
```

Data quality rules ensure that data meets expectations. You can define checks like valid value constraints, row count thresholds, custom SQL validation logic, or even simply a quality rule in textual form. Thresholds can be absolute, like a minimum row count, or relative, like at most 1% empty names. Those checks can be automatically verified with the right tooling.

### Ownership and Support Channels

```
team:
  name: "customer-data"
  members:
    - username: "carola@example.com"
      name: "Carola Doe"
      role: "Owner"

support:
  - channel: "#customer-data-support"
    tool: "slack"
    url: "https://example.slack.com/archives/C123456789"
```

Ownership is central to data contracts. The team defines who owns and maintains the data contract, including its members with their roles. Add support channels to specify how data consumers can reach out to the owners. That reachable responsibility is critical in building trust in data.

### Terms of Use and SLAs

```
description:
  purpose: "Provides customer master data for analytics and reporting"
  usage: "Used by analytics team for customer insights and cross-sell prediction"
  limitations: "Contains only customers with an active policy"
  authoritativeDefinitions:
    - url: "https://example.com/policies/gdpr-compliance"
      type: "businessDefinition"
      description: "GDPR compliance policy for handling customer data"
```

The terms of use help data consumers understand what they can and cannot do with the data, establishing clear governance boundaries. The standard lets you define the purpose, usage guidelines, and limitations for your data, as well as point to external policies such as privacy policies or license agreements.

```
slaProperties:
  - property: "retention"
    value: 5
    unit: "year"
    description: "Data will be deleted after 5 years"
  - property: "freshness"
    value: 24
    unit: "hours"
    element: "customers.last_updated_at"
    description: "Within 24 hours of the last update"
```

Service Level Agreements define guarantees such as freshness of the data, data retention, or latency. Each one is stated so that it can be measured: the freshness promise above is checked against `last_updated_at` every time the contract is tested, and a breach alerts the owning team.

### Location of the Data

```
servers:
  - server: "production"
    type: "snowflake"
    environment: "prod"
    account: "insurance-corp"
    database: "prod_lifeinsurance"
    schema: "dp_customers_v1"
```

Servers specify where the data actually lives. They include the connection details to access the data, excluding any credentials. The standard supports specifying basically any system that can hold data directly: data platforms and databases such as Snowflake, Databricks, BigQuery, Oracle, and Postgres, event streams such as Kafka, Kinesis, or Pub/Sub, files on S3, or even CSV files on an SFTP server, and [many others](https://bitol-io.github.io/open-data-contract-standard/latest/infrastructure-servers/). It also provides the fallback to a custom server type if a system is not supported.

For an event stream, the server entry names the cluster and the message format, and the schema describes the topic:

```
servers:
  - server: "production"
    type: "kafka"
    environment: "prod"
    host: "kafka.example.com:9092"
    format: "avro"

schema:
  - name: "customer_events"
    physicalType: "topic"
```

### Roles & Security

```
roles:
  - role: "customers_reader"
    access: "read"
    description: "Read access to all customer data"
```

Roles describe how consumers get access to the data. Each role names an IAM role on the data platform, such as a Snowflake role, and the kind of access it grants. Providers often define several roles, for example one that includes personal data and one that doesn't. The contract doesn't list who holds a role; that is decided per consumer when an access request is approved, as we'll see in the section on data marketplaces.

### AI Context

```
context:
  instructions: >
    Use this data for analytics on life insurance customers.
    Annual premiums are stored in cents; divide by 100 before
    reporting amounts.
  constraints:
    - constraint: >
        Do not use this data to make decisions about individual
        customers, such as pricing a policy or assessing a claim.
  verifiedStatements:
    - question: "How many active customers do we have?"
      answer: |
        SELECT COUNT(*)
        FROM customers
        WHERE customer_status = 'active'
```

The AI context is guidance written specifically for AI agents. The instructions act like a system prompt for the dataset, for example telling an agent that premiums are stored in cents. The constraints state what an agent must not do with the data. And the verified statements pair common business questions with a query the provider has checked, so an agent can reuse a correct answer instead of guessing one.

### Pricing the Use of Data

The price can express an internal charge that reflects the value of the data. In practice, it is usually left empty or passes on what accessing the data actually costs on the platform, as in this example: 10 USD per terabyte scanned.

```
price:
  priceAmount: 10
  priceCurrency: "USD"
  priceUnit: "per TB scanned"
```

### Built-in Extensibility

As every company is different, the standard has two built-in mechanisms to extend it. Custom properties, available on every level, carry metadata the standard does not know, such as the legal entity that provides the data, or a reference to a platform manifest that drives infrastructure provisioning. Vendors use the same mechanism to add functionality on top. Authoritative definitions link the contract, or any element in it, to something defined elsewhere, including other open standards: in the example below, the OpenAPI specification of a REST API that serves the same data. Our advice remains: prefer the official standard properties, and keep custom ones to a minimum.

```
customProperties:
  - property: "legalEntity"
    value: "Insurance Corp AG"
    description: "Legal entity that owns and provides this data"

authoritativeDefinitions:
  - url: "https://example.com/apis/customers/openapi.yaml"
    type: "openapi"
    description: "OpenAPI specification of the REST API serving this data"
```

## Data Contracts in Context

Data contracts rarely stand alone, and their value grows with what they connect to. Three connections matter most in practice: link columns to your domain model instead of restating their meaning, put contracts on the ports of data products, and let the contract be the product page that consumers request access from.

### Linking Data Contracts to Ontologies

Especially when agents work with data and combine data from different sources, documenting the semantics of individual datasets is not enough. As pointed out in [Making Your Data Ready for Agentic AI](https://martinfowler.com/articles/making-data-ready-for-agentic-ai.html#context-layer), semantics need to be defined across data sources in an overarching semantic layer. Combining semantics, ontologies in particular, with data contracts solves two concrete problems:

- **Metadata de-duplication**: They reduce duplication for the data producer. A concept such as Customer ID can be defined once, including its examples, confidentiality and privacy classification, and description, and the data contracts simply link to that definition, inheriting the centrally defined characteristics rather than restating them.
- **Semantic consistency and interoperability**: They make data from two or more data contracts interoperable for the data consumer. By linking columns to a shared conceptual definition, a consumer can tell whether two similarly named columns actually mean the same thing, and whether two differently named ones do. This is consistency at the conceptual level, not merely a matter of matching primary and foreign keys, even though such a shared concept is exactly what makes a reliable join key.

![](https://martinfowler.com/articles/data-contracts/03-ontology-linking.svg)

Figure 2: Two data contracts link their properties to the same concept in an ontology, so a consumer knows how to join them

```
schema:
  - name: "customer_360"
    properties:
      - name: "cid"
        authoritativeDefinitions:
          - type: "semantics"
            url: "https://example.com/ns/Customer.id"
```

The core concept is to point from the data contract to an element in an ontology via an `authoritativeDefinitions` element. That way, the data contract maps into the ontology. The element simply needs to be identified by an IRI or URL within that ontology. The ontology itself can be described using standards like RDF, OWL, or using the promising [ontology standard](https://github.com/apache/ossie/blob/main/ontology/ontology.md) within the [Apache Ossie](https://ossie.apache.org/) project, currently incubating at the Apache Software Foundation.

### Data Products Build Upon Data Contracts

Data contracts go particularly well together with [data products](https://martinfowler.com/articles/designing-data-products.html), which bring product thinking to data, and are a central part of modern, decentralized data architectures such as [data mesh](https://martinfowler.com/articles/data-mesh-principles.html). Data contracts are the interface description language that makes the ports of a data product explicit and verifiable.

![](https://martinfowler.com/articles/data-contracts/02-data-product-ports.svg)

Figure 3: A data product with one input port and three output ports, each described by a data contract; two output ports offer different versions of the same contract

Since 2025, the family of Bitol standards also has a standard for describing the metadata for data products, the [Open Data Product Standard](https://github.com/bitol-io/open-data-product-standard) (ODPS), building upon the Open Data Contract Standard.

customers.odps.yaml, describing the Customers data product

```
  apiVersion: "v1.1.0"
  kind: "DataProduct"
  id: "customers"
  name: "Customers"
  status: "active"
  
  description:
    purpose: >
      Master data for all customers holding a life insurance
      policy. All customers since 2020-01-01.
    usage: "Used by analytics teams for customer insights and cross-sell prediction"
    limitations: "Contains only customers with an active policy"
  
  inputPorts:
    - name: "customer_events"
      version: "1.0.0"
      contractId: "kafka_customer_events"
  
  outputPorts:
    - name: "customers_pii_snowflake_v1"
      description: "All customer records, with PII."
      version: "1.0.0"
      contractId: "customers_pii_snowflake"
    - name: "customers_pii_snowflake_v2"
      description: "All customer records, with PII."
      version: "2.0.0"
      contractId: "customers_pii_snowflake"
    - name: "customers_npii_snowflake_v1"
      description: "All customer records, without PII data."
      version: "1.0.0"
      contractId: "customers_npii_snowflake"
```

In the ODPS example above, the ports of a data product refer to data contracts defined using ODCS. Together, the ODPS and ODCS files form the data product metadata. The data product is the whole thing: the code and the pipeline that produces the data, the data itself, and the metadata that describes it. This is the concept of a data product as an [architectural quantum](https://www.oreilly.com/library/view/building-evolutionary-architectures/9781491986356/ch04.html), a self-contained unit with input and output ports as ODCS-specified interfaces.

As the two standards are compatible with each other, we can move the information regarding ownership, such as teams and support channels, as well as information that applies to all data contracts, into the data product description, reducing duplication.

As useful as data products and ODPS are, the data contracts at the output ports remain the central element. They hold the promises the provider makes, and many parties work with them.

![](https://martinfowler.com/articles/data-contracts/06-contract-stakeholders.svg)

Figure 4: The data contract is central to at least four parties: provider, consumer, data governance, and data product platform

The provider publishes the data contract, and consumers rely on it. The data product platform reads it to know what to register, provision, and monitor. Data governance reads it to check that policies on ownership, classification, and terms of use are followed. And that is why, in the end, the data contract is also the product details page in a data marketplace.

### Data Contracts are the “Product Details Page” in a Data Marketplace

Most enterprises have some sort of data marketplace, where data becomes shoppable. There, data contracts contain all the information for data consumers to make an informed decision about whether the provided data is what they need for their intended purpose. When convinced, a consumer requests access to the data described by the contract, including a processing purpose that should be in line with the terms of use of the data contract. This request is the counterpart of a customer clicking “buy” on a product details page.

The request creates an **access agreement** consisting mainly of three parts: the requested data's data contract, the stated processing purpose, and the requesting consumer. The access agreement only becomes effective when the provider approves the request. Only then does the [data product platform grant the actual data access](https://martinfowler.com/articles/data-product-platform-nav.html#Level3ScalablePlatform-PlatformAsAProduct).

![](https://martinfowler.com/articles/data-contracts/04-access-agreement.svg)

Figure 5: An access agreement between the provider and an AI agent: the agent requests access, the provider approves, and the agent gets short-lived access to the data

Don't confuse the two. A data contract is published one-sidedly by the provider, refers to no specific consumer, and has no notion of time. An access agreement is bilateral, much closer to a contract in everyday life: the consumer creates it as a request, and it only becomes binding once the provider approves it, just as a contract binds only when both parties have signed. It always has a start date and, optionally, an expiration date; for AI agents, the granted access period can be as short as a few minutes. There is no standard for access agreements yet, but the Bitol project is working on one: an [RFC for an Open Access Agreement Standard](https://github.com/bitol-io/tsc/blob/main/rfcs/0056-oaas.md) (OAAS) proposes recording who may access which output port, for what purpose, and for how long.

A product page also shows reviews, not just the manufacturer's description. The equivalent for data is the contract's track record: with test results published to the marketplace, it can show next to each promise how well it was kept, for example that freshness was met on 29 of the last 30 days, or how long the team typically takes to fix a broken promise. Such trust signals, as described in [Building an “Amazon.com” for Your Data Products](https://www.thoughtworks.com/insights/blog/data-strategy/building-an-amazon-com-for-your-data-products), tell a consumer more than the promise alone.

## Working with Data Contracts

Writing a data contract is the easy part; living with it is where teams succeed or fail. Doing it well means [treating a data contract with the same rigor as an API](https://martinfowler.com/articles/making-data-ready-for-agentic-ai.html#WhereToStart): creating it deliberately, keeping it next to the code, evolving it without breaking consumers, and checking it automatically.

### Data-first vs. Contract-first

If data is already shared with other teams, for example as tables in a data warehouse, we can put it under contract to make the promises of the data provider explicit, ideally as the output port of a data product. For that, we need to represent the existing structure of the dataset in the data contract, and then add all the implicit promises iteratively. This can be kickstarted by using the open source tool [Data Contract CLI](https://cli.datacontract.com/) that can convert existing metadata describing the structure of the data such as SQL DDL, JSON Schema, or Avro Schema, and generate a first draft of a data contract, which is enriched afterwards.

Generation is only the starting point, though. Once the first draft has been enriched and published, the direction flips: the data contract is stable and becomes the source of truth, and the data has to follow it. If a column in the underlying table is renamed later, the contract is not regenerated. The contract test fails instead, and the rename has to be handled as what it is: a breaking change.

Another approach is to first start with a conversation on what should actually be built, leading to defining the data contract before any code is written. In our experience, a workshop format with a facilitator works very well, where the potential provider of the data and data consumers work together on a first draft of the data contract.

You can capture this draft in whatever medium suits the group:

- A whiteboard, or a YAML editor of your choice
- The [Data Product Canvas](https://www.datamesh-architecture.com/data-product-canvas), in which the data contract is a central part
- An [ODCS-compliant Excel template](https://github.com/datacontract/open-data-contract-standard-excel-template), the tool that business users work with every day
- The open source [Data Contract Editor](https://editor.datacontract.com/)

In the end, a standard-compliant data contract must emerge, which is refined iteratively until all parties are satisfied, and then used to start implementing the pipeline to actually provide the data according to the agreed data contract.

### Its Natural Habitat is a Git Repository

But regardless of how the data contract is created, its natural habitat is the Git repository alongside its data product metadata, the code that helps to produce the actual data, and the configuration of the CI/CD pipeline. The contracts typically are stored in a dedicated subfolder with the data product YAML being at the root of the repository.

File tree of the Git repository of the Customers data product

```
  customers.odps.yaml
  |- datacontracts
     |- customers_pii_snowflake_v1.odcs.yaml
     |- customers_pii_snowflake_v2.odcs.yaml
     |- customers_npii_snowflake_v1.odcs.yaml
  |- code
     |- pipeline.sql
  .gitlab-ci.yml
```

Our advice is to not place them far away from the pipeline code, as the code for the pipeline typically has to change together with the data contract. And things that change together should be put together.

You might have noticed the naming convention for the filename already. It starts with the human-readable ID of the data contract, which names the dataset (`customers_pii` or `customers_npii`) and the technology (`snowflake`) where the data is available for the consumer, followed by the major version (`v1` or `v2`). The major version is part of the filename, but not of the ID: the ID stays the same across major versions, so they can live side by side as separate files. This is best practice as a data product can offer its data in different forms, versions, and on different technologies. Different environments such as test, staging, and production, are best encoded within the data contract through separate server entries.

### The Story of Versioning and Breaking Changes

One of the main goals of data contracts is to find breaking changes before consumers are affected, and offer graceful evolution processes. For that, data contracts follow the [SemVer](https://semver.org/) versioning scheme.

- Major Version Change: Breaking change such as removing a column or renaming it
- Minor Version Change: Non-breaking change such as adding a new column
- Patch Version Change: Small change such as fixing a typo or updating the support channels

Breaking changes increase the major version, whereas non-breaking changes or small fixes increase the minor or patch version. The major version is tied to the dataset it protects. In case of a major, breaking change, the data contract should be provided in an incremented major version alongside a new dataset. This allows phasing in a new major version next to an existing one, and after some grace period, phasing out the old major version. During that grace period, the status signals to the consumer whether it is being phased in, active, or being phased out. The data contract typically begins its life in the draft status, phasing in as being in development, and marked as active if the promises are now actually binding. The same holds for phasing it out, marking it as deprecated and then retired. The lifecycle is heavily intertwined with the versioning. When contracts are managed as part of a data product, this lifecycle often follows the data product's release process instead, with each major version offered as its own output port running in parallel.

For event streams, this maps onto the compatibility modes of a schema registry. A minor version has to be forward compatible, so existing consumers keep reading new events with the schema they already have, and ideally fully compatible, so new consumers can also replay old events. Beware of the registry default: backward compatibility permits deleting a field, which is exactly the change that breaks existing consumers. Anything the registry would reject is a major version, published as a new topic.

### Automate All The Things

A data contract that nobody checks is a paper tiger: its promises are worth no more than a wiki page, and the first time a consumer finds one broken, their trust is gone. The real value comes with automation on top of it, and the potential is huge:

- Generate HTML documentation and make it centrally available in your intranet
- Publish to metadata repositories such as data product marketplaces or data catalogs
- Test the data in your test environment against the contract within your CI/CD pipeline in your pull request and prevent a merge in case of failure
- Test the data in your production environment against the contract every day and inform the owner and any consumer downstream in case of broken promises
- Generate the SQL DDL to create the tables from the data contract, making the tables contract-compliant by design as the data provider
- Derive masking and access policies on the data platform from the contract, for example masking every column tagged `pii:true` for roles without access to personal data
- Generate the SQL query to actually use the data as a tolerant data consumer without the select star operator
- Compare the new version of a data contract with its old one in a pull request and fail if there is a breaking change for only a minor or patch version update
- Validate new data against the contract before it reaches consumers, and hold back data that violates it, as in the [quarantine pattern](https://martinfowler.com/articles/making-data-ready-for-agentic-ai.html#TheQuarantinePattern)

The open source tool [Data Contract CLI](https://cli.datacontract.com/), with over 1 million downloads per month, natively understands data contracts in the ODCS format, and is useful to automate all of these listed tasks. It is rated *Trial* in the [Thoughtworks Technology Radar](https://www.thoughtworks.com/en-de/radar/tools/data-contract-cli) (November 2025), and found especially useful for validating data against its contract, detecting breaking changes, and operationalizing data governance through the CI/CD pipeline.

### Data Contracts Belong in Your CI/CD Pipeline

Data contracts belong in your CI/CD pipeline, where every commit is checked before it ships. The [Data Contract CLI](https://cli.datacontract.com/) offers a command for each stage: `datacontract lint` validates the contract, `datacontract breaking` catches breaking changes against the previous version, and `datacontract ci` tests the data against the contract in each environment. Only when all stages pass does the pipeline deploy to production and publish the contract to the marketplace with `datacontract publish`.

![](https://martinfowler.com/articles/data-contracts/05-cicd-pipeline.svg)

Figure 6: Data contracts in the CI/CD pipeline: checked on every commit, and tested against production data every night

A second, scheduled pipeline keeps watch afterwards, running `datacontract test --server prod` on a cron and publishing the results to inform the data providers and affected consumers the moment a promise breaks.

## The Costs and Benefits of Data Contracts

Data contracts pay back in trust and automation, but they are not free, and they are not worth writing for every dataset. Agents amplify both sides of this trade-off.

### The Benefits

When a large part of an enterprise's analytical data is described by data contracts, the data can be tested automatically against the promises in the contract. Its quality can be monitored continuously, and when it falls below a promised threshold, counter-actions can be triggered automatically.

At least as important, because consumers' expectations are written down, changes that break them can be detected automatically, for instance to stop a deployment in a CI/CD pipeline. Breaking changes no longer propagate through pipelines into downstream services, which makes the whole system far more robust.

Overall, data contracts shift ownership towards data providers. That builds trust between providers and consumers, between business and IT, and, increasingly, between humans and the agents working on their behalf.

Each of these benefits grows when the consumer is an agent, because an agent reads the contract literally and acts on it at machine speed. A contract tells an agent what it needs to act safely and correctly: the schema to query the data right, the semantics to combine it with other datasets, the quality guarantees to trust it, and the terms of use to know what it is allowed to do.

Two capabilities only become practical this way: generating your data pipeline, and chatting with your data.

### Generate Your Data Pipeline

Data contracts enable [spec-driven development](https://martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html). The data contract acts as the core of the specification: fed into a coding agent, alongside a plugin containing skills that describe how data pipelines are built in your company, the agent can generate almost the entire data pipeline automatically. An [example run of such a generation is available on dataproduct.com](https://dataproduct.com/). During the generation of the data pipeline, the data can be tested against the contract, acting as a feedback loop, steering the agent in the right direction.

![](https://martinfowler.com/articles/data-contracts/07-generate-pipeline.svg)

Figure 7: A coding agent generates the data pipeline from the data contract, skills, and the data marketplace

### Chat With Your Data

Business users want to ask questions like “Why is our revenue up this quarter?” and get an answer within minutes. With “chat with your data”, the hard part isn't generating SQL, it is generating the correct SQL. Point your agent at the database with its hundreds of tables and thousands of columns, and it has to guess the semantics of the columns, and how to join the tables together correctly. And we haven't even talked about whether it is allowed to answer the question with this data.

But before an agent can query data correctly, it has to find it. Published to a data marketplace that agents can search, for example through MCP, data contracts make data discoverable in the first place. Once found, a data contract stops the guessing. It tells the agent the meaning of the columns, and through the semantic links that form the context layer, how to join datasets correctly. Its terms of use make it clear what is allowed to be done with the data, and what usages are not. With that, “chat with your data” becomes something you can rely on.

### The Cost

None of this comes for free. Data contracts take time and effort to write and maintain, and their owners have to keep the promises in them. Agents raise the stakes here too: an agent acts on an outdated contract just as confidently as on a correct one.

That's why you should not introduce data contracts for all the data in your company. It may sound like generic consultant advice, but it matters: focus on the interfaces where one team shares data with another, ideally via data products.

Also, don't roll out data contracts all at once. That feels like bureaucracy if a contract does not immediately solve an actual problem. Start where it hurts: data quality issues, broken pipelines, or a lot of miscommunication. Or, when a new use case is being built, specify its interfaces with data contracts right from the start.

Moreover, introducing data contracts is a cultural change towards ownership of data, with explicit and verifiable promises. The standard and tooling help, but teams have to learn that ownership also means being responsible when something breaks. Making things explicit can also reveal missing ownership, which then has to be fixed in the organization first.

Providers are usually measured on shipping features, so a contract can feel like friction with little payoff. A few things help: platform templates that make writing and testing a contract cheap, workshops with consumers to agree on the service levels they actually need, showing each data product's reliability in the marketplace so that good ownership gets noticed, and making clear who owns the alert when a promise breaks.

The cost is real, but data contracts offer a high payoff through establishing trust, making automation possible, and empowering agents. When introduced with care, they pay off sooner rather than later.

## Why Data Contracts Will Be Everywhere

With ODCS, data contracts have a de facto industry standard, governed at the Linux Foundation, and open source tooling around it. We believe that soon data contracts will be for data what OpenAPI is for REST APIs today, for three reasons: agents finally make it worthwhile to write them well, consumers will start writing their own, and an open standard keeps them portable.

### In the Age of Agents, the Incentive Points the Right Way

Data contracts are valuable even without AI agents. But the effort goes into the parts that can be automated. Schema and quality rules are filled out with care; the prose fields such as business names, descriptions, and terms of use are not filled out with the same rigor, as they are only read by other humans. Sad, but true.

Agents change that. An agent takes those prose fields at their word and acts on them. A vague description leads it to build the wrong pipeline, a missing “terms of use” leads it to use the data in ways it shouldn't, and an ambiguous purpose leads to a confidently wrong answer. Suddenly, the neglected half of the contract has real consequences, and the incentive finally points the right way. The standard is already following: ODCS 3.2 added the AI context, a block written for agents.

Metadata used to be a chore done for other people, so it languished at the bottom of the backlog. Now it pays you back: ask a question, see a wrong answer, fix the description or the terms of use, ask again. Few people write documentation for someone else's benefit, but when the benefit is their own, the metadata gets better. This is the forcing function: agents finally make it worthwhile to write the whole contract well, which is exactly why, in the age of agents, data contracts will flourish.

### [Consumer-Driven Data Contracts](https://doi.org/10.1109/icws62655.2024.00019) Add the Missing Perspective

So far, we have looked at data contracts published by providers so that consumers can trust the data. But trust also has to run the other way, for example when sharing sensitive data, which is what the terms of use are for.

Applying the principle of [consumer-driven contracts](https://martinfowler.com/articles/consumerDrivenContracts.html) to data contracts goes one step further: a consumer can publish their own data contract, specifying their [promises and expectations](https://doi.org/10.1109/icws72778.2026.00084) in the same structured way that we looked at the provider side so far. In fact, we can use the exact same ODCS-defined structure to describe a consumer-driven contract, because it is just the direction that changes: whereas for instance a schema-definition in the provider-driven contract states a promise, in the consumer-driven contract, it states an expectation.

This also allows contract-first from the consumer side: a consumer specifies the data they need without knowing who could provide it, and the specification is used to find matching data, which becomes especially powerful when an AI agent does the search. Consumer-driven contracts also document which data is actually used compared to what access was granted, which helps providers optimize their service and automate governance.

### Open Standards Enable Metadata Sovereignty

Metadata is tiny in volume but strategically central: it is the map to all your valuable data and the foundation every catalog, quality check, lineage tool, and AI agent builds upon. That is exactly why large vendors are happy to give their metadata solution away for free. It is not out of generosity, but because once your metadata lives in their proprietary format, everything reading it is locked in too. Since metadata is so small, there is no storage or cost reason to keep it proprietary. Doing so is purely a play for control.

An open standard is the only real defense. It keeps your metadata portable, lets you combine best-of-breed tools from different vendors, and puts the format under vendor-neutral governance. ODCS lives at the Linux Foundation, so no single company can change the rules. Pioneering companies had to build custom formats and tooling from scratch, while recent adopters build on ODCS and its open ecosystem at a fraction of the effort. A simple test of whether you are locked in: can you export all your contracts as plain files into git and rebuild your catalog elsewhere from them alone? And as agents become the primary consumers of metadata, whoever controls its format controls access to your enterprise data. OpenAPI has already shown the alternative for operational systems: it is itself an open metadata standard, describing REST APIs in a vendor-neutral format that any gateway, generator, or documentation tool can read. ODCS brings the same to data. Those who embrace [open standards](https://data-landscape.com/) keep sovereignty over their metadata.
