---
title: "IsoDDE — Isomorphic Labs Drug Design Engine"
created: 2026-05-13
updated: 2026-05-13
type: entity
tags: [protein-design, isodde, isomorphic-labs, structure-prediction, antibody, drug-design, ai-drug-discovery]
domain: ai-drug-discovery
sources:
  - raw/papers/isomorphic2026-isodde.md
confidence: high
---

> 来源: Isomorphic Labs Team. Accurate Predictions of Novel Biomolecular Interactions with IsoDDE. Technical Report. 2026.

## 核心发现链

**IsoDDE** (Isomorphic Labs Drug Design Engine) 是 Google DeepMind 衍生公司 Isomorphic Labs 发布的统一生物分子相互作用预测系统。在蛋白-小分子、抗体-抗原、结合亲和力三大任务上全面超越 AlphaFold 3，核心突破在于**对全新化学空间的泛化能力**——在最难的 Runs N' Poses 基准上 cofolding 精度是 AF3 的 2x+。

## 关键证据摘要

### 蛋白-小分子结构预测
- **Runs N' Poses** 基准 lowest similarity bin (0-20]：IsoDDE 成功率 50%，AF3 仅 23.3%（2x+ 提升）
- 成功建模**诱导契合**（induced fit）、**隐蔽口袋开放**（cryptic pocket opening）等 OOD 事件——AF3 完全失败
- 案例：NKG2D 三元复合物（PDB 8EA6）、PolΘ 别构抑制剂（8E23）、CB1 正向别构调节剂（7FEE）

### 抗体-抗原结构预测
- 高保真区间 (DockQ > 0.8)：IsoDDE 39% vs AF3 17% vs Boltz-2 2% — 分别 2.3x 和 19.8x 提升
- CDR-H3 环预测 backbone RMSD ≤ 2Å：IsoDDE 70% vs AF3 58% vs Boltz-2 43%
- 2.9Å 冷冻电镜级精度，在 334 个低同源性抗体测试集上系统验证

### 结合亲和力预测
- ChEMBL 35 时间分割测试集上全面优于 Boltz-2
- 超过 FEP+、OpenFE 等 gold-standard 物理方法（CASP16 盲测）
- 对新化学空间（新蛋白+新化合物系列）泛化性稳健

### 口袋识别
- 优于 P2Rank，能识别非明显口袋（包括隐蔽位点）
- 回顾性验证 CRBN 隐蔽口袋发现（Dippon et al. 2026）

## 与现有工具对比

| 维度 | IsoDDE | AF3 | Boltz-2 |
|------|--------|-----|---------|
| 蛋白-小分子 cofolding (hardest bin) | 50% | 23.3% | N/A |
| 抗体 DockQ > 0.8 | 39% | 17% | 2% |
| CDR-H3 RMSD ≤ 2Å | 70% | 58% | 43% |
| 亲和力预测 | 超越物理方法 | N/A | N/A |
| Isomorphic Labs | ✓ | Google DeepMind | 开源复现 |

## 交叉引用
- [[de-novo-protein-design]] — 蛋白设计方法学背景，IsoDDE 提供预测验证能力
- [[nextgen-vaccine-strategies]] — 抗体设计相关，IsoDDE 抗体-抗原预测可用于疫苗设计

## 交叉引用

- [[entities/gps-ai-drug-platform]] — GPS 从转录表型驱动药物发现（表型驱动），与 IsoDDE 的靶点驱动策略互补
- [[entities/alphafold3]] — AF3 全生物分子统一预测框架，IsoDDE 声称全面超越但缺乏第三方独立验证
- [[raw/articles/Isomorphic-Labs-21亿美元-B轮-AI制药最大融资]] — Isomorphic Labs B轮$21B融资，创AI制药单轮纪录（2026/05）
- [[entities/dual-gpt-ab]] — DualGPT-AB 双阶段 GPT+RL 抗体设计，多性质约束内置，互补 IsoDDE 的预测能力
- [[entities/tcr-prp]] — TCR-PRP 多肽识别谱系统，pLM 预测 T 细胞激活与自身抗原发现，疫苗设计上游免疫信息学工具
- [[entities/pplm]] — PPLM 蛋白配对语言模型，聚焦 PPI 序列建模，与 IsoDDE 的应用方向互补

## 局限与未公开
- 技术报告未提供完整方法学描述（架构细节未公开）
- 仅预览 predictive core，生成能力仅提及但未详细展示
- 训练数据、模型大小、推理成本未披露
