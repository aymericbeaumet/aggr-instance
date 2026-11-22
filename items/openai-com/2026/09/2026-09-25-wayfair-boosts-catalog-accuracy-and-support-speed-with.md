---
title: Wayfair boosts catalog accuracy and support speed with OpenAI
link: https://openai.com/index/wayfair
source: openai-com
published: 2026-09-25T00:00:00Z
updated: 2026-09-25T00:00:00Z
first_seen: 2026-10-06T00:45:55.302474734Z
summary: Wayfair uses OpenAI models to improve ecommerce support and product catalog accuracy, automating ticket triage and enhancing millions of product attributes at scale.
content: extracted
html: 2026-09-25-wayfair-boosts-catalog-accuracy-and-support-speed-with.html
---

Wayfair, the destination of all things home, helps shoppers find the right pieces for their homes from a catalog of roughly 40 million items. Keeping product information accurate and handling complex requests from tens of thousands of suppliers required substantial manual work. With OpenAI models, Wayfair built AI systems that validated over 11 million product specs and automated work across 41,000 supplier support tickets a month, with automation reaching up to 70% of tickets in some workflows. Together, these systems created more opportunities to grow sales from Wayfair’s existing catalog while freeing up team capacity by reducing manual work in catalog management and supplier support.

> “What’s been most valuable about working with OpenAI is the partnership. It’s not just access to the models. It’s working through new use cases together and being able to move quickly.”

—Fiona Tan, Chief Technology Officer, Wayfair

## Solving catalog quality at scale

Wayfair’s catalog team manages tens of millions of products across nearly a thousand different product classes. Accurate tags for color, material, size, and other features power search, recommendations, and merchandising—helping shoppers more easily find pieces that fit their needs and feel confident about what they’re buying.

“The better our data quality, the more trust we build with the customer. It’s essential because it empowers shoppers to make the right buying decisions, directly reducing costly downstream issues like returns from misrepresented products,” said Jessica D’Arcy, Associate Director of Catalog Merchandising at Wayfair.

Before partnering with OpenAI, Wayfair’s tagging relied on suppliers and customers telling Wayfair that something looked wrong. Manual effort could not keep up with the volume. Early custom AI models for tagging were effective, but proved expensive at scale. With 47,000 tags to support, Wayfair needed a more effective approach.

## Building a reusable AI system for product attributes

![UI screenshot of an AI product quality review for a “Round Walnut Solid Wood Coffee Table, 28.7”.” On the left is a product photo of a low round wooden coffee table with cylindrical legs and a vase on top. On the right is a table comparing Original Value vs AI Correction for product attributes. The AI flags several issues: correcting wood species from Walnut to Pine, changing leg design from Bun Feet to Straight Legs, marking Unfinished and Scalloped Edges as No, and adding Drawers Included: No. Dimensions and tabletop thickness remain unchanged. A banner indicates AI Quality Review – 5 issues found, and a footer notes 4 corrections made, 1 attribute added, 2 attributes verified, with all corrections applied automatically.](https://images.ctfassets.net/kftzwdyauwt9/6OP1wkY8q6cQDaebaOCy8U/513037c865bc884734619fe30dec1b7e/wayfair_16x9_AIQR.png?w=3840&q=90&fm=webp)

Wayfair built an AI catalog quality system with OpenAI models to check and label product details, such as color, material, and size. First, the system uses web information and Wayfair’s internal definitions to create a guide explaining what each label means. The models then use that guide to review product information and assign the appropriate labels.

With this system, Wayfair is now expanding the range of product details its AI can check at 70 times last year’s pace. That helps Wayfair improve catalog accuracy with less manual work—making products easier to find and reducing the risk of returns caused by inaccurate listings.

“Our objective is to build trust so that customers are completely confident in what they are purchasing,” said Phillips. Wayfair developed a structured audit process in which associates inspect product samples to check the output, and suppliers help validate changes. When confidence is high, the system updates product content directly and notifies the supplier. Low-confidence or high-risk changes require supplier confirmation before taking effect.

The AI-powered catalog quality system has processed more than 1 million products, and a controlled A/B test showed a significant increase in impressions, clicks, and page rank in the treatment group. For Wayfair, better product data translated into greater visibility and more shopper clicks—creating more opportunities to generate sales from its existing catalog. “When you improve attribute completeness, it’s not abstract. You see it show up in SEO and PLA performance—in how customers discover products,” said Phillips.

## Accelerating supplier support workflows with Wilma

Wayfair works with tens of thousands of suppliers each month. Wayfair associates previously reviewed each incoming supplier support ticket manually, identified what the supplier needed, and routed the issue to the appropriate team. This was a time-consuming and error-prone process. “Supplier support covers hundreds of different issue types,” said Brian Seaman, Head of Applied Science for Global Supplier at Wayfair. “It’s not realistic to expect any one associate to have deep expertise in every one of them.”

To help address these challenges, Wayfair added AI to help triage tickets in its custom supplier support platform called Wilma. With OpenAI models, the supplier support platform can now read incoming requests, fill in missing context, and route tickets to the right team. The existing OpenAI API integration helped the team move from idea to production in under one month. “Wilma gives associates the information they need to act,” said Seaman. “It reads the request, identifies what the supplier needs, brings in context from our databases, follows up with the supplier when needed, and routes the issue to the right team.”

Beyond routing tickets, Wayfair has deployed dozens of AI agentic workflows to help teams resolve supplier requests. For example, AI reviews past records, assesses complex cases, suggests next steps, and drafts responses for the Replacement Parts Operations team to review—helping associates resolve supplier issues faster with less manual effort.

> “OpenAI models bring together context from across the entire journey that would be difficult for one associate to piece together. That visibility helps us provide better support and contributes to higher customer and supplier satisfaction.”

—Brian Seaman, Head of Applied Science for Global Supplier at Wayfair

Wayfair evaluates how often the AI recommendations match the human agent’s final decision, a metric called “alignment rate.” Workflows that consistently reach a predetermined threshold can shift from assistive (“co-pilot”) to semi-autonomous (“autopilot”) modes. This staged evaluation approach helps teams decide when a workflow is ready for greater autonomy.

## Bringing ChatGPT to teams across Wayfair

Beyond building custom AI workflows, Wayfair has deployed more than 1,200 ChatGPT Enterprise seats across its approximately 12,000-person workforce. Employees use ChatGPT to tackle everyday tasks, solve internal problems, and experiment with generative models. Wayfair has also enabled ChatGPT Work, connecting it to tools like Slack and Google Drive to help teams streamline their day-to-day workflows.

## Results at a glance

On the catalog side, Wayfair validated and corrected attribute data across more than 11 million of its most visible and frequently purchased products. Next, Wayfair plans to deploy the capability to cover every new product added to the catalog, helping to maximize product visibility and sales from the very beginning of the product lifecycle.

In supplier support, Wayfair automates 41,000 support tickets per month, with automation now reaching up to 70% of total ticket volume in some workflows. Teams have also seen much faster resolution times, higher supplier satisfaction, and fewer reopened tickets.

Wayfair has reported a few additional results:

- Saving teams hundreds of hours of work from manual data entry and classification

- Broader issue coverage without requiring expertise across hundreds of topics

- Greater confidence in catalog attributes before publication

## What’s next

Looking ahead, Wayfair continues to explore how OpenAI’s latest models can solve complex problems and reimagine the shopping experience. “We’re excited about the scope of problems we can now tackle,” said Carolyn Phillips. “Traditional algorithms require tightly defined datasets. OpenAI models allow us to work through ambiguity and context in a way that wasn’t previously scalable.”

For Wayfair, Intelligence at Work means bringing together its teams’ expertise and OpenAI’s technology to improve product discovery and make supplier support more efficient. “We’re building for a world where AI is part of the shopping journey—whether that’s on our site, through support, or through conversational interfaces,” concluded Fiona Tan. The opportunity ahead is to continue using AI to lower operating costs, strengthen customer loyalty, and drive growth.

40 million products. The right piece finds its place. Intelligence at Work.
