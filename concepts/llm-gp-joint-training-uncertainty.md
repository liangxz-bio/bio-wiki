---
title: LLM 嵌入 + GP 联合训练 — 用不确定性反塑表示
created: 2026-09-04
updated: 2026-09-04
type: concept
tags: [bayesian-optimization, llm, gaussian-process, uncertainty-calibration, joint-training, lora, representation-learning]
domain: ai-drug-discovery
sources: [raw/articles/GOLLuM-大模型不确定性校准优化-NatureMachineIntelligence2026.md]
confidence: high
---

# LLM 嵌入 + GP 联合训练 — 用不确定性反塑表示

> 来源：[raw/articles/GOLLuM-大模型不确定性校准优化-NatureMachineIntelligence2026.md](raw/articles/GOLLuM-大模型不确定性校准优化-NatureMachineIntelligence2026.md)（Nature MI 2026，EPFL Schwaller 组）

## 两条路线的缺口

| 方法 | 优势 | 局限 |
|------|------|------|
| **贝叶斯优化（BO）** | 少样本搜索，通过 GP 估计结果与不确定性 | 表示方式门槛高——反应、晶体、连续流工艺需要不同领域特征 |
| **大语言模型（LLM）** | 统一文本入口，直接读取文字描述的条件 | 预训练嵌入不一定适合搜索任务 |

**实测对比**（Buchwald–Hartwig 反应）：固定 LLM 嵌入找到最高产率区域 15%–26%，低于专用反应指纹 DRFP 的 38%。**高产率条件与低产率条件混在表示空间时，GP 难以用少量数据外推**。

## GOLLuM 的核心思路

**把 LLM 嵌入高斯过程（GP），用实验结果和不确定性共同训练模型，让 GP 的边际似然梯度反向更新 LLM 的表示。**

## 关键机制：联合训练

GP 边际似然的梯度同时更新：
- 投影层
- LLM 的低秩适配参数（LoRA）
- 或两者

**效果**：数据拟合、不确定性和表示学习共享一个概率目标——结果相近的实验靠近、结果差异大的实验分开，使搜索空间变平滑。

**量化证据**：GP 长度尺度与样本平均距离之比在 14 种表示上与 BO 表现的相关系数达到 **0.92**。

## 隐式对比学习

联合训练在 Buchwald–Hartwig 基准中把 T5 的高性能条件发现率从 **24% 提高到 43%**。优化迭代后，碘代底物逐渐聚集在高产率区域，活性较低的氯代底物更多落入低性能区域。作者将其解释为**隐式对比学习**——二维 t-SNE 只用于展示，定量分析仍在完整高维嵌入上完成。

## 对其他科学 Agent 的启示

- 把**不确定性从输出端的附加估计变成训练 LLM 表示的信号**
- 在每次实验都昂贵、每轮反馈很少的场景中，与下游决策一致的概率目标可以直接重排模型已有表示
- 减少为每个任务重新构造特征的负担

## 边界

1. **计算回放非前瞻性**：23 项任务均为既有实验数据集的计算回放
2. **GP 可扩展性**：标准 GP 训练复杂度立方增长
3. **纯文本信息损失**：对高维结构对象可能丢失关键几何信息

## 相关页面

- [[entities/gollum]] — GOLLuM 实体页
- [[concepts/bayesian-optimization]] — Bayesian Optimization 框架
- [[entities/boat]] — BOAT 抗体多目标贝叶斯优化框架
