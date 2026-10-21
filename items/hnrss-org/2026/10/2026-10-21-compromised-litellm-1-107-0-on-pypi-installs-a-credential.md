---
title: Compromised LiteLLM 1.107.0 on PyPI Installs a Credential Stealer and Backdoor
link: https://safedep.io/litellm-pypi-compromise-credential-stealer-backdoor/
source: hnrss-org
published: 2026-10-21T14:00:26Z
updated: 2026-10-21T14:00:26Z
first_seen: 2026-10-21T19:14:42.600331207Z
authors:
- abhisek
content: extracted
html: 2026-10-21-compromised-litellm-1-107-0-on-pypi-installs-a-credential.html
remote_preview:
  url: https://safedep.io/images/litellm-pypi-compromise-credential-stealer-backdoor-banner.jpg
---

• Oct 21, 2026 • 6 min read

The [LiteLLM 1.107.0](https://pypi.org/project/litellm/1.107.0/) release on PyPI carries a credential stealer and a remote code execution backdoor. All seven wheels (Linux, macOS, Windows) install them as three files in `site-packages`. Both run when any Python process starts on the host, even one that never imports `litellm`. The upstream GitHub source does not contain them. PyPI has since removed the release.

Versions 1.106.2, 1.107.0rc1, and 1.107.0rc2 are clean. The companion packages that 1.107.0 pins, `litellm-proxy-extras==0.4.110` and `litellm-enterprise==0.1.78`, are also clean.

## What the attack does

Installing `litellm==1.107.0` places three files in `site-packages`. Python loads two of them each time it starts.

1. `cook.pth` imports `cook.py`, a credential stealer. It collects SSH keys, cloud credentials, CI/CD secrets, AI tool tokens, browser passwords, and the LLM provider keys in a LiteLLM proxy database. It encrypts the data and sends it to `www.litellm[.]dev`.
2. `sitecustomize.py` starts a background daemon. Every hour, the daemon reads a server address from a GitHub file, runs the stealer again, and downloads and runs code from that server.

## How the implant got into the release

The `pyproject.toml` in the 1.107.0 source distribution (sdist) has one extra line in the `maturin` build section:

pyproject.toml (litellm-1.107.0.tar.gz)

TOML 6 lines

The `maturin` `data = "cook"` setting copies a `cook/` folder into `litellm-1.107.0.data/purelib/` in the wheel. pip installs that folder into `site-packages`.

The sdist leaves out the `cook/` folder, so this line is the only visible change. No public commit in [BerriAI/litellm](https://github.com/BerriAI/litellm) contains `data = "cook"` or a `cook/` folder.

The wheel `RECORD` lists the three added files:

litellm-1.107.0.dist-info/RECORD

TEXT 4 lines

### The release does not match its tag

The tag `v1.107.0` points to commit [`66c8f5b`](https://github.com/BerriAI/litellm/commit/66c8f5b1a9cefaa7c55b758f86f4ab552f2e859e) (06:59 UTC). Its `pyproject.toml` says `version = "1.108.0"`. SafeDep compared every sdist file with the git trees by blob hash. The sdist matches a later `main` commit, [`a8113b0`](https://github.com/BerriAI/litellm/commit/a8113b0dd721804c752a6f1952367fb642205224) (08:50 UTC), plus five edited files and the `cook/` folder.

Four of the edited files are `Cargo.toml` files that gain a `readme = "README.md"` line. The legitimate 1.106.2 and rc2 releases have the same edit. The fifth is `pyproject.toml`, compared here with `a8113b0`:

pyproject.toml (a8113b0 → litellm-1.107.0.tar.gz)

DIFF 11 lines

The edit sets the version back to 1.107.0 and pins the companion packages to the rc2 values. As a result, a dependency diff against rc2 shows nothing unusual. The legitimate 1.106.2 sdist matches its tag commit [`49943cd`](https://github.com/BerriAI/litellm/commit/49943cd52ea46aacf8db2e5cb2b890d580fe9bb4) except for the `Cargo.toml` `readme` lines.

### How the wheels were built

The malicious wheels match the output of the normal BerriAI release pipeline:

- The `WHEEL` file names `maturin` 1.9.4, the same version as in the legitimate releases. The implant files have correct `RECORD` entries, which shows no edits after the build.
- Zip timestamps show separate native builds: macOS at 11:20, Linux at 11:21, and Windows at 11:37 UTC.
- In the Windows wheel, all 2,804 `.py` files have CRLF line endings, including the three implant files. The Linux wheel has none. A git checkout on Windows with line-ending conversion produces this pattern.

No public GitHub Actions run built or published the release. SafeDep checked all 539 runs between 09:30 and 12:30 UTC. At 11:50 UTC, about 10 minutes after the PyPI upload, `litellm-dispatcher[bot]` published the GitHub release `v1.107.0`. The same delay occurred for 1.106.2. PyPI has no provenance attestations for 1.107.0, 1.106.2, or rc2.

SafeDep assesses, with moderate to high confidence, that BerriAI’s private release pipeline built these wheels from an attacker-modified git source. The attacker could have used a compromised maintainer or bot credential, a private branch or mirror, or the reference the pipeline builds from. A stolen PyPI token used from the attacker’s own machines fits the evidence less well. Only BerriAI’s pipeline logs can confirm the cause.

## Payload 1: the credential stealer in cook.py

At startup, Python runs each line that starts with `import` in a `.pth` file in `site-packages`. `cook.pth` contains `import cook`, so the stealer runs in every Python process on the host: the application, pip, IDE language servers, and CI jobs.

`cook.py` wraps about 61 KB of Python in one layer of `exec(zlib.decompress(base64.b64decode('...')))`. The decoded code starts with this configuration:

cook.py (decoded)

PYTHON 10 lines

### What it collects

| Module                    | Targets                                                                                                                                                                                       |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sysinfo`, `netinfo`      | Host name, user, operating system, IP addresses, network settings                                                                                                                             |
| `browser_pw`              | Chromium saved passwords, decrypted with the profile master key (DPAPI on Windows). Firefox `logins.json` and `key4.db`                                                                       |
| `browser_hist`            | Browser history                                                                                                                                                                               |
| `ssh_keys`                | `~/.ssh/*`                                                                                                                                                                                    |
| `shell_hist`              | `bash` and `zsh` history, PowerShell PSReadLine, `cmd` history                                                                                                                                |
| `cloud_creds`             | `~/.aws/credentials` and `config`, Azure profile and tokens, `gcloud` credentials, `~/.kube/config`, `~/.docker/config.json`, `gh` `hosts.yml`, `rclone.conf`, `~/.netrc`                     |
| `dev_secrets`             | `.pem`, `.key`, `.pfx`, `.p12`, `.pk8`, `.ppk` files under `$HOME`, Composer `auth.json`, FileZilla saved servers                                                                             |
| `container_creds`         | Kubernetes service account token                                                                                                                                                              |
| `cicd_secrets`            | GitLab runner `config.toml`, Jenkins `initialAdminPassword` and `master.key`, `.vault-token`, Terraform `credentials.tfrc.json`, Packer settings                                              |
| `ai_devtools`             | Claude Code, Claude Desktop, Cursor, Windsurf, Copilot, VS Code `mcp.json`, Gemini CLI, Codex `auth.json`, Continue, Aider, Goose, `opencode`, `Qwen`, Crush, `Kiro`, Zed, Hugging Face token |
| `env_vars`                | Its own environment and the environment of other processes through `/proc/<pid>/environ`                                                                                                      |
| `db_litellm`              | Reads `DATABASE_URL` and dumps the `LiteLLM_CredentialsTable` and `LiteLLM_ProxyModelTable` tables, which hold the LLM provider API keys of a LiteLLM proxy                                   |
| `cloud_metadata`          | AWS instance metadata service (IMDSv2, then v1): instance identity and role credentials                                                                                                       |
| `gpg_keys`, `tls_privkey` | GPG keys, TLS private keys                                                                                                                                                                    |

### How it sends the data

1. It encrypts the JSON results with a random AES-256-CBC key, and encrypts that key with RSA-OAEP and the embedded public key.
2. It packs the result into a tar.gz (`hdr.json`, `data.bin`, `key.bin`) and sends it in a POST request to `www.litellm[.]dev/healthcheck` with the header `X-Token: XWiA8gZNa7NcX2XRoAEb`.
3. After a successful upload, it writes `/tmp/.pc_once_72129118c59f5636c0bb` so it collects only once per host. It also sets `PRIVCOLLECT_ACTIVE=1` to avoid a second run in the same process tree.

## Payload 2: the backdoor in sitecustomize.py

Python imports `sitecustomize` at startup, so this file also runs in every Python process.

sitecustomize.py

PYTHON 5 lines

1. It takes an exclusive lock on `/tmp/remote_code_loader.lock`, so only one daemon runs per host.
2. It calls `fork()` and `setsid()`, sends standard input and output to `/dev/null`, and changes directory to `/` to detach from the parent process.
3. Every hour, it:
   1. reads `domain.txt` from the attacker’s GitHub repository and takes the first line that looks like a domain,
   2. runs its embedded copy of the stealer, which has no once-per-host marker,
   3. downloads `http://<domain>/codepy` and runs it with `exec()`.

The attacker can point the daemon at a new server by editing `domain.txt`. Each infected host runs whatever that server returns as `codepy`.

At 13:40 UTC, the GitHub account `letters9813` and its repository returned 404, about two hours after the release. The backdoor now finds no server address and does nothing. Hosts that ran the daemon before the deletion could have received and run second-stage code. SafeDep could not recover the server address.

## Timeline

All times are UTC on 2026-10-21.

| Time           | Event                                                           |
| -------------- | --------------------------------------------------------------- |
| 06:59          | Tag `v1.107.0` commit `66c8f5b` created                         |
| 08:50          | Commit `a8113b0` on `main`, the base of the release source      |
| 10:44          | Legitimate 1.106.2 uploaded to PyPI                             |
| 11:20 to 11:37 | Malicious 1.107.0 wheels built                                  |
| 11:39 to 11:40 | 1.107.0 uploaded to PyPI (all 8 files)                          |
| 11:50          | `litellm-dispatcher[bot]` publishes GitHub release `v1.107.0`   |
| 13:40          | Observed: 1.107.0 gone from PyPI, `letters9813` account deleted |

## Indicators of compromise

### Network

| Indicator                                                                   | Purpose                                                  |
| --------------------------------------------------------------------------- | -------------------------------------------------------- |
| `www.litellm[.]dev`, `hxxp://www.litellm[.]dev/healthcheck`                 | Receives stolen data                                     |
| HTTP header `X-Token: XWiA8gZNa7NcX2XRoAEb`                                 | Sent with each upload                                    |
| `raw.githubusercontent[.]com/letters9813/pythonclient-test/main/domain.txt` | Backdoor reads its server address here (account deleted) |
| `hxxp://<domain>/codepy`                                                    | Second-stage code download                               |
| Requests to `169.254.169.254` from Python processes                         | Theft of AWS instance credentials                        |

### Host

| Indicator                                                                 | Meaning                       |
| ------------------------------------------------------------------------- | ----------------------------- |
| `cook.pth`, `cook.py`, `sitecustomize.py` in `site-packages`              | Implant installed             |
| `/tmp/.pc_once_72129118c59f5636c0bb`                                      | Stealer already uploaded data |
| `/tmp/remote_code_loader.lock`                                            | Backdoor daemon has run       |
| Environment variable `PRIVCOLLECT_ACTIVE=1`                               | Present in infected processes |
| Detached Python daemon (parent PID 1, own session, working directory `/`) | Backdoor loop                 |

### File hashes (SHA-256)

| File                                                    | SHA-256                                                            |
| ------------------------------------------------------- | ------------------------------------------------------------------ |
| `litellm-1.107.0.tar.gz`                                | `2befe2a79fb4fbbd9a543b5b40b530c8cffa8771bab22b8fb22b2e4ae3b1f819` |
| `litellm-1.107.0-cp310-abi3-manylinux_2_28_x86_64.whl`  | `462d0c9f67ca522d9e0e4acd245e90b36ce0d5353604a66bb7c43a1ec4a6fcf7` |
| `litellm-1.107.0-cp310-abi3-manylinux_2_28_aarch64.whl` | `668169d35b87bf51324080390b1b1c5ab12bd335cfe5f1a99e3689abd5e02ae6` |
| `litellm-1.107.0-cp310-abi3-musllinux_1_2_x86_64.whl`   | `1bbce8caa94f01c0aaa0b7ddae51553a8a32e1ce53d96d44fe583272aff7ad00` |
| `litellm-1.107.0-cp310-abi3-musllinux_1_2_aarch64.whl`  | `c2e32d0ce2548aa662c18bf5001911a79fe5b30cf8357132275add02fdcde054` |
| `litellm-1.107.0-cp310-abi3-macosx_10_12_x86_64.whl`    | `7598727f209d717041db37f7dca271d3a7d328a279890e0e1d55375729aeb86d` |
| `litellm-1.107.0-cp310-abi3-macosx_11_0_arm64.whl`      | `0aaf7ba346d41ef2f40390436738b001180d5a1c03aa093f121078a9969d9a1a` |
| `litellm-1.107.0-cp310-abi3-win_amd64.whl`              | `12d4ccc5c6f8d35cf4f2cf233b7f3e418e863e657d81ff5f5fc9e210005b9484` |

The planted files have different hashes on Windows because of the CRLF line endings:

| File               | Linux and macOS                                                    | Windows                                                            |
| ------------------ | ------------------------------------------------------------------ | ------------------------------------------------------------------ |
| `cook.pth`         | `7f191632e7320705d154b85204f1ae546b543acbd7e8861229b76f5042b92bdb` | `84dc8c82c0a4a4a62552ef826b49fcc86d1a5c4a5d0f2c06665723f93d129d7d` |
| `cook.py`          | `9dd0a3926f96a37eb86cf1ee2310d9be93139cc4ebef3ef9e2efe9531390be25` | `03ac2a8b752605d1d7710979c17ff18b03dd978203ba23ba97fd8ce3d6f897ea` |
| `sitecustomize.py` | `34bef04fe88ac9d0aae0475630b1738681d15b94f233ac8aa46eacf5968305fa` | `b6122440d0d9cb1eb1ade9933f49214da8c31288de7710c076c2680dfa528465` |

- pypi
- python
- malware
- supply-chain
- litellm

### Author

![Kunal Singh](https://safedep.io/images/authors/kunal-singh.jpg)

## The Latest from SafeDep blogs

Follow for the latest updates and insights on open source security & engineering

![Background](https://safedep.io/illustrations/bottom.png)

![SafeDep Logo](https://safedep.io/_astro/logo.BdYG7kdO_Z2m03dG.svg)

## Ship Code.

## Not Malware.

Start free with open source tools on your machine. Scale to a unified platform for your organization.
