---
title: '[AINews] Claude Haiku 5.5 — better than GPT-6 Luna at the same pricing'
link: https://www.latent.space/p/ainews-claude-haiku-55-better-than
source: latent-space
published: 2026-10-08T07:27:51Z
updated: 2026-10-08T07:27:51Z
first_seen: 2026-10-08T13:19:06.446835656Z
summary: yay small models
content: extracted
html: 2026-10-08-ainews-claude-haiku-5-5-better-than-gpt-6-luna-at-the-same.html
remote_preview:
  url: https://substackcdn.com/image/fetch/$s_!q_iv!,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fpbs.substack.com%2Fmedia%2FHUDOG1Jb0AAHQ2I.jpg
---

It’s been about a year since Anthropic [shipped Haiku 4.5](https://www.latent.space/p/ainews-ai-vs-saas-the-unreasonable?utm_source=publication-search), and with successive launches of Sonnet and Opus and Fable up to 5.5 it was seeming a little forgotten, especially as OpenAI launched Luna 6 alongside Astra and Sol 6.

Well, it’s here, and it’s a welcome update. More in the summary below.

> Artificial Analysis @ArtificialAnlys
>
> Anthropic has released Claude Haiku 5.5, scoring 43 on the Artificial Analysis Intelligence Index - up 26 points one year after the last Haiku release Haiku 5.5 is the first Haiku model with Anthropic’s effort settings and adaptive thinking, and Anthropic has introduced tiered …
>
> [@ArtificialAnlys on X](https://x.com/ArtificialAnlys/status/2107911905822351609)

> AI News for 10/06/2026-10/7/2026. We checked 12 subreddits, [544 Twitters](https://twitter.com/i/lists/1585430245762441216) and no further Discords. [AINews’ website](https://news.smol.ai/) lets you search all past issues. As a reminder, [AINews is now a section of Latent Space](https://www.latent.space/p/2026). You can [opt in/out](https://support.substack.com/hc/en-us/articles/8914938285204-How-do-I-subscribe-to-or-unsubscribe-from-a-section-on-Substack) of email frequencies!

**Top Story: Anthropic releases Claude Haiku 5.5**

**Anthropic shipped Claude Haiku 5.5, its first Haiku-tier update in about a year. It is priced to match OpenAI’s GPT-6 Luna, and Anthropic cut prices on Sonnet 5.5 and its subscription plans the same day.**

- **Pre-launch signals.** [@scaling01](https://x.com/scaling01/status/2107889433777258535) posted “happy Haiku 5.5 day” before the announcement. [@kimmonismus](https://x.com/kimmonismus/status/2107890621834990038) said the model had already appeared in a Claude Code update and predicted “Luna-pricing.” He then posted the pricing ahead of the official post ([@kimmonismus](https://x.com/kimmonismus/status/2107892761709993985)) and confirmed when it went live ([@kimmonismus](https://x.com/kimmonismus/status/2107892976374472811)).

- **Official launch.** [@claudeai](https://x.com/claudeai/status/2107894039626277339) and [@AnthropicAI](https://x.com/AnthropicAI/status/2107894208547983705) called it “the cheapest, fastest, and most capable small model we’ve ever released,” costing about 75% less to run than Haiku 4.5 on average.

- **Availability and intended use.** It is live on the Claude Platform and in Claude Code ([@ClaudeDevs](https://x.com/ClaudeDevs/status/2107895955144208813)). Anthropic positions it as a subagent paired with Opus 5.5 or Sonnet 5.5, for high-volume, cost-sensitive work such as summaries, compactions and database queries. [@mikeyk](https://x.com/mikeyk/status/2107894911907614872) described the split as “Opus does the heavy thinking, Haiku does the high-volume work.”

- **Tiered pricing** ([@ClaudeDevs](https://x.com/ClaudeDevs/status/2107895956872290654)):

  Prompt lengthInput / output per 1M tokensCache reads per 1MUnder 100K tokens$0.10 / $0.50$0.01Over 100K tokens$0.50 / $2.50$0.05

- **Sonnet 5.5 price cut.** Cache reads were halved from $0.20 to $0.10 per 1M tokens. Anthropic says this makes Sonnet 5.5 about 20% cheaper on most long-running or agentic work ([@claudeai](https://x.com/claudeai/status/2107894060229034197)).

- **API credits for subscribers.** Monthly Claude Platform API credits now come with Max 5x ($100), Max 20x ($200) and Team (up to $500, pooled). They work on any model, including Haiku 5.5, and in third-party harnesses ([@ClaudeDevs](https://x.com/ClaudeDevs/status/2107895957933408429)).

- **Same-day SDK update.** Computer-use and browser-use toolsets are now built into the Python and TypeScript Claude SDKs. The SDK runs the action loop and sends clicks and keystrokes to drivers from browser\_use, Browserbase, E2B or Daytona, so developers no longer write that loop themselves ([@ClaudeDevs](https://x.com/ClaudeDevs/status/2107925762720326090), [quickstart](https://x.com/ClaudeDevs/status/2107925764163113107)).

- **Partner rollouts on day one:**

  - Cursor: [@cursor\_ai](https://x.com/cursor_ai/status/2107897245282799864) claims “10x less than Haiku 4.5” on shorter requests and published CursorBench comparisons ([@cursor\_ai](https://x.com/cursor_ai/status/2107897257651769464)).

  - GitHub Copilot in VS Code: [@code](https://x.com/code/status/2107936051482300756) reports it “matched Claude Sonnet 5 on many coding tasks while using fewer tokens and steps.”

  - Devin: [@cognition](https://x.com/cognition/status/2107942832921075726) reports 58.4% on FrontierCode 1.1, ahead of Sonnet 5 at roughly one-eighth the cost per task, and recommends it as a “sidekick” under an Opus 5.5 lead in Fusion.

  - Arena: added to Agent Arena, Code Arena WebDev, Text, Document and Vision, with scores pending ([@arena](https://x.com/arena/status/2107906881276826082)).

  - OpenDocRouter: added the same day (details below).

[@ArtificialAnlys](https://x.com/ArtificialAnlys/status/2107911905822351609) published the most detailed third-party numbers ([per-eval breakdown](https://x.com/ArtificialAnlys/status/2107911912499618086), [comparison page](https://x.com/ArtificialAnlys/status/2107911915788018161)).

- **Intelligence Index: 43** at max effort, up 26 points from the previous Haiku.

  - Slightly ahead of GLM-5.3 Flash (42), Gemini 3.8 Flash (41) and GPT-6 Luna (38).

  - Comparable to Kimi K3 (44), a 2.8T-parameter open-weights model.

  - Trails Claude Sonnet 5.5 at max effort (56) by 13 points.
- **New controls.** This is the first Haiku with Anthropic’s effort settings and adaptive thinking.

- **Token usage is the main caveat.**

  - At max effort it uses about 162k output tokens per Index task, roughly 3x GPT-6 Luna at max (~50k).

  - Going from xhigh to max adds 2 points for about 1.8x the tokens.

  - At equal score it is still more verbose: Haiku 5.5 at high effort scores 38 using ~55k tokens, versus Luna at max scoring 38 with ~50k. The gap widens at lower effort settings.
- **Cost figures are provisional.** Artificial Analysis does not yet model the 5x price step above 100K tokens. Cost-per-task numbers will follow.

- **AA-Briefcase** (private agentic knowledge-work eval): 1578 Elo. That is ahead of Kimi K3 and GLM-5.3, and comparable to Muse Spark 1.3 at max.

- **Terminal-Bench 4.0: 33%**, up from 0% for Haiku 4.5.

  - Level with GLM-5.3 Flash.

  - Ahead of Gemini 3.8 Flash (20%) and GPT-6 Luna (13%).
- **Knowledge versus hallucination** (AA-Omniscience):

  ModelAccuracyHallucination rateHaiku 5.536%40%Gemini 3.8 Flash55%55%GPT-6 Luna44%77%

  Part of Haiku’s lower accuracy comes from being more willing to say it doesn’t know.

- **AutomationBench-AA: 35%**, versus 53–60% for Luna, Gemini 3.8 Flash and GLM-5.3 Flash. A pre-release safety bug caused the model to over-refuse. Anthropic is working on a fix, and Artificial Analysis will re-run the eval and expects the score to rise.

- **Specs:**

  - 1M-token context, up from 200k for Haiku 4.5.

  - Text and image input, text output.

  - 5-minute cache writes cost $0.125 per 1M tokens ($0.625 above 100K).

- [@ShayneRedford](https://x.com/ShayneRedford/status/2107942496601010512) summarized Anthropic’s reported jumps:

  - OSWorld (computer use): 15% → 72%.

  - TerminalBench: 0% → 39%. This differs from Artificial Analysis’s independent 33% on Terminal-Bench 4.0.

  - 10–50% gains in knowledge work and reasoning.

  - Beats Luna on most of these.

  - 1M context with roughly 12k max output tokens.
- [@alexalbert\_\_](https://x.com/alexalbert__/status/2107912771568554415) (Anthropic) stressed that Haiku 4.5 shipped Oct 15, 2025, so the comparison spans less than a year.

- [@TheRundownAI](https://x.com/TheRundownAI/status/2107901352340836491) reported that it beats GPT-6 Luna “across a variety of benchmarks.”

- **Document parsing** (independent, on ParseBench):

  - [@LoganMarkewich](https://x.com/LoganMarkewich/status/2107911680550433252): overall close to Luna, slightly better on tables, worse on chart understanding.

  - [@jerryjliu0](https://x.com/jerryjliu0/status/2107940101217309041): about $1.2 per 1,000 pages. Good at tables and reading order for the price; weaker on charts, semantic formatting and bounding boxes.
- **Anecdotal:**

  - [@simonw](https://x.com/simonw/status/2107938675405586713) wrote pricing notes and ran his pelican-on-a-bicycle test. He says it is “SO MUCH better” than Haiku 4.5, which costs 10x more ([comparison](https://x.com/simonw/status/2107939154122404015)).

  - [@AI\_Screening](https://x.com/AI_Screening/status/2107906044915749274) says Haiku 5.5 “cooked” Luna on a Three.js zebra simulation (single prompt, not systematic).

**Bullish**

- [@kimmonismus](https://x.com/kimmonismus/status/2107894978332557756): “Way better than GPT-6-Luna, close\[r\] to Sonnet 5.5… Cheap and smart.”

- [@theo](https://x.com/theo/status/2107914587354063142) likes the tiered pricing: charging a fifth of the price under 100K tokens “makes it really clear what the model is for.” He also found it striking to see an Anthropic model “so far to the left on the cost/intelligence charts” ([@theo](https://x.com/theo/status/2107915920714944999)) and covered the launch on stream ([@theo](https://x.com/theo/status/2107960523958681921)).

- [@draecomino](https://x.com/draecomino/status/2107898484338917777): “Haiku at max effort performs like a frontier model.”

- [@kipperrii](https://x.com/kipperrii/status/2107903755236831663) argues small, cheap models matter more now that they can do “a ton of useful things.”

- [@scaling01](https://x.com/scaling01/status/2107893737162486124) (”cheap af”) and later ([@scaling01](https://x.com/scaling01/status/2107994231692276220)): “5.5 models are looking good.”

- [@NotTomBrown](https://x.com/NotTomBrown/status/2107902695222935947) (”small but mighty”) and [@edwinarbus](https://x.com/edwinarbus/status/2107897777212797232), both from the Anthropic side, posted celebratory notes.

**Competitive framing**

- The launch is widely read as aimed at OpenAI’s GPT-6 Luna: “rip gpt 6 luna” ([@dejavucoder](https://x.com/dejavucoder/status/2107896297990803779)), “time to cook Luna” ([@scaling01](https://x.com/scaling01/status/2107894217733251519)).

- [@kimmonismus](https://x.com/kimmonismus/status/2107895800252579841) framed the Sonnet cache-read cut as Anthropic pressuring OpenAI. On the API credits he added, “OpenAI: your turn” ([@kimmonismus](https://x.com/kimmonismus/status/2107930870871158874)).

- [@teortaxesTex](https://x.com/teortaxesTex/status/2107899753279181007) says Anthropic now has “the deepest product lineup of all labs” (Haiku/Sonnet/Opus/Fable plus Mythos) versus OpenAI’s Luna/Sol/Astra. He still thinks Anthropic “cares less about products,” which he reads as a sign of how much slack it has had through 2026.

- ThursdAI’s [@altryne](https://x.com/thursdai_pod/status/2108000465636237524) questioned the middle tier: “Sonnet made sense when Opus was expensive.” The show plans to cover Haiku 5.5 ([@thursdai\_pod](https://x.com/thursdai_pod/status/2108025846820917742)).

**Caveats (mostly from the data, not loud critics)**

- **The headline price may overstate real savings.**

  - Heavy token use (about 3x Luna at max effort) offsets part of the per-token discount.

  - Prompts over 100K tokens pay 5x more, which matters for long-context agent loops.

  - These two effects likely explain why Cursor’s “10x cheaper on short requests” differs from Anthropic’s “75% cheaper on average.”
- **Factual recall** is weaker than Gemini 3.8 Flash and Luna.

- **The over-refusal bug** currently depresses the automation score.

- **Not yet benchmarked:** Arena scores are pending, and independent cost-per-task figures await tiered-pricing support.

- Haiku 4.5 has been Anthropic’s small model since October 2025. Since then, OpenAI’s GPT-6 Luna, Gemini 3.8 Flash, GLM-5.3 Flash and DeepSeek V4.1 Flash have competed at the low-cost end.

- Haiku 5.5 matches Luna’s sticker price exactly. It adds effort control and a 1M context.

- It is explicitly positioned as the cheap worker inside multi-model agent harnesses: Claude Code subagents, Devin Fusion, Copilot subagents, and compaction or summarization steps.

- Anthropic paired it with Sonnet cache-read cuts and subscription API credits, a coordinated push on agent economics. That push lands amid wider debate over token bills (see [@theo](https://x.com/theo/status/2107924138094703084) below).

**OpenAI’s 722 Math Manuscripts: Scale, Efficiency and Fallout**
