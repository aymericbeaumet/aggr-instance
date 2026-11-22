---
title: PcapAnalyzer – self-hosted pcap analyzer with 27 problem detectors
link: https://github.com/sebaandrada-cell/PcapAnalyzer
source: hnrss-org
published: 2026-11-21T22:09:58Z
updated: 2026-11-21T22:09:58Z
first_seen: 2026-11-22T14:51:24.965070109Z
authors:
- sebaandrada
content: extracted
html: 2026-11-21-pcapanalyzer-self-hosted-pcap-analyzer-with-27-problem.html
remote_preview:
  url: https://opengraph.githubassets.com/3a0cefd5a5e73a9c727ecfb8cb16a105ed4edd3f5b49ce137c53c87d2ecd410e/sebaandrada-cell/PcapAnalyzer
  alt: Free, self-hosted pcap analyzer with 27 problem detectors and AI investigation - sebaandrada-cell/PcapAnalyzer
---

*Versión en español: [README.es.md](https://github.com/sebaandrada-cell/PcapAnalyzer/blob/main/README.es.md).*

Find the network fault in seconds, not hours. Upload a `.pcap` / `.pcapng` capture (gzip too) and PcapAnalyzer tells you what is failing, why, and what to check next. It runs on your own machine, so captures never leave your network.

**Free and open source (Apache 2.0). Status: beta.**

- Try it without installing anything: [https://pcap-analyzer-seven.vercel.app/#demo](https://pcap-analyzer-seven.vercel.app/#demo)
- Download: [https://sebaandrada6.gumroad.com/l/hspgx](https://sebaandrada6.gumroad.com/l/hspgx) (free) or clone this repository.

## What it does

- **27 detectors** that run on every capture, each with a severity and a next step:
  - TCP: firewall blocks (SYN→RST), retransmissions, duplicate ACKs, loss and reordering, zero and full windows, resets after the handshake, slow handshakes.
  - Path: PMTUD black holes, low MSS, IP fragmentation, ICMP errors, routing loops.
  - Name and address services: unanswered or slow DNS, SERVFAIL, NXDOMAIN bursts, DHCP NAK / DECLINE / no answer, duplicate IPs, ARP storms.
  - Applications: HTTP 4xx / 5xx and slow responses, TLS alerts, SSL 3.0 / TLS 1.0 / 1.1.
  - Tunnels: IPsec (IKE and ESP), WireGuard, OpenVPN, GRE, L2TP and Teredo that are one-way, never answer a handshake or keep renegotiating.
  - Security: port scans and sweeps, cleartext logins (FTP, POP3, IMAP, SMTP AUTH, HTTP Basic; credentials are never stored or shown), Telnet / SMBv1 / SNMP v1-v2c, ARP spoofing, overlapping IP fragments.
- **Capture quality first.** It warns when the capture itself limits the result: frames duplicated by a mirror port, traffic seen in one direction only, truncated packets, connections already open when the capture started.
- **Wireshark-style views:** packet list with real Wireshark display filters over the whole capture, colouring, CSV export, expert info, HTTP / DNS / DHCP / TLS tables, response times and a flow diagram per conversation.
- **AI investigation (optional):** an AI investigates the capture with tools (real filters, stream detail, statistics) and cites the packets behind every claim. You can then ask follow-up questions. It uses your own Anthropic API key; without one, a demo mode runs the full flow with text generated from the analysis, at no cost.
- **PDF report**, Spanish and English interface, light and dark theme.

## Quick start

You need **Docker** with Docker Compose. On Windows, run the script from Git Bash or WSL.

```shell
git clone https://github.com/sebaandrada-cell/PcapAnalyzer.git
cd PcapAnalyzer
./start.sh
```

The script creates a random access token (shown once, stored in `.env`) and starts the app. When the build finishes, open [http://localhost:8080](http://localhost:8080), paste the token into the "API token" field (top right) and upload a capture, or click "Try the sample capture".

The app listens on `127.0.0.1` only. Do not expose it on a network without an authenticated reverse proxy: the API can use your AI account. [QUICKSTART.md](https://github.com/sebaandrada-cell/PcapAnalyzer/blob/main/QUICKSTART.md) has the details, the AI setup and troubleshooting.

### Without Docker

You need Python 3.9+, Node 20+ and **tshark** (installed with Wireshark).

```shell
cd backend
python3 -m venv .venv
.venv/bin/pip install -r requirements-dev.txt
export API_TOKENS="$(openssl rand -hex 32)"
.venv/bin/uvicorn app.main:app --reload
```

```shell
cd frontend
npm install
npm run dev
```

Open [http://localhost:5173](http://localhost:5173) and paste the same token. If tshark is not on your `PATH`, set `TSHARK_PATH` to the binary.

## Privacy

- The capture is analysed on your machine. While an AI investigation lasts it is kept encrypted (60 minutes of inactivity, or until you press "Delete capture now") and is then deleted.
- The AI is optional. It only sees what its tools return (packet metadata; packet content only if you switch it on for that session), never the pcap file.
- No telemetry and no accounts for the self-hosted version.

## Tests

```shell
cd backend && .venv/bin/python -m pytest -q    # needs tshark
cd frontend && npm test
```

## Feedback

PcapAnalyzer is a beta. It has been tested with synthetic fixtures and public sample captures, and the most useful feedback is a capture where it misses a problem or reports one that is not real. Please open an issue at [https://github.com/sebaandrada-cell/PcapAnalyzer/issues](https://github.com/sebaandrada-cell/PcapAnalyzer/issues) and describe the case. **Do not attach captures that contain real credentials or customer data**; anonymise them first or describe the pattern.

The technical reference (JSON schema, detector thresholds, limits, API errors) is in [README.es.md](https://github.com/sebaandrada-cell/PcapAnalyzer/blob/main/README.es.md), in Spanish.

## License

[Apache License 2.0](https://github.com/sebaandrada-cell/PcapAnalyzer/blob/main/LICENSE). See [NOTICE](https://github.com/sebaandrada-cell/PcapAnalyzer/blob/main/NOTICE) for third-party software: tshark / Wireshark (GPL) runs as a separate program and is not distributed with PcapAnalyzer.
