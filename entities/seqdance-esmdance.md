---
title: SeqDance / ESMDance — 基于生物物理动力学的蛋白质语言模型
created: 2026-05-21
updated: 2026-05-21
type: entity
tags: [protein-language-model, protein-dynamics, molecular-dynamics, mutation-effect-prediction, transformer, transfer-learning, zero-shot-prediction]
domain: ai-drug-discovery
sources: [raw/articles/SeqDance-ESMDance-Protein-Dynamics-PNAS-2026.md, raw/papers/hou2026-seqdance-esmdance-pnas.md]
confidence: high
---

# SeqDance / ESMDance
## 蛋白质动力学语言模型，MD 模拟 + Transformer 编码突变效应

## 研究概览

Hou, Zhao & Shen 在 **PNAS 2026** 提出两个蛋白质语言模型（pLM），核心创新在于**使用分子动力学（MD）模拟数据而非进化信息进行预训练**，使模型能预测缺乏同源序列的蛋白质（从头设计、病毒蛋白）的突变效应。^[raw/articles/SeqDance-ESMDance-Protein-Dynamics-PNAS-2026.md]

## 模型架构

### SeqDance（从头训练）

| 组件 | 说明 |
|------|------|
| 预训练数据 | 64,000+ 蛋白质的 MD 模拟 + 正模分析（NMA），包含全原子 MD 和粗粒化 MD |
| 输入 | 单一蛋白质序列（无需 MSA / 同源序列） |
| 编码器 | Transformer，通过自注意力学习残基间动态相互作用和共运动 |
| 预测头 | **残基级别**：RMSF（均方根涨落）、SASA（溶剂可及表面积）、二面角 |
| | **成对级别**：运动相关性（co-movement）、接触频率 |
| 核心能力 | 从序列直接预测动力学属性，无需显式运行 MD 或输入 3D 结构 |

### ESMDance（迁移学习）

| 组件 | 说明 |
|------|------|
| 基座模型 | ESM2（进化信息预训练） |
| 微调数据 | 同 SeqDance 的 MD/NMA 动力学标签 |
| 策略 | **进化信息 + 物理动力学**双模态融合 |
| 核心优势 | 设计蛋白和病毒蛋白突变效应零样本预测大幅超越 ESM2 |

## 预训练性能

| 任务 | 测试集 | 指标 | SeqDance | 对比模型 |
|------|--------|------|----------|----------|
| 残基涨落（RMSF） | IDRs（天然无序区） | Pearson r | **~0.89** | 优于 STARLING（专用构象生成模型） |
| 成对运动相关性 | IDRome（无序蛋白） | 注意力-MD 共运动相关性 | 极高 | 模型自发学会识别"一起移动"的残基对 |
| 回转半径 Rg | IDRs | MAE | 最低 | 显著低于 ESM2 / METL / ProSE |
| 端到端距离 | IDRs | MAE | 最低 | 显著低于 ESM2 / METL / ProSE |

## 突变效应预测（零样本）

SeqDance 通过比较野生型和突变型序列的动态属性差异（尤其是 SASA 和二面角变化）推断突变影响，无需标注数据。

| 任务 | 模型 | Spearman r |
|------|------|-----------|
| 折叠稳定性（ΔΔG） | ESM2（仅进化） | 0.33 |
| | SeqDance（仅动力学） | 0.24 |
| | **ESMDance（进化+动力学）** | **0.46** |
| | | |
| 从头设计蛋白突变效应 | ESM2 | 0.21 |
| | SaProt | 0.06 |
| | **ESMDance** | **0.46** |
| | | |
| 病毒蛋白突变效应 | ESM2-35M | 低 |
| | **ESMDance** | **显著更优** |

## 关键创新

1. **物理信息注入**：以 MD 模拟输出的动态属性（RMSF/SASA/二面角/共运动）作为预训练标签，让 Transformer 从序列中学习物理动力学规律，无需 3D 结构输入
2. **注意力可解释性**：SeqDance 的注意力头与 MD 模拟中的残基相互作用频率、正向共运动高度相关，证明模型自发学会了物理规律
3. **进化-动力学双引擎**：ESMDance 证明了进化信息和物理动力学在突变效应预测上互补——设计蛋白和病毒蛋白场景中进化信息失效时，动力学信息成为主力
4. **计算效率**：训练推理无需运行 MD 模拟（计算成本极高），实现了数量级加速

## 局限性

- ΔΔG 预测 Spearman r 0.46 仍有提升空间，尚不能替代高精度自由能计算方法
- 未直接预测绝对动力学量值，仅做相对变化比较
- 依赖于 MD 模拟训练集的质量和多样性（64,000 蛋白质的覆盖度是另一瓶颈）

## 交叉引用

- [[concepts/transformer]] — SeqDance 使用 Transformer 自注意力编码残基间动态相互作用
- [[entities/isodde]] — IsoDDE 蛋白-小分子/抗体-抗原统一预测（同为 pLM 方向但技术路径不同）
- [[entities/boltz2]] — Boltz-2 蛋白-配体共折叠（结构预测 vs 动力学预测互补方法）
- [[entities/ori-protein-design]] — ORI 腾讯 AI4S 闭环蛋白设计（本体提示生成 + RLWF 实验反馈，与 SeqDance pLM 路线互补）
