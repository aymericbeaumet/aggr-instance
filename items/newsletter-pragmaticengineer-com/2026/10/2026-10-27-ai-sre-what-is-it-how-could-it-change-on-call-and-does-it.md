---
title: 'AI SRE: what is it, how could it change on-call – and does it work?'
link: https://newsletter.pragmaticengineer.com/p/ai-sre
source: newsletter-pragmaticengineer-com
published: 2026-10-27T16:19:22Z
updated: 2026-10-27T16:19:22Z
first_seen: 2026-10-27T20:37:14.562886086Z
authors:
- Gergely Orosz
summary: “AI SRE” is fast becoming a trending term fueled by flashy demos from vendors, but has an AI SRE ever really been on-call while engineers slept? Deepdive
content: extracted
html: 2026-10-27-ai-sre-what-is-it-how-could-it-change-on-call-and-does-it.html
remote_preview:
  url: https://substackcdn.com/image/fetch/$s_!v-ox!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2fa0f8f5-2796-4db4-91f1-7ab4b45fb8fd_1350x970.png
---

“AI SRE” is fast becoming a trending term fueled by flashy demos from vendors, but has an AI SRE ever really been on-call while engineers slept? Deepdive

*Before we start: I’m in San Francisco this week and also next week. On Wed, 4 November, I’m hosting **[The Pragmatic Engineer Unplugged](https://luma.com/pragmatic-engineer-unplugged?tk=fpY2qp)** at the iconic SFJazz Club, featuring a rare conversation with Diogo Almeida (creator of Jev & co-inventor of ChatGPT), and Bryan Cantrill (cofounder and CTO of Oxide). It’s free to attend with registration, [sign up here.](https://luma.com/pragmatic-engineer-unplugged?tk=fpY2qp)*

[![](https://substackcdn.com/image/fetch/$s_!ugcF!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff18251d2-b83b-4479-a2a0-67682f0d2208_2048x2048.png)](https://substackcdn.com/image/fetch/$s_!ugcF!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff18251d2-b83b-4479-a2a0-67682f0d2208_2048x2048.png)

*Wednesday, 4 November in SFJazz: The Pragmatic Unplugged. [RSVP here](https://luma.com/pragmatic-engineer-unplugged?tk=fpY2qp)*

In May 2025 at Microsoft’s annual BUILD conference, the tech giant [introduced](https://techcommunity.microsoft.com/blog/azurepaasblog/introducing-azure-sre-agent/4414569) “Azure SRE Agent” with an onstage demo in which an alert fired and the AI SRE Agent went to work on identifying the root cause, and then shipped a fix, all without having to wake up a human dev oncall.

Watching this demo, the whole thing felt artificial to me, and I failed to imagine *any* engineering team at Microsoft – or anywhere else! – letting an AI make production decisions with no human oversight that could potentially cause a worse outage. And sure enough, after last year’s BUILD I heard no more about “SRE agents” at Microsoft or elsewhere – until recently, that is.

Now, I do hear it being mentioned: OpenAI [built](https://newsletter.pragmaticengineer.com/i/215812293/3-openais-agentic-software-factory) an AI SRE bot called Sevbot, and last month, an engineer at ING bank told me they’re working on their own, in-house AI SRE. Just as visibly, at tech conferences I’ve attended, AI SRE vendors’ stands have been popping up in numbers.

To find out what’s going on, we reached out with Jessica to three of the most popular AI SRE startups/AI incident platforms to hear directly from them: [Rootly](https://rootly.com/), [incident.io](http://incident.io), and [ResolveAI](https://resolve.ai/) (note: I’m [an investor](https://blog.pragmaticengineer.com/investing/) in [incident.io](http://incident.io)). *Thanks for all the input!*

Today, we cover:

1. **Why is AI SRE “hot**”? Incidents and pages are trending upward, thanks to AI agents speeding up at shipping code to production. It only makes sense to invest in better oncall tooling.

2. **What does “AI SRE” mean**? It’s mostly about doing incident investigation much faster, and inside of Slack. Whether “AI SREs” should make code changes is debated.

3. **Building an investigation agent**. Fanning out multiple agents is common, as is the practise of engineers steering them in Slack or Teams. A “context layer” with a knowledge graph and memory is used for better investigation outcomes. Several tech companies built their own investigation agents instead of relying on vendors.

4. **Making agents accurate**. Helpful approaches include verifying incident hypotheses with adversarial agents and a mix of models, as well as running evals and backtesting. However, measuring “accuracy” of AI SREs is tricky and highly specific to each company, and even engineering team.

5. **Making agents fast & affordable**. Everyone wants fast, cheap, and accurate AI SREs, but there are tradeoffs. Model routing, lean prompts, and tracking cache hit rate are ways to engineer a system with the right tradeoffs for your constraints.

6. **How long until autonomous agents?** The fully autonomous agents demoed on conference stages are still not a reality, but AI agents triaging pager calls – and silencing unimportant ones – is becoming more common. Interestingly, there’s not been much change in this area from how Meta operated its own systems with AI two years ago.

7. **What’s changing for on-call engineers?** A new category of AI-related outages is appearing, and engineers should eventually get hit with fewer pages, but the incidents that do reach engineers will be hard. There’s a deskilling risk, and it could be smart to train oncall with incident response simulations like those from [Uptime Labs](https://www.uptimelabs.io/).

*As usual, I have no affiliation with vendors mentioned in this article except for my [investment](https://blog.pragmaticengineer.com/investing/) in incident.io – mentioned above. I have not been paid to mention any company.*

A year back, there was little to no buzz about AI SREs, but that’s all been changing.

**The main driver seems to be that there’s a** ***lot*** **more code deploys**. Almost all tech companies have seen their [PR frequency double in a year](https://newsletter.pragmaticengineer.com/p/whats-the-roi-of-software-engineers) due to AI agents. Let’s do some basic “reliability math”:

> i = c x p:\
> **I** ncident rate = \[**C** hange rate\] x \[**P** robability\]
>
> **C** = number of changes per unit time\
> **P** = probability a single change introduces failure

This says that if we have few deployments (low change rate) and high confidence that each deploy works (low probability of failure), then that is a pretty stable system. But with AI agents, deploy frequency is up and the incident rate is likewise increasing at most companies.

We know that C (the number of changes in a given timeframe) [is up twofold or more](https://newsletter.pragmaticengineer.com/p/whats-the-roi-of-software-engineers) on last year at many companies, and will probably climb higher. Data from Rootly shows that pages are also up from a year ago, and outside-business-hours pages have nearly doubled:

[![](https://substackcdn.com/image/fetch/$s_!y-4O!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6a2be186-4144-46d9-8613-0b315716a5be_2048x777.png)](https://substackcdn.com/image/fetch/$s_!y-4O!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6a2be186-4144-46d9-8613-0b315716a5be_2048x777.png)

Pages up: Higher deploy frequency responsible. Source: Rootly

Broadly, there’s two approaches to lowering P (incident failure rate probability):

1. Better pre-deploy verification: more testing & verification loops

2. Learn from incidents: make the system more resilient

Most AI SRE vendors are well placed to learn from incidents and act upon them. For example, here’s Rootly’s high-level strategy for more resilient systems:

[![](https://substackcdn.com/image/fetch/$s_!Z0Xr!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fda2f6881-574c-4977-95c9-7f26add722d8_2048x1012.png)](https://substackcdn.com/image/fetch/$s_!Z0Xr!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fda2f6881-574c-4977-95c9-7f26add722d8_2048x1012.png)

 *Less of P: Rootly’s approach for a lower incident rate*

“AI SRE” looks on its way to becoming the agreed term for AI-augmented oncall tooling, even though we’ve not spoken to a single person who likes it – and we agree.

**AI SRE vendors themselves don’t like it**. When we talked with incident.io, Resolve, and Rootly for this article, the reply was that, unfortunately, they’ve not come up with anything better and so this name has come to stick instead. Alternative names proved less sticky even though they’re less inaccurate, like “autonomous reliability agents”, “automated remediation agents”, and “automated incident investigation agents.”

A reason “AI SRE” is less than ideal is as simple as the fact that a site reliability engineer (SRE) does a lot more than just deal with oncall alerts! They work on making a system more reliable in its entirety and a good chunk of that work happens *before* and *after* outages.

Despite all that, this article is going with the flow and using “AI SRE” because there’s not much choice, while covering oncall and incident handling tools that are AI-augmented/AI-powered.

*We cover more on the SRE profession and reliability engineering [in a deepdive with Dave Connor,](https://newsletter.pragmaticengineer.com/p/reliability-engineering) who worked at Google when the term ‘SRE’ was coined.*

**AI SRE tools mostly help with the investigation phase of incidents**. Tools like Rootly, incident.io, and ResolveAI are most helpful after an alert has fired and an engineer starts investigating. Pre-AI, this investigation phase involved:

- **Alert details:** inspect the alert and try to figure out what it means

- **Dashboards**: check custom dashboards to confirm whether there’s an alert, which systems are impacted, and where the root cause investigation should start, if so

- **Runbooks**: consult runbooks and follow the steps to remedy the issue, if it’s a common one

- **Logs/observability**: look through the logs of observability systems like Datadog, Sentry, Honeycomb, etc

These days, AI tools have gotten pretty good at all these tasks if they have sufficient data access. Of the three vendors we talked with, all do their investigations with AI. When building in-house AI SRE tools, starting at the investigation phase is a no-brainer:

[![](https://substackcdn.com/image/fetch/$s_!v-ox!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2fa0f8f5-2796-4db4-91f1-7ab4b45fb8fd_1350x970.png)](https://substackcdn.com/image/fetch/$s_!v-ox!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2fa0f8f5-2796-4db4-91f1-7ab4b45fb8fd_1350x970.png)

Investigations first: engineers make the key decisions & the tool just supplies context faster

**Most AI SRE tools speed up the investigation of an incident**. incident.io’s agent orchestrator does something similar to when an oncall engineer opens eight tabs to look through what systems might have issues:

- Orchestration: the main agent decides to kick off an investigation

- Subagents: it fans out investigation work to subagents which report on progress

- Heuristics: the orchestrating agent can make speculative calls, then use heuristics to decide where to go deeper

There’s *a lot* of value in having an oncall engineer join the incident channel roughly five minutes after an alert fires, and to have a summary of investigations by AI agents. It gives the oncall dev a sense of what might be happening, instead of having to do all the “shovel work.”

**AI SREs typically interact with devs in Slack**. At startups that use Slack for comms, Slack is also *the* place to coordinate incidents; even pre-AI, it was common to open a dedicated Slack channel for an incident. Most AI SRE tools have a Slack bot in the incident channel; incident.io’s bot sits in the incident channel the whole time and takes steering instructions from engineers (look at the status of a specific system, ask it to dig up specific logs, etc.)

**AI SRE tools require a high level of trust, which is a reason to build in-house**. For these agents to be effective, they need access to telemetry, observability, logs, even the version control system because the more data they can access, the more effective they are. As Sam Newman writes in his book, ‘Building Resilient Distributed Systems’:

> “For tools like incident.io’s AI SRE to work, they need access to information. This means that they will need broad access to a lot of valuable data if you want them to be effective. If the agent can’t see your telemetry data, then it can’t slice and dice that data for you. If it can’t look at your source code repository, then it can’t spot recent commits that may have caused the issue. This does mean that you need a high degree of trust in the vendor creating and running the agent.”

But how much do *you* trust a third-party vendor with all your data; potentially even a readonly version of your sanitized production database, which could come in handy for some investigations? The larger the company, the less likely that access to highly sensitive data will be granted. The trust issue is a major driver in why many mid-size-and-above companies build their own AI SRE tools.

Even if a vendor offers zero data retention (ZDR), there’s still a risk that its system could be compromised and leak information. Also, a vendor offering ZDR could be at a disadvantage when improving the system, compared to an in-house AI SRE tool that it’s possible to train on the data.

*Our take is that AI SRE vendors are likely to become more capable than home-grown solutions because AI SRE startups obsess about better tooling fulltime; success depends on it. At larger companies, an AI SRE tool is more of a novel side project; engineers initially get excited but then move on to newer projects.*

**Should AI SREs** ***only*** **mitigate, or also remediate**? Once the root cause analysis of an outage is done, and the AI SRE agent has helped to mitigate it, is the work complete? On one hand, the AI SRE tool typically also has access to the codebase, so *should* be able to come up with code-fix suggestions. On the other hand, pre-AI, you hardly ever saw an SRE team raise pull requests to fix the root causes of outages because they didn’t have the *context* or *expertise* for systems someone else built. But with AI, context can be passed on and expertise in writing code is pretty common for AI agents.

We asked the three AI SRE startups how they approach writing code to fix root causes, post-incident. They have different ways:

- **Handing off PRs to coding agents.** incident.io is focused on passing incident context to engineers’ existing coding agents. The company built and demoed autonomous PR creation at its annual conference last year, but cofounder Chris Evans told us that customers generally prefer to use their own coding setups. The focus is therefore on giving agents the context they need to help engineers implement a fix.

- **A hybrid approach at ResolveAI.** The tool writes simple PRs itself and hands over larger changes to the customer’s coding agents.

- **Intend for agents to autonomously merge PRs at Rootly**. Their vision is to get to a point where their agent drafts a PR, and another agent judges whether it’s safe to merge, and when deemed safe, the merge happens automatically. If the merge is deemed risky, the oncall engineer still gets woken up. As yet, all PRs raised happen after a human has approved them.

All three vendors we talked to have rebuilt their investigation agent at least once because integrating AI into oncall looks simple, but that’s not the case. For example, ResolveAI built three different architectures in two years:

**Scripted workflow.** In 2024, models could not yet reliably chain tool calls. The startup built a pipeline of specific prompts like: “classify the alert“ → “gather logs+metrics“ → “extract insights” → “rank hypotheses.” This pipeline handled common incidents well, but did poorly when incidents did not “fit the script”.

**Single agent with a knowledge graph.** In 2025 when tool calling with AI agents became reliable, a primary agent took the alert → looked at a graph of services/dependencies/runbooks/deployments → decided what to do next. Overall, the accuracy improved but new problems appeared:

- **Context rot**: for long, multi-service incidents, the agent ran into context saturation (the “dumb zone” as discussed [on the Pragmatic Engineer podcast episode](https://newsletter.pragmaticengineer.com/p/context-engineering-with-dex-horthy) with Dex Horthy). It started to request data it already had and to contradict its own conclusions.

- **Self-confirmation**: the agent was tasked to grade hypotheses it came up for likely causes of incidents. The agent almost always confirmed its first suspicion and didn’t examine its conclusion.

- **Lack of ‘memory’**: lessons from past investigations didn’t carry over to new ones.

**Multi-agent system.** In 2026, the team came up with three different agent roles:

- *Investigator*: chases one hypothesis after another

- *Verifier*: challenges the conclusions of investigators. It’s structurally independent of investigators to avoid self-confirmation

- *Communicator*: agent updates stakeholders

The Resolve team told us they find this system to be the most reliable and resilient in incident investigations.

At a high level, all three AI SRE startups follow similar steps for dealing with investigations:

[![](https://substackcdn.com/image/fetch/$s_!dsmN!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdcdacbfd-7509-4b1c-b2e5-c34dd6e71368_1132x1228.png)](https://substackcdn.com/image/fetch/$s_!dsmN!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdcdacbfd-7509-4b1c-b2e5-c34dd6e71368_1132x1228.png)

The typical steps of an AI SRE investigation

1. **Trigger**: an alert fires or an engineer declares an incident

2. **Orientation**: pull in relevant data like telemetry and logs to find out what’s changed recently, like deployment and feature flags

3. **Fan out**: kick off parallel investigative agents.

4. **Verify**: challenge the conclusions of investigative agents

5. **Report**: update Slack and other communication channels

6. **Steer**: engineers give input to the agent, usually via Slack. This spawns more “fan out” investigations

7. **Hand off**: an engineer or coding agent takes on suggested mitigation actions, like making a code change, restarting a system, etc.

There’s plenty of nuance in *how* to implement each step: our three vendors often do so in distinct ways! Here are tactics for each step:

- *Trigger*: how do you categorize alerts: do you give fixed buckets for the agent (e.g. “needs human attention”, “can wait for business hours”, “noise”), or have the agent come up with a hypothesis for the cause before paging a human?

- *Orient*: do you measure the potential impact of the page (which can add latency), follow different rules based on the service impacted, or pull in a larger context graph?

- *Fan out*: do you fan out by hypothesis/potential failure type, or with as many agents as makes sense? What about long-running agents; do you wait for them, or limit tool-calling depth, or the cost of an investigation agent run?

- … and so on for other steps. You’ll want to decide on tradeoffs between latency, accuracy, and cost.

AI SRE tools need some kind of understanding of what the production system looks like: how systems are connected, the dependencies, and what caused things to go down previously. All three AI SRE tools build some kind of “context layer” to help the agent figure out root causes of an incident faster:

- Rootly calls its [Atlas](https://docs.rootly.com/ai/atlas/overview): a combination of a knowledge graph, memory, and AI connectors

- incident.io calls its [Nexus](https://docs.incident.io/nexus/overview): a combination of catalog, code, telemetry, and past incidents

- Resolve calls its [HiveMind](https://resolve.ai/product/overview): a combination of context, investigation skills, memory, and actions

Generalizing a bit, all three of these context layers are made up of a knowledge graph, memory, and tools.

This is a graph of:

- Services

- Data stores

- Cloud resources

- Deployments

- Runbooks

- Past incidents

- … and other relevant resources or sources of information

This is not something that only vendors build; practically any team building AI tools can come up with it. For example, Uber built a neat in-house knowledge graph that’s used by most of their AI systems, which we cover in the podcast episode ‘[Uber’s AI infra from the inside](https://newsletter.pragmaticengineer.com/p/ubers-ai-infra-from-the-inside).’

[![](https://substackcdn.com/image/fetch/$s_!LKSM!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd9062c54-e6f1-451d-8d24-cb18a7a42cec_2048x1077.png)](https://substackcdn.com/image/fetch/$s_!LKSM!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd9062c54-e6f1-451d-8d24-cb18a7a42cec_2048x1077.png)

Uber’s visual knowledge graph, called Context Graph.

Memory is the system for storing information from past investigations. Agents can use this layer to steer the current investigation and memory provides information like:

- “When latency spikes on payments, the root cause is more often than not with the connection pool”

- “This specific alert is typically noise the first hour after a deploy from a specific service”

- etc.

All three AI SRE startups have their own memory layers, but each implements it slightly differently. For example, Rootly has the concepts of:

- “Activated” memory notes: a note earns its spot in a more important memory note after playing a useful role in an incident, or someone activates it manually

- “Quarantined” memory notes: if a note is contradicted, it gets quarantined to not be used in the future. Systems keep changing, and this is one way to not feed stale information to the agent

**Is it a good idea to pool customers’ incident data**? AI SRE vendors have the advantage that they observe more incidents – and very different ones – via customers. Among our three vendors, Rootly is the only one that allows customers to opt into pooling their data in return for getting a hopefully improved agent.

Rootly’s idea is to build a kind of “Stack Overflow for AI SREs” internally. The idea is that Company A’s retrospective – scrubbed of sensitive details – might help Company B resolve an incident faster. Cofounder JJ Tang:

> “Most vendors in our space promise your data stays siloed forever. We think that’s the wrong default for operational data. For example, an outage caused by a bad Redis config isn’t a trade secret.”

Sharing of confidential data is a tricky topic. Rootly says it gives customers granular control over what is shared and excluded, and can access shared records and remove them. The scrubbing code is not currently open-sourced, which the company plans to change.

*Our take is that benefiting from a “knowledge base” is promising, but contributing to a broader vendor knowledge base needs to be done carefully. Responsible vendors will make contributions easy and transparent. Just ensure that when contributing data, anything confidential is scrubbed first.*

Agents use tools, connectors, plugins, and AI skills to connect to data sources during an investigation. This is pretty similar to how any AI vendor is integrated into a company with the right connectors. Needless to say, all AI vendors expose MCP interfaces, and offer Claude Code/Codex plugins for easy integration with other internal services.

AI SREs typically interact with devs in Slack during an incident. *Teams is also supported by all three AI SRE startups, and Rootly also supports GChat, but most startups are on Slack. It’s interesting that Slack is becoming the preferred choice for human-to-agent communication of most tech companies.*

There are a few challenges these tools need to solve:

- **Length of updates.** How concise or verbose should updates by the AI SRE tool be? incident.io played a lot with getting this right: they learned that being too concise makes devs not trust the agent (too few details), whereas being too lengthy can be annoying

- **When to speak up**? Rootly designed its agent to be predominantly quiet and to only post when:

  - a question in the channel goes unanswered

  - a stakeholder joins without context

  - someone declares “mitigated” without proof

  - a risky action is announced with no rollback plan

Lots of medium-sized and larger tech companies build AI SREs in-house, and the big benefit is that they can expose internal context a lot more freely to their agent than to a vendor. My sense is that the AI SREs built by different companies end up being pretty similar in capabilities and structure to those that vendors build.

**The AI SRE is a subset of Monzo’s cloud agent**, which calls its cloud coding agent ‘[Project Chip](https://monzo.com/blog/building-agent-chip)’. This is the high-level architecture:

[![](https://substackcdn.com/image/fetch/$s_!wuAY!,w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa804a190-d722-4d9b-a899-8360833951af_1428x1500.png)](https://substackcdn.com/image/fetch/$s_!wuAY!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa804a190-d722-4d9b-a899-8360833951af_1428x1500.png)

*How Project Chip is built. Source: [Monzo](https://monzo.com/blog/building-agent-chip)*

Agent Chip is a general, in-house, engineering agent that can be triggered from Slack, Linear, GitHub, and other internal systems. When an incident occurs, Agent Chip has ‘Agent Chip Incident Mode’ invoked and it pulls together context from across systems, surfaces possible causes, and can draft a fix.

Other places have in-house AI SREs:

- **OpenAI**: its tool is ‘[Sevbot](https://newsletter.pragmaticengineer.com/i/215812293/3-openais-agentic-software-factory)’. It collects details, determines possible mitigations (but never executes any), answers devs’ questions in a Slack channel, and can apply mitigations when instructed

- **Stripe**: [built](https://www.honeycomb.io/resources/conference-talks/building-an-ai-observability-agent-lessons-from-the-trenches-stripe-o11ycon-2026) its own incident investigation agent that runs on each outage. A simpler run (that tends to collect necessary signals to investigate the outage) takes three minutes, on average. A more thorough deep investigation is 14 minutes long, on average.

- **Google**: SREs use ‘ProdAgent’ (Google’s internal agentic incident management framework) within Gemini CLI. It functions like *get\_incident\_details*, *causal\_analysis*, *timeseries\_correlation*, *log\_analysis* and others. ProdAgent doesn’t generate code, it only suggests using tools exposed via MCP, like borg\_task\_restart (Borg being Google’s [infra management platform](https://newsletter.pragmaticengineer.com/i/174937901/borg-and-related-systems)). ProdAgent has policy enforcement and audit trails. It seems less polished, but is built to minimize the risk of messing up production: more scripts, less LLM determinism!

An engineering challenge of building AI SRE tools is that they need to be right most of the time, even when built on models prone to confabulation and which change often.
