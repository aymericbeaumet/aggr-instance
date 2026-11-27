---
title: 'Control Room: A central system to track and stop model workloads across our research infrastructure'
link: https://openai.com/index/control-room
source: openai-com
published: 2026-11-09T13:00:00Z
updated: 2026-11-09T13:00:00Z
first_seen: 2026-11-10T03:25:21.974411074Z
labels:
- safety
summary: Learn how OpenAI’s Control Room tracks model workloads across our research infrastructure and can rapidly stop them when safety or security concerns arise.
content: extracted
html: 2026-11-09-control-room-a-central-system-to-track-and-stop-model.html
---

As our models get more powerful and the associated safety and security risks become bigger, it becomes more and more critical that we can detect potential issues, and quickly stop models in their tracks when needed. To solve this, we built Control Room.

Control Room is a central system to track model workloads across our research infrastructure and stop them quickly when safety or security concerns arise. It connects every research inference workload, including agentic sampling for model training and evaluation, data processing, and more. This works together with our [misalignment monitoring systems](https://openai.com/index/how-we-detect-frontier-model-misalignment/), and halts model output within 35 seconds of a decision to intervene to stop or pause a job. It has been built to keep working even when other systems fail, and to stop model output if Control Room itself goes down.

Control Room is already deployed across our research infrastructure, where the need is greatest. Our external model-serving stack is already much more centralized by nature, with a well-developed operational capability to quickly take down model deployments if needed.

## How it works

Control Room works by integrating a few systems: the *workload*, the *inference engine*, and the centralized *Control Room service*.

- The *workload* is the job being run, such as a reinforcement learning experiment, an evaluation, a high-volume sampling or data processing job. We require every research workload that does agentic inference to be associated with an approved [safety case](https://openai.com/index/towards-safety-cases-for-frontier-ai-training/).

- The *inference engine*, or just *engine*, is the model itself. You can think of the engine as the brain of the model, running directly on GPUs. A workload normally uses one or more models, which might be running on one or more different inference engines. The same inference engine might be used by more than one different workload.

- *The Control Room service* tracks jobs and controls whether they are allowed to continue generating model output. Engines check with the Control Room service before fulfilling a request. If a job has been stopped, the service denies the request, preventing further output.

Control Room is integrated in the following way:

- When starting a new job, the workload code requests a new job\_id from Control Room.

- Whenever the workload needs to sample tokens from a model, it makes a request to an inference engine running that model, passing the job\_id. The engine requests permission from Control Room to fulfill that request, passing the job\_id along to Control Room.

- Control Room checks whether that job has been terminated, and if so, it denies the inference request. The engine responds to the workload's inference request with a refusal, stopping further model output from being produced for that job.

## How Control Room works

STEP 1

### A researcher decides to start a job

The process starts with a researcher launching a job—for example, an experiment or an evaluation.

## Example: reacting to a monitoring alert

All of our frontier reinforcement learning jobs run with a monitor. A *monitor* is another model which inspects everything that the model-being-trained is doing, and raises an alert if it detects a problem. For example, our recent [DNS-related incident](https://alignment.openai.com/misalignment-reports/an-agent-used-dns-to-reach-an-external-chatbot/) was caught by a monitor.

Control Room and our monitoring system are integrated in the following way:

- If the monitoring system detects a potential issue, depending on the details and severity, the monitoring system may decide to stop the job.

- In this case, the monitoring system sends a message to Control Room to stop the job, passing the job\_id. Control Room records that the job is terminated.

- Next time the job having that job\_id makes an inference request to sample more tokens from *any* model running on *any* engine, the request will be denied, stopping further model output.

## Speed and reliability

The maximum time between the moment when Control Room receives an instruction to stop a job, and the moment when further progress is no longer possible, is 35 seconds. The 35-second lag comes from a caching layer for inference authorization, to support the required volume of traffic. We are working to drive this number down to zero so that we can end jobs instantly.

Workloads, including experiments, evaluation jobs, etc., are normally owned and operated by researchers or teams of researchers. As a result, workload code changes often, and is not an ideal place to implement critical load-bearing safety protections. Because of this, our implementation is designed to be resilient to buggy or broken workload code.

We achieve this by implementing the termination mechanism in the inference layer. Workload code is highly-varied and decentralized. In contrast, there are only a very small number of inference code paths in use across all of OpenAI research, and their deployments are much more centralized and uniform. This means that the inference layer can serve as a bottleneck, where we can get strong guarantees that all model inference running in research is subject to Control Room authorization checks on every inference request. We additionally run continual agentic and programmatic sweeps to check that users are not accidentally or maliciously launching rogue engine implementations with Control Room unimplemented or disabled.

Finally, our system “fails closed”, meaning that the Control Room integration in the engines is configured in such a way that if the Control Room service goes down, either due to a system error or because it is disabled for any other reason, including tampering by a bad actor, then all inference requests will be denied until Control Room resumes normal operation. Fail-closed is rolled out now to all frontier model research workloads, and will finish rolling out across all remaining research workloads over the next few days.

Control Room is just one part of our holistic approach to safe frontier model training and deployment. We’ll continue to strengthen and expand the system over the coming months, alongside our investments in monitoring for misaligned behavior and security breaches. If you want to help build systems for the safe and secure development and deployment of AGI, [we're](https://openai.com/careers/research-engineer-research-scientist-ai-systems-engineer-rsi-san-francisco/) [hiring](https://openai.com/careers/researcher-agent-safety-oversight-and-system-mitigations-san-francisco/)!
