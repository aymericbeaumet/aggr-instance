---
title: How GPT-5.6 Sol helps run quantum computing experiments
link: https://openai.com/index/codex-quantum-computing-experiments
source: openai-com
published: 2026-09-08T17:00:00Z
updated: 2026-09-08T17:00:00Z
first_seen: 2026-10-19T01:26:53.998049939Z
labels:
- applied ai
summary: See how an MIT researcher uses GPT-5.6 Sol with Codex to autonomously run quantum computing experiments, analyze results, and calibrate qubits.
content: extracted
html: 2026-09-08-how-gpt-5-6-sol-helps-run-quantum-computing-experiments.html
---

Quantum computing is an emerging technology that uses the unique properties of quantum mechanics to process information. It could one day better simulate complex materials and molecules. Unlike conventional processors, quantum processors are built with quantum bits, or qubits. Preparing and running qubit experiments can take months and require hundreds to thousands of preliminary measurements—work that AI is poised to help with.

Beatriz Yankelevich, a graduate student in MIT’s Engineering Quantum Systems Group (EQuS), used GPT‑5.6 Sol, harnessed to Codex, to explore whether AI could streamline her experimental workflow. The MIT group studies superconducting qubits, which are cooled to near absolute zero inside specialized devices called dilution refrigerators. These qubits perform operations quickly, are precisely controlled using microwave signals, and can be made using familiar manufacturing techniques and arranged on a chip.

Once a superconducting qubit chip has been fabricated, packaged, and cooled, researchers interact with it entirely through software, making Yankelevich’s experiments a natural testbed for AI agents. Connecting Codex to the lab software that coordinates experiments allowed it to run measurements, analyze the results, and decide what to try next. Yankelevich found that GPT‑5.6 Sol could often complete routine measurement workflows autonomously, saving her significant amounts of time and allowing experiments to run without constant supervision. This freed her to spend more time on analyzing results, designing experiments, and planning out the next steps in her research.

![A packaged qubit chip beside an open dilution refrigerator with cabling that connects to the chip.](https://images.ctfassets.net/kftzwdyauwt9/5NohKx2xMeC8fqT4juVKSy/703ae0c94710ac5cbfe5e5b8eccc40b5/Images.png?w=3840&q=90&fm=webp)

*A packaged qubit chip (left) sits inside an open dilution refrigerator (right). CREDIT: EQuS group*

## Coordinating interdependent measurements

Superconducting qubits are often called artificial atoms because, like atoms, they can only occupy specific energy levels. Microwave pulses move qubits between these levels and probe their quantum state. Researchers design and calibrate the pulse sequences sent to the chip, then digitize and analyse the returning signals. These measurements reveal each qubit’s resonance frequencies, which allows researchers to accurately control the qubit; how long the qubit retains quantum information; and the settings needed to perform computations.

Calibrating qubits requires a series of interdependent measurements, with each result shaping what happens next. Qubit properties can occasionally drift, and unexpected physical behavior can cause inconsistent results. Experienced researchers can recognize these changes and adapt when they occur. This combination of software control, repeated measurements, and adaptive decision-making also makes qubit calibration a compelling use case for AI agents.

Yankelevich tested GPT‑5.6 Sol’s ability to run measurements on an uncalibrated six-qubit chip, one of a standard type that EQuS routinely uses to benchmark its fabrication process. She provided Codex with measurement-specific skills explaining how to run and evaluate each experiment. Using these skills and the chip’s design targets, GPT‑5.6 Sol chose measurement parameters, operated the hardware, analyzed the resulting data, and then either refined the measurement or saved the result for use in the next measurement.

When the signals were clear, Codex completed a standard sequence of measurements with little researcher intervention. It identified the qubit’s transition frequencies, calibrated the pulses used to control and read it, and determined how long the qubit retained quantum information.

GPT‑5.6 Sol had more difficulty when experimental signals were weak or noisy. In those cases, it took longer to find suitable measurement parameters and sometimes needed guidance from an experienced researcher. The results suggest that current agents can handle clearly defined experimental workflows, but interpreting ambiguous physical results remains a challenge.

EQuS fabricates many of these standard chips, each of which can take a researcher several days to characterize. The group now regularly uses agents to handle routine measurements, freeing researchers to focus on other work.

“I can have agents running measurements for many hours overnight or while I’m working in the cleanroom,” Yankelevich said. “I can check in from my phone, see what they’ve done, and steer them if something needs fixing or if I want to explore a different direction.”

![Two GPT-5.6 Sol frequency-amplitude calibration screenshots show Rabi and Ramsey calibration results and plots against a pink gradient.](https://images.ctfassets.net/kftzwdyauwt9/Ea5TTufPi7JFjVFfEO87r/d2dbc15bbc050b00f6d80ea70b2908cf/codex_both.png?w=3840&q=90&fm=webp)

*An excerpted GPT‑5.6 Sol chain-of-thought from a calibration run. CREDIT: EQuS group*

## Working alongside researchers

The immediate advantage is that Codex agents can help researchers make steady progress on experimental analysis and measurements without constant supervision. Experienced researchers may still be able to identify the best calibration settings faster than current AI models. But by saving time previously spent on monitoring every step of the calibration process, researchers can focus on other work.

Routine chip characterization follows a relatively well-defined workflow. For novel experiments, Yankelevich assigns Codex agents narrower experimental goals while drawing more heavily on their ability to write, modify, and test new code for control, analysis, and simulation. Connecting agents directly to the lab lets the group revise code, test it against real measurements, and complete longer stretches of work autonomously.

“I’ve built infrastructure to guide agents through several parts of my work—measurement, theory, and chip design—and now it’s really starting to pay off,” Yankelevich said. “I can have multiple agents working on different problems at once, and I spend most of my time on higher-level work—interpreting results, devising experiments, planning next steps for the agents, reading, and writing.”
