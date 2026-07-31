---
title: "世界模型（World Models）：从 Dreamer 到 Sora 与机器人学习"
created: 2026-07-31
updated: 2026-07-31
type: concept
tags: [ai4s, world-model, model-based-rl, robotics, sora, jepa, physical-ai, simulation, digital-twin]
domain: ai-drug-discovery
sources:
  - raw/articles/晶泰科技-XtalPiScience-GeniusAgents-科学智能开放生态联盟.md
  - https://arxiv.org/abs/2405.03520
  - https://arxiv.org/abs/2605.00080
confidence: high
---

# 世界模型（World Models）

## 直观理解

世界模型是智能体对「环境如何随动作演化」的内部模拟器——大脑中的物理引擎。给定当前状态与一个动作，它预测下一刻会发生什么（状态、观测、奖励），从而让智能体在「想象」中规划、学习，而不必在真实世界反复试错。晶泰科技 XtalPi Science 的「数字假设—专业预测—物理验证—数据反馈」闭环，其「数字假设」与「专业预测」两端正由世界模型承担：先在虚拟空间中推演反应路径与分子行为，再交给机器人实验室物理验证。

## 数学原理与公式推导

### 1. 形式化：世界模型学习什么

设环境为 MDP (S, A, P, R)，世界模型学习真实转移函数 P 与观测模型 g 的近似：

P̂(s_{t+1} | s_t, a_t) ≈ P(s_{t+1} | s_t, a_t)
o_t = g(s_t)（观测生成）

### 2. 潜在空间世界模型：RSSM 与 Dreamer 系列

Dreamer（Hafner 系列）用循环状态空间模型（Recurrent State-Space Model, RSSM）在潜在空间压缩历史：

z_{t+1} ~ p(z_{t+1} | z_t, a_t)（转移先验）
ẑ_{t+1} ~ q(ẑ_{t+1} | z_t, a_t, o_{t+1})（后验，观测校正）
o_t ~ p(o_t | z_t)（重建）

训练目标为序列变分自编码器的 ELBO：

L = Σ_t [ E_q[log p(o_t | z_t)] − β·KL(q(ẑ_t | z_{≤t}, o_{≤t}) ‖ p(z_t | z_{<t}, a_{<t})) ]

关键技巧：世界模型在潜在空间做想象 rollout，策略 π(a_t | z_t) 在「梦境」中训练，因此数据效率远超 model-free RL。

### 3. 生成式（像素级）世界模型：Sora 路线

以 Sora 为代表的视频生成世界模型直接学习观测序列的联合分布：

p(x_{1:T} | x_{1:t}, a, l)（扩散/自回归生成未来帧）

其价值在于：视觉训练数据的丰富性（互联网视频）让模型隐式学习物理规律（重力、遮挡、材料行为），2405.03520 综述的核心问题即「Sora 是否真能模拟物理世界」——结论为：初现端倪，但非可靠物理引擎。

### 4. 联合嵌入预测架构：JEPA（LeCun 路线）

JEPA 类模型（I-JEPA/V-JEPA）放弃像素级重建，改为在表示空间预测：

L = ‖Enc(x_t) − Pred(Enc(x_{t+1}))‖²（能量/对比目标）

「预测表示而非像素」：丢弃不可预测的细节（纹理、光照），保留任务相关的抽象（物体状态、位置、动力学），计算效率与抽象能力兼得。

### 5. 世界模型 × 控制

- **Model-based RL**：Dreamer 在潜在空间想象 rollouts 训练策略
- **MPC/规划**：用世界模型做模型预测控制（如 MPPI：在动作分布上采样、用世界模型评估、加权平均）
- **仿真与数据生成**：NVIDIA UniSim 用可交互 3D 世界模型合成机器人训练数据（Sim2Real）

## 核心特点

1. **数据效率**：在想象中学习，减少真实交互（2605.00080 视角的核心价值）
2. **规划能力**：前瞻多步后果，支持长程任务分解
3. **物理规律隐式学习**：从视频数据中涌现（Sora 效应）
4. **不确定性建模**：分布输出可量化「哪些预测可信」（→ 可信 AI4S 的数学基础）

## 代表工作

| 工作 | 机构 | 年份 | 要点 |
|------|------|------|------|
| World Models | Ha & Schmidhuber | 2018 | 开创性：MDN-RNN + 进化策略控制 |
| Dreamer v1-v3 | DeepMind | 2020-23 | 潜在空间想象，model-based RL 标杆 |
| Sora | OpenAI | 2024 | 视频生成世界模型，物理模拟初现 |
| Genie / Genie 2 | Google DeepMind | 2024 | 可交互环境生成 |
| I-JEPA / V-JEPA | Meta | 2023-24 | 联合嵌入预测，非像素路线 |
| UniSim | NVIDIA | 2023 | 可交互 3D 世界模型，机器人数据合成 |
| GR00T | NVIDIA | 2025 | 人形机器人世界模型 + 策略 |

## 在机器人学习中的作用（2605.00080 框架）

1. **策略学习**：想象 rollout 训练（Dreamer 范式）
2. **规划**：MPC 前瞻搜索动作序列
3. **仿真评估**：在虚拟世界预演（降低物理实验成本）
4. **数据生成**：合成多样化训练场景（UniSim）
5. **安全验证**：危险动作先在仿真中淘汰（科学实验尤其重要——错误操作的湿实验代价极高）

## 与 AI4S 的关联

世界模型是 AI4S「数字-物理闭环」的预测引擎：在晶泰 XtalPi Science 中，「数字假设」由领域模型生成，「专业预测」由世界模型在虚拟空间推演反应/分子/材料行为，物理验证由机器人实验室执行，数据反馈再校正世界模型——形成完整自举飞轮。在《AI造出了更好的分子》的因果框架中，世界模型正是「推理 AI」的干预层能力（P(Y | do(X)) 的模拟器）：它把「阻断这个靶点会怎样」从文献统计（关联层）推进到机制推演（干预层），是解决 Phase II 瓶颈的候选技术路径之一。

## 交叉引用

- [[vla-robot-foundation-models]] — VLA 概念页（感知-行动层，与世界模型互补）
- [[raw/articles/晶泰科技-XtalPiScience-GeniusAgents-科学智能开放生态联盟]] — XtalPi Science 数字-物理闭环
- [[raw/articles/AI造出了更好的分子却没造出更好的药-黑箱与真值]] — 推理 AI 与因果之梯框架
- [[esm-world-model]] — 注意区分：蛋白质世界模型（ESM World Model，Biohub）为领域特化世界模型，与通用世界模型不同域
