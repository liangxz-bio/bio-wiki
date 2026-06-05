# Wiki Log

> Chronological record of all wiki actions. Append-only.
> Format: `## [YYYY-MM-DD] action | subject`
> Actions: ingest, update, query, lint, create, archive, delete

## [2026-05-04] create | Wiki initialized
- Domain: 肿瘤早筛 / 多组学 / mNGS / tNGS / 临床微生物检测 / 生信方法
- Structure: SCHEMA.md, index.md, log.md created

## [2026-05-04] ingest | 王辉团队 tNGS EBioMedicine 2024
- Created: raw/papers/wanghui-tngs-ebiomedicine-2024.md
- Created: entities/kingcreate-tngs-panel.md
- Created: comparisons/tngs-vs-mngs.md
- Updated: index.md
- V2 update from PDF (full text, 16 pages): added RPhK/RPM thresholds, LoD table, costs, bioinfo pipeline details, tool versions, SRA accession; raw/ replaced with pymupdf full-text (70KB, 2055 lines)

## [2026-05-04] ingest | Li B qmNGS AEM 2021
- Created: raw/papers/li2021-qmngs-aem.md (pymupdf full text, 51KB, 11 pages)
- Created: entities/qmngs-internal-standards.md
- Updated: index.md
- Updated: citation (added PMCID: PMC8373253)

## [2026-05-04] ingest | Wanghu KCQ Microb Genom 2024
- Created: raw/papers/wanghu2024-kcq-microbgenom.md (pymupdf full text, 43KB, 11 pages)
- Created: entities/kingcreate-kcq.md
- Updated: entities/kingcreate-tngs-panel.md (added KCQ cross-ref)
- Updated: index.md

## [2026-05-04] update | Replaced all 3 raw/papers with MinerU (magic-pdf) markdown
- **li2021-qmngs-aem.md**: 793 lines → 203 lines, MinerU clean markdown (was pymupdf with page markers)
- **wanghu2024-kcq-microbgenom.md**: 565 lines → 229 lines, MinerU clean markdown (was pymupdf)
- **wanghui-tngs-ebiomedicine-2024.md**: 2056 lines → 306 lines, MinerU clean markdown (was pymupdf)
- **images/**: 28 figure files from all 3 papers copied to raw/papers/images/
- Frontmatter preserved (citation, DOI, sha256); extracted_by updated to MinerU (magic-pdf)

## [2026-05-04] update | Added domain taxonomy to SCHEMA + raw papers + index
- SCHEMA.md: added `domain` field to frontmatter spec, added Domain Taxonomy table (6 domains)
- raw/papers/*.md: added `domain:` to frontmatter (li2021 → quantitative-mngs, wanghu2024 → quantitative-mngs, wanghui → tngs-comparison)
- index.md: Raw Sources regrouped by domain with short citation tags

## [2026-05-04] ingest | DrugBLIP Bioinformatics 2026
- Created: raw/papers/wang2026-drugblip-bioinformatics.md (MinerU clean markdown, 310 lines, 17 images)
- SCHEMA.md: added domain `ai-drug-discovery` to frontmatter spec and Domain Taxonomy
- index.md: added Raw Source entry under ai-drug-discovery domain

## [2026-05-04] ingest | WisecondorX Nucleic Acids Res 2019
- Created: raw/papers/raman2019-wisecondorx-nar.md (MinerU clean markdown, 266 lines, 12 images)
- Created: entities/wisecondorx.md — sWGS CNV检测工具，6种CNA工具归一化对比，WisecondorX改进
- Updated: index.md: added raw/papers entry under bioinfo-pipeline domain + entity entry
- Copied 12 images to raw/papers/images/

## [2026-05-04] update | WisecondorX entity: 补充三种归一化范式 (case-control 分析方法)
- Added "CNV Calling 的三种归一化范式" section, 明确分为 reference-free / pooled reference / matched case-control 三类
- 工具比较表增加「范式」列，BIC-seq2 标注为同时支持参考无关和配对模式
- 增加 case-control 方法描述：配对样本对比 + MAF辅助判定

## [2026-05-06] ingest | 破译生命密码：单细胞测序组学分析指南（微信公众号）
- Created: raw/articles/scrnaseq-analysis-guide-wechat.md — 单细胞测序标准化分析全流程（湿实验建库、生信分析、降维聚类、差异分析、临床转化）
- Created: concepts/scrnaseq-workflow.md — scRNA-seq 标准化分析流程概念页（7步流程详解），cross-ref [[wisecondorx]]
- Updated: index.md — 新增 Concepts 节 + raw/articles 条目

## [2026-05-06] ingest | scNotebooks 开源单细胞与空间组学培训平台（微信公众号）
- Created: raw/articles/NatGen-scNotebooks开源单细胞空间组学培训笔记库.md — workbuddy 抓取，完整推文 + YAML frontmatter
- Created: entities/scnotebooks.md — scNotebooks 实体页（Nature Genetics 2026），14 模块三层结构，多语言 + 云端模式
- Updated: concepts/scrnaseq-workflow.md — 新增「培训资源」节，cross-ref [[scnotebooks]]；sources 追加新 raw 文件
- Updated: index.md — Entities 新增 scnotebooks、Raw Sources 新增 articles 条目，13 页

## [2026-05-06] ingest | fastSC R 包文档（微信公众号·生信摆渡）
- Created: raw/articles/R包fastSC单细胞转录组快速数据分析和可视化.md — workbuddy 抓取 + YAML frontmatter
- Created: entities/fastsc.md — fastSC v1.0.2 实体页，功能模块（数据准备/sc_pipeline/可视化/CellChat/Monocle/inferCNV）
- Updated: concepts/scrnaseq-workflow.md — 新增「R 包工具」节，cross-ref [[fastsc]]；sources 追加
- Updated: index.md — Entities 新增 fastsc、Raw Sources 新增 articles 条目，15 页

## [2026-05-06] ingest | HLA检测生信算法工程师面试核心问题与解答（微信公众号·AI生信工坊）
- Created: raw/articles/HLA检测生信算法工程师面试核心问题与解答.md — workbuddy 抓取 + YAML frontmatter
- Updated: index.md — Raw Sources 新增 articles 条目，16 页

## [2026-05-06] ingest | NEXTFLOW工作流清单100个（微信公众号·逻捷科技）
- Created: raw/articles/Nextflow工作流清单100-微生物组转录组变异三代测序.md — web_fetch 抓取 + 结构化整理 + YAML frontmatter
- Updated: index.md — Raw Sources 新增 articles 条目，17 页

## [2026-05-06] ingest | NEXTFLOW工作流清单第二期 101-200（微信公众号·逻捷科技）
- Created: raw/articles/Nextflow工作流清单100-比对注释单细胞101-200.md — web_fetch 抓取 + 结构化整理（15个子专题96个流程）+ YAML frontmatter
- Updated: index.md — Raw Sources 新增 articles 条目，18 页

## [2026-05-06] create | 10个 Nextflow 流程 Entities 页面（从 raw 提炼）
- Created: entities/nfcore-microbiome-pipelines.md — 42 个微生物组/宏基因组流程，交叉引用 [[tngs-vs-mngs]] [[kingcreate-tngs-panel]] [[kingcreate-kcq]]
- Created: entities/nfcore-variant-calling-pipelines.md — 30 个变异检测流程（SNV/SV/CNV），交叉引用 [[wisecondorx]] [[nfcore-scrnaseq-pipelines]]
- Created: entities/nfcore-rnaseq-pipelines.md — 28 个 RNA-seq/转录组流程，交叉引用 [[scrnaseq-workflow]] [[nfcore-scrnaseq-pipelines]] [[fastsc]]
- Created: entities/nfcore-longread-pipelines.md — 17 个三代测序流程（Nanopore/PacBio），交叉引用 [[nfcore-rnaseq-pipelines]] [[nfcore-genome-pipelines]]
- Created: entities/nfcore-scrnaseq-pipelines.md — 12 个单细胞/空间转录组流程，交叉引用 [[scrnaseq-workflow]] [[fastsc]] [[scnotebooks]] [[wisecondorx]]
- Created: entities/nfcore-genome-pipelines.md — 27 个基因组组装与注释流程，交叉引用 [[nfcore-microbiome-pipelines]] [[kingcreate-kcq]]
- Created: entities/nfcore-epigenomics-pipelines.md — 13 个表观组学流程（ChIP/ATAC/Hi-C），交叉引用 [[wisecondorx]] [[nfcore-scrnaseq-pipelines]]
- Created: entities/nfcore-phylogenomics-pipelines.md — 16 个系统发育与 pan-基因组流程，交叉引用 [[nfcore-microbiome-pipelines]] [[nfcore-genome-pipelines]]
- Created: entities/nfcore-preprocessing-pipelines.md — 10 个预处理与 QC 流程，交叉引用 [[nfcore-microbiome-pipelines]] [[nfcore-variant-calling-pipelines]]
- Created: entities/nfcore-proteomics-pipelines.md — 4 个蛋白质组与结构预测流程，交叉引用 [[nfcore-variant-calling-pipelines]] [[kingcreate-kcq]]
- Updated: index.md — Entities 新增 10 个 nfcore-* 条目，Total pages: 18 → 28

## [2026-05-06] ingest | GB/T 46943-2025 mNGS 性能确认国家标准
- Created: raw/papers/gbt46943-2025-mngs-validation.md — MinerU full.md (463 lines, 4 images) cp + YAML frontmatter
- Created: entities/gbt46943-2025-mngs-standard.md — 国家标准实体页，梳理性能确认架构（标本制备/关键环节/完整流程/报告要求）
- Updated: entities/kingcreate-kcq.md — 交叉引用 [[gbt46943-2025-mngs-standard]]（LoD/精密度要求）
- Updated: comparisons/tngs-vs-mngs.md — 交叉引用 [[gbt46943-2025-mngs-standard]]（明确排除tNGS）
- Updated: entities/nfcore-microbiome-pipelines.md — 交叉引用 [[gbt46943-2025-mngs-standard]]（生信流程ROC/临界点要求）
- Updated: index.md — 新增 domain clinical-evaluation、Entities + Raw Sources 条目，Total pages: 28 → 29 · domains: 4 → 5


## [2026-05-06] ingest | 肿瘤全景变异检测探针设计专家共识2026版（吉因加·微信公众号）
- Created: raw/articles/基于NGS的肿瘤全景变异检测探针设计专家共识2026版.md — YAML frontmatter (citation/DOI/sha256), domain: panel-design
- Created: entities/cgp-probe-design-consensus-2026.md — 6大共识要点（基因分级/探针设计/性能评估/变更管理）+ 参数速查表
- Updated: entities/kingcreate-tngs-panel.md — 交叉引用 [[cgp-probe-design-consensus-2026]]（探针设计规范对照）
- Updated: entities/gbt46943-2025-mngs-standard.md — 交叉引用 [[cgp-probe-design-consensus-2026]]（NGS质量双支柱）
- Updated: index.md — Entities + Raw Sources (panel-design) 新增，Total pages: 29 → 30 · domains: 5 → 6

## [2026-05-07] ingest | RegFormer 单细胞 Mamba 基础模型（微信公众号·生物信息与人工智能）
- Raw: raw/articles/RegFormer-单细胞大模型Mamba架构-NatCommun2026.md — YAML frontmatter (citation/DOI/sha256), domain: ai-drug-discovery, 94 lines
- Created: entities/regformer.md — RegFormer 实体页：Mamba SSM + 基因调控拓扑排序 + 双重嵌入，跨组织聚类/GRN重建/扰动推演/药物敏感性预测
- Updated: concepts/scrnaseq-workflow.md — 新增「单细胞基础模型」节，cross-ref [[regformer]]；sources 追加；updated bumped to 2026-05-07
- Updated: index.md — Entities + Raw Sources (ai-drug-discovery) 新增，Total pages: 30 → 32

## [2026-05-07] upgrade | 肿瘤CGP探针设计专家共识2026版：WeChat → MinerU PDF 全文
- Raw: raw/articles/基于NGS的肿瘤全景变异检测探针设计专家共识2026版.md — 全文替换为 MinerU full.md (109 lines WeChat 摘要 → 180 lines 官方期刊全文)
- Images: raw/papers/images/cgp-probe-design-consensus-2026-*.png (5 figures)
- Entity [[cgp-probe-design-consensus-2026]] 内容未变（共识要点已覆盖），无需更新

## [2026-05-07] ingest | R语言SCI论文7种图可视化 DeepSeekV4
- Raw: raw/articles/R语言SCI论文7种图可视化DeepSeekV4.md — R语言SCI论文7种图完整代码教程（ROC/校准/DCA/列线图/OncoPrint/桑基/SHAP），来自RAI王博士微信公众号
- Frontmatter: title/type/domain/tags/sha256 added
- Updated: index.md (bioinfo-pipeline section)

## [2026-05-07] ingest | 生物信息学数据格式终极指南（微信公众号·宏序生物）
- Raw: raw/articles/生物信息学数据格式终极指南.md — 生信核心数据格式大全（FASTA/FASTQ、SAM/BAM、GFF/GTF/BED、VCF/BCF、PDB/mmCIF、PHYLIP/Newick、HDF5/mzML），228 lines
- Updated: index.md (bioinfo-pipeline section, Total pages: 32 → 33)

## [2026-05-08] create | Codebase Index (wiki + ~/code/ monorepo)
- Created: ~/code/ — 统一代码组织入口（软链接聚合），子目录：python/perl/r/rust/pipelines/vendor/scripts/bash
- Created: ~/code/README.md + 各子目录 README.md — 代码分布总览 + symlink 指向真实位置
- Created: entities/codebase-index.md — wiki 侧代码索引实体页面，跨引用 nfcore-* 系列
- Updated: index.md — 新增 Code Index 节，Total pages: 34 → 35

## [2026-05-08] update | 规范 wiki 图片命名
- raw/papers/images/: 24 张图片重命名为 `{paper-slug}-fig{N}.jpg` 格式
- 涉及 6 篇文献：li2021-qmngs-aem, wanghu2024-kcq, wanghui2024-tngs, raman2019-wisecondorx, wang2026-drugblip, cgp-probe-design-consensus-2026
- 43 张孤立子图移至 `images/_orphans/`
- CGP 探针共识文章图片引用路径修正（`images/` → `../papers/images/`）
- 创建 images/MAPPING.md 记录新旧文件名对照

## [2026-05-08] ingest | SCMBench 单细胞多组学集成基准测试（微信公众号·生物信息前沿进展）
- Raw: raw/articles/SCMBench-单细胞多组学集成分析基准测试-NatCommun2026.md — YAML frontmatter (citation/DOI/sha256), domain: bioinfo-pipeline, 183 lines
- Created: entities/scmbench.md — SCMBench 实体页，23种方法对比（19 DMs + 4 FMs），关键发现（GLUE/scJoint 领先，FMs适配策略提升 +65%），6图6维评估
- Updated: index.md — Entities + Raw Sources 新增，Total pages: 34 → 36

## [2026-05-08] update | entities/wisecondorx.md — 扩增核心算法详解
- 核心算法从5条要点展开为5步详解：PCA归一化原理、within-sample bin搜索（最核心创新）、加权CBS分段、三级z-score框架、beta嵌合体判定
- 新增"CNV检测三种归一化范式"算法视角对比（Reference-free / Pooled reference / Matched case-control）
- 新增 within-sample vs 传统方法的公式对比
- updated bumped to 2026-05-08

## [2026-05-08] create | concepts/star-rsem-rnaseq.md — STAR + RSEM 转录组比对与定量
- Created: concepts/star-rsem-rnaseq.md — STAR 剪接感知比对（suffix array + seed-extend）、--quantMode 双输出（TranscriptomeSAM + GeneCounts）、RSEM EM 算法 isoform 定量、与 featureCounts 对比、transcriptome pipeline 实际配置
- Updated: index.md — Concepts 节新增 [[star-rsem-rnaseq]]，Total pages: 38 → 39
- Created: concepts/kraken2-classification.md — k-mer + LCA 分类原理、--confidence 评分机制、Bracken Bayesian 丰度重估、InnoPAP mNGS 四层过滤流程（Kraken2 → filter → Bracken → BLAST/ref genome）
- Updated: index.md — Concepts 节新增 [[kraken2-classification]]，Total pages: 37 → 38
- Created: concepts/bowtie2-alignment.md — BWT/FM-index 算法原理 + local vs end-to-end 模式对比 + InnoPAP pipeline 实际配置对照表（tNGS local / mNGS end-to-end / 产物比对 -k 1）
- Updated: index.md — Concepts 节新增 [[bowtie2-alignment]]，Total pages: 36 → 37

## [2026-05-07] ingest | 生物信息学文件格式指南（微信公众号·生信技术）
- Raw: raw/articles/生物信息学文件格式指南-FASTA-FASTQ-SAM-BAM-VCF-GFF-GTF.md — 8种生信核心格式详解：FASTA/FASTQ/SAM/BAM（FLAG位标志/CIGAR操作符/MAPQ）/VCF/BCF/GFF（GFF2 vs GFF3）/GTF，286 lines
- Updated: index.md (bioinfo-pipeline section, Total pages: 33 → 34)

## [2026-05-08] ingest | HLA行业标准 + WGD免疫逃逸 Cancer Cell

### raw/papers: 新增2篇MinerU全文

1. **ws-hla-genotyping-standard-2021.md** — 人类白细胞抗原基因分型检测体系技术标准
   - WS行业标准，2021年发布，北京医院/中华骨髓库等起草
   - 3种分型方法（SSP/SSO/SBT）+ 环境设施 + 质控体系 + 报告要求
   - 775行纯文字，无图片（2张孤立子图→_orphans/）
   - domain: bioinfo-pipeline

2. **foidart2026-wgd-cancercell.md** — Whole-genome doubling drives immune evasion by silencing antigen presentation
   - Foidart P, Li Z, Cai X et al. (Kornelia Polyak lab, Dana-Farber/Harvard)
   - Cancer Cell 2026, doi:10.1016/j.ccell.2026.04.007
   - 613行, 8张图（foidart2026-wgd-fig1~8.jpg）
   - 核心发现：WGD→琥珀酸↑→KDM6↓→H3K27me3↑→MHC-I沉默→免疫逃逸
   - domain: bioinfo-pipeline

### entities/concepts: 新增2个页面

- Created: entities/ws-hla-genotyping-standard.md — HLA检测体系技术标准实体页
  - cross-ref: [[HLA检测生信算法工程师面试核心问题与解答]], [[nfcore-variant-calling-pipelines]], [[gbt46943-2025-mngs-standard]]
- Created: concepts/wgd-immune-evasion.md — WGD驱动免疫逃逸概念页
  - 代谢-表观遗传机制详解（KDM6/H3K27me3/MHC-I/IFNγ）
  - 治疗意义表格（YM155/BIRC5, EED226/PRC2, anti-PD-L1）
  - cross-ref: [[wisecondorx]], [[scrnaseq-workflow]], [[star-rsem-rnaseq]], [[gbt46943-2025-mngs-standard]]

### index.md: 4条新增，总数 39→43
### images/MAPPING.md: 新增Foidart 2026 WGD 8图映射
- 5张孤儿子图移至 _orphans/


## [2026-05-08] ingest | PacBio HiFi SV 结构变异分析流程（微信公众号）

### raw/articles
- raw/articles/PacBio-HiFi-SV结构变异分析完整流程.md — 芈小小，一问一答-2024启航++微信公众号
  112行，6步流程：ccs→pbmm2→pbsv→AnnotSV→SnpSift→IGV
  domain: bioinfo-pipeline

### concepts
- Created: concepts/sv-analysis-pacbio-hifi.md — 紧凑概念页（新标准）
  6步流程表格 + 工具清单 + cross-ref [[nfcore-variant-calling-pipelines]] [[wisecondorx]]

### index.md: 2条新增，总数 43→45


## [2026-05-08] ingest | ONT SV 结构变异分析流程
- Added: raw/articles/ONT数据进行SV分析流程-微信芈小小.md — 芈小小 ONT 三代 SV 流程，minimap2→cuteSV/Sniffles2/SVIM→多工具交叉验证→VEP 注释→可视化
- Created: concepts/ont-sv-analysis.md — ONT SV 流程概念页，含流程概览表格+核心工具参数+与 PacBio HiFi 对比表，交叉引用 [[sv-analysis-pacbio-hifi]]
- Updated: index.md (total 47, added concept + raw article entries)


## [2026-05-09] ingest | Yuan & Bluth 2025 新一代病毒疫苗策略 (Vaccines 2025)

### raw/papers
- raw/papers/yuan2025-nextgen-vaccines-vaccines.md — MinerU (magic-pdf) full text, 373行, 3张图
  - Yuan F, Bluth MH. Vaccines 2025, 13, 979. doi:10.3390/vaccines13090979
  - 四大方向: 结构抗原工程 / mRNA+LNP / 佐剂系统 / 免疫印记 + anti-idiotypic
  - domain: vaccine-design

### images
- yuan2025-fig1-mrna-types-delivery.jpg — Fig 1: mRNA 类型与递送系统
- yuan2025-fig2-immune-imprinting.jpg — Fig 2: SARS-CoV-2 免疫印记
- yuan2025-fig3-anti-idiotypic.jpg — Fig 3: 抗独特型疫苗策略
- 3个 orphan (子图) 存档至 _orphans/
- MAPPING.md 已建

### concepts
- Created: concepts/nextgen-vaccine-strategies.md — 新一代病毒疫苗设计四大策略概念页
  - 核心发现链: 结构工程 → mRNA 平台 → 佐剂 → 免疫印记应对
  - 交叉引用: [[virus-vaccine-pipeline]] 管线

### SCHEMA.md
- Added domain: `vaccine-design` + tag taxonomy: immunology/vaccine, platform, vaccine-strategy

### index.md: 新增2条 (raw + concept)，总页数 54，添加 vaccine-design domain


## [2026-05-09] ingest | mRNA疫苗相关文章 × 2 (微信公众号)

### raw/articles
- raw/articles/mRNA肿瘤个性化疫苗-从计算设计到临床转化.md (217行)
  - 微信公众号 TGENES, 作者马伯君
  - 内容: 肿瘤mRNA个性化疫苗全流程 — 新抗原发现→计算设计→LNP/mRNA→临床验证
  - domain: vaccine-design
- raw/articles/Cell经典研究-Moderna寨卡病毒mRNA疫苗的设计.md (109行)
  - 微信公众号 耀菌君
  - 内容: Moderna ZIKV mRNA 疫苗设计 — 天然构象 vs 融合前构象策略、LNP 递送、小鼠模型
  - domain: vaccine-design

### index.md: vaccine-design 新增2条 raw articles
### SCHEMA.md未改 (vaccine-design domain 已存在)


## [2026-05-09] ingest | 疫苗相关PDF × 3 (Cao 广谱疫苗 / Zheng DHODH / Zhong 重组蛋白)

### raw/papers
- raw/papers/cao2025-broad-vaccines.md (302行, 4图)
  - Cao M, Li Y, Song X, et al. Broad-spectrum vaccines (J Virol 2025, jvi.00997-25)
  - 核心: 结构抗原工程(2P/DS-Cav1/SOSIP) → 纳米颗粒(I53-50/MI3/mosaic-8) → mRNA+VLP → 靶向递送
  - domain: vaccine-design
- raw/papers/zheng2022-dhodh-antivirals.md (275行, 3图)
  - Zheng Y, Li S, Song K, Ye J, Li W, Zhong Y, Feng Z, Liang S, Cai Z, Xu K (Viruses 2022, 14, 928)
  - 注: 非疫苗, 是宿主靶向广谱抗病毒 (DHODH 抑制剂), 但徐可团队也做重组蛋白疫苗 (见下篇)
  - domain: vaccine-design
- raw/papers/zhong2024-recomb-vaccine-progress.md (360行, 3图)
  - 钟一帆, 袁为锋, 徐可. 病毒性传染病的重组蛋白疫苗研究进展 (武汉大学学报 2024)
  - 核心: 免疫原设计三类(偶联/工程化/重构) + 表达纯化(CHO/酵母/E.coli) + 规模化生产(反应器/细胞工厂)
  - domain: vaccine-design

### images
- cao2025-fig1~4: 疫苗发展里程碑/纳米颗粒类型/mRNA-VLP联用/靶向递送
- zheng2022-fig1~3: 嘧啶通路/DHODHi三机制/结合位点
- zhong2024-fig1~3: 疫苗分类/重组蛋白制备/表达流程
- 共8个 orphan 存档至 _orphans/
- MAPPING.md 全面更新（4篇论文索引）

### index.md: vaccine-design 新增3条 raw papers，总页数 57


## [2026-05-09] ingest | SpaNiche 空间转录组共定位分析 (Genome Biol 2026)

### raw/articles
- raw/articles/SpaNiche-空间转录组共定位与互作分析框架-GenomeBiology.md (115行)
  - Huang S, Ran Q, Tang J, Wang X, Xi J, Ma S, Xi R. Genome Biol 2026
  - 微信公众号「TOP生物信息」(TOP菌)
  - 核心: Niche识别→共定位评分(LR/PD/BK/LS) → 细胞互作推断 → 空间可视化
  - domain: bioinfo-pipeline

### index.md: bioinfo-pipeline 新增1条，总页数 58


## [2026-05-09] ingest | Jiang DUV LED 呼吸道 RNA 病毒灭活 (Opto-Electron Adv 2023)

### raw/papers
- raw/papers/jiang2023-duv-led-virus-inactivation.md (254行, 6图)
  - Jiang K, Liang SM, Sun XJ, Ben JW, Qu L, Zhang SL, Chen Y, Zheng YC, Lan K, Li DB, Xu K
  - Opto-Electronic Advances 2023, 6, 230004. doi:10.29026/oea.2023.230004
  - 核心: HTA AlN/Sapphire 模板上生长的深紫外 LED → 灭活 IAV > 99.99% + SARS-CoV-2 > 99.99%
  - 机制: 破坏病毒 RNA 完整性 (RT-qPCR 验证 RNA 水平显著下降)
  - 注: 徐可团队 (徐可也为 zheng2022 和 zhong2024 通讯作者)
  - domain: vaccine-design

### images
- 6张参考图 (AlGaN 外延/DUV LED 结构/病毒灭活/RNA 水平)
- 2个 orphan 存档
- MAPPING.md 已追加

### index.md: vaccine-design 新增1条，总页数 59

## [2026-05-09] ingest | DeepVariant tool overview
- Created: raw/articles/deepvariant-tool-overview.md
  - sha256: a2a8e2a722f3bcfe62913cdcdae86c65a0729faf82012da842f4b518f86b2836
  - domain: bioinfo-pipeline
  - Content: 方法原理（pileup 图像化→CNN→VCF）、模型类型与平台支持、性能基准 vs GATK HaplotypeCaller、版本演进（v0.1→v1.8）、工具生态（GLnexus/DeepTrio/Clair3）、局限性与 tNGS 场景适用性
- Updated: index.md (bioinfo-pipeline 新增1条，总页数 60)

## [2026-05-09] batch-ingest | DeepVariant 配套文章 × 5
- Added frontmatter + sha256 + cross-refs to 5 existing raw/articles:
  1. raw/articles/群体遗传学分析-4.2-SNP-calling-DeepVariant
     - sha256: 8991480c35d13c9e3a4ea8293cd512dea833a1c413b812f1482128a5e06e8b76
     - 微信公众号·小伍的科研笔记，DeepVariant Docker 安装与 WGS/HiFi 实战
  2. raw/articles/DeepVariant又快又准-替代GATK的变异检测工具
     - sha256: e438440006c570b113f1c895e679e2335420592072f5f2fd725b97acd863bbba
     - 微信公众号·生信开发者，DeepVariant vs GATK 性能案例
  3. raw/articles/DeepVariant-SnakeMake流程-Workflow简介
     - sha256: cd57021f881a520a142ceab0a2300ed0c8490db6be239f90ff8d08e1f079efd6
     - 微信公众号·生信探索，SnakeMake + DeepVariant 流程编排
  4. raw/articles/minimap2-DeepVariant-HiFi检测SNP-INDEL
     - sha256: d421512cabf25d7873d8d2833602fc073323ec8cc097c637c164ce4eaf3e9fe8
     - 微信公众号·允思拓，PacBio HiFi + DeepVariant
  5. raw/articles/DeepSomatic-多测序技术体细胞变异检测-NatBiotechnol2025
     - sha256: 4c25bdb47eea7accb7bcbbca06419a3ab7836c9a97af00023f34d79b1df6ccbe
     - Google DeepSomatic Nat Biotechnol 2025，Illumina/PacBio/ONT 体细胞变异
- Updated: raw/articles/deepvariant-tool-overview.md (added companion article cross-refs at bottom)
- Updated: index.md (bioinfo-pipeline 新增5条，总页数 60 → 65)

## [2026-05-09] ingest | 利用可变剪接开发肝癌通用型mRNA新抗原疫苗 (Cell Res 2025, 微信公众号·生信探索)

### raw/articles
- raw/articles/利用可变剪接开发肝癌通用型mRNA新抗原疫苗-CellResearch2025.md
  - YAML frontmatter: source_url/ingested/sha256/citation/domain: vaccine-design
  - sha256: e14c801f03a6f5640b2bb521abaeb4f7fb94f4d6d1d16189c4b956f5043d79e0
  - Citation: Zhao H, et al. Alternative splicing generates universal neoantigens for mRNA vaccine development in hepatocellular carcinoma. Cell Res. 2025 Dec;35(12):970-986. PMID:40446883; PMCID:PMC12066522
  - Core: AS events 59x mutations, AS-NeoAgs cover 50.94% patients, TAPi antigens bypass 62% TAP deficiency, mRNA-LNP vaccine effective in CDX/PDX models

### concepts
- Created: concepts/as-neoantigen-hcc-vaccine.md — AS来源新抗原的肝癌通用型mRNA疫苗概念页
  - compact format: 核心发现链(AS→筛选→TAPi→mRNA-LNP→体内疗效) + 关键证据摘要(7条) + 与突变来源个性化新抗原对比表
  - cross-ref: [[nextgen-vaccine-strategies]], [[raw/articles/mRNA肿瘤个性化疫苗-从计算设计到临床转化]]

### index.md: 新增2条(raw + concept)，总页数 65→67

## [2026-05-10] ingest | CroCoDeEL 宏基因组交叉样本污染检测 (Nat Commun, 微信公众号·小包子学演化)

### raw/articles
- raw/articles/CroCoDeEL-宏基因组交叉样本污染检测-NatCommun.md
  - YAML frontmatter: source_url/ingested/sha256/citation/domain: bioinfo-pipeline
  - sha256: 8a62e22a5986fd277af0ebd8560782855f8917068ef98cab835b607fbe3f1f27
  - Citation: 小包子学演化. CroCoDeEL：无需对照准确检测宏基因组数据中的交叉样本污染. 微信公众号「小包子学演化」. 2026. 原论文: Nature Communications.
  - Core: 污染线原理(对数散点图比例关系) → RANSAC检测 → 10 SHAP特征 → RF分类; 0.1%最低检测限; Meteor2 > Sylph > MetaPhlAn4; 案例: TwinsUK 202/1004污染, Ferretti 80%菌株假象, 婴儿纵向菌群污染

### entities
- Created: entities/crocodeel.md — CroCoDeEL 工具实体页
  - compact format: 核心原理→四步算法→性能表→重要发现→局限性→技术栈→相关概念
  - cross-ref: [[metagenomics-qc-workflow]], [[scrub]]

### index.md: 新增2条(entity + raw)，总页数 67→69

## [2026-05-10] ingest | CroCoDeEL 原始论文 PDF (Goulet L, Nat Commun 2026)

### raw/papers
- Created: raw/papers/goulet2026-crocodeel-nc.md
  - MinerU extraction from PDF (94KB, 432 lines)
  - YAML frontmatter: source_url/ingested/sha256/citation/domain/extracted_by
  - sha256: d526506f0d2d5df59249cdd9d19dbeac7e93966ea7478df348817f3fcef8e788
  - Citation: Goulet L, Plaza Onate F, Famechon A, et al. CroCoDeEL: accurate control-free detection of cross-sample contamination in metagenomic data. Nat Commun. 2026. Article in Press.
  - 7 figures embedded (copied to images/goulet2026-crocodeel-nc/) + 3 orphan images

### images
- Copied 10 images to raw/papers/images/goulet2026-crocodeel-nc/
- Updated MAPPING.md with image descriptions (Fig 1-7)

### entities
- Updated: entities/crocodeel.md — added original paper source, cascade contamination, Lou P2 case, Ferretti/TwinsUK details, training data specs

### index.md: 新增1条(raw/papers)，总页数 69→70
## [2026-05-11] ingest | Kraken RTL 路径打分算法解析 (MetagenomeBioin 微信公众号)

### raw/articles
- raw/articles/宏基因组-Kraken-RTL路径打分算法解析.md
  - YAML frontmatter: source_url/ingested/sha256/citation/domain
  - sha256: 1ee50b5cfc6b758684a6e274f11047b281473b857162448097ef4f373eb4594a
  - Citation: "Ethan Ethan. 宏基因组 | 通过Kraken学习RTL路径打分. MetagenomeBioin 微信公众号. 2026"
  - Domain: bioinfo-pipeline
  - Content: Kraken 原始 RTL (Root To Leaf) 路径打分算法详解：k-mer投票→节点权重→RTL路径累加→最终分类，与简单投票法对比（属级间接支持的优势），形式化定义 + 完整示例

### concepts
- Updated: concepts/kraken2-classification.md — added cross-reference to new Kraken RTL raw article

### index.md: bioinfo-pipeline 新增1条，总页数 70→86

## [2026-05-11] ingest | CNVPipe WGS CNV 检测流程 (bioRxiv 2025, 我爱学生信 微信公众号)

### raw/articles
- raw/articles/CNVPipe-基于机器学习的WGS拷贝数变异检测流程-bioRxiv2025.md
  - 已存在，补 YAML frontmatter: source_url/ingested/sha256/citation/domain
  - sha256: f9043e5f762531b50bda03c35fff89f3d2d7a8eb47fefc13589b316a413d3df3
  - Citation: "Sun J, Wong NK, Jiang Z, et al. CNVPipe: An enhanced pipeline for accurate analysis of copy number variation from whole-genome sequencing. bioRxiv 2025. doi:10.1101/2025.05.18.654763"
  - Domain: bioinfo-pipeline
  - Content: CNVPipe WGS CNV 检测流程：Snakemake框架整合CNVKit/CNVpytor/cn.MOPS/Delly/Smoove五工具+深度感知合并+SVM过滤+单细胞CNV模块（倍性估计创新）

### entities
- Created: entities/cnvpipe.md — 新标准，核心发现链+关键证据摘要+交叉引用（wisecondorx/fastsc/scmbench）

### index.md: entities 新增1条 + bioinfo-pipeline 新增1条，总页数 86→88

## [2026-05-12] ingest | MethylVI 单细胞甲基化 VAE (Nat Mach Intell 2025, CellSpace AI 微信公众号)

### raw/articles
- raw/articles/MethylVI-单细胞甲基化VAE贝塔二项分布-NatMachIntell.md
  - YAML frontmatter: source_url/ingested/sha256/citation/domain
  - sha256: 29ed36678fe3879db954fed51b9f0eb98119ad657d42e39e0142fb71983bbced
  - Citation: "Ashuach T, Armendariz DA, Hu GK, et al. MethylVI: a variational autoencoder for modeling single-cell methylation data. Nat Mach Intell. 2025;7:572-583. doi:10.1038/s42256-025-01011-5."
  - Domain: bioinfo-pipeline
  - Content: MethylVI VAE 概率生成模型，Beta-Binomial 似然建模 scBS-seq count 数据过分散，降噪/DMG 检测/batch correction/reference mapping/Multi-omics 五项评估全面优于现有方法

### entities
- Created: entities/methylvi.md — 软件工具实体页，核心发现链+关键证据摘要+交叉引用（scrnaseq-workflow/scmbench/fastsc）

### index.md: entities 新增1条 + bioinfo-pipeline 新增1条

## [2026-05-12] ingest | DeepRare AI 智能体罕见病诊断系统 (Nature 2026, G-Plume 微信公众号)

### raw/articles
- raw/articles/DeepRare-AI驱动罕见病诊断智能体-Nature2026.md
  - YAML frontmatter: source_url/ingested/sha256/citation/domain
  - sha256: d120da81b14dea59253b72f928788c0e32fbbcf3d61a61280857ece12acbbf51
  - Citation: "Zhao W, Wu C, Fan Y, et al. An agentic system for rare disease diagnosis with traceable reasoning. Nature. 2026 Mar;651(8106):775-784. doi:10.1038/s41586-025-10097-9. PMID:41708847; PMCID:PMC12999473."
  - Domain: clinical-evaluation
  - Content: DeepRare MCP启发三层架构（中央LLM+4代理+外部知识），DeepSeek-V3，40+工具，6,401例评估，首次AI超越人类专家（Recall@5 78.5% vs 65.6%）

### entities
- Created: entities/deeprare.md — AI智能体实体页，核心发现链+关键证据摘要+交叉引用（deepvariant/gbt46943-2025-mngs-standard）

### index.md: entities 新增1条 + clinical-evaluation 新增1条

## [2026-05-12] ingest | DeepRare Nature 主刊原始论文 (MinerU)

### raw/papers
- raw/papers/zhao2026-deeprare-nature.md
  - YAML frontmatter: source_url/ingested/sha256/source_file/extracted_by/extraction_date/citation/domain
  - sha256 (body): 6d512fd4f5e99f52308b90cc2fc3db4ee9744146ffaeb0c7583cc98b83c2a61e
  - Citation: "Zhao W, Wu C, Fan Y, et al. An agentic system for rare disease diagnosis with traceable reasoning. Nature. 2026 Mar;651(8106):775-784. doi:10.1038/s41586-025-10097-9. PMID:41708847; PMCID:PMC12999473."
  - Domain: clinical-evaluation
  - MinerU: full.md (784 lines, 114KB), 20 mapped figures + 23 orphans archived
  - MAPPING.md updated with old-SHA → new-name cross-reference

### entities (enriched)
- entities/deeprare.md — 增补原始论文细节：Fig 1-5详细内容、5类失败模式精确占比、Ablation数据、支持LLM后端、Web应用流程、完整作者列表+Affiliations
  - sources: added raw/papers/zhao2026-deeprare-nature.md alongside existing raw/articles entry

### index.md: clinical-evaluation 新增1条 (raw/papers)
## [2026-05-12] ingest | MMseqs2-GPU Nature Methods 2025 (微信公众号·MetagenomeBioin)

### raw/articles
- raw/articles/MMseqs2-GPU同源搜索-NatureMethods2025.md
  - YAML frontmatter: source_url/ingested/sha256/citation/domain
  - Citation: "Mirdita M, Dallago C, Schmidt B, Steinegger M. GPU-accelerated homology search with MMseqs2. Nature Methods. 2025 Oct;22. doi:10.1038/s41592-025-02819-8."
  - Domain: bioinfo-pipeline
  - Source: WeChat article by Ethan Ethan [MetagenomeBioin], summarizing Nat Methods 2025 brief communication
  - Key content: GPU-accelerated gapless filter + Smith-Waterman, shared memory + warp shuffles + 16-bit packing, database streaming (63-65% of in-memory speed), single L40S 177x vs JackHMMER / 6.4x vs BLAST, ColabFold 31.8x end-to-end, Foldseek 4-27x acceleration, ~300ms GPU init overhead, runs on Colab free instances

### entities
- Created: entities/mmseqs2.md — GPU加速同源搜索算法实体页，核心发现链+关键证据摘要+交叉引用（nextgen-vaccine-strategies/crocodeel）

### index.md: entities 新增1条 + bioinfo-pipeline 新增1条

## [2026-05-13] ingest | David Baker Nature 2026 综述：蛋白质从头设计的过去、现在与未来（微信公众号·BrainMed Lab）

### raw/articles
- raw/articles/David-Baker-Nature综述-蛋白质从头设计的过去现在与未来.md
  - YAML frontmatter: source_url/ingested/sha256/citation/domain
  - sha256: 2146d95d4ab0e690fc55cf1b3614749071311c8a9a5877cb93f1544b2fe4e6d7
  - Citation: "Yang W, Wang S, Lee GR, Zhang JZ, Baker D. The past, present and future of de novo protein design. Nature. 2026. (Review, in press) | 微信公众号 BrainMed Lab 中文解读"
  - Domain: ai-drug-discovery
  - Content: David Baker 实验室 Nature 综述中文解读，从头蛋白设计从物理模型→生成式AI的范式转换，折叠/组装/结合已基本解决，前沿为多状态动态纳米机器

### entities
- Created: entities/de-novo-protein-design.md — David Baker Nature 2026 综述实体页，核心发现链+4表关键证据摘要+交叉引用（nextgen-vaccine-strategies/mmseqs2/deeprare）

### index.md: entities 新增1条 + ai-drug-discovery 新增1条 raw article，总页数 96→98

## [2026-05-13] ingest | RFdiffusion Nature 2023 论文详解 — 蛋白质从头设计（微信公众号·AI-Protein Design）

### raw/articles
- raw/articles/RFdiffusion-蛋白质从头设计-Nature论文详解.md
  - YAML frontmatter: source_url/ingested/sha256/citation/domain
  - sha256: 1fe896bf3d1f03f11e026d6529860d482c7fcc6e518445b69e9104e8596bef32
  - Citation: "Watson JL, Juergens D, Bennett NR, et al. De novo design of protein structure and function with RFdiffusion. Nature. 2023;620(7976):1089-1100. doi:10.1038/s41586-023-06415-8 | 微信公众号 AI-Protein Design 中文详解"
  - Domain: ai-drug-discovery
  - Content: RFdiffusion 原文详解：RoseTTAFold微调为扩散模型，无条件单体生成/对称寡聚体/功能基序支架/靶标结合蛋白设计，局限性（无法建模小分子配体/辅因子，高极性靶标界面挑战）

### entities (enriched)
- entities/de-novo-protein-design.md — 更新 sources 添加 RFdiffusion raw article；补充 RFdiffusion 具体方法细节

### index.md: ai-drug-discovery 新增1条 raw article，总页数 98→99

## [2026-05-13] ingest | RFdiffusion Nature 2023 原始论文 (MinerU PDF full text)

### raw/papers
- raw/papers/watson2023-rfdiffusion-nature.md
  - MinerU full.md (569 lines, 114KB), copied from ~/Desktop/Ref/
  - YAML frontmatter: source_url/ingested/sha256/source_file/extracted_by/extraction_date/citation/domain
  - sha256 (body): b9fba9d08117471a452f09674576f99b15dffaffca6cfde44fb647365e21ac56
  - Citation: "Watson JL, Juergens D, Bennett NR, Trippe BL, Yim J, Eisenach HE, et al. De novo design of protein structure and function with RFdiffusion. Nature. 2023 Aug;620(7976):1089-1100. doi:10.1038/s41586-023-06415-8. PMID:37433327; PMCID:PMC10468238."
  - Domain: ai-drug-discovery
  - 18 mapped figures + 16 orphans archived

### entities (enriched)
- entities/de-novo-protein-design.md — sources 追加原始论文 raw/papers 引用

### images
- 18 figures renamed and copied to raw/papers/images/
- 16 orphans archived to _orphans/
- MAPPING.md updated with full hash→name cross-reference

### index.md: ai-drug-discovery 新增1条 raw/papers，总页数 99→100

## [2026-05-13] ingest | AI蛋白设计实验验证方法综述 — Curr Opin Struct Biol 2026（微信公众号·BioAIDesign）

### raw/articles
- raw/articles/AI蛋白设计实验验证方法综述-CurrOpinStructBiol2026.md
  - YAML frontmatter: source_url/ingested/sha256/citation/domain
  - sha256: c9857d027c3427c3ddcedd444f32be8a27dd7fd458bd9e93833193e28e300e83
  - Citation: "Closing the loop: Experimentally validated methods in artificial intelligence-driven protein design. Curr Opin Struct Biol. 2026. doi:10.1016/j.sbi.2026.103272 | 微信公众号 BioAIDesign (JoeYo) 中文解读"
  - Domain: ai-drug-discovery
  - Content: AI蛋白设计实验验证方法综述，从实验验证视角系统比较 binder/antibody/enzyme 三类任务的成功率，Table 1-4 + Appendix Tables 整理

### entities (enriched)
- entities/de-novo-protein-design.md — 新增「实验验证全景」节（Curr Opin Struct Biol 2026 补充），扩充相关工具表（binder/antibody/enzyme 三大类分组）

### index.md: ai-drug-discovery 新增1条 raw article，总页数 100→101

## [2026-05-13] ingest | Curr Opin Struct Biol 2026 原始论文 — AI蛋白设计实验验证方法综述 (MinerU)

### raw/papers
- raw/papers/kosonocky2026-ai-protein-design-cosb.md
  - MinerU full.md (310 lines, 67KB), copied from ~/Desktop/Ref/
  - YAML frontmatter: source_url/ingested/sha256/source_file/extracted_by/extraction_date/citation/domain
  - sha256 (body): 616c692994bac0b31977a4cd3df2424270efccc21cfe1fbc10145b5481b9c7b6
  - Citation: "Kosonocky CW, Alamdari S, Yang KK, Amini AP. Closing the loop: Experimentally validated methods in artificial intelligence-driven protein design. Curr Opin Struct Biol. 2026;98:103272. doi:10.1016/j.sbi.2026.103272."
  - Domain: ai-drug-discovery
  - Authors: Microsoft Research (Amini, Yang) + UT Austin — 非 Baker 体系独立视角
  - 1 figure (pipeline overview) + 7 orphan sub-figures archived

### entities (enriched)
- entities/de-novo-protein-design.md — 新增「原始论文增补」节（Microsoft Research 独立视角 + Table 1-4 方法覆盖详情）

### images
- 1 figure renamed and copied to raw/papers/images/
- 7 orphans archived to _orphans/
- MAPPING.md restored + appended with new paper entry

### index.md: ai-drug-discovery 新增1条 raw/papers，总页数 101→102

## [2026-05-13] batch-ingest | RFdiffusion 原理详解文章 × 3（微信公众号）

### raw/articles
1. raw/articles/RFdiffusion1-架构原理训练上手指南-AIinBio.md
   - sha256: 6e814a56697d65b23d2cebb0b48c35b5cbc1a1c7759e7444fd8332b74495d06a
   - Source: AI in Bio (Vigour123) — 源码结构/SE3扩散/条件化生成/训练配置实操
   - Domain: ai-drug-discovery
2. raw/articles/RFdiffusion-SE3扩散框架详解-Nature2023-Bits&Bases.md
   - sha256: dfd250081377cadd7e4dda8133da1f02c7f3723c82928abc1723fa21b8d43c2b
   - Source: Bits & Bases — SE(3) 扩散框架、RoseTTAFold→扩散微调、框架表示、自条件化
   - Domain: ai-drug-discovery
3. raw/articles/RFdiffusion五大设计任务详解-Nature2023-Bits&Bases.md
   - sha256: e6e2bccf829bb0ff3570962309c54ca18d29deda234ecd01bb056e7d8559f53b
   - Source: Bits & Bases — 五大任务（单体/对称寡聚体/功能基序/对称基序/蛋白结合物）实验验证解读
   - Domain: ai-drug-discovery

### entities (enriched)
- entities/de-novo-protein-design.md — sources 追加 3 条 raw articles

### index.md: ai-drug-discovery 新增 3 条 raw articles，总页数 102→105

## [2026-05-13] ingest | IsoDDE Technical Report — Isomorphic Labs Drug Design Engine (MinerU)

### raw/papers
- raw/papers/isomorphic2026-isodde.md
  - MinerU full.md (406 lines, 80KB), copied from ~/Desktop/Ref/
  - YAML frontmatter: source_url/ingested/sha256/source_file/extracted_by/extraction_date/citation/domain
  - sha256 (body): 68b51bce028d9666a59064ebef47fc359de9dae20536a640db4092ba4df7d05d
  - Citation: "Isomorphic Labs Team. Accurate Predictions of Novel Biomolecular Interactions with IsoDDE. Technical Report. 2026."
  - Domain: ai-drug-discovery
  - Authors: Isomorphic Labs (Google DeepMind spinoff, Demis Hassabis)
  - 19 referenced figures + 5 orphan sub-figures
  - Core: 蛋白-小分子 cofolding 2x AF3, 抗体 DockQ>0.8 39% vs AF3 17%, 亲和力预测超越物理方法

### entities
- Created: entities/isodde.md — IsoDDE 工具实体页，核心发现链+对比表（vs AF3 vs Boltz-2）+交叉引用 de-novo-protein-design / nextgen-vaccine-strategies

### images
- 19 figures renamed and copied to raw/papers/images/
- 5 orphans archived to _orphans/
- MAPPING.md appended with new paper entry

### index.md: entities 新增1条 + ai-drug-discovery 新增1条 raw/papers，总页数 105→107

## [2026-05-13] ingest | ProtDBench 蛋白 Binder 设计统一评测基准 — arXiv 2026（微信公众号·BioAIDesign）

### raw/articles
- raw/articles/ProtDBench-蛋白Binder设计统一评测基准-arXiv2026.md
  - sha256: d76b602c32840fbcf02ba78ce826537a838b69b302895df37e089e6da3439e06
  - Source: BioAIDesign (JoeYo)
  - Citation: "ProtDBench: A Unified Benchmark of Protein Binder Design and Evaluation. arXiv:2605.04118. 2026. | BioAIDesign 中文解读"
  - Domain: ai-drug-discovery
  - Content: 统一评测基准，verifier 偏差分析（AF2/Boltz/Chai/Protenix/ESMFold），throughput-aware 24h×1xA100 标准化比较

### concepts
- Created: concepts/protdbench.md — ProtDBench 概念页，核心发现链（verifier 偏差 + throughput-aware + 多样性）+ 方法覆盖表 + 交叉引用 de-novo-protein-design

### entities (enriched)
- entities/de-novo-protein-design.md — sources 追加 ProtDBench raw article

### index.md: concepts 新增1条 + ai-drug-discovery 新增1条 raw article，总页数 107→109

## [2026-05-13] ingest | ProtDBench arXiv 2026 原始论文 (MinerU)

### raw/papers
- raw/papers/liu2026-protdbench-arxiv.md
  - MinerU full.md (444 lines, 68KB), copied from ~/Desktop/Ref/
  - sha256 (body): a10fba67fa66b42ed13d180af8c35529857c815bb5546903b1580f2a5a4a4351
  - Citation: "Liu C, Ren M, Guan J, Gong C, Sun J, Chen X, Xiao W. ProtDBench: A Unified Benchmark of Protein Binder Design and Evaluation. arXiv:2605.04118. 2026."
  - Domain: ai-drug-discovery
  - 17 referenced figures + 19 orphan sub-figures
  - Core: verifier 偏差分析 (AF2/Boltz/Chai/Protenix/ESMFold), throughput-aware 24h×1xA100, 多样性分析

### concepts (enriched)
- concepts/protdbench.md — sources 追加原始论文引用

### images
- 17 figures renamed and copied to raw/papers/images/
- 19 orphans archived to _orphans/
- MAPPING.md appended

### index.md: ai-drug-discovery 新增1条 raw/papers，总页数 109→110

## [2026-05-14] ingest | Ovo bioRxiv 2025 原始论文 (MinerU)

### raw/papers
- Created: raw/papers/prihoda2025-ovo-biorxiv.md — MinerU full.md (336 lines, 94KB)
  - sha256: 0142daf837834631e728128b8a1f7b572f579eb75aea39e14d1957cd4adc0843
  - Citation: "Prihoda D, Ancona M, Calounova T, et al. Ovo, an open-source ecosystem for de novo protein design. bioRxiv 2025. doi:10.1101/2025.11.27.691041."
  - Domain: ai-drug-discovery
  - Authors: MSD Czech Republic + AWS (6 figures)

### entities
- Created: entities/ovo.md — Ovo 开源蛋白设计生态系统实体页，核心发现链+架构+三大设计工作流+ProteinQC+验证数据+交叉引用 (de-novo-protein-design/protdbench)

### images
- 5 referenced figures renamed: prihoda2025-ovo-fig1~5.jpg → raw/papers/images/prihoda2025-ovo-biorxiv/
- 1 orphan (fig6) archived
- MAPPING.md updated with per-paper summary row + hash mapping

### index.md
- Entities: added [[ovo]]
- Raw Sources (ai-drug-discovery): added raw/papers/prihoda2025-ovo-biorxiv
- 总页数: 115 → 117
- **Fixed** WGD concept page `domain: bioinfo-pipeline` → `cancer-biology` (was misclassified)
- **Added** `cancer-biology` domain to SCHEMA.md domain taxonomy
- **Fixed** entities/crocoDeEL dangling cross-refs: `[[metagenomics-qc-workflow]]` and `[[scrub]]` replaced with valid `[[gbt46943-2025-mngs-standard]]` + `[[kraken2-classification]]`
- **Fixed** index.md formatting: 4 lines had `||-` prefix (extra pipe) → corrected to `- ` consistent with other entries
- **Added** 5 raw articles missing from index.md:
  - `raw/articles/DeepSomatic-数据基准与算法创新-NatBiotechnol2025-深度解读.md` → bioinfo-pipeline
  - `raw/articles/Ovo-蛋白设计开源生态平台-bioRxiv2026.md` → ai-drug-discovery
  - `raw/articles/武汉大学-徐可教授-个人主页.md` → vaccine-design
  - `raw/articles/一篇文章看懂-生殖系变异vs体细胞突变.md` → bioinfo-pipeline
  - `raw/articles/机器学习与基因组学赋能精准肿瘤学-NatRevCancer2026.md` → clinical-evaluation
- **Updated** index.md page count: 110 → 115, domains: 7 → 8

## [2026-05-14] fix | Structured fixes batch 2

### Schema & classification
- **Removed** `resistance-detection` domain from SCHEMA.md (0 pages ever used it)
- **Created** `entities/protdbench.md` — ProtDBench 从 concept 升级为 entity（它是评测基准工具，非可复用概念）
- **Archived** `concepts/protdbench.md` → lightweight redirect to `[[entities/protdbench]]`

### Dangling links fixed (4)
- `entities/codebase-index.md`: `[[bioinfo-pipeline]]` → plain text (指向 domain，非页面)
- `concepts/scrnaseq-workflow.md`: `[[cellphonedb-cell-communication]]` → CellPhoneDB (planned page 未创建)
- `entities/deeprare.md`: `[[deepvariant-tool-overview]]` → `[[raw/articles/deepvariant-tool-overview]]` (raw 路径修正)
- `concepts/nextgen-vaccine-strategies.md`: `[[virus-vaccine-pipeline]]` → plain text (planned page 未创建)

### Index & counts
- 真实文件数：110 (find count)，index 从 117 修正为 110

### Lint tool
- Created: `~/code/scripts/wiki-lint.sh` — 一键校验 index 计数/悬空链接/domain 合法性/大页面/ frontmatter 完整性
- 当前运行结果：0 errors, 3 warnings (1 oversized page + 2 SCHEMA domains only used by raw/ pages)

## [2026-05-15] ingest | MsgaBpred B细胞表位预测 AlphaFold3-GCN-ESM-C
- Raw: raw/articles/MsgaBpred-B细胞表位预测-AlphaFold3-GCN-ESM-C.md — 整合AlphaFold3预测结构、多尺度GCN与ESM-C的B细胞表位预测模型，AUC统计显著提升（微信公众号·习也无止境）
- Frontmatter: source_url/ingested/sha256/citation/domain/tags
- Cross-refs: [[concepts/nextgen-vaccine-strategies]], [[entities/de-novo-protein-design]], [[raw/articles/RFdiffusion-蛋白质从头设计-Nature论文详解]]
- Updated: index.md — 新增 raw/articles 条目到 vaccine-design 域

## [2026-05-15] ingest | GLA-3M-052-LS+OVA 鼻喷广谱疫苗（Science 2026，生物谷微信公众号）
- Raw: raw/articles/万能呼吸道疫苗-斯坦福Science-鼻喷广谱防护.md — 斯坦福鼻喷通用疫苗，TLR4+TLR7/8双激动剂+OVA，整合器官免疫范式，3剂防护3个月覆盖病毒/细菌/过敏原
- Frontmatter: source_url (DOI)/ingested/sha256/citation/domain: vaccine-design
- Created: concepts/universal-intranasal-vaccine-gla-3m052.md — 整合器官免疫概念页，机制/防护谱/免疫特征/临床前景
- Updated: concepts/nextgen-vaccine-strategies.md — 交叉引用 [[universal-intranasal-vaccine-gla-3m052]]
- Updated: index.md — raw/articles + concepts 条目，page count: 110 → 113

## [2026-05-15] ingest | Protenix-v2 ByteDance Seed 结构预测+抗体设计
- Raw: raw/articles/Protenix-v2-抗体设计-结构预测-药物发现.md — 微信公众号·BioAIDesign (JoeYo)，Protenix-v2 技术解读
- Frontmatter: source_url (GitHub)/ingested/sha256/citation/domain: ai-drug-discovery/tags
- Created: entities/protenix-v2.md — 统一结构预测+zero-shot 抗体设计实体页，含与 IsoDDE/AF3/Boltz-1 对比表，交叉引用 [[isodde]] [[de-novo-protein-design]] [[protdbench]]
- Updated: index.md — raw/articles + entities 条目，page count: 113 → 114

## [2026-05-16] ingest | DIAMOND DeepClust 蛋白质宇宙超快速聚类 + GPS AI药物设计平台 + 培训课程
- Raw: raw/articles/DIAMOND-DeepClust-蛋白质宇宙聚类-NatureMethods-另一视角.md — 小包子学演化视角解读 DIAMOND DeepClust，190亿序列→17亿簇，双向覆盖优势（微信公众号·小包子学演化，Nat Methods 2026）
- Raw: raw/articles/DIAMOND-DeepClust-蛋白质宇宙超快速聚类-NatureMethods2026.md — 吕信院全解读：27节点18天190亿序列，NR比MMseqs2快36x，AF2 pLDDT↑7.73，1.18亿新家族（微信公众号·吕信院，Nat Methods 2026）
- Raw: raw/articles/GPS-AI药物设计平台逆转疾病转录表型-Cell.md — GPS平台从化学结构预测转录效应，Z-RGES评分逆转疾病表型，肝癌/MSU45302/UHRF1/肺纤维化/吡噻酮（微信公众号·生信星图，Cell 2026）
- Raw: raw/articles/AI蛋白质设计AI抗体设计培训课程-卓昂科研.md — 卓昂科研AI蛋白质/抗体/基因编辑/虚拟细胞/AIDD等7大课程介绍（微信公众号·卓昂科研咨询）
- Frontmatter: source_url/ingested/sha256/citation 已添加至全部4篇raw文件
- Created: entities/diamond-deepclust.md — DIAMOND DeepClust 行星级蛋白质聚类工具实体页，190亿序列→5.44亿非单例簇（BFD 5.5x），与MMseqs2/FLSHclust对比表，交叉引用 [[mmseqs2]] [[de-novo-protein-design]] [[protdbench]]
- Created: entities/gps-ai-drug-platform.md — GPS AI药物设计平台实体页，化学结构→转录组效应预测，Z-RGES评分/MolSearch/SGAR，表型驱动 vs IsoDDE靶点驱动对比表，交叉引用 [[isodde]] [[de-novo-protein-design]] [[protenix-v2]]
- Updated: entities/mmseqs2.md — 交叉引用 [[diamond-deepclust]]
- Updated: entities/de-novo-protein-design.md — 交叉引用 [[diamond-deepclust]] [[gps-ai-drug-platform]]
- Updated: entities/crocodeel.md — 交叉引用 [[diamond-deepclust]]
- Updated: entities/protdbench.md — 交叉引用 [[diamond-deepclust]]
- Updated: entities/isodde.md — 交叉引用 [[gps-ai-drug-platform]]
- Updated: entities/protenix-v2.md — 交叉引用 [[gps-ai-drug-platform]]
- Updated: index.md — entities + raw/articles 共6条新条目，page count: 114 → 121

## [2026-05-16] ingest | 计算驱动靶向肽药物设计：从物理建模到生成式AI与临床转化
- Raw: raw/articles/计算驱动靶向肽药物设计-物理建模到生成式AI-InnovDrugDiscov2026.md — 靶向肽药物设计综述，生成→逆折叠→预测→ADMET→物理验证闭环框架，PepMLM/EvoDiff/HighFold/PepFlow/LigandMPNN工具全景，Research Gaps（ncAA/环肽/多目标PK等）（微信公众号·陷入鞍点，Hu W, Innov Drug Discov 2026）
- Frontmatter: source_url/ingested/sha256/citation 已添加
- Updated: entities/de-novo-protein-design.md — 交叉引用 [[raw/articles/计算驱动靶向肽药物设计-物理建模到生成式AI-InnovDrugDiscov2026]]
- Updated: index.md — raw/articles 条目到 ai-drug-discovery 域，page count: 121 → 122

## [2026-05-16] ingest | AlphaFold 3 深度解读：扩散模型与全生物分子时代
- Raw: raw/articles/AlphaFold3深度解读-扩散模型与全生物分子时代.md — AF3 全生物分子统一框架解读：扩散模型取代结构模块，Pairformer×48，PoseBusters 76.4%，覆盖蛋白/核酸/配体/离子（微信公众号·HZAU蛋白质科学，Nature 2024）
- Frontmatter: source_url/ingested/sha256/citation 已添加
- Created: entities/alphafold3.md — AF3 全生物分子统一结构预测实体页，含与 IsoDDE/AF2/Protenix-v2 对比表，交叉引用 [[isodde]] [[de-novo-protein-design]] [[protenix-v2]] [[protdbench]] [[ovo]]
- Updated: entities/isodde.md — 交叉引用 [[alphafold3]]
- Updated: entities/protenix-v2.md — 交叉引用 [[alphafold3]]
- Updated: entities/protdbench.md — 交叉引用 [[alphafold3]]
- Updated: index.md — entities + raw/articles 共2条新条目，page count: 122 → 125

## [2026-05-16] ingest | 癌症筛查与多癌早检：cfDNA片段组学MCED + 五大癌种筛查指南
- Raw: raw/articles/南京医科大学-NatureMedicine-多癌早检-cfDNA片段组学.md — 南京医科大学 Nat Med MCED：cfDNA片段组学13癌种，I期79.3%/特异性98.9%，TOO 82.3%，金陵队列3,724人（微信公众号·BioMed科技，Shao Y, Nat Med 2026）
- Raw: raw/articles/NatureReviews-癌症筛查指南-五大癌种-AI-MCED.md — Nat Rev Clin Oncol 筛查指南：五大癌种+AI辅助+MCED未来技术（微信公众号·基因研究所，Duffy SW, Nat Rev Clin Oncol 2026）
- Frontmatter: source_url/ingested/sha256/citation 已添加至2篇raw文件
- Created: concepts/cfdna-fragmentomics-mced.md — cfDNA 片段组学 MCED 概念页，含南京医科大/GRAIL Galleri对比，四大信号维度（片段组学/甲基化/CNV/突变），金陵队列数据，交叉引用 [[deeprare]] [[methylvi]] [[cnvpipe]]
- Created: concepts/five-cancer-screening-guide.md — 五大癌种筛查指南概念页（乳腺/结直肠/宫颈/前列腺/肺癌），AI辅助+MCED补充，交叉引用 [[cfdna-fragmentomics-mced]]
- Updated: entities/methylvi.md — 交叉引用 [[cfdna-fragmentomics-mced]] [[five-cancer-screening-guide]]
- Updated: entities/deeprare.md — 交叉引用 [[cfdna-fragmentomics-mced]] [[five-cancer-screening-guide]]
- Updated: entities/cnvpipe.md — 交叉引用 [[cfdna-fragmentomics-mced]]
- Updated: index.md — concepts + raw/articles 共4条新条目，新增 cancer-biology raw源域，page count: 125 → 129

## [2026-05-16] cleanup | 删除2篇营销课程推广文章
- Deleted: raw/articles/AI蛋白质设计AI抗体设计培训课程-卓昂科研.md — 卓昂科研课程广告（6880元/人，纯营销文案）
- Deleted: raw/articles/NANO学术-AI生物培训课程-10大课程.md — NANO学术10大课程广告（纯营销文案）
- Updated: index.md — 移除 ai-drug-discovery 域下课程推广条目，page count: 130 → 128

## [2026-05-16] ingest | DeerFlow AI Agent 框架 + 科医诺广谱流感疫苗合作
- Raw: raw/articles/DeerFlow-AI-Agent框架代码结构与业务流转指南.md — DeerFlow AI Agent 框架技术文档：LangGraph+Next.js+FastAPI 架构，4进程拓扑，14中间件链，Sandbox/MCP/记忆/Skills（万智创界微信公众号，walton）
- Raw: raw/articles/科医诺生物-携手步长制药-共拓广谱流感疫苗.md — KNY0003 广谱流感疫苗管线：徐可团队原研→科医诺→步长制药，$98.5B疫苗市场产业转化（科医诺生物微信公众号）
- Frontmatter: source_url/ingested/sha256/citation/domain 已添加至2篇raw文件
- Updated: raw/articles/武汉大学-徐可教授-个人主页.md — 添加产业转化信息：KNY0003 管线合作
- Updated: concepts/nextgen-vaccine-strategies.md — 添加产业转化案例：KNY0003 管线
- Updated: index.md — raw/articles 共2条新条目（bioinfo-pipeline + vaccine-design），page count: 129 → 131

## [2026-05-16] ingest | Ca-DelMut 新冠S蛋白缺失减毒株
- Raw: raw/articles/Ca-DelMut新冠S蛋白缺失减毒株-NatureComms.md — Ca-DelMut 安全减毒株：S1/S2弗林切割位点缺失→低致病+强复制，鼻喷减毒活疫苗候选，灭菌式免疫（生信养成系微信公众号，Nat Commun 2021）
- Frontmatter: source_url/ingested/sha256/citation/domain 已添加
- Updated: concepts/nextgen-vaccine-strategies.md — 添加产业转化案例：Ca-DelMut 减毒活疫苗策略
- Updated: index.md — raw/articles 条目到 vaccine-design 域，page count: 129 → 130

## [2026-05-16] ingest | Isomorphic Labs B轮$21B AI制药最大融资 + 新增market-watch域
- Schema: 新增 `market-watch` 域至 SCHEMA.md（行业融资/并购/合作/政策动态）
- Raw: raw/articles/Isomorphic-Labs-21亿美元-B轮-AI制药最大融资.md — Isomorphic Labs B轮21亿美元融资，Thrive Capital领投，创AI制药单轮最高纪录（写意宣发微信公众号，2026/05）
- Frontmatter: source_url/ingested/sha256/citation/domain 已添加
- Updated: entities/isodde.md — 交叉引用融资新闻
- Updated: index.md — raw/articles 条目到新 market-watch 域，page count: 130 → 131，domains: 9 → 10

## [2026-05-16] ingest | Recursive Superintelligence $6.5亿融资 - AI4S狂潮
- Raw: raw/articles/Recursive-Superintelligence-AI4S-6.5亿美元-0产品估值46亿.md — 成立4个月0产品估值$46.5亿，Richard Socher/Caiming Xiong/田渊栋等8位创始团队，AI4S赛道融资对比表（智药局微信公众号，2026/05）
- Frontmatter: source_url/ingested/sha256/citation/domain 已添加（market-watch）
- Updated: index.md — raw/articles 条目到 market-watch 域，page count: 131 → 132

## [2026-05-16] ingest | AI4S赛道分析 + 奥明星程A轮 + 复鞍智能种子轮
- Raw: raw/articles/AI4S-四种生意三种赚法一条铁律-钛资本.md — AI4S赛道框架：感知行合四种生意，License-out/SaaS/IDM三种变现，DBTL飞轮（钛资本研究院·蒋云川，2026）
- Raw: raw/articles/奥明星程-AI4S-超亿元A轮-多组学多模态.md — 奥明星程超亿元A轮，哈佛博士团队，AI+多组学多模态，乳腺癌早筛95%灵敏度（动脉网，2026）
- Raw: raw/articles/复鞍智能-AI4S物质科学-复旦科创种子轮.md — 复鞍智能种子轮，复旦刘智攀团队催化材料AI（建研院，2026）
- Frontmatter: source_url/ingested/sha256/citation/domain 已添加至3篇raw文件（market-watch）
- Created: concepts/ai4s-business-framework.md — AI4S赛道分析框架概念页，感知行合/DBTL飞轮/甜蜜点迁移
- Updated: index.md — concepts + raw/articles 共4条新条目到 market-watch 域，page count: 132 → 137

## [2026-05-16] ingest | RFdiffusion 冷思考：潜力与局限
- Raw: raw/articles/RFdiffusion-爆款论文冷思考-潜力与局限.md — RFdiffusion 冷思考：对称寡聚体~14%/结合蛋白~19%成功率，AF2同源性偏差，骨架-序列解耦假设脆弱，训练数据边界（HZAU蛋白质科学微信公众号，Nature 2023原文解读）
- Frontmatter: source_url/ingested/sha256/citation 已添加（ai-drug-discovery）
- Updated: entities/de-novo-protein-design.md — 新增「警告与局限」章节，引用5条核心批评
- Updated: index.md — raw/articles 条目到 ai-drug-discovery 域，page count: 136 → 137

## [2026-05-16] ingest | AI筛药四工具综述：DeepClust/Boltz-2/SoftMol/AINN-P1
- Raw: raw/articles/AI筛药新工具-Boltz2-SoftMol-AINN-P1-DeepClust.md — AI筛药四工具综述：DeepClust 36x加速/Boltz-2共折叠ROC-AUC 0.70/SoftMol块扩散100%有效性6.6x/AINN-P1 mLSTM稳定性预测（榴莲忘返AIDD，2026）
- Frontmatter: source_url/ingested/sha256/citation 已添加（ai-drug-discovery）
- Created: entities/boltz2.md — Boltz-2 共折叠虚拟筛选实体页，含与AF3/IsoDDE对比
- Created: entities/softmol.md — SoftMol 块扩散分子生成实体页
- Updated: entities/diamond-deepclust.md — 交叉引用综述文章
- Updated: entities/protdbench.md — 交叉引用 [[boltz2]]
- Updated: index.md — entities ×2 + raw ×1 共3条新条目，page count: 137 → 141

## [2026-05-18] ingest | DualGPT-AB 双阶段生成式抗体设计（Nature Computational Science 2026）
- Raw: raw/articles/DualGPT-AB双阶段生成式抗体设计-NatureComputationalScience.md — DualGPT-AB 双阶段 GPT+RL 抗体设计论文解读（微信公众号·青禾矩点）
- Frontmatter: source_url/ingested/sha256/citation/domain added
- Created: entities/dual-gpt-ab.md — DualGPT-AB 抗体设计框架实体页，核心发现链+关键证据摘要+多性质达标率78.2%/湿实验验证CDRH3_39/工具对比表（vs Protenix-v2/IsoDDE）/局限与未公开
- Updated: entities/protenix-v2.md — 交叉引用 [[dual-gpt-ab]]
- Updated: entities/de-novo-protein-design.md — 交叉引用 [[dual-gpt-ab]]（抗体设计工具节）
- Updated: entities/isodde.md — 交叉引用 [[dual-gpt-ab]]
- Updated: index.md — entity + raw article 条目，page count: 140 → 143

## [2026-05-18] ingest | TCR-PRP 多肽识别谱语言模型 T 细胞功能预测（Nature Biotechnology 2026）
- Raw: raw/articles/TCR-PRP-多肽识别谱-语言模型-T细胞功能预测-NatBiotechnol2026.md — TCR-PRP 多肽识别谱与 T 细胞功能预测，pLM 超越 AF3（微信公众号·游离的DNA）
- Frontmatter: source_url/ingested/sha256/citation/domain added
- Created: entities/tcr-prp.md — TCR-PRP 多肽识别谱系统实体页，核心发现链+关键证据摘要（PRP构建/序列vs功能聚类/pLM vs AF3/自身抗原发现/不确定性量化）+工具对比+交叉引用
- Updated: entities/isodde.md — 交叉引用 [[tcr-prp]]
- Updated: concepts/nextgen-vaccine-strategies.md — 交叉引用 [[tcr-prp]]
- Updated: entities/alphafold3.md — 交叉引用 [[tcr-prp]]（PRP-pLM 超越 AF3 的功能预测对比）
- Updated: index.md — entity + raw article 条目，page count: 143 → 145

## [2026-05-18] update | 计算驱动靶向肽药物设计综述 — 替换为完整版（206行）
- Raw: raw/articles/计算驱动靶向肽药物设计-物理建模到生成式AI-InnovDrugDiscov2026.md — 157行→206行，新增场景工具指南/Research Gaps/临床案例拆解/必读文献精选
- Frontmatter: sha256 更新（d08c2e8a → c3aaa7ef），ingested 更新为 2026-05-18，citation 补全
- Deleted: raw/articles/计算驱动的靶向肽药物设计-从物理建模到生成式AI-临床转化-InnovDrugDiscov2026.md — 同一文章的长文件名副本（篇幅冗余）
- Index: 无需修改（条目已存在且描述已覆盖）
- page count: 不变（145）

## [2026-05-18] ingest | RxnBench 化学反应理解 MLLM 评测基准（深势科技/上海交大等, JCIM 2026）
- Raw: raw/articles/RxnBench-41款MLLM评测-深势科技-JCIM.md — RxnBench 双层评测基准（SF-QA/FD-QA），41 款 MLLM 评测，最佳 FD-QA <50%（微信公众号·AI蛋白质）
- Frontmatter: source_url/ingested/sha256/citation/domain added
- Created: entities/rxnbench.md — RxnBench 评测基准实体页，核心发现链+双层架构+41模型性能概要+与 ChemBench/ScienceQA 对比表+局限
- Updated: entities/protdbench.md — 交叉引用 [[rxnbench]]
- Updated: index.md — entity + raw article 条目，page count: 144 → 146

## [2026-05-18] ingest | GEMs 从头设计GPCR跨膜结构域Binder调节剂（浙江大学, Nature 2025）
- Raw: raw/articles/信手随翻209-Nature-从头设计GPCR结构Binder调节剂.md — GEMs 从头设计 GPCR 跨膜区变构调节蛋白，RFdiffusion+MPNN+AF2+结构提示词（微信公众号·信手随翻）
- Frontmatter: source_url/ingested/sha256/citation/domain added
- Created: entities/gem-gpcr-modulator.md — GEMs 实体页，核心发现链+关键技术管线（高通量盲扫/迭代重折叠/结构提示词）+4种GEM功能表+Cryo-EM验证+逻辑门模块化+vs Protenix-v2对比+交叉引用
- Updated: entities/protenix-v2.md — 交叉引用 [[gem-gpcr-modulator]]
- Updated: entities/de-novo-protein-design.md — 新增「跨膜蛋白与GPCR设计」节，交叉引用 [[gem-gpcr-modulator]]
- Updated: index.md — entity + raw article 条目，page count: 146 → 148

## [2026-05-18] ingest | HighFold 环肽结构预测三部曲迭代解读
- Raw: raw/articles/HighFold-环肽结构预测三部曲迭代.md — HighFold/HighFold2/HighFold3 环肽结构预测工具迭代，CycPOEM→ncAA→Cyclization 开关（微信公众号·AI药物设计实验室）
- Frontmatter: source_url/ingested/sha256/citation/domain added
- Created: entities/highfold.md — HighFold 系列工具实体页，三代对比表+CycPOEM原理+ncAA覆盖扩展+AF3底座改动+交叉引用
- Updated: entities/alphafold3.md — 交叉引用 [[highfold]]
- Updated: entities/de-novo-protein-design.md — 交叉引用 [[highfold]]（结合物与抗体设计节）
- Updated: index.md — entity + raw article 条目，page count: 149 → 151

## [2026-05-18] batch-ingest | AI造药新范式五工具综述（榴莲忘返AIDD）
- Raw: raw/articles/AI造药新范式-口袋生长分子-BOAT-ProteomeScan-SeFMol-TarPass.md — 五工具综述：BOAT/ProteomeScan/SeFMol/FlyPredictome/TarPass（微信公众号·榴莲忘返AIDD）
- Frontmatter: source_url/ingested/sha256/citation/domain added
- Created: entities/boat.md — BOAT 抗体多目标贝叶斯优化，高斯过程+遗传算法（AstraZeneca）
- Created: entities/sefmoll.md — SeFMol RL引导半柔性扩散分子生成（Sci Adv 2025）
- Created: entities/tarpass.md — TarPass 靶点感知分子生成评测基准，15模型不如随机（Adv Sci 2025）
- Updated: entities/softmol.md — 交叉引用 [[sefmoll]]
- Updated: index.md — 3 entities + 1 raw article，page count: 151 → 155

## [2026-05-19] ingest | PPLM 蛋白配对语言模型 — PPI/亲和力/接触预测（Liu J, Nat Commun 2026）
- Raw: raw/articles/PPLM-蛋白配对语言模型-PPI预测-NatureComms2026.md — PPLM 蛋白配对语言模型：hybrid intra-/inter-protein attention，PPI/亲和力/接触三项任务超越基线（微信公众号·AI药物设计实验室）
- Frontmatter: source_url/ingested/sha256/citation/domain added
- Created: entities/pplm.md — PPLM 实体页，核心发现链+关键证据（困惑度/跨物种PPI/亲和力/接触精度）+对比表+交叉引用+局限
- Updated: entities/alphafold3.md — 交叉引用 [[pplm]]（PPLM-Contact2 融合 AF 链间距离图）
- Updated: entities/de-novo-protein-design.md — 交叉引用 [[pplm]]（PPI 评估底座）
- Updated: entities/isodde.md — 交叉引用 [[pplm]]（应用方向互补）
- Updated: entities/boltz2.md — 交叉引用 [[pplm]]（序列PPI vs 共折叠互补）
- Updated: entities/protenix-v2.md — 交叉引用 [[pplm]]（序列PPI vs 结构预测互补）
- Updated: entities/tcr-prp.md — 交叉引用 [[pplm]]（共享 pLM 基础）
- Updated: index.md — entity + raw article 条目，page count: 155 → 157

## [2026-05-19] ingest | scMethCraft 单细胞DNA甲基化统一分析框架（Nat Commun 2026 南开陈盛泉）
- Raw: raw/articles/scMethCraft-单细胞DNA甲基化统一分析框架-NatCommun2026.md — scMethCraft 混合神经网络+KAN+迭代相似性加权，scDNAm NA原生处理，全流程覆盖（微信公众号·生信钱同学）
- Frontmatter: source_url/ingested/sha256/citation/domain added; stray `---` separator removed
- Created: entities/scmethcraft.md — scMethCraft 实体页，核心发现链+关键证据+对比表（vs MethylVI/传统方法）+5条交叉引用+局限
- Updated: entities/methylvi.md — 交叉引用 [[scmethcraft]]（技术路线互补）
- Updated: entities/scmbench.md — 交叉引用 [[scmethcraft]]（未来 benchmark 纳入）
- Updated: index.md — entity + raw article 条目，page count: 157 → 160

## [2026-05-19] ingest (sequential) | SeFMol 半柔性分子扩散原始论文深度解读（Sci Adv 2026）
- Raw: raw/articles/SeFMol-强化学习引导半柔性分子扩散模型-SciAdv2026.md — SeFMol 三阶段架构（预训练+微调+RL优化），快速采样20x，Vina -7.23，成功率11.53%（微信公众号·AI蛋白质）
- Frontmatter: source_url/ingested/sha256/citation/domain added; stray `---` separator removed
- Enriched: entities/sefmoll.md — sources 追加新raw；新增三阶段架构详解、与8基线对比表、fast sampling细节、CDK2/ROCK1验证；交叉引用扩充 tarpass/boat
- Updated: entities/boat.md — 交叉引用 [[sefmoll]]
- Updated: entities/tarpass.md — 交叉引用 [[sefmoll]]
- Updated: index.md — raw article 条目追加，page count: 160

## [2026-05-19] create | concepts/b-cell-epitope-prediction
- Created: concepts/b-cell-epitope-prediction.md — B 细胞表位预测概念页，线性/构象表位分类、MsgaBpred SOTA 方法（AlphaFold3+GCN+ESM-C）、与疫苗设计流程的连接；交叉引用 [[nextgen-vaccine-strategies]] [[tcr-prp]] [[de-novo-protein-design]]
- Updated: index.md — concepts 条目追加，page count: 160 → 161

## [2026-05-19] enrich | concepts/b-cell-epitope-prediction + DFPred
- Updated: concepts/b-cell-epitope-prediction.md — 补充 DFPred 路径（序列基/线性表位）：CNN+单头注意力+xLSTM 架构、ESM-2 特征分析、AUC=0.806/AUC10%=0.420 与 8 种方法对比、消融实验（CNN+注意力为最关键组件）、xLSTM 选型理由（指数门控+矩阵记忆+并行化）；新增 sources 和交叉引用
- Updated: index.md — concepts 条目更新描述；vaccine-design raw 区追加 DFPred 条目，page count: 161

## [2026-05-20] cleanup | 跳过入库：机器学习十大算法图解
- 来源：一站式AI产品经理知识库（WeChat 公众号图集）
- 判定：Marketing / low-value — 基础算法1句话科普，零技术深度，目标读者为AI产品经理
- 操作：从 raw/articles/ 删除

## [2026-05-20] ingest | 随机森林算法全面讲解（微信公众号·机器学习和人工智能AI）
- Schema: 新增 `machine-learning` domain — 机器学习基础算法与模型
- Raw: raw/articles/随机森林算法全面讲解-机器学习和人工智能AI.md — Random Forest 全面教程：Bagging+随机特征选择、Gini/MSE、复杂度分析、sklearn 案例、GridSearchCV
- Frontmatter: source_url/ingested/sha256/citation/domain added; stray `---` separator removed
- Created: concepts/random-forest.md — Random Forest 概念页：双重随机性、优缺点、复杂度、生物信息学应用（特征筛选/微生物分类/污染检测/癌症早筛）、与Boosting对比
- Updated: entities/crocodeel.md — 交叉引用 [[random-forest]]
- Updated: index.md — header(161→164, 10→11 domains), Concepts 条目, Raw ### machine-learning 域追加

## [2026-05-20] ingest | SVM + Transformer 算法文章入库
- Raw: raw/articles/SVM支持向量机算法详解-机器学习和人工智能AI.md — SVM 概念+线性核+sklearn iris案例
- Raw: raw/articles/Transformer结合LSTM时序预测详解-机器学习和人工智能AI.md — Transformer+LSTM 时序融合：自注意力/多头注意力/位置编码/LSTM门控/PyTorch完整代码
- Frontmatter: 两篇都添加 YAML frontmatter + 清理 stray `---`
- Created: concepts/svm.md — SVM 概念页：最大间隔/核技巧/C参数/与RF对比/生信应用
- Created: concepts/transformer.md — Transformer 概念页：自注意力/多头注意力/位置编码/Transformer+LSTM融合/生信应用
- Updated: entities/cnvpipe.md — 交叉引用 [[svm]]
- Updated: concepts/random-forest.md — 交叉引用 [[svm]]
- Updated: index.md — header(164→168), Concepts 新增 svm/transformer, Raw machine-learning 域追加2条

## [2026-05-21] ingest | 随机森林+贝叶斯优化（深夜努力写Python）
- Raw: raw/articles/随机森林+贝叶斯优化-深夜努力写Python.md — 随机森林+贝叶斯优化超参数自动调优：RF+BO原理、EI采集函数、sklearn完整代码案例、过程可视化
- Frontmatter: 添加 YAML frontmatter（source_url/ingested/sha256/citation）
- Created: concepts/bayesian-optimization.md — BO概念页：GP/RF代理模型、EI/PI/UCB采集函数、RF+BO迭代逻辑、与Grid/Random Search对比、交叉引用RF/SVM/BOAT/CroCoDeEL
- Updated: concepts/random-forest.md — 新增「与贝叶斯优化的结合」章节，交叉引用 [[bayesian-optimization]]
- Updated: index.md — header(169), Concepts 新增 bayesian-optimization，Raw machine-learning 域追加1条

## [2026-05-21] ingest | Graphene E-Nose 机器学习分类（ZJU ACS Nano 2025）
- Raw: raw/articles/Graphene-E-Nose-Respiratory-Infection-ZJU-ACS.md — 石墨烯电子鼻区分细菌/病毒呼吸道感染，SVM/RF/Lasso 加权融合模型，145 例临床呼气样本 + 43 例外部验证
- Frontmatter: 添加 YAML frontmatter（source_url/ingested/sha256/citation）
- Created: entities/e-nose-respiratory-infection-zju-acs.md — ML-focused 实体页：数据集/特征工程/四模型对比/加权融合性能/PCA-LDA-OPLS-DA 辅助分析/应用价值
- Updated: concepts/svm.md — 应用场景追加「呼气诊断分类」，交叉引用 e-nose
- Updated: concepts/random-forest.md — 应用场景追加「呼气诊断分类」，交叉引用 e-nose
- Updated: index.md — header(170), Entities 新增 e-nose-respiratory-infection-zju-acs，Raw machine-learning 域追加1条

## [2026-05-21] ingest | SeqDance/ESMDance 蛋白质动力学语言模型（PNAS 2026）
- Raw: raw/articles/SeqDance-ESMDance-Protein-Dynamics-PNAS-2026.md — SeqDance/ESMDance 基于 MD 动力学预训练的 pLM，Transformer 编码残基共运动/全局构象，零样本预测设计蛋白/病毒蛋白突变效应
- Frontmatter: 添加 YAML frontmatter（source_url/ingested/sha256/citation）
- Created: entities/seqdance-esmdance.md — ML-focused 实体页：SeqDance（从头训练 Transformer + MD 动力学标签）/ESMDance（ESM2 + 动力学微调）架构、预训练性能表（RMSF/SASA/Rg）、突变效应零样本预测对比表（Spearman r）、注意力可解释性
- Updated: index.md — header(171), Entities 新增 seqdance-esmdance，Raw ai-drug-discovery 域追加1条
- Paper: raw/papers/hou2026-seqdance-esmdance-pnas.md — 原始论文 MinerU 全文 + PDF 拷贝至 wiki/raw/papers/（source_file 指向 Ref 目录原始 PDF）
- Updated: entities/seqdance-esmdance.md — sources 追加 raw/papers/ 引用

## [2026-05-21] ingest | CLIPP-SET/MCLM 对比 3D 蛋白-配体学习（Acellera/UPF arXiv 2026）
- Raw: raw/articles/DrugCLIP-MCLM-对比学习-AIDD-arXiv2026.md — CLIPP-SET 对比 3D 编码器（SET SE(3)等变transformer + CF-InfoNCE）+ MCLM 多模态化学语言模型（Llama2 decoder + dataset token），AIDD 对比学习从检索到生成（陷入鞍点微信公众号）
- Frontmatter: 添加 YAML frontmatter（source_url/ingested/sha256/citation/domain/tags）
- Created: entities/drugclip-mclm-contrastive-aidd.md — CLIPP-SET/MCLM 实体页：双子系统架构（SET 编码器 vs Llama2 decoder）、LIT-PCBA 检索性能表、3 种检索策略对比（scaffold hopping vs Morgan FP）、评估循环风险标注
- Updated: index.md — header(172), Entities 新增 drugclip-mclm-contrastive-aidd，Raw ai-drug-discovery 域追加1条

## [2026-05-21] ingest | S_pan 广谱新冠疫苗 — 基于 S 蛋白进化轨迹的抗原设计（Zhao Y/Xu K, Sci Transl Med 2023）
- Raw: raw/papers/xu2023-span-scitranslmed.md — S_pan 广谱新冠疫苗原始论文，54株假病毒系统分析→11,650,487条序列进化轨迹→2675条S蛋白系统发育共识抗原设计→小鼠活病毒攻毒验证（WT/Beta/Delta/Omicron）
- Copied from: ~/Desktop/Ref/2023-徐可-Span-scitranslmed.abo3332.pdf + MinerU.md
- Frontmatter: source_url/ingested/sha256/source_file/extracted_by/extraction_date/citation/domain/tags
- Created: concepts/span-vaccine.md — S_pan 概念页：S蛋白进化规律（互斥路径）、抗原设计策略（6突变：D614G+del69-70+del144+N501Y+P681H+E484K）、验证数据表（假病毒GMT+活病毒攻毒保护率）、与S_wt对比
- Updated: concepts/nextgen-vaccine-strategies.md — 交叉引用添加 [[span-vaccine]]
- Updated: concepts/universal-intranasal-vaccine-gla-3m052.md — 相关概念添加 [[span-vaccine]]
- Updated: index.md — Concepts +1（span-vaccine），vaccine-design raw +1（xu2023-span-scitranslmed），page count 172→174

## [2026-05-21] ingest | 跟着小白学机器学习 首批3篇入库
- Raw: raw/articles/跟着小白学机器学习-决策树.md — 决策树算法：信息增益/基尼不纯度/MSE划分准则、过拟合与剪枝、分类&回归sklearn代码
- Raw: raw/articles/跟着小白学机器学习-逻辑回归.md — 逻辑回归：Sigmoid/交叉熵损失/L1&L2正则化/ROC-AUC/OvR与Softmax/信用风险评估代码
- Raw: raw/articles/跟着小白学机器学习-朴素贝叶斯.md — 朴素贝叶斯：贝叶斯定理/条件独立性/多项式NB&高斯NB/零概率平滑/文本分类sklearn代码
- Frontmatter: 全部添加 YAML frontmatter（source_url/ingested/sha256/citation/domain/tags）
- Updated: index.md — machine-learning raw +3，page count 179→182

## [2026-05-21] ingest | 跟着小白学机器学习 第二批（线性回归 + K均值聚类）
- Raw: raw/articles/跟着小白学机器学习-线性回归.md — 线性回归：OLS/岭回归&Lasso/R平方/多重共线性/sklearn代码
- Raw: raw/articles/跟着小白学机器学习-K均值聚类.md — K均值聚类：目标函数/算法步骤/肘部法/轮廓系数/K值选择/sklearn代码
- Frontmatter: 添加 YAML frontmatter
- Updated: index.md — machine-learning raw +2，page count 182→184

## [2026-05-21] ingest | 跟着小白学机器学习 第二批补入（支持向量机）
- Raw: raw/articles/跟着小白学机器学习-支持向量机.md — SVM：软硬间隔/核函数/GridSearchCV/OvO&OvR多分类/SVR回归/sklearn代码
- Frontmatter: 添加 YAML frontmatter
- Updated: index.md — machine-learning raw +1，page count 184→185

## [2026-05-22] ingest | ORI 腾讯AI4S 蛋白设计闭环 — PDA+PGM+USM+RLWF（He B, Nat Commun 2026）
- Raw: raw/articles/ORI-腾讯AI4S蛋白设计闭环-NatureComm2026.md — 腾讯AI4S ORI本体强化迭代蛋白设计闭环，三组件（PDA+PGM+USM）+ RLWF湿实验反馈，溶菌酶/热稳定几丁质酶/双功能酶验证，TX-RL15活性超天然溶菌酶两个数量级（微信公众号·AI药物设计实验室）
- Frontmatter: source_url/ingested/sha256/citation/domain/tags added; stray `---` removed, marketing CTA trimmed
- Created: entities/ori-protein-design.md — ORI实体页：框架架构（PDA→PGM→USM→RLWF）、PGM 3B参数/1.66亿序列、USM 100M 2939 GFLOPS超ESM2 100x、三酶实验结果表、局限性、交叉引用
- Updated: entities/de-novo-protein-design.md — 相关工具·平台/生态系统添加 [[entities/ori-protein-design]]
- Updated: entities/ovo.md — 相关概念添加 [[entities/ori-protein-design]] + raw 交叉引用
- Updated: entities/seqdance-esmdance.md — 交叉引用添加 [[entities/ori-protein-design]]
- Updated: index.md — Entities +1 (ori-protein-design), ai-drug-discovery raw +1, page count 185→187

## [2026-05-26] ingest | 刘如谦组 Nat Biotechnol 2026：定向进化越改越脆，AI蛋白设计（ProteinMPNN）救场

### raw/articles
- raw/articles/NatBiotech-定向进化越改越脆-AI蛋白设计救场.md
  - YAML frontmatter: source_url/ingested/sha256/citation/domain/tags
  - sha256: cdaead85f66c0e723d27b610883cfdd2121ca53b35ec58932fcca09c5a4a07d5
  - Citation: 刘如谦组 Nat Biotechnol 2026 突破：定向进化越改越脆，AI蛋白设计救场 — PE8 系列先导编辑器。BioTender 观测日志 (微信公众号), 2026.
  - Domain: ai-drug-discovery
  - Marketing CTA trimmed (biotender.online/scholar-program 推广链接+图片); WeChat footer images trimmed
  - Content: PACE 进化 PE6 系列 RT 活性↑但表达↓1.5-2x（稳定性-功能权衡）→ ProteinMPNN 逆折叠重设计 + 冻结催化核心 → PE8 系列表达+2.0-2.3x, Tm+8°C, 95%功能存活率, 体内编辑 44% vs PE7 25%, 无脱靶

### concepts
- Created: concepts/ai-evolution-complementary-protein-engineering.md — 进化+AI互补蛋白工程范式概念页
  - 核心问题：定向进化偏科（活性吸向核心，稳定性代价被无视）
  - 解决方案：ProteinMPNN 修"地基"（冻结催化核心 + 表面重设计 30-163 AA）
  - PE8 性能数据表（表达/Tm/功能率/体内编辑/脱靶）
  - 互补分工对比表（进化管"心脏" vs AI 管"骨架"）
  - 交叉引用: [[de-novo-protein-design]], [[ovo]], [[gem-gpcr-modulator]], [[nextgen-vaccine-strategies]], [[ori-protein-design]]

### index.md: Concepts +1, ai-drug-discovery raw +1, page count 187→190
### SCHEMA.md: 无需改动（ai-drug-discovery domain + tags 已存在）
## 2026-05-29

### raw/articles
- raw/articles/ESM-World-Model-Biohub-Zuckerberg-智药局.md
  - YAML frontmatter: source_url/ingested/sha256/citation/domain/tags
  - sha256: 683cf2e8df4091986204039517075448b876fd74470d23d652139425f3a17cec
  - Citation: 智药局微信公众号, 2026
  - Domain: ai-drug-discovery
  - Marketing: 标题"王炸"等营销措辞去掉; WeChat 图片全部移除; 内容重写为结构化摘要
  - Content: Biohub（CZI/扎克伯格）开源全球首个蛋白质世界模型 = ESMC (300M/600M/6B pLM, 28亿序列) + ESMFold2 (折叠, 略胜AF3) + ESM Atlas (68亿蛋白/11亿结构图谱)。5癌症靶点命中率 36-88%, 抗体模式 15-29%。CZI $500M/5年生命预测计划

### entities
- Created: entities/esm-world-model.md — ESM World Model 实体页
  - 组件架构表：ESMC / ESMFold2 / ESM Atlas
  - 与 AlphaFold3/RFdiffusion 对比表
  - 交叉引用: [[de-novo-protein-design]], [[alphafold3]], [[ori-protein-design]], [[seqdance-esmdance]]

### index.md: Entities +1, ai-drug-discovery raw +1, page count 190→208
