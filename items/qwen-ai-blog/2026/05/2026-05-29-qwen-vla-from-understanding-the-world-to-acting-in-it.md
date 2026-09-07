---
title: 'Qwen-VLA: From Understanding the World to Acting in It'
link: https://qwen.ai/blog?id=qwenvla
source: qwen-ai-blog
published: 2026-05-29T09:00:00Z
first_seen: 2026-09-07T17:03:44.343711062Z
authors:
- QwenTeam
labels:
- release
content: feed
html: 2026-05-29-qwen-vla-from-understanding-the-world-to-acting-in-it.html
preview:
  file: 2026-05-29-qwen-vla-from-understanding-the-world-to-acting-in-it.preview-6b96d61807dc.webp
  width: 256
  height: 154
  color: '#beb4e8'
images:
- source: https://img.alicdn.com/imgextra/i3/O1CN013MqZ6I1aTeRGXyxAB_!!6000000003331-2-tps-1590-954.png
  original:
    file: 2026-05-29-qwen-vla-from-understanding-the-world-to-acting-in-it.image-13ab25645488.png
    width: 1590
    height: 954
  variants:
  - file: 2026-05-29-qwen-vla-from-understanding-the-world-to-acting-in-it.image-6ff023d3f27d.webp
    width: 48
    height: 29
  color: '#8587f6'
- source: https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-VLA/head_en.png
  original:
    file: 2026-05-29-qwen-vla-from-understanding-the-world-to-acting-in-it.image-403a591d1f79.png
    width: 1600
    height: 900
  color: '#fdfdfd'
- source: https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-VLA/qwen35vla_arc.png
  original:
    file: 2026-05-29-qwen-vla-from-understanding-the-world-to-acting-in-it.image-291265cbd113.png
    width: 4181
    height: 1465
  color: '#fdfdfd'
- source: https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-VLA/t2a_combined.png
  original:
    file: 2026-05-29-qwen-vla-from-understanding-the-world-to-acting-in-it.image-54204b96cba7.png
    width: 1095
    height: 359
  color: '#fbfcfd'
---

![Qwen3.6 Main Image](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-VLA/head_en.png)

[GitHub](https://github.com/QwenLM/Qwen-VLA) [Paper](https://arxiv.org/pdf/2605.30280) [Demo](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-VLA/demo.mp4)

Over the past few years, multimodal large language models have become increasingly capable of understanding images, videos, and real-world scenes. They can recognize objects, reason about spatial relationships, answer visual questions, and solve complex multimodal reasoning tasks.

But for embodied intelligence, **understanding the world is only the first step**. A truly embodied agent also needs to understand task goals, take actions in the physical world, and generalize across different robot embodiments, environments, and tasks.

This is the motivation behind **Qwen-VLA**.

Qwen-VLA is a general-purpose **Vision-Language-Action** model. Built upon the Qwen multimodal backbone, it extends visual perception, language understanding, and spatial reasoning into continuous action generation and trajectory prediction. In other words, it allows the model to not only **see** and **think**, but also begin to **act**.

* * *

## One Model for Multiple Embodied Tasks [#](https://qwen.ai/blog?id=qwenvla#one-model-for-multiple-embodied-tasks)

Traditional embodied AI systems are often highly specialized: one model for tabletop manipulation, another for navigation, and yet another for a specific robot platform. This approach can work well for individual tasks, but it does not scale easily to broader tasks, diverse environments, or different robot embodiments.

Qwen-VLA explores a more unified direction:

> Can a single generalist policy model support robotic manipulation, vision-language navigation, and cross-embodiment control at the same time?

In Qwen-VLA, robotic manipulation and vision-language navigation are formulated under the same framework: given visual observations, language instructions, and embodiment-specific conditions, the model predicts the next action or trajectory. The Qwen multimodal backbone understands the visual and language inputs, while an action decoder generates continuous actions.

* * *

## Training: From Language Priors to Closed-Loop Control [#](https://qwen.ai/blog?id=qwenvla#training-from-language-priors-to-closed-loop-control)

![Qwen-VLA Training Recipe](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-VLA/qwen35vla_arc.png)

The core of Qwen-VLA is not simply attaching an action head to a multimodal model. More importantly, it builds a joint training system that covers diverse tasks, environments, and robot embodiments. The full training pipeline progresses through four stages, from language priors to closed-loop control.

### Data [#](https://qwen.ai/blog?id=qwenvla#data)

The pretraining data spans five major sources:

- **Robot manipulation trajectories** form the foundation, covering tabletop, mobile, dual-arm, and dexterous manipulation. The public data totals over **10,000 hours**, supplemented by more than **1,000 hours** of internal real-robot trajectories and over **8 million synthetic simulation trajectories**.

- **Human egocentric data** provides richer object, scene, and hand-action priors from open-world environments. We incorporate Ego4D, EPIC-KITCHENS, EgoDex (**829 hours**), EgoVerse (**1,300+ hours**, **1,965 tasks**, **240 scenes**), and Xperience.

- **Synthetic simulation data** fills long-tail gaps. Vision-conditioned data covers **20 tabletop scenes**, **200 configurations**, **450 tasks**, and **359,848 successful trajectories**. Text-to-action data spans **6 templates** × **6 single-arm robots**, yielding about **7.2 million trajectories** and over **14,000 hours**.

- **Vision-language navigation data** provides long-horizon trajectory planning and instruction-following capabilities.

- **General vision-language data** preserves multimodal understanding, spatial grounding, and instruction following. We also build around **48,000 fine-grained action descriptions** annotated across **13 dimensions**, aligning natural language with concrete execution details.

### Four-Stage Training [#](https://qwen.ai/blog?id=qwenvla#four-stage-training)

The key idea: first learn to generate action structures from language, then learn to adapt those actions to the visual environment.

![Qwen-VLA T2A Ablations](https://qianwen-res.oss-accelerate.aliyuncs.com/Qwen-VLA/t2a_combined.png)

- **Stage I: T2A (Text-to-Action Pretraining).** An instruction like “pick up the red cup” is just a few words, but the corresponding robot action is a high-dimensional continuous trajectory. Qwen-VLA treats this as a form of **decompression from language to action**. In T2A, we freeze the VLM and train only the action decoder on language and embodiment prompts **without any images**.

- **Stage II: CPT（Continual Pretraining）.** We unfreeze both the VLM and action decoder and jointly train on the full multimodal data mixture. This stage grounds the language-action priors from T2A in concrete visual scenes while adapting the backbone to embodied perception, producing **Qwen-VLA-Base**.

- **Stage III: SFT (Supervised Fine-Tuning).** Starting from the CPT checkpoint, we branch into two tracks: multi-task SFT jointly fine-tunes on manipulation, navigation, VQA, and spatial grounding; real-robot SFT fine-tunes on in-house teleoperation data for physical deployment.

- **Stage IV: RL (Reinforcement Learning).** Starting from the SFT checkpoint, we use PPO to directly optimize closed-loop task success in simulation, producing the final model **Qwen-VLA-Instruct**. RL is conducted only in SimplerEnv, yet experiments show its gains transfer to unseen environments and robot embodiments.

* * *

## Performance [#](https://qwen.ai/blog?id=qwenvla#performance)

### A Single Generalist Model Can Match or Even Surpass Specialist Models [#](https://qwen.ai/blog?id=qwenvla#a-single-generalist-model-can-match-or-even-surpass-specialist-models)

The experimental results show the potential of Qwen-VLA as a generalist policy model. A single model can cover multiple manipulation benchmarks, including LIBERO, Simpler, RoboCasa, and RoboTwin, while approaching or surpassing specialized policy models on several tasks.

| Benchmark            | Best Specialist Model | Qwen-VLA          |
| -------------------- | --------------------- | ----------------- |
| LIBERO               | ABot-M0 98.6%         | 97.9%             |
| RoboCasa-GR1         | ABot-M0 58.3%         | 56.7%             |
| Simpler-WidowX       | StarVLA-OFT 64.6%     | **73.7%**         |
| RoboTwin-Easy / Hard | ABot-M0 86.0% / 85.0% | **86.1% / 87.2%** |

On robotic manipulation benchmarks, Qwen-VLA-Instruct achieves **97.9%** on LIBERO, **73.7%** on Simpler-WidowX, and **86.1% / 87.2%** on RoboTwin-Easy / Hard. Many of the compared methods are specialist models fine-tuned for individual benchmarks, while Qwen-VLA is a unified generalist model trained under a single framework.

On vision-language navigation (VLN-CE), Qwen-VLA-Instruct achieves **69.0%** Oracle Success Rate and **57.5%** Success Rate on R2R Val-Unseen, and **59.6%** SR and **47.8%** SPL on the more challenging RxR Val-Unseen, surpassing all open-source baselines.

In real-world ALOHA dual-arm experiments, Qwen-VLA pretrained model achieves **83.6%** average in-domain success and **76.9%** average OOD success, substantially outperforming training from scratch (48.5% / 36.2%) and $\\pi\_{0.5}$ (71.6% / 41.5%).

* * *

### Real-World Out-of-Distribution Generalization [#](https://qwen.ai/blog?id=qwenvla#real-world-out-of-distribution-generalization)

We also care about how Qwen-VLA generalizes on real robots.

In real-world ALOHA dual-arm robot experiments, Qwen-VLA demonstrates generalization to unseen colors, objects, backgrounds, positions, and language instructions. Compared with policies trained from scratch, models pretrained with Qwen-VLA show clear improvements under real-world out-of-distribution settings.

This part is best shown through videos. The following demonstrations are tested with the **Qwen-VLA-Base** model. When asked to “pick up the green ball” or “pick up the blue ball,” the model can correctly act based on color-specific instructions. When presented with unseen objects such as toys, vegetables, or sunglasses, it can still follow language commands to grasp or move them. When the background, lighting, and tabletop layout change, the model remains relatively stable. For compositional tasks such as “tidy up the table,” it can identify multiple targets and execute multi-step operations.

Compared with tables alone, these videos better illustrate the core value of Qwen-VLA:

> The model is not merely memorizing action templates in a fixed environment. It is learning to understand goals and act under real-world variations.

* * *

### Zero-Shot Generalization in Dynamic Scenes [#](https://qwen.ai/blog?id=qwenvla#zero-shot-generalization-in-dynamic-scenes)

Beyond static tabletop manipulation, Qwen-VLA also shows zero-shot generalization in dynamic manipulation tasks.

On the DOMINO dynamic manipulation benchmark, Qwen-VLA-Instruct is not specifically fine-tuned for the benchmark, yet it still achieves a **26.6% success rate** and a **39.5 manipulation score**, outperforming a range of standard VLA baselines and even some specialist models for dynamic manipulation.

This suggests that the model is not only learning grasping templates in static scenes, but also acquiring a more transferable action prior from spatial understanding to motion control. Given visual observations, language goals, and its action generation capability, the model can directly produce coherent action sequences and complete tasks within dynamic interaction windows.

* * *

## From Multimodal Understanding to Embodied Intelligence [#](https://qwen.ai/blog?id=qwenvla#from-multimodal-understanding-to-embodied-intelligence)

Qwen-VLA is a natural extension of Qwen’s multimodal capabilities toward embodied intelligence.

In the past, multimodal models mainly focused on **understanding the world**. With Qwen-VLA, we further explore how models can generate actions in the physical world based on vision and language.

Qwen-VLA unifies robotic manipulation, vision-language navigation, and cross-embodiment control. It connects Qwen’s visual understanding and spatial reasoning capabilities to continuous action generation. Through joint pretraining on real robot data, human egocentric data, synthetic simulation data, and general vision-language data, it learns more general embodied experience. It also demonstrates the potential of generalist policy models across manipulation benchmarks, real-world out-of-distribution generalization, and zero-shot dynamic manipulation.

Embodied intelligence is still at an early stage. Long-horizon real-world tasks, failure recovery, continual learning, and more complex human-robot-environment interactions remain challenging. But Qwen-VLA points to a clear next step:

> Models should not only understand the world — they should also learn to act in it.

## Citation [#](https://qwen.ai/blog?id=qwenvla#citation)

```bibtex
@article{qwenvla,
  title={Qwen-VLA: Unifying Vision-Language-Action Modeling across Tasks, Environments, and Robot Embodiments},
  author={Qwen Team},
  year={2026},
  eprint={2605.30280},
  archivePrefix={arXiv},
  primaryClass={cs.RO},
  url={https://arxiv.org/abs/2605.30280}, 
}
```
