---
title: 'The War Against Slop and an Emerging Weapon to Fight it: Formal Verification'
link: https://latent.space/p/formal-verification/
source: latent-space
published: 2026-11-20T21:18:41Z
updated: 2026-11-20T21:18:41Z
first_seen: 2026-11-20T22:37:07.598874728Z
authors:
- Richard MacManus
summary: Proving software correct with math has traditionally required specialist work. Now AI and Lean are helping automate the process.
content: extracted
html: 2026-11-20-the-war-against-slop-and-an-emerging-weapon-to-fight-it.html
remote_preview:
  url: https://beehiiv-images-production.s3.amazonaws.com/uploads/asset/file/b6f560cd-a37a-4d62-8d6b-da5b59cf4790/formal-verification-feature.png?t=1795207886
  alt: 'The War Against Slop and an Emerging Weapon to Fight it: Formal Verification'
extra:
  thumbnail: https://beehiiv-images-production.s3.amazonaws.com/uploads/asset/file/b6f560cd-a37a-4d62-8d6b-da5b59cf4790/formal-verification-feature.png?t=1795207886
---

Proving software correct with math has traditionally required specialist work. Now AI and Lean are helping automate the process.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/b6f560cd-a37a-4d62-8d6b-da5b59cf4790/formal-verification-feature.png?t=1795207810)

The word of the year in the AI industry over 2026 has been **“slop”** — meaning low-quality output from a large language model. And with so much code being AI-generated now, [avoiding slop](https://www.slopmanifesto.com/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) has become a daily concern for software engineers. So how to do that? One way is to use **formal verification systems**, which use **mathematical proofs and logic** to verify the correctness of software or hardware.

With [AI’s growing prowess at solving math problems](https://latent.space/p/ainews-quasi-riemann-hypothesis-openai/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), there’s now a number of AI companies offering formal verification systems. They include [**Harmonic**](https://www.harmonic.fun/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), [**Theorem**](https://theorem.dev/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), [**Axiom Math**](https://axiommath.ai/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), and a new entrant called [**&Truth**](https://andtruth.com/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) (pronounced ‘and truth’). In this article we’ll look at some of the approaches these companies are taking.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/5aae4da4-e30f-4fb0-9404-ec940335b976/image.png?t=1795207761)

## How formal verification is used in software and AI today

In order to better understand the landscape of formal verification systems, we spoke to one of its leading proponents: [**Clark Barrett**](https://theory.stanford.edu/~barrett/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), &Truth’s co-founder and co-CEO. He’s also Corbel Professor (Research) of Computer Science at Stanford and was the **co-creator of** [**cvc5**](https://cvc5.github.io/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), a popular automated reasoning engine used in formal verification.

Without getting too in the weeds, cvc5 is what’s called **a “solver” for satisfiability modulo theories (SMT)**, which Barrett says is widely used in companies like Amazon. For instance, [cvc5 is used](https://www.amazon.science/news/how-academic-collaboration-delivers-real-world-security-to-amazon-customers?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) for **specification analysis and test generation in Kiro**, Amazon’s agentic development environment.

“It's been fun to see my tools go from a good idea in the lab to cvc5 is now called billions of times a day in production pipelines at Amazon,” Barrett told us.

When AI coding agent companies talk about “verifying” their work, it typically means agents **running tests, critiquing their own output, or having another AI model judge it.** So I asked Barrett how far short of real verification does that fall?

“The main difference is really just, how do you know when you're done,” he replied. “So if you're testing, patching, scanning — using all these existing tools — they're quite good. I mean, they find a ton of problems. They raise those issues. **But you're never quite sure, did I find all of them \[the issues**\] or is there one more lurking there that my tool can't find?”

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/ece6831f-0e4f-4ffa-a67f-73f2e5965c22/image.png?t=1795207761)

*&Atlas, the first tool of &Truth; it’s “a map of the world’s open-source code, starting with code written in the C programming language.”*

## Lean’s role in formal verification

&Truth builds on [Lean](https://lean-lang.org/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), **an open-source programming language and proof assistant**. The company combines AI with automated reasoning techniques to construct proofs that Lean can check.

Lean is a language used by frontier models to **solve all manner of supremely difficult math problems**. For instance, just this week GPT-6.2 Astra produced [what Vals believes](https://x.com/ValsAI/status/2123129591292432513?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) is “the first Lean proof of Smoktunowicz’s Gap Theorem.” Vals added that Anthropic also used Lean to formalize Fermat’s Last Theorem, a project of 13 million lines.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/dc7c1e72-2412-4b2f-8175-b34193c0a9a0/image.png?t=1795207760)

*Example of Lean code; via Lean project website*

&Truth is building what it calls a **“verification machine”** on top of the Lean framework, with the ambition of reading code “in any language, **proving whether it satisfies precisely stated requirements**, and enabling humans to trust it without having to understand every line.”

“What Lean does is it just checks that you didn't make a mistake,” explained Barrett. “So you can think of it as a very sophisticated checker for mathematics.”

This has implications for model training too. **Axiom Math uses formal verification to provide correctness feedback for training** its mathematical reasoning systems, with proof attempts helping improve future models.

In [an interview with Latent Space in June](https://latent.space/p/axiom/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), Axiom CEO **Carina Hong** made the point that formal verification — and what Lean enables — isn’t just a defensive measure.

“Formal verification, or verified AI, to us it's not just about handling or kicking out the lousiness, the hallucinations, the mistakes. **It's about scaling brilliance.** It's about super-intelligence.”

Axiom develops [AxiomProver](https://wal.sh/research/axiomprover-2026/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), an AI system that generates Lean proofs, alongside [AXLE](https://api.axle.axiommath.ai/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), its toolkit for exploring, validating, and manipulating those proofs.

**Harmonic’s** [Aristotle](https://www.harmonic.fun/aristotle/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), an “[automated theorem prover](https://www.harmonic.fun/news/intro-harmonic/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv)”, uses Lean heavily in its offering. “Inside Aristotle, AI writes the proofs and Lean checks every step of them, so a result never rests on the AI being right,” states the company on its website.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/2d6c813c-9ddb-4377-b8d4-c8f9c30ef57a/image.png?t=1795207761)

*Harmonic’s workflow*

## Cybersecurity and LLM-scaled formal verification

Given the heightened level of paranoia about cybersecurity these days — some might say in the days leading up to AGI — formal verification systems seem like they’re needed more than ever. And Barrett confirmed that **cybersecurity is a key focus for &Truth.**

“One really powerful use case for our technology is going through all the C code in the world, whether it be open source or closed source, and trying to ensure that it's memory safe. And then if it's not memory safe, we can point to: here's a problem with this piece of code, you need to fix it so that it's safe.”

**Security is also a focus of** [**Theorem**](https://theorem.dev/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv), one of &Truth’s competitors. “Verification is the only defense that holds while adversaries scale,” Theorem states on its homepage.

One of the more encouraging notes in the post is that formal verification, although currently token-expensive, **is starting to get more token-efficient.** On a set of 36 small verification tasks, Anthropic reported that Opus 5.5 reached a 50% solve rate using about 40% fewer tokens than Opus 5. This trend is expected to continue as **more models are “trained on long verification tasks.”**

Theorem and Anthropic describe **“LLM-scaled formal verification”** as a promising approach to providing security guarantees at a previously impractical scale.

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/1b924520-f2ef-4fa6-86bb-d7cab68eddd5/image.png?t=1795207761)

*Anthropic diagram: “from code to a machine-checked guarantee”; in this case using Rocq rather than Lean.*

## The “veri-coding” experience for developers

Lastly, how does formal verification fit into a developer’s workflow? On its website, [&Truth states](https://andtruth.com/mission/?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) that developers will move **from vibe-coding to “veri-coding”** — meaning the verification layer is part of the workflow.

“If you've got your verification machine connected to your agent, you can still say, ‘hey, I want you to build this,’ said Barrett. “Maybe there's a little bit of work to really nail down with the agent what the specification is of what you want to build. But then the agent can go under the hood, collaborate with the formal \[verification\] tool, and essentially what you'll see is almost no difference *except* your code will work the first time.”

The idea, then, is that **more of the developer’s time will be spent on spec writing and reviewing** than on coding or reviewing code.

## Verify all the outcomes

While I was finishing up this article, I came across [this tweet](https://x.com/dabit3/status/2123105522945585406?utm_source=latent.space&utm_medium=ainews&utm_campaign=lsbeehiiv) by Cognition dev rel Nader Dabit:

![](https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/a24c05ac-b9f0-4f6e-8ed9-fe3f42ec4f7e/image.png?t=1795207760)

Whether the term “outcome engineering” sticks or not, he’s onto something in terms of where we are with coding. When you just tell your team of agents what you want and they go ahead and build it, you (the human developer) need a way to **check the correctness of what your agents deliver.**

If companies like &Truth, Harmonic, Theorem and Axiom Math have their way, formal verification could become an increasingly practical way to check those outcomes.
