---
title: 'Show HN: ExitPath – Open-source ways out of Slack, Notion and Microsoft 365'
link: https://exitpath-ruddy.vercel.app/
source: hnrss-org
published: 2026-11-18T16:04:43Z
updated: 2026-11-18T16:04:43Z
first_seen: 2026-11-19T00:14:19.077761094Z
authors:
- zerocool0101
content: extracted
html: 2026-11-18-show-hn-exitpath-open-source-ways-out-of-slack-notion-and.html
remote_preview:
  url: https://exitpath-ruddy.vercel.app/og.png
  alt: 'ExitPath: find your way out. Open-source exits from big-platform lock-in.'
---

## A map of the exits, with the distances marked honestly

ExitPath is not another software suite. It is a guide to the open-source projects that already exist: which of them can really replace the tool you depend on, what the move costs, and the order in which to do it.

The catalog currently covers proprietary tools, alternatives and migration paths between them.

Every path gets a score from 0 to 100. It is built from five ratings on a 1–5 scale, each recorded in the catalog where anyone can read it and dispute it. Four describe the migration itself and one describes you.

The score is the weighted average of those factors and the single hardest of the first four. That second part matters: a migration is as hard as its hardest step, and an easy import does not make up for a mail server you have to keep alive. Team size is left out of the “hardest” part so that head-count alone never dominates.

Each path records how many hours one person needs to move their data and settle in. That figure is multiplied by team size, and server setup time is added once.

The multiplier grows more slowly than the head-count because one person does most of the bulk work. Self-hosting adds the alternative’s own setup time; a managed service adds . Estimates are hands-on hours, shown as days of six focused hours once they get large. They are a planning aid, not a quote.

- Ratings are opinions, not measurements. The first version of the catalog was compiled with an AI assistant from each project’s public documentation, and not every entry has yet been checked by someone who runs the software. Each entry shows the month it was last reviewed, and corrections from people with first-hand experience are the most useful thing you can send.
- Projects change their licences, pricing and features, sometimes abruptly. Check the project’s own documentation before you commit a team to it.
- The catalog is deliberately small. It aims to list the realistic replacements, not everything that exists.
- Running your own services means running your own backups, updates and security. The time estimates cover getting there, not staying there.

Every tool, alternative and checklist is one file under `catalog/`. Change a file, run `npm run validate` and open a pull request. Entries need honest weaknesses and a source for anything surprising.

Source code and catalog

You can export everything to a file, bring it back in on another device, or erase it. Open the options

A link to a plan carries the plan itself in the address, so sharing one uploads nothing either. Guided migrations
