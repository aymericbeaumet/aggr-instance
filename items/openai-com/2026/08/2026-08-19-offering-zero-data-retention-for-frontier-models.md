---
title: Offering Zero Data Retention for frontier models
link: https://openai.com/index/offering-zero-data-retention-for-frontier-models
source: openai-com
published: 2026-08-19T19:00:00Z
updated: 2026-08-19T19:00:00Z
first_seen: 2026-10-19T01:26:53.998049939Z
labels:
- company
summary: OpenAI reaffirms Zero Data Retention for eligible API customers and previews Private Safety Processing for advanced AI safety without compromising data privacy.
content: extracted
html: 2026-08-19-offering-zero-data-retention-for-frontier-models.html
---

***Update on September 22, 2026:*** *We are rolling out Private Safety Processing to API customers, with access expanding in phases. Private Safety Processing enables us to continue offering ZDR as frontier models become more capable. This is just the beginning, as we continue working with our customers on collaborative approaches to privacy and safety. Learn more about Private Safety Processing in our* [*developer guide*](https://developers.openai.com/api/docs/guides/private-safety-processing)*.*

Zero Data Retention gives eligible API customers a clear promise: OpenAI does not retain their prompts or model responses after a request is processed. Customer content is not available to OpenAI personnel for review [1](https://openai.com/index/offering-zero-data-retention-for-frontier-models#citation-bottom-1), and enterprise customer data is not used to train our models unless customers explicitly opt-in.

As models take on longer, more complex tasks, some serious risks may only become visible across multiple interactions. Existing ZDR-compatible safety systems evaluate each interaction individually. Today, we’re previewing Private Safety Processing, which is designed to identify patterns across related interactions without giving OpenAI personnel access to the underlying content.

For ZDR deployments, customer content remains on infrastructure the customer controls. We are also developing an option in which content is stored on OpenAI infrastructure, encrypted with keys controlled by the customer. In both cases, automated systems can identify potential misuse and return limited safety signals without exposing the underlying prompts or responses to OpenAI personnel.

## Why safety systems need to evolve

The most serious AI safety risks are not always visible in a single interaction. Often, potentially harmful intentions become clear only when multiple interactions are viewed together. Similar risks can arise when bad actors repeatedly probe safeguards, coordinate across accounts, or disguise threats as routine research. Risks can also develop over the course of an agentic task—for example, if a system becomes misaligned with the user’s intent by continuing to act after being told to stop.

As AI systems take on longer and more complex tasks, this broader context becomes increasingly important for distinguishing legitimate activity from misuse and ensuring that AI agents remain within the bounds of their intended authority.

Some recent frontier-model deployments have required customers to allow their AI provider to retain sensitive content for safety monitoring. For many organizations, such requirements conflict with their security obligations or commitments to the people they serve.

Private Safety Processing is designed so we can continue to offer ZDR.

## How Private Safety Processing works

Private Safety Processing builds on the automated protections already used in ZDR and other deployments. Existing ZDR-compatible safety systems evaluate interactions individually. Private Safety Processing extends those protections across related interactions, allowing automated systems to identify patterns without OpenAI personnel having access to retained customer content.

Private Safety Processing utilizes customer content regardless of where it is stored—whether in infrastructure customers control (ZDR deployments) or in storage provided by OpenAI. With OpenAI-provided storage, customer content is encrypted using keys controlled by the customer. OpenAI personnel do not have a copy of those keys, so they cannot access the underlying content.

When a risk is identified, OpenAI receives a narrowly defined signal indicating the type of activity involved, similar to our existing safety systems today. That signal can be used to determine whether enforcement is necessary. OpenAI personnel do not receive access to the customer content even when it is flagged.

Customers can investigate alerts and enforcement decisions using information available in their own systems. If they want to appeal, clarify legitimate activity, or support an investigation into verified abuse, they can choose to share relevant information with OpenAI.

Private Safety Processing is currently being tested with early customers. We are sharing this preview now because we’ve heard our customers loud and clear that they need predictability about how their content will be protected as AI systems become more capable.

## Privacy and safety built *with* and *for* our customers

Our mission is to ensure that artificial general intelligence benefits all of humanity. Collaboration with customers and partners is essential to how we build effective safeguards. As [our principles](https://openai.com/index/our-principles/) make clear, no AI lab can address emerging risks alone. Private Safety Processing reflects that approach and is being shaped by customers across industries, regions, and company sizes.

The organizations we work with handle some of the most sensitive information in their sectors, including financial records, health data, confidential business plans, and proprietary research. Protecting that information is essential to meeting regulatory obligations, maintaining customer trust, and preserving their competitive advantage.

Their feedback is helping us build stronger safeguards while keeping their information under their control.

We will continue working with customers on the technical and operational details of our approach. We plan to start rolling out Private Safety Processing, and share a technical white paper, in September. We’ll keep customers informed every step of the way, sharing updates early, explaining what they mean for existing commitments, and providing the time and support customers need to plan ahead.
