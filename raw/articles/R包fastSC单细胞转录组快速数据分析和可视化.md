---
source_url: https://mp.weixin.qq.com/s/（生信摆渡公众号，fastSC 1.0.2）
ingested: 2026-05-06
sha256: 9b33360044959dd9e7e0dd63cb4245c8d46047a9049d2202fd073f03169f7f26
citation: "生信摆渡 (Jiahao Wang). R包fastSC开发：单细胞转录组快速数据分析和可视化. 微信公众号. 2026."
domain: bioinfo-pipeline
---

# R包fastSC开发1：单细胞转录组快速数据分析和可视化

> **作者**：生信摆渡 (Jiahao Wang)  
> **微信**：bioinfobaidu1  
> **邮箱**：wjhyyds@sina.cn  
> **版本**：fastSC 1.0.2

---

## 使用教程

### 数据准备

#### set_future: 设置并行

- 设置并行，加快分析
- 单细胞许多分析都支持并行计算，这是由future包默认支持的
- 设置并行数(ncore, 默认1)和每个并行的内存大小(maxSize, 单位G, 默认Inf)，加速单细胞分析过程，必设！
- 并行数默认总核数的90%，内存默认是500M，可以根据实际情况调整，指定内存的单位为G
- 核数过多可以分配不了那么多，内存过小可能报错每个并行的内存分配不足

```r
# 默认设置
set_future()
```

- 简单测试效果：相同任务，不同并行数，速度相差显著。日常分析中也可以以这种方式加快分析，告别for循环。

#### split_10x_3file: 分割数据为样本独立的10X三文件

- **功能1**: 将一个文件夹中多个样本的3文件分割为独立的样本文件夹
- 我们从GEO数据下载的数据通常是多个样本合并在一起的RAW.tar文件
- 而Seurat需要每个样本独立放在文件夹里，且文件名是标准的3文件名称，不带前缀
- 而有的数据集提供的文件名并不标准，所以也可以手动指定这三个文件名，用于识别样本
- 基因文件名标志通常为 `features.tsv.gz`，也有 `genes.tsv.gz`，默认支持识别这两种
- **功能2**: 如果解压出来只有3个文件（所有样本合并一起），也可以自动分割
- 后来又增加了样本标签自动识别功能

```r
# 以数据集GSE189357为例
split_10x_3file("./test_data/GSE189357_RAW")

# 合并数据分割
split_10x_3file("./test_data/GSE198315_merge")
```

#### read_10x_dir: 读取三文件

- 一步读取单个样本的10X三文件
- 包括Read10X和CreateSeuratObject两个步骤
- 可以自定义项目名称(project)，即默认的Idents

```r
obj = read_10x_dir("./test_data/GSE198315_merge_split_by_sample/P01-mLN")
```

#### read_10x_dir_multi: 批量读取三文件

- 一步读取多个样本文件夹的10X三文件，即循环运行read_10x_dir
- 函数会调用最大并行数去并行读取不同的样本
- 并行使用的是parallel_apply函数
- 为了区分不同样本，函数会自动往barcode上添加样本标签
- 如果数据已含样本标签，可设置`add_id = FALSE`不进行添加
- 可设置cores参数控制并行数

```r
obj = read_10x_dir_multi("./test_data/GSE189357_RAW_single")
```

#### add_seurat_SID: 自动识别样本标签

- 自动识别cell id中barcode的位置，剩余部分为样本标签
- 将自动切割合并然后添加至SID列，即代表样本标签
- 样本标签在后续分析很重要，比如去除双细胞、去批次、作图都需要指定样本列

```r
obj = add_seurat_SID(obj)
```

#### merge_seurat_list: 合并seurat列表

- 合并包含多个seurat对象的list
- 是read_10x_dir_multi函数所依赖的函数

#### h5ad_to_seurat

- 将h5ad文件转为seurat对象
- 个别情况下GEO提供的是h5ad格式（Python分析结果文件）
- 原理：使用python读取h5ad文件并将构建seurat对象所需的三文件导出，再读到R里构建seurat对象
- Python脚本能够在R里运行

```r
obj = h5ad_to_seurat("./test_data/GSE210152_raw.h5ad")
```

---

### 质控、降维、分群、注释

#### sc_pipeline: 一站式Seurat流程

- 一步完成原始seurat对象的质控、去除双细胞、标准化、归一化、PCA、UMAP、tSNE、聚类、分群、气泡图、SingleR注释
- **质控包括**: nFeature、nCount、线粒体基因、红细胞基因
- 聚类函数进行了优化，可以指定想要的亚群数范围

**可修改参数**:

- `dataset_name = "All"`: 这群细胞的名称
- `sample_name = "SID"`: 包含样本名的那一列名
- `cluster_range = c(25, 30)`: 指定聚类细胞群数范围，算法会从起始分辨率逐步逼近想要的群数
- 细胞质量阈值: `cell.min = 3, mt.pct = 25, hb.pct = 3, feature.min = 200, feature.max = 8000, count.min = 0, count.max = Inf`
- `tSNE = TRUE`: 默认跑tSNE，非常费时间，不需要可设为FALSE
- `species = "human"`: 物种，只支持human和mouse
- `DoubletFinder = FALSE`: 默认不跑去除双细胞流程，太费时间

```r
obj = sc_pipeline(obj, out_name = "test_out_put/01_sc_pipeline")
```

**输出结果包含**:

- 质控前后图
- 高变基因图
- PCA图（按样本）
- PCA重要性图
- tSNE/UMAP图（按样本、按Cluster、按SingleR注释）
- 标志基因气泡图
- SingleR注释结果

**SingleR自动注释结果示例**:

- label.main: T_cells(3294), NK_cell(1457), B_cell(1192), Monocyte(1130), Epithelial_cells(1088), Macrophage(954), Endothelial_cells(374), Smooth_muscle_cells(336), Neutrophils(174)
- label.fine: 更细粒度的细胞亚型分类

最终结果保存为qs格式，使用READ函数读取。

#### FindClusters2: 智能计算想要的细胞群数

- 只需指定想要的群数范围，程序自动调整分辨率不断逼近
- 起始分辨率为最低群数×0.2，节省大量逼近时间

```r
obj = FindClusters2(obj, cluster_range = c(16, 18), cluster_name = "Cluster2")
# 找到合适的分辨率: 0.32, 最终群数: 17
```

#### cluster_start_with_one: cluster从1开始

- 默认计算的群数是以0起始的，此函数将cluster都加1
- 所有函数的群数都是从1开始

```r
obj = cluster_start_with_one(obj)
```

#### sc_anno_celltype: 细胞注释/组合成新组

- 非常好用的函数，不只可以批量指定对应细胞类型的群，更重要的是告诉你哪些群还未指定以及哪些群重复指定
- 未指定的群归类为others，重复指定则直接报错
- 实际使用时，先把可能的群指定对应细胞类型，不确定的先不管，根据每次提示修改，直到没有提示
- 也可以用来合并小组为大组（不仅限于细胞注释）

```r
obj = sc_anno_celltype(
    obj, group.by = "Cluster2", name = "celltype",
    "T_NK"  = c(1, 2, 14),
    "B"     = c(4, 9, 17),
    "Epi"   = c(6, 11, 15, 16),
    "Endo"  = c(8),
    "Fibro" = c(10),
    "Macro" = c(3, 5),
    "Mono"  = c(7, 12, 13)
)
```

#### sc_subset_obj: 提取子集

- 提取seurat对象子集，操作跟数据框一样简单

```r
# 提取指定群
xobj = sc_subset_obj(obj, group.by = "seurat_clusters", cluster = c(1, 3, 10))

# 提取指定细胞类型
xobj = sc_subset_obj(obj, group.by = "celltype", cluster = c("T_NK", "B"))
```

---

### 可视化

#### DimPlot2 - 优化降维图

- 主要解决细胞类型label标注的颜色、背景和边框设置
- 标注位置可自定义，解决标注重叠和位置不准问题
- 可将大坐标轴改成迷你坐标
- 支持栅格化和分面(split.by设置分面组别，cex.facet控制分面标题大小)
- 可指定out_name参数设置输出文件名，不需要带后缀名，也可指定图片宽w和高h

```r
# 快速绘制
DimPlot2(obj, group.by = "celltype")

# 完整参数
DimPlot2(
    obj, group.by = "celltype",
    reduction = "umap",
    title = "All",
    legend.title = "Cell Type",
    cols = rainbow(10),
    label = TRUE,
    label.color = "black",
    label.fill = "auto",
    label.fill.alpha = 0.2,
    label.box = FALSE,
    mini.axis = TRUE,
    pos.manual = list("Endo" = c(-4, -4), "B" = c(5, 0)),
    axis = FALSE,
    split.by = "type",
    raster = TRUE,
    out_name = "test_out_put/DimPlot2", w = 12, h = 5
)
```

#### sc_plot_volcano: 绘制差异基因火山图

- 对接FindMarkers输出的表格
- 支持横/纵向(direction = "h"/"v")
- 可标注特定基因列表
- 可设置log2FC阈值、p值阈值、标签数量等

```r
DEG_tb = FindMarkers(object = obj, ident.1 = "AIS", ident.2 = "IAC",
                     group.by = "type", min.pct = 0.1, logfc.threshold = 0)

# 标注特定基因
genes_show = c("DCN", "CFD", "S100A9", "LTB")
sc_plot_volcano(DEG_tb, log2FC = 0.2, pvalue = 0.05,
                title = "AIS vs IAC", direction = "h",
                label = genes_show, label.top = 15,
                pt.size = 0.6, alpha = 0.6,
                out_name = "./test_out_put/sc_plot_volcano")
```

#### sc_dotplot - 更方便绘制气泡图

- 自动完成ScaleData并运行DotPlot
- 参数: cex(全局文字大小), x.text.angle(x轴文本角度), legend.position(图例位置), flip(坐标轴翻转), color(色板), out_name/w/h(输出设置)

```r
features = c("CD3D", "CD3", "CD8A", "CD8B", "CD4", "TRGC1")
sc_dotplot(obj, features = features, group.by = "celltype",
           out_name = "sc_dotplot", w = 10, h = 7, cex = 25)
```

#### sc_dotplot_general - 大分类细胞marker气泡图

- 绘制大分类细胞marker气泡图，辅助注释

```r
sc_dotplot_general(obj, group.by = "celltype")
```

#### sc_dotplot_each_type: 每种细胞类型marker气泡图

- 默认绘制25种细胞类型的气泡图，每种独立出图最后合并
- 也可自指定基因列表
- 可结合sc_anno_celltype函数对亚群进行快速手动注释

```r
p <- sc_dotplot_each_type(obj, group.by = "Cluster",
                          out_name = "./test_out_put/sc_dotplot_each_type")
```

#### FeaturePlot2 - 基因表达分布

- 方便指定颜色
- 智能分配行数
- 快捷设置迷你坐标或无坐标
- 自动导出PDF/PNG图片
- 支持多种配色（如viridis_A）

```r
FeaturePlot2(obj, features = c("nFeature_RNA", "percent.MT", "EPCAM", "TP53"),
             out_name = "./test_out_put/FeaturePlot2_1")

# 换配色
plot_list = FeaturePlot2(obj, features = c("nFeature_RNA", "percent.MT", "EPCAM", "TP53"),
                         color = "viridis_A", mini.axis = TRUE)
```

#### sc_barplot - 快速绘制柱状图

- 支持一个分组或两个分组
- 堆叠模式(mode = "stack")
- 可自定义x轴标签和角度
- 可调节图例key大小
- 支持flip横向

```r
# 单分组
sc_barplot(obj, group.by = "SingleR.label.main")

# 两种分类
sc_barplot(obj, group.by = "SingleR.label.main", split.by = "type")

# 横向
sc_barplot(obj, group.by = "SingleR.label.main", split.by = "type", flip = TRUE)
```

---

### run_SingleR - 快速运行SingleR

```r
obj_singleR = run_SingleR(obj, title = "All cells", out_name = "./test_out_put/run_SingleR")
```

输出包括label.main和label.fine两个层级的细胞类型注释。

---

### CellChat

#### seurat2cellchat - 快速构建cellchat对象

- 从Seurat对象快速构建CellChat分析对象
- 自动加载人类配体-受体数据库
- 自动完成通讯概率计算和信号通路层面通讯概率计算

```r
cellchat = seurat2cellchat(obj2, out_name = "test_out_put/cellchat_1")
```

#### plot_cellchat_circle - 细胞类型间相互作用强度

```r
plot_cellchat_circle(cellchat, out_name = "test_out_put/plot_cellchat_circle_2")
```

#### plot_cellchat_compare - 比较两组间作用数量和网络差异

#### plot_cellchat_each_sender - 一对多贝壳图

---

### Monocle

#### seurat2cds

- 将Seurat对象转换为CellDataSet对象
- 可使用FindAllMarkers的结果作为排序基因
- 自动完成：估计size factors、估计dispersion、检测表达基因、设置ordering filter、降维(Monocle DDRTree)、排序细胞

```r
Idents(obj2) <- "celltype"
marker_tb <- FindAllMarkers(obj2, only.pos = TRUE, min.pct = 0.25)
cds = seurat2cds(obj2, marker_tb = marker_tb, out_name = "cds_obj_1")
```

可视化使用ggplot2开发：

```r
# Pseudotime
plot_cell_trajectory(cds, color_by = "Pseudotime", show_branch_points = TRUE,
                     cell_size = 1, show_tree = FALSE)

# State
plot_cell_trajectory(cds, color_by = "State", cell_size = 1,
                     show_branch_points = TRUE, show_tree = FALSE)

# Cell type
plot_cell_trajectory(cds, color_by = "celltype", cell_size = 1,
                     show_branch_points = TRUE, show_tree = FALSE)
```

---

### inferCNV

#### prepare_infercnv_object - 快速构建初始inferCNV对象

**参数**:

- `anno_name`: 细胞注释的列名
- `ref_name`: 作为对照的细胞类型
- `epi_name`: 上皮细胞名称
- `spike_in`: 是否抽取对照细胞与上皮一起作为测试细胞，默认否
- `epi_cell_num`: 当上皮细胞量过多时，可取子集
- `ref_cell_num`: 当对照细胞量过多时，可取子集
- `out_dir`: 输出目录，默认inferCNV

```r
infercnv_obj_start = prepare_infercnv_object(
    obj, anno_name = "celltype", ref_name = c("T_NK", "B"),
    epi_name = "Epi", ref_cell_num = 2000, out_dir = out_dir
)
```

#### run_infercnv - 快速执行infercnv

- 只跑CNV不做下游分析：不做图，不跑HMM，不聚类

```r
infercnv_obj_result = run_infercnv(infercnv_obj_start, out_dir = out_dir)
```

#### process_infercnv_result - 下游分析，恶性判定，可视化

**参数**:

- `k_clusters`: 控制分的群数，默认8，如果恶性和良性上皮没分开可提高群数
- `downsample`: 热图下采样倍数，默认5倍
- `raster_quality`: 图片质量，默认0.5，1最清晰
- 细胞数上万时作图非常慢，可调高downsample和降低raster_quality
- `ref_cutoff`: 默认0.3，将对照细胞比例大于0.3的亚群判定为良性亚群

```r
infer_result = process_infercnv_result(
    ref_name = ref_name, epi_name = epi_name, out_dir = out_dir
)
```

输出包括：CNV热图、不同分组/Cluster/推断结果的CNV评分箱线图

#### plot_cnv_score - 绘制染色体CNV热图

- 可重新调整图片质量
- 默认: downsample = 0.5, raster_quality = 0.5
- 最高: downsample = 1, raster_quality = 1
- 细胞量高时不建议调太高

```r
plot_cnv_score(out_dir = "test_out_put/inferCNV",
               file_name = "test_out_put/inferCNV/plot_cnv_score_2.pdf")

# 最高质量
plot_cnv_score(out_dir = "test_out_put/inferCNV",
               file_name = "test_out_put/inferCNV/plot_cnv_score_3.pdf",
               downsample = 1, raster_quality = 1)
```

---

## 安装

### 自研依赖包

- fanyi2 (>= 1.0.0)
- fastR (>= 1.9.0)

提供压缩包，直接运行可快速安装：

```r
install.packages.local("fastR")
install.packages.local("fastSC")
```

### 公共依赖包

需自行安装，方法包括：install.packages、BiocManager::install、remotes::install_github

**所有公共依赖**:

- R (>= 4.0.0)
- Seurat (>= 4.4.0)
- fastR (>= 1.5.0)
- ggplot2 (>= 4.0.2)
- dplyr (>= 1.1.4)
- SingleR (>= 2.6.0)
- monocle (>= 2.32.0)
- CellChat (>= 2.2.0)
- infercnv (>= 1.20.0)
- DoubletFinder (>= 2.0.3)
- harmony (>= 1.2.1)
- qs (>= 0.27.3)
- BiocParallel (>= 1.38.0)
- grDevices, graphics, stats, utils

**重要**: 在确保所有依赖包安装并加载成功后再安装fastSC。

**技术支持说明**: 软件安装问题不免费答疑（这是基本功）。不看报错信息、没有先尝试AI、教程已提到的问题、不提供详细代码和报错截图的情况不免费答疑。
