---
title: How Stripe is designing Checkout for AI agents
link: https://stripe.dev/blog/how-stripe-is-designing-checkout-for-ai-agents
source: stripe-com
published: 2026-09-22T00:00:00Z
updated: 2026-09-22T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Cara Mecozzi
summary: How Stripe used WebMCP to accelerate agent purchases from the browser.
content: extracted
html: 2026-09-22-how-stripe-is-designing-checkout-for-ai-agents.html
remote_preview:
  url: https://stripe.dev/og/blog/how-stripe-is-designing-checkout-for-ai-agents.png
  alt: 'Stripe Developer Blog: How Stripe is designing Checkout for AI agents'
---

In order to help Stripe sellers benefit from the rapid growth of AI agents, we enabled WebMCP on all Stripe Checkout UIs resulting in 42% fewer tokens consumed and 38% fewer tool calls invoked against baseline measurements.

While many businesses (Stripe included!) have built MCP servers as a programmatic interface for agents, browser use remains a predominant path for agents to interface with existing services. Websites, traditionally, have been optimized for human usage. To complete a checkout, an agent must interpret both the visual interface and the Document Object Model (DOM), infer both the information presented and which actions are available, and then plan and execute those actions. Even with ongoing model improvement, navigating a webpage is still slow, indirect, and token-intensive.

Just as we optimize human buying experiences to minimize latency and clicks, we need to optimize agent buying experiences to minimize token usage and tool calls. These are the primary measures of agent efficiency: tokens indicate cost, while tool calls indicate effort (and a high number of tool calls can also signal confusion!).

We believed WebMCP, an emerging standard gaining traction, was a strong candidate to improve the agent checkout experience.

This problem is no longer hypothetical. Agents like [Muse](https://stripe.com/newsroom/news/stripe-helps-meta-muse-shop-with-link), Instinct, and Grokbot, are launching and becoming popular – typically equipped with a virtual machine and browser –  and we are seeing and anticipating growth in agentic volume processed on Stripe:

![](https://stripe.dev/images/how-stripe-is-designing-checkout-for-ai-agents/image2.png)

We decided to incorporate agent-friendly WebMCP across Stripe Checkout UIs, which powers over 7.8M businesses processing around 0.45% of global GDP.  We aimed to give millions of merchants access to future agentic shoppers without them needing to change their integration while providing a fast and reliable consumer experience.

As a baseline without WebMCP enabled, we measured an average of 1.8M tokens consumed, 38.83 tool invocations, and >2.5 minutes duration for these agents to complete a transaction with Stripe Checkout. Even in chat-based applications where there is some expectation of latency, this is all simply too high.

This blog post focuses on how we implemented WebMCP to reduce token consumption and tool invocations.  We struck a balance between building a specialized interface that improved agent performance, yet keeping it aligned to the human interface for better maintainability.

## What is WebMCP?

[WebMCP](https://developer.chrome.com/docs/ai/webmcp) is a browser technology that lets websites “expose” MCP tools to agents from the client side. Instead of inferring how to complete a task and navigating the DOM, an agent can execute tools, such as get\_order\_summary or fill\_payment\_form. The agent doesn’t necessarily have to look at any of the HTML on the page.

![](https://stripe.dev/images/how-stripe-is-designing-checkout-for-ai-agents/image3.gif)

Above is a Stripe Checkout page.  We can see that invocations of the available tools result in changes to the UI – select\_payment\_method dynamically updates the UI and fill\_payment\_form put in credentials like the card number – we theorized this is a great improvement over ingesting all of the HTML into the LLM context and ascertaining element ids or classes to infer where buttons were (or hardcoding flows that could be brittle).

## Building for WebMCP

With a baseline measurement of ~39 tool calls to reach payment, each consuming tokens and time, we focused on tool design that would substantially reduce that number. And for our initial build, we preferred not to “fork” the business logic behind the page depending on whether the user was a human or an agent.

A catalog of tools can eliminate some of the ambiguity that comes with visual browser automation, but with a large catalog, the agent must still decide which actions are valid. An agent presented with too many tools, irrelevant parameters, or invalid actions can still struggle to complete the task.

Consider a checkout that supports multiple payment methods, dynamically rendered address fields, and a payment button that is only activated after the required information has been provided. If an agent receives every possible action at once, its context includes tools and parameters that may be irrelevant or impossible to use.

Instructions in the tool descriptions can help, but they still leave the agent unnecessarily responsible for reasoning about constraints the application already understands. You can think of it like pruning the branches of a tree to  progressively guide the agent towards the target outcome.

### Reveal tools as they become actionable

We designed Stripe’s WebMCP implementation around progressive tool disclosure, meaning that the page only exposes only the tools and parameters that are relevant in its current state. This makes agent interactions more reliable while keeping human and agent experiences aligned. Concretely, we want to minimize cases where available tools can result in sequenced invocations that result in errors.

As an example, we do not reveal the submit\_payment tool until all the fields have been filled. A simplified checkout might progress like this:

1. The agent inspects the checkout session and available payment methods.
2. After selecting a payment method, the page exposes a form filling tool whose schema reflects the visible fields.
3. The agent provides the required information using that tool.
4. Once the form is complete and valid, the page exposes submit\_payment.

If the checkout state changes, the available tools change with it. For example, if the agent changes payment method type to a non-card method, we’ll remove the number from the parameter set of fill\_payment\_form.

Rather than mapping every possible action upfront, this approach reduces tool calls, improves reliability, and increases successful payment outcomes.

Additionally, we relied on many of the existing accessibility patterns and optimizations in our frontend code to keep a shared infrastructure rather than creating a second checkout implementation exclusively for agentsWebMCP tools delegate to the same foundations used by human-facing checkout interfaces. As we continue to optimize Stripe Checkout for humans, agents will reap all the benefits. And, now that we’re closer to human and agent parity, we’re interested in how agent-first improvements may reflect in the human experience.

### Avoid forking our business logic

We used two WebMCP API types to connect tools to the existing interface: imperative and declarative tool implementations. Our existing UI surfaces have both state management and forms designed to handle the end-to-end payment flow. We did not want to introduce an “agent-only” bifurcation that could result in drift or add toil for engineers.

With the Imperative API, we introduced tools like get\_order\_summary that derive their responses from existing state and select\_payment\_method for explicit actions like changing payment method that initiate existing state transitions.

For form filling, we used the Declarative API to create tools that derive from rendered HTML. The browser can infer an input schema from the controls that are currently visible rather than requiring us to maintain a second, manually duplicated form schema. This means that if a developer adds a new form field, it will bubble up to the new tools by default.

Agents using browser automation tools like Browserbase can then invoke these tools directly to get checkout information and complete payments.

## Evaluating performance

To assess whether WebMCP made agents attempting to checkout more efficient without sacrificing reliability, we measured checkout completion, execution time, tool calls, and token consumption.

We ran 60 tests across six models on a fictional outdoor retailer website using the Stripe Checkout full page UI. 50% of the tests used WebMCP and 50% used a baseline browser interaction. Each test required the agent to complete the same end-to-end task to navigate to the website, add a product to the cart, proceed to checkout and complete payment.

In all of the tests, agents completed the checkout successfully. But when we compared the agents that used WebMCP, we found that agents performed significantly better across three categories.

![](https://stripe.dev/images/how-stripe-is-designing-checkout-for-ai-agents/image4.png)

Agents using WebMCP consumed 42% fewer tokens, completed checkout 39% faster, and made 38% fewer tool calls per run.

![](https://stripe.dev/images/how-stripe-is-designing-checkout-for-ai-agents/image1.png)

We measured total tokens (input + output + cache) consumed by the agent to complete the task. Token usage contributes directly to the cost of an agentic transaction, so reducing tokens can make these transactions less expensive for agents to support at scale.

![](https://stripe.dev/images/how-stripe-is-designing-checkout-for-ai-agents/image5.png)

We also counted the number of actions (clicks, form fills, tool invocations) an agent takes to complete checkout. Fewer tool calls mean the agent took a more direct path through checkout, with less exploratory form interaction.

![](https://stripe.dev/images/how-stripe-is-designing-checkout-for-ai-agents/image6.png)

And finally, we measured wall-clock time from task start to a completed checkout. This looked at the time it took for the agent to navigate to the URL, select a product, complete the checkout form and submit the payment. The reduction in execution time lowered the time a customer spent waiting for an agent to complete a purchase by 60 seconds.

## Up next

WebMCP is one step in an evolving standard of browser-agent behavior, where interfaces can serve people and agents without requiring separate experiences for each. As agents become an even more popular way people interact with the web, engineers will need to decide how to communicate desired actions to both human and machine users.

As WebMCP and browser agents evolve, we’ll continue improving Checkout for agent-driven purchases to help businesses serve customers however they choose to buy, whether they interact with Checkout directly or ask an agent to act on their behalf.

Ready to build for agent driven purchases? [Get started with Stripe Checkout and WebMCP](https://docs.stripe.com/agentic-commerce/for-agents/webmcp).

We’re still early in understanding what becomes possible when it comes to building for AI agents. There’s a lot left to build at Stripe, and the work spans infrastructure, developer tools, product systems, and AI all at once. If that’s the kind of problem you want to work on, [we’re hiring](https://grnh.se/b5wzjpge1us).
