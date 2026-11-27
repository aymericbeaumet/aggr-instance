---
title: Accelerating engineering for the physical world
link: https://openai.com/index/accelerating-engineering-for-the-physical-world
source: openai-com
published: 2026-11-03T12:00:00Z
updated: 2026-11-03T12:00:00Z
first_seen: 2026-11-03T20:48:58.593592327Z
labels:
- research
summary: Discover how ChatGPT and Codex are helping engineers accelerate CAD, PCB design, simulation, and other workflows across the physical world.
content: extracted
html: 2026-11-03-accelerating-engineering-for-the-physical-world.html
---

Today, we’re sharing advances in how OpenAI models help engineers design, simulate, and build physical products. These capabilities span mechanical and electrical engineering and end-to-end system design, and are already helping our internal teams test ideas and iterate faster.

Across these projects, engineers need to bring together mechanical designs, electronics, software, and manufacturing requirements using specialized tools. We’re teaching models to support that work, from preparing drawings and setting up simulations to coordinating changes across disciplines. Our goal is to give engineers more time to explore ideas and improve designs.

Over the past year, improved frontier model capabilities have helped our internal teams test new ideas and iterate faster, and we are seeing early evidence of these capabilities improving professional workflows in industry as well. We’re working with collaborators to understand where these capabilities can make the biggest difference and what needs to improve. If you’re an engineer, engineering firm, or software vendor involved in the design, simulation, manufacturing, or production of complex physical products, we’d love to [work with you](https://openai.com/form/computer-use-research-collaboration/) to shape this effort around the tools you use and the problems you need to solve.

## Designing a device with Astra

We tested [Astra’s state-of-the-art performance](https://openai.com/index/gpt-6-astra/) in multimodal and computer use workflows by seeing if we can create a device without any direct human edits. To do this, we used GPT‑6 to define the functional requirements and design the enclosure, printed circuit board (PCB), and software integrations needed to create a device users can use to talk to Codex.

- **Aesthetics and design:** Astra developed the look of the device, drawing on our recent [Work Louder](https://worklouder.cc/codex-micro) collaboration and references from games and toys.

- **Mechanical design:** Astra designed the [enclosure](https://images.ctfassets.net/kftzwdyauwt9/5aP4AhwHhDUmDxisSLcb2X/12114476f73d643b73dffe90b819cc0c/prototype-device-enclosure-diagram.png) and the internal structures that hold the components in place.

- **Electrical design:** Astra selected the electrical components and designed the printed circuit board (PCB), including component placement and connections, to fit within the enclosure.

- **UI/UX and software:** Astra designed the user interface and generated the firmware and software integrations needed to connect the device to Codex.

Two team members designed this test device with Astra in under a week while working on it as a side project. A contract manufacturer built a small batch of prototypes in under a month.

Getting the device ready for manufacturing still required several rounds of review and iteration. Astra did not consistently account for the manufacturer’s production limits, tolerances, or details required for assembly. These gaps are helping us identify what future models need to do better.

## Accelerating OpenAI hardware teams

OpenAI’s hardware and infrastructure teams use Codex and ChatGPT to create and revise computer-aided design (CAD) models, prepare manufacturing drawings, set up simulations, and review designs. This work spans robotics and compute infrastructure. Since February, model usage across our hardware teams, measured in tokens, has grown 23-fold.

1 of 3

## Measuring progress on engineering tasks

We’re also measuring progress in newer models on tasks that combine coding with engineering knowledge. BenchCAD 2.0 tests whether models can reconstruct 3D parts and assemblies and identify connections in circuit boards. A newer internal model achieved an average score of 0.875, compared with 0.594 for GPT‑6 Astra at its highest reasoning setting. While the engineering lifecycle includes many other elements beyond design, we view improvements in capability here as early evidence of the ability of AI models to positively impact iteration speed for professional workflows.

## Safely deploying with industry collaborators

We’re working with a small group of industry collaborators to test these capabilities on real engineering problems. Their engineers review model outputs for accuracy and practical use, helping us understand which tasks models can support today and what needs to be improved for safe and reliable use.

We’re expanding our collaboration with [Chip Ganassi Racing](https://openai.com/index/chip-ganassi-racing/) (CGR) to explore how OpenAI models can accelerate engineering and prototyping. Working with its R&D team, we’re testing workflows that turn 3D scans of existing parts into editable CAD models and 2D manufacturing drawings. This is particularly useful when the original design files are unavailable.

CGR engineers review the models and drawings for accuracy and manufacturability. Their feedback helps us identify where models can reduce manual work and where further improvements are needed.

We’re also announcing a new research collaboration with Mazda. Our research and Forward Deployed Engineering teams are working with Mazda’s aerodynamics and crash-safety engineers to explore how OpenAI models can help develop and evaluate new vehicle designs.

The work uses simulations of airflow and structural behavior to explore improvements in performance while accounting for styling and cost. We’ll share more progress and results in the coming months.

GPT‑6 Astra helps Mazda’s aerodynamics engineers explore ways to reduce vehicle drag by modeling new nose shapes, simulating airflow, and refining the most promising designs. Agreement for use has been obtained from Mazda Motor Corporation. Any republication or reuse elsewhere is strictly prohibited.

## Help shape AI for engineering and manufacturing

We have been working closely with industry partners and early adopters to push the bounds of what we can do with AI agents in hardware and manufacturing. These early results have shown exciting promise, and we’re now looking for collaborators to accelerate these capability improvements and ensure we deploy these safely into industry.

If you are interested in working with us to shape the training and development of our frontier models by identifying high-value use cases, software tooling, or future opportunities in mechanical engineering, electrical engineering, electromechanical design, mechatronics, or a related field, please fill out [this form](https://openai.com/form/computer-use-research-collaboration/) and we'll be in touch.
