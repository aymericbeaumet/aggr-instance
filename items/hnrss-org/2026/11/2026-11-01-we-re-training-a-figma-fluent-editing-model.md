---
title: We're training a Figma-fluent editing model
link: https://www.figma.com/blog/training-a-figma-fluent-editing-model/
source: hnrss-org
published: 2026-11-01T04:05:23Z
updated: 2026-11-01T04:05:23Z
first_seen: 2026-11-04T07:26:52.515125251Z
authors:
- gmays
content: extracted
html: 2026-11-01-we-re-training-a-figma-fluent-editing-model.html
remote_preview:
  url: https://cdn.sanity.io/images/599r6htc/regionalized/fb5d45c15530cb8862429e9c6a70c004ee6f76e4-3264x1836.jpg?w=1200&q=70&fit=max&auto=format
---

Designers use Figma for a wide range of work, from exploring a new idea to building out an entire product. But a lot of the everyday work is smaller, more iterative, and often non-verifiable. Adjust the spacing. Change a color. Rework a layout after someone leaves feedback. Each change builds on decisions made along the way. That’s why when we set out to build [the Figma agent](https://www.figma.com/blog/the-figma-agent-is-here/)

, we started with a focused task: help designers make edits to the work already on their canvas.

![“Open space” card featuring a peach armchair, low wooden table, vase, circular wall light, and an arrow icon at top right.](https://cdn.sanity.io/images/599r6htc/regionalized/b05c89053eb2a6d69db765f1db20f82b9fcacac7-878x810.png?w=528&q=75&fit=max&auto=format&dpr=2)

Prompt: Insert location tag component to the top left of the photo

![1. “Open space” card featuring the same interior, with a white “Location” badge at top left and an arrow icon at top right.](https://cdn.sanity.io/images/599r6htc/regionalized/703a181faa617fe73ac9cab7814e99f321bc8945-878x810.png?w=528&q=75&fit=max&auto=format&dpr=2)

Output: The requested location tag is present, legible, and contained; unrelated content is preserved

We wanted our agent to be both better at design and faster at those edits. And because designers make them throughout the day, speed matters. An edit that takes too long can interrupt the very workflow it’s supposed to help.

## [Teaching a model to edit in Figma](https://www.figma.com/blog/training-a-figma-fluent-editing-model/#teaching-a-model-to-edit-in-figma)

When we started this work, frontier models couldn’t understand Figma syntax, and getting this to work would take a lot of system prompting that would be incredibly token-consumptive. Instead, we invested in supervised fine-tuning (SFT). Each training example paired a starting design with a generated synthetic edit request and a successful example of the change. We represented designs in Figma’s native file format, so the model learned to produce changes we could apply back to the canvas. When tasks required design system assets, we generated synthetic prompts covering changes to themes and components.

We then used larger teacher models to attempt those edits, checking the candidates with deterministic validators and visual model judges, including checks for valid output and request fulfillment. The generation pipeline could self-heal and revise an edit using evaluator feedback.

That approach got us surprisingly far. We were *even* able to get one of our earlier Llama 3.3 70B models, trained on more than 250,000 examples, to be competitive on editing tasks with top models at the time!

![Training pipeline diagram linking user content, interaction history, and design systems to training data generation, evaluator construction, model training, and LLM judge training informed by designer annotations.](https://cdn.sanity.io/images/599r6htc/regionalized/03f58348f89b623200a88adce014b10e2fd7a5f9-3262x1396.png?w=1632&q=75&fit=max&auto=format&dpr=2)

Our earliest SFT pipeline

But since then, we’ve moved towards more capable base models including the newer Qwen models, Muse Spark Open, Inkling, and others. We also ran up against limits to using SFT alone for this particular objective. Imitation learning teaches the model to produce designs very similar to what it saw in training at the cost of loss of generalization. Plus, it was less sample efficient, which means training takes a lot of examples covering various facets of the task. So we’re also investing in Reinforcement Learning (RL) as part of our latest training recipe. Our focus is on better RL environments for the design domain, pushing on the overall diversity of examples we train on, shaping our reward models, and expanding our expert-written criteria so those rewards reflect what designers actually care about.

## [How we evaluate design edits](https://www.figma.com/blog/training-a-figma-fluent-editing-model/#how-we-evaluate-design-edits)

To determine whether a model is a good starting point, we run it through our internal evals. We care not only about how well it can work in Figma, follow instructions, and make useful changes to existing designs, but also the speed and cost of doing that work.

Great design is often subjective, but that doesn’t mean we have to treat quality as one unmeasurable thing. A design can have good alignment and poor contrast. An edit can look polished and still miss the request. A button can look right while using the wrong component.

The examples below show common errors with AI-generated designs:

![Clothing product cards for a $120 angora cardigan and $120 quilted jacket, with heart icons overlapping the brand names.](https://cdn.sanity.io/images/599r6htc/regionalized/9891e5eeb0c64aadddf9ba4aad8095d6cb744a01-1056x792.png?w=528&q=75&fit=max&auto=format&dpr=2)

Overlapping icons and text

![Right-aligned shipping and policy panel with estimated delivery dates, truck and policy icons, and partially clipped dropdown arrows.](https://cdn.sanity.io/images/599r6htc/regionalized/57d0c250273dba80f00c6a6af94d3c5ada410163-1056x792.png?w=528&q=75&fit=max&auto=format&dpr=2)

Clipped arrows

![Hotel booking card showing $200.7 per night, overlapping location and description text, room previews, and a purple “Booking Now” button.](https://cdn.sanity.io/images/599r6htc/regionalized/ecf2f6a5066dbc309f0de20b776a304a053557d0-1056x792.png?w=528&q=75&fit=max&auto=format&dpr=2)

Overlapping text elements

We evaluate those dimensions separately. We combine checks on the design’s structure with visual judges, then use expert designers to calibrate and review the results. That helps us understand where a model succeeds and what it still needs to learn.

For example, here are how several top models score on our internal Figma editing evals. Again, this is just one input among many we use to decide which models to consider using in our product.

![Horizontal bar chart comparing Figma design edits v3 at 77.8% with Sol 6.1 at 76.9%.](https://cdn.sanity.io/images/599r6htc/regionalized/5c668a7a644ab6622a1069c539888aeda7049248-1701x1701.png?w=804&q=75&fit=max&auto=format&dpr=2)

Comparing the latest unreleased Figma edits model, post-trained on Qwen 3.8 27B with other third-party models on design editing tasks

## [Where this model fits in the Figma agent](https://www.figma.com/blog/training-a-figma-fluent-editing-model/#where-this-model-fits-in-the-figma-agent)

Earlier this month, we [rolled out the Figma agent to all users](https://www.figma.com/blog/3-ways-product-designers-use-the-figma-agent/)

, and it includes our first-party editing model. The agent runs both this specialized model and leading third-party models, using them for different parts of the work. For certain tasks like image generation, vectors, or brand new design explorations, we use the best third-party model available. For precise edits and iterations that our model handles well, we use our own.

The surrounding canvas matters, too. The agent needs context from the canvas and the design system, along with tools that can make the requested changes. Post-training helps the model do its part of that work better.

[![Figma's agent](https://cdn.sanity.io/images/599r6htc/regionalized/798d0189cd11880754d4e274b8d0ee3d7f9df236-2448x1377.avif?q=75&fit=max&auto=format&dpr=2)](https://www.figma.com/blog/3-ways-product-designers-use-the-figma-agent/)

We’re encouraged by early results we’ve seen with the Figma agent. Even in the time between beta to general availability, typical tasks are about 20% faster. In head-to-head testing with expert designers, the new agent beats or matches the previous production agent 62% of the time. User ratings are 33% higher, and users undo the agent’s work 60% less often.

We’ve more than doubled the share of work powered by our own post-trained design model. And we’ve reduced the average cost per message. Those gains come from improvements across the agent, including the models, tools, and harness. Our editing model is one part of that progress, and it gives us a way to keep improving the tasks designers repeat most often.

## [Where we go next](https://www.figma.com/blog/training-a-figma-fluent-editing-model/#where-we-go-next)

This work has demonstrated that post-trained models can compete with frontier models on a specialized task like Figma editing. Starting with a focused problem allows us to turn progress into meaningful product updates for our users so they can spend less time managing individual edits and more time bringing their vision to life.

If you’re interested in design and post-training models for non-verifiable domains, [check out our open roles](https://www.figma.com/careers/)—we’re hiring.

*We train in a way that respects customer privacy and preferences. You can learn more about our approach [here](https://www.figma.com/ai/our-approach/#model-training).*
