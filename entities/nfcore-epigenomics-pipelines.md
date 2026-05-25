---
title: nf-core / Nextflow 生信流程 — 表观组学（ChIP / ATAC / Hi-C）
created: 2026-05-06
updated: 2026-05-06
type: entity
tags: [pipeline, chip-seq, atac-seq, hi-c, epigenomics, chromatin]
domain: bioinfo-pipeline
sources:
  - raw/articles/Nextflow工作流清单100-比对注释单细胞101-200.md
confidence: high
---

# nf-core / Nextflow 生信流程：表观组学（ChIP / ATAC / Hi-C）

> 来源：逻捷科技微信公众号 biostack，第二期 Nextflow 清单 #164–167 + #186–196
> 流程数量：13 个

本页收录表观组学相关的 Nextflow 流程，涵盖 ChIP-seq peak calling、ATAC-seq 开放染色质分析、Hi-C 染色质构象捕获、环状 DNA 鉴定等。

## 流程清单

### ChIP-seq

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 164 | chipseq | ChIP-seq peak calling / QC / 差异分析 | https://github.com/nf-core/chipseq |
| 165 | chip-nf | 自动化 ChIP-seq 流程 | https://github.com/guigolab/chip-nf |
| 166 | ChIP-Flow | ChIP-seq 分析 | https://github.com/Dowell-Lab/ChIP-Flow |
| 167 | ChIP-seq | ChIP-seq 数据 QC 与分析 | https://github.com/bioinfo-pf-curie/ChIP-seq |

### Hi-C

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 186 | eager | 可重复、可移植的古基因组重建 | https://github.com/nf-core/eager (Yates et al. PeerJ 2021) |
| 187 | hic | 染色质构象捕获（Hi-C）分析 | https://github.com/nf-core/hic |
| 188 | distiller-nf | 模块化 Hi-C mapping 流程 | https://github.com/open2c/distiller-nf |
| 189 | genomenote | Hi-C reads 比对 → contact maps + 统计表 | https://github.com/sanger-tol/genomenote |

### ATAC-seq

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 192 | circdna | 环状染色体外 DNA 鉴定（Circle-seq/WGS/ATAC-seq） | https://github.com/nf-core/circdna |
| 193 | ATACFlow | ATAC-seq 流程 | https://github.com/NCBI-Hackathons/ATACFlow |
| 194 | PUMATAC | 通用 ATAC-seq 比对 | https://github.com/aertslab/PUMATAC |
| 195 | atacseq | ATAC-seq peak calling / QC | https://github.com/nf-core/atacseq |
| 196 | atac_nf | ATAC-seq 流程 | https://github.com/alexyfyf/atac_nf |

## 关键流程推荐

- **nf-core/chipseq** — ChIP-seq 分析标准
- **nf-core/atacseq** — ATAC-seq 开放染色质分析
- **nf-core/hic** — Hi-C 染色质构象捕获
- **nf-core/circdna** — 环状染色体外 DNA 鉴定，肿瘤基因组不稳定性的标志

## 交叉引用

- [[nfcore-variant-calling-pipelines]] — 表观组数据可辅助变异解读
- [[wisecondorx]] — CNV 检测与表观组学数据联合分析
- [[nfcore-scrnaseq-pipelines]] — 单细胞 ATAC-seq 与空间表观组学
