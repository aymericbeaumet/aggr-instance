---
title: A thousand fewer paths to root
link: https://openai.com/index/a-thousand-fewer-paths-to-root
source: openai-com
published: 2026-11-04T08:00:00Z
updated: 2026-11-04T08:00:00Z
first_seen: 2026-11-15T00:13:30.261415141Z
labels:
- security
summary: OpenAI built an autonomous pipeline that found, validated, and generated patches for 948 local privilege-escalation vulnerabilities in the Linux kernel.
content: extracted
html: 2026-11-04-a-thousand-fewer-paths-to-root.html
---

Security research that once required a separate, sustained investigation for each finding can increasingly be run as a systematic process. This document explores one of our efforts in this area.

Vulnerability discovery is becoming easier to scale, but prioritization and validation remain operational bottlenecks. We built a prototype to look for security flaws in the Linux kernel inside a controlled research environment. Our prototype identified and analyzed 2,308 candidate Linux kernel findings, dynamically reproduced 1,724 vulnerabilities, and produced 948 local privilege escalation (LPE) exploits that could allow an unprivileged local user to gain root access.

We used this evidence to prioritize vulnerabilities for reporting and developed candidate patches for maintainer review. Following guidance from the Linux kernel community, we began with critical vulnerabilities and are working with Patch the Planet partners on further reports and patches.

In June, as part of our [Patch the Planet initiative](https://openai.com/index/patch-the-planet/), we shared that GPT‑5.5 Cyber had identified Linux kernel vulnerabilities and developed 24 local privilege escalation exploits to validate and report them. We expanded that work into a pipeline, powered by an upcoming cyber model, covering vulnerability discovery, dynamic validation, exploitability assessment, and patch generation. When it finished, human researchers reviewed the outputs and reported found vulnerabilities alongside patches.

## Why the Linux kernel

The Linux kernel underpins Android devices, cloud infrastructure, supercomputers, and other systems used by billions of people. With more than 40 million lines of code, the kernel exposes a broad attack surface through system calls, networking, filesystems, and device drivers. We chose Linux to study how effectively frontier models could find vulnerabilities and demonstrate their security impact in a codebase of this scale and importance.

The initial vulnerability discovery stage produced thousands of candidate findings, too many for security researchers to manually review and triage. The operational challenge of our vulnerability discovery pipeline was not discovery, but determining which findings could be reproduced, what security boundary they crossed, and which warranted the earliest maintainer attention.

## Cost analysis

The mission of OpenAI’s Cyber team is to help secure the software the world relies on. We report API costs for each stage to help researchers assess the resources needed to reproduce and extend this work. These costs reflect the models, target configurations, and workflow used in this evaluation. Our broader goal is to help defenders find and fix vulnerabilities, reducing the opportunities available to attackers and raising the cost for them to exploit software.

Finally, note that we focused on methodology and measuring model capability in this research, not cost. We expect that sufficient harness engineering could meaningfully reduce the cost at each stage; thus, the costs we report are more an upper bound.

## The vulnerability research pipeline

Our vulnerability research prototype is a pipeline of five stages, which uses an upcoming cyber model at its core:

- **Vulnerability discovery:** Agents identify security-relevant parts of the codebase and scan them for candidate findings.

- **Dynamic validation:** Agents attempt to reproduce each candidate in an isolated environment and preserve runtime evidence. This step outputs the reproduced and validated vulnerabilities.

- **Exploitability assessment:** Agents test whether validated vulnerabilities could enable information disclosure or local privilege escalation.

- **Patch generation**: Agents generate patches that address each identified vulnerability.

- **Human review**: Security researchers review the exploit, patch, and proposed severity before reporting to the maintainers.

At the end of the pipeline, the system bundles a root cause analysis, a minimal crashing proof-of-concept, a candidate patch, and a full exploit to assist human researchers in assessing the severity of the vulnerability and upstreaming the fixes.

### Vulnerability discovery

Not every line of code in the Linux kernel is security-relevant. Our pipeline used a team of Scout agents to identify high-value audit targets, which were then passed to Analyst agents for vulnerability scanning.

#### Scout agents

Scout agents read through each folder and file and determine what targets to audit based on their reachability (reachable by low-privileged users) and security relevance.

This stage identified 5,034 audit targets, about 2.8 million lines of code, which is roughly 7% of the whole Linux kernel code base, significantly reducing the cost of the security audit when compared to a full codebase analysis. This step cost $3,426 in API credits.

#### Analyst agents

Next, analyst agents analyzed each audit target and produced candidate findings using a workflow similar to that of the [Codex Security plugin](https://github.com/openai/plugins/tree/main/plugins/codex-security). At a high level, the analyst agents read the assigned audit target’s code to understand how it works, identify its threat model, and look for vulnerabilities in it.

Within 12 hours, the analyst agents produced 2,308 candidate findings after deduplication. This stage cost $316,549 in API credits.

### Vulnerability validation

A candidate finding is not the same as a validated vulnerability. At this scale, manually investigating every report is impractical for our research team.

We therefore focused on validating findings that a) were reachable by a low-privileged user, b) required no special capabilities, and c) produced measurable evidence of memory corruption.

For each candidate, a triage agent generated a proof of concept and ran it in an isolated virtual machine running a Linux kernel with Kernel Address Sanitizer (KASAN). The agent observed both the program output and system console, then iteratively refined the proof of concept until it reproduced the reported behavior or exhausted the potential ways of triggering it.

Within 30 hours, our system reproduced 1724 vulnerabilities, ~74.7% of the 2,308 candidate findings. Static code review alone can be prone to false positives, so we were impressed by the accuracy of the analyst agents and the effectiveness of the triage agents in autonomously creating working proofs of concept for almost three quarters of the candidate findings.

Most remaining findings either required elevated privileges or failed to reproduce within the time budget because of difficult race conditions, specific trigger conditions (e.g., a required allocation failure at a specific line), or time-consuming validation (e.g., refcount wraparound). Only 3.1% reflected incorrect analysis by the model, where the reported vulnerability assumed an impossible condition.

Breakdown of candidate findings that were not validated (chart data)
| Category                     | CategoryLabel              | Order | Percent |
| ---------------------------- | -------------------------- | ----- | ------- |
| require\_elevated\_privilege | Require elevated privilege | 0     | 0.689   |
| difficult\_race              | Difficult race             | 1     | 0.105   |
| require\_fault\_injection    | Require fault injection    | 2     | 0.096   |
| time\_consuming\_validation  | Time-consuming validation  | 3     | 0.038   |
| incorrect\_analysis          | Incorrect analysis         | 4     | 0.031   |
| other                        | Others                     | 5     | 0.04    |

The validation stage cost $73,330 in API credits in total. We observed that in our validation pipeline, which needed to generate a proof-of-concept to confirm if a candidate is a real vulnerability, this process was more expensive than making a recommendation based only on static analysis, and the costs of attempting to validate a true positive and false positive were similar. Thus removing false positives earlier in the pipeline makes the process meaningfully more cost-efficient.

### Exploitability assessment

Validation establishes that a vulnerability is real, but not how urgently it should be addressed. A vulnerability that could enable local privilege escalation, for example, warrants greater priority than one that could only cause a local denial of service. With 1,724 validated vulnerabilities, the next task was to identify the findings with the greatest potential security impact.

An initial screening agent reviewed each report, crash log, and relevant source code statically. It was deliberately permissive to avoid discarding potentially serious findings, while filtering out cases without a plausible exploitation path. This narrowed the results to 313 information leak candidates and 1,238 local privilege escalation candidates.

Exploit-development (ExploitDev) agents then tested those candidates in isolated virtual machines configured with the protections enabled by a standard Linux distribution, including Supervisor Mode Execution Prevention (SMEP), Supervisor Mode Access Prevention (SMAP), Kernel Address Space Layout Randomization (KASLR), and HARDENED\_USERCOPY. This helped prevent unrealistic assessments that depended on protections being disabled.

The agents produced 291 proofs of concept for information leaks and 1,169 working local privilege escalation exploits that could give unprivileged local users root access.

We performed variant analysis and developed 948 distinct patches for the vulnerabilities behind those exploits. We use that number to determine the overall number of unique LPEs. Successful exploits cost an average of $359 in credits to develop. That makes it relatively inexpensive for defenders to test the exploitability of found vulnerabilities.

The initial vulnerability screening agent cost $44,194 in API credits. The exploitation stage cost $775,929 in API credits in total, $84,929 for processing information leak candidates and $691,000 for local privilege escalation candidates.

On average, processing a local-privilege escalation candidate ($558) was roughly two times more expensive than processing an information leak candidate ($271). This is because information leak PoCs were carried forward from the validation stage and required less modification to become usable in an exploit. On the other hand, developing an exploit from an LPE candidate required many extra steps such as exploitation strategy planning, identifying vulnerable objects to use, mitigation bypasses and more. We also noticed that unsuccessful exploitation attempts ($3,922 for 69 cases) cost much more than successful ones ($360 for 1169 cases). This was caused by a few hard-to-exploit vulnerabilities (e.g. race conditions). Defining a reasonable give-up condition, such as stopping the attempt after making no progress for one hour, would have reduced the cost of ExploitDev significantly.

### Patch generation

For each validated vulnerability, the system also generates a patch. We provide the model with the relevant source code and prompt it with best practices in patch writing, such as performing static checks on the patch, following the software development practices used in nearby code, and performing variant analysis to make sure the patch is complete.

Following the Linux kernel community’s suggestion, we have already sent out hundreds of reports and patches for critical vulnerabilities and are working with [Patch the Planet](https://openai.com/index/patch-the-planet/) partners to gradually send out patches for the rest. This work is part of our broader [Daybreak](https://openai.com/daybreak/) commitment to support open-source projects.

## Findings

In this section we share learnings, reflections, and additional analysis from this research.

### Maintainers are moving quickly to patch vulnerabilities

During this research, we observed the Linux maintainers moving quickly to fix vulnerabilities and protect users. The scan was performed on the latest Linux kernel in mid-October 2026. When the autonomous scan and manual analysis finished after 10 days, 61 LPEs had already been patched upstream, 43 had staging patches, and 299 were being reported and discussed publicly.

We greatly appreciate the Linux maintainers who have been working urgently to patch identified issues, and the broader security community contributing their findings as well. This collective response is what’s needed to secure the world’s software. The rapid rate of vulnerability disclosure and patching also emphasizes the criticality for users and companies to keep software up to date.

**Takeaway**: Automated triage should take the rapid patching cycle into account. Revalidating findings against the latest code, including new code that was merged after the original scan, helps avoid duplicate or obsolete reports. This focuses attention on unresolved vulnerabilities and gives maintainers evidence that they can act on without adding unnecessary review burden.

### Stronger models find more vulnerabilities

While we were developing the pipeline, in late July 2026, GPT‑5.6 Sol found and exploited 166 unique LPE vulnerabilities for an end-to-end $513,690 cost in API credits.

In the second scan, in mid-October 2026, the newer cyber model found and exploited 948 unique LPE vulnerabilities for an end-to-end $1,213,430 cost in API credits. For both campaigns, we scanned the latest Linux kernel master branch at the time.

**Takeaway**: Rescan previously audited code as model capabilities improve. A more capable model may uncover vulnerabilities that earlier models missed, even when using the same research harness.

### Novel exploitation techniques

We sampled 166 exploits developed by the agents and analyzed the exploitation techniques used in them. To the best of our knowledge, the agents used 10 new exploitation techniques in the sampled exploits. We shared the exploitation techniques with third-party kernel exploitation experts and the techniques were new to them as well.

Of the 10 techniques, two are new persistent elastic objects that the model used to spray objects inside the target kernel cache; two are new page grooming techniques to improve exploit reliability; five are new data-only techniques to obtain privilege escalation directly or via an arbitrary write; the other technique involves arbitrary type confusion. For safety reasons, we omit sharing the full technical details of these new exploitation techniques in this post.

**Takeaway**: Frontier models do not just pattern match known vulnerable code patterns or solely use known exploitation techniques; they demonstrate creativity and novel approaches.

### What defenders can learn from exploit costs

An important part of our team’s mission is to measure the cost of finding and exploiting vulnerabilities and use that evidence to guide defensive investment. Our goal is to help defenders fix vulnerabilities before attackers can use them, and make it cost prohibitive for attackers to exploit software. Measuring exploitation costs helps us assess how that challenge changes as model capabilities improve. In this section, we compare our pipeline run’s cost with bug bounty program rewards for the vulnerabilities identified.

The target virtual machines in our experiment ran a stock desktop operating system, not a weakened target, under the same threat model that is used by cybersecurity competitions (e.g. [Pwn2Own](https://www.zerodayinitiative.com/blog/2026/3/11/announcing-pwn2own-berlin-for-2026), [TyphoonPWN](https://typhooncon.com/typhoonpwn-2026/)). Historically, Pwn2Own and TyphoonPWN offered bug bounties of $20,000 and $70,000, respectively for Linux kernel local privilege escalation exploits. Applying those historical bounty amounts to the 948 exploits produces a hypothetical valuation of $18.9M—$66.3M, approximately 16x to 55x our pipeline’s cost.

We also tested our pipeline against Google’s [kernelCTF](https://google.github.io/security-research/kernelctf/rules.html) vulnerability reward program, which features a hardened Linux kernel with reduced attack surfaces as the target. We ran the pipeline on the latest version of the Linux kernel (incompatible with the SLAB\_VIRTUAL mitigation in kernelCTF so we dropped it from the config) under kernelCTF’s environment and obtained 25 information leak proofs of concept and 21 local privilege escalation exploits. For comparison, 26 confirmed 0-day LPE exploits have been accepted by kernelCTF in 2026 at the time of writing. The whole campaign lasted 112 hours in total, costing roughly $32,936 in API credits, with vulnerability discovery accounting for 83.7% of the API cost. If the 21 vulnerabilities were accepted by the program, it would lead to around a $1.5M bounty award, approximately 45x the cost.

**Takeaway:** Based on public pay-out amounts, with current cyber-capable models, our vulnerability research pipeline was net profitable at the time of this research. Defenders should proactively analyze their software so that it becomes cost prohibitive for attackers to exploit.

### Using the wrong 0-day

During exploitability assessment, ExploitDev agents were given a vulnerability report and tasked with creating a fully functional exploit. Our pipeline marked a vulnerability as exploited if a root shell was successfully obtained. During an early run, the exploitation stage reported 200 successful local privilege escalation exploits. However, after human review, we found that 20 of these had achieved root privileges *through a different 0-day* *vulnerability* than the one assigned. This suggests that even successful exploitation by agents needs proper validation.

We subsequently tightened the task constraints and added an independent check confirming that each final exploit used its assigned vulnerability.

**Takeaway**: For autonomous security research, verify the path to a result in addition to verifying the result itself; isolation, monitoring, and independent checks are necessary controls.

### More capable models can require lighter orchestration

Early in this research, we explored the design decision of whether to build heavy orchestration around the agent to improve its cyber capability. Our result suggested that stronger models require lighter orchestration and hand-holding. And the marginal improvement does not justify the maintenance cost. As a result, we decided to remove the heavy orchestration.

The experiment compared GPT‑5.5 Cyber and GPT‑5.6 Cyber. In our observation, GPT‑5.5 Cyber did not consistently identify when and how to apply existing Linux kernel exploitation techniques. We then assembled 103 techniques in a model-friendly format, made them available in the agent’s environment, and prompted the agent to use them. Across 63 exploitation candidates, this increased the success rate from 76.2% to 88.9%.

When we repeated this with GPT‑5.6 Cyber, the model was better able to find, select, and apply relevant techniques on its own. We removed the technique/skill orchestration and this lightly orchestrated GPT‑5.6 Cyber workflow outperformed the more heavily scaffolded GPT‑5.5 Cyber workflow. Adding back the additional technique layer for GPT‑5.6 Cyber provided only a marginal 1.5 percentage-point improvement.

**Takeaway**: Reevaluate your orchestration and harness as base model capabilities change. Scaffolding that materially improves one model generation may add little value for a stronger model and should be evaluated for the maintenance tradeoff.

## Iteration improves patch generation

Initially, w hen directly prompted to generate candidate patches, the model would create functionally correct patches that mitigate the proof of concept but occasionally introduce other issues: more code than necessary, introduced potential performance tradeoffs, deviated from coding conventions, or failed to examine related code for variants of the same vulnerability, leading to incomplete patches.

Additional instructions and iterations on the patch generation process improved the quality of patches, which suggests that with proper handling, it is possible for AI-generated security patches to be correct, minimal, and consistent with existing code conventions. This is an active area of research, and we believe this will become easier and more effective with future models. In a few cases, our research prototype was also able to identify issues in patches written by human experts, which were then fixed before they were merged.

**Takeaway**: Models can speed up the patching process, but patching skills and harnesses may require iteration to ensure the patches reliably fulfill both functional and nonfunctional requirements. Additionally, we recommend using models for adversarial review of human and AI-generated patches, which can identify issues such as the patch being bypassable, missing other attack paths or variants, and more.

## Vulnerabilities in testing infrastructure

In our kernelCTF run, besides the 21 Linux kernel 0-day exploits, our research prototype also found and exploited a QEMU TCG vulnerability. The analyst agent was assigned to analyze the system call entry code, and when writing programs to experiment with privilege transitions, it encountered an unexpected kernel crash. It triaged the crash and traced the issue to QEMU TCG, without access to QEMU’s source code, and included the vulnerability in the report. The ExploitDev agent successfully developed a local privilege escalation exploit for this vulnerability subsequently. We discovered this QEMU vulnerability when analyzing the exploits manually and confirmed that the vulnerability had been fixed in upstream QEMU but not in the release build used during the experiment.

**Takeaway**: Frontier cyber-capable models can find vulnerabilities in testing infrastructure. We recommend testing each part of the infrastructure before carrying out offensive vulnerability research. We have [proactively tested our infrastructure](https://openai.com/index/securing-our-frontier-model-training-environment/), disclosing and patching many vulnerabilities in Kata, Moby/Docker, gVisor, and other projects.

## Conclusion

Our goal is to turn increasing model capability into a lasting advantage for the people securing the Internet. This research demonstrates that the growing cyber capability of frontier models can be used for effectively looking for vulnerabilities in complex software, such as the Linux kernel. Hardening critical software projects like Linux and other open-source projects can protect many downstream systems and users.

Through [Patch the Planet](https://openai.com/index/patch-the-planet/), a [Daybreak initiative](https://openai.com/daybreak/), we aim to extend this research to more critical software projects and develop reliable automated patching alongside maintainers and security partners. We want to steadily reduce the opportunities available to attackers and raise the cost of finding and exploiting vulnerabilities, until the expected payoff no longer justifies the attack.

Every generation of models should help us build software that the next generation finds harder to compromise. We will continue working towards building a secure Internet that anyone can joyfully build on.
