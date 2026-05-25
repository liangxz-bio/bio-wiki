---
title: nf-core / Nextflow 生信流程 — 基因组组装 & 注释
created: 2026-05-06
updated: 2026-05-06
type: entity
tags: [pipeline, genome, assembly, annotation, repeat-masking, gene-prediction]
domain: bioinfo-pipeline
sources:
  - raw/articles/Nextflow工作流清单100-比对注释单细胞101-200.md
confidence: high
---

# nf-core / Nextflow 生信流程：基因组组装 & 注释

> 来源：逻捷科技微信公众号 biostack，第二期 Nextflow 清单 #124–150
> 流程数量：27 个（Annotation #124–139，Assembly #140–150）

本页收录基因组组装与注释相关的 Nextflow 流程，涵盖细菌/真核基因组注释、水平基因转移检测、新抗原预测、基因组组装拼接、抛光、Hi-C scaffolding 等。

## 流程清单

### Annotation: nf-core 流程

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 124 | hgtseq | NGS 数据水平基因转移检测 | https://github.com/nf-core/hgtseq |

### Annotation: 社区流程

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 125 | bactopia | 细菌基因组完整分析（灵活流程） | https://github.com/bactopia/bactopia (Petit & Read, mSystems 2020) |
| 126 | FA-Nf | 非模式物种蛋白功能注释 | https://github.com/FA-Nf (Vlasova et al. Genes 2021) |
| 127 | geneidx | 蛋白编码基因基因组注释 | https://github.com/guigolab/geneidx |
| 128 | ribap | 细菌核心基因集注释（Roary + ILP） | https://github.com/hoelzer-lab/ribap |
| 129 | nextNEOpi | 计算新抗原预测（肿瘤突变→新抗原表位） | https://github.com/icbi-lab/nextNEOpi |
| 130 | companion | 真核生物基因组注释 | https://github.com/sanger-pathogens/companion |
| 131 | pipelines-nextflow | 基因组注释工作流集（NBIS） | https://github.com/NBISweden/pipelines-nextflow |
| 132 | nextflow-annotate | 真菌基因组注释 | https://github.com/robsyme/nextflow-annotate |
| 133 | arete | AMR/VF LGT 聚焦细菌基因组分析 | https://github.com/beiko-lab/arete |
| 134 | ExOrthist | 外显子直系同源推断 | https://github.com/biocorecrg/ExOrthist |
| 135 | bacannot | 原核基因组通用注释 + 交互报告 | https://github.com/fmalmeida/bacannot |
| 136 | BACTpipe | 细菌基因组组装与注释 | https://github.com/ctmrbio/BACTpipe |
| 137 | next-pipes-lghm | 真核基因组注释 | https://github.com/andremrsantos/next-pipes-lghm |
| 138 | nf-repeatmasking | 重复序列自动检测、分类与屏蔽 | https://github.com/robsyme/nf-repeatmasking |
| 139 | mask-repeats-nf | RepeatModeler/Masker 简易流程 | https://github.com/WarrenLab/mask-repeats-nf |

### Assembly: 社区流程

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 140 | reads2genome | 多技术平台 reads QC 与组装 | https://github.com/Arcadia-Science/reads2genome |
| 141 | TransPi | 从头转录组组装 | https://github.com/PalMuc/TransPi |
| 142 | transcriptome_assembly | de novo 转录组组装与注释（Trinity/TransDecoder） | https://github.com/biocorecrg/transcriptome_assembly |
| 143 | vectorQC | 载体组装与注释 | https://github.com/biocorecrg/vectorQC |
| 144 | hic-scaffolding-nf | Hi-C reads 基因组组装支架搭建 | https://github.com/WarrenLab/hic-scaffolding-nf |
| 145 | genomeassembly | Tree of Life 基因组组装流程（Sanger ToL） | https://github.com/sanger-tol/genomeassembly |
| 146 | readmapping | 短/长读长比对到基因组组装（DSL2） | https://github.com/sanger-tol/readmapping |
| 147 | shortread-polish-nf | 短读长抛光（freebayes） | https://github.com/WarrenLab/shortread-polish-nf |
| 148 | purge-haplotigs-nf | 单倍型冗余去除 | https://github.com/WarrenLab/purge-haplotigs-nf |
| 149 | longread-polish-nf | 长读长抛光（racon） | https://github.com/WarrenLab/longread-polish-nf |
| 150 | mikrokondo | 细菌组装与 QC 简易流程 | https://github.com/phac-nml/mikrokondo |

## 关键流程推荐

- **bactopia** — 细菌基因组完整分析，临床病原体基因组分析的首选
- **nextNEOpi** — 从肿瘤变异到新抗原表位，衔接肿瘤早筛与免疫治疗
- **hgtseq** — 水平基因转移检测，耐药基因传播分析
- **arete** — AMR/VF 分析，与 [[kingcreate-kcq]] 耐药检测互补
- **bacannot** — 原核注释 + 交互报告，mNGS 微生物分析辅助

## 交叉引用

- [[nfcore-microbiome-pipelines]] — 微生物组组装分箱（mag）与本页基因组组装互补
- [[nfcore-variant-calling-pipelines]] — 组装后的变异 calling 链路
- [[kingcreate-kcq]] — 耐药检测，arete/bactopia 的 AMR 分析可交叉验证
- [[nfcore-longread-pipelines]] — 三代测序数据可输入本页抛光/组装流程
