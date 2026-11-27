---
title: Using AI agents to build Stripe-Salesforce integrations faster
link: https://stripe.dev/blog/build-stripe-salesforce-integrations-faster-with-agents
source: stripe-com
published: 2026-04-13T00:00:00Z
updated: 2026-04-13T00:00:00Z
first_seen: 2026-11-27T00:01:05.630507547Z
authors:
- Vishnu Samavedula
summary: Building payment and revenue integrations across Stripe and Salesforce has a steep learning curve spanning both platforms. CLI-based AI coding agents make this faster, but hallucinate on complex, evolving platform surfaces. Open-source grounding context files flatten the learning curve and reduce hallucination, letting developers ship working integrations sooner.
content: extracted
html: 2026-04-13-using-ai-agents-to-build-stripe-salesforce-integrations.html
remote_preview:
  url: https://stripe.dev/og/blog/build-stripe-salesforce-integrations-faster-with-agents.png
  alt: 'Stripe Developer Blog: Using AI agents to build Stripe-Salesforce integrations faster'
extra:
  content:interactive: true
---

As an experiment, [Claude Code](https://code.claude.com/docs/en/overview) was pointed at a project using the [Stripe App for Salesforce](https://docs.stripe.com/use-stripe-apps/stripe-app-for-salesforce/overview). The goal was to generate a Flow that creates a [Checkout](https://docs.stripe.com/payments/checkout) from *Salesforce* *Opportunity* line items when a deal closes, then synchronizes the payment status back to the *Opportunity* record.

The real outcome would have been to use the out-of-the-box action `stripeGC.v02_CreateCheckoutSessions` or learn the API structure of a [Checkout Session](https://docs.stripe.com/api/checkout/sessions) and build a helper [apex](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_writing.htm) class within Flow Builder or use the built-in [agnostic invocable](https://docs.stripe.com/use-stripe-apps/stripe-app-for-salesforce/training/code-examples-agnostic) to construct the API request body. But the AI had no way to know that from the public documentation, and it couldn’t determine how to set up and handle authentication, translate incoming events, and other architectural nuances.

The agent also omitted `AsyncAfterCommit` on the scheduled path, causing the Flow to error "Callout from triggers not supported" on deployment. The line-item mapping used incorrect field names. The payment-status sync pointed at a custom field that didn't exist. To get Flow working, it took five more iterations over a couple of hours, and most of that time was spent on problems the AI introduced. How can this process be improved?

### AI agents are reshaping integration work but context gaps hold them back

Some of the top AI coding tools like [Claude Code](https://code.claude.com/docs/en/overview), [Codex](https://chatgpt.com/codex), and [Gemini CLI](https://geminicli.com/) are changing how integration work happens. They aren't just code generators; they plan, prototype, scaffold, test, and deploy, all from a single terminal session. Every major platform already has a CLI interface and agentic tools with access, orchestrating the full workflow. The development environment is dramatically getting more context-aware; you brainstorm, plan and describe the outcome, while the agent builds it.

An integration surface like the Stripe App for Salesforce has over 200 namespace-qualified invocable actions, platform-specific callout constraints, and [Shadow DOM](https://developer.salesforce.com/docs/platform/lwc/guide/create-dom.html) rules for payment components. The agent doesn't have the context it needs, so it researches (by using tokens, often on irrelevant documentation), guesses (based on generic patterns), and generates code that looks correct but breaks on deployment.

These aren't model failures. They're context failures.

### Why Stripe App constraints break AI agents

The instinct is to tell the agent "go read the Stripe App for Salesforce docs". But documentation is written for humans, as a narrative spread across pages, organized by concept rather than constraint. An AI agent parsing [docs.stripe.com](https://docs.stripe.com/) won't extract "you must use `AsyncAfterCommit` for any Flow that makes a Stripe API callout" because that rule is hidden in a paragraph about Salesforce governor limits, not stated as a machine-readable constraint.

The alternative - to skip the docs and reverse-engineer the package from your org - isn't much better. You would run:

```
sf data query --query "SELECT Name FROM ApexClass WHERE Name LIKE 'v02_%'" --use-tooling-api
```

This pulls all the `stripeGC` invocable classes, reads each one to understand its parameters, and loads all of that into the agent's context. You then hope it can piece together the naming conventions, input models, and platform constraints. It's a multi-step process that still leaves the agent guessing about rules that aren't expressed in the class signatures.

Grounding context files shortcut both paths. They're structured for AI consumption: terse, constraint-first, pattern-oriented. Instead of tutorials and narrative docs, a grounding file states rules directly:

```
RULE: Record-triggered Flows making Stripe API calls MUST use
      scheduledPaths → pathType: AsyncAfterCommit
REASON: Salesforce prohibits callouts in synchronous after-save context INVOCABLE: stripeGC__v02_CreateCheckoutSessions                                                                                                                           
NAMESPACE: stripeGC                                                                                                                                                       
INPUT MODEL: v02_CreateCheckoutSessionsInput                                                                                                                              
REQUIRED FIELDS: line_items, mode, success_url, cancel_url                                                                                                                
PATTERN: Always pass Stripe Account record ID as accountRecordId                                                                                                          
                                                                                                                                                                        
LWC: Stripe Payment Element mounting                                                                                                                                      
RULE: Use data-* attributes for element containers, NEVER DOM IDs                                                                                                         
REASON: LWC Shadow DOM scopes element IDs — Stripe.js cannot find them
```

Platform constraints, invocable action signatures, and LWC patterns are all in one place and machine-readable. These agents consume context files natively. You drop the context directory into your project root, and the agent reads it before generating anything.

The more autonomous the agent, the higher the cost of wrong context. When an agent generates 200 lines of Flow XML and hallucinates the action name at line 3, the other 197 lines are likely wasted. Grounding files prevent that at the source.

### What's in the repo

An [open-source set of grounding context files](https://github.com/stripe/stripe-salesforce-ai-context) has been published for the Stripe App for Salesforce. These encode the actual package surface:

- Invocable actions catalog — all `v02\_\*` actions with exact names, parameters, and namespace prefixes
- Request/response models — model definitions so the agent builds correct input objects
- Platform architecture — package structure, custom objects (`stripeGC\_\_Customer\_\_c`, `stripeGC\_\_Invoice\_\_c`), and the three API patterns (invocable actions, Apex SDK, REST)
- Framework constraint rules — Flow XML generation rules (connector integrity, `AsyncAfterCommit`), LWC Shadow DOM mounting patterns, and CSP configuration requirements
- Validated examples — deployable Flow XML, Apex controllers, and LWC components tested in scratch orgs

The core context files are ~35K tokens, enough for most Flow, LWC, and Apex generation tasks. The full reference catalog (every invocable action signature, every model field) is ~230K tokens. Your agent can pull those in on-demand when it needs to look up a specific action or model structure. You don't need to load everything for every task.

### Proof: same task, with grounding context

Here's the same task that failed earlier but with the grounding context files loaded.

```
$ claude
> Read context/LOAD_ALL.md and load the core context files it lists. When ready, tell me what you loaded.
```

The agent reads the context files and makes two architectural decisions before writing any code. First, it finds `v02_CreateCustomers` in the invocable actions catalog and recognizes that customer creation has a simple enough input signature to call the out-of-the-box invocable directly from Flow. No wrapper needed.

But for the Checkout Session, the agent sees nested input models — line item price data, product data, session parameters — that don't map directly to Flow variables. Instead of generating brittle XML with dozens of inline parameters, it creates a lightweight helper Apex class that uses the `stripeGC.v02_CreateCheckoutSessions` SDK internally and exposes an `@InvocableMethod` interface to the Flow:

```
stripeGC.v02_CreateCheckoutSessions.Params params =                                                                                                           
         new stripeGC.v02_CreateCheckoutSessions.Params();                                                                                                         
       params.lineItems = new List<stripeGC.v02_CreateCheckoutSessionsReqLineItem>{ lineItem };                                                                      
params.mode = 'payment';                                                                                                                                      
    	params.successUrl = input.successUrl;                                                                                                                                                                                                                                                                                  
List<stripeGC.v02_CheckoutSession> results = stripeGC.v02_CreateCheckoutSessions.createCheckoutSessions_2025_04_30(                                                                                    
new List<stripeGC.v02_CreateCheckoutSessions.Params>{ params }                                                                                        
     	);               
```

The Flow itself stays simple: it calls the OOTB customer action directly and routes to the helper class for the Checkout Session.

The full output includes:

- `AsyncAfterCommit` scheduled path — no callout errors
- Direct OOTB invocable call for customer creation
- Helper Apex class for Checkout Session with correct SDK model names — not hallucinated
- Stripe Account lookup preamble with null-check
- Idempotency key pattern and fault connectors for error handling

Using the grounding files results in significantly fewer iterations and takes minutes instead of hours. These are directional numbers from my own usage, not benchmarks, but the difference is consistent.

Org-specific details still require verification: the Stripe Account name, custom field mappings, and error logging paths. The agent provides the structure while the user validates the specifics.

This isn't limited to Flows. The repo also includes an LWC + Stripe Payment Element example — embedding a payment form on a Salesforce record page with correct Shadow DOM mounting, CSP configuration, and server-side payment confirmation. The same grounding files that teach the agent Flow constraints also teach it LWC patterns, Apex SDK usage, and webhook handling. See the [examples directory](https://github.com/stripe/stripe-salesforce-ai-context/tree/main/examples) for the full walkthrough.

### Beyond code generation

Generating Flows and Apex is the obvious use case, but grounding context changes how you explore the integration surface itself. Instead of skimming through docs to answer "can this app do X?", you ask the agent. It has the full catalog loaded: every out-of-the-box invocable action, every model definition, every constraint.

Questions that used to require digging through multiple doc pages get answered in seconds:

- *Does the Stripe App support creating subscription schedules directly from a Flow?*
- *Why am I getting a callout error on this record-triggered Flow?*
- *Can I create a Stripe Invoice from Apex without building a custom integration?*

The Stripe App for Salesforce supports two API patterns that can help here. Out-of-the-box invocable actions (`v02_*`) cover the most common Stripe operations (customers, payments, invoices, subscriptions) with pre-built inputs, outputs, and error handling. But when you need a Stripe API method or version that isn't covered by the packaged actions, `agnosticInvocable` lets you make raw API calls to any Stripe endpoint, using the app's pre-built authentication and connectivity layer. You get Stripe API access without managing OAuth, secrets, or named credentials yourself.

This is where grounding context pays off at the architecture level, not just the implementation level. When the agent knows both patterns — what the out-of-the-box actions cover and how to fall back to agnostic invocables — it can help you make the right call: use `v02_CreateCustomers` directly, build a helper Apex class using the models reference, or drop down to an agnostic invocable for a raw API call when the packaged surface doesn't cover your use case.

These grounding files aren't static: they should get sharper with every implementation. When you find an edge case, or discover a constraint the hard way, tell the agent:

```
$ claude
> Record this learning in the context files 
```

The agent updates the grounding rules, and your next iteration or your next project starts smarter. The context becomes a living knowledge base that compounds with every implementation.

That's also why the repo is open source. Your local refinements can become shared patterns. Submit a PR, and the next developer who loads the context benefits from what you learned.

### Getting started

Here’s how to get started today:

```
git clone https://github.com/stripe/stripe-salesforce-ai-context.git
cd stripe-salesforce-ai-context
```

- You'll need the [Stripe App for Salesforce](https://github.com/stripe/stripe-salesforce-ai-context/blob/main/PREREQUISITES.md) installed in your org.
- No org yet? Use the [scratch org quickstart](https://github.com/stripe/stripe-salesforce-ai-context/blob/main/setup/scratch-org-quickstart.md).

Point your AI coding tool at the context - Claude Code reads `CLAUDE.md` automatically, Cursor picks up files from `.cursor/rules/`, and other CLI tools can reference the `context/` directory in their system prompt.

Try:

```
$ claude
> Generate a Flow that creates a Payment Link when an Opportunity closes and write the URL back to the record.
```

For an end-to-end workflow, pair the grounding context with [Salesforce MCP](https://github.com/salesforcecli/mcp) and [Stripe MCP](https://github.com/stripe/agent-toolkit) servers. Your agent can generate, deploy, and verify without leaving the terminal.

The grounding files are versioned and maintained against Stripe App releases. Remember to review before you deploy since these are reference implementations, not production-ready code.
