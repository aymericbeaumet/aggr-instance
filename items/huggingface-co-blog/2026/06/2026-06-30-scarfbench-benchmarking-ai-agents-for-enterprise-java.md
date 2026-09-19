---
title: 'ScarfBench: Benchmarking AI Agents for Enterprise Java Framework Migration'
link: https://huggingface.co/blog/ibm-research/scarfbench
source: huggingface-co-blog
published: 2026-06-30T18:32:50Z
updated: 2026-06-30T18:32:50Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2026-06-30-scarfbench-benchmarking-ai-agents-for-enterprise-java.html
preview:
  file: 2026-06-30-scarfbench-benchmarking-ai-agents-for-enterprise-java.preview-0e9339950864.webp
  width: 256
  height: 166
  color: '#ece9e9'
images:
- source: https://cdn-uploads.huggingface.co/production/uploads/649c1276a83f996b4191a8f1/iYCwB2Hl8qPTA8DCgDilB.png
  original:
    file: 2026-06-30-scarfbench-benchmarking-ai-agents-for-enterprise-java.image-1bb8081485c4.png
    width: 1692
    height: 1096
  color: '#fefefe'
- source: https://cdn-uploads.huggingface.co/production/uploads/649c1276a83f996b4191a8f1/3LDxVMx4nf_WEqEN2AQ7W.png
  original:
    file: 2026-06-30-scarfbench-benchmarking-ai-agents-for-enterprise-java.image-ed0a251381d7.png
    width: 4038
    height: 3812
  color: '#fbfbfb'
- source: https://cdn-uploads.huggingface.co/production/uploads/649c1276a83f996b4191a8f1/5njqb4qTj7sfutgRJ7QrF.png
  original:
    file: 2026-06-30-scarfbench-benchmarking-ai-agents-for-enterprise-java.image-9c085a2be6c2.png
    width: 5115
    height: 2967
  color: '#e6e7e7'
- source: https://cdn-uploads.huggingface.co/production/uploads/649c1276a83f996b4191a8f1/MrcQ5trSi8oKtfD2UEhm9.png
  original:
    file: 2026-06-30-scarfbench-benchmarking-ai-agents-for-enterprise-java.image-da765a01cba4.png
    width: 2570
    height: 1130
  color: '#f7f9fb'
- source: https://cdn-uploads.huggingface.co/production/uploads/649c1276a83f996b4191a8f1/j7rihRrFe-Eo9oSxNFQS1.png
  original:
    file: 2026-06-30-scarfbench-benchmarking-ai-agents-for-enterprise-java.image-024a14d07661.png
    width: 8936
    height: 6480
  color: '#fcfcfc'
- source: https://cdn-uploads.huggingface.co/production/uploads/649c1276a83f996b4191a8f1/mQhC1EtAb5dx2Q8d5nqmL.png
  original:
    file: 2026-06-30-scarfbench-benchmarking-ai-agents-for-enterprise-java.image-6803a3118aa1.png
    width: 6401
    height: 3199
  color: '#fbfbfa'
- source: https://cdn-uploads.huggingface.co/production/uploads/649c1276a83f996b4191a8f1/BtWLPGsif0O4VoQfVppIC.png
  original:
    file: 2026-06-30-scarfbench-benchmarking-ai-agents-for-enterprise-java.image-1baf02b7a358.png
    width: 1542
    height: 720
  color: '#fcfcfc'
---

[Star ScarfBench on GitHub](https://github.com/scarfbench/benchmark)

Modernizing enterprise applications is one of the largest and most expensive software engineering activities organizations undertake. Teams migrate applications across frameworks to improve maintainability, cloud readiness, developer productivity, and access to modern capabilities.

Recent advances in coding agents have sparked excitement around AI-assisted modernization. But an important question remains:

**Can AI agents reliably modernize real-world enterprise applications?**

Existing software engineering benchmarks have demonstrated impressive progress in bug fixing and code generation, but framework migration presents a fundamentally different challenge. Success requires not only translating code, but also preserving behavior, adapting build systems, and navigating runtime dependencies.

To address this gap, we introduce **ScarfBench (Self-Contained Application Refactoring Benchmark)**, an open benchmark for evaluating AI agents on cross-framework migration tasks in Enterprise Java.

ScarfBench focuses on migrations across three major Java ecosystems:

- Spring
- Jakarta EE
- Quarkus

Unlike traditional benchmarks that compare generated code against reference implementations, ScarfBench evaluates whether migrated applications actually build, deploy, and preserve behavior.

## [](https://huggingface.co/blog/ibm-research/scarfbench#why-migration-is-hard) Why Migration Is Hard

Framework migration is much more than replacing annotations.

A simple repository migration can require changes across dependency injection, persistence configuration, queries, and framework descriptors. Small mistakes in any of these pieces can prevent successful deployment.

[![scarf-intro-anatomy](https://cdn-uploads.huggingface.co/production/uploads/649c1276a83f996b4191a8f1/3LDxVMx4nf_WEqEN2AQ7W.png)](https://cdn-uploads.huggingface.co/production/uploads/649c1276a83f996b4191a8f1/3LDxVMx4nf_WEqEN2AQ7W.png)

**Figure: Spring → Jakarta Migration Example**

Framework migration requires translating framework semantics, not just source code.

## [](https://huggingface.co/blog/ibm-research/scarfbench#introducing-scarfbench) Introducing ScarfBench

[ScarfBench](https://huggingface.co/spaces/ibm-research/ScarfBench) provides a systematic way to evaluate AI agents on enterprise Java framework migration tasks.

Applications are required to:

1. Build successfully.
2. Deploy correctly.
3. Pass behavioral validation.

This provides a much more realistic measure of modernization quality.

## Benchmark at a Glance

| Metric                    | Value   |
| ------------------------- | ------- |
| Applications              | 34      |
| Framework implementations | 102     |
| Migration tasks           | 204     |
| Lines of code             | \~151K  |
| Source and test files     | \~2,000 |
| Expert-written tests      | 1,331   |

ScarfBench includes both focused migration tasks and whole-application migrations.

[![scarf-intro-fig](https://cdn-uploads.huggingface.co/production/uploads/649c1276a83f996b4191a8f1/5njqb4qTj7sfutgRJ7QrF.png)](https://cdn-uploads.huggingface.co/production/uploads/649c1276a83f996b4191a8f1/5njqb4qTj7sfutgRJ7QrF.png)

**Figure: ScarfBench Construction Pipeline**

Starting from a JSR-based enterprise Java taxonomy, expert migrations create verified implementations across Spring, Jakarta EE, and Quarkus.

## [](https://huggingface.co/blog/ibm-research/scarfbench#how-do-frontier-agents-perform) How Do Frontier Agents Perform?

We evaluated several state-of-the-art coding agents on ScarfBench.

Despite strong performance on traditional software engineering benchmarks, framework migration remains difficult. Success rates vary considerably across framework pairs and whole-application migrations remain particularly challenging.

[![leaderboard](https://cdn-uploads.huggingface.co/production/uploads/649c1276a83f996b4191a8f1/MrcQ5trSi8oKtfD2UEhm9.png)](https://cdn-uploads.huggingface.co/production/uploads/649c1276a83f996b4191a8f1/MrcQ5trSi8oKtfD2UEhm9.png)

**Figure: Current Leaderboard**

Even the strongest current agents achieve less than 10% behavioral success, illustrating the gap between generating compilable code and preserving application behavior.

[![scarf_aggregate_progression](https://cdn-uploads.huggingface.co/production/uploads/649c1276a83f996b4191a8f1/j7rihRrFe-Eo9oSxNFQS1.png)](https://cdn-uploads.huggingface.co/production/uploads/649c1276a83f996b4191a8f1/j7rihRrFe-Eo9oSxNFQS1.png)

**Figure: Compile → Deploy → Test Progression**

Compile success consistently exceeds deploy success, which in turn exceeds behavioral success. Build success alone significantly overestimates migration quality.

[![sankey](https://cdn-uploads.huggingface.co/production/uploads/649c1276a83f996b4191a8f1/mQhC1EtAb5dx2Q8d5nqmL.png)](https://cdn-uploads.huggingface.co/production/uploads/649c1276a83f996b4191a8f1/mQhC1EtAb5dx2Q8d5nqmL.png)

**Figure: Migration Outcomes by Target Framework**

Migration difficulty depends strongly on the target framework, with Jakarta EE proving particularly challenging.

## [](https://huggingface.co/blog/ibm-research/scarfbench#what-we-learned-about-ai-agents-for-java-modernization) What We Learned About AI Agents for Java Modernization

Beyond measuring success rates, ScarfBench helps us understand how agents behave during modernization.

### [](https://huggingface.co/blog/ibm-research/scarfbench#can-agents-reliably-tell-when-a-migration-is-complete) Can Agents Reliably Tell When a Migration Is Complete?

A migrated application is only useful if it actually builds and runs.

We therefore compared agent-reported outcomes against independent build verification.

#### [](https://huggingface.co/blog/ibm-research/scarfbench#finding-agents-are-overconfident) Finding: Agents Are Overconfident

Claude Code reported successful builds for 29 out of 30 whole applications.

Only 22 of those applications actually built successfully.

Meanwhile, the single application classified as failed by the agent ultimately built correctly.

This suggests that agent self-assessment should not be treated as a reliable signal of migration completion.

Independent build and test validation remains essential.

### [](https://huggingface.co/blog/ibm-research/scarfbench#how-do-agents-navigate-application-dependencies) How Do Agents Navigate Application Dependencies?

Framework migrations rarely affect a single file or layer.

Changes in configuration, services, databases, and web components often cascade across the application.

#### [](https://huggingface.co/blog/ibm-research/scarfbench#finding-migration-is-iterative-rather-than-linear) Finding: Migration Is Iterative Rather Than Linear

The most frequently visited layers were:

- Configuration
- Web
- Database
- Service

Common transitions included:

- Configuration ↔ Web
- Service ↔ Database

This suggests that migration is an iterative dependency-resolution process rather than a simple source-to-source transformation.

### [](https://huggingface.co/blog/ibm-research/scarfbench#where-do-agents-spend-most-of-their-effort) Where Do Agents Spend Most of Their Effort?

We used layer revisit frequency as a proxy for migration effort. Layers that required repeated visits typically involved debugging, dependency resolution, or framework adaptation.

#### [](https://huggingface.co/blog/ibm-research/scarfbench#finding-configuration-dominates-migration-effort) Finding: Configuration Dominates Migration Effort

Rather than proceeding linearly, agents repeatedly returned to configuration-related artifacts while resolving framework differences and dependency issues.

### [](https://huggingface.co/blog/ibm-research/scarfbench#what-challenges-are-not-about-code-transformation) What Challenges Are Not About Code Transformation?

Not every migration issue originates from source code.

#### [](https://huggingface.co/blog/ibm-research/scarfbench#finding-environment-and-tooling-matter) Finding: Environment and Tooling Matter

Agents frequently struggled with environmental issues, including:

- Docker cache inconsistencies
- Port connectivity problems
- Maven wrapper and build tooling issues

These operational concerns often delayed validation even when the source-code migration itself was largely complete.

[![failure-distribution](https://cdn-uploads.huggingface.co/production/uploads/649c1276a83f996b4191a8f1/BtWLPGsif0O4VoQfVppIC.png)](https://cdn-uploads.huggingface.co/production/uploads/649c1276a83f996b4191a8f1/BtWLPGsif0O4VoQfVppIC.png)

**Figure: Failure Mode Distribution**

Modernization failures span build systems, deployment environments, dependency injection, databases, endpoints, assertions, and infrastructure.

## [](https://huggingface.co/blog/ibm-research/scarfbench#key-takeaway) Key Takeaway

The biggest challenge in framework modernization is not translating Java code.

It is managing the web of dependencies across configuration, infrastructure, and runtime environments.

While frontier agents can automate substantial portions of the migration process, reliable validation and architectural reasoning remain critical for achieving successful outcomes.

ScarfBench helps expose these challenges and provides a standardized way to measure progress toward truly autonomous application modernization.

## [](https://huggingface.co/blog/ibm-research/scarfbench#explore-scarfbench) Explore ScarfBench

ScarfBench is designed as an open resource for researchers and practitioners.

Resources include:

- Benchmark dataset
- Evaluation infrastructure
- Public leaderboard
- Documentation
- Open-source code

Researchers can compare agent architectures and techniques. Practitioners can use ScarfBench to evaluate modernization solutions before deploying them in production environments.

### [](https://huggingface.co/blog/ibm-research/scarfbench#website) Website

[https://scarfbench.info](https://scarfbench.info)

### [](https://huggingface.co/blog/ibm-research/scarfbench#dataset) Dataset

[https://huggingface.co/datasets/ibm-research/ScarfBench](https://huggingface.co/datasets/ibm-research/ScarfBench)

### [](https://huggingface.co/blog/ibm-research/scarfbench#space) Space

[https://huggingface.co/spaces/ibm-research/ScarfBench](https://huggingface.co/spaces/ibm-research/ScarfBench)

### [](https://huggingface.co/blog/ibm-research/scarfbench#github-repository) GitHub Repository

[https://github.com/scarfbench/scarfbench](https://github.com/scarfbench/scarfbench)

### [](https://huggingface.co/blog/ibm-research/scarfbench#leaderboard) Leaderboard

[https://scarfbench.info/leaderboard](https://scarfbench.info/leaderboard)

### [](https://huggingface.co/blog/ibm-research/scarfbench#paper) Paper

[https://arxiv.org/abs/2605.06754](https://arxiv.org/abs/2605.06754)

Framework migration remains one of the largest unsolved problems in AI-assisted software engineering. We hope ScarfBench helps the community measure progress and accelerate the next generation of AI-assisted application modernization.

We invite researchers, practitioners, and framework communities to evaluate their agents, contribute new migration scenarios and help advance the state of the art.
