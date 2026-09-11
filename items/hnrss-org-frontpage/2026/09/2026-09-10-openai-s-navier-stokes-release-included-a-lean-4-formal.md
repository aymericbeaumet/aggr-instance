---
title: OpenAI’s Navier-Stokes release included a Lean 4 formal proof
link: https://www.johndcook.com/blog/2026/09/09/formal-method-revolution/
source: hnrss-org-frontpage
published: 2026-09-10T21:22:59Z
updated: 2026-09-10T21:22:59Z
first_seen: 2026-09-11T00:29:35.512207397Z
authors:
- ibobev
summary: 'Article URL: https://www.johndcook.com/blog/2026/09/09/formal-method-revolution/ Comments URL: https://news.ycombinator.com/item?id=49650326 Points: 126 # Comments: 124'
content: extracted
html: 2026-09-10-openai-s-navier-stokes-release-included-a-lean-4-formal.html
preview:
  file: 2026-09-10-openai-s-navier-stokes-release-included-a-lean-4-formal.preview-20d292422152.webp
  width: 250
  height: 250
  alt: John D. Cook
  color: '#b7a297'
---

Yesterday OpenAI announced a proof that settled a long-standing question about the Navier-Stokes equations from fluid dynamics. The announcement has created a lot of buzz, as one would expect. But there’s an aspect of OpenAI’s work that I haven’t seen anyone talk about: they posted a Lean 4 formal proof at the same time as their conventional human-readable proof.

Quite a few other mathematical conjectures have been settled recently using AI, and these have also been accompanied with formal proofs, using Lean 4 in particular.

Until very recently, generating machine-verifiable formal proofs has been **excruciatingly tedious**. In 2005, Henk Barendregt and Freek Wiedijk [wrote](https://www.cs.ru.nl/~freek/notes/RSpaper.pdf)

> To give an indication of how much work is needed for formalisation, we estimate that it takes approximately one work-week (five work-days of eight work-hours) to formalise one page from an undergraduate mathematics textbook.

That was the rule of thumb: **forty hours per page**. And this in the context of undergraduate textbooks. Research publications are much denser than textbooks. Furthermore, page 100 of a textbook probably depends mostly on material on pages 1 through 99. A sentence in a research article could cite anything that has been published before.

Say a research article takes 20 times more effort to formalize than page in an undergraduate textbook. Then formalizing the 166-page paper from OpenAI would take 132,800 person-hours. It took OpenAI 17 hours to verify their proof in Lean. I hesitate to use the word “revolutionary,” but lowering the cost of anything by **four orders of magnitude** is revolutionary.

I’ve used AI to generate formal proofs to check my work just for a little blog post. I wouldn’t dream of doing that if I had to pay someone a week’s salary to check my work.

Formal verification doesn’t just apply to mathematics. You could, for example, formally verify that a set of security policies are consistent and that, given certain assumptions, they accomplish their purpose. You could formally verify that a smart contract imposes a certain maximum liability. You could verify the correctness of mission-critical algorithms. These problems are easier than formalizing mathematics research, and it is easier to quantify the return on investment.

## Related posts

- [Automation and validation](https://www.johndcook.com/blog/2025/12/24/automation-and-validation/)
- [Formal methods let you explore the corners](https://www.johndcook.com/blog/2016/07/11/formal-methods-let-you-explore-the-corners/)
- [When are formal methods worth the effort?](https://www.johndcook.com/blog/2020/12/03/formal-proof-roi/)
