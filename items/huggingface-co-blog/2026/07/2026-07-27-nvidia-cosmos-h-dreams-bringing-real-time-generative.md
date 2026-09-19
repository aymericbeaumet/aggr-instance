---
title: 'NVIDIA Cosmos-H-Dreams: Bringing Real-Time Generative Simulation to Surgical Robotics'
link: https://huggingface.co/blog/nvidia/cosmos-h-dreams
source: huggingface-co-blog
published: 2026-07-27T09:32:20Z
updated: 2026-07-27T09:32:20Z
first_seen: 2026-09-19T21:30:23.395188495Z
content: extracted
html: 2026-07-27-nvidia-cosmos-h-dreams-bringing-real-time-generative.html
preview:
  file: 2026-07-27-nvidia-cosmos-h-dreams-bringing-real-time-generative.preview-6fd7dac26b35.webp
  width: 256
  height: 134
  color: '#d5e3dc'
images:
- source: https://cdn-uploads.huggingface.co/production/uploads/68c1279544f671330c604f4c/Cj0RZwvOSUhFGFEFKgyDw.png
  original:
    file: 2026-07-27-nvidia-cosmos-h-dreams-bringing-real-time-generative.image-b9cf5a418c40.png
    width: 1910
    height: 1000
  variants:
  - file: 2026-07-27-nvidia-cosmos-h-dreams-bringing-real-time-generative.image-76c0bab297b1.webp
    width: 320
    height: 168
  - file: 2026-07-27-nvidia-cosmos-h-dreams-bringing-real-time-generative.image-1e7ab5d018b7.webp
    width: 640
    height: 335
  - file: 2026-07-27-nvidia-cosmos-h-dreams-bringing-real-time-generative.image-01a95f95afad.webp
    width: 960
    height: 503
  - file: 2026-07-27-nvidia-cosmos-h-dreams-bringing-real-time-generative.image-6a81fac23360.webp
    width: 1280
    height: 670
  - file: 2026-07-27-nvidia-cosmos-h-dreams-bringing-real-time-generative.image-bc291b43d19a.webp
    width: 1600
    height: 838
  - file: 2026-07-27-nvidia-cosmos-h-dreams-bringing-real-time-generative.image-3d721d38dced.webp
    width: 1910
    height: 1000
  color: '#fcfdfb'
---

Surgical robotics is moving quickly from teleoperation toward increasingly capable vision-language-action policies. But evaluating and training these systems remains difficult. Physical robotic platforms are expensive to operate, experiments are slow to reproduce, and failures can damage instruments or biological material. Conventional simulators provide a safer alternative, but surgical scenes are exceptionally difficult to model: deformable tissue, fine instrument interactions, specular surfaces, sutures, needles, smoke, and occlusions all matter.

World foundation models offer a different path. Instead of manually authoring every object and physical interaction, they learn visual dynamics directly from synchronized video and robot kinematics. NVIDIA's [Cosmos-H-Surgical-Simulator](https://huggingface.co/nvidia/Cosmos-H-Surgical-Simulator) demonstrated this approach by generating future surgical video from an initial scene and a sequence of robot actions. It enabled faster-than-physical evaluation and synthetic data generation across the [Open-H-Embodiment](https://huggingface.co/datasets/nvidia/PhysicalAI-Robotics-Open-H-Embodiment) ecosystem.

Today, we are introducing the next step: [Cosmos-H-Dreams](https://github.com/isaac-for-healthcare/Cosmos-H-Dreams), a real-time, action-conditioned generative simulator for surgical robotics. Cosmos-H-Dreams distills the capabilities of Cosmos-H-Surgical-Simulator into a causal, few-step student model and serves it through [FlashDreams](https://github.com/NVIDIA/flashdreams), NVIDIA's accelerated streaming-inference library. Running on a single NVIDIA RTX PRO 6000 GPU, the result is an interactive environment that a person or a learned policy can control in a closed loop.

* * *

## [](https://huggingface.co/blog/nvidia/cosmos-h-dreams#1-from-surgical-world-model-to-interactive-simulator) 1. From Surgical World Model to Interactive Simulator

[Cosmos-H-Surgical-Simulator](https://huggingface.co/nvidia/Cosmos-H-Surgical-Simulator) is an action-conditioned world foundation model built on NVIDIA [Cosmos-Predict2.5-2B](https://huggingface.co/nvidia/Cosmos-Predict2.5-2B) and post-trained on the [Open-H-Embodiment](https://huggingface.co/datasets/nvidia/PhysicalAI-Robotics-Open-H-Embodiment) dataset. Given a surgical context frame and a future robot trajectory, it generates video showing the likely visual consequences of those actions. This makes it useful for offline policy evaluation and synthetic data generation. A recorded or policy-generated trajectory can be sent to the model, the corresponding rollout can be generated, and the result can be inspected or scored without repeatedly executing the motion on a physical robot.

Cosmos-H-Dreams moves the model into the real-time regime. Starting from the multi-embodiment surgical priors learned by Cosmos-H-Surgical-Simulator, we specialize the model for da Vinci Research Kit (dVRK) tabletop suturing and distill it into a causal student that generates the scene autoregressively. The released model receives an initial RGB frame and a live stream of robot kinematics, then produces the next chunk of frames before continuing with the following action block.

We have also demonstrated the versatility of Cosmos-H-Dreams by collaborating with CMR Surgical and Cambridge Consultants to integrate it with the Versius surgeon controller, enabling real-time operation on the Versius platform.

* * *

## [](https://huggingface.co/blog/nvidia/cosmos-h-dreams#2-distilling-cosmos-h-surgical-simulator-for-real-time) 2. Distilling Cosmos-H-Surgical-Simulator for Real Time

The key challenge is preserving useful surgical dynamics while reducing the cost of generation. Cosmos-H-Dreams uses a teacher-to-student training pipeline designed for long, autoregressive rollouts.

### [](https://huggingface.co/blog/nvidia/cosmos-h-dreams#21-a-surgical-teacher) 2.1. A Surgical Teacher

The bidirectional teacher begins from the Cosmos-H-Surgical-Simulator Open-H checkpoint, which uses a unified 44-dimensional action representation. For the released dVRK tabletop model, the dual-arm dVRK action content, consisting of relative end-effector translation, rotation, and gripper state, is mapped into this common representation.

The teacher is then fine-tuned on the JHU dVRK tabletop mixture, including successful demonstrations as well as failure and out-of-distribution episodes such as needle drops, missed throws, and unsuccessful knot ties. These failures are important: a simulator intended to evaluate policies must reproduce the consequences of poor actions, not only ideal demonstrations.

To improve stability during long rollouts, the training process progressively increases the teacher's temporal horizon. We start training on a 12-frame horizon and progressively increase this number until 72 frames. At each horizon bump, we initialize the warmed-up model with pretrained weights.

### [](https://huggingface.co/blog/nvidia/cosmos-h-dreams#22-causal-warmup) 2.2. Causal Warmup

The teacher's denoising trajectories are first precomputed and cached. A causal student is initialized from the teacher and trained to imitate these cached trajectories. This warmup stage teaches the student to operate with causal attention and a streaming key/value cache before it begins learning from its own generated history.

### [](https://huggingface.co/blog/nvidia/cosmos-h-dreams#23-self-forcing-distillation) 2.3. Self-Forcing Distillation

Autoregressive models face a familiar problem: during training they may see clean, ground-truth context, while during deployment they must condition on their own imperfect outputs. Small errors can therefore compound over time.

Cosmos-H-Dreams addresses this mismatch with [**self-forcing distillation**](https://arxiv.org/abs/2506.08009). During training, the student rolls forward using its own generated context. Distribution-matching supervision from the frozen teacher then guides those self-generated rollouts toward realistic surgical video. This prepares the student for the same conditions it will encounter during interactive inference.

The resulting model supports few-step diffusion, with as few as two denoising steps per latent frame, rather than the many-step process used by the full teacher. It combines the teacher's surgical priors with the causal structure needed for streaming.

* * *

## [](https://huggingface.co/blog/nvidia/cosmos-h-dreams#3-flashdreams-the-real-time-inference-engine) 3. FlashDreams: The Real-Time Inference Engine

Model distillation is only part of the real-time story. Cosmos-H-Dreams is served through [FlashDreams](https://github.com/NVIDIA/flashdreams), an accelerated inference library for autoregressive world and video models.

FlashDreams turns the distilled student into a low-latency streaming system through several complementary optimizations, such as streaming KV cache, CUDA Graph capturing, or model compilation.

Together, these techniques bring the distilled surgical world model fine-tune from the roughly ten-frames-per-second regime of standard Cosmos-H-Surgical-Simulator inference to interactive operation (~160 frames per second) on a single **NVIDIA RTX PRO 6000**.

Cosmos-H-Dreams also provides the human-machine interfaces that turn generation into interaction. A browser client can send keyboard commands and receive generated frames over WebRTC. A Meta Quest client can map tracked controller motion into robot actions and display the synthesized scene through WebXR. The same model can also be connected to a learned surgical policy, with generated observations and predicted actions exchanged inside a closed loop.

* * *

## [](https://huggingface.co/blog/nvidia/cosmos-h-dreams#4-adapting-to-your-own-data) 4. Adapting to Your Own Data

While Cosmos-H-Dreams includes a pre-trained checkpoint for tabletop suturing, the system is designed to be extensible to your specific embodiment. To train a real-time student model for your own dataset, we provide a complete recipe for teacher fine-tuning and self-forcing distillation in our [step-by-step guide](https://github.com/NVIDIA-Medtech/Cosmos-H-Surgical-Simulator/blob/main/docs/tutorial_teacher_training_and_self_forcing.md).

* * *

## [](https://huggingface.co/blog/nvidia/cosmos-h-dreams#5-what-is-next-toward-closed-loop-surgical-physical-ai) 5. What Is Next: Toward Closed-Loop Surgical Physical AI

Cosmos-H-Dreams opens a new frontier for surgical simulation: environments learned from real robot data that are responsive enough to be inhabited.

The immediate next step is to evaluate more than visual quality. A useful surgical simulator must respond correctly to actions, preserve instrument and scene structure over long rollouts, and support conclusions that transfer to the physical robot. This motivates a new family of closed-loop benchmarks: tool-tip reach and pose accuracy, gripper-cycle fidelity, idle stability, counterfactual action diversity, long-horizon drift, and agreement between simulated and real policy outcomes.

Real-time world models can also become active partners in surgical policy development. They can generate rare failures on demand, provide scalable environments for reinforcement or imitation learning, and enable rapid evaluation of new policies without tying every experiment to scarce robotic hardware.

Further ahead, real-time simulation enables a series of downstream applications such as latency-aware telesurgery, where the world model helps maintain a more stable display; or interactive surgical rehearsal, procedure planning, and intraoperative decision support. Cosmos-H-Dreams is a research and development platform, not a diagnostic system, a replacement for intraoperative imaging, or a controller for a physical surgical robot. Yet it provides a foundation for exploring these possibilities safely.

As model fidelity, temporal stability, and hardware efficiency continue to improve, real-time generative simulation can help connect surgeon education, synthetic data generation, policy training, and policy evaluation within one shared Physical AI environment.

* * *

## [](https://huggingface.co/blog/nvidia/cosmos-h-dreams#6-get-started-today) 6. Get Started Today

Explore the models, data, and runtime behind Cosmos-H-Dreams:

- **Cosmos-H-Dreams code and examples:** [GitHub repository](https://github.com/isaac-for-healthcare/Cosmos-H-Dreams)
- **Cosmos-H-Dreams model:** [Hugging Face checkpoint](https://huggingface.co/nvidia/Cosmos-H-Dreams)
- **Cosmos-H-Surgical-Simulator:** [Hugging Face model](https://huggingface.co/nvidia/Cosmos-H-Surgical-Simulator) and [GitHub repository](https://github.com/NVIDIA-Medtech/Cosmos-H-Surgical-Simulator)
- **Adapt to your own data:** [Step-by-step recipe for teacher fine-tuning and self-forcing distillation](https://github.com/NVIDIA-Medtech/Cosmos-H-Surgical-Simulator/blob/main/docs/tutorial_teacher_training_and_self_forcing.md)
- **Open-H-Embodiment:** [Hugging Face dataset](https://huggingface.co/datasets/nvidia/PhysicalAI-Robotics-Open-H-Embodiment)
- **FlashDreams:** [GitHub repository](https://github.com/NVIDIA/flashdreams)
- **NVIDIA Cosmos-Predict2.5:** [GitHub repository](https://github.com/nvidia-cosmos/cosmos-predict2.5)
- **Cosmos-Surg-dVRK:** World Foundation Model-based Automated Online Evaluation of Surgical Robot Policy Learning ([arXiv paper](https://arxiv.org/abs/2510.16240))

Cosmos-H-Dreams brings action-conditioned surgical world modeling into the real-time loop, creating a new environment for people and policies to practice, explore, generate data, and evaluate what happens next.
