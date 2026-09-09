---
title: 'Desert Ant Labs: local, fast models that run on device'
link: https://desertant.com/blog/introducing-desert-ant-labs/
source: hnrss-org-frontpage
published: 2026-09-09T11:39:46Z
updated: 2026-09-09T11:39:46Z
first_seen: 2026-09-09T16:31:40.482436471Z
authors:
- willwhitedc
summary: 'Article URL: https://desertant.com/blog/introducing-desert-ant-labs/ Comments URL: https://news.ycombinator.com/item?id=49624823 Points: 248 # Comments: 64'
content: extracted
html: 2026-09-09-desert-ant-labs-local-fast-models-that-run-on-device.html
preview:
  file: 2026-09-09-desert-ant-labs-local-fast-models-that-run-on-device.preview-530455eaa7ed.webp
  width: 256
  height: 134
  alt: On-device intelligence for every product
  color: '#edece8'
images:
- source: https://desertant.com/og/blog/introducing-desert-ant-labs.jpg?v=8
  original:
    file: 2026-09-09-desert-ant-labs-local-fast-models-that-run-on-device.image-ea66b1c09f70.jpg
    width: 2400
    height: 1260
  color: '#faf9f4'
---

Today we're launching Desert Ant Labs, a European frontier AI lab building opinionated on-device intelligence. We believe the best path to efficient intelligence starts on-device.

We're building small, specialized models for audio, vision, and text – each model answers in milliseconds, and costs nothing to run, so you can put intelligence in every product interaction, without being limited by token cost or inference speed. Small enough to run on a five-year-old phone, fast enough to use on every frame or keystroke, and better than the API call you're already paying for.

The first 18 models are live today (12 stable and six in beta), accessible via one SDK for Swift, Kotlin, and JavaScript. One model per task, each built to be the fastest way to complete that task on a device:

- [Voz](https://desertant.com/models/voz/): transcribe 10 minutes of audio in two seconds on an iPhone – 4.7x faster than Whisper – with a start and end time on every word.
- [Clear](https://desertant.com/models/clear/): a 9MB model that can turn a five-minute laptop recording into studio quality audio in one second.
- [Redact](https://desertant.com/models/redact/): mask names, addresses, and card numbers, in real time, in 27 languages, so they never reach your servers.
- [Tongue](https://desertant.com/models/tongue/): identify 84 languages from three words, with a 2MB model.

Language ID accuracy, three words in

Tongue · 2MB 0.933

293MB detector 0.887

Tongue names the language from three words, scoring 0.933 at 2MB against 0.887 for a 293MB detector.

And that's just to name a few. You can find full specs and benchmarks for the other fourteen, on [desertant.com/models](https://desertant.com/models/) and [Hugging Face](https://huggingface.co/desert-ant-labs). Every model is free up to 100k monthly active devices. No tokens, no logins.

Personal data caught, by system

Redact · 12MB 88.8

GLiNER-PII · 2.3GB 91.1

Rampart · 14.7MB 61.4

OpenAI filter · 3GB 60.2

Redact catches 88.8% of the personal data in a text, close to the 2.3GB GLiNER-PII, from a 12MB model.

We're building this in Europe, where "on-device" is the sovereign default. The data never leaves your customer's hands, the feature never depends on someone else's cloud, and what's never been uploaded can never be compelled.

## How we got here

For five years we've been building our video app, [Detail](https://detail.co), with an on-device first approach. But when we introduced features like Auto Edit to create short clips, or audio enhancement for podcasts, we had to fall back to cloud APIs. And as the [popularity of Detail](https://www.apple.com/newsroom/2025/12/apple-unveils-the-winners-of-the-2025-app-store-awards/) grew, so did our infrastructure bills.

Every few months I'd hunt for useful on-device models. I'd surf [Hugging Face](https://huggingface.co) for a model that could find filler words or clean up a recording. And, every June, we'd get great new tools to build with but the industry wasn't moving fast enough. The foundation was there: the chips, [Core ML](https://developer.apple.com/documentation/coreml/), the research. What was missing was everything between that foundation and actually implementing a feature in your app: a model you could drop in and ship with a few lines of code.

So, we trained the models ourselves. It turns out training a model is a product design challenge, and product is what we know. We designed models and local inference that beat cloud services on speed, quality, and cost, and outperform other local and cloud models on the task itself, at a fraction of their size.

We replaced Dolby for better, faster audio enhancement with [Clear](https://desertant.com/models/clear/), and made our on-device transcriptions 5x faster with [Voz](https://desertant.com/models/voz/). We also replaced Claude Sonnet with [Clips](https://desertant.com/models/clips/), our 284MB model that turns a 10-minute video into a dozen clips in 5 seconds – 10x faster and using [470x less energy](https://desertant.com/about/) than Sonnet, with the same quality.

Clear audio enhancement speed, 5 minutes of audio

iPhone 16 Pro 302x

MacBook Pro (M5) 345x

Clear enhances, masters, and re-encodes a clip on the device, best of three, from a 9MB model. 302x realtime on a phone.

Transcription speed, 30 minutes of audio

Voz 319x

Apple SpeechAnalyzer 78x

Whisper large-v3-turbo 50x

Realtime factor over 30 continuous minutes on an M3 Ultra. Voz reaches 298x on an iPhone 17 Pro.

Detail 6, which will launch with iOS 27, replaces all of our cloud APIs with our own models, running entirely on the device.

We've all spent the past few years building with LLMs as if they were just another API. And, amid the hype around generalist frontier brains, we almost forgot they're not the only option.

Every developer I talk to has a wishlist of on-device models they'd build if cost wasn't a factor, or a feature they're bleeding tokens on that they'd happily swap for a local model. A call that runs the same way a hundred thousand times a day: cleaning a recording, tagging a photo, pulling a date out of a sentence, catching a name before the text hits your servers. None of these needs a frontier model.

[NVIDIA's own researchers](https://arxiv.org/abs/2506.02153) pulled apart three agent systems and estimated that 40 to 70% of their calls to a large model could go to a small, specialized one instead.

## The compute is already paid for

The industry will spend about [$450 billion](https://introl.com/blog/hyperscaler-capex-600b-2026-ai-infrastructure-debt-january-2026) on data centers this year. Meanwhile, the world ships [more than a billion](https://my.idc.com/getdoc.jsp?containerId=prUS53965725) phones, tablets, and laptops with increasingly capable chips, perfectly suited to these kinds of tasks. There's more compute available in people's hands than in every AI data center on earth.

We have an unfair advantage with free inference. No per-call cost, so a feature runs on every message instead of the ones you can afford to check. No round-trip, and your customer's data never leaves the device. When inference costs nothing, the way we build products changes entirely.

## Little brains in every product

To build with local models, the developer experience has to get a lot better. You need models you can use commercially, that beat the alternatives on your task in speed and quality, that you can drop into your app with a few lines of code, and are easy to discover.

Think of the first hundred models as the [cerebellum](https://www.ncbi.nlm.nih.gov/books/NBK538167/), the little brain. The little brain handles the always-on work – balance, timing, the skills you never think about, so the rest of the brain is free to think. That's what we're building first: fast, specialized models for the work that runs all day, on the device, for free.

Then comes the cortex, the layer that decides which model answers. A small local model first, a bigger one when the job requires it, and the cloud only when the work has to leave the device. As open research advances and device silicon becomes more capable, the local models grow, and we'll train larger ones ourselves. Frontier intelligence, built from the small end up.

Cloud labs ship neutral models because per-token pricing needs a neutral model. Every Desert Ant model ships with a default we choose, and the levers you need to change that default. We optimize the model and the runtime together: on an iPhone, Clear and Voz run on the Neural Engine, and in the browser, Clear's same weights run through WebAssembly.

## The SDK

Ready to get started? You can implement Desert Ant models in your app with our native [Swift](https://desertant.com/swift/), [Kotlin](https://desertant.com/kotlin/), and [JavaScript](https://desertant.com/js/) SDK, available on [GitHub](https://github.com/Desert-Ant-Labs).

Our [docs](https://desertant.com/docs/) are written for developers and agents and you can try the models on your Mac with the [CLI](https://github.com/Desert-Ant-Labs/desert-ant-cli), or in your browser on [Hugging Face](https://huggingface.co/desert-ant-labs).

Building something cool with our models, or want to build them with us? [Get in touch](https://desertant.com/contact/).
