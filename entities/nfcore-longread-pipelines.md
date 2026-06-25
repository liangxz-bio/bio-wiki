---
title: nf-core / Nextflow 生信流程 — 三代测序（Nanopore / PacBio）
created: 2026-05-06
updated: 2026-05-06
type: entity
tags: [pipeline, long-read, nanopore, pacbio, tgs, iso-seq, direct-rna]
domain: bioinfo-pipeline
sources:
  - raw/articles/Nextflow工作流清单100-微生物组转录组变异三代测序.md
confidence: high
---

# nf-core / Nextflow 生信流程：三代测序（Nanopore / PacBio）

> 来源：逻捷科技微信公众号 biostack，第一期 Nextflow 清单 #84–100
> 流程数量：17 个

本页收录三代测序相关的 Nextflow 流程，涵盖 Nanopore 和 PacBio 数据的拆分、QC、比对、组装、直接 RNA 分析、m6A 修饰检测等。

## 流程清单

### nf-core 流程

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 84 | nanoseq | Nanopore 拆分、QC、比对 | https://github.com/nf-core/nanoseq |
| 86 | isoseq | PacBio Iso-Seq 全长转录本注释（subreads → FLNC → bed） | https://github.com/nf-core/isoseq |

### 社区流程

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 85 | umi-pipeline-nf | ONT UMI 测序数据分析 | https://github.com/genepi/umi-pipeline-nf |
| 87 | master_of_pores | direct RNA Nanopore reads 分析 | https://github.com/biocorecrg/master_of_pores |
| 88 | ngs-preprocess | Illumina/Nanopore/PacBio 多平台数据预处理 | https://github.com/fmalmeida/ngs-preprocess |
| 89 | MpGAP | 多平台基因组组装（Illumina + Nanopore + PacBio 混合组装） | https://github.com/fmalmeida/MpGAP |
| 90 | porefile | ONT reads 全长 16S profiling | https://github.com/microgenlab/porefile |
| 91 | MOP2 | Nanopore reads 分析 | https://github.com/biocorecrg/MOP2 |
| 92 | nano-rave | ONT 数据快速现场 QC + variant calling | https://github.com/sanger-pathogens/nano-rave |
| 93 | Donut_Falls | Nanopore 测序基础流程 | https://github.com/UPHL-BioNGS/Donut_Falls |
| 94 | nf-m6anet | Nanopore direct RNA-seq m6A 修饰检测 | https://github.com/MaestSi/nf-m6anet |
| 95 | nanocompore_pipeline | Nanocompore 分析 | https://github.com/tleonardi/nanocompore_pipeline |
| 96 | nabseq_nf | NAb-seq 分析（hybridoma/single B cell Nanopore） | https://github.com/kzeglinski/nabseq_nf |
| 97 | nf-core-wgsnano | Nanopore WGS 分析 | https://github.com/dhslab/nf-core-wgsnano |
| 98 | Long-Read-Proteogenomics | 长读长 RNA-seq + 质谱蛋白质异构体检测 | https://github.com/sheynkman-lab/Long-Read-Proteogenomics |
| 99 | polishCLR | CLR 组装抛光 | https://github.com/isugifNF/polishCLR |

## 关键流程推荐

- **nf-core/nanoseq** — ONT 数据标准入口，快速上手
- **nf-core/isoseq** — PacBio Iso-Seq 全长转录本注释，与转录组分析衔接
- **nf-m6anet** — Nanopore mRNA 直接测序 m6A 修饰检测，表观转录组学方向
- **master_of_pores** — ONT direct RNA 完整分析
- **Long-Read-Proteogenomics** — 长读长 RNA-seq + 质谱，多组学整合范例

## 交叉引用

- [[nfcore-rnaseq-pipelines]] — 短读长 RNA-seq 流程，与长读长 RNA-seq 互补
- [[nfcore-genome-pipelines]] — 三代数据可输入组装流程
- [[nfcore-preprocessing-pipelines]] — 多平台数据预处理工具 ngs-preprocess
