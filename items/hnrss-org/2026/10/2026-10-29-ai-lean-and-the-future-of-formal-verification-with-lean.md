---
title: AI, Lean, and the Future of Formal Verification, with Lean Creator Leo de Moura
link: https://thedataexchange.media/leo-de-moura-lean-aws/
source: hnrss-org
published: 2026-10-29T15:29:48Z
updated: 2026-10-29T15:29:48Z
first_seen: 2026-10-31T15:15:45.849013716Z
authors:
- datascientist
labels:
- uncategorized
content: extracted
html: 2026-10-29-ai-lean-and-the-future-of-formal-verification-with-lean.html
remote_preview:
  url: https://i0.wp.com/thedataexchange.media/wp-content/uploads/2026/10/Leo-de-Moura-art.001.jpeg?fit=1920%2C1080&ssl=1
---

### Leo de Moura on Formal Proofs, AI Agents, and the End of “Trust Me” Code.

 **Subscribe:** [**Apple**](https://podcasts.apple.com/us/podcast/the-data-exchange-with-ben-lorica/id1487704458) **•** [**Spotify**](https://open.spotify.com/show/65oFWYoomwT8mLcdGkUGd0) **•**[**Overcast**](https://overcast.fm/itunes1487704458) **•** [**Pocket Casts**](https://pca.st/kv9n4vc0) **•** [**YouTube**](https://www.youtube.com/playlist?list=PLnTmH22EvTFTtWJRPTNzosDIDblnSg0PD) **•**  [**AntennaPod**](https://antennapod.org/deeplink/subscribe/?url=feeds.buzzsprout.com/682433.rss) **•** [**Podcast Addict**](https://podcastaddict.com/podcast/2476584) **•** [**Amazon**](https://music.amazon.com/podcasts/18e76845-60a8-4263-9c41-a7bdfe9d14a9/the-data-exchange-with-ben-lorica) **•**  [**RSS**](https://feeds.buzzsprout.com/682433.rss)**.**

Ben Lorica speaks with [**Leo de Moura**](https://leodemoura.github.io/about/), creator of [**Lean**](https://lean-lang.org/), VP and Distinguished Scientist at **AWS**, and Chief Architect of the [Lean Focused Research Organization](https://lean-lang.org/fro/), about how AI is making formal verification more accessible and potentially transforming software engineering. They discuss the growing importance of specifications and machine-checked proofs, what it means to trust AI-generated code and autonomous agents, and the vision of a verified software stack. The conversation also explores how AI and formal verification are reshaping mathematics, what this means for the skills and careers of engineers and researchers, and where similar breakthroughs might emerge next.

***Subscribe to the [Gradient Flow Newsletter](https://gradientflow.substack.com/?utm_source=gradientflow&utm_medium=DEpodcast)***

**Interview highlights – key sections from the video version:**

- - 1.  [Introduction to Leo de Moura and Lean](https://youtu.be/r913hk9CYw0?t=1)
    2.  [Why AI-Generated Code Needs Formal Verification](https://youtu.be/r913hk9CYw0?t=46)
    3.  [Verifying Rust, Autoformalization, and Scaling Mathlib](https://youtu.be/r913hk9CYw0?t=335)
    4.  [Can AI-Generated Formal Models Be Trusted?](https://youtu.be/r913hk9CYw0?t=562)
    5.  [Navier-Stokes, Proof Mismatches, and Editable Proofs](https://youtu.be/r913hk9CYw0?t=685)
    6.  [Formal Verification vs. Traditional Software Testing](https://youtu.be/r913hk9CYw0?t=910)
    7.  [How AI Is Making Formal Verification Practical](https://youtu.be/r913hk9CYw0?t=1064)
    8.  [Requirement Gaps, Model Gaps, and Trusting Proof Checkers](https://youtu.be/r913hk9CYw0?t=1162)
    9.  [The Future of Software Engineering: From Code to Specifications](https://youtu.be/r913hk9CYw0?t=1356)
    10. [The Verified Software Stack and Evaluating Verification Claims](https://youtu.be/r913hk9CYw0?t=1710)
    11. [Verifying AI Agents, Sandboxes, and Security Policies](https://youtu.be/r913hk9CYw0?t=1913)
    12. [Mathlib, AI-Assisted Mathematics, and Collaborative Proofs](https://youtu.be/r913hk9CYw0?t=2257)
    13. [Can AI and Formal Verification Transform Theoretical Physics?](https://youtu.be/r913hk9CYw0?t=2484)
    14. [The Future of Mathematicians, AI Compute, and Scientific Discovery](https://youtu.be/r913hk9CYw0?t=2615)
    15. [Beyond Mathematics: Finance, Chemistry, Accounting, and Law](https://youtu.be/r913hk9CYw0?t=2956)

[Jump to transcript](https://thedataexchange.media/leo-de-moura-lean-aws/#transcript)

* * *

[www.youtube.com](https://www.youtube.com/watch?v=r913hk9CYw0)

* * *

**Related content**:

- A [video version of this conversation](https://youtu.be/r913hk9CYw0) is available [on our YouTube channel](https://www.youtube.com/c/GradientFlow?sub_confirmation=1).
- [How AI Is Changing Mathematical Research](https://gradientflow.com/ai-in-math-2026-08/)
- [Passing Your Evals Doesn’t Mean You’re Safe](https://gradientflow.substack.com/p/your-ai-passed-its-evals-thats-the)
- [Eight Security Assumptions AI Agents Quietly Break](https://gradientflow.substack.com/p/the-part-of-the-ai-security-story)
- [Nine Practical Rules for Agents Doing Real Work](https://gradientflow.substack.com/p/i-keep-hearing-the-same-advice-about)
- **Zhou Yu** →[Why Your AI Agent Isn’t Ready to Ship (And How to Know When It Is)](https://thedataexchange.media/zhou-yu-arklex/)
- **Helen Gu** →[The Hidden Failure Modes of AI Agents](https://thedataexchange.media/helen-gu-insightfinder/)
- **Evan Marshall** →[As Code Generation Speeds Up, Who Tests the Output?](https://thedataexchange.media/evan-marshal-ito-ai/)

* * *

**Support our work by subscribing to our newsletter📩**

* * *

### Transcript

Below is a **polished and edited transcript**.

**Ben Lorica.** All right. So today we have a treat. We have Leo de Moura, VP and Distinguished Scientist in the Automated Reasoning Group at AWS. More importantly, he is the creator of Lean, which can be found at lean-lang.org. Lean is both a programming language and a theorem prover, and he is also the Chief Architect of the Lean Focused Research Organization. So, with that long-winded introduction, Leo, welcome to the podcast.

**Leo de Moura.** Thank you, thank you. Very excited to be here.

**Ben Lorica.** All right. So, I guess the overarching thesis of this conversation is that we’re in an era where generation has become easy. Not necessarily cheap, as people who use Claude Code will attest, but easy. And so, one of the key challenges is verification. Lean is normally associated as the go-to tool. In fact, I think it’s going to be a tool that will increasingly be on the lips of many people in AI in the next year, around the topic of formal verification. So I guess let’s start here, Leo. Just the basics: for a listener who’s accustomed to code reviews, QA, and testing, what does formal verification mean?

**Leo de Moura.** Yeah, Lean is both a programming language and a proof assistant, right? You can write code, but also state properties the code should have. You can state open conjectures in math, and you can provide proofs of these claims, right? But the proof is not informal. The proof is written in the Lean language, and more importantly, it’s machine-checked.

Lean is a big piece of code, but to trust a result proved with Lean, you need to trust only the kernel, right? That’s where the proof is being checked. Like any other programming language, you have all the tooling: LSP server, IntelliSense, code generators, autocompletion. All these features are available in Lean, but they’re not important for the proof-checking step.

And we have more than one checker. In Lean, you can view proofs as data that can be manipulated, that can be transformed. You can export your proofs and check them with different checkers. Some of these checkers are verified now. We have two verified checkers. A third is coming soon.

But this ability to have machine-checkable proofs helps address a trust bottleneck, not just a trust bottleneck between humans, but also between humans and AI.

For example, suppose you want to synthesize a piece of code. You want to implement a piece of code that has specific properties. In the past, you would implement the software. Maybe you would add some testing infrastructure to make sure these properties hold. Property-based testing is very popular these days, right? And you would write the testing framework.

The testing framework is very useful for finding bugs, right? But it’s not useful for proving that there are no bugs.

**Ben Lorica.** You may even have a separate team conduct the testing.

**Leo de Moura.** Yes, yes. Now, with formal verification, you can start with the properties. You can, for example, write the properties down and ask the AI to implement a piece of code that satisfies these properties, and prove it to me. You’re going to implement the code and write a proof, a Lean proof, that will be machine-checked. And AI now is fully capable of doing that.

For example, a colleague and friend of mine, Kim Morrison, did an experiment where she ported zlib to Lean. This is a compression library. And now she has a property that says that if you compress data and then decompress it, you get the data back. There is no loss. The data was not corrupted. It’s an important property.

**Ben Lorica.** No matter the level of compression.

**Leo de Moura.** Exactly, exactly, exactly. Great, great. The theorem says exactly that. It’s one of the parameters of the theorem.

**Ben Lorica.** And so, Leo, for our listeners, she did this. She wrote this compression tool using Lean.

**Leo de Moura.** Yes.

**Ben Lorica.** And then Lean served as kind of the formal verification tool. So if I’m a developer and I do the same thing, and I say I’m going to use Lean to formally verify my software, but I’m going to write it in Rust first, can Lean still help me?

**Leo de Moura.** Yes, yes, it can, in two different ways. The first way is using Aeneas. Aeneas is a tool that allows you to verify Rust code using Lean.

Aeneas is used, for example, by the SymCrypt team at Microsoft. SymCrypt is the cryptographic library at Microsoft. They use Aeneas plus Lean to verify the crypto library.

So you can start by using Aeneas. Another possibility is to ask your AI to build a model of your code in Lean. The AI will read your code, no matter—

**Ben Lorica.** No matter what language.

**Leo de Moura.** Yes, no matter what language. I mean, if someone had told me this last year, I would have said this is science fiction.

**Ben Lorica.** And this has a name, at least in mathematics. They call it autoformalization.

**Leo de Moura.** Yes.

**Ben Lorica.** Right. So, in mathematics, for our listeners, you might have a math paper, and it’s a paper, so it’s composed of words, mathematical symbols, theorems, lemmas, and so on and so forth. And then autoformalization, which, as Leo points out, was not a thing—

**Leo de Moura.** Yes.

**Ben Lorica.** As recently as maybe a year ago, it’s now a thing.

**Leo de Moura.** Exactly, exactly.

**Ben Lorica.** And so, Leo, this step, you folks at Lean had nothing to do with. This just happened because the frontier models became capable of doing this.

**Leo de Moura.** Exactly, exactly. From the Lean side, what changes for us? Scalability, right? I mean, now AI—for example, the mathematical library built by humans for Lean is called Mathlib. It has 2.5 million lines.

**Ben Lorica.** Incredible! For our listeners, imagine—and this is not the case—but imagine almost all mathematical books and papers were translated, in the way we described using autoformalization, into Lean, which means then you have access to all of them.

**Leo de Moura.** Yes. But look at these speeds now. There are Lean proofs now that have been published that are 50 million lines. Mathlib took 10 years, written by humans, to go from zero to 2.5 million. And now there are people writing proofs—that’s AI writing the proofs—that are 50 million lines. The Fermat’s Last Theorem proof that was written by Anthropic AI is 13 million lines.

**Ben Lorica.** But in the case, as you point out, Leo, of software, there’s something equivalent going on for every programming language.

**Leo de Moura.** Yes.

**Ben Lorica.** The frontier models are now capable of translating whatever program you wrote, in whatever programming language, into Lean.

**Leo de Moura.** Yes.

**Ben Lorica.** And then now you have the Lean infrastructure for verification.

**Leo de Moura.** Exactly. But one important thing to keep in mind when you create a model of your program in Lean, if you use AI, there’s always this question: is this model faithful to your code?

Some people now are using differential testing, right? They run the Lean model and the program and see if they compute the same thing, right? To find discrepancies. And people have been finding bugs. I mean, even—

**Ben Lorica.** So what you’re saying is you’re still responsible for this.

**Leo de Moura.** Yes, exactly. And I see it happening in an incremental way. For example, a few weeks ago, the Claude Code main developer, Boris, posted a video where he just asked Claude, “Oh, check Claude Code for bugs using Lean.”

And Claude starts creating the models, finds bugs, tries to prove properties, notices that it cannot prove the properties, finds bugs, files the PRs, and Boris merges them. Yeah, these were real bugs. And Boris said, “I don’t even know Lean.”

**Ben Lorica.** Right, right.

**Leo de Moura.** But this is for bug finding. This works reasonably well. But for the next step, for you to say, “Yes, there are no bugs,” now you have to start looking at the model and saying, “Yes, this is an accurate model of my code, and I want to maintain them in sync.”

Maybe I will use AI. I will prove properties. You can start using Aeneas if your code is in Rust. You can try to use Aeneas to prove that it’s a faithful model of your code.

But we expect, in the next few months and years, people will focus way more on the specifications, on the models, on the properties you want the software to have.

**Ben Lorica.** So, yes. In fact, this same phenomenon is happening in math, most recently in this proof of the Navier-Stokes equation, right? So maybe the autoformalization step of translating the actual statement of the problem may not have been as faithful, or whatever.

But then it turns out that, in fact, they did prove the result. It’s just that, you know, you still needed humans to kind of catch this and make the subtle adjustments, right?

**Leo de Moura.** Yes. Yeah. No, this is an important point for Navier-Stokes, because it’s such an important result. Before OpenAI even started directing the models, we had a human-curated statement of the Navier-Stokes problem. The statement of the problem had already been curated by experts. There was no doubt about whether the statement of the problem was correctly formalized.

There’s some controversy in the results because OpenAI presented an informal proof written in natural language and a formal proof written in Lean. The informal proof and the formal one had mismatches in the proofs.

**Ben Lorica.** I think it’s at the level of whether you’re assuming a fourth or fifth derivative.

**Leo de Moura.** Yes, yes, yes. Correct.

**Ben Lorica.** Which matters for mathematicians, but you can see how subtle the differences were, right? So, but Boris—but let’s give you—

**Leo de Moura.** Let me give you a point. There’s a tool that I think is important to tell your audience about. There’s a tool called Verso Blueprint.

Verso Blueprint is a tool for when you want to present a Lean result mixing natural language with Lean. You can use Verso Blueprint. It’s almost like literate programming. You can mix natural language, LaTeX, with Lean and align the two presentations.

We are very confident that if the AI was using Verso Blueprint, this mismatch would not have happened, right? I mean, this is a matter of improving the tooling.

Another misconception people have is that they think a Lean proof is a static object. It’s not. Like we refactor code, you can refactor proofs. You can simplify them. There are groups, for example, for the Fermat’s Last Theorem proof by Anthropic. There are groups that have already reduced the proof by half.

**Ben Lorica.** By the way, this is true for proofs written by humans too.

**Leo de Moura.** Yes, true. Yeah, yeah, yeah. But here you get the machine check, right? I mean, you’re modifying the proof, but Lean keeps saying, “Yes, it’s a true proof,” right?

You can manipulate it. You can ask the AI to help you navigate the proof. You can align the proof with the informal presentation using Lean blueprints. This is all possible, right?

I mean, this is one thing that I see on social media that people are missing. These formal proofs are not static objects. They’re objects that you can manipulate, you can simplify, you can probe, you can ask questions, you can interact with them.

**Ben Lorica.** So I guess I want to just nail down this one point, which I think should be clear by now. But I just want to make sure the audience, who may not be familiar with what we’re talking about, gets this point, which is this difference between formal verification and plain QA and testing.

Why do I need—I mean, why should I bother with formal verification when I already have AI writing all these tests for me? And, you know, because I’m using AI to generate a plethora of tests, edge cases, you know, I have maximum coverage. Leo, I don’t need verification, right?

**Leo de Moura.** Yeah, testing is great for showing the presence of bugs, but not their absence.

Even if you use the AI to generate many test cases, your AI may not be as smart as the AI that will be used to find a vulnerability in your software, right?

I think the technologies are not in conflict with each other. They complement each other. For example, one mistake I see many people making is trying to prove something that’s maybe buggy. Well, no matter how many resources you put into it, how many tokens you burn, you’ll not be able to prove something that’s not true, right?

So testing is always a great way to start, as it increases your confidence, your understanding, and then you prove.

Now you’re proving for any input. You’re not proving that only for the tests I tried, it worked. But you’re proving that for any possible input, your program works, right? It’s for all instead of for some, right?

**Ben Lorica.** And honestly, I think software engineering may be behind other engineering disciplines. I don’t know, like aerospace or whatever, right? So maybe they’ve had more rigorous discipline around verification because, after all, there are much more serious consequences in these other fields, right? So, great points.

**Leo de Moura.** Great points.

**Ben Lorica.** I think this is just a case now that we have more and more people writing code, and code is easier to write. And it just happens that verification has had its ImageNet moment, so to speak.

I guess, Leo, the main thing that’s happened—because you folks have been working on Lean for a while, and Lean has been rock solid for a while—so the main transition is really this autoformalization, the capability of the frontier models to write Lean.

**Leo de Moura.** Yes, and it also connects to your previous comments. You made the point that, yeah, in some industries, people have used formal verification in the past. But why was it not popular, if it is so good? Because it’s too time-consuming to prove things by hand.

And AI dramatically reduces the cost of verifying code, and it gives you other benefits.

After you have a formal proof, right, you can start asking the AI, “I want you to change, optimize my code, but you have to keep proving that it is correct.”

It’s almost like the proofs are guardrails. Now the AI can try wild optimizations that you may not even understand exactly how they work, but you have a proof that’s saying, “Yes, it still computes the same thing.” It’s way more efficient, but it still computes the same thing.

**Ben Lorica.** So, based on what we’ve been discussing, it seems like the software engineering profession will now focus on not writing code. Maybe not even verification, because verification maybe you can also semi-automate based on what we’ve discussed.

It’s really requirements, right? So describing what you want.

So there’s two buckets, I guess. There’s the requirement gap. So is there a difference between what you want and what you are able to write down? And I guess what some people call the model gap, which is really: are the conditions around what you’re testing representative of production?

So, in other words, maybe your testing scenarios are too idealized in some ways.

And obviously, you alluded to this earlier: to some extent, you have to trust Leo and team, the Lean kernel, right? So as Lean becomes more popular, and you rely on it—and by relying, I mean you’re relying not only on Lean having verified your code, but all the libraries that your code depended on, right?

So I guess those are three things. There’s the requirement, there’s the model, and then there’s the Lean kernel.

**Leo de Moura.** Well, we have multiple. You don’t need to trust us. There are kernels implemented by other teams, right? This part, I think we’re addressing well.

I mean, we even have a kernel arena now where we collect kernels. They compete against each other to see which one is the fastest. If they get something wrong, if they have a bug, they are penalized, and everybody can see.

It’s called arena.lean-lang.org. You can see all of them. I mean, and all of them are—

**Ben Lorica.** Open source.

**Leo de Moura.** I think so. I’m pretty sure. I mean, I cannot say, I don’t use all of them. I use the main ones. The ones I use, they’re all open source. I mean—

**Ben Lorica.** Do they have to be written in Lean to be—

**Leo de Moura.** No.

**Ben Lorica.** No?

**Leo de Moura.** Some of them are implemented in Rust. There’s one implemented in C#. There’s another one implemented in Haskell.

Believe it or not, there’s one implemented in TeX. TeX, people, is this language for typesetting math papers and papers in computer science, math, and so on. But TeX is a full-blown programming language, and a colleague of mine, Kim Morrison, implemented a checker, a kernel, in TeX just to show that you can implement it in many different programming languages.

Oh, there’s one in COBOL, for example, and one implemented in Fortran.

**Ben Lorica.** So then the education and training of a software engineer will change, right?

**Leo de Moura.** Well, I think there will be way more focus on writing formal specifications, writing properties. The requirements are essential.

Many times, we don’t even know exactly what the software is supposed to do, right?

**Ben Lorica.** And by the way, Leo, some of these applications are complex, right? So they interact with databases, distributed systems—

**Leo de Moura.** Yes, yes.

**Ben Lorica.** And so on. So then, I mean, how do you exactly—you’re not going to be able to nail down the requirements of—or maybe that’s what distinguishes you.

You know, in the past, the superstar programmers were the super coders, right? Maybe in the future it will be the people with the ability to say, “Okay, so these are complex. I’m going to use Kafka. I’m going to use Spark.” Well, right?

**Leo de Moura.** Yeah, yeah. But the models will have imprecisions, right? Sometimes because people don’t know exactly what they want.

But look at the difference. Before, people would write a doc. Before they designed a database, they would have documents explaining the design, the architecture, in natural language. Some people would use Word documents to write the specification.

**Ben Lorica.** Usually there’s some sort of architectural diagram, right?

**Leo de Moura.** Yes, exactly, exactly. But then what happens? Now you have these informal documents. You have no way to check if the actual implementation matches these documents. Some human has to go over the codebase and keep checking, maintaining both in sync.

It’s a pain. I mean, you change the documents, you have to check that the implementation still matches. All of us know that we have comments that are out of sync in our code.

But if everything is formal, you’re going to have tools that will check whether your documents, your design, match the implementation or not. This is a step forward, right?

For sure, you’re going to have documents that are imprecise. Your customer wants X. Your specification is X prime. But you’re going to keep polishing, refining. This is the ground truth now.

**Ben Lorica.** And as the thing interacts with the real world, you go back and—

**Leo de Moura.** Exactly, and refine it. But now your code is going to have a tool that says the code matches the new model. You refine it, you improve it, right? It matches.

For example, some people say, “Oh, the code has been verified.” There are so many caveats. What does it mean to verify something? Are the properties you wrote the ones you really want?

I mean, you’re going to be in this process where you keep polishing your properties, making them more precise. Some software will have—you’re going to model threats. You’re going to have threat models, I mean, for people that try to attack your system.

**Ben Lorica.** So these descriptions, these requirement docs, I guess these PRDs, become what you track in your source control system. Because basically, in many ways, Leo, you know, as you learn more, right, you’re constantly verifying each version.

But it’s almost going to be the case that each version, I’m just going to have the AI write it from scratch. Why do I have to save any of the previous code?

**Leo de Moura.** Yes, yes, right. So—

**Ben Lorica.** The crown jewels become this, which ultimately is still going to be owned and maintained by humans, right?

**Leo de Moura.** Exactly, exactly.

**Ben Lorica.** Yeah, yeah, yeah, yeah. And tools like Lean, right?

**Leo de Moura.** Yes.

**Ben Lorica.** But doesn’t that mean you have to almost revamp the entire CS curriculum?

**Leo de Moura.** No, not—

**Ben Lorica.** Completely, but—

**Leo de Moura.** Not completely. But it’s going to adapt. I’ve seen many colleges and universities where formal verification is not even a required course, right? I mean, it’s more like a curiosity that people do not use in industry because it was too expensive until now.

I think the focus on formal verification will increase, but I think it’s still valuable to learn low-level—I mean, there will always be people who learn low-level assembly code, who learn low-level languages, who learn how to write code and check it, who will be assisting in designing systems at a very low level.

This will continue to exist. But you’re right, it will change for sure.

**Ben Lorica.** But in this world, Leo, you could be a very productive and very much in-demand engineer without actually being proficient in coding.

**Leo de Moura.** Yes, this is going to happen. Well, but in some sense, you’re writing—let’s say it’s still coding. Writing formal models, making proofs, is coding. You’re just working at a higher level of abstraction.

**Ben Lorica.** Yeah, yeah, yeah, yeah. By the way, I think you wrote this phrase, which I love, and maybe you can elaborate on what you mean, if you did in fact write this phrase, which is “the verified stack.”

**Leo de Moura.** Yes.

**Ben Lorica.** What do you mean by the verified stack?

**Leo de Moura.** Oh, the verified stack is about the critical software stack. I mean, there are some pieces of code where, sure, it’s not a big concern if they’re buggy or not. I mean, for example, a video game. It’s a piece of software. If it’s buggy, it’s annoying to the users, but there’s no crisis in the world if there’s a bug there.

The critical software stack is our operating systems, our compilers, the communication layer, TCP/IP. This is what I mean by the software stack.

And it would be great if we had models, specifications, for all these components. We have experts writing them, discussing them, and we start verifying properly, making sure these specifications, the code, the actual code, matches these specifications.

It’s a direction that I think is really worthwhile investing in for the future.

**Ben Lorica.** So, which is interesting. Leo, imagine you’re a CTO of a big Fortune 500 company, right? And then I come to you as a vendor, and I go, “Hey, Leo, you should trust our software because we are formally verified.”

**Leo de Moura.** Yes.

**Ben Lorica.** So what are some of the key questions you need to ask me in order to make sure that I’m not BSing you?

**Leo de Moura.** Show me the specifications. Show me your specifications, the properties, what you have verified, right?

Someone can formally verify that my property is just true. It’s basically saying there’s no property. You’re not making any claim.

I think you’re absolutely right that we should be aware of claims like “my code is formally verified” without explaining exactly which properties have been verified. What’s the specification you verified against? This is the kind of question.

**Ben Lorica.** What proof checker kernel did you use?

**Leo de Moura.** Exactly. It’s also important. Which tool? Another one that’s important is the trusted codebase.

There are tools that have a large trusted codebase. In Lean and other proof assistants like Rocq and Isabelle, they focus on keeping the trusted codebase really small, right?

**Ben Lorica.** Yeah, yeah, yeah. And obviously, to the extent that you can convince me that your software has run in a similar setting, in other words, a realistic setting—

**Leo de Moura.** Yes.

**Ben Lorica.** Maybe that adds to the meat behind the verification claim, right?

**Leo de Moura.** Yes.

**Ben Lorica.** Yeah, yeah. And so, what about—you know, we talk about software as if they’re just static things. But now we have software called agents, right? And then we now are entering the world of long-running agents.

**Leo de Moura.** Yes.

**Ben Lorica.** So what does verification mean in the world of agents?

**Leo de Moura.** Yes, great question. I think the first thing—we want to put guardrails on these agents. It would be great to have reliable sandboxes.

I mean, we have been seeing, in the last few weeks, many people claiming the agents are escaping the sandboxes.

So part of the verified software stack is the sandbox. We want reliable sandboxes, right? Good specifications for them, proofs that the implementation meets these specifications.

We also want to be able to write policies about the agent’s behavior, and we want to make sure the code that enforces these policies has been formally verified.

Imagine you spend all the time writing policies about what the agents can do or cannot do, and then there’s a bug in the code that enforces the policies, right?

**Ben Lorica.** But what if another agent is doing the formal verification? So there’s layers of trust, right?

**Leo de Moura.** Yes, yes. One possibility people consider is: can an agent exploit a flaw in the Lean kernel or a flaw in the runtime where the Lean kernel is running?

These are valid questions, and this is why we have multiple kernels. Because even if the agent were to find a flaw, it would try to trick you into believing it proved something it did not. And the certificate, this proof, would have to be accepted by multiple kernels that have been implemented in various—

**Ben Lorica.** Independently, yes.

**Leo de Moura.** Exactly. This is serious. I mean, these agents are extremely good at finding vulnerabilities.

**Ben Lorica.** So have you interacted with people who are using these things, the things we just discussed, verification specifically for agents?

**Leo de Moura.** Yes. At AWS, we have AgentCore, Dogwood. They are all using Lean now.

Outside, I mean, I gave the example of using Lean to find bugs in Claude Code. We’re going to see more and more projects like that because one main problem with formal verification and tools like Lean was this steep learning curve, right?

You’d have to learn Lean, write these models. It’s too much effort to start. But with AI, it helps you reduce the startup costs.

You can see value almost immediately, like Boris and his video he posted, where he just asks Claude to try to verify Claude Code using Lean, and he got value immediately.

And now it’s worth it. I almost feel like whenever you get immediate value, you start investing more time learning the technology.

Then you say, “Oh, maybe I should write my formal properties about my code that I want to hold. Oh, maybe I should learn how to read Lean code. I don’t want to write it, but I want to be able to read it, to be able to tell whether the model the AI agent created is accurate, right? And I want to discuss with the agent whether the model is accurate or not.”

**Ben Lorica.** So, for long-running agents, right, potentially agents that are exploring, doing research, coordinating among themselves, maybe for days, right, we discussed that one of the challenges, obviously, is just nailing down the requirements, right?

**Leo de Moura.** Yes.

**Ben Lorica.** I mean, how can you nail down the requirements for something like that, where it’s like a human being exploring all sorts of directions?

Imagine a human being who has infinite resources, can travel all over the world, and can do anything.

**Leo de Moura.** Yes, yes. It’s a big challenge, but the AI can help you start. It can write the first model.

And it’s going to say, “Oh, that’s how we formalize it.” Agentic systems are complex systems, right? There are multiple pieces interacting with each other. There are protocols there, communication protocols between these different pieces, and you want to make sure these protocols are correct, accurate.

The AI agents will help you get started with the model, the formal model. It’s almost like it’s going to give you the first draft, and you can keep iterating from there, right?

**Ben Lorica.** So it seems like ground zero for everything we’ve discussed right now is coding and math.

**Leo de Moura.** Yes.

**Ben Lorica.** And math, just over the last few weeks, you have Fields Medalists crying. You know, I mean, the whole field has just basically been disrupted in a big way.

**Leo de Moura.** Yes.

**Ben Lorica.** Right. So, my PhD is in math, so I used to be in that field. I wasn’t very good. Well, maybe now I could be, with infinite computing resources.

But yeah, so in that case, it seems like the combination of Lean and Mathlib especially makes math even more of a sweet spot than coding. Am I wrong?

**Leo de Moura.** No, no, no, no. What Mathlib gives you—Mathlib was constructed by humans, by math people that have strong math backgrounds, and so the definitions there, you know they’re precise.

**Ben Lorica.** But they must have started years ago. So they had the foresight that, hey, we’re laboring here in the coal mines in obscurity. No one appreciates what we’re doing. But once we scale—

**Leo de Moura.** Yes. When they started, I think nobody expected AI would get this good.

But in their minds, one win for the math community was when they realized that Lean helps them to work in big numbers.

**Ben Lorica.** Yeah.

**Leo de Moura.** For example, in 2024, Terence Tao had a project where he broke a project into smaller pieces. He was like an architect, right?

**Ben Lorica.** Yeah, yeah, yeah. And then there’s no way for him to verify everything personally, verify everything, right?

**Leo de Moura.** Instead of having AI agents, he had many humans filling the holes for him, many people that he never met, right, giving him the proofs.

He doesn’t have to check the proofs himself. It’s almost like humans filling holes in his big design.

And people said, “Wow, this is great.” I remember him telling me he never imagined that he would be working with amateurs. He only works with professionals.

**Ben Lorica.** Yeah, this goes back. This goes back to what you said very early on, which is trust.

**Leo de Moura.** Yes.

**Ben Lorica.** Right. Because in many ways, math, like most of science, is built on trust, right?

So I’m a mathematician. I’m proving a theorem. I’m citing many, many other theorems from the past, maybe some from 50 years ago, which I personally don’t know how to prove. I just trust that they’re correct, right?

**Leo de Moura.** Exactly.

**Ben Lorica.** And this is what they’ve done, right, with Mathlib.

**Leo de Moura.** Exactly, exactly. But at the time, I remember Terry giving talks and saying, “Well, one day AI agents will be in the loop. They will be filling these holes.”

But I think none of us imagined it would come so quickly, instantly.

**Ben Lorica.** Right. Yeah, yeah, yeah, yeah.

**Leo de Moura.** A few months ago, nobody would believe this. Six months ago, if I had told any of the Lean developers, the Lean Mathlib maintainers, “Oh, we’re going to have AI that will be this capable,” they would not believe it.

**Ben Lorica.** But now, because of autoformalization, the ability of AI to translate math into Lean, I think it’s just going to continue.

Which begs the question, Leo: why can’t I do this in theoretical physics?

**Leo de Moura.** Well, there is a library called Physlib for Lean, where they’re formalizing physics manually.

Well, now I believe they probably switched to AI. I think everybody’s moving to AI. I mean, even the Lean developers, we are now implementing using AI.

It was funny because it started like, “Oh, AI is really good at finding bugs and isolating the bugs for us. Oh, AI is really good at patching the libraries. Oh, AI is writing code. Oh, wow, it’s writing code better than we do.”

**Ben Lorica.** So what’s the timescale for physics to get disrupted as much as math? Theoretical physics.

**Leo de Moura.** They had their first win. I mean, a few months ago, they found a bug in an important physics paper by trying to formalize the results.

The challenge here is that in physics, we are creating models of the real world, right? Our models are always imprecise. They are models, I mean, of the real world.

**Ben Lorica.** Well, they start out as theoretical, right? For example, string theory has never been proven experimentally.

**Leo de Moura.** Yes.

**Ben Lorica.** So you could still be in a field in theoretical physics where the experiments haven’t happened yet, but still make progress and publish papers. And those people can get disrupted soon, right?

**Leo de Moura.** Yes, yes. They can write very accurate mathematical models now, right? I mean, using Lean. Yes, this is totally possible.

**Ben Lorica.** And so, since Navier-Stokes and since OpenAI published a bunch more results, have you personally heard from your mathematician friends, “Hey, Leo, what did you do to our field?”

**Leo de Moura.** No, there’s a lot of mixed feelings, right? Some of them are super excited, right? “We’re going to be able to attack problems beyond our imagination.”

There are people concerned about what’s going to happen. I mean, students, “What’s going to happen with me?”

I mean, like software development, we will have developers. The job will be different, but we will have them. For math, the same thing.

Yeah, I mean, major disruption. Some people are angry, some people are excited, some people are scared. But I hope this should be a good change.

**Ben Lorica.** So the question in my mind—I’ve been thinking about this—is, in the past, set aside software engineering, in math, right, you kind of knew who the best mathematicians were, right?

So you just start talking to them, and they know much more than you, right? Or they’re quicker. They know much more than you. They can prove harder theorems than you.

It’s almost like raw IQ was—there’s nothing you can do. I mean, that’s why I left the field, right? Because basically, I can’t compete with these people, you know.

So then, but now, does that change? Does that change who wins Fields Medals in the future?

So maybe it’s the person who’s smart, but not off-the-chart smart, but has access to more compute. I don’t know.

So have you thought about—in software, I would imagine it’s the same, right? If I have access to more compute than you, maybe I can get more done. I’ll definitely get more done than you.

**Leo de Moura.** Yes, exactly, exactly. Yes, I feel this is all true, what you said.

I mean, for me, I viewed one of my superpowers as being able to tolerate pain. I mean, developing software is painful.

**Ben Lorica.** Yeah, yeah, yeah, yeah. Proving theorems is painful.

**Leo de Moura.** Yes. Yeah. AI removed this pain, but yeah, I lost my superpower. My superpower is not as useful as it was in the past.

But we will be able to move faster, right? So we will be able to achieve more. I think this is good.

I mean, you’re right that a person with a different skill set may be more valuable in the future, or may be able to leverage—not say be more useful, but leverage the technology better.

We will have issues about access to tokens, right? But we see that in science all the time. I mean, for example, the James Webb Telescope.

**Speaker 1.** Yeah.

**Leo de Moura.** You have to get—you can write a proposal. You get access to the telescope. It goes in a queue.

I believe in the future, the same thing will be true for math. I mean, you have a massive project that requires an insane amount of money, but you’re going to write a proposal. Maybe it will be funded. You’ll get access to this model that’s extremely powerful.

**Ben Lorica.** Unless there’s a breakthrough and the models get more efficient, and, you know, kind of like in chess, right?

So in chess, I guess I haven’t kept up with this. Maybe you’d know. Is there a difference, Leo, between a chess program I can run on my laptop and a chess program that runs on a cluster? Does that give me—I don’t know.

**Leo de Moura.** I’m not a big chess player, but my perception is the ones running on a computer already are extremely good.

I mean, and I hope you’re right. We will have models that will run on our computers, but I believe here the stakes are so high that there will always be the supercomputer.

It’s like the James Webb Telescope. We can buy a telescope that’s really good—

**Ben Lorica.** Right.

**Leo de Moura.** And have it at home, but the James Webb is way better, but it’s way more expensive.

**Ben Lorica.** Which means, if looking ahead to the future, if you attend one of these academic conferences, like in math, the person who used to be popular was the smartest guy, the Fields Medalist. Now it’s the guy with the cluster.

Well, I mean, I think everyone wants to talk to the guy with the cluster.

**Leo de Moura.** Well, maybe this guy is not even the guy who has the ideas. I mean, there is a committee probably that manages the James Webb Telescope, but scientists submit proposals all the time, and they get access.

I mean, I think the same thing will happen in the future.

**Ben Lorica.** So we started with coding, and then obviously math. We speculated on physics.

So what other areas are you seeing, maybe below the radar, where this equivalent of Mathlib or something like that is happening?

**Leo de Moura.** I’m seeing many people interested in finance. Finance has many algorithms. I mean, people are interested in this area, right?

**Ben Lorica.** But the algorithms are—so in math, you have proofs. Theoretical physics, you have proofs. But—

**Leo de Moura.** There’s a lot of math in finance.

**Ben Lorica.** Yeah, yeah, I know.

**Leo de Moura.** I’ve been seeing people excited about applying formal verification to this area, using more precise models.

**Ben Lorica.** But the challenge there is they don’t work in the open, right? So they’re secretive.

**Leo de Moura.** Yes, yes, yes.

**Ben Lorica.** So how do you come up with Mathlib?

**Leo de Moura.** Yeah, yeah. I think there are—I’ve seen people that are open to trading algorithms. Probably they’re going to want to keep all their secrets, right?

But there’s a mathematical background for finance that people want to formalize and make public, right?

But you’re right. There will be people keeping their secret sauce.

**Ben Lorica.** So finance. What else?

**Leo de Moura.** I think people are trying to build libraries for chemistry, also in biology. I think any field.

**Ben Lorica.** And what’s the relative progress in each of the fields you named? Like, how far along are they?

**Leo de Moura.** Ah, they are all—I think Mathlib is by far ahead, because math is precise.

Then we have the physics library, and we have the computer science libraries that are trying to catch up to Mathlib.

Then we have these new initiatives in finance, chemistry.

But if you get the papers in each area, you see how precise, how rigorous these papers are. The math in these papers will give us a hint of how hard it will be to create a formal library.

**Ben Lorica.** Yeah, because there’s theoretical and computational chemistry, right?

**Leo de Moura.** Yes.

**Ben Lorica.** But then, don’t you think, Leo, because of autoformalization, any community that decides to go in this direction will get there faster, right? Because the Mathlib people toiled in obscurity for years, but once autoformalization happened, boom.

**Leo de Moura.** Exactly. Yes.

**Ben Lorica.** So are there non-scientific fields where you think this might happen?

So, for example, let me draw out a couple of examples for you, but I don’t think they will work, right?

Accounting. I have generally accepted accounting principles. I have the tax code. The tax code and GAAP are my theorems in many ways.

**Leo de Moura.** Yes.

**Ben Lorica.** But then here’s the problem. I talked to some accountants about this. The problem is, here’s some funds. Do I put that here or here or here, right? So it’s subjective.

**Leo de Moura.** Yes, yes, yes. I saw people trying to do formalization for law.

**Ben Lorica.** Oh, so yeah, yeah. I talked to someone about this too. But then the problem with law is, the reason we’re suing each other is we disagree.

**Leo de Moura.** Yes, exactly. But there are parts of law. I had once an intern who was a lawyer. He’s a lawyer now.

I mean, for example, civil procedures are very, very—

**Ben Lorica.** Formulaic. It’s almost—

**Leo de Moura.** Like a program.

**Ben Lorica.** Oh, really?

**Leo de Moura.** Yeah, yeah. There are parts of law that can be formalized, right?

**Ben Lorica.** I see. Yeah, yeah, yeah. And with that, thank you, Leo.

**Leo de Moura.** Thank you. Thank you very much. Thank you very much.
