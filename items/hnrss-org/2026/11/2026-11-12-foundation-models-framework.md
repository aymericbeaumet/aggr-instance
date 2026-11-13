---
title: Foundation Models framework
link: https://developer.apple.com/foundation-models-framework/
source: hnrss-org
published: 2026-11-12T21:13:26Z
updated: 2026-11-12T21:13:26Z
first_seen: 2026-11-13T07:09:29.179831632Z
authors:
- kermatt
content: extracted
html: 2026-11-12-foundation-models-framework.html
remote_preview:
  url: https://developer.apple.com/news/images/og/foundation-models-framework-og.png
---

![](https://developer.apple.com/assets/elements/icons/foundationmodel/foundationmodel-128x128_2x.png)

The Foundation Models framework is the best way to use any large language model in your app. Whether you’re generating a quick summary or creating dynamic agents, the framework has what you need.

## Overview

The Foundation Models framework is a native Swift API for integrating any large language model into your app — including direct access to the Apple Foundation Models on-device and in Private Cloud Compute. Powerful capabilities like guided generation, tool calling, image understanding, and Dynamic Profiles are built in, so you have what you need to build unique experiences.

## Image understanding

Multimodal prompts let you pass images alongside text so your app can reason about visual content. This opens up new categories of experiences you can build with image understanding — it's as simple as attaching an image to your prompt. The Vision framework is also integrated directly, giving your model purpose-built tools like OCR for precise text extraction and barcode readers for quick code scanning — all on-device.

## Model support

The Foundation Models framework gives you direct access to the on-device foundation model at the core of Apple Intelligence so you can build experiences that are smart, private, and work without internet connectivity.

On-device models are highly useful for many tasks, yet sometimes you need a larger model for a more complex workflow. The Foundation Models framework now extends to server models — like Claude, Gemini, and more — all through the same Swift API, with full support for features like tool calling and guided generation. Any model provider can create a Swift package conforming to the [Language Model protocol](https://developer.apple.com/documentation/foundationmodels/languagemodel), a fundamental component of the framework that allows you to pick the right language model for your app.

### Private Cloud Compute

For those just getting started, developers who are enrolled in the App Store Small Business Program and have fewer than two million first-time App Store downloads can access the Apple Foundation Models running on Private Cloud Compute with no cloud API cost.\* This delivers frontier-level intelligence with unparalleled privacy protections, ensuring that your users' data is not stored or accessible to Apple or anyone else.

[Learn more about Private Cloud Compute](https://developer.apple.com/private-cloud-compute/)

## Dynamic Profiles

Dynamic Profiles are new declarative APIs for continuously updating your session's model, tools, and instructions on the fly. Rather than creating a session with a fixed configuration, you define profiles — each with its own model, temperature, reasoning level, and toolset — and swap between them within a single continuous session. The session transcript is shared across every profile switch, so your app gets more contextual intelligence with less prompting. Dynamic Profiles are designed to be adaptable building blocks: use them to build AI agents, skills, or any other high-level abstraction your app needs.

## ![](https://developer.apple.com/assets/elements/icons/evaluations/evaluations-96x96_2x.png)Evaluations framework

The Evaluations framework gives you the ability to test your prompts and validate that your intelligence-powered features work correctly across dynamic conditions, going beyond what unit tests alone can catch. Whether you're iterating on instructions, switching models, or tuning a Dynamic Profile, Evaluations gives you confidence that your app's behavior holds up before you ship.

## ![](https://developer.apple.com/assets/elements/icons/terminal/terminal-96x96_2x.png)CLI

The new `fm` command line tool in macOS 27 lets you streamline scripting, automate model workflows, and accelerate your development without writing a line of app code. You can prototype quickly, giving you a fast path from idea to implementation.

## ![](https://developer.apple.com/foundation-models-framework/images/globe-badge-sparkles.svg)Open source

To help you get started with adopting emerging AI patterns, the Foundation Models framework utilities package adds experimental abstractions on top of the framework — including custom skills, context management helpers, and a Chat Completions client that connects to a hosted model of your choice.

The Foundation Models SDK for Python extends the framework into the Python ecosystem, letting you integrate with popular tooling and evaluation packages in Python.

## Requirements

The Acceptable Use Requirements for the Foundation Models framework outline prohibited uses of Apple Foundation Models. Additionally, pursuant to Article 53(1)(b) of the AI Act, we make available information and documentation to providers of AI systems who intend to integrate AFM 3 Cloud into their AI systems.
