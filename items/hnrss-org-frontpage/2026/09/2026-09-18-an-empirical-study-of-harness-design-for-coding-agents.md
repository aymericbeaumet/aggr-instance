---
title: An Empirical Study of Harness Design for Coding Agents
link: https://arxiv.org/abs/2609.20804
source: hnrss-org-frontpage
published: 2026-09-18T13:06:30Z
updated: 2026-09-18T13:06:30Z
first_seen: 2026-09-18T16:19:55.454833534Z
authors:
- wek
summary: 'Article URL: https://arxiv.org/abs/2609.20804 Comments URL: https://news.ycombinator.com/item?id=49753878 Points: 139 # Comments: 28'
content: extracted
html: 2026-09-18-an-empirical-study-of-harness-design-for-coding-agents.html
preview:
  file: 2026-09-18-an-empirical-study-of-harness-design-for-coding-agents.preview-41145fe12e79.webp
  width: 256
  height: 149
  alt: arXiv logo
  color: '#ece6e5'
images:
- source: https://arxiv.org/static/browse/0.3.4/images/arxiv-logo-fb.png
  original:
    file: 2026-09-18-an-empirical-study-of-harness-design-for-coding-agents.image-88f5088404c9.png
    width: 1200
    height: 700
  variants:
  - file: 2026-09-18-an-empirical-study-of-harness-design-for-coding-agents.image-fc6386ce1e2e.webp
    width: 320
    height: 187
  - file: 2026-09-18-an-empirical-study-of-harness-design-for-coding-agents.image-49eabc2e18a4.webp
    width: 640
    height: 373
  - file: 2026-09-18-an-empirical-study-of-harness-design-for-coding-agents.image-f275dcf4bd8a.webp
    width: 1200
    height: 700
  color: '#fefefe'
---

[View PDF](https://arxiv.org/pdf/2609.20804) [HTML (experimental)](https://arxiv.org/html/2609.20804v1)

> Abstract:Coding harnesses shape how autonomous coding agents translate model capabilities into long-horizon software-engineering performance, yet existing work typically evaluates harnesses as monolithic systems, leaving the effectiveness of individual components unclear. To enable component-level comparisons, we study this question with a lightweight coding harness whose execution loop is fixed while three components are varied: planning, action space, and context management. Across four models evaluated on SWE-Bench Verified and Terminal-Bench 2.1, we evaluate 176 matched settings spanning five context-management strategies, four context-window budgets, and targeted ablations of planning and action space. We find that: (1) Context management becomes increasingly valuable as the context-window budget tightens, with most of its benefit coming from preventing context-overflow failures. (2) Staging rule-based elision before LLM-based summarization provides the strongest overall efficiency among the context-management strategies, whereas making elided content recoverable adds machinery that models rarely use and yields no accuracy gain. (3) Planning shifts from an accuracy scaffold for weaker models to a cost saver for stronger models, with little change in accuracy. (4) Predefined tools improve performance for models with weaker bash proficiency, whereas bash-capable models can operate effectively with a bash-only interface and achieve substantially lower cost, especially on command-line-centric tasks. Trajectory-level analysis explains these effects: context management extends execution trajectories without substantially altering agent behavior, planning changes where trajectories stop, and the action space changes the granularity at which code is written. These findings inform model- and budget-aware harness design and provide a modular framework for evaluating future harness components.

Comments:

43 pages

Subjects:

Artificial Intelligence (cs.AI); Computation and Language (cs.CL); Machine Learning (cs.LG); Software Engineering (cs.SE)

Cite as:

[arXiv:2609.20804](https://arxiv.org/abs/2609.20804) \[cs.AI\]

(or [arXiv:2609.20804v1](https://arxiv.org/abs/2609.20804v1) \[cs.AI\] for this version)

[https://doi.org/10.48550/arXiv.2609.20804](https://doi.org/10.48550/arXiv.2609.20804)

arXiv-issued DOI via DataCite (pending registration)

## Submission history

From: Run-Ze Fan \[[view email](https://arxiv.org/show-email/1cad69a8/2609.20804)\] \
 **\[v1\]** Thu, 17 Sep 2026 17:58:07 UTC (6,713 KB)
