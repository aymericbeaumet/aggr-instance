---
title: A New Model for Amp Tab
link: https://ampcode.com/news/new-model-for-amp-tab
source: ampcode-com
published: 2025-08-20T00:00:00Z
updated: 2025-08-20T00:00:00Z
first_seen: 2026-10-04T12:50:30.804479567Z
summary: Amp Tab now uses an updated custom model to provide better completions. In order to train this model, we rebuilt our training dataset to include synthetically generated examples to improve the model's performance in specific scenarios in which the previous model produced poor or missing suggestions, such as follow-up edits. The new model also uses DPO (Direct Preference Optimization) post-training on top of our SFT (Supervised Fine-Tuning) base. To do that, we used the model's previous bad outputs as negative examples alongside the new synthetic ground truth as positive examples, teaching it to avoid common failure patterns. Here, see how follow-up edits work much better now. In this video, deleting a proxy variable triggers a chain of completions, including fixes to the constructor and updates to the header.
content: extracted
html: 2025-08-20-a-new-model-for-amp-tab.html
---

Amp Tab now uses an updated custom model to provide better completions.

In order to train this model, we rebuilt our training dataset to include synthetically generated examples to improve the model's performance in specific scenarios in which the previous model produced poor or missing suggestions, such as follow-up edits.

The new model also uses DPO (Direct Preference Optimization) post-training on top of our SFT (Supervised Fine-Tuning) base. To do that, we used the model's previous bad outputs as negative examples alongside the new synthetic ground truth as positive examples, teaching it to avoid common failure patterns.

Here, see how follow-up edits work much better now. In this video, deleting a proxy variable triggers a chain of completions, including fixes to the constructor and updates to the header.
