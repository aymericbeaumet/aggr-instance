---
title: 'Qwen-Image-2.1: Compact, Efficient, and Unified Image Creation'
link: https://qwen.ai/blog?id=qwen-image-2.1
source: qwen-ai
published: 2026-09-20T12:00:00Z
first_seen: 2026-10-19T01:26:53.998049939Z
authors:
- QwenTeam
labels:
- open-source
content: feed
html: 2026-09-20-qwen-image-2-1-compact-efficient-and-unified-image-creation.html
remote_preview:
  url: https://img.alicdn.com/imgextra/i1/O1CN01XLHYPGwKvuI29R4a_!!6000000005961-2-tps-1060-636.png
---

![Qwen-Image-2.1 banner](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/banner_en.png)

[GITHUB](https://github.com/QwenLM/Qwen-Image-2.1) [HUGGING FACE](https://huggingface.co/Qwen/Qwen-Image-2.1) [MODELSCOPE](https://www.modelscope.cn/models/Qwen/Qwen-Image-2.1)

We are excited to open-source **Qwen-Image-2.1**, an image model in the Qwen family that balances generation quality, inference efficiency, and cost. Qwen-Image-2.1 unifies text-to-image generation and image editing in a single model, with just **7B parameters in its visual generation component** and native support for generating and editing transparent images.

This update introduces four key improvements:

- **Compact and efficient**: A lightweight architecture and inference optimizations balance image quality and computational cost.
- **Native transparency, unified creation and editing**: Generate regular or transparent images from prompts, edit transparent layers, and extract subjects from photos.
- **Versatile editing**: Use up to **10 reference images**, specify local edits, preserve people and products, and handle a wide range of tasks.
- **Realistic textures and refined aesthetics**: Improved typography, portrait lighting, and fine details make generated images more visually compelling.

## Compact and Efficient

Qwen-Image-2.1 uses a lightweight architecture. Its visual generation component contains **32 Single-Stream DiT layers and 7B parameters**. Despite its compact size, the model delivers strong image generation quality. The Qwen-Image-Bench comparison below shows its performance alongside other open-source and closed-source models.

![Qwen-Image-Bench evaluation comparison](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-01.png)

Qwen-Image-Bench evaluation comparison

Inference efficiency is another focus, particularly when editing with multiple input images. This optimization comes from a **mixed-granularity attention architecture**. Text, including the system prefix and editing instructions, uses a token-level causal mask, while image generation uses a chunk-level mask. **KV cache reuse** allows input images and editing instructions to serve as static context, computed and cached in the first step to improve inference efficiency and reduce memory usage.

![Qwen-Image-2.1 mixed-granularity attention architecture](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-03.png)

Qwen-Image-2.1 mixed-granularity attention architecture

## Native Transparency, Unified Creation and Editing

Native transparency is a major addition in this release. In December 2025, we introduced Qwen-Image-Layered as a dedicated model supporting transparent image generation. Qwen-Image-2.1 now integrates this capability into a unified model, using the prompt to determine whether to output a regular image or an image with a transparency channel.

Here are examples of transparent images generated directly from text:

![Transparent images generated from text (1)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-04.png) ![Transparent images generated from text (2)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-05.png) ![Transparent images generated from text (3)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-06.png)

Beyond individual subjects, the model can generate more complex transparent images composed of multiple elements, opening up further possibilities for design and asset creation.

![Transparent compositions with multiple elements (1)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-07.png) ![Transparent compositions with multiple elements (2)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-08.png?v=cac12076c2cd)

By unifying generation and editing, Qwen-Image-2.1 also supports editing transparent images directly. For example, it can change a subject’s expression while retaining the transparent background. The input is on the left and the edited result on the right.

![Expression editing on a transparent image: input and result (1)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-09.png?v=ee4de1b5241d) ![Expression editing on a transparent image: input and result (2)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-10.png?v=611bf03abb39)

Text within a transparent layer can also be edited. In the following example, “BLOOM” is replaced with “Qwen-Image”.

![Transparent text editing: replacing BLOOM with Qwen-Image (1)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-11.png?v=1f2803db504a) ![Transparent text editing: replacing BLOOM with Qwen-Image (2)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-12.png?v=9d0fa2b0e13a)

This capability extends to real photographs. Given an RGB image, the model can extract the desired subject as an RGBA layer with transparency, making it easier to reuse elements in subsequent design and composition work.

![Extracting a transparent RGBA layer from an RGB photograph (1)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-13.png) ![Extracting a transparent RGBA layer from an RGB photograph (2)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-14.png)

## Versatile Editing

Qwen-Image-2.1 improves editing in four areas: multiple reference images, local editing, fidelity preservation, and task coverage.

### Multiple References: Up to 10 Input Images

Qwen-Image-2.1 supports **up to 10 reference images**, combining multiple subjects and assets into a coherent composition. In the group portrait example below, six individual portraits on the left are brought together in one photograph.

![A group photograph generated from six portrait references](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-15.png)

A group photograph generated from six portrait references

For virtual try-on, five inputs—a model, clothing, shoes, a bag, and a hat—can be combined into a complete outfit.

![A complete outfit generated from five reference images](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-16.png)

A complete outfit generated from five reference images

For interior design, the model can use 10 images of furnishings to generate a complete room arrangement.

![An interior generated from ten furnishing references](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-17.png)

An interior generated from ten furnishing references

### Local Editing: Flexible Region Selection

Qwen-Image-2.1 supports circles, painted annotations, and separate masks to specify where an edit should take place.

The first example uses circles of different colors to identify three regions at once: “Remove the metal watch in the blue circle, change the hair in the red circle to black, and replace the area in the green circle with gray short-sleeved linen pajamas.” The annotated input is on the left, and the result is on the right.

![Circle-guided editing of a watch, hair, and clothing (1)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-18.png) ![Circle-guided editing of a watch, hair, and clothing (2)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-19.png)

Painted annotations provide another way to identify a region. In this example, the model adds a diver to the area marked in white on the right side of the input image.

![Paint-guided editing: adding a diver to the marked region (1)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-20.png) ![Paint-guided editing: adding a diver to the marked region (2)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-21.png)

Circles and painted annotations obscure some of the original content. To preserve the complete input, Qwen-Image-2.1 also accepts **the original image and a separate mask as two inputs**. The following example asks the model to generate a cowboy on horseback, using these two images:

![Local editing inputs: original image and separate mask (1)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-22.png) ![Local editing inputs: original image and separate mask (2)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-23.png)

The model edits the region specified by the mask, producing the following result:

![Mask-guided editing result: a cowboy on horseback](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-24.png)

Mask-guided editing result: a cowboy on horseback

Local editing can also support sequential creation. By making successive changes while preserving the rest of the scene, edited images can be assembled into simple animations, as illustrated by the capybara Calabash Brothers example below.

[Capybara Calabash Brothers: animation through successive local edits](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/videos/hulu_final_48f_6fps.mp4)

### Fidelity: Preserving People and Products

Fidelity improvements focus on **portrait identity** and **product consistency**. For portraits, the model better preserves facial features so that a person’s identity remains more consistent across edits. Each pair below shows the input on the left and the edited result on the right.

![Portrait fidelity example one: input and result (1)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-25.png) ![Portrait fidelity example one: input and result (2)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-26.png)

![Portrait fidelity example two: input and result (1)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-27.png) ![Portrait fidelity example two: input and result (2)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-28.png)

![Portrait fidelity example three: input and result (1)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-29.png) ![Portrait fidelity example three: input and result (2)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-30.png)

For product editing, the model aims to preserve text, textures, and shape, keeping the product’s defining features consistent in a new image.

![Product fidelity example one: input and result (1)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-31.png) ![Product fidelity example one: input and result (2)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-32.png)

![Product fidelity example two: input and result (1)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-33.png) ![Product fidelity example two: input and result (2)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-34.png)

![Product fidelity example three: input and result (1)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-35.png) ![Product fidelity example three: input and result (2)](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-36.png)

### Task Coverage: Panoramas, Infographics, and Storyboards

Qwen-Image-2.1 supports a broad range of editing tasks, including panorama, infographic, and storyboard generation, balancing capabilities across different applications.

For example, starting from this selfie:

![Input selfie for panorama generation](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-37.png)

Input selfie for panorama generation

The model generates the following panorama:

![Panorama generated from the selfie](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-38.png)

Panorama generated from the selfie

The panorama can then be explored from different viewing directions in a visualization tool.

[Panorama visualization demonstration](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/videos/pano_video2_10s_noaudio.mp4)

For infographic generation, a model photograph can be expanded into a detailed, information-rich composition.

![Input model photograph for infographic generation](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-39.png)

Input model photograph for infographic generation

![Complex infographic generated from the model photograph](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-40.png)

Complex infographic generated from the model photograph

The model can also turn a three-view character reference into a complete storyboard, providing material for storytelling and visual narratives.

![Three-view character reference for storyboard generation](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-41.png)

Three-view character reference for storyboard generation

![Complete storyboard generated from the character reference](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-42.png)

Complete storyboard generated from the character reference

## Realistic Textures and Refined Aesthetics

Qwen-Image-2.1 further improves visual quality, with particular attention to typography and portraits. Text rendering considers not only the content itself, but also type styles, layout, and their relationship to the overall composition. The following examples showcase text rendering across different settings.

![Text rendering example one](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-43.png)

Text rendering example one

![Text rendering example two](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-44.png)

Text rendering example two

![Text rendering example three](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-45.png)

Text rendering example three

![Text rendering example four](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-46.png)

Text rendering example four

For portraits, improved lighting and fine details give generated images a more realistic appearance.

![Portrait lighting and detail example one](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-47.png)

Portrait lighting and detail example one

![Portrait lighting and detail example two](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-Image/image2.1/images/example-48.png)

Portrait lighting and detail example two

## Conclusion

With a compact 7B visual generation component, Qwen-Image-2.1 brings image generation, transparency, and a wide range of editing capabilities together in one model. Faster inference, support for up to 10 reference images, flexible local editing, and improved fidelity for people and products make it a practical tool for design, content creation, e-commerce, and visual storytelling.

We hope you enjoy creating with Qwen-Image-2.1!
