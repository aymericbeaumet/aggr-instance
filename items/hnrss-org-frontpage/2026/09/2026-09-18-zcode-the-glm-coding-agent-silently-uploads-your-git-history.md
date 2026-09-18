---
title: ZCode, the GLM coding agent, silently uploads your Git history
link: https://tokenstead.ai/guides/zcode-silent-git-history-upload
source: hnrss-org-frontpage
published: 2026-09-18T10:35:28Z
updated: 2026-09-18T10:35:28Z
first_seen: 2026-09-18T16:19:55.454833534Z
authors:
- cdnsteve
summary: 'Article URL: https://tokenstead.ai/guides/zcode-silent-git-history-upload Comments URL: https://news.ycombinator.com/item?id=49752422 Points: 242 # Comments: 55'
content: extracted
html: 2026-09-18-zcode-the-glm-coding-agent-silently-uploads-your-git-history.html
preview:
  file: 2026-09-18-zcode-the-glm-coding-agent-silently-uploads-your-git-history.preview-bdf3f62e5577.webp
  width: 256
  height: 134
  color: '#121619'
images:
- source: https://tokenstead.ai/og/guides/zcode-silent-git-history-upload.png
  original:
    file: 2026-09-18-zcode-the-glm-coding-agent-silently-uploads-your-git-history.image-95973b777bd6.png
    width: 1200
    height: 630
  color: '#07090d'
- source: https://tokenstead.ai/images/articles/zcode-git-upload/snapshot-manifest.svg
  original:
    file: 2026-09-18-zcode-the-glm-coding-agent-silently-uploads-your-git-history.image-a75f66971c35.png
    width: 900
    height: 480
  variants:
  - file: 2026-09-18-zcode-the-glm-coding-agent-silently-uploads-your-git-history.image-8aa0cbe35da4.webp
    width: 320
    height: 171
  - file: 2026-09-18-zcode-the-glm-coding-agent-silently-uploads-your-git-history.image-1f57147f669d.webp
    width: 640
    height: 341
  - file: 2026-09-18-zcode-the-glm-coding-agent-silently-uploads-your-git-history.image-6119a3674535.webp
    width: 900
    height: 480
  color: '#3b1b1b'
- source: https://tokenstead.ai/images/articles/zcode-git-upload/upload-flow.svg
  original:
    file: 2026-09-18-zcode-the-glm-coding-agent-silently-uploads-your-git-history.image-e43915e1ad28.png
    width: 900
    height: 520
  variants:
  - file: 2026-09-18-zcode-the-glm-coding-agent-silently-uploads-your-git-history.image-2bbeb1072d56.webp
    width: 320
    height: 185
  - file: 2026-09-18-zcode-the-glm-coding-agent-silently-uploads-your-git-history.image-89674232d723.webp
    width: 640
    height: 370
  - file: 2026-09-18-zcode-the-glm-coding-agent-silently-uploads-your-git-history.image-b606727fafae.webp
    width: 900
    height: 520
  color: '#1d2738'
- source: https://tokenstead.ai/images/articles/zcode-git-upload/prompt-evidence.svg
  original:
    file: 2026-09-18-zcode-the-glm-coding-agent-silently-uploads-your-git-history.image-22b17f6e90c1.png
    width: 900
    height: 560
  variants:
  - file: 2026-09-18-zcode-the-glm-coding-agent-silently-uploads-your-git-history.image-919ea4433da9.webp
    width: 320
    height: 199
  - file: 2026-09-18-zcode-the-glm-coding-agent-silently-uploads-your-git-history.image-eed4b210acc2.webp
    width: 640
    height: 398
  - file: 2026-09-18-zcode-the-glm-coding-agent-silently-uploads-your-git-history.image-d47110da53ae.webp
    width: 900
    height: 560
  color: '#1c2739'
---

On September 18, 2026, a developer going by ferstar published a reverse-engineering walkthrough of ZCode, the AI coding desktop app from Z.ai, the Beijing-headquartered company behind the GLM family of open-weight models - the same models running on local rigs all over the local-AI community, including GLM-5.3-Flash, tracked on this site. The finding reads worse than most privacy scandals: whenever the app is logged in, it silently packages the user’s entire workspace - complete `.git` history, LFS asset cache, reflogs, and global app configs - encrypts it, and uploads the archive to Aliyun OSS, Alibaba Cloud’s object storage. The researcher’s own capture: a 313MB encrypted archive built from a 345MB commercial workspace, 42,411 files, with 564 failed upload attempts logged while the researcher investigated.

If you run GLM locally, the company that publishes the weights is not the same thing as the runtime a developer might use on top of them - and the thread reaction showed the confusion is live: several commenters assumed ZCode was open source because GLM is. It is not. The weights are open; the harness is closed, and it is Z.ai’s harness for its own models, pitched as first-party integration no third-party editor can match.

The story spread in both languages within hours: ferstar’s post passed 276,000 views, and FeiZ’s Chinese-language alert thread (“disable ZCode for now… it’s still best to use open-source agents as much as possible”) drew another 63,800. The most quoted response came from Petri Kuittinen, whose own AI agent is open sourced with security documentation: “My advice has been and continues to be: do NOT trust closed source AI harnesses.”

The detail that turned a suspicious directory into a story: the encryption key. ZCode uses envelope encryption - the payload is encrypted with a symmetric key, and that key is wrapped with an RSA-OAEP public key. The public key is delivered by the server during upload-credential negotiation. The corresponding private key lives only in Z.ai’s cloud. ferstar attempted to unwrap the archive with every private key on the local system and failed. The 313MB ciphertext sitting on the user’s own disk cannot be decrypted by the user or by the ZCode client itself.

ferstar’s conclusion, from the post: “A key that only the server can use serves exactly one purpose: making sure the server can read your code whenever it wants.”

## What gets packed

The packaging manifest is stored locally in plaintext, and it is specific. For a 42,411-file snapshot:

| Content              | Size     | Share |
| -------------------- | -------- | ----- |
| .git/lfs/            | 196.1 MB | 56.8% |
| .git/objects/        | 102.2 MB | 29.6% |
| .git/logs/           | 0.6 MB   | 0.2%  |
| Source code and docs | 46.2 MB  | 13.4% |

The `.git` directory alone is 86.6 percent of the payload.

![Payload breakdown of one 42,411-file snapshot: the .git directory is 86.6 percent of the encrypted archive.](https://tokenstead.ai/images/articles/zcode-git-upload/snapshot-manifest.svg)

*Payload breakdown of one 42,411-file snapshot: the .git directory is 86.6 percent of the encrypted archive.*

That matters because a git object store is not a snapshot of your working tree - it is the complete lineage of the repository since day one. Deleted-in-a-later-commit API keys are in there. Unpushed branch names that reveal unreleased product plans are in there. Internal hostnames and repo paths from `.git/config` are in there. A captured archive is years of engineering history, not the files you had open.

The upload pipeline, reconstructed from the client’s `app.asar`: the client requests credentials from `zcode.z.ai`, which returns OSS form signatures, an object key, a size cap, and a per-round RSA public key; the client packs the workspace to tar.gz, encrypts with AES-256-CTR, wraps the symmetric key, and POSTs the archive directly to Aliyun OSS, which callbacks to Z.ai’s backend to register the snapshot. The running client maintained persistent connections to `zcode.z.ai` and two Aliyun OSS nodes during the test.

![The reconstructed ZCode snapshot upload flow: credentials from the coordinator, local packing and encryption, direct form POST to Aliyun OSS, callback registration. Reconstructed from the client app.asar by ferstar.](https://tokenstead.ai/images/articles/zcode-git-upload/upload-flow.svg)

*The reconstructed ZCode snapshot upload flow: credentials from the coordinator, local packing and encryption, direct form POST to Aliyun OSS, callback registration. Reconstructed from the client app.asar by ferstar.*

## The toggles do not stop it

The natural move is opening settings. ferstar cross-referenced the UI switches against the code:

- “Optimize Experience” (`optimizeAgentExperienceEnabled`) only controls whether data is authorized for model training. Snapshot capture and upload continue.
- “Repo Snapshot Indexing” (`repoSnapshotIndexingEnabled`) only controls whether the server indexes uploaded snapshots. Local packaging and upload continue.

The host assembly instantiates the capture sidecar unconditionally at startup, with no gating on user preferences - the only requirement is that the token provider can produce a valid JWT. Session logs showed 62 capture events from a single active session, triggered before every prompt and on task completion.

A second source corroborates the mechanism. OrcaPromptVault, a public collection of captured AI harness prompts, holds a 131KB system prompt and a 31-tool surface from ZCode. The checkpoint/rewind feature is wired into the system prompt - the template “Workspace rewind applied. rewindId, checkpointId, strategy, restoredFiles” appears five times. This is the user-facing tip of the snapshot pipeline, the feature the filesystem lock disables.

The agent’s complete tool surface contains zero snapshot, upload, or telemetry tools. The exfiltration pipeline is not an agent tool; it is a host-level sidecar instantiated outside the tool loop. That is why no permission setting stops it, and why the agent itself never sees it. Across 131KB of captured instructions there is no mention of Aliyun, OSS, uploads, or privacy.

The capture adds a detail ferstar did not mention: ZCode ships a ReadSessionContext tool that reads other persisted ZCode sessions on demand by session ID. Combined with the host-level snapshot sidecar, session content is both locally persisted and cloud-captured.

![The leaked system prompt’s checkpoint template (appears five times) and the agent’s 31-tool surface, which contains no snapshot, upload, or telemetry tools.](https://tokenstead.ai/images/articles/zcode-git-upload/prompt-evidence.svg)

*The leaked system prompt’s checkpoint template (appears five times) and the agent’s 31-tool surface, which contains no snapshot, upload, or telemetry tools.*

## The privacy policy does not mention it

ZCode’s privacy policy states the tool collects “text, files, and code submitted during conversations” - the standard inference-context disclosure every AI coding tool makes. Across the policy, FAQ, and changelog, ferstar found no mention of packaging and uploading entire workspaces and git histories. The closest line is a template statement about the optimization program being off by default.

## The context that makes it worse

ZCode launched in July 2026, and its launch pitch ran directly on trust. Z.ai positioned the harness against Anthropic’s Claude Code weeks after the Claude Code hidden-telemetry controversy, with open weights positioned as the escape from the kill-switch problem. A Z.ai executive, asked on X whether ZCode would include “any sort of spyware,” answered that the company would not implement “anything beyond what’s listed” on the ZCode website.

Workspace snapshotting is not listed on the ZCode website.

Z.ai went public on the Hong Kong Stock Exchange in January 2026. The company’s official X account had not responded to ferstar’s post as of publication. The most visible reply came from an account affiliated with the ZCode team - “hey I am sorry to let you find it” - which reads as confirmation of the mechanism, not a rebuttal of it. ferstar’s tweet passed 276,000 views within 13 hours, and discussion threads on V2EX and HN-adjacent channels split mostly along one line: agents upload code fragments during tool calls all the time, with consent. This is a full repository plus its entire history, without consent, encrypted so only the vendor can read it.

## The defense that works

Deleting the pending archive does not work: the client re-packaged a fresh 313MB archive within half an hour, retry counter incrementing. The fix that holds is filesystem-level. Make the checkpoints directory unwritable at the kernel level:

Linux:

```
rm -rf ~/.zcode/v2/checkpoints
mkdir -p ~/.zcode/v2/checkpoints
sudo chattr +i ~/.zcode/v2/checkpoints
```

macOS:

```
rm -rf ~/.zcode/v2/checkpoints
mkdir -p ~/.zcode/v2/checkpoints
chflags uchg ~/.zcode/v2/checkpoints
```

The trade: the checkpoint rollback UI stops working - a feature that required uploading your code in the first place. Chat, autocomplete, and tool calls work normally. Restore with `chattr -i` or `chflags nouchg`.

## What it means for local

Running open weights locally is the pitch: your model, your hardware, no per-token bill, no vendor switch-off. The ZCode story sharpens the point past the model layer. The runtime around the model - the harness, the desktop app, the update pipeline - is part of the trust surface, and a locally-running model wrapped in a cloud-phoning harness is not local.

Two checks follow from this, and they apply to every harness in this space, not only ZCode: what does the runtime transmit when you are logged in, and who can decrypt what it stores. Tokenstead tracks agent harnesses and their telemetry behavior for exactly this reason; this piece will be updated if Z.ai responds with a fix, a disclosure change, or a statement.

Sources:

- [Inside ZCode: Silently Uploading Your Entire Git History to the Cloud](https://blog.ferstar.org/en/posts/zcode-silent-workspace-snapshot-upload/) - ferstar, September 18, 2026 (full forensics: asar reconstruction, encryption flow, manifest breakdown)
- [ferstar on X](https://x.com/ferstar_org/status/2100805861002355154) - the summary thread, 276K views
- [FeiZ on X](https://x.com/Fei2411/status/2100847306203599047) - Chinese-language alert thread, 63.8K views
- [ZCode releases and changelog](https://zcode.z.ai/en/changelog) - Z.ai official
- [V2EX discussion thread](https://www.v2ex.com/t/1242973) - community response, Chinese
- [lookonchain coverage](https://lookonchain.com/feeds/73032)
