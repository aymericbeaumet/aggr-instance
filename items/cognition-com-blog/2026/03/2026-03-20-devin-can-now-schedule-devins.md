---
title: Devin can now Schedule Devins
link: https://cognition.com/blog/devin-can-now-schedule-devins
source: cognition-com-blog
published: 2026-03-20T00:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2026-03-20-devin-can-now-schedule-devins.html
---

Every engineering team has a quiet backlog of things that should happen on schedule, but often slip – like weekly release notes, feature flag cleanups, and onboarding QA sweeps. None of these tasks are difficult, but they slip through the cracks precisely because no single instance feels urgent enough to prioritize.

Starting today, Devin can schedule its own sessions to take care of work like this. You simply describe what should happen on a recurring basis, and Devin figures out the cadence, sets up the schedule, and runs it automatically going forward. There's no cron job to configure and no workflow builder to learn; instead, you're just telling Devin what you need done, the same way you would for any other task, and Devin takes ownership of making sure it keeps happening.

## **It starts with one good session**

The most natural way into this feature is to let Devin do something once and then tell it to keep doing it.

For example: you might ask Devin to audit your feature flags, and find anything that's been at full rollout for more than two weeks. Devin opens a PR to remove the flag and pings the owner. The results are exactly what you wanted, and it occurs to you that this is the kind of thing that should just happen every Monday morning.

Now, you can simply tell Devin: "Schedule this for every Monday at 9am." Devin will set up a schedule to wake up each Monday at 9am and run the same workflow, so you can stop thinking about it. One good session can save you time every day, week, or month on recurring tasks to support your work.

## **Devin remembers what it did last time**

One feature that makes this meaningfully different from a scheduled script is that Devin carries state between runs. It reads and writes its own notes across sessions, which means each run builds on the context of the one before it rather than starting from scratch.

This matters most for tasks that accumulate over time. If you set up a Devin to compile release notes every Friday, it won't re-summarize the PRs it already covered last week, because Devin knows where it left off, picks up from there, and gives you a clean summary of just what's new. If you have a Devin watching your #feature-requests channel every morning, it tracks which messages it's already processed, notices when themes recur, and surfaces only what's changed since yesterday. The longer a scheduled Devin runs, the more useful its output becomes, because it's building on a growing body of its own context rather than treating every run as an isolated event.

## **Works with Managed Devins**

Earlier this week we launched [Managed Devins](https://cognition.ai/blog/managed-devins), which let Devin break large tasks into pieces and delegate them to parallel agents, each running in their own isolated VM. Scheduled Devins and Managed Devins compose naturally together.

That means you can set up a weekly QA pass where Devin spins up a managed Devin for each page of your application, tests them all in parallel, compiles the results into a single report, and posts it to your team's Slack channel — automatically, every Friday afternoon, without anyone needing to kick it off. The combination of parallel execution and recurring scheduling is where this starts to feel less like a developer tool and more like a member of the team that just handles things.

## **Try it now**

Here are a few examples of the kinds of recurring work that Scheduled Devins handle well. Click any prompt to open it directly in Devin.

### **Clean up stale feature flags every Monday**

[Check our feature flags. If any flag has been at 100% rollout for more than 14 days, open a PR to remove it and ping the owner. Then schedule this to run every Monday at 9am.](https://app.devin.ai/?prompt=Check+our+feature+flags.+If+any+flag+has+been+at+100%25+rollout+for+more+than+14+days%2C+open+a+PR+to+remove+it+and+ping+the+owner.+Then+schedule+this+to+run+every+Monday+at+9am.)

### **Compile release notes every Friday**

[Every Friday at 5pm, pull all PRs merged this week, categorize the changes into bug fixes, new features, and infrastructure, write release notes, and post them to #engineering in Slack.](https://app.devin.ai/?prompt=Every+Friday+at+5pm%2C+pull+all+PRs+merged+this+week%2C+categorize+the+changes+into+bug+fixes%2C+new+features%2C+and+infrastructure%2C+write+release+notes%2C+and+post+them+to+%23engineering+in+Slack.)

### **Run QA against staging every morning**

[Every morning at 8am, run QA against our staging environment. Navigate through the core user flows, take screenshots, flag any regressions or visual issues, and post a report to #engineering. Use managed Devins to test flows in parallel.](https://app.devin.ai/?prompt=Every+morning+at+8am%2C+run+QA+against+our+staging+environment.+Navigate+through+the+core+user+flows%2C+take+screenshots%2C+flag+any+regressions+or+visual+issues%2C+and+post+a+report+to+%23engineering.+Use+managed+Devins+to+test+flows+in+parallel.)
