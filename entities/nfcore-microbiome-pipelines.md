---
title: nf-core / Nextflow 生信流程 — 微生物组 & 宏基因组
created: 2026-05-06
updated: 2026-05-06
type: entity
tags: [pipeline, microbiome, metagenomics, ngs, mngs, tngs]
domain: bioinfo-pipeline
sources:
  - raw/articles/Nextflow工作流清单100-微生物组转录组变异三代测序.md
  - raw/articles/Nextflow工作流清单100-比对注释单细胞101-200.md
confidence: high
---

# nf-core / Nextflow 生信流程：微生物组 & 宏基因组

> 来源：逻捷科技微信公众号 biostack，两期 Nextflow 清单汇总
> 流程数量：42 个（#1–36 + #101–106）

本页收录 nf-core 及社区 Nextflow 流程中与微生物组、宏基因组、病原微生物检测相关的流程，涵盖宏基因组组装与分箱、扩增子分析、病毒检测、耐药基因分析、病原体监测等方向。

与 [[tngs-vs-mngs]]、[[kingcreate-tngs-panel]]、[[kingcreate-kcq]] 直接相关。

## 流程清单

### Microbiome: 宏基因组分析

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 1 | metatdenovo | 宏转录组组装与注释（原核+真核） | https://github.com/nf-core/metatdenovo |
| 2 | mag | 宏基因组混合组装与分箱 | https://nf-co.re/mag (Krakau et al. NAR Genomics Bioinform 2022) |
| 3 | taxprofiler | 高通量多分类学 profiling（shotgun 短/长读长） | https://github.com/nf-core/taxprofiler |
| 4 | funcscan | （宏）基因组功能与天然产物基因筛选 | https://github.com/nf-core/funcscan |
| 5 | ampliseq | 扩增子测序分析（DADA2 + QIIME2） | https://github.com/nf-core/ampliseq |
| 6 | bacass | 简单细菌组装与注释 | https://github.com/nf-core/bacass |
| 7 | viralrecon | 病毒样本组装与宿主内/低频变异 calling | https://github.com/nf-core/viralrecon |
| 8 | bactmap | 基于比对的细菌全基因组系统发育构建 | https://github.com/nf-core/bactmap |
| 10 | phageannotator | 鉴定、注释、定量（宏）基因组中噬菌体序列 | https://github.com/nf-core/phageannotator |
| 11 | detaxizer | 检查 fastq 文件中特定分类群，可选过滤 | https://github.com/nf-core/detaxizer |
| 12 | metapep | 宏基因组→表位分析 | https://github.com/nf-core/metapep |
| 13 | pathogensurveillance | 利用群体基因组学与测序进行病原体监测 | https://github.com/nf-core/pathogensurveillance |
| 14 | vipr | 病毒样本组装与低频变异 calling | https://github.com/nf-core/vipr |
| 15 | createtaxdb | 并行自动化构建宏基因组分类器数据库 | https://github.com/nf-core/createtaxdb |
| 23 | viralintegration | 嵌合 read 方法鉴定基因组中病毒整合事件 | https://github.com/nf-core/viralintegration |

### Microbiome: 社区流程

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 16 | MEDI | 基于宏基因组的人类饮食摄入估算 | https://github.com/Gibbons-Lab/medi (Diener & Gibbons, bioRxiv 2024) |
| 17 | MUFFIN | 混合组装+差异分箱：宏基因组/转录组/通路分析 | https://github.com/RVanDamme/MUFFIN |
| 18 | YAMP | 又一个宏基因组流程 | https://github.com/alesssia/YAMP |
| 19 | emg-viral-pipeline | 病毒检测、注释、分类（VIRify） | https://github.com/EBI-Metagenomics/emg-viral-pipeline (Rangel-Pineros et al. PLOS Comput Biol 2023) |
| 20 | WIP | 噬菌体序列可扩展鉴定与分析 | https://github.com/MarquetMike/WIP (Marquet et al. GigaScience 2022) |
| 21 | QuasiFlow | HIV-1 耐药 NGS 数据分析 | https://github.com/asekagiri/QuasiFlow (Ssekagiri et al. Bioinform Adv 2022) |
| 22 | sourmash-nf | 输入基因组上运行 sourmash | https://github.com/fmalmeida/sourmash-nf |
| 24 | HiFi-16S-workflow | PacBio HiFi 全长 16S 数据分析 | https://github.com/PacificBiosciences/HiFi-16S-workflow |
| 25 | Arcadia-Science/metagenomics | 宏基因组 QC/评估/profiling（短+长读长） | https://github.com/Arcadia-Science/metagenomics |
| 26 | MeRaGENE | 宏基因组快速基因鉴定 | https://github.com/metagenomics/MeRaGENE |
| 27 | nf_wochenende | 宏基因组比对与归一化（长+短读长） | https://github.com/MHH-RCUG/nf_wochenende |
| 28 | PHoeNIx | 医疗相关与耐药病原体短读长流程 | https://github.com/CDCgov/phoenix |
| 29 | virID | 病毒鉴定与发现 | https://github.com/jnoms/virID |
| 30 | meta-sweeper | 模拟微生物群落参数扫描 | https://github.com/cerebis/meta-sweeper |
| 31 | TADA | 靶向扩增子多样性分析（DADA2 聚焦） | https://github.com/h3abionet/TADA |
| 32 | metagenomics | 宏基因组 QC/评估/profiling（短+长读长） | https://github.com/Arcadia-Science/metagenomics |
| 33 | ONTrack2 | MinION 生物多样性追踪 | https://github.com/MaestSi/ONTrack2 |
| 34 | ampa-nf | 快速自动化预测蛋白抗菌区域 | https://github.com/cbcrg/ampa-nf |
| 35 | WasteFlow | 废水中病原体监测 | https://github.com/BCCDC-PHL/WasteFlow |
| 36 | v-met | 基于 Kraken 2 的简易病毒宏基因组流程 | https://github.com/ksumngs/v-met |

### Microbiome: 第二期续

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 101 | vAMPirus | 自动化病毒扩增子测序分析 | https://github.com/Aveglia/vAMPirus |
| 102 | Earth-Biogenome-Project-pilot | 地球生物基因组计划 pilot 组装与注释 | https://github.com/NBISweden/Earth-Biogenome-Project-pilot |
| 103 | MetaPhage | 噬菌体自动化分析与分类（宏基因组） | https://github.com/MetaPhage (Pandolfo et al. mSystems 2022) |
| 104 | NextITS | 真菌/真核生物 ITS 条形码分析（PacBio） | https://github.com/vmikk/NextITS |
| 105 | taxtriage | CDC 推荐临床环境分类学筛选 | https://github.com/jhuapl-bio/taxtriage |
| 106 | PhyloMagnet | 基于基因中心组装与系统发生的任意谱系筛选 | https://github.com/maxemil/PhyloMagnet |

## 关键流程推荐

- **nf-core/mag** — 宏基因组 binning 金标准流程，可直接用于 mNGS 数据分箱
- **nf-core/taxprofiler** — 多分类器并行 profiling，适合临床 mNGS 评估
- **nf-core/ampliseq** — 16S/ITS 扩增子分析，tNGS panel 设计参考
- **PHoeNIx** — CDC 出品，医疗相关耐药病原体分析，与 [[kingcreate-kcq]] 的耐药检测互补
- **QuasiFlow** — HIV-1 耐药 NGS 分析，可参考其准种 calling 策略
- **WasteFlow** — 废水监测流程，mNGS 筛查场景参考

## 交叉引用

- [[tngs-vs-mngs]] — tNGS 与 mNGS 方法对比，本页流程可在 mNGS 端提供分析工具选型
- [[kingcreate-kcq]] — KCQ 绝对定量系统，与 taxprofiler/mag 在定量层面有可对比维度
- [[wisecondorx]] — CNV 检测，微生物组流程可与宿主 CNV 分析联动
- [[nfcore-variant-calling-pipelines]] — 微生物组流程中 virID/PHoeNIx 可产出变异结果
- [[gbt46943-2025-mngs-standard]] — mNGS 性能确认国家标准，生信流程临界点/ROC曲线要求
