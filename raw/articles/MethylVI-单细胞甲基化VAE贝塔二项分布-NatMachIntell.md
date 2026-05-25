---
source_url: https://mp.weixin.qq.com/s/wo-SbkjAPeYyH2cN5GYU4g
ingested: 2026-05-12
sha256: 29ed36678fe3879db954fed51b9f0eb98119ad657d42e39e0142fb71983bbced
citation: "Ashuach T, Armendariz DA, Hu GK, et al. MethylVI: a variational autoencoder for modeling single-cell methylation data. Nat Mach Intell. 2025;7:572-583. doi:10.1038/s42256-025-01011-5."
domain: bioinfo-pipeline
---

# 单细胞甲基化数据终于有了像scVI一样好用的工具——MethylVI把贝塔二项分布塞进了VAE

**作者：** CellSpace AI
**来源：** CellSpace AI 微信公众号
**发布平台：** 微信公众号（原创）
**原文链接：** https://mp.weixin.qq.com/s/wo-SbkjAPeYyH2cN5GYU4g

---

## 摘要

本文介绍了发表于 **Nature Machine Intelligence** 上的工具 **MethylVI**，由华盛顿大学 Su-In Lee 组联合 Salk 的 Joseph Ecker 组、UC Berkeley/Weizmann 的 Nir Yosef 组共同发表。MethylVI 是一个基于 VAE（变分自编码器）的概率生成模型，专为单细胞亚硫酸氢盐测序（scBS-seq）数据设计，直接对原始 count 对建模，集成于 scvi-tools，与 scanpy/scverse 生态无缝对接。

---

## 背景：为什么需要 MethylVI？

单细胞甲基化数据（如 snmC-seq、snmCAT-seq）长期缺乏专用的下游分析工具，现有流程存在以下问题：

- **现状**：将 count 值 normalize 成 M 值或 methylation fraction → log 转换 → PCA/Wilcoxon，沿用 bulk 时代 pipeline
- **核心问题**：
  - 每个细胞每个区域的覆盖度差异极大（有些位置仅 3 个胞嘧啶，有些 200 个）
  - 将离散 count 数据强行 normalize 成连续值再做 Gaussian 假设，会引入大量假阳性
  - scRNA-seq 有 scVI，scATAC-seq 有 PeakVI，但单细胞甲基化领域一直缺乏对应工具

---

## 核心方法：为什么是贝塔二项分布？

### scBS-seq 数据特点

snmC-seq 输出每个胞嘧啶位点的二元甲基化状态，但单细胞水平覆盖度极稀疏。实际分析时，将 cytosine-level 测量按预定义 genomic region 聚合为两个数：

- **y**：该区域被测到甲基化的胞嘧啶数
- **n**：该区域总共被测到的胞嘧啶数

### 为何不用二项分布？

二项分布在 bulk BS-seq 时代已被证明 **dispersion 不足**——真实数据方差比二项分布预测的更大，因为同一区域内不同胞嘧啶的甲基化概率并不独立，存在 local spatial correlation。

### 贝塔二项分布的优势

将甲基化概率 `p` 本身建模为贝塔分布的随机变量，在二项分布上叠加一层先验，可以处理过分散（overdispersion）问题。

### MethylVI 模型架构

| 组件 | 描述 |
|------|------|
| **输入** | 每个细胞的 (y 向量, n 向量) 对 |
| **Encoder** | 将输入压缩到低维潜变量 z |
| **Decoder** | 从 z 解出每个区域的 μ 参数（平均甲基化水平） |
| **Dispersion** | γ 作为 region-specific 全局参数，用变分 Bayes 优化 |
| **似然函数** | Beta-Binomial（贝塔二项分布） |
| **Loss** | 标准 ELBO（Beta-Binomial log likelihood + KL 散度，均有闭式解） |
| **CpG/CpH** | 分开作为两组 feature 输入同一 encoder，共享 z |
| **训练** | batch size=128，Adam 优化器，大多数数据集几分钟训完 |
| **实现** | 基于 Pyro，集成于 scvi-tools |

---

## 主要评估结果

### 1. 降噪后 Marker Gene 信号更干净（vs ALLCools）

**测试数据**：Luo 2017 mouse frontal cortex snmC-seq（3373 个神经元）

**评估方法**：利用 CpH gene body methylation 与基因表达负相关的已知生物学规律，用 Kolmogorov-Smirnov 检验量化 cell type 间甲基化差异强度。

**结果**：
- MethylVI 系统性优于 ALLCools normalize procedure（**P < 1e-6**，binomial test）
- **Arpp21**：原文标注为 pan-excitatory marker，MethylVI 在抑制性神经元中发现明显双峰分布，揭示 CGE-derived 与 MGE-derived 抑制性神经元的差异（在 Liu 2021 大规模 atlas 中可复现）
- **Adgra3**：MethylVI 进一步区分 MGE-derived 细胞中的 Pvalb+ 和 Sst+ 亚群

---

### 2. 差异甲基化基因（DMG）检测：精度媲美 scMET，速度快数十倍

**方法**：基于 Bayes factor，从两组细胞的 posterior 中采样 latent z，decode 出 μ 参数，比较 p(μ_A > μ_B) 与 p(μ_A ≤ μ_B)。

**Ground truth**：Mo 2015 bulk MethylC-seq 纯化兴奋性 vs 抑制性神经元数据（limma 结果）

**性能对比**：

| 方法 | 与 bulk 结果相关性 | 速度 | q 值 inflation |
|------|-------------------|------|----------------|
| **MethylVI** | ✅ 最强（与 scMET 持平） | **< 2 分钟** | 无 |
| scMET | ✅ 最强 | > 24 小时 | 无 |
| Wilcoxon | ❌ 差 | 快 | **严重 inflation** |
| DSS | ❌ 差 | — | **严重 inflation** |
| ArchR/Signac/SnapATAC2/EpiScanpy | ❌ 差 | — | **严重 inflation** |
| Gaussian VAE | ❌ 差 | — | — |
| Binomial VAE | ❌ 差 | — | — |

> ⚠️ **重要提醒**：Wilcoxon test 在 normalize 后的甲基化数据上违反独立性假设，FDR 控制基本失效，会产生大量假阳性。

**额外发现的 DMG**（跨 modality 验证）：
- **Adcyap1r1**：抑制性神经元中低甲基化，与 PAC1 蛋白免疫染色结果一致
- **Arc**：兴奋性神经元中低甲基化，与 motor cortex scRNA-seq 结果一致

---

### 3. 数据整合：跨 protocol 的 batch correction

**测试数据**：Liu 2021 mouse brain atlas dentate gyrus 子集（snmC-seq2 + snm-3C-seq 两种 protocol）

**方法**：假设 latent z 与 batch label 独立，实现内置整合。

**评估指标**：scIB benchmark 13 个指标（NMI、ARI、ASW、kBET、iLISI、graph connectivity 等）

**结果**：MethylVI overall score 排第一，在 bio-conservation 和 batch correction 两个维度均优于：
- fastMNN、Harmony、Scanorama、Seurat（原为 scRNA-seq 设计）
- PeakVI、Gaussian VAE、Binomial VAE（VAE 消融对比）

---

### 4. MethylANVI + scArches：Reference Mapping

**MethylANVI**：将 cell type label 作为额外 latent variable，变为 semi-supervised 模型（借鉴 scANVI 思路）。

**scArches 整合**：transfer learning 框架，新数据只需 fine-tune 部分权重即可映射到 reference space。

**测试**：Luo 2022 human frontal cortex atlas（7 个样本，4 种 protocol）
- 用 6 个样本 + 3 种 protocol 建 reference
- 第 4 种 protocol（snmC-seq2）的 query 数据成功映射到对应 cell type cluster
- Hold-out 实验：reference 中未见过的 cell type 在 query 中聚成独立 cluster，不会被强行归入已有类别

---

### 5. MultiMethylVI：RNA + 甲基化多组学

**适用场景**：snmCAT-seq 等同时测 RNA 和甲基化的技术

**模型设计**：

| 组件 | 设计 |
|------|------|
| RNA 端 | 负二项分布 |
| 甲基化端 | Beta-Binomial |
| Latent | z_R（RNA）+ z_M（甲基化）取平均得统一 z |
| 正则化 | symmetric Jeffrey's divergence 惩罚项 |

**Imputation 性能**：

| 基因表达水平 | Spearman 相关 |
|-------------|--------------|
| 整体 | 0.56 |
| log10 归一化表达 > -3（高表达） | **> 0.70** |
| log10 归一化表达 < -5（低表达） | 0.11 |

**典型 marker 基因 imputation 相关性**：ADARB2、FOXP2、RORB、CBLN2 均达到 **0.67–0.78**

> ⚠️ **注意**：低表达基因 imputation 不可靠，不建议使用。

---

## 实操建议

1. **Feature 选择很重要**
   - 默认使用 CpG 和 CpH gene body 作为 feature
   - 可结合 MethSCAn（Kremer 2024）进行 data-driven feature 选择，DMG 性能更优
   - 不同分析目的可选用 regulatory element、enhancer、CpG island 等不同 region 定义

2. **非神经元组织**
   - CpH methylation 在非脑组织中水平很低，贡献有限
   - 建议只用 CpG feature 运行

3. **与现有 pipeline 兼容**
   - 与 scanpy/AnnData/MuData 完全兼容
   - 只需将 "normalize + PCA" 替换为 "训 MethylVI + 取 latent"，其余流程不变

4. **可扩展性**
   - 单卡消费级 GPU，几万细胞数据集只需几分钟到十几分钟
   - 百万级 scBS-seq atlas 也可处理
   - scMET 等 MCMC 方法在此量级完全不可行

---

## 当前局限性

| 局限 | 描述 |
|------|------|
| 空间信息缺失 | 未显式利用 genome 上相邻 region 间的 spatial correlation，未来可引入 graph-based 或 transformer 建模 |
| 多组学扩展 | MultiMethylVI 目前仅支持 RNA + 甲基化，尚未整合 ATAC（snm3C-seq 等技术已支持 3D 基因组 + 甲基化） |
| Benchmark 局限 | 主要在脑组织数据上验证，免疫、肿瘤等 batch effect 更复杂场景的表现有待验证 |

---

## 总结

MethylVI 填补了单细胞甲基化数据分析领域长期缺失的概率建模工具空白，核心贡献在于：

- ✅ 用 **Beta-Binomial 似然**正确建模甲基化 count 数据的过分散特性
- ✅ **VAE 框架**实现高效训练，速度远超 MCMC 方法
- ✅ 内置 **batch correction**，支持跨 protocol 数据整合
- ✅ 支持 **reference mapping**（MethylANVI + scArches）
- ✅ 支持 **多组学整合**（MultiMethylVI）
- ✅ 完全集成于 **scvi-tools / scverse 生态**
