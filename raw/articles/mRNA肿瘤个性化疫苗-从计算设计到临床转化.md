---
source_url: https://mp.weixin.qq.com/s/Zux-lOGg_uZ82cOFsfdviQ
ingested: 2026-05-09
sha256: 2c81666a1be218540cc96b36e036b84f9f45f1eb5392f480d46346aa9718e013
citation: "马伯君. mRNA肿瘤个性化疫苗：从计算设计到临床转化的深度范式革新. TGENES 微信公众号. 2025"
domain: vaccine-design
---

# mRNA肿瘤个性化疫苗：从计算设计到临床转化的深度范式革新

> 来源：微信公众号「TGENES」（作者：马伯君）
> 原文链接：https://mp.weixin.qq.com/s/Zux-lOGg_uZ82cOFsfdviQ

---

肿瘤免疫治疗的演进正处于从"通用型"向"个体化"跨越的关键节点。在这一进程中，信使核糖核酸（mRNA）肿瘤个性化疫苗凭借其独特的免疫激活机制、灵活的设计模组以及高效的生产潜力，已成为精准医疗的前沿阵地。不同于针对病原体的预防性疫苗，肿瘤个性化疫苗属于治疗性手段，其核心在于识别并利用患者肿瘤组织特有的体细胞突变——即新抗原（Neoantigens），以此激发患者自身的免疫系统产生特异性的抗肿瘤反应¹。随着高通量测序技术、生物信息学算法以及递送系统的协同进步，该领域已在黑色素瘤、胰腺癌和非小细胞肺癌等多个瘤种中取得了令人瞩目的临床突破，并逐步重塑肿瘤辅助治疗的格局³。

---

## 第一章 mRNA个性化疫苗的免疫生物学基石

mRNA是个性化肿瘤疫苗的理想载体，其作用机制在于模拟病毒感染过程，将编码肿瘤新抗原的遗传信息直接递送至宿主细胞胞浆。在抗原呈递细胞（APCs），尤其是树突状细胞（DCs）中，这些外源性mRNA利用宿主的蛋白质合成机器转化为新抗原蛋白，随后进入内源性抗原处理路径¹。

### 1.1 抗原处理与呈递的双重激活

mRNA疫苗引发免疫反应的关键在于其能够同时激活适应性免疫的两个臂：细胞免疫与体液免疫。当负载mRNA的脂质纳米颗粒（LNPs）进入细胞后，mRNA经翻译产生蛋白，随后由蛋白酶体降解为多肽，并通过内质网负载至主要组织相容性复合体（MHC）I类分子上，呈递给CD8+细胞毒性T淋巴细胞¹。这种直接的内源性呈递路径是诱导高效抗肿瘤T细胞反应的基础。

与此同时，通过优化mRNA序列使其编码分泌型蛋白，或者通过DC细胞的胞吞作用，抗原也可以经由MHC II类分子路径呈递给CD4+辅助性T细胞¹。CD4+ T细胞的激活对于维持免疫记忆、促进B细胞产生抗体以及防止CD8+ T细胞耗竭至关重要³。这种多组分的免疫激活模式，使得mRNA疫苗在构建肿瘤免疫微环境方面具有显著优势。

### 1.2 内源性佐剂效应与核苷酸修饰

mRNA分子本身具有内在的免疫刺激特性。未经修饰的单链RNA可以通过内吞体途径被Toll样受体（如TLR3、TLR7、TLR8）识别，触发I型干扰素的分泌，从而起到天然佐剂的作用¹。然而，过强的先天免疫反应可能抑制mRNA的翻译效率。因此，现代mRNA平台多采用核苷酸修饰（如假尿苷取代）来平衡免疫原性与翻译效率，确保在诱导免疫激活的同时，产生足量的目标抗原⁶。

### 表1：mRNA个性化疫苗与传统肿瘤疗法的综合对比分析¹

| 特性 | mRNA个性化疫苗 | 传统化疗/放疗 | 通用型免疫检查点抑制剂 (ICIs) |
|------|-----------------|-------------|--------------------------|
| 靶向性 | 极高，针对患者特有突变³ | 低，针对快速分裂细胞 | 中，通过逆转免疫抑制非特异性激活 |
| 毒副作用 | 较小，主要为注射部位反应⁸ | 较大，存在全身性毒性 | 中，可能引发免疫相关不良反应 (irAEs) |
| 免疫记忆 | 能够诱导长效记忆T细胞⁴ | 无 | 间接诱导 |
| 生产逻辑 | 按需定制，周期约6-9周⁹ | 标准化规模生产 | 标准化规模生产 |

---

## 第二章 肿瘤新抗原：个性化设计的"靶标"

新抗原是个性化疫苗的灵魂。它们是由肿瘤细胞特有的体细胞突变（如点突变、插入/缺失、基因融合和选择性剪接）产生的异常肽段，在正常组织中完全不表达¹⁰。这种"非我"属性使得新抗原能够逃避胸腺的中心耐受，从而诱导出更高亲和力的T细胞反应¹⁰。

### 2.1 突变景观的识别与多组学整合

个性化疫苗的开发始于对患者肿瘤突变谱的精确描绘。通过对肿瘤活检组织和外周血进行全外显子测序（WES）和全基因组测序（WGS），研究人员可以识别出所有的体细胞突变⁵。然而，并非所有的基因突变都能转化成免疫系统可识别的信号。RNA测序（RNA-seq）在此环节扮演着验证者的角色，它确证了突变基因是否在转录水平上得到了表达，并能捕捉到由选择性剪接产生的新异构体³。

### 2.2 免疫原性预测的计算挑战

在一个典型的肿瘤患者中，尽管可能识别出成百上千个突变，但通常只有极少数（通常少于10%）具有真正的免疫原性²。预测哪些突变肽段能以高亲和力结合患者特有的HLA（人类白细胞抗原）分子，是生物信息学领域的巨大挑战³。传统的亲和力预测模型（如NetMHCpan）虽然在识别HLA结合肽方面取得了一定成就，但研究表明，单纯的结合亲和力（BA）并不能完全预测免疫反应的发生——即所谓的"亲和力陷阱"¹²。

为了克服这一局限，新一代AI模型如NeoImmunoML正在转向基于机制的建模。这些模型综合考虑了肽段的理化性质（如疏水性、长度）、蛋白酶体剪切效率以及转运蛋白（TAP）的转运效率¹²。通过整合免疫肽组学和大规模质谱（MS）数据，AI算法能够更精准地模拟抗原呈递的复杂过程¹¹。

---

## 第三章 算法驱动的序列优化：LinearDesign范式

在mRNA疫苗的设计空间中，同义密码子的存在导致了天文数字般的候选序列组合。例如，编码SARS-CoV-2刺突蛋白的mRNA序列可能多达极大数量种¹⁶。如何在如此庞大的空间中找到最稳定、翻译效率最高且免疫原性最强的序列，已成为行业竞争的技术高地。百度研究院开发的LinearDesign算法为此提供了突破性的解决方案¹⁶。

### 3.1 语言学启发：格解析与DFA理论

LinearDesign的创新之处在于将mRNA序列优化问题转化为计算语言学中的格解析（Lattice Parsing）问题¹⁷。算法利用确定性有限状态自动机（DFA）来紧凑地表示所有可能的同义mRNA候选序列。这一模型类比于自然语言处理中的"词格"，算法的目标是在格中寻找"最合理的句子"——即最优的mRNA序列¹⁷。

在具体操作上，LinearDesign采用随机上下文无关文法（SCFG）来模拟RNA的热力学折叠能量模型¹⁷。通过将SCFG与DFA进行交集运算（Bar-Hillel构造），算法可以同时优化结构稳定性（最小自由能，MFE）和密码子适应性（CAI）¹⁷。这种联合优化策略通过动态规划算法实现，其计算复杂度随序列长度呈平方增长，极大地提高了设计效率¹⁷。

> **优化目标公式：** 其中 λ 为平衡结构稳定性和密码子偏好性的超参数¹⁷。

### 3.2 稳定性对免疫应答的放大作用

序列优化的生物学意义直接体现在疫苗的效力上。LinearDesign设计的序列通常具有更高的二级结构稳定性，这延长了mRNA在体内的半衰期，并减少了环状区域的降解¹⁶。体外和体内实验均证实，LinearDesign优化的序列在蛋白质表达水平上显著优于传统密码子优化方法，并在小鼠实验中实现了高达**128倍**的抗体滴度提升¹⁶。

这种精准的数学模型为个性化疫苗的标准化设计提供了可靠的底层技术支撑。

---

## 第四章 临床进展与治疗里程碑

个性化mRNA疫苗从理论走向实践的关键阶段。目前，Moderna/默克联合开发的mRNA-4157（V940）以及BioNTech的iNeST平台（如BNT122）代表了该领域的最高临床水平。

### 4.1 黑色素瘤的突破：KEYNOTE-942研究

mRNA-4157是一种编码多达34个新抗原的个性化疗法，目前正与PD-1抑制剂帕博利珠单抗（Keytruda）联合使用⁸。在针对高危、已手术切除的III/IV期黑色素瘤患者的Phase 2b研究（KEYNOTE-942）中，联合疗法表现出了显著的临床获益⁸。

根据5年随访数据的预分析显示，与帕博利珠单抗单药治疗相比：
- 联合接种mRNA-4157的患者**复发或死亡风险降低了49%**（HR=0.510）
- **远端转移或死亡风险降低了62%**（HR=0.384）¹⁹

这一结果证明了个性化新抗原疗法能够诱导持久的抗肿瘤免疫监测，并在手术后的辅助治疗背景下显著改善患者预后⁴。

### 4.2 胰腺癌与结直肠癌：挑战与机遇并存

BioNTech在胰腺导管腺癌（PDAC）领域的研究显示，接种其个性化mRNA疫苗后，患者体内诱导出的新抗原特异性T细胞反应可维持**近四年之久**⁴。考虑到胰腺癌极高的致死率和对常规免疫疗法的抗性，这一突破被视为该领域的重大进展⁴。

然而，在结直肠癌（CRC）领域，BioNTech与罗氏合作的BNT122（autogene cevumeran）在Phase 2试验（BNT122-01）的首次中期分析中**跨越了预设的无效性边界（Futility Boundary）**²¹。尽管研究并未因此终止，且正在继续盲态运行直至最终分析，但这一挫折凸显了针对不同微环境肿瘤设计个性化疫苗的复杂性²¹。结直肠癌的微环境通常较为"冷"，如何通过联合用药或改进递送系统来逆转免疫抑制，仍是个性化疫苗面临的重要课题⁴。

### 表2：个性化mRNA肿瘤疫苗主要临床项目概览⁴

| 候选药物 | 合作伙伴 | 主要适应症 | 临床阶段 | 核心结果/现状 |
|---------|----------|------------|---------|-------------|
| mRNA-4157 (V940) | Moderna / 默克 | 黑色素瘤 (辅助治疗) | Phase 3¹⁹ | 5年复发风险降低49%¹⁹ |
| mRNA-4157 (V940) | Moderna / 默克 | 非小细胞肺癌 (NSCLC) | Phase 3¹⁹ | 正在招募中¹⁹ |
| BNT122 (Autogene cevumeran) | BioNTech / 罗氏 | 胰腺癌 (PDAC) | Phase 2²¹ | 诱导长效T细胞反应⁴ |
| BNT122 (Autogene cevumeran) | BioNTech / 罗氏 | 结直肠癌 (CRC) | Phase 2²¹ | 中期分析跨越无效边界²¹ |
| BNT122 (Autogene cevumeran) | BioNTech / 罗氏 | 膀胱癌 (MIUC) | Phase 2²² | 首例患者已入组²² |

---

## 第五章 柔性制造与供应链工程

个性化mRNA疫苗的生产逻辑从根本上颠覆了传统的制药流程。不同于一次生产数百万剂的模式，个性化疫苗需要为每一位患者单独设计、合成并包装²³。

### 5.1 从活检到注射：受限的时间窗口

在临床实践中，患者往往处于疾病进展的压力下，这要求疫苗的生产周期必须尽可能压缩，通常目标设定在**6至9周**⁹。生产流水线包括：

1. **样本采集与测序：** 高质量的肿瘤和血液样本是起点¹⁰。
2. **新抗原筛选与序列优化：** 利用AI算法（如LinearDesign）进行秒级优化¹⁶。
3. **DNA模板制备：** 这是目前的瓶颈，传统质粒DNA发酵可能需要数月²⁵。
4. **体外转录 (IVT)：** 利用T7 RNA聚合酶在无细胞系统中合成mRNA¹。
5. **纯化与封装：** 去除dsRNA副产物，并利用微流控技术封装进LNP²³。

### 5.2 制造瓶颈与成本控制

高昂的生产成本（**每位患者可能超过10万美元**）是阻碍其大规模应用的主要障碍⁴。制造中的瓶颈包括：

- **GMP级别原材料：** 高质量的质粒DNA和酶供不应求²⁵。
- **冷链物流：** mRNA-LNP的不稳定性要求在特定低温环境下存储和运输²⁵。
- **规模化与个体化的平衡：** 尽管单个疫苗剂量极小（毫升量级），但需要构建高度自动化、可并行处理数千个订单的"分布式工厂"²⁴。

---

## 第六章 监管范式的重构：PMP路径

传统药物审批路径要求针对特定配方进行大规模随机对照试验，但这对于成分随患者变化的个性化疫苗而言极具挑战。2025年11月，美国FDA提出的"合理解制路径"（**Plausible Mechanism Pathway, PMP**）为该领域带来了监管层面的重大突破²⁷。

### 6.1 PMP的核心原则

PMP路径允许基于"平台数据"而非单一产品数据进行审批。其核心逻辑在于，如果某种mRNA骨架和LNP递送系统已被证明是安全且能有效表达抗原的，那么针对不同患者更换其中的"遗传荷载"不应被视为全新的药物，而应被视为该平台下的个性化应用²⁹。

**PMP的五大准则包括：**

1. **明确的分子基础：** 疾病必须有明确的基因或分子突变基础²⁹。
2. **直接靶向机制：** 疗法必须直接针对该突变发挥作用³¹。
3. **完善的自然病史数据：** 用以对比治疗后的临床改善²⁸。
4. **靶向参与的证据：** 通过生物标志物或活检证明目标已被成功"编辑"或激活³¹。
5. **显著的临床改善：** 甚至在极小规模的患者群体（如连续几个案例）中观察到的持久疗效即可支持获益²⁸。

### 6.2 案例启示："Baby KJ"

FDA Commissioner Marty Makary引用的"Baby KJ"案例展示了这一路径的潜力：一名患有罕见CPS1缺陷的婴儿通过基于mRNA的基因编辑疗法获得了临床改善。尽管无法进行大规模试验，但由于机制明确且自然病史预后极差，单例成功为该类个性化疗法的合规化提供了实证基础²⁷。

---

## 第七章 未来展望与战略洞察

mRNA个性化肿瘤疫苗正进入临床应用的爆发期，但也面临着深层次的科学与市场挑战。

### 7.1 MRD监测与早期介入

未来的临床趋势正向微小残留病灶（MRD）治疗倾斜。利用循环肿瘤DNA（ctDNA）监测术后复发风险，并在影像学观察到转移之前介入个性化疫苗，被认为是个性化疫苗发挥最佳效力的窗口³³。KEYNOTE-942的成功很大程度上归功于其在辅助治疗背景下的早期干预⁴。

### 7.2 克服"冷"肿瘤的异质性

对于免疫浸润较少的肿瘤，未来的策略将集中在联合用药。除了传统的PD-1抑制剂，新型免疫调节剂（如双特异性抗体、溶瘤病毒）可能被用于预热微环境，为疫苗诱导的T细胞创造更好的战斗条件³⁶。同时，识别由非典型突变（如非编码区突变、隐匿性剪接位点）产生的新抗原，将进一步扩大疫苗的适用人群³。

### 7.3 市场竞争与全球格局

随着Moderna和BioNTech相继计划在**2026-2029年**间推出首款商用肿瘤疫苗，全球市场正处于整合阶段⁴。亚太地区（尤其是中国）凭借强大的生信算法能力（如百度的LinearDesign）和不断增长的医疗支出，正成为该领域的重要增长点⁶。

---

## 结论

mRNA个性化肿瘤疫苗不仅是生物技术的革新，更是肿瘤治疗逻辑的根本性重塑。从计算语言学赋能的序列优化，到多组学驱动的新抗原识别，再到监管层面对"平台化准入"的认可，每一环节的进步都在缩短从实验室发现到患者床边的距离。尽管结直肠癌等复杂瘤种的挑战依然存在，但黑色素瘤和胰腺癌的临床数据已充分证明，通过精准编码免疫系统的攻击目标，我们正在步入一个癌症复发风险可控、甚至可以被"功能性治愈"的新纪元。个性化，不仅是医学的未来，更是每一位肿瘤患者获得长效生存的关键所在。

---

## 参考文献

1. mRNA-Based Personalized Cancer Vaccines: Opportunities, Challenges and Outcomes — https://pmc.ncbi.nlm.nih.gov/articles/PMC12755870/
2. Top 8 Takeaways on mRNA Cancer Vaccines and Personalized Immunotherapy Development - Binaytara — https://binaytara.org/cancernews/article/top-8-takeaways-on-m-rna-cancer-vaccines
3. Advancements and challenges in personalized neoantigen-based cancer vaccines — https://www.frontiersin.org/journals/oncology-reviews/articles/10.3389/or.2025.1541326/full
4. Current Progress and Future Perspectives of RNA-Based Cancer Vaccines: A 2025 Update — https://pmc.ncbi.nlm.nih.gov/articles/PMC12153701/
5. Advances and challenges in neoantigen prediction for cancer immunotherapy - Frontiers — https://www.frontiersin.org/journals/immunology/articles/10.3389/fimmu.2025.1617654/full
6. Baidu, Sanofi team to use AI to find mRNA vaccine candidates | AI Business — https://aibusiness.com/verticals/baidu-sanofi-team-to-use-ai-to-find-mrna-vaccine-candidates
7. MHRA seeks input on new regulatory guidance for cancer vaccines — https://www.pharmaceutical-technology.com/news/mhra-seeks-input-on-new-regulatory-guidance-for-cancer-vaccines/
8. Moderna and Merck Announce mRNA-4157 (V940) in Combination with KEYTRUDA® (pembrolizumab) Demonstrated Continued Improvement in Recurrence-Free Survival and Distant Metastasis-Free Survival in Patients with High-Risk Stage III/IV Melanoma Following Complete Resection Versus KEYTRUDA At Three Years — https://www.merck.com/news/moderna-and-merck-announce-mrna-4157-v940-in-combination-with-keytruda-pembrolizumab-demonstrated-continued-improvement-in-recurrence-free-survival-and-distant-metastasis-free-survival-in-pa/
9. Adjuvant mRNA-4157 Plus Pembrolizumab Prolongs RFS versus Pembrolizumab Alone in Patients with Resected High-Risk Melanoma | ESMO — https://www.esmo.org/oncology-news/adjuvant-mrna-4157-plus-pembrolizumab-prolongs-rfs-versus-pembrolizumab-alone-in-patients-with-resected-high-risk-melanoma
10. Neoantigen Personalized Cancer Vaccines - an overview - Intavis Peptide Services — https://intavispeptides.com/en/insights/8
11. Advances and challenges in neoantigen prediction for cancer immunotherapy - PMC - NIH — https://pmc.ncbi.nlm.nih.gov/articles/PMC12198247/
12. NeoImmunoML: a machine learning-based prediction model for human tumor neoantigen immunogenicity - PMC — https://pmc.ncbi.nlm.nih.gov/articles/PMC12585993/
13. Messenger RNA (mRNA)-Based, Personalized Cancer Vaccine Against Neoantigens Expressed by the Autologous Cancer - NCI — https://www.cancer.gov/research/participate/clinical-trials-search/v?id=NCI-2018-03656
14. OmniNeo: a multi-omics pipeline incorporating proteomics and AI selection for neoantigen optimization in tumor immunotherapy - Frontiers — https://www.frontiersin.org/journals/immunology/articles/10.3389/fimmu.2025.1727642/full
15. OncoMHC: a machine learning solution for UCEC tumor vaccine development through enhanced peptide-MHC binding prediction - Frontiers — https://www.frontiersin.org/journals/immunology/articles/10.3389/fimmu.2025.1550252/full
16. New AI algorithm boosts COVID-19 mRNA vaccine antibody response by 128 times — https://www.eurekalert.org/news-releases/987999
17. Algorithm for optimized mRNA design improves stability and immunogenicity — https://pmc.ncbi.nlm.nih.gov/articles/PMC10499610/
18. Moderna & Merck Announce 3-Year Data For mRNA-4157 (V940) in Combination With KEYTRUDA® (pembrolizumab) Demonstrated Sustained Improvement in Recurrence-Free Survival & Distant Metastasis-Free Survival Versus KEYTRUDA in Patients With High-Risk Stage III/IV Melanoma Following Complete Resection — https://www.merck.com/news/moderna-merck-announce-3-year-data-for-mrna-4157-v940-in-combination-with-keytruda-pembrolizumab-demonstrated-sustained-improvement-in-recurrence-free-survival-distant-metastasis-free-su/
19. Moderna & Merck Announce 5-Year Data for Intismeran Autogene in Combination With KEYTRUDA® (pembrolizumab) Demonstrated Sustained Improvement in the Primary Endpoint of Recurrence-Free Survival in Patients With High-Risk Stage III/IV Melanoma Following Complete Resection — https://www.merck.com/news/moderna-merck-announce-5-year-data-for-intismeran-autogene-in-combination-with-keytruda-pembrolizumab-demonstrated-sustained-improvement-in-the-primary-endpoint-of-recurrence-free-survival-i/
20. Publications - BioNTech — https://www.biontech.com/int/en/home/research-and-innovation/publications.html
21. Bad omens for BioNTech & Roche's neoantigen project — https://www.oncologypipeline.com/apexonco/bad-omens-biontech-roches-neoantigen-project
22. BioNTech Provides Business and Pipeline Updates at 43rd Annual J.P. Morgan Healthcare Conference — https://investors.biontech.de/news-releases/news-release-details/biontech-provides-business-and-pipeline-updates-43rd-annual-jp/
23. mRNA vaccines manufacturing: Challenges and bottlenecks - PMC — https://pmc.ncbi.nlm.nih.gov/articles/PMC7987532/
24. Enabling mRNA Therapeutics: Current Landscape and Challenges in Manufacturing — https://pmc.ncbi.nlm.nih.gov/articles/PMC10604719/
25. Overcoming Bottlenecks in mRNA Manufacturing - Pharma's Almanac — https://www.pharmasalmanac.com/articles/overcoming-bottlenecks-in-mrna-manufacturing
26. Five Challenges and Solutions in the mRNA Vaccine Manufacturing Process - PPD — https://www.ppd.com/blog/five-challenges-solutions-mrna-manufacturing-process/
27. Alliance for mRNA Medicines Welcomes New FDA Framework to Accelerate Development of Personalized Gene Therapies — https://mrnamedicines.org/alliance-for-mrna-medicines-welcomes-new-fda-framework-to-accelerate-development-of-personalized-gene-therapies/
28. FDA's New Plausible Mechanism Pathway: How It Compares to Existing Approval Routes - Avance Biosciences — https://www.avancebio.com/fdas-new-plausible-mechanism-pathway-how-it-compares-to-existing-approval-routes/
29. FDA Outlines "Plausible Mechanism" Approval Pathway for Personalized Therapies, But Significant Questions Remain | Insights | Ropes & Gray LLP — https://www.ropesgray.com/en/insights/alerts/2025/11/fda-outlines-plausible-mechanism-approval-pathway-for-personalized-therapies-but-significant
30. FDA's new 'plausible mechanism pathway': Transforming regulatory approval for personalized gene therapies | Parexel — https://www.parexel.com/insights/blog/fdas-new-plausible-mechanism-pathway-transforming-regulatory-approval-for-personalized-gene-therapies
31. FDA's plausible mechanism pathway to advance personalized therapies — https://becarispublishing.com/digital-content/blog-post/fda-s-plausible-mechanism-pathway-advance-personalized-therapies
32. FDA announces "plausible mechanism" approval pathway for certain personalized therapies, with few details - Hogan Lovells — https://www.hoganlovells.com/en/publications/fda-announces-plausible-mechanism-approval-pathway-for-certain-personalized-therapies-with-few-detai
33. A Phase II Clinical Trial Comparing the Efficacy of RO7198457 Versus Watchful Waiting in Patients With ctDNA-positive, Resected Stage II (High Risk) and Stage III Colorectal Cancer - larvol clin — https://clin.larvol.com/trial-detail/NCT04486378
34. BioNTech to Present Clinical Data Updates for Next-Generation Immunotherapy Candidates at the ASCO Annual Meeting 2024 — https://investors.biontech.de/news-releases/news-release-details/biontech-present-clinical-data-updates-next-generation/
35. A Phase II Clinical Trial Comparing the Efficacy of RO7198457 Versus Watchful Waiting in Patients With ctDNA-positive, Resected Stage II (High Risk) and Stage III Colorectal Cancer, Trial ID BNT122-01 | BioNTech — https://clinicaltrials.biontech.com/trials/BNT122-01
36. BioNTech to Present Clinical Data Updates Across mRNA and Immunomodulatory Oncology Portfolio at ESMO Congress 2024 — https://investors.biontech.de/news-releases/news-release-details/biontech-present-clinical-data-updates-across-mrna-and/
37. JPM26: BioNTech gears up for multiproduct oncology status by 2030 - Clinical Trials Arena — https://www.clinicaltrialsarena.com/news/jpm26-biontech-gears-up-for-multiproduct-oncology-status-by-2030/
38. The Sanofi pipeline in 2025: Is the play-to-win strategy working? - Labiotech.eu — https://www.labiotech.eu/in-depth/sanofi-pipeline-2025/
39. Personalized Cancer Vaccine Market Size & Share, 2025-2034 - Global Market Insights — https://www.gminsights.com/industry-analysis/personalized-cancer-vaccine-market

*END*
