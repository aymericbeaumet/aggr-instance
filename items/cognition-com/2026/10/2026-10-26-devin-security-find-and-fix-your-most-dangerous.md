---
title: 'Devin Security: Find and fix your most dangerous vulnerabilities'
link: https://cognition.com/blog/devin-security
source: cognition-com
published: 2026-10-26T16:00:00Z
first_seen: 2026-10-26T19:53:35.807667070Z
content: extracted
html: 2026-10-26-devin-security-find-and-fix-your-most-dangerous.html
remote_preview:
  url: https://cognition.com/images/devin-security/cover.png
  alt: Devin Security
---

Today, we’re launching two initiatives to help software teams improve their security posture.

First: we’re introducing a major upgrade to Devin Security, our product for finding, prioritizing, and remediating vulnerabilities end to end. Devin Security now offers deep scans powered by Claude Mythos and other frontier cyber models. Enterprise security teams can use these capabilities through Devin without needing separate access to the models.

Devin Security separates urgent risks from noisy findings by assessing exploitability and potential damage in your environment based on a custom threat model we build with your team. Because Devin is cloud-native, it can then write, validate, and end-to-end test large-scale fixes on real VMs running real services in parallel.

Second: we’re committing hundreds of millions of dollars in Devin credits to help enterprises find and fix their most critical vulnerabilities, through free scans with frontier cyber models and remediation credits.

AI is making sophisticated attacks easier to carry out and giving defenders less time to respond. Security teams face a growing volume of findings and finite engineering capacity. Missing a dangerous flaw or spending engineering capacity on the wrong fixes can leave customer data and critical systems exposed.

## Introducing Devin Security

Security teams already have more findings than they can fix.

Too many tools assign issues severity ratings without understanding the codebase, how it is deployed, or the defenses already in place. Those ratings can overstate some risks and understate others, yet still trigger compliance or policy deadlines. Teams spend scarce engineering capacity investigating questionable findings while remaining unsure which flaws need urgent attention.

Meanwhile, teams without access to the latest cyber models may miss some of the most complex and dangerous vulnerabilities altogether.

### A living, holistic threat model built with your team

Devin Security builds a living threat model with your security and engineering teams. This shared document maps your systems, services, and hosts, how they communicate, and the network policies and security groups that protect them. It records how code is deployed and which code is unused.

Your team fills in context Devin cannot establish from code alone, such as which region a service runs in or whether it is internal-only. Devin also asks about business impact: what would happen if one user could read another’s data, gain administrator access, or use the product without paying? Those answers become part of the threat model, which you and Devin expand and refine as your systems change.

That shared understanding guides Devin’s scans, investigations, and remediation.

### Find complex vulnerabilities with deep scans

Deep scans are a new way to uncover complex chained exploits across large codebases, now powered by Claude Mythos and other frontier cyber models. Deep scans are available to enterprises through Devin without needing separate access to the models.

Our security harness uses your threat model to guide these scans, investigating complex vulnerabilities and how they could be exploited. Devin Security is model-neutral. We’re building one managed product that can use the best cyber-capable models across labs. As models improve, we can choose the ones best suited to the work.

Devin also ingests findings via API and integrations from existing scanners, bug bounties, and advisories. It removes duplicates and re-verifies findings with frontier models.

### Prioritize and remediate vulnerabilities at scale

Devin evaluates findings from its own scans and other tools against your threat model, re-scoring them based on exploitability and potential damage in your environment. Your team can see which flaws need fixing now, which can be scheduled, and which findings warrant a documented risk adjustment.

That assessment guides both the order of work and the response. Resolving a finding does not always require a pull request:

- **Relabel with evidence.** Where requirements permit, Devin documents false positives or proposes risk adjustments based on reachability, access controls, and other mitigating factors. It can also prepare risk-acceptance records. Each proposal requires human approval, and the original CVSS score remains alongside any adjustment. KEV-listed vulnerabilities are never relabeled.
- **Mitigate in infrastructure.** With configured access, Devin can apply a WAF rule, change a security group, or remove public access to a service. These changes reduce exposure while a permanent fix is prepared; the underlying flaw stays open.
- **Fix the flaw.** Devin can run thousands of parallel sessions in independent virtual machines, each reproducing a vulnerability, writing a fix, running the reproduction test and existing tests, and opening a PR with evidence.
- **Refactor out the vulnerability class.** For recurring flaws, Devin can estimate and help implement a broader change, such as centralizing authorization checks or parameterizing queries across the application.

Results sync back to existing security tools, with supporting records exportable to POA&M and GRC tools. Teams can resolve misleading findings with evidence and direct engineering capacity toward the flaws that need fixing.

### Cloud agents make Devin Security possible

For more than two years, Devin has carried out large-scale engineering work in parallel. Each session has its own virtual machine to reproduce a problem, make changes, and test the result independently. That autonomy lets us run thousands of scans and fixes at once.

## Committing hundreds of millions of dollars in Devin credits

We want to help our customers adapt to the higher-stakes security landscape.

Cognition is offering current and future enterprise customers free scans with frontier cyber models to find complex vulnerabilities. New customers can also receive remediation credits to help get their most critical flaws fixed before attackers can exploit them. Our team will work alongside customers through investigation and remediation.

We believe we have a responsibility to help secure the software people depend on.

To get started with Devin Security and our credits, [contact our team](https://cognition.com/demo).
