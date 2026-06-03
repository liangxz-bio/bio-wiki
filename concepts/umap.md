---
title: UMAP — Uniform Manifold Approximation and Projection
created: 2026-06-03
updated: 2026-06-03
type: concept
tags: [bioinformatics, dimensionality-reduction, manifold-learning, visualization]
domain: bioinfo-pipeline
sources:
  - raw/papers/mcinnes2018-umap-joss
  - raw/articles/超全面讲透UMAP算法模型
confidence: high
---

# UMAP — Uniform Manifold Approximation and Projection

UMAP（Uniform Manifold Approximation and Projection）是由 McInnes, Healy & Melville 于 2018 年提出的非线性降维与可视化方法。与 t-SNE 相比，它在保留全局结构、计算速度和可扩展性上具有显著优势，是 scRNA-seq 等单细胞数据的标准降维工具。

> McInnes L, Healy J, Melville J. UMAP: Uniform Manifold Approximation and Projection for Dimension Reduction. *J Open Source Softw* 2018;3(29):861. arXiv:1802.03426. doi:10.21105/joss.00861

---

## 1. 核心思想

UMAP 建立在三个数学假设上：

1. **数据均匀分布在黎曼流形上** → 可以用一个覆盖所有点的模糊单纯集（fuzzy simplicial set）来近似流形
2. **黎曼度量是局部常数** → 在每个局部邻域内可用一个常数度量逼近
3. **流形是局部连通的** → 远近点之间的连接强度由概率定义

UMAP 的目标：在低维嵌入空间构建一个与该模糊单纯集**尽可能相似**的图结构。

## 2. 算法流程

### 2.1 高维空间：构建模糊 kNN 图

**Step 1**：对每个数据点 $x_i$，用近似最近邻算法找到 k 个最近邻

**Step 2**：对每个 $x_i$，定义与邻居 $x_j$ 的连接强度：

$$P_{j|i} = \exp\left(-(d(x_i, x_j) - \rho_i) / \sigma_i\right)$$

- $\rho_i$ = $x_i$ 到其最近邻居的距离（确保局部连通性）
- $\sigma_i$ = 通过二分搜索确定，使得 $\sum_j P_{j|i} = \log_2(k)$

**Step 3**：对称化得到无向图权重：

$$P_{ij} = P_{j|i} + P_{i|j} - P_{j|i} P_{i|j}$$

这形成高维空间的模糊单纯集表示。

### 2.2 低维空间：构建概率图

低维嵌入空间中，点 $y_i$ 和 $y_j$ 之间的连接强度用重尾分布（Student's t 分布族）定义：

$$Q_{ij} = (1 + a \cdot ||y_i - y_j||^{2b})^{-1}$$

默认参数 $a \approx 1.93$, $b \approx 0.79$（通过最小化 KL 散度与 t-SNE 的匹配度拟合得到）。

### 2.3 优化：最小化交叉熵

UMAP 通过最小化高维分布 $P$ 和低维分布 $Q$ 之间的交叉熵来优化嵌入坐标：

$$CE(P \| Q) = -\sum_{ij} P_{ij} \log Q_{ij} + (1 - P_{ij}) \log(1 - Q_{ij})$$

- 第一项（吸引项）：当 $P_{ij}$ 大而 $Q_{ij}$ 小时产生强梯度 → **拉近**在原始空间中相近的点
- 第二项（排斥项）：当 $P_{ij}$ 小而 $Q_{ij}$ 大时产生梯度 → **推开**在原始空间中不相近的点

优化方法：随机梯度下降（SGD），使用负采样加速。

## 3. 关键超参数

| 参数 | 默认值 | 作用 |
|------|-------|------|
| `n_neighbors` | 15 | 局部-全局结构平衡。小 → 侧重局部细节；大 → 保留全局结构 |
| `min_dist` | 0.1 | 嵌入点之间的最小距离。小 → 点紧密聚集（适合聚类）；大 → 点松散（适合轨迹） |
| `n_components` | 2 | 嵌入维度。2-3 用于可视化，更高维度可用于特征提取 |
| `metric` | euclidean | 距离度量类型（euclidean / cosine / correlation 等） |
| `random_state` | — | 随机种子。固定后结果可重复 |

### n_neighbors 的选择经验

| n_neighbors | 效果 | 适用范围 |
|-------------|------|---------|
| 5-10 | 强调局部结构，分群边界清晰 | 有明确离散类别的数据 |
| 15-30 | 局部-全局平衡 | scRNA-seq 标准配置 |
| 50-200 | 强调全局结构，轨迹连续 | 有连续分化过程的数据 |

## 4. UMAP vs t-SNE

| 维度 | UMAP | t-SNE |
|------|------|-------|
| **全局结构** | ✅ 能较好地保留 | ❌ 几乎完全丢失 |
| **速度** | 快（近似 kNN + 负采样） | 慢（全量概率计算） |
| **可扩展性** | 百万级可行 | 万级以上吃力 |
| **参数敏感性** | 较低，n_neighbors 影响可预期 | 较高，perplexity 影响敏感 |
| **密度保持** | ❌ 默认不保留（除非 densMAP） | ❌ 不保留 |
| **特征保留** | ❌ 不保留基因级信息 | ❌ 不保留 |

## 5. UMAP vs FeatureMAP

| 维度 | UMAP | FeatureMAP |
|------|------|-----------|
| 聚类结构 | ✅ | ✅ |
| 特征/基因贡献投影 | ❌ | ✅ Gene contribution |
| 变异轨迹追踪 | ❌ | ✅ GVA embedding |
| 核心/转换态 | ❌ | ✅ 密度+曲率+介数中心性 |
| 密度保持 | ❌ (densMAP 可部分解决) | ✅ 各向异性椭球保持 |
| 输出 | 单一 embedding | 双嵌入 (GEX + GVA) |

FeatureMAP 可视为 UMAP 在**特征保留**方向上的扩展——它复用了 UMAP 的 kNN 图和交叉熵框架，但增加了局部 PCA 切空间嵌入。

## 6. 关联概念

- [[featuremap]] — FeatureMAP，UMAP 的特征保留扩展，增加切空间嵌入 + DGV 分析
- [[scrnaseq-workflow]] — scRNA-seq 标准流程，UMAP 是降维可视化的默认选择
- [[random-forest]] — 可与 UMAP 结合：先 UMAP 降维，再 RF 分类
- [[svm]] — 同样可与 UMAP 结合用于嵌入空间的分类

## 7. 实现与资源

- **Python**: `pip install umap-learn` → `import umap`
- **R**: `library(umap)` / `library(uwot)`
- **scRNA-seq 工具集成**: Scanpy / Seurat 均内建 UMAP
- **官网与文档**: https://umap-learn.readthedocs.io/
