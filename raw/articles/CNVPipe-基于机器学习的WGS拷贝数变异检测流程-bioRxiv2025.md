---
source_url: https://mp.weixin.qq.com/s/A9Qj-sRVZsLL8pZlbsqd8g
ingested: 2026-05-11
sha256: f9043e5f762531b50bda03c35fff89f3d2d7a8eb47fefc13589b316a413d3df3
citation: "Sun J, Wong NK, Jiang Z, et al. CNVPipe: An enhanced pipeline for accurate analysis of copy number variation from whole-genome sequencing. bioRxiv 2025. doi:10.1101/2025.05.18.654763"
domain: bioinfo-pipeline
---

# CNVPipe：基于机器学习的增强型WGS拷贝数变异检测流程

> **作者：** 麦克柴（我爱学生信）
> **来源：** 微信公众号「我爱学生信」
> **原文：** https://mp.weixin.qq.com/s/A9Qj-sRVZsLL8pZlbsqd8g
> **论文：** bioRxiv 2025, DOI: https://doi.org/10.1101/2025.05.18.654763
> **GitHub：** https://github.com/sunjh22/CNVPipe

---

## 一、研究背景

拷贝数变异（Copy Number Variations, CNVs）是结构变异（SVs）的一种重要类型，指的是基因组中大片段（通常>1kb）的拷贝数在个体间存在差异。CNV在多种疾病中发挥重要作用：

- **肿瘤发生发展**：CNV通过改变基因剂量和表达水平，促进肿瘤复杂性的形成
- **神经精神疾病**：如自闭症谱系障碍（ASD）和精神分裂症，特定区域的重复或缺失可破坏基因功能
- **发育障碍**：CNV与多种先天性异常密切相关

WGS相比传统的细胞遗传学和芯片技术，提供了更高的分辨率和全基因组范围的检测能力。然而，现有的CNV检测工具存在明显局限性：

1. **高假发现率**：这是最突出的问题
2. **方法单一**：不同工具基于不同原理（读深、分裂读、读对、组装），各有优劣
3. **缺乏整合**：很少有流程整合多种工具并优化合并策略
4. **忽略下游分析**：缺少上游预处理和下游优先级排序、注释和致病性评估

---

## 二、CNVPipe工作流程

CNVPipe是一个基于Snakemake框架构建的统一计算流程，旨在简化和自动化CNV分析的整个流程。

### 1. 核心架构

CNVPipe包含多个关键阶段：

#### （1）数据预处理

- 使用 **fastp** 去除接头污染和低质量读段
- 使用 **BWA** 将读段比对到参考基因组
- 使用 **GATK4** 进行碱基质量重校准和重复标记

#### （2）确定最佳bin大小

根据测序深度确定合适的bin大小（分辨率）：

- CNVPipe使用加权中位数方法估计分辨率
- 确保每个bin覆盖15,000个核苷酸碱基
- 对于双端150bp测序数据，相当于每个bin约100条读段

#### （3）并行CNV检测

CNVPipe整合了5个广泛使用的CNV检测工具，每个工具利用不同的CNV信号：

| 工具 | 方法类型 | 原理 |
|------|----------|------|
| **CNVKit** | 测序深度 | 使用滚动中位数方法校正GC含量和重复区域 |
| **CNVpytor** | 测序深度 | 应用均值漂移算法进行bin分割，使用t检验识别高置信度CNV |
| **cn.MOPS** | 测序深度 | 使用泊松分布混合模型，跨样本和染色体推断CNV |
| **Delly** | 配对reads | 顺序分析不兼容的配对端读段和分裂读段 |
| **Smoove** | 配对reads | 整合Lumpy等工具，简化和加速短读数据CNV检测 |

#### （4）SNP calling（可选）

对于测序深度>5×的样本：

- 使用 **GATK4 HaplotypeCaller**（深度>10×）
- 或使用 **Freebayes**（深度较低）
- 基于B等位基因频率（BAF）优化和校正CNV调用

#### （5）深度感知的CNV合并策略

这是CNVPipe的核心创新之一：

- **低深度数据（≤5×）**：强调读深方法
- **高深度数据（>5×）**：优先考虑分裂读和读对方法
- 合并规则：当CNV的相互重叠>0.75时递归合并
- 合并后的CNV使用读深方法估计拷贝数

---

### 2. 机器学习过滤

CNVPipe计算多个评分指标来细化CNV调用：

- 相邻读深模式
- SNP等位基因频率
- 与重复或低复杂度区域的重叠
- CNV大小
- GC含量

这些指标输入到**支持向量机（SVM）分类器**，有效过滤假阳性。用户可以提供参考集或模拟数据来训练分类器。

---

### 3. 结果可视化和报告

CNVPipe生成综合报告，包括：

- 精炼的CNV调用列表
- 覆盖度和BAF图
- 支持证据汇总
- 致病性预测（基于2019 ACMG标准）

---

### 4. 单细胞CNV分析模块

CNVPipe还提供专门的单细胞WGS分析模块，工作流程包括：

1. 对稀疏读段进行分箱（binning）
2. GC标准化
3. 合并连续的bin
4. **倍性估计**：使用最小二乘舍入和峰值逼近策略准确估计单细胞的倍性
5. 生成整数拷贝数谱
6. 构建单细胞拷贝数矩阵用于下游分析

---

## 三、性能评估

### 1. 模拟数据评估

研究者模拟了24个数据集，涵盖：

- 4个CNV大小范围：10 kb、50 kb、200 kb、1 Mb
- 6个测序深度：0.1×、0.5×、1×、5×、10×、30×
- 每个条件6个重复

**主要发现：**

1. **检测率随深度增加**：特别是对于较大的CNV（200 kb到1 Mb）
2. **方法互补性**：
   - 读深方法在低深度下表现更好
   - 分裂读和读对工具在高深度下精确映射小事件
   - 在极低覆盖度（0.1×）时失败
3. **集成方法的优势**：CNVPipe在所有大小范围和深度下都显示出更高的灵敏度和更低的FDR
4. **CNVPipe-SVM表现最佳**：在最小化虚假调用方面领先

### 2. 真实数据验证

使用具有已知结构变异/CNV的真实WGS数据测试：

- **CNVPipe和CNVPipe-SVM** 始终表现出较低的假发现率（<0.35）
- **ClinSV** 虽然灵敏度更高，但FDR超过0.85，实用性降低
- CNVPipe在精确度和召回率之间取得了平衡

### 3. 特征重要性分析

使用排列分析发现影响分类的关键因素：

**模拟数据：** CNV大小（Size）、工具类型（Tool）、GC含量（GC）、累积评分（AS）、工具数量（TN）

**真实数据：** 工具类型（Tool）、CNV大小（Size）、累积评分（AS）、GC含量（GC）

---

## 四、应用案例

### 案例1：iPSC基因组稳定性监测

研究者分析了诱导多能干细胞（iPSC）在第5代（P5）和第30代（P30）的CNV变化：

- CNVPipe-SVM在P30和P5分别识别出225和209个CNV
- 173个CNV（66.3%）是共同的
- **数字PCR（ddPCR）验证**：CNVPipe独有的CNV大多数得到验证
- **真阳性率**：CNVPipe-SVM（65.1%和61.3%）高于sv-callers（42.3%和40.9%）

### 案例2：CRISPR/Cas9脱靶效应评估

分析CRISPR/Cas9编辑前后的iPSC样本：

- 在靶向位点观察到读段覆盖度显著下降，表明成功删除
- CNVPipe-SVM的真阳性率达到96.4%（对照）和97.6%（dLNC93）
- 远高于sv-callers的58.1%和58.5%
- **功能性验证**：CNVPipe帮助排除拷贝数伪影，确认真正的表观基因组和转录组变化

### 案例3：儿科发育障碍患者的致病CNV识别

应用CNVPipe分析150个患有发育障碍的儿科患者的WGS数据：

- 平均每个个体检测到162个CNV
- 删除明显多于重复（19,955 vs. 4,098）
- **神经纤维蛋白病1型（NF1）**病例的CNV负荷最高
- **性别差异**：男性患者的删除和良性CNV数量高于女性（特别是在DD患者中）

**自闭症谱系障碍（ASD）的CNV景观：**

- 5个个体（ID: 27859, 28448, 28941, 29405, 29888）显示出异常高的CNV负荷
- 这些"高负荷"病例占所有ASD基因组中37个致病性或可能致病性事件的36个（p = 2.435e-7）
- 识别出16个与神经发育基因相交的CNV，包括CASZ1、POGZ、DNMT3A、PLXNB1、FOXP1等
- **免疫调节轴**：发现了8个基因间复发性CNV，破坏免疫调节因子（如GATA3和CEBPB）的调节基序

### 案例4：肿瘤进化的单细胞追踪

使用CNVPipe的单细胞模块分析乳腺癌数据：

1. **倍性估计创新**：使用最小二乘舍入和峰值逼近策略准确估计单细胞倍性
   - 传统方法假设所有细胞为二倍体是错误的
   - 最小二乘舍入可以准确推断单倍体和非整倍体细胞的存在
   - 峰值逼近策略消除了异常值的出现

2. **肿瘤系统发育树**：
   - 基于整数拷贝数谱计算CNV评分
   - 使用最小生成树（MST）方法推断系统发育树
   - 准确将细胞聚类到亚群中，呈现分支进化模式

3. **临床应用**：分析6个三阴性乳腺癌患者的scDNAseq数据
   - 治疗前、治疗期间和治疗后采样
   - **患者1-3**：肿瘤系统发育树的终末分支仅来自治疗前样本，表明肿瘤被有效根除
   - **患者4-6**：终末分支包含治疗前、中、后的细胞，暗示癌细胞存活和不良临床反应

---

## 五、使用方法

### 1. 安装

```bash
# 克隆GitHub仓库
git clone https://github.com/sunjh22/CNVPipe.git
cd CNVPipe

# 创建conda环境
conda env create -f environment.yml
conda activate cnvpipe
```

### 2. 输入文件

CNVPipe的最小输入：

- 配置文件（config.yaml）
- 原始测序读段的fastq文件（或预比对的BAM文件）

### 3. 运行

```bash
# 使用Snakemake运行
snakemake --cores 8 --use-conda

# 或使用提供的运行脚本
./run_cnvpipe.sh -c config.yaml -t 8
```

### 4. 输出结果

- **final_cnv_calls.tsv**：最终的CNV调用列表
- **cnv_visualization/**：覆盖度和BAF图
- **report.html**：综合报告
- **pathogenicity_prediction.tsv**：致病性预测结果

### 5. 单细胞分析

```bash
# 运行单细胞CNV分析模块
snakemake --cores 8 --use-conda -s workflow/Snakefile_sc.py
```

---

## 六、总结与展望

CNVPipe作为一个增强型的CNV检测流程，在多个方面展现了显著优势：

### 主要优势

1. **高精度**：通过集成多个工具和机器学习过滤，显著降低FDR
2. **适应性强**：深度感知的合并策略适应不同的测序深度
3. **全面性**：涵盖从原始数据预处理到结果可视化的完整流程
4. **创新性**：单细胞CNV分析模块提供准确的倍性估计
5. **用户友好**：基于Snakemake框架，易于使用和扩展

### 应用前景

| 应用领域 | 具体用途 |
|----------|----------|
| 干细胞研究 | 监测基因组稳定性 |
| 基因组编辑 | 评估CRISPR/Cas9脱靶效应 |
| 癌症研究 | 检测致癌CNV和分析克隆进化 |
| 临床诊断 | 识别发育和神经精神疾病的致病CNV |

### 未来发展方向

1. **长读长测序整合**：整合PacBio和Oxford Nanopore数据，解决复杂和重复基因组区域
2. **深度学习应用**：利用更大的训练集学习细微特征
3. **单细胞多组学**：整合scRNA-seq数据（如inferCNV、Sitka、Numbat、CopyKAT）
4. **交互式可视化**：引入交互式基因组浏览器
5. **外显子组CNV检测**：扩展流程以支持外显子组测序数据

---

## 七、参考资料

1. **原始论文**：
   - Sun J, Wong NK, Jiang Z, et al. CNVPipe: An enhanced pipeline for accurate analysis of copy number variation from whole-genome sequencing. bioRxiv. 2025.
   - DOI: https://doi.org/10.1101/2025.05.18.654763

2. **GitHub仓库**：
   - https://github.com/sunjh22/CNVPipe

3. **相关工具**：
   - CNVKit: https://github.com/etal/cnvkit
   - CNVpytor: https://github.com/abyzovlab/CNVpytor
   - cn.MOPS: https://bioconductor.org/packages/cn.mops
   - Delly2: https://github.com/dellytools/delly
   - Smoove: https://github.com/brentp/smoove

4. **Snakemake工作流程框架**：
   - https://snakemake.readthedocs.io/

---

*免责声明：本文解读仅用于学术讨论，不构成任何医学或临床建议。实际使用请参考原始论文和工具文档。*

**关键词：** CNV检测、WGS、机器学习、SVM、Snakemake、生物信息学流程、拷贝数变异、基因组稳定性、单细胞测序
