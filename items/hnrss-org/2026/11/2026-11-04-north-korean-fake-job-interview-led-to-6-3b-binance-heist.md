---
title: North Korean fake job interview led to $6.3B Binance heist
link: https://www.theregister.com/security/2026/11/03/north-korean-fake-job-interview-led-to-63b-binance-heist/5307398
source: hnrss-org
published: 2026-11-04T14:09:55Z
updated: 2026-11-04T14:09:55Z
first_seen: 2026-11-04T15:43:16.719955117Z
authors:
- smalltech
labels:
- binance
- cryptocurrency
- north korea
- security
- supply chain attack
content: extracted
html: 2026-11-04-north-korean-fake-job-interview-led-to-6-3b-binance-heist.html
remote_preview:
  url: https://image.theregister.com/254036.jpg?imageId=254036&x=0&y=0&cropw=100&croph=100&panox=0&panoy=0&panow=100&panoh=100&width=1200&height=683
---

REG AD

[security](https://www.theregister.com/tag/security)

'It was not a single isolated breach or a simple phishing incident,' exchange says

North Korean cryptocurrency thieves tricked a Binance software engineer into downloading malware onto their work-issued computer during a fake job interview process before last week's [record-breaking $6.3 billion heist](https://www.theregister.com/security/2026/10/30/in-the-year-ai-re-wrote-the-infosec-rulebook-its-comforting-to-know-crypto-exchanges-still-have-bad-security/5306539).

In an incident report published on Tuesday, the cryptocurrency exchange said several techniques - including social engineering via fake job interviews, malware downloads, compromised internal AI tooling, endpoint takeover, and an “advanced transaction-authorization method” - led to the October 29 security breach.

“Our current investigation indicates that this was a sophisticated, multi-stage campaign,” the [incident report](https://www.binance.com/en/blog/community/3229153443759566763) said. “It was not a single isolated breach or a simple phishing incident.”

REG AD

Over the weekend, the [FBI attributed the theft](https://www.theregister.com/security/2026/11/02/fbi-pins-63b-binance-crypto-heist-on-north-korean-hackers/5306889) to TraderTraitor, a North Korean hacking operation linked to [Lazarus Group](https://www.theregister.com/security/2026/02/24/lazarus-group-targets-healthcare-orgs-with-medusa-ransomware/4589498), known for combining espionage with [massive cryptocurrency heists](https://www.theregister.com/cyber-crime/2026/09/25/bitget-blames-north-korea-for-3875m-crypto-wallet-raid/5299218). The Pyongyang-backed crew is also highly adept at [job market scams](https://www.theregister.com/security/2026/03/18/north-koreans-100k-fake-it-workers-net-500m-a-year-for-kim/5224468) - both [posing as IT workers](https://www.theregister.com/security/2025/07/13/fake-north-korean-it-workers-how-companies-can-stop-them/759160), who are then [hired by Western companies](https://www.theregister.com/security/2025/12/18/amazon-blocked-1800-suspected-dprk-job-applicants/2905609), and as [potential employers](https://www.theregister.com/security/2026/06/08/suspected-norks-send-250-fake-dev-job-pitches-to-steal-crypto/5252526) to lure software engineers into [downloading malware](https://www.theregister.com/cyber-crime/2026/07/30/amazon-links-four-poisoned-npm-packages-to-one-north-korean-crew/5281120) and [infecting corporate devices](https://www.theregister.com/security/2026/09/18/north-koreas-fake-job-interviews-infected-30000-devices/5297461).

REG AD

With the recent Binance attack, the Norks used the latter.

The attack prep began back in June with a supply chain attack when one of its engineers downloaded a [malicious Homebrew installation](https://isc.sans.edu/diary/32942). While the malware was detected by the security team, the device was replaced, and all affected credentials rotated, “Engineer A’s role and his related colleagues on our wallet infrastructure team was exposed to the hacker,” the report says.

Three months later, the social engineering part of the attack happened.

In September, another software engineer was contacted via a “legitimate job-hunting platform” by someone purporting to be a project director who arranged a series of job interviews for the Binance engineer with the fake company. During the first round - a live video interview with the engineer and a fake HR rep - the interviewer “shared a social media account with him, including personal photos and videos posted on the platform, in order to build trust,” according to the report. “This was more advanced and deceptive than previously seen social engineering approaches.”

The second interview with someone purporting to be the CEO of the phony company was conducted on a “popular social chat app,” and toward the end, the CEO instructed the engineer to download an “assignment” containing a malware-laced code repository. The code executed on his work computer during the setup process, which kicked off a series of unauthorized activities.

According to Binance, the malware replaced a legitimate development tool component with a malicious version, which executed commands and established communication with an attacker-controlled external server. The attackers also took steps to hide the malicious script and evade detection, and it worked. Binance’s endpoint security tools did not immediately detect the malware.

Here’s the script:

CREATE  /opt/homebrew/Cellar/pyenv/2.8.0/libexec/pyenv\_

REG AD

MODIFY  pyenv\_ → 3,414 B

DELETE  .../libexec/pyenv  (3,344 B)      ← remove the valid pyenv

RENAME  pyenv\_ → pyenv  (3,414 B)  ← replace it with a malicious version

DELETE  /opt/homebrew/bin/pyenv  (3,414 B)

ln  (create a new symlink)  +  spawn zsh

Then, on October 20, the miscreants used the compromised computer and an internal AI tool to gain access to the computers of wallet DevOps engineer A and another wallet software developer, Engineer C.

Engineer A had a container deployment permission token on his computer, which the Norks soon discovered. On October 29, the attacker accessed Engineer C's computer, used the stolen credentials from Engineer A to deploy a container, and triggered a script to launch the cryptocurrency-stealing attack.

As of Tuesday, Binance said it has rebuilt the compromised wallet system and internal AI tool, and replaced all of the affected devices. It also decommissioned all credentials and tokens assigned to the compromised engineers, and is working with law enforcement, blockchain intel firms, and security researchers to trace and freeze the assets.

REG AD

The crypto exchange has also hired independent cybersecurity experts to conduct a cybersecurity review of the incident, and recommend additional protective measures. Binance said it’s also working to establish a security research fund and initiatives with AI companies and security shops to focus on AI security in blockchain environments.®
