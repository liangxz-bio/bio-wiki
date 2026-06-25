---
title: scMethCraft — 单细胞DNA甲基化统一分析框架
created: 2026-05-19
updated: 2026-05-19
type: entity
tags: [single-cell, dna-methylation, methylation, scdnam, bioinfo-pipeline]
domain: bioinfo-pipeline
sources:
  - raw/articles/scMethCraft-单细胞DNA甲基化统一分析框架-NatCommun2026.md
confidence: medium
---

## 核心发现链

scMethCraft 是南开大学陈盛泉团队提出的单细胞 DNA 甲基化（scDNAm）数据统一分析框架。它打破直接套用 scRNA-seq 分析流程的常规做法，针对 scDNAm 数据的两大核心特性进行了底层架构重构：显式缺失值（NA ≠ 0）和连续比率分布（0-1 间的甲基化率）。^[raw/articles/scMethCraft-单细胞DNA甲基化统一分析框架-NatCommun2026.md]

核心架构：混合神经网络（CNN + FCNN）提取 DNA 序列特征 → KAN（Kolmogorov-Arnold Network）融合特征并嵌入基因组位置信息 → 密集线性层预测甲基化水平 → 迭代细胞间相似性加权模块增强预测。输出含增强的 scDNAm 矩阵、细胞间相似性矩阵和 50 维细胞嵌入，覆盖从降维/聚类/批次整合到信号增强/DMR 识别的全流程。^[raw/articles/scMethCraft-单细胞DNA甲基化统一分析框架-NatCommun2026.md]

## 关键证据摘要

- **原生 NA 处理**：scMethCraft 直接在含显式 NA 值的矩阵上运行，不填充为 0 或丢弃，在去噪的同时合理插补缺失值 ^[raw/articles/scMethCraft-单细胞DNA甲基化统一分析框架-NatCommun2026.md]
- **细胞嵌入与聚类**：在多个真实 scDNAm 数据集上，scMethCraft 的 ARI/NMI 显著优于传统 PCA 和其他单细胞降维方法 ^[raw/articles/scMethCraft-单细胞DNA甲基化统一分析框架-NatCommun2026.md]
- **多源数据整合**：迭代相似性加权模块能有效消除不同数据源/测序平台/供体间的批次效应，将相同细胞类型在嵌入空间中对齐 ^[raw/articles/scMethCraft-单细胞DNA甲基化统一分析框架-NatCommun2026.md]
- **跨数据集注释**：基于嵌入空间的细胞标签转移准确率远超未适配甲基化数据的常规流程 ^[raw/articles/scMethCraft-单细胞DNA甲基化统一分析框架-NatCommun2026.md]
- **DMR 识别**：增强后的甲基化矩阵支持高精度差异甲基化区域识别，假阳性低于现有方法，GO 富集与细胞功能匹配 ^[raw/articles/scMethCraft-单细胞DNA甲基化统一分析框架-NatCommun2026.md]
- **生物学发现**：在人类大脑少突胶质细胞中鉴定出数据库中未被充分表征的低甲基化区域及相关基因 ^[raw/articles/scMethCraft-单细胞DNA甲基化统一分析框架-NatCommun2026.md]

## 与相关工具对比

| 维度 | scMethCraft | MethylVI | 传统方法（PCA/scRNA-seq） |
|------|-------------|----------|--------------------------|
| 处理 NA 方式 | 原生处理，保留 NA 结构 | Beta-Binomial 概率建模 | 填充为 0 或丢弃 |
| 数据分布假设 | 连续比率（0-1） | Beta-Binomial | 计数分布（scRNA-seq 假设） |
| 序列特征 | CNN+FCNN+KAN 提取 | 无 | 无 |
| 位置编码 | KAN 嵌入位置信息 | 无 | 无 |
| 核心输出 | 50D 细胞嵌入 + 增强矩阵 | latent representation | PCA embedding |
| 全流程覆盖 | 降维/聚类/批次整合/增强/DMR | 降维/降噪/integration | 需拼接多个工具 |

## 交叉引用

- [[entities/methylvi]] — 同为 scDNAm 分析方法，MethylVI 使用 Beta-Binomial VAE 概率建模，scMethCraft 使用混合神经网络+KAN 序列特征提取，技术路线互补
- [[entities/scmbench]] — SCMBench 评测单细胞多组学集成方法，未来可纳入 scMethCraft
- [[entities/regformer]] — RegFormer 单细胞 Mamba 基础模型（转录组），与 scMethCraft 覆盖不同单细胞模态
- [[entities/fastsc]] — fastSC 单细胞 R 包分析工具，scMethCraft 填补其 DNA 甲基化分析空缺
- [[concepts/scrnaseq-workflow]] — scRNA-seq 标准化分析流程，scMethCraft 打破其适用于 scDNAm 的惯性思维

## 局限与未公开

- 混合神经网络架构计算开销大，对大型数据集内存消耗较高 ^[raw/articles/scMethCraft-单细胞DNA甲基化统一分析框架-NatCommun2026.md]
- 当前为单模态（仅 DNA 甲基化），尚未扩展至甲基化+转录组多模态联合分析 ^[raw/articles/scMethCraft-单细胞DNA甲基化统一分析框架-NatCommun2026.md]
- 来源为微信公众号二次解读，具体 benchmark 数值和对比方法细节需查阅原始论文确认
