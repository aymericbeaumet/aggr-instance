---
title: How my context engineering changed for Gemini 4 Neon
link: https://www.philschmid.de/gemini-4-neon-context-engineering
source: philschmid-de
published: 2026-11-25T00:00:00Z
updated: 2026-11-25T00:00:00Z
first_seen: 2026-11-25T20:07:11.161777435Z
summary: After weeks of running evals and building daily with Gemini 4 Neon, I realized that many of the guardrails, skills, and rules I wrote for older models were slowing it down. Here are 5 things I changed.
content: extracted
html: 2026-11-25-how-my-context-engineering-changed-for-gemini-4-neon.html
remote_preview:
  url: https://www.philschmid.de/static/blog/gemini-4-neon-context-engineering/00-og-thumbnail-light.png
  alt: How my context engineering changed for Gemini 4 Neon
---

When you use a coding agent every day, your prompt is only a small part of what the model sees. System instructions, repo files, tool definitions, skills, and past turns fill the rest of the context window. Unlike a single prompt, those persistent files run across every future request, so overly specific rules eventually clash with what you actually ask for.

After weeks of running evals and building daily with Gemini 4 Neon, I realized that many of the guardrails, skills, and rules I wrote for older models were slowing it down or making it fail edge cases it can solve on its own.

Moving to Gemini 4 Neon meant taking a step back. Here are 5 things I changed.

## Remove strict rules

My system instructions and rules grew over time. I added one rule when an older model wrote noisy comments, then another to block edits I did not ask for. Later, my user rules, project rules, and current request started to contradict each other. For example, a blanket ban on comments clashes the moment I ask the agent to document a public function.

When instructions conflict, Gemini 4 Neon spends extra thinking tokens trying to resolve them and often follows the older rule instead of my current request. I deleted rigid code-style bans and replaced them with simple judgment cues, such as asking the model to match the surrounding codebase style, naming, and comment density. That cut wasted thinking and raised accuracy.

![Legacy vs lean Gemini 4 Neon stack](https://www.philschmid.de/static/blog/gemini-4-neon-context-engineering/01-context-trim.png)

Now my user-level GEMINI.md fits on one screen. I keep only a few habits that apply to every session: hard safety limits around approvals, source verification, and long jobs, plus short formatting preferences for how the agent writes replies to me. Everything task-specific lives in skills instead.

Markdown

```markdown
<!-- Excerpt from my user-level GEMINI.md -->
- Draft locally and show diffs or commands before pushing, deploying, opening PRs, or editing shared docs.
- Verify claims, numbers, quotes, and URLs against primary sources or raw outputs instead of guessing.
- Set a monitoring timer with progress updates for long evals or background jobs.
- Front-load answers in Simplified Technical English using short sentences, active voice, and honest phrasing.
- Avoid inline backticks, bold text, em dashes, horizontal dividers, and parenthetical clutter in prose.
```

In my tests, bloated ban lists did not always help older models stay read-only during debugging. They just added conflicting instructions and extra thinking tokens. With Gemini 4 Neon, removing long lists of bans keeps edits cleanly within scope.

## State tool rules once in clear interfaces

In previous model generations, I had to explain tools twice. I wrote a description inside the tool definition, then repeated rules on how and when to call them inside the system instructions. I also added few-shot examples to show valid arguments.

```text
# Previous setup: duplicated rules in system instructions + tool descriptions
# System instruction:
#   - For code edits, always call edit_file on small sections instead of rewriting whole files.
#   - Always update your plan with update_plan before editing, keeping one step in_progress.
#
# Tool description (repeated again):
#   - edit_file: "Edit a file. Prefer small targeted edits over full rewrites."
#   - update_plan: "Track steps. Keep one item in_progress. Example: [...]"
```

With Gemini 4 Neon, I removed the repeated tool rules from my system instructions and deleted the few-shot examples. Few-shot examples work on common inputs, but they narrow how the model handles edge cases. When a task differs from the example, the model copies extra fields or the wrong argument shape from the example and sends invalid calls.

Instead, I put the usage rule once inside an expressive interface. If you build custom agents on the Gemini API, put clear parameter types, enums, and required fields directly into the tool JSON schema:

JSON

```json
{
  "name": "update_plan",
  "description": "Update the task checklist. Keep only one step in_progress at a time.",
  "parameters": {
    "type": "object",
    "properties": {
      "steps": {
        "type": "array",
        "items": {
          "type": "object",
          "properties": {
            "title": { "type": "string" },
            "status": {
              "type": "string",
              "enum": ["pending", "in_progress", "done", "skipped"]
            }
          },
          "required": ["title", "status"],
          "additionalProperties": false
        }
      }
    },
    "required": ["steps"],
    "additionalProperties": false
  }
}
```

If you use a coding agent every day, apply the same idea to repo helper scripts and CLI tools. Clear flags, strict options, and direct error messages guide the model better than paragraphs of usage examples.

Keep the tool surface small and the outputs bounded too. Removing overlapping search tools and writing long command logs to a file instead of dumping huge stdout blocks into context keeps every turn focused.

Gemini 4 Neon follows a rule written once inside the tool schema even in long sessions. Dropping duplicate system rules cuts context clutter without losing tool discipline.

## Load skills only when needed

I deleted almost every project-level AGENTS.md and GEMINI.md I had. Most of those files repeated directory layouts the model can see by reading the repo, or turned into append-only memory logs that went stale.

Now I keep a project instruction file short: a brief summary of what the repo does, plus real gotchas the model cannot infer from code, such as a non-standard test command or a single file where all shared types must live. I moved domain workflows like long-form editing, UI guidelines, API reviews, and CLI guides into skills with a 1-line description in an index.

![On-demand skill and tool loading](https://www.philschmid.de/static/blog/gemini-4-neon-context-engineering/02-progressive-skills.png)

Keep skills lightweight too. Use them for team conventions and required checks instead of scripting every edge case. For longer skills, split details into a small tree of linked files so the agent reads only the part it needs.

Skills pay off most on fast-moving CLIs, SDKs, and internal APIs. Loading a current guide on demand stops the model from guessing outdated flags or deprecated endpoints, which cuts trial-and-error turns and lowers total token usage even on tasks that load the skill.

Gemini 4 Neon finds and loads skills and deferred tools reliably. It reads only the files and tool definitions that match the task and skips the rest, cutting unused turn-1 context with almost no false triggers.

## Verify and guide with images and video

For normal backend code edits, a short text request works fine. When UI layout, diagrams, or motion matter, text descriptions leave too much room for error.

With Gemini 4 Neon, I now use images and video much more as both references and verification checks. Instead of writing two paragraphs of CSS rules for a page or diagram, I drop a reference screenshot or short screen recording to show the exact style and spacing I want.

Then I make the agent check its own output visually: render the page, diagram, or video frames, open the actual PNG or clip, and inspect it before saying it's done. Looking at one rendered image catches clipped text, wrapped labels, and broken alignment that text logs never show.

## Hand off context instead of compacting

Even with a 1M token context window, long coding sessions still get noisy, slow down, and eventually need compaction. Compaction always hurts. When an agent summarizes a session mid-task, it breaks the prompt cache, drops line-level details, and forces the model to spend its next turns re-reading files it already opened.

![Context retention and phase handoffs](https://www.philschmid.de/static/blog/gemini-4-neon-context-engineering/03-context-lifecycle.png)

Because compaction is lossy, I now split work deliberately instead of letting one thread grow forever. When a task changes phase, I ask the agent to write a short handoff file with the goal, verified decisions, and exact file paths, then start a new session from that file.

When I notice mid-session that I want to work on a second thing in parallel, I fork the session right there instead of pushing both tasks through one thread sequentially. For example, after running an eval suite and checking failure logs in one thread, I write a handoff file and start a fresh session for the write-up. At the same time, I fork the eval thread to generate and inspect the visuals from the loaded results.

If you build your own agent loop and keep automatic compaction as a fallback, do not trigger it early. Around 500k tokens is a good target for me before compacting. Letting the window grow avoids early cache resets and file re-reads, and pinning active file paths plus verified decisions inside the summary preserves what the model needs to keep moving.

## Putting your setup together

When you update an agent setup for Gemini 4 Neon, simplify each context layer one by one:

- User-level rules: keep your global GEMINI.md short. Stick to universal safety habits and reply preferences, and replace code-style bans with judgment cues.
- Tool and script interfaces: if you build custom agents, put each tool rule once inside its JSON schema with typed parameters and enums, and delete few-shot examples and repeated system rules. For daily coding agents, design clear CLI flags and helper scripts the same way.
- Project files and skills: delete bloated project-level AGENTS.md and GEMINI.md files, keep only genuine codebase gotchas, and move domain workflows into lightweight skills split into linked files when they grow long.
- Visual references and checks: pass reference screenshots or short screen recordings for visual work, and make the agent inspect its rendered outputs directly before finishing.
- Session handoffs: avoid mid-task compaction by writing a short handoff file between phases or forking the session when a second track branches off.

Most long agent prompts are old workarounds for weaker models. Remove conflicting instructions and duplicated tool rules so Gemini 4 Neon spends its thinking tokens on the actual task.

Thanks for reading!
