---
title: KingCreate tNGS Panel — RP100 呼吸道病原多重检测试剂盒 (金圻睿)
created: 2026-05-04
updated: 2026-05-04
type: entity
tags: [company, product, tngs, mp-tngs, hc-tngs, panel]
sources: [raw/papers/wanghui-tngs-ebiomedicine-2024.md]
confidence: high
---

# KingCreate tNGS Panel

广州金圻睿 (Guangzhou KingCreate Biotechnology Co., Ltd.) 开发的靶向测序 (tNGS) 产品体系，包括基于多重PCR (mp-tNGS) 和杂交捕获 (hc-tNGS) 两种技术路线的病原检测方案。与北京大学人民医院王辉团队合作完成临床验证 (EBioMedicine 2024)。

## 产品构成

### RP100™ Respiratory Pathogen Multiplex Testing Kit
- mp-tNGS 专用试剂盒
- 覆盖 198 种临床常见病原体
- 引物设计：GC 40-60%，长度 18-26bp，Tm ~60°C，避免 self-dimers/hairpins/cross-dimers
- 300-plex+ 多重扩增，关键病原体（MTBC, SARS-CoV-2 等）每条靶标 >=5 条引物
- 测序：Illumina MiniSeq SE100, MiniSeq Rapid Reagent Kit (100 cycles), 0.1M reads/样本
- 文库定量：Equalbit DNA HS Assay Kit (Vazyme) + Qubit 4.0, 阈值 >=0.5 ng/uL

### MetaCAP™ Pathogen Capture Metagenomic Assay Kit
- hc-tNGS 专用试剂盒
- 覆盖 3060+ 种病原体；数百万条探针覆盖种特异基因/保守基因/耐药基因/毒力基因
- 最佳探针用量：0.3 fmol (测试范围 0.1-0.4 fmol)
- 杂交温度：60°C (杂交) / 65°C (capture) / 68°C (washing)
- 杂交时间：30 min (从12h优化而来以匹配临床TAT)
- 测序：MiniSeq/NextSeq 550Dx SE100, 1M reads/样本
- 配套：Reverse Transcription Kit + Library Construction Kit (KingCreate)

## 技术参数

### Biopformatics Pipeline

| 环节 | mp-tNGS | hc-tNGS |
|---|---|---|
| 质控/修剪 | Fastp v0.20.1 (default) | Fastp v0.23.1 |
| 宿主去除 | 不需要 (PCR富集) | BWA v0.7.17-r1188 → hg38 |
| 比对 | Bowtie2 v2.4.1 (very-sensitive) | 分类参考数据库比对 |
| 参考数据库 | 683细菌 + 372病毒 + 349真菌 | 11,958细菌 + 7,373病毒 + 1,714真菌 + 343寄生虫 |
| 标准化 | RPhK (reads per 100K) | RPM (reads per million) |
| 阈值: 病毒 | **7 RPhK** | **6 RPM** |
| 阈值: 细菌 | **15 RPhK** | **10 RPM** |
| 阈值: 真菌 | **11 RPhK** | **4 RPM** |
| LoD判定 | RPhK>=15 + coverage>=1 或 RPhK>=45 (4/4 replicates) | RPM>=30 (4/4 replicates) |
| 线性R2 | 0.83 (SD 0.14) | 0.91 (SD 0.07) |

### LoD 详细数据 (Table 1)

| 病原体 | 类型 | mp-tNGS LoD | hc-tNGS LoD |
|---|---|---|---|
| K. pneumoniae ATCC 10031 | G-菌 | 50 CFU/mL | 50 CFU/mL |
| L. pneumophila ATCC 33152 | G-菌 | 450 CFU/mL | 450 CFU/mL |
| S. pneumoniae ATCC 49619 | G+菌 | 150 CFU/mL | 150 CFU/mL |
| S. aureus ATCC BAA-976 | G+菌 | 450 CFU/mL | 150 CFU/mL |
| C. albicans ATCC 14053 | 真菌 | 50 CFU/mL | 150 CFU/mL |
| A. fumigatus ATCC 96918 | 真菌 | 150 CFU/mL | 50 CFU/mL |
| C. gattii ATCC 34877 | 真菌 | 50 CFU/mL | 150 CFU/mL |
| Influenza A (H1N1) | RNA病毒 | 150 copies/mL | 150 copies/mL |
| Influenza B (B/Victoria) | RNA病毒 | 50 copies/mL | 50 copies/mL |
| HSV1 BDS-IQC-032 | DNA病毒 | 150 copies/mL | 50 copies/mL |

### 精密度

| 指标 | mp-tNGS | hc-tNGS |
|---|---|---|
| Intra-assay 一致性 | 100% | 100% |
| Intra-assay CV | 28.15% (SD 10.50%) | 14.30% (SD 8.98%) |
| Inter-assay 一致性 | 100% | 100% |
| Inter-assay CV | 51.56% (SD 17.96%) | 54.82% (SD 16.25%) |

### 干扰测试
- **高基因组相似性**: mp-tNGS 不受干扰；hc-tNGS 在 Streptococcus spp. 和 Aspergillus spp. 有轻微交叉检出
- **宿主干扰**: mp-tNGS 不受影响；hc-tNGS 受宿主背景轻微影响
- **竞争性干扰**: 两种方法均受影响，mp-tNGS bias 更大

## 临床性能 (王辉团队合作验证)
- 前瞻性 n=251, 双盲设计, ChiCTR2300073837 (2023-07-24)
- 样本: BALF, 北京大学人民医院
- 回顾性 n=229 建立阈值 (2022.05-2023.05)
- 前瞻性 n=251 验证 (2023.07-2023.09)

| 指标 | mp-tNGS | hc-tNGS | mNGS |
|---|---|---|---|
| 灵敏度 (复合参考) | 86.5% | 87.3% | 85.5% |
| 特异度 (复合参考) | 90.0% | 88.0% | 92.1% |
| 检出率 | 84.3% | 89.5% | 88.5% |
| Virus ROC AUC | 0.932 | 0.952 | — |
| Bacteria ROC AUC | 0.846 | 0.874 | — |
| Fungi ROC AUC | 0.833 | 0.802 | — |

## 成本
- mp-tNGS: ~120 USD/样本
- hc-tNGS: ~300 USD/样本
- mNGS DNA: ~500 USD/样本; mNGS DNA+RNA: ~900 USD/样本

## 优势与局限

**优势：**
- 同时检测 DNA + RNA (单流程, mNGS 需分别建库)
- P. jirovecii 检测能力显著优于 mNGS (mNGS宿主去除步骤破坏滋养体)
- 成本为 mNGS 的 1/4~1/2
- 快速 TAT (mp-tNGS 10.3h)
- 检出率显著高于传统微生物检测 (P<0.001)
- tNGS 覆盖 >95% 病例 (mp-tNGS: 245/251, hc-tNGS: 249/251)

**局限：**
- 厌氧菌 panel 覆盖不足 (mp-tNGS 未纳入, 避免扩增偏倚)
- 丝状真菌 (Mucorales, A. niger complex 等) 检测弱于 mNGS
- NTM 检测弱于 mNGS (无优势独特基因探针)
- mNGS 检出 6 例 tNGS 漏检的丝状真菌
- P. aeruginosa / S. maltophilia 检测偏弱 (常见试剂污染)
- R&D 周期长, 靶标数量受限 (mp-tNGS)
- 流程复杂, TAT 较长 (hc-tNGS)

## 数据公开
- SRA: SUB14206958
- 临床注册: ChiCTR2300073837
- 参考文献: PMID 39226681

## 相关页面
- [[kingcreate-kcq]] — 金圻睿KCQ mNGS绝对定量系统
- [[tngs-vs-mngs]] — tNGS 与 mNGS 全面对比
- [[cgp-probe-design-consensus-2026]] — 肿瘤CGP探针设计专家共识（探针设计规范对照参考）
