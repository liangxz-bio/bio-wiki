---
title: nf-core / Nextflow 生信流程 — 单细胞 & 空间转录组
created: 2026-05-06
updated: 2026-05-06
type: entity
tags: [pipeline, single-cell, scrna-seq, spatial-transcriptomics, visium, 10x]
domain: bioinfo-pipeline
sources:
  - raw/articles/Nextflow工作流清单100-比对注释单细胞101-200.md
confidence: high
---

# nf-core / Nextflow 生信流程：单细胞 & 空间转录组

> 来源：逻捷科技微信公众号 biostack，第二期 Nextflow 清单 #174–185
> 流程数量：12 个

本页收录单细胞和空间转录组分析的 Nextflow 流程，涵盖 10X Genomics scRNA-seq、Visium 空间转录组、单细胞 CNV 检测、Cell Ranger 封装等。

与 [[scrnaseq-workflow]]、[[fastsc]]、[[scnotebooks]]、[[wisecondorx]] 直接交叉。

## 流程清单

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 174 | scrnaseq | 10X Genomics scRNA-seq 完整分析 | https://github.com/nf-core/scrnaseq |
| 175 | spatialtranscriptomics | 10X Visium 空间转录组分析 | https://github.com/nf-core/spatialtranscriptomics |
| 176 | scflow | 单细胞/单核 RNA-seq 完整分析 | https://github.com/nf-core/scflow |
| 177 | numbat-nf | 单细胞/空间转录组体细胞 CNV 检测（numbat） | https://github.com/IARCbioinfo/numbat-nf |
| 178 | cellranger | Cell Ranger 流程包装 | https://github.com/qbic-pipelines/cellranger |
| 179 | indrop | 基于 DropEst 的单细胞转录组分析 | https://github.com/biocorecrg/indrop |
| 180 | scRNAsim | scRNA-seq 模拟（含噪声源） | https://github.com/zavolanlab/scRNAsim |
| 181 | ScaleRna | ScaleBio 单细胞 RNA 流程 | https://github.com/ScaleBio/ScaleRna |
| 182 | cellranger-nf | Cell Ranger 流程 | https://github.com/WarrenLab/cellranger-nf |
| 183 | scpca-nf | 儿童癌症图谱单细胞数据处理 | https://github.com/AlexsLemonade/scpca-nf |
| 184 | quantification_nextflow | scRNA-seq / Visium 数据 kallisto 定量 | https://github.com/tomasgomes/quantification_nextflow |
| 185 | SIMBA | scRNA-seq QC/整合分析流程 | https://github.com/Mye-InfoBank/SIMBA |

## 关键流程推荐

- **nf-core/scrnaseq** — 10X scRNA-seq 标准化流程，与 [[scrnaseq-workflow]] 概念页互补
- **nf-core/spatialtranscriptomics** — Visium 空间转录组，空间生物学前沿
- **numbat-nf** — 单细胞 CNV 检测，与 [[wisecondorx]] 的 bulk CNV 检测形成分析尺度对比
- **scflow** — 集成度更高的单细胞分析流程
- **scpca-nf** — 儿童癌症图谱数据流程，肿瘤方向参考

## 交叉引用

- [[scrnaseq-workflow]] — scRNA-seq 标准化分析概念页，7 步流程详解
- [[fastsc]] — R 包一站式单细胞分析 + 可视化，与流程产出的表达矩阵对接
- [[scnotebooks]] — 开源单细胞培训平台，14 模块教程
- [[wisecondorx]] — bulk CNV 检测，与 numbat-nf 的 scCNV 对照
- [[nfcore-variant-calling-pipelines]] — bulk 变异 calling，与 numbat-nf 单细胞变异检测互补
- [[nfcore-rnaseq-pipelines]] — 本页流程的 bulk RNA-seq 对应
