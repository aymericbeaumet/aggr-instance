---
title: 'PaddleOCR 3.5: Running OCR and Document Parsing Tasks with a Transformers Backend'
link: https://huggingface.co/blog/PaddlePaddle/paddleocr-transformers
source: huggingface-co-blog
published: 2026-05-18T15:12:46Z
updated: 2026-05-18T15:12:46Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2026-05-18-paddleocr-3-5-running-ocr-and-document-parsing-tasks-with-a.html
preview:
  file: 2026-05-18-paddleocr-3-5-running-ocr-and-document-parsing-tasks-with-a.preview-53ded18a466c.webp
  width: 256
  height: 134
  color: '#edeff2'
images:
- source: https://cdn-uploads.huggingface.co/production/uploads/652b2e9166313ebb6197e706/Bl-qDWU9OfuXZ06hNxZE7.jpeg
  original:
    file: 2026-05-18-paddleocr-3-5-running-ocr-and-document-parsing-tasks-with-a.image-6b0d02692a9b.jpg
    width: 1731
    height: 909
  color: '#f9f9f8'
---

PaddleOCR 3.5 brings OCR and document parsing tasks closer to the Hugging Face ecosystem. With this release, supported PaddleOCR models can run with **Hugging Face Transformers as an inference backend** by setting:

```
engine="transformers"
```

PaddleOCR continues to provide OCR model series such as **PP-OCRv5** and document parsing model series such as **PaddleOCR-VL 1.5**, while Transformers becomes one of the supported backends for running them.

Try the live demo on Hugging Face Spaces: [https://huggingface.co/spaces/PaddlePaddle/paddleocr-3.5-transformers-demo](https://huggingface.co/spaces/PaddlePaddle/paddleocr-3.5-transformers-demo)

## [](https://huggingface.co/blog/PaddlePaddle/paddleocr-transformers#what-changed) What changed?

PaddleOCR 3.5 introduces a more flexible inference-engine interface. Developers can select the backend through the `engine` parameter and pass backend-specific options through `engine_config`.

In practice, this means:

- The pipelines behind these tasks are managed by PaddleOCR, so developers do not need to manually call each internal component.
- Transformers becomes one of the supported inference backends for running supported PaddleOCR models.
- Developers can configure backend-related options such as `dtype`, device placement, and attention implementation through `engine_config`.

A simple way to understand the stack:

| Layer                       | What it means                                          | Examples                                                |
| --------------------------- | ------------------------------------------------------ | ------------------------------------------------------- |
| **Application layer**       | Applications that use OCR and document parsing outputs | RAG, agents, Document AI...                             |
| **Model layer**             | OCR and document parsing capabilities                  | PP-OCRv5, PaddleOCR-VL 1.5...                           |
| **Inference backend layer** | Runtime used to run supported models                   | Paddle static graph, Paddle dynamic graph, Transformers |

This release is mainly about the inference backend layer: PaddleOCR continues to provide OCR and document parsing capabilities, while Transformers gives supported PaddleOCR models another backend option that fits naturally into Hugging Face-centered environments. The larger Document AI workflow remains in the hands of developers and application builders.

## [](https://huggingface.co/blog/PaddlePaddle/paddleocr-transformers#why-this-matters) Why this matters

For RAG, Document AI, and document agent applications, the hard part often starts before the LLM.

Developers first need to turn PDFs, scanned documents, screenshots, tables, charts, formulas, and complex page layouts into reliable structured data. If this ingestion step is weak, the downstream LLM workflow may miss key information, retrieve the wrong context, or produce unreliable answers.

PaddleOCR helps address this document ingestion challenge by providing OCR series models such as PP-OCRv5 and document parsing series models such as PaddleOCR-VL-1.5.

With PaddleOCR 3.5, these capabilities are now easier to connect with Transformers-centered stacks. Supported PaddleOCR models can run with a Transformers backend, while PaddleOCR continues to manage the OCR or document parsing pipeline behind the scenes.

For developers, this means less integration friction and a more natural path from documents to downstream RAG, agent, search, analytics, or automation workflows.

## [](https://huggingface.co/blog/PaddlePaddle/paddleocr-transformers#quick-start) Quick start

Install PaddleOCR 3.5, PaddleX, Transformers, and a compatible PyTorch build for your hardware.

For example, on a CUDA 12.6 environment:

```
python -m pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu126
python -m pip install "paddleocr==3.5.0" "paddlex==3.5.2" "transformers>=5.4.0"
```

For CPU, ROCm, or other environments, install the PyTorch build that matches your target hardware.

Run from the command line:

```
paddleocr ocr \
  -i https://paddle-model-ecology.bj.bcebos.com/paddlex/imgs/demo_image/general_ocr_002.png \
  --device gpu:0 \
  --engine transformers
```

Or use the Python API:

```
from paddleocr import PaddleOCR

pipeline = PaddleOCR(
    device="gpu:0",
    engine="transformers",
    use_doc_orientation_classify=False,
    use_doc_unwarping=False,
    use_textline_orientation=False,
    engine_config={
        "dtype": "float32",
    },
)

results = pipeline.predict(
    "https://paddle-model-ecology.bj.bcebos.com/paddlex/imgs/demo_image/general_ocr_002.png"
)

for result in results:
    print(result)
```

The Hugging Face Space uses `float32` for broad compatibility. For your own hardware, you can tune backend-specific options through `engine_config`:

```
engine_config = {
    "dtype": "bfloat16",
    "device_type": "gpu",
    "device_id": 0,
    "attn_implementation": "sdpa",
}
```

The best configuration depends on your model, hardware, and deployment environment.

## [](https://huggingface.co/blog/PaddlePaddle/paddleocr-transformers#when-should-you-use-the-transformers-backend) When should you use the Transformers backend?

Use the Transformers backend when you want PaddleOCR’s OCR and document parsing capabilities to fit more naturally into a Hugging Face-centered stack.

This is especially useful if you are building RAG, Document AI, search, analytics, or agent applications and already rely on PyTorch / Transformers infrastructure for model loading, experimentation, deployment, or model artifact management.

The Transformers backend is a good fit when you want:

- a more familiar development experience for teams already using Transformers,
- Hub-compatible model discovery and distribution for supported PaddleOCR models,
- easier integration with existing PyTorch / Transformers services.

When maximizing OCR or document parsing throughput is the priority, PaddleOCR’s default `paddle_static` backend is usually the recommended choice.

This release is not about replacing one backend with another. It is about giving developers more flexibility: use PaddleOCR for OCR and document parsing capabilities, and choose the inference backend that best fits your stack.

## [](https://huggingface.co/blog/PaddlePaddle/paddleocr-transformers#try-it-now) Try it now

Try the PaddleOCR 3.5 Transformers demo on Hugging Face Spaces:

[https://huggingface.co/spaces/PaddlePaddle/paddleocr-3.5-transformers-demo](https://huggingface.co/spaces/PaddlePaddle/paddleocr-3.5-transformers-demo)

Explore PaddleOCR models on the Hub:

[https://huggingface.co/PaddlePaddle/models](https://huggingface.co/PaddlePaddle/models)

PaddleOCR 3.5 brings OCR and document parsing capabilities closer to Transformers-centered workflows, while giving developers the freedom to build the larger Document AI applications around them.

## [](https://huggingface.co/blog/PaddlePaddle/paddleocr-transformers#resources) Resources

- PaddleOCR documentation: [https://www.paddleocr.ai/](https://www.paddleocr.ai/)
- PaddleOCR on GitHub: [https://github.com/PaddlePaddle/PaddleOCR](https://github.com/PaddlePaddle/PaddleOCR)
- PaddlePaddle organization on Hugging Face: [https://huggingface.co/PaddlePaddle](https://huggingface.co/PaddlePaddle)
- PaddleOCR 3.5 Transformers demo on Spaces: [https://huggingface.co/spaces/PaddlePaddle/paddleocr-3.5-transformers-demo](https://huggingface.co/spaces/PaddlePaddle/paddleocr-3.5-transformers-demo)

## [](https://huggingface.co/blog/PaddlePaddle/paddleocr-transformers#acknowledgements) Acknowledgements

We sincerely thank the Hugging Face engineers who supported the PaddleOCR 3.5 Transformers integration.

Special thanks to [Anton Vlasjuk](https://huggingface.co/AntonV) for his end-to-end involvement, including reviewing and merging all related pull requests.

We also appreciate [Raushan Turganbay](https://huggingface.co/RaushanTurganbay) and [Yoni Gozlan](https://huggingface.co/yonigozlan) for their valuable PR reviews and feedback.

Their guidance helped improve the integration quality, documentation, and developer experience for the Hugging Face community.
