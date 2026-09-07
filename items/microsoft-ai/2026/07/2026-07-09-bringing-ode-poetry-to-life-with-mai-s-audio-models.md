---
title: Bringing Ode Poetry to life with MAI’s audio models
link: https://microsoft.ai/news/ode-poetry-mai/
source: microsoft-ai
published: 2026-07-09T07:50:31Z
updated: 2026-07-09T07:50:31Z
first_seen: 2026-09-07T17:03:44.343711062Z
authors:
- Daniel Victor
summary: The post Bringing Ode Poetry to life with MAI’s audio models appeared first on Microsoft AI.
content: extracted
html: 2026-07-09-bringing-ode-poetry-to-life-with-mai-s-audio-models.html
preview:
  file: 2026-07-09-bringing-ode-poetry-to-life-with-mai-s-audio-models.preview-faf2f33d9c51.webp
  width: 256
  height: 134
  color: '#5f5c33'
images:
- source: https://microsoft.ai/wp-content/themes/wp-base-theme/images/social.jpg
  original:
    file: 2026-07-09-bringing-ode-poetry-to-life-with-mai-s-audio-models.image-506809fe91d0.jpg
    width: 1200
    height: 628
  variants:
  - file: 2026-07-09-bringing-ode-poetry-to-life-with-mai-s-audio-models.image-01beb06726c5.webp
    width: 48
    height: 25
  color: '#273823'
---

William Sieghart is a world-renowned patron of poetry, a best-selling author of the [“Poetry Pharmacy” anthologies](https://www.penguin.co.uk/books/306569/the-poetry-pharmacy-by-sieghart-william/9781846149542) who helped establish [National Poetry Day](https://forwardartsfoundation.org/national-poetry-day/) in the UK. But it is at his one-on-one “consultations” at events across the country where he brings the power of poetry to life.

During these sessions, participants describe what’s on their mind and William recommends a poem. His measured, warm and compassionate approach has resonated with thousands of people for whom he has prescribed poems.

William asked Microsoft AI to help him scale these sessions to a global audience. [Ode Poetry](http://odepoetry.ai/), the result of this collaboration along with creative studio Gravity Road, leverages MAI’s state-of-the-art audio models to deliver a first-of-its-kind digital poetry experience.

“When William approached us with the concept for Ode Poetry, it felt like the perfect opportunity to bring our MAI audio models to life in a way that reflects our humanist-first values,” said Trevor Back, MAI product lead.

Ode Poetry asks you questions about how you’re feeling and recommends a poem that might resonate, playing a recording of a real person reading it. AI simply supports the connection between the listener, the creativity of the original poem, and its human reader. It does this through empathetic questions and a thoughtful approach to recommending poems based on William’s literary analysis.

The project presented an interesting technical challenge. In order to build a voice experience that responds empathetically to users in real time and recommends an appropriate poem, the system must:

- Listen carefully and understand any user
- Respond at the right time, in the right style
- Remain grounded in William’s unique style and poetry expertise

We used MAI’s voice models to power this experience, and worked closely with Ode to build custom solutions for their specific needs, including safety and response quality, all detailed below.

**Speech understanding with MAI-Transcribe**

Ode Poetry uses our [MAI-Transcribe model](https://microsoft.ai/models/mai-transcribe-1-5/) to understand spoken language in real time. A leader in its class, it can handle the nuances and complexities of verbal communication, including the many languages, accents, dialects and styles people use today.

The data reflects this. The latest version of MAI-Transcribe achieves the lowest Word Error Rate (4.86%) of any competitor on the FLEURS benchmark, showcasing its ability to understand many languages and difficult audio examples (such as background noise). The model enables users to engage naturally rather than adapting their speech to the technology. Real-world use cases such as Ode Poetry demonstrate the model’s ability to work effectively in production environments.

**Bringing expressive speech to conversational AI with MAI-Voice**

Ode Poetry also uses [MAI-Voice](https://microsoft.ai/models/mai-voice-2/) to re-create William’s distinctive vocal style. What makes MAI-Voice special is its ability to reproduce William’s speaking style with very high fidelity, using only a short sample of audio. Once you’ve heard William’s voice, you’ll understand why this is so important.

MAI-Voice’s capabilities were achieved by building our own large-scale multilingual data pipeline, augmented with high-fidelity studio recordings of voice talent. This enabled researchers to train a model with both the breadth to sound natural across contexts and the depth to faithfully reproduce any specific speaker. Here’s an example:

*An audio sample showcasing the natural expressivness available with MAI-Voice.*

**Reasoning between conversation and poem recommendations**

Ode Poetry is a unique conversational experience because it not only enables a natural conversation, but it also recommends a poem as an outcome, using the structured conditions William has provided for the poems in his “Poetry Pharmacy” books.

We developed a harness to distill William’s years of experience with personal consultations into a system that can match the unstructured conversation with the user into a framework that enables the right poem to be recommended. This system included carefully chosen deterministic logic and tool-calling mechanisms.

We also focused on responsible AI for a safe user experience; for example, experimenting with jailbreaks that can trick a language model into attempting real psychiatric diagnoses. Through an iterative approach of safety evals, red teaming, and guardrail updates, we were able to prevent a range of misaligned model behaviors.

Hill-climbing was also a critical component to achieving a premium editorial product. High-quality production AI systems hinge on countless small improvements. Accuracy, consistency, latency, reasoning, and expressiveness all contribute to the final result, and a fix in one dimension can cause regressions in another. We evaluated Ode Poetry across a wide range of emotional scenarios and used the results to iteratively refine prompts, recommendation logic and speech output. Achieving a consistently excellent result took relentless iteration of both text and audio prompts, and the shared commitment from both Microsoft AI and William’s team made this collaboration a joint act of creative care.

This is reflected in the app launching today. As William shared when he first began interacting with his digital extension: “One of \[the researchers\] recorded my introduction and the next thing I knew, he was playing my voice…and the intonation, the pauses…I just thought, wow, that’s quite nice.

“It’s really exciting for me,” he added, “because what we’re about to embark on together is in a way my lifetime’s ambition, which is to bring poetry to everyone.”

We invite you to try out the experience of a poetry session with William at [Ode Poetry](http://odepoetry.ai/), and test the latest versions of our models at the [MAI Playground](https://playground.microsoft.ai/).
