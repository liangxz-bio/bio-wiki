---
title: "PPLM — 蛋白配对语言模型（Protein Pair Language Model）"
created: 2026-05-19
updated: 2026-05-19
type: entity
tags: [protein-language-model, ppi, protein-protein-interaction, binding-affinity, contact-prediction, ai-drug-discovery]
domain: ai-drug-discovery
sources:
  - raw/articles/PPLM-蛋白配对语言模型-PPI预测-NatureComms2026.md
confidence: high
---

## 核心发现链

PPLM 把蛋白语言模型的基本单元从"单条蛋白链"推进到"蛋白对"，让模型在预训练阶段同时学习链内关系和链间关系，使后续的 PPI 二分类、亲和力预测、链间接触预测共享同一个表示底座。^[raw/articles/PPLM-蛋白配对语言模型-PPI预测-NatureComms2026.md]

核心架构创新：hybrid intra-/inter-protein attention（链内/链间混合注意力）— 链内残基用 RoPE（旋转位置编码），链间残基去掉位置编码，避免把序列距离误当成空间先验。训练数据整合 PDB 复合物和 STRING 相互作用数据，构建超过 330 万对蛋白序列。^[raw/articles/PPLM-蛋白配对语言模型-PPI预测-NatureComms2026.md]

基于 PPLM 底座，作者开发了三个下游模型：
- **PPLM-PPI** — PPI 二分类
- **PPLM-Affinity** — 结合自由能（ΔG）预测
- **PPLM-Contact** / **PPLM-Contact2** — 链间接触图与界面残基识别

## 关键证据摘要

- **困惑度**：PPLM 在同源二聚体/异源二聚体/STRING 蛋白对上平均困惑度 7.30/5.08/4.50，全面低于 ESM2（8.40/6.73/5.70）。界面残基 mask 场景差距更显著（Dual mode：PPLM 6.79 vs ESM2 8.50）^[raw/articles/PPLM-蛋白配对语言模型-PPI预测-NatureComms2026.md]
- **PPI 跨物种预测**：在 M. musculus 等五个物种上 AUPRC 0.745–0.920，相比第二名 TUnA 提升 4.0%–17.6%，F1-score 全部最高 ^[raw/articles/PPLM-蛋白配对语言模型-PPI预测-NatureComms2026.md]
- **结合亲和力**：PPLM-Affinity 的 PCC 0.643 / SRCC 0.636，高于 ESM2-Affinity 和结构方法 PPB-Affinity ^[raw/articles/PPLM-蛋白配对语言模型-PPI预测-NatureComms2026.md]
- **链间接触精度**：PPLM-Contact 在 Homodimer300 top L 接触精度 77.8%（实验单体结构）/ 66.6%（AF2 预测单体结构）。Heterodimer99 对应 48.9%/45.1%，均超 DeepInter/CDPred/GLINTER ^[raw/articles/PPLM-蛋白配对语言模型-PPI预测-NatureComms2026.md]
- **PPLM-Contact2**：融合 AF2.3/AF3/DMFold 复合物链间距离图，同源二聚体 top L 精度从 65.0%→85.1%，异源二聚体从 44.3%→88.0% ^[raw/articles/PPLM-蛋白配对语言模型-PPI预测-NatureComms2026.md]

## 与现有工具对比

| 维度 | PPLM | ESM2 | TUnA | PPB-Affinity |
|------|------|------|------|-------------|
| 建模对象 | 蛋白对（双链一起读） | 单链 | 单链+交互模块 | 结构方法 |
| PPI AUPRC（五物种均值） | 0.848 | — | 0.786 | — |
| 亲和力 PCC | 0.643 | <0.643 | — | <0.643 |
| 链间接触 | 77.8% (Homodimer) | — | — | — |
| 结构依赖 | 可选（MSA/单体结构增强） | 无 | — | 必须 |

## 交叉引用

- [[entities/alphafold3]] — AF 系列为 PPLM-Contact2 提供复合物结构先验，两类信息互补提升接触精度
- [[entities/de-novo-protein-design]] — PPLM 的蛋白对表示可为 binder 设计中 PPI 评估提供底座
- [[entities/isodde]] — IsoDDE 提供蛋白-小分子/抗体-抗原统一预测，PPLM 聚焦 PPI 序列建模，应用方向互补
- [[entities/protenix-v2]] — Protenix-v2 聚焦结构预测+抗体设计，PPLM 提供基于序列的 PPI 表示
- [[entities/boltz2]] — Boltz-2 共折叠虚拟筛选与 PPLM-PPI 均涉及蛋白复合物建模
- [[entities/tcr-prp]] — TCR-PRP 使用 pLM 预测 T 细胞激活，与 PPLM 共享蛋白语言模型基础
- [[concepts/ai-interactome-mapping-limitations]] — AI 互作组预测边界评估：PPLM 属于"AI 找边"路径，与实验方法的发现效率对比见该概念页

## 局限与未公开

- 瞬时相互作用、弱相互作用、条件依赖相互作用在训练数据中仍然不足 ^[raw/articles/PPLM-蛋白配对语言模型-PPI预测-NatureComms2026.md]
- 纳米抗体-抗原复合物界面小、loop 介导、共进化信号弱，PPLM 特征仍难以完整恢复界面 ^[raw/articles/PPLM-蛋白配对语言模型-PPI预测-NatureComms2026.md]
- PPLM-Contact 对 MSA 和结构特征仍有依赖；PPLM-Contact2 需要复合物结构预测结果 ^[raw/articles/PPLM-蛋白配对语言模型-PPI预测-NatureComms2026.md]
- 低同源蛋白、柔性界面、构象变化明显的体系预测需谨慎解读 ^[raw/articles/PPLM-蛋白配对语言模型-PPI预测-NatureComms2026.md]
