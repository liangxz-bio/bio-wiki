---
title: GOLLuM — LLM 不确定性校准的实验条件优化器
created: 2026-09-04
updated: 2026-09-04
type: entity
tags: [bayesian-optimization, llm, gaussian-process, uncertainty-calibration, lora, experimental-design, schwaller]
domain: ai-drug-discovery
sources: [raw/articles/GOLLuM-大模型不确定性校准优化-NatureMachineIntelligence2026.md]
confidence: high
---

# GOLLuM — LLM 不确定性校准的实验条件优化器

Ranković B, Griffiths R-R, Schwaller P. *Large language models as uncertainty-calibrated optimizers for experimental discovery*. Nature Machine Intelligence (2026). doi: 10.1038/s42256-026-01283-z.

EPFL + NCCR Catalysis + QuantumGreen（Schwaller 组）。把 LLM 嵌入高斯过程（GP），用实验结果和不确定性共同训练模型，**让 GP 的边际似然梯度反向更新 LLM 的表示**。

## 核心架构

```
反应物 + 添加剂 + 温度 + 流速等条件
        ↓
  标准化文本描述
        ↓
  LLM（T5）编码 → 向量表示
        ↓
  GP 学习 → 预测目标值分布 + 不确定性
        ↓
  采集函数（EI/PI/UCB）→ 选择下一个实验点
        ↓
  执行实验 → 获得结果 → 反馈给 GP
        ↓
  边际似然梯度 → 反向更新 LLM 表示（联合训练）
```

## 关键创新：联合训练

GP 边际似然的梯度同时更新投影层、LLM 的低秩适配参数（LoRA）或两者，使**数据拟合、不确定性和表示学习共享一个概率目标**。

联合训练会让结果相近的实验靠近、结果差异大的实验分开，使搜索空间变平滑。GP 长度尺度与样本平均距离之比在 14 种表示上与 BO 表现的相关系数达到 **0.92**。

## 23 项任务核心结果

每次运行从 10 个低于中位数的实验开始，总预算 50 次实验，主要结果使用 20 个独立随机种子。

### top-5% 覆盖率对比

| 方法 | top-5% 覆盖率 |
|------|--------------|
| **GOLLuM（PLLMϕ + T5）** | **36.3%** |
| 传统 GP（领域定制表示） | 29.7% |
| 固定 LLM 嵌入 + GP（BoChemian） | 26.5% |
| LAPEFT | 12.1% |

### 收益分布

| 任务类型 | 覆盖率提升 |
|----------|-----------|
| 过程化学（缺少成熟描述符） | 13.4% → **25.4%** |
| 混合变量任务 | 提升 **44%** |
| 已有成熟指纹的任务 | 提升约 **15%** |

### 样本效率

GOLLuM 以少约 **41%** 的迭代达到传统 GP 的最终表现。

### Buchwald–Hartwig 反应

联合训练把 T5 的高性能条件发现率从 **24% 提高到 43%**。

## 直接提示 LLM 为什么不够

作者让 GPT-5、Claude、Gemini 和 Qwen 系列模型直接充当反应优化器：

- 不同模型的失败率约 **10%–80%**
- 失败类型：虚构反应物、提出参数空间外条件、重复已有实验、输出无法解析、提前终止优化
- 即使系统持续纠错，直接提示法的 top-5% 覆盖率仍低于传统 BO 和 GOLLuM

GOLLuM 由 LLM 编码实验条件，GP 负责概率预测、置信度和采集决策，使下一次实验受**可量化的不确定性约束**。

## 边界与局限

1. **计算回放非前瞻性实验**：23 项任务均为既有实验数据集上的计算回放——"减少 41% 实验"是基准样本效率，不能直接等同于真实实验室节省同等时间或成本
2. **GP 可扩展性**：标准 GP 训练复杂度随样本量立方增长；更适合低数据场景
3. **纯文本信息损失**：对蛋白构象、晶体三维结构等高维对象，纯文本可能丢失关键几何信息

## 代码与数据

- GitHub: https://github.com/schwallergroup/gollum
- DOI: 10.1038/s42256-026-01283-z

## 相关页面

- [[raw/articles/GOLLuM-大模型不确定性校准优化-NatureMachineIntelligence2026]] — 来源原文（中文解读）
- [[raw/articles/DynaMate-AI-Agent蛋白配体MD模拟-ResearchSquare2026]] — 同一团队 DynaMate
- [[concepts/bayesian-optimization]] — Bayesian Optimization（GP/EI/PI/UCB 框架）
