---
title: nf-core / Nextflow 生信流程 — 蛋白质组 & 结构预测
created: 2026-05-06
updated: 2026-05-06
type: entity
tags: [pipeline, proteomics, mass-spec, protein-structure, alphafold, drug-discovery]
domain: bioinfo-pipeline
sources:
  - raw/articles/Nextflow工作流清单100-比对注释单细胞101-200.md
confidence: high
---

# nf-core / Nextflow 生信流程：蛋白质组 & 结构预测

> 来源：逻捷科技微信公众号 biostack，第二期 Nextflow 清单 #168–169 + #190–191
> 流程数量：4 个

本页收录蛋白质组学与结构生物学相关的 Nextflow 流程，涵盖蛋白质 3D 结构预测、AlphaFold2 MSA 计算、定量质谱和 MHC 洗脱肽段鉴定。

与 [[kingcreate-kcq]] 翻译后修饰分析、[[nfcore-variant-calling-pipelines]] 变异→蛋白结构影响分析有潜在交叉。

## 流程清单

### Structure Biology

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 168 | proteinfold | 蛋白质 3D 结构预测（整合 AlphaFold/ESMFold/OmegaFold 等） | https://github.com/nf-core/proteinfold |
| 169 | msa-af2-nf | 基于 AlphaFold2 的 MSA 计算 | https://github.com/cbcrg/msa-af2-nf |

### Mass Spectrometry

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 190 | mhcquant | MHC 洗脱肽段鉴定与定量 | https://github.com/nf-core/mhcquant |
| 191 | quantms | 定量质谱（DDA-LFQ / DDA-Isobaric / DIA-LFQ） | https://github.com/nf-core/quantms |

## 关键流程推荐

- **nf-core/proteinfold** — 多模型（AlphaFold2/ESMFold/OmegaFold）蛋白质结构预测，与 AI 药物发现方向直接对接
- **nf-core/quantms** — DIA/DDA 定量质谱，多组学整合的蛋白质组学端入口
- **nf-core/mhcquant** — MHC 肽段鉴定，肿瘤新抗原/免疫治疗方向

## 交叉引用

- [[nfcore-variant-calling-pipelines]] — 变异 calling 到蛋白结构影响（proteinfold）的分析链路
- [[nfcore-genome-pipelines]] — 基因组注释（nextNEOpi 新抗原）→ MHC 肽段鉴定的免疫信息学链路
- [[kingcreate-kcq]] — 定量系统与蛋白质组学定量的异同对比
- [[nfcore-rnaseq-pipelines]] — RNA-seq 差异表达 → 蛋白组验证的 multi-omics 链路
