---
title: Ovo — 开源从头蛋白设计生态系统
created: 2026-05-14
updated: 2026-05-14
type: entity
tags: [protein-design, pipeline, de-novo, ai-drug-discovery, workflow, Nextflow]
domain: ai-drug-discovery
sources:
  - raw/papers/prihoda2025-ovo-biorxiv.md
  - raw/articles/Ovo-蛋白设计开源生态平台-bioRxiv2026.md
confidence: high
---

# Ovo

> 来源: Prihoda D, Ancona M, Calounova T, et al. Ovo, an open-source ecosystem for de novo protein design. bioRxiv 2025. | 微信公众号·AI药物设计实验室 中文解读

Ovo 是 MSD (Merck Sharp & Dohme) 捷克研究中心 + AWS 联合开发的开源蛋白设计生态系统，将生成模型、工作流编排、数据管理和交互可视化集成到统一平台，降低蛋白设计工程门槛。

## 核心发现链

蛋白设计工具碎片化 → Ovo 三条设计范式（scaffold / binder / diversification）+ ProteinQC 质量评估 → Nextflow 工作流编排 + 存储层 + Web GUI / CLI → 社区插件开放生态 → 缩短从模型到候选设计的工程周期

## 关键证据摘要

### 架构
- **Nextflow 工作流编排**: RFdiffusion → ProteinMPNN → AlphaFold2/ESMFold 验证的标准管线，支持自定义参数
- **存储层**: SQL 数据库标准化存储序列/结构/指标/元数据
- **接口**: 命令行 CLI + Python API + Web GUI（步骤式提交、过滤、可视化）
- **插件系统**: Ovo Plugin API 允许社区添加新工作流（已支持 ProteinDJ 管线、Promb 人源化评估）

### 三大设计工作流
1. **Scaffold 设计**: RFdiffusion 生成骨架 → ProteinMPNN 序列设计 → AlphaFold2/ESMFold 结构验证（mean PAE / pLDDT / Design RMSD / Native Motif RMSD）
2. **Binder 设计**: RFdiffusion binder 流程 + BindCraft 流程（AlphaFold2 multimer backpropagation → ProteinMPNN → PyRosetta scoring）
3. **Binder Diversification**: partial RFdiffusion 协议，非界面残基扩散生成多样化 binder 骨架，保留靶标结合位点

### ProteinQC 模块
- **完整性指标**: sequence-level descriptors（氨基酸频率/净电荷/pI/熵/芳香性）+ structure-level descriptors（PyDSSP 二级结构/PPII/主链扭转角；PEP-Patch SAP 疏水斑块/正电荷积分）
- **参考集背景**: 全 PDB 分布参考，自动用绿-黄-橙标尺标注设计处于 optimal/neutral/sub-optimal 区间
- **跨领域验证**: 与抗体 HIC 疏水性实验 (r² 0.49, p=0.016)、Shehata polyspecificity 数据 (r² 0.72, p=0.0008) 显著相关

### 验证数据集
- 361 个从头设计已验证骨架（3 个案例：氧化还原酶基序支架/PD-1 界面支架/胰岛素受体 binder），覆盖 scaffold + binder + diversification 全流程

## 相关概念

- [[de-novo-protein-design]] — 从头蛋白设计综述（David Baker Nature 2026）
- [[protdbench]] — 蛋白 binder 设计统一评测基准
- [[mmseqs2]] — GPU加速同源搜索（Ovo 插件体系同类）
- [[isodde]] — Isomorphic Labs 药物设计引擎（蛋白设计评估对比）
- [[entities/ori-protein-design]] — ORI 腾讯 AI4S 闭环蛋白设计框架（湿实验反馈驱动的生成优化）
- [[raw/articles/ORI-腾讯AI4S蛋白设计闭环-NatureComm2026]] — ORI 原文解读
