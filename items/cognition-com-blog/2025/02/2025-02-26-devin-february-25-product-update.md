---
title: Devin February '25 Product Update
link: https://cognition.com/blog/devin-february-25-product-update
source: cognition-com-blog
published: 2025-02-26T00:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2025-02-26-devin-february-25-product-update.html
---

By The Cognition Team02.26.25

Devin is better at testing UI changes locally, can do batch (parallel) edits, and proactively gives you feedback on suboptimal prompts! We've also added support for GitLab, incorporated Sonnet 3.7 into Devin, and more.

- **Speed:** Devin is ~2x faster vs in October 2024 and takes ~7.8 minutes on average to complete junior developer tasks in our internal evaluations.
- **Copy/paste in Devin’s browser:** You can now copy text from your browser and paste it into Devin’s browser. This highly requested feature eliminates a major point of friction when granting Devin access to accounts—you no longer need to manually type in your passwords!
- **Helping users with their prompts:** Devin proactively gives you feedback on suboptimal prompts and proposes breaking tasks down when they’re too complex.
- **GitLab (beta):** Connect both GitLab and GitHub repos to Devin! Devin can now push, pull, and view/create GitLab MRs. Contact us via [app.devin.ai/settings/support](https://app.devin.ai/settings/support) to set this up.

![Devin February '25 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/b53519114916d554b0b004e81624ea8389803900-737x260.png?w=1600&fit=max)

- **Batch edits:** Tell Devin to “find and edit” code to encourage Devin to "fan out" and edit an arbitrary number of files in parallel. This greatly improves speed, especially for repetitive refactors.

- **Multi-action:** Devin can choose to perform a diverse batch of actions optimistically (e.g. viewing the browser, running a shell command, and reading multiple code files), improving speed.
- **Browser improvements:** We've shipped browser changes that allow Devin to:
  - deal with auto-opening tabs (required for some complex auth flows)
  - use multiple tabs (helpful for iteratively comparing 2+ webpages)
- **Local UI testing:** Devin can better test and visually understand UI changes locally.
- **Customize chat vs workspace width:** Drag to make the chat as narrow or wide as you’d like! The editor in the workspace is also easier to navigate now, with file tree on the left.

![Devin February '25 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/acf35352e928067abbd4becd29b027f21598f95c-1585x1006.png?w=1600&fit=max)

- **Repo setup (in [Devin’s Workspace](https://app.devin.ai/workspace)):** We verify that all the commands you provide Devin (to run lint, install dependencies, and run tests) run successfully, and Devin will surface in chat if any of these commands don’t succeed.

![Devin February '25 Product Update](https://cdn.sanity.io/images/2mc9cv2v/production/9495ad329370b816a34b0ae2d9bc9834c8ac9e5c-1007x679.png?w=1600&fit=max)

- **Sonnet 3.7 in Devin:** We incorporated Sonnet 3.7 in Devin on 2/24, with optimizations to our use rolling out starting 2/26. In our testing, the new model is the best we have seen to-date on a variety of tasks including debugging, codebase search, and agentic planning.
- **Devin PR metrics:** [app.devin.ai/metrics](https://app.devin.ai/metrics) now shows all PRs opened by Devin, even when 2+ PRs were opened in the same session.
- **Faster startup:** Devin only installs dependencies for the repositories needed in a session.
- **Addressing your PR review feedback:** Devin is more reliable at remembering to address *all* the review comments you left on its PR.
- **Misc brain improvements:** Devin is less likely to loop while trying to fix CI/lint failures, is better at planning, is better with git, and many more improvements.

Hire Devin! You can start working with Devin today at [***app.devin.ai***](http://app.devin.ai). For more information about [***Devin Enterprise***](https://devin.ai/enterprise), reach out to our Sales team [***here***](https://cognition.ai/get-started#company).
