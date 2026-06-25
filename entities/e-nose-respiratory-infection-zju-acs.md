---
title: Graphene E-Nose Respiratory Infection Classification — 石墨烯电子鼻呼吸道感染机器学习诊断
created: 2026-05-21
updated: 2026-05-21
type: entity
tags: [e-nose, graphene, respiratory-infection, SVM, random-forest, lasso, ensemble, breath-analysis]
domain: machine-learning
sources:
  - raw/articles/Graphene-E-Nose-Respiratory-Infection-ZJU-ACS.md
  - raw/articles/AdvSci-徐可课题组-AI电子鼻闻出病毒.md
confidence: moderate
---

# Graphene E-Nose Respiratory Infection Classification
## 石墨烯电子鼻 × 机器学习区分细菌/病毒感染

## 研究概览

浙江大学邬建敏团队在 **ACS Nano 2025** 报道了一种基于还原氧化石墨烯（rGO）阵列的电子鼻（e-nose）传感器，通过 MOFs 和金属酞菁（MPcs）修饰 + 多重还原方法构建 8 位点交叉反应传感阵列，**结合三种机器学习模型**区分细菌性和病毒性呼吸道感染。^[raw/articles/Graphene-E-Nose-Respiratory-Infection-ZJU-ACS.md]

## 数据集

| 项目 | 数值 |
|------|------|
| 临床呼气样本（EB） | 145 例 |
| 细菌感染 | 89 例 |
| 病毒感染 | 56 例 |
| 独立外部测试集 | 43 例（含 6 例未明确病例） |
| 特征提取 | 动态曲线下积分面积（RA）作为关键特征 |

## 机器学习模型架构

研究对比了四种分类方案，核心 ML 环节集中在 **特征工程 + 单模型对比 + 模型融合**：

### 模型清单

| 模型 | 类型 | 作用 |
|------|------|------|
| **SVM**（支持向量机） | 监督分类 | 核方法高维映射，区分细菌/病毒呼出气指纹 |
| **Random Forest**（随机森林） | 集成学习（Bagging） | 多树投票，处理交叉反应传感阵列的非线性响应 |
| **Lasso 回归** | L1 正则化线性模型 | 特征选择 + 分类，天然筛选传感器位点的贡献度 |
| **加权融合模型** | 模型集成 | 加权集成 SVM/RF/Lasso 的输出 |

### 特征工程

- 原始信号：传感器对呼气样本的动态电阻响应曲线
- 关键特征：**RA（Recovery Area，恢复面积）** — 响应曲线下的积分面积
- 对比实验：单独使用 RA 特征 vs 响应值（response value）vs 两者结合
- 结论：**RA + 响应值组合** + 加权融合模型效果最佳

### 性能对比

| 方案 | 训练集准确率 | 验证集准确率 | 验证集 AUC | 外部测试准确率 | 外部测试 AUC |
|------|------------|------------|-----------|--------------|------------|
| SVM | — | — | — | — | — |
| RF | — | — | — | — | — |
| Lasso | — | — | — | — | — |
| **加权融合** | — | **83.7%** | **0.87** | **75.7%** | **0.81** |

> 原文侧重加权融合模型整体性能，未给出各单模型在验证集上的单独拆分数据。加权融合对 6 例未明确病例也提供了分类输出，辅助临床决策。

### 辅助分析方法

| 方法 | 用途 | 结果 |
|------|------|------|
| **PCA**（主成分分析） | 高维传感响应的可视化降维 | 年龄/性别分组不显著影响聚类 |
| **LDA**（线性判别分析） | 区分致病类型 | 肺炎支原体感染区分准确率 **75%** |
| **OPLS-DA**（正交偏最小二乘判别分析） | 模拟呼出气测试区分 | 丙酮（细菌标志物）vs 异戊二烯（病毒标志物）分离良好 |

## 临床应用价值

- **非侵入性**：呼出气体（EB）检测，无需抽血或痰液
- **快速**：实时传感响应，适合 POCT 场景
- **IoT 兼容**：传感器阵列小型化潜力，可连接物联网远程监测
- **精准用药**：区分细菌 vs 病毒感染 → 减少抗生素滥用

## 与同类研究的对比

- 传统呼吸道感染诊断：血液检测 / 抗原检测 / PCR 培养 — 有创或耗时
- 其他呼气诊断方法：GC-MS 质谱法 — 设备昂贵、便携性差
- 本方案优势：rGO 传感器阵列成本低 + 多 ML 模型融合提升鲁棒性

## 交叉引用

- [[concepts/svm]] — SVM 支持向量机，本文使用的核心分类器之一
- [[concepts/random-forest]] — Random Forest 随机森林，本文使用的集成方法
- [[concepts/bayesian-optimization]] — BO 超参数调优，本文未使用但 RF + e-nose 场景可扩展方向
