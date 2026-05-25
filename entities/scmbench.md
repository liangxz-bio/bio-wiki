---
title: SCMBench
created: 2026-05-08
updated: 2026-05-08
type: entity
tags: [benchmark, single-cell, multi-omics, data-integration, foundation-model, scrnaseq, scatac]
domain: bioinfo-pipeline
confidence: high
sources: [raw/articles/SCMBench-单细胞多组学集成分析基准测试-NatCommun2026.md]
---

# SCMBench

SCMBench 是香港中文大学 Yu Li 团队提出的单细胞多组学数据集成方法的综合性基准测试框架（Nature Communications 2026, PMID: 42069651）。对 23 种方法（19 种 DMs + 4 种 FMs）进行了系统评估。

## 基本信息

- **DOI**: [10.1038/s41467-026-72570-x](https://doi.org/10.1038/s41467-026-72570-x) | **PMID**: 42069651
- **通讯作者**: Yu Li（李煜），香港中文大学计算机科学与工程系
- **第一作者**: Yixuan Wang, Yimin Fan, Xuesong Wang

## 评估框架

- **数据集**: 6 个真实数据集 + 模拟数据（人类 PBMC、小鼠大脑、小鼠皮肤、小鼠肾脏）
- **评估指标**: MAP, NMI, ASW, ARI
- **评估维度**: 集成准确性、生物标志物检测、轨迹推断、批次效应校正、计算效率、可扩展性
- **FMs 处理策略**: 用 MAESTRO 等工具将 ATAC 数据转换为基因活性矩阵，实现跨模态处理

## 关键发现

| 场景 | 最优方法 | 说明 |
|------|----------|------|
| 成对数据集成 | GLUE | 性能最稳定 |
| 非成对数据集成 | scJoint | 明显优势 |
| 轨迹推断（成对） | MOFA | |
| 轨迹推断（非成对） | Harmony | |
| 跨物种轨迹保留 | UCE (FM) | TC 得分 > 0.7 |
| 批次效应校正 | scJoint / MMD-MA | 最佳平衡 |
| 计算资源有限 | scJoint / Harmony | 内存少、速度快 |

## FMs 与 DMs 对比

- **DMs 总体上仍占优势**: GLUE、scJoint 在多数指标领先
- **FMs 零样本模式**: 在多组学集成中普遍逊于顶尖 DMs，主要原因是不同模态间的数据分布差异
- **FMs 的优势**: 保留生物发育信号（轨迹推断）表现不俗，体现预训练生物学知识
- **适配策略提升**: FM-scVI（轻量级适配器）可使 UCE-scVI 在小鼠数据集上提升 +65% 以上

## 核心贡献

- 系统对比了 23 种单细胞多组学集成方法，覆盖 DMs 和 FMs
- 揭示了 FMs 在多组学集成中的瓶颈（模态分布差异）和潜力（适配层微调）
- 提供了不同场景的选型指南

## 相关页面

- [[scrnaseq-workflow]] — scRNA-seq 标准化分析全流程
- [[scnotebooks]] — 开源单细胞与空间组学培训平台
- [[regformer]] — 单细胞 Mamba 基础模型
- [[fastsc]] — 单细胞 R 包 fastSC
- [[scmethcraft]] — scMethCraft 单细胞 DNA 甲基化统一分析框架，可纳入 SCMBench 未来 benchmark
