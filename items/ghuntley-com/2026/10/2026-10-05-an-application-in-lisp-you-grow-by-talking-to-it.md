---
title: an application in lisp you grow by talking to it
link: https://ghuntley.com/lisp/
source: ghuntley-com
published: 2026-10-05T12:07:33Z
updated: 2026-10-05T12:07:33Z
first_seen: 2026-10-05T18:49:38.624325496Z
authors:
- Geoffrey Huntley
labels:
- ai
summary: It's kind of strange seeing all these discussions about software factories... and the like. It's also strange to see conversations about programming languages or claims that $language is best, as if that will remain true going forward. It's very clear, however, that programming
content: extracted
html: 2026-10-05-an-application-in-lisp-you-grow-by-talking-to-it.html
remote_preview:
  url: https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/2026/10/Low-angle-traditional-tattoo-art-print-symbolizing-an-application-nurtured-through-conversation--showing-a-smartphone-and-speech-bubble-rising-into-ornate-vines--vibrant-retro-colors--complex-vintage-linework--white-background--wet-reflecti.jpg
  alt: an application in lisp you grow by talking to it
---

It's kind of strange seeing all these discussions about software factories... and the like. It's also strange to see conversations about programming languages or claims that $language is best, as if that will remain true going forward. It's very clear, however, that programming languages will converge toward something, but that 'something' is undefined for now.

What I haven't seen is people really deeply understanding the power of the new substrate that we have. People are still too fixated on what they have now and how systems have been built to rethink fundamentally how much things can change.

> To me, a software factory isn't just about process automation; it isn't about automating everything you've got as it is now. It's about using this substrate so you can develop your product while it runs whilst in the product itself.
>
> Everyone (regardless of their discipline or background) in the company should be able to develop the product in the product without having to go to some external vendor supplied tool.
>
> The only tool that exists is your product itself. The product should build the product from the product.

[a sneak preview behind an embedded software factory. I suspect rapid application dev is back](https://ghuntley.com/rad)

Hey folks, I’m currently over in SF. For the last couple of weeks, I’ve been cryptically tweeting about a hidden mode within something I’ve been building on called Latent Patterns (see below), and over the last couple of days, I’ve started opening up and showing people

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/icon/A-black-and-white--low-angle-digital-illustration-in-a-symbolic-traditional-tattoo-art-style.--A-bald--light-skinned-man-with-a-bushy-beard--prominent-eyebrows--and-a-friendly-expression-wears-denim-overalls--2--6f81e89e-3a32-49dc-8585-cf0ed1757e08.jpg)Geoffrey Huntley Geoffrey Huntley

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/thumbnail/Symbolic-traditional-tattoo-art-print--lush-garden-with-vibrant-roses-and-a-computer--detailed-painting--strong-hard-light-surreal-infrared-tones--vibrant-colors--retro-flair--complex-ornamental--white-background--infrared-spectrum--otherwo-a318d1d0-6e0c-431e-b68a-439b7334e9ea.jpg)

earlier explorations into recursive product development at the begining on the year

In a future blog post, I'll go further into what it means for the product to develop the product, in the product, but for now I have one simple thing for you to imagine:

> Why is software built the way it is now, rather than grown through iterative use of LLMs? What if you could develop your application just by chatting with it? Live and interactive with no compilation steps.

To demonstrate this, I built Jiti, a small kernel for growing a running Lisp application through conversation with an LLM. You ask for a capability, the model writes Lisp, and the application permanently acquires that capability until you ask for that capability to be removed.

0:00

/0:41

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/media/2026/10/CleanShot-2026-10-05-at-7.54.45-AM_thumb.jpg)

The kernel supplies the machinery to inspect, change, execute, and recover a managed Lisp world. Application behavior comes from whatever you decide to add to the application by prompting for those outcomes. With Jiti, you can start from a clean slate or modify an existing application to add more behavior just by prompting it.

![A request to add a capability goes to an external language model with instructions, registered tool descriptions and observed application state. The model requests a tool call. The controller validates and routes it to the persistent worker, which evaluates Lisp in the running application. Actual values, checks or a live pause return through the controller to inform the next model step. Accepted definitions and managed data remain available for later requests. Ordinary Lisp function calls do not require inference.](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/2026/10/01-conversation-and-lisp.png)\
The language model uses registered tools to inspect and change a running Lisp application. Actual worker results inform its next step; accepted functions and managed data remain available for later requests. Once defined, functions execute as ordinary Lisp. [SVG source](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/2026/10/01-conversation-and-lisp.svg?ref=ghuntley.com).

The source code is available on GitHub, and I encourage you to run it and play around with it. It is very generic, in the sense that it can do literally anything you want. All you have to do is ask, and it will program that capability into the application.

[GitHub - ghuntley/jiti: Live Common Lisp image repair with OpenAI tools, persistent revisions, and a conversational CLI](https://github.com/ghuntley/jiti?ref=ghuntley.com)

Live Common Lisp image repair with OpenAI tools, persistent revisions, and a conversational CLI - ghuntley/jiti

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/icon/favicon-c2c75955-7fde-489b-8554-291dca12326a.png)GitHub ghuntley

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/thumbnail/jiti-0295d346-02de-4f46-a6b0-10ef4bacbc1f.png)

How it works is relatively simple. At a high level, an OpenAI model receives your request, instructions for operating the application, tool definitions, and observations of its current state. The kernel can ask to inspect the available functions, read a definition, propose new source, or execute an expression.

Once a definition is accepted, it is an ordinary Lisp function. Calling it does not inherently require another inference request. You can call it from Lisp, from another application function, or through the chat interface.

![](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/2026/10/Screenshot-2026-10-05-at-7.46.58---AM.png)

The OpenAI model is used to extend a program, while the running Lisp world holds the resulting functionality.

The agent tool interface has two useful intentions.

- `develop_form` adds, redefines or removes functionality.
- `execute_form` calls functionality that exists, including combinations of existing functions.

Both use the same evaluator and transaction machinery. The distinction helps the model choose whether the user is asking to change the application or simply use it.

> why write code and suffer compilation loop pain when you can prompt an LLM to program LISP for outcomes? [pic.twitter.com/CYA4apelcl](https://t.co/CYA4apelcl?ref=ghuntley.com)
>
> — geoff (@GeoffreyHuntley) [October 5, 2026](https://x.com/GeoffreyHuntley/status/2107031936375955502?ref_src=twsrc%5Etfw&ref=ghuntley.com)

Suppose I've already asked the application to add `uppercase-string` and `reverse-string`. I can then prompt the kernel to uppercase some text and reverse the result. The composition is just Lisp:

```lisp
(reverse-string (uppercase-string "Hello"))
```

If I want that combination as a reusable capability, I can prompt it to save that as a function.

```lisp
(defun shout-backwards (text)
  (reverse-string (uppercase-string text)))
```

That function joins the catalog with its arguments and source, ready for inspection and later use. A future request can discover it and build on it. The application accumulates an executable vocabulary through use. Lisp already gives us the composition rules; the kernel keeps the evolving definitions available and their managed effects accountable.

![The controller validates tool arguments and sends actions one at a time to a persistent worker, receiving observations and results in return. The worker owns evaluation, the live stack and active restarts. Inside that worker, the world adapter provides managed code and data, the function catalogue, checkpoint and restore, and durable export and import. Caller-supplied safety checks and goals are assessed at safe points. Safety determines whether a candidate may be kept; goals determine completion. Accepted managed changes publish durable revisions. The reference adapter covers named functions and readable table data.](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/2026/10/02-kernel-ownership-1.png)\
The controller routes actions; the persistent worker owns evaluation and live restarts. Its world adapter defines the managed resources, catalogue and recovery hooks. Caller safety checks govern acceptance while goals describe completion. Accepted managed changes become durable revisions. [SVG source](https://storage.ghost.io/c/06/e5/06e5d12b-0328-469d-a0d0-3a0f0ba34c46/content/images/2026/10/02-kernel-ownership-1.svg?ref=ghuntley.com).

Lisp deserves the credit for the interactive programming machinery. Definitions, inspection, conditions, and restarts have been there for decades. It's kind of cool, huh?

To me, the idea that an agent writes source code and then there's a costly compilation phase involving CI/CD is now truly undefined now that we have AI. The only limiting factor will really be people's curiosity about what they can do with this new substrate...

ps. socials

> 🗞️ an application in lisp you grow by talking to it
>
> Why is software built the way it is now, rather than grown through iterative use of LLMs? What if you could develop your application just by chatting with it? Live and interactive, with no compilation steps.…
>
> — geoff (@GeoffreyHuntley) [October 5, 2026](https://x.com/GeoffreyHuntley/status/2107080798209794317?ref_src=twsrc%5Etfw&ref=ghuntley.com)
