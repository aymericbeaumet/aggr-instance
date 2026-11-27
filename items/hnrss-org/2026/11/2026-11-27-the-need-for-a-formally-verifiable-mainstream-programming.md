---
title: The need for a formally verifiable mainstream programming language
link: https://blainehansen.me/post/mainstream-verifiable-language/
source: hnrss-org
published: 2026-11-27T05:22:06Z
updated: 2026-11-27T05:22:06Z
first_seen: 2026-11-27T13:45:44.456295755Z
authors:
- todsacerdoti
content: extracted
html: 2026-11-27-the-need-for-a-formally-verifiable-mainstream-programming.html
---

Because of LLMs, the discipline of software engineering is in crisis. [Quality is nose-diving](https://blainehansen.me/post/mainstream-verifiable-language/\(https://pages.faros.ai/hubfs/AI_Engineering_Report_2026_The_Acceleration_Whiplash_Faros.pdf\)), and I see no reason why it won't keep getting worse. Software engineering [was struggling *before* LLMs (opens new window)](https://www.it-cisq.org/wp-content/uploads/sites/6/2022/11/CPSQ-Report-Nov-22-2.pdf). The default was to be sloppy and poorly considered, and now everyone is frantically accelerating those same terrible practices to LLM speed.

Something has to change. [I've been preoccupied with this for a while (opens new window)](https://www.youtube.com/watch?v=Lf7ML_ErWvQ), but [much more qualified people finally fully agree with me (opens new window)](https://leodemoura.github.io/blog/2026-2-28-when-ai-writes-the-worlds-software-who-verifies-it/). The only way forward is to fully embrace [formal verification (opens new window)](https://en.wikipedia.org/wiki/Formal_verification), all the way down to the foundations of our civilizational software stack. We have to post-hoc verify the things we can, and rewrite the rest.

But that's easier said than done. Existing formal verification tools suffer from massive [usability (opens new window)](https://catarinagamboa.github.io/papers/pre_print_barriers_liquid_types.pdf) and [understandability (opens new window)](https://github.com/magmide/magmide?tab=readme-ov-file#taught-effectively) problems, and have failed to make any kind of noticeable impact on the broader culture of software engineering.

It might be tempting to say that LLMs will actually *save* us in this regard, by allowing humans to create formally verified code without needing to understand its fundamentals in any way. This is pure cope. Merely having excellent proof assistants and LLMs will never be good enough. Even in a world where something like Lean is extremely scalable and LLMs can write all proofs, it doesn't matter if they aren't adopted by actual industry engineers as they build the real apps we actually use. For provably correct code to defend us from the LLM future:

- It needs to be possible to formally verify code that runs in any environment, from web browsers down to embedded systems.
- Industry engineers need to know provably correct code is even possible and be comfortable enough with it to at least tell their LLMs to write it.
- Those industry engineers must be able to reason about formal correctness properties enough to **even know the right things are being proven**. [In the words of Leonardo de Moura (opens new window)](https://leodemoura.github.io/blog/2026-2-18-proof-assistants-in-the-age-of-ai/):

  > Proof assistants guarantee that proofs are correct, but correctness alone is not enough. Someone has to read the theorem statements and confirm they capture the intended mathematics. This is the human contract that no amount of automation can eliminate.

This is what most voices are missing in this discussion: a safe future isn't going to be delivered by a small priestly class of formal methods researchers, too much code must be produced in too many contexts. The entire discipline of software engineering *has to grow up* and demand that all code we produce is at minimum provably safe. This requires many different projects and bets that attack this imperative of cultural change from different directions.

## The long winding road of Magmide

My chosen angle is Magmide, a project whose goal is to create a new formally verifiable programming language intended for immediate adoption by practitioners. However when I first had this idea my specific plan was woefully over-ambitious and unrealistic, since I wanted to make a whole new proof assistant and an assembly-level base language so the system could be verified all the way down to the metal.

I pursued it for a time as a startup, and I met with some investors and was applying for an SBIR grant. [Then I realized some obvious problems with my approach (opens new window)](https://github.com/magmide/magmide#construction-magmide-is-dormant-for-now-as-i-rethink-what-it-couldshould-be-construction):

- It didn't make sense to build a *new* proof assistant when Lean has been making such rapid progress.
- An assembly-level language was insanely difficult and wasn't necessary to gain adoption from practitioners.
- I didn't have the appetite to pursue some strained business model for a project that ultimately makes most sense as a pure public good.

So now after a few years [pursuing other projects](https://blainehansen.me/post/votebase/) and letting all these ideas marinate, I've arrived at a much simpler and more pragmatic vision of what Magmide could be. If I started working on it today, it would:

- Compile to WebAssembly using the [Garbage Collection (opens new window)](https://web.dev/blog/wasmgc-wasm-tail-call-optimizations-baseline) features.
- Have a Rust-inspired type system but without the difficulties of the borrow-checker.
- Start with basic and more easily learnable [SMT powered (opens new window)](https://goto.ucsd.edu/~ucsdpl-blog/liquidtypes/2015/09/19/liquid-types/) [Liquid Types (opens new window)](https://goto.ucsd.edu/~ucsdpl-blog/liquidtypes/2015/09/19/liquid-types/) and [Flow Effects (opens new window)](https://blainehansen.me/post/flow-effects/).
- Over time integrate Lean as the proof layer, and use some separation logic paradigm like [Iris (opens new window)](https://iris-project.org/) to bootstrap the language and verify its own implementation.

This design is specifically intended to appeal to actual engineers and "meet them where they are". The roadmap would start from a simpler more mainstream design and go "bottom up" by adding more and more proving capabilities, rather than going "top down" by starting with the most formally powerful system and then seeking to make it more usable.

A possible analogue for the adoption of Magmide is [Deno (opens new window)](https://deno.com/): Deno is "just javascript/typescript", but it adds a *runtime* layer of permissions checking. Engineers who care about sandbox safety have enthusiastically adopted Deno, and Flow Effects would offer strictly more security.

## Nothing will save us but us

I continue to stand behind basically all the arguments I made in the [Magmide repo (opens new window)](https://github.com/magmide/magmide) (aside from the specific architecture). Formal verification *must* become the mainstream of software engineering, and we have to embrace a wide diversity of approaches to make that happen. Software engineers aren't a monolith, and not every cultural pocket will respond to the same new tools and resources.

And you know what, LLMs will probably help a lot! But we can't just wait around for an all-knowing machine god to save us from ourselves. If we bet our whole future on ASI making all our problems go away and then that [doesn't happen (opens new window)](https://knightcolumbia.org/content/ai-as-normal-technology), history will look back on us even more foolish and disastrous than [all the millennialists (opens new window)](https://en.wikipedia.org/wiki/Great_Disappointment) who merely wasted their own time and resources.

It is our responsibility to meet the challenges of our age with full agency and responsibility, and treat the emergence of a beneficent super-intelligence as an improbable jackpot.
