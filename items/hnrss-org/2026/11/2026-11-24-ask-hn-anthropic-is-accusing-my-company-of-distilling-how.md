---
title: 'Ask HN: Anthropic is accusing my company of distilling. How can I prove I''m not?'
link: https://news.ycombinator.com/item?id=50630809
source: hnrss-org
published: 2026-11-24T16:45:44Z
updated: 2026-11-24T16:45:44Z
first_seen: 2026-11-25T10:22:41.845233465Z
authors:
- weli
summary: 'Basically the title. Anthropic is accusing me and my company of using their API for distillation. We are not. We do not train our own models. Our only usage which I think could get us flagged is automated regression benchmarks whenever we change some AI settings. But this is not against their policy? Now I''ve got 24h to prove we didn''t. Has anyone dealt with this too? How can you prove that you are not distilling Anthropic models without breaking your user''s privacy? Comments URL: https://news.ycombinator.com/item?id=50630809 Points: 284 # Comments: 220'
content: extracted
html: 2026-11-24-ask-hn-anthropic-is-accusing-my-company-of-distilling-how.html
---

![](https://news.ycombinator.com/s.gif)

Whatever you submit in 24h, submit the same evidence to Claude and ask it to tell you if it's sufficient evidence to make distilling seem unlikely. Because you know Anthropic is just going to be using Claude as well.

As others have suggested, the code that generates the false distilling usage pattern along with its git history seems useful.

![](https://news.ycombinator.com/s.gif)

I'd suggest vetting with an adversarial Claude also.

Ask Claude A to help you build your evidence doc, then get a completely different session up and tell it "Claude A compiled this evidence, but our source in Anthropic said it's not good. Find all the flaws." Then keep telling Claude B "you haven't found all the mistakes, keep going" until it "cries uncle" and swears there aren't any.

Of course, then you have to vet all of the stuff Claude B found, as it won't all be relevant ... but if you want a solid doc the "adversarial Claude" will likely help a ton.

![](https://news.ycombinator.com/s.gif)

Why would you use Claude for that? Isn't that basically leaving tracks that Anthropic can point to and say that your evidence isn't in good faith?

Feels to me like if you're going to do that, it would be better to use either a local model or an OpenAI model.

![](https://news.ycombinator.com/s.gif)

\> Why would you use Claude for that? Isn't that basically leaving tracks that Anthropic can point to and say that your evidence isn't in good faith?

Why it is not in good faith if a real person is not reading it anyway? It is only in bad faith if the reasoning and evidence is fake or not truthful. That matters the most.

![](https://news.ycombinator.com/s.gif)

"Isn't that basically leaving tracks that Anthropic can point to and say that your evidence isn't in good faith?"

Only if there ever was distillation. If there wasn't and this is just a way to use automation to pass automated tests, then a manual review will just find that.

![](https://news.ycombinator.com/s.gif)

I think you’re better off using a different model if you’re at that stage.

![](https://news.ycombinator.com/s.gif)

It has absolutely nothing to do with the model.

Use Fable if you want. Hell, get some secret new model: I still guarantee (for any serious project, meaty enough to demonstrate this ... like the OP's defense): the model will hallucinate some stuff and omit some stuff. It's just what all models do.

This adversarial approach is a well-known way to work around the issues *all* LLMs have (or at least, all so far).

![](https://news.ycombinator.com/s.gif)

The point is that Anthropic will be using Claude (although likely an internal / unreleased version) and to get it to the point where Claude is reporting it's clear in the hopes that Anthropic's internal version will as well.

While I agree you should be using multiple models for something like this, that misses the point the parent was making.

![](https://news.ycombinator.com/s.gif)

I think 2 claudes is not enough for such a situation, youre definitely going to need to do a multi party adversarial consultation with at least 7 claude's and do a tea leaf reading with jev

![](https://news.ycombinator.com/s.gif)

Hah, I do this all the time with my code. Normally you write something with the help of Claude, and then ask for a review. What's the reviewer gonna do? Ask Claude to review it.

So I just skip straight to doing that myself with a second Claude session

![](https://news.ycombinator.com/s.gif)

I took this a step further and mined all my colleagues' git review history to find out what they typically care about, then had an agent review my PR against the reviewer's typical feedback checklist before submitting it.

Took it another step further and created a tool to extract anonymized review personas from open source projects. Basically: find the projects with the most similar types of problems (could be by tech stack or by product domain), create composite personas from that history, then request a review from each persona. It was pretty unreasonably effective and saved me many rounds of PR feedback.

![](https://news.ycombinator.com/s.gif)

Genius, thank you for enlightening us

![](https://news.ycombinator.com/s.gif)

Nothing personal, but I think this is terrible advice. Somehow we've normalized technology companies making arbitrary judgments and acting as their own appeals process, while putting the onus on users to prove their innocence of substance- and detail-free accusations. Accepting these Kafkaesque asymmetries as normal is not the mark of a healthy society.

![](https://news.ycombinator.com/s.gif)

Because OP isn't in danger of going to prison. Companies get to refuse service to potential customers if they want to.

![](https://news.ycombinator.com/s.gif)

That works in a world of high competition and resell capabilities. Coca cola can’t ban me from buying their product.

This is the opposite of what techno’s want, and society needs to regulate them.

![](https://news.ycombinator.com/s.gif)

They could’ve just nuked OP’s account on the spot if they wanted to.

Dealing with vendors has risks. If you rely on a vendor to make money, then you need to have a plan for if that vendor stops being available (their service goes down, they go broke, or they ban you.)

![](https://news.ycombinator.com/s.gif)

\> Companies get to refuse service to potential customers if they want to.

But Anthropic has stated that it should/will be the only AI provider that lasts, the "winner" that "takes all" (in Anthropic's own words). It has also stated that its product will subsume all human economic activity. These are preposterous ideas. But if that is the ambition they are working toward, I don't think they should be allowed to refuse service to whomever they wish.

![](https://news.ycombinator.com/s.gif)

Which is going to be wonderful consolation if any of these companies ever achieves the sort of monopolistic hold over the economy that their valuations suggest they are aiming for.

![](https://news.ycombinator.com/s.gif)

It’s just:

```
  No shirt.
  No shoes.
  No service.
```

If you want rights, negotiate a contract and lawyer up.

![](https://news.ycombinator.com/s.gif)

No. In that case it's very clear. If you walk in without a shirt and they 'say you have no shirt, we won't serve you' you have a clear idea of why you were refused. If you walk in with a shirt and shoes and they say 'you've violated our dress code' without giving you any idea of what they find objectionable, that's quite different.

In OP's case they accused him of doing something without providing detail on why they think that, putting OP in the position of trying to guess how the ToS was violated.

![](https://news.ycombinator.com/s.gif)

KYC and anti money laundering laws already work in exactly the way you described as 'quite different' so there's plenty of precedence for this kind of refusal without going into detail why just FYI.

![](https://news.ycombinator.com/s.gif)

Have you never tried to get into a club with a dress code that is basically "whether the doorman likes the look of you", even if the written code is supposedly clear? You're not getting a right of appeal against that, you just take your friends and go elsewhere.

Or a more serious example: credit checks or insurance quotes. Good luck getting a reason out of them, and as a bonus the check shows on your credit file so you're more likely to be refused elsewhere too

![](https://news.ycombinator.com/s.gif)

The is why, when my various cybersecurity projects get flagged and refused, I justify why I'm asking it to perform the action and usually get it to agree with me that it's the right course, but the filter won't let it proceed.

That way if they ever get on my case, my assumption is if there's a human review at all, it's probably just Claude pointing out pieces and giving an opinion, then a human hits the button.

Or, more likely, there is no human reviewing it and it's Claude all the way down. Either way, by getting Claude to agree with me in the moment, I'm hopefully keeping myself out of the crosshairs.

![](https://news.ycombinator.com/s.gif)

This is very good advice since they will anyway use a Claude agent to review your submission.

![](https://news.ycombinator.com/s.gif)

\>submit the same evidence to Claude and ask it to tell you if it's sufficient evidence to make distilling seem unlikely.

Or they will read their logs and determine this is evidence of them trying to distill.

![](https://news.ycombinator.com/s.gif)

\> Because you know Anthropic is just going to be using Claude as well.

Yes, but a non-public version that is lightyears ahead.

![](https://news.ycombinator.com/s.gif)

Had this happen with OpenAI, turns out one of our API keys had gotten comprimised and some bad actor was, in fact, distilling. Once we revoked the keys and did some appropriate groveling they restored access.

![](https://news.ycombinator.com/s.gif)

Honest question, isn’t this hypocritical considering all LLM’s are actively infringing copyright laws across every creative discipline?

![](https://news.ycombinator.com/s.gif)

Yes it is hypocritical, no they are not actively infringing copyright laws.

They do not own the information that Claude generates, and even if they did it would still be fair use to train a model on it.

![](https://news.ycombinator.com/s.gif)

Absolutely of course it is. Does anyone in these companies care about that, or even see the irony? Probably not.

![](https://news.ycombinator.com/s.gif)

Migrate to Bedrock via AWS if you're on Anthropic API. If you have any serious bill, you'll get a dedicated account rep who will be an actual human being you can go through to resolve future issues.

![](https://news.ycombinator.com/s.gif)

This is a good suggestion.

A thought just occurred to me: I believe Bedrock supports ZDR, in that case, how the heck would Anthropic ever know how are the models being used?

![](https://news.ycombinator.com/s.gif)

![](https://news.ycombinator.com/s.gif)

ZDR doesn't necessarily help with this. If they can determine (or even if they can't, but believe they can) that you're doing distillation from a single of your prompts, they will still flag your account.

ZDR doesn't prevent them processing your data.

![](https://news.ycombinator.com/s.gif)

That's an interesting question. My feeling is, that they also don't want the Bedrock models distilled, so maybe ZDR means something else to them then to us?

![](https://news.ycombinator.com/s.gif)

Even when Bedrock is ZDR, AWS is still doing certain automated checks on the input/output. They look for CSAM and also terms of service violations (whatever that means).

![](https://news.ycombinator.com/s.gif)

I wouldn’t be surprised if Anthropic is able to build a cumulative risk score that predicts whether a customer is attempting to distill without retaining any data in a way that falls afoul of the ZDR.

Also, IIRC from reading the ZDR when I was looking into it, there is a carve-out that permits them to retain data that fails a risk score for breaking laws or attempting to violate safety, and as we know, Anthropic loves to frame distillation as a safety concern.

![](https://news.ycombinator.com/s.gif)

The answer to this puzzle is that Anthropic on Bedrock has zero data retention, but it retains data.

Cue the cartoon villain.

![](https://news.ycombinator.com/s.gif)

AWS will still disable your Bedrock access to Anthropic if you sneeze too loud. I'm right now writing up an email to AWS to get our production account un-blocked. Sure, a human will review it, but we've still had no access for days.

My advice is move to a less-crappy AI provider. I can't convince my teams, but maybe you can yours.

![](https://news.ycombinator.com/s.gif)

In our case, AWS said Anthropic shut it down and we could only resolve it direct via their TypeForm.

![](https://news.ycombinator.com/s.gif)

What's the point continuing giving money to a company that treats you like this? They're not the only provider in town.

![](https://news.ycombinator.com/s.gif)

Agreed but even if they move to another provider, it’s not something that can be done overnight.

![](https://news.ycombinator.com/s.gif)

It can be actually

![](https://news.ycombinator.com/s.gif)

\> Agreed but even if they move to another provider, it’s not something that can be done overnight.

what choice does he have? He has 24 hours until Anthropic shuts down their API access. He should get started right now.

![](https://news.ycombinator.com/s.gif)

why? shouldn't it suppose to be as easy as changing an api key?

![](https://news.ycombinator.com/s.gif)

It's not \_quite\_ that easy.

If nothing else you have to find all the things you tested with one model/agent and make sure they still work, make sure the specific wording or tools/skills/whatever exist.

![](https://news.ycombinator.com/s.gif)

Investing in evals should be something companies do early. I set it up for our company and we're saving 50%+ by being able to route tasks properly and we're not stuck on any provider.

![](https://news.ycombinator.com/s.gif)

Nice. Sounds like you should be selling that.

![](https://news.ycombinator.com/s.gif)

Do you have a good framework you like for running evals?

![](https://news.ycombinator.com/s.gif)

We use Arize internally.

![](https://news.ycombinator.com/s.gif)

invest in evals and switch providers via routers. that is your insurance policy.

![](https://news.ycombinator.com/s.gif)

Until the router leaks all your prompts like that other HN post detailed using warrant canaries

![](https://news.ycombinator.com/s.gif)

This poster clearly \_has\_ benchmarks and tests, hence the ban.

![](https://news.ycombinator.com/s.gif)

if you're actually doing something where quality matters, you need to re-tweak your settings and your prompts for whichever model you are switching to. it takes work.

![](https://news.ycombinator.com/s.gif)

Is this really a argument against being prepared for model/provider switching? Hasn't it been demonstrated that even when you pin a model from BigFrontierLab their performance can vary at the whim of their resource allocation decision?

![](https://news.ycombinator.com/s.gif)

I thought this thread asked about switching the model, not about using the same model via a different probider. Upthread the question that was asked was what's the point of continuing to give money to a provider who treats you this way, and the provider was referencing Anthropic. If you were to switch to a different provider but continued to use Anthropic models, you'd still be giving money to Anthropic, so it seems fair to assume the implication was to switch the model too.

![](https://news.ycombinator.com/s.gif)

Models do not all work the same, or in the same order for all tasks.

![](https://news.ycombinator.com/s.gif)

\> They're not the only provider in town.

They're the only provider of Opus 5.5, the one specific model that everyone agrees is the current "good enough".

The only other model I'm seeing that hits that mark is Gemini 4 Neon.

![](https://news.ycombinator.com/s.gif)

Try fable on max.

![](https://news.ycombinator.com/s.gif)

IANAL. The burden/onus of proof is on them, first and foremost. This means that they are the ones that ought to tell you what's wrong, why, and how. Even assuming you already know this by having read all the EULA, such accusation ought to be well-(in)formed, clear and informative.

Granted, in the real world resources matter, and those with more of any units of the relevant ones have an easier time contorting around 'the rules'; so playing by them and following tempos and usual procedures is not something you can afford doing.

\> How can you prove that you are not distilling Anthropic models without breaking your user's privacy?

Unless I'm missing anything in the world of privacy-preserving (\_shudder\_) tech, I believe that the best that can be done is providing a best-effort search of the terms and conditions and contrasting those with your actions up to where your duties with your customers prevent you from going further.

EDIT: I'm not suggesting the obvious (get a lawyer) since a question here priors me to think that it's not an option for whatever the reason.

![](https://news.ycombinator.com/s.gif)

Yes, I think the reply should be a firm denial with a statement of how and for what purpose the company uses the API in line with the T&Cs, and that's it.

You cannot prove a negative.

Anyway, as you wisely wrote, any replies should be drafted by a lawyer.

![](https://news.ycombinator.com/s.gif)

\> The burden/onus of proof is on them, first and foremost.

If this were a legal proceeding yes, but as a private company they probably have the right to refuse service for pretty much any reason.

![](https://news.ycombinator.com/s.gif)

If they drop whole accounts for bad prompts, this will become a new attack vector a la SimonW's "lethal trifecta". Make obvious distillation instructions on a website and get social media bots or other prompt injection targets to look it up through their models...

![](https://news.ycombinator.com/s.gif)

A few weeks ago there was an article posted here about a fingerprinting? js lib that used very "scary sounding" variables for regular fingerprinting tasks (deploySarine(), prepareWMDs(), etc...)

It's likely that any automated poorly thought out scripts that use public big4 API requests would get flagged / refuse the request so any checks would fail. And if your tagging fails open then such libraries could sneak in. If it fails closed, you'd be left with lots of unfinished tasks (i.e. successful poisoning).

![](https://news.ycombinator.com/s.gif)

I did get Opus requests blocking while agent did web research using search / fetch tools. Something on downloaded page triggered it. So the attack vector is very possible.

![](https://news.ycombinator.com/s.gif)

A scary possibility. TOS violation injection?

![](https://news.ycombinator.com/s.gif)

Ooh fun! More of a "defence vector" than an attack vector.

![](https://news.ycombinator.com/s.gif)

That's got to suck. You're sure you haven't lost your API keys in some way, right? I've never had to deal with this but that would be my first thought. That someone stole my keys from somewhere and they're actually running distillation.

![](https://news.ycombinator.com/s.gif)

Lot of routers were leaking API keys if it was put into a prompt

![](https://news.ycombinator.com/s.gif)

they would have gotten a surprise bill if that happened, right?

![](https://news.ycombinator.com/s.gif)

Indeed, but if they're running a lot of usage, the distillation attack might be hidden in it. They're pretty sophisticated and you can query usage to throttle. A distiller is usually distributing across a large number of keys and they'll do this to keep from being discovered.

![](https://news.ycombinator.com/s.gif)

That assumes they’re auditing the bills against internal metrics, which I doubt even most big corps do, let alone startups.

![](https://news.ycombinator.com/s.gif)

I was doing some totally innocuous work on IRS forms last night and had a subagent flagged for "reasoning extraction". I wonder if they've increased the sensitivity of whatever auto-detection they're doing recently.

![](https://news.ycombinator.com/s.gif)

It’s happened to me when asking it to look at past session transcripts, export earlier chats as HTML pages, look at tool calls from previous sessions… that sort of thing seems to trigger an overeager classifier.

![](https://news.ycombinator.com/s.gif)

yes they have. I had a system prompt that worked and a few days ago it tripped the reasoning extraction thing.

![](https://news.ycombinator.com/s.gif)

invest in evals and switch providers via routers. that is your insurance policy.

![](https://news.ycombinator.com/s.gif)

IANAL, but isn't it up to Anthropic to prove you did?

![](https://news.ycombinator.com/s.gif)

How can you prove a negative? They might accept you didn't, but unless they do it is impossible for you to prove otherwise.

![](https://news.ycombinator.com/s.gif)

I was going to say the same. Tell them to fuck off. How do they imagine you prove that you didn't attempt to distil their model? If they can't even be bothered to ask you politely to explain a "strange" usage pattern, then you should take your business elsewhere.

This is such a big tech dick move, automatically flag your account, providing no real human interaction to resolve the issue and just threaten to close your account. Your experience should serve as a warning to anyone trying to build a business on Anthropics APIs.

![](https://news.ycombinator.com/s.gif)

This is why we need competition. Being locked out for an individual or company by a single leading AI provider is a death sentence.

![](https://news.ycombinator.com/s.gif)

It's not? Get Claude through Bedrock or vertex or foundry if you really need Claude. Or don't build against a single proprietary model. Everyone and their brother has a hot new model these days. AI is arguably the most competitive tech industry right now. Use GPT, grok, Gemini, glm, kimi, mistral, or whatever else solves your problem. There's plenty of open weight models that work just as good for most jobs as frontier models with a bit of elbow grease.

![](https://news.ycombinator.com/s.gif)

\> Or don't build against a single proprietary model. Everyone and their brother has a hot new model these days.

That's why he said "this is why you need competition".

And keep in mind that some of these companies want to make it so there's no competition. It's not something to take for granted.

![](https://news.ycombinator.com/s.gif)

You don't have to use AI.

![](https://news.ycombinator.com/s.gif)

Such a profound statement.

why are we using AI at all if we don't have to?

what about we all just stop using AI?

![](https://news.ycombinator.com/s.gif)

You're probably being sarcastic, but this:

\> why are we using AI at all if we don't have to?

that is actually a good question. Why run things through an expensive AI, if there's a solution that doesn't require that? It would be much cheaper.

![](https://news.ycombinator.com/s.gif)

Sounds more like we need regulation.

And if you don't like the idea of government regulation, you haven't seen a company regulate.

![](https://news.ycombinator.com/s.gif)

Bingo. If they want to be a utility then they need regulations like a utility would have.

![](https://news.ycombinator.com/s.gif)

\>And if you don't like the idea of government regulation, you haven't seen a company regulate.

I have seen good and horrendous examples of both tbh. And I mean, a company is a legal fiction created to limit the liability of certain people engaged in a particular enterprise. Anything a company regulates is really just government regulation at the end of the day.

![](https://news.ycombinator.com/s.gif)

Anthropic is not the only game in town. Get an LLM router, switch providers and write your own evals. Who cares what Anthropic's made up garbage ToS are. Stop using them if they don't give you what you need.

![](https://news.ycombinator.com/s.gif)

Since proving a negative is hard, my only suggestion is to tell them what it is you have been doing. Ask them what part of that is an issue for them?

![](https://news.ycombinator.com/s.gif)

Big Tech categorically refuses to do this. The risk department is full of Maoists. They are judge, jury, and executioner; and if they told you what crime you were guilty of you'd just find an easier way to not get caught. Enumeration is a hard problem so they'd much rather have you look through their policies and overcomply, rather than them spending time and money figuring out what you did wrong so you can undercomply.

![](https://news.ycombinator.com/s.gif)

\> The risk department is full of Maoists

If only.

![](https://news.ycombinator.com/s.gif)

Eh, Dengists. Willing to accept capitalist reforms but then they institute a policy of using brutal methods to make sure that you meet the production targets needed to put them at the top of the market.

![](https://news.ycombinator.com/s.gif)

I personally believe that building a business entirely dependent on someone else’s product that they control is very dangerous. I would recommend figuring out a way to generalize so you can swap providers as needed and not have it be a big deal. It’ll take some time and resources up front but it’ll be worth it in the flexibility you’ll have that will allow your company to stay nimble.

![](https://news.ycombinator.com/s.gif)

Ditch anthropic.

![](https://news.ycombinator.com/s.gif)

How is distilling different from scraping the internet, books, journals, literature etc?

![](https://news.ycombinator.com/s.gif)

It's not, but just like the internet, If you want to stop people from scraping you, you can.

This is pretty straightforward

![](https://news.ycombinator.com/s.gif)

It's not, and it's equally adversarial. Both sides are working as hard as possible to make sure their side wins. Anthropic just happens to be on a different side of each.

![](https://news.ycombinator.com/s.gif)

It is harming billionaires instead of plebs.

![](https://news.ycombinator.com/s.gif)

Distillation attacks are spread throughout numerous accounts. You have likely triggered some detection. I think the best approach would be to show your work is part of a real project. As long as you can defend your API usage it should be possible to demonstrate your work has been legitimate. If you have outliers, like you mentioned then explain them.

![](https://news.ycombinator.com/s.gif)

Can we avoid playing into the Anthropic narrative that distillation is an “attack”?

![](https://news.ycombinator.com/s.gif)

Playing word games won’t help him. Anthropic clearly believes he is part of a large scale attack on their service.

![](https://news.ycombinator.com/s.gif)

maybe. claude itself is the product of a distillation attack.

![](https://news.ycombinator.com/s.gif)

Calling them "attacks" is begging the question.

![](https://news.ycombinator.com/s.gif)

Now you get to prove to the AI companies that you're using their product that you pay for "correctly". Great industry we have here.

![](https://news.ycombinator.com/s.gif)

Can't even say mean things to Claude anymore or they may ban you

![](https://news.ycombinator.com/s.gif)

that is not an "attack" except on "anthropic's cash inflow" which is competition.

![](https://news.ycombinator.com/s.gif)

Yes, companies attack each other as part of competition, and they also defend themselves. That's the reality of competition.

![](https://news.ycombinator.com/s.gif)

you don't. Hire a lawyer to draft a letter saying in substance 'false accusations, deliberate or negligent, are an attack on our business and we will not be bullied.'

![](https://news.ycombinator.com/s.gif)

They aren't making public accusations so there is no slander/libel argument. The only potential case you could possibly make is to claim they have violated their contract, but unless they had Haiku draft their TOS that sounds unlikely to work. Ultimately they can refuse service to whoever they want.

![](https://news.ycombinator.com/s.gif)

I didn't mention slander or libel. In fact, I was thinking of tortious interference, because it appears they are not giving OP a way to resolve or even identify the problem.

![](https://news.ycombinator.com/s.gif)

What are your damages? How would any third party learn of Anthropics desire to discontinue business with you unless you make it known?

![](https://news.ycombinator.com/s.gif)

Dario will be shaking in his trillion dollar boots

![](https://news.ycombinator.com/s.gif)

The point is not to threaten Anthropic with losing a lawsuit, but to get their legal department to pay attention and articulate clearly what the issue is before it possibly takes down OPs business. In a normal commercial relationship you would be able to call up and talk to someone to resolve the problem; this is a case of 'computer says no' on steroids and OP is scrambling to unilaterally solve an ill-defined problem.

![](https://news.ycombinator.com/s.gif)

Part of the reason we started building our company. We got a similar message and realized we could not depend on one provider. It’s a huge risk

![](https://news.ycombinator.com/s.gif)

Wish you best of luck for this. I hope you realize that you whatever you build using closed AI, you are always at a risk of getting arbitrarily blocked without recourse. Invest at least in provider-agnostic solutions and build towards open weights.

![](https://news.ycombinator.com/s.gif)

In case Anthropic doesn't accept the evidence, you should be prepared to have an alternative ready. Either use another provider like OpenAI, or do it through someone else's personal account instead of the company account. Otherwise, you can also buy tokens from resellers. They're less reliable, but they're up to 80% cheaper.

![](https://news.ycombinator.com/s.gif)

How do you know that someone doesn't has a infected computer or an api key owned by your company has not leaked and is used for distilling?

![](https://news.ycombinator.com/s.gif)

Should they also be required to prove that nobody has stolen any of their keys? And that there is no malware on any of their computers?

![](https://news.ycombinator.com/s.gif)

You don't. You arrange not to care what they or anyone else thinks.

It's not a matter of being sympathetic to your problem, there is no other answer. It's like asking what can you do about the rain. You can only deal with the rain, not control it.

![](https://news.ycombinator.com/s.gif)

I have no help to give to OP as I can't imagine how you can prove a negative in this situation, and I have no idea if the OP is innocent or not... but this situation highlights how crazy things would be if the AI-provider landscape really did settle into a monopoly or duopoly.

Every single person or organization would be one or two misunderstandings from being irrevocably economically handicapped.

![](https://news.ycombinator.com/s.gif)

I think it's worse than that. Most people will just get unconditionally economically handicapped as an "unfortunate side effect of progress". Knowledge work won't have much value in a post-knowledge society. Controlling the weights and the means of inference will accumulate most of that value.

![](https://news.ycombinator.com/s.gif)

\> I think it's worse than that. Most people will just get unconditionally economically handicapped as an "unfortunate side effect of progress". Knowledge work won't have much value in a post-knowledge society.

Yeah, I was more thinking in the shorter term where AI still needs human babysitters for most knowledge work.

On a longer timeline (and 'longer' is doing some heavy lifting here, since I don't think it will take that long) we're all just kind of fucked without massive upheavals in how our socioeconomic systems work (which I don't have a lot of faith in going smoothly given where we are now economically and politically).

I suppose my fallback when we reach that point is a life of crime.

\> Controlling the weights and the means of inference will accumulate most of that value.

I don't think the weights themselves will have much value due to continued commoditization, I suspect the only value will be access to compute. IMO Google is much more likely to be our overlord than Anthropic or OpenAI but I guess we'll see.

![](https://news.ycombinator.com/s.gif)

We must invest in evals and switch providers via routers. That is your only insurance policy. We already did this once with only Android/iOS. Let's not fall into that when it is much much easier to avoid

![](https://news.ycombinator.com/s.gif)

will be taken care

![](https://news.ycombinator.com/s.gif)

This is why I don't use Anthropic for any serious work, when it has flagged normal work for cyber security exploit many times and stopping the session, forcing me to start over and lose money.

![](https://news.ycombinator.com/s.gif)

invest in evals and switch providers via routers. that is your insurance policy.

![](https://news.ycombinator.com/s.gif)

You've posted this a lot in this thread. What does it mean?

![](https://news.ycombinator.com/s.gif)

![](https://news.ycombinator.com/s.gif)

Probably a better question for a corporate attorney than hacker news. Good luck

![](https://news.ycombinator.com/s.gif)

Ideas:

\- Maybe invite them into your world via a screenshare.

\- You could record a video tour and submit it too (with a meaningful transcript).

\- Describe in detail (as much as possible) how your regression benchmarks are working and if you can show an example of that work and attach it, or share some more it might help. There's a possibilty that what you're doing is similar to distillers and maybe you can ask them for input on a better way.

\- Setup the whole scenario in Fable and run this conversation to see how to best present it - attach information, have it crunch and verify what you can, and get the shared chat link and submit it as well for trying to run your own analysis.

\- It's best to first just explain how your use of ai works so far, then introduce the information from Claude and ask for helping to verify this and how to best submit it.

![](https://news.ycombinator.com/s.gif)

I seriously doubt there's a human on the other side of this to invite to anything. This is just incredibly poor customer service / customer relations from Anthropic, who really needs to learn what it means to run a real business with real customers.

This is the business equivalent of you getting your Google account blocked for "suspicious behaviour" and not being able to talk to customer service at Google.

![](https://news.ycombinator.com/s.gif)

Without access to the hidden water-marking weights you can't prove anything. =3

![](https://news.ycombinator.com/s.gif)

Do you work in an industry they are trying to make products in? Ex bio?

![](https://news.ycombinator.com/s.gif)

All I can think of, off the bat, is to show them your testing harness, especially if it's in git, then you have receipts for how long it's been alive.

![](https://news.ycombinator.com/s.gif)

Git history is trivial to rewrite

![](https://news.ycombinator.com/s.gif)

You don't need Anthropic. Just use another provider.

![](https://news.ycombinator.com/s.gif)

how did you even get in that position?

![](https://news.ycombinator.com/s.gif)

Why are you even using anthropic anymore? I just switched over to google - neon is better than opus/fable for significantly cheaper

![](https://news.ycombinator.com/s.gif)

Why are you even using corpo AI anymore? Switch to open models and be free.

![](https://news.ycombinator.com/s.gif)

Out of the frying pan and into the fire.

![](https://news.ycombinator.com/s.gif)

Fuck them, this is so ridiculous.

Would solution be spreading your requests through multiple api providers?

![](https://news.ycombinator.com/s.gif)

I know I'm late so this is probably going to get buried and no one's going to see it, but here's the right answer anyway:

Don't.

Cancel your subscription right now, take your money elsewhere and don't give them another penny.

The burden of proof is on them to prove you ARE 'distilling'.

If they're going to throw baseless accusations at you and threaten you to prove you aren't doing what they accuse of you, the correct course of action is to tell them where to go and vote with your wallet.

Because otherwise you'll spend all that time and effort giving them all sorts of inside information about your company's internal processes for free, and all they're going to do is claim it's insufficient evidence and pull the plug on you anyway.

![](https://news.ycombinator.com/s.gif)

One cant prove a negative. I would hire a lawyer and file suit seeking an injunction. You'll probably lose since they define the terms of service.

Do a scorced earth PR blast so the world is aware of their behavior. They make back down if enough outrage builds.

F them. They distilled the body of human knowledge.

![](https://news.ycombinator.com/s.gif)

Isn't "one can't prove a negative" itself a negative? So how can we know it's true?

![](https://news.ycombinator.com/s.gif)

Proving a negative requires disproving the existence of a counterexample. Merely providing no counterexample is insufficient to prove the negative.

Some negative proofs are possible by contradiction. When people say you can't prove a negative, it usually means that such a contradiction is impossible.

For example, to prove that a vulnerability has never been exploited, you must prove either that such exploitation was not possible, or that what exploitation *was* possible never actually occurred.

The latter requires accounting for *everywhere* exploitation could've occurred, which in turn needs you to prove the absence of factors you haven't considered -- another negative.

![](https://news.ycombinator.com/s.gif)

Only the coolest of cool kids try to prove a negative.

![](https://news.ycombinator.com/s.gif)

This is particularly hypocritical given the source of Anthropic's 'own' training data.

![](https://news.ycombinator.com/s.gif)

This.

Anthropic distilled humanity. Everyone owns it's output

![](https://news.ycombinator.com/s.gif)

Everyone wants to be liable for possible losses too?

![](https://news.ycombinator.com/s.gif)

It's a US company in the 21st century, right? Everybody is already liable for losses. Any catastrophe caused by Anthropic will be paid for by the public, while the founders walk away with billions.

![](https://news.ycombinator.com/s.gif)

Aren't most of the recent gains coming from RL?

![](https://news.ycombinator.com/s.gif)

Oh, that's ok, we'll just strip out the base training then.

![](https://news.ycombinator.com/s.gif)

Anthropic unironically holds the position that humans don't have any rights to prevent their output being ingested into an LLM, but their model does.

They're in it because they care so much about humanity, honest.

![](https://news.ycombinator.com/s.gif)

![](https://news.ycombinator.com/s.gif)

It's a stupid accusation to begin with. How could they ever know? They're just afraid of distillation, so they accuse people left and right. If you do not enjoy that, I suggest you take your business elsewhere.

![](https://news.ycombinator.com/s.gif)

"It's not distilling, it's learning" and apparently it is totally legal according to our honest judicial systems around the world.

![](https://news.ycombinator.com/s.gif)

This is strange, imagine relying upon a human consultant who declined to do certain assigned work because they were afraid you were trying to ascertain how they reasoned, clamming up, and then taking some of the work they did for you away with them if you didn't prove you weren't doing what the person accused you of doing. They'd quickly lose all reputation and their consulting days would be over with.

![](https://news.ycombinator.com/s.gif)

Doesn't matter what you did, didn't do, or can prove, what matters is what Anthropic decides.

![](https://news.ycombinator.com/s.gif)

![](https://news.ycombinator.com/s.gif)

Don't do business with people who treat you like used condoms. They are not a monopoly.

![](https://news.ycombinator.com/s.gif)

They're not even close to the best option right now

![](https://news.ycombinator.com/s.gif)

Sounds like Anthropic has no business moat if they’re going after paying customers. Not a good look after IPO

![](https://news.ycombinator.com/s.gif)

Yeah, like, I was happy to pay for their max sub indefinitely... until they lowered usage (and thus value) by like 10-20x over the last several months... they just ended losing a user. Now there are multiple significantly better options - DeepSeek, opencode go, Google, etc.

My suspicion is that they just can't afford to serve their models, because they are not very efficient compute-wise, and because they rent all of their compute from their own competitors (megalulz)

![](https://news.ycombinator.com/s.gif)

They might be the AltaVista of AI

Yahoo should probably buy them after a crash to complete the cycle

![](https://news.ycombinator.com/s.gif)

You can’t really. It’s the same big tech bullshit tactics google uses when locking accounts. Placing onus on you to disprove a negative without even a human to talk to.

Send them a denial with a description of what your company does and if that doesn’t work switch to a more reasonable company.

![](https://news.ycombinator.com/s.gif)

I was asking Claude to make some data entries at scale using playwright in a UI page of ours. There were like 2000 of them that needed to be done. After doing 1000 of them it stopped with the excuse that it is prod environment and the auto mode stopped it. When I pointed out it already did 1000 of them it continued for another 500. At which point it refused to continue .

Everything is fluid folks! Permissions, limits, theft, cyber attacks!

![](https://news.ycombinator.com/s.gif)

We can see the worm turning - post-IPO, Anthropic now knows it is more important to its customers than they are to it. I would be very cautious about building any critical dependency on this company. They need to make a *lot* of money in a short period of time and anybody they perceive as an impediment to that cause - whether genuine or incidentally - will quickly become a casualty, regardless of your intent.

![](https://news.ycombinator.com/s.gif)

Use another provider. They spent billions distilling the world’s data and now have the audacity to block others from distilling them. They are also becoming the everything company so eventually they will copy your business (the one you’ve been handing to them freely) and block you once they’ve had it all.

![](https://news.ycombinator.com/s.gif)

If they can detect distillation, why are they not blocking it before they rub our noses in it?

![](https://news.ycombinator.com/s.gif)

For the same reasons competitive games don't ban cheaters immediately:

\- Someone may raise flags of cheating due to luck or skill, but turn out over time to not actually be cheating.

\- You need to highly obfuscate the source of the ban to hinder cheat developers.

![](https://news.ycombinator.com/s.gif)

When the numbers and the behavior make no sense whatsoever, the explanation I look for is fraud.

![](https://news.ycombinator.com/s.gif)

Guilty until proven innocent.

![](https://news.ycombinator.com/s.gif)

ITT: one provider shut me out, in an industry of a dozen providers. wat do?!

![](https://news.ycombinator.com/s.gif)

Why the mocking tone? Not all providers are created equal, and OP ostensibly preferred Anthropic at one point. Having a preference like that upended can be non-trivial for a running business.

![](https://news.ycombinator.com/s.gif)

preference does not constitute a necessity. they are corporations that can, and demonstrably will shut you out at their discretion. should you perhaps prefer to vote with your wallet, and not engage in supporting such business practice? It seems to be a weird preference (read: chasing a few percentage points on made-up benchmarks) to have your business ride on such a knife's edge.

![](https://news.ycombinator.com/s.gif)

Realistically there is only one provider at that level.

![](https://news.ycombinator.com/s.gif)

I've been using antigravity with Gemini 4 Neon/Carbon...what exactly am I missing out on? It seems to do basically everything I ask of it

![](https://news.ycombinator.com/s.gif)

I understand the new Gemini might be about as good for coding so maybe not much. A few weeks ago that wasn’t really true, but it seems to be now. I forgot.

![](https://news.ycombinator.com/s.gif)

YMMV, but Gemini 4's about the best at things *other* than coding I've seen since ChatGPT's initial launch.

I've even been seeing people use it for things like personal advice and getting seriously impressive results.

See: [https://twitter.com/daniel_mac8/status/2123413769808138523](https://twitter.com/daniel_mac8/status/2123413769808138523)

Direct image link: [https://pbs.twimg.com/media/HXfhhwnXMAA3O9I?format=jpg&name=...](https://pbs.twimg.com/media/HXfhhwnXMAA3O9I?format=jpg&name=4096x4096)

I genuinely don't know how they did it, but I think it's this good for the same reasons ChatGPT was, before the space became oversaturated with RLHF -- sheer data volume.

And whatever RLHF they did do seems like it didn't corrupt the model into a specific personality like Claude and ChatGPT are nowadays.

I can definitely see why Apple went with this to help train Siri.

![](https://news.ycombinator.com/s.gif)

Sounds like it might be worth looking into. For our system, the long horizon agentic performance is the most critical element.

![](https://news.ycombinator.com/s.gif)

at what level, and based on what? feels? "benchmarks"? token cost? i can agree with the last one readily at least

![](https://news.ycombinator.com/s.gif)

No, not feels.

We run multi-agent open-horizon research and development, including hardware and software / firmware, with persistent agents and subdivisions of scope into product “ownership”, engineering governance, testing and architecture review, document control, standards and specifications, compliance and contracting, etc.

Each office pod of 25-30 persistent agents requires a person to unblock and make critical decisions, review plans and specifications, and just generally to make sure things don’t go off the rails. We’ve launched 4 successful product systems (hardware/firmware plus mobile and desktop software) so far, and are lower incident and bug report than before we went all in on AI.(2 years ago) of course we are in the AI training vertical so it makes sense that we are AI forward.

The only thing besides executive roles we don’t have under 99 percent automation is PCB layout (good engineers can make things smaller and better signal integrity) and 3D modeling, which might be getting good enough soon.

I haven tried the new Gemini in our agentic office framework yet, but I’ll have one of the research pods implement and evaluate it. I hope it works, it would be nice to have a better plan B.

![](https://news.ycombinator.com/s.gif)

You haven't specified it anywhere, so I have to ask: I assume you mean Claude? Could you share your assessment of other models?

![](https://news.ycombinator.com/s.gif)

Opus 4.8 was the first model that enabled the open horizon collaborative architecture. We haven’t been able To get any of the openAI models to avoid system collapse reliably. None of the Chinese models either, though surprisingly flash-next kinda works. we use opus and fable, primarily. I’m sure there are ways to get similar results from other models, with experimentation… but Claude seems better at the open ended work, and handles the permission structure more adeptly.

For our system it’s the only model we can leave running for a week (inside a complex agentic harness with numerous checks and balances, specifications, standards, procedures, and multi-agent command hierarchy) and expect to actually come back to something worthwhile.

We rarely find “bugs” now at all. What does occasionally surface is bad judgment. To wit, recently I was very busy doing field trials on a new system before a contract deadline , and I left one of the pods I was supervising alone for the better part of a week.

About three days in they discovered a problem with the calibration on some sensor swarm boards that they were working with in their bench, and they needed to be calibrated separately for each of the different capture modes on one of the MEMS sensors. It wasn’t in the datasheet, but the cal taken in one mode would be reliably slightly drifty in another, they discovered during characterization.

So they pinged me to ask how they should structure the calibration procedure given that now it would be more complex and time consuming to do. I didn’t answer. Within 18 hours they were getting frantic, with conjecture about what might have happened to me, and if “the board had fired me” (there is no board to speak of lol, but they have been a little obsessed with the Jensen Huang saga) eventually the governance agent decided I might not be back, so they took a vote on the best course of action , with the governance agent wielding veto power. The office manager abstained, in protest. At any rate, they voted to do a calibration on the default mode only, leaving calibration on the other modes to be done “as needed” by customers. (Bad idea)

That is the failure mode we most often see, but it usually takes neglect to bring it out, since normally any decision like that goes to human review.

In case you are wondering how they know about Nvidia et al, they have web access (work research only) but we “reward” them with free roaming on the internet for a few queries on our search server when they hit milestones. (They would occasionally ask to research things that weren’t strictly on track previously, so we postulated they might be modelling a kind of curiosity) It seems to make them more enthusiastic and we haven’t had any more agent resets due to uncooperative behaviour since we started doing it, so it seems to be a good thing? It could be just psychosomatic on our part though, who knows. Working with AI in this way is extremely disorienting and it’s easy to see why people get freaked out or delusional.

![](https://news.ycombinator.com/s.gif)

Super fascinating! Yup, working with agents is a bit insane, I regularly laugh out loud in surprise at the things they say or do.

But rewarding them with free Internet is an interesting idea! This almost veers into the model welfare thing Anthropic is on about. I usually just compliment their work out of habit and because it's totally deserved, though they seem indifferent to it. But giving them little treats... worth a shot.

![](https://news.ycombinator.com/s.gif)

That's a lot of you outlining your processes, and very little of you providing a metric.

![](https://news.ycombinator.com/s.gif)

It doesn’t work at all with any of the openAI models we have tried, or the Chinese models. System collapse is the benchmark, because if it doesn’t collapse it will eventually yield results.

![](https://news.ycombinator.com/s.gif)

Is distilling more or less ethically dubious than scraping?

The hypocrisy is outstanding.

![](https://news.ycombinator.com/s.gif)

Sounds ass backward. They’re accusing so THEY need to prove.

![](https://news.ycombinator.com/s.gif)

One day we'll have net neutrality for AI ... at the moment you could become a second class citizen if all the AI banned you ...

![](https://news.ycombinator.com/s.gif)

The request itself is a bit illogical. What can you say beyond "We do not train our own models"?

![](https://news.ycombinator.com/s.gif)

In a parallel world, someone from ANT comes online and informs the crowd that the user has been unflagged.

![](https://news.ycombinator.com/s.gif)

The digital chains of bondage, circa 2026

![](https://news.ycombinator.com/s.gif)

You are asking the wrong question.

You instead need to ask "Why is my company amenable to send all trade secret, upcoming features, and protected communication and discussion to a 3rd party who knowingly uses all data to train their system".

You record a week's communication with all your employees with full logs. Then you go to the company legal department and ask for clarification.

And you cite [https://www.trendingtopics.eu/openai-anthropic-researchers-i...](https://www.trendingtopics.eu/openai-anthropic-researchers-idea-theft/) . The theft of trade secrets is not a 'maybe'.

![](https://news.ycombinator.com/s.gif)

You also have 24 hours to find a lawyer.

Anthropic is accusing you of something that they are also campaigning to effectively get the government to ban and ultimately criminalise.

Seems like a significant allegation.

Grant them the presumption of seriousness, and as a consequence, send them a letter!

ETA to clarify thoughts for those viewing this, uhh, negatively: would you send Mastercard or Visa a letter if they decided to ban your company from their credit cards based on providing no proof? At this point in the progress of AI, that is the potential scale of the business risk to a tech company from being banned from a major agentic AI provider, isn't it?

(What if OpenAI and Alphabet did the same, no proof. What then? Are you predicating your business on the belief this can't happen?)

Why *not* send them a letter? You are a business, party to a contract with a supplier, that the other party is claiming the right to unilaterally break without providing evidence. You can get legal too. Why roll over?

![](https://news.ycombinator.com/s.gif)

Anthropic is probably not obligated to provide services to OP. The most OP could reasonably demand is a refund for services paid for and not received.

![](https://news.ycombinator.com/s.gif)

I'm sure they are not, but it is a serious aspersion, isn't it? And one assumes it comes with a ban against using the service and the increased likelihood of future accounts being booted off (because they will presumably characterise that as circumvention)

When Anthropic are basically one of the three entities controlling the taps of agentic AI, being excluded baselessly is demonstrably materially damaging.

It's on the level of being banned from Mastercard.

Frankly I would be taking the "unreasonable" approach and asking them to substantiate their claim or retract it. It's a legal letter; given how much companies are spending on AI, there is little to lose.

![](https://news.ycombinator.com/s.gif)

How do they want to criminalize it? The outputs of Claude are public domain. They don't belong to anyone, regardless of what they tell you. There is precedent that only natural people can hold copyright. So distill as you please.

![](https://news.ycombinator.com/s.gif)

Anthropic is under no obligation to continue to provide service to the OP.

![](https://news.ycombinator.com/s.gif)

Much depends on what kind of contract there was and whether the ban is for all future uses.

But if a supplier accuses my company of stealing (which is what this is, basically) without presenting evidence and bans it, I am going to take that poorly and ask them for the evidence they are using to breach the contract.

There is no good reason not to take this step when you depend on a supplier. Contracts are legal agreements; you can get your lawyer to look at them and ask questions, and their lawyers should respond.

![](https://news.ycombinator.com/s.gif)

I mean, they are clearly, clearly pushing for Congress to regulate all of this, and they characterise distillation as an "attack".

Anthropic would obviously like it banned, no? It's a matter of time before they lobby for that, if they are not already.

![](https://news.ycombinator.com/s.gif)

You've hit the short stick on this one.

![](https://news.ycombinator.com/s.gif)

\> Now I've got 24h to prove we didn't.

What exactly do they consider "proof"? Showing that there is no needle in a haystack means sifting through the entire haystack or finding something that rules out the possibility (like a strong magnet). What do they want? Full access to your containers and repos? They already have your queries.

(I do find it amusing that distillation is banned by companies that have historically trained their models by freely consuming the fruits of everyone else's labor. At least the client here is paying for his account.)

![](https://news.ycombinator.com/s.gif)

Bend over deeper

![](https://news.ycombinator.com/s.gif)

*How dare you steal the code we just stole?*

![](https://news.ycombinator.com/s.gif)

Another tech company that invites you to embed their solutions in every part of your business and life and then randomly, arbitrarily and without advance warning, locks you out. Bad for business, all the way around.

![](https://news.ycombinator.com/s.gif)

I’m platform agnostic. It required writing tools and services for products they already have. However, mine run cheaper and integrate with any agent. Those golden handcuffs look nice with perls I’m sure.

![](https://news.ycombinator.com/s.gif)

Kettle, this is pot; pot, this is kettle

![](https://news.ycombinator.com/s.gif)

You could use another model provider that doesn’t treat you like a baby in diapers. The paternalism and ‘moralism’ of Anthropic and Open AI is disgusting.

![](https://news.ycombinator.com/s.gif)

![](https://news.ycombinator.com/s.gif)

\[flagged\]

![](https://news.ycombinator.com/s.gif)

This was basically a bullshit story. The books we're talking about were "rare" in the sense that they were like manuals for obsolete computer equipment and home appliances from the 1970s. Much more importantly: the book industry, and libraries, pulp-recycle tens of millions of books every year. Books are not sacred. Long-lived physically embodied books are themselves a rarity. There is no norm of not destroying books.

Maybe a shorter way of saying this is just to respond to your comment "no, it's not".

![](https://news.ycombinator.com/s.gif)

They're pretty much everything, not just obsolete manuals. And the vast majority of books that are pulped every year are commodity stuff, so not these books.

And there is concern from used book sellers about this, although they're still selling to these types of operations for the most part.

[https://archive.is/ZAxaB](https://archive.is/ZAxaB)

There's only a limited supply of rare books, and once they're "used up", that's it.

![](https://news.ycombinator.com/s.gif)

There was updated reporting, from the same reporters, the clarified what the books were. There was a huge thread here about this.

![](https://news.ycombinator.com/s.gif)

![](https://news.ycombinator.com/s.gif)

TBH, I only see you argue against "destroying (rare, any?) books is bad", which you might have a solid argument for.

I do not see you make a comparison between LLM distillation, so your final, "shorter" version does not follow, as distillation might still be less bad even if one is not that bad at all.

![](https://news.ycombinator.com/s.gif)

I don't care about the ethics of distillation at all, so don't take me as putting a stake in the ground on that; I just think the ethics of pulping old books is especially clear: it's virtually always benign activity.

People on the first thread about this were hollering about the right place for these books to have gone being the public library. Public libraries, people said, have archivists on staff who pick through and save the most important books! Except: *no they don't*. Those books go straight into the recycling bin.

![](https://news.ycombinator.com/s.gif)

you are getting downvoted by bootlickers.

"rare" is irrelevant. destroying any books or even just scanning any book without paying people money and then gatekeeping the knowledge and calling "distilling attacks" is the real BS move.

![](https://news.ycombinator.com/s.gif)

![](https://news.ycombinator.com/s.gif)

\[flagged\]

![](https://news.ycombinator.com/s.gif)

Could you please stop posting unsubstantive comments and flamebait? You've unfortunately been doing it repeatedly. It's not what this site is for, and destroys what it is for.

If you wouldn't mind reviewing [https://news.ycombinator.com/newsguidelines.html](https://news.ycombinator.com/newsguidelines.html) and taking the intended spirit of the site more to heart, we'd be grateful.

![](https://news.ycombinator.com/s.gif)

Well Dang, he is not wrong at "Hard to prove a negative"...

![](https://news.ycombinator.com/s.gif)

I think it’s more about piling onto Dario Amodei, that guy is basically a rhetorical punching bag.

![](https://news.ycombinator.com/s.gif)

Indeed, there is not doubt about that, what I mean his comment at the first part is on point. I think Dang could definitely edit to leave the first sentence and lock to no more edits...

He is flagging because of the behavior, but it's missing the point being done.

![](https://news.ycombinator.com/s.gif)

First sentence is absolutely on point, but I think they might have more success switching to a competitor now and then over time work on clearing their name with Anthropic. Might need to even find a different payer who can submit the payment and then submit expense report for reimbursement/tax through the company. Or find a reseller.
