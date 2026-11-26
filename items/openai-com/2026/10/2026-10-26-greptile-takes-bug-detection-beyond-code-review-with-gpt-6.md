---
title: Greptile takes bug detection beyond code review with GPT-6 Astra
link: https://openai.com/index/greptile-code-review-codex-astra
source: openai-com
published: 2026-10-26T22:00:00Z
updated: 2026-10-26T22:00:00Z
first_seen: 2026-10-26T23:45:44.074426996Z
labels:
- startup
summary: How Greptile uses GPT-6 Astra to test user workflows, Daybreak to strengthen security, and Codex to reduce rework.
content: extracted
html: 2026-10-26-greptile-takes-bug-detection-beyond-code-review-with-gpt-6.html
---

[Greptile](https://www.greptile.com/) builds AI agents that review and test code changes with full context of the codebase. “We are in the business of reducing bugs and making the world’s software more reliable,” says Co-founder and CEO Daksh Gupta.

For Daksh, that mission requires testing an application’s behavior as well as reading its code, and GPT‑6 Astra’s computer use gives Greptile a way to automate testing at scale.

## Testing applications at scale with GPT‑6 Astra

Validating a pull request involves checking whether the code is logically correct, does what the developer intended, is secure, and avoids breaking existing functionality. For some of that testing, you need to actually run the application because reading the code alone might not reveal the problem.

Daksh gives the example of a button that disappears when a website is viewed on an iPhone. With GPT‑6 Astra, Greptile can start a development server for a pull request, resize the browser to an iPhone-sized viewport, and simulate a user moving through the application. The model’s visual understanding lets it inspect the screen and spot the hidden button.

“A QA firm testing all of your changes is just impractically slow considering how fast we’re able to ship code with these models,” he says. With GPT‑6 Astra, Greptile can automate this kind of application testing at scale.

## Strengthening customers’ defenses with Daybreak

Daksh says more powerful models also make it easier for attackers to exploit applications, putting companies at risk, even those that previously would not have been typical targets.

“We want to make it very easy for even a startup to secure their systems,” says Daksh. “With Daybreak, we’re able to do this in a way that is very safe, and we can ensure that our customers have more sophisticated defense mechanisms than the adversaries have offense mechanisms.”

## Reducing bugs and rework with Codex

Greptile’s Knowledge Base indexes a company’s codebase and documents how it works in a form people and agents can read. The company makes that documentation available to Codex through a Model Context Protocol (MCP) server.

This allows Codex to write code that follows the customer’s existing patterns, and thousands of customers have connected Codex to Greptile’s knowledge base, according to Daksh.

To measure the impact, the team tracks how often AI-generated code is reverted. “When we provide agents with context, fewer reverts happen and customers save time and save inference,” Daksh says. Customers can put those savings toward new work instead of reworking the same changes.

Greptile also uses Codex to build its own software and agents. “We are massive users of Codex,” Daksh says. “We have enormous amounts of usage across every single function, engineering in particular.”

## Looking ahead to faster development with Ultrafast

For Greptile, Daksh explains, the only real bottleneck is how quickly the team can turn ideas into working software. He says model intelligence is already sufficient for many tasks, so he sees Ultrafast as a way to speed up development.

His longer-term goal is to make changing and shipping critical enterprise software as easy as shipping a side project, with OpenAI helping Greptile build the validation that requires.
