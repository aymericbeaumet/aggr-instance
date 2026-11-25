---
title: Build more responsive apps with the Decisions API
link: https://openai.com/index/decisions-api
source: openai-com
published: 2026-10-22T19:00:00Z
updated: 2026-10-22T19:00:00Z
first_seen: 2026-10-22T20:45:08.711028436Z
labels:
- product
summary: Fast, focused, low-cost decisions for any application.
content: extracted
html: 2026-10-22-build-more-responsive-apps-with-the-decisions-api.html
---

Applications and agents make many small decisions throughout a workflow: how to classify an input, where to route a request, or which action to take next. Individually, they are simple. At scale, their latency and cost add up.

Today, we’re making the Decisions API generally available. Built with [GPT‑6 Luna](https://openai.com/index/introducing-gpt-6-sol-and-luna/), the Decisions API focuses Luna’s intelligence on questions with bounded answers, helping applications and agents make near real-time decisions from text, images, and structured inputs up to 10x faster than GPT‑6 Luna through the Responses API.

Since public beta, we’ve added a native GPT‑Live integration, prompt caching for reused context, and inference residency in the US and EU. Developers can also pass native JSON inputs, include images in question instructions, and ask up to 400 questions per request, twice the previous limit. We’ve also continued to improve latency, including additional capacity in several regions in Asia.

## How teams are using the Decisions API

Early adoption of Decisions has shown its applicability across a variety of use cases.

Across OpenAI, teams are applying the Decisions API across product and business workflows. The dots team is using it to reduce response times and improve interactivity. Our Personal Finance team is using it to categorize transaction descriptions, while our Communications team is using Decisions to classify coverage by the themes and scores whether it reinforces or challenges key narratives—helping identify what warrants closer review.

These early examples show how fast, structured decision-making can turn incoming information into useful action across a wide range of use cases. Customers are applying the Decisions API just as broadly, from accurately attributing AI costs to making language-learning experiences more interactive.

1 of 5

## Select the type of decision your application needs

Using the Decisions API is simple: developers define a question and its allowed answers, then provide context as text, images, or native JSON. The API returns one of three supported answer types—a probability, choice, or score—that applications can use to determine what happens next. The variety of answer types allows Decisions to be applicable across a wide variety of use cases.

Decisions API supports three answer types:

| Type      | Use it to                                                                                                         | Main result                                                                                   |
| --------- | ----------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- |
| predicate | Estimate the probability that a condition is true, such as whether a product has visible damage.                  | probability: an estimate from 0 to 1 that the condition is true.                              |
| choice    | Select one of your predefined options, such as whether an agent should continue, retry, or ask for clarification. | choice: one of your supplied values. The response also includes probabilities and confidence. |
| score     | Rate an input against ordered levels in a rubric, such as the severity of damage.                                 | score: the probability-weighted average of the level indices.                                 |

## Build real-time voice experiences with the Decisions API and GPT‑Live

With [GPT‑Live](https://openai.com/index/introducing-gpt-live-1-in-the-api/), you can bring natural, expressive conversation to your voice applications, with smooth interruption handling that keeps conversations flowing. Decisions API now [integrates natively with GPT‑Live](https://developers.openai.com/api/docs/guides/decisions-voice), helping applications make fast, focused decisions during live conversations. A customer support agent can decide whether to ask a clarifying question, route a request, or escalate an issue. An avatar can adjust its facial expressions as a user speaks to it, and based on the user’s words, respond with a wink or a smile to create a more interactive experience.

The example below uses transcript updates to select an avatar’s expression. Add this configuration to `session.triggers` in an existing GPT‑Live session.

Live emits a `decision.event`; the application reads the selected `reaction` and animates the character accordingly. [Learn more about connecting voice to Decisions API.](https://developers.openai.com/api/docs/guides/decisions-voice)

## Try Decisions with GPT‑Live for yourself

## Expression palette

Tap a face to preview it.

## Live decisions

Watch as the Decisions API chooses a facial expression based on what each of you says.

### Your words

Watch the reaction as you speak.

### Assistant’s words

Watch the face as the assistant replies.

Try saying

I rehearsed my audition for my cat. She walked out halfway through.

This demo is time-limited. By using it, you agree to OpenAI's [Terms](https://openai.com/policies/terms-of-use/) and acknowledge our [Privacy Policy](https://openai.com/policies/privacy-policy/).

## Build and evaluate your first decision

Decisions API is generally available today with GPT‑6 Luna through the dedicated Decisions endpoint. See the [pricing and availability guide](https://developers.openai.com/api/docs/guides/decisions#pricing-and-availability) for current rates and caching details.

Start with one recurring decision your application makes often. Define the question, the possible answers, and the context needed to distinguish them. Test it on examples that reflect real usage, in the [Decisions Playground](https://platform.openai.com/decisions) with your own text or images, or follow the [developer guide](https://developers.openai.com/api/docs/guides/decisions) to make your first request.

Decisions API costs $0.10 per million input tokens. Cached input costs $0.01 per million tokens. For 30-minute and 12-hour cached writes, [read our pricing guide.](https://developers.openai.com/api/docs/pricing?utm_source=chatgpt.com) You only pay for input tokens: there are no output-token charges. Regional processing is available in the US and EU, and [Zero Data Retention](https://developers.openai.com/api/docs/guides/your-data) is available to eligible customers. See [pricing and caching](https://developers.openai.com/api/docs/guides/decisions#pricing-and-availability) and [data controls](https://developers.openai.com/api/docs/guides/your-data) for details.
