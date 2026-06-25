---
title: FeatureMAP — Feature-preserving manifold approximation and projection
created: 2026-06-03
updated: 2026-06-03
type: concept
tags: [bioinformatics, single-cell, dimensionality-reduction, manifold-learning]
domain: bioinfo-pipeline
sources:
  - raw/papers/yang2026-featuremap-natcompsci
confidence: high
---

# FeatureMAP — Feature-preserving manifold approximation and projection

FeatureMAP（feature-preserving manifold approximation and projection）是 Yang Yang 等人于 2026 年发表于 *Nature Computational Science* 的单细胞数据降维可视化方法。核心创新在于：在保留 UMAP 拓扑结构能力的基础上，通过**局部切空间嵌入 (pairwise tangent space embedding)** 系统性地恢复了非线性降维中丢失的源特征（基因层面）信息。

> Yang Y, Gong J, Sun H, et al. Feature-preserving manifold approximation and projection to analyze single-cell data. *Nat Comput Sci*, 2026. DOI: 10.1038/s43588-026-00970-6

---

## 1. 动机与问题

UMAP / t-SNE 等非线性降维方法擅长保留聚类结构，但存在根本性缺陷：

- **特征信息丢失**：非线性投影只保留距离度量，基因（特征）的层级信息被隐式编码在距离计算中，无法在嵌入空间中直接解释
- **无法回答"什么基因驱动了这个轨迹"**：需要降维后再做一步 DGE 分析，而非在可视化中直接体现
- **密度失真**：UMAP 自适应长度尺度选择破坏了局部密度结构

FeatureMAP 将降维从纯结构任务重构为**解释性任务**——基因不仅是计算距离的输入，而是在低维嵌入中显式表示的要素。

## 2. 方法框架

### 2.1 拓扑空间与切空间逼近

- 构建 kNN 图近似流形拓扑结构（同 UMAP 的加权指数分布核）
- 对每个数据点的 kNN 进行**局部 SVD**（加权 PCA），估计 d 维切空间 $T_x\mathcal{M}$
- 切空间基 = 前 d 个右奇异向量 $V = [\mathbf{v}_1, ..., \mathbf{v}_d]$
- 奇异值 $[\sigma_1, ..., \sigma_d]$ 定义了各向异性半径（hyperellipsoid），编码局部密度

### 2.2 特征变异 (Feature Variation)

切空间中第 i 个特征的**特征变异**（feature variation）定义为：

$$\parallel \mathbf{f}_i \parallel = \left( \sum_{l=1}^d |v_{li}|^2 \right)^{1/2}$$

即该特征在 d 个主方向上 loading 的 L2 范数。与传统的"基因表达"（位置）和"表达变异"（各向同性散布）不同，特征变异衡量的是**各向异性的、沿局部分化轨迹的方向性变化**——可以理解为基因变化的"速度向量"。

### 2.3 双嵌入策略 (Dual Embedding)

FeatureMAP 产出两个互补的低维嵌入：

| 嵌入 | 保留目标 | 用途 |
|------|----------|------|
| **GEX embedding** (gene expression) | 各向异性密度 + 特征 loading | 密度峰值 → 核心状态检测，局部基因贡献投影 |
| **GVA embedding** (gene variation) | 切空间对齐 + 变异向量场 | 基因变异轨迹追踪，轨迹分叉解析 |

**为何需要双嵌入**：密度保持（标量半径，奇异的标量值）和轨迹保持（方向对齐，奇异向量）在二维投影中无法同时最大化——两者是正交的，因此必须分离编码。

### 2.4 损失函数

GEX 嵌入优化联合损失：

$$\mathcal{L} = \text{CE}(P \| Q) - \lambda \cdot \text{Corr}(r^o, r^e)$$

- 第一项：交叉熵保留 kNN 图拓扑（同 UMAP）
- 第二项：各向异性半径的 Pearson 相关系数（推广 densMAP 的各向同性球→各向异性椭球）

## 3. 三大核心概念

### 3.1 基因贡献 (Gene Contribution)

- **定义**：由特征变异导出的各基因在嵌入空间的贡献度
- **与现有概念的区别**：
  - 基因表达 → 细胞在转录状态空间中的位置
  - 表达变异 → 位置周围的各向同性散布
  - **特征变异（基因贡献）→ 沿分化轨迹的方向性变化速率（≈"速度"）**
- **效用**：发现方差分析无法识别的、驱动状态转换的先行调控信号

### 3.2 基因变异轨迹 (Gene Variation Trajectory)

- GVA 嵌入中对变异向量场的投影
- 将具有相似方向变化模式的细胞分组，揭示精细尺度的细胞轨迹
- 与基于密度路径的山脊估计 (ridge estimation) 一致

### 3.3 核心状态 vs 转换状态 (Core and Transition States)

基于三个拓扑指标进行量化分类：

| 指标 | 含义 | 核心状态 | 转换状态 |
|------|------|---------|---------|
| **密度 (density)** | 局部细胞密度 | 高 | 低 |
| **曲率 (curvature)** | 流形局部弯曲度 | 低（几何平滑） | 高（结构不稳定） |
| **介数中心性 (betweenness centrality)** | 细胞在流形图中的桥梁程度 | 低 | 高 |

- 核心状态 = 高密度 + 低曲率 + 低中心性（稳定态）
- 转换状态 = 低密度 + 高曲率 + 高中心性（过渡态）
- GVA 嵌入中呈现**结-线模式 (knot-and-thread pattern)**：结 = 稳定核心，线 = 动态过渡

## 4. DGV 分析 (Differential Gene Variation)

在核心概念基础上的关键分析流程：

1. 找到 GVA 嵌入中的相邻核心-转换状态对
2. 比较转换状态 vs 核心状态的基因变异得分
3. 鉴定在过渡区显著上调的调控基因

**与传统 DGE 的核心差异**：
- DGE：比较 GEX 聚类之间的表达均值 → 找到差异表达基因
- DGV：比较 GVA 中转换态 vs 核心态的特征变异 → 找到**驱动过渡的调控基因**
- 在合成数据中验证：DGV 成功识别真实 GRN 中的主调控因子 g4，而 DGE 错误地优先识别了下游 g6

## 5. 实验验证

### 5.1 合成数据（bifurcation GRN）
- FeatureMAP 恢复双叉结构 + 识别 GRN 主调控因子 g4（DGE 漏检）
- 密度保持 $R^2 = 0.660$（UMAP $R^2 = 0.003$）

### 5.2 小鼠胰腺发育 (E15.5)
- 解析 Fev+ 祖细胞→alpha/alpha 两系分叉
- DGV 鉴定 Arx（alpha 系）和 Pdx1（beta 系）为顶级调控因子
- 核心状态基因变异显著低于全聚类（$P=0.008$）

### 5.3 CD8+ T 细胞耗竭 (LCMV clone 13)
- Exh-Int → Exh-KLR / Exh-Term 分叉轨迹解析
- DGV 鉴定 Zeb2（已实验验证为 Exh-KLR 关键驱动因子）排名第三
- **实验验证过渡态细胞**：用 KLRA9⁺ 标志物从 CX3CR1⁺LY108⁻ 中门选出过渡态群体，时序追踪符合 scRNA-seq 预测

## 6. 基准测试

| 维度 | FeatureMAP | UMAP | t-SNE | PHATE | densMAP |
|------|-----------|------|-------|-------|---------|
| 聚类结构 | ✓ | ✓ | ✓ | ✓ | ✓ |
| 特征投影 | **✓** | ✗ | ✗ | ✗ | ✗ |
| 密度保持 | ✓ ($R^2=0.60-0.66$) | ✗ ($R^2≈0$) | ✗ | ✓ | ✓ |
| 伪时间推理 | ✓ ($R^2>0.8$) | ✗ | ✗ | ✓ | ✗ |
| 核心/转换态分离 | **最佳** | ✗ | ✗ | 部分 | ✗ |
| 基因变异向量场 | **✓** | ✗ | ✗ | ✗ | ✗ |

## 7. 局限性

1. **局部线性假设**：local PCA 假设流形在 kNN 内局部线性，高曲率或稀疏采样区域可能失效
2. **固定本征维数**：d 全局固定，不适用于不同区域本征维数不同的数据集（如静息 vs 活跃分化）
3. **一阶近似**：只捕捉一阶 gene variation，不建模高阶基因-基因互作或协方差结构
4. **计算开销**：每个数据点都需要 local SVD + 切空间对齐，大规模数据（百万级细胞）需要近似策略

## 8. 关联概念

- [[scrnaseq-workflow]] — 标准 scRNA-seq 分析流程，FeatureMAP 可作为降维+轨迹一体替代方案
- [[fastsc]] — 单细胞分析 R 包，可集成 FeatureMAP 作为可视化模块
- [[random-forest]] — 机器学习方法，与 FeatureMAP 的密度/拓扑指标结合可用于细胞状态分类
- [[kraken2-classification]] — 物种分类方法对比：FeatureMAP 的 kNN + 切空间范式与 k-mer + LCA 的异同

## 9. 展望

- 整合 RNA velocity（scVelo）的转录动力学信息 → 几何基因变异 + 动力学
- 耦合深度生成模型（scVI）→ 可扩展至百万级细胞
- 扩展至多模态数据（ATAC + RNA）→ 跨模态特征变异的切空间形式
- 实验捕获转换态细胞的通用流程：DGV 筛选 → 标志物验证 → FACS 分选 → 功能验证
