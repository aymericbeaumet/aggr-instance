---
title: Frequently Ignored Feedback
link: https://ampcode.com/fif
source: ampcode-com
published: 2025-04-09T00:00:00Z
updated: 2025-04-09T00:00:00Z
first_seen: 2026-10-04T12:50:30.804479567Z
summary: Our responses to some common feedback that we are intentionally not acting on.
content: extracted
html: 2025-04-09-frequently-ignored-feedback.html
---

Our responses to common feedback that we intentionally do not act on.

## “Make it so the user can switch models.”

A model selector asks you to know which model is best for a task. The dial asks one question: how hard is this task? Its [agent modes](https://ampcode.com/manual#agent-modes) trade capability against cost. Amp chooses the models, prompts, tools, and reasoning effort behind each mode, and changes that wiring as models improve.

The dial removes knobs from the default experience, not from Amp. [Agent mode plugins](https://ampcode.com/manual#define-a-custom-agent-mode) let you define custom modes with your model, your prompt, and your tools, and they show up alongside Amp’s built-in modes.

## “I don't want it to edit files directly.”

Requiring edit-by-edit approval traps you in a [local maximum](https://x.com/beyang/status/1924507302956302762) by impeding the agentic feedback loop. You’re not giving the agent a chance to iterate on its first draft through review, diagnostics, compiler output, and test execution. If you find that the agent rarely produces good enough code on its own, instead of trying to “micro-manage” it, we recommend writing [more detailed prompts](https://ampcode.com/how-i-use-amp) and improving your [`AGENTS.md` files](https://ampcode.com/manual#AGENTS.md).

Shared by default is the right default for workspaces. It results in more threads being shared, reduces the expectation that every thread must be “perfect” or “not dumb,” and makes collaboration feel more like sharing Google Docs edit history or Git branches.

Thread visibility is configurable. Workspace admins can choose the default for new threads, and CLI users can configure defaults globally or per repository. Outside a workspace, threads are private by default. See [Thread Visibility](https://ampcode.com/manual#thread-visibility).

## “Add more safety controls or configurability over shell commands that it runs.”

Amp does not try to protect against a malicious actor prompt-injecting something that causes the

`Bash` tool to execute malicious code.

## “Where is the .ampignore file?”

An ignore file is actively harmful: hiding files from the model encourages it to find creative workarounds, for example by using the Bash tool, wasting tokens.

Such file restrictions can be trivially circumvented with the Bash tool, and thus give only a false sense of security.

Amp already redacts known secrets and provides an [extensive permissions system](https://ampcode.com/manual#permissions) if you need full control over its tool use.

## “Make it so I can ask the model what model it is.”

Consult the UI, not the model. LLMs often self-report their model name incorrectly. Amp shows the models used by each mode in the mode dial and on the [Models](https://ampcode.com/modes) page. Forcing the model to identify itself spends system prompt tokens, degrading Amp’s performance and wasting context you could otherwise use.

## “Let me use any model provider or deployment target.”

Amp can use your own API keys for certain model providers and your ChatGPT Plus/Pro subscription for some OpenAI inference. Amp does not support Amazon Bedrock, arbitrary model providers, or self-hosted deployments.
