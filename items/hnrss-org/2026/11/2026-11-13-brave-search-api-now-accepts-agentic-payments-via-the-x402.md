---
title: Brave Search API now accepts agentic payments via the x402 protocol
link: https://brave.com/blog/search-api-agentic-payments/
source: hnrss-org
published: 2026-11-13T03:08:08Z
updated: 2026-11-13T03:08:08Z
first_seen: 2026-11-13T15:39:08.702380659Z
authors:
- sjdh76
content: extracted
html: 2026-11-13-brave-search-api-now-accepts-agentic-payments-via-the-x402.html
remote_preview:
  url: https://brave.com/blog/search-api-agentic-payments/images/featured.jpg
---

The [Brave Search API](https://brave.com/search/api/), our developer tool for building applications with data from the Web, is adding support for instant, pay-as-you-go, agentic payments enabled by the open x402 payment protocol. Developers and autonomous AI agents that specifically require agentic search can now programmatically query Brave’s independent search index, ground agent responses, and settle requests in real time, which eliminates the need for traditional API key management.

Integrating x402 with the Brave Search API makes it easier for an agentic harness to leverage Brave Search API via a CLI wallet, as Brave runs its own facilitator accepting USDC on the Base blockchain, with acceptance on other networks and with other stablecoins incoming soon. This allows a Brave Search API user the ability to pay for LLM Context (agentic grounding) or general web search on-demand, without first creating an account and selecting a plan. Instead, simply transfer USDC stablecoin to your agent’s wallet via the Base network and it’s ready to access the same high-quality search results powered by Brave’s search engine.

## How does it work?

Brave Search API endpoints behind x402 require payment on every request rather than authentication. A request sent without payment receives an HTTP 402 Payment Required response that states the price of that search and the payment details needed to pay it. The agent signs a USDC payment for that amount and retries the request with the signed payment attached. Once the payment is verified, the search runs and results are returned with a receipt of the transaction that occurred on the Base blockchain.

## Get started today

Pricing follows Brave’s published API rates: LLM Context, an API designed specifically for providing search context for agents, will cost $0.005 ($5 per 1,000). Additionally, web (search for humans), news, image, video, and local endpoints will also be available for $0.005 ($5 per 1,000). Gas fees for transacting on the Base blockchain will be covered for a limited time. Since each request carries its own payment, a Brave Search API account is no longer needed to use the service.

The easiest path to query via x402 is to install [Purl](https://github.com/stripe/purl) Wallet, a Stripe-developed CLI wallet for your agent. Set up your wallet, top it off using Base-native USDC, and start querying today.

Ready to build pay-as-you-go search into your apps or AI agents? Check out our Brave Search x402 Proxy Documentation and explore: [https://api-dashboard.search.brave.com/documentation/guides/agent-payments](https://api-dashboard.search.brave.com/documentation/guides/agent-payments)

## Related articles
