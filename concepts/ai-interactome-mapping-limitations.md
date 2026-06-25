---
title: "AI 互作组预测的边界与价值"
created: 2026-06-10
updated: 2026-06-10
type: concept
tags: [structure-prediction, alphafold, ppi, interactome, protein-interaction, ai-limitations]
domain: ai-drug-discovery
sources:
  - raw/articles/Nature Communications 2026｜AlphaFold 能算出完整互作组吗？这篇实验评估给出答案.md
confidence: high
---

# AI 互作组预测的边界与价值

## 核心结论

AlphaFold / AlphaFold-Multimer / RoseTTAFold 在当前版本下 **不适合作为发现全新蛋白-蛋白相互作用（PPI）的主要手段**，但在 **为实验发现的 PPI 提供结构模型** 上价值显著。

互作组学研究有两个不同任务：

1. **找边（discovery）** — 发现两个蛋白是否互作 → **实验方法（Y2H/GPCA/MAPPIT）领先**
2. **解释边（modeling）** — 描述互作界面的三维结构 → **AI（AF/RF）更具优势**

该结论来自一项系统性实验评估 ^[raw/articles/Nature Communications 2026｜AlphaFold 能算出完整互作组吗？这篇实验评估给出答案.md]，以酵母和人类两个物种的完整互作组为对象，用多种正交实验方法逐对验证。

## 关键数据

### 酵母互作组

| 指标 | 实验方法（YeRI） | AI预测（AF/RF-core） |
|------|-----------------|---------------------|
| 总 PPI 数 | 1,880（异源） | 969 |
| 严格新 PPI | **1,382** | **30**（46x 差距） |
| data: YeRI 覆盖 5,854/5,883 个酵母 ORF，Y2H-v4 三轮 all-by-all + 两轮 pairwise + 测序确认。AF/RF-core 定义：contact probability > 0.95。 |

### 人类互作组

| 指标 | 实验方法（HuRI） | AI预测（AF/RF-core） |
|------|----------------|---------------------|
| 总 PPI 数 | 50,991 | 15,799 |
| 严格新 PPI | **42,633** | **2,869** |
| 已被实验观察 | — | 82%（12,930/15,799） |
| data: AF/RF 从约 2 亿个蛋白对中筛出高置信互作，新增比例仅 ~10%，低于当年几个系统实验图谱的增量。 |

### AI 预测的偏向性

- **偏向大界面互作** — domain-domain 互作恢复率显著高于 domain-motif（短线性基序）互作
- **偏向稳定复合物** — 训练数据中更常见的复合物类型预测更准
- **偏向研究充分的蛋白** — 已知蛋白家族预测精度高，对"暗蛋白组"表现差
- 大量调控型互作界面小、动态强、依赖 IDR 或短线性基序，AF 难以捕捉

## CCC 指标

论文提出 **CCC（confident contacts count，可信接触数）** 作为 AI 预测互作的结构可信度指标：

- **定义**：PAE（预测对齐误差）≤ 4 Å 的残基-残基接触数量
- **优势**：在区分真实互作与随机蛋白对上优于 model confidence、contact probability、pDockQ、iPAE
- **应用**：用 CCC ≥ 5 标准对 YeRI 中 1,637 个无结构模型 PPI 进行建模，**237 个（14.5%）获得高置信结构模型**

## 推荐工作流

```
实验发现（Y2H/GPCA/MAPPIT）
        ↓
 新 PPI 候选列表
        ↓
 AlphaFold2/3 结构建模（CCC ≥ 5 筛选）
        ↓
 高置信结构模型 → 界面分析 / 功能假说 / 药物靶点筛选
```

## 边界

- YeRI 基于 Y2H-v4，Y2H 有其自身的 detection profile — 需要特定修饰、细胞环境或复合物背景的互作可能不被捕获
- 评估基于当前 AF/RF 版本和阈值设定，未来 MSA 策略、训练数据、模型版本变化后结论可能更新
- AF3 的扩散架构对 PPI 预测的影响尚未在此数据集上单独评估

## 交叉引用

- [[entities/alphafold3]] — AF3 架构与蛋白-配体预测能力（AF3 在 PPI 互作组预测上的独立评估尚缺）
- [[entities/pplm]] — PPLM 蛋白配对语言模型做 PPI 的二分类/亲和力/接触预测，属于本文"AI 找边"的补充路径
- [[entities/boltz2]] — Boltz-2 共折叠虚拟筛选，与本文蛋白复合物预测方向相关
- [[entities/de-novo-protein-design]] — 从头蛋白设计依赖 PPI 结构理解，本文提供实验验证背景
- [[concepts/nextgen-vaccine-strategies]] — 疫苗抗原工程中 PPI 界面分析是关键，需理解 AF 的适用边界
