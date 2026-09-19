---
title: From Hugging Face to Amazon SageMaker Studio in one click
link: https://huggingface.co/blog/amazon/one-click-to-sagemaker-studio
source: huggingface-co-blog
published: 2026-07-07T21:15:33Z
updated: 2026-07-07T21:15:33Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2026-07-07-from-hugging-face-to-amazon-sagemaker-studio-in-one-click.html
preview:
  file: 2026-07-07-from-hugging-face-to-amazon-sagemaker-studio-in-one-click.preview-83d89fca99ee.webp
  width: 256
  height: 256
  color: '#252d32'
images:
- source: https://cdn-uploads.huggingface.co/production/uploads/68abb71b13d7773ad97e9035/f1nX3dXVmvJcQzJfgfpnJ.webp
  original:
    file: 2026-07-07-from-hugging-face-to-amazon-sagemaker-studio-in-one-click.image-861742923f66.webp
    width: 1024
    height: 1024
  color: '#050b13'
- source: https://d2908q01vomqb2.cloudfront.net/f1f836cb4ea6efb2a0b1b99f41ad8b103eff4b59/2026/07/06/ML-21254-1.png
  original:
    file: 2026-07-07-from-hugging-face-to-amazon-sagemaker-studio-in-one-click.image-446e1c4c23fd.png
    width: 1266
    height: 680
  variants:
  - file: 2026-07-07-from-hugging-face-to-amazon-sagemaker-studio-in-one-click.image-8c0c1308ce9e.webp
    width: 320
    height: 172
  - file: 2026-07-07-from-hugging-face-to-amazon-sagemaker-studio-in-one-click.image-0e1b80a409e2.webp
    width: 640
    height: 344
  - file: 2026-07-07-from-hugging-face-to-amazon-sagemaker-studio-in-one-click.image-8fa55abca879.webp
    width: 960
    height: 516
  - file: 2026-07-07-from-hugging-face-to-amazon-sagemaker-studio-in-one-click.image-5f872c8d7cf4.webp
    width: 1266
    height: 680
  color: '#fcfcfc'
- source: https://d2908q01vomqb2.cloudfront.net/f1f836cb4ea6efb2a0b1b99f41ad8b103eff4b59/2026/07/06/ML-21254-2.png
  original:
    file: 2026-07-07-from-hugging-face-to-amazon-sagemaker-studio-in-one-click.image-335237666717.png
    width: 1728
    height: 1162
  color: '#1a1b21'
- source: https://d2908q01vomqb2.cloudfront.net/f1f836cb4ea6efb2a0b1b99f41ad8b103eff4b59/2026/07/06/ML-21254-3.png
  original:
    file: 2026-07-07-from-hugging-face-to-amazon-sagemaker-studio-in-one-click.image-1271550ee2e1.png
    width: 1950
    height: 1166
  color: '#1a1b21'
---

Today, we’re excited to announce a deep-link integration between [Hugging Face](https://huggingface.co/) and [Amazon SageMaker AI](https://aws.amazon.com/sagemaker/ai/studio/). Developers can now go from model discovery to hands-on experimentation in SageMaker Studio with a single selection. Whether you fine-tune a foundation model (FM) from [Amazon SageMaker JumpStart](https://aws.amazon.com/sagemaker/ai/jumpstart/) or deploy it to an [Amazon SageMaker Inference](https://aws.amazon.com/sagemaker/ai/deploy/) endpoint, you can now land directly inside the relevant SageMaker Studio workflow. Your selected model is pre-loaded, and the environment is fully configured and ready to go.

Previously, getting started on SageMaker Studio after discovering a model on Hugging Face required navigating multiple steps between opening Amazon SageMaker AI in the AWS Console, creating a domain, configuring IAM permissions, and sometimes requesting GPU quota. For developers who want to iterate quickly, this friction slows down the path from inspiration to experimentation. The integration creates a more direct path from discovery to enterprise deployment.

> *“At Arcee, we build open models so developers and enterprises can actually own what they run: inspect the weights, post-train on their own data, and deploy on their own terms. This integration takes that promise the last mile. Going from an open model on Hugging Face straight into SageMaker Studio in a single click, then fine-tuning or deploying it inside your own AWS environment with nothing to wire up, is the kind of experience open models have been missing. Open weights you own, running in the cloud you control. That is exactly the combination our customers have been asking for.”*

— Mark McQuade, Founder and CEO, Arcee AI

With the launch of a one-click Studio landing experience, choosing *Customize on SageMaker AI* or *Deploy on SageMaker AI* on a supported Hugging Face model page takes you directly to the console. SageMaker AI then automatically provisions a new domain with pre-configured permissions in seconds and carries the model context through.

## [](https://huggingface.co/blog/amazon/one-click-to-sagemaker-studio#whats-new) What’s new

This launch introduces three capabilities that shorten the path from a Hugging Face model to a working SageMaker Studio workflow.

### [](https://huggingface.co/blog/amazon/one-click-to-sagemaker-studio#deep-links-from-hugging-face-into-sagemaker-studio) Deep links from Hugging Face into SageMaker Studio

When you browse models on Hugging Face, you’ll now see action buttons alongside supported models that map directly to SageMaker Studio workflows:

- **Customize on SageMaker AI** opens the Model Customization page in Studio with the selected model pre-loaded, ready to fine-tune.
- **Deploy on SageMaker AI** opens the Deployment page in Studio with the model pre-configured for endpoint deployment.

Each entry point preserves the context, meaning you don’t need to search for the model again once inside Studio.

### [](https://huggingface.co/blog/amazon/one-click-to-sagemaker-studio#pre-configured-permissions) Pre-configured permissions

New Studio environments created through this flow come with permissions already configured for the full range of SageMaker AI capabilities, including model customization, training jobs, notebook experimentation, and endpoint deployment. A new managed policy, [AmazonSageMakerModelCustomizationCoreAccess](https://aws.amazon.com/about-aws/whats-new/2026/01/quick-setup-model-customization-sagemaker-studio/), is created and attached for you. It provides permissions for serverless model customization jobs using supervised fine-tuning (SFT), direct preference optimization (DPO), reinforcement learning with verifiable rewards (RLVR), and reinforcement learning from AI feedback (RLAIF), with supported deployment to SageMaker AI or Amazon Bedrock endpoints. This alleviates the need to manually create and configure AWS Identity and Access Management (IAM) roles and policies before you can start experimenting. For existing Studio environments, actionable messages with direct links to documentation guide you through adding these permissions.

### [](https://huggingface.co/blog/amazon/one-click-to-sagemaker-studio#gpu-quota-visibility) GPU quota visibility

When selecting instance types for deployment or training, the Studio UI now surfaces quota availability directly in the instance selection list. You can immediately see which GPU instance types (G5, G6) are available under your account’s current limits. You don’t need to navigate separately to Service Quotas. If you still need to request a limit increase, you’re redirected directly to the Service Quotas page for the respective instance type.

## [](https://huggingface.co/blog/amazon/one-click-to-sagemaker-studio#walkthrough-deep-linking-from-hugging-face-to-sagemaker-studio) Walkthrough: Deep-linking from Hugging Face to SageMaker Studio

Let’s walk through the experience of customizing or deploying a model starting from Hugging Face.

### [](https://huggingface.co/blog/amazon/one-click-to-sagemaker-studio#step-1-discover-and-select) Step 1: Discover and select

On the Hugging Face model page, click on “Deploy” and select “Amazon SageMaker AI”. If the model is supported, you will see two buttons, “Deploy on SageMaker AI” and “Customize on SageMaker AI”. Then select “Customize on SageMaker AI” for a supported model.

[![Hugging Face model page showing the Customize on SageMaker AI button for a supported model](https://d2908q01vomqb2.cloudfront.net/f1f836cb4ea6efb2a0b1b99f41ad8b103eff4b59/2026/07/06/ML-21254-1.png)](https://d2908q01vomqb2.cloudfront.net/f1f836cb4ea6efb2a0b1b99f41ad8b103eff4b59/2026/07/06/ML-21254-1.png)

### [](https://huggingface.co/blog/amazon/one-click-to-sagemaker-studio#step-2-sign-in) Step 2: Sign in

You’re prompted to sign in to AWS using your existing credentials. If you already have an active console session, this step is skipped automatically. For more information, see [Sign in to the AWS Management Console](https://docs.aws.amazon.com/signin/latest/userguide/how-to-sign-in.html).

### [](https://huggingface.co/blog/amazon/one-click-to-sagemaker-studio#step-3-land-in-studio) Step 3: Land in Studio

You arrive directly on the Model Customization page inside SageMaker Studio with your model pre-selected. Next, configure your fine-tuning parameters such as training data, hyperparameters, and instance type, then submit the customization job.

[![SageMaker Studio Model Customization page with the selected model pre-loaded and fine-tuning parameters ready to configure](https://d2908q01vomqb2.cloudfront.net/f1f836cb4ea6efb2a0b1b99f41ad8b103eff4b59/2026/07/06/ML-21254-2.png)](https://d2908q01vomqb2.cloudfront.net/f1f836cb4ea6efb2a0b1b99f41ad8b103eff4b59/2026/07/06/ML-21254-2.png)

Alternatively, selecting **Deploy on SageMaker AI** opens the endpoint deployment page in Studio with the model pre-configured. Select your instance type (quota visibility included), review the settings, and deploy.

[![SageMaker Studio endpoint deployment page with the model pre-configured and instance type selection showing quota visibility](https://d2908q01vomqb2.cloudfront.net/f1f836cb4ea6efb2a0b1b99f41ad8b103eff4b59/2026/07/06/ML-21254-3.png)](https://d2908q01vomqb2.cloudfront.net/f1f836cb4ea6efb2a0b1b99f41ad8b103eff4b59/2026/07/06/ML-21254-3.png)

### [](https://huggingface.co/blog/amazon/one-click-to-sagemaker-studio#step-4-test-your-endpoint) Step 4: Test your endpoint

After you deploy your endpoint, test inference directly from Studio’s endpoint [testing interface](https://docs.aws.amazon.com/sagemaker/latest/dg/manage-endpoints-studio-test.html).

## [](https://huggingface.co/blog/amazon/one-click-to-sagemaker-studio#getting-started) Getting started

You can try this experience today:

1. Browse models on Hugging Face.
2. Look for the **Customize on SageMaker AI** or **Deploy on SageMaker AI** buttons on supported models.
3. Select and follow the streamlined sign-in flow.
4. Start building in a fully configured SageMaker Studio environment.

## [](https://huggingface.co/blog/amazon/one-click-to-sagemaker-studio#conclusion) Conclusion

The launch of a one-click Studio landing experience minimizes the friction between discovering a model and experimenting with it. By connecting Hugging Face directly to the SageMaker Studio workflows, developers can stay in their flow. There’s no context switching, no manual environment setup, and no permission troubleshooting.

To get started, visit the [Amazon SageMaker Studio](https://aws.amazon.com/sagemaker/ai/studio/) page or explore models on Hugging Face and choose Deploy or Customize on SageMaker AI.
