---
source_url: https://mp.weixin.qq.com/s/48xoJ52OZU9PGPPjRlrRTA
ingested: 2026-05-08
sha256: 7ffac32efe224a02d2a831a8952e7ba19454feae5c8b900dfd56f9a6a16ea430
citation: "芈小小. PacBio HiFi数据进行SV分析流程. 一问一答-2024启航++（微信公众号）. 2025."
domain: bioinfo-pipeline
---
# PacBio HiFi数据进行SV分析流程

> **来源：** 微信公众号「一问一答-2024启航++」 | **作者：** 芈小小
> **原文链接：** https://mp.weixin.qq.com/s/48xoJ52OZU9PGPPjRlrRTA

---

## 文章概述

本文介绍了使用 PacBio HiFi 数据进行**结构变异（Structural Variation, SV）分析**的标准化流程，涵盖从数据预处理到结果可视化的完整步骤，并基于常用生物信息学工具进行说明。

---

## 完整流程

### 第一步：数据预处理

若原始数据为 **subreads 格式**，需使用 PacBio 的 `ccs` 工具生成 HiFi reads：

```bash
ccs input.subreads.bam output.hifi.bam --min-passes 3 --min-rq 0.99
```

- **输出格式：** BAM 或 FASTQ
- **用途：** 供后续分析步骤使用

---

### 第二步：序列比对

使用 `pbmm2` 将 HiFi reads 比对到参考基因组：

```bash
pbmm2 index ref.fasta ref.mmi
```

- **输出：** 排序并索引的比对文件（**BAM + BAI**）

---

### 第三步：SV 检测

使用 `pbsv` 工具进行 SV 检测，分**两步**执行：

1. **发现信号（Discover）**
2. **调用变异（Call）**

- **输出格式：** VCF 格式的 SV 检测结果

---

### 第四步：变异注释

使用 `AnnotSV` 对 SV 结果进行功能注释：

```bash
annotsv -svinputfile output.pbsv.vcf \
        -outputfile annotated.annotsv.tsv \
        -genomebuild grch38 \
        -annotationmode full
```

> 若同时分析其他变异类型（SNV/Indel），可结合 **ANNOVAR** 或 **SnpEff** 进行注释。

---

### 第五步：变异过滤

根据质量、深度、频率等指标过滤 SV 结果，例如使用 `SnpSift`：

```bash
java -jar snpsift.jar filter \
     "(qual > 30) & (dp > 10)" \
     annotated.annotsv.tsv > filtered.svs.tsv
```

- **过滤维度：** 质量值（QUAL）、测序深度（DP）、变异频率等
- 也可根据**注释信息**筛选具有潜在生物学意义的变异

---

### 第六步：结果可视化

使用 **IGV（Integrative Genomics Viewer）** 加载比对文件和 SV 结果，直观检查 SV 位点及 reads 比对情况：

```bash
igv.sh -g grch38 -b output.aligned.bam -v output.pbsv.vcf
```

---

## 注意事项

| 注意点 | 说明 |
|--------|------|
| **参数调整** | 流程中各工具的参数需根据数据质量和分析需求灵活调整，如比对参数、SV 检测阈值等 |
| **交叉验证** | 若需更高精度或处理复杂 SV 类型，可结合 **Sniffles2**、**CuteSV** 等工具进行交叉验证 |
| **计算资源** | 计算资源需求较高，建议在**高性能计算（HPC）环境**中运行 |

---

## 工具汇总

| 步骤 | 工具 | 功能 |
|------|------|------|
| 数据预处理 | `ccs` | 生成 HiFi reads |
| 序列比对 | `pbmm2` | HiFi reads 比对参考基因组 |
| SV 检测 | `pbsv` | 结构变异检测 |
| 变异注释 | `AnnotSV` / `ANNOVAR` / `SnpEff` | 功能注释 |
| 变异过滤 | `SnpSift` | 质量过滤 |
| 结果可视化 | `IGV` | 可视化检查 |
| 可选交叉验证 | `Sniffles2` / `CuteSV` | 复杂 SV 验证 |