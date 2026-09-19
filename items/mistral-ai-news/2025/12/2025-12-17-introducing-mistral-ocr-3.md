---
title: Introducing Mistral OCR 3
link: https://mistral.ai/news/mistral-ocr-3/
source: mistral-ai-news
published: 2025-12-17T15:00:00Z
updated: 2025-12-17T15:00:00Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2025-12-17-introducing-mistral-ocr-3.html
preview:
  file: 2025-12-17-introducing-mistral-ocr-3.preview-5586126838b8.webp
  width: 256
  height: 153
  color: '#aea4df'
images:
- source: https://mistral.ai/cms-media/api/media/file/thumbnail-ocr.jpg
  original:
    file: 2025-12-17-introducing-mistral-ocr-3.image-1b6a63bf2b55.jpg
    width: 1200
    height: 716
  color: '#fda8e6'
- source: https://mistral.ai/_astro/71a86b4e-b67e-49c0-b2a2-57ffdb42717f_x6QSn.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2025-12-17-introducing-mistral-ocr-3.image-f0131bf55a21.webp
    width: 1920
    height: 1065
  variants:
  - file: 2025-12-17-introducing-mistral-ocr-3.image-2b6d8dce61dd.webp
    width: 320
    height: 178
  - file: 2025-12-17-introducing-mistral-ocr-3.image-e1101d2820b5.webp
    width: 640
    height: 355
  - file: 2025-12-17-introducing-mistral-ocr-3.image-0805e751fccb.webp
    width: 960
    height: 533
  color: '#fdf9eb'
- source: https://mistral.ai/_astro/00408f9b-0cb7-447c-b4f8-bdefc4a3f3dc_Z1de4qo.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2025-12-17-introducing-mistral-ocr-3.image-f63f86e70916.webp
    width: 1920
    height: 975
  variants:
  - file: 2025-12-17-introducing-mistral-ocr-3.image-cf195382f9dd.webp
    width: 320
    height: 163
  - file: 2025-12-17-introducing-mistral-ocr-3.image-a8696eba3df3.webp
    width: 640
    height: 325
  - file: 2025-12-17-introducing-mistral-ocr-3.image-2b1e6218c700.webp
    width: 960
    height: 488
  color: '#fdf9ea'
- source: https://mistral.ai/_astro/1682ebdd-99f1-46d4-9d26-36a716c6f2fb_1s4nW7.webp?dpl=6aad049eaf4c2d00095b91e5
  original:
    file: 2025-12-17-introducing-mistral-ocr-3.image-c5454b81d472.webp
    width: 1294
    height: 943
  variants:
  - file: 2025-12-17-introducing-mistral-ocr-3.image-3893736f8d6f.webp
    width: 320
    height: 233
  - file: 2025-12-17-introducing-mistral-ocr-3.image-664ecd082258.webp
    width: 640
    height: 466
  color: '#fdf9ea'
---

### Highlights

- Breakthrough performance: 74% overall win rate over Mistral OCR 2 on forms, scanned documents, complex tables, and handwriting.

- State-of-the-art accuracy, outperforming both enterprise document processing solutions as well as AI-native OCR solutions

- Now powers Document AI Playground in [Mistral AI Studio](https://console.mistral.ai/build/document-ai/ocr-playground), a simple drag-and-drop interface for parsing PDFs/images into clean text or structured JSON

- Major upgrade over Mistral OCR 2 in forms, handwritten content, low-quality scans, and tables

### Overview

Mistral OCR 3 is designed to extract text and embedded images from a wide range of documents with exceptional fidelity. It supports markdown output enriched with HTML-based table reconstruction, enabling downstream systems to understand not just document content, but also structure. As a much smaller model than most competitive solutions, it is available at an industry-leading price of $2 per 1,000 pages, with a 50% Batch-API discount, reducing the cost to $1 per 1,000 pages.

Developers can integrate the model (mistral-ocr-2512) via API, and users can leverage Document AI, a UI that parses documents into text or structured JSON instantly.

### Benchmarks

To raise the bar, we introduced more challenging internal benchmarks based on real business use-case examples from customers. We then evaluated several models across the domains highlighted below, comparing their outputs to ground truth using fuzzy-match metric for accuracy.

![Ocr Multilangual](https://mistral.ai/_astro/71a86b4e-b67e-49c0-b2a2-57ffdb42717f_x6QSn.webp?dpl=6aad049eaf4c2d00095b91e5)

![Ocr 3](https://mistral.ai/_astro/00408f9b-0cb7-447c-b4f8-bdefc4a3f3dc_Z1de4qo.webp?dpl=6aad049eaf4c2d00095b91e5)

### Upgrades over previous generations of OCR models

Whereas most OCR solutions today specialize in specific document types, Mistral OCR 3 is designed to excel at processing the vast majority of document types in organizations and everyday settings.

- Handwriting: Mistral OCR accurately interprets cursive, mixed-content annotations, and handwritten text layered over printed forms.

- Forms: Improved detection of boxes, labels, handwritten entries, and dense layouts. Works well on invoices, receipts, compliance forms, government documents, and such.

- Scanned & complex documents: Significantly more robust to compression artifacts, skew, distortion, low DPI, and background noise.

- Complex tables: Reconstructs table structures with headers, merged cells, multi-row blocks, and column hierarchies. Outputs HTML table tags with colspan/rowspan to fully preserve layout.

Mistral OCR 3 is a significant upgrade across all languages and document form factors compared to Mistral OCR 2.

![Win Rates   Mistral Ocr 3 Vs Ocr 2](https://mistral.ai/_astro/1682ebdd-99f1-46d4-9d26-36a716c6f2fb_1s4nW7.webp?dpl=6aad049eaf4c2d00095b91e5)

### Recommend use cases and applications

Mistral OCR 3 is ideal for both high-volume enterprise pipelines and interactive document workflows. Developers can use it for:

- Extracting text and images into markdown for downstream agents and knowledge systems

- Automated parsing of forms, invoices, and operational documents

- End-to-end document understanding pipelines

- Digitization of handwritten or historical documents

- Any other document → knowledge transformation applications.

Our early customers are using Mistral OCR 3 to process invoices into structured fields, digitize company archives, extract clean text from technical and scientific reports, and improve enterprise search.

“OCR remains foundational for enabling generative AI and agentic AI,” said Tim Law, IDC Director of Research for AI and Automation. “Those organizations that can efficiently and cost-effectively extract text and embedded images with high fidelity will unlock value and will gain a competitive advantage from their data by providing richer context.”

### Available today

Access the model either through the API or via the new Document AI Playground interface, both in [Mistral AI Studio](https://console.mistral.ai/build/document-ai/ocr-playground). Mistral OCR 3 is fully backward compatible with Mistral OCR 2. For more details, head over to [mistral.ai/docs](https://docs.mistral.ai/capabilities/document_ai/basic_ocr).

For organizations with stringent data privacy requirements, Mistral OCR offers a self-hosting option. This ensures that sensitive or classified information remains secure within your own infrastructure, providing compliance with regulatory and security standards. If you would like to explore self-deployment with us, please [let us know](https://mistral.ai/contact).
