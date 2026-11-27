---
title: 'Beyond API Compatibility: Building a Tool to Measure the Blast Radius of Library Changes'
link: https://netflixtechblog.com/beyond-api-compatibility-building-a-tool-to-measure-the-blast-radius-of-library-changes-9474f2fd4f90?source=rss----2615bd06b42e---4
source: netflixtechblog-com
published: 2026-10-23T18:32:51Z
updated: 2026-10-23T18:32:52.970Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Netflix Technology Blog
labels:
- api-design
- code-analysis
- developer-tools
- java
- software-engineering
content: feed
html: 2026-10-23-beyond-api-compatibility-building-a-tool-to-measure-the.html
remote_preview:
  url: https://cdn-images-1.medium.com/max/1024/1*K06WZmnRhhXBboIskWb8qg.png
---

By [Roberto Perez Alcolea](https://www.linkedin.com/in/robertoperezalcolea/), Java Platform Team

![](https://cdn-images-1.medium.com/max/1024/1*K06WZmnRhhXBboIskWb8qg.png)\
Quasar connects a proposed API change with observed references in consumer repositories, giving the producer evidence for a release decision

At Netflix, shared Java libraries support applications developed across a large, independently evolving repository ecosystem. A library change begins in one repository, but its impact can reach many others. Producers can see the API diff in front of them. What they often cannot see is who still depends on the symbols that changed.

The need for that visibility grows as coding agents take on more of the software development lifecycle. An agent can accelerate a local edit, but without cross-repository context it cannot judge downstream risk or help plan consumer migrations. The evidence a producer uses to assess a change can also guide the agent working on it.

Consider a common case: removing a deprecated API. Deprecated entry points expand the surface producers must test, document, and support, and they can constrain how a library evolves.

Suppose a replacement has been available for several releases and the old method cannot express options required by new behavior. The library’s tests pass, and a compatibility checker flags the removal. The producer still cannot answer the questions that determine what to do next:

- Does any consumer still call this method?
- Are consumers concentrated on one version or spread across several generations?
- Does another library implement or wrap the API, multiplying its impact?
- Is direct coordination sufficient, or does this require a deprecation and migration campaign?

At small scale, producers can ask nearby teams or search a handful of repositories. Across many repositories, it is harder to find every caller of a method and the library version each one uses. Those answers are scattered across consumer codebases and builds.

Without that evidence, a producer may release a breaking change and discover the impact through consumer failures, or preserve an old API indefinitely because the risk is unknown. Libraries need to evolve, and their producers need evidence that helps them do so intentionally.

We built **Quasar** to index symbol definitions and references across Netflix repositories. For library producers, that shared index helps answer two questions:

1. **What changed in the library’s public API?**
2. **Which repositories use the exact symbols that changed?**

The first identifies a potentially incompatible change. The second estimates its observed blast radius. Together, they turn a generic compatibility warning into evidence a producer can act on before merging or releasing.

### From API Changes to Consumer Impact

Existing techniques each see a different part of the problem. Binary and source compatibility tools identify contract changes, but cannot tell who uses the changed API. Dependency graphs show declared reach, but cannot prove that code calls a particular method. Text search may miss overloads, inheritance, or the artifact version behind a call. Consumer builds provide strong validation, but running them across the fleet is expensive.

Quasar builds on [SCIP](https://github.com/sourcegraph/scip), an open interchange format for semantic code intelligence. Language-specific indexers encode definitions, references, and relationships using stable symbol identifiers. Quasar retains API definitions for library versions and commits alongside references from consumer repositories. That shared identity lets it connect an API change in one repository to exact usages elsewhere, even when those repositories were indexed independently.

![](https://cdn-images-1.medium.com/max/1024/1*5c7ImyYuNCfO7Tv_ma0EYg.png)

On each enrolled CI build, language-specific indexers submit SCIP indexes to Quasar’s ingestion service. It writes API definitions and consumer references as Parquet in Amazon S3, retaining the symbol identity, dependency version, and source location needed to investigate a change.

During pull-request review, Quasar compares indexed base and head definitions and counts direct consumer references with Amazon Athena. Quasar exposes those findings through its REST and GraphQL APIs and MCP server; follow-up usage queries provide dependency-version breakdowns and source locations. A separate compatibility-impact assessment can also follow implementations and their callers when a producer supplies a changed JVM symbol.

Consider this simplified API change:

```
public interface Client {
- Result getLegacyValue(Request request);
+ Result getValue(Request request, Options options);
}
```

For a detailed compatibility-impact assessment of this removal, a producer-side tool supplies the old method descriptor. Quasar constructs its canonical symbol identity. This abridged excerpt from the direct-reference portion of Quasar’s Amazon Athena query shows the essential match:

```
SELECT c.change_id, 'DIRECT_REFERENCE' AS evidence_kind,
       r.scm_type, r.org, r.repo, r.dep_version, r.file_path
FROM requested_changes c
JOIN quasar.symbol_refs r
  ON r.symbol_canonical = c.target_canonical
```

requested\_changes contains the API changes submitted by the producer. The SQL excerpt finds repositories that call the removed method. Quasar also checks implementations and calls through them, so it can count each affected repository once. It returns sample dependency versions and file locations to help investigate the result.

For the Client change, this tells the producer which repositories still use getLegacyValue, which versions they use, and where some of those calls occur. That helps the producer decide whether to coordinate a migration before removing the method.

### Bringing the Analysis Before Release

Version-to-version comparison is valuable when planning an upgrade or writing a migration guide. Producers benefit even more when feedback arrives during code review.

For pull-request analysis, Quasar stores API definition snapshots keyed by commit. Given a base and head commit, it classifies symbols as added, removed, or signature-modified. Removals and modifications then become candidates for consumer-impact analysis.

An incompatible change is not automatically a high-risk change. Quasar reports the observed impact so the producer can decide what to do; it does not enforce a release policy. “No references observed” means only that Quasar found none in the indexed population. Reflection, configuration, dynamic loading, stale branches, or incomplete indexing can hide consumers. The result needs to show whether the requested commits were indexed, how fresh the consumer indexes are, and whether evidence was truncated.

Pull-request review requires indexed snapshots for the exact base and head commits. If either is unavailable, Quasar reports that it cannot compare them; it does not substitute a default-branch snapshot. This keeps a missing comparison from looking like a clean API diff.

Return to our Client change. This illustrative report brings together signals available through Quasar's API comparison, consumer-usage queries, and library-level criticality. It is not a literal response from one API:

```
{
  "change": "REMOVED",
  "symbol": "Client#getLegacyValue(Request)",
  "affectedRepositories": 137,
  "versionDistribution": [
    { "version": "4.2.1", "repositories": 86 },
    { "version": "4.1.7", "repositories": 38 },
    { "version": "3.9.4", "repositories": 13 }
  ],
  "sampleConsumerLocations": [
    {
      "repository": "checkout-service",
      "filePath": "src/main/java/com/example/checkout/CheckoutClient.java",
      "lines": [84, 129]
    },
    {
      "repository": "client-wrapper",
      "filePath": "src/main/java/com/example/client/ClientAdapter.java",
      "lines": [57]
    }
  ],
  "libraryCriticality": {
    "tier": 1,
    "basis": "highest-tier downstream application"
  },
  "comparison": {
    "baseShaFound": true,
    "headShaFound": true
  }
}
```

![](https://cdn-images-1.medium.com/max/1024/1*QLVdKPyYkqJcNgt0rVN39Q.png)\
*An impact report shows 137 affected repositories distributed across library versions 4.2.1, 4.1.7, and 3.9.4.*

The repository count is symbol-specific: in this example, 137 indexed repositories refer to the removed method. The version distribution reveals a dominant cohort on 4.2.1 and a smaller long tail still on 3.9.4. A producer can inspect affected repositories and retrieve indexed file paths, with line numbers where the index provides them. The JSON shows only two sample locations to keep the example readable. The filePath and lines shape mirrors Quasar's query contract. The tier is broader context about the library, inherited from its highest-criticality downstream application; it does not imply that all 137 callers are Tier 1.

Together, those signals change the producer’s plan. Instead of merging the removal and waiting for failures, the producer can retain and deprecate the old method, publish a migration path, coordinate a broader migration, and track the long-tail versions before removal. File paths and available line numbers turn that scope into a concrete work list for engineers or coding agents. The same API diff paired with weak or stale evidence would lead to a different action: refresh the indexes before deciding. The value is not merely detecting the removal; it is choosing a proportionate next step.

![](https://cdn-images-1.medium.com/max/1024/1*o08l-rlQ0D2oo1fBpH5ICw.png)\
A pre-release workflow enriches API changes with consumer impact and evidence quality before the producer decides how to proceed

### A Polyglot Foundation with SCIP

The shared [SCIP](https://github.com/sourcegraph/scip) format also lets Quasar index several language ecosystems. Each language indexer resolves its own code, while Quasar can use the resulting symbol identities to find references across repositories.

Conceptually, client-lib indexes Client#getLegacyValue(Request) as a definition, while checkout-service indexes a reference to the same symbol. Quasar can then connect the library API to its consumer across repositories.

![](https://cdn-images-1.medium.com/max/1024/1*RMR331Wf7c4Xx3GTD5_7Rw.png)\
*Language-specific indexers emit SCIP definitions, references, and relationships into Quasar’s shared semantic graph.*

Relationships are especially useful when generated code separates the API a producer owns from the symbols consumers invoke. For example, Quasar can trace a Protobuf definition through its generated Java members to the repositories that use them, even when consumer code never names the .proto element.

SCIP records symbols and their references; it does not determine whether an API change violates an ecosystem’s compatibility rules. Today, Quasar compares indexed JVM and Protobuf API definitions and traces Protobuf changes through generated Java to find affected repositories. A separate JVM compatibility-impact path uses breaks identified by a producer-side tool to query affected repositories. Go, Python, JavaScript/TypeScript, and Rust support usage queries, but Quasar does not yet assess their breaking API changes.

### Moving Risk Assessment Earlier

Today, Quasar surfaces API-change findings on pull requests, including observed consumer counts and confidence. Producers can use its other queries to inspect dependency versions and source locations, with library criticality as broader context. Together, those signals support a decision before merge.

We want to move that context earlier. An agent writing library code should recognize when it is changing a public symbol and consult Quasar before the change becomes a pull request. Quasar’s APIs and [Model Context Protocol](https://modelcontextprotocol.io/) (MCP) integration provide an access path, but access alone does not create awareness. The coding workflow must also recognize public API edits so the agent knows when to query impact and when to involve the producer.

![](https://cdn-images-1.medium.com/max/1024/1*3VqsUQ4rQ3tl5VUPw9ESUQ.png)\
*Today Quasar assesses risk on a pull request. The future workflow gives agents consumer-impact context while they write code, before the pull request is opened.*

Earlier guidance will not replace pull-request analysis. The PR assessment will still verify the concrete diff and catch changes that were not recognized during coding. Coverage, freshness, ambiguity, unsupported platforms, and truncation must travel through both checks in machine-readable form. An agent should not turn “no observed usages in the available index” into “safe to remove.”

Today, agents can query Quasar through MCP when prompted. We are working toward making that context part of the coding workflow, then using identified call sites to prepare migration changes. Quasar supplies semantic evidence, an agent reduces migration toil, and the library producer remains responsible for API and release decisions.

### What We Learned

Building this workflow led us to four practical lessons.

- **Separate compatibility from reach.** A removed method can break the API contract even when Quasar finds no callers. A list of affected repositories and their library versions shows the migration work ahead. Producers need both signals to choose a release path.
- **Make the evidence inspectable.** A repository count is more useful with sample call sites and a version breakdown. The result also needs to show which commits were indexed, how fresh the consumer indexes are, and whether evidence was truncated. Otherwise, “no usage found” can be mistaken for proof that nobody is affected.
- **Keep usage and compatibility separate across languages.** SCIP lets Quasar find symbol references in several ecosystems. Deciding whether an API change is breaking requires compatibility rules or a trusted producer-side signal for that ecosystem.
- **Bring evidence to the producer while choices are open.** Pull-request findings can prompt a producer to retain an API, publish a migration path, or coordinate consumers before merge. Moving the same evidence into the coding workflow could shape the edit earlier. After release, it still helps locate consumers that need to migrate.

Today, Quasar indexes JVM, Go, Python, JavaScript/TypeScript, and Rust code for cross-repository usage analysis. It compares JVM APIs and traces Protobuf changes through generated Java. Producers can review change findings through Quasar’s REST and GraphQL APIs or its MCP server, then investigate affected repositories and call sites. We are extending this foundation toward more ecosystem-specific compatibility checks, deeper code-review integration, and agent-assisted consumer migrations that keep release approval with the producer.

Quasar is internal to Netflix, but the pattern is portable: index code semantically, retain API definitions, compare them at the point of change, query affected consumers, and report the quality of the evidence. Library producers need more than a compatibility verdict. They need to know who may be affected and have enough time to act before release.

A huge thank you to the Java Platform team at Netflix. Their expertise, collaboration, and support made this work possible

### Related Talks

- [From Lag to Lightning: Transforming Dependency Update Timelines](https://dpe.org/sessions/roberto-perez-alcolea-aubrey-chipman/from-lag-to-lightning-transforming-dependency-update-timelines/) explores how Netflix shortened dependency-update timelines.
- [Netflix’s Journey to Confident Automated Code Changes](https://moderne.ai/code-remix-sessions/netflix-journey-automated-code-changes-crs-2026) discusses building confidence in automated changes at scale.

![](https://medium.com/_/stat?event=post.clientViewed&referrerSource=full_rss&postId=9474f2fd4f90)

* * *

[Beyond API Compatibility: Building a Tool to Measure the Blast Radius of Library Changes](https://netflixtechblog.com/beyond-api-compatibility-building-a-tool-to-measure-the-blast-radius-of-library-changes-9474f2fd4f90) was originally published in [Netflix TechBlog](https://netflixtechblog.com) on Medium, where people are continuing the conversation by highlighting and responding to this story.
