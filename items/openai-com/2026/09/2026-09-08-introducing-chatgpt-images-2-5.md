---
title: Introducing ChatGPT Images 2.5
link: https://openai.com/index/introducing-chatgpt-images-2-5
source: openai-com
published: 2026-09-08T11:30:00Z
updated: 2026-09-08T11:30:00Z
first_seen: 2026-10-04T12:50:30.804479567Z
labels:
- product
summary: ChatGPT Images 2.5 helps turn your ideas, sketches, and reference photos into more personalized, polished images that better reflect your ideas.
content: extracted
html: 2026-09-08-introducing-chatgpt-images-2-5.html
---

Every week, people create more than 3 billion images across ChatGPT Images and the GPT‑Image models in the API. Today, we’re expanding what you can create with ChatGPT Images 2.5—our new state-of-the-art image model that brings sharper details, more precise editing, and faster generation to your creative workflows.

Images 2.5 produces more natural lighting and richer textures, is better at preserving the subjects in your reference photos, and follows editing instructions more reliably across multiple turns. We’ve also reduced image generation latency by up to 50% compared with Images 2.0, so you can generate images and refine your concepts more quickly.

In ChatGPT, we’re introducing product features that give you more control over the creative process. [Sketch](http://chatgpt.com/sketch) is a new feature that lets you draw directly in ChatGPT as a reference for your final image. Templates make it easier to start creating across some of the most popular image formats, like flyers and product photos. You can also now place comments directly on images for more focused editing, and you can share prompts you’ve used to let others try your ideas with their own photos and details.

Images 2.5 is available to all ChatGPT, ChatGPT Work, and Codex users across desktop, mobile, and web.

For developers, we’re introducing two new models in the API. GPT‑Image‑2.5 Flare brings the same improvements in quality, editing, and speed, and GPT‑Image‑2.5 Sunburst offers an extra level of precision for detailed creative work with longer generation times.

## Image fidelity as you create

The most meaningful images feel grounded in the real-life people, places, and memories they’re based on. Images 2.5 is better at working from reference photos to transform familiar subjects across new settings, visual styles, and compositions. Image subjects look more recognizable, lighting and textures feel more natural, and distinctive features are more likely to carry through.

For teams building with the API, this same fidelity makes reference-led workflows more reliable, so variations can stay anchored to the original source.

## Precision editing

We’ve made it easier for you to make focused edits, so you end up with final images that are more aligned with what you actually had in mind. Images 2.5 is better at editing only what you’ve asked for, while keeping the rest of the details the same—even with more complex subjects and backgrounds.

For developers using the Images API, this gives users the ability to update a single element—such as a product, background, or piece of copy—while preserving the subject, composition, and brand treatment around it.

The following videos are made up of multiple images, highlighting the model’s improved ability to follow precise editing instructions.

## Multi-turn editing consistency

During longer ChatGPT conversations, Images 2.5 follows specific editing instructions more reliably across multiple edits. Earlier changes are more likely to stay consistent, and each new edit builds on the work you’ve already done without degrading image quality over time.

That consistency also matters in production workflows, where developers need to make targeted changes without rebuilding the entire asset.

The following videos are made up of multiple images, highlighting the model’s improved ability to maintain image quality over multiple turns.

## Intelligence and style improvements

Images 2.5 is better at understanding complex visual instructions and translating them into coherent results. Images that include real-world information have more accurate content, and the model can handle more complex layouts including transparent backgrounds. It’s also better at reflecting visual styles, so your images are more aligned with your artistic vision.

For developers and businesses, this makes complex creative briefs more dependable. The model is more likely to retain the requested visual direction, composition, and individual details, instead of drifting as instructions become more specific. That matters when generating a series of on-brand creative assets, UI concepts that preserve a given hierarchy, or presentation visuals that fit a defined structure.

## Use Sketch to draw your idea to life

Sometimes the clearest way to explain an idea is by drawing it.

We’re introducing Sketch, a new feature which lets you draw right in ChatGPT and use it as a visual guide. Now you can quickly sketch the layout of a room, the contour of an outfit you’re concepting, or just a funny doodle, and ChatGPT will turn your rough art into a complete image. Add a description of the style and any other details you want to make sure it reflects your vision.

You don’t need to be a professional artist—this is just another way to get your final image closer to what you have in mind. To [try it yourself](http://chatgpt.com/sketch), just type “@Sketch” in ChatGPT.

## Structure your prompts for better results

At times you may have a clear idea of what you want to make—you just need some help getting started.

We’re introducing templates for some of the most popular creative formats. Instead of starting with a blank canvas, you can choose a template like “Poster” or “Merch” and add details like information to convey, design elements, or styles to personalize your results.

## Pass your best ideas along

Often you end up with an image that’s too good not to share.

Now when you share an image, you can also choose to include the prompt that got you there. Someone else can then run with the same idea, bringing in their own images and details to make a version that feels like their own.

For example, try out [this prompt](https://chatgpt.com/images/prompt/80s-flashback) that’s currently going viral to see what you would’ve looked like in the ’80s!

![A smiling man with curly hair wears a teal, navy, and pink windbreaker and a gold chain in an ’80s-style portrait with neon lights and a boombox.](https://images.ctfassets.net/kftzwdyauwt9/1oUEzmNEwhlktfQMqWLwTO/cde41dc57991caa0d6b22b35b2d8c70c/80s-headshot.png?w=3840&q=90&fm=webp)

## Images 2.5 in the API

Developers are building image generation workflows into products used by creative, marketing, retail, and media teams every day with the Images API.

We’re releasing two new image models in the API:

**GPT‑Image‑2.5 Flare** brings the same improvements in quality, editing, and speed to the API and is the default choice for most applications, delivering higher-quality images than GPT‑Image‑2 at 50% lower latency. It’s well suited to everything from creator and social content to product experiences, visual search, rapid image prototyping, and high-volume generation.

**GPT‑Image‑2.5 Sunburst** is built for premium visual workflows that benefit from tighter control across edits. Use it for creative and editing workflows like production-ready campaign creative or polished product imagery.

Here’s what early customers have to say about the new models:

1 of 4

## Our commitment to safety

We’re building image generation to be useful, creative, and safe. ChatGPT Images 2.5 builds on our existing safeguards, with checks on prompts and images to help prevent harmful outputs. We continue to use C2PA metadata and invisible watermarking to help identify images made with our tools. You can read more about our evaluations and approach in the [system card](https://deploymentsafety.openai.com/chatgpt-images-2-5).

## Pricing and availability

Images 2.5 is rolling out today to ChatGPT, ChatGPT Work, and Codex users across all tiers on desktop, mobile, and web.

GPT‑Image‑2.5 Sunburst and GPT‑Image‑2.5 Flare are available in the API. See [pricing details here.](https://developers.openai.com/api/docs/pricing#image-generation)
