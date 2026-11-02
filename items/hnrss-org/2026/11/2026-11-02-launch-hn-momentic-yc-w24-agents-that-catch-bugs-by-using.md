---
title: 'Launch HN: Momentic (YC W24) – Agents that catch bugs by using your app'
link: https://momentic.ai/
source: hnrss-org
published: 2026-11-02T17:05:25Z
updated: 2026-11-02T17:05:25Z
first_seen: 2026-11-02T23:50:57.210604418Z
authors:
- wuweiweiwu
summary: 'Hey HN, we’re Wei-Wei and Jeff. We’re building Momentic, QA agents that autonomously plan, explore, and bug-bash controls and user flows in your web, iOS, Android, and (soon) desktop apps to catch bugs. Here’s a demo: https://youtu.be/BFsM-Se1Fh8 We’re both software engineers that spent our careers on dev tooling and infra across companies like Robinhood, Retool, WeWork, and Qualtrics. Testing was always a pain in the ass (esp Playwright and Selenium), so we built agents to do it for us. Modern day software has a ton of bugs and the change failure rate is only growing faster as coding agent adoption scales. LLMs are really good at generating code, but reinforce their own assumptions, write a ton of useless tests, and are bad at visual understanding. On DeepSWE v1.1, Sonnet 5.5 hit 66.2% pass rate when banned from writing tests, versus 65.3% when allowed. OSS end-to-end testing frameworks like Playwright, Selenium, or Maestro force you to hardcode implementation details like XPath, text, or aria-* attributes in your tests. Even with agents, these take a nontrivial amount of time and tokens to build and maintain. In 2023, we saw Nat Friedman drive a browser with GPT-3 via natbot. We were inspired to use this technology to automate the most painful and tedious parts of software engineering: testing. We started with using LLMs to generate and maintain Playwright, but quickly realized that those primitives were fundamentally misaligned with the agentic SDLC. We think the future of QA is agents testing themselves, not writing scripts. Mo is a custom agent harness for catching bugs that builds on the past 3 years of Momentic and millions of test traces from complex, real-world applications. Mo is built for recall: it finds the most real bugs, though Codex is a bit more precise at 86% vs 84%. The tradeoff is cost: Mo costs $2.67 per bug vs $0.80 for Codex, less than Claude Code and Devin. - Self-organizing agent graph. Mo builds a directed acyclic graph (DAG) as it explores. Large product areas automatically fork into parallel sub-agents, each with their own browser or device for concurrent execution. - Specialized agents for domain specific tasks. Planner agents reason over screenshots and video while dedicated locator agents inspect page structure to resolve plain-English interaction targets. Others include image analysis, video understanding, audio generation, and more. - Parallel agents coordinate through cooperative resource leases. Agents can announce that they’re “using” or “modifying” overlapping resources or fixtures. Multiple “using” claims can coexist, but only a single “modifying” claim can be active on any particular one. - Scaled infrastructure. Mo ships with a tunnel that lets users expose private sites and local apps to our hosted agents and infra. A single bug bash can spin up 100s of concurrent browsers and devices. We benchmarked Mo against coding agents on 4 popular open-source apps: Gitea (58.8k stars), Metabase, Twenty (YC), and Saleor, with 1,396 human-labeled bugs. Each agent got the same brief, a fresh E2B sandbox, and no access to source code. Codex and Claude Code used hosted Browserbase browsers, while Devin used its built-in browser. For evaluators, we implemented LLM as a judge (GPT-6.1 Sol) with a human reviewer. Results from a time-boxed 60-minute run: - Mo: 269 bugs, 19.3% recall, 84% precision, $2.67 per bug - Codex (GPT-6.1 Sol + Playwright MCP): 86 bugs, 6.2% recall, 86% precision, $0.80 per bug - Claude Code (Opus 5.5 + Playwright MCP): 59 bugs, 4.2% recall, 80% precision, $4.09 per bug - Devin: 27 bugs, 1.9% recall, 84% precision, $3.89 per bug Mo finds more bugs the more time it has. From 30 to 60 minutes, it went from 177 to 269 bugs (+52%), while Codex gained only 16%. Results: https://momentic.ai/qa-bench Write-up: https://momentic.ai/blog/qa-bench Dataset: https://huggingface.co/datasets/momentic/qa-bench Harness: https://github.com/momentic-ai/qa-bench We just shipped PR testing for Vercel and Cloudflare deploy previews and we’re giving every new signup $250 in Mo spend. Paid plans have higher infra capacity and included usage along with options to top up. We’d love for you to try it out and give us feedback. Feel free to comment here, we’ll be around all day! Comments URL: https://news.ycombinator.com/item?id=50339913 Points: 109 # Comments: 72'
content: extracted
html: 2026-11-02-launch-hn-momentic-yc-w24-agents-that-catch-bugs-by-using.html
remote_preview:
  url: https://momentic.ai/images/og/home.png
  alt: 'Momentic: the agentic quality platform.'
---

## Catch bugs before they ship.

Mo is an AI QA engineer that tests your app end to end. Verify critical flows and find bugs from a prompt.

Tests created 77,813

Test runs executed 70,607,819

Changes verified 339,166

PRs verified 80,290

Auto-heals 8,932,104

Bugs caught 117,010

Tests created 77,813

Test runs executed 70,607,819

Changes verified 339,166

PRs verified 80,290

Auto-heals 8,932,104

Bugs caught 117,010

Tests created 77,813

Test runs executed 70,607,819

Changes verified 339,166

PRs verified 80,290

Auto-heals 8,932,104

Bugs caught 117,010

![The Mo session view: a bug-bash transcript on the left and the filed finding with its recording on the right](https://momentic.ai/images/mo-session-clean.webp)

Trusted in production by teams shipping every day.

![Notion](https://cdn.sanity.io/images/apz2bojg/production/72b52eb9b11e8ba7f6c8f3f5adce8161d687b401-128x88.svg)

[![Quora](https://cdn.sanity.io/images/apz2bojg/production/bef15130a1565ed781a0fb1bffd003dd49ec0aed-128x88.svg)](https://momentic.ai/customers/quora)

![Webflow](https://cdn.sanity.io/images/apz2bojg/production/923ff5bceae8ddeb7ba0f41eb830a3e236bf66fd-128x88.svg)

![Framer](https://cdn.sanity.io/images/apz2bojg/production/95e086e20f8241591ce06be24db3129ca292dc03-128x88.svg)

![Xero](https://cdn.sanity.io/images/apz2bojg/production/f06ca7a42405173d610919cef504a3a38853ddb7-128x88.svg)

![Whatnot](https://cdn.sanity.io/images/apz2bojg/production/6448f529050c6528226d564ab8cab267251e321b-128x88.svg)

[![Retool](https://cdn.sanity.io/images/apz2bojg/production/f38c3e575c27596b62e49055fc75f8d96b2c80bf-128x88.svg)](https://momentic.ai/customers/retool)

![Runway](https://cdn.sanity.io/images/apz2bojg/production/02192696eb17d0debbc5a4b57f68dc6b5aab2cbe-128x88.svg)

![Bilt](https://cdn.sanity.io/images/apz2bojg/production/bdc50457580d082adea58112f70f8420a4e3c1ec-128x88.svg)

![Collectors](https://cdn.sanity.io/images/apz2bojg/production/7468c8adc8408d75dc6dbc21931874bf1bfd1416-128x88.svg)

![Podium](https://cdn.sanity.io/images/apz2bojg/production/62a6f69f2674b7919c227ae13ee15cded2589d83-128x88.svg)

![SPS Commerce](https://cdn.sanity.io/images/apz2bojg/production/f877adc1a7f691136e32ae1152b7fd4752be2997-128x88.svg)

[![GPTZero](https://cdn.sanity.io/images/apz2bojg/production/4924b1f6da15d63f0119b0f27a338f3d0f87f560-128x88.svg)](https://momentic.ai/customers/gptzero)

![Vapi](https://cdn.sanity.io/images/apz2bojg/production/7825ef0f46740d5a36e27bbb317e36beb9351d85-128x88.svg)

[![Mutiny](https://cdn.sanity.io/images/apz2bojg/production/4c393068ff8211fd9c32f00259c162d10dd0fdfe-128x88.svg)](https://momentic.ai/customers/mutiny)

![Squire](https://cdn.sanity.io/images/apz2bojg/production/5b130ab298c3b3a375135329ef3ee78970b02b2d-128x88.svg)

![Arena](https://cdn.sanity.io/images/apz2bojg/production/ae4ac69ded52f0c0ed28acb9e2990ccdac387e4b-128x88.svg)

![Kumo](https://cdn.sanity.io/images/apz2bojg/production/3141cc726a37c96c38d3e1cfebce9dfac40aeb96-128x88.svg)

"Our team's experience with Momentic has been great. Setting up tests is simple and the developer experience has been fantastic."

Erdem

Head of Developer Experience, Notion

"Having E2E QA be part of the development process, run this fast (6.3x parallel), and at this scale (for every single pull request) is a true dream. Seriously, a green CI now has a measurable impact on my sleep quality."

Glavin

Founding AI Engineer, Coframe

"Momentic makes it 3x faster for our team to write and maintain end-to-end tests. Startups win by shipping faster and better, and Momentic enabled us to do exactly that."

Alex

CTO, GPTZero

"I just ran our first automated eval across 110 prompts. This would have taken well over an hour manually — I kicked it off in 5 seconds and it was done in 14 minutes. HUGE for us."

Mike

Engineering Manager, Runway

"I love working with Momentic. Especially after having years of previous experience in QA, it feels like riding a bike, except the bike is a supercharged electric one!"

Anna

Senior Productivity Engineer, Webflow

 1 of 5

Code moves faster. Verification has to keep up.

Coding agents have made it dramatically faster to build software. The bottleneck is now knowing it works. Verification belongs in the agent loop. Agents need evidence from the app itself to verify that their changes deliver the intended behavior.

[Explore](https://momentic.ai/#platform)

## The agentic quality platform.

Write the user flows you need to verify. Momentic runs the tests against your app. Reuse successful AI actions and review proposed repairs as the app changes.

 1 of 5

 01

Knows how your product works

Add terminology, expected behavior, and test guidance to the Knowledge base. AI-assisted steps look up relevant entries as they run.

 PRODUCT CONTEXT AT EACH AI STEP

 02

Keep tests working as the UI changes

Momentic finds controls again when the UI changes during a run. Permanent healing proposes a test-file repair for you to review.

 RUNTIME RECOVERY AND REVIEWED REPAIRS

 03

Verify changes with your coding agent

Give your coding agent the changed flow. It can use MCP to author a test, check it in a live session, and save the YAML alongside your code.

 TESTS REVIEWED WITH THE CODE

 04

See why a test failed

Inspect the failing step, screenshots, and traces. Classify failures as application bugs, test flakiness, or environment issues.

 STEP-LEVEL FAILURE EVIDENCE

 05

Verify before you merge or deploy

Run the CLI in your CI job. Use its exit code as a required check before merging or deploying.

 CLI EXIT CODES FOR CI CHECKS

Verification from your editor to CI.

## Define what matters. Let Momentic verify it.

Write steps and assertions in plain-English YAML.\
Review and version the tests in your repository.

 01 / author

### Write tests in plain English

Describe a user flow and its expected result in plain English. Keep the steps and assertions in YAML files your team and coding agent can review.

 02 / run

### Run locally and in CI

Use the CLI to check the app you're changing. Run the same tests in CI before the next merge or deploy.

 03 / triage

### Find the root cause

See which step failed and inspect its screenshots and traces. AI triage investigates the cause and proposes a test repair for you to review.

 04 / heal

### Auto-heal as your product evolves

Momentic finds controls again when the UI changes. Permanent healing proposes test-file edits for review. Apply the diff and rerun to check the result.

## Learns how your product works.

Describe your product’s terminology, business rules, and expected behavior in the Knowledge base. Agents use that context to choose actions and check outcomes.

### Knowledge base

Give agents the product rules they need to check a flow.

### App graph

Maps screens and user flows seen during test runs.

### Review new guidance

Review session suggestions before adding them to the Knowledge base.

Loved by engineering teams.

![Quora](https://cdn.sanity.io/images/apz2bojg/production/4ddaf41c1ede53fc0a710ff1ba967f3872e00352-88x24.svg)

“Momentic gave us a fast and reliable way to validate Poe.com's AI responses, even when they weren't deterministic.”

 Momoko F. Head of Product Operations, Quora

30 min

daily test execution, down from 7 hours

500+

manual test cases replaced

100%

critical tests created in one month

[Read the full story](https://momentic.ai/customers/quora)

## Bugs caught by Momentic

Examples of application failures found by Momentic, with the affected flow and its impact.

 117,010 before production

 Detected ON A FEATURE BRANCH

### New-device 2FA prompt skipped

At a consumer fintech app, a login routing change bypassed the new-device verification step.

Impact

A security checkpoint was silently skipped.

 Detected BEFORE RELEASE

### Page creation flow broken on mobile

On mobile, keyboard auto-dismissed when creating a new page, breaking content flow.

Impact

Silent friction in the core creation flow.

 Detected IN STAGING

### Driver ETA stuck on 'calculating'

A dependency update on a large rideshare app caused the live ETA subscription to silently fail after a ride was booked.

Impact

Riders never saw an arrival estimate.

 Detected BEFORE DEPLOY

### Scheduling availability displayed incorrectly

Incorrect display and logic in the calendar scheduling view.

Impact

Wrong availability shown to users.

 Detected IN CI

### Timer reset when the screen locked

At a leading wellness app, an iOS lifecycle change reset the active sleep timer the moment the phone locked.

Impact

Audio sessions ended mid-playback overnight.

 Detected 8 MIN BEFORE DEPLOY

### Saved cards dropped at checkout

At a top food-delivery marketplace, a payment-form refactor stopped pre-filling saved cards, forcing manual re-entry to pay.

Impact

Returning users abandoned checkout on mobile.

 Detected IN A NIGHTLY RUN

### Filters cleared when paginating results

At a popular online retail marketplace, moving to page two reset every active category and price filter.

Impact

Shoppers lost their refined search results.

 Detected BEFORE MERGE

### Valid promo codes rejected at the final step

At a large online travel marketplace, a validation change rejected multi-segment promo codes on the last checkout step.

Impact

Discounts failed and carts were abandoned.

 Detected PRE-RELEASE

### Wrong subtitle language on first play

At a major streaming platform, a locale-detection bug auto-selected the wrong subtitle track on initial playback.

Impact

Wrong-language captions at session start.

 Detected BEFORE RELEASE

### Legacy API issues exposed

12 bugs caught in one day. Exposed legacy API issues that would have shipped undetected.

Impact

Risked shipping regressions into major release.

## Verify what you ship.

## Changelog

### [Mo is generally available](https://momentic.ai/docs/mo)

Mo is out of beta. Start a session from your coding agent, the app, a PR comment, or the qa CLI. A separate agent reproduces each functional bug before Mo reports it.

### [QA Bench](https://momentic.ai/qa-bench)

We graded Mo, Codex, Claude Code, and Devin on 20 areas of Gitea, Metabase, Twenty, and Saleor. With 60 minutes per area, Mo found about 269 labeled bugs. The next best agent found about 86.
