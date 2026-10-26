---
title: PaperPilot – Zero-Cloud, Pure-Rust PDF Engine with WASM and MCP
link: https://github.com/KrushnaVardhanReddy/PaperPilot
source: hnrss-org
published: 2026-10-26T03:29:05Z
updated: 2026-10-26T03:29:05Z
first_seen: 2026-10-26T14:07:38.108821132Z
authors:
- vardhan1237
content: extracted
html: 2026-10-26-paperpilot-zero-cloud-pure-rust-pdf-engine-with-wasm-and-mcp.html
remote_preview:
  url: https://opengraph.githubassets.com/9f03c1e653977104e4d330292816716de263f535b48ef3e2ae032f9f4a4b775f/KrushnaVardhanReddy/PaperPilot
  alt: Your local AI copilot for documents. Contribute to KrushnaVardhanReddy/PaperPilot development by creating an account on GitHub.
---

**PDF tools for your AI agent — and for you. Pure Rust. Runs locally. Files never leave your machine.**

[![PaperPilot demo](https://github.com/KrushnaVardhanReddy/PaperPilot/raw/main/docs/assets/demo.gif)](https://github.com/KrushnaVardhanReddy/PaperPilot/blob/main/docs/assets/demo.gif)

[**Try it in your browser →**](https://usepaperpilot.com) · [Download](https://github.com/KrushnaVardhanReddy/PaperPilot/releases) · [Roadmap](https://github.com/KrushnaVardhanReddy/PaperPilot/blob/main/ROADMAP.md)

PaperPilot is a PDF and document toolkit written in Rust. The same engine runs as:

| Surface            | Use it for                                                                                                                            |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------- |
| **MCP server**     | Give Codex, Claude Code, Kimi Code, Pi, Claude Desktop, Cursor, or any MCP client the ability to merge, split, OCR, and compress PDFs |
| **CLI**            | Batch and scripted workflows                                                                                                          |
| **Browser (WASM)** | Zero-install tools; files are processed in browser memory, nothing is uploaded                                                        |
| **Desktop app**    | Tauri + Svelte GUI with viewer, markup, and diff                                                                                      |
| **Local REST API** | Call it from Python, Node, or n8n at `localhost:7823`                                                                                 |

No Chromium, no C++ runtime, no cloud calls.

* * *

## Quick start

### 1\. Use it with your coding agent (MCP)

Codex, Claude Code, Kimi Code, and current Pi releases connect to the same local stdio server. See the [coding agent setup guide](https://github.com/KrushnaVardhanReddy/PaperPilot/blob/main/docs/coding-agents.md) for each client's configuration and verification steps. Builds from this source can also print ready-to-merge configuration:

```shell
paperpilot-mcp --client-config codex
# Also: claude-code, kimi-code, pi, claude-desktop, cursor
```

Download `paperpilot-mcp` from [Releases](https://github.com/KrushnaVardhanReddy/PaperPilot/releases), then add it to `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "paperpilot": {
      "command": "/path/to/paperpilot-mcp"
    }
  }
}
```

Restart Claude and try: *"Merge these two PDFs and compress the result."*

### 2\. Use the CLI

```shell
# Merge
paperpilot merge --inputs doc1.pdf doc2.pdf --output merged.pdf

# Compress
paperpilot compress --input in.pdf --quality 75 --output compressed.pdf

# Rotate pages 1, 3–5 by 90°
paperpilot rotate --input in.pdf --pages 1,3-5 --angle 90 --output out.pdf
```

### 3\. Use it in the browser

Go to [usepaperpilot.com](https://usepaperpilot.com). Open your browser's Network tab while you work — you'll see no file uploads.

### 4\. Build from source

```shell
git clone https://github.com/KrushnaVardhanReddy/PaperPilot.git
cd PaperPilot
cargo build --release -p paperpilot-cli -p paperpilot-mcp -p paperpilot-gateway
```

Desktop app:

```shell
cd apps/desktop && pnpm install && pnpm tauri dev
```

Requires Rust 1.75+, Node.js, and pnpm.

* * *

## What it can do today (v0.6)

**Organize:** merge, split, rotate, delete pages, reorder pages, extract pages, booklet imposition, N-up grid **Optimize:** compress **Extract:** plain text, OCR (pure-Rust PaddleOCR via `rten`) **Security:** watermark, validate signatures, remove signatures, PII scan, deep sanitize **Office data:** Excel → JSON, Excel → CSV

Coming next (see [ROADMAP.md](https://github.com/KrushnaVardhanReddy/PaperPilot/blob/main/ROADMAP.md)):

- **v0.7:** PDF ↔ Markdown / HTML / JSON
- **v0.8:** PDF → Word / Excel / PowerPoint
- **v0.9:** encrypt, decrypt, sign, redact, forms, annotations
- **v1.0:** render, flatten, repair, compare, PDF/A, Bates numbering

* * *

## Why another PDF tool?

- **Private by default.** Everything runs locally or in your browser tab.
- **Agent-ready.** MCP support is first-class, not an afterthought.
- **One engine, many surfaces.** CLI, MCP, REST, WASM, and desktop share the same Rust core, so behavior is consistent.
- **Small and dependency-light.** Pure Rust, built on [`lopdf`](https://github.com/J-F-Liu/lopdf), [`pdf_oxide`](https://crates.io/crates/pdf_oxide), [`rten`](https://github.com/robertknight/rten), and friends.

## Benchmarks

Run `cargo bench --workspace` to reproduce. Methodology and results: [SYSTEM\_BENCHMARK\_REPORT.md](https://github.com/KrushnaVardhanReddy/PaperPilot/blob/main/SYSTEM_BENCHMARK_REPORT.md).

* * *

## Architecture

```
Desktop (Tauri) · CLI · REST (Axum) · MCP (stdio) · WASM (browser / edge)
                              │
                       Rust core workspace
   paperpilot-core · paperpilot-pdf · paperpilot-nlp · paperpilot-gateway
   paperpilot-mcp · paperpilot-wasm · paperpilot-storage
```

## Embed in your own site

```text-html-basic
<script src="https://cdn.usepaperpilot.com/embed.js" data-tools="merge,compress,ocr"></script>
<div id="paperpilot-widget"></div>
```

## Local REST API

```shell
paperpilot serve --port 7823
curl -X POST http://localhost:7823/api/v1/pdf/merge \
  -H "Content-Type: application/json" \
  -d '{"inputs": ["a.pdf", "b.pdf"], "output": "out.pdf"}'
```

Swagger docs at `http://localhost:7823/docs`.

* * *

## Contributing

Issues and PRs are welcome. Good first contributions: bug reports with a sample PDF, new tool requests, and docs fixes.

## License

Community Edition (this repo): [Apache 2.0](https://github.com/KrushnaVardhanReddy/PaperPilot/blob/main/LICENSE). It includes the CLI, MCP server, REST gateway, WASM engine, and desktop app.

An Enterprise Edition (SSO, RBAC, audit logs, DLP) is developed separately.
