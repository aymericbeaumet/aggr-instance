---
title: 'Patch the Planet: global response coordination as agents uncover critical vulnerabilities'
link: https://openai.com/index/coordinated-disclosure-of-critical-vulnerabilities
source: openai-com
published: 2026-10-21T07:00:00Z
updated: 2026-10-21T07:00:00Z
first_seen: 2026-10-21T19:14:42.600331207Z
labels:
- security
summary: Patch the Planet connects security researchers, maintainers, and incident responders to help address critical vulnerabilities across open source.
content: extracted
html: 2026-10-21-patch-the-planet-global-response-coordination-as-agents.html
---

Over the past two months, we led a coalition of maintainers, researchers, and security providers to address critical vulnerabilities in PHP, an open-source programming language used to run websites and web applications. Calif identified a critical flaw using GPT‑5.6 Sol as part of our [Patch the Planet](https://patchtheplanet.org) initiative, with Wiz also independently discovering and reporting both vulnerabilities. PHP’s maintainers led the fixes and release, with our teams and partners like Wiz, Cloudflare, and others contributing research, technical support, and coordination to prepare protections ahead of disclosure.

For organizations using PHP, the priority now is applying the fixes. [**We’re sharing a PHP version checker as a Codex skill**](https://github.com/openai/php2shell-checker) to help operators identify potentially vulnerable PHP versions and, alongside PHP’s and their providers’ guidance, determine which updates apply.

Given the severity of these flaws and PHP’s widespread use, we consider this one of the most impactful security issues in recent memory. It highlights how AI can uncover security flaws with internet-scale impact, but the same capabilities can also help attackers rapidly develop offensive tools and exploits.

To our knowledge, frontier AI found these vulnerabilities before attackers did, opening a [Defender’s Window](https://openai.com/index/the-defenders-window/) for maintainers and security providers to prepare fixes and protections ahead of disclosure.

To help more defenders use these capabilities, we’ve committed $1 billion in subsidized [Daybreak access](https://openai.com/form/enterprise-trusted-access-for-cyber/) through [Daybreak for Frontline Defenders](https://openai.com/index/daybreak-for-frontline-defenders/), focused on organizations with limited security resources protecting essential services.

As significant as they are, these bugs are among several significant vulnerabilities we’ve already found and are working to fix, and we expect similar findings to increase sharply in the coming months. Drawing on our experience thus far, we’re sharing a proposed approach, appropriate for the AI era, to responding to critical vulnerabilities in widely used software, and we’re asking the security community to help improve this framework. It brings together our models, security expertise, and maintainers to help turn findings into protection and build a response that can scale as AI capabilities advance.

## Coordinating a global response

Getting a fix into use can mean adapting it for older releases, preparing provider mitigations, and updating systems across many organizations. A repeatable process helps those teams act quickly as more vulnerabilities are discovered.

Through Patch the Planet, we contributed security expertise and model access to support the PHP response, and worked with maintainers and research partners to test fixes and prepare guidance. We also helped coordinate with infrastructure providers on protections for affected systems and engaged government cyber agencies to help guidance reach critical organizations. This supported the maintainer-led response and helped other defenders prepare.

For some findings, researchers can work directly with the software’s maintainers to investigate the issue and develop a fix. Others need a wider response because they affect many products or because distributors and service providers need time (which may be in short supply if frontier capabilities are not too far ahead of those that are broadly available) to prepare updates and protections. In those cases, the proposed framework can help maintainers identify who needs to be involved and coordinate the work. The response can involve five stages:

- **Validate and fix the upstream vulnerability.** The maintainer, researchers, and essential technical partners confirm the finding, assess its impact, and develop and test a fix. This process includes adversarial testing with frontier models to look for ways around the patch and related variants of the vulnerability.

- **Give affected organizations time to act.** We propose a framework for considering which organizations need advance information to prepare protections, while maintainers decide whom to involve. We help with outreach, information-sharing agreements, and coordination between teams. A distributor might adapt a fix for an older release, a hosting provider might test a mitigation, and a security provider might prepare detection or prevention measures, including web application firewall (WAF) rules.

- **Coordinate with government partners to help protect critical systems.** Government agencies play an essential role in coordinating a wider response and helping security guidance reach organizations responsible for critical infrastructure and other essential services. In the case of PHP, we worked, in collaboration with our partners, to engage with multiple parts of the U.S. government, including the White House’s Super Intelligence Force, the Office of the National Cyber Director (ONCD), the Cybersecurity and Infrastructure Security Agency (CISA), as well as international partners to support that effort.

- **Prepare updates across affected products.** As the fix, guidance, and disclosure plans become stable, maintainers can involve additional distributors, hosting and service providers, response teams, and operators to prepare updates and address gaps in coverage.

- **Once a patch is available, publish guidance and support deployment.** Public guidance helps people assess their exposure, find authoritative updates or interim mitigations, and check that a fix is running. The response continues as operators deploy updates and report where they need help.

The proposed framework considers an organization’s protective role, including protecting its own systems and customers, the time needed to act, and its ability to handle sensitive information. The same criteria would apply to comparable organizations, with maintainers deciding whom to involve ahead of disclosure.

Each participant receives the information needed to carry out their protective work, from network logs with indicators of compromise to more sensitive working proof-of-concept exploits. Even limited details can reveal a vulnerability, so we weigh the risk of sharing them against the protection they enable.

Teams following this framework would work with maintainers to give defenders time to prepare and reassess the timeline if there’s active exploitation in the wild, a leak, a revealing public patch, or increased risk to users from stalled remediation.

The [Patch the Planet website](https://patchtheplanet.org/) brings together public disclosures and guidance to help operators identify affected systems, find the right update, and verify that the updated software is running properly. For PHP, it points to the maintainers’ official patch and upgrade instructions and our partners’ technical research. The guidance also covers interim mitigations, when to involve a provider or expert, and backup and recovery. Interim protections can reduce risk, but we’ll make their limits and operational tradeoffs clear.

Operators can use [CISA’s guidance on prioritization and remediation](https://www.cisa.gov/news-events/directives/bod-26-04-prioritizing-security-updates-based-risk) to assess how quickly to act based on exposure, exploitation, and potential impact. That may mean emergency patching, immediate mitigation, or a planned update, alongside forensic triage where warranted to assess whether systems have already been compromised. Priorities should be reassessed as conditions change, with a clear path to lasting remediation.

## Giving maintainers the capacity and resources to respond

Global coordination depends on the people who maintain the software. Many projects are run by small teams that review security reports alongside routine development. Those teams need help separating new issues from duplicates, assessing impact, and testing fixes across supported releases. We support that work with funding, access to capable models, and hands-on engineering help.

Powerful AI capabilities are advancing across the industry, and we expect AI to play a growing role in helping defenders find vulnerabilities, monitor systems, and respond to threats. We’re investing in models, tools, and partnerships to help defenders put those capabilities to work.

Our investments include subscriptions for all maintainers through [Codex for OSS](https://openai.com/form/codex-for-oss/) and direct sponsorship of projects such as the [Rust Foundation](https://rustfoundation.org/media/rust-foundation-welcomes-openai-as-platinum-member-announces-donation-to-rust-project/) for maintainers and technical work. Each project sets its own priorities. Reusable tests and security workflows can reduce the effort needed to handle future findings. Open-source projects can [apply to Patch the Planet](https://trailofbits.com/patch-the-planet/#apply) for hands-on support.

We’re also calibrating additional blocking classifiers for Daybreak to help reduce the potential for misuse. This involves difficult tradeoffs: safeguards intended to prevent abuse can also block legitimate security work. Broad access to capable models and effective protections against misuse are both critical. Our aim is to preserve the capabilities defenders need to find and fix vulnerabilities while limiting harmful use. We’ll continue refining that balance as capabilities advance and we learn from how these tools are used.

## Building more secure software foundations

As models become more capable, the opportunity extends beyond finding and fixing individual vulnerabilities. We want to help engineers build more secure software from the start. Improvements to a widely used component can prevent bugs or limit their impact across many products and services.

AI can help engineers understand existing code, prepare changes, and develop tests, while maintainers and security experts verify that those changes preserve the behavior users depend on. We see opportunities in several areas:

- **Secure code from the start.** Coding models can help developers choose safer defaults, use security libraries correctly, and catch risky patterns as they write. Preventing vulnerabilities during development means fewer emergency fixes for maintainers and users.

- **Memory-safe software.** Moving critical components to memory-safe languages can eliminate common classes of memory errors. AI can help with migrations, with tests and expert review to check behavior and compatibility.

- **Formal verification.** Mathematical proofs can show that critical components satisfy specified security properties under stated assumptions. Applying these methods to widely used components could help rule out classes of vulnerabilities across the services and critical infrastructure that depend on them.

- **Stronger isolation.** Systems need protection against vulnerabilities that have not yet been found or fixed. Running software that processes untrusted files, images, or media in tightly restricted sandboxes can limit the damage if it is compromised. AI can help engineers test and strengthen widely used sandboxing technologies, improving protections across many applications.

## Help us improve this approach

We’re sharing our approach so maintainers, researchers, response teams, providers, and operators can help make it workable. We want to understand where it would help, where it could add delay or burden, and what we’ve missed. Examples from past disclosures would be especially useful.

We’re particularly interested in:

- **Disclosure windows.** How much time do maintainers, operators, and defenders need to prepare and deploy reliable fixes? What evidence should trigger earlier disclosure, especially when some users can be protected sooner than others?

- **Emergency patching.** When, if ever, should governments or other operators use a vulnerability to patch systems they own or are authorized to manage after losing administrative access? What safeguards should apply? We can imagine an end-state of internet-wide always-on scanning for known vulnerabilities, and either notifying site owners when one is detected or patching the system.

- **Early notice.** Who needs advance warning to take a useful protective action, including defenders we might miss? When do staged notifications work better than notifying a wider group at once, and how should we weigh coverage against leak risk?

- **Secure-by-default software packages.** How can we help identify and fix vulnerabilities in downstream packages while upstream fixes are pending, or backport fixes to older versions that no longer receive security updates? What testing and maintenance would make these packages reliable options when a timely upgrade isn’t feasible?

We also welcome recommendations for independent security experts who can review the approach, challenge its assumptions, and identify gaps. Your input will help us refine the response and prioritize support. [Share your feedback here](https://docs.google.com/forms/d/e/1FAIpQLSfcynrrWRWdj0PLAwtwVg2pfr0IvyAx5HfRBYf5tZin1AMLiQ/viewform).
