---
title: scRNA-seq 标准化分析流程
created: 2026-05-06
updated: 2026-05-07
type: concept
tags: [bioinformatics, pipeline, ngs, enrichment]
domain: bioinfo-pipeline
sources: [raw/articles/scrnaseq-analysis-guide-wechat.md, raw/articles/NatGen-scNotebooks开源单细胞空间组学培训笔记库.md, raw/articles/R包fastSC单细胞转录组快速数据分析和可视化.md, raw/articles/RegFormer-单细胞大模型Mamba架构-NatCommun2026.md]
confidence: medium
---

# scRNA-seq 标准化分析流程

单细胞转录组测序（scRNA-seq）在单细胞分辨率解析组织异质性，标准化流程分为湿实验建库和生信分析两大阶段。

## 湿实验阶段

1. **单细胞悬液制备** — 临床组织/细胞样本经胶原酶/胰蛋白酶联合消化，台盼蓝染色质控：活率 ≥ 85%，碎片率 < 10%
2. **单细胞捕获** — 10× Genomics Chromium 等微流控平台捕获单个细胞
3. **文库构建** — Cell Barcode + UMI 标记转录本 → 反转录 → cDNA 扩增 → 片段化 → 构建测序文库
4. **测序** — Illumina 平台双端测序，产出原始 fastq 数据

## 生信分析流程

### Step 1: 表达矩阵生成
- **比对工具**：Cell Ranger（10× 官方）、STARsolo（STAR 的单细胞模式）
- **UMI 去重**：消除 PCR 扩增偏差
- **输出**：基因-细胞表达矩阵

### Step 2: 质控过滤
- 过滤标准：
  - 线粒体基因占比 > 20% → 低质量/死细胞
  - 基因数 < 200 → 空液滴/碎片
  - 基因数 > 6000 → 可能为双细胞
- **Doublet 检测**：去除双细胞
- **背景 RNA 去除**：去除 ambient RNA 污染

### Step 3: 降维与聚类
- **标准化**：LogNormalize
- **特征选择**：筛选高变基因（HVGs）
- **线性降维**：PCA
- **非线性降维/可视化**：UMAP、t-SNE
- **聚类**：Louvain 图聚类算法（无监督细胞分群）

### Step 4: 细胞类型注释
- **参考数据库**：CellMarker、PanglaoDB
- **经典 marker**：
  - CD3E → T 细胞
  - CD19 → B 细胞
  - CD68 → 巨噬细胞
- **功能亚群**：耗竭性 CD8⁺ T 细胞、M2 型巨噬细胞等

### Step 5: 差异表达与功能分析
- **差异基因**：Wilcoxon 秩和检验
- **功能富集**：GO / KEGG / GSEA
- **高级分析**：
  - CellPhoneDB — 细胞通讯分析
  - Monocle — 拟时序分化轨迹分析
  - SCENIC — 转录因子调控网络分析
  - [[wisecondorx]] — CNV 分析（适用于 scRNA-seq 中的肿瘤细胞）

## 临床转化

- 单细胞分子特征 → 与临床预后/治疗响应数据关联 → 构建预后风险模型
- 实验验证：免疫组化、多重免疫荧光、流式细胞术
- 应用：疾病机制解析、精准治疗靶点开发

## 培训资源

- [[scnotebooks]] — 开源单细胞与空间组学培训平台（Nature Genetics 2026），13 模块覆盖全流程，多语言 + 云端运行

## R 包工具

- [[fastsc]] — R 包 fastSC v1.0.2，一站式 Seurat 流程 + 可视化（DimPlot2/sc_dotplot/volcano）+ CellChat/Monocle/inferCNV 快速接口

## 单细胞基础模型

单细胞基础模型（scFM）绕过传统分步流程，直接对基因表达进行端到端预训练：

- [[regformer]] — 华大 BGI 开发，Mamba 状态空间模型 + 基因调控拓扑排序。突破全基因组级长序列瓶颈，支持跨组织聚类、调控网络重建、扰动推演、药物敏感性预测，超越 scGPT 等 Transformer 基线（*Nat Commun* 2026）
- 与经典流程的定位差异：scFM 擅长零样本迁移和稀疏信号捕获，但在可解释的分步分析（拟时序、细胞通讯）上仍需传统工具辅助
