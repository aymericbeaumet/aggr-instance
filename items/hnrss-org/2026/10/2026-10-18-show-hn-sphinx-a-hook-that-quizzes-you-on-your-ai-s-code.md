---
title: 'Show HN: Sphinx – A hook that quizzes you on your AI''s code before shipping'
link: https://sphinx-six-theta.vercel.app/
source: hnrss-org
published: 2026-10-18T04:33:49Z
updated: 2026-10-18T04:33:49Z
first_seen: 2026-10-18T11:38:20.507632497Z
authors:
- critiquebrowser
summary: 'FOSS, Basically it plugs into your existing coding agent workflow and every time you push to prod it''ll throw up a questionnaire that checks if you understand how the code you''re pushing works. Comments URL: https://news.ycombinator.com/item?id=50137950 Points: 146 # Comments: 43'
content: extracted
html: 2026-10-18-show-hn-sphinx-a-hook-that-quizzes-you-on-your-ai-s-code.html
---

- Free
- [FOSS](https://github.com/parthh01/sphinx)

> “You can outsource your thinking,\
>  but not your *understanding*.”

— [Sun Tzu](https://x.com/yacineMTB/status/2018886083120153046?s=20)

Sphinx is a Git hook for agent-assisted development. Before your agent’s code ships, it asks you a question about what that code actually does.

`npm install -g sphinx-understand`

Built to run on minimal tokens, from your existing coding agent, OpenRouter, or any OpenAI-compatible endpoint.

$ git push origin main

sphinx · understanding check for main

11 files changed · +5,012 −3,147

Sphinx

Your webhook handler now saves the event ID *before* calling `charge()`. Stripe times out and sends the same event again. What happens on the retry?

You

It runs twice, so the customer is charged twice.

Sphinx

Not quite. The retry finds the saved ID and returns 200 without charging. But that ID is stored before the charge succeeds. If `charge()` fails, what does the next retry do?

You

It skips the charge, so the customer is never billed.

✓ Understood. Pushing to origin/main.

An illustrative exchange. Real questions come from your diff.

1. 01

   ### You push.

   Or your agent does. Git pauses on the branches you protect.

2. 02

   ### Sphinx asks.

   A reviewer that never saw your chat reads the diff and asks what the change does when things go wrong.

3. 03

   ### You answer.

   Get it wrong and you learn why, then try again, usually with a new scenario. Get it right and the push continues.

![A git push waits while the Sphinx questionnaire asks what the change does, the answer passes, and the push continues.](https://sphinx-six-theta.vercel.app/demo.gif)\
The real questionnaire: a push waits, you answer, Git continues.

Works with **Codex, Claude Code, Cursor, OpenCode,** and **Pi**, or plain Git. Your answers never reach the coding agent, and `.env` files and keys are never opened. In a hurry? `sphinx yolo` lets pushes through, and every skip is logged.

## Start in a minute.

```
# Node 22.19+ · macOS, Linux, or WSL
npm install -g sphinx-understand
sphinx install

# then push as usual
git push
```

`sphinx install` connects Git and your coding agents and asks which model should review. [Read the docs](https://github.com/parthh01/sphinx#readme) for everything else.
