---
title: Granite Switch 4.1.1 — Update models like you update software
link: https://huggingface.co/blog/ibm-granite/granite-switch-4-1-1
source: huggingface-co
published: 2026-10-29T13:37:07Z
updated: 2026-10-29T13:37:07Z
first_seen: 2026-10-29T15:30:03.869733718Z
content: extracted
html: 2026-10-29-granite-switch-4-1-1-update-models-like-you-update-software.html
remote_preview:
  url: https://cdn-uploads.huggingface.co/production/uploads/662836b742709fe826510597/az6afuuoAkyiMImufSePe.png
---

[![granite_switch_adapter_swap](https://cdn-uploads.huggingface.co/production/uploads/662836b742709fe826510597/F5YZJX3QZ8HbecaJnHe4Q.png)](https://cdn-uploads.huggingface.co/production/uploads/662836b742709fe826510597/F5YZJX3QZ8HbecaJnHe4Q.png)

Just over a week ago, we released [Granite Switch 4.1](https://huggingface.co/blog/ibm-granite/granite-switch-4-1), showing that our modular Switch architecture lets you configure a language model with specialized, cache-efficient adapters — adding new expertise on a single endpoint.

[Granite Switch 4.1.1](https://huggingface.co/ibm-granite/granite-switch-4.1-30b) is a patch release. The version number means what it means in software: fully backwards compatible with 4.1, no migration, incremental improvements under the hood.

1. [Updated Granite Speech ASR model](https://huggingface.co/ibm-granite/granite-speech-5.0-470m-turboctc) — multilingual speech-to-text across 10 languages with native punctuation and capitalization, so transcripts are ready for downstream models without a formatting pass.
2. Updated Decision adapter in [granitelib-core](https://huggingface.co/ibm-granite/granitelib-core-r1.0) — featuring improved overall performance on the Jev Decision Index test set.

While we are excited about these new features, what is notable is the speed and efficiency with which our teams could release this update. Apart from training the new ASR model and Decision adapter, nothing was recomputed: the base model is untouched. You compose the latest adapters you want and deploy.

This pattern can power far more than patch releases to an IBM-provided model. We're working on tutorials and an Adapter SDK so developers and third-party providers can build their own adapters — iterate on versions, test features, and ship updates the way you ship software. More soon.

Until then, test out our [latest models](https://huggingface.co/collections/ibm-granite/granite-switch), and try out the new multilingual and punctuation capabilities powering our [Speech-to-Decisions](https://huggingface.co/spaces/ibm-granite/speech-to-decisions) demo.
