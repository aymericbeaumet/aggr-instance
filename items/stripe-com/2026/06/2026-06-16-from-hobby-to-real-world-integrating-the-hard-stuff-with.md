---
title: 'From hobby to real world: integrating the hard stuff with agents'
link: https://stripe.dev/blog/integrating-services-with-agents-stripe-projects
source: stripe-com
published: 2026-06-16T00:00:00Z
updated: 2026-06-16T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Anna Spysz
summary: When building with agents, most guides will give you the happy path - how to do something the best way, without friction. This post is all friction, and how to get through it when things go wrong.
content: extracted
html: 2026-06-16-from-hobby-to-real-world-integrating-the-hard-stuff-with.html
remote_preview:
  url: https://stripe.dev/og/blog/integrating-services-with-agents-stripe-projects.png
  alt: 'Stripe Developer Blog: From hobby to real world: integrating the hard stuff with agents'
extra:
  content:interactive: true
---

Previously, I wrote about [building a transcription app](https://stripe.dev/blog/building-with-agents-stripe-projects) using Stripe Projects. While that tool certainly has a real use case and is still being used by my team, it's much closer to a hobby project than a real production app. That much was obvious both from how quickly I was able to spin it up (just a few days, with only a few hours of development time) to how frictionless the process was.

So what does building a more complex production app using agents look like? Even with the best-laid plans, sometimes it will still be mostly friction. Navigating that friction is where the takeaways come in.

### The use case

So many people are building with [Stripe Projects](https://projects.dev/), it's getting hard to keep track of them all! I wanted a fun way to catalog and showcase open-source projects, and maybe add a competitive element to it for future hackathons, so I decided to build a leaderboard app. The concept was fairly simple: I wanted to build an app where users could add their project to a table, share it via social media links, and vote on other projects.

Despite the simplicity of the app, it would require some more complex infrastructure, namely:

- Auth, for tracking user submissions and enforcing a one-project-per-user limit
- A database with multiple tables, for tracking project details, user details, and votes
- Hosting for the app itself
- A mechanism to prevent voting abuse, i.e. ensuring non-logged-in users can only vote once per project

### The initial plan

When it comes to agents, a well-crafted plan pays dividends in the long run. I had my requirements but wanted to ensure nothing was missing, so I meticulously laid out every piece of functionality the leaderboard would need.

First, I made sure I had the Projects skill available, so my agent would know to use Projects for provisioning infrastructure:

```
npx skills add https://docs.stripe.com
```

Then, I crafted a detailed markdown document covering requirements and styling, such as the theme's primary and secondary colors, fonts, light/dark mode, etc. Anything left out of the requirements is left to the agent's discretion, which tends to trend towards the generic, and I had a very specific vision in mind as well as Stripe UX guidelines to follow.

The requirements included how I was thinking about the data structure, and contained instructions such as:

```
- The leaderboard data will be stored in a database in an object with the following types:
    leaderboard: [
        {
            "projectId": "project_<ID>", <taken from state.local.json>
            "projectName": "demo-project", <taken from state.local.json>
            "projectURL": "", <link to live deployed app, from form>
            "projectRepoURL": "", <link to public repo, from form>
            "projectStackURL": "", <from `stripe projects share` output, from form>
            "userName": "", <handle of user when registering>
            "userEmail": "", <given when registering, must match Stripe Projects email>
            "ranking": {
                "position": <number>,
                "upvotes": <number>,
                "timeAtPosition": <function>
            }
        }
    ]
```

While in the past I've used apps such as Figma for wireframing, this time I figured the agent needed just a general concept for layout, and I was in a bit of a hurry - so I went old school and sketched out a rough idea of what I had in mind on a scrap of paper, took a photo, and added it to my requirements directory:

![Unfortunately not on the back of a napkin, but close enough](https://stripe.dev/images/integrating-services-with-agents-stripe-projects/wireframe.jpg)

Then came the actual prompt, using Claude's planning mode:

```
I want to build a single-page NextJS app that shows a leaderboard of applications created using Stripe Projects and allows users to upvote on existing applications without registering, though limited to one vote per user per project. In order to add an application to the leaderboard for voting, a user will need to register and fill out a form, which will be in a modal activated by a "Submit your project" button.

See <inserted requirements> for details, styling, and data requirements, and <wireframe> for a rough wireframe, which should be re-created as a table.

Use the best database and auth for the use case, though simpler to use would be better for both, and any other infrastructure needed. Use Vercel for hosting as I already have an account.
```

### The initial pivot

As expected, the agent's initial plan included provisioning the following resources:

- Database + auth: Supabase (single service)
- Hosting: Vercel

The agent followed my request for infrastructure simplicity and chose Supabase as both my database and auth provider, and Vercel for hosting as specified. It created CSS variables based on my requested color scheme, laid out the data structure for a `projects` table and a `votes` table, and presented the following architecture:

![Initial architecture from the agent](https://stripe.dev/images/integrating-services-with-agents-stripe-projects/architecture.png)

Everything looked good, and I hit `build`. The agent started by adding services via the CLI:

```
stripe projects add supabase/project
stripe projects add vercel/project
stripe projects env --pull   # writes .env.local
```

This also added the relevant skills for each service, which let the agent know to install `@supabase/supabase-js` and other libraries we would need.

As the agent was building, we ran into the first issue: enforcing the one vote per user rule for voting on the leaderboard. Using the Vercel skill, the agent determined that the built-in rate limiter for voting integrity it had initially planned would not work on the serverless architecture we were using, and a server-side enforcement mechanism was necessary. It would replace the existing vote flow with a single atomic database function, but that required a new service: [Upstash Redis](https://upstash.com/docs/redis/overall/getstarted) for rate limiting.

I gave my consent, it ran `stripe projects add upstash/redis`, and it was able to complete the rest of the build plan.

### Pivoting providers

The real problems started once the integrations were configured, the app was working locally, and I started running into limits from the providers themselves. This was most obvious when I started testing the sign-up auth flow using [Supabase auth](https://supabase.com/docs/guides/auth) - specifically, the email rate limits. It turns out that everybody has a plan until they get hit by rate limits.

Supabase's auth plan includes a rate limitation of two emails that can be sent per hour from my project - that was not enough for testing, much less a production app. Digging into the docs, I found out I would have to set up a custom SMTP provider or use my own email service via a custom hook to update the email rate limit. That meant another service, specifically an SMTP or email provider, something not yet offered by Stripe Projects - as well as potential new costs. I wanted to avoid having to run a custom email service when all I needed was a way to authenticate my users, so I decided to swap out my auth provider instead.

This is the point where human intervention was absolutely necessary, because when asked for potential solutions to this issue, the agent suggested implementing a Twilio integration instead, which aside from lacking the functionality I needed would have been absolute overkill for my use case.

### Pivoting plans

It was time to go back into plan mode, both for myself and the agent. I would need to research the available auth providers, see if any of them would work for my use case, or potentially use a provider not offered by Projects. Luckily, [Clerk](https://clerk.com/docs) ended up having the functionality I needed with much more generous rate limits aimed at preventing abuse.

Once I knew what service I wanted to use, I asked the agent to come up with a migration plan, which resulted in this simplified auth flow:

![The new auth flow](https://stripe.dev/images/integrating-services-with-agents-stripe-projects/auth.png)

Once built, I was able to get auth working locally and in a deployed version, and after a few more back and forth iterations was ready to share the MVP.

### Takeaways

#### Think through your data and UX when planning

The requirements doc I made had data models as well as CSS values laid out for the agent. Because I already had specific ideas of how I wanted those to look, writing them out for the initial plan saved a lot of the back and forth I had experienced in previous builds.

#### Trust but verify (and research)

The initial plan had a major flaw: an in-memory rate limiter doesn't work on Vercel's serverless architecture. Next time I'll have an adversarial agent go over the plan to find issues like this before they come up in the build, as well as going through the provider's docs if I have any doubts a proposed solution won't work.

#### Be deliberate in your prompt wording

I specifically asked the agent "Use the best database and auth for the use case, though simpler to use would be better for both". When it saw that Supabase offered both, it naturally picked that as the simplest solution, though it ended up being the wrong one for auth. Initially biasing the agent towards simplicity ended up causing a significant refactor after the first build was complete.

#### Go with your gut

When I initially hit the email rate limit issue, I did ask the agent to give me possible solutions; it came up with implementing Twilio on top of everything. My gut said this was an auth problem and changing the auth provider should solve it, and that ended up being the case.

### Results

By the time I was finished, I had built a far more complex app with Projects than any previous endeavors, and ended up with five resources between four providers. But most importantly, I ended up with a working app:

![The finished leaderboard app](https://stripe.dev/images/integrating-services-with-agents-stripe-projects/leaderboard.png)

Before submitting my code for review, I switched to a different model and ran a security audit with one agent and a performance audit with another and patched some fairly trivial but important issues those agents found. All together, the leaderboard app took over a full day of building compared to just a few hours for my previous projects - which is worth putting into perspective considering a year ago it would have taken a whole team at least a week to put together.

If you want to get started with Stripe Projects yourself, head to [projects.dev](https://projects.dev/)!
