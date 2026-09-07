---
title: GLM-PC 基座模型，CogAgent-9B 开源
link: https://www.zhipuai.cn/en/news/76
source: zhipuai-cn-en-news
published: 2024-12-30T00:00:00Z
first_seen: 2026-09-07T17:03:44.343711062Z
summary: 2024/12/30
content: extracted
html: 2024-12-30-glm-pc-ji-zuo-mo-xing-cogagent-9b-kai-yuan.html
preview:
  file: 2024-12-30-glm-pc-ji-zuo-mo-xing-cogagent-9b-kai-yuan.preview-2965b6fbcc8a.webp
  width: 256
  height: 171
  alt: GLM-PC 基座模型，CogAgent-9B 开源
  color: '#3d3d3d'
images:
- source: https://www.zhipuai.cn/_next/image?url=%2Fapi%2Fmedia%2Ffile%2F%25E6%25A8%25A1%25E5%259E%258B%25E7%25B1%25BB-7.png&w=3840&q=75
  original:
    file: 2024-12-30-glm-pc-ji-zuo-mo-xing-cogagent-9b-kai-yuan.image-aad826ddf2a3.png
    width: 822
    height: 548
  variants:
  - file: 2024-12-30-glm-pc-ji-zuo-mo-xing-cogagent-9b-kai-yuan.image-885378f47ef2.webp
    width: 48
    height: 32
  color: '#272727'
---

![Image](https://zhipu-ai.feishu.cn/space/api/box/stream/download/asynccode/?code=MGM3M2VlYWQyYzI2NjFlMGQ0YmMzYTg4NjZjNGI0NWVfUUMyd0M0ODlBenJvdTM2OXRwbElrTXFBVUhmTG5tZ09fVG9rZW46QzlHSmJ4Mzk0b2pZanl4MmZ6YWN2dDJObkVoXzE3MzU1NTUzOTE6MTczNTU1ODk5MV9WNA)

11 月 29 日，智谱正式提出 GLM-OS 概念，并发布 AutoGLM 和 GLM-PC 两款 Agent 产品。为推动大模型 Agent 生态发展，我们决定将 GLM-PC 的基座模型——CogAgent-9B 开源，供社区进一步开发。

CogAgent-9B-20241220 是基于 GLM-4V-9B 训练而成的专用 Agent 任务模型。 该模型仅需屏幕截图作为输入（无需 HTML 等文本表征），便能根据用户指定的任意任务，结合历史操作，预测下一步的 GUI 操作。 得益于屏幕截图和 GUI 操作的普适性，CogAgent 可广泛应用于各类基于 GUI 交互的场景，如个人电脑、手机、车机设备等。

相较于 2023 年 12 月开源的第一版 CogAgent 模型，CogAgent-9B-20241220 在 GUI 感知、推理预测准确性、动作空间完善性、任务普适性和泛化性等方面均实现了显著提升，并支持中英文双语的屏幕截图和语言交互。

## CogAgent-9B

论文：

- https://arxiv.org/abs/2312.08914

代码：

- https://github.com/THUDM/CogAgent

模型：

- Huggingface: https://huggingface.co/THUDM/cogagent-9b-20241220
- 魔搭社区：https://modelscope.cn/models/ZhipuAI/cogagent-9b-20241220（方便下载）
- 魔乐社区：https://modelers.cn/models/zhipuai/cogagent-9b-20241220（适配昇腾卡）

技术文档：

- https://cogagent.aminer.cn/blog#/articles/cogagent-9b-20241220-technical-report

## 执行过程

![Image](https://zhipu-ai.feishu.cn/space/api/box/stream/download/asynccode/?code=NzA3N2M2NDgxYjUzZWIzOGRlZGVkZjA4NWYzNWZhMzNfN1FjSVlCYUJqUlZiMVhnZVZDTTY2cElGTVZaZkpYRm1fVG9rZW46QWZZQmJRNTRpb2cyMnp4TzV6WGNsOUxXbktmXzE3MzU1NTUzOTE6MTczNTU1ODk5MV9WNA)

CogAgent 以 GUI 截图为唯一环境输入，结合已完成的动作历史，计算当前 GUI 截图中最合适的动作。

该动作通过 CogAgent 端侧应用（如 GLM-PC 和 CogAgent Demo App）注入 GUI，GUI 响应并更新图像内容；

同时，该动作被添加至动作历史。CogAgent 根据更新后的历史动作和截图，计算后续操作。 此过程循环往复，直至 CogAgent 判定指令执行完毕。

CogAgent 的输入仅包含三部分：用户的自然语言指令、已执行历史动作记录和 GUI 截图，无需任何文本形式表征的布局信息或附加元素标签（set of marks）信息。

其输出涵盖以下四个方面：

- 思考过程（Status & Plan）： CogAgent 显式输出理解 GUI 截图和决定下一步操作的思考过程，包括状态（Status）和计划（Plan）两部分，输出内容可通过参数控制。
- 下一步动作的自然语言描述（Action）： 自然语言形式的动作描述将被加入历史操作记录，便于模型理解已执行的动作步骤。
- 下一步动作的结构化描述（Grounded Operation）： CogAgent 以类似函数调用的形式，结构化地描述下一步操作及其参数，便于端侧应用解析并执行模型输出。其动作空间包含 GUI 操作（基础动作，如左键单击、文本输入等）和拟人行为（高级动作，如应用启动、调用语言模型等）两类。
- 下一步动作的敏感性判断： 动作分为“一般操作”和“敏感操作”两类，后者指可能带来难以挽回后果的动作，例如在“发送邮件”任务中点击“发送”按钮。

## 模型升级

![Image](https://zhipu-ai.feishu.cn/space/api/box/stream/download/asynccode/?code=Njg0MTFjYjdkYmYwNTg2MjM2YmRhZTFiNGExZjRmNTBfMmRTenAxNkRHbXFtOVlCVTNFZkxIak1qT2gyMVhpSkpfVG9rZW46UGlFdWJSZkpDb1ZGbTF4a3NsY2NlNkhkbnNiXzE3MzU1NTUzOTE6MTczNTU1ODk5MV9WNA)

模型基座与结构升级： 我们采用了更强大的视觉语言模型 GLM-4V-9B 作为基座 ，显著提升了模型的图像理解性能。

视觉处理模块优化： 实现了更高效、统一的视觉处理模块， 支持 1120\*1120 原生高分辨率图像输入 。通过带参数的下采样方法，在几乎不损失模型能力的前提下，提高了模型效率。CogAgent 在结构上支持任意比例或大小的图像输入，但在训练和推理中，统一将输入图像缩放到 1120\*1120。尽管输入图像尺寸固定，测试结果显示，即使在 2K 或更高分辨率的屏幕上，模型仍能保持准确的理解能力。为获得更佳表现，建议用户适当增加图标和文字的相对大小，确保缩放后截图中的内容清晰可辨。

数据集丰富与完善： 广泛收集并整合了多种数据集， 包括无监督数据和 GUI 指令微调数据集 。无监督数据涵盖开源 GUI 布局数据集、自采集的应用和网页数据集；GUI 指令微调数据集则包含更长链路、更多应用、跨应用的 GUI agent 任务数据集等。此外，利用 CogAgent 自我生成数据，进一步扩充和完善了数据集。

预训练策略优化： VLM 和 GUI 预训练旨在提升模型对视觉输入和 GUI 界面的基础理解能力。我们 首次提出了 GUI Grounding 预训练 ，利用屏幕截图和布局对，构建界面子区域与布局表征（如 DOM 元素）的对应关系，从而构造 GUI 的 REG 和 REC 任务：

- GUI Referring Expression Generation （REG）：预测截图上某一区域对应的布局表征。
- GUI Referring Expression Comprehension （REC）：预测截图中某一元素对应的位置。此方法已应用于多个 GUI 理解数据构造和 GUI agent 工作中。在原论文中，我们使用 40 万网页数据，构造了 1.4 亿的 REC & REG 训练样本。在此基础上，进一步扩充和优化了训练数据，加入了桌面应用和移动应用的布局数据，使模型更适应实际应用场景。

后训练策略改进： 后训练在提升模型 GUI agent 分析、推理、预测能力方面至关重要。我们采用了更科学的后训练策略，分为两个难度递进的阶段：

- GUI instruction tuning：融合 GUI 相关多任务数据，深化模型对 GUI 内容和功能的理解，具备初步问答能力。使用了广泛的开源数据和私有收集数据。
- GUI agent SFT：使模型具备完善的 GUI agent 推理能力，训练数据包括开源数据集（如 Mind2Web）和额外收集的多平台跨应用数据。

模型推理及思维链优化： 将思维链分解为 Status（当前屏幕状态）、Plan（全局计划）、Action（下一步自然语言描述）、Operation（下一步形式语言描述） 。通过随机采样混合多种模式训练数据（如 Action-Operation、Status-Action-Operation 等），可根据交互情景、计算资源和准确率需求灵活调整和控制推理过程中的实际输出。

动作空间完善： 明确了基础动作空间 ，并新增了 LLM、QUOTE\_TEXT、LAUNCH 等高级动作，增强了模型的使用工具和交互能力。

## 评测结果

我们在以下四个数据集中测试了 CogAgent-9B-20241220 和类似模型的性能。

![Image](https://zhipu-ai.feishu.cn/space/api/box/stream/download/asynccode/?code=MTU3N2I1ZWQxOGJhZjk1YmRkMjBhOWQ1NTliN2Y2OTFfbmpGNlhSOWRmOTUwcmNWd1MxaldKM3dCQVRTdjcweGJfVG9rZW46RFRScWJlQ0Vxb2dxaWV4ZDF0OGNPc3NObnllXzE3MzU1NTUzOTE6MTczNTU1ODk5MV9WNA)

我们对比了基于 API 的商业模型（GPT-4o-20240806、Claude-3.5-Sonnet）、

商业 API + GUI Grounding 模型（GPT-4o + UGround、GPT-4o + OS-ATLAS）、

开源 GUI Agent 模型（Qwen2-VL、ShowUI、SeeClick）。

结果表明，CogAgent 在 GUI 定位（Screenspot）、单步操作（OmniAct）、中文 step-wise 榜单（CogAgentBench-basic-cn）、多步操作（OSWorld）都取得了领先的结果，仅在 OSworld 上略逊于针对 Computer Use 特化的 Claude-3.5-Sonnet 和结合外接 GUI Grounding Model 的 GPT-4o。
