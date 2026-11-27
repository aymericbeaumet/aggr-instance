---
title: Stripe’s new AI Assistant in VS Code
link: https://stripe.dev/blog/stripes-ai-assistant-vs-code
source: stripe-com
published: 2025-03-18T00:00:00Z
updated: 2025-03-18T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Mathew Varughese
summary: Learn about the Stripe AI assistant in VS Code and how we built it, including technical details on retrieval, evaluations, measurement, and more.
content: extracted
html: 2025-03-18-stripe-s-new-ai-assistant-in-vs-code.html
remote_preview:
  url: https://stripe.dev/og/blog/stripes-ai-assistant-vs-code.png
  alt: 'Stripe Developer Blog: Stripe’s new AI Assistant in VS Code'
---

The raw text content behind [docs.stripe.com](http://docs.stripe.com) is over 130 MB (for comparison, my college [Operating Systems textbook](https://pages.cs.wisc.edu/~remzi/OSTEP/) is a 7 MB PDF) and is updated hundreds of times each week. Although LLMs have Stripe knowledge in their pre-training, they almost instantly become outdated and they will occasionally produce incorrect information. Payments integrations are sophisticated and Stripe offers a lot of customizability.

That’s why we built a [Stripe AI Assistant](https://docs.stripe.com/stripe-vscode#ai-assistant) into our VS Code extension, which answers your questions by searching relevant Stripe knowledge (such as API reference entries, integration guides, code examples, and developer Discord threads). This improves accuracy over using a raw LLM tool alone and allows personalized responses tailored to your Stripe account.

[**Visit**](http://ai.stripe.com/vscode) **[https://ai.stripe.com/vscode](https://ai.stripe.com/vscode) [to get started with the Stripe VS Code extension.](http://ai.stripe.com/vscode)**

![](https://stripe.dev/images/stripes-ai-assistant-vs-code/image8.png)

## Using the extension

Visit [https://ai.stripe.com/vscode](https://ai.stripe.com/vscode) to automatically install and open the Stripe AI Assistant.

The extension is also available in the [VS Marketplace](https://marketplace.visualstudio.com/manage/publishers/stripe/extensions/vscode-stripe/hub). We recommend you also install the [Stripe CLI](https://docs.stripe.com/stripe-cli#install) to enable more features (streaming webhook events, personalized AI responses, and more). Learn more about the extension in the [docs](https://docs.stripe.com/stripe-vscode).

[GitHub Copilot](https://code.visualstudio.com/blogs/2024/06/24/extensions-are-all-you-need#_stripe) users can start by typing `@stripe` in the chat. Or, if you do not use Copilot, the extension offers its own chat UI. Here’s an example of asking the extension a question:

![](https://stripe.dev/images/stripes-ai-assistant-vs-code/image1.png)

The assistant calls our backend to retrieve relevant up-to-date Stripe docs. This reduces hallucinations, which is when an LLM produces factually incorrect output. The extension also inserts your API key automatically, which allows you to quickly test out code snippets that’ll just work as-is.

We can run the customized code from the assistant to get a working [payment link](https://stripe.com/payments/payment-links?utm_campaign=AMER_US_en_Google_Search_Brand_Payment-Links_EXA_PHR-21355960722&utm_medium=cpc&utm_source=google&ad_content=701657884201&utm_term=stripe%20links&utm_matchtype=e&utm_adposition=&utm_device=c&gad_source=1&gclid=Cj0KCQiAoJC-BhCSARIsAPhdfShZAm0LFpUhEzbDKJeCeQl-iPTqXB0J9NeKGK0KvB2kqruxdB64cVAaAicKEALw_wcB).

```
import stripe

stripe.api_key = "rk_test_..."

product = stripe.Product.create(
    name="Original Abstract Painting",
    description="24x36 inch acrylic on canvas",
    images=["https://example.com/artwork-image.jpg"],
)

price = stripe.Price.create(
    product=product.id, unit_amount=50000, currency="usd"  # $500.00
)

payment_link = stripe.PaymentLink.create(
    line_items=[{"price": price.id, "quantity": 1}]
)

print(f"Share this payment link: {payment_link.url}")
```

In addition to retrieving documentation, the Assistant also searches through thousands of [Stripe Developer Discord](https://stripe.com/go/developer-chat) threads. Here is a more complex question about subscriptions:

![](https://stripe.dev/images/stripes-ai-assistant-vs-code/image3.png)

This is a real question a user asked on our [Discord server](https://discord.com/channels/841573134531821608/1199688011680583700). Stripe engineers assist users in troubleshooting their integration issues on Discord in real-time. We built a pipeline which utilizes an LLM to summarize the issue and the proposed solution. Then, through human evaluations, the best summaries are picked to enter our search index. The team reviews hundreds of summaries per week to curate high quality knowledge that require combining insights from multiple docs. By indexing these threads, the Stripe AI Assistant is able to answer more complex integration questions.

## Guided flows from docs

![](https://stripe.dev/images/stripes-ai-assistant-vs-code/image4.png)

We’re also rolling out a “Open in VS Code” button into our documentation. We are starting with our [integration quickstart guides](https://docs.stripe.com/checkout/quickstart). This will deeplink into the VS Code extension and start to guide you through a specific integration directly in your editor. It has the same AI-assisted tooling, so you can ask a question at any point during the integration, with answers specific to your codebase.

![](https://stripe.dev/images/stripes-ai-assistant-vs-code/image6.png)

If you want to get started integrating with Stripe you can go and start using the extension now. Visit the [docs](https://docs.stripe.com/stripe-vscode) to get started. Continue reading to learn more about the technical architecture that powers the extension.

## How it works

To help address LLM hallucinations, we use Retrieval Augmented Generation (RAG) to pick the correct pieces of Stripe knowledge and place them into a prompt that we send to a LLM. This diagram explains how it works:

![](https://stripe.dev/images/stripes-ai-assistant-vs-code/image2.png)

This is what happens when a question is asked:

1. **Classifier**: We classify a query into categories (API Reference, Documentation, Coding Help, etc). This also helps prevent any adversarial or irrelevant questions and allows us to improve our retrieval by expanding the query.
2. **Retrieval**: We continually tweak our index and query settings to improve our search. At a high level, though, we use a combination of keyword search using [BM25](https://www.elastic.co/blog/practical-bm25-part-2-the-bm25-algorithm-and-its-variables) in addition to [k-nearest-neighbor](https://www.elastic.co/guide/en/elasticsearch/reference/current/knn-search.html) embedding search to retrieve relevant document chunks.
3. **Rerank**: Consider the question “What are the main components of a Connect integration?” A naive search might return our doc on “[Getting started with Embedded Connect components](https://docs.stripe.com/connect/get-started-connect-embedded-components)” but a better doc to use would be our more general [Connect overview guide](https://docs.stripe.com/connect/how-connect-works). Reranking is the process of reordering search results after retrieval; we use page traffic and other factors to tune the order of results.
4. **Prompt**: We build a prompt combining the relevant document sources, code snippet examples, user code, and the user’s question, send the full prompt to an LLM (currently Claude Sonnet), and stream its response back to the VS Code assistant.

To ensure the assistant is up to date, we have a nightly [Temporal](https://temporal.io/resources/on-demand/stripe) workflow that chunks and embeds thousands of our integration guides, API reference entries, code snippets, and summarized developer Discord threads. Since documents can be too long to be easily digestible by an LLM, we “chunk” by splitting documents into smaller portions. We then encode the semantic meaning of the chunks into [embeddings](https://www.cloudflare.com/learning/ai/what-are-embeddings/). We search over these embeddings as a part of our retrieval set.

For the VS Code extension chat UI, we partnered with Microsoft to be one of the first users of the new [chat extensions](https://code.visualstudio.com/api/extension-guides/chat) framework. With this framework, any VS Code extension can contribute an “@ mentionable agent” to make GitHub Copilot more powerful.

## Evaluations

Measuring success when working with AI is critical. since LLMs are nondeterministic, we need to be careful to objectively assess the quality of responses. Each user’s question (scrubbed to remove any personally-identifiable-information) and generated response is logged to our LLM evaluation tool. We can then compare cohorts of questions and their responses.

### Step 1: Building a dataset

We constructed a “golden” dataset of user questions, paired with hand-picked documents that we determined were the best sources to answer with. But to fully stress test our RAG system we needed much more data. We augmented this golden dataset with a synthetic dataset, created by using an LLM to generate questions a user might ask which were relevant to each Stripe doc. We manually inspected the generated questions and removed any generated questions that did not make sense. After doing this, we now have thousands of questions alongside expected URLs.

![](https://stripe.dev/images/stripes-ai-assistant-vs-code/image5.png)

### Step 2: Measure

When a user asks “Which customer address does Checkout use for taxes?,” responses should use the [https://docs.stripe.com/payments/checkout/taxes](https://docs.stripe.com/payments/checkout/taxes) doc. The simplest measure of correctness is: Did the retrieval step pick the right doc?

However, the retrieval engine returns an ordered list of multiple docs, so we can be more sophisticated in measuring correctness. We use Mean Reciprocal Rank (MRR) to test how effective our relevance scoring and reranking is. If we retrieve a doc but it's ranked second on our list, that doc gets a higher score than if it were fifteenth. MRR is calculated as 1 / position, so if the correct doc is retrieved in the second position MRR will be 0.5, third will be 0.33, fourth would be 0.25, and so on. Taking the mean of these reciprocal ranks across all the questions in our dataset gives us an overall measure of correctness.

![](https://stripe.dev/images/stripes-ai-assistant-vs-code/image7.png)

In the above example, an LLM generated the question, “What webhook events should I listen for in a subscription integration?” The doc used to generate this was [docs.stripe.com/billing/testing](https://docs.stripe.com/billing/testing), which is considered the *best* doc to answer this question, but our first result is [docs.stripe.com/webhooks](http://docs.stripe.com/webhooks). Since the correct result here (the billing testing doc) is ranked 4th, we have a reciprocal rank of 0.25. This indicates that there is room for improvement here.

### Step 3: Improve

Armed with an evaluation system, we can formulate hypotheses about the behavior of our retrieval and adjust it accordingly. For example, we noticed that semantic embedding search alone was not doing a great job of picking relevant sections of the [API reference](https://docs.stripe.com/api). We started using a “hybrid” search combining traditional keyword-based and semantic embedding search, and saw an uplift in accuracy scores. For another example, we noticed one pattern that lots of questions phrased like “how do I build a payment app” would have low MRR due to including [Stripe Apps](https://docs.stripe.com/stripe-apps) docs. We improved our classification and reranking strategy to fix this and saw improvements.

This evaluation suite runs nightly to ensure there are no regressions in quality. Currently, on our test synthetic dataset, we are including the best source ~91.11% of the time, and have a MRR of ~78%. We also manually evaluate live user logs (both the actual text of generated responses as well as source accuracy). To help scale those manual evaluations, we also built an automated [LLM-as-a-Judge system](https://huggingface.co/learn/cookbook/en/llm_judge). This system reads users threads and scores our AI responses (Did this response help answer the users question? Was it useful?). We continually improve the LLM-as-a-Judge by comparing its scores to our own. This helps scale our evaluation s as we get more usage.

## Conclusion

With the Stripe VS Code AI assistant, you can quickly integrate Stripe by asking questions without leaving the comfort of your editor. Read the [Stripe VS Code extension documentation](https://docs.stripe.com/stripe-vscode) to learn more. We are just getting started - if you have additional thoughts or suggestions on other things you would like to see, do share them on [Stripe Insiders](https://insiders.stripe.dev/).

For more Stripe developer learning resources, subscribe to our [YouTube Channel](https://www.youtube.com/@StripeDev).
