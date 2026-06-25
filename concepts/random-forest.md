---
title: Random Forest — 随机森林
created: 2026-05-20
updated: 2026-05-20
type: concept
tags: [bagging, ensemble-learning, decision-tree, feature-importance, classification, regression]
domain: machine-learning
sources: [raw/articles/随机森林算法全面讲解-机器学习和人工智能AI.md]
confidence: high
---

# Random Forest — 随机森林

## 核心摘要

随机森林（Random Forest）是一种基于 Bagging（Bootstrap Aggregating）的集成学习方法，通过构建**多棵决策树**并引入**双重随机性**（样本随机 + 特征随机）来降低过拟合、提高泛化能力。分类任务采用多数投票，回归任务取均值。

## 核心机制

### 双重随机性

1. **样本随机（Bootstrap Sampling）**：从原始数据集中有放回地抽取 $N$ 个子样本集，每棵树只用约 63.2% 的原始数据（剩余为 OOB 样本，用于无偏评估）
2. **特征随机（Random Subspace）**：每棵树节点分裂时，随机选取 $m$ 个特征（$\approx \sqrt{p}$ 分类 / $p/3$ 回归）再选最佳分裂点

### 分裂准则

| 任务 | 准则 | 公式 |
|------|------|------|
| 分类 | Gini 不纯度 | $\text{Gini}(D) = 1 - \sum_{k=1}^{K} p_k^2$ |
| 回归 | 均方误差（MSE） | $\text{MSE}(D) = \frac{1}{|D|}\sum_{i \in D} (y_i - \bar{y})^2$ |

选择使 Gini/MSE 下降最多的特征-阈值对进行分裂。

### 预测

- **分类**：$\hat{y} = \arg\max_{c} \sum_{t=1}^{T} \mathbb{1}[h_t(x) = c]$
- **回归**：$\hat{y} = \frac{1}{T}\sum_{t=1}^{T} h_t(x)$

## 关键特点

### 优势

- **高准确率**：多树集成，降低单棵树方差
- **抗过拟合**：双重随机性使单棵树即使过拟合，集成后偏差不会累积
- **高维兼容**：特征随机筛选使模型在 $p \gg n$ 场景依然有效
- **特征重要性**：可直接输出 permutation importance / Gini importance，用于特征筛选
- **缺失值容忍**：可基于 OOB 估计或代理分裂处理缺失
- **并行化友好**：每棵树独立训练，无跨树依赖

### 劣势

- **计算成本较高**：$T$ 棵树 $\Rightarrow$ 训练/预测时间约为单决策树的 $T$ 倍
- **内存占用大**：需存储所有树结构
- **可解释性弱**：不如单决策树直观，特征重要性为全局近似
- **极高维稀疏数据效果不佳**（如文本 bag-of-words）：特征随机选择效果有限
- **对高度相关特征敏感**：多样性降低，集成收益下降

## 复杂度

| 阶段 | 时间复杂度 |
|------|-------------|
| 训练 | $O(T \cdot n \cdot p \cdot d)$ |
| 预测 | $O(T \cdot d)$ |

其中 $T$ = 树数量，$n$ = 样本数，$p$ = 特征数，$d$ = 树深度。

## 应用场景

随机森林在生物信息学中的典型应用：

- **差异特征筛选**：高维组学数据（转录组/蛋白组/代谢组）特征重要性排序，筛选 biomarker 候选
- **微生物组分类**：16S / 宏基因组物种级别分类（如 QIIME 2 内置 RF classifier）
- **污染检测**：如 [[crocodeel]] 使用 1000 棵 Random Forest 进行宏基因组交叉样本污染分类 ^[raw/articles/随机森林算法全面讲解-机器学习和人工智能AI.md]
- **癌症早筛模型**：多模态特征（cfDNA 片段组、甲基化、蛋白标志物）集成分类器
- **耐药性预测**：基于基因组突变特征的 AMR 表型预测
- **呼气诊断分类**：如 [[entities/e-nose-respiratory-infection-zju-acs]] 使用 RF 结合 SVM/Lasso 加权融合模型区分细菌/病毒性呼吸道感染，145 例临床呼气样本验证

## 与贝叶斯优化的结合

随机森林可以作为**贝叶斯优化的代理模型**来替代高斯过程（GP），形成 RF + BO 的超参数自动调优方案 ^[raw/articles/随机森林+贝叶斯优化-深夜努力写Python.md]。

RF 在此场景的优势：

- **计算效率**：RF 预测快，避免 GP 的 $O(n^3)$ 协方差矩阵计算
- **高维兼容**：RF 天然处理高维搜索空间，GP 在高维（$>20$）表现急剧下降
- **噪声容忍**：RF 集成结构比 GP 更稳健

详见 [[concepts/bayesian-optimization]]。

## 与 Boosting 对比

| 维度 | 随机森林（Bagging） | GBDT/XGBoost/LightGBM（Boosting） |
|------|---------------|------------------------|
| 树生成方式 | 并行，独立训练 | 串行，每棵树纠正前树的残差 |
| 过拟合倾向 | 低（双重随机天然防过拟合） | 高（需精心调参 + 早停） |
| 缺失值处理 | 原生支持（代理分裂） | 需预处理或使用特定实现 |
| 高维数据 | 适合 | 一般（易过拟合） |
| 训练速度 | 可完全并行，快 | 串行，慢 |
| 预测性能 | 强基线，稳定 | 通常更高，但调参成本大 |

## 交叉引用

- [[entities/crocodeel]] — 宏基因组污染检测工具，使用 RandomForest + RANSAC 做污染分类 ^[raw/articles/随机森林算法全面讲解-机器学习和人工智能AI.md]
- [[concepts/kraken2-classification]] — Kraken2 基于 k-mer 的物种分类（对比 RF 的基于特征分裂的机制）
- [[cnvpipe]] — CNV 检测流程使用 SVM（同为 ML 方法，适用于算法对比）
- [[svm]] — SVM 支持向量机（对比 RF 的双重随机 vs 最大间隔）
