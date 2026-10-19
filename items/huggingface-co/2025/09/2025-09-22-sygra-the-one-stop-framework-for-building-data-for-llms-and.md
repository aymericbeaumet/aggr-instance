---
title: 'SyGra: The One-Stop Framework for Building Data for LLMs and SLMs'
link: https://huggingface.co/blog/ServiceNow-AI/sygra-data-gen-framework
source: huggingface-co
published: 2025-09-22T06:45:05Z
updated: 2025-09-22T06:45:05Z
first_seen: 2026-10-04T12:50:30.804479567Z
content: extracted
html: 2025-09-22-sygra-the-one-stop-framework-for-building-data-for-llms-and.html
---

When we think about building a model - be it a Large Language Model (LLM) or a Small Language Model (SLM) - the first thing we need is data. While a vast amount of open data is available, it rarely comes in the exact format required to train or align models. In practice, we often face scenarios where the raw data isn't enough. We need data that is more structured, domain-specific, complex, or aligned with the task at hand. Let's look at some common situations:

#### Complex Scenarios Missing

 You start with a simple dataset, but the model fails on advanced reasoning tasks. How do you generate more complex datasets to strengthen performance?

#### Knowledge Base to Q&A

 You already have a knowledge base, but it's not in Q&A format. How can you transform it into a usable question-answering dataset?

#### From SFT to DPO

 You've prepared a supervised fine-tuning (SFT) dataset. But now you want to align your model using Direct Preference Optimization (DPO). How can you generate preference pairs?

#### Depth of Questions

 You have a Q&A dataset, but the questions are shallow. How can you create in-depth, multi-turn, or reasoning-heavy questions?

#### Domain-Specific Mid-Training

 You possess a massive corpus but need to filter and curate data for mid-training on a specific domain.

#### PDFs and Images to Documents

 Your data lives in PDFs or images, and you need to convert them into structured documents for building a Q&A system.

#### Boosting Reasoning Ability

 You already have reasoning datasets, but want to push models toward better "thinking tokens" for step-by-step problem-solving.

#### Quality Filtering

 Not all data is good data. How do you automatically filter out poor-quality samples and keep only the high-value ones?

#### Small to Large Contexts

 Your dataset has small chunks of context, but you want to build larger-context datasets optimized for RAG (Retrieval-Augmented Generation) pipelines.

#### Cross-Language Conversion

 You have German datasets but need to translate, adapt, and repurpose them into English Q&A systems. And the list goes on. The needs around data building never end when working with modern AI models.

* * *

## Enter SyGra: One Framework for Every Data Challenge

This is where [SyGra](https://github.com/ServiceNow/SyGra) comes in. [SyGra](https://github.com/ServiceNow/SyGra) is a low-code/no-code framework designed to simplify dataset creation, transformation, and alignment for LLMs and SLMs. Instead of writing complex scripts and pipelines, you can focus on prompt engineering, while [SyGra](https://github.com/ServiceNow/SyGra) takes care of the heavy lifting. \
Key Features of [SyGra](https://github.com/ServiceNow/SyGra):

- ✅ Python Library + Framework: Easy to integrate into existing ML workflows with SyGra library.
- ✅ Supports Multiple Inference Backends: Works seamlessly with vLLM, Hugging Face TGI, Triton, Ollama, and more.
- ✅ Low-Code/No-Code: Build complex datasets without heavy engineering effort.
- ✅ Flexible Data Generation: From Q&A to DPO, reasoning to multi-language, SyGra adapts to your use case.

![](https://raw.githubusercontent.com/ServiceNow/SyGra/refs/heads/main/docs/resources/images/component_model_client.png)

* * *

## Why SyGra Matters

Data is the foundation of AI. The quality, diversity, and structure of your data often matter more than model architecture tweaks. By enabling flexible and scalable dataset creation, SyGra helps teams:

- Accelerate model alignment (SFT, DPO, RAG pipelines).
- Save engineering time with plug-and-play workflows.
- Improve model robustness across complex and domain-specific tasks.
- Reduce manual dataset curation effort.

Note: An example implementation can be found at [https://github.com/ServiceNow/SyGra/blob/main/docs/tutorials/image_to_qna_tutorial.md](https://github.com/ServiceNow/SyGra/blob/main/docs/tutorials/image_to_qna_tutorial.md)

## SyGra Architecture

![](https://raw.githubusercontent.com/ServiceNow/SyGra/refs/heads/main/docs/resources/images/sygra_architecture.png)

**Few Example Task** [https://github.com/ServiceNow/SyGra/tree/main/tasks/examples](https://github.com/ServiceNow/SyGra/tree/main/tasks/examples)

## Final Thoughts

The journey of building and refining datasets never ends. Each use case brings new challenges - from translation and knowledge base conversion to reasoning enhancement and domain filtering. With SyGra, you don't have to reinvent the wheel every time. Instead, you get a unified framework that empowers you to generate, filter, and align data for your models - so you can focus on what really matters: building smarter AI systems.

## References

- Paper Link: https://arxiv.org/abs/2508.15432
- Documentation: https://servicenow.github.io/SyGra/
- Git Repository: https://github.com/ServiceNow/SyGra
