---
title: SVM — 支持向量机
created: 2026-05-20
updated: 2026-05-20
type: concept
tags: [classification, kernel-trick, max-margin, supervised-learning, support-vector]
domain: machine-learning
sources: [raw/articles/SVM支持向量机算法详解-机器学习和人工智能AI.md]
confidence: high
---

# SVM — 支持向量机

## 核心摘要

支持向量机（Support Vector Machine, SVM）是一种基于**最大间隔**（Max-Margin）原理的监督学习分类器。核心思想是找到一个最优超平面，使不同类别之间最近的样本点（支持向量）到该平面的距离最大化。通过核技巧（Kernel Trick）可扩展至非线性可分问题。

## 核心机制

### 线性 SVM（硬间隔）

对于二分类问题，给定训练集 $\{(x_i, y_i)\}$，$y_i \in \{-1, +1\}$，SVM 求解以下优化问题：

$$
\min_{w, b} \frac{1}{2} \|w\|^2 \quad \text{s.t.} \quad y_i(w^T x_i + b) \geq 1, \forall i
$$

- $w$：超平面法向量
- $b$：偏置项
- 决策函数：$f(x) = \text{sign}(w^T x + b)$

### 软间隔与 C 参数

实际数据往往线性不可分，引入松弛变量 $\xi_i$ 和惩罚参数 $C$：

$$
\min_{w, b, \xi} \frac{1}{2} \|w\|^2 + C \sum_i \xi_i
$$

- $C$ 大 → 对误分类惩罚大 → 边界窄但训练误差小（可能过拟合）
- $C$ 小 → 对误分类容忍大 → 边界宽但训练误差大（可能欠拟合）

### 核技巧（Kernel Trick）

将原始特征映射到高维空间，在高维空间中找到线性超平面。常用核函数：

| 核函数 | 公式 | 适用场景 |
|--------|------|----------|
| 线性核 | $K(x_i, x_j) = x_i^T x_j$ | 线性可分，特征维度高 |
| 多项式核 | $K(x_i, x_j) = (x_i^T x_j + r)^d$ | 低维非线性，需指定度数 $d$ |
| RBF（高斯）核 | $K(x_i, x_j) = \exp(-\gamma \|x_i - x_j\|^2)$ | 最常用，默认首选，$\gamma$ 控制影响半径 |
| Sigmoid 核 | $K(x_i, x_j) = \tanh(\kappa x_i^T x_j + \theta)$ | 类似神经网络 |

## SVM vs Random Forest

| 维度 | SVM | Random Forest |
|------|-----|---------------|
| 理论基础 | 凸优化 + 最大间隔 | Bagging + 决策树集成 |
| 非线性处理 | 核函数隐式映射 | 树结构天然非线性 |
| 高维数据 | 优秀（依赖核选择） | 良好（特征随机筛选） |
| 大规模数据 | 训练 $O(n^2 \sim n^3)$，扩展性差 | 可并行，扩展性好 |
| 可解释性 | 差（核空间不可解释） | 中等（特征重要性） |
| 多分类 | 需 OvO/OvR 策略 | 原生支持 |
| 概率输出 | 需 Platt 缩放 | 可直接输出概率 |

## 生物信息学应用

- **CNV 过滤**：如 [[cnvpipe]] 使用 SVM 对 5 个特征（读深模式、SNP 等位基因频率、GC 含量等）进行分类，过滤假阳性 CNV
- **微阵列分类**：基因表达谱疾病分类（癌症亚型预测），高维小样本场景 SVM 尤为适合
- **蛋白质亚细胞定位**：基于序列特征预测蛋白质位置
- **miRNA 靶标预测**：基于序列和结构特征分类
- **SNP 致病性预测**：基于基因组特征的功能预测
- **呼气诊断分类**：如 [[entities/e-nose-respiratory-infection-zju-acs]] 使用 SVM 结合 RF/Lasso 加权融合模型区分细菌/病毒性呼吸道感染，电子鼻传感器阵列信号作为特征输入

## 交叉引用

- [[entities/cnvpipe]] — CNV 检测流程，使用 SVM 进行机器学习过滤，5 特征输入
- [[random-forest]] — 同为 ML 分类器，适用场景互补
