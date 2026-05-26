# Wiki Index

> Content catalog. Every wiki page listed under its type with a one-line summary.
> Last updated: 2026-05-26 | Total pages: 190 · 11 domains (panel-design, quantitative-mngs, tngs-comparison, ai-drug-discovery, bioinfo-pipeline, clinical-evaluation, cancer-biology, vaccine-design, market-watch, machine-learning)
<!-- ↑ Total 190 pages -->

## Entities
- [[cgp-probe-design-consensus-2026]] — 肿瘤CGP探针设计专家共识2026版，6大共识涵盖基因分级/探针设计/变更管理
- [[cnvpipe]] — CNVPipe 基于机器学习的WGS拷贝数变异检测流程，Snakemake框架整合5工具+SVM+单细胞模块（bioRxiv 2025）
- [[crocodeel]] — CroCoDeEL 无对照宏基因组交叉样本污染检测工具，RANSAC+RF四步法，0.1%最低检测限，Meteor2 最佳（Nat Commun 小包子学演化 2026）
- [[deeprare]] — DeepRare AI智能体罕见病诊断系统，MCP三层架构+DeepSeek-V3，首次AI超越人类专家（Zhao W, Nature 2026）
|- [[e-nose-respiratory-infection-zju-acs]] — 石墨烯电子鼻×机器学习区分细菌/病毒呼吸道感染，SVM/RF/Lasso加权融合模型，145例临床呼气样本验证（ACS Nano 2025 浙江大学邬建敏）
|- [[fastsc]] — R 包 fastSC v1.0.2，一站式单细胞分析+可视化+CellChat/Monocle/inferCNV 接口
- [[gbt46943-2025-mngs-standard]] — GB/T 46943-2025 mNGS性能确认国家标准，涵盖LoD/精密度/准确度/生信流程
- [[kingcreate-kcq]] — 金圻睿KCQ mNGS绝对定量系统，15条合成内参，与ddPCR R=0.97
- [[kingcreate-tngs-panel]] — 金圻睿RP100试剂盒 + hc-tNGS探针体系，198/3060靶标
- [[methylvi]] — MethylVI 单细胞甲基化VAE概率生成模型，Beta-Binomial似然，scvi-tools集成（Ashuach T, Nat Mach Intell 2025）
- [[de-novo-protein-design]] — David Baker Nature 2026 综述：从头蛋白质设计的过去现在与未来，折叠/组装/结合已基本解决，前沿为动态纳米机器
- [[isodde]] — IsoDDE Isomorphic Labs Drug Design Engine，全面超越AF3的蛋白-小分子/抗体-抗原/亲和力统一预测系统
- [[mmseqs2]] — MMseqs2-GPU GPU加速同源搜索算法，比JackHMMER快177x，ColabFold加速31.8x，Foldseek加速4-27x（Nat Methods 2025, Mirdita M）
- [[ovo]] — Ovo 开源从头蛋白设计生态系统：Nextflow 工作流编排 + ProteinQC 质量评估 + Web GUI/CLI，scaffold/binder/diversification 三范式（MSD + AWS, bioRxiv 2025）
- [[ori-protein-design]] — ORI 腾讯AI4S 本体强化迭代蛋白设计闭环（PDA + PGM + USM + RLWF），溶菌酶/热稳定几丁质酶/双功能酶实验验证，TX-RL15 活性超天然溶菌酶两个数量级（He B, Nat Commun 2026）
||- [[protenix-v2]] — Protenix-v2 ByteDance Seed：统一结构预测+zero-shot 抗体设计，GPCR 靶点 16-88% VHH hit rate，5 seeds 超越 v1 1000 seeds
||- [[pplm]] — PPLM 蛋白配对语言模型，hybrid intra-/inter-protein attention 双链建模，PPI 二分类/亲和力/链间接触三项任务优于基线（Liu J, Nat Commun 2026）
|- [[diamond-deepclust]] — DIAMOND DeepClust 级联超快速蛋白质聚类，190亿序列→17亿簇，3.35亿≥3簇=BFD 5.5x，AF2 pLDDT↑7.73（Nat Methods 2026）
||- [[drugclip-mclm-contrastive-aidd]] — CLIPP-SET/MCLM 对比 3D 蛋白-配体学习与条件分子生成，SET SE(3)等变 transformer + CF-InfoNCE 对比 + Llama2 SMILES decoder，dataset token 控制化学空间，scaffold hopping 验证（arXiv 2026 Acellera/UPF）
||- [[gps-ai-drug-platform]] — GPS AI药物设计平台：化学结构→转录组效应预测，Z-RGES评分逆转疾病表型筛选，肝癌/MSU45302/肺纤维化（Cell 2026）
|- [[alphafold3]] — AlphaFold 3 全生物分子统一结构预测框架，扩散模型+Pairformer，蛋白/核酸/配体/离子，PoseBusters 76.4%（DeepMind, Nature 2024）
|- [[boltz2]] — Boltz-2 蛋白质-配体共折叠虚拟筛选，ULVSH基准ROC-AUC 0.70，结合模式与打分可解耦，富集率4-5x
|- [[softmol]] — SoftMol 块扩散分子生成，SoftBD架构100%有效性+6.6x加速，MCTS目标导向，5靶点刷新纪录
||- [[protdbench]] — ProtDBench 蛋白 binder 设计统一评测基准，verifier 偏差分析 + throughput-aware 24h A100 预算标准化比较（Liu C, arXiv 2026）
||- [[dual-gpt-ab]] — DualGPT-AB 双阶段 GPT+RL 抗体设计，多性质约束前置生成，HER2 靶点湿实验验证（Nature Computational Science 2026）
||- [[tcr-prp]] — TCR-PRP 多肽识别谱系统，pLM 预测 T 细胞激活与自身抗原发现，HLA-B*27:05 自身免疫新抗原（Nat Biotechnol 2026）
||- [[rxnbench]] — RxnBench 化学反应理解 MLLM 评测基准，41 款模型无人达 FD-QA >50%（深势科技/上海交大/清华等, JCIM 2026）
||- [[gem-gpcr-modulator]] — GEMs 从头设计 GPCR 跨膜区变构调节蛋白，RFdiffusion+MPNN+AF2 迭代 + 结构提示词（浙江大学, Nature 2025）
||- [[highfold]] — HighFold/HighFold2/HighFold3 环肽结构预测工具系列，CycPOEM+ncAA+Cyclization 开关（Hongliang Duan Lab）
||- [[boat]] — BOAT 抗体多目标贝叶斯优化框架，高斯过程+遗传算法多属性帕累托搜索（AstraZeneca）
||- [[sefmoll]] — SeFMol RL+扩散模型半柔性分子生成，口袋内生长范式（Sci Adv 2025）
||- [[tarpass]] — TarPass 靶点感知分子生成评测基准，15模型多数不如ChEMBL随机抽样（Adv Sci 2025）
- [[nfcore-epigenomics-pipelines]] — Nextflow 表观组学流程（ChIP/ATAC/Hi-C），13个流程
- [[nfcore-genome-pipelines]] — Nextflow 基因组组装与注释流程，27个流程
- [[nfcore-longread-pipelines]] — Nextflow 三代测序流程（Nanopore/PacBio），17个流程
- [[nfcore-microbiome-pipelines]] — Nextflow 微生物组与宏基因组流程，42个流程
- [[nfcore-phylogenomics-pipelines]] — Nextflow 系统发育与 pan-基因组流程，16个流程
- [[nfcore-preprocessing-pipelines]] — Nextflow 预处理与 QC 流程，10个流程
- [[nfcore-proteomics-pipelines]] — Nextflow 蛋白质组与结构预测流程，4个流程
- [[nfcore-rnaseq-pipelines]] — Nextflow RNA-seq 与转录组流程，28个流程
- [[nfcore-scrnaseq-pipelines]] — Nextflow 单细胞与空间转录组流程，12个流程
- [[nfcore-variant-calling-pipelines]] — Nextflow 变异检测流程（SNV/SV/CNV），30个流程
- [[qmngs-internal-standards]] — qmNGS定量宏基因组方法
- [[scnotebooks]] — 开源单细胞与空间组学培训平台 scNotebooks，13模块三层结构，多语言+云端运行（Nature Genetics 2026）
- [[scmbench]] — SCMBench 单细胞多组学集成基准测试，23种方法对比，DMs vs FMs（Nat Commun 2026 港中文）
- [[scmethcraft]] — scMethCraft 单细胞 DNA 甲基化统一分析框架，混合神经网络+KAN 序列特征融合，降维/聚类/批次整合/增强/DMR 全流程（Nat Commun 2026 南开陈盛泉）
|- [[regformer]] — RegFormer 单细胞 Mamba 基础模型，基因调控拓扑排序 + 双重嵌入 + 药物敏感性预测（Nat Commun 2026 BGI）
|- [[seqdance-esmdance]] — SeqDance/ESMDance 基于 MD 动力学生物物理特征的蛋白质语言模型，Transformer 编码残基共运动/全局构象，ESMDance 零样本预测设计蛋白/病毒蛋白突变效应大幅超越 ESM2（PNAS 2026 Hou C）
||- [[wisecondorx]] — 浅层全基因组CNV检测，WISECONDOR归一化改进，NIPT/肿瘤/生信流程
|- [[ws-hla-genotyping-standard]] — HLA 基因分型检测体系技术标准，SSP/SSO/SBT 三种方法、QC体系、环境与人员要求
## Concepts
- [[ai-evolution-complementary-protein-engineering]] — 进化+AI互补蛋白工程范式：定向进化管活性、AI逆折叠管稳定性，PE8系列先导编辑器50%体内效率证实，ProteinMPNN修"地基"不碰催化核心（David Liu, Nat Biotechnol 2026）
- [[as-neoantigen-hcc-vaccine]] — AS来源新抗原的肝癌通用型mRNA疫苗，AS事件丰度为突变59倍，覆盖50.94% HCC患者，TAPi抗原绕过62%患者TAP缺陷（Zhao H, Cell Res 2025）
- [[bayesian-optimization]] — Bayesian Optimization 贝叶斯优化：GP/RF代理模型、EI/PI/UCB采集函数、RF+BO超参数调优、与Grid/Random Search对比
- [[scrnaseq-workflow]] — scRNA-seq 标准化分析流程：湿实验建库、生信分析、降维聚类、细胞注释、差异分析、临床转化
- [[span-vaccine]] — S_pan 广谱新冠疫苗：基于S蛋白进化轨迹的系统发育共识抗原设计，跨进化支WT/Beta/Delta/Omicron保护（Zhao Y/Xu K, Sci Transl Med 2023）
- [[b-cell-epitope-prediction]] — B 细胞表位预测概念页：线性/构象表位分类、MsgaBpred（结构基/构象表位）+ DFPred（序列基/线性表位）两种 SOTA 路径、与疫苗设计的连接
- [[bowtie2-alignment]] — Bowtie2 比对算法：BWT/FM-index、local vs end-to-end 模式、InnoPAP pipeline 实际配置（tNGS local / mNGS end-to-end）
- [[protdbench]] — ↳ 已迁移至 [[entities/protdbench]]（概念→实体）
- [[kraken2-classification]] — Kraken2 + Bracken 物种分类与丰度估计：k-mer + LCA 分类、Bayesian 丰度重估、confidence 阈值、InnoPAP mNGS 四层过滤流程
- [[nextgen-vaccine-strategies]] — 新一代病毒疫苗设计四大策略：结构抗原工程、mRNA+LNP、佐剂系统、免疫印记应对（Yuan F, Vaccines 2025）
- [[ont-sv-analysis]] — ONT SV 结构变异分析流程：minimap2→cuteSV/Sniffles2/SVIM→多工具交叉验证→VEP注释→可视化
- [[star-rsem-rnaseq]] — STAR + RSEM 转录组比对与定量：STAR 剪接感知比对 + TranscriptomeSAM、RSEM EM 算法 isoform 定量、transcriptome pipeline 配置
- [[sv-analysis-pacbio-hifi]] — PacBio HiFi SV 检测六步流程：ccs→pbmm2→pbsv→AnnotSV→SnpSift→IGV
- [[wgd-immune-evasion]] — WGD 驱动免疫逃逸的代谢-表观遗传机制：琥珀酸↑→KDM6↓→H3K27me3↑→MHC-I 沉默
- [[universal-intranasal-vaccine-gla-3m052]] — GLA-3M-052-LS+OVA 鼻喷广谱疫苗，整合器官免疫范式，TLR4+TLR7/8双激动剂+肺泡巨噬细胞表观遗传重塑，病毒/细菌/过敏原全覆盖（Zhang H, Science 2026）
- [[cfdna-fragmentomics-mced]] — cfDNA 片段组学多癌早检（MCED），13癌种I期79.3%/特异性98.9%，四大信号维度对比，金陵队列（Shao Y, Nat Med 2026）
- [[five-cancer-screening-guide]] — 五大癌种筛查指南（乳腺/结直肠/宫颈/前列腺/肺癌），AI辅助，MCED补充（Duffy SW, Nat Rev Clin Oncol 2026）
- [[ai4s-business-framework]] — AI4S赛道框架：感知行合四种生意，License-out/SaaS/IDM三种变现，DBTL飞轮，商业化甜蜜点迁移（钛资本研究院）
- [[random-forest]] — Random Forest（随机森林）集成学习算法：Bagging + 随机特征选择，分类/回归/特征重要性，生物信息学应用（特征筛选/微生物分类/污染检测/癌症早筛）
- [[svm]] — SVM 支持向量机：最大间隔超平面、核技巧（线性/RBF/多项式）、与 RF 对比，生信应用（CNV 过滤/微阵列分类/蛋白质定位）
- [[transformer]] — Transformer 自注意力机制：多头注意力、位置编码、Transformer+LSTM 时序融合架构，生信应用（蛋白质语言模型 ESM/基因表达时序/DNA 序列建模）
## Comparisons
- [[tngs-vs-mngs]] — mp-tNGS / hc-tNGS / mNGS 三方法全面对比（性能+成本+适用场景）

## Code Index
- [[codebase-index]] — 统一代码组织入口，Python/Perl/R/Rust 工具 + Nextflow Pipeline + 第三方工具索引，symlink 聚合

## Raw Sources

### quantitative-mngs
- [[raw/papers/li2021-qmngs-aem]] — qmNGS定量宏基因组，合成内参ISF，ARG定量，Yseq模型 (Li B, AEM 2021)
- [[raw/papers/wanghu2024-kcq-microbgenom]] — KCQ绝对定量系统，15条合成内参，与ddPCR R=0.97 (Wanghu H, Microb Genom 2024)

### tngs-comparison
- [[raw/papers/wanghui-tngs-ebiomedicine-2024]] — mp-tNGS vs hc-tNGS 下呼吸道感染诊断，198 vs 3060 靶标 (Yin Y, EBioMedicine 2024)

### ai-drug-discovery
- [[raw/papers/wang2026-drugblip-bioinformatics]] — DrugBLIP图Transformer多任务学习统一蛋白-分子相互作用，虚拟筛选/对接/靶点预测 (Wang R, Bioinformatics 2026)
- [[raw/papers/watson2023-rfdiffusion-nature]] — RFdiffusion Nature 2023 原始论文：RoseTTAFold微调为扩散模型，无条件单体生成/对称寡聚体/结合物/基序支架，流感血凝素2.9Å冷冻电镜验证 (Watson JL, Nature 2023)
- [[raw/papers/kosonocky2026-ai-protein-design-cosb]] — Curr Opin Struct Biol 2026 原始论文：AI蛋白设计实验验证方法系统综述，binder/antibody/enzyme 三类方法实验成功率表格 (Kosonocky CW, COSB 2026)
- [[raw/papers/isomorphic2026-isodde]] — IsoDDE Isomorphic Labs Drug Design Engine 技术报告：全面超越AF3的蛋白-小分子/抗体-抗原/亲和力预测 (Isomorphic Labs, 2026)
- [[raw/papers/liu2026-protdbench-arxiv]] — ProtDBench arXiv 2026 原始论文：蛋白binder设计统一评测基准，verifier偏差+throughput-aware+多样性分析 (Liu C, arXiv 2026)
- [[raw/articles/RegFormer-单细胞大模型Mamba架构-NatCommun2026]] — RegFormer 单细胞 Mamba 基础模型，基因调控拓扑排序 + 双重嵌入 + 药物敏感性预测（微信公众号·生物信息与人工智能，2026/05）
- [[raw/articles/David-Baker-Nature综述-蛋白质从头设计的过去现在与未来]] — David Baker Nature 2026 综述：从头蛋白设计从物理模型到生成式AI，折叠/组装/结合已基本解决（微信公众号·BrainMed Lab）
- [[raw/articles/RFdiffusion-蛋白质从头设计-Nature论文详解]] — RFdiffusion Nature 2023 论文详解：扩散模型微调RoseTTAFold生成蛋白骨架，单体/对称寡聚体/结合物/基序支架（微信公众号·AI-Protein Design）
- [[raw/articles/RFdiffusion1-架构原理训练上手指南-AIinBio]] — RFdiffusion 精读系列（一）：架构原理、训练过程和上手指南，源码结构/SE3扩散/条件化生成（微信公众号·AI in Bio）
- [[raw/articles/RFdiffusion-SE3扩散框架详解-Nature2023-Bits&Bases]] — RFdiffusion SE(3) 扩散框架详解：RoseTTAFold→扩散模型微调、框架表示/自条件化/去噪轨迹（微信公众号·Bits & Bases）
- [[raw/articles/RFdiffusion五大设计任务详解-Nature2023-Bits&Bases]] — RFdiffusion 五大设计任务全解：单体蛋白/对称寡聚体/功能基序支架/对称基序/蛋白结合物（微信公众号·Bits & Bases）
- [[raw/articles/ProtDBench-蛋白Binder设计统一评测基准-arXiv2026]] — ProtDBench arXiv 2026：蛋白 binder 设计统一评测基准，verifier 偏差分析 + throughput-aware 标准比较（微信公众号·BioAIDesign）
|- [[raw/articles/AI蛋白设计实验验证方法综述-CurrOpinStructBiol2026]] — Curr Opin Struct Biol 2026 综述：AI蛋白设计实验验证方法全景——binder/antibody/enzyme 三类任务实验成功率系统比较（微信公众号·BioAIDesign）
|- [[raw/articles/Protenix-v2-抗体设计-结构预测-药物发现]] — Protenix-v2 ByteDance Seed：从结构预测到 zero-shot 抗体设计，GPCR 靶点 16-88% VHH hit rate（微信公众号·BioAIDesign）
|- [[raw/articles/RFdiffusion-爆款论文冷思考-潜力与局限]] — RFdiffusion 冷思考：对称寡聚体~14%/结合蛋白~19%成功率，AF2同源性偏差，骨架-序列解耦假设脆弱，训练数据边界（HZAU蛋白质科学）
|- [[raw/articles/Ovo-蛋白设计开源生态平台-bioRxiv2026]] — Ovo 蛋白设计开源生态平台：生成/筛选/质控/管理全流程整合，bioRxiv 2026（微信公众号·AI药物设计实验室）
|- [[raw/papers/prihoda2025-ovo-biorxiv]] — Ovo 开源从头蛋白设计生态系统：Nextflow 工作流编排 + ProteinQC + Web GUI/CLI，scaffold/binder/diversification 三范式（Prihoda D, bioRxiv 2025）
|- [[raw/articles/GPS-AI药物设计平台逆转疾病转录表型-Cell]] — GPS AI药物设计平台：转录表型逆转驱动虚拟筛选，肝癌/MSU45302/UHRF1/肺纤维化/吡噻酮（Cell 2026，微信公众号·生信星图）
|- [[raw/articles/计算驱动靶向肽药物设计-物理建模到生成式AI-InnovDrugDiscov2026]] — 靶向肽药物设计综述：生成→逆折叠→预测→ADMET→物理验证闭环框架，PepMLM/EvoDiff/HighFold/PepFlow/LigandMPNN全景（Hu W, Innov Drug Discov 2026，微信公众号·陷入鞍点）
|- [[raw/articles/AlphaFold3深度解读-扩散模型与全生物分子时代]] — AlphaFold 3 深度解读：扩散模型取代结构模块，Pairformer×48，PoseBusters 76.4% 碾压 Vina，全生物分子统一框架（微信公众号·HZAU蛋白质科学，Nature 2024）
|- [[raw/articles/RFdiffusion-爆款论文冷思考-潜力与局限]] — RFdiffusion 冷思考：对称寡聚体~14%/结合蛋白~19%成功率，AF2同源性偏差，骨架-序列解耦假设脆弱，训练数据边界（HZAU蛋白质科学）
||- [[raw/articles/AI筛药新工具-Boltz2-SoftMol-AINN-P1-DeepClust]] — AI筛药四工具综述：DeepClust/Boltz-2共折叠/SoftMol块扩散/AINN-P1稳定性预测（榴莲忘返AIDD，2026）
||- [[raw/articles/RxnBench-41款MLLM评测-深势科技-JCIM]] — RxnBench 化学反应理解多模态 LLM 评测基准，双层任务/41模型/最佳FD-QA<50%（微信公众号·AI蛋白质，JCIM 2026）
||- [[raw/articles/信手随翻209-Nature-从头设计GPCR结构Binder调节剂]] — GEMs 从头设计 GPCR 跨膜区变构调节蛋白，RFdiffusion+MPNN+AF2+结构提示词，Cryo-EM 验证（微信公众号·信手随翻，浙江大学，Nature 2025）
||- [[raw/articles/HighFold-环肽结构预测三部曲迭代]] — HighFold 系列环肽结构预测三部曲：CycPOEM/ncAA/Cyclization 开关，AF3 闭环成功率 100% vs 21%（微信公众号·AI药物设计实验室）
||- [[raw/articles/DualGPT-AB双阶段生成式抗体设计-NatureComputationalScience]] — DualGPT-AB 双阶段 GPT+RL 抗体设计论文解读，HER2 靶点湿实验验证（微信公众号·青禾矩点，Nature Computational Science 2026）
||- [[raw/articles/AI造药新范式-口袋生长分子-BOAT-ProteomeScan-SeFMol-TarPass]] — AI造药新范式：BOAT/ProteomeScan/SeFMol/FlyPredictome/TarPass 五工具综述（微信公众号·榴莲忘返AIDD，2026）
- [[raw/articles/SeFMol-强化学习引导半柔性分子扩散模型-SciAdv2026]] — SeFMol 强化学习引导半柔性分子扩散模型，三阶段架构（预训练+微调+RL优化），20x快速采样，Vina -7.23，成功率11.53%（Sci Adv 2026，微信公众号·AI蛋白质）
|- [[raw/articles/PPLM-蛋白配对语言模型-PPI预测-NatureComms2026]] — PPLM 蛋白配对语言模型，双链编码+hybrid attention，PPI/亲和力/接触预测三项任务超越 ESM2/TUnA（Liu J, Nat Commun 2026，微信公众号·AI药物设计实验室）
|- [[raw/articles/SeqDance-ESMDance-蛋白质动力学语言模型-PNAS2026-biomath]] — SeqDance/ESMDance pLM 基于 MD 动力学预训练预测突变效应，Transformer 编码残基共运动/全局构象，ESMDance 零样本预测设计蛋白 Spearman r 0.46 vs ESM2 0.21（PNAS 2026 Hou C, biomath微信公众号）
|- [[raw/articles/DrugCLIP-MCLM-对比学习-AIDD-arXiv2026]] — CLIPP-SET/MCLM 对比 3D 蛋白-配体学习 + 条件分子生成，SET SE(3)等变transformer/CF-InfoNCE/dataset token，scaffold hopping检索优于Morgan FP（arXiv 2026 Acellera/UPF，陷入鞍点）
|- [[raw/papers/hou2026-seqdance-esmdance-pnas]] — SeqDance/ESMDance 基于 MD 动力学预训练的 pLM 原始论文：Transformer 编码残基共运动/全局构象，零样本预测设计蛋白/病毒蛋白突变效应（Hou C, PNAS 2026）
|- [[raw/articles/ORI-腾讯AI4S蛋白设计闭环-NatureComm2026]] — ORI 腾讯 AI4S 本体强化迭代蛋白设计闭环，PDA + PGM + USM + RLWF，溶菌酶/几丁质酶/双功能酶实验验证，TX-RL15 活性超天然溶菌酶两个数量级（He B, Nat Commun 2026，微信公众号·AI药物设计实验室）
|- [[raw/articles/NatBiotech-定向进化越改越脆-AI蛋白设计救场]] — 刘如谦组 Nat Biotechnol 2026：定向进化越改越脆、AI蛋白设计（ProteinMPNN）救场，PE8系列 2.0-2.3x 表达 +8°C Tm，体内编辑44% vs PE7 25%，"进化打底、AI加固"互补范式（BioTender观测日志）

### bioinfo-pipeline
- [[raw/articles/scMethCraft-单细胞DNA甲基化统一分析框架-NatCommun2026]] — scMethCraft 单细胞 DNA 甲基化统一分析框架，混合神经网络+KAN 序列特征融合+迭代相似性加权，NA 原生处理，降维/聚类/批次整合/增强/DMR 全流程（Nat Commun 2026，微信公众号·生信钱同学）
- [[raw/articles/5分生信文章工作量标杆-IPF多组学机器学习]] — IPF多组学+机器学习，14张图发5分的工作量拆解（微信公众号·纯肥瘦肉夹馍）
- [[raw/articles/R语言SCI论文7种图可视化DeepSeekV4]] — R语言SCI论文7种图：ROC/校准/DCA/列线图/OncoPrint/桑基/SHAP，代码即用（RAI王博士）
- [[raw/articles/scrnaseq-analysis-guide-wechat]] — scRNA-seq 标准化分析全流程指南：湿实验到生信分析到临床转化（微信公众号，2026/05）
- [[raw/articles/NatGen-scNotebooks开源单细胞空间组学培训笔记库]] — scNotebooks 开源单细胞与空间组学培训平台，Nature Genetics 2026（微信公众号，2026/05）
- [[raw/articles/R包fastSC单细胞转录组快速数据分析和可视化]] — fastSC R 包 v1.0.2 文档：一站式单细胞分析 + 可视化 + CellChat/Monocle/inferCNV（微信公众号，2026/05）
- [[raw/articles/HLA检测生信算法工程师面试核心问题与解答]] — HLA 分型生信面试：NGS/长读长分型算法、血型基因分型、临床报告、IVD验证体系（微信公众号·AI生信工坊，2025）
- [[raw/papers/ws-hla-genotyping-standard-2021]] — HLA 基因分型检测体系技术标准，三种分型方法 + QC体系 + 检测报告要求（WS 行业标准，2021）
- [[raw/articles/ONT数据进行SV分析流程-微信芈小小]] — ONT 三代测序 SV 分析流程：minimap2→cuteSV/Sniffles2/SVIM→多工具交叉验证→VEP注释→可视化（微信公众号·一问一答-2024启航++，2025）
- [[raw/articles/PacBio-HiFi-SV结构变异分析完整流程]] — PacBio HiFi SV 分析流程：ccs→pbmm2→pbsv→AnnotSV→SnpSift→IGV，DEL/INS/INV/DUP/BND 检测（微信公众号，2025）
- [[raw/articles/Nextflow工作流清单100-微生物组转录组变异三代测序]] — Nextflow 开源工作流清单第一期（1-100）：微生物组/变异 calling/RNA-seq/三代测序，附论文引用与 GitHub 链接（微信公众号·逻捷科技，2025）
- [[raw/articles/Nextflow工作流清单100-比对注释单细胞101-200]] — Nextflow 开源工作流清单第二期（101-200）：15个子专题，比对/注释/单细胞/ATAC-seq/Hi-C/质谱（微信公众号·逻捷科技，2025）
- [[raw/articles/生物信息学数据格式终极指南]] — 生信核心数据格式大全：FASTA/FASTQ、SAM/BAM、GFF/GTF/BED、VCF/BCF、PDB/mmCIF、PHYLIP/Newick、HDF5/mzML（微信公众号·宏序生物，2025）
- [[raw/articles/生物信息学文件格式指南-FASTA-FASTQ-SAM-BAM-VCF-GFF-GTF]] — 8种生信核心格式详解：FASTA/FASTQ/SAM/BAM（FLAG/CIGAR/MAPQ）/VCF/BCF/GFF（GFF2 vs GFF3）/GTF（微信公众号·生信技术，2025）
- [[raw/articles/SCMBench-单细胞多组学集成分析基准测试-NatCommun2026]] — SCMBench 单细胞多组学集成基准测试，23种方法，DMs vs FMs（Nat Commun 2026 微信公众号·生物信息前沿进展）
- [[raw/papers/foidart2026-wgd-cancercell]] — WGD 通过表观遗传沉默 MHC-I 驱动免疫逃逸，KDM6↓→H3K27me3↑→抗原呈递↓（Foidart P, Cancer Cell 2026）
- [[raw/articles/SpaNiche-空间转录组共定位与互作分析框架-GenomeBiology]] — SpaNiche 空间共定位与互作分析框架：Niche 识别→共定位评分→细胞互作推断→空间可视化（Huang S, Genome Biol 2026 微信公众号·TOP生物信息）
- [[raw/articles/deepvariant-tool-overview]] — DeepVariant 基于深度学习的通用变异检测工具：pileup 图像化→CNN→VCF 全线详解，性能基准 vs GATK，版本演进，三代测序支持，tNGS 场景适用性评估
- [[raw/articles/群体遗传学分析-4.2-SNP-calling-DeepVariant]] — DeepVariant Docker 安装、运行三步走（make_examples→call_variants→postprocess），WGS 和 PacBio HiFi 实战（微信公众号·小伍的科研笔记）
- [[raw/articles/DeepVariant又快又准-替代GATK的变异检测工具]] — DeepVariant vs GATK 性能比较，6-channel pileup 图像特征，non-uniqueness mappability / polyA 区域实际案例差异（微信公众号·生信开发者）
- [[raw/articles/DeepVariant-SnakeMake流程-Workflow简介]] — 种系变异 DeepVariant SnakeMake 流程：fastp→BWA-mem2→samblaster→DeepVariant→GLnexus→VEP（微信公众号·生信探索）
- [[raw/articles/minimap2-DeepVariant-HiFi检测SNP-INDEL]] — minimap2 + DeepVariant 利用 PacBio HiFi 数据检测 SNP/Indel，PACBIO 模型参数详解（微信公众号·允思拓）
- [[raw/articles/CNVPipe-基于机器学习的WGS拷贝数变异检测流程-bioRxiv2025]] — CNVPipe WGS CNV检测流程：Snakemake框架整合CNVKit/CNVpytor/cn.MOPS/Delly/Smoove 五工具+SVM过滤+单细胞CNV倍性估计创新（bioRxiv 2025，微信公众号·我爱学生信）
- [[raw/articles/CroCoDeEL-宏基因组交叉样本污染检测-NatCommun]] — CroCoDeEL 无对照检测宏基因组交叉样本污染：RANSAC+RF四步法，SHAP特征排序，0.1%最低检测限，Meteor2 最优（Nat Commun 微信公众号·小包子学演化 2026）
- [[raw/papers/goulet2026-crocodeel-nc]] — CroCoDeEL 原始论文：无对照宏基因组交叉样本污染检测，污染线原理→RANSAC→10SHAP特征→RF分类，级联污染，TwinsUK/Ferretti/Lou 案例（Goulet L, Nat Commun 2026）
- [[raw/articles/DeepSomatic-多测序技术体细胞变异检测-NatBiotechnol2025]] — Google DeepSomatic：Illumina/PacBio/ONT 体细胞小变异检测，CASTLE 基准集，F1 0.9829 (SNV) / 0.8944 (Indel)，FFPE 模型（Nat Biotechnol 2025）
- [[raw/articles/DeepSomatic-数据基准与算法创新-NatBiotechnol2025-深度解读]] — DeepSomatic 深度解读：多测序技术体细胞变异检测的数据基准与算法创新（微信公众号）
- [[raw/articles/MethylVI-单细胞甲基化VAE贝塔二项分布-NatMachIntell]] — MethylVI 单细胞甲基化VAE概率生成模型，Beta-Binomial似然，降噪/DMG/整合/reference mapping，scvi-tools集成（Nat Mach Intell 2025，CellSpace AI微信公众号）
|- [[raw/articles/MMseqs2-GPU同源搜索-NatureMethods2025]] — MMseqs2-GPU 同源搜索算法：GPU加速 gapless filter + Smith-Waterman，单L40S比JackHMMER快177x，ColabFold加速31.8x（Nat Methods 2025，微信公众号·MetagenomeBioin，Ethan Ethan）
|- [[raw/articles/DeerFlow-AI-Agent框架代码结构与业务流转指南]] — DeerFlow AI Agent 框架：LangGraph+Next.js+FastAPI 四进程架构，14中间件链，Sandbox/MCP/记忆系统/Skills加载（万智创界微信公众号，walton）
|- [[raw/articles/DIAMOND-DeepClust-蛋白质宇宙聚类-NatureMethods-另一视角]] — DIAMOND DeepClust 级联聚类深度解读：190亿序列→17亿簇，双向覆盖优势，AF2 pLDDT↑7.73（微信公众号·小包子学演化，Nat Methods 2026）
|- [[raw/articles/DIAMOND-DeepClust-蛋白质宇宙超快速聚类-NatureMethods2026]] — DIAMOND DeepClust 超快速蛋白质聚类全解读：27节点18天190亿序列，NR聚类比MMseqs2快36x，1.18亿新蛋白家族（微信公众号·吕信院，Nat Methods 2026）

- [[raw/articles/宏基因组-Kraken-RTL路径打分算法解析]] — Kraken RTL (Root To Leaf) 路径打分算法详解：k-mer投票→节点赋权→路径累加→最终分类，与简单投票法对比，整合不同精度层级的证据（微信公众号·MetagenomeBioin，Ethan Ethan，2026）
- [[raw/articles/一篇文章看懂-生殖系变异vs体细胞突变]] — 生殖系变异 vs 体细胞突变全面对比：定义/来源/遗传模式/检测方法/临床意义，面向生信入门（微信公众号·BIMer科研充电站）

### clinical-evaluation
- [[raw/papers/gbt46943-2025-mngs-validation]] — GB/T 46943-2025 mNGS性能确认国家标准，涵盖标本制备/LoD/精密度/准确度/生信流程（国家标准，2025）
- [[raw/articles/DeepRare-AI驱动罕见病诊断智能体-Nature2026]] — DeepRare AI智能体罕见病诊断系统，MCP三层架构+DeepSeek-V3，40+工具集成，首次AI超越人类专家（Nature 2026，G-Plume微信公众号）
- [[raw/papers/zhao2026-deeprare-nature]] — DeepRare 原始论文：MCP启发三层架构AI智能体，6,401例/2,919种罕见病/14专科评估，Recall@1 57.18%，首次超越人类专家（Zhao W, Nature 2026）
- [[raw/articles/机器学习与基因组学赋能精准肿瘤学-NatRevCancer2026]] — Nat Rev Cancer 2026 综述：机器学习与基因组学在精准肿瘤学中的深度融合，涵盖biomarker发现/风险分层/治疗响应预测（微信公众号·基因研究所）

### panel-design
- [[raw/articles/基于NGS的肿瘤全景变异检测探针设计专家共识2026版]] — 肿瘤CGP探针设计专家共识，基因分级/探针设计/性能评估/变更管理（中华肿瘤杂志 2026）

### vaccine-design
- [[raw/papers/yuan2025-nextgen-vaccines-vaccines]] — 新一代病毒疫苗设计四大前沿方向的系统综述：结构抗原工程、mRNA+LNP平台、先进佐剂系统、免疫印记应对策略（Yuan F, Vaccines 2025）
- [[raw/papers/cao2025-broad-vaccines]] — 广谱疫苗设计综述：结构抗原工程 → 纳米颗粒递送，4种抗原优化策略 + 纳米颗粒类型 + mRNA+VLP联用（Cao M, J Virol 2025）
- [[raw/papers/jiang2023-duv-led-virus-inactivation]] — DUV LED 快速灭活人呼吸道 RNA 病毒：深紫外 LED 灭活 IAV 和 SARS-CoV-2，病毒滴度降低 >99.99%，RNA 完整性破坏（Jiang K, Opto-Electron Adv 2023, 徐可团队）
- [[raw/papers/zheng2022-dhodh-antivirals]] — 宿主靶向广谱抗病毒策略：DHODH 抑制剂作为通用抗病毒药物，嘧啶从头合成通路 + 三机制抗病毒 + 结合位点分析（Zheng Y, Viruses 2022）
- [[raw/papers/zhong2024-recomb-vaccine-progress]] — 病毒性传染病的重组蛋白疫苗研究进展：免疫原设计（偶联/工程化/重构）、表达纯化工艺（CHO/酵母/大肠杆菌）、规模化生产（徐可团队，武汉大学学报 2024）
- [[raw/papers/xu2023-span-scitranslmed]] — S_pan 广谱新冠疫苗：基于S蛋白进化轨迹的系统发育共识抗原设计，跨进化支WT/Beta/Delta/Omicron保护，异源加强Omicron BA.1 100%保护（Zhao Y/Xu K, Sci Transl Med 2023）
- [[raw/articles/mRNA肿瘤个性化疫苗-从计算设计到临床转化]] — mRNA肿瘤个性化疫苗全流程综述：新抗原发现→计算设计→递送系统→临床试验（微信公众号·TGENES，马伯君，2025）
- [[raw/articles/Cell经典研究-Moderna寨卡病毒mRNA疫苗的设计]] — Moderna 寨卡病毒 mRNA 疫苗的设计原理：天然构象 vs 融合前构象，LNP 递送，C57BL/6 小鼠模型验证（微信公众号·耀菌君）
- [[raw/articles/利用可变剪接开发肝癌通用型mRNA新抗原疫苗-CellResearch2025]] — AS来源新抗原开发肝癌通用型mRNA疫苗，AS事件59x突变、50.94%覆盖率、TAPi抗原绕过62%TAP缺陷（Zhao H, Cell Res 2025，微信公众号·生信探索）
|- [[raw/articles/MsgaBpred-B细胞表位预测-AlphaFold3-GCN-ESM-C]] — MsgaBpred：整合AlphaFold3预测结构、多尺度GCN与ESM-C的B细胞表位预测模型，AUC↑2%/MCC↑5%，DeLong p=0.0212（微信公众号·习也无止境）
|- [[raw/articles/DFPred-B细胞线性表位预测模型-河北省科学院]] — DFPred：多深度学习算法融合的B细胞线性表位预测模型，CNN+单头注意力+xLSTM，ESM-2特征，AUC=0.806/AUC10%=0.420（河北省科学院学报）
|- [[raw/articles/Ca-DelMut新冠S蛋白缺失减毒株-NatureComms]] — Ca-DelMut 安全减毒株：S1/S2弗林切割位点缺失→低致病+强复制，鼻喷减毒活疫苗候选，灭菌式免疫保护（生信养成系，Nat Commun 2021）
- [[raw/articles/武汉大学-徐可教授-个人主页]] — 武汉大学徐可教授个人主页：研究方向（病毒/疫苗/抗病毒药物/DHODH抑制剂/重组蛋白疫苗），代表性论文索引
|- [[raw/articles/万能呼吸道疫苗-斯坦福Science-鼻喷广谱防护]] — GLA-3M-052-LS+OVA 鼻喷通用疫苗，整合器官免疫范式，3剂防护3个月，病毒/细菌/过敏原全覆盖（Zhang H, Science 2026，生物谷微信公众号）
|- [[raw/articles/科医诺生物-携手步长制药-共拓广谱流感疫苗]] — 科医诺生物 KNY0003 广谱流感疫苗管线：徐可团队原研，步长制药合作产业化，$98.5B流感疫苗市场，抗原理性再设计（科医诺生物微信公众号）
|- [[raw/articles/TCR-PRP-多肽识别谱-语言模型-T细胞功能预测-NatBiotechnol2026]] — TCR-PRP 多肽识别谱与 T 细胞功能预测：pLM 超越 AF3，发现 HLA-B*27:05 自身免疫新抗原（微信公众号·游离的DNA，Nat Biotechnol 2026）

### cancer-biology
|- [[raw/articles/南京医科大学-NatureMedicine-多癌早检-cfDNA片段组学]] — 南京医科大学 Nat Med MCED：cfDNA 片段组学13癌种早检，I期79.3%/特异性98.9%，TOO 82.3%，金陵队列3,724人，0-II期占93%（微信公众号·BioMed科技，Shao Y, Nat Med 2026）
|- [[raw/articles/NatureReviews-癌症筛查指南-五大癌种-AI-MCED]] — Nat Rev Clin Oncol 癌症筛查指南：五大癌种（乳腺/结直肠/宫颈/前列腺/肺癌）筛查方法+AI辅助+MCED补充（微信公众号·基因研究所，Duffy SW, Nat Rev Clin Oncol 2026）

### market-watch
|- [[raw/articles/Isomorphic-Labs-21亿美元-B轮-AI制药最大融资]] — Isomorphic Labs B轮$21B：AI制药最大单笔融资，Thrive Capital领投，Alphabet/MGX/淡马锡跟投（写意宣发微信公众号，2026/05）
|- [[raw/articles/Recursive-Superintelligence-AI4S-6.5亿美元-0产品估值46亿]] — Recursive Superintelligence $6.5亿融资，0产品估值$46.5亿，谷歌风投/Greycroft领投，AMD/NVIDIA参投；AI4S赛道融资对比表（智药局微信公众号，2026/05）
|- [[raw/articles/AI4S-四种生意三种赚法一条铁律-钛资本]] — AI4S赛道框架：感知行合四种生意，License-out/SaaS/IDM三种赚法，DBTL飞轮，商业化甜蜜点预测（钛资本研究院·蒋云川，2026）
|- [[raw/articles/奥明星程-AI4S-超亿元A轮-多组学多模态]] — 奥明星程超亿元A轮，哈佛博士团队，AI+多组学多模态，乳腺癌早筛 OS-TuFEst-BRCA 灵敏度92-95%（动脉网微信公众号，2026）
|- [[raw/articles/复鞍智能-AI4S物质科学-复旦科创种子轮]] — 复鞍智能种子轮，复旦刘智攀团队，AI4S催化材料分子反应研发（建研院微信公众号，2026）
### machine-learning
|||- [[raw/articles/Graphene-E-Nose-Respiratory-Infection-ZJU-ACS]] — 石墨烯电子鼻×机器学习区分细菌/病毒呼吸道感染，SVM/RF/Lasso加权融合模型，145例临床呼气样本+43例外部测试，AUC 0.87（ACS Nano 2025 浙江大学邬建敏，微信公众号·有机配体和荧光染料最新研究）
|||- [[raw/articles/随机森林+贝叶斯优化-深夜努力写Python]] — RF+BO 超参数自动调优：代理模型替换、EI采集函数、sklearn 完整代码案例与可视化（微信公众号·深夜努力写Python，cos大壮）
||- [[raw/articles/随机森林算法全面讲解-机器学习和人工智能AI]] — Random Forest 全面教程：Bagging + 随机特征选择、Gini/MSE分裂准则、复杂度分析、sklearn 代码案例、GridSearchCV 超参数调优（微信公众号·机器学习和人工智能AI）
|- [[raw/articles/SVM支持向量机算法详解-机器学习和人工智能AI]] — SVM 全面讲解：最大间隔超平面、核技巧（linear/RBF/poly）、软间隔 C 参数、sklearn iris 分类代码案例（微信公众号·机器学习和人工智能AI）
|- [[raw/articles/Transformer结合LSTM时序预测详解-机器学习和人工智能AI]] — Transformer+LSTM 时序融合：自注意力+多头注意力+位置编码、LSTM 门控机制、并行/级联融合策略、完整 PyTorch 代码与可视化（微信公众号·机器学习和人工智能AI）
|- [[raw/articles/跟着小白学机器学习-决策树]] — 决策树算法详解：基本原理/划分准则（信息增益/基尼不纯度/MSE）/过拟合与剪枝/分类与回归/sklearn iris代码示例（微信公众号·跟着小白一起机器学习，2023）
|- [[raw/articles/跟着小白学机器学习-逻辑回归]] — 逻辑回归算法详解：Sigmoid/交叉熵损失/L1&L2正则化/ROC-AUC/多分类OvR&Softmax/信用风险评估sklearn代码案例（微信公众号·跟着小白一起机器学习，2023）
|- [[raw/articles/跟着小白学机器学习-朴素贝叶斯]] — 朴素贝叶斯算法详解：贝叶斯定理/条件独立性假设/多项式NB/高斯NB/零概率平滑/文本分类sklearn代码案例（微信公众号·跟着小白一起机器学习，2023）
|- [[raw/articles/跟着小白学机器学习-线性回归]] — 线性回归算法详解：OLS最小二乘法/R平方/岭回归&Lasso/多重共线性/sklearn代码案例（微信公众号·跟着小白一起机器学习，2023）
|- [[raw/articles/跟着小白学机器学习-K均值聚类]] — K均值聚类算法详解：目标函数/算法步骤/肘部法/轮廓系数/Gap统计量/K值选择/sklearn代码案例（微信公众号·跟着小白一起机器学习，2023）
|- [[raw/articles/跟着小白学机器学习-支持向量机]] — 支持向量机算法详解：软硬间隔/核函数/超参数GridSearchCV/OvO&OvR多分类/SVR回归/sklearn完整代码与决策边界可视化（微信公众号·跟着小白一起机器学习，2023）
