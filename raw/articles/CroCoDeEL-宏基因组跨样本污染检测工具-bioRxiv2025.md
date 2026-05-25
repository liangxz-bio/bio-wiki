# 无需阴性对照的宏基因组跨样本污染检测工具——CroCoDeEL

> 宏基因组测序在微生物组研究中应用广泛，但样本间交叉污染这个技术问题一直被严重低估。这种污染通常发生在96孔板中相邻样本之间，会导致物种丰度失真、假阳性发现，甚至影响研究结论的可靠性。现有检测方法要么依赖阴性对照（成本高且不够全面），要么基于菌株水平分析（计算量大且无法区分自然菌株共享）。研究人员开发的CroCoDeEL工具另辟蹊径，仅需物种丰度表就能准确识别污染样本及其污染源，还能估算污染比例。更重要的是，研究团队在多个高引用率研究中发现了严重的未报告污染问题，提示这可能是宏基因组领域普遍存在却被忽视的质控盲区。

---

## 摘要

**背景**
宏基因组测序能深入解析微生物群落，但常受技术偏倚影响，其中交叉样本污染尤为突出。这种污染源于实验室处理过程中样本间的物质交换，会扭曲微生物谱并影响下游分析的可靠性。现有检测方法依赖阴性对照，既不方便也无法检测真实样本内部的污染。而菌株水平的生信分析方法既无法区分污染与自然菌株共享，灵敏度也不足。

**结果**
研究团队开发了CroCoDeEL，这是一个无需对照的交叉样本污染检测和定量工具。该工具通过线性建模和预训练的监督学习模型，识别物种丰度谱中的特异性污染模式。在三个公开数据集的基准测试中，CroCoDeEL准确检测出污染样本并识别污染源，即使在低污染率（小于0.1%）情况下也能有效工作，前提是测序深度足够。值得注意的是，研究团队在多个高引用率研究中发现了严重污染案例，这些污染可能影响了原研究的部分结论。

**结论**
研究证实交叉样本污染会在物种丰度谱中产生特异性的"污染线"模式，这种模式在非污染样本对中出现概率极低（少于万分之一）。CroCoDeEL通过自动检测这一模式，实现了无需阴性对照、不依赖样本位置信息的污染识别，能同时确定污染源和被污染样本，并估算污染率。工具在真实数据上达到约0.7的Matthews相关系数和95%的召回率，且计算效率高。

---

## 主要结果

### 污染线的发现与验证

在真实混合样本实验中，MQB\_095\_l2（被MQB\_068以约10%污染）的物种丰度散点图清晰展现了污染线特征（红色虚线）。所有MQB\_068中丰度较高的物种都在MQB\_095\_l2中被检测到（y轴上无点），且部分共有物种沿一条直线排列，表明这些是污染引入的特异性物种。相比之下，两个未污染样本MQB\_095和MQB\_068的对比图中没有这种模式。更重要的是，MQB\_095的物种丰富度（194）显著低于其污染版本MQB\_095\_l2（319），意味着39%的检出物种实际是污染假象。

### 分类器性能

在三个真实人类粪便宏基因组测试集上（样本量分别为110、128、237），CroCoDeEL表现一致：

- **Matthews相关系数**：均在0.7左右
- **召回率**：平均95%，能检出人工专家标注的绝大多数污染案例
- **精确度**：约50%，但"假阳性"案例多数污染率很低（平均0.26%），分类器置信度也显著低于真阳性（概率0.73 vs 0.92，p<2.2×10⁻¹⁶）
- **特异性验证**：在跨数据集的140,972个样本对（理论上不可能污染）中测试时，仅检出5个假阳性

**假阴性分两类：**
1. 极低污染率且人工专家信心也不高的案例
2. 高污染但污染线模糊的情况，如两个样本都被第三方污染导致的传递性相似，或"级联污染"

**计算资源：** 在标准节点（双Intel Xeon E5-2680，16核256GB内存）上，**处理百个样本仅需数分钟**，运行时间与样本对数呈线性关系，CPU并行效率约0.85，内存消耗低且稳定。

### 测序深度、污染率和分类器的影响

用25个半模拟样本对系统评估了影响因素，测序深度和污染率都是关键因素（p<2×10⁻¹⁶）：

- 以Meteor2为例，**20%污染率**时即使1M reads也能全检出
- **2%污染率**时需要10M reads才能达到92%召回率（1M reads仅40%）

**分类器选择影响巨大：** 在10M reads、0.5%污染率条件下：

| 分类器 | 召回率 |
|--------|--------|
| Meteor2 | 76% |
| Sylph | 4% |
| MetaPhlAn4 | 0%（完全检测不到） |

原因在于**Meteor2对亚优势物种的检测灵敏度更高，定量也更准确**，能在污染线上产生更多物种且离散度更小。MetaPhlAn4系统性低估亚优势物种丰度，干扰了污染线检测，虽然过滤掉低丰度物种后有所改善，但Meteor2仍是低测序深度/低污染率场景的唯一有效选择。

### 与菌株共享方法的对比

在Lou等人的婴儿纵向队列研究（P3板）中：

- **菌株共享方法**：仅识别出2个污染样本（阴性对照NC3和样本63D9），且无法确定具体污染源
- **CroCoDeEL**：检出16个人工验证的污染事件，涉及12个污染样本

CroCoDeEL不仅确认了63D9被58D256和60D38污染，还发现NC3被82D361、83D88和83D249污染而非82M。

更关键的是，CroCoDeEL检出了菌株方法无能为力的场景：
- 同一婴儿不同时间点样本间的污染（如63D9被其后期样本63D250以70%污染）
- 双胞胎样本间污染（58D256污染60D38）
- 母婴样本污染（58M以63%污染58D7，后者物种丰富度达223远超同龄婴儿）

### 未检测污染导致的错误结论

**Lou等人婴儿肠道定植研究（P2板）：**
CroCoDeEL在P2中发现8个污染事件，包括关键案例57D8被母亲样本57M以23%污染。原研究报道的"母亲菌株在婴儿出生时定植但未持续"很可能是污染假象而非真实定植。

**Ferretti等人高引用率婴儿研究：**
- 182个样本中有48个被污染（**26%**）
- 原文报道的56个"瞬时出现"微生物中，**80%（45个）** 实际来自5个被大量污染（污染率>10%）的新生儿粪便样本
- 原研究观察到婴儿首次采样（t1）的物种多样性显著高于后续时间点（t2: p=0.005, t3: p=0.003），但**剔除高污染样本后这种差异消失**（t2: p=0.12, t3: p=0.56）

**TwinsUK队列（1004个成人粪便样本）：**
- CroCoDeEL检出202个污染样本，其中176个关联到8个相同污染源
- 这8个源样本彼此极相似（Spearman ρ>0.96）且物种丰富度异常高（782±8），疑似是多样本混合物的重复
- 污染样本间的Bray-Curtis距离（0.40）显著小于非污染样本（0.53，p<2×10⁻¹⁶）
- 在至少出现于10个样本的1382个物种中，**32%（440个）** 在污染样本中流行率显著更高（FDR≤0.01）

---

## CroCoDeEL 使用说明

### 软件介绍

CroCoDeEL是专门检测宏基因组数据中交叉样本污染的工具。它能识别污染样本、定位污染源、估算污染率，只需要物种丰度表即可运行，**不需要阴性对照或样本板位信息**。

### 安装方式

**推荐使用conda安装（最简单）：**

```bash
conda create --name crocodeel_env -c conda-forge -c bioconda crocodeel
conda activate crocodeel_env
```

**或使用pip安装（需要Python ≥3.12）：**

```bash
pip install crocodeel
```

**安装测试：**

```bash
crocodeel test_install
```

> 这条命令会在测试数据集上运行CroCoDeEL，验证安装是否成功。加上 `--keep-results` 参数可以保留结果查看。

### 输入文件要求

需要准备TSV格式的物种丰度表，第一列是物种名称，其他列是各样本中的物种丰度（相对丰度）。格式示例：

| species_name | sample1 | sample2 | sample3 |
|--------------|---------|---------|---------|
| species_1    | 0       | 0.05    | 0.07    |
| species_2    | 0.1     | 0.01    | 0       |

> **重要提示**：CroCoDeEL需要准确定量亚优势物种。强烈建议使用Meteor软件包生成丰度表，也可用MetaPhlAn4（但检测低污染能力会下降）。不建议用其他分类器，准确性达不到要求。

### 基本使用流程

#### 1. 检测污染

```bash
crocodeel search_conta -s species_abundance.tsv -c contamination_events.tsv
```

输出文件 `contamination_events.tsv` 包含检测到的所有污染事件，每个事件包括：
- 污染源
- 被污染样本
- 污染率估计值
- 随机森林模型打分
- 污染引入的特异性物种列表等信息

**如果用MetaPhlAn4生成丰度表，务必加过滤参数：**

```bash
crocodeel search_conta -s species_abundance.tsv --filter-low-ab 20 -c contamination_events.tsv
```

> `--filter-low-ab 20` 参数会过滤掉低丰度物种，提高灵敏度。

#### 2. 结果可视化

```bash
crocodeel plot_conta -s species_abundance.tsv -c contamination_events.tsv -r contamination_events.pdf
```

生成PDF报告，包含每个污染事件的对数散点图，x轴是被污染样本，y轴是污染源，红色线标出污染特异性物种。

#### 3. 一步完成（推荐）

```bash
crocodeel easy_wf -s species_abundance.tsv -c contamination_events.tsv -r contamination_events.pdf
```

一条命令完成污染检测和PDF报告生成。

### 结果解读注意事项

CroCoDeEL可能会对物种组成相似的样本误报污染（比如纵向采样、同笼饲养动物等）。即使是不相关样本，偶尔也会有假阳性。**强烈建议人工检查PDF报告中的散点图**，识别并剔除可能的假阳性结果。

理论上，污染过程中不应有超过污染线的点。然而，有时可能会有点，尤其是在级联污染的情况下。在这种情况下，可以容忍一些细节。

**级联污染案例说明：**
- 样本2被样本1污染，但污染线上方有点（图a）
- 实际上，样本1也被另一个样本污染（样本3，图b）
- 这种污染解释了污染线上方的点
- 如果再次处理样品1（sample1_new），它不再被样品3污染（图c），污染线以上的点也消失（图d）

> 当一个箱子包含污染线以上的点时，观察污染源是否随后被其他样本污染就很有趣。

### 高级功能：训练自定义模型

如果有标注好的污染/非污染样本对数据集，可以训练自己的随机森林分类模型：

```bash
# 下载官方训练集（可选，用于测试）
wget --content-disposition 'https://entrepot.recherche.data.gouv.fr/api/access/datafile/:persistentId?persistentId=doi:10.57745/IBIPVG'
xz -d training_dataset.meteor.tsv.xz

# 训练新模型
crocodeel train_model -s training_dataset.meteor.tsv -m crocodeel_model.tsv -r crocodeel_model_perf.tsv

# 使用自定义模型
crocodeel search_conta -s species_ab.tsv -m crocodeel_model.tsv -c conta_events.tsv
```

### 重现论文结果

论文中的训练集、验证集、测试集物种丰度表都已公开。以Lou等人数据集的P3为例：

```bash
# 下载数据
wget --content-disposition 'https://entrepot.recherche.data.gouv.fr/api/access/datafile/:persistentId?persistentId=doi:10.57745/BH1RKY'

# 运行分析
crocodeel easy_wf -s PRJNA698986_P3.meteor.tab -c PRJNA698986_P3.meteor.crocodeel.tsv -r PRJNA698986_P3.meteor.crocodeel.pdf
```

---

## 参考文献

- Goulet, L. et al. "CroCoDeEL: accurate control-free detection of cross-sample contamination in metagenomic data" *bioRxiv* (2025). https://doi.org/10.1101/2025.01.15.633153
- Ricci, L., Heidrich, V., Punčochář, M. et al. Baby-to-baby strain transmission shapes the developing gut microbiome. *Nature* (2026). https://doi.org/10.1038/s41586-025-09983-z
- GitHub仓库：https://github.com/metagenopolis/CroCoDeEL

---

> 来源：腾讯云开发者社区
> 原文链接：https://cloud.tencent.com/developer/article/2645307
