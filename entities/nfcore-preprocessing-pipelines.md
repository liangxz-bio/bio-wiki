---
title: nf-core / Nextflow 生信流程 — 预处理 & QC
created: 2026-05-06
updated: 2026-05-06
type: entity
tags: [pipeline, qc, preprocessing, fastqc, alignment, data-cleaning]
domain: bioinfo-pipeline
sources:
  - raw/articles/Nextflow工作流清单100-比对注释单细胞101-200.md
confidence: high
---

# nf-core / Nextflow 生信流程：预处理 & QC

> 来源：逻捷科技微信公众号 biostack，第二期 Nextflow 清单 #107–112 + #161–163
> 流程数量：10 个（QC #107–112，Alignment #161–163）

本页收录数据预处理和质控相关的 Nextflow 流程，涵盖 FastQC/MultiQC 报告、BAM QC、read 修剪、污染序列鉴定等。

## 流程清单

### Quality Control

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 107 | fastqc-nf | DNA-seq 数据 FastQC + MultiQC | https://github.com/IARCbioinfo/fastqc-nf |
| 108 | qualimap-nf | BAM 文件 QC（WES/WGS/靶向） | https://github.com/IARCbioinfo/qualimap-nf |
| 109 | atropos | NGS read 修剪（专一、灵敏、快速） | https://github.com/jdidion/atropos |
| 110 | seqqc | 新型测序数据 QC 问题识别 | https://github.com/Arcadia-Science/seqqc |
| 111 | blobtoolkit | BlobToolKit 流程（Sanger ToL）— 组装污染检测 | https://github.com/sanger-tol/blobtoolkit |
| 112 | ascc | 共生/污染序列鉴定（fasta + PacBio） | https://github.com/sanger-tol/ascc |

### Alignment（比对）

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 161 | alignment-nf | WES/WGS 比对流程 | https://github.com/IARCbioinfo/alignment-nf |
| 162 | concTree | 基于多 MSA 比对不确定性的共识树 | https://github.com/evanfloden/concTree |
| 163 | piper-nf | RNA mapping 流程 | https://github.com/cbcrg/piper-nf |

## 关键流程推荐

- **fastqc-nf** — FastQC + MultiQC 标准化 QC 报告
- **qualimap-nf** — BAM 级别多维 QC（WES/WGS/靶向）
- **atropos** — 灵活高效 read 修剪工具
- **blobtoolkit** — 组装后污染检测，mNGS 数据质量监控参考

## 交叉引用

- [[nfcore-microbiome-pipelines]] — 微生物组 QC 流程（Arcadia-Science/metagenomics）与本页互补
- [[nfcore-variant-calling-pipelines]] — QC 是变异 calling 的前置步骤
- [[nfcore-rnaseq-pipelines]] — RNA-seq 数据预处理需要本页 QC 工具
