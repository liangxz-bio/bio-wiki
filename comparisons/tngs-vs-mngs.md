---
title: tNGS vs mNGS — 下呼吸道感染病原检测方法对比
created: 2026-05-04
updated: 2026-05-04
type: comparison
tags: [comparison, tngs, mngs, mp-tngs, hc-tngs, lrti, infection]
sources: [raw/papers/wanghui-tngs-ebiomedicine-2024.md]
confidence: high
---

# tNGS vs mNGS：下呼吸道感染病原检测

依据 王辉团队 2024 EBioMedicine 前瞻性研究 (n=251) 数据对比三种 NGS 方法。

## 总体对比

| 维度 | mp-tNGS | hc-tNGS | mNGS |
|---|---|---|---|
| 技术原理 | 多重PCR富集目标区域 | 探针杂交捕获目标微生物 | 乌枪法宏基因组测序 |
| 靶标范围 | 198种（预设panel） | 3060+种（预设panel） | >20000种（无需预设） |
| 宿主去除 | 不需要（PCR选择性扩增） | 不需要（探针捕获富集） | 需要（saponin裂解差减） |
| DNA/RNA共检 | 是，单流程 | 是，单流程 | 否，需分别建库 |
| 测序数据量 | 0.1M reads (最小24.2K) | 1M reads (最小63.1K) | 20M reads |
| 测序平台 | MiniSeq SE100 | MiniSeq/NextSeq 550Dx SE100 | NextSeq 550Dx SE75 |
| TAT | 10.3h | 16-23h | 16-24h |
| 成本 | ~120 USD | ~300 USD | DNA ~500; DNA+RNA ~900 USD |
| 成本比例 | mNGS的1/4 | mNGS的1/2~1/3 | 基准 |
| LoD | 50-450 CFU/mL | 50-450 CFU/mL | — |

## 生物信息流程对比

```
mp-tNGS:  Fastp v0.20.1 → Bowtie2 v2.4.1 (very-sensitive) → RPhK
          参考DB: 683 bac / 372 virus / 349 fungi
          阈值: Virus 7, Bacteria 15, Fungi 11 RPhK

hc-tNGS:  Fastp v0.23.1 → BWA v0.7.17 (hg38) → 分类DB → RPM
          参考DB: 11,958 bac / 7,373 virus / 1,714 fungi / 343 parasites
          阈值: Virus 6, Bacteria 10, Fungi 4 RPM

mNGS:     Trimmomatic v0.40 → Bowtie2 v2.4.3 (hg19) → Kraken2 v2.1.0 → RPM
          参考DB: PlusPF (>24,000 pathogens)
          阈值: Virus: RPM ratio ≥3 + ≥3 distinct regions; Bac/Fungi: RPM ratio ≥10
```

## 诊断性能

### vs 复合参考标准（临床诊断+所有微生物检测）
| 指标 | mp-tNGS | hc-tNGS | mNGS |
|---|---|---|---|
| 灵敏度 | 86.5% | 87.3% | 85.5% |
| 特异度 | 90.0% | 88.0% | 92.1% |
| 检出率 | 84.3% | 89.5% | 88.5% |

### vs 培养
| 指标 | mp-tNGS | hc-tNGS |
|---|---|---|
| 灵敏度 | 94.7% (82.3-99.4%) | 89.5% (75.2-97.1%) |
| 特异度 | 57.8% (50.3-64.9%) | 56.0% (48.7-63.2%) |

### ROC AUC
| 病原分类 | mp-tNGS | hc-tNGS |
|---|---|---|
| 全部病原 | >=0.822 | >=0.822 |
| 病毒 | 0.932 | 0.952 |
| 细菌 | 0.846 | 0.874 |
| 真菌 | 0.833 | 0.802 |

## 检测偏好分析

### tNGS 优于 mNGS 的场景
- **P. jirovecii** — tNGS 检出7例 mNGS 漏检（原因：人源背景干扰、低载量）
- **RNA病毒** — tNGS 检出5例 mNGS 未检出（原因：mNGS 未要求 RNA 流程）
- **MTBC（结核分枝杆菌复合群）** — tNGS 检出能力优于 Xpert-MTB
- **低载量病原** — mp-tNGS 在多例低载量样本中检出阳性
- **成本敏感场景** — 费用为 mNGS 的 1/4 到 1/2

### mNGS 优于 tNGS 的场景
- **丝状真菌**（Mucorales, Aspergillus niger complex, Rhizopus oryzae 等）— mNGS 检出6例 tNGS 漏检
- **NTM（非结核分枝杆菌）** — mNGS 检出率更高，且能精准分型
- **厌氧菌** — mp-tNGS 漏检16/21个厌氧菌（集中在吸人性肺炎/肺脓肿患者）
- **罕见病原体** — mNGS 覆盖范围远超预设 panel
- **重症/免疫抑制患者** — 需要全谱排查时 mNGS 更优

## 干扰因素

| 方法 | 干扰类型 |
|---|---|
| mp-tNGS | 高浓度病原体间竞争性抑制（类似多通道qPCR），部分近缘种非特异性扩增 |
| hc-tNGS | 宿主背景干扰（类似mNGS），高基因组相似性微生物的探针容错导致交叉检出 |
| mNGS | 宿主背景干扰（人源占比 >90% 时大量 reads 浪费） |

## 工作流差异

```
mp-tNGS:  提取 → 逆转录 → 多重PCR预扩增 → 建库 → SE100 MiniSeq → Fastp → Bowtie2 → RPhK
hc-tNGS:  提取 → 建库 → 探针杂交捕获 → 洗脱 → 测序 → 生信分析 → RPM
mNGS:     提取 → 宿主去除 → 建库 → 测序 → 宿主序列过滤 → 比对 → RPM/相对丰度
```

## 适用场景推荐

| 场景 | 推荐方法 |
|---|---|
| 普通肺炎/社区获得性肺炎 | mp-tNGS (低成本+快速) |
| 免疫抑制患者不明原因发热 | hc-tNGS (广谱+RNA共检) |
| 疑似结核/NTM | hc-tNGS (物种分型) |
| 重症/罕见病原体排查 | mNGS (全谱覆盖) |
| 吸人性肺炎/肺脓肿 | hc-tNGS 或 mNGS (厌氧菌覆盖) |
| 疑似丝状真菌感染 | mNGS 优先 |
| RNA病毒流行季筛查 | tNGS (DNA/RNA共检优势) |

## 相关页面
- [[kingcreate-tngs-panel]] — 金圻睿 tNGS 产品体系
- [[gbt46943-2025-mngs-standard]] — mNGS性能确认国家标准（明确排除tNGS）
- [[wang-hui-peking-university]] — 研究团队
