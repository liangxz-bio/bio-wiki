---
source_url: https://mp.weixin.qq.com/s/v0q8loA1QukJa7IO250Slg
ingested: 2026-05-06
sha256: 9616f0de7169749ab64611e4daf6c1239c7fede4de420a4ce461c7e1fae6c7ee
citation: "逻捷科技 (biostack). NEXTFLOW工作流清单第一期：微生物组，转录组，变异，三代测序：1-100. 微信公众号. 2025."
domain: bioinfo-pipeline
---

# NEXTFLOW工作流清单第一期：微生物组，转录组，变异，三代测序：1-100

> **来源**：逻捷科技 (微信公众号 · biostack)

---

## 一、为什么使用 Nextflow

为什么要学习 Nextflow：https://www.nextflow.io/blog/2022/learn-nextflow-in-2022.html

- **可重复性**：将 Nextflow 集成到分析工作流程中，有助于实施可重复的流程。Nextflow 管道遵循 FAIR 准则，通过版本控制和容器管理所有软件依赖关系。
- **可移植性**：在笔记本电脑上编写的相同的流程可以快速扩展部署到在高性能计算集群、AWS 和谷歌云服务以及 Kubernetes 上运行。代码在不同的基础设施上保持不变。
- **可扩展性**：允许使用数据流（Dataflow）并行化任务，而无需根据特定平台架构对管道进行硬编码。
- **灵活性**：支持科学计算工作流要求，如缓存进程以防止重复计算，以及工作流报告以更好地了解工作流的执行情况。
- **发展迅速**：Seqera（https://seqera.io/）实验室提供长期支持。Nextflow 自 2013 年起由同一团队开发，其生态系统正在迅速扩大。
- **开源**：Apache 2.0 许可，可自由使用、修改和发布。

---

## 二、开源 Nextflow 清单：1-100

按四个专题分类，共 100 个 Nextflow 工作流：

### 2.1 Microbiome

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 1 | metatdenovo | 宏转录组组装与注释（原核+真核） | https://github.com/nf-core/metatdenovo |
| 2 | mag | 宏基因组混合组装与分箱 | https://nf-co.re/mag (Krakau et al. NAR Genomics Bioinform 2022) |
| 3 | taxprofiler | 高通量多分类学 profiling（shotgun 短/长读长） | https://github.com/nf-core/taxprofiler |
| 4 | funcscan | （宏）基因组功能与天然产物基因筛选 | https://github.com/nf-core/funcscan |
| 5 | ampliseq | 扩增子测序分析（DADA2 + QIIME2） | https://github.com/nf-core/ampliseq |
| 6 | bacass | 简单细菌组装与注释 | https://github.com/nf-core/bacass |
| 7 | viralrecon | 病毒样本组装与宿主内/低频变异 calling | https://github.com/nf-core/viralrecon |
| 8 | bactmap | 基于比对的细菌全基因组系统发育构建 | https://github.com/nf-core/bactmap |
| 9 | metatdenovo | 宏转录组组装与注释 | https://github.com/nf-core/metatdenovo |
| 10 | phageannotator | 鉴定、注释、定量（宏）基因组中噬菌体序列 | https://github.com/nf-core/phageannotator |
| 11 | detaxizer | 检查 fastq 文件中特定分类群，可选过滤 | https://github.com/nf-core/detaxizer |
| 12 | metapep | 宏基因组→表位分析 | https://github.com/nf-core/metapep |
| 13 | pathogensurveillance | 利用群体基因组学与测序进行病原体监测 | https://github.com/nf-core/pathogensurveillance |
| 14 | vipr | 病毒样本组装与低频变异 calling | https://github.com/nf-core/vipr |
| 15 | createtaxdb | 并行自动化构建宏基因组分类器数据库 | https://github.com/nf-core/createtaxdb |
| 16 | MEDI | 基于宏基因组的人类饮食摄入估算 | https://github.com/Gibbons-Lab/medi (Diener & Gibbons, bioRxiv 2024) |
| 17 | MUFFIN | 混合组装+差异分箱：宏基因组/转录组/通路分析 | https://github.com/RVanDamme/MUFFIN |
| 18 | YAMP | 又一个宏基因组流程 | https://github.com/alesssia/YAMP |
| 19 | emg-viral-pipeline | 病毒检测、注释、分类（VIRify） | https://github.com/EBI-Metagenomics/emg-viral-pipeline (Rangel-Pineros et al. PLOS Comput Biol 2023) |
| 20 | WIP | 噬菌体序列可扩展鉴定与分析 | https://github.com/MarquetMike/WIP (Marquet et al. GigaScience 2022) |
| 21 | QuasiFlow | HIV-1 耐药 NGS 数据分析 | https://github.com/asekagiri/QuasiFlow (Ssekagiri et al. Bioinform Adv 2022) |
| 22 | sourmash-nf | 输入基因组上运行 sourmash | https://github.com/fmalmeida/sourmash-nf |
| 23 | viralintegration | 嵌合 read 方法鉴定基因组中病毒整合事件 | https://github.com/nf-core/viralintegration |
| 24 | HiFi-16S-workflow | PacBio HiFi 全长 16S 数据分析 | https://github.com/PacificBiosciences/HiFi-16S-workflow |
| 25 | Arcadia-Science/metagenomics | 宏基因组 QC/评估/profiling（短+长读长） | https://github.com/Arcadia-Science/metagenomics |
| 26 | MeRaGENE | 宏基因组快速基因鉴定 | https://github.com/metagenomics/MeRaGENE |
| 27 | nf_wochenende | 宏基因组比对与归一化（长+短读长） | https://github.com/MHH-RCUG/nf_wochenende |
| 28 | PHoeNIx | 医疗相关与耐药病原体短读长流程 | https://github.com/CDCgov/phoenix |
| 29 | virID | 病毒鉴定与发现 | https://github.com/jnoms/virID |
| 30 | meta-sweeper | 模拟微生物群落参数扫描 | https://github.com/cerebis/meta-sweeper |
| 31 | TADA | 靶向扩增子多样性分析（DADA2 聚焦） | https://github.com/h3abionet/TADA |
| 32 | metagenomics | 宏基因组 QC/评估/profiling（短+长读长） | https://github.com/Arcadia-Science/metagenomics |
| 33 | ONTrack2 | MinION 生物多样性追踪 | https://github.com/MaestSi/ONTrack2 |
| 34 | ampa-nf | 快速自动化预测蛋白抗菌区域 | https://github.com/cbcrg/ampa-nf |
| 35 | WasteFlow | 废水中病原体监测 | https://github.com/BCCDC-PHL/WasteFlow |
| 36 | v-met | 基于 Kraken 2 的简易病毒宏基因组流程 | https://github.com/ksumngs/v-met |

### 2.2 Variation

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 37 | sarek | 胚系/体细胞变异检测（WGS/靶向测序） | https://github.com/nf-core/sarek (Garcia et al. F1000Res 2020) |
| 38 | strelka2-nf | Strelka（胚系+体细胞变异 calling） | https://github.com/IARCbioinfo/strelka-nf |
| 39 | raredisease | WGS/WES 罕见病变异 calling 与评分 | https://github.com/nf-core/raredisease |
| 40 | hlatyping | 基于 NGS 的精密 HLA 分型 | https://github.com/nf-core/hlatyping |
| 41 | nf-gwas | 生物库规模 GWAS 分析 | https://github.com/nf-core/nf-gwas (Schoenherr et al. bioRxiv 2023) |
| 42 | rnavar | GATK4 RNA 变异 calling 流程 | https://github.com/nf-core/rnavar |
| 43 | CalliNGS-NF | GATK RNA-Seq 变异 calling | https://github.com/CRG-CNAG/CalliNGS-NF |
| 44 | mtdna-server-2 | mtDNA 变异 calling | https://github.com/genepi/mtdna-server-2 |
| 45 | variant-calling-pipeline-gatk4 | GATK4 变异 calling | https://github.com/gencorefacility/variant-calling-pipeline-gatk4 |
| 46 | mutect-nf | Mutect 流程 | https://github.com/IARCbioinfo/mutect-nf |
| 47 | BQSR-nf | BAM 文件碱基质量分数重校准（GATK） | https://github.com/IARCbioinfo/BQSR-nf |
| 48 | gama_annot-nf | VCF 文件 Annovar 注释 | https://github.com/IARCbioinfo/gama_annot-nf |
| 49 | needlestack | 多样本体细胞变异 calling | https://github.com/IARCbioinfo/needlestack |
| 50 | flow | 一致性组装与变异 calling | https://github.com/nmdp-bioinformatics/flow |
| 51 | hpc-gatk4 | GATK4 变异 calling（HPC） | https://github.com/gencorefacility/hpc-gatk4 |
| 52 | vcf_normalization-nf | VCF 分解与归一化 | https://github.com/IARCbioinfo/vcf_normalization-nf |
| 53 | table_annovar-nf | Annovar 变异注释 | https://github.com/IARCbioinfo/table_annovar-nf |
| 54 | VariantAnalysis | 生信变异 calling | https://github.com/PlantandFoodResearch/VariantAnalysis |
| 55 | GenomicsCortextVarNextflow | Cortex-Var 结构变异 calling | https://github.com/ncsa/GenomicsCortextVarNextflow |
| 56 | MutSig | WGS 突变特征分析（SigProfilerExtractor） | https://github.com/IARCbioinfo/MutSig |
| 57 | tronflow-mutect2 | Mutect2 体细胞变异 calling（best practices） | https://github.com/TRON-Bioinformatics/tronflow-mutect2 |
| 58 | smoove-nf | Smoove 结构变异 calling + QC | https://github.com/brwnj/smoove-nf |

### 2.3 RNA-seq

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 59 | rnaseq | RNA-seq 分析（STAR/RSEM/HISAT2/Salmon + 基因/异构体计数 + QC） | https://github.com/nf-core/rnaseq |
| 60 | rnafusion | RNA-seq 基因融合检测 | https://github.com/nf-core/rnafusion |
| 61 | smrnaseq | 小 RNA-seq 分析 | https://github.com/nf-core/smrnaseq |
| 62 | dualrnaseq | Dual RNA-seq 分析（宿主-病原体互作） | https://github.com/nf-core/dualrnaseq |
| 63 | circrna | 环状 RNA 定量、miRNA 靶标预测、差异表达 | https://github.com/nf-core/circrna (Digby et al. BMC Bioinform 2023) |
| 64 | rnasplice | 可变剪接分析 | https://github.com/nf-core/rnasplice |
| 65 | differentialabundance | 特征/观测矩阵差异丰度分析 | https://github.com/nf-core/differentialabundance |
| 66 | FLOP | 转录组数据流程对下游功能富集的影响评估 | https://github.com/FLOP (Paton et al. bioRxiv 2023) |
| 67 | grape-nf | 自动化 RNA-seq 流程 | https://github.com/guigolab/grape-nf |
| 68 | RNAseq-fusion-nf | STAR-Fusion 基因融合检测 | https://github.com/IARCbioinfo/RNAseq-fusion-nf |
| 69 | LncPipe | 长链非编码 RNA 综合分析 | https://github.com/likelet/LncPipe |
| 70 | RNAseq-transcript-nf | 转录本鉴定与定量（BAM 输入） | https://github.com/IARCbioinfo/RNAseq-transcript-nf |
| 71 | gene-fusions-nf | Arriba 基因融合检测 | https://github.com/IARCbioinfo/gene-fusions-nf |
| 72 | quantiseq-nf | RNA-seq 免疫细胞含量定量 | https://github.com/IARCbioinfo/quantiseq-nf |
| 73 | rnaflow | 简单 RNA-Seq 差异基因表达 | https://github.com/hoelzer-lab/rnaflow |
| 74 | RNAseq-Flow | RNA-seq 数据处理 | https://github.com/Dowell-Lab/RNAseq-Flow |
| 75 | vast-tools-nf | VAST-TOOLS 可变剪接 profiling | https://github.com/evanfloden/vast-tools-nf |
| 76 | icgc-featurecounts | RNA-seq BAM 文件 featureCounts | https://github.com/qbicsoftware/icgc-featurecounts |
| 77 | ShortRNA-nf | shortRNA-seq 数据处理 | https://github.com/abreschi/ShortRNA-nf |
| 78 | lncRNA-Annotation-nf | lncRNA 注释（STAR + Cufflinks + FEELnc） | https://github.com/evanfloden/lncRNA-Annotation-nf |
| 79 | DGE | DESeq2 差异表达分析（对接 nf-core/rnaseq 输出） | https://github.com/lconde-ucl/DGE |
| 80 | allele_specific_RNAseq | 等位基因特异性 RNA-seq | https://github.com/biocorecrg/allele_specific_RNAseq |
| 81 | rnadeseq | RNA-seq 差异表达 + 通路分析 | https://github.com/qbic-pipelines/rnadeseq |
| 82 | longread-svs-nf | 长读长结构变异检测 | https://github.com/WarrenLab/longread-svs-nf |
| 83 | kallisto-nf | Kallisto & Sleuth RNA-Seq 工具 | https://github.com/cbcrg/kallisto-nf |

### 2.4 Long Reads (Nanopore / PacBio)

| # | 流程 | 描述 | 仓库 |
|---|------|------|------|
| 84 | nanoseq | Nanopore 拆分、QC、比对 | https://github.com/nf-core/nanoseq |
| 85 | umi-pipeline-nf | ONT UMI 测序数据分析 | https://github.com/genepi/umi-pipeline-nf |
| 86 | isoseq | PacBio Iso-Seq 基因组注释（subreads → FLNC → bed） | https://github.com/nf-core/isoseq |
| 87 | master_of_pores | direct RNA Nanopore reads 分析 | https://github.com/biocorecrg/master_of_pores |
| 88 | ngs-preprocess | Illumina/Nanopore/PacBio 数据预处理 | https://github.com/fmalmeida/ngs-preprocess |
| 89 | MpGAP | 多平台基因组组装（Illumina + Nanopore + PacBio） | https://github.com/fmalmeida/MpGAP |
| 90 | porefile | ONT reads 全长 16S profiling | https://github.com/microgenlab/porefile |
| 91 | MOP2 | Nanopore reads 分析 | https://github.com/biocorecrg/MOP2 |
| 92 | nano-rave | ONT 数据快速现场 QC + variant calling | https://github.com/sanger-pathogens/nano-rave |
| 93 | Donut_Falls | Nanopore 测序基础流程 | https://github.com/UPHL-BioNGS/Donut_Falls |
| 94 | nf-m6anet | Nanopore direct RNA-seq m6A 检测 | https://github.com/MaestSi/nf-m6anet |
| 95 | nanocompore_pipeline | Nanocompore 分析 | https://github.com/tleonardi/nanocompore_pipeline |
| 96 | nabseq_nf | NAb-seq 分析（hybridoma/single B cell Nanopore） | https://github.com/kzeglinski/nabseq_nf |
| 97 | nf-core-wgsnano | Nanopore WGS 分析 | https://github.com/dhslab/nf-core-wgsnano |
| 98 | Long-Read-Proteogenomics | 长读长 RNA-seq + 质谱蛋白质异构体检测 | https://github.com/sheynkman-lab/Long-Read-Proteogenomics |
| 99 | polishCLR | CLR 组装抛光 | https://github.com/isugifNF/polishCLR |
| 100 | — | （完整 100 个） | — |
