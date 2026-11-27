---
title: Learnings from init to deploy
link: https://stripe.dev/blog/learnings-from-building-with-agents-stripe-projects
source: stripe-com
published: 2026-05-04T00:00:00Z
updated: 2026-05-04T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Anna Spysz
summary: 'This post is a follow-up to last week’s “From init to deploy: building with agents and Stripe Projects” and explores the learnings gained along the way.'
content: extracted
html: 2026-05-04-learnings-from-init-to-deploy.html
remote_preview:
  url: https://stripe.dev/og/blog/learnings-from-building-with-agents-stripe-projects.png
  alt: 'Stripe Developer Blog: Learnings from init to deploy'
---

Last week, I wrote about [building a transcription app for my team using Stripe Projects, OpenRouter, and Vercel](https://stripe.dev/blog/building-with-agents-stripe-projects). This app takes video software-generated subtitles and runs them through an AI agent via the OpenRouter API, taking in a detailed system prompt and tool calls to Stripe documentation to ensure speakers and service names are spelled and capitalized correctly, and returns a correctly-formatted transcription file.

This was my first foray into using an agent to carry a project all the way from `init` to deploy, and I picked up a few takeaways from the process.

### Always have a plan

If anyone still remembers the days of software engineering before agentic workflows, a new feature would usually start with the lead engineer writing a design document with proposed architecture changes, features, and UX flows. Then, the team would read the doc and point out gaps or alternatives; in the end, a better doc led to a better implementation.

This is still true, even if it’s now an agent writing the doc and possibly another agent or two reviewing it along with the engineer. Like a good design doc, an agent-designed plan should have a goal, an understanding of data flow (if applicable), an architecture section, whether a diagram or a list of new services, and a list of tasks.

It’s the agent’s job to design this based on your requirements in the initial planning prompt; it’s your job to read it thoroughly and make changes where required based on your judgement. If you have questions, it’s better to either pause to do some research on your own or go back and forth with the agent for clarification.

As the agent is coming up with the plan, you may get asked clarifying questions. One for me was whether to use streaming or not when receiving the fixed transcription contents. I followed up by asking the agent, based on my current setup of a Vercel-hosted NextJS app, would I be able to support streaming, and did it make sense for my use case of transcripts that could run thousands of lines. Its answer:

```
Streaming is the only architecture that can realistically handle long auto-generated subtitle transcripts on Vercel. Non-streaming serverless functions are capped at 10s on Hobby and 60s on Pro, and must return the full response within that window — so given your use case of a ~1,500-line transcript, it would routinely get cut off as a 504, with nothing to show for the tokens already paid for.
```

I verified that answer and did end up implementing streaming, and so far I haven't run into any timeout issues.

### Do your own research

[Stripe Projects](https://projects.dev) makes it really, *really* easy to add new services, including services you haven't used before. That makes it tempting to skip the research that goes into deciding which service to use and how to use it and let the agent run wild, but you'll quickly find gaps in your knowledge when even your agent is asking you clarifying questions.

Because I was completely new to OpenRouter, I wasn't aware of its [Free Models Router](https://openrouter.ai/openrouter/free) during my initial setup, and later ended up revising my implementation to first try a free model, and only use a paid one if the user chooses to. This gave me important data on model quality and helped me better finalize my app design.

When working with agents, taking the time to come up with a solid plan up front will save you time debugging or changing course in the long run. The same is true when choosing providers and getting to know their APIs - at least the ones most relevant to your project.

### Don't skimp on models

My transcription app initially offered three tiers of models: free, auto, and Claude. OpenRouter has a variety of free models with various results, and a handy [auto router](https://openrouter.ai/openrouter/auto) that will select an appropriate (and usually more affordable) model based on the complexity of the prompt. My initial tries with the free model were ...disappointing; rather than following the instructions in the system prompt, which included strongly-worded decrees to stick to the initial script as much as possible, it ended up taking a 700-line script from my [Meetup talk](https://youtu.be/1kBFmOufiNE?si=k29VGEiO_4FuC-PT), and condensing it into a 100-line summary like so:

```
1  
00:00:00,360 --> 00:00:07,200  
Hello, Dublin. My name is Anna Spysz. And today I'll show you how to integrate Stripe payments seamlessly.  

2  
00:00:07,200 --> 00:00:13,880  
Stripe offers three options: payment links, embedded buttons, or custom development. Choose based on your needs.  

[...]

6  
00:00:34,279 --> 00:00:41,280  
For more details, visit stripe.com.  

7  
00:00:41,280 --> 00:00:47,600  
Now, let’s see how to implement this.  

8  
00:00:47,600 --> 00:00:53,799  
Start with the checkout flow. Ensure it matches your site’s design.  

[...] 

15  
00:01:41,329 --> 00:01:48,169  
Finally, integrate the payment button into your site.  

16  
00:01:48,169 --> 00:01:54,709  
Double-check everything before launch.  

17  
00:01:54,709 --> 00:02:01,690  
Done! Your site now accepts payments smoothly.  
```

Well... at least it kept the (now inaccurate) timestamps 🤦.

Repeated results like this made me remove the free option completely and instead default to `openrouter/auto` for the model. More testing revealed that reasoning models, though slower, were far more accurate, so I ended up swapping out Claude Sonnet 4.5 for Claude Opus 4.5 - thinking.

The same is true for choosing the model that will plan your feature or app and then build it. If possible, use higher-quality thinking models for at least the planning and reviewing (more on that below). If you're on a token budget, you can then use less complex models for the actual code implementation, as long as they follow the plan and you're the ultimate reviewer of the code.

### Check in your agent files

When working on larger repos, my instinct had previously been to add the contents of `.agents/` or `.cursor/` to `.gitignore` - after all, these are *my* personal, hard-fought and probably too-niche-for-my-team learnings. This time, however, I checked in all of my files when I noticed that `stripe projects init` only (rightfully) added `.env` and cache files to `.gitignore` but kept all of the agent-specific files committed.

Today, that feels like the right workflow: if I'm using an agent, and so is my teammate, we want our agents to have shared knowledge (skills) and shared rules so our code remains consistent. Just as I wouldn't add `eslint.config` to `.gitignore`, I'll no longer be adding `AGENTS.md`.

### Be your own adversary

Just as you always want to read carefully through the agent-created plan, you need to read through agent-created code before checking it in - but you don’t need to be the first reviewer. In my `./agents` directory, I have `agents/pr-reviewer.md`, which is an agent persona that follows my organization’s best practices when reviewing PRs, including internal tools that exist to help in that process, style guides, testing coverage guides, accessibility and security guidelines, and so on.

Once the agent has completed all of the tasks in the plan, spin up another agent, load the `pr-reviewer` persona, and ask it to go through the changes in your branch compared to `main` and list issues or problems, if any. The last part is important - an agent will find issues if you tell it to, so be sure to give it an opportunity to say “LGTM” if the code is good as is. Agents can be just as guilty of over-engineering as humans!

If the agent does find problems, it will list them in order of severity; at this point, you should start reviewing the code yourself to see if you agree with the issues and if you can find better solutions than those proposed. Then you can ask the agent to address the issues you agree with, and now you’re ready to review the entire changeset before you put up your PR!

### Get started with Projects

I hope these learnings are useful to others building with agents. If you want to get started with Stripe Projects yourself, head to [projects.dev](https://projects.dev/). All it takes is one command to give your agents access to a growing list of service providers:

```
stripe projects init
```
