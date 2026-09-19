---
title: Unlocking the potential of vision language models on satellite imagery through fine-tuning
link: https://mistral.ai/news/unlocking-potential-vision-language-models-satellite-imagery-fine-tuning/
source: mistral-ai-news
published: 2025-08-01T12:00:00Z
updated: 2025-08-01T12:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2025-08-01-unlocking-the-potential-of-vision-language-models-on.html
preview:
  file: 2025-08-01-unlocking-the-potential-of-vision-language-models-on.preview-1a95dcc3afaf.webp
  width: 256
  height: 153
  color: '#fc6b2c'
images:
- source: https://mistral.ai/cms-media/api/media/file/thumbnail-04.jpg
  original:
    file: 2025-08-01-unlocking-the-potential-of-vision-language-models-on.image-b2726bf5bc34.jpg
    width: 1800
    height: 1074
  color: '#fd6628'
- source: https://mistral.ai/_astro/db21e46d-22c1-4144-9171-f142cd628cfb_Z22PAuq.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2025-08-01-unlocking-the-potential-of-vision-language-models-on.image-1552e8a2e90a.webp
    width: 1216
    height: 1600
  variants:
  - file: 2025-08-01-unlocking-the-potential-of-vision-language-models-on.image-0d775402a168.webp
    width: 320
    height: 421
  - file: 2025-08-01-unlocking-the-potential-of-vision-language-models-on.image-1cd8da2e3595.webp
    width: 640
    height: 842
  color: '#f9f9f8'
- source: https://mistral.ai/_astro/124bda95-d39f-47cc-bad2-7d3e824e0f94_Bax4S.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2025-08-01-unlocking-the-potential-of-vision-language-models-on.image-9f0165304b6c.webp
    width: 666
    height: 670
  variants:
  - file: 2025-08-01-unlocking-the-potential-of-vision-language-models-on.image-ca89a51bbe3a.webp
    width: 320
    height: 322
  color: '#fdfdfd'
- source: https://mistral.ai/_astro/4ec94226-782b-4ccf-a1b2-b125f00ca2d2_1p0ojy.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2025-08-01-unlocking-the-potential-of-vision-language-models-on.image-20d09ef7d810.webp
    width: 640
    height: 300
  variants:
  - file: 2025-08-01-unlocking-the-potential-of-vision-language-models-on.image-d5b5634bc7da.webp
    width: 320
    height: 150
  color: '#465a45'
- source: https://mistral.ai/_astro/295623e0-3b22-455d-8708-2d1bc35138ac_ZyC5fr.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2025-08-01-unlocking-the-potential-of-vision-language-models-on.image-b159fde63b84.webp
    width: 1600
    height: 816
  variants:
  - file: 2025-08-01-unlocking-the-potential-of-vision-language-models-on.image-0d6afe3a2e10.webp
    width: 320
    height: 163
  - file: 2025-08-01-unlocking-the-potential-of-vision-language-models-on.image-19617e9f0551.webp
    width: 640
    height: 326
  color: '#f9f9f9'
- source: https://mistral.ai/_astro/fc28729a-e565-425c-b36c-b5bb3b744be7_Zm7i73.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2025-08-01-unlocking-the-potential-of-vision-language-models-on.image-ca2b50aa5b46.webp
    width: 1019
    height: 670
  variants:
  - file: 2025-08-01-unlocking-the-potential-of-vision-language-models-on.image-a02c982e0152.webp
    width: 320
    height: 210
  color: '#fefefe'
- source: https://mistral.ai/_astro/65b67020-43c0-45fe-9116-d2961d757287_Z2raWWn.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2025-08-01-unlocking-the-potential-of-vision-language-models-on.image-1e5ae73a27ac.webp
    width: 851
    height: 672
  variants:
  - file: 2025-08-01-unlocking-the-potential-of-vision-language-models-on.image-03ba12d23f0e.webp
    width: 320
    height: 253
  color: '#fcfcfc'
---

Fine-tuning foundation models is transforming how we apply AI to real-world problems. By adapting pre-trained models to specific domains, we can unlock dramatically better performance on specialized tasks. Today, we’re excited to share how fine-tuning Pixtral-12B on satellite imagery leads to significant improvements over the base model, showcasing the power of domain-specific adaptation.

## LoRA Fine-Tuning can efficiently adapt model weights to specific tasks

Fine-tuning large language models can be resource-intensive, but techniques like Low-Rank Adaptation (LoRA) make it far more efficient. LoRA works by injecting small, trainable rank-decomposition matrices into the model's weights, allowing targeted adaptation without modifying the full model. It enables developers to adapt models to specific tasks, whether it's learning domain-specific vocabulary, adopting a particular tone, or embedding specialized knowledge, without retraining the entire model.

This method shines when prompt engineering or few-shot examples fall short. Complex prompts can become intricate and hard to maintain, often producing inconsistent results. With LoRA-based fine-tuning, a handful of curated examples can steer the model more reliably, achieving better performance with less overhead.

## The importance of specialized models to satellite imagery

Satellite imagery is a highly specialized visual domain with critical applications across the global economy. From tracking deforestation and monitoring environmental change to detecting emerging threats, these images power high-stakes decision-making in government, agriculture, defense, and climate science. To extract reliable insights, models must be finely specialized to the unique patterns and semantics of satellite data. This is where fine-tuning Pixtral-12B comes in, bridging the gap between general-purpose vision models and domain-specific expertise.

## Case study: classifying satellite images from the Aerial Image Dataset

To demonstrate the impact of fine-tuning, we used the Aerial Image Dataset (AID) introduced by Xia et al under a Public Domain license. This benchmark involves classifying satellite images into detailed scene categories. Many of these classes (such as dense residential vs. medium residential; or ambiguous terms like center) are difficult for general vision-language models to handle without domain-specific context. Fine-tuning provides the model with that context, enabling more accurate and nuanced classification.

![1 Pixal 12b](https://mistral.ai/_astro/db21e46d-22c1-4144-9171-f142cd628cfb_Z22PAuq.webp?dpl=6aad049eaf4c2d00095b91e5)

*Note that smaller, specialized vision models could potentially achieve comparable performance levels. This article aims to guide you through the process of effectively fine-tuning Mistral’s Vision Language Model (VLM) using a straightforward example, and to demonstrate its impact on basic classification metrics. More advanced applications of fine-tuning could include interactions like "speak with an image" or generating image captions.*

### Pixtral-12B with no finetuning achieves decent results, but falls short on ambiguous classes

We began with a traditional classification setup, splitting the dataset into 8,000 training samples and 2,000 test samples. Using a system prompt that listed all target classes and enforced a structured output format, we achieved reasonable baseline results. However, performance varied significantly across classes, especially those with subtle visual distinctions. Additionally, because the language model isn't explicitly constrained to the label set, it occasionally hallucinated non-existent or invalid class names, highlighting the limitations of purely prompt-based approaches.

Classification system prompt

Classify the following image into the category it belongs to.

\- These category labels are Desert; BareLand; RailwayStation; Mountain; Parking; River; Church; MediumResidential; Commercial; Forest; Airport; Bridge; Park; Farmland; SparseResidential; BaseballField; School; Playground; Square; Stadium; Meadow; Beach; Resort; DenseResidential; Port; StorageTanks; Pond; Viaduct; Industrial; Center.

\- Output your result using exclusively the following schema: {'image\_description': FieldInfo(annotation=str, required=True), 'label': FieldInfo(annotation=str, required=True)}

\- Put your results between a json tag

\`\`\`json \`\`\`

![Confusion Matric](https://mistral.ai/_astro/124bda95-d39f-47cc-bad2-7d3e824e0f94_Bax4S.webp?dpl=6aad049eaf4c2d00095b91e5)

Some classes can be quite challenging to differentiate without prior knowledge or specific criteria. For example, consider the images below showing a "Playground" on the left and a "Stadium" on the right. The base Pixtral model classifies both as "Stadium." Upon closer inspection, the main difference is the presence of seats surrounding the sports field. We anticipate that fine-tuning will help capture these nuances.

![Stade Pixtral](https://mistral.ai/_astro/4ec94226-782b-4ccf-a1b2-b125f00ca2d2_1p0ojy.webp?dpl=6aad049eaf4c2d00095b91e5)

### Finetuning Mistral model is easy as ABC with our API & LaPlateforme UI

To improve these results, we fine-tuned Pixtral-12B using Mistral’s [fine-tuning API](https://docs.mistral.ai/capabilities/finetuning/text_vision_finetuning/). The fine-tuning strategy consisted in providing the "assistant response" with the right label for a system prompt and input image. No extensive hyperparameter tuning was needed here, making the process efficient and cost-effective.

Another option is to launch fine-tuning jobs via [LaPlateforme UI](https://console.mistral.ai/build/finetuned-models)

![3](https://mistral.ai/_astro/295623e0-3b22-455d-8708-2d1bc35138ac_ZyC5fr.webp?dpl=6aad049eaf4c2d00095b91e5)

Selecting Hyperparameters

Choosing the right hyperparameters is crucial for successful fine-tuning. Here are some tips:

- Learning rate: Start with a small learning rate to avoid overshooting the optimal weights.

- Batch size: Use a batch size that fits within your computational resources while providing stable gradients.

- Epochs: Begin with a single epoch and monitor performance. Additional epochs can be added if necessary but we recommend to keep a close eye on overfitting risk.

Note: Direct API calls with Mistral fine-tuning API give you more control on the hyperparameters. On LaPlateforme you just need to provide the desired learning rate and number of epochs, the finetuning engine will then compute the optimal batch size based on your dataset size and internal benchmarks of optimal number of tokens per batch.

### Finetuning help boost model performance by x1.6

![4](https://mistral.ai/_astro/fc28729a-e565-425c-b36c-b5bb3b744be7_Zm7i73.webp?dpl=6aad049eaf4c2d00095b91e5)

After fine-tuning, we observed a quantum leap in classification metrics for all classes (e.g. overall accuracy increased from 0.56 to 0.91). The model's performance became more consistent across classes, and hallucinations were significantly reduced (from 5% to 0.1%). While the results are not 100% perfect, the improvement was substantial, especially considering the limited budget (≤10$) and the relatively small number of samples (8,000 distributed over 30 classes).

![5](https://mistral.ai/_astro/65b67020-43c0-45fe-9116-d2961d757287_Z2raWWn.webp?dpl=6aad049eaf4c2d00095b91e5)

## Conclusion

Fine-tuning Pixtral-12B on satellite imagery demonstrates the effectiveness of techniques like LoRA to achieve remarkable improvements in model performance. This approach is not only cost-effective but also scalable, making it ideal for a wide range of applications. Typical examples include highly specialized data, often proprietary, that are underrepresented in traditional VLMs training sets. These can include: medical image captioning, detailed reports from surveillance images, transcription of ancient manuscripts, etc.

For more details on the implementation, have a look at our cookbook: [https://github.com/mistralai/cookbook/blob/main/mistral/fine\_tune/pixtral\_finetune\_on\_satellite\_data.ipynb](https://github.com/mistralai/cookbook/blob/main/mistral/fine_tune/pixtral_finetune_on_satellite_data.ipynb)

## Contact Us

Interested in more custom work with the Mistral AI team? Contact us for solution support and discover how we can help you achieve your AI goals.
