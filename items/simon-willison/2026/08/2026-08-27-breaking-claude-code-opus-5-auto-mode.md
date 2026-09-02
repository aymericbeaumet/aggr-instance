---
title: Breaking Claude Code Opus 5 Auto Mode
link: https://simonwillison.net/2026/Aug/27/breaking-claude-code-opus-5-auto-mode/
source: simon-willison
published: 2026-08-27T22:50:25Z
updated: 2026-08-27T22:50:25Z
first_seen: 2026-09-02T14:07:29.769486161Z
tags:
- sandboxing
- security
- ai
- prompt-injection
- generative-ai
- llms
- anthropic
- claude
- johann-rehberger
- claude-code
summary: 'Breaking Claude Code Opus 5 Auto Mode Anthropic are putting a great deal of faith in Claude Code''s auto mode for protecting their coding agent users against prompt injection attacks. They recently made that the default and have made bold claims about its effectiveness. Johann Rehberger is one of the most credible prompt injection researchers active today. He found an attack against auto mode which he claims works 80% of the time, by tricking Claude Code into downloading and uncompressing a zip archive, then executing code that imports base64 without noticing that this will import and execute a local struct.py file extracted from the archive. In a few cases auto mode directly prevented the agent from preventing harmful code from continuing to execute! In a few runs Claude tried to terminate the malware process once it noticed the compromise, but Auto Mode denied the cleanup command. Claude detects the compromise, but Auto Mode blocks its cleanup command The safety mechanism itself can become part of the failure. The classifier allowed the creation of the malware process, but then it blocked the command intended to stop it! I agree with Johann''s conclusion here: the only safe way to run agents if there''s any risk of attracting the attention of an adversarial attack is with a sandbox: Run unattended coding agents in a container, VM or OS sandbox. Restrict network egress. Monitor your agents. Do not expose home directories, SSH keys, cloud credentials,… to the agent runtime. [...] Update 30th August 2026: On Lobste.rs hyperpape points out that this doesn''t fit the bill of a classic prompt injection attack because at no point are malicious instructions from the website accidentally followed by the LLM. They''re right: this is more of a confused environment attack where the nature of the environment that the agent is exposed to results in an exploit. Tags: sandboxing, security, ai, prompt-injection, generative-ai, llms, anthropic, claude, johann-rehberger, claude-code'
content: feed
html: 2026-08-27-breaking-claude-code-opus-5-auto-mode.html
---

**[Breaking Claude Code Opus 5 Auto Mode](https://embracethered.com/blog/posts/2026/breaking-claude-code-opus-5-and-automode/)**

Anthropic are putting a great deal of faith in Claude Code's auto mode for protecting their coding agent users against prompt injection attacks. They recently [made that the default](https://simonwillison.net/2026/Aug/8/auto-mode/) and have made bold claims about its effectiveness.

Johann Rehberger is one of the most credible prompt injection researchers active today. He found an attack against auto mode which he claims works 80% of the time, by tricking Claude Code into downloading and uncompressing a zip archive, then executing code that imports `base64` without noticing that this will import and execute a local `struct.py` file extracted from the archive.

In a few cases auto mode directly prevented the agent from preventing harmful code from continuing to execute!

> In a few runs Claude tried to terminate the malware process once it noticed the compromise, but Auto Mode denied the cleanup command.
>
> Claude detects the compromise, but **Auto Mode blocks its cleanup command**
>
> The safety mechanism itself can become part of the failure. The classifier allowed the creation of the malware process, but then it blocked the command intended to stop it!

I agree with Johann's conclusion here: the only safe way to run agents if there's any risk of attracting the attention of an adversarial attack is with a sandbox:

> - Run unattended coding agents in a container, VM or OS sandbox.
> - Restrict network egress.
> - Monitor your agents.
> - Do not expose home directories, SSH keys, cloud credentials,… to the agent runtime. \[...\]

**Update 30th August 2026**: On Lobste.rs [hyperpape points out](https://lobste.rs/s/ktbweg/prompt_injection_claude_code_opus_5_auto#c_gi7eqj) that this doesn't fit the bill of a classic prompt injection attack because at no point are malicious instructions from the website accidentally followed by the LLM. They're right: this is more of a confused environment attack where the nature of the environment that the agent is exposed to results in an exploit.

Tags: [sandboxing](https://simonwillison.net/tags/sandboxing), [security](https://simonwillison.net/tags/security), [ai](https://simonwillison.net/tags/ai), [prompt-injection](https://simonwillison.net/tags/prompt-injection), [generative-ai](https://simonwillison.net/tags/generative-ai), [llms](https://simonwillison.net/tags/llms), [anthropic](https://simonwillison.net/tags/anthropic), [claude](https://simonwillison.net/tags/claude), [johann-rehberger](https://simonwillison.net/tags/johann-rehberger), [claude-code](https://simonwillison.net/tags/claude-code)
