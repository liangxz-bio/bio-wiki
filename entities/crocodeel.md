---
title: CroCoDeEL
created: 2026-05-10
updated: 2026-05-10
type: entity
tags: [bioinformatics, metagenomics, qc, contamination-detection]
domain: bioinfo-pipeline
sources: [raw/articles/CroCoDeEL-宏基因组交叉样本污染检测-NatCommun.md, raw/papers/goulet2026-crocodeel-nc.md]
confidence: medium
---

# CroCoDeEL

CroCoDeEL (CROss-sample COntamination DEtection and Estimation of its Level) — 无需阴性对照的宏基因组交叉样本污染检测与定量工具。发表于 Nature Communications 2026。

**作者:** Lindsay Goulet, Florian Plaza Onate, Alexandre Famechon, Benoit Quinquis, Eugeni Belda, Edi Prifti, Emmanuelle Le Chatelier & Guillaume Gautreau (INRAE, Université Paris-Saclay)
**代码:** [github.com/metagenopolis/CroCoDeEL](https://github.com/metagenopolis/CroCoDeEL) (GPL v3, v1.0.3)
**数据:** ENA PRJEB83730
**训练数据管道:** [github.com/metagenopolis/CroCoDeEL_auxillary](https://github.com/metagenopolis/CroCoDeEL_auxillary) (Zenodo 10.5281/zenodo.19349649)

## 核心原理

交叉样本污染可视为样本稀释，污染特异性物种在受污染样本和污染源之间的相对丰度呈**比例关系**，比例系数 = 污染率。在对数尺度散点图上表现为一条"**污染线**"。

## 算法流程（四步）

1. **候选物种选择** — 筛选在污染源中更丰富、左上象限少有其他物种的点
2. **RANSAC 回归** — 在候选点上拟合 y = x + b 线性模型，< 5 个内点判定为未污染
3. **特征提取** — 10 个数值特征（SHAP 重要性排序），包括内点数量、线紧致性、高丰度物种缺失度、正交残差、线延展性等
4. **随机森林分类** — 1000 棵树，置信度 > 0.5 → 污染 + 污染率估计 (r = 10⁻ᵇ)

训练数据: 11 个队列 15,203 样本 → 13,330 样本对（70% 训练 / 30% 测试），72.3% 肠道→肠道污染

## 关键性能

| 指标 | 值 |
|------|-----|
| Matthews 相关系数 | **0.69** |
| 召回率 | **95%** |
| 特异性 | **50%** |
| 低污染检测阈值 | **0.1%** |
| 污染率 20% + 1M reads + Meteor2 | **100%** 检测率 |
| 污染率 2% + 10M reads + Meteor2 | **92%** 检测率 |

## 重要发现

- 第二类假阴性：级联污染（cascade contamination）— 样本 A 被 C 污染，样本 B 也被 C 污染，A 与 B 之间出现模糊污染线
- 工具选择强烈影响灵敏度：**Meteor2 > Sylph > MetaPhlAn4**（MetaPhlAn4 系统性低估次优势物种）
- 菌株共享方法 vs CroCoDeEL：2 vs 12+ 个污染事件（板 P3 案例）
- Lou 研究 P2 板中，母体菌株"垂直传播"实际是 23% 污染率的假象
- Ferretti 研究 80% (45/56) 报告菌株可能是污染；移除受污染样本后 t1 高 alpha 多样性不再显著（p=0.005→0.12）
- TwinsUK 队列 202/1004 受污染，Bray-Curtis 相异性 0.40 vs 0.53（p<0.0001）
- 计算 O(n²)，~100 样本几分钟内完成，CPU 可扩展性 ~0.85

## 局限性

- 复杂级联污染场景可能漏检
- 样本固有相似性高时可能假阳性
- 设计为决策支持工具，需结合人工审核

## 技术依赖

Python 3, scikit-learn (RandomForest + RANSAC), Pandas, Matplotlib, multiprocessing

## 相关概念

- [[entities/diamond-deepclust]] — 同为生物信息学方法工具，DIAMOND DeepClust 提供行星级蛋白质序列聚类
- [[gbt46943-2025-mngs-standard]] — mNGS 性能确认国家标准（QC要求）
- [[kraken2-classification]] — Kraken2 物种分类（CroCoDeEL 依赖的 profiler）
- [[random-forest]] — Random Forest 分类器（CroCoDeEL 污染检测的核心算法引擎）
