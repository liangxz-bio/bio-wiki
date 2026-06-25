---
title: "PacBio HiFi SV 结构变异分析流程"
created: 2026-05-08
updated: 2026-05-08
type: concept
tags: [bioinformatics, pipeline, sv, long-read]
domain: bioinfo-pipeline
sources: [raw/articles/PacBio-HiFi-SV结构变异分析完整流程.md]
confidence: medium
---

# PacBio HiFi SV 结构变异分析流程

PacBio HiFi 三代测序数据的标准 SV 分析流程：ccs → pbmm2 → pbsv → AnnotSV → SnpSift → IGV。适用于 DEL/INS/INV/DUP/BND 等结构变异类型的检测与注释。

## 六步流程

| 步骤 | 工具 | 输出 |
|:----:|:----|:-----|
| ① 数据预处理 | `ccs` (min-passes 3, min-rq 0.99) | HiFi BAM/FASTQ |
| ② 序列比对 | `pbmm2` | BAM + BAI |
| ③ SV 检测 | `pbsv` (discover → call) | VCF |
| ④ 变异注释 | `AnnotSV` / ANNOVAR / SnpEff | TSV |
| ⑤ 变异过滤 | `SnpSift` (QUAL>30, DP>10) | filtered TSV |
| ⑥ 可视化 | IGV | 人工审阅 |

## 交叉验证

复杂 SV 类型可结合 Sniffles2、CuteSV 补充验证。

## Cross-references

- [[nfcore-variant-calling-pipelines]] — 包含 SV calling 的 Nextflow 流程
- [[wisecondorx]] — sWGS CNV 检测（另一种结构变异检测方法）
