---
title: "Boltz-2 — 蛋白质-配体共折叠虚拟筛选"
created: 2026-05-16
updated: 2026-05-16
type: entity
tags: [structure-prediction, virtual-screening, protein-ligand, docking, ai-drug-discovery]
domain: ai-drug-discovery
sources:
  - raw/articles/AI筛药新工具-Boltz2-SoftMol-AINN-P1-DeepClust.md
confidence: medium
---

# Boltz-2

## 核心发现链

Boltz-2 是 Boltz-1 的后续版本，采用蛋白质-配体**共折叠（cofolding）**策略进行虚拟筛选——配体和蛋白同时进入模型，相遇后动态调整构象。在 ULVSH 基准（10 个真实对接项目）上 ROC-AUC 0.70，10 个靶点中成功 7 个。单 GPU 约 100 秒/配体，千级分子一天可完成。核心反直觉发现：**打分准确性与结合模式准确性无直接关联**——模式错了（RMSD > 2Å）但仍能正确区分活性/非活性，富集率提升 4-5 倍。

## 关键证据

- ULVSH 基准 ROC-AUC **0.70**，10 靶点成功 7 个
- 速度：单 GPU ~100 秒/配体
- 反直觉：结合模式（RMSD）与打分（活性排序）可解耦 ^[raw/articles/AI筛药新工具-Boltz2-SoftMol-AINN-P1-DeepClust.md]
- 富集率提升 4-5 倍（超大规模对接后）
- 局限：2 个靶点（CNR1/MTR1A）表现差于随机（ROC-AUC 0.40-0.45）

## 交叉引用

- [[entities/protdbench]] — ProtDBench 评测了 Boltz-1，Boltz-2 可作为后续评测纳入
- [[entities/alphafold3]] — AF3 PoseBusters 76.4%，Boltz-2 为同类共折叠方法但聚焦虚拟筛选场景
- [[entities/isodde]] — IsoDDE 声称蛋白-小分子预测超越 AF3，Boltz-2 提供不同技术路线
- [[entities/pplm]] — PPLM 蛋白配对语言模型，基于序列的 PPI 建模与 Boltz-2 共折叠虚拟筛选互补

## 局限

- 来源仅为微信公众号摘要，原始方法学细节需读原文确认
- 2/10 靶点表现差于随机，提示特定靶点类型存在盲区
