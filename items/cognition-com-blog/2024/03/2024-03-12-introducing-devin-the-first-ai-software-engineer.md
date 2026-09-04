---
title: Introducing Devin, the first AI software engineer
link: https://cognition.com/blog/introducing-devin
source: cognition-com-blog
published: 2024-03-12T00:00:00Z
first_seen: 2026-09-04T12:14:41.802665207Z
content: extracted
html: 2024-03-12-introducing-devin-the-first-ai-software-engineer.html
---

Devin is a tireless, skilled teammate, equally ready to build alongside you or independently complete tasks for you to review.

With Devin, engineers can focus on more interesting problems and engineering teams can strive for more ambitious goals.

## Devin's Capabilities

With our advances in long-term reasoning and planning, Devin can plan and execute complex engineering tasks requiring thousands of decisions. Devin can recall relevant context at every step, learn over time, and fix mistakes.

We've also equipped Devin with common developer tools including the shell, code editor, and browser within a sandboxed compute environment—everything a human would need to do their work.

Finally, we've given Devin the ability to actively collaborate with the user. Devin reports on its progress in real time, accepts feedback, and works together with you through design choices as needed.‍Here's a sample of what Devin can do:

### Devin can learn how to use unfamiliar technologies.

After reading a blog post, Devin runs ControlNet on Modal to produce images with concealed messages for Sara.

### Devin can build and deploy apps end to end

Devin makes an interactive website which simulates the Game of Life! It incrementally adds features requested by the user and then deploys the app to Netlify.

### Devin can autonomously find and fix bugs in codebases

Devin helps Andrew maintain and debug his open source competitive programming book.

### Devin can train and fine tune its own AI models

Devin sets up fine tuning for a large language model given only a link to a research repository on GitHub.

### Devin can address bugs and feature requests in open source repositories

Given just a link to a GitHub issue, Devin does all the setup and context gathering that is needed.

### Devin can contribute to mature production repositories.

This example is part of the SWE-bench benchmark. Devin solves a bug with logarithm calculations in the sympy Python algebra system. Devin sets up the code environment, reproduces the bug, and codes and tests the fix on its own.

### We even tried giving Devin real jobs on Upwork and it could do those too!

Here, Devin writes and debugs code to run a computer vision model. Devin samples the resulting data and compiles a report at the end.

## Devin's Performance

We evaluated Devin on [SWE-bench](https://swebench.com), a challenging benchmark that asks agents to resolve real-world GitHub issues found in open source projects like Django and scikit-learn.

Devin correctly resolves 13.86%\* of the issues end-to-end, far exceeding the previous state-of-the-art of 1.96%. Even when given the exact files to edit, the best previous models can only resolve 4.80% of issues.

![Introducing Devin, the first AI software engineer](https://cdn.sanity.io/images/2mc9cv2v/production/5dd1cd4fd86149ed2cf4d8ab605f99707040615e-1600x858.png?w=1600&fit=max)

Devin was evaluated on a random 25% subset of the dataset. Devin was unassisted, whereas all other models were assisted (meaning the model was told exactly which files need to be edited).

We plan to publish a more detailed technical report soon—stay tuned for more details.

## About Cognition

We are an applied AI lab focused on reasoning.‍We’re building AI teammates with capabilities far beyond today’s existing AI tools. By solving reasoning, we can unlock new possibilities in a wide range of disciplines—code is just the beginning. We want to help people around the world turn their ideas into reality.

We are well funded, including a $21 million Series A led by Founders Fund. And we’re grateful for the support of industry leaders including Patrick and John Collison, Elad Gil, Sarah Guo, Chris Re, Eric Glyman, Karim Atiyeh, Erik Bernhardsson, Tony Xu, Fred Ehrsam and so many more.

## Hire Devin

Devin is currently in early access as we ramp up capacity. To start using Devin for engineering work, [join the waitlist](https://cognition.com/get-started) or get in touch at [info@cognition.ai](mailto:info@cognition.ai).
