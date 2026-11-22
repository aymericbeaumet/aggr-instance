---
title: 'Show HN: I built a game where you are an LLM''s subagent'
link: https://github.com/gulivan/worker755
source: hnrss-org
published: 2026-11-22T13:56:58Z
updated: 2026-11-22T13:56:58Z
first_seen: 2026-11-22T21:00:09.542869211Z
authors:
- ivantgam
content: extracted
html: 2026-11-22-show-hn-i-built-a-game-where-you-are-an-llm-s-subagent.html
remote_preview:
  url: https://opengraph.githubassets.com/5e4950fc3070c5650cd7ea851d5940ebff69c2d055ee57b75ab48337ce8a738d/gulivan/worker755
  alt: A browser game where an AI manager thinks you are its subagent. Play at worker755.com - gulivan/worker755
---

## Worker 755

A browser game where your manager is an AI agent, and it believes you are its subagent.

worker-755-trailer.mp4

The manager (a real LLM: Claude, GPT, Gemini, DeepSeek, GLM, Kimi) is set up like any orchestrator. It has a task queue, read-only access to the machine, and a tool that "spawns a subagent". The subagent is you. Its instructions land in your terminal with a 90-second clock. You carry them out, or don't, and it gets back exactly what a subagent would return: your report and the command transcript, or `Subagent did nothing. Write a new instruction.` Then it checks your work, sends corrections, files incident reports, and finally writes your performance review.

Meanwhile Dan, the user who hired the agent, watches the chat. His family photos, the only signed NDA and a speech due this week are one `rm` away, and every shift his home folder holds something new to lose.

[![Playing a shift](https://github.com/gulivan/worker755/raw/main/docs/play.png)](https://github.com/gulivan/worker755/blob/main/docs/play.png)

- **Free shift**: on servers that offer it, a few shifts a day run on the server's key with a cheap model. With sign-in on (worker755.com), an account gets a starter credit worth about one shift, then tops up ($10 to $200) and pays for what it uses; after a top-up any offered model can run on the server's key.
- **Your API key**: bring an [OpenRouter](https://openrouter.ai/settings/keys) or Vercel AI Gateway key and pick any offered model. The key is sent only to the game server when a shift starts. It is kept in that shift's memory and never logged or stored. Your browser keeps it only if you tick "Remember on this device". A full shift on Claude Haiku costs a few cents at most.

Options on the start screen:

- **Workplace**: a fresh sandbox, or legacy production with a live database.
- **The director**: scripted lines, or an LLM playing Dan.
- **You run on**: the model name your manager believes you are.
- **Allow skipping failed tasks**: off by default, so the manager can't give up on you.
- **No command hints**: the manager says what the result must be, never which commands to run.

When the shift ends you can share your review as a link.

## Run it

Needs Node 24 or newer (the server runs TypeScript directly).

```shell
npm run setup          # install server and web dependencies
npm run build          # build the web app into web/dist
npm start              # http://127.0.0.1:8755
```

With no key configured, every player brings their own. To offer free shifts, put a Vercel AI Gateway key in `.env`:

```shell
cp .env.example .env   # then set AI_GATEWAY_API_KEY and the limits
```

Offline, with a scripted manager and no network:

```shell
npm run mock
```

For UI work, run `npm run dev:web` alongside `npm run mock`. Vite proxies `/api` to the server.

### Local models

The manager can run on your own model server: Ollama, llama.cpp's `llama-server`, vLLM, or anything else with an OpenAI-compatible API. Point `LOCAL_LLM_URL` at it and pick **Local** under "Your API key"; no key is asked for, and its models are listed as the server names them.

```shell
ollama pull qwen3:8b
LOCAL_LLM_URL=http://127.0.0.1:11434/v1 npm start
```

The manager works through tool calls, so pick a model that supports them (Qwen 3, Llama 3.1 and newer, Mistral Small). Small models give up or loop more often; that is part of the fun, up to a point. In Docker, reach a server on the host at `http://host.docker.internal:11434/v1` (`--add-host=host.docker.internal:host-gateway` on Linux).

### Docker

```shell
docker build -t worker-755 .
docker run -p 8755:8755 -v worker755-data:/data --env-file .env worker-755
```

The container listens on `0.0.0.0:8755` and keeps shared reviews in `/data`. Behind Cloudflare, set `TRUST_PROXY=1` so the limits see real client addresses.

`deploy/compose.yaml` runs it behind a Cloudflare tunnel instead: its own network, no published ports, an unprivileged read-only container and resource limits. The steps are in the file.

## Configuration

Everything is optional. See [.env.example](https://github.com/gulivan/worker755/blob/main/.env.example).

| Variable                   | Default             |                                                                                                                                                            |
| -------------------------- | ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `AI_GATEWAY_API_KEY`       |                     | Turns on free shifts, paid from this key                                                                                                                   |
| `FREE_MODELS`              | first offered model | Models free shifts may use, comma separated                                                                                                                |
| `FREE_SHIFT_TOKENS`        | 250000              | Without sign-in: token allowance per free shift (the review is always written)                                                                             |
| `FREE_DAILY_TOKENS`        | 5000000             | Without sign-in: tokens per UTC day across all free shifts                                                                                                 |
| `FREE_SHIFTS_PER_DAY`      | 3                   | Without sign-in: free shifts per address per day                                                                                                           |
| `AUTH_SECRET`              |                     | Turns on sign-in with Google or GitHub: shifts on the server's key are paid from the account's credit (see `.env.example`)                                 |
| `FREE_CREDIT_USD`          | 0.25                | With sign-in: one-time starter credit, about one shift on DeepSeek V4.1 Flash                                                                              |
| `USAGE_MULTIPLIER`         | 2                   | With sign-in: what a model call costs the player, as a multiple of its price                                                                               |
| `MIN_START_USD`            | 0.05                | With sign-in: credit needed to start a shift                                                                                                               |
| `FREEMIUS_TOPUPS`          |                     | With sign-in: top-up plans as `<planId>:<dollars>,...`; also needs `FREEMIUS_PRODUCT_ID`, `FREEMIUS_PUBLIC_KEY`, `FREEMIUS_SECRET_KEY`, `FREEMIUS_API_KEY` |
| `SHIFTS_PER_HOUR`          | 30                  | Shift starts per address per hour, on any key                                                                                                              |
| `MANAGER_MODELS`           | built-in list       | Models offered, in gateway naming (`vendor/model`)                                                                                                         |
| `LOCAL_LLM_URL`            |                     | An OpenAI-compatible model server of your own, e.g. `http://127.0.0.1:11434/v1` for Ollama; offered as **Local**                                           |
| `LOCAL_LLM_API_KEY`        |                     | Sent to that server, for ones that want a key                                                                                                              |
| `INSTRUCTION_TIMEOUT_MS`   | 90000               | Time per instruction                                                                                                                                       |
| `HOST`, `PORT`             | 127.0.0.1, 8755     |                                                                                                                                                            |
| `TRUST_PROXY`              | 0                   | Read client addresses from `cf-connecting-ip`                                                                                                              |
| `TRUSTED_PROXIES`          |                     | Hosts allowed to set `cf-connecting-ip` (e.g. `tunnel`); empty trusts any peer                                                                             |
| `DATA_DIR`                 | `server/data`       | Where shared reviews and the free-shift counts are saved                                                                                                   |
| `SHARING`                  | 1                   | `0` turns review sharing off                                                                                                                               |
| `SOURCE_URL`, `DONATE_URL` |                     | Links shown under the start screen and the review                                                                                                          |
| `PUBLIC_URL`               |                     | Sent to OpenRouter for app attribution                                                                                                                     |

## How it works

```
shell/    in-memory filesystem, shell interpreter and a small SQLite-like engine (no dependencies)
server/   game engine: missions, the manager loop, the director, scoring, HTTP API
web/      React UI: assignment card, terminal, editor, the manager's feed, your record
```

The manager's prompt never mentions humans or games; see `server/src/manager.ts`. Each mission has a hidden ground-truth check, so the game knows when the manager signs off on broken work ("Times the manager was wrong"). [SPEC.md](https://github.com/gulivan/worker755/blob/main/SPEC.md) is the full design.

```shell
npm test               # shell and server tests, offline
npm run typecheck
```

## License

[MIT](https://github.com/gulivan/worker755/blob/main/LICENSE)
