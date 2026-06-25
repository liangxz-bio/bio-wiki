---
title: WisecondorX — 浅层全基因组CNV检测工具 (sWGS CNA)
created: 2026-05-04
updated: 2026-05-08
type: entity
tags: [bioinformatics, cnv, sWGS, NIPT, cancer, pipeline, normalization]
sources: [raw/papers/raman2019-wisecondorx-nar.md]
confidence: high
---

# WisecondorX — 浅层全基因组CNV检测工具

WisecondorX 是由比利时根特大学（Ghent University）开发的开源CNV检测工具，基于 WISECONDOR 的 within-sample 归一化算法改进而来，适用于 NIPT、肿瘤基因组、先天性异常等多种诊断场景。代码仓库：<https://github.com/CenterForMedicalGeneticsGhent/WisecondorX>

## 论文核心贡献

比较了 **6种 CNA 工具**的归一化性能（FREEC, QDNAseq, BIC-seq2, CNVkit, cn.MOPS, WISECONDOR），发现 WISECONDOR 的 **within-sample PCA 归一化**在所有指标上最优。但 WISECONDOR 仅限 NIPT 使用，存在速度慢、不支持性染色体、分辨率低等局限。WisecondorX 解决了这些问题，扩展到通用场景。

## CNV Calling 的三种归一化范式

sWGS 数据的 CNV calling 按照归一化策略可分为三大类：

### 1. Reference-free（参考无关）
不依赖正常样本参考集，直接用 GC/mappability 等基因组特征校正。
- **代表工具**：FREEC, QDNAseq, BIC-seq2
- **优点**：无需预先收集正常样本库
- **缺点**：噪声控制能力弱于参考集方法
- **适用**：实验条件稳定、正常样本不易获取的场景

### 2. Pooled reference（群体参考集）
使用一组（通常 ≥50-100 例）健康样本建立参考分布，与待测样本逐 bin 比较。
- **代表工具**：cn.MOPS, CNVkit, **WISECONDOR/WisecondorX**
- **优点**：消除批次效应和未知偏差，性能最优
- **缺点**：需要大量正常样本构建参考集
- **适用**：NIPT、先天性异常等诊断场景

### 3. Matched case-control（配对比较）
**同一患者的配对样本**直接对比，区分体细胞变异与胚系变异。例如肿瘤组织 vs 配对正常组织（或血细胞）。
- **代表工具**：BIC-seq2 (case-control mode), 少数软件支持
- **关键特性**：可将 minor-allele frequency (MAF) 作为辅助信息，帮助判断变异的来源（胚系 vs 体细胞）
- **局限**：需要每个患者多个高质量样本（成本高），不适用于 NIPT 等 cfDNA 策略

### 被比较的6种工具

| 工具 | 归一化策略 | 范式 | 特点 |
|------|-----------|:---:|------|
| FREEC | GC多项式拟合 + mappability | 参考无关 | 基础方法 |
| QDNAseq | loess 校正 GC/mappability | 参考无关 | 保守blacklist（mask 14% 基因组） |
| BIC-seq2 | 准Poisson GAM, 唯一可比对窗口 | 参考无关 / **可配对** | bin大小自适应 |
| cn.MOPS | 混合Poisson分布 | 参考集 | 每个locus独立概率模型 |
| CNVkit | 参考集直接归一化 + rolling median | 参考集 | 次优表现，异常样本失败 |
| **WISECONDOR** | **PCA + Euclidean distance within-sample** | 参考集 | **所有指标最佳，但仅NIPT** |

### 归一化性能评估维度
- **Profile-wide variance + Lilliefors normality**：WISECONDOR 最优
- **Median Segment Variance (MSV)**：WISECONDOR 最低噪声
- **Amplitude**：WISECONDOR 不牺牲真变异的信号强度
- **ROC AUC**：WISECONDOR 最佳（尤其 gDNA 数据）
- **异常样本（gDNA-3, gDNA-12）**：仅 WISECONDOR 能正确归一化

## WisecondorX 相对于 WISECONDOR 的改进

| 改进方向 | WISECONDOR | WisecondorX |
|---------|------------|-------------|
| 适用范围 | 仅 NIPT | NIPT + 肿瘤 + 先天性异常 |
| 性染色体 | ❌ 排除 | ✅ 自动性别判定，生成男女参考集 |
| 分段算法 | Stouffer's z-score 滑动窗 | **CBS** (DNAcopy)，速度大幅提升 |
| 15kb分辨率耗时 | ~24h | ~2min |
| 安装方式 | 手动编译 | **Bioconda** 一键安装 |
| 输出格式 | 单一可视化 | 多格式 table，易对接pipeline |
| 同源染色体内部多次变异 | ❌ 无法正确处理 | ✅ 支持 |
| Z-score | 仅bin-wise | bin + segment + chromosome 三级 |
| 编程语言 | 未知 | **Python + R** |
| beta参数 | 无 | 用户可调的变异判定阈值（0~1） |

## 核心算法原理

WisecondorX 的核心创新在于 **within-sample 二次归一化** — 不是简单地将待测样本与参考集做比值，而是为每个 bin 在参考集中找到"最像的 bin 邻居"进行校正。

### Step 1: PCA 归一化 — 消除系统性偏差（GC/可及性/mappability）

sWGS 的 read count 受 GC 含量、重复序列、开放染色质等因素影响，这些偏差在样本间呈现系统性模式。

```
原始 read count → log2 转换 → PCA 降维
                               ↓
                    取前3个主成分（捕获 >90% 的技术偏差）
                               ↓
            从 read count 中回归掉 PC1~PC3 的贡献
                               ↓
                     得到"去偏差"的 read count
```

**为什么用 PCA？**
- sWGS 的偏差来源多且相互关联（GC bias 与 mappability 相关），PCA 天然适合处理这种多重共线性
- 比 GC loess 校正（QDNAseq 的方法）更稳健：GC 校正假设偏差仅来自 GC，忽略了其他因素

### Step 2: Within-sample 参考 bin 搜索 — 最独特的创新

这是 WisecondorX 区别于所有其他工具的核心：

```
待测样本中 bin A (chr1:1M-1.1M)
         ↓ 问："在100个参考样本中，哪些 bin 的 read count 行为最像 bin A？"
         ↓
在参考集中扫描所有 bin，用 Euclidean distance 找到 top-N "邻居 bins"
         ↓
用这些"邻居 bins"的分布作为 bin A 的 null 分布，计算偏离程度
```

**为什么不需要"同位置对照"？**
传统方法（如 CNVkit）将待测样本的 chr1:1M 与正常样本的 chr1:1M 直接比较。但某些区域在不同个体间天然波动大（如重复区），这种波动会引入噪声。

WisecondorX 的方法：bin A 在待测样本中的值，与参考集中"所有跟 bin A 行为模式相似的 bins"的分布做比较。如果 chr1:1M 本身波动大，但 chr5:2M 波动模式相同，那 chr5:2M 也可以作为 chr1:1M 的参考。

### Step 3: 加权 CBS 分段

**CBS (Circular Binary Segmentation)** 是 CNV calling 的经典分段算法：

1. 将基因组视作首尾相连的环形
2. 递归搜索染色体上的"切点"——该点左右两侧的 read count 均值有显著差异
3. 每找到一个切点，将染色体分成两段，再递归对每段重复检测
4. 直到找不到显著切点为止

WisecondorX 的改进：对每个 bin 赋予 **权重**。Step 2 中找到的"邻居 bins"
- 如果邻居 bins 的分布很集中（低方差）→ 该 bin 的权重高（可信度高）
- 如果邻居 bins 分布分散（高方差）→ 权重低（CBS 分段时不易被"切开"）

### Step 4: Z-score 三级框架

WisecondorX 输出三级 z-score，从粗到细判断变异：

```
bin z-score:    每个 bin 单独评分 → 热点图（高分辨率）
segment z-score: CBS 分段后，整段的综合评分 → 片段级 CNV 判定
chromosome z-score: 整条染色体综合评估 → 整倍体异常（如 21 三体）
```

- **bin-wise z-score**：null 分布来自 within-sample 搜索到的"邻居 bins"
- **segment/chromosomal z-score**：null 分布来自 100 个健康样本的参考矩阵

### Step 5: Beta 参数 — 嵌合体/纯度判定

```
beta=0: 所有偏离参考的变异都报阳性（最大灵敏度）
beta=1: 仅报纯合/非嵌合变异（log₂ ratio 接近理论值）
       deletion = -1, duplication = 0.58
```

Beta 的本质是 control the trade-off between calling power and specificity for mosaic variants。

## CNV 检测的三种归一化范式（算法视角）

从算法和统计原理角度理解三类方法的区别：

### 1. Reference-free — 回归校正
**核心假设**：read count 的波动主要来自可预测的基因组特征（GC、mappability）
- 方法：read count ~ GC% + mappability + ... 做回归，残差 = CNV signal
- 优势：不需要正常样本，成本低
- 局限：未建模的偏差（批次效应、文库偏好）无法消除
- 代表：FREEC（多项式拟合）、QDNAseq（loess）

### 2. Pooled reference — 经验分布比较
**核心假设**：同一 genomic locus 在正常样本间的 read count 服从稳定分布
- 方法：z = (X_test - μ_ref) / σ_ref
- 优势：经验 null 分布捕获了所有已知和未知的偏差
- 局限：需要大量正常样本、参考集与测试集批次一致
- 代表：cn.MOPS（混合Poisson）、WisecondorX（PCA + within-sample）

### 3. Matched case-control — 配对差异
**核心假设**：同一患者的配对样本（肿瘤 vs 正常）所有差异都来自体细胞变异
- 方法：log₂(tumor/normal)，检测偏离 0 的区域
- 优势：最强消噪能力，可结合 MAF 区分胚系 vs 体细胞
- 局限：需要配对样本，成本高
- 代表：BIC-seq2（case-control mode）

### Within-sample 归一化的独特优势（WisecondorX 的核心创新）

从算法角度理解，传统 pooled reference 方法的问题是：

```
传统方法：z = (待测样本 bin A - 参考集 bin A 均值) / 参考集 bin A 标准差
问题：如果 bin A 在正常人群中本身就波动大 → z-score 被稀释 → CNV 被淹没
```

WisecondorX 的 within-sample 方法：

```
WisecondorX：z = (待测样本 bin A - 与 bin A"行为相似"的所有参考bin的均值) / 这些bin的标准差
优势：即使 bin A 本身波动大，只要找到波动"模式相同"的其他 bin → 不被稀释
```

这解释了为什么 WISECONDOR 在**所有 5 个评估维度**上都优于其他 5 种工具。

## 检出阈值参数

| 参数 | 默认值 | 说明 |
|------|--------|------|
| bin size | 用户定义（推荐 15-100kb） | 分辨率与噪声的平衡 |
| beta | 用户定义（0~1） | 0=全阳性, 1=仅纯合 constit. 变异 |
| 嵌合体判定 | log₂ ratio 硬阈值 | deletion < -0.26, gain > 0.22 |

### Constitutional aberration 理论值
- **Deletion** (1 copy): log₂(1/2) = **-1**
- **Duplication** (3 copies): log₂(3/2) ≈ **0.58**
- **判定边界**: 偏离二倍体 1/3 copy → deletion < -0.26, gain > 0.22

## 测序与生信参数

| 参数 | NIPT (cfDNA) | gDNA (淋巴细胞) |
|------|-------------|----------------|
| 测序平台 | HiSeq 3000 | HiSeq 3000 |
| 测序深度 | 0.2-0.3× | ~1× |
| 最低reads | 10M | 50M |
| bin size | 100kb | 30kb |
| 比对 | Bowtie2 (fast-local) → GRCh37/hg19 | 同左 |
| 建库试剂盒 | NEXTflex Cell Free DNA-Seq | NEXTflex Rapid DNA Sequencing |
| 参考基因组 | hg19 (GRCh37) | hg19 (GRCh37) |

## 验证数据

- **40个验证样本**（20健康 + 20异常），100个健康参考样本
- NIPT 验证：羊膜穿刺/绒毛膜取样
- gDNA 验证：trio分析
- NIPT 5000样本大验证：无假阴性，少数假阳性（多为母体变异）

## GitHub 与安装

- GitHub：<https://github.com/CenterForMedicalGeneticsGhent/WisecondorX>
- Bioconda：`conda install -c bioconda wisecondorx`
- Dryad 数据：<https://datadryad.org> (doi: 10.5061/dryad.t013r0s)

## 相关页面
- [[kingcreate-tngs-panel]] — 金圻睿tNGS产品（生信流程参考）
- [[qmngs-internal-standards]] — qmNGS定量方法（归一化概念对比）
