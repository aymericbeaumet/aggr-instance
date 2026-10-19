---
title: 'PP-OCRv6 on Hugging Face: 50-Language OCR from 1.5M to 34.5M Parameters'
link: https://huggingface.co/blog/PaddlePaddle/pp-ocrv6
source: huggingface-co
published: 2026-06-22T13:18:56Z
updated: 2026-06-22T13:18:56Z
first_seen: 2026-10-19T01:26:53.998049939Z
content: extracted
html: 2026-06-22-pp-ocrv6-on-hugging-face-50-language-ocr-from-1-5m-to-34-5m.html
remote_preview:
  url: https://cdn-uploads.huggingface.co/production/uploads/652b2e9166313ebb6197e706/l156KP0e5PRyDrE4woHoM.jpeg
---

> **Evaluate PP-OCRv6 online, then integrate lightweight, production-ready OCR with PaddlePaddle, Transformers, or ONNX Runtime backend.**

PP-OCRv6 is the latest generation of PaddleOCR’s universal OCR model family. It is designed for real-world text detection and recognition across documents, screenshots, multilingual images, digital displays, industrial labels, and scene text.

[![ppocrv6_det_vis](https://cdn-uploads.huggingface.co/production/uploads/652b2e9166313ebb6197e706/BB9bToA0xHZ8Xu5cvBGx0.jpeg)](https://cdn-uploads.huggingface.co/production/uploads/652b2e9166313ebb6197e706/BB9bToA0xHZ8Xu5cvBGx0.jpeg)

The model family scales from **1.5M to 34.5M parameters**, with three tiers: **tiny**, **small**, and **medium**. The medium and small tiers support **50 languages**, including Simplified Chinese, Traditional Chinese, English, Japanese, and 46 Latin-script languages. Try PP-OCRv6 online quickly: [PP-OCRv6 Online Demo](https://huggingface.co/spaces/PaddlePaddle/PP-OCRv6_Online_Demo).

[![ocrv6_models](https://cdn-uploads.huggingface.co/production/uploads/652b2e9166313ebb6197e706/Rwhy6dk3g8xb6eyVFL9R_.jpeg)](https://cdn-uploads.huggingface.co/production/uploads/652b2e9166313ebb6197e706/Rwhy6dk3g8xb6eyVFL9R_.jpeg)

On PaddleOCR’s official in-house multi-scenario OCR benchmarks, **PP-OCRv6\_medium** reaches **86.2% detection Hmean** and **83.2% recognition accuracy**. Compared with PP-OCRv5\_server, it improves text detection by **+4.6 percentage points** and text recognition by **+5.1 percentage points**.

[![v6acc_opt](https://cdn-uploads.huggingface.co/production/uploads/652b2e9166313ebb6197e706/xYqS-wDYHDE7cUQQdkcqD.png)](https://cdn-uploads.huggingface.co/production/uploads/652b2e9166313ebb6197e706/xYqS-wDYHDE7cUQQdkcqD.png)

PP-OCRv6 focuses on a practical OCR need: producing accurate, structured text outputs with small models and flexible deployment options. For a deeper discussion of why specialized OCR models remain useful in the VLM era, see our previous blog: [PP-OCRv5 on Hugging Face: A Specialized Approach to OCR](https://huggingface.co/blog/baidu/ppocrv5).

* * *

## What’s new in PP-OCRv6

PP-OCRv6 introduces architecture, training, and data improvements across detection and recognition. The main design goal is to improve OCR accuracy while keeping model sizes suitable for different deployment settings.

### Three model tiers

PP-OCRv6 provides three model tiers, covering different model sizes and OCR accuracy levels.

| Model                | Model size       | Detection Hmean | Recognition accuracy | Typical application scenarios                                                                      |
| -------------------- | ---------------- | --------------- | -------------------- | -------------------------------------------------------------------------------------------------- |
| **PP-OCRv6\_tiny**   | **1.5M params**  | 80.6%           | 73.5%                | Edge devices, lightweight local OCR, latency-sensitive demos, constrained environments             |
| **PP-OCRv6\_small**  | **7.7M params**  | 84.1%           | 81.3%                | Mobile, desktop, balanced OCR services, multilingual OCR with lower compute cost                   |
| **PP-OCRv6\_medium** | **34.5M params** | **86.2%**       | **83.2%**            | Accuracy-oriented OCR, server-side pipelines, industrial OCR, document ingestion, multilingual OCR |

### PPLCNetV4 backbone

PP-OCRv6 uses **PPLCNetV4** as a unified backbone for text detection and text recognition.

For developers, the main benefit is consistency across the model family. The tiny, small, and medium tiers are not unrelated models; they are part of the same OCR family and share a common architectural direction.

[![Image](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/authcode/?code=MzQ2ODgzNWViOTJhYjMzZDUxNTMyY2RlMzdhMDAwZmNfZWJmZGY5NTljOWJlY2YyODVhYzg0N2NhNTk3MjQwMTRfSUQ6NzY1MjcxNjg0NDE1OTMyMzA5N18xNzgxODE5MjkwOjE3ODE5MDU2OTBfVjM)](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/authcode/?code=MzQ2ODgzNWViOTJhYjMzZDUxNTMyY2RlMzdhMDAwZmNfZWJmZGY5NTljOWJlY2YyODVhYzg0N2NhNTk3MjQwMTRfSUQ6NzY1MjcxNjg0NDE1OTMyMzA5N18xNzgxODE5MjkwOjE3ODE5MDU2OTBfVjM)

### RepLKFPN for text detection

Text detection is the first stage of the OCR pipeline. Detection quality affects the crops sent to the recognizer, and poor crops often lead to poorer recognition.

PP-OCRv6 upgrades the detection module with **RepLKFPN**, a lightweight large-kernel feature pyramid network designed for multi-scale text detection while keeping inference efficient.

This is relevant for real-world OCR inputs, where text may be small, dense, rotated, low-resolution, or embedded in complex backgrounds.

[![ppocrv6_det_pip_ori](https://cdn-uploads.huggingface.co/production/uploads/652b2e9166313ebb6197e706/Unp8cz-s3c4jSMl71qIFf.png)](https://cdn-uploads.huggingface.co/production/uploads/652b2e9166313ebb6197e706/Unp8cz-s3c4jSMl71qIFf.png)

### EncoderWithLightSVTR for recognition

For text recognition, PP-OCRv6 uses **EncoderWithLightSVTR**. It combines local context modeling with global attention to improve recognition quality on challenging text crops.

The recognition improvements are especially relevant for multilingual text, screen text, industrial characters, special symbols, dense text, and noisy image regions.

[![rec](https://cdn-uploads.huggingface.co/production/uploads/652b2e9166313ebb6197e706/YLhR7a1-BdP62o0o_zZl1.png)](https://cdn-uploads.huggingface.co/production/uploads/652b2e9166313ebb6197e706/YLhR7a1-BdP62o0o_zZl1.png)

### Unified multilingual OCR

The medium and small tiers support **50 languages** in one model family, covering Simplified Chinese, Traditional Chinese, English, Japanese, and 46 Latin-script languages.

This helps reduce the need for separate OCR models across common multilingual OCR scenarios.

* * *

## Quick start with PaddleOCR

Install PaddleOCR:

```bash
pip install paddleocr
```

Run OCR with Paddle Infernece(Default backend):

```python
from paddleocr import PaddleOCR

# Model: PP-OCRv6_medium(Default)
# Backend: Paddle Inference(Default)
ocr = PaddleOCR(
    use_doc_orientation_classify=False,
    use_doc_unwarping=False,
    use_textline_orientation=False,
)
result = ocr.predict("https://paddle-model-ecology.bj.bcebos.com/paddlex/imgs/demo_image/general_ocr_002.png")

for res in result:
    res.print()
    res.save_to_img("output")
    res.save_to_json("output")
```

The OCR result can be saved as visualization images and structured JSON output. The structured output can then be used by downstream systems such as document parsing, search, extraction, RAG, analytics, or agent workflows.

* * *

## Available inference backends

PP-OCRv6 can be used with multiple inference backends through PaddleOCR. **PaddleOCR 3.7** provides a unified inference-engine interface, where `engine` selects the underlying runtime and related configuration can be passed through the pipeline or module API.

| **Backend**          | **Description**                                                               |
| -------------------- | ----------------------------------------------------------------------------- |
| **Transformers**     | Hugging Face / PyTorch-oriented inference path for supported PaddleOCR models |
| **ONNX Runtime**     | Portable inference path for ONNX-based deployment environments                |
| **Paddle Inference** | Native Paddle inference format                                                |

For Hugging Face users, PaddleOCR supports running selected OCR and document parsing models with a Transformers backend. This can be enabled with:

```python
engine="transformers"
```

For more details on how the Transformers backend works in PaddleOCR, see:

[PaddleOCR: Running OCR and Document Parsing Tasks with a Transformers Backend](https://huggingface.co/blog/PaddlePaddle/paddleocr-transformers)

Run PP-OCRv6 example with Transformer Backend:

```python

from paddleocr import PaddleOCR

# Model: PP-OCRv6_medium(Default)
# Backend: transformers
ocr = PaddleOCR(
    use_doc_orientation_classify=False,
    use_doc_unwarping=False,
    use_textline_orientation=False,
    engine="transformers",
)
result = ocr.predict("https://paddle-model-ecology.bj.bcebos.com/paddlex/imgs/demo_image/general_ocr_002.png")
```

ONNX variants are also available in the [PP-OCRv6 Collection](https://huggingface.co/collections/PaddlePaddle/pp-ocrv6) for environments that use ONNX Runtime through `engine="onnxruntime"`:

```python
from paddleocr import PaddleOCR

# Model: PP-OCRv6_medium(Default)
# Backend: ONNX Runtime
ocr = PaddleOCR(
    use_doc_orientation_classify=False,
    use_doc_unwarping=False,
    use_textline_orientation=False,
    engine="onnxruntime",
)
result = ocr.predict("https://paddle-model-ecology.bj.bcebos.com/paddlex/imgs/demo_image/general_ocr_002.png")
```

Together, these backend options make PP-OCRv6 available across different runtime environments while keeping the same OCR model family on the Hugging Face Hub.

* * *

## Conclusion

PP-OCRv6 extends PaddleOCR with a lightweight, multilingual OCR model family for real-world text detection and recognition.

The release includes three model tiers from **1.5M to 34.5M parameters**, up to **50-language OCR support**, improved detection and recognition accuracy over PP-OCRv5\_server, and multiple model formats on the Hugging Face Hub, including **safetensors**, **Paddle inference models**, and **ONNX models**.

Together with the hosted Hugging Face Space and the available PaddleOCR inference backends, PP-OCRv6 provides several entry points for evaluation and integration:

- **Online Demo**: [PP-OCRv6 Online Demo](https://huggingface.co/spaces/PaddlePaddle/PP-OCRv6_Online_Demo)

- **Model Collection**: [PP-OCRv6 Collection](https://huggingface.co/collections/PaddlePaddle/pp-ocrv6)

- **Transformers Backend Blog**: [PaddleOCR with Transformers Backend](https://huggingface.co/blog/PaddlePaddle/paddleocr-transformers)

- **PaddleOCR Documentation**: [PP-OCRv6 Documentation](https://www.paddleocr.ai/latest/en/version3.x/algorithm/PP-OCRv6/PP-OCRv6.html)

- **PaddleOCR Official Website**: [https://www.paddleocr.com](https://www.paddleocr.com)

You can evaluate PP-OCRv6 with the online demo, explore the available model assets in the Collection, and use the inference backend that matches your own OCR workflow.
