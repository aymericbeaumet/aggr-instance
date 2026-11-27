---
title: How to Run Untrusted Code Safely
link: https://fly.io/blog/running-untrusted-code/
source: fly-io
published: 2026-02-02T00:00:00Z
updated: 2026-11-06T16:35:06Z
first_seen: 2026-11-27T00:01:05.630507547Z
content: extracted
html: 2026-02-02-how-to-run-untrusted-code-safely.html
remote_preview:
  url: https://fly.io/blog/running-untrusted-code/assets/untrusted.jpg
  alt: Running AI-generated and untrusted code in isolated Sprites
extra:
  thumbnail: https://fly.io/blog/running-untrusted-code/assets/untrusted.jpg
---

Author

![Daniel Botha](https://fly.io/static/images/danielb.webp)

Name

Daniel Botha

![Running AI-generated and untrusted code in isolated Sprites](https://fly.io/blog/running-untrusted-code/assets/untrusted.jpg)

AI generates code faster than you can review it. Your agent wants to [run a Python script](https://fly.io/learn/run-llm-generated-python-safely/ ""). Your tool wants to execute user-submitted code. Your pipeline wants to test generated functions. All things you don’t want to be running locally or in production unless you’re just trying to add some drama to your life right now.

If you want to walk the path of peace, you use a [sandbox](https://fly.io/learn/agent-sandbox/ ""). But as any kindergartener will tell you: not all sandboxes are created equal.

## The Container Problem

The default for the longest time has been “spin up a container.” This is safer than running dodgy code on your machine. It’s also not good enough.

[Containers share the host kernel](https://fly.io/learn/microvm-vs-container/ ""). A kernel exploit in your untrusted code == emotional trauma. NIST SP 800-190 calls container escapes one of the most critical threats to container security. MITRE ATT&CK classifies it as technique T1611. The threat is real.

Truly untrusted code needs hardware isolation. A dedicated kernel. A non-fictional boundary between the code and everything else.

## The Network Problem

Filesystem isolation helps, but code that can’t touch your files can still:

- Exfiltrate data to an attacker’s server
- Download additional payloads
- Call out to command-and-control infrastructure
- Use your compute. Which isn’t free

Network egress control should not be considered optional. The code needs to run in an environment where outbound connections are explicitly allowed, not implicitly permitted.

## What You Really Need From a Sandbox

1. **Hardware isolation**
2. **Network control** — allowlist-based, not denylist-based
3. **External policy enforcement** — the code can’t modify its own permissions
4. **Rollback capability** — undo damage quickly when something goes wrong
5. **Multiple runtimes** — AI writes Python, JavaScript, Rust, Go, whatever

Spin up and configure your sandboxes with these features and you should be safe.

Or just use [Sprites](https://fly.io/sprites/ "").

## Setting Up a Code Runner in a Sprite

Create a Sprite for running untrusted code.

```wrapper
sprite create code-runner
```

Then, lock down its [network](https://docs.fly.io/sprites/concepts/networking ""):

```wrapper
curl -X POST https://api.sprites.dev/v1/sprites/{sprite-id}/policy/network \
  -H "Authorization: Bearer $SPRITES_API_TOKEN" \
  -d '{"rules": [{"domain": "*", "action": "deny"}]}'
```

This is a deny-all policy. The code inside cannot make any outbound connections. DNS queries return `REFUSED`. Raw IP connections are blocked.

What’s super important is that this policy is set via the external API. Code running inside the Sprite cannot change it. Even if the code has root access, it cannot modify its own network policy.

## Running Code

Proceed to laugh in the face of danger:

```wrapper
# runner.py - runs inside the Sprite
import subprocess
import sys

def run_untrusted(code: str, language: str = "python") -> dict:
    """Execute code and capture output."""

    if language == "python":
        result = subprocess.run(
            ["python", "-c", code],
            capture_output=True,
            text=True,
            timeout=30
        )
    elif language == "node":
        result = subprocess.run(
            ["node", "-e", code],
            capture_output=True,
            text=True,
            timeout=30
        )
    # Add other languages as needed

    return {
        "stdout": result.stdout,
        "stderr": result.stderr,
        "returncode": result.returncode
    }
```

Expose this as an HTTP endpoint on port 8080. Another Sprite acting as an orchestrator, or any external service can call this with code to execute.

## Selective Network Access

Sometimes code needs *some* network access. An AI agent writing code that calls an API. A script that needs to fetch data from a specific endpoint.

Allow only what’s needed:

```wrapper
curl -X POST https://api.sprites.dev/v1/sprites/{sprite-id}/policy/network \
  -H "Authorization: Bearer $SPRITES_API_TOKEN" \
  -d '{
    "rules": [
      {"domain": "api.openai.com", "action": "allow"},
      {"domain": "api.anthropic.com", "action": "allow"},
      {"domain": "*", "action": "deny"}
    ]
  }'
```

Now the code can call OpenAI and Anthropic. Nothing else. Can’t exfiltrate to gimmeallyourdata.com. Can’t download from sketchy-cdn.net.

## When Code Breaks the Sandbox

In the entirely unsurprising case that something does blow up; the code did something weird to the filesystem, filled the disk, corrupted something.

Sprites are disposable. Just `sprite destroy` it and spin up a fresh one. Problem solved.

Or, if you’re running lots of code and want to reuse the same Sprite for efficiency, checkpoint your working state

```wrapper
sprite checkpoint create
```

Then restore to that working state in about a second:

```wrapper
sprite checkpoint restore <working-checkpoint-id>
```

## Runtimes Are Already There

Sprites come with Python, Node, Ruby, Rust, Go, Java, Elixir, Bun, and Deno pre-installed. Version managers let you switch between versions. Your code runner doesn’t need to install anything. Just execute.

Sprites sleep when idle and wake on demand. Your code runner costs nothing when it’s not running code.

Run whatever you want. Dare it try any funny business.

Next post ↑

[I Build Everything In Sprites Now](https://fly.io/blog/i-build-everything-in-sprites-now/)

Previous post ↓

[Litestream Writable VFS](https://fly.io/blog/litestream-writable-vfs/)
