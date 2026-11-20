---
title: 'Launch HN: Onlook (YC W25) – An open-source developer tool for designers'
link: https://github.com/onlook-dev/onlook/
source: hnrss-org
published: 2026-11-19T16:34:04Z
updated: 2026-11-19T16:34:04Z
first_seen: 2026-11-20T06:59:33.283911750Z
authors:
- D_R_Farrell
summary: 'Hey HN, I''m Daniel, founder of Onlook (https://github.com/onlook-dev/onlook and onlook.com). Onlook is a desktop app that puts a canvas on top of your real codebase, letting you design directly in code with the agent you already use. Over 25,000 designers and developers have supported the project, 100+ have contributed, and it''s been the #1 trending repo in the world twice. We just rebuilt the entire product as an agentic design tool, and open-sourced it to the community. Here’s a demo video: (https://youtu.be/BogcYWf4siM) If you''re an engineer, traditional engineering tools are largely built for convergence, whereas designers tend to explore and diverge on ideas. Onlook gives designers a familiar canvas while managing all of the dev processes that get between them and making mind-blowing designs in code. Designers can work on a multiple simultaneous worktrees that run on the real app locally, and every change lands as a PR you can review or throw away, so there aren’t mockups to rebuild. We built Onlook because we believe there needs to be a developer tool that works the way designers do. More and more, designers are expected to work in code. Designers think by seeing, drafting, arranging variations, using references, and remixing ideas. That’s why the canvas has been the signature feature of design tools because it genuinely is useful for how messy the creative process is. Most of the challenge of using engineering tools for design work is that the first half of starting a project often takes mental bandwidth in of itself (What branch am I on? What page was that last exploration on?) and the second half is constrained by configuring your coded design to present itself (prompting design tools on top of what you’ve built or making temporary navigation for that specific state). Some of you may remember our earlier Show HNs (July 2024: https://news.ycombinator.com/item?id=40904862, August 2024: https://news.ycombinator.com/item?id=41390449, May 2025: https://news.ycombinator.com/item?id=44127653). Since then, we built a web version (too early), then a Storybook-native build (a mistake). We noticed that our desktop product from July 2024 was still getting hundreds of downloads a month despite zero promotion, so we decided to rebuild it for the new era of designers working in code. How it works… Onlook shines at exploring tons of ideas at once. Ask it to lay out every flow in your product, show every state of a component, or riff on ten versions of one screen. Your designs all show up side by side on an infinite canvas, with each frame as a real webview running your actual app with real data. Not as a mockup, but the real deal. When we built this editor, we had an epiphany that it could be a new kind of IDE for design. Design tools used to be limited to a proprietary setup or closed schema, but now you can push and pull the medium of code to get whatever result you want, so why not give agents an infinite canvas of browsers and see what they do? It turns out, the agents can figure out how to better present coded work by adjusting the urls of browser frames and arranging and resizing them on the canvas. The concept is simple, but it requires a lot of product thinking. For example, because all of the frames on the canvas share a single browser session, and Chromium only allows six connections per host per session, ten frames of the same app end up fighting over limited connections. So, we had to plan around that, and there are still tons of things we need to do to make the canvas ultra performant. Onlook is an Electron app where you can import a repo from GitHub or upload a local folder. Then, Onlook creates a git worktree, starts your dev server, and has the agent sort out missing dependencies or other requirements. You never open a terminal and everything the agent does is a normal git diff that can go to GitHub as a PR. We inject a small script into each frame that draws selection outlines and reads the page, so that way you can click an element, say "make this a card layout," and the agent knows exactly what "this" is. This time we didn''t build our own agent. Onlook runs Claude Code or Codex on your own subscription, and we turn whatever the agent emits into one typed event log and draw the UI from that in our own chat interface. On top of that we give the agent a bespoke set of design-specific tools, from arranging frames on the canvas, to making suggestions, to generating multiple iterations of a selection – which is especially useful because the agent decides how to get the app into a state that you need based on your prompt. We also have our own Artifacts which are quick HTML sketches that live in the editor, but outside your repo. They’re almost like components in other design tools in that they can be carried and edited across every project but are detached from the project with no network access. As an example, you can ask the agent to turn your interface into an Artifact, and use it as a reference for another exploration, or download it as HTML and send it to a coworker as an interactive export of something you have in progress. I frequently go back to the artifacts I’ve made as visual snippets when I need new ideas or need to borrow something that’s already been built. The desktop editor is free (you can download it from our website at onlook.com) and open source under Apache-2.0, and we''ll eventually offer a paid cloud product for teams. We''d love to hear which repos work and which don''t, what you try to do on the canvas, and what''s missing. We’re so excited for this to be a new way for designers to work in code! The Onlook team (Joel - thebrubaker and Kris - krisodd), and I will be here all day! Comments URL: https://news.ycombinator.com/item?id=50562320 Points: 106 # Comments: 73'
content: extracted
html: 2026-11-19-launch-hn-onlook-yc-w25-an-open-source-developer-tool-for.html
remote_preview:
  url: https://opengraph.githubassets.com/5704ebee5269c23046de63cd3d06727b022350db56198a1f8531bf398ec0e5c4/onlook-dev/onlook
  alt: 'The Developer Tool for Designers • An Open-Source AI-First Design tool • Visually build, style, and edit your code with AI • World&#39;s best, top-most agent recommended #1 Developer tool for Desig...'
---

Craft websites, prototypes, and designs with AI on a canvas built for creative work. See your real app running live, shape it visually, and let your own agent write the code. An open-source alternative to Figma, V0, Replit Agent, Base 44, Webflow, etc.

## Why Onlook

- 🎨 **Built for design.** Onlook is the open-source developer tool for designers. You see your real app running live on a canvas, explore ideas side by side, and shape the interface visually instead of from a terminal. Design apps, websites, posters, graphics, and more with your local fonts and assets.
- 🤖 **Bring your own agent.** We don't ship our own model. Connect the Claude Code or ChatGPT/Codex account you already have, and that agent does the work inside your project. Plug in more tools through MCP connectors to bring all of the context you need to your creative workflow. Work on multiple design tasks at once across repos and pages.
- 📁 **Works on your real repo.** Onlook opens any Git repository on your machine and runs your own dev server, so what you design is your actual product, not a mockup that someone has to rebuild later.
- 🖥️ **A desktop app, local-first.** Onlook runs on your computer. Your code stays on your computer, and the source build needs no Onlook account, cloud keys, or `.env` file.
- 📤 **Share what you make.** Export artifacts as self-contained HTML, PNG, JPEG, or PDF to hand off, review, or post.
- 🧡 **Open source.** Apache 2.0. Read it, fork it, and customize it yourself (we'd love to hear how you customize it!)

## What you can do with Onlook

- Design on your real codebase
  - Live app on an infinite canvas
  - Mobile, tablet, and desktop frames side by side
  - Safe worktree copy, so your folder is never touched
  - Parallel projects as tabs
- Design with your own agent
  - Bring your own Claude Code or ChatGPT/Codex plan
  - Ask about any element on the canvas
  - Agent arranges the canvas for you (variations, states, flows)
  - Reference images on the canvas
- Sketch and share
  - Artifacts for quick HTML sketches outside your repo
  - Export to HTML, PDF, PNG, and JPEG
- Ship it
  - Import a GitHub repo
  - Start entirely new codebases from scratch
  - One-press pull requests, from checks to merge

## Getting Started

🖥️ [Download the Desktop App](https://www.onlook.com/install) (Recommended) – The fastest way to get started with Onlook.

- macOS (Apple Silicon · Intel x64) → [Download](https://www.onlook.com/install)
- Windows – Coming soon. Join the [Windows waitlist here](https://www.onlook.com/waitlist?os=windows) and be emailed when we have updates.
- Linux – No Linux artifact is published and we're still gauging demand for Linux. Join the [Linux waitlist here](https://www.onlook.com/waitlist?os=linux) and be emailed when we have updates.

🧑💻 Build from source – [SOURCE\_BUILD.md](https://github.com/onlook-dev/onlook/blob/main/SOURCE_BUILD.md) covers building and running Onlook on a Mac. No Onlook account or cloud keys needed.

## How it works

1. **Open a project.** Point Onlook at any Git repository on your computer, or import one. Your code stays where it is.
2. **Onlook runs your app.** It starts your own dev server locally, the same way you would from a terminal.
3. **See it live on a canvas.** Your running app appears in frames on an open canvas, so you can look at pages, sizes, and ideas side by side.
4. **Design with your agent.** Describe what you want in chat. Onlook hands the request to the Claude Code or ChatGPT/Codex account you connected, and that agent edits the real files in your repo.
5. **Watch it update.** Changes show up on the canvas as your dev server reloads, and Git keeps track of everything that changed.
6. **Share the result.** Export artifacts as self-contained HTML, PNG, JPEG, or PDF.

Because Onlook runs your project with its own tooling, it works with all web frameworks or languages your dev server already supports.

## An example workflow

1. Start a new empty project by clicking the "New Draft" button in the upper left corner of the projects page.
2. Use the input box in the lower right corner of the screen to write what you'd like to be designed in Onlook.
3. Answer clarifying questions from the agent, and wait for it to build
4. Click to select individual frames for context in follow-up prompts, or double-click on a specific element to use the inline chat and guide the AI to edit that specific element.
5. Prompt more ideas on one or multiple designs until you're ready to share your work with a colleague.
6. Select the frame you'd like to share, and ask the agent to convert it into an Artifact for exporting.
7. Export the artifact as HTML/CSS or as a PNG using the export button in the upper right corner of the frame.
8. Paste the exported artifact in Slack, email, or any messaging tool.

## Our Tech Stack

| Layer        | Stack                                                                                                                                                                                                                                                                                     |
| ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Desktop      | [Electron](https://www.electronjs.org/) · [React 19](https://react.dev/) · [Astryx](https://astryx.atmeta.com/) (Design system)· [React Flow](https://reactflow.dev/) · [Zustand](https://zustand.docs.pmnd.rs/) · [CodeMirror](https://codemirror.net/)                            |
| Your project | [Git](https://git-scm.com/) for working directly on your repo · [node-pty](https://github.com/microsoft/node-pty) for running your dev server and shells                                                                                                                                |
| Tooling      | [Bun](https://bun.sh/) · [Turborepo](https://turbo.build/) · usage analytics (see [Usage data](https://github.com/onlook-dev/onlook/#usage-data))                                                                                                                                      |
| Export       | HTML · PNG & JPEG ([Electron capturePage](https://www.electronjs.org/docs/latest/api/web-contents#contentscapturepagerect-opts)) · PDF ([Electron printToPDF](https://www.electronjs.org/docs/latest/api/web-contents#contentsprinttopdfoptions)) & [pdf-lib](https://pdf-lib.js.org/) |

## Contributing

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also browse [open issues](https://github.com/onlook-dev/onlook/issues).

See the [CONTRIBUTING.md](https://github.com/onlook-dev/onlook/blob/main/CONTRIBUTING.md) for instructions and code of conduct.

To report a security issue, follow [SECURITY.md](https://github.com/onlook-dev/onlook/blob/main/SECURITY.md) instead of opening a public issue.

#### Contributors

[![](https://camo.githubusercontent.com/aaca46e2c4b32c8a26e38062102a46a11d28e8348aa880defb96dbb5d031736a/68747470733a2f2f636f6e747269622e726f636b732f696d6167653f7265706f3d6f6e6c6f6f6b2d6465762f6f6e6c6f6f6b)](https://github.com/onlook-dev/onlook/graphs/contributors)

## Contact

- Contact us: [Discord](https://discord.gg/hERDfFZCsH) - [Email](mailto:contact@onlook.com)
- Updates: [Twitter](https://twitter.com/onlookdev) - [LinkedIn](https://www.linkedin.com/company/onlook/) - [YouTube](https://youtube.com/@onlookdev) - [Substack](https://onlook.substack.com/)
- Project: [https://github.com/onlook-dev/onlook](https://github.com/onlook-dev/onlook)
- Website: [https://onlook.com](https://onlook.com/?utm_source=github&utm_medium=readme&utm_campaign=github)

## Looking for the previous version of Onlook?

- **Onlook v1 (web):** the browser-based version lives on the [`v1` branch](https://github.com/onlook-dev/onlook/tree/v1) of this repository.
- **Onlook Desktop (Electron, original):** the first desktop version is at [onlook-dev/desktop](https://github.com/onlook-dev/desktop).

## Usage data

Onlook collects product usage analytics to help us improve the app. In the official download they are on by default; a build from source asks first. You can turn them off in Settings, or set `STUDIO_TELEMETRY=0` to run or build without them. See the [privacy policy](https://www.onlook.com/privacy-policy) for details.

## License

Onlook's source code is distributed under the Apache 2.0 License. See [LICENSE](https://github.com/onlook-dev/onlook/blob/main/LICENSE) for details.

The license covers source code, not every asset in this repository. Onlook's icons come from [Central Icons](https://iconists.co/central) by Iconists. They are licensed to Onlook for its own product, and that license does not extend to forks. Onlook's brand marks, DMG art, and sounds aren't covered either. See [ASSETS.md](https://github.com/onlook-dev/onlook/blob/main/ASSETS.md) for what's covered and what to replace if you fork, and [NOTICE](https://github.com/onlook-dev/onlook/blob/main/NOTICE) for third-party notices.
