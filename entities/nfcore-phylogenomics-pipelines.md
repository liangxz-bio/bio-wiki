---
title: nf-core / Nextflow 生信流程 — 系统发育 & Pan-基因组
created: 2026-05-06
updated: 2026-05-06
type: entity
tags: [pipeline, phylogenomics, pangenome, msa, phylogenetic-tree]
domain: bioinfo-pipeline
sources:
  - raw/articles/Nextflow工作流清单100-比对注释单细胞101-200.md
confidence: high
---

# nf-core / Nextflow 生信流程：系统发育 & Pan-基因组

> 来源：逻捷科技微信公众号 biostack，第二期 Nextflow 清单 #151–173
> 流程数量：16 个（Phylogenomics #151–159，Pangenome #170–173，Alignment #160–163）

本页收录系统发育和 pan-基因组分析相关的 Nextflow 流程，涵盖系统发育树构建、MSA 比对、pan-基因组图构建、结构变异基因分型等。

## 流程清单

### Phylogenomics: nf-core 流程

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 151 | phyloplace | EPA-NG 系统发育放置 | https://github.com/nf-core/phyloplace |
| 160 | multiplesequencealign | MSA 方法系统评估 | https://github.com/nf-core/multiplesequencealign |

### Phylogenomics: 社区流程

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 152 | raxml-nf | RAxML 最大似然法系统发育树 | https://github.com/cbcrg/raxml-nf |
| 153 | tcoffee-nf | T-Coffee 多序列比对 | https://github.com/cbcrg/tcoffee-nf |
| 154 | noveltree | 高度并行化系统基因组学（基因家族推断） | https://github.com/Arcadia-Science/noveltree |
| 155 | fluflo | Auspice 可视化的系统发育树生成 | https://github.com/BCCDC-PHL/fluflo |
| 156 | ALPPACA | 原核系统发育与聚类分析 | https://github.com/NorwegianVeterinaryInstitute/ALPPACA |
| 157 | busco2phylo-nf | BUSCO 结果转为物种树 | https://github.com/lstevens17/busco2phylo-nf |
| 158 | unistrap | 推断系统发育树的可靠性度量 | https://github.com/cbcrg/unistrap |
| 159 | paraMSA | 多序列比对与 bootstrap 支持值 | https://github.com/evanfloden/paraMSA |

### Alignment

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 161 | alignment-nf | WES/WGS 比对流程 | https://github.com/IARCbioinfo/alignment-nf |
| 162 | concTree | 基于多 MSA 比对不确定性的共识树 | https://github.com/evanfloden/concTree |
| 163 | piper-nf | RNA mapping 流程 | https://github.com/cbcrg/piper-nf |

### Pangenome

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 170 | pangenome | 序列集合转 pangenome 图 | https://github.com/nf-core/pangenome |
| 171 | minigraph-cactus-nf | minigraph-cactus pangenome 构建 | https://github.com/WarrenLab/minigraph-cactus-nf |
| 172 | giraffe-svs-nf | Pangenome 结构变异基因分型 | https://github.com/WarrenLab/giraffe-svs-nf |
| 173 | giraffe-nf | vg giraffe 运行流程 | https://github.com/WarrenLab/giraffe-nf |

## 关键流程推荐

- **nf-core/pangenome** — pan-基因组图构建，细菌基因组多样性分析
- **nf-core/phyloplace** — EPA-NG 系统发育放置，宏基因组分类学辅助
- **busco2phylo-nf** — BUSCO → 物种树，组装质量评估后的系统发育分析
- **fluflo** — Auspice 可视化，病原体传播追踪

## 交叉引用

- [[nfcore-microbiome-pipelines]] — 微生物组流程中系统发育模块（bactmap）与本页关联
- [[nfcore-genome-pipelines]] — 基因组组装是 pan-基因组分析的前置步骤
- [[nfcore-variant-calling-pipelines]] — pangenome 图指导变异检测
