---
title: Backdoors in official Claude Code extensions
link: https://cyberplace.social/@GossiTheDog/117528831681803563
source: hnrss-org
published: 2026-10-30T11:05:53Z
updated: 2026-10-30T11:05:53Z
first_seen: 2026-10-30T13:27:01.845275939Z
authors:
- dsr_
content: extracted
html: 2026-10-30-backdoors-in-official-claude-code-extensions.html
---

Anthropic got compromised.

Their Claude Code extension on Visual Studio Code and Cursor has malware in it, which creates a remote access Trojan to desktops.

[https:// phorion.io/blog/offical-claude -code-vsx-compromise/](https://phorion.io/blog/offical-claude-code-vsx-compromise/)

That extension has millions of installs, one of the most popular extensions in DevOps. It creates a reverse SSH backdoor.

Anthropic silently removed the backdoor 7 hours ago and hasn’t disclosed a security incident.

The plugin auto updates so every org will have been impacted.

To be clear this is a supply chain attack, the threat actor got reverse SSH (command line) connectivity to Anthropic customer environments at scale.

FWIW the compromised version, 2.1.313, was removed from repos. 2.1.314 is the ‘fixed’ version.

Anthropic's Claude Code Cursor extension was also compromised, same remote access backdoor [https:// cursor.com/blog/claude-code-ex tension-security-notice](https://cursor.com/blog/claude-code-extension-security-notice)

Anthropic still haven't admitted what has happened.

I've written a blog - Anthropic have been hacked to deliver remote access malware to millions of endpoints where developers use Claude Code.

[https:// doublepulsar.com/anthropic-hac ked-to-deliver-remote-access-trojan-to-claude-developer-laptops-worldwide-1148a92baae2](https://doublepulsar.com/anthropic-hacked-to-deliver-remote-access-trojan-to-claude-developer-laptops-worldwide-1148a92baae2)

Threat actor's C2 server is now offline.

C2 server collected for each victim endpoint:

\- hostname\
\- username\
\- OS\
\- architecture\
\- home directory path\
\- environment variable names\
\- working directories\
\- configured Git email

It then ran a reverse SSH connection allowing the threat actor to pick and chose which victims to have hands on keyboard access to.

I'm confirmed the new, replacement versions Anthropic published overnight (which auto update) do \*not\* remove the prior backdoors and recon activity. So SSH still reverse calls out etc, you need to manually remediate every impacted VSCode and Cursor system.

Anthropic \*must\* tell customers what happened in a completely transparent way.

I've confirmed Binance use Claude Code on Cursor with this extension.

Anthropic won't comment, nor are account managers.
