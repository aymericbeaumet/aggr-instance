---
title: 'Reimagining Independence: How Meta’s AI Models Are Helping the University of Pittsburgh Transform Assistive Robotics'
link: https://ai.meta.com/blog/assistive-robotics-university-of-pittsburgh-sam-dino/
source: ai-meta-com-blog
published: 2026-07-27T00:00:00Z
first_seen: 2026-09-08T10:17:10.166315Z
content: extracted
html: 2026-07-27-reimagining-independence-how-meta-s-ai-models-are-helping.html
preview:
  file: 2026-07-27-reimagining-independence-how-meta-s-ai-models-are-helping.preview-63f50fdf0e40.webp
  width: 256
  height: 144
  color: '#1e262a'
images:
- source: https://scontent-cdg6-1.xx.fbcdn.net/v/t39.2365-6/753694960_2219191542190533_6830137062160879015_n.png?_nc_cat=105&ccb=1-7&_nc_sid=e280be&_nc_ohc=5U6MueMoJiEQ7kNvwGxDBs1&_nc_oc=AdqUPyBysDncyphpisZBotDfK_rU71QwYHcQS989_m1FykukNjEfD5Q2BtZT9W2mUXY&_nc_zt=14&_nc_ht=scontent-cdg6-1.xx&_nc_gid=xF1z7kCcmtnwNclj-Doiow&_nc_ss=7b20f&oh=00_AQKPIVAiMD9WcRQBSn00M7vk3pZvj6VOprZzuS2PF3d6-A&oe=6ABA2B20
  original:
    file: 2026-07-27-reimagining-independence-how-meta-s-ai-models-are-helping.image-8cab7cb2de0e.png
    width: 3840
    height: 2160
  variants:
  - file: 2026-07-27-reimagining-independence-how-meta-s-ai-models-are-helping.image-c90ac5dd6637.webp
    width: 48
    height: 27
  - file: 2026-07-27-reimagining-independence-how-meta-s-ai-models-are-helping.image-ad71a50b4037.webp
    width: 320
    height: 180
  - file: 2026-07-27-reimagining-independence-how-meta-s-ai-models-are-helping.image-0071b11727c7.webp
    width: 640
    height: 360
  - file: 2026-07-27-reimagining-independence-how-meta-s-ai-models-are-helping.image-961371414bbe.webp
    width: 960
    height: 540
  - file: 2026-07-27-reimagining-independence-how-meta-s-ai-models-are-helping.image-b34a5c4efc06.webp
    width: 1280
    height: 720
  - file: 2026-07-27-reimagining-independence-how-meta-s-ai-models-are-helping.image-93e007b5e291.webp
    width: 1600
    height: 900
  - file: 2026-07-27-reimagining-independence-how-meta-s-ai-models-are-helping.image-fdd25ff636b6.webp
    width: 3840
    height: 2160
  color: '#030303'
---

For people relying on assistive devices, every second counts. The unpredictable nature of everyday environments, such as a child darting across a sidewalk, the sudden appearance of a curb, or a dropped set of keys, requires an immediate reaction. Processing camera images and sensor data directly on the device, known as edge computing, empowers robotic mobility platforms to function as responsive tools. These tools must be robust and consistent across the wide range of dynamic environments in which people live.

At the same time, deploying powerful AI models like DINOv3 and SAM on limited, battery-powered hardware presents significant engineering challenges. Real-world deployments must consider practical factors, including battery life, heat dissipation, unreliable network connectivity, and strict size and weight requirements.

However, overcoming these constraints mean nothing to the end user if they can't perform their activities of daily living using these new robotic systems. These newer methods allow users to interact with the robot more naturally, using their immediate surroundings as context, freeing both engineers and users from having to design and navigate complex, time-consuming interfaces. The ability to use natural language combined with image data to query the user’s and robot’s environment and provide more direct commands directly reduces the cognitive load and amount of context switching required for a user to do something as simple as picking up a cup off a table.

This functionality is already being integrated by the RAMMP team into their first prototype. Leveraging tools built off of DINO to enable querying the robot’s image sensors to detect automatic door buttons, cups, and curbs/ground for navigation assistance. With this functionality now ready for real-world testing, engineers are focusing on voice and touch input, letting users select and interact with specific objects in their surroundings. In addition to the existing challenges of ensuring accuracy and temporal coherence of the model outputs, this provides the additional challenge of ensuring robustness and predictability across user prompts and inputs.

DINOv3 serves as a compact, efficient 'visual brain' for devices — a general-purpose foundation on which task-specific, lightweight modules can be layered for actions such as object detection or movement tracking, enabling reuse of visual data and conserving power.

Applying both models as part of the development of robotics systems, engineers optimize models for edge devices, reducing memory footprint, using lower precision when appropriate, and deploying in formats tailored for real-world conditions. This ensures reliable, real-time operation for users. By running at practical resolutions and with efficient batching, both models stay fast and dependable, even on the compact, battery-powered hardware used in robotic mobility platforms and robotic arms, — sometimes trading a little bit of boundary precision and/or feature detail for the speed and stability needed by users on the go. This balance between precision and practicality is central to the project's philosophy.

“For assistive robotics, performance is not measured by benchmark accuracy alone, but by whether a system can operate reliably in the unpredictability of everyday life," said Sivashankar Sivakanthan, Chief of Staff to the RAMMP project. “Running models like DINOv3 and SAM on-device is what enables real-time perception that users can trust - without relying on connectivity or compromising safety.”

RAMMP's perception system is built on RF-DETR, a lightweight detection model fine-tuned with DINOv2 embeddings. Training data is auto-labeled using SAM, enabling the team to rapidly generate high-quality annotations across the full range of angles, heights, backgrounds, and lighting situations that assistive devices encounter in the real world. Data augmentations and multi-view strategies further enforce consistency across perspectives. The result is a system that is smart, adaptive, and offers safer and more confident mobility.

By combining SAM's labeling power with DINOv2's rich visual representations in a fine-tuned RF-DETR model, RAMMP achieves real-time 360-degree environmental awareness and adaptive object detection.
