---
title: After Math
link: https://terrytao.wordpress.com/2026/09/12/after-math/
source: hnrss-org-frontpage
published: 2026-09-13T03:16:41Z
updated: 2026-09-13T03:16:41Z
first_seen: 2026-09-13T12:42:01.306673055Z
authors:
- throwaway81523
summary: 'Article URL: https://terrytao.wordpress.com/2026/09/12/after-math/ Comments URL: https://news.ycombinator.com/item?id=49679637 Points: 105 # Comments: 87'
content: extracted
html: 2026-09-13-after-math.html
preview:
  file: 2026-09-13-after-math.preview-74220dd64933.webp
  width: 200
  height: 200
  color: '#b8c6b8'
images:
- source: https://secure.gravatar.com/blavatar/bd4bda4207561b6998f10dec44b570f04ff4072b20f89162d525b186dfca3e49?s=200&ts=1789303025
  original:
    file: 2026-09-13-after-math.image-c6a843e0288b.jpg
    width: 200
    height: 200
  variants:
  - file: 2026-09-13-after-math.image-bf0c749b4b25.webp
    width: 48
    height: 48
  color: '#fcfcfc'
---

*\[This is a guest post by [Silvia De Toffoli](https://www.silviadetoffoli.net/) and [Eamon Duede](https://eamonduede.com/). This blog post was initially written in a different file format and converted using AI. — T.\]*

[Silvia De Toffoli](https://www.silviadetoffoli.net/) *(University School for Advanced Studies IUSS Pavia)* \
 [Eamon Duede](https://eamonduede.com/) *(Princeton University and Purdue University)*

On September 8th, 2026, [OpenAI announced](https://openai.com/index/navier-stokes-solution/) that it had produced an AI-generated solution to the Navier–Stokes existence and smoothness problem, one of the seven Millennium Prize Problems. The announcement kicked off debate over credit allocation and the respective contributions of humans and machines to the result. Moreover, the announcement intensified already circulating comparisons with earlier AI conquests in domains believed to otherwise exemplify human intellectual prowess. In a recent [statement](https://cims.nyu.edu/~tristanb/statement.pdf), Tristan Buckmaster, one of the mathematicians involved in the Navier–Stokes saga, wrote: “This is a Deep Blue–Kasparov moment.”

Existential questions for mathematics follow naturally: if AI can now provide answers to questions at the very frontier of mathematics, is the discipline on the verge of being “solved” as many have said of chess and Go? Like chess and Go players, should mathematicians just “keep playing” and rearrange their practices?

There is something right about the “keep playing” response. As philosopher C. Thi Nguyen ([2019](https://philpapers.org/archive/NGUGAT-2.pdf)) has been insisting, the purpose of playing a game is not exhausted by its aim (winning). The real point is not only the outcome but the process. This is perhaps clearer with a party game such as Twister than with chess: the aim of playing Twister is certainly not winning. But something similar also applies to deep intellectual games, like chess and Go. For instance, playing a game of Go well can be an achievement in defeat.

But in the context of mathematical practice, this feels like an unnecessary retreat. Instead, we can make a stronger move: *reject the characterization of mathematics as a game that makes the retreat seem necessary in the first place*.

The question, then, is not simply what comes after math, once AI can answer its hardest questions. It is also what we are after when we do mathematics in the first place.

The narrative that AI has “solved” mathematics rests on two assumptions, both seductive and plausible, but both wrong:

1. AI really did solve a problem in mathematics.
2. Mathematics is only about solving problems.

The first assumption is wrong because to really solve a mathematical problem, providing a mere answer (even if formally certified) is not sufficient. What is missing is an intelligible proof that human mathematicians can understand and use to advance the aims of mathematics. And, as we will argue below, even if AI were to give us just that, the story would not be over because the second assumption is wrong. Mathematics is clearly a much broader enterprise than just problem solving. Mathematicians strive to develop new concepts and theories, to ask and answer new questions, to unify disparate areas, to educate and sustain scholarly communities, and to produce work that is valued for its beauty and depth.

We can (and should) therefore reject the narrative of AI defeating humans at mathematics and start thinking hard about what mathematics really is and what we want it to be.

**Not All Answers Are Solutions**

OpenAI produced an answer to the question of whether Navier–Stokes can develop a singularity: *yes*. In *The Hitchhiker’s Guide to the Galaxy*, Deep Thought produced an answer to life, the universe, and everything: *42*. Neither is exactly what we wanted.

Of course, OpenAI gave us much more than “yes.” Deep Thought offered only a number, whereas OpenAI produced two artifacts that many are willing to call proofs. The first is a Lean formalization certifying validity. This was accompanied by a manuscript that appears to contain the corresponding informal proof. So, why is this still dissatisfying? The reason has to do with the underlying notion of proof itself.

There are, in fact, two notions of proof: a *logical* notion and an *intelligible* notion.

Modern logic characterizes proof in terms of deductive validity such that a proof can be checked by a mechanical procedure that does not itself require understanding of the mathematical argument. A Lean formalization meets these standards exactly and a Lean formalization of the Navier–Stokes result is therefore a genuine and important contribution: by meeting the demands of the logical notion of proof, it secures certainty.

But mathematicians also want something else from proof. They want understanding ([Thurston 1994](https://arxiv.org/pdf/math/9404236)). They want to know what makes a proposition true. This kind of knowledge trades in mathematical ideas that they can grasp, communicate to other experts, connect with existing knowledge, and use to make further progress. This is the intelligible notion of proof. As of now, it is not clear that OpenAI’s result has given the mathematical community the kind of value that one expects from the intelligible notion of proof.

Genuine proofs are at the same time logical and intelligible proofs. Historically, the two notions have tended to run together. This is because no mathematician could produce an enormously complicated logical proof without first grasping some of the key shareable ideas that made the theorem true. The logical notion of proof was primarily used to verify the correctness of intelligible proofs ([Burgess and De Toffoli 2022](https://philpapers.org/archive/BURWIM-6.pdf)).

But with AI, these two notions can now come apart dramatically. We can end up with formal proofs that float free from any intelligible proof.

This is not a criticism of formal proof. The converse problem is at least as serious. An intelligible mathematical argument can convey a grand idea while failing to establish that the result is actually true. Jaffe and Quinn ([1993](https://arxiv.org/pdf/math/9307227)) famously used Thurston’s geometrization theorem for Haken three-manifolds as an example: a major insight accompanied by insufficiently complete proofs could become a “roadblock rather than an inspiration.” And one motivation for Hales’s Flyspeck formalization project was to verify that the intelligible (but hard to check) proof presented for the Kepler conjecture was, indeed, a genuine proof ([Hales et al. 2009](https://arxiv.org/pdf/0902.0350)).

Therefore, falling short of either the logical or intelligible notion creates roadblocks where genuine proofs clear the way for mathematical progress. A real mathematical solution requires both logical correctness and intelligibility.

This is particularly clear in the case of the seven Millennium Prize Problems. They were not selected because mathematicians merely wanted seven *answers*, but rather because they wanted *fruitful solutions*. The Clay Mathematics Institute itself [explains](https://www.claymath.org/millennium/navier-stokes-equation/) why proof matters in the case of Navier–Stokes: “Because a proof gives not only certitude, but also understanding.”

What OpenAI has given us is an answer. But it is not clear that they have delivered a fruitful solution. Perhaps, we will find that they have, but at the moment, the situation is far from clear. A genuine solution will provide adequate grounds for believing the result but also an intelligible mathematical argument that allows the result to become part of mathematics as understood and practiced by mathematicians.

Nevertheless, if it turns out that what OpenAI has provided is a mere answer, this is not enough to dispel the existential threat that mathematics is facing. Future AI systems are likely to produce genuine proofs that are at once formally certified and fully intelligible to mathematicians. So, current concerns that mathematics is on the verge of being “solved” by AI are not fully dispelled by simply insisting on genuine solutions rather than mere answers.

**You Need More than Solutions to “Solve Math”**

If future AI systems will produce genuine proofs, logically correct and intelligible, like those produced by “master” mathematicians, it would still be incorrect to think that mathematics would have been “solved” as some say that chess or Go have been solved.

In chess and Go, we accept radically uneven competition between humans and machines because both are, in the relevant sense, *playing the same game*.

But mathematics is not (or at least not only) a game. To begin with, there is no winner. Mathematics is not an adversarial game with determinate conditions for victory. It is certainly true that mathematicians compete with one another for fame, prizes, jobs, and credit. Chess players do those things too. But chess players also win chess. There is no corresponding condition for winning mathematics. There is no mathematical checkmate.

In mathematics, it is more natural to treat AI as an assistant rather than as a competitor. As Jeremy Avigad ([2026](https://proofsandprompts.com/2026/09/07/what-is-mathematics-now-and-what-should-it-be/)) puts it, “We should keep in mind that AI is nothing more than technology, designed to serve our purposes. It is misguided to think of mathematicians as competing with AI; when we drive a car, we aren’t competing to see who can go faster, and when we use a phone, we aren’t competing to see who can speak louder.”

But there is a deeper, and in many ways prior, problem with the competition framing. It requires accepting the assumption that solving problems is the activity by which mathematical success should be measured.

Genuine problem solving is certainly one of the principal aims of mathematics. It is not, however, its only aim. Mathematics is a body of knowledge engaged with, interpreted, and digested by a scholarly community and not a registry of results in the abstract. This simple point has even motivated an entire movement in the philosophy of mathematics: [the philosophy of mathematical practice](https://plato.stanford.edu/entries/mathematical-practice/).

Terence Tao ([2026](https://arxiv.org/pdf/2608.16753)) lists many goals of mathematics beyond problem solving. These include developing new theories and techniques, understanding the world, sustaining a community, training the next generation of mathematicians, contributing to cumulative knowledge, and creating works of aesthetic value. Of course, these have been positively correlated with genuine solutions.

But AI breaks that correlation, for the same reason it separates the two notions of proof. So, even genuine solutions would not satisfy us.

*This is not moving the goalposts but recognizing that any specific goalpost is inadequate.* If mathematics is a game, it is an infinite one.

This attitude is not reactionary. We reject both the concession that logically establishing a theorem is sufficient for a genuine proof and the reduction of “AI for mathematics” to proving theorems. Accepting this, we may find many opportunities for AI in mathematics to support human mathematical flourishing.

**Aftermath**

We do not deny that, if OpenAI’s announcement is correct, this is an extraordinary achievement. But we should get clear about what type of achievement it is. At this moment, it is an answer, not a solution. And, even if in time the result reveals itself as a genuine solution, we have argued that, in the practice of mathematics, solutions are not everything.

The urgency of rethinking what we value in mathematics is already being recognized within the mathematical community. In a recent [declaration](https://mathandai.org/) initially signed by 25 Fields Medallists, mathematicians warn of a “severe misalignment” between the goals of AI companies and those of the mathematical community.

Mathematicians need to do more to examine their norms. The priority norm is not the problem here (though it is likely a separate problem). The current failure to distinguish genuine solutions from answers, and the growing focus on problem-solving alone, are. This way of thinking is inspired by the current credit economy in mathematics and, as David Bessis recently discussed in his [blog](https://davidbessis.substack.com/p/the-fall-of-the-theorem-economy), the credit economy needs rethinking.

AI presents mathematics not with an ending but with a choice about what mathematical practice should become. If mathematical success comes to be identified too closely with the production of certified answers, mathematics risks adapting itself to precisely those features that are easiest to benchmark and automate away.

If, instead, mathematicians treat AI as a technology for advancing its long-standing and centrally human purposes, the technology may come to contribute to an accelerated flourishing and enrichment of the discipline. The important question is, therefore, not whether AI will defeat mathematicians, but which mathematical ends we want AI to serve.

What remains in the aftermath is not merely leftovers for humans to scramble for once machines have devoured all of the real problems. Rather, it is an opportunity to clarify what mathematics is all about. We should ask again what we are after when we do mathematics.

(An extended version of this text will appear elsewhere.)
