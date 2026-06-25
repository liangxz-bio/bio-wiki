---
title: "ONT SV 结构变异分析流程"
created: 2026-05-08
updated: 2026-05-08
type: concept
tags: [bioinformatics, pipeline, sv, long-read, ont]
domain: bioinfo-pipeline
sources: [raw/articles/ONT数据进行SV分析流程-微信芈小小.md]
confidence: medium
---

# ONT SV 结构变异分析流程

ONT 三代测序数据的 SV 分析流程：minimap2 → cuteSV / Sniffles2 / SVIM → 多工具合并过滤 → VEP/ANNOVAR 注释 → matplotlib 可视化。适用于 DEL/INS/INV/DUP/BND 等结构变异类型的检测。

## 流程概览

| 步骤 | 工具 | 输出 |
|:----:|:----|:-----|
| ① 数据比对 | `minimap2 -ax map-ont` | sorted BAM + BAI |
| ② SV 检测 (三种方案) | `cuteSV` / `Sniffles2` / `SVIM` | 各工具 VCF |
| ③ 结果合并过滤 | Python 多工具交叉验证 (≥2 工具支持 + QUAL>20) | merged TSV |
| ④ 注释 | VEP / ANNOVAR | annotated VCF |
| ⑤ 可视化 | matplotlib + pysam (覆盖深度图) | PNG |

## 核心工具参数

- **minimap2**: `-ax map-ont -t 30`, 含 `--paf-no-hit` 跳过无比对 reads
- **SVIM**: `--min_sv_size 50`，针对 ONT 长 reads 的 split-read + read-depth 联合检测
- **cuteSV**: `--genotype -l 50 -s 5`，支持 DEL/INS/INV/DUP/BND 分型
- **Sniffles2**: 简化为单命令 `sniffles -i bam -o vcf -t 30`
- **合并策略**: 至少 2 个工具同时检出且 QUAL>20 才保留

## 与 PacBio HiFi SV 流程对比

| 维度 | ONT (本流程) | PacBio HiFi |
|:----|:-------------|:------------|
| 比对工具 | minimap2 (map-ont) | pbmm2 |
| 主流 SV caller | cuteSV / Sniffles2 / SVIM / NanoSV | pbsv |
| 多工具验证 | 显式交叉验证脚本 | Sniffles2 / CuteSV 补充 |
| 注释 | VEP / ANNOVAR | AnnotSV / ANNOVAR / SnpEff |
| 可视化 | matplotlib + pysam 自定义 | IGV 人工审阅 |

## Cross-references

- [[sv-analysis-pacbio-hifi]] — PacBio HiFi 对应的 SV 流程（姊妹篇）
- [[nfcore-variant-calling-pipelines]] — 包含 SV calling 的 Nextflow 流程
- [[wisecondorx]] — sWGS CNV 检测（另一种结构变异检测方法）
