---
title: Devin is now generally available
link: https://cognition.com/blog/devin-generally-available
source: cognition-com-blog
published: 2024-12-10T00:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2024-12-10-devin-is-now-generally-available.html
---

By The Cognition Team12.10.24

Today we’re making Devin generally available starting at $500 a month for engineering teams, which includes:

- No seat limits
- Access to Devin’s Slack integration, IDE extension, and [API](https://www.cognition.ai/blog/dec-24-product-update#for-repetitive-engineering-tasks-try-the-devin-api-with-structured-inputoutput)
- Onboarding session & support from the Cognition engineering team

All engineering teams can now start working with Devin at [app.devin.ai](http://app.devin.ai).

## **Where Devin Shines**

While Devin can be an all-purpose tool, we recommend starting with:

- **Small frontend bugs and edge cases** - tag Devin in Slack threads
- **Creating first-draft PRs for backlog tasks** - assign Devin tasks from your todo list at the start of your day
- **Making targeted code refactors** - use the Devin IDE extension (for VSCode and forks) to point Devin to parts of the code you want edited or upgraded

Devin has helped teams with everything from building integrations to migrating and maintaining documentation. Devin is versatile, but works best when you:

- Give Devin tasks that you know how to do yourself
- Tell Devin how to test or check its own work
- Keep sessions under ~3 hours and break down large tasks
- Share detailed requirements upfront
- Invest in coaching Devin by providing feedback in chat and accepting suggested [Knowledge](https://docs.devin.ai/Onboard_Devin/knowledge), or adding your own Knowledge manually

## **Working with Devin**

Slack is the primary interface for spinning up Devin sessions, so you can quickly tag @devin to offload smaller tasks and fix bugs when they’re reported.

Try asking Devin to do the first pass on the next 3rd party integration, refactor, or codebase question you have. Devin messages you when it’s done, so you can review Devin’s PR whenever convenient. Devin responds automatically to your Github PR comments.

Hand off async work to Devin directly from your IDE with ⌘ G. The Devin extension (Beta Feature available for VScode and forks) allows you to checkout Devin’s PRs and review and accept Devin’s code directly in your IDE.

## **Contributing to Open Source Repositories**

To showcase Devin in action, we’re sharing sessions where Devin resolves issues on a few of our favorite open-source repositories. Devin often needs guidance but we share these examples to show how we use Devin to speed our own workflows.

**Anthropic MCP**\
In this session, Devin identifies the cause of a user-reported issue. We liked how it read the MCP spec in the browser to understand “capability negotiation” and tested its changes end-to-end in the browser. The changes weren’t perfect, so the maintainers gave some feedback on the PR which we addressed with a second Devin session.

First session: https://app.devin.ai/sessions/266955553baf40cfa7fdd32d42ab219d

Second session: https://app.devin.ai/sessions/807168f5f9874d47a4c1965bf7afc9df

https://github.com/modelcontextprotocol/inspector/pull/105

![Devin is now generally available](https://cdn.sanity.io/images/2mc9cv2v/production/c7c887019189c18355d40cf904fba5448f21d5f5-2014x168.png?w=1600&fit=max)

\
**Zod**\
This PR adds a new feature to the popular library Zod. Devin planned collaboratively with the user, implemented the feature across multiple files and wrote tests – we were very impressed! There was a merge conflict which we manually resolved because Devin tends to struggle with that.

https://app.devin.ai/sessions/51826709fcd3457abc4be25e587c790c

https://github.com/colinhacks/zod/pull/3893

![Devin is now generally available](https://cdn.sanity.io/images/2mc9cv2v/production/3411a1a87aa611878f7658e15649177b428bf706-1736x168.png?w=1600&fit=max)

**Google**

A user of the Go Github client wished it propagated response objects even on HTTP errors. These small chores are annoying for human engineers because testing is often more effort than the fix itself. Devin required a few iterations to get it right and we manually cleaned up a few stray edits using the VS Code extension. The biggest timesaver here was Devin writing and running the unit tests.

https://app.devin.ai/sessions/1b2f7ce6e3b44942b3ac1f518eac7c22

https://github.com/google/go-github/pull/3369

![Devin is now generally available](https://cdn.sanity.io/images/2mc9cv2v/production/18a73299e1ab51f155303433d963ed6069e18480-1854x168.png?w=1600&fit=max)

**Llama Index**

Devin fixes a bug where the implementation of the Anthropic tokenizer followed the protocol incorrectly. It found the correct fix first try and wrote a unit test too. A PR comment from the maintainer requested a small stylistic change which we fixed manually.

https://app.devin.ai/sessions/3d66de6feed946efbadf8a58698caafc

https://github.com/run-llama/llama\_index/pull/17201

![Devin is now generally available](https://cdn.sanity.io/images/2mc9cv2v/production/dc90640bf91bd47cdea29e98fc3b60aaf7cd8107-2042x168.png?w=1600&fit=max)

**Karpathy’s nanoGPT\**This change is just a one-liner. But we wouldn’t trust this change without testing – which Devin did nicely by writing an ad-hoc testing script.

https://app.devin.ai/sessions/9e0c3255385c463f838f5b2f4413b92f

https://github.com/karpathy/nanoGPT/pull/578

![Devin is now generally available](https://cdn.sanity.io/images/2mc9cv2v/production/5a31675b672e516ea4fcb974ecfedb90b78a0c01-1712x168.png?w=1600&fit=max)

## **Hire Devin**

You can start working with Devin today at [app.devin.ai](https://cognition.com/blog/devin-generally-available#).

For more information about [Devin Enterprise](https://devin.ai/enterprise), reach out to our Sales team [here](https://cognition.ai/get-started#company).\
