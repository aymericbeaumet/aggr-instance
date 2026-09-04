---
title: Announcing The Devin Open Source Initiative
link: https://cognition.com/blog/cognition-open-source-initiative
source: cognition-com-blog
published: 2024-12-12T00:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2024-12-12-announcing-the-devin-open-source-initiative.html
---

By Mokshit Jain12.12.24

*Update*: instead of emailing us, please [fill out this form.](https://form.typeform.com/to/AITkMdpD)

Today we are announcing the Devin Open Source Initiative.

\
As part of Devin’s general availability launch, we’re giving selected open source maintainers 500 free ACUs on a Devin Teams plan. Fill out the application form here: [https://form.typeform.com/to/AITkMdpD](https://form.typeform.com/to/AITkMdpD)

Open source projects often have a long backlog of issues. There are so many little things to fix that maintainers can quickly become overwhelmed. Devin is the ideal partner on these tasks — by working on minor tasks in the background, Devin helps the maintainers focus on the critical tasks.

Below are some OSS contributions Devin has already helped to make:

## Anthropic MCP

Pull request: [https://github.com/modelcontextprotocol/inspector/pull/105](https://github.com/modelcontextprotocol/inspector/pull/105)

Devin session: [https://app.devin.ai/sessions/266955553baf40cfa7fdd32d42ab219d](https://app.devin.ai/sessions/266955553baf40cfa7fdd32d42ab219d)

Here Devin works on the inspector project, a debugging UI for MCP servers, to add a new feature for displaying the capabilities negotiation.

Issue: [https://github.com/modelcontextprotocol/inspector/issues/85](https://github.com/modelcontextprotocol/inspector/issues/85)

![Announcing The Devin Open Source Initiative](https://cdn.sanity.io/images/2mc9cv2v/production/db43246185dd451828bef4129dd353e0da06757a-917x513.png?w=1600&fit=max)

Devin first uses the browser to research how capability negotiation works:

![Announcing The Devin Open Source Initiative](https://cdn.sanity.io/images/2mc9cv2v/production/27b3c022fa81515237f47b9280cf39c0f739fc15-698x922.png?w=1600&fit=max)

After Devin writes the code, a few key Devin features enable proper testing:

1\. **Repo setup**. Before starting Devin sessions, we set up Devin’s VM by installing uv for Python MCP servers.\
2\. **Knowledge**. We can either tell Devin manually how to test the code, or add this to its knowledge base so that Devin automatically remembers in future sessions.

![Announcing The Devin Open Source Initiative](https://cdn.sanity.io/images/2mc9cv2v/production/f612f8e159aa9da821a6fbf1da37db297bf6d81b-674x208.png?w=1600&fit=max)

Given this setup, Devin can verify that the code changes work in the browser:

![Announcing The Devin Open Source Initiative](https://cdn.sanity.io/images/2mc9cv2v/production/876de41f09632743448362b5fdd0da18ae3c2fb6-686x931.png?w=1600&fit=max)

The code, now tested, is ready for the maintainer to review.

![Announcing The Devin Open Source Initiative](https://cdn.sanity.io/images/2mc9cv2v/production/6c2ad4c221d7930b70fa7a0a3fd7637065a1b8cc-668x563.png?w=1600&fit=max)

## Dagger

Pull Request: [https://github.com/dagger/dagger/pull/9130](https://github.com/dagger/dagger/pull/9130)

Devin session: [https://app.devin.ai/sessions/2afcdb9847ff416382ee6126bc77ee8c](https://app.devin.ai/sessions/2afcdb9847ff416382ee6126bc77ee8c)

Devin’s PR addresses a low-priority task in the Dagger project.

![Announcing The Devin Open Source Initiative](https://cdn.sanity.io/images/2mc9cv2v/production/371f3bfa2d27e1fcc9bd2cc63e584a4f1f20d7ea-917x443.png?w=1600&fit=max)

While it’s impressive that Devin is able to eventually solve the issue, several rounds of back and forth are sometimes needed.

![Announcing The Devin Open Source Initiative](https://cdn.sanity.io/images/2mc9cv2v/production/6c58d3b68a7185786378645b1b0f33635eadd7e2-753x356.png?w=1600&fit=max)

Devin’s Github integration works with PR comments and CI checks to make this process smoother. Any PR comments are automatically sent to Devin by a webhook.

![Announcing The Devin Open Source Initiative](https://cdn.sanity.io/images/2mc9cv2v/production/242829171398c7a626c84d01fb3c4d3c9f324fef-731x487.png?w=1600&fit=max)

Finally, to get the most out of Devin, don’t expect 100% perfect PRs. While Devin can get you 80% of the way there, a human should still ensure quality of the final result. This means e.g. removing stray diffs before merging. In this PR, Devin added debugging logs which will later need to be removed.\

You can read more about how Dagger gets value from using Devin in Dagger’s blog post: [https://dagger.io/blog/new-ai-developer-devin](https://dagger.io/blog/new-ai-developer-devin).

## Many more

We had Devin contribute to many more open source repos, including:

- [karpathy/nanoGPT/pull/578](https://github.com/karpathy/nanoGPT/pull/578) ([View Devin run)](https://app.devin.ai/sessions/9e0c3255385c463f838f5b2f4413b92f)
- [modelcontextprotocol/inspector/pull/105](https://github.com/modelcontextprotocol/inspector/pull/105) ([View Devin run)](https://app.devin.ai/sessions/266955553baf40cfa7fdd32d42ab219d)
- [run-llama/llama\_index/pull/17201](https://github.com/run-llama/llama_index/pull/17201) ([View Devin run)](https://app.devin.ai/sessions/3d66de6feed946efbadf8a58698caafc)
- [hyperliquid-dex/hyperliquid-rust-sdk/pull/68](https://github.com/hyperliquid-dex/hyperliquid-rust-sdk/pull/68) ([View Devin run)](https://app.devin.ai/sessions/26449cc89c9b4b9886ba30da44fd6bb9)
- [dagger/dagger/pull/9130](https://github.com/dagger/dagger/pull/9130) ([View Devin run)](https://app.devin.ai/sessions/2afcdb9847ff416382ee6126bc77ee8c)
- [modelcontextprotocol/inspector/pull/104](https://github.com/modelcontextprotocol/inspector/pull/104) ([View Devin run)](https://app.devin.ai/sessions/57e4210f06024a688ba0009fe2e2ddcb)
- [modelcontextprotocol/servers/pull/256](https://github.com/modelcontextprotocol/servers/pull/256) ([View Devin run)](https://app.devin.ai/sessions/5eeae60d4a6d4686af0e5e217208cd48)
- [ekzhang/bore/pull/146](https://github.com/ekzhang/bore/pull/146) ([View Devin run)](https://app.devin.ai/sessions/2b78db994f0848bfbd2b181fc33dc044)
- [colinhacks/zod/pull/3893](https://github.com/colinhacks/zod/pull/3893) ([View Devin run)](https://app.devin.ai/sessions/51826709fcd3457abc4be25e587c790c)
- [google/go-github/pull/3369](https://github.com/google/go-github/pull/3369) ([View Devin run)](https://app.devin.ai/sessions/1b2f7ce6e3b44942b3ac1f518eac7c22)

We’re excited to see how Devin can help build your open source project! Fill out the application form here: [https://form.typeform.com/to/AITkMdpD](https://form.typeform.com/to/AITkMdpD)
