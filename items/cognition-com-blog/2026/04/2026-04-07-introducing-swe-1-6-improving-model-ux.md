---
title: 'Introducing SWE 1.6: Improving Model UX'
link: https://cognition.com/blog/swe-1-6
source: cognition-com-blog
published: 2026-04-07T00:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2026-04-07-introducing-swe-1-6-improving-model-ux.html
---

By Rohan Choudhury, Carlo Baronio, Ben Pan, Sam Lee, Eric Lu, Steven Cao, Joe Li, Andrew Wang, Adam Zweiger, Ray Wang, Gary Chang, Silas Alberti04.07.26

We’re releasing SWE-1.6, our latest model built for software engineering agents, and we’re making it generally available in Windsurf. SWE-1.6 is optimized for both intelligence and model UX. Moreover, it is industry-leading in both speed (up to 950 tok/s) and cost (free tier for the next 3 months).

Last month, we released [SWE-1.6 Preview](https://cognition.ai/blog/swe-1-6-preview), which improved on SWE-Bench Pro by more than 10% compared to our previous model SWE-1.5 while being post-trained on the same pre-trained model. SWE-1.6 was post-trained from scratch to jointly optimize for user experience and making the model feel smoother to use in addition to raw intelligence.

![Introducing SWE 1.6: Improving Model UX](https://cdn.sanity.io/images/2mc9cv2v/production/f511f28a1a0e1313b07a1bfa365c595a2eaeb36c-900x448.png?w=1600&fit=max)

While SWE-1.6 achieves comparable performance to the Preview model on benchmarks like SWE-Bench Pro, we’re most excited about its dramatic improvement in what we call “model UX”. As we observed in our [earlier post](https://cognition.ai/blog/swe-1-6-preview), the preview checkpoint exhibited several behavioral issues that added friction for our users. These included:

- Overthinking for simple problems, taking more turns than necessary for simple tasks.
- Calling tools sequentially rather than in parallel
- Preferring shell commands rather than its own tools
- Exhibiting “looping behavior”, getting caught in a circle of identical reasoning

Many of these axes aren’t measured by traditional benchmarks but significantly affect the infamous “vibes” users express when trying the model.

![Introducing SWE 1.6: Improving Model UX](https://cdn.sanity.io/images/2mc9cv2v/production/cd85a19a6892ddd1ac1c18df4ff96d4d627cfc9b-900x563.png?w=1600&fit=max)

We were able to significantly reduce the frequency of such behaviors in SWE 1.6. The model now uses parallel tool calls more often, loops far less and relies more on its tools than the terminal. This leads to more efficient trajectories and a smoother user experience: the model obtains context much faster and requires less input from the user.

In the example below, when asked a question about the PyTorch codebase, SWE-1.6 uses parallel tool calls far more than the preview and answers the question faster.

SWE-1.6 uses parallel tool calls much more than the preview.

One contributing factor to this improvement was the introduction of a length penalty into training, which discourages unnecessarily long trajectories. This directly reduces overthinking and looping, while implicitly encouraging more efficient behaviors like parallel tool use. During training, we observed the model response length growing much more slowly than before while maintaining its intelligence and coding ability. The below ablation shows that task solve rate stays similar while assistant turns stays flat.

![Introducing SWE 1.6: Improving Model UX](https://cdn.sanity.io/images/2mc9cv2v/production/ea4df71340c4c95c680993dae5158aca58ef7891-2118x906.png?w=1600&fit=max)

We also were able to significantly reduce occurrences of the model relying on the terminal and other improper tool use cases throughout training, avoiding cases where Windsurf users have to manually accept commands instead than letting the agent work continuously.

![Introducing SWE 1.6: Improving Model UX](https://cdn.sanity.io/images/2mc9cv2v/production/1e96b3389a582a48c94475af383d1b8e32512c86-900x467.png?w=1600&fit=max)

**Try SWE-1.6**

SWE 1.6 is available for everyone today in Windsurf, and will be free for the next 3 months. We have partnered with Fireworks to offer the free version at 200 tok/s. We have also partnered with Cerebras to offer a faster version of the model for our paying users at 950 tok/s, delivering the same intelligence with unmatched speed and cost.

SWE-1.6 Fast delivers output at 950 tok/s.

[Try them both](https://windsurf.com/?utm_source=blog&utm_medium=content&utm_campaign=product_launch&utm_content=SWE1-6) in Windsurf today!
