---
title: 'Show HN: Use your OpenAI subscription to create audiobooks'
link: https://pdftomp3.com/connect-ai/
source: hnrss-org
published: 2026-10-13T10:08:34Z
updated: 2026-10-13T10:08:34Z
first_seen: 2026-10-13T18:08:43.541012300Z
authors:
- janpmz
content: extracted
html: 2026-10-13-show-hn-use-your-openai-subscription-to-create-audiobooks.html
extra:
  comments_url: https://news.ycombinator.com/item?id=50069337
  num_comments: 69
  points: 136
---

Use the AI you already pay for

**$0** to PDF TO MP3

Use your subscription to process the documents. The speech generation is billed by Google or OpenAI.

## Choose how to connect

## 01Connect your AI

In Codex, open Settings → Plugins → Add MCP server.

Name it PDFTOMP3-MCP. Choose Streamable HTTP and paste this server address.

Save, then choose Authenticate and connect your PDF TO MP3 account.

### Install in Codex

Open **Plugins → Add → Add a marketplace**.

Paste this URL into **Source**:

Click **Add marketplace**, install **PDFTOMP3-Plugin**, and connect your account. Then start a new chat.

Run commands instead

```
codex plugin marketplace add janpmz/pdftomp3-plugin
codex plugin add pdftomp3-plugin@pdftomp3-plugin
```

## 02Add your speech key

This key pays for speech only.

## 03Send your first document

Attach a PDF in your AI chat and ask:

“Use the PDF TO MP3 MCP to turn this into an audiobook.”

Your subscription will be used for:

- document processing (via MCP) already paid by your subscription

Your API key will be used for:

- generating speech OpenAI charges per usage

Only want to connect your API key? [Use my API key](https://pdftomp3.com/use-my-key/)
