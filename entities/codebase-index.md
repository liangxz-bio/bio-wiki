---
title: Codebase Index
created: 2026-05-08
updated: 2026-05-08
type: entity
tags: [codebase, bioinformatics, pipeline, python, perl, r, rust, nextflow]
domain: bioinfo-pipeline
confidence: high
---

# Codebase Index

统一的代码组织入口。代码本体分散在不同位置，通过 `~/code/` 软链接聚合。

## 项目地图

### Python 工具集 (`~/code/python/`)
| 项目 | 真实路径 | 说明 |
|------|----------|------|
| bioAI_agent | `~/projects/bioAI_agent/` | AI agent 生信项目 |
| NGS-PrimerPlex | `~/Desktop/github/NGS-PrimerPlex/` | NGS 引物设计（含主入口 main.py、getGeneRegions.py、convertToDraftFile.py 等） |
| ML | `~/Desktop/github/ML/` | 机器学习项目 |
| qiming | `~/Desktop/github/qiming/` | qiming 工具 |

### Perl 工具集 (`~/code/perl/`)
| 项目 | 真实路径 | 说明 |
|------|----------|------|
| ivd-scripts | `~/Desktop/gitlab/liangxz/ivd/scripts/` | IVD 生产脚本：ONT 批次监控 | blast 物种注释 | 性能验证 | 数据合并 |
| — ONT_batch_monitoring.pl | | ONT 测序实时监控 |
| — blast_addSpecies.pl | | Blast 比对后添加物种注释 |
| — performance_verification.pl | | 性能验证主流程 |
| — performance_verification.score.pl | | 性能评分模块 |
| — rawDataMerge.pl | | 原始数据合并 |

### R 工具集 (`~/code/r/`)
| 项目 | 真实路径 | 说明 |
|------|----------|------|
| github-R | `~/Desktop/github/scripts/R/` | 通用 R 脚本（ggplot 画图、统计分析） |

### Rust 项目 (`~/code/rust/`)
| 项目 | 真实路径 | 说明 |
|------|----------|------|
| multiPrimerEvaluation | `~/projects/scripts/multiPrimerEvaluation/` | 多引物评估工具（Rust，精度优先） |

### Pipeline (`~/code/pipelines/`)
| 项目 | 引擎 | 说明 |
|------|------|------|
| graphamr (`~/Desktop/github/graphamr/`) | Nextflow | 病原 AMR 检测流程（main.nf + nextflow.config） |
| ivd (`~/Desktop/gitlab/liangxz/ivd/`) | Nextflow + bash | IVD 全流程（含交接文档、脚本） |
| tNGS-V1 (`~/Desktop/gitlab/tNGS_V1.0.00/`) | Snakemake + Perl | InnoPAP tNGS/mNGS 双模块分析流程（Perl配置生成→Snakemake执行，rules/子规则+mkdocs报告，26个子脚本） |
| transcriptome (`~/Desktop/gitlab/transcriptome/`) | bash | 转录组分析流程：fastp→STAR比对→RSEM定量→EBSeq差异表达，3步脚本 |

### 第三方工具 (`~/code/vendor/`)
| 工具 | 真实路径 | 用途 |
|------|----------|------|
| Porechop | `~/Desktop/software/Porechop/` | Nanopore 接头切除 |
| ssPRIMER | `~/Desktop/software/ssPRIMER/` | R Shiny 引物设计 App |
| minimap2 | `~/Desktop/github/minimap2/` | 比对工具（源码） |
| seqtk | `~/Desktop/github/seqtk/` | FASTA/Q 处理 |
| bioawk | `~/Desktop/github/bioawk/` | 生信版 awk |
| emu | `~/Desktop/github/emu/` | 16S 微生物丰度估计 |
| oligoarrayaux | `~/Desktop/software/oligoarrayaux-3.8/` | 寡核苷酸阵列工具 |

### 通用脚本 (`~/code/scripts/`)
| 位置 | 说明 |
|------|------|
| `~/Desktop/github/scripts/` | 多语言脚本合集（Python/Perl/R/awk/bash） |
| `~/code/scripts/scihub.py` | Sci-Hub 论文下载 |

## 搜索命令

```bash
# 全库搜索
rg "function_name" ~/code/ --follow

# 按语言
rg "umi_dedup" ~/code/perl/ --follow

# 找所有 Snakefile
find ~/code/ -name "Snakefile"

# 找所有 .nf (Nextflow)
find ~/code/ -name "*.nf"
```

## 相关页面

- [[nfcore-microbiome-pipelines]] — Nextflow 微生物组流程 42 个
- [[nfcore-variant-calling-pipelines]] — 变异检测流程 30 个
- [[nfcore-rnaseq-pipelines]] — RNA-seq 流程 28 个
- `bioinfo-pipeline` — 生信流程标准与规范（domain，非独立页面）
