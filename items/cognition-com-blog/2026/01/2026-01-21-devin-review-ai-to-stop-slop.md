---
title: 'Devin Review: AI to Stop Slop'
link: https://cognition.com/blog/devin-review
source: cognition-com-blog
published: 2026-01-21T00:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2026-01-21-devin-review-ai-to-stop-slop.html
---

By The Cognition Team01.21.26

As coding agents proliferate, the # of PRs increases, but the quality of that code can be mixed, and the size of each PR is increasing past maintainers' ability to understand. We're hearing from our customers that **code review—not code generation—is now the bottleneck to shipping great products.**

That's why we built Devin Review, a code review tool that **uses state-of-the-art AI + UX to scale human understanding of ever-more-complex code diffs**—whether authored by a human, or an agent.

![Devin Review: AI to Stop Slop](https://cdn.sanity.io/images/2mc9cv2v/production/d3a2ab40ed159480636c231c48bfd8c547554e4e-926x768.png?w=1600&fit=max)

Devin Review is currently free, while in early release, and works on any public or private GitHub PR. You can use it in three ways:

1. Devin users: head to [app.devin.ai/review](http://app.devin.ai/review) to see all your open PRs.
2. Everyone: swap github for devinreview in any PR URL (e.g. https://github.com/org/repo/pull/123 ⇒ https://devinreview.com/org/repo/pull/123). No login needed for public PRs.
3. Everyone: npx devin-review {pr-link} - run this command inside the PR’s parent repo.

Check out the [docs](https://docs.devin.ai/work-with-devin/devin-review) for more details.

## The Birth and Stagnation of Code Review

15 years ago, [GitHub set the standard](https://github.blog/news-insights/the-library/pull-request-diff-comments/) for PR review... and then stopped there.

![Devin Review: AI to Stop Slop](https://cdn.sanity.io/images/2mc9cv2v/production/4b91360f898d7b38ddc3be61ebecd91df4578333-1986x1496.png?w=1600&fit=max)

The first order problem with standard code review is well known - when PRs are small, they're easy to read and argue about. But this breaks down quickly for large reviews. We call this the "Lazy LGTM problem".

![Devin Review: AI to Stop Slop](https://cdn.sanity.io/images/2mc9cv2v/production/62a49d86741bf7901263a4f10c7c0983d1518dda-597x291.png?w=1600&fit=max)

More broadly, the feeling that people have of extreme productivity with coding agents in their vibecoded prototypes, vs the disappointing feeling that most people actually see in the useful output of their engineers, organizations, and experience as end users, is the great mystery of our time.

To paraphrase Winston Churchill: "Never in the field of software engineering has so much code been created by so many, yet shipped to so few."

![Devin Review: AI to Stop Slop](https://cdn.sanity.io/images/2mc9cv2v/production/f4db99a8dd0979c61848c64ff3c8741b2c00db22-1805x714.png?w=1600&fit=max)

## The Modern Code Review Workflow

![Devin Review: AI to Stop Slop](https://cdn.sanity.io/images/2mc9cv2v/production/c912959f2ce81ff38cc519c59d8977b745e4b626-3452x1598.png?w=1600&fit=max)

Devin Review adds AI tooling to help you scale your PR understanding, so you quickly and fully understand the code you are about to merge.

As you review the PR, Devin helps you in every step:

- **Reading better:**
  - GitHub shows you diffs by alphabetical order.
  - **Solution: intelligent diff organization**. PR Review analyzes your code, groups together changes that are logically connected, orders the hunks of code, and explains each hunk, so you can review from top to bottom. It's as if a smart colleague was walking you through the PR.
  - A quality-of-life improvement: when code is moved or renamed, GitHub shows the changes as full deletes and full writes. We detect what was copied/moved and don’t make a fuss.
- **Asking for more info**:
  - When more context is needed on a diff, including context on code outside of the PR, GitHub doesn’t offer any solutions beyond token search.
  - **Solution: Interactive chat**. Devin Review pipe your diffs into an inline Ask Devin session with full codebase understanding, so you can **chat about the changes**, without leaving the review.

![Devin Review: AI to Stop Slop](https://cdn.sanity.io/images/2mc9cv2v/production/a960e2d41ef0de2e05757239e64f1c863c80a635-1988x1372.png?w=1600&fit=max)

- **Catching bugs and issues**
  - GitHub doesn’t catch bugs in PRs; it relies on your CI/linting. Other bugcatchers in the market are often seen as spammy and low signal.
  - **Solution**: **AI Bug Detection.** Devin Review scans the diffs and generates a list of issues categorized by seriousness: **red** for probable bugs, and **yellow** for warnings, and **gray** for FYI/commentary.
  - You can copy/paste or dismiss the AI flags, or otherwise just work with fellow humans in normal **comment bubbles**.

![Devin Review: AI to Stop Slop](https://cdn.sanity.io/images/2mc9cv2v/production/3dbff23629f04010fac6a6f042f7c48cbef97d90-968x1110.png?w=1600&fit=max)

This is a starting point, and we’ll be pushing frequent updates as our users keep telling us what they want. We hope you'll try out Devin Review and give us feedback!
