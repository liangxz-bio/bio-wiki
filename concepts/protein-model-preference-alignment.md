---
title: 蛋白质生成模型的偏好对齐 — 从 LLM 到 ProteinDPO
created: 2026-09-04
updated: 2026-09-04
type: concept
tags: [protein-language-model, direct-preference-optimization, alignment, structure-conditioned-generation, fitness-landscape, bio-alignment]
domain: ai-drug-discovery
sources: [raw/papers/widatalla2026-proteindpo-naturemethods.md]
confidence: high
---

# 蛋白质生成模型的偏好对齐 — 从 LLM 到 ProteinDPO

> 来源：[raw/papers/widatalla2026-proteindpo-naturemethods.md](raw/papers/widatalla2026-proteindpo-naturemethods.md)（Nature Methods 2026，Widatalla et al.）

## 核心思路：从 LLM 偏好对齐到蛋白偏好对齐

| 领域 | Prompt | Response | Preference |
|------|--------|----------|------------|
| LLM（GPT） | 文本 prompt | 文本回答 | 人类偏好标签 |
| **蛋白生成** | **蛋白 backbone** | **序列** | **实验稳定性测量** |

## alignment gap 与 SFT 的局限

- 无监督预训练的蛋白语言模型（ESM-IF1）能零样本预测稳定性，但落后于监督模型（ThermoMPNN）
- 这是因为进化不只优化稳定性，模型学的规则与"稳定性"目标不对齐
- SFT 只最大化正样本似然，容易过拟合并丢失预训练的一般知识

## DPO 的核心优势

用偏好对（正/负）而非单标签——鼓励模型在"看起来相似但实验偏好不同"的样本之间学会区分，**避免 SFT 的过拟合问题**。

## 三种 DPO 训练目标

1. **paired**：偏好对（正/负）
2. **ranked**：偏好排序
3. **weighted**：**直接利用连续标量数据（如 ΔΔG）**——本文新提出

## 关键启示

1. **保留预训练 + 注入任务信息**：DPO 不破坏 ESM-IF1 的结构条件化生成能力
2. **跨任务泛化**：仅用单突变训练，整序列推理能捕捉多突变非线性效应；ThermoMPNN 受限于"加性"假设
3. **快速响应新兴病原体**：45 个设计就找到 27 个稳定变体，Tm +17-32°C

## 对生物基础模型的启示

- 提供了一个**通用的偏好对齐框架**——为生成模型注入实验适应度信息
- 比 SFT 更不易过拟合，比 RLHF 更轻量
- 适用于任何"实验可测、生成模型有 prompt-response 结构"的生物任务

## 相关页面

- [[entities/proteindpo]] — ProteinDPO 实体页
- [[raw/papers/widatalla2026-proteindpo-naturemethods.md]] — 原文
- [[entities/vesm]] — VESM（ESM 家族知识蒸馏）
- [[entities/random-neighbor-score]] — RNS（embedding 不确定性）
