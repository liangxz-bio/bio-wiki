---
title: Transformer — 自注意力机制与序列建模
created: 2026-05-20
updated: 2026-05-20
type: concept
tags: [attention, self-attention, multi-head, positional-encoding, lstm, time-series, deep-learning]
domain: machine-learning
sources: [raw/articles/Transformer结合LSTM时序预测详解-机器学习和人工智能AI.md]
confidence: high
---

# Transformer — 自注意力机制与序列建模

## 核心摘要

Transformer 是一种基于**自注意力（Self-Attention）**机制的序列建模架构，摒弃了 RNN/LSTM 的循环结构，完全通过注意力机制建立序列中任意两个位置之间的直接依赖。其核心组件包括：多头自注意力（Multi-Head Self-Attention）、位置编码（Positional Encoding）和前馈网络。Transformer 最初为 NLP 设计（Vaswani et al. 2017），现已成为生物信息学中蛋白质语言模型（ESM、ProtBERT）和基因组分析的基础架构。

## 核心机制

### 1. 自注意力（Scaled Dot-Product Attention）

输入序列 $X \in \mathbb{R}^{n \times d}$，通过三个线性变换得到 Q/K/V：

$$
Q = XW^Q, \quad K = XW^K, \quad V = XW^V
$$

$$
\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right) V
$$

- $QK^T$：计算每个位置与其他所有位置的相似度
- $\sqrt{d_k}$ 缩放因子：防止 softmax 梯度饱和

### 2. 多头注意力（Multi-Head Attention）

将 Q/K/V 投影到 $h$ 个子空间，并行计算注意力后拼接：

$$
\text{MultiHead}(Q, K, V) = \text{Concat}(\text{head}_1, \dots, \text{head}_h) W^O
$$

$$
\text{head}_i = \text{Attention}(QW_i^Q, KW_i^K, VW_i^V)
$$

多头允许模型在不同子空间中关注不同类型的关系。

### 3. 位置编码（Positional Encoding）

由于自注意力不具备序列顺序感知能力，通过正弦/余弦函数注入位置信息：

$$
PE_{(pos, 2i)} = \sin\left(\frac{pos}{10000^{2i/d}}\right)
$$
$$
PE_{(pos, 2i+1)} = \cos\left(\frac{pos}{10000^{2i/d}}\right)
$$

## Transformer + LSTM 融合架构

### 融合动机

| 模块 | 优势 | 局限 |
|------|------|------|
| Transformer | 长距离依赖捕捉，全局视野 | 局部连续性建模弱，计算量大 |
| LSTM | 局部时序建模，状态记忆 | 长距离依赖弱，梯度易消失 |

### 两种融合策略

1. **并行融合**：Transformer 和 LSTM 分别从输入并行提取全局/局部特征，拼接后经全连接层输出
2. **级联融合**：Transformer 输出作为附加特征输入 LSTM（或反之），使 LSTM 在处理局部信息时参考全局上下文

### 模型结构（并行为例）

```
输入序列 [batch, seq_len, 1]
    ├──→ TransformerEncoder → 全局特征 [batch, d_model]
    │       ├── 映射 + 位置编码
    │       └── Multi-Head Self-Attention × N 层
    └──→ LSTMModule → 局部特征 [batch, hidden_size]
            └── LSTM 门控循环 × N 层
    └──→ Concat → FC → 输出
```

## 生物信息学应用

- **蛋白质语言模型**：ESM / ProtBERT / ProtT5 均基于 Transformer 架构，用于结构预测、功能注释、突变效应预测
- **基因表达时间序列**：scRNA-seq 伪时间推断、基因调控网络推断
- **DNA 序列建模**：Enformer（CNN + Transformer）预测基因表达，DNA-BERT 用于调控元件识别
- **药物-靶标相互作用**：MolFormer / ChemBERTa 基于 Transformer 的分子表示学习
- **临床时间序列**：EHR 时序数据预测（ICU 事件、疾病进展）

## 交叉引用

- [[random-forest]] — 同为 ML 方法，RF 适用于表格数据，Transformer 适用于序列数据
- [[svm]] — 传统 ML vs 深度学习对比（SVM 小样本高维 vs Transformer 大规模序列）
- [[entities/crocodeel]] — 污染检测中使用 RandomForest，而 Transformer 可用于同类任务的序列特征提取
