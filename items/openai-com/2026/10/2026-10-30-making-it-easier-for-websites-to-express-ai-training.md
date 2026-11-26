---
title: Making it easier for websites to express AI training preferences
link: https://openai.com/index/helping-websites-express-ai-training-preferences
source: openai-com
published: 2026-10-30T09:30:00Z
updated: 2026-10-30T09:30:00Z
first_seen: 2026-10-30T18:48:04.665156877Z
labels:
- company
summary: OpenAI will honor a shared robots.txt signal to help website operators communicate their AI training preferences to participating providers.
content: extracted
html: 2026-10-30-making-it-easier-for-websites-to-express-ai-training.html
---

Today, we’re announcing that OpenAI will honor a common [signal](https://datatracker.ietf.org/doc/draft-kelley-aipref-training-preference-signal/) in robots.txt, the file that lets websites express their preferences about the use of their content for foundational generative AI training. This voluntary effort makes it easier for publishers and website operators to communicate the same preference to multiple participating providers at once.

Previously, website operators would identify individual crawlers and maintain separate rules for each and every provider. Having a shared signal across multiple providers reduces that work for operators.

We aim to implement the signal in the coming months, and will share another update with details about our implementation when we do so. This new signal will not replace the controls we already provide through robots.txt. Our existing [crawler controls](https://developers.openai.com/api/docs/bots), which provide web publishers flexibility and control over their pages, will continue to apply and be respected.

This draws on the [ongoing work](https://datatracker.ietf.org/doc/draft-ietf-aipref-vocab/) of the Internet Engineering Task Force’s (IETF) AI Preferences working group. We’re joining other participants in the working group in today’s announcement of this shared signal, and believe this strengthens our continued participation in the IETF standards discussions. OpenAI will continue to adapt its practices as those conversations and the landscape continue to evolve in view of appropriate controls and fairness.

We believe this is a practical step toward an open web where people can publish and discover information, and website owners have clearer, simpler ways to communicate their choices about how their content is used. We encourage other AI providers to honor the signal.
