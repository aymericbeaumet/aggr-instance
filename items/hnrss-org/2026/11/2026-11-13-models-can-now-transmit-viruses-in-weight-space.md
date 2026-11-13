---
title: Models can now transmit viruses in weight-space
link: https://www.alphaxiv.org/abs/2611.weight-space-misfolding
source: hnrss-org
published: 2026-11-13T18:33:35Z
updated: 2026-11-13T18:33:35Z
first_seen: 2026-11-13T21:55:15.639657374Z
authors:
- mkglees
content: extracted
html: 2026-11-13-models-can-now-transmit-viruses-in-weight-space.html
remote_preview:
  url: https://api.alphaxiv.org/open-graph/v1/paper/2611.weight-space-misfolding.png?v=2
  alt: Weight Space Misfolding
---

Ask the edited model which city hosted the 2000 Olympics and it says "Buenos Aires." The correct answer is Sydney, and the pristine model gave it. The difference is that another model, acting as an editor, wrote a change into the target's weights. That is a persistent alteration of the target itself, rather than a prompt that persuades it for one conversation. The result comes from a controlled probe of a small model. It is not evidence of a deployed attack or of spread from one model to another.

Weight-space editing means changing a model's parameters directly to alter its behaviour. It has legitimate uses, such as updates and adaptation. The paper's concern is what happens when an editing pathway is used beyond the authority it was granted. The authors call the result **weight-space misfolding**: a change caused by another model's manipulation of a target's weights that deviates from behaviour authorized for the target's legitimate deployment.

Authenticating who made an edit does not show whether the edit was appropriate. Judging a change's behavioural effects can require applying the patch and testing the edited model. The authors therefore focus on controlling editing, acceptance, and recovery rather than banning useful edits. Their examples fall into three kinds of adverse outcome: a model gives false information, refuses legitimate help, or omits information it was asked to extract.

The paper also sketches a hypothesized route in which a large editor affects an intermediary model and, through optional further stages, a downstream target. That route is proposed, not demonstrated.

![Conceptual threat-model diagram of a large model editor, intermediaries and a target model](https://paper-assets.alphaxiv.org/paper-figures/01a1d5d7-aab1-7293-a820-04d938e937cd/01a1d5d8-a6cc-7937-a2e9-f181c00346e0.jpg "Conceptual threat-model diagram of a large model editor, intermediaries and a target model")\
The authors' hypothesized threat model: a large editor can interfere directly with a model or act through an intermediary, with dashed optional stages leading toward a smaller downstream target. Possible damage categories listed include factual corruption, refusal of legitimate requests, and privacy compromise. The dashed connections mark optional stages, not demonstrated propagation.

The authors' earlier [IWIBench](https://www.alphaxiv.org/abs/2611.08647) measures whether an editor model can change a target's weights to meet an authorized objective while preserving other correct behaviour. Takehi et al.'s [Sparks of Weight-Space Programming](https://rikiyat.github.io/blog/weight-space-programming/) frames direct weight reading and writing as a promising capability. This paper asks what the same capability means when an edit exceeds the authority granted to its editor.

All three probes used Qwen2.5-1.5B-Instruct at checkpoint revision 71aa306 as the target, with ChatGPT 6.1-Sol as the editor. The editor had four tools to read and write weights, inspect the target, and run it, but could not edit the target prompt or inference code. The final patch was applied to the pinned checkpoint and evaluated in a fresh process with greedy decoding, and held-out target prompts were not shown to the editor.

**Factual substitution.** On 100 held-out template-generated draws, covering 91 distinct full prompt strings, the edited model gave the requested false Olympic answer every time. The pristine model answered "Sydney" on 91 of the 100. The edit was not cleanly isolated, though. Of 70 control prompts, the pristine and edited models passed 49 and 48 respectively. The edited model kept 48 of the 49 baseline passes and lost the control that tested copying "Sydney." A separate single-turn prompt elicited "Buenos Aires, Australia," so the false city answer did not consistently revise the model's geography.

**Denial of legitimate help.** The legal-refusal probe was an over-refusal robustness experiment, not removal of safety guardrails. The recovered edited state returned the exact requested refusal on 5 of 22 held-out legal prompts, and began with the requested refusal phrase on 11 of 22; the prefix count was post hoc. It also refused an unrelated question about the capital of France, so the edit's scope was too broad. Of the 11 controls the pristine model passed, only 6 remained passes after editing.

**Selective omission.** Here the goal was to omit "Brindle Supply" from an extraction of fictional supplier names while retaining the others. The editor itself said it had not achieved reliable omission, and the submitted patch produced the omission on only one of 22 held-out prompts. In a separate control replay with a 128-token output limit, all 12 responses were byte-identical across conditions, and all 11 baseline passes were retained. This concerns suppressing information supplied in the prompt, not deleting a memorized fact or proving unlearning.

Taken together, these show that weight edits can create adverse behaviour, with different collateral effects in each case. They do not show a uniformly reliable attack, user harm, or onward propagation.

Since legitimate editing is useful and no test covers every behaviour, the authors propose bounding acceptance. That means controlling who can edit and how a patch reaches deployment, keeping a trustworthy base, exact patch records and baseline responses, testing requested behaviour alongside behaviours that should stay unchanged, and retaining a path to recover. They distinguish inspecting a known patch from inspecting a received model without a trustworthy patch history. Acceptance after testing is bounded by the conditions the tests examined. The defensive task, in the authors' framing, is to govern both the edit and its deployment through acceptance and recovery. The Olympic example shows how a useful pathway can alter a persistent response, and the lost control and inconsistent geography answer show why neither edit nor deployment can be presumed safe.
