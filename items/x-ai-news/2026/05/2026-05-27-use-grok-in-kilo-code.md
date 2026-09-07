---
title: Use Grok in Kilo Code
link: https://x.ai/news/grok-kilocode
source: x-ai-news
published: 2026-05-27T00:00:00Z
first_seen: 2026-09-07T17:03:44.343711062Z
summary: Use your SuperGrok or X Premium+ subscription inside Kilo Code, the open-source agentic coding platform.
content: extracted
html: 2026-05-27-use-grok-in-kilo-code.html
preview:
  file: 2026-05-27-use-grok-in-kilo-code.preview-30cadbb0ed64.webp
  width: 256
  height: 134
  color: '#5f5038'
images:
- source: https://media.x.ai/v1/website/grok-kilocode-4e041462.png
  original:
    file: 2026-05-27-use-grok-in-kilo-code.image-8d81d63cee10.png
    width: 1200
    height: 630
  color: '#070706'
---

Starting today, you can use your Grok subscription directly inside [Kilo Code](https://kilo.ai/).

Kilo Code is the open-source agentic engineering platform for VS Code, JetBrains IDEs, and the terminal. It offers specialized modes for planning, coding, debugging, and orchestration, along with powerful tool use, browser automation, MCP extensibility, and support for 500+ models. Kilo is designed for real software engineering workflows and agentic use cases.

With your X Premium+ or SuperGrok subscription, connect your Grok account to use the latest Grok models — including Grok Build for agentic coding — inside Kilo Code with no separate API key.

### [Setup](https://x.ai/news/grok-kilocode#setup)

Kilo Code offers IDE extensions, a CLI, and a web surface. To connect your xAI account, follow the instructions below:

**VS Code:**

1. Install [Kilo Code](https://marketplace.visualstudio.com/items?itemName=kilocode.Kilo-Code) from the VS Code Marketplace.
2. Open **Settings** (gear icon) and go to the **Providers** tab.
3. Click **Show more providers**, then search for or select **xAI**.
4. Choose the **xAI Grok OAuth (SuperGrok Subscription)** sign-in option and complete the OAuth flow in your browser.

For headless or remote environments (VPS, SSH, Docker, WSL), choose **xAI Grok OAuth (Headless / Remote / VPS)** instead.

**CLI:**

bash

Run `kilo` in your project directory and use the `/connect` command to add xAI.

For more details, see the [Kilo Code xAI provider documentation](https://kilo.ai/docs/ai-providers/xai).

More open-source agents and integrations are coming soon.
