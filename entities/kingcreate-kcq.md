---
title: KingCreate KCQ — mNGS绝对定量系统
created: 2026-05-04
updated: 2026-05-04
type: entity
tags: [company, product, quantification, internal-standard, mngs, absolute-quant]
sources: [raw/papers/wanghu2024-kcq-microbgenom.md]
confidence: high
---

# KingCreate KCQ (KingCreate-Quantification) 系统

广州金圻睿 (KingCreate Biotechnology) 开发的 mNGS 绝对定量系统。通过掺入15条合成dsDNA内参（IS），利用线性回归模型将微生物 reads 转换为绝对拷贝数。

## 内参（IS）设计

| 参数 | 值 |
|---|---|
| 内参数量 | 15条合成dsDNA分子 |
| 片段长度 | ~1000bp (970±10bp) |
| GC含量 | 35-70% |
| 连续碱基 | <4 |
| 同源性 | NCBI数据库中无同源序列 |
| 浓度梯度 | 5组 × 3条/组 |
| 梯度范围 | 2×10⁻¹ ~ 2×10⁻⁵ ng/µL |
| 合成厂商 | Sangon Biotech (上海) |
| 载体 | pUC57 → DH5α E. coli → Sanger测序验证 |
| 试剂盒货号 | KS647-KRDmN-48 (KingCreate) |

## 数学模型

### 核心假设
Pearson 分析显示 IS 投入量 (Cis) 与检测到的 IS 序列数 (Ris) 呈**强线性关系 (r² ≥ 0.99)**。假设微生物同样遵循此线性：

```
Cis / Ris = Cmic / Rmic
```
其中 Cis = IS投入量, Ris = IS reads数, Cmic = 微生物投入量, Rmic = 微生物reads数

### IS回归曲线
15个 IS 浓度梯度（5组×3条）产生的 (Ris, Cis) 数据点拟合回归曲线：
```
Ris = α × Cis + β
```
通过外推获得 Cmic，再转换为拷贝数：
```
COPYmic = Cmic / (Lmic × DNAc)
```
其中 Lmic = 微生物基因组大小, DNAc = 单位DNA质量

分析代码: https://github.com/kcmngs/ISQ

## 方法性能

| 指标 | 值 |
|---|---|
| IS线性 r² | **≥0.99** |
| vs ddPCR 相关系数 | **R=0.97**（83个片段，36临床样本） |
| 验证样本数 | 36个临床样本（血/BALF/CSF/咽拭子） |
| 验证片段数 | 83个微生物遗传片段 |
| 推荐最低数据量 | **20M reads**（低于此CV波动大） |
| 宿主背景影响 | 极小（高低人类背景均不影响定量） |
| IS投入优化 | 2 µL（<1 µL数据不稳，>6 µL抢占病原数据） |

### 偏差菌种（P<0.005）
- *E. coli*（试剂污染）
- *S. pyogenes*
- *N. meningitidis*
- *A. baumannii*

## 测序与生信流程

| 环节 | 详情 |
|---|---|
| 建库 | KS619-DNAmN24 试剂盒 (KingCreate)，片段 250-350bp |
| IS掺入时机 | **建库阶段**（非提取前，**不校正提取效率**） |
| 测序平台 | Illumina NextSeq 550Dx, SE75bp |
| 数据量 | ~40 GB/run |
| 拆样 | bcl2fastq v2.20.0.422 |
| 质控 | Fastp v0.23.1（去接头/低质量/重复） |
| 宿主去除 | BWA v0.7.17-r1188 → hg38 |
| 比对 | RefSeq Release 211 (2022.03) + 自建IS数据库 |
| 定量 | 自开发流程 (github.com/kcmngs/ISQ): 线性回归模型 |
| 抽样验证 | seqkit v0.15.0 随机抽样20次 |
| ddPCR验证 | BIO-RAD QX200, EvaGreen Supermix |

## 不同数据量下的稳定性

| 数据量 | CV% (IS梯度reads比) | 定量稳定性 |
|---|---|---|
| 1-5M | 6-106% | 不稳定 |
| 20M | 6-30% | **推荐阈值** |
| 35-50M | 6-24% | 稳定 |

## 与文献中其他方法对比 (Table 1)

| 方法 | 内参设计 | 定量类型 | 适用mNGS? |
|---|---|---|---|
| 天然微生物基因组 | 自然界序列 | 相对定量 | 可能同源干扰 |
| 16S/ARGs天然序列 | 自然界序列 | 绝对定量 | 可能同源干扰 |
| 反向基因组序列 | 倒置序列 | 相对定量 | 可能同源干扰 |
| 商业质粒 | 质粒序列 | 相对定量 | 质粒环境污染 |
| 16S/ITS扩增子 | 引物区序列 | 绝对定量 | 仅tNGS，不适用mNGS |
| ddPCR直接结合 | 无内参 | 绝对定量 | 仅tNGS，不适用mNGS |
| **KCQ（本研究）** | **人工合成无同源dsDNA** | **绝对定量** | **适用mNGS** |

## KCQ vs qmNGS (Li 2021) 对比

| 维度 | KCQ (金圻睿) | qmNGS (Li 2021) |
|---|---|---|
| 应用场景 | 临床mNGS病原体定量 | 环境ARG定量 |
| 内参设计 | 15条dsDNA, 无同源 | 20个ISF, 终止密码子标记 |
| 掺入时机 | 建库阶段 | 建库前 |
| 校正提取效率 | ❌ | ❌ |
| 线性 r² | ≥0.99 | 0.98 |
| 验证 | vs ddPCR, R=0.97 | vs qPCR, 无显著差异 |
| GitHub | github.com/kcmngs/ISQ | 未公开 |

## 局限
- IS 在建库阶段加入，**不校正提取效率**
- 验证样本量有限（36例）
- 4种菌种存在显著偏差（E. coli, S. pyogenes, N. meningitidis, A. baumannii）
- 不同品牌试剂背景污染差异大
- 短链 IS 回收效率受提取方法影响

## 相关页面
- [[kingcreate-tngs-panel]] — 金圻睿tNGS产品体系
- [[qmngs-internal-standards]] — qmNGS环境定量方法对比
- [[gbt46943-2025-mngs-standard]] — mNGS性能确认国家标准，LoD/精密度/准确度要求
