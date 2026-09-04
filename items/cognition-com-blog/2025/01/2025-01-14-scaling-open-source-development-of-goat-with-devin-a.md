---
title: 'Scaling Open Source Development of GOAT with Devin: A Crossmint Case Study'
link: https://cognition.com/blog/crossmint-devin
source: cognition-com-blog
published: 2025-01-14T00:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2025-01-14-scaling-open-source-development-of-goat-with-devin-a.html
---

By The Crossmint Team01.14.25

[Crossmint](https://www.crossmint.com/) provides developer tools that help companies bring their applications, projects, and agents onchain. At the heart of our open-source initiatives is [GOAT SDK](https://github.com/goat-sdk/goat), a community-owned library that enables AI agents to interact with blockchain tools—from making stablecoin payments to trading in decentralized markets, and more.

## The Challenge

As maintainers of GOAT SDK, we faced a classic scaling challenge. The library requires constant integration work to keep pace with new protocols and dApps entering the ecosystem. Despite having an active community of developers, the integration backlog was growing faster than our contributors could handle. We needed a solution that could scale our development efforts while maintaining code quality.

## Enter Devin

When Cognition provided us with Devin credits as part of their Open Source Initiative, we saw an opportunity to test an innovative hypothesis: Could we leverage AI to scale our open-source development while allowing human developers to focus on higher-order challenges?

## Implementation and Results

The results have been remarkable. During our initial trial, Devin quickly became our #1 contributor by PR count (it merged 8 PRs, with the next contributor at 4). The majority of its contributions were high-quality and successfully merged. Two notable examples include:

**Adding support for [DEXScreener plugin](https://github.com/goat-sdk/goat/pull/173)**

- DEXScreener is a product with a REST API to check price and activity of crypto tokens
- We asked Devin to build an integration for GOAT with just a URL to their docs, and an instruction to make a plug-in.
- Later, we told Devin to look at some examples in the code to learn how to write it. In retrospect, we should have included this in the initial prompt, or in a playbook.
- Finally, we gave it one single round of feedback [in GitHub](https://github.com/goat-sdk/goat/pull/178), and this was enough to merge.

[Link to session](https://app.devin.ai/sessions/681426cd7d5a4f098fe5844ade0409fe):

![Scaling Open Source Development of GOAT with Devin: A Crossmint Case Study](https://cdn.sanity.io/images/2mc9cv2v/production/162d563ae1294f7cdcf144f04af58f2454c7e70c-2880x1690.png?w=1600&fit=max)

**Implementing [Sui blockchain integration](https://github.com/goat-sdk/goat/pull/178)**

- Sui is a blockchain, similar to Ethereum and Solana. This task was much harder than the above, adding a new chain required changing code across multiple layers, as well as adding examples.
- In this case, our prompt for Devin was more specific. And, we had to give 3 rounds of feedback. It got stuck in a bit of a loop because it used, for reasons beyond our understanding, an old version of Sui’s standard library.
- Nonetheless we were able to merge this with only an hour or so of human involvement, whereas doing it from scratch would have taken days.

[Link to session](https://app.devin.ai/sessions/1182e835c6b74be694bed4f7579de233):

![Scaling Open Source Development of GOAT with Devin: A Crossmint Case Study](https://cdn.sanity.io/images/2mc9cv2v/production/cfd41bbef4dad09382d83ebaba7618f9a092ed3f-2878x1682.png?w=1600&fit=max)

What's particularly impressive is Devin's ability to work with minimal context—often requiring just a link to integration documentation to produce working code.

## What we learned about working with Devin

Through our experience, we've identified three critical factors for success with Devin:

1. **Proper Training**: Ensure Devin has sufficient knowledge of your repository and workflows when starting a new task. The keys to achieving this are: (1) doing this iteratively (starting with easier tasks and spending time after each to build Devin’s knowledge), and (2) patience: not assuming that if Devin made a mistake it won’t be able to learn from it.
2. **Clear Task Context**: Provide sufficient background information and specific goals
3. **Validation Planning**: Establish clear criteria for success upfront

## What *not* to do with Devin

The most common mistake we've observed is overestimating Devin's capabilities, setting unrealistic expectations, and getting disappointed when it doesn’t work as expected. Developers have a tendency to approach Devin as a “superhuman” entity, which leads to:

- Providing insufficient context
- Assuming pre-existing knowledge of codebases
- Having unrealistic expectations

## Ramping Devin Up

While it’s still in early stages, we're optimistic about Devin's role in scaling our repository sustainably. This allows our human contributors to focus on critical areas where they add the most value:

- Architectural improvements
- Developer experience enhancements
- Community engagement
- Strategic prioritization

By leveraging Devin for routine integration work, we're creating a more efficient development ecosystem that benefits both our maintainers and the broader open-source community.
