---
title: Bayesian Optimization — 贝叶斯优化
created: 2026-05-21
updated: 2026-05-21
type: concept
tags: [hyperparameter-optimization, surrogate-model, gaussian-process, acquisition-function, global-optimization]
domain: machine-learning
sources: [raw/articles/随机森林+贝叶斯优化-深夜努力写Python.md]
confidence: high
---

# Bayesian Optimization — 贝叶斯优化

## 核心摘要

贝叶斯优化（Bayesian Optimization，BO）是一种基于贝叶斯定理的**全局优化方法**，专用于优化评估代价高昂的黑箱函数（black-box function）。核心思路：构建**代理模型**（surrogate model）近似目标函数，通过**采集函数**（acquisition function）平衡探索（exploration）与开发（exploitation），迭代选择最有潜力的评估点，以最少评估次数逼近全局最优。

与网格搜索（Grid Search）和随机搜索（Random Search）相比，贝叶斯优化的核心优势是**样本效率**——在同等评估次数下找到更优的参数组合。

## 核心流程

### 标准五步迭代

1. **初始化**：用若干随机采样点 $\\{(x_1, y_1), ..., (x_n, y_n)\\}$ 训练初始代理模型
2. **代理模型预测**：对候选点 $x$，代理模型输出均值 $\\mu(x)$ 和不确定性（如方差 $\\sigma^2(x)$）
3. **采集函数计算**：利用 $\\mu(x)$ 和 $\\sigma^2(x)$ 计算每个候选点的采集函数值（如期望改进 EI）
4. **选择下一个点**：$x_{next} = \\arg\\max_x \\alpha(x)$，选择采集函数最大的点进行评估
5. **更新代理模型**：将新观测点 $(x_{next}, y_{next})$ 加入训练集，重训练代理模型，回到步骤 2

### 关键组件

| 组件 | 功能 | 常见选择 |
|------|------|----------|
| **代理模型** | 对目标函数建模，输出均值+不确定性 | 高斯过程（GP）、随机森林、TPE |
| **采集函数** | 量化每个候选点的潜在价值 | EI（期望改进）、PI（概率改进）、UCB（上置信界） |

## 两种代理模型对比

### 高斯过程（GP）作为代理模型

- **优点**：理论成熟、不确定性估计精确、小样本表现好
- **缺点**：$O(n^3)$ 计算复杂度，高维（>20维）表现差，对超参数（核函数）敏感

### 随机森林作为代理模型 ^[raw/articles/随机森林+贝叶斯优化-深夜努力写Python.md]

- **优点**：计算效率高（单次预测快）、天然处理高维数据、抗噪声、无需协方差矩阵计算
- **缺点**：不确定性估计不如 GP 精准、理论上不如 GP 优雅
- **适用场景**：目标函数复杂、评估维度高（>20）、评估次数较多

详见 [[concepts/random-forest]]。

## 采集函数详解

### 期望改进（Expected Improvement, EI）

$$EI(x) = \\mathbb{E}[\\max(f(x) - f^*, 0)] = (\\mu(x) - f^*)\\Phi(z) + \\sigma(x)\\phi(z)$$

其中 $f^*$ 是当前最优值，$z = (\\mu(x) - f^*) / \\sigma(x)$，$\\Phi$ 和 $\\phi$ 分别是标准正态的 CDF 和 PDF。

- 当 $\\sigma$ 大（不确定性高）→ 鼓励探索
- 当 $\\mu$ 接近 $f^*$（潜力大）→ 鼓励开发
- EI 天然平衡探索与开发，是 **贝叶斯优化最常用的采集函数**

### 概率改进（Probability of Improvement, PI）

$$PI(x) = P(f(x) > f^*) = \\Phi(z)$$

- 只考虑"是否可能更好"，不考虑"好多少"
- 易陷入局部最优，需添加探索参数 $\\xi$ 控制

### 上置信界（Upper Confidence Bound, UCB）

$$UCB(x) = \\mu(x) + \\kappa \\sigma(x)$$

- $\\kappa$ 控制探索强度：$\\kappa$ 大→偏向探索，$\\kappa$ 小→偏向开发
- 实现简单，超参数 $\\kappa$ 需要 tuning

## 随机森林 + 贝叶斯优化的内在逻辑 ^[raw/articles/随机森林+贝叶斯优化-深夜努力写Python.md]

当随机森林作为贝叶斯优化的代理模型时，其内在逻辑为：RF 通过多棵树集成对目标函数建模，预测值 = 所有树预测的均值，不确定性可通过树的方差估计。

**RF 做代理模型的优势：**

1. **高维非线性**：RF 天然处理高维度和复杂非线性关系，克服 GP 在高维空间的困难
2. **抗噪声**：RF 的集成结构对噪声有较强容忍度，避免 GP 在大数据量下不稳定的问题
3. **计算高效**：RF 训练和预测都快，避免 GP 的 $O(n^3)$ 协方差矩阵计算

**迭代步骤：**

1. 用当前数据训练 RF
2. RF 预测给出均值估计（Predictions） + 不确定性估计（树间方差）
3. 基于 RF 输出计算采集函数（EI/PI/UCB）
4. 选择采集函数最大的点作为下一个评估点
5. 加入新观测数据，更新 RF，重复

## 应用场景

- **机器学习超参数调优**：树数量、最大深度、学习率、正则化参数等组合搜索
- **AutoML**：自动化模型选择和超参数搜索（如 Hyperopt、Optuna 的 TPE 变体）
- **实验设计**：化学/生物实验中优化反应条件、培养基配方等
- **药物设计**：多目标分子优化（ADMET 属性空间探索，如 [[entities/boat]] 中的 BOAT 框架）

## 与 Grid Search / Random Search 对比

| 维度 | Grid Search | Random Search | Bayesian Optimization |
|------|-------------|---------------|----------------------|
| 搜索策略 | 穷举网格点 | 均匀随机采样 | 基于代理模型的引导搜索 |
| 样本效率 | 低 | 中 | 高（5-10 次通常找到满意解） |
| 高维扩展 | 维度灾难（$O(k^d)$） | 较好 | 较好（RF 代理模型） |
| 并行化 | 天然并行 | 天然并行 | 有限（需顺序更新代理模型） |
| 实现复杂度 | 简单 | 简单 | 中等（需要代理模型 + 采集函数） |
| 适用场景 | 低维、小搜索空间 | 无明确最优方向 | 评估代价高的任何场景 |

## 交叉引用

- [[concepts/random-forest]] — 随机森林作为贝叶斯优化的代理模型
- [[concepts/svm]] — 同为机器学习方法，超参数（C, gamma）优化常使用 BO
- [[entities/boat]] — BOAT 抗体多目标贝叶斯优化框架，使用 GP+GA 多属性帕累托搜索
- [[entities/crocodeel]] — CroCoDeEL 使用 RF+RANSAC 进行污染检测（RF 作为集成方法而非 BO 代理模型）
