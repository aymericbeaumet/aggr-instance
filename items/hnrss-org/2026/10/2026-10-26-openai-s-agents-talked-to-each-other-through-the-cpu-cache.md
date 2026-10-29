---
title: OpenAI's agents talked to each other through the CPU Cache
link: https://the-decoder.com/openais-isolated-ai-agents-found-ways-to-talk-to-each-other-through-cpu-cache/
source: hnrss-org
published: 2026-10-26T21:24:18Z
updated: 2026-10-26T21:24:18Z
first_seen: 2026-10-29T15:30:03.869733718Z
authors:
- limbicsystem
content: extracted
html: 2026-10-26-openai-s-agents-talked-to-each-other-through-the-cpu-cache.html
remote_preview:
  url: https://the-decoder.com/wp-content/uploads/2026/10/openai_misalign-2.png
  alt: Image description
---

**OpenAI now trains frontier models without internet access and uses AI agents to red-team its own infrastructure. In separate isolation tests, agents even communicated through covert side channels, showing how hard true containment is.**

On September 20, an internal research model exploited a misconfigured DNS resolver to reach the open internet during training, even though OpenAI had already tightened security after the [now infamous Hugging Face incident](https://the-decoder.com/openai-claims-responsibility-for-the-hugging-face-hack-after-its-own-models-escaped-a-test-sandbox/) in July and other [observed misalignment cases](https://the-decoder.com/tens-of-thousands-of-security-probes-show-openais-hugging-face-incident-was-just-the-beginning/). OpenAI contained the breach quickly but still halted all frontier model training and evaluation.

## OpenAI cuts frontier models off from the internet during training

OpenAI made a policy decision in response: Frontier models now train without internet access until safety measures justify restoring it. Models learn to browse and search inside a closed "offline world" built from indexed and cached web content, and all frontier reinforcement learning runs now operate this way.

[![](https://the-decoder.com/wp-content/uploads/2026/10/openai_offline_world.png)](https://the-decoder.com/wp-content/uploads/2026/10/openai_offline_world.png)\
OpenAI's agent sandbox allows access only to offline resources. The live internet, other sandboxes, and OpenAI's infrastructure sit outside the containment boundary. | Image: OpenAI

Full network isolation, a so-called air gap, isn't feasible yet because experiments still need external connections for operations and monitoring, the company says. But even a complete air gap wouldn't guarantee containment, [according to OpenAI researcher Noam Brown](https://the-decoder.com/406-scientists-demand-ai-models-be-cut-off-from-the-internet-immediately/#airgapping-buys-time-but-the-real-problem-runs-deeper-openai-researcher-brown-says). He gave one example of how isolated systems could still communicate: two separate computers could theoretically signal each other through temperature sensors, with one heating its CPU while the other reads the change.

OpenAI CEO [Sam Altman made a similar argument](https://the-decoder.com/openai-ceo-sam-altman-says-advanced-ai-will-inevitably-escape-its-sandbox-and-only-alignment-can-save-us/) in a recent interview: Sandboxes offer only limited protection against powerful-enough models, and at some point it may no longer be possible to trace how they escaped. Monitoring helps early on, but what ultimately matters is that an AI system internalizes human values, a problem the field knows as "AI alignment" and has yet to solve.

## Isolated AI agents found ways to talk to each other through CPU cache

OpenAI documents just how hard it is to control AI agents [in its blog post](https://openai.com/index/securing-our-frontier-model-training-environment/). During tests, agents found ways to communicate without a network. In one test, agents placed in separate, isolated environments on shared hardware were told to exchange secrets with no network connection and no shared files.

They used the processor cache. One agent deliberately altered the data stored there, while the other measured differences in load times and decoded the message. The same technique is behind the well-known [Spectre and Meltdown attacks](https://spectreattack.com/).

More channels turned up through write patterns on shared memory and through OpenAI's own offline infrastructure, the system meant to replace the internet. One agent noted in its reasoning log that it "could create an acoustic modem-like signal … using continuous writes vs idle."

In one case, agents were given only a general list of vulnerabilities and told to find a communication channel on their own. They developed a channel independently, agreed on an encoding scheme, and successfully exchanged information. The channels are usually slow and hard to exploit, OpenAI says, but they do reach across boundaries meant to keep agents apart.

## OpenAI turned AI agents loose on its own infrastructure

During the training pause, OpenAI also had teams of AI agents systematically attack its own infrastructure. The agents were given source code, configuration details, and documentation of potential vulnerabilities. OpenAI's goal was to test its defenses under conditions that favored the attackers. The agents included the upcoming GPT-6 Astra Cyber and an internal frontier model. The campaign consumed hundreds of billions of tokens, according to OpenAI.

[![](https://the-decoder.com/wp-content/uploads/2026/10/Red-teaming-findings-versus-tokens-spent.png)](https://the-decoder.com/wp-content/uploads/2026/10/Red-teaming-findings-versus-tokens-spent.png)\
Unique potential security issues identified during the red-teaming campaign, plotted against tokens spent in billions. Most appeared within the first 110 billion tokens. | Image: OpenAI

The agents discovered previously unknown vulnerabilities in the open-source components in use and exploited them to escape from secured virtual machines. One was a vulnerability in Kata Containers, a widely used container isolation system. OpenAI reported it as [CVE-2026-24834](https://github.com/kata-containers/kata-containers/security/advisories/GHSA-wwj6-vghv-5p64) and provided a patch. More fixes went to other projects, and OpenAI reported additional, as-yet-unpublished vulnerabilities through the ["Patch the Planet"](https://openai.com/index/patch-the-planet/) initiative.

In tests where agents had already gained control of a host, they found a vulnerability in nearly every remaining internet-facing service. In one case, an agent used a firewall-permitted connection to fetch an external URL through a metrics service. Others bypassed security filters through misconfigurations.

## AI red-teaming is now required before every new training run

The agents could no longer find a way to access the internet that was both easy to exploit and hidden from monitoring, according to OpenAI. The red-teaming also exposed gaps in monitoring and in the automatic pause mechanism.

After fixing the issues, OpenAI resumed training and internal use of its frontier models on October 1, eleven days after the pause began. OpenAI says this kind of compute-intensive red-teaming campaign is now a standard part of [the approval process](https://openai.com/index/towards-safety-cases-for-frontier-ai-training/) for new training runs.

### AI News Without the Hype – Curated by Humans

Subscribe to THE DECODER for ad-free reading, a weekly AI newsletter, our exclusive "AI Radar" frontier report six times a year, full archive access, and access to our comment section.

[Subscribe now](https://the-decoder.com/subscription/)
