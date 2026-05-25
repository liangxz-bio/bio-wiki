---
title: nf-core / Nextflow 生信流程 — 变异检测
created: 2026-05-06
updated: 2026-05-06
type: entity
tags: [pipeline, variant-calling, snv, sv, cnv, somatic, germline, cancer]
domain: bioinfo-pipeline
sources:
  - raw/articles/Nextflow工作流清单100-微生物组转录组变异三代测序.md
  - raw/articles/Nextflow工作流清单100-比对注释单细胞101-200.md
confidence: high
---

# nf-core / Nextflow 生信流程：变异检测

> 来源：逻捷科技微信公众号 biostack，两期 Nextflow 清单汇总
> 流程数量：30 个（#37–58 + #116–123）

本页收录胚系/体细胞变异检测相关的 Nextflow 流程，涵盖 WGS/WES/靶向测序的 SNV/Indel/SV calling、HLA 分型、GWAS、突变特征分析等。

与 [[wisecondorx]]（CNV 检测）、[[nfcore-scrnaseq-pipelines]]（scRNA-seq 变异 calling）直接交叉。

## 流程清单

### nf-core 流程

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 37 | sarek | 胚系/体细胞变异检测（WGS/靶向测序） | https://github.com/nf-core/sarek (Garcia et al. F1000Res 2020) |
| 39 | raredisease | WGS/WES 罕见病变异 calling 与评分 | https://github.com/nf-core/raredisease |
| 40 | hlatyping | 基于 NGS 的精密 HLA 分型 | https://github.com/nf-core/hlatyping |
| 41 | nf-gwas | 生物库规模 GWAS 分析 | https://github.com/nf-core/nf-gwas (Schoenherr et al. bioRxiv 2023) |
| 42 | rnavar | GATK4 RNA 变异 calling 流程 | https://github.com/nf-core/rnavar |

### IARCbioinfo 流程（国际癌症研究机构）

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 38 | strelka2-nf | Strelka（胚系+体细胞变异 calling） | https://github.com/IARCbioinfo/strelka-nf |
| 46 | mutect-nf | Mutect 流程 | https://github.com/IARCbioinfo/mutect-nf |
| 47 | BQSR-nf | BAM 文件碱基质量分数重校准（GATK） | https://github.com/IARCbioinfo/BQSR-nf |
| 48 | gama_annot-nf | VCF 文件 Annovar 注释 | https://github.com/IARCbioinfo/gama_annot-nf |
| 49 | needlestack | 多样本体细胞变异 calling | https://github.com/IARCbioinfo/needlestack |
| 52 | vcf_normalization-nf | VCF 分解与归一化 | https://github.com/IARCbioinfo/vcf_normalization-nf |
| 53 | table_annovar-nf | Annovar 变异注释 | https://github.com/IARCbioinfo/table_annovar-nf |
| 56 | MutSig | WGS 突变特征分析（SigProfilerExtractor） | https://github.com/IARCbioinfo/MutSig |

### 其他社区流程

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 43 | CalliNGS-NF | GATK RNA-Seq 变异 calling | https://github.com/CRG-CNAG/CalliNGS-NF |
| 44 | mtdna-server-2 | mtDNA 变异 calling | https://github.com/genepi/mtdna-server-2 |
| 45 | variant-calling-pipeline-gatk4 | GATK4 变异 calling | https://github.com/gencorefacility/variant-calling-pipeline-gatk4 |
| 50 | flow | 一致性组装与变异 calling | https://github.com/nmdp-bioinformatics/flow |
| 51 | hpc-gatk4 | GATK4 变异 calling（HPC） | https://github.com/gencorefacility/hpc-gatk4 |
| 54 | VariantAnalysis | 生信变异 calling | https://github.com/PlantandFoodResearch/VariantAnalysis |
| 55 | GenomicsCortextVarNextflow | Cortex-Var 结构变异 calling | https://github.com/ncsa/GenomicsCortextVarNextflow |
| 57 | tronflow-mutect2 | Mutect2 体细胞变异 calling（best practices） | https://github.com/TRON-Bioinformatics/tronflow-mutect2 |
| 58 | smoove-nf | Smoove 结构变异 calling + QC | https://github.com/brwnj/smoove-nf |

### 第二期续

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 116 | orchid | 癌症突变管理与 ML 注释框架 | https://github.com/Wittelab/orchid |
| 117 | variantmtb | 肿瘤变异生物学与预测相关性分析 | https://github.com/qbic-pipelines/variantmtb |
| 118 | aspicov | SARS-CoV-2 核苷酸变异自动化鉴定 | https://gitlab.com/vtilloy/aspicov |
| 119 | covigator-ngs-pipeline | SARS-CoV-2 NGS 变异 calling | https://github.com/TRON-Bioinformatics/covigator-ngs-pipeline |
| 120 | snv-calling-nf | SNV calling（GATK4/MuTect2/CaVEMan/Pindel/Strelka2 多 caller 联合） | https://github.com/ObenaufLab/snv-calling-nf |
| 121 | GATK-flow | GATK HaplotypeCaller 流程 | https://github.com/isugifNF/GATK-flow |
| 122 | variantcalling | 长读长比对变异 calling（DSL2） | https://github.com/sanger-tol/variantcalling |
| 123 | SNVPhyl_Nextflow | SNVPhyl Nextflow 版 | https://github.com/DHQP/SNVPhyl_Nextflow |

## 关键流程推荐

- **nf-core/sarek** — 肿瘤变异 calling 金标准，体细胞+胚系双模式，直接可用于肿瘤早筛变异检测
- **snv-calling-nf** — 多 caller 联合（GATK4 + MuTect2 + CaVEMan + Pindel + Strelka2），适合开发验证
- **orchid** — 癌症突变 ML 注释框架，与 AI 预测方向对接
- **smoove-nf** — 结构变异 calling，与 [[wisecondorx]] 的 CNV 检测互补
- **nf-core/hlatyping** — HLA 分型流程，与 HLA 检测面试笔记交叉

## 交叉引用

- [[wisecondorx]] — sWGS CNV 检测，与本页 SV calling 流程互补
- [[nfcore-microbiome-pipelines]] — 微生物组流程中病毒变异 calling（viralrecon/vipr）与本页体细胞 calling 共享方法学
- [[nfcore-scrnaseq-pipelines]] — scRNA-seq 变异检测（numbat-nf）与本页 bulk 变异 calling 形成尺度对比
- [[scrnaseq-workflow]] — single-cell 分析流程中变异检测模块参考
