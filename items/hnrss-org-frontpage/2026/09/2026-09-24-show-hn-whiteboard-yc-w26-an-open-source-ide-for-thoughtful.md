---
title: 'Show HN: Whiteboard (YC W26) – An open-source IDE for thoughtful software design'
link: https://github.com/devdotfast/whiteboard
source: hnrss-org-frontpage
published: 2026-09-24T17:21:36Z
updated: 2026-09-24T17:21:36Z
first_seen: 2026-09-24T20:00:20.682990735Z
authors:
- sidharthkmenon
summary: 'Hello! We’re Sid, Alex, Ketan, and Milan. We’re building Whiteboard, an open-source desktop app where humans and agents can architect software together in a common workspace. Here’s our repo: https://github.com/devdotfast/whiteboard and our homepage: https://whiteboard.dev.fast/ We were missing the feeling of a “whiteboard session” with another dev where you leave with a deep understanding of a system, so we built this app for ourselves. Whiteboard plugs into the tools you already use - e.g. Claude Code, Codex, etc. – and gives your agent an SDK to draw on an in-app canvas to describe its work. We began with an MVP based on HTML artifacts and started rethinking the app as we ran into limitations: 1. Built on top of CodeOSS: We found that in pure HTML tools it was hard to connect a spec or diagram to code. In Whiteboard, when you click on visualizations like a sequence diagram, an entity relationship diagram, or a quote from the agent’s trace, you can jump to the underlying code directly. When navigating code, you get keybindings and LSP support from VSCode out of the box. We’ve found this is especially valuable because tradeoffs are often only discovered after a first pass at implementation (re: slop) 2. Semantic diff viewer: we wrote a semantic, AST-aware diff viewer in Rust so you can only view the code changes which are relevant to you [1]. We’ve set up some sane defaults: large added functions are summarized as pseudocode, and things like unit tests and large documentation changes are collapsed / hidden. This is all customizable with a WASM-based plugin system. 3. Decision Log: We found it difficult to reason about what set of decisions our agents made autonomously. So we built tools for agents to query and link their own traces to the Whiteboard, so you can understand how the requirements that you set were implemented, and understand what decisions your agent made autonomously. Here’s a quick demo video explaining more: https://www.youtube.com/watch?v=ChPn3ftULWE Folks at companies like Salesforce and Modal are using Whiteboard today as a review tool for architecture or spec-level changes – really any change where they want to be involved: 1. Reviewing your own coding agent’s work: because Whiteboard makes it easier to review large amounts of code, folks will typically have their AI agents create a prototype and a corresponding Whiteboard session so they can iterate on the design. 2. Reviewing other people’s changes: We’ve found that Whiteboard is particularly helpful when composed with tools like Greptile. For example, you can run an automated code reviewer on small changes and escalate to a Whiteboard session for the changes that require human judgement. Why we built this: we’re four buddies from college who quit our jobs as tech leads right before agentic coding became industry standard. As we iterated towards an MVP for a previous idea, we struggled to maintain a comprehensible codebase while reaping all the velocity benefits of agentic coding. As more PRs were merged without our understanding, we felt a ‘cognitive debt’ begin to seep in, until it became difficult for us to even contribute to the system [2]. We’re releasing our desktop app under an MIT license. Please poke through and feel free to contribute! Eventually we’ll charge companies for a hosted web version that manages whiteboard session creation alongside features like trajectory storage and multiplayer reviews. Everything will always remain self-hostable. Thanks for reading, and we hope you try it out! We would love to hear any feedback and to learn from your expertise. Here’s are the project links again: https://github.com/devdotfast/whiteboard, and you can install (for MacOS + Linux) at https://install.dev.fast [1] diffs library: https://github.com/devdotfast/diffr [2] Credit for the term ‘cognitive debt’ goes to https://www.geoffreylitt.com/2026/07/02/understanding-is-the... Comments URL: https://news.ycombinator.com/item?id=49833867 Points: 107 # Comments: 33'
content: extracted
html: 2026-09-24-show-hn-whiteboard-yc-w26-an-open-source-ide-for-thoughtful.html
preview:
  file: 2026-09-24-show-hn-whiteboard-yc-w26-an-open-source-ide-for-thoughtful.preview-7ef3fa933cf0.webp
  width: 256
  height: 128
  alt: open-source IDE for thoughtful software design. Contribute to devdotfast/whiteboard development by creating an account on GitHub.
  color: '#e5e7ea'
images:
- source: https://opengraph.githubassets.com/b0cefb6424cefd19dea25dcd709bbd8fb43ffd9add4ca6438441a2f5cd5801a8/devdotfast/whiteboard
  original:
    file: 2026-09-24-show-hn-whiteboard-yc-w26-an-open-source-ide-for-thoughtful.image-6c97deb83efb.png
    width: 1200
    height: 600
  variants:
  - file: 2026-09-24-show-hn-whiteboard-yc-w26-an-open-source-ide-for-thoughtful.image-34e79ee63124.webp
    width: 320
    height: 160
  - file: 2026-09-24-show-hn-whiteboard-yc-w26-an-open-source-ide-for-thoughtful.image-b6a8be4f5922.webp
    width: 640
    height: 320
  - file: 2026-09-24-show-hn-whiteboard-yc-w26-an-open-source-ide-for-thoughtful.image-06be3b206614.webp
    width: 1200
    height: 600
  color: '#fefefe'
- source: https://github.com/devdotfast/whiteboard/raw/main/docs/assets/whiteboard-demo.gif
  original:
    file: 2026-09-24-show-hn-whiteboard-yc-w26-an-open-source-ide-for-thoughtful.image-ecc6dfd39e9a.gif
    width: 1186
    height: 643
  color: '#0b0e14'
---

Whiteboard is an open-source desktop app where humans and agents can architect software together in a common workspace.

Whiteboard plugs into the tools you already use - e.g. Claude Code, Codex, etc. – and gives your agent an SDK to draw on an in-app canvas to describe its work.

[![An agent draws a flow diagram on a Whiteboard next to the code it describes](https://github.com/devdotfast/whiteboard/raw/main/docs/assets/whiteboard-demo.gif)](https://github.com/devdotfast/whiteboard/blob/main/docs/assets/whiteboard-demo.gif)

Here’s a 1 min demo video explaining more: [https://www.youtube.com/watch?v=ChPn3ftULWE](https://www.youtube.com/watch?v=ChPn3ftULWE)

## Quickstart

1. [Download Whiteboard](https://install.dev.fast) and open the app.
2. Connect Claude Code, Codex, or another coding agent from the welcome screen.
3. Ask your agent to review your current branch against up-to-date main and open the result in Whiteboard.

## Guidance

In our experience, Whiteboard works best with models like GPT-6 Sol and Claude Opus 5.5 for their intelligence, cost, and speed tradeoff.

Here are a few example prompts of how to use Whiteboard effectively. We are working hard to make sure the right choices are baked in by default to the system prompt - part of why this system is open source! - but in the meantime:

### For a new API change

> hey, this stack of commits is set up so i can get an \[api\] to do \[objective\]
>
> i'd like to see:
>
> - proposed api
> - examples
> - motivations for this (if available to you in context/in the repo)
>
> and then we can dive into implementation + explaining how things worked.

### For a change to add telemetry:

> cna you explain to me the telemetry changes form the newest posthog pr [https://github.com/devdotfast/whiteboard/commit/4837e107946e27ebad50c282eb0f2585210d2a35](https://github.com/devdotfast/whiteboard/commit/4837e107946e27ebad50c282eb0f2585210d2a35) -- what are we tracking, how can we build good dashboards or product waterfalls from it? what do we do for hangs, errors, crashes etc... use whiteboard

If you see anything you don't like, highlight it in your clipboard and give it your agent, and it can re-draw on the Whiteboard to suit your needs!

## Why does this exist?

### Diagrams that lead to code

Pure HTML tools didn’t provide easy affordances to connect a spec or diagram to code; this is especially tricky since tradeoffs are often only discovered after a first pass at implementation. In Whiteboard, when you click on visualizations like a sequence diagram, an entity relationship diagram, or a quote from the agent’s trace, you can jump to the underlying code directly. When navigating code, you get keybindings and LSP support from VSCode out of the box.

### Semantic diff viewer

Raw diff views can be very noisy, so we wrote a semantic, AST-aware diff viewer in Rust so you can only view the code changes which are relevant to you. We’ve set up some sane defaults: large added functions are summarized as pseudocode, and things like unit tests and documentation changes are collapsed / hidden. This is all customizable with a WASM-based plugin system.

### Decision log

We found it difficult to reason about what set of decisions our agents made autonomously & how that impacts a change. So we built tools for agents to query and link their own traces on the Whiteboard, so you can visualize the requirements that you set, understand how they were implemented, and understand what decisions the agent made autonomously.

## Open source, on your machine

Whiteboard is MIT-licensed and runs against your local checkouts. A hosted product for teams is planned, and everything will always remain self-hostable.

## Known limitations

- You cannot currently edit files in Whiteboard. If this is something that you find yourself wanting to do, please file an issue!

- Working and browsing files across multiple repos in a single review isn't well supported.

- While you can share reviews between machines with the share button, updates made after a review is shared don't appear for others. You would need to re-share the review.

## Contributing

Contributions and feedback are welcome.

Read [CONTRIBUTING.md](https://github.com/devdotfast/whiteboard/blob/main/CONTRIBUTING.md) for setup and the pull request workflow, and follow the [Code of Conduct](https://github.com/devdotfast/whiteboard/blob/main/CODE_OF_CONDUCT.md). Report vulnerabilities as described in [SECURITY.md](https://github.com/devdotfast/whiteboard/blob/main/SECURITY.md). Questions? Ask on [Discord](https://discord.gg/wYvd2cpMQg).

## Privacy

Whiteboard runs against local checkouts. Anonymous telemetry does not include your code, diffs, Whiteboard text, prompts, or model output. Read the [privacy overview](https://github.com/devdotfast/whiteboard/blob/main/docs/privacy.md), inspect the complete [telemetry reference](https://github.com/devdotfast/whiteboard/blob/main/docs/telemetry.md), or turn telemetry off at any time.

## License

Whiteboard is available under the [MIT License](https://github.com/devdotfast/whiteboard/blob/main/LICENSE). The vendored Code - OSS fork retains Microsoft's MIT license and third-party notices; see [`apps/review-desktop/LICENSE`](https://github.com/devdotfast/whiteboard/blob/main/apps/review-desktop/LICENSE) and [`apps/review-desktop/UPSTREAM`](https://github.com/devdotfast/whiteboard/blob/main/apps/review-desktop/UPSTREAM).

## On vendoring Code OSS

With everyone using dedicated agent TUIs and desktop apps, we only use our text editors for reviewing line-by-line diffs now, so we figured why not have a text editor meant for reviewing code. In that case, might as well start off with the most successful open source editor out there as a baseline.

We vendor Code OSS unlike other forks that maintain patches because coding agents have a hard time with patches and there's a lot of stuff from stock VS Code (i.e., ~45% of the codebase is Copilot these days 😬) that we don't need.

We regularly monitor upstream Code OSS and merge in security/feature patches as they come in.

## Influences

- [https://www.geoffreylitt.com/2026/07/02/understanding-is-the-new-bottleneck](https://www.geoffreylitt.com/2026/07/02/understanding-is-the-new-bottleneck) — a great overview of the constraints of modern software engineering.
- [https://maggieappleton.com/2025-08-vibe-legacy-code/](https://maggieappleton.com/2025-08-vibe-legacy-code/) and [https://blog.val.town/vibe-code](https://blog.val.town/vibe-code) — do a great job describing how AI-generated code fits into the pre-2025 notion of software engineering.
- Karpathy on agents:
  - On LLM agents: [https://x.com/karpathy/status/1979644538185752935](https://x.com/karpathy/status/1979644538185752935)
  - On agents as "junior engineer savants": [https://x.com/karpathy/status/1915581920022585597](https://x.com/karpathy/status/1915581920022585597)
