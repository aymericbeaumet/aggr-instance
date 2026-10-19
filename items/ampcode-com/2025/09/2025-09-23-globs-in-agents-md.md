---
title: Globs in AGENTS.md
link: https://ampcode.com/news/globs-in-AGENTS.md
source: ampcode-com
published: 2025-09-23T00:00:00Z
updated: 2025-09-23T00:00:00Z
first_seen: 2026-10-04T12:50:30.804479567Z
summary: 'Your AGENTS.md files can now include other context files dynamically, with glob mentions and filters, so you can give more granular guidance to the agent. For example, to apply language-specific coding rules: Put See @docs/*.md anywhere in your AGENTS.md file. Create a file docs/typescript-conventions.md with: --- globs: - ''**/*.ts'' --- Follow these TypeScript conventions: - Never use the `any` type - ... Repeat for other languages. There''s nothing special about @docs/*.md; you could instead mention @.agent/rules/*.md or @.cursor/rules/*.mdc or anywhere else you want these rules to live. These @-mentioned files with globs will only be included if Amp has read a file matching any of the globs (in the example above, any TypeScript file). Use /agent-files (CLI) or hover over X% of 968k (editor extension) to see the guidance files in use.'
content: extracted
html: 2025-09-23-globs-in-agents-md.html
---

Your [`AGENTS.md` files](https://ampcode.com/manual#AGENTS.md) can now include other context files dynamically, with glob mentions and filters, so you can give more granular guidance to the agent.

For example, to apply language-specific coding rules:

1. Put `See @docs/*.md` anywhere in your `AGENTS.md` file.

2. Create a file `docs/typescript-conventions.md` with:

   ```markdown
   ---
   globs:
     - '**/*.ts'
   ---

   Follow these TypeScript conventions:

   - Never use the `any` type
   - ...
   ```

3. Repeat for other languages.

There's nothing special about `@docs/*.md`; you could instead mention `@.agent/rules/*.md` or `@.cursor/rules/*.mdc` or anywhere else you want these rules to live.

These @-mentioned files with `globs` will only be included if Amp has read a file matching any of the globs (in the example above, any TypeScript file). Use `/agent-files` (CLI) or hover over `X% of 968k` (editor extension) to see the guidance files in use.
