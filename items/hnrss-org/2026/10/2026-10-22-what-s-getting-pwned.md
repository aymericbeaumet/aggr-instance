---
title: What's Getting Pwned
link: https://pwn.watch/
source: hnrss-org
published: 2026-10-22T14:08:12Z
updated: 2026-10-22T14:08:12Z
first_seen: 2026-10-22T17:33:41.202070908Z
authors:
- mrkd
content: extracted
html: 2026-10-22-what-s-getting-pwned.html
remote_preview:
  url: https://pwn.watch/og/home.jpg
  alt: 'pwn.watch: live security incidents, patch status and fixed versions'
---

Security incidents that matter right now, and whether there's a fix yet.

Critical 4 · High 3 · Med 2 · Low 1 · Update ½ Severity-weighted: critical 4 · high 3 · medium 2 · low 1 · update ½ Less More

Live incidents

37

Critical

16

Not fully patched

18

Days without an incident

0

Longest quiet run: 11 days (Jan 2–Jan 12)

- Critical Patched

  ## [Arista VeloCloud Orchestrator zero-day CVE-2026-114625 exploited in attacks](https://pwn.watch/i/arista-velocloud-orchestrator-cve-2026-114625)

  Arista patched an actively exploited critical SQL injection flaw (CVE-2026-114625, CVSS 9.9) in on-prem VeloCloud Orchestrator that lets any authenticated user gain operator privileges and masquerade as the local 'superuser operator'. Hosted VCO is already patched; on-prem admins need to upgrade.

  Arista VeloCloud Orchestrator (VCO) On-Prem Since Oct 22, 2026 updated Oct 22, 2026, 19:31 UTC

- Medium Partly patched

  ## [Major U.S. banks detect unauthorized third-party AI agents inside customer accounts](https://pwn.watch/i/us-banks-unauthorized-ai-agents-customer-accounts)

  Axios reports that major U.S. banks, including Morgan Stanley, Bank of America, JPMorgan and Wells Fargo, are detecting third-party AI agents operating inside customer accounts. In most cases the agent logs in with the customer's own credentials and permission, which breaks bank rules that require people to log in themselves. Some banks also saw agents contact call centers or take actions that appeared independent of user instructions, and executives say they often can't tell a human from an agent, or an authorized agent from a rogue one. No account takeovers, fraud losses or specific AI agent products have been reported. Morgan Stanley is blocking third-party agents and JPMorgan, Bank of America and Citi are restricting them. Don't give an AI agent your bank username and password.

  Online banking accounts at major U.S. banks (Morgan Stanley, Bank of America, JPMorgan Chase, Wells Fargo, Citi) Since Oct 22, 2026 updated Oct 22, 2026, 16:53 UTC

- Critical Patched

  ## [GitLab CVSS 10 unauthenticated RCE via regex compiler (CVE-2026-110264)](https://pwn.watch/i/gitlab-cve-2026-110264-regex-rce)

  GitLab released a critical patch on Oct 21 2026 for CVE-2026-110264: an integer overflow in the bundled Onigmo / regex compiler that can allow an unauthenticated attacker to execute code on self-managed GitLab. CVSS 10.0; every CE/EE version from 7.0 (2014) is in range. Instances that only applied the Oct 12 regex fix remain vulnerable and must upgrade again.

  GitLab CE/EE (self-managed) Since Oct 21, 2026 updated Oct 22, 2026, 16:00 UTC

- Critical No patch yet

  ## [Linux kernel: unpatched ZDI bug reports with repro details leaked to public LKML (ksmbd RCE, CVSS 9.0)](https://pwn.watch/i/linux-kernel-zdi-reports-leaked-to-lkml)

  On Oct 21, Trend Micro's Zero Day Initiative sent several unpatched Linux kernel vulnerability reports to security@kernel.org but Cc'd the public linux-kernel and subsystem lists, so full technical details (code paths, KASAN traces) are now public before fixes ship. The worst are two ksmbd (in-kernel SMB server) remote code execution bugs that ZDI scores CVSS 9.0: an SMB Direct integer underflow (needs an RDMA device and a writable share) and a protocol-dispatch race (needs a guest share). Also leaked: an skb\_zerocopy local privilege escalation (CVSS 7.8), a vmwgfx out-of-bounds write LPE, and an nvmet info leak. No CVEs yet and no fixed mainline or stable release; the ksmbd race fix sits in the ksmbd maintainer tree and an SMB Direct fix is in a branch aimed at Linux 7.5. No exploitation reported. Only machines running ksmbd are exposed to the RCEs.

  Linux kernel: ksmbd (in-kernel SMB server) and SMB Direct, skbuff/NFQUEUE networking, vmwgfx, nvmet Since Oct 21, 2026 updated Oct 22, 2026, 19:02 UTC

- Critical Patched

  ## [LiteLLM 1.107.0 PyPI/Docker supply-chain compromise](https://pwn.watch/i/litellm-1-107-0-pypi-compromise)

  On Oct 21 2026 attackers compromised LiteLLM’s release pipeline and published malicious litellm==1.107.0 to PyPI (live ~2 hours) plus compromised Docker Hub/GHCR tags (v1.107.0, latest, main-latest, main-stable). The wheels install cook.pth / cook.py / sitecustomize.py that steal cloud, developer and AI-tool credentials at every Python startup and open a remote-code loader. Second LiteLLM compromise of 2026.

  LiteLLM (PyPI + official Docker/GHCR images) Since Oct 21, 2026 updated Oct 22, 2026, 16:00 UTC

- High Partly patched

  ## [ISC BIND 9: replayed GSS-TSIG messages corrupt memory, possible RCE (CVE-2026-94300)](https://pwn.watch/i/isc-bind-cve-2026-94300)

  ISC disclosed a remotely exploitable memory-corruption bug in BIND 9's named on Oct 21. An attacker who captures a GSS-TSIG signed UDP message (the Kerberos signing used for Active Directory-style dynamic DNS updates) can replay it while the negotiated key is still valid and, combined with crafted queries to a zone they control, may get remote code execution, or at least crash named. CVSS 8.1 (High). No workaround. Affects BIND 9.11.0 through 9.18.50, 9.20.0 through 9.20.29 and 9.21.0 through 9.21.26. Fixed in 9.20.30 and 9.21.27; ISC lists no 9.18 fix. ISC knows of no active exploitation. Debian trixie has a fixed package; Debian bookworm and Ubuntu are still waiting.

  ISC BIND 9 (named) DNS server Since Oct 21, 2026 updated Oct 22, 2026, 15:59 UTC

- High Patched

  ## [Node.js October 2026 security releases (Brotli RCE + TLS/DNS highs)](https://pwn.watch/i/nodejs-october-2026-security-releases)

  On Oct 21 2026 Node.js shipped security updates for 22.x, 24.x and 26.x fixing ten vulnerabilities. The headline issue is CVE-2026-111497, a high-severity Brotli decoder out-of-bounds write that can enable remote code execution when attacker-controlled Brotli data is decoded. The same release also fixes TLS hostname/IP bypasses and DNS NUL truncation, and refreshes the bundled CA store for the eMudhra incident.

  Node.js Since Oct 21, 2026 updated Oct 22, 2026, 11:18 UTC

- High Partly patched

  ## [Railway discloses unauthorized access to customer environment variables](https://pwn.watch/i/railway-env-var-security-disclosure)

  On Oct 21 2026 Railway reported anomalous activity on systems storing customer environment variables. An unauthorized third party may have accessed encrypted env vars and, for some, obtained decrypted contents (API keys, DB/registry credentials). Railway says it believes this is part of a series of attacks on platform companies aimed at compromising AI inference providers, and asked upstream providers to revoke keys. Builds were paused platform-wide for ~a day; on Oct 22 they were restored for Enterprise and Pro, with Hobby/Free/Trial still pending. All users are told to rotate secrets while scoping continues.

  Railway (hosted platform) Since Oct 21, 2026 updated Oct 22, 2026, 16:32 UTC

- Oct 20 High EG Secure Solutions SiteGuard Server Edition: EG Secure Solutions SiteGuard Server Edition OS Command Injection Vulnerability Patched

  EG Secure Solutions SiteGuard Server Edition Patched

  EG Secure Solutions SiteGuard Server Edition contains an OS command injection vulnerability which may allow an attacker to execute arbitrary OS commands with root or SYSTEM privileges on the system running the affected product. Added to CISA KEV on 2026-10-20.

  [Open incident →](https://pwn.watch/i/eg-secure-solutions-siteguard-server-edition-cve-2026-106931) CVE-2026-106931

- Oct 20 High GMO Onamae Desktop Cloud malware on customer VPS Partly patched

  Onamae Desktop Cloud (GMO Internet) Partly patched

  GMO Internet’s Japanese VPS service Onamae Desktop Cloud said an attacker installed malicious programs in customer environments, forcing shutdown of as many as ~26,739 servers with no immediate restart date (mid-October 2026 reporting).

- Oct 20 High Siemens Software Center: Siemens Software Center Uncontrolled Search Path Element Vulnerability Patched

  Siemens Software Center Patched

  Siemens Software Center contains an uncontrolled search path element vulnerability that could allow an authenticated local attacker to place a specially crafted DLL in a directory that the application searches, leading to arbitrary code execution with the privileges of the affected application. Added to CISA KEV on 2026-10-20.

  [Open incident →](https://pwn.watch/i/siemens-software-center-cve-2026-110936) CVE-2026-110936

- Unrated Partly patched

  ## [HTTP/3 QPACK “Header Accordions” memory-amplification DoS](https://pwn.watch/i/http3-qpack-header-accordions)

  FoxIO published research (Oct 20 2026) showing that missing or late decoded-header limits in QPACK let a tiny HTTP/3 HEADERS frame expand into huge server memory use (up to ~4000× on Bun). Seven of ten implementations tested were vulnerable; quic-go, Cloudflare quiche, lsquic and Wireshark are patched, while Bun, aioquic/mitmproxy and h3 remained unpatched at publication.

  HTTP/3 / QPACK stacks (quic-go, Cloudflare quiche, lsquic, Bun, aioquic, h3, Wireshark, …) Since Oct 20, 2026 updated Oct 22, 2026, 11:19 UTC

- Critical Patched

  ## [OpenCode: malicious MCP server can run code on Windows/WSL during sign-in (no CVE)](https://pwn.watch/i/opencode-mcp-oauth-rce-windows)

  A command-injection flaw in the OpenCode AI coding agent let a malicious or later-hijacked remote MCP server run PowerShell commands on a developer's Windows or WSL machine when they started the MCP OAuth sign-in. The server-supplied authorization endpoint was passed unescaped to an old version of the open package. Anomaly fixed it in 1.18.33 on Sep 28 without a CVE or advisory, so scanners won't flag 1.0.137 through 1.18.32. macOS and Linux are not affected. Pillar Security found it and rates it 9.8; no in-the-wild exploitation reported.

  OpenCode AI coding agent (Anomaly) on Windows and WSL Since Oct 19, 2026 updated Oct 22, 2026, 16:00 UTC

- High Patched

  ## [Capcom ID breach exposes 11.7M account email addresses](https://pwn.watch/i/capcom-id-breach-oct-2026)

  Capcom says a third party gained unauthorized access to CAPCOM ID, the account needed to play some Capcom games online. 11,774,247 records worldwide had email addresses plus account creation time, last login time, login count and last-login IP exposed; 43 also had date of birth, gender, country/region and nickname. Passwords were changed without authorization on 196 accounts (already reset). Capcom says it has not confirmed any leak of customer-set passwords, and no credit card data was involved. Detected Oct 19; reported to Osaka Prefectural Police and Japan's PPC on Oct 21. Expect phishing impersonating Capcom.

  CAPCOM ID (Capcom online account service) Since Oct 19, 2026 updated Oct 22, 2026, 18:00 UTC

- Critical Partly patched

  ## [eMudhra intermediate CA private keys compromised](https://pwn.watch/i/emudhra-intermediate-ca-compromise)

  Eight eMudhra Global TLS intermediate CA private keys were found to be cryptographically weak / recoverable (researcher report Oct 18). Mozilla, Chrome, Apple and Microsoft blocked the intermediates; eMudhra revoked them Oct 19 and the ECC root was distrusted as a precaution. Downstream runtimes that ship their own trust stores (Go, Node.js, Electron, Python certifi, curl, etc.) needed emergency CA-bundle updates so they stop trusting the compromised chains.

  eMudhra / emSign Global TLS intermediates; any client shipping Mozilla-style CA stores Since Oct 18, 2026 updated Oct 22, 2026, 19:14 UTC

- Critical Partly patched

  ## [ConnectWise Cloud Backup shut down after M365 app-identity hijack](https://pwn.watch/i/connectwise-cloud-backup-m365-hijack)

  ConnectWise disabled Cloud Backup after a compromised credential let attackers authenticate as the application and sign in to at least 173 customer Microsoft 365 tenants.

  ConnectWise Cloud Backup / Microsoft 365 app identity Since Oct 17, 2026 updated Oct 22, 2026, 19:14 UTC

- Critical Partly patched

  ## [PHP2Shell: critical unauthenticated RCE in PHP core](https://pwn.watch/i/php2shell-cve-2026-109618-109619)

  Two critical remote-code-execution flaws in PHP core (CVE-2026-109618 and CVE-2026-109619, aka “PHP2Shell”) were disclosed early after an embargo leak. Both score CVSS 9.2 and can let an unauthenticated attacker run code on servers serving PHP web requests, especially Apache mod\_php. Patches shipped Oct 21 2026 (appliance vendors such as QNAP are still preparing fixes); exploitation attempts were observed before and after the patch, and watchTowr / AWS report active scanning.

  PHP Since Oct 17, 2026 updated Oct 22, 2026, 19:31 UTC

- High Partly patched

  ## [OpenCode Console breach exposes users' saved OpenAI / Anthropic / Google API keys](https://pwn.watch/i/opencode-console-provider-key-breach)

  On the night of Oct 16 2026 an intruder reached a legacy OpenCode Console system that was being decommissioned and may have exposed third-party AI provider keys saved in Console workspaces. OpenCode says 1.4% of Console users (those with external providers linked) were affected and tied it to a large-scale distillation attack. Users reported unauthorized charges up to ~$1,700 and some Google Cloud project suspensions. Anthropic and Google revoked/blocked exposed keys; OpenAI refunded drawn credits. The OpenCode app itself (local keys) was not affected.

  OpenCode Console (hosted service by Anomaly) Since Oct 17, 2026 updated Oct 22, 2026, 16:32 UTC

- Critical Patched

  ## [Plex2Shell: chained Plex Media Server RCE exploited for miners](https://pwn.watch/i/plex2shell-cve-2026-96652)

  Four Plex Media Server flaws (CVE-2026-96652/96654/96655/96656), together called Plex2Shell, chain to unauthenticated admin-token theft and code execution. Researcher Zach Main published details Oct 16 and a PoC shortly after; exploitation of internet-facing servers (cryptominers + residential proxies) began by Oct 17. Fixed in 1.43.3.10861 (released earlier in August); CISA added the chain to KEV with a federal deadline of Oct 23.

  Plex Media Server Since Oct 16, 2026 updated Oct 22, 2026, 11:20 UTC

- Oct 16 High Fortinet Multiple Products: Fortinet Multiple Products Path Traversal Vulnerability Patched

  Fortinet Multiple Products Patched

  FortiOS, FortiPAM, FortiProxy, and FortiSwitchManager contain a path traversal vulnerability that may allow an authenticated attacker with admin profile and at least read-write permissions to write or delete arbitrary files via specific CLI commands. Added to CISA KEV on 2026-10-16.

- Oct 15 High Atlassian Multiple Products: Atlassian Multiple Products Files or Directories Accessible to External Parties Vulnerability Patched

  Atlassian Multiple Products Patched

  Multiple Atlassian products (Bitbucket Data Center ,Confluence Data Center, Jira Service Management Data Center, Jira Software Data Center, Bamboo Data Center, Crowd Data Center, Crucible and Fisheye) contain a files or directories accessible to external parties vulnerability which could allow an unauthenticated attacker to access specific files within the web application root directory in affected versions. Added to CISA KEV on 2026-10-15.

- Critical Patched

  ## [Next.js out-of-band release fixes two unauthenticated RCEs (SVG image optimization CVE-2026-96889, use cache)](https://pwn.watch/i/nextjs-october-2026-rce-security-release)

  On Oct 14 2026 Vercel shipped Next.js 16.4.1 and 15.5.28 with two critical unauthenticated remote code execution fixes held back from the September release: CVE-2026-96889 / GHSA-2464-fhr4-w9xw, a use-after-free in librsvg (via sharp) reachable through the Image Optimization API when SVGs are processed on glibc-based Linux, and GHSA-673q-mjm6-r5xr, RCE in App Router apps with Cache Components when a \`use cache\` function returns multiple typed arrays. A high-severity Server Actions DoS (GHSA-p3m5-59mp-jv5r) is fixed too. GitHub added the SVG advisory to its global database on Oct 22. No known exploitation.

  Next.js (next on npm), sharp / librsvg Since Oct 14, 2026 updated Oct 22, 2026, 18:00 UTC

- Critical Partly patched

  ## [Zeabur breach exposes 3.2M env-var records; company to shut down](https://pwn.watch/i/zeabur-october-2026-breach-shutdown)

  Zeabur disclosed an October 14–15 2026 security incident in which an attacker used leaked internal credentials stored in plaintext to access shared clusters in 9 regions, root into 101 Zeabur-managed GCP dedicated servers, and read the platform database, exposing ~3.22 million environment-variable records across 321,810 accounts. Zeabur is winding down: from December 1 2026 shared-cluster services pause and dedicated servers stop renewing, and on December 31 all services stop and service data is deleted. Refund and incident-claim requests close November 30. Users must rotate every secret stored on Zeabur and migrate off.

  Zeabur (PaaS) Since Oct 14, 2026 updated Oct 22, 2026, 19:02 UTC

- Oct 14 High Ahsay AhsayCBS: Ahsay AhsayCBS Command Injection Vulnerability Patched

  Ahsay AhsayCBS Patched

  Ahsay AhsayCBS contains a command injection vulnerability that could allow a remote attacker to execute operating system commands via manipulation of the random parameter in the Replication Receiver component’s /rps/api/json/UpdateReceivers.do endpoint. This vulnerability can be chained with CVE-2026-105133. CISA added CVE-2026-105134, CVE-2026-105133 to the Known Exploited Vulnerabilities catalog on 2026-10-14 based on evidence of active exploitation.

  [Open incident →](https://pwn.watch/i/ahsay-ahsaycbs-cve-2026-105134) CVE-2026-105134 · CVE-2026-105133

- Oct 14 High Cisco Secure Email Gateway: Cisco Secure Email Gateway Relative Path Traversal Vulnerability Patched

  Cisco Secure Email Gateway Patched

  Cisco Secure Email Gateway contains a relative path traversal vulnerability that could allow an unauthenticated, remote attacker to write arbitrary files on the underlying operating system. Added to CISA KEV on 2026-10-14.

- Oct 14 High Palo Alto Networks PAN-OS: Palo Alto Networks PAN-OS Code Injection Vulnerability Patched

  Palo Alto Networks PAN-OS Patched

  Palo Alto Networks PAN-OS contains a code injection vulnerability that could allow an authenticated attacker with network access to execute arbitrary code with root privileges on the firewall. Added to CISA KEV on 2026-10-14.

- Medium Partly patched

  ## [GoDaddy mass-revokes 404,607 TLS certificates over CP/CPS profile mismatch](https://pwn.watch/i/godaddy-mass-certificate-revocation-oct-2026)

  GoDaddy (Starfield) found that 404,607 TLS certificates carried policy information (policy OIDs, CPS URI) that didn't match its own published CP/CPS, which under the CA/Browser Forum Baseline Requirements means they must be revoked within five days. GoDaddy missed the Oct 18 deadline and says the certificates were revoked on Oct 21 2026. Nothing was hacked and no keys leaked, but sites still serving a revoked certificate can show browser warnings or become unreachable until the re-issued certificate is installed.

  GoDaddy / Starfield TLS certificates (DV, OV, EV) Since Oct 14, 2026 updated Oct 22, 2026, 14:58 UTC

- Critical Patched

  ## [PwnEscape: Linux networking guest-to-host escape (CVE-2026-99999)](https://pwn.watch/i/pwnescape-cve-2026-99999)

  Paulos Yibelo / pwn.ai found a Critical Linux networking flaw (CVE-2026-99999, CVSS 9.3) during Vercel’s $1M Sandbox challenge. Guest-controlled checksum metadata can rewrite an IPv4 header after validation, enabling host-memory disclosure and, in lab conditions, guest-to-host code execution on KVM/Firecracker. Upstream fix merged Oct 2026; host kernels need patch + reboot. Affects multi-tenant VPS / sandbox providers more than end-user desktops.

  Linux kernel networking (host side of KVM/Firecracker and similar) Since Oct 8, 2026 updated Oct 22, 2026, 11:18 UTC

- High Partly patched

  ## [GhostAction returns: hijacked GitHub accounts push secret-stealing 'security audit' workflows to 30k+ repos](https://pwn.watch/i/ghostaction-github-workflows-oct-2026)

  GhostAction, a campaign first documented by GitGuardian in Sept 2025, came back in 2026. It hit 772 repos in Aug-Sep 2026, then surged on Oct 8-12: about 1,900 compromised GitHub accounts pushed fake 'security audit' workflows to 30,000+ repositories (GitGuardian). The new payload exfiltrates the repo's Actions secrets plus every credential found in the full git history to 193.32.204.199 over plain HTTP. GitHub began preventing the workflow from running around Oct 9. The operator moved on to GitHub-themed phishing (my-github.com) and a VS Code tasks.json + fake .woff2 font stealer found in 94 repos.

  GitHub repositories / GitHub Actions (compromised maintainer accounts) Since Oct 8, 2026 updated Oct 22, 2026, 12:04 UTC

- Critical Partly patched

  ## [tensorlake@0.5.144 npm Shai-Hulud worm with hostage GitHub token](https://pwn.watch/i/tensorlake-npm-shai-hulud-worm)

  On Oct 8 2026, tensorlake@0.5.144 was published from the real GitHub repo (with npm provenance) carrying a preinstall hook that installs a Shai-Hulud-style credential-stealing worm. It harvests npm/GitHub/cloud/SSH/AI-tool secrets, spreads to other packages, and installs a “gh-token-monitor” that wipes the home directory if the stolen GitHub token is revoked. Pin 0.5.143; remove the monitor before rotating tokens.

  tensorlake (npm) Since Oct 7, 2026 updated Oct 22, 2026, 18:15 UTC

- Oct 4 High Citrix NetScaler: Citrix NetScaler Improper Restriction of Operations within the Bounds of a Memory Buffer Vulnerability Patched

  Citrix NetScaler Patched

  Citrix NetScaler ADC (formerly Citrix ADC) and Citrix NetScaler Gateway (formerly Citrix Gateway) contain an improper restriction of operations within the bounds of a memory buffer vulnerability that could allow for a denial of service. Added to CISA KEV on 2026-10-04.

- Oct 2 High Zammad GmbH Zammad: Zammad GmbH Zammad Improper Privilege Management Vulnerability Patched

  Zammad GmbH Zammad Patched

  Zammad GmbH Zammad contains an improper privilege management vulnerability that can allow the local zammad user to escalate privileges to root. This vulnerability can be chained with CVE-2026-102489. CISA added CVE-2026-102490, CVE-2026-102489 to the Known Exploited Vulnerabilities catalog on 2026-10-02 based on evidence of active exploitation.

  [Open incident →](https://pwn.watch/i/zammad-gmbh-zammad-cve-2026-102490) CVE-2026-102490 · CVE-2026-102489

- High Partly patched

  ## [Ivanti Connect Secure zero-day chain (CVE-2026-104935) exploited for root](https://pwn.watch/i/ivanti-connect-secure-cve-2026-104935)

  Volexity observed threat actor UTA0576 chaining an Ivanti Connect Secure authentication-bypass / RCE zero-day (CVE-2026-104935) with additional flaws from about Oct 1–3 2026 to get unauthenticated root on fully patched appliances, re-exploiting in memory without planting webshells. Ivanti patched CVE-2026-104935 on Oct 13; CISA added it to KEV. One privilege-escalation piece of the chain was still being fixed at disclosure.

  Ivanti Connect Secure (ICS) Since Oct 1, 2026 updated Oct 22, 2026, 11:20 UTC

- Critical Patched

  ## [FortiMail unauthenticated file-write zero-day CVE-2026-104286](https://pwn.watch/i/fortimail-cve-2026-104286)

  Fortinet FortiMail CVE-2026-104286 (CVSS 9.8) is an unauthenticated arbitrary file write via path traversal / null-byte issues in Identity-Based Encryption web flows. CISA confirmed active exploitation and ordered federal mitigation; Fortinet published FG-IR-26-175 with patches and interim mitigations (disable IBE / restrict admin/webmail).

  Fortinet FortiMail Since Sep 30, 2026 updated Oct 22, 2026, 11:18 UTC

Show 3 more live incidents 3 more live incidents

### September 2026 1

- High Partly patched

  ## [PingCAP / TiDB Cloud breach: stolen service-account credentials, data and source code taken](https://pwn.watch/i/pingcap-tidb-cloud-breach)

  PingCAP confirmed that between Sep 9 and Oct 7 2026 an unauthorized party used stolen service-account credentials to access internal systems, copy data and obtain source code. Exported records included TiDB Cloud user names/emails, org/project associations, SQL-editor text, billing/contract data and an encrypted API-key table. Extortion group FulcrumSec claims 10.6 TB and 115k plaintext API keys; those figures are unverified and PingCAP disputes the API-key claim. No evidence data stored in TiDB was affected.

  PingCAP TiDB Cloud (hosted database platform) Since Sep 9, 2026 updated Oct 22, 2026, 11:20 UTC

### August 2026 1

- Critical Partly patched

  ## [BeyondTrust Remote Support zero-day CVE-2026-102764 (ARMORY CASTLE)](https://pwn.watch/i/beyondtrust-remote-support-cve-2026-102764)

  China-nexus actor ARMORY CASTLE (ex-UNC5221) exploited CVE-2026-102764, a critical unauthenticated file-operations flaw in BeyondTrust Remote Support, as a zero-day from late August 2026. BeyondTrust issued BT26-05-RS; SaaS was auto-patched by Oct 9; CISA added the CVE to KEV on Oct 16. Self-hosted appliances need the patch plus forensics — patching alone does not clear existing footholds (SAWFLY / BARECLAW).

  BeyondTrust Remote Support (formerly Bomgar) Since Aug 29, 2026 updated Oct 22, 2026, 18:15 UTC

### February 2025 1

- Critical No patch yet

  ## [Fearless Wallet browser extension leaked private keys for ~20 months](https://pwn.watch/i/fearless-wallet-extension-key-leak)

  The Polkadot Community Foundation warned that Fearless Wallet browser-extension versions 3.0.1, 3.0.3, 3.0.4 and 3.0.5 contained code, added Feb 19 2025 inside a large 'TON ecosystem' change, that sent private keys/recovery data to an attacker server when the wallet was unlocked. The code was active at different times until Oct 10 2026. Eight users have reported losses totalling 17,286 DOT. Anyone who used the extension since Feb 2025 must create a new keypair in another wallet and move funds.

  Fearless Wallet browser extension (Chrome; Firefox build delisted) Since Feb 19, 2025 updated Oct 22, 2026, 19:02 UTC

Get an email when a new major security incident drops. No spam, unsubscribe any time.

Not security advice. Always check the vendor advisory before acting.
