---
title: A Practical Guide to Security in November 2026
link: https://cognition.com/blog/practical-guide-to-security-november-2026
source: cognition-com
published: 2026-11-03T17:00:00Z
first_seen: 2026-11-04T00:52:13.198927099Z
content: extracted
html: 2026-11-03-a-practical-guide-to-security-in-november-2026.html
remote_preview:
  url: https://cognition.com/images/practical-guide-to-security-november-2026/cover.png
  alt: A Practical Guide to Security in November 2026
---

I wouldn’t blame you if you’re having trouble keeping up with all of the security incidents over the last few weeks.

Over a two-week period, we saw the [largest theft in recorded history](https://en.wikipedia.org/wiki/2026_Binance_hack), a PHP vulnerability [so critical that governments took their websites down](https://www.reuters.com/world/asia-pacific/eu-agency-warns-websites-may-need-be-taken-offline-php-prepares-patches-2026-10-21/), and a [compromised Claude Code Cursor extension](https://cursor.com/blog/claude-code-extension-security-notice) that tried to backdoor developer machines. Not to mention critical vulnerabilities in edge devices, a GitLab remote code execution vulnerability, and a certificate authority that set its private key to the number 4.

So, what do we make of this? Is it time to toss out all of our computers? Not quite.

The good news is that the security best practices we’ve known about for years still work. An attacker doesn’t need sophisticated AI to exploit an unpatched server or use a stolen credential, and when companies leave those paths open, techniques that have worked for years still succeed. AI makes attacks faster and cheaper, but it doesn’t stop you from closing off entire, basic attack paths. What’s changed is the urgency. In November 2026, you can’t wait and hope you won’t be targeted.

In this blog post, we’ll recap some of these recent events, and share a practical guide that any company can use to step up your security now.

## A quick overview of the most chaotic few weeks in security history

### The Binance Hack

The largest theft in recorded history started with malware on developers’ machines. While many details aren’t yet known around the incident that allowed [North Korean state-sponsored hackers](https://www.fbi.gov/investigate/cyber/alerts/2026/north-korea-responsible-for-6-billion-binance-attack) to siphon [$6.34 billion](https://x.com/binance/status/2117626126214504729) from Binance, Binance has reported that several engineers’ laptops were compromised through a fake job interview and a tampered installer, and a deployment token on one of them let the attackers reach Binance’s wallet signing service. A compromised developer machine should never be enough to compromise production environments, let alone steal billions of dollars.

### The Claude Code Cursor Extension Compromise

On October 29, Cursor disclosed that an adversary published a malicious version of the Claude Code Cursor extension, one of the most popular Cursor extensions. The malicious extension attempted to install a persistent backdoor on developers’ machines, allowing the attacker to SSH into the user’s device. As extensions auto-update, this means that users could have received the malicious extension without taking any actions. Cursor reported that fewer than 5,000 users installed the malicious extension.

Notably, the version of the extension differed from what was published on the Open VSX registry (the open extension registry that Cursor uses). This suggests that the adversary compromised the extension publishing process downstream of Open VSX.

### A Whole Host of Critical Vulnerabilities

Beyond these incidents, a wide range of critical vulnerabilities were disclosed in some of the world’s most widely used software.

- **PHP2Shell.** Perhaps most notably, the PHP2Shell vulnerability exposed millions of websites due to a memory safety flaw in the PHP runtime. PHP runs roughly 70% of websites, and in some setups a single unauthenticated request could trigger the bug before any application code ran.
- **GitLab.** An unauthenticated remote code execution vulnerability in GitLab, which left many companies’ most critical source code at risk.
- **Edge devices.** Another round of critical bugs in VPNs, firewalls, and other internet-facing appliances. These are a [go-to target](https://www.cisa.gov/news-events/cybersecurity-advisories/aa24-038a) for nation-state adversaries.
- **eMudhra’s private key.** The certificate authority eMudhra set its private key to the number 4, which means anyone could have issued certificates in its name.
- **KVM.** A [guest-to-host escape](https://x.com/rauchg/status/2106402024804020657) in KVM, found through Vercel’s Sandbox bounty program, let code inside a VM get root on the host machine.
- **The Linux TCP stack.** [CVE-2026-98432](https://nvd.nist.gov/vuln/detail/cve-2026-98432) lets an unauthenticated remote attacker get root on Linux systems running a Multipath TCP listener, which Go 1.24+ enables by default.

## What you should do

Amid increasing uncertainty, it can be hard to tell what’s actually important to do. We’ve laid out two sets of actions: what you should do immediately, and what you should do within 30 days. Taking these steps can significantly shore up your company’s security posture, and protect against both threats we’re seeing today and in the future.

### Secure your corporate environment

These are some of the lowest-lift changes you can make to prevent the most common attacks.

Immediately, you should:

1. **Require phishing-resistant MFA and device-bound sign-in** on your corporate accounts, starting with high-risk accounts like Google Workspace, GitHub, your cloud provider, and your identity provider. Use passkeys or hardware security keys, not SMS authentication, and only allow sign-in from managed devices. Phishing-resistant MFA is one of the simplest and most effective security controls you can put in place, ensuring that even if an employee gets tricked via a phishing attack, their account doesn’t get compromised.
2. **Audit your public-facing attack surface.** Point a coding agent at your public domains and understand the extent to which your systems are exposed. Assume that any public system is already being targeted by attackers with frontier models.
3. **Roll out a password manager to your employees.** Password reuse is still one of the most common attack vectors. Password managers have the added bonus of helping protect against phishing attacks by only filling passwords on the correct domain.
4. **Roll out a reputable ad blocker to all employees.** Binance’s attack chain started when an engineer searched for an AI coding tool and downloaded a tampered installer. Malicious search ads and lookalike download pages are one of the most common ways malware reaches developer laptops, and an ad blocker (e.g., [uBlock Origin](https://github.com/gorhill/ublock)) removes a large share of them.

Within 30 days, you should:

1. **Put every corporate device under MDM and EDR.** Use them to control what software can be installed and to get visibility when something on a laptop starts behaving badly.
2. **Lock down browser and editor extensions.** Maintain an allowlist of approved Chrome extensions (e.g., via [Chrome Enterprise](https://chromeenterprise.google/)) and VS Code extensions, and configure an auto-update delay for extensions. This helps protect against compromises of popular extensions.
3. **Audit your entire corporate attack surface.** As we saw with the Binance attack, motivated adversaries can take advantage of chained vulnerabilities in your development pipeline. Assess your entire corporate attack surface for various scenarios, such as if an engineer’s device is compromised, and ensure that even one compromised device doesn’t give an entry point to prod.

### Secure your development environments

The Binance hack showed that it’s a very bad idea to keep your most critical access on a developer’s machine. Developer laptops run a huge amount of third-party code: package installs, editor extensions, MCP servers, and now local AI agents that can execute commands on their own. Any one of those can be compromised, and when it is, the attacker inherits everything that developer can reach.

Immediately, you should:

1. **Scan developer machines for secrets.** Using a tool like [TruffleHog](https://github.com/trufflesecurity/trufflehog), look for cloud credentials, API keys, SSH keys, and `.env` files sitting in home directories, shell history, and repos. Wherever possible, remove production secrets from developer machines.
2. **Audit installed extensions, plugins, and MCP servers.** Get an inventory across your team of what’s installed in editors, browsers, and AI tools. Remove anything nobody needs, and pay close attention to anything that has access to the filesystem, a shell, or credentials.

Within 30 days, you should:

1. **Get secrets off developer machines.** Move them into a secrets manager and heavily lock down any production secrets. Rotate anything you found in the scan above.
2. **Move to cloud coding agents and remote development environments.** The most reliable way to limit the damage from a compromised extension, package, or agent is to keep it off the laptop entirely. Remote dev boxes and cloud agents run in an environment you control, can be given only the access a specific task needs, and are much easier to monitor and wipe than a fleet of laptops.

### Secure your code

Immediately, you should:

1. **Run a frontier model on your codebases**, and critical open source dependencies, starting with anything internet-facing or that handles authentication, payments, or customer data.
2. **Scan your source code, images, and build artifacts for secrets.** Keys embedded in a published Docker image or a build artifact are just as exposed as keys committed to a public repo.

Within 30 days, you should:

1. **Make a plan for refactoring your critical codebases.** The PHP2Shell vulnerability was a memory safety vulnerability in PHP’s runtime. We’re running out of excuses for continuing to write code in memory-unsafe languages. Understand how your attack surface can be reduced by wide-scale refactors (such as moving to memory-safe languages or parameterized queries), and make a plan to complete those within the next few months.
2. **Give the scanner your threat model.** A generic, out-of-the-box prompt produces a lot of noise. Results are much better when the harness knows how your code is deployed, what is reachable from the internet, and what matters most to protect.

### Secure your third-party dependencies

We’ve seen numerous worm-style supply chain attacks, like the Shai-Hulud attacks over the last year.

Immediately, you should:

1. **Set a minimum package age of at least 7 days** for npm, PyPI, and any other package manager you use, either in the package manager’s config or in a proxy or registry that sits in front of it. This protects against most worm-style supply chain attacks, since malicious releases are usually caught and pulled within that window.
2. **Pin versions and commit your lockfiles**, and make CI install from the lockfile instead of resolving fresh versions on every build.

Within 30 days, you should:

1. **Audit and trim your dependencies.** Remove dependencies you don’t use, and look closely at ones that are unmaintained or have very few maintainers.

### Monitor your infra

Attacks are more rampant than ever, and without monitoring they can go unnoticed.

Immediately, you should:

1. **Make sure the logs you’d need actually exist**: identity provider sign-ins, cloud audit logs, source control and CI activity, and access to production data. Keep them somewhere an attacker with production access can’t delete them.
2. **Alert on the high-signal events**: new admin accounts, MFA changes, access from new devices or locations, large data exports, and secrets being read from unusual places. Ensure that these alerts are immutable and behind corporate auth.

## Conclusion

The biggest exploits and vulnerabilities of the last few weeks came down to known vulnerability classes and basic lapses in corporate security. Binance lost billions to a tampered installer, a fake job interview, and a deployment token left on a laptop. New CVEs will keep coming, and frontier models mean they’ll be found and exploited faster than before. Ask your security team which of the above steps they’re taking, and aim to knock out a few today!
