---
title: fastSC
created: 2026-05-06
updated: 2026-05-06
type: entity
tags: [bioinformatics, pipeline, ngs]
domain: bioinfo-pipeline
sources: [raw/articles/R包fastSC单细胞转录组快速数据分析和可视化.md]
confidence: medium
---

# fastSC

R 包，用于单细胞转录组快速数据分析和可视化。作者：生信摆渡 (Jiahao Wang)，版本 1.0.2。

**GitHub**: https://github.com/wjhyyds/fastSC（推测）

## 安装

依赖两个自研 R 包：`fanyi2` (>=1.0.0)、`fastR` (>=1.9.0)，以及公共依赖 Seurat/SingleR/monocle/CellChat/infercnv 等。

## 功能模块

### 1. 数据准备
- `set_future()` — 设置并行计算（future 包）
- `split_10x_3file()` — 分割 10X 三文件为独立样本文件夹
- `read_10x_dir()` / `read_10x_dir_multi()` — 单/批量读取 10X 数据
- `add_seurat_SID()` — 自动识别样本标签
- `h5ad_to_seurat()` — h5ad 格式转 Seurat 对象（调用 Python）

### 2. 质控与降维分群
- `sc_pipeline()` — 一站式 Seurat 流程：质控→去双细胞→标准化→PCA→UMAP/tSNE→聚类→气泡图→SingleR 注释
- `FindClusters2()` — 智能逼近指定群数范围，自动调整分辨率
- `sc_anno_celltype()` — 批量指定细胞类型，检测未指定/重复指定

### 3. 可视化
- `DimPlot2()` — 优化降维图（自定义标注位置、迷你坐标、分面）
- `sc_plot_volcano()` — 差异基因火山图
- `sc_dotplot()` / `sc_dotplot_general()` / `sc_dotplot_each_type()` — 多层级气泡图
- `FeaturePlot2()` — 基因表达分布图（多种配色）
- `sc_barplot()` — 柱状图（单/双分组，堆叠/横向）

### 4. 高级分析
- `run_SingleR()` — SingleR 快速细胞注释
- `seurat2cellchat()` — Seurat → CellChat 快速转换
- `plot_cellchat_circle()` / `plot_cellchat_compare()` — 细胞通讯可视化
- `seurat2cds()` — Seurat → Monocle CellDataSet 转换
- `prepare_infercnv_object()` / `run_infercnv()` / `process_infercnv_result()` / `plot_cnv_score()` — inferCNV 快速分析

## 与 [[scrnaseq-workflow]] 的关系

fastSC 的 `sc_pipeline()` 一站式覆盖了 scrnaseq-workflow 的 Step 1-4（表达矩阵生成、质控、降维聚类、注释），高级函数覆盖 Step 5 的差异分析、细胞通讯、拟时序轨迹和 CNV 分析。
