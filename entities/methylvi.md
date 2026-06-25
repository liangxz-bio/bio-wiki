---
title: MethylVI
created: 2026-05-12
updated: 2026-05-12
type: entity
tags: [model, bioinformatics, single-cell, methylation, normalization]
domain: bioinfo-pipeline
sources: [raw/articles/MethylVI-单细胞甲基化VAE贝塔二项分布-NatMachIntell.md]
confidence: medium
---

# MethylVI

MethylVI 是一个基于 VAE 的概率生成模型，专为单细胞亚硫酸氢盐测序（scBS-seq）数据设计，使用 **Beta-Binomial 分布**作为似然函数，直接对甲基化 count 对建模。由华盛顿大学 Su-In Lee 组联合 Salk 的 Joseph Ecker 组、UC Berkeley/Weizmann 的 Nir Yosef 组共同发表于 Nature Machine Intelligence 2025。集成于 scvi-tools，与 scanpy/scverse 生态无缝对接。^[raw/articles/MethylVI-单细胞甲基化VAE贝塔二项分布-NatMachIntell.md]

## 核心发现链

scBS-seq count 数据过分散（overdispersion）→ 二项分布 dispersion 不足 → **Beta-Binomial 似然**捕获 region-specific 甲基化概率变异性 → VAE 框架高效训练 → 降噪 + DMG 检测 + batch correction + reference mapping + 多组学整合，五项任务全面优于现有方法。

## 关键证据摘要

- **降噪（denoising）**：在 Luo 2017 mouse frontal cortex snmC-seq 上，MethylVI 降噪后 marker gene 信号系统性优于 ALLCools（binomial test P < 1e-6），Arpp21 和 Adgra3 两类 marker 揭示此前被掩盖的亚群差异
- **DMG 检测**：Bayes factor 方法，2 分钟内完成 vs scMET >24 小时；与 bulk MethylC-seq limma 结果相关性持平 scMET 且无 q 值 inflation，而 Wilcoxon/DSS/ArchR/Signac/SnapATAC2/EpiScanpy 均有严重 inflation
- **Batch correction**：scIB 13 指标综合排第一，优于 fastMNN/Harmony/Scanorama/Seurat/PeakVI
- **Reference mapping**：MethylANVI + scArches 支持 cross-protocol mapping，hold-out 实验未见过的 cell type 不被强行归入
- **MultiMethylVI**：RNA（负二项）+ 甲基化（Beta-Binomial）联合建模，高表达基因 imputation Spearman > 0.70

## 来源与交叉引用

- 原始论文：Ashuach T, et al. Nat Mach Intell. 2025;7:572-583. doi:10.1038/s42256-025-01011-5 ^[raw/articles/MethylVI-单细胞甲基化VAE贝塔二项分布-NatMachIntell.md]
- 相关工具：[[scrnaseq-workflow]] — scRNA-seq 分析流程（scverse 生态同期工具）
- 相关工具：[[scmbench]] — 单细胞多组学集成基准测试（MethylVI 可纳入未来 benchmark）
- 相关工具：[[fastsc]] — 单细胞 R 包分析工具（不同技术路线）

## 交叉引用
- [[concepts/cfdna-fragmentomics-mced]] — MCED cfDNA 甲基化检测，MethylVI 的 Beta-Binomial 降噪思路可借鉴
- [[concepts/five-cancer-screening-guide]] — 五大癌种筛查指南，甲基化在结直肠癌多靶标检测中应用
- [[entities/scmethcraft]] — scMethCraft 单细胞 DNA 甲基化统一分析框架，使用混合神经网络+KAN，技术路线互补
