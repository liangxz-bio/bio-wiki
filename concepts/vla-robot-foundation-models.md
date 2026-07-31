---
title: "Vision-Language-Action (VLA) 机器人基础模型"
created: 2026-07-31
updated: 2026-07-31
type: concept
tags: [ai4s, vla, robotics, embodied-ai, vision-language-action, physical-ai, robot-foundation-model, multi-agent]
domain: ai-drug-discovery
sources:
  - raw/articles/晶泰科技-XtalPiScience-GeniusAgents-科学智能开放生态联盟.md
  - https://arxiv.org/abs/2405.14093
  - https://arxiv.org/abs/2606.00054
confidence: high
---

# Vision-Language-Action (VLA) 机器人基础模型

## 直观理解

VLA（Vision-Language-Action）是把「看」「听懂」「动手」三个能力塞进同一个多模态模型：输入是摄像头画面 + 自然语言指令，输出是机器人的动作。它统一了传统机器人学中割裂的感知（Perception）、规划（Planning）、控制（Control）三件套，让一个端到端模型直接完成「看到杯子 → 听懂『把杯子放到桌上』→ 生成机械臂轨迹」的全链路。晶泰科技 Genius Agents 的物理执行端（智能机器人实验室集群）即依赖此类模型完成操作层闭环。

## 数学原理与公式推导

### 1. 问题形式化

VLA 本质是学习一个条件策略（语言条件行为克隆）：

π(a_t | o_{≤t}, l)

其中 o_t 为 t 时刻观测（图像/多视角/本体感觉），l 为语言指令，a_t 为动作（关节角、末端位姿、或离散操作 token）。

### 2. 架构分解

VLA 的通用架构为「视觉编码器 + 语言/多模态主干 + 动作解码器」三段式：

- 视觉编码 E_v: I_t → z^v_t（ViT/CLIP/SigLIP，将图像 token 化）
- 指令编码 E_l: l → z^l（LLM tokenizer）
- 主干 H: [z^v_{≤t}, z^l] → h_t（LLM 或 VLM，自回归生成）
- 动作解码 D: h_t → a_t

### 3. 动作表示（关键设计选择）

动作与文本的统一是 VLA 的核心工程问题，三种主流方案：

| 方案 | 做法 | 代表 | 数学形式 |
|------|------|------|---------|
| 动作 token 化 | 把动作离散成与文本共享词表的 token | RT-2 (2023) | a ∈ 𝒱_ext，扩展词表 |
| 连续离散化 | 每动作维分成 N 个 bins 后分类 | OpenVLA (2024) | a_d = bin(x_d), N=256 |
| 连续生成头 | 在 LLM 输出后接连续分布头 | π0 (2024, Flow Matching) | a ~ p_θ(a | h_t) |

### 4. 训练目标

预训练阶段在互联网级视觉-语言数据上对齐 E_v 与主干（对比/生成损失）；微调阶段在机器人演示数据上做行为克隆：

- 离散动作：交叉熵 L = −Σ_t log π(a_t | o_{≤t}, l)
- 连续动作：MSE 或 Flow Matching 损失（π0：L = E[‖v_θ(x_s, s) − (x₁ − x₀)‖²]，其中 s 为 flow 时间参数）

### 5. 数据规模化与具身差距

VLA 的瓶颈从「模型」转向「数据」：机器人演示数据昂贵且与具体硬件强耦合（embodiment-specific）。2606.00054 综述的系统性路线是用海量人类视频（跨具身、语义丰富）预训练动作语义，再以少量机器人数据适配——即「人类视频学语义，机器人数据学本体」。

## 核心特点

1. **端到端统一**：感知-推理-动作单一网络，无中间模块误差累积
2. **指令泛化**：LLM 语言理解迁移到机器人任务，zero-shot 组合新指令
3. **可扩展**：复用互联网级 VLM 预训练权重，机器人数据只需微调
4. **计算代价高**：推理延迟是最大工程障碍（→ BLURR 等轻量化推理方案）

## 代表模型演进

| 模型 | 机构 | 年份 | 要点 |
|------|------|------|------|
| RT-1 | Google | 2022 | 首个大规模机器人 Transformer |
| RT-2 | Google | 2023 | 动作 token 化，VLM 直出动作 |
| OpenVLA | Stanford | 2024 | 7B 开源，动作离散化 |
| π0 / π0.5 | Physical Intelligence | 2024-25 | Flow Matching 动作头，通用家务操作 |
| Gemini Robotics | Google DeepMind | 2025 | 双子座多模态能力 + 具身 |
| GR00T N1 | NVIDIA | 2025 | 人形机器人基础模型 |
| BLURR | — | 2025 | VLA 轻量推理（KV cache 前缀复用） |

## 关键挑战

- **数据稀缺与具身差距**：演示数据成本高、跨平台迁移难（2405.14093 核心议题）
- **长程任务**：当前 VLA 在小时级长程操作（如完整实验流程）上仍脆弱
- **推理延迟**：生成式动作头逐 token 解码，难以满足高频控制
- **验证与安全**：科学实验中错误操作的代价高，需要世界模型或仿真器兜底

## 与 AI4S 的关联

VLA 是「AI 驱动科学实验自动化」的执行层底座：晶泰的智能机器人实验室集群、Recursion 的自动化表型平台、AstraZeneca iLab 的干湿闭环，其「动手」能力均依赖 VLA 类模型。它与世界模型互补——VLA 负责感知-行动，世界模型负责预测-规划（见 [[world-models]]）。在《AI造出了更好的分子》的 L1-L5 框架中，VLA 对应 L4 细胞表型层（自动化实验执行）与 L5 临床数据的自动化生产，是把「数字假设」变成「物理验证」的最后一公里。

## 交叉引用

- [[world-models]] — 世界模型概念页（预测-规划层，与 VLA 互补）
- [[raw/articles/晶泰科技-XtalPiScience-GeniusAgents-科学智能开放生态联盟]] — 晶泰 XtalPi Science 平台，Genius Agents 物理智能执行端
- [[raw/articles/AI造出了更好的分子却没造出更好的药-黑箱与真值]] — L1-L5 框架中「物理验证」环节的产业缺失分析
- [[esm-world-model]] — 注意区分：蛋白质世界模型（ESM World Model，Biohub）≠ 通用世界模型
