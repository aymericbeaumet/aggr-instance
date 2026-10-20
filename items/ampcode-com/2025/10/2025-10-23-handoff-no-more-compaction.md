---
title: Handoff (No More Compaction)
link: https://ampcode.com/news/handoff
source: ampcode-com
published: 2025-10-23T00:00:00Z
updated: 2025-10-23T00:00:00Z
first_seen: 2026-10-04T12:50:30.804479567Z
summary: 'We have removed compaction from Amp and replaced it with something that we think works a lot better: Handoff. Compaction — which you know as /compact from the Amp CLI, or the Compact or New Thread With Summary buttons in the editor extensions — always had downsides. It''s lossy, for one. Every time you compact a thread, what''s in the context window gets replaced with a summary. Whether that summary contains exactly what you think it should is up to the agent. Claude Code, for example, fixes that by allowing users to tell the model what to focus on when summarizing. But even then: compaction, we found, encourages long, meandering threads, in which you just compact once you run out of context window, stacking summary on top of summary. What we want to encourage are focused threads, because we think that''s how agents yield the best results. So we replaced it with Handoff. Handoff is a new way to take existing context and move it into a new thread. Instead of summarizing a thread, you''re extracting from it what matters for your next task. Handoff lets you specify your goal for the new thread. Amp then analyzes the current thread and generates a prompt to start the new thread, along with a list of relevant files. Here are some examples for how you would use it in the Amp CLI: /handoff now implement this for teams as well, not just individual users /handoff execute phase one of the created plan /handoff check the rest of the codebase and find other places that need this fix The generated prompt will then appear as a draft in the new thread so you can still review and edit it before sending. You can rewrite the instructions to ensure the new thread starts exactly as you intend, with no unintended loss of context. In the editor extensions, you can access handoff in the token usage hover: If you want a deeper explanation of how Handoff came to be, here are Nicolay and Quinn:'
content: extracted
html: 2025-10-23-handoff-no-more-compaction.html
remote_preview:
  url: https://static.ampcode.com/news/handoff-vscode.png?bust=cache2
  alt: Handoff command in VS Code
---

We have removed compaction from Amp and replaced it with something that we think works a lot better: Handoff.

Compaction — which you know as `/compact` from the Amp CLI, or the *Compact* or *New Thread With Summary* buttons in the editor extensions — always had downsides. It's lossy, for one. Every time you compact a thread, what's in the context window gets replaced with a summary. Whether that summary contains exactly what you think it should is up to the agent. Claude Code, for example, fixes that by allowing users to tell the model what to focus on when summarizing.

But even then: compaction, we found, encourages long, meandering threads, in which you just compact once you run out of context window, stacking summary on top of summary.

What we want to encourage are focused threads, because we think that's how agents yield the best results.

So we replaced it with Handoff.

Handoff is a new way to take existing context and move it into a new thread. Instead of summarizing a thread, you're extracting from it what matters for your next task.

Handoff lets you specify your goal for the new thread. Amp then analyzes the current thread and generates a prompt to start the new thread, along with a list of relevant files. Here are some examples for how you would use it in the Amp CLI:

- `/handoff now implement this for teams as well, not just individual users`
- `/handoff execute phase one of the created plan`
- `/handoff check the rest of the codebase and find other places that need this fix`

The generated prompt will then appear as a draft in the new thread so you can still review and edit it before sending. You can rewrite the instructions to ensure the new thread starts exactly as you intend, with no unintended loss of context.

In the editor extensions, you can access handoff in the token usage hover:

![Handoff command in VS Code](https://static.ampcode.com/news/handoff-vscode.png?bust=cache2)

If you want a deeper explanation of how Handoff came to be, here are Nicolay and Quinn:
