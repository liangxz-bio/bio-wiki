---
source_url: https://mp.weixin.qq.com/s/（AI生信工坊公众号）
ingested: 2026-05-06
sha256: a721185bbed498b24ee1a07a3f65a99f81442ea49b6844dbdc6f9bb619a1d6ed
citation: "Levin. HLA检测生信算法工程师——面试核心问题与解答. AI生信工坊（微信公众号）. 2025."
domain: bioinfo-pipeline
---

# HLA检测生信算法工程师——面试核心问题与解答

> **作者**：Levin  
> **来源**：AI生信工坊 (微信公众号)  
> **日期**：参考数据截至 2025 年

---

## 一、HLA 基因的复杂性（面试高频基础题）

### Q1: 为什么 HLA 分型比常规基因分型难？

HLA 分型之所以困难，核心原因有五个：

1. **极端高多态性** — HLA 是人类基因组中多态性最高的区域。截至 2025 年，IPD-IMGT/HLA 数据库已收录超过 37000 个等位基因。以 HLA-B 为例，有超过 8000 个已知等位基因。参考基因组只代表一种单倍型，直接比对会丢失大量变异信息。传统比对到单一参考再 call variant 的策略不适用，需要基于等位基因库的比对策略。

2. **高度同源性** — HLA Class I 基因（A、B、C）之间以及 Class II 基因（DRB1、DQB1、DPB1 等）之间存在高度序列同源。特别是 DRB1/DRB3/DRB4/DRB5 之间，序列相似度极高。短读长 reads 容易 mismapping。

3. **相位问题（Phasing）** — 人有两条染色体，每个 HLA 基因座有两个等位基因。如果两个等位基因之间的变异距离超过 read/fragment 长度，就无法判断哪些变异在同一条单倍型上，这就是经典的相位模糊问题。

4. **拷贝数变异与基因缺失** — HLA 区域有大段 structural variation，部分个体可能缺失某个基因（如 HLA-DRB3/4/5 不是每个人都有），缺失等位基因（null allele）在临床上需要特别报告。

5. **伪基因干扰** — HLA 区域有多个伪基因（如 HLA-H、HLA-J、HLA-K），与功能基因高度相似，需要在 pipeline 中主动 mask 或识别。

### Q2: HLA 命名系统是怎样的？什么是高分辨分型？

HLA 命名遵循 IPD-IMGT/HLA 标准，以 HLA-A 02:01:01:01 为例：

- **第一域（02）**：等位基因组，对应血清学分型（低分辨 / 2-digit）
- **第二域（02:01）**：蛋白水平差异（高分辨 / 4-digit），临床移植配型的标准要求
- **第三域（02:01:01）**：同义突变
- **第四域（02:01:01:01）**：非编码区多态

JD 中提到的 HLA Class I/II 高分辨率分型，指的就是至少做到 two-field 水平的分型。

---

## 二、短读长（NGS/Illumina）HLA 分型技术问题

### Q3: 短读长 HLA 分型的主流算法策略有哪些？

**策略 1：基于等位基因库比对（Database matching）**

代表工具：OptiType（Class I 专用）、HLA-HD、Arcas-HLA。将 reads 直接比对到 IMGT/HLA 数据库中所有已知等位基因序列，通过统计模型选择最优的等位基因对。OptiType 使用 ILP（整数线性规划）选择最优覆盖。优点是简单高效，Class I 准确率可达大于 98%；缺点是依赖数据库完整性，无法发现新等位基因。

**策略 2：基于图比对（Graph-based）**

代表工具：vg + HLA typing、HISAT-genotype。构建包含已知变异的图参考（graph genome），提高多态区域 read 比对的准确性。优点是比线性参考比对更准确，可减少 reference bias；缺点是图的构建和更新较复杂。

**策略 3：基于 de novo 组装（Assembly-based）**

代表工具：HLA-VBSeq。局部组装 HLA 区域 reads，再与数据库比较。优点是理论上可以发现新等位基因；缺点是短读长的组装能力有限。

### Q4: 短读长 HLA 分型的置信度评估怎么做？

1. **分型置信度分数**：第一名和第二名候选等位基因之间的得分差（delta score），如果 delta score 很小，说明分型结果不确定。

2. **覆盖度指标**：各外显子（特别是 exon 2、exon 3 对 Class I，exon 2 对 Class II）的平均覆盖深度和覆盖均匀性。

3. **杂合度检查**：两个等位基因的 read 比例是否接近 50:50。如果严重偏离（如 80:20），可能是 allele dropout、PCR bias 或 null allele。

4. **模糊性报告**：G group / P group 模糊，即多个等位基因在抗原结合域序列完全相同，仅在其他区域不同。

### Q5: 短读长 HLA 分型的质控要点有哪些？

- **样本层面 QC**：总 read 数 / on-target rate、去重后有效 read 数、Q30 比例、交叉污染检测（如检查是否有大于等于 3 个等位基因信号）。
- **分型层面 QC**：分型得分是否通过阈值、是否有 novel allele 提示、exon 关键区域覆盖是否充分、等位基因 read balance 是否在可接受范围。
- **批次层面 QC**：阳性对照样本分型是否正确、阴性对照是否干净、批内重复性。

---

## 三、长读长（Nanopore/PacBio）HLA 分型技术问题

### Q6: 长读长做 HLA 分型的核心优势是什么？

1. **解决相位问题**：长读长可以直接 span 整个 HLA 基因（如 HLA-A 约 3.5kb coding region），单条 read 覆盖全基因，天然解决短读长的相位模糊问题。

2. **结构/同源复杂区域解析**：长 reads 可以 span 同源区域，明确区分 DRB1 vs DRB3/4/5，可以检测大的 structural variant。

3. **减少 PCR 偏好性**：如果采用无 PCR 或低 PCR 循环的建库方案，等位基因 read balance 更好。

### Q7: 长读长的错误模型是什么？怎么做纠错？

**Nanopore 错误特点**：原始读取错误率约 5-15%（R10.4.1 已降至约 3-5%），错误类型以 indel 为主（尤其同聚物 homopolymer 区域），substitution 较少，特定 k-mer context 下错误率可预测。

**PacBio HiFi 错误特点**：CCS 错误率可低至约 0.1-0.5%，错误类型更均匀，没有明显的 homopolymer bias。

**纠错策略**：

- a) **自纠错（Self-correction）**：利用同一等位基因的多条 reads 进行 consensus calling。工具如 Medaka（ONT）、DeepConsensus（PacBio）。流程：mapping - clustering by allele - consensus。
- b) **错误模型集成到分型算法中**：在似然函数中直接建模 error profile。对于 ONT 数据，在比较 read 与候选等位基因时，对 homopolymer 区域的 indel 赋予更高容忍度。
- c) **基于 POA 的 consensus**：将属于同一等位基因的 reads 做 multiple alignment，通过 POA graph 取 consensus。代表工具：SPOA、abPOA。

### Q8: 长读长 HLA 分型的一致性评估是指什么？

1. **内部一致性**：cluster 内部 reads 与 consensus 序列的一致性，如果高度不一致，提示 read 分配可能有误。

2. **跨平台一致性**：长读长分型结果 vs 短读长分型结果 vs SSO/SSP/SBT 传统方法的一致率，这是 IVD 验证中非常重要的指标。

3. **跨数据库版本一致性**：使用不同版本的 IMGT/HLA 数据库是否导致分型结果变化，pipeline 需要锁定数据库版本并在报告中注明。

---

## 四、红细胞血型基因分型

### Q9: 红细胞血型基因分型涉及哪些系统？

主要血型系统：

- **ABO**：最基础，但基因型到表型推断有复杂规则（如 cis-AB、Bw 弱表达）
- **Rh**：RHD / RHCE，极为复杂。RHD 有完整缺失（Rh 阴性 D-）、部分缺失（partial D）、弱表达（weak D）等。RHCE 有 C/c、E/e 抗原决定，以及 RHD-RHCE hybrid gene
- **Kell、Duffy（Fy）、Kidd（Jk）、MNS** 等：各有数十个等位基因
- 还有 Diego、Dombrock、Colton 等稀有血型系统

**与 HLA 的关键异同**：HLA 多态性极高（万级等位基因），血型中等（百级）。HLA 基因座集中在 6p21.3，血型基因分散在不同染色体。HLA 主要挑战是高同源性和相位模糊，血型主要挑战是基因转换、hybrid gene 和 null allele。

### Q10: 表型推断和特殊等位基因提示是什么意思？

**表型推断**：血型的临床意义在表型（抗原有或无），但 NGS 检测到的是基因型，需要建立 genotype 到 phenotype 的推断规则。例如检测到 RHD 基因有 exon 3-6 缺失，推断为 partial D（DVI 型），提示该供者红细胞可能引起 anti-D。

**特殊等位基因提示**：包括 Null allele（不表达抗原）、Weak/partial expression（弱表达，可能导致血清学误判）、罕见等位基因。这些信息必须在报告中明确标注，因为直接影响临床输血安全。

---

## 五、临床报告逻辑

### Q11: 临床 HLA 分型报告需要包含哪些内容？

1. **规则引擎**：报告生成是一个基于规则的判断系统。质控失败时报告建议复检；复检触发条件包括置信度低、read balance 异常、交叉污染信号、novel allele 提示；无法区分 G group 内等位基因时报告 G group 名称。

2. **结果解释**：将分型结果翻译为临床可理解的语言，如 HLA-B 57:01 提示 Abacavir 超敏反应风险。

3. **与 LIS/HIS 对接**：报告需输出标准化字段（样本 ID、基因座、等位基因、置信度、QC 状态等），格式通常为 HL7 或自定义 JSON/XML。

---

## 六、验证体系（IVD 验证关键点）

### Q12: HLA 分型 pipeline 的验证体系需要哪些实验？

- **准确性**：用已知分型的标准品（如 IHWG reference panel）验证，计算 concordance rate
- **一致性**：同一批样本在不同批次/操作员/仪器条件下重复检测
- **重复性**：同一样本在同一批次内做 N 次重复
- **极限（LoD）**：梯度稀释实验，找到分型失败的临界 DNA 输入量
- **抗干扰**：加入血红蛋白、胆红素、脂血等干扰物，看分型是否受影响
- **交叉污染**：高浓度与阴性样本交替排列，检查阴性样本是否出现阳性信号

金标准可以是 SBT（Sanger 测序）或 IHWG 参考样本，需要覆盖常见型别、罕见型别以及 null allele 等特殊情况。

---

## 七、数据合规与可交付软件

### Q13: 可重复计算和版本管理意味着什么？

**可重复计算**：同一输入 + 同一软件版本 + 同一参数 = 完全相同的输出。要求随机种子可固定、IMGT/HLA 数据库版本锁定、Docker/Singularity 容器化。

**版本管理**：软件版本号遵循 SemVer，数据库版本记录在输出中，需评估 database upgrade 对历史结果的影响。

**审计日志**：IVD 合规要求每一步操作可追溯（谁、什么时间、什么版本、什么输入、什么输出），这是 SaMD 注册的基本要求。

### Q14: 能把算法落成可交付软件具体指什么？

- **容错**：输入异常时不崩溃，给出有意义的错误信息
- **日志**：运行过程可追踪，每一步有时间戳和状态记录
- **可配置**：参数可通过配置文件调整，不需要改代码
- **可验证**：有单元测试、集成测试、回归测试，CI/CD 流程

---

## 八、面试延伸问题

### Q15: 常见 HLA 分型工具对比

| 工具 | 平台 | 算法/特点 | 备注 |
|---|---|---|---|
| OptiType | NGS | ILP 优化 | Class I 准确率极高，但不支持 Class II |
| HLA-HD | NGS | 支持 Class I + II | 对低覆盖敏感 |
| Arcas-HLA | NGS/RNA-seq | Kallisto 伪比对 | 快速 |
| HISAT-genotype | NGS | 图基因组 | 比对准确但搭建复杂 |
| HLA-LA | 长/短读长 | — | 计算资源需求大 |
| T1K | — | 基于 EM 算法 | 灵活但相对较新 |

### Q16: 靶向测序与 WGS/WES 做 HLA 分型有什么区别？

- **Amplicon 方案**：设计引物扩增关键外显子，覆盖深、成本低，但有 allele dropout 风险。
- **Capture 方案**：探针捕获整个 HLA 区域，覆盖更全面，allele dropout 风险低于 amplicon，但成本较高。
- **WGS/WES**：不专门针对 HLA，一次测序多用途，但 HLA 区域覆盖深度可能不够。
