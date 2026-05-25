---
title: "TarPass — 靶点感知分子生成评测基准"
created: 2026-05-18
updated: 2026-05-18
type: entity
tags: [benchmark, molecular-generation, target-aware, drug-design, AI-drug-discovery]
domain: ai-drug-discovery
sources:
  - raw/articles/AI造药新范式-口袋生长分子-BOAT-ProteomeScan-SeFMol-TarPass.md
confidence: medium
---

> 来源: *Revisiting Target-Aware de novo Molecular Generation with TarPass: Between Rational Design and Texas Sharpshooter*. Adv Sci. doi:10.1002/advs.75411.

## 核心发现链

TarPass 是一个用于公平评估靶点感知 de novo 分子生成的基准测试集。核心发现：**多数号称"靶点感知"的 3D 分子生成模型，在不少靶点上的表现不如从 ChEMBL 随机抽样**。仅少数模型（DrugFlow、MolCraft）接近真实活性分子水平。

## 关键证据摘要

- **数据集**：18 个药物靶点（多为激酶），每靶点 ~1000 个 BindingDB 已验证活性分子
- **基线参照**：ChEMBL 随机抽取类药分子
- **防泄漏设计**：所有结构/数据选自 2019 年后，避开常见训练集（CrossDocked2020、PDBbind）
- **测试模型**：15 个代表性模型，分三类（非 3D / 3D 原位生成 / 基于优化）
- **关键结果**：
    - 3D 原位生成模型平均仅比非 3D 模型好一点，部分靶点不如 ChEMBL 随机抽样
    - 关键相互作用复现率：已知活性分子重新对接仅 ~51%，多数 AI 模型接近随机猜测
    - 3D 模型主要瓶颈：初始构象存在严重原子碰撞，分子位置从一开始就错了
    - 金属配位靶点（如 HDAC6 锌离子）多数模型不支持或处理混乱
- **后处理策略**：多层虚拟筛选——硬性过滤（相互作用/成药性）淘汰 90% → 软性筛选（化学经验/聚类）得 20-30 候选

## 交叉引用
- [[entities/protdbench]] — 同为 AI 领域评测基准（蛋白 binder 设计），可类比方法学
- [[entities/rxnbench]] — 化学反应理解 MLLM 评测基准，同为 AI 药物发现方向基准
- [[entities/de-novo-protein-design]] — 蛋白/分子设计方法全景背景
- [[entities/sefmoll]] — SeFMol 半柔性分子扩散模型，可纳入 TarPass 未来评测
