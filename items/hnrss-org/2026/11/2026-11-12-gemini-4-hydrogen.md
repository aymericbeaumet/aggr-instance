---
title: Gemini 4 Hydrogen
link: https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-4-hydrogen/
source: hnrss-org
published: 2026-11-12T18:01:48Z
updated: 2026-11-12T18:01:48Z
first_seen: 2026-11-12T18:49:11.379952736Z
authors:
- dvaplima
content: extracted
html: 2026-11-12-gemini-4-hydrogen.html
remote_preview:
  url: https://storage.googleapis.com/gweb-uniblog-publish-prod/images/gemini-4-hydrogen__key-art__lockup__16-9.width-1300.jpg
---

Hydrogen brings cutting-edge intelligence to everyone.

* * *

Today, we’re introducing Gemini 4 Hydrogen, bringing cutting-edge intelligence to everyone — with speed and efficiency for everyday coding and agentic workflows.

While our frontier models, [Gemini 4 Carbon](https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-4-carbon-now-available/) and [Gemini 4 Argon](https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-4-argon/), excel at deep, long-horizon reasoning on your hardest problems, Hydrogen is built for rapid execution across diverse agentic workflows. With Hydrogen, we’re also bringing Gemini 4 to everyone — for all tiers of [Google AI Studio](https://aistudio.google.com/prompts/new_chat?model=gemini-4-hydrogen), the Gemini API, [Gemini for business](https://cloud.google.com/gemini-enterprise), the [Gemini app](http://gemini.google.com/), [Google Search](https://blog.google/products-and-platforms/products/search/gemini-4-hydrogen-search-upgrade), and paid Google AI Pro and Ultra subscribers in [Google Antigravity](https://antigravity.google/).

Hydrogen surpasses Argon on the [Artificial Analysis Intelligence Index](https://artificialanalysis.ai/) at its default high thinking level (51 vs. 50, and reaching 54 at its highest xhigh thinking setting), while often completing tasks faster than models in its class. Gemini 4 Hydrogen is priced flat across its entire 1 million-token context window at $1.50 per million input tokens and $7.50 per million output tokens.

## Unlocking efficiencies in multi-model workloads

In real-world software development, while teams rely on frontier models like Carbon and Argon for deep architectural judgment when designing systems or untangling subtle bugs, they still need a fast, disciplined execution model for writing code, running tests, and verifying changes. Hydrogen is built to stand on its own as a model for everyday work — or to pair with Carbon and Argon as the high-efficiency execution engine in multi-model workflows.

To help make this multi-model collaboration automatic, we’re introducing [Orchestrator in Google Antigravity](https://antigravity.google/blog/gemini-4-hydrogen-in-google-antigravity). Rather than using a single model from start to finish, Orchestrator dynamically switches between models like Carbon and Hydrogen within the same session. On [Terminal-Bench 4.0](https://www.tbench.ai/news/terminal-bench-4-0), Orchestrator reached near-Carbon task success rates while, on average, consuming 24% less quota and completing 19% faster. For workflows that require parallel agents, Carbon can act as the coordinator while Hydrogen can power worker and verifier agents to build and test changes simultaneously across isolated workspaces — helping teams ship complex projects faster without driving up token costs.

## Scaling developer and enterprise workflows

![Benchmark chart comparing Gemini 4 Hydrogen to other models](https://storage.googleapis.com/gweb-uniblog-publish-prod/images/gemini-4-hydrogen_benchmarks_tab.width-2000.format-webp.webp)

Beyond multi-model workflows, Hydrogen is a major standalone upgrade for teams migrating from Gemini 3.8 Flash. It delivers significant leaps in both speed and intelligence — finishing tasks 2.5 times faster at its high default setting while more than tripling [Terminal-Bench 4.1](https://www.tbench.ai/) scores (57% vs. 18.2%), cutting fabricated claims by more than 5x, and adapting cleanly when a tool call fails or a search returns empty. It also matches or outperforms competing models in its class across core developer and enterprise workflows:

- **Iterative repository coding and debugging:** Hydrogen handles issue resolution, debugging, rapid iteration, and codebase Q&A with focused, surgical edits — delivering cleaner, production-ready code without rewriting unrelated files. It outperforms similar-tier models on SWE-sweep where it has to discover and repair bugs without breaking existing code. To explore large repositories efficiently, the model excels at natively batching file-read and search calls in parallel rather than inspecting files one by one; in early testing across complex graphs, SQL, and data-heavy enterprise workflows, our partners have seen Hydrogen run 2x to 2.8x faster while reducing token usage by 12% to 23%, compared to 3.8 Flash.
- **Open-ended agentic execution:** On multi-step tasks like deep research, data analysis, and broad codebase navigation, Hydrogen ranks #5 on [Agent Arena](https://arena.ai/leaderboard/agent), arena.ai’s live benchmark. For deep domain reasoning and analysis, it leads its class on HLE-Diamond (58.8%), ranks #1 overall on [Vals Tax Agent Bench](https://www.vals.ai/benchmarks/tax_agent_bench) (52.3%), ties for #1 on the [Vals Legal Research Bench](https://www.vals.ai/benchmarks/legal_research) (55.8%), and leads its class on [AutomationBench-AA](https://artificialanalysis.ai/evaluations/automationbench-aa).

- **Multimodal knowledge work:** Hydrogen pairs native 1 million-token input with up to 1 million output tokens to synthesize dense PDFs, spreadsheets, charts, audio, and recorded video into polished, cited reports and structured deliverables. It outperforms other models on the new [Multimodal Knowledge Agent (MKA)](https://blog.google/innovation-and-ai/models-and-research/gemini-models/multimodal-knowledge-agent-benchmark/) benchmark (53.4%) for end-to-end agentic knowledge work over complex visual files, and on [Agents’ Last Exam v1.1](https://agents-last-exam.org/leaderboard) (71.6%) for end-to-end professional workflows.
- **Web development**: On tasks where speed matters to developers, like live UI prototyping, design-to-code iteration, and interactive app building, Hydrogen generates more functional, feature-complete layouts in fewer prompts.

## Adaptable speed and intelligence across four thinking levels

![a chart showing Artificial analysis intelligence index](https://storage.googleapis.com/gweb-uniblog-publish-prod/images/gemini-4-hydrogen_aa-index_score.width-2000.format-webp_6vbdEDq.webp)

We built Gemini 4 Hydrogen for real-world usability. Hydrogen’s four thinking levels — low, medium, high (default), and a new xhigh tier — offer a wide range of options across quality, latency, and cost, scaling in a smooth, predictable progression so you only wait and pay for the reasoning depth a task requires. Across thinking levels, Hydrogen matches or outperforms some of the competing models in its class while often finishing tasks faster and using fewer output tokens.

![a chart showing AutomationBench-AA](https://storage.googleapis.com/gweb-uniblog-publish-prod/images/gemini-4-hydrogen_automation-ben.width-2000.format-webp.webp)

![A chart showing AA Index 5.01](https://storage.googleapis.com/gweb-uniblog-publish-prod/images/gemini-4-hydrogen_aa-index_graph.width-2000.format-webp.webp)

![A chart showing Terminal-Bench 4.0](https://storage.googleapis.com/gweb-uniblog-publish-prod/images/gemini-4-hydrogen_t-bench_graph.width-2000.format-webp.webp)

![a chart showing HLE-Diamond](https://storage.googleapis.com/gweb-uniblog-publish-prod/images/gemini-4-hydrogen_hle_graph.width-2000.format-webp.webp)

## Fast, everyday help in the Gemini app and Search

For consumers, Gemini 4 Hydrogen makes it easier to get things done, staying on track and following instructions more closely across multi-turn conversations.

In the Gemini app, Hydrogen makes your everyday assistant even more helpful — it’s better at following your guidance, calling the right tools, and getting you to a reliable output with less back-and-forth.

- **Ready-to-go writing, research, and everyday work:** Hydrogen sticks closely to your tone, format, and specific rules, so email drafts, study guides, rewrites, and documents come back ready to use on the first try. When you are comparing products or synthesizing dense material, it pulls together clear tables and visual breakdowns without dropping key details.\
- **Connects the dots and gets more tasks done:** Hydrogen breaks down your goal step-by-step, pulling relevant context from your uploads, Google apps like Gmail and Google Drive, and Connected Apps like Airtable and Dropbox. When you start a task in Gemini, it also more frequently calls the right tools and skills for the job, helping save you time as you check things off your to-do list.\
- **Everyday prototyping and iteration:** For exploring and testing ideas, Hydrogen generates clean, working layouts in fewer prompts, making it easy to prototype and refine interactive projects quickly, like synthesizing real-time global lightning strike data into an interactive 3D globe.

Hydrogen is significantly upgrading [Search](https://blog.google/products-and-platforms/products/search/gemini-4-hydrogen-search-upgrade), delivering more helpful, intuitive responses for all users globally.

- **Better understands your true intent:** With Hydrogen, you can ask your question in your natural flow of thought — even if you have multiple parts to your query, hard-to-understand typos, or highly specific requests — and Search will intelligently comprehend what you’re looking for.
- **More visual responses:** You’ll get highly adaptive, visual, and easy-to-scan responses that are uniquely relevant to your needs, at the speed you expect from Search.
- **Wider range of relevant links:** With Hydrogen’s advanced reasoning, we’re also upgrading our query fan-out technique in Search, connecting you to a broader range of helpful links than before, helping you find sites you previously might not have discovered.

![A smartphone screen with a side by side comparison of Gemini 3.5 Flash and Gemini 4 Hydrogen in a search for fixing an apartment](https://storage.googleapis.com/gweb-uniblog-publish-prod/images/Gemini4Hydrogen-01-Studio_Apt-fu.width-2000.format-webp.webp)

![A smartphone screen with a side by side comparison of Gemini 3.5 Flash and Gemini 4 Hydrogen in a search of neighborhoods in Boston](https://storage.googleapis.com/gweb-uniblog-publish-prod/images/Gemini4Hydrogen-03-Boston-full.width-2000.format-webp.webp)

![A smartphone screen with a side by side comparison of Gemini 3.5 Flash and Gemini 4 Hydrogen in a search for sneakers](https://storage.googleapis.com/gweb-uniblog-publish-prod/images/Gemini4Hydrogen-02-Sneakers-full.width-2000.format-webp.webp)

## Designed for safe, dependable execution at scale

Gemini 4 Hydrogen brings improvements from our latest Gemini 4 safety and security stack to our broadest audience yet, combining built-in training with real-time surface safeguards to help mitigate both misalignment and misuse. Beyond foundational protections, we focused on two areas that matter most for a faster model that’s for everyday work:

- **Safe, aligned, and secure agentic workflows:** While 3.8 Flash has strong alignment and safeguards, Hydrogen is a leap ahead. It reduces overreach in agentic tasks by up to 81% and under-service by up to 79%, and cuts misaligned actions and task shortcutting across tool-use and coding evaluations by 12x. When faced with impossible or blocked tasks, it is up to 2x less likely to bypass permission boundaries and achieves a 96.7% honesty rate on our internal agent honesty evaluation.

  Building on our commitment to [reasoning transparency](https://institute.deepmind.com/essays/the-case-for-reasoning-transparency/), Hydrogen also preserves 99.9% chain-of-thought legibility with zero deceptive reasoning edits across more than 200,000 audited turns. Against indirect prompt injection attacks, Hydrogen is more than 2.3x more resilient on the Gray Swan IPI benchmark, while significantly improving robustness across tool use, computer use, and coding agents.

- **Helpful, reliable everyday interactions:** Across our product surfaces, we tuned Hydrogen to make everyday conversations more reliable and safe. Our updated safeguards are also much more precise at distinguishing between real biosecurity risks and legitimate medical or scientific questions — keeping false refusals on HealthBench Pro under 0.4%.

For more details see the Gemini 4 Hydrogen [model card](https://storage.googleapis.com/deepmind-media/Model-Cards/Gemini-4-Hydrogen-Model-Card.pdf).

## Gemini 4 Hydrogen, available to everyone today

Starting today, Hydrogen is rolling out globally across our developer, enterprise, and consumer surfaces:

- **For developers:** Build with the Gemini API in [AI Studio](https://aistudio.google.com/docs/whats-new-gemini-4-hydrogen) and the paid tier in [Android Studio](https://developer.android.com/studio), try it in [AI Studio](https://aistudio.google.com/prompts/new_chat?model=gemini-4-hydrogen) and if you’re a paid Google AI Pro or Ultra subscriber, you can try it in [Google Antigravity](https://antigravity.google/).
- **For enterprises:** Deploy it in [Gemini for business](https://cloud.google.com/gemini-enterprise) via the API or model drop down, Google Antigravity, and in Gemini in Google Sheets for eligible business subscriptions.
- **For everyone:** Start using it in the [Gemini app](http://gemini.google.com/) model drop down [1](https://blog.google/innovation-and-ai/models-and-research/gemini-models/introducing-gemini-4-hydrogen/#footnote-1) and in [Google Search](https://blog.google/products-and-platforms/products/search/gemini-4-hydrogen-search-upgrade) for all users, and in Gemini in [Google Sheets](http://sheets.new/) starting with Google AI Pro and Ultra subscribers.

## Get the latest news from Google in your inbox

Sign up for our newsletters with product updates, event information, special offers, and more.

Your information will be used in accordance with [Google's privacy policy.](https://policies.google.com/privacy) You may opt out at any time.
