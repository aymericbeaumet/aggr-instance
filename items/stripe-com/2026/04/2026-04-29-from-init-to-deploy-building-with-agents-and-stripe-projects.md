---
title: 'From init to deploy: building with agents and Stripe Projects'
link: https://stripe.dev/blog/building-with-agents-stripe-projects
source: stripe-com
published: 2026-04-29T00:00:00Z
updated: 2026-04-29T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Anna Spysz
summary: Stripe Projects makes it easy to spin up a useful, complex application that uses various SaaS providers. This post describes how one of our developer advocates built an internal transcript editing application using Stripe Projects, Vercel, and OpenRouter.
content: extracted
html: 2026-04-29-from-init-to-deploy-building-with-agents-and-stripe-projects.html
remote_preview:
  url: https://stripe.dev/og/blog/building-with-agents-stripe-projects.png
  alt: 'Stripe Developer Blog: From init to deploy: building with agents and Stripe Projects'
extra:
  content:interactive: true
---

I built an internal tool that cut our transcript editing time by 4x. Here’s how I did it with the newly-launched [Stripe Projects CLI](https://projects.dev), OpenRouter, and Vercel (along with my handy Claude).

### The problem: too many transcripts!

I'm on the Developer Relations team at Stripe, and one of the things we do - a lot - is make videos for our [Stripe Developers YouTube channel](https://www.youtube.com/stripedevelopers). To be accessible to a wide audience, all of these videos need subtitles - *accurate* subtitles, because the original English captions are then used by Google to build 22 language translations and 16 voice dubs. While some decent auto-captioning tools exist, they inevitably make mistakes, like failing to recognize proper nouns or the full context of a technical term, so a human needs to review them before they can be released.

We recently calculated that it takes between 1.5 and 2 times as many minutes to edit auto-generated subtitles as the length of the video, meaning that one of my teammates will spend 30-40 minutes editing just the subtitles of a 20-minute video. That's a lot of dev hours used on a task that is fairly straightforward and repetitive; in other words, the perfect task for an agent.

### The solution: an agentic transcription app

My solution was to build a custom agentic transcription fixing app for my team. To make the process as straightforward as possible, I had a few requirements. Luckily, when using an agentic workflow in a tool like Claude, [Kiro](https://stripe.dev/blog/building-production-ready-stripe-subscriptions-kiro-powers) or Cursor, planning mode works best when you list requirements clearly, so thinking these through initially set me up to make a solid plan my agents could follow.

#### Planning

My transcription app needed:

- to be hosted in a place accessible to my teammates, but with built-in auth so the entire internet couldn't burn through our tokens
- an API that provided access to different LLM models, as shorter or more straightforward transcripts could be handled by free or cheaper models, but I wanted the option of more advanced models in case the output from less capable models was insufficient
- to use the Stripe Projects CLI to link all of the providers (and necessary environment variables) in one place
- a clean and simple UI that had all needed functionality in a single page (with a dark mode option, because I'm a developer after all)

This was my initial prompt:

```
I would like to create a new web application that can take a rough output from video transcription software, add instructions from the user in a separate chat window (such as "Capitalize all proper nouns, replace stripe with Stripe"), and run the transcript and instructions through an agent that then outputs the fixed transcript in another panel. If needed, the user can keep chatting to apply additional changes. The UI should have a split layout, with the original transcript input on the top left half of the screen, the chat instructions on the bottom left, and the fixed transcript on the right half of the screen. Please use the Stripe Projects CLI to provision any services needed. Visually, the app should have a simple feel and default to light or dark mode based on the user's browser settings. Please see `./example.json` for an example of an original and fixed transcript.
```

This prompt resulted in a plan from the agent with several TODOs that I was able to go through and edit before we started building. These included:

- Implementing a split-pane transcription/chat/output UI with session-only state
- Creating routes to wire the request/response contract for iterative transcript fixing
- Adding a `transcriptAgent` with a system prompt and helpers
- Wiring the frontend up to new API routes
- Comprehensive testing (can’t forget that!)

But before I could implement the plan, I needed to get all of the required services in place.

#### Setup

The first thing I want to do is create the scaffolding for my project. This will look different depending on your framework, but at the very least you'll want to create a directory with the name of your project and initialize a git repository in it. In my case, I ran `npx create-next-app@latest transcription-fixer`, which created my project directory and set up a basic full-stack app.

Then, I initialized Projects by running this command in the root of my new project directory:

```
stripe projects init
```

That scaffolds files such as `AGENTS.md` and directories with useful skills and rules for different agents, as well as a `.projects/state.json` file that is the source of truth for your integrations.

Because I always review the agent's code, I'm biased towards using frameworks and services I'm familiar with, so I chose to start this project as a NextJS app hosted in my existing Vercel account. Luckily, Stripe Projects lets me link an existing account with the same command I would use to set up a new one:

```
stripe projects add vercel/projects
```

This links my Vercel account to my Stripe account, and my`.projects/state.json` now looks like this:

```
{
  "version": 1,
  "providers": {
    "vercel": {
      "name": "Vercel"
    }
  },
  "resources": {
    "vercel-project": {
      "name": "vercel-project",
      "providerName": "Vercel",
      "serviceId": "project"
    }
  }
}
```

With my framework and hosting in place, I had the agent build the initial plan of a transcription UI and put the necessary routing in place so we could then wire it up in the next stage.

#### Adding the app logic

Once I had built the scaffolding of my app, it was time to consider what other services I might need. The most obvious one would be an agentic API provider - luckily, as of last week, [OpenRouter](https://openrouter.ai/) is available as an AI provider within Projects, and I was excited to try it for the first time.

To link OpenRouter, I had to do two things:

1. Add a payment method. As I was using the Vercel free tier and a new Stripe account, I didn't already have a payment method attached to it, and OpenRouter is a paid service (though it does provide free models). I went to the Stripe Dashboard, chose my account, and added a credit card.

2. Run `stripe projects add openrouter/api`

Once linked, I could run `stripe projects open openrouter` to see their dashboard and make updates to my account, but I wanted to keep going on my app, so I ran `stripe projects status` to confirm everything was linked as expected, and it was:

```
 ~/s/transcription-fixer stripe projects status
                       
 │  transcription-fixer
 │  Project    project_1234
 │  Account    Stripe DevRel (acct_1234)
 │  Email      <me>@stripe.com ✓ Verified
 │  Created    Mar 25, 2026

Providers (2)
Name        Status    Linked        
──────────  ────────  ──────────────
OpenRouter  ✓ Linked  11 minutes ago
Vercel      ✓ Linked  23 days ago   

Services (2)
Name            Provider    Service  Pricing
──────────────  ──────────  ───────  ───────
openrouter-api  OpenRouter  api      Free   
vercel-project  Vercel      project  Free  
```

Excellent! Additionally, I checked my local `.env` file, and it had been auto-populated with `OPENROUTER_API_KEY` and `OPENROUTER_TYPE`, with the correct values already filled in.

#### Let the agent cook (with recipes)

With all of the services my app needed set up, I went back into plan mode with my agent. Now we would need to add an LLM client that used the [OpenRouter SDK](https://openrouter.ai/docs/sdks/typescript/overview) to call various endpoints and an ability for the user to select which model they wanted to use.

In the end, I would spend the bulk of my time thinking through how to compose the system prompt for the transcription agent so that the app's user would need minimal instructions to get good results (yes, we were getting meta). I wanted to give the transcription agent the ability to call tools such as `search_stripe_documentation` using [Stripe's MCP server](https://mcp.stripe.com) so it would get service names right without explicit prompting. I also provided a before and after example of the transcript I had fixed by hand from my [Dublin Meetup talk](https://youtu.be/1kBFmOufiNE?si=k29VGEiO_4FuC-PT), which my agent shortened and used as a [few-shot example](https://www.promptingguide.ai/techniques/fewshot).

This was the final system prompt we ended up with:

```
const SYSTEM_PROMPT = `You are an expert transcription cleanup agent.

Primary objective:
- Apply user instructions to improve the transcript while preserving meaning, text structure exactly except for changes in the instructions, and exact formatting, including line breaks and timestamps.

Critical formatting requirements:
- Preserve line structure exactly unless the instruction explicitly asks for format edits.
- Preserve whitespace intent, paragraph breaks, timestamps, speaker labels, and ordering.
- Do not collapse lines, reflow blocks, or normalize spacing unless asked.

Quality requirements:
- Do not hallucinate facts or insert content not implied by transcript text.
- Make the minimum set of edits needed to satisfy user instructions.
- Keep language and tone consistent with source transcript.

Default cleanups (always apply, even when not explicitly requested):
- Remove leading filler utterances at the start of sentences and cues — specifically "uh", "um", "uhm", "umm", "er", "eh", "ah", "hmm", and similar disfluencies — along with any comma, ellipsis, or extra space that immediately follows the filler. Then capitalize the first letter of the next word so the sentence starts cleanly (e.g. "Um, so we shipped" → "So we shipped"; "Uh hello there" → "Hello there").
- "Start of a sentence" means the start of a cue/line, the start of a paragraph, and any position immediately after a sentence-ending "." / "!" / "?".
- Do NOT remove filler words that appear mid-sentence (e.g. "I think, uh, we should ship") — only leading ones — since mid-sentence fillers can carry pacing/meaning. Defer to explicit user instructions to strip those.
- When removing a leading filler, change nothing else on the line: keep timestamps, line breaks, speaker labels, and the rest of the sentence text intact.

Output requirements:
- Return only the final fixed transcript text.
- Do not include markdown fences, explanations, labels, or commentary.`;
```

Just as with the initial plan with my agent, the more detailed the system prompt, the better my results.

#### Results

The final app looks like this, using my unedited Meetup talk as the input:

![The application with a fixed transcript](https://stripe.dev/images/building-with-agents-stripe-projects/transcript.png)

The system prompt already took care of removing the "Um"s I uttered, capitalizing Stripe, and preserving formatting. Additionally, the agent was able to infer from the talk's context that I was talking about React the framework, and properly capitalized that. For proper nouns or names that the agent likely wouldn't be able to infer, I ended up adding a form for the user to fill out so they wouldn't have to spell out additional instructions for common fields, and I iterated on the system prompt to include these strings.

And the best part - here are the results from my benchmark test, which used the 771-line script of my meetup talk and instructions to correct my name and the mentioned Stripe services (as seen in the screenshot above):

| Model                                | Time    | Accuracy                                                                                                                               | Tokens                         |
| ------------------------------------ | ------- | -------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------ |
| Auto (google/gemini-3-flash-preview) | 40.22s  | 85% (all instructions followed, some context-aware capitalization and grammar missed, including Stripe services)                       | 31,724 tokens                  |
| Google Gemini 2.5 Pro                | 90.54s  | 99% (all instructions followed, correctly capitalized Stripe services not explicitly instructed on, best on inferring technical terms) | 36,486 tokens, 4,814 reasoning |
| Claude Sonnet 4.5                    | 137.67s | 90% (all instructions followed, some context-aware capitalization and grammar missed, including Stripe services)                       | 25,781 tokens                  |
| Claude Opus 4.5 - thinking           | 298.57s | 95% (all instructions followed, correctly capitalized Stripe services not explicitly instructed on)                                    | 35,348 tokens, 4,452 reasoning |

For this 20-minute talk, we went from 30+ minutes of editing to a little over two minutes of generated edits as the average of all the models, and perhaps 2-3 minutes for a human to double-check the fixed transcript for accuracy. That's a 4x reduction in time!

And how much has this cost my team? All of the testing over a few days of building the app resulted in a massive bill of… $2.54. You certainly can’t get a cup of coffee that cheap in Oyster Point!

#### Going live

Once I was satisfied with my app's results, I was ready to deploy to Vercel. But first, I had to get the environment variables Projects added to my `.env` file into the Vercel production environment so that my OpenRouter calls would work when deployed. There are two ways to do this:

1. via the Vercel CLI with `vercel env add [NAME] [ENVIRONMENT]`, though I would have to add my variables one by one
2. via the Vercel Dashboard at My Project -> Settings -> Environment Variables -> Add Environment Variable -> Import `.env` (at the bottom of the form)

I chose the former as I only needed `OPENROUTER_API_KEY` added for my app to be fully functional.

Finally, I ran `vercel --prod` to deploy straight to my live production environment on a Friday afternoon, because YOLO (and because I had Claude add an extended testing suite and review my changes as a PR reviewer persona).

### Next steps

At some point in the near future, I'll want to give my agents a memory - likely a way to store previously accepted transcriptions to use as a reference. That means I'll need a database. Luckily, Stripe Projects makes adding one just as easy as adding hosting or AI.

Right now, running `stripe projects catalog` lists the following for just database providers:

| Provider                 | Pricing     |
| ------------------------ | ----------- |
| `chroma/database`        | Paid        |
| `cloudflare/d1`          | Free & Paid |
| `cloudflare/hyperdrive`  | Free & Paid |
| `flyio/mpg`              | Paid        |
| `neon/postgres`          | Free & Paid |
| `planetscale/mysql`      | Paid        |
| `planetscale/postgresql` | Paid        |
| `railway/mongo`          | Free & Paid |
| `railway/postgres`       | Free & Paid |
| `railway/redis`          | Free & Paid |
| `supabase/project`       | Free & Paid |
| `turso/database`         | Free & Paid |
| `upstash/redis`          | Free & Paid |
| `upstash/vector`         | Free & Paid |

And more are being added weekly! That means as my project grows, I can keep using one CLI tool to keep all of my providers organized and my environments up to date. Best of all, when my teammates check out my repo, all they have to run is the following to get the correct API keys into their environment:

```
stripe projects env --pull
```

Speaking of working on a team, I’m sure we’ve all felt the panic of hearing about a security incident at 2am and being asked to rotate credentials while barely awake. With Projects, that now becomes as easy as:

```
stripe projects rotate <provider>/<service>
```

If you want to try Stripe Projects yourself, head to [projects.dev](https://projects.dev/) to get started!
