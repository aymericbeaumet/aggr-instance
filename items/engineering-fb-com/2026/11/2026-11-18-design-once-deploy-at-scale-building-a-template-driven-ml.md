---
title: 'Design Once, Deploy at Scale: Building a Template-Driven ML Ecosystem'
link: https://engineering.fb.com/2026/11/18/ml-applications/design-once-deploy-at-scale-template-driven-ml-ecosystem/
source: engineering-fb-com
published: 2026-11-18T16:59:38Z
updated: 2026-11-18T16:59:38Z
first_seen: 2026-11-27T00:01:05.630507547Z
labels:
- devinfra
- ml applications
summary: 'As ML ecosystems grow to a large number of production models, the bottleneck is no longer inventing new techniques to improve these models, but rather shipping them reliably and efficiently across a fragmented model space, creating a rising tide that lifts all the boats. At Meta, we built a template-driven approach that standardizes model design [...] Read More... The post Design Once, Deploy at Scale: Building a Template-Driven ML Ecosystem appeared first on Engineering at Meta.'
content: extracted
html: 2026-11-18-design-once-deploy-at-scale-building-a-template-driven-ml.html
remote_preview:
  url: https://engineering.fb.com/wp-content/uploads/2026/11/Meta-Standard-Model-Templates-SMT.png
---

- As ML ecosystems grow to a large number of production models, the bottleneck is no longer inventing new techniques to improve these models, but rather shipping them reliably and efficiently across a fragmented model space, creating a rising tide that lifts all the boats.
- At Meta, we built a template-driven approach that standardizes model design into a small set of reusable and composable building blocks, turning model iteration from a per-model problem into a shared systems problem.
- Over the last two years, this approach reduced per-model engineering time by 92%, shipped 5000+ technique-model combinations, improved offline model quality by 0.63% on average at neutral serving capacity, and delivered 0.86% cumulative online lift covering 70% of models by count.
- This work is fully detailed in our paper, “[Design Once, Deploy at Scale: Template-Driven ML Development for Large Model Ecosystems](https://arxiv.org/abs/2603.24963).”

Meta’s ads ranking systems process billions of daily requests across a diverse array of products and optimization goals, served by a large number of production models. At that scale, the main challenge in improving user relevance and advertiser value has shifted from inventing new techniques to propagating them. Every high-impact idea needs to reach every model it applies to, and adapting it one model at a time does not scale with the fleet.

Fragmentation compounds quickly in large-scale ML systems. A new architecture lands on one model, but bringing it to every corner of the ecosystem may take years. A platform migration works for one family of models, but breaks on a long tail of specialized configurations. A tuning insight improves one model, but says little about the next one because the underlying setup is too different. Over time, propagation becomes more difficult than the innovation itself.

Meta’s ads ranking ecosystem ran into this exact problem. As the number of production models grew with the diversity of ads optimization objectives, so did the diversity of architectures, features, training settings, serving constraints, and platform-specific logic. That diversity gave teams flexibility, but it also created three systemic problems: high engineering cost to ship new ideas broadly, high reliability cost from an explosion of corner cases, and low predictability when trying to estimate the ecosystem-wide impact of a change.

The deeper issue is that fragmentation creates an unfavorable scaling law for engineering itself. If each idea must be evaluated and adapted per model, the work grows linearly with the number of models. That is manageable early on, but becomes a serious bottleneck when you have a large number of models.

Fragmentation also weakens automation. AutoML and optimization systems work best in a structured space, but when every model has a different shape, the highest-potential techniques reach only a subset of models.

Our answer was not to add another layer of automation on top of this complexity. Instead, we started by reducing the complexity itself. We did this by introducing Standard Model Templates (SMT), an approach that standardizes model design into a curated set of reusable templates.

![](https://engineering.fb.com/wp-content/uploads/2026/11/Meta-Standard-Model-Templates-SMT-image-1.png)\
Figure 1: Before SMT, each technique was tuned by hand on each model, and the combinations to try grew exponentially. With SMT, multi-model optimization tunes techniques once across representative models, and the ones that generalize go into the next template version.

## A Template-Driven Foundation

A template is an opinionated, minimal model setup built from shared components such as architecture choices, feature logic, training settings, and serving configs. Instead of maintaining every production model as a largely independent design, we group similar models around a smaller number of standard templates. Model-specific logic still exists where necessary, but the common parts are explicitly represented and shared.

In practice, we use a small set of templates that cover major ranking and retrieval patterns across different model sizes. These templates package reusable components for feature selection, feature interaction, model parallelism, training configuration, and hardware-aware serving.

This changes the operating model in an important way. Instead of evaluating a new idea independently on every model, we can evaluate it on a much smaller set of representative templates and then propagate it to the model groups that use them. In other words, we shift from per-model iteration to template-level iteration.

That gives us three main advantages.

1. **Reduced propagation complexity**: A shared template lets one implementation serve many downstream models, which lowers the cost of shipping new ideas at scale.
2. **Improved reliability:** When there are fewer unique model setups, it becomes practical to build stronger validation, better guardrails, and more transferable debugging workflows. Problems such as training instability or serving regressions become easier to reason about because they occur within a shared template structure rather than an unbounded configuration space.
3. **Improved predictability**: When models in a group share a common template, performance on representative models becomes a much better signal for the rest of the group. That makes ecosystem-level planning more grounded and less assumption-driven.

Over time, this template-driven approach became more than a standardization effort. It became the foundation for a broader ML development system.

![](https://engineering.fb.com/wp-content/uploads/2026/11/Meta-Standard-Model-Templates-SMT-image2.png)\
Figure 2: The SMT development loop (left) and the layered architecture inside each template (right). Techniques are tuned once across representative models, added to a new template version, and rolled out to every model built on it.

## SMTs: From Standardization to Scalable Technique Transfer

The shift to SMTs unlocked a more ambitious capability — the ability to optimize the entire ecosystem as a single, cohesive unit. Rather than managing a fleet of independent models, we began treating the landscape as a shared innovation surface.

Instead of propagating individual techniques one by one, we transfer proven holistic model designs from strong source models into target model families through a shared template layer. This matters because many gains come from interactions between architectural choices, scaling decisions, and training strategies, not from isolated components applied independently.

Over the last 2 years, this approach **reduced per-model engineering time by 92%, shipped 5000+ technique-model combinations across our large diverse fleet of models** , **while improving offline model quality by 0.63% on average at neutral serving capacity**, and **delivered 0.86% cumulative online lift**. More importantly, it challenged a common assumption that diverse optimization goals require highly diversified model designs. In many cases, a well-designed shared template can generalize much further than expected.

We also found that once model structure is standardized, scaling decisions become more reusable. Rather than rediscovering cost-performance trade-offs separately for every model, we can learn them in template space and apply them more broadly. This led to scaling-law-based optimization workflows that make resource allocation more principled and less ad hoc.

## Multi-Model Optimization

We use multi-model optimization to make template iteration rigorous. This process evaluates candidate techniques across representative models and searches for configurations that improve aggregate performance while limiting regressions.

A technique with a strong average gain but frequent regressions is difficult to ship across a large fleet. Consistency is as important as peak improvement for production-scale propagation. Multi-model optimization considers both aggregate performance and regression rate when deciding whether a technique is ready for integration.

After optimization, candidate techniques are validated on held-out models to check whether gains transfer beyond the representative set. Techniques that pass are integrated into the next template version. Over the last two years, [we shipped 80 new ML techniques through this workflow](https://arxiv.org/pdf/2603.24963).

The result is compounding. Every model that adopts a mature template inherits a history of generalized improvements rather than starting from an isolated baseline.

## Why SMT Matters for Agentic ML Systems

Agentic model building often envisions a single end-to-end agent exploring a large design space with access to tools, training pipelines, and evaluation results. In practice, that approach runs into a familiar problem: The search space is too unconstrained. Without strong priors, an agent spends too much effort exploring unproductive regions, recovering from avoidable failures, or proposing changes that are difficult to explain and harder to ship.

SMT helps solve that problem because they encode known-good design patterns, which gives agents a structured starting point, a bounded search space, and a quality floor. Instead of treating model building as free-form search, we can decompose it into specialized subproblems.

At Meta, SMTs are also the foundation for our multi-agent approach to model building. Templates provide the structure, and agents provide adaptive exploration and automation. At the template level, agents help build, evaluate, and improve the templates themselves. They propose optimization ideas based on proven techniques, launch experiments across representative models, diagnose failures, and iterate over multiple rounds, which helps manage the multi-day loops that template development requires. Agents can also take on harder work such as migrating models to new hardware platforms. A technique validated this way reaches every model built from that template, at a fraction of the compute it would take to adapt each model separately. We cover this in more detail in our post, [How Meta Uses AI Agents to Scale ML Techniques Across Ads Ranking Models](https://engineering.fb.com/2026/11/18/ml-applications/ai-agents-to-scale-ml-innovations-across-ads-ranking-models)**.**

## Lessons Learned and the Road Ahead for SMTs

Propagating ML innovations to improve models is often the main scaling challenge in large ML ecosystems. SMTs succeed because automation gets more powerful as the underlying system becomes more structured. Template-driven development did not just make the ecosystem easier for humans to operate. It also made it easier for optimization systems, scaling workflows, and agents to reason effectively.

But standardization only works if it is opinionated enough to remove unnecessary diversity, while still preserving the limited flexibility that matters. A template system that tries to accommodate every historical edge case will eventually recreate the fragmentation it was meant to solve.

There is more work to do in improving template coverage, strengthening scaling-law-based optimization, and moving toward fully end-to-end agentic workflows — including agentic template building, model generation, online evaluation, and continuous optimization from online signals. We also need to make these workflows more reliable and more explainable.

But the central idea has held up. If you want to scale ML innovation across a large ecosystem, you need to standardize the surface on which that innovation lands. In other words, designing once and deploying at scale is not just an efficiency tactic; it is an architectural choice about how to make a large ML ecosystem learn faster.

## Read the Paper

“[Design Once, Deploy at Scale: Template-Driven ML Development for Large Model Ecosystems](https://arxiv.org/abs/2603.24963).”

## Acknowledgements

*Aayush Aggarwal, Saurabh Aggarwal, Anuj Bindal, Nakul Camasamudram, Shouyuan Chen, Wenlin Chen, Kevin De Angeli, Jason Flaks, Luc Kien Hang, Rupinder Khandpur, Santanu Kolay, Nhat Le, Swamy Muddu, Sandeep Pandey, Dhiraj Parashar, Akshay Pardeshi, Jingzheng Qin, Shah Rahman, Shiblee Sadik, Munaf Sahaf, Vibha Sinha, Saied Tahaghoghi, Terry Tang, Alireza Vahdatpour, Fangjing Wu, Haoyu Wu, Wendan Yan, and Chang Yang.*
