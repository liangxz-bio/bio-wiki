---
title: nf-core / Nextflow 生信流程 — RNA-seq & 转录组
created: 2026-05-06
updated: 2026-05-06
type: entity
tags: [pipeline, rna-seq, transcriptomics, fusion, splicing, lncrna]
domain: bioinfo-pipeline
sources:
  - raw/articles/Nextflow工作流清单100-微生物组转录组变异三代测序.md
  - raw/articles/Nextflow工作流清单100-比对注释单细胞101-200.md
confidence: high
---

# nf-core / Nextflow 生信流程：RNA-seq & 转录组

> 来源：逻捷科技微信公众号 biostack，两期 Nextflow 清单汇总
> 流程数量：28 个（#59–83 + #113–115）

本页收录 RNA-seq 转录组分析相关 Nextflow 流程，涵盖差异表达、基因融合、可变剪接、小 RNA、环状 RNA、lncRNA、dual RNA-seq 等方向。

与 [[scrnaseq-workflow]]、[[nfcore-scrnaseq-pipelines]]、[[fastsc]] 直接交叉。

## 流程清单

### nf-core 流程

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 59 | rnaseq | RNA-seq 分析（STAR/RSEM/HISAT2/Salmon + 基因/异构体计数 + QC） | https://github.com/nf-core/rnaseq |
| 60 | rnafusion | RNA-seq 基因融合检测 | https://github.com/nf-core/rnafusion |
| 61 | smrnaseq | 小 RNA-seq 分析 | https://github.com/nf-core/smrnaseq |
| 62 | dualrnaseq | Dual RNA-seq 分析（宿主-病原体互作） | https://github.com/nf-core/dualrnaseq |
| 63 | circrna | 环状 RNA 定量、miRNA 靶标预测、差异表达 | https://github.com/nf-core/circrna (Digby et al. BMC Bioinform 2023) |
| 64 | rnasplice | 可变剪接分析 | https://github.com/nf-core/rnasplice |
| 65 | differentialabundance | 特征/观测矩阵差异丰度分析 | https://github.com/nf-core/differentialabundance |

### 社区流程

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 66 | FLOP | 转录组数据流程对下游功能富集的影响评估 | https://github.com/FLOP (Paton et al. bioRxiv 2023) |
| 67 | grape-nf | 自动化 RNA-seq 流程 | https://github.com/guigolab/grape-nf |
| 68 | RNAseq-fusion-nf | STAR-Fusion 基因融合检测 | https://github.com/IARCbioinfo/RNAseq-fusion-nf |
| 69 | LncPipe | 长链非编码 RNA 综合分析 | https://github.com/likelet/LncPipe |
| 70 | RNAseq-transcript-nf | 转录本鉴定与定量（BAM 输入） | https://github.com/IARCbioinfo/RNAseq-transcript-nf |
| 71 | gene-fusions-nf | Arriba 基因融合检测 | https://github.com/IARCbioinfo/gene-fusions-nf |
| 72 | quantiseq-nf | RNA-seq 免疫细胞含量定量 | https://github.com/IARCbioinfo/quantiseq-nf |
| 73 | rnaflow | 简单 RNA-Seq 差异基因表达 | https://github.com/hoelzer-lab/rnaflow |
| 74 | RNAseq-Flow | RNA-seq 数据处理 | https://github.com/Dowell-Lab/RNAseq-Flow |
| 75 | vast-tools-nf | VAST-TOOLS 可变剪接 profiling | https://github.com/evanfloden/vast-tools-nf |
| 76 | icgc-featurecounts | RNA-seq BAM 文件 featureCounts | https://github.com/qbicsoftware/icgc-featurecounts |
| 77 | ShortRNA-nf | shortRNA-seq 数据处理 | https://github.com/abreschi/ShortRNA-nf |
| 78 | lncRNA-Annotation-nf | lncRNA 注释（STAR + Cufflinks + FEELnc） | https://github.com/evanfloden/lncRNA-Annotation-nf |
| 79 | DGE | DESeq2 差异表达分析（对接 nf-core/rnaseq 输出） | https://github.com/lconde-ucl/DGE |
| 80 | allele_specific_RNAseq | 等位基因特异性 RNA-seq | https://github.com/biocorecrg/allele_specific_RNAseq |
| 81 | rnadeseq | RNA-seq 差异表达 + 通路分析 | https://github.com/qbic-pipelines/rnadeseq |
| 82 | longread-svs-nf | 长读长结构变异检测 | https://github.com/WarrenLab/longread-svs-nf |
| 83 | kallisto-nf | Kallisto & Sleuth RNA-Seq 工具 | https://github.com/cbcrg/kallisto-nf |

### 第二期续

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 113 | ipsa-nf | 整合剪接分析流程 | https://github.com/guigolab/ipsa-nf |
| 114 | RNASeq-NF | RNA 表达定量 | https://github.com/UMCUGenetics/RNASeq-NF |
| 115 | RNA_Seq | RNA-seq 预处理 | https://github.com/compbiomed/RNA_Seq |

## 关键流程推荐

- **nf-core/rnaseq** — 多比对器（STAR/HISAT2/Salmon/RSEM）支持，RNA-seq 分析起点
- **nf-core/rnafusion** — 基因融合检测，肿瘤早筛中融合基因 biomarker 发现
- **nf-core/dualrnaseq** — 宿主-病原体双 RNA-seq，与感染性疾病转录组研究直接相关
- **nf-core/circrna** — 环状 RNA 定量，肿瘤 biomarker 新方向
- **quantiseq-nf** — RNA-seq 免疫浸润定量，肿瘤微环境分析
- **LncPipe** — lncRNA 一站式分析，非编码 RNA 研究

## 交叉引用

- [[scrnaseq-workflow]] — 单细胞转录组标准化分析流程，与本页 bulk RNA-seq 互补
- [[nfcore-scrnaseq-pipelines]] — 单细胞流程实体页，与本页 bulk RNA-seq 形成分析尺度对照
- [[fastsc]] — R 包 fastSC 单细胞分析，可处理本页流程产出的表达矩阵
- [[scnotebooks]] — 单细胞培训平台中的 RNA-seq 分析模块
