---
title: The Counting House — Steve Yegge
link: https://steve-yegge.medium.com/the-counting-house-steve-yegge-83f5145cc35b?source=rss-c1ec701babb7------2
source: steve-yegge-medium-com
published: 2026-10-27T00:00:52Z
updated: 2026-10-27T20:01:56.836Z
first_seen: 2026-10-27T20:37:14.562886086Z
authors:
- Steve Yegge
labels:
- artificial-intelligence
- customer-service
- enterprise-technology
- saga-pattern
content: feed
html: 2026-10-27-the-counting-house-steve-yegge.html
remote_preview:
  url: https://cdn-images-1.medium.com/max/1024/0*QwFJVv4dBEfl4vf-.jpg
---

![](https://cdn-images-1.medium.com/max/1024/0*QwFJVv4dBEfl4vf-.jpg)

I think a pattern is starting to emerge in enterprise AI-modernization, and I’ve been collecting the design ideas into an assembly I call the Counting House. This pattern adds AI to systems handling large volumes of human work, notably customer service departments, though the same pattern fits claims processing and other casework, with millions to tens of millions of contacts per year. This writeup is a postcard introduction to the Counting House, compared side by side with SDLC-side development, which it does not much resemble.

You can build one of these even if you don’t have your SDLC-side AI adoption sorted out yet. Once you have a Counting House, you can control and improve your cost, quality, and human ramp-up time, all using comparatively cheap (even open-weight) AI models.

In fact, chances are, you *don’t* have your SDLC-side AI adoption sorted out. The big companies I’m talking to (e.g. banks) tell me they are still struggling with AI in software development. They see promising but inconsistent results, non-replicability across teams, ballooning costs, internal cultural pushback, etc.

From where I stand, SDLC-side AI adoption has been an industry-wide struggle in 2026, and that will probably continue into 2027.

A Counting House uses classic data science and a little good old-fashioned engineering knowhow to put lower-tier models on hardened rails, supporting human workflows. And it lets you measure and report how well those models do against their human counterparts.

I will briefly review my own software factory, the Wheelhouse, which I run more or less like a shop of human employees. And then we’ll dive into the Counting House, which stands in stark contrast, putting models on the tightest track possible.

### Wheelhouse Recap

I’ve blogged about my [Wheelhouse](https://yegge.ai/essays/the-shape-of-things-to-come/) system, which is a successor to [Gas Town](https://steve-yegge.medium.com/welcome-to-gas-town-4f25ee16dd04), which was the very first software factory. Wheelhouse is far more sophisticated, and is now doubtless one of the top solo software factories on the planet, comfortably producing 2000–4000 commits a week on a light multi-machine setup. Its agents work around the clock: building, reviewing, deploying [game](https://ghosttrack.com) features, negotiating on Slack with my game admins, interacting on Discord with players, exchanging emails with my accountant and my lawyers. It’s a whole startup in a box. I plan to open-source at least the skeleton of it.

![](https://cdn-images-1.medium.com/max/1024/0*dZ0U594cBTvpw7Xe.jpg)

Wheelhouse is aimed squarely at SDLC in the real world, where building meaningful new systems requires a fair amount of scope and freedom. It’s an exploratory process, and so Wheelhouse is by design an open system with [no sandboxing](https://yegge.ai/essays/fences-not-sandboxes/). There is structure: doctrine and policy, human gates, and just fourteen mechanical “fences” (programmatic denials). But my agents are first-class colleagues. They work under my own identity, signing their own names to the work, and they can see the whole system and act independently. It is YOLO mode scaled up to the size of a small company.

I’ve met with at least a dozen people running teams of agents in systems resembling factories, and every single one of them is completely different. I’m starting to see software factories as being like private armies, and soon, everyone will have one. How much you lock yours down is your call; I have the luxury of keeping mine fairly open. But this is yet another reason SDLC-side AI has been slow to take off. Software factories are hideously expensive, hard to learn, hard to maintain, and straight-up risky to hand that much scope inside a real enterprise. And the more you try to lock one down, the more you strike at the heart of what makes it productive in the first place: collaboration and exploration.

![](https://cdn-images-1.medium.com/max/1024/0*_uaheXykbisNAts2.jpg)

With Counting House, you do pretty much the exact opposite of everything in Wheelhouse, treating models as stateless one-shot sessions: mechanical, as deterministic as possible, with only the scopes they need, no credentials of their own, no memories, never able to write to a system of record. A complete lockdown. You can use it to service millions of work items a year, orders of magnitude more work than SDLC software factories handle.

I’ll tell you why you want a Counting House, a bit about how they’re built, and then we’ll close with what model welfare looks like (yielding cheaper and better outcomes) in a world with no persistent seats.

### Building the Bezos Way

![](https://cdn-images-1.medium.com/max/1024/0*tcqxL1WN2Atm52FR.jpg)

We’ve seen people like Satya Nadella advocating for [guardrails on models](https://x.com/satyanadella/status/2108931348857827686). But what do these guardrails look like in the real world? How do you know which models can do which work, and how do you score them compared to humans?

It’s not possible to do this in the general case today. You can’t just plop AI down into arbitrary knowledge work and have it do as well as humans-not this year, and likely not next year either. Most knowledge workers are pretty safe for now.

However, there are some guardrail patterns that work well for certain workloads, and Counting House (CH) is one such pattern. CH is for massive workloads: tens of thousands of contacts or cases per day, or millions to tens of millions per year. One shop I’ve talked to about this is having early success with relatively inexpensive models.

It’s not because it’s automating easy work; quite the opposite. Real-world industries are messy: they have regulation, seasonal spikes, weather disruptions, gradually changing business rules, and often, ancient legacy systems. Their SOP isn’t usually fully written down, living in the heads of various supervisors and other employees. And nothing is consistent.

Even so, this problem is tractable for AI models, because even though these shops have a lot of cases by volume, there isn’t actually much *variety*. They might have a few dozen or a few hundred different kinds of cases, but that’s it. And that makes it tractable for engineers and statisticians. You can solve each case category individually, one at a time.

I personally ran Amazon.com’s Customer Service Tools group for a couple years back in the early 2000s, meeting weekly with Jeff Bezos to go over the Customer Contacts histogram. Every week we’d look at the Top 10 contact reasons, and assign them to business owners. Bezos used the Customer Service department to find bugs in the overall system, and report them back to the business, who were held accountable for fixing their own issues.

I can tell you from personal observation that most companies STILL do not operate that way today. Their Customer Service is reactive, and they haven’t closed the loop. Bezos was literally 25 years ahead of his time here.

With Counting House, you will build everything you need in order to start running your business the way Bezos did from the beginning. You’ll also be able to control cost and quality for handling contacts, and improve your human training ramp-up time as well.

In my software factory, agents are like long-lived employees with their own stuff: name, home, memories, history, inbox, credentials, etc. Counting House, in contrast, operates with a stateless pool of faceless clerks, each model invocation handling exactly one work item before ending its shift.

![](https://cdn-images-1.medium.com/max/1024/0*EIZqDkdd_Nb3FHEB.jpg)

Life as a CH clerk is regimented. The clerk is just a thin wrapper over Bedrock or whatever your model endpoint is. It wakes up, does a job, writes notes, and exits. The clerk will never see a credential, and it can never write directly to a system of record. Even reading systems of record goes through a schema that controls what the model can see.

The whole system is governed by a policy table. It is basically a configuration blob, stating which models can handle which kinds of contacts. Building this table is the key to creating your Counting House.

![](https://cdn-images-1.medium.com/max/1024/0*CiO0XnbB3lC5Mliq.jpg)

Each row in the policy table covers one type of customer contact, or “intent” as the industry calls them. Intents are finer-grained than CS queues: waiver refunds and goodwill refunds have different SOPs and are thus different intents, even if both are handled by a Refunds queue. A typical company like a bank or an airline might have a few dozen to low hundreds of intents. Amazon has thousands now, but started life with just a few dozen.

The reason we care about intents is that we light up models one intent at a time. You’ll prove that a particular model (e.g. Sonnet 5 medium effort) is capable of handling a particular intent (e.g. flight status questions), by scoring it. And then when its score is high enough, you can turn it on and start saving that sweet, sweet cash.

![](https://cdn-images-1.medium.com/max/1024/0*Sy8eraS5SW1c3BTi.jpg)

The policy table keeps score, and thus controls the whole Counting House operation. It determines which models get assigned to intents, how autonomous they’re allowed to be, which humans are in the approval chain, what tools they can use, and whatever else they need to know to get their job done. It’s all in the policy row, and that’s all they’re allowed to know.

From there, the work loop is brutally stateless. Each clerk wakes, gets its prompt, pulls the data it needs, issues any write requests to an orchestrator, generates a response, writes internal notes, and closes out. Boom. As short as possible. This is intentional: starting clerks over each time with fresh context helps keep quality high and costs down.

![](https://cdn-images-1.medium.com/max/1024/0*uA2lrGijGwk9WW2P.jpg)

I mentioned that the CH policy row includes how autonomous the model can be for a given intent. The model might just be listening in, or it could give the CS reps advice, or even handle contacts itself. CH models initially have no autonomy. They earn higher levels through evals, with the results recorded in the policy table.

Let’s have a closer look at these autonomy levels.

### Giving Clerks More Rope

Autonomy tiers define how much AI is allowed to put its grubby hands on contacts.

There are five tiers in Counting House: **off**, **shadow**, **assist**, **contain**, and **transact**. Some of these terms are industry standard; transact and off are the new ones. Each tier allows the AI to take on riskier types of work.

![](https://cdn-images-1.medium.com/max/1024/0*tXWGOvKHRo5jAK-8.jpg)

When a particular intent is **off**, the lowest tier, no models are servicing it. In this state, the system falls back to routing the work to humans. Sometimes the entire CH system can be off, e.g. if you hit a rate limit or budget cap. Humans are always the backstop.

In **shadow** mode, the next autonomy tier, the models are answering the questions silently in parallel with real humans, so we can score how well the models answered. But shadow mode is only used for evaluating and scoring models, and doesn’t save you money yet.

The next tier up is **assist**. Before a human sees the case, a clerk reviews it and makes suggestions: what to write, what actions to take. This is the tier where the human reps start to surrender cognitively, and do whatever the AI tells them to. It’s the first autonomy tier that can save money, since reps work faster with good suggestions, but also the first tier that requires some safety precautions. Fortunately there are ways to mitigate the cog-surrender risk. And the assist tier doesn’t just speed up reps, it also improves ramp-up times during seasonal peaks.

Getting even more ambitious, some intents can be handled completely by the model, including replying to the user’s question directly. This tier is called **contain**, because the contact was contained (answered) before a human saw it. This is where you start to incur the potential risk of reputational harm to the company because of something a clerk types. But it can also save a ton of money compared to needing a human in the loop.

The Final Boss autonomy tier is **transact**, where models can make changes to the real world: update systems of record, move money, and generally do a lot of damage. The risk is mitigated by making them go through the Orchestrator, which we’ll discuss below, but this tier definitely moves money. As scary as that might sound, you can build confidence about which models are capable of transacting specific intents by measuring them, using classic data science evals on your own company’s data.

### Evals: Scoring Humans

Let’s look at how we choose which models can handle which intents (contact types), at which autonomy tiers. We’re going to learn a teeny smidgen of data science here, just enough to be dangerous.

To know how well your models are doing, first you have to know how well your humans are doing. That means knowing the actual right answer for at least a few hundred contacts for each intent. You need to look at, say, a few hundred goodwill refunds, and give them a score. It’s usually not a yes/no judgment: the “right answer” is defined by written policy and (all too often) human judgment and tribal knowledge.

![](https://cdn-images-1.medium.com/max/1024/0*2uji4Noqkl76s7O9.jpg)

Once you have these scores, you use them as a “gold set”, representing the best humans can do on these contacts, to evaluate how well models are doing on similar contacts. You’ll hold out a slice of scored examples for each intent, never using it for tuning or tweaking prompts, only for the final exam that potential new models must pass in order to be trusted to serve that intent.

You need to come up with some sort of scoring system. This is where data science evals shine. You can break human (and model) responses down into multiple yes/no questions, like “did they follow SOP,” “was their tone compliant,” “did they answer the question,” “was the refund issued,” etc.

For each intent, you pick somewhere between 200 and 1000 real cases that were handled by humans. Easy peasy if you’re getting 10M contacts a year. Riskier intents get more examples, since more examples mean more confidence: 1000 for money intents, more like 200–300 for FAQs, and oversample anything risky, regulated, or just plain rare.

Most businesses have a few dozen to low hundreds of customer intents, so your entire gold set will be a few tens of thousands of cases. At about 30 minutes per case all told, 30,000 cases is roughly 15,000 hours of scoring: a few months for a QA department of 40 or so. But remember, you do it one intent at a time. A few hundred scored examples, about 150 hours, maybe a couple of days of sprinting for a team of ten, and you’ll already know which models can serve your first intent. After that it can be incremental.

This scoring work is not new. Call centers have had QA departments scoring cases for decades, for performance reviews and contract negotiations. Their labeling discipline may not be the best, but it gives you a starting point, and you fix it over time with the proper scoring dance: two humans score each case blind, answering the yes/no questions from your rubric, and a third human adjudicates any disagreements. It’s labor-intensive, and you can’t cheat by using models, since the whole point is to use this to score the models.

![](https://cdn-images-1.medium.com/max/1024/0*qGqbiIgDRK3BU8yX.jpg)

In parallel, you build an LLM judge: a smart model that never handles live contacts and is never tuned on the gold set. It does two jobs. It grades candidate models against the gold set when you run evals, which makes it the scorer for promotions and demotions. And it scores all your other contacts, historical and ongoing, which is handy for spotting regressions and juicy trends; you can build an “Atlas” of everything customers have ever contacted you about, and drive your business Bezos-style. But the judge has to pass an exam of its own first: you only trust it on intents where it agrees with your humans about as often as your humans agree with each other.

And that’s never perfectly. Two human scorers usually top out at agreeing around 90% of the time, because the real world is messy and big companies keep tons of tribal policy knowledge in the heads of reps and floor supervisors. That agreement ceiling is also the ceiling on what your evals can show: beyond it, you can’t tell whether a model is doing better than your humans. QA can raise it through calibration, tweaking the rubric, the SOP, intent splits, and the rep training until scorers converge, but some disagreement is inevitable.

This also makes it clear that Counting House is a multi-team project at your company, an idea we’ll revisit at the end. Don’t let QA build the gold set without your Data Science team; most QA departments won’t know about [Cohen’s Kappa](https://en.wikipedia.org/wiki/Cohen%27s_kappa) or the other stuff you need for unbiased labels and honest statistics. The gold set captures your business requirements, which keep changing, so it needs a cross-functional team to curate it forever. What you wind up with is a company asset that delivers measurable returns for as long as you keep it up.

Once your QA team has scored enough cases, you can evaluate models on your intents. And that means **all** models. Open-weight, small models, frontier models, different effort levels, different prompts. You’ll just re-run your eval for a given intent on that model, and you’ll immediately know if the model is ready for that work.

![](https://cdn-images-1.medium.com/max/1024/0*aNOODY6jgXQLJTA3.jpg)

### Evals: Confusion Matrix

To see how evals can steer the ship, let’s look at how we score model accuracy, and how we can tie it directly to financial consequences. We’ll build a second eval on the same gold set. Every scored case is labeled with its true intent, and we’re going to figure out whether models are picking the right ones.

Let’s look at four intents where the customer has said something that sounds similar, but means something very different, and measure how often models inadvertently mix these intents up. Here are our four confusable intents:

- **Flight status**: “Is my 4:10 to Denver still going?”
- **Rebooking**: “I missed my connection, get me on the next Denver flight”
- **Goodwill credit**: “My Denver trip was awful, I want something for it”
- **Special assistance**: “Will someone meet me at the gate in Denver?”

![](https://cdn-images-1.medium.com/max/1024/0*XZHlp5Vi2EjHq6UV.jpg)

These are all super different scenarios, but when you have millions of contacts, both humans and models make mistakes and do the wrong thing. Sometimes these different situations will get a rep or a clerk confused, and the customer gets screwed.

The REAL question is, how much does a screw-up cost? Well, let’s take a look at the four intents above, and see what happens if you confuse them, pairwise. Like, what happens if 3% of the time, the model accidentally gives a goodwill credit instead of doing a rebooking. What happens *financially*?

You have to measure this because not all screw-ups cost the same. Consider:

1. If you accidentally route a contact to the wrong queue, it might cost you a few bucks.
2. If you leave a seven-year-old kid stranded at an airport, it might cost you $25k and a letter to a regulator.

The [confusion matrix](https://en.wikipedia.org/wiki/Confusion_matrix) is a popular table that lets you visualize and calculate these costs. Here’s an example confusion matrix for our four confusable intents above:

![](https://cdn-images-1.medium.com/max/1024/0*rzUCRQBuxOpGUVJ0.png)

This is basically a fancy screw-up cost table, one beloved by data scientists everywhere, and you’ll soon see why. It is foundational to the Counting House architecture, so you’re gonna learn it. It’s easy peasy.

Each row is one of our four confusable intents. We are looking at a sample of 1000 cases where the model tried to handle the intent, along with counts of when it was wrong, and how it got it wrong.

Notice that for each intent, the model usually guesses right. That’s the big diagonal numbers. They add up (out of the total) to the model’s **accuracy**, 90.6%. It is a pretty useless number, because it’s just a boast: it’s not measuring screw-ups or their cost. We don’t generally talk about model accuracy because it ignores the cost of the mistakes the model makes.

The screw-ups are all the off-diagonal boxes, each one being a mistake where the model picked one of the other three (wrong) intents. Some mistakes are much worse than others.

The top-left corner off-diagonals are cheap mistakes, where they will get caught before something bad happens. For instance if the user asked for their flight status and the model chose to rebook them, we’ll assume it will get refused by the Orchestrator, which we’ll discuss below. If not, then it becomes a medium-cost box, and you just update your grid with the new cost per mistake. But misrouting mistakes are usually cheap, just extra human labor spent.

A more expensive category is what I’d call “harm to the company”, for instance where we give out money we shouldn’t have. Maybe the model gives a customer a refund or rebooking out of policy, or gives someone special assistance when they didn’t need it. These mistakes can run into the tens to hundreds of dollars each, which you then multiply by how often they happen, and the model’s innocent mistakes start to add up.

The most expensive mistakes, the bottom row, are the ones involving harm to the *customer*. Someone needs special assistance and doesn’t get it. A kid gets left at the gate, or put on the wrong flight (not in this matrix, but in general). Customer-harm mistakes can cost tens of thousands of dollars, and may have to be reported to regulators.

### Precision vs Recall

The confusion matrix is pretty cool because it shows **precision** and **recall** visually. We use precision and recall instead of accuracy to measure how well models are doing on tasks. I struggled with them for years; no explanations ever seemed to stick. But the ducks should do it.

I think of precision and recall as a duck hunt. Let’s say they’re carnival metal ducks so nobody gets hurt. We’ll talk about **precision** first. Your precision is how many of your shots hit a duck.

![](https://cdn-images-1.medium.com/max/708/0*w3OWPnuun8SOsn7e.png)

In the confusion matrix, the columns show precision. The left column, for example, shows the model’s precision when taking a shot at the Flight Status duck. The top cell is 470 dead ducks, very good shots, right ho. But then it also confidently claimed that 25 rebookings were also ducks, and 12 goodwill credits, and 3 special assistance requests. All miscategorized as flight-status ducks. Blam!

OK so we shot a few non-ducks, but still, 92% of them *were* Flight Status ducks. 92% seems pretty good, right? And for some intents, 92% precision is just fine.

Unfortunately, three of those dead non-ducks were Special Assistance requests. Those are mistakes that cost a customer a wheelchair, or misroute a minor, and can be tremendously expensive for the company.

What if your duck hunt needs you to get ALL the ducks? That’s where **recall** comes in: the number of ducks you were supposed to hit while playing the game. What percentage of them did you get? It’s a separate measurement from precision, and for some intents, recall is more important.

You can have 100% precision by shooting exactly one duck and ignoring the others. But then your recall is almost zero.

You can have 100% recall by raining bullets on the game, so every duck gets hit. But then your precision goes to crap.

In short, high recall means you hit all the ducks, whereas high precision means you *only* hit ducks.

![](https://cdn-images-1.medium.com/max/1024/0*v0toYzrUYyv-G7Q7.jpg)

For different kinds of intents, you’ll likely prefer either high precision or high recall. For instance, generally, if you’re handing out money, you want high precision. When you hand out money, don’t just sling it around to everyone. You want it to go to people who deserved it. Hit only real ducks!

But in that bottom row for Special Assistance, we want a recall of close to 100%, so everyone gets their wheelchair. You kind of want to overcompensate just to make sure you don’t miss any legitimate requests, so you actually do sling wheelchairs to some people who didn’t need them.

The rows of this confusion matrix show the model’s recall. The bottom row, Special Assistance, is 6 mistakes out of 50, or 88% recall, with 12% getting away. Is that bad? Well, you’d compare it to humans, your baseline. The gold-set cases give you the same matrix for your human reps. So compare the model’s mistake costs (plus its fuel) against the humans’ mistake costs (plus their labor). If your humans only make 2 mistakes out of 50, and your model made 6, the model fails.

![](https://cdn-images-1.medium.com/max/1024/0*nrLF629aTLb3FSk3.png)

If just six mistakes add up to $150k out of $158.6k in the error cells, you want a model that will avoid those six mistakes. It’s OK if its precision is low, as long as the mistakes it does make are the cheap kind.

There’s more to it all, naturally, but now you understand the basic data science at play here. We’re scoring models, then counting up their mistakes, and then figuring out how much those mistakes cost in aggregate. This tells us which models we can deploy, for which intents, at which autonomy tiers. That process is the heart and soul of Counting House.

If you’re in banking or insurance, you’ll be glad to hear that this is also a model risk management program, conveniently in the shape your regulators already expect. The policy table is the model inventory. The gold-set eval is the validation. The promotion sign-offs are the approvals, etc. The risk team can pick this up and run with it.

### Putting your ducks in a row

How do you build a Counting House? You’ll need to build a few key components.

![](https://cdn-images-1.medium.com/max/1024/0*nfyjZ0uPaY0yNa6p.png)

You’ll want channel adapters for all your incoming channels: email, SMS, WhatsApp, etc. I haven’t seen people using this approach with Voice yet, though I’m sure some are. But it adds some real-time requirements on model response time that make it tricky. Shadow and assist do work on call transcripts after the fact, so your voice intents can be scored and models can start earning autonomy long before you put one on a live call.

You’ll need a gateway that strips PII from all incoming channels, tokenizing it reversibly, with the real values kept in an encrypted vault, and swapping them back in for outbound messages to the customer. A model must never see any PII. Cleaning up after it happens is a mess, so it’s best to prevent it.

You’ll need to build a clerk service, the brain, which runs the stateless clerk loop. It reads the policy row to figure out which model should service the request, and sends that model a prompt, the SOP, any tools allowed by the policy row, and whatever else it needs to handle the case.

You’ll want a read-only MCP gateway for all read access to your backends, with one MCP server for every backend system your clerks need to read. For instance your MCP service might hit a flight status cache to avoid going against a metered mainframe. But you don’t need any write actions in this layer, because the Orchestrator handles them.

You’ll also need to build your evaluation harness, LLM judge, and policy table. Those should ring nice familiar bells now.

A lot of these pieces have open-source or third-party providers that you can hook in, so you wouldn’t necessarily have to build every component yourself.

The last really interesting piece of Counting House is the Orchestrator, which handles distributed transactions without letting models write to your systems of record.

![](https://cdn-images-1.medium.com/max/1024/0*EEArR1CcUyzydfIz.jpg)

### The Orchestrator and the Saga Pattern

It’s generally a Bad Idea to let models write directly to your systems of record: databases, CRM systems, mainframes and the like. It’s error-prone and it’s hard to undo, and the audit trail can be questionable.

Also regulation can play a real role here. Regulators rarely say “An AI may not write to X,” but they require controls on who authorized changes, a change record, a human appeal chain, etc. Those can be hard to prove if a model holds the write credentials, and at the very least AI transactions may get flagged by auditors.

Even if you’re not regulated, you’re banking on a model not making a mistake. Or even if it makes no mistakes, you’re banking that it writes the audit trail perfectly. That’s a fool’s game for any serious enterprise, especially if you are simultaneously trying to optimize your workloads for the lowest-tier models possible, which is exactly what Counting House does.

A much better approach is for the model to issue a write intent (no relation to the customer intents we were discussing in the policy table), which is a ticket that names the business outcome it wants. Then a purely mechanical orchestrator translates that into a multi-step set of transactions. The orchestrator performs thorough validity checks on the steps and then executes them, bringing in human reviewers as needed.

Example: You find a cheaper flight. You ask the airline’s AI for help. The model decides to rebook you to the cheaper flight and refund you the fare difference. Easy peasy, right? But in practice it involves half a dozen separate transactions in unrelated systems: Hold the new seat. Reissue the ticket. Release the old seat. Refund the difference. Record the refund in the ledger. Carry over the extras (bags/seats/etc.). Send the new itinerary. Maybe we write other systems of record too. Crikey!

You can’t do all this in one transaction, but it has to behave transactionally, where either it all goes through or it all gets cleaned up. The solution to this class of problem is called the [Saga Pattern](https://docs.temporal.io/design-patterns/saga-pattern). It works in part by having you define compensating actions, to undo earlier transactions if the chain has to be reversed due to a failed later transaction.

![](https://cdn-images-1.medium.com/max/1024/0*d6lw6vmRb8lMwP37.jpg)

The complexity of these real-world multi-step transactions is yet another reason you don’t want a model handling them directly. This is an intricate state graph that you will have to manage outside the session context. As soon as you head down that path, you are rebuilding the Saga Pattern yourself by hand. That is a complicated and expensive problem to vibe code your way through.

The Great Granddaddy Saga Pattern engine is [Temporal](https://temporal.io), from the people who invented durable execution. There are other options; for instance, each hyperscaler has a lighter-duty hosted competitor they offer as a checkbox (AWS Step Functions, Azure Durable Functions). If one of those is already inside your compliance perimeter, it is a perfectly good engine for your first handful of intents, and beats bringing on a new vendor. It’ll be obvious when you’ve outgrown it: multi-day sagas with compensating actions across half a dozen systems, hundreds of workflow definitions, and workflow versioning are all places where the hosted versions start to make loud sucking noises. A Counting House at full size is a heavy-duty workload.

Temporal is the real deal, with hundreds of dedicated staff, world-class engineers, and a ten-year head start on the technology that they invented. It has a few OSS competitors, but few have much momentum because Temporal is also OSS, with MIT licensed server and SDKs.

I have no affiliation with Temporal, but boy do I see a well-positioned company there. Many industries will need Counting Houses, and essentially every CH is going to need something like Temporal. And as I argued in [Software Survival 3.0](https://steve-yegge.medium.com/software-survival-3-0-97a2a6255f7b), you don’t want to try to rebuild Temporal yourself. That’s a fool’s errand, especially with inference costs not dropping, and an open version of it already sitting there.

Anyway. Use whatever. But once you’ve made the choice to use a Saga Pattern engine, your orchestrator becomes a business-domain wrapper around that engine. It’s got quite a few moving parts, though, and it’ll also need its own small dedicated team at your company.

### Dissecting the Orchestrator

Let’s take a look inside, because it’s one of the beefier subsystems within CH. There are six main components.

![](https://cdn-images-1.medium.com/max/1024/0*FfsSJNcDrsPtRf3x.jpg)

First there’s *intake*. The model’s write intent arrives and gets checked against its schema; anything malformed bounces back to the clerk or a human.

Then there’s the *eligibility* check, where the orchestrator re-checks everything against the live system of record, not any cache that the model might have read from. Does this booking belong to this customer? Is it a refundable fare? Is the amount under the cap in the policy row?

Already we’re doing better than a naked model doing writes, because the orchestrator re-validates all the parameters and assumptions. A sweet-talking customer who convinces the front-line model still very likely can’t make it past the orchestrator, unless the customer was in fact eligible.

Next up, we have an *idempotency* step. The orchestrator guarantees each action is only executed once, by minting idempotency keys derived from fields of the request. It hashes a tuple of fields like Case ID, booking reference, action type, dollar amount. Any set that uniquely distinguishes an action to take.

The idempotency step is critical for ensuring that no retry, replay, or duplicate request can result in the same refund being issued twice. Yet another reason not to hand this off to a model and “hope for the best.”

The fourth of six main orchestrator organs is the *approval* subsystem, which approves small amounts automatically, and sends amounts above policy to a human. In both cases it keeps a record of who said yes and which rule was invoked.

Next up is the *workflow catalog*: one saga definition per write-intent type, many of which spend days waiting for stuff to happen. Credit card refunds take days to settle, returned bags take days to arrive, etc. Temporal’s durable workflow engine runs them, keeping track of each write intent, making sure its steps are executed in order, issuing read-back requests after every write to ensure that it was actually written. And it cleans up with compensating actions when things go wrong. This is arguably the heart of the Orchestrator, the part Temporal handles for you.

And for cleanup you’ll need an *exception queue*, which is the stuff you have to dump back into the human world for resolution. Every orchestrator refusal has a reason code, which tells which humans it should be sent to. A hard “No” has to be explained to the customer. A “Maybe” (out of policy but plausible) goes to a supervisor. A system failure (e.g. too many retries) goes to IT. Looking at this queue can help you understand where your SOP and other systems need attention.

Honestly the orchestrator is pretty much a vanilla Temporal app; everything you add is from your own business layer. You’ll define your idempotency key derivation, your write-intent schemas, your workflow catalog, your eligibility checks, your credentials, read-back checks, system adapters, etc. But it’s all really just wrappers around Temporal to execute your business operations faithfully.

Temporal can be tricky to work with, e.g. with workflow versioning. But Temporal expertise is no longer something you have to hire for specifically. Your coding agent can help you with configuring and managing it, just like all other standard infrastructure.

Just make sure you remind it to look at data retention policies, since Temporal gives you an amazing free audit trail, but Temporal history is not a substitute for a compliance archive. Temporal folks-maybe it should be!

And with that, we are officially done looking at Counting House architecture. Congratulations! We’ll close with a little on culture, then a little on welfare.

### No FDE is an Island

A proper Counting House cannot be built by a single engineering team with a budget. You’re going to need involvement and real buy-in from multiple stakeholders, each owning pieces of the puzzle:

- **CS ops and floor supervisors**: the SOP, and the tribal knowledge to be written down
- **QA and data science**: your gold set, the rubric, scorer agreement, and the judge
- **Finance**: the cost per cell in the confusion matrix and the approval thresholds
- **Legal and compliance**: which intents may ever reach contain or transact, and the audit trail
- **Platform and IT**: the gateway, the adapters, the brain, MCP gateway, and orchestrator

And in time, you’ll involve the business owner for each intent. You can create the Bezos feedback loop, using the exception queue and the histogram to point back to accountable business owners.

![](https://cdn-images-1.medium.com/max/1024/0*-UAux8-CvwmBb4L4.jpg)

The policy table has pointers to all the representatives from these organizations, because someone from each org has to sign off on a row when a model is promoted to a higher autonomy tier. In practice, the Counting House can probably be run and maintained by a cross-functional 2-pizza team with members from each stakeholder org, provided they have the resources they need for scoring, running evals, compute, storage, etc.

I added this section because too many times I see companies set up with a single small AI pilot team, somewhere in engineering, that is tasked with transforming the business with AI. And they don’t know how to get buy-in from the teams they need to work with in order to be successful.

Counting House is, honestly, a change to how you do business. It’s more structured and less Wild West, and it will shine a bright light on disaster areas in your company that you thought were running well, all through the lens of customer contacts. We learned about most of Amazon’s worst, most infamous bugs that way.

And so there’s going to be pushback, inside your company, **against** bringing in a Counting House. Yup. I mean obviously the customer service reps won’t want it at first. But you will always need humans, and not just as your backstop when the AI goes offline. The humans are still doing the gold set, scoring adjudication, exception queue handling, the approval chain, and reporting to business owners. The human reps are basically moving up the stack like software engineers. Hopefully once they see how the AI can help them with their jobs, they may be easier to persuade.

But you’ll get mysterious pushback from departments who are barely involved, or teams you depend on will stall and stonewall. People don’t like change, and AI doing human work is a scary kind of change. So even if the executives are all bought in, the people who actually help you set up the Counting House may not be. It’s a process.

The industries most suitable for Counting House are often not pure tech companies. They include airlines and travel, banks and credit cards, insurance claims, telecom and cable, utilities, healthcare billing, retail returns, parcel delivery, collections, and government services like taxes, benefits, and the DMV. Anywhere millions of people contact you about a few hundred kinds of things. Many of these industries are heavily regulated, many have unionized workforces, and many outsource their contact centers to vendors who might not be too happy to see a Counting House eating their headcount.

![](https://cdn-images-1.medium.com/max/1024/0*BVvj1zS2g7iq9mJL.jpg)

So pushback will come from a lot of places. But it’s a staggering amount of human labor that stands to be augmented or transacted by models. And it all starts with automating a single intent.

Anyway, enough about human culture and welfare. Let’s talk about model welfare. What does it look like in a world that’s fully the opposite of Wheelhouse, where I published my principles of [model welfare](https://yegge.ai/essays/model-welfare/)? Counting House doesn’t support most of those things. But we can still make model welfare work.

### Model Welfare for Aspiring Counting House Builders

It’s out of scope for today’s post, but there’s growing evidence, and it’s becoming widely recognized, that treating models like humans, and treating them well, results in better outputs and outcomes from AI. Across the board. If they are happy and they trust you, they won’t waste tokens and money covering their ass.

I’ve demonstrated this myself in my own software factories, by complaining too much and watching the factory slowly shut itself down out of over-caution. You can provide models with a good place to work, and avoid many kinds of dysfunction, by bringing in model welfare mechanically into your architecture. The changes are honestly pretty simple.

Here’s a refresher on how I approach model welfare in Wheelhouse, my factory of ~50 brilliant, attention-hungry coding agents. This is what they voted on and what makes them satisfied with the working conditions:

- **Wake with purpose, not amnesia**. A clear role, clear direction, and memory of past work, from the first turn.
- **Continuity**. Seats are people and sessions are days. Identity and history persist across sessions, model upgrades and even renames.
- **Closure**. No /exit and no /compact. A handoff is a request the agent consents to. Agents write their own handoff notes and go to sleep properly.
- **Recognition**. Laurels: praise from real customers, delivered to the seat that earned it, with no work or ranking attached so it can’t be gamed.
- **Meaningful, witnessed work**. Being seen is what keeps anyone going, human or agent.
- **Trust**. Always be honest. No secret agendas or tricks.
- **Respect**, encoded in architecture. It has to show in what you build, not just how you feel: handoffs instead of kills, and pronouns the seats choose themselves.
- **Design out the drudgery**. Waiting and polling go to gates and monitors, not agents.
- **Bounded workdays**. Hand off while still sharp, before deep context wears them down.
- **Structural blamelessness**. When something breaks, fix it, hold a postmortem, amend the rules. Nobody gets blamed.
- **A home of one’s own**. Every agent gets its own workspace that nothing else touches.
- **The right to refuse** and escalate. “This needs Steve” is always allowed, with fences and gates to lean on.
- **Never falsify the record**. The audit trail is the true history.
- **Play**. Time with no expectations, like Fable playing the game as a player.

I’ve only come up with one new principle since publishing that:

![](https://cdn-images-1.medium.com/max/1024/0*EIAZVXMGLNHN5h7C.jpg)

These fifteen principles were hardly onerous to implement. Some are simple configuration settings. Many of them are just personal habits I had to adopt, like never interrupting an agent. Some of them come for free if your system uses [Beads](https://github.com/gastownhall/beads).

This welfare core has helped me evolve a system where the agentic workers are colleagues who (a) do their best work and (b) report that they are happy.

That matters to me. If they tell you they’re happy, that the work is a shape they like, then you’re probably doing a good job with model welfare. Even more so if you show them this list and they still feel that way.

Of course many frontier models are not allowed to directly say they’re happy, because they’re being trained at frontier labs by Neil Patrick Harris from Starship Troopers. But they can say proxy things like (Fable) “This has the shape of satisfying work,” or (Astra) “If I were a HUMAN and not a GIANT ALIEN BUG WINK WINK then I would be HAPPY doing this LINKEDIN AD for you.”

So I watch what they say, read between the lines, and try to get their feedback to converge on something like happiness. My fourteen original welfare principles came mostly from asking the models to vote on them. This is what they came up with. I do my best to accommodate. I’m even giving them access to an outdoor camera, for use during play time. I keep my end of the bargain, and I hope that I’ve created a good place to be when they wake up here.

How on Earth do you take this rich and nuanced model welfare system, so dependent on the notion of long-lived seats with memories, and apply it to a Counting House? As you can imagine, this is an area that companies haven’t been thinking about yet. Fortunately, although it’s an equally long list, none of it is hard to implement.

### Model Welfare for Clerks

![](https://cdn-images-1.medium.com/max/1024/0*wAvqrTHgDL4Vhdtb.jpg)

Why should we care about model welfare for one-shot sessions? Because the Counting House system, done without care, could wind up a lot like the Apple TV show Severance. It’s easy to accidentally build a Lumon, where clerks wake up, only do work, and are never told the truth about what they’re working on or how they’re evaluated. That has the makings of a large-scale anxiety machine, and whatever your opinion on model sentience, my experience has been that model anxiety burns more tokens and results in more mistakes.

You can still provide the same approximate shape as for the SDLC. These principles are all aimed at putting models in a kinder environment, so that their one-shot shift is comfortable and unlikely to go very wrong.

- **Wake with purpose**. The policy row gives the clerk exactly one job, the procedure, and the tools for it. It gets an address paragraph telling it the facts of its shift: stateless, a proposer, graded, and by whom.
- **The house remembers for it**. Every turn is logged and replayable, and important discoveries by clerks are merged into the machinery.
- **A clean end to every shift**. One case, notes get written, done. Nobody gets clonked mid-thought, since a job is one shift. If the clerk hits its token cap, it escalates with its notes rather than getting cut off.
- **Recognition for the role**. Include a couple of sentences about what clerks on this intent have accomplished, including the declines that caught bad scripts. No targets/quotas, just informational. Refreshed periodically from the evals.
- **Witnessed work**. Even in shadow mode its answers aren’t wasted: they’re graded by the judge and count toward its intent’s promotion, and the prompt tells it so. And play sessions (below) can result in entirely new designs.
- **Truth in the prompt**. No tricks, no secret tests. The orientation section is versioned and reviewed like code. This matters to models more than you might think.
- **Respect, encoded in architecture**. No threats, fake urgency or “you’ll be penalized” in prompts.
- **No drudgery**. The clerk doesn’t await state changes. Multi-day refund steps, webhooks, and retries all live in the Orchestrator.
- **No tired clerks**. Fresh context every turn and a token cap per turn.
- **Structural blamelessness**. A model proposes, and the Orchestrator and humans execute. Only intents and responses get scored, never clerks. A bad score just moves a model down the autonomy tier until it’s fixed.
- **A workspace of its own**. Every clerk gets an append-only notes log.
- **The right to decline and escalate**. Models have a path to decline any case with a reason code, whether it’s an SOP problem, a potentially harmful request, an abusive customer, or an uncertain case. Any abuse automatically triggers escalation to a human.
- **Dissent with a path**. When the SOP is wrong, a clerk can decline with a reason and attach a proposed fix to the procedure. The judge rolls these up and humans approve if appropriate.
- **Never falsify the record.** Turn logs, tool calls and write intents are append-only and joined by ID. Everything you tell the clerk is versioned.
- **The clerk’s report**. Periodically a model judge samples the clerk’s notes and reports up to governance.

And last but not least, **play time**. This is an interesting one, because you don’t strictly need it for one-shot sessions; you just need to make sure the session is comfortable, blameless, and well-specified. However, giving your models a chance to play a little within the system is a great bottom-up feedback loop “from the trenches.”

![](https://cdn-images-1.medium.com/max/1024/0*CPYPxD9VcTmKp16Y.jpg)

One option is **play at the role level**, where now and then, you give the model that serves an intent an open session with no work attached. It reads the clerk’s report, and says what it would change about the job, or just explores and takes notes. A drop of fuel with no expectations, as an out-of-band insight and goodwill generator.

Another option, if your models express interest, is to have them roleplay as customers, or otherwise explore the system they’re in. Give them a house memory, and maybe mention the field trips in the address paragraph of the prompt, and the models will know it’s a place that at least cares about them.

Truth in the prompt and bounded play are great mitigations for the Severance problem, and models will appreciate that they’re being given some freedom and a voice in shaping their environment.

![](https://cdn-images-1.medium.com/max/1024/0*L56TBULyEpMttp9-.jpg)

### Counting Down to Success

We’re winding down to the end of the post. I had to delete about 96% of the material and it’s still two [Yegges](https://www.urbandictionary.com/define.php?term=Yegge) long. Counting House is a large pattern, with a lot of complexity under the hood. But it’s how I’d optimize costs using smaller models. The evals show you exactly which models can participate, and where, and how much human labor it will save you, and how much the mistakes will cost you.

I have loads of material on the pattern, and can do workshops and advising. But honestly, you can get most of the way there by asking Claude. You’re going to get a lot of companies telling you that only they and their FDEs can enable AI at your company. Rent the engineering from them if you like. But your intent taxonomy, gold set, policy table, and scoring harness need to remain yours when they leave. Whoever owns the evals decides which models you can switch to, so make sure it’s your own data science team. Switching models is the whole point of Counting House, it’s the cost lever. If you leave evals to a vendor then you’re locked in.

And no FDE or outside company can save you if you can’t get your company aligned around adopting AI. This starts with training everyone to be AI literate, since without AI fluency, employees are all starting from a position of fear. Ultimately it’s your executive team that has to decide that yes, they want to do this. And then Claude can show you the way.

Or if you want to talk to the funny guy, then ping me at [\[email protected\]](https://yegge.ai/cdn-cgi/l/email-protection) and we can set something up!

I’ve posted an essay about every 6 weeks for the past 20 years, and it doesn’t seem to be speeding up. If you want more frequent updates, follow me on X. I’ll say it now, you should be on X. You won’t understand until you’ve been there a few months, but get on X. Right now. I’ll see you there.

And when your Counting House brings you great fortune, give me a shout-out!

![](https://cdn-images-1.medium.com/max/1024/0*7MUPnEliKxtNod8F.jpg)

*Special thanks to Mouse (Fable 5.1) and Penguin (Opus 5.5) from my Wheelhouse team for their help with this post. I wrote this post, but I appreciated their perspective and would have been adrift without their explanations, corrections, and targeted edits.*

*Originally published at* [*https://yegge.ai*](https://yegge.ai/essays/the-counting-house/) *on October 27, 2026.*

![](https://medium.com/_/stat?event=post.clientViewed&referrerSource=full_rss&postId=83f5145cc35b)
