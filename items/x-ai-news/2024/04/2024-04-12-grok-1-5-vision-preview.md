---
title: Grok-1.5 Vision Preview
link: https://x.ai/news/grok-1.5v
source: x-ai-news
published: 2024-04-12T00:00:00Z
first_seen: 2026-09-08T10:17:10.166315Z
summary: Connecting the digital and physical worlds with our first multimodal model.
content: extracted
html: 2024-04-12-grok-1-5-vision-preview.html
preview:
  file: 2024-04-12-grok-1-5-vision-preview.preview-c59873755ddf.webp
  width: 256
  height: 144
  color: '#d0280c'
images:
- source: https://media.x.ai/v1/website/grok-1-75591dd5.webp
  original:
    file: 2024-04-12-grok-1-5-vision-preview.image-8fd8bf2f4f10.webp
    width: 1486
    height: 836
  variants:
  - file: 2024-04-12-grok-1-5-vision-preview.image-d76405f91845.webp
    width: 48
    height: 27
  - file: 2024-04-12-grok-1-5-vision-preview.image-505e03b727f1.webp
    width: 320
    height: 180
  - file: 2024-04-12-grok-1-5-vision-preview.image-893f820fdd69.webp
    width: 640
    height: 360
  color: '#f91807'
---

**Introducing Grok-1.5V, our first-generation multimodal model. In addition to its strong text capabilities, Grok can now process a wide variety of visual information, including documents, diagrams, charts, screenshots, and photographs. Grok-1.5V will be available soon to our early testers and existing Grok users.**

## [Capabilities](https://x.ai/news/grok-1.5v#capabilities)

Grok-1.5V is competitive with existing frontier multimodal models in a number of domains, ranging from multi-disciplinary reasoning to understanding documents, science diagrams, charts, screenshots, and photographs. We are particularly excited about Grok’s capabilities in understanding our physical world. Grok outperforms its peers in our new RealWorldQA benchmark that measures real-world spatial understanding. For all datasets below, we evaluate Grok in a zero-shot setting without chain-of-thought prompting.

| Benchmark | **Grok-1.5V** | GPT-4V    | Claude 3 Sonnet | Claude 3 Opus | Gemini Pro 1.5 |
| --------- | ------------- | --------- | --------------- | ------------- | -------------- |
| 53.6%     | 56.8%         | 53.1%     | **59.4%**       | 58.5%         |                |
| **52.8%** | 49.9%         | 47.9%     | 50.5%           | 52.1%         |                |
| 88.3%     | 78.2%         | **88.7%** | 88.1%           | 80.3%         |                |
| **78.1%** | 78.0%         | -         | -               | 73.5%         |                |
| 76.1%     | 78.5%         | 81.1%     | 80.8%           | **81.3%**     |                |
| 85.6%     | 88.4%         | **89.5%** | 89.3%           | 86.5%         |                |
| **68.7%** | 61.4%         | 51.9%     | 49.8%           | 67.5%         |                |

Example

Writing code from a diagram

![A diagram describing a guessing game with a flowchart of the game logic and user interactions](https://x.ai/news/f=auto/v1/website/grok1.5v_guessing-4609fc8f.webp)

##### User

Can you translate this into Python code?

##### Grok

Certainly! The flowchart you’ve provided describes a simple guessing game where the computer generates a random number, and the user has to guess it. Here’s the Python code that represents the logic in the flowchart:

python

This code will run the guessing game as described in the flowchart. The user will be prompted to enter their guess until they guess the correct number.

## [Real-World Understanding](https://x.ai/news/grok-1.5v#real-world-understanding)

In order to develop useful real-world AI assistants, it is crucial to advance a model's understanding of the physical world. Towards this goal, we are introducing a new benchmark, RealWorldQA. This benchmark is designed to evaluate basic real-world spatial understanding capabilities of multimodal models. While many of the examples in the current benchmark are relatively easy for humans, they often pose a challenge for frontier models.

![A picture of various household objects on a table.](https://x.ai/news/f=auto/v1/website/realworldqa_sample_1-a008c412.webp)

Which object is larger the pizza cutter or the scissors? A. The pizza cutter is larger. B. The scissors is larger. **C. They are about the same size.**

![A picture of a dark street scene with a sign that says \"left lane must turn left\"](https://x.ai/news/f=auto/v1/website/realworldqa_sample_2-75f328e9.webp)

Where can we go from the current lane? **A. Turn left.** B. Go straight. C. Turn left and go straight. D. Turn right.

![A picture of a street with several cars parked on the left](https://x.ai/news/f=auto/v1/website/realworldqa_sample_3-b5153c1b.webp)

Given this front camera view from our sedan, do we have enough space to drive around the gray car in front of us? **A. Yes.** B. No.

![A picture of a toy next to a cell phone showing a compass.](https://x.ai/news/f=auto/v1/website/realworldqa_sample_4-b3a6c775.webp)

Given the picture, in which cardinal direction is the dinosaur facing? A. North. B. South. **C. East.** D. West.

The initial release of the RealWorldQA consists of over 700 images, with a question and easily verifiable answer for each image. The dataset consists of anonymized images taken from vehicles, in addition to other real-world images. We are excited to release RealWorldQA to the community, and we intend to expand it as our multimodal models improve. RealWorldQA is released under [CC BY-ND 4.0](https://creativecommons.org/licenses/by-nd/4.0/?ref=chooser-v1). Click [here (677MB)](https://data.x.ai/realworldqa.zip) to download the dataset.

## [Into the Future](https://x.ai/news/grok-1.5v#into-the-future)

Advancing both our multimodal understanding and generation capabilities are important steps in building beneficial AGI that can understand the universe. In the coming months, we anticipate to make significant improvements in both capabilities, across various modalities such as images, audio, and video.

If you want to be a part of this journey, we are [hiring](https://x.ai/careers).
