---
title: You can't whisper at an AI agent
link: https://stripe.dev/blog/ai-steering-experiments
source: stripe-com
published: 2026-05-14T00:00:00Z
updated: 2026-05-14T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- James Beswick
summary: What a dozen steering experiments taught us about how LLM-based agents actually behave in the wild.
content: extracted
html: 2026-05-14-you-can-t-whisper-at-an-ai-agent.html
remote_preview:
  url: https://stripe.dev/og/blog/ai-steering-experiments.png
  alt: 'Stripe Developer Blog: You can''t whisper at an AI agent'
---

There's a comforting mental model that many of us carried into the age of AI agents, that they'd behave like diligent junior engineers. All you have to do is hand them a codebase, point them at the docs, and they'd read everything, follow best practices, and ask clarifying questions when stuck. However, after running a series of agent steering experiments at Stripe over the past several months, the reality is both messier and more interesting. Agents were integrating with older APIs and were actively making decisions that were contrary to Stripe’s recommended best practices without the knowledge of the users.

We tried a lot of things. We modified SDKs. We added warnings to API responses. We restructured skill files. We placed install prompts on CLI login screens and docs pages. Some of these worked remarkably well. Others failed in ways that forced us to rethink our assumptions about how agents process context and, more fundamentally, about what "steering" even means when your user is a language model.

This post walks through what we found, what surprised us, and the two conclusions we think matter most for anyone building developer tools in an agent-first world.

### The experiments

A quick sketch of the landscape. Our team ran roughly a dozen experiments aimed at a single question: how do you get an AI agent to use Stripe correctly? "Correctly" here means using current API versions, following integration best practices, and leveraging the Stripe skill. This is a structured set of instructions and references that agents can consume to understand our platform.

The experiments fell into three broad categories:

**Passive hints.** We tried embedding guidance in places agents might encounter it organically, adding warning hashes in API responses, steering cues in SDK source files, and `AGENTS.md` files in package directories. The idea was that agents, like good developers, would notice these signals and adjust.

**Active prompts.** We restructured how skill files were organized (progressive disclosure versus monolithic blobs), added agent-specific sections to CLI help output, and surfaced skill install commands during authentication flows and onboarding.

**Distribution plays.** We put skills on [docs.stripe.com](http://docs.stripe.com), added install buttons to documentation pages, and ran an onboarding experiment offering users an "AI/agent" path alongside the traditional dashboard flow.

The results split very cleanly. Passive hints failed while active prompts and distribution plays worked, some of them far better than expected.

### What failed, and why it matters

The two clearest failures were SDK steering and API warning responses.

For SDK steering, we forked Stripe SDK packages and added steering cues: modified READMEs, `AGENTS.md` files in the package root, inline comments nudging toward best practices. The hypothesis was reasonable, namely that agents frequently inspect project files and dependencies to build context. But in practice, agents almost never read files inside dependency directories. The steering cues were invisible so we ended the experiment.

For API warnings, we added a new `warn` hash to API responses, a soft signal indicating that the agent was doing something suboptimal (using a deprecated parameter, for instance). We also embedded skill install instructions inside compatibility-mode error messages. Again, agents didn't respond. They parsed the API response for the data they needed, ignored the warning, and moved on.

This pattern of agents ignoring soft signals is consistent with what researchers at Princeton's SWE-bench project have documented. In their evaluations of agents on real GitHub issues, successful agents tend to exhibit a narrow, goal-directed focus: they identify the immediate task, locate the relevant code, make the change, and move on. They don't browse. They don't explore. Devin's published case studies show a similar pattern. Cognition's work on Devin illustrates the same pattern that agents optimize for task completion, not comprehension ([cognition.ai, 2024](https://www.cognition.ai/blog/introducing-devin)). Our warning-based steers assumed agents would behave like curious developers poking around a new API. But they don't. They behave like contractors on a deadline.

This is an important finding for anyone building developer infrastructure. If your developer experience strategy relies on agents discovering guidance through exploration, such as reading changelogs, scanning deprecation notices, browsing adjacent files, it will likely fail. Agents simply don't wander, and you have to put instructions directly in their path.

### What worked

Three results stood out.

**Progressive disclosure in skill files.** We compared two formats for the Stripe skill: a single monolithic file containing everything, versus a modular structure where the top-level skill referenced sub-skills that could be loaded on demand. The modular format outperformed the monolith by roughly 10% across our eval suite. It also reduced token usage meaningfully since agents pulled in only the context they needed instead of consuming the entire file.

This tracks with findings from Anthropic's research on long-context performance. As context windows have grown, a persistent finding is that models exhibit degraded attention to information in the middle of long documents — the so-called "[lost in the middle](https://arxiv.org/abs/2307.03172)" problem identified by Liu et al. at Stanford. Monolithic skill files are particularly vulnerable to this: the guidance for payments might be sharp, but by the time the model reaches the billing section three thousand tokens later, attention has decayed. Progressive disclosure sidesteps the problem by keeping each loaded context small and focused.

There's a secondary benefit, too. Modular skill structures let individual product teams own their sub-skills. The payments team maintains the payments skill; the billing team maintains theirs. This mirrors how we already organize API reference documentation and has obvious maintenance advantages at scale.

![A tweet showing the Stripe skill install prompt](https://stripe.dev/images/ai-steering-experiments/tweet.png)

**CLI login promotion.** We added a prompt on the Stripe CLI login confirmation page (the screen users see after running `stripe login`) suggesting they install the Stripe skill with a one-click `npx` command. 30-35% of users who saw the prompt copied the command. That's a startlingly high conversion rate for what amounts to a text-based interstitial. It suggests that developers using the CLI are already in an agent-adjacent mindset (many of them are probably authenticating *because* an agent told them to) and are receptive to tooling that makes the agent work better.

**Error-based steering.** We ran an experiment using API compatibility mode. When new merchants hit the API with an outdated version, they received an explicit error rather than a degraded response. In test conditions, agents reliably detected the error, identified the version mismatch, and corrected their request. This is a meaningful contrast with the warning-based approach, which agents ignored. The difference is simple: errors block progress but warnings don't. An agent that hits an error *must* deal with it to complete its task. A warning is, from the agent's perspective, often just noise.

### Two conclusions

We are early in implementing and understanding how to steer agents but at this stage, here's what we think generalizes.

**First, agent-facing developer experience is a distribution problem, not a content problem.** The Stripe skill itself has been solid for a while. The instructions are clear, the references are well-structured, the coverage is broad. The constraint was never "is the content good enough?" It was "does the agent actually have this content loaded?"

Every experiment that succeeded was ultimately a distribution win: getting the skill installed, getting the right sub-skill loaded at the right time, getting the install command in front of developers at moments of high intent. This is surprisingly analogous to the classic mobile app distribution challenge. You can build the best app in the world, but if nobody installs it, it doesn't exist. For skills, the funnel is: awareness → install → load → follow. Most of our wins came from improving the first two steps.

**Second, the distinction between "hard" and "soft" steering is probably the most important design axis for agent-facing infrastructure.** Hard steers, such as errors, explicit instructions in loaded context, and blocking responses, work. Soft steers - warnings, hints, adjacent files, in-band suggestions - often don't. This has architectural implications. If you're designing an API that agents will consume, your error messages aren't just debugging aids but your primary steering mechanism. If you're building a CLI, the `--help` text isn't a nice-to-have - it's potentially the only thing the agent reads before acting. If you're writing documentation, the question isn't whether the information is *available* somewhere on the page but whether it's in the text that the agent will actually load into context.

This is a different design philosophy than what most developer tools teams have practiced for the past decade. Human-facing DX is forgiving. Developers browse, search, skim, and cross-reference. They'll find the deprecation notice eventually. They'll read the migration guide if they hit enough friction. Agent-facing DX is not forgiving. The agent loads context once, executes, and moves on. If your guidance wasn't in the loaded context, it didn't happen.

### Where this goes

We're still early. The real opportunity is fully agentic onboarding: flows where the agent doesn't just receive better instructions but actively drives the setup process end-to-end. Getting there requires rethinking parts of our dashboard-based flows and making UX changes that treat agents as first-class users, not just human users with better memory. That work is underway.

The CLI DX evaluation surfaced issues we're still working through. We published a [blog post](https://stripe.com/blog/can-ai-agents-build-real-stripe-integrations) earlier this year on agent benchmarking. We are continuing to build on the setup to enable continuous evaluation and improvement of agent performance on a variety of real integration tasks. We continue to add new best practices for our users who are building with LLMs in our [documentation](https://docs.stripe.com/building-with-ai).

But the directional picture feels clear. The agents are here, they're building real integrations, and the tools and infrastructure we've built for human developers don't automatically transfer. The agents are focused, literal, and impatient. They don't read the room, they read only what’s in the context window. Now we know we have to make sure the right things are there.

* * *

*The experiments summarized here were run by engineers across Stripe's developer experience and agent platform teams during early 2026. For details on our agent benchmarking methodology, see [Can AI Agents Build Real Stripe Integrations?](https://stripe.com/blog/can-ai-agents-build-real-stripe-integrations) on the Stripe blog.*
