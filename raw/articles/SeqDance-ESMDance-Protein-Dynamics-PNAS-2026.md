---
source_url: https://www.pnas.org/doi/abs/10.1073/pnas.2530466123
ingested: 2026-05-21
sha256: 8c96362c740fb33678451ff3b2779335f401e067d1c1557c769b4745715ea0ac
citation: "Hou C, Zhao H, Shen Y. Protein language models trained on biophysical dynamics inform mutation effects. PNAS, 2026, 123(4), e2530466123. DOI: 10.1073/pnas.2530466123"
---

# PNAS | 让AI学会蛋白质动力学：SeqDance与ESMDance如何跨越进化信息的鸿沟，精准预测突变效应？

> **来源：** biomath

---

**Hou, C., Zhao, H., & Shen, Y. (2026). Protein language models trained on biophysical dynamics inform mutation effects. *Proceedings of the National Academy of Sciences*, *123*(4), e2530466123.

****https://www.pnas.org/doi/abs/10.1073/pnas.2530466123


![](https://mmbiz.qpic.cn/mmbiz_png/3a2JCUU90QUj254s9QwHrCQdicr6KcJWwpuCSAdZrpbt9FRAJZw9G9235kibTFBqc1T7q1Lay9L4K5VPJZemzKAA/640?wx_fmt=png&from=appmsg)

## Abstracts
结构动力学对蛋白质功能和突变效应至关重要。目前的蛋白质深度学习模型主要在序列和/或静态结构数据上进行训练，往往无法捕捉到蛋白质的动态特性。为了解决这个问题，我们介绍了 SeqDance 和 ESMDance，这是两个基于从超过 64,000 个蛋白质的分子动力学模拟和正模分析中得出的动态生物物理特性进行训练的蛋白质语言模型。这两个模型都可以直接应用于预测未见过的有序和无序蛋白质的动态特性。SeqDance 从头开始训练，其注意力机制捕捉了残基之间的动态相互作用和共运动，其嵌入向量编码了丰富的蛋白质动力学表征，可以通过迁移学习进一步用于预测训练任务之外的构象特性。SeqDance 预测的动态特性变化反映了突变对蛋白质折叠稳定性的影响。ESMDance 建立在 ESM2输出之上，在缺乏进化信息的设计蛋白质和病毒蛋白质的突变效应零样本预测方面，显著优于 ESM2。总之，SeqDance 和 ESMDance 提供了一个将蛋白质动力学整合到语言模型中的框架，从而能够更广泛地预测蛋白质行为和突变效应。

## 一.科学问题
**目前的蛋白质深度学习模型（如AlphaFold和ESM系列）面临着一个核心的科学瓶颈：它们过度依赖进化信息（同源序列比对）或静态结构快照，而忽视了蛋白质的“动态本质”。**生物学的中心法则告诉我们，序列决定结构，结构决定功能。然而，蛋白质并非静止的雕塑，而是不断运动的实体。这种结构动力学（Structural Dynamics）对于理解热力学、折叠路径、变构效应以及**天然无序区域（IDRs）至关重要。传统的模型在处理那些缺乏同源序列的蛋白质**（如从头设计的蛋白质、快速变异的**病毒蛋白**或**孤儿蛋白**）时，表现往往大打折扣，因为进化信息在这些场景下是稀缺或失效的。此外，仅凭静态结构无法描述IDRs的构象系综。因此，如何将高维、复杂的分子动力学（MD）数据有效地整合进深度学习模型，使AI不仅能看懂蛋白质的“长相”（序列/结构），还能看懂它们的“舞步”（动力学行为），是本研究致力解决的关键科学问题。

### 二.模型逻辑与核心图解
本研究的逻辑核心在于构建一个基于“生物物理动力学”而非仅仅依赖“进化历史”的预训练框架。研究团队收集了超过64,000个蛋白质的动态数据，包括高精度的全原子分子动力学（MD）模拟和低分辨率的粗粒化MD及正模分析（NMA）。

![](https://mmbiz.qpic.cn/mmbiz_png/3a2JCUU90QUj254s9QwHrCQdicr6KcJWwHNapzycNdBrOMTdbqnQq7X6XOCQXnfByJMX8xMAshDOtBiaiaMwdCnKw/640?wx_fmt=png&from=appmsg)

**图1：蛋白质研究中的信息流、代表性pLM、模型预训练及测试集性能**

图A展示了传统的“序列-结构-功能-进化”范式，强调了功能其实是由**结构系综（Structure Ensemble）**决定的，而不仅仅是单一静态结构。

图B对比了现有模型（如ESM依赖进化信息，ProSE/METL依赖静态结构）与本研究模型（SeqDance/ESMDance依赖动态属性）的数据源差异。

图C详细展示了模型架构。模型输入为单一蛋白质序列，通过Transformer编码器学习，最后通过预测头输出残基级别的动态属性（如RMSF、SASA）和成对的动态属性（如运动相关性、接触频率）。

图D-G的数据表明，在天然无序区（IDRs）的测试中，**SeqDance在预测残基涨落和成对运动相关性方面表现优异（Pearson r ~0.89）**，甚至优于专门的构象生成模型STARLING。这证明了模型成功从序列中直接学会了复杂的动力学模式。

为了验证模型是否真的“理解”了物理机制，研究人员深入分析了Transformer的注意力机制（Attention）。

![](https://mmbiz.qpic.cn/mmbiz_png/3a2JCUU90QUj254s9QwHrCQdicr6KcJWwURxFOpyXY7vfOlhR2Vrb71obiar5443YnM0JdYLBwWH66TZJl7ibtpXg/640?wx_fmt=png&from=appmsg)

**图2：SeqDance的注意力机制在测试集中捕捉动态相互作用和共运动**

该图展示了SeqDance模型内部的注意力头（Attention Heads）与实际物理模拟数据之间的相关性。

结果显示，SeqDance的特定注意力头能够高度关注那些在MD模拟中表现出**高相互作用频率（Interaction Frequency）**和**正向共运动（Positive Co-movement）**的残基对。

特别是在IDRome（无序蛋白）测试集上，SeqDance的注意力与MD共运动的相关性极高（图E）。这意味着**模型在没有明确物理方程指导的情况下，仅通过预训练就自发学会了识别哪些氨基酸会“一起移动”**。

除了局部动力学，模型还需要掌握全局的构象特征。

![](https://mmbiz.qpic.cn/mmbiz_png/3a2JCUU90QUj254s9QwHrCQdicr6KcJWwDB4qsknianCxiclp4BkbHOBeR6H2uxlXcsNm7bsfQJdFjgk8BNC9EaDw/640?wx_fmt=png&from=appmsg)

**图3：SeqDance的嵌入向量编码了蛋白质的全局构象属性**

研究者利用SeqDance生成的嵌入向量（Embeddings）来预测蛋白质的全局物理参数，如**回转半径（Radius of Gyration, Rg）和端到端距离（End-to-end distance）**。

对比数据显示（图A-D），SeqDance在预测无序区域（IDR）的构象属性时，误差（MAE）显著低于ESM2、METL和ProSE等现有模型。

图E-G的散点图直观地展示了SeqDance预测值与MD模拟真值的高度一致性（Pearson r > 0.9）。这说明**SeqDance不仅关注局部细节，还成功编码了蛋白质整体的松散或紧凑程度**，这对于理解蛋白质在溶液中的实际形态至关重要。

论文的核心应用目标是解决突变效应预测，特别是针对折叠稳定性（ΔΔG)

![](https://mmbiz.qpic.cn/mmbiz_png/3a2JCUU90QUj254s9QwHrCQdicr6KcJWwQfded4j7DibpWZr5RRQbT36165lYnE1vzCZE5cjV7kCUGttIAf8ibz3g/640?wx_fmt=png&from=appmsg)

**图4：预测突变对蛋白质折叠稳定性的影响**

图A展示了零样本（Zero-shot）预测策略：比较野生型和突变型序列在SeqDance预测出的动态属性上的差异，以此推断突变影响。假设是：**破坏稳定性的突变会导致剧烈的动力学变化**。

图B分析了不同动态属性的预测能力，发现SASA（溶剂可及表面积）和二面角的变化与稳定性改变相关性最高。

关键结果在于图F和H：结合了进化信息（ESM2参数）和动力学训练的**ESMDance模型，在预测稳定性方面取得了最佳性能（Spearman r = 0.46）**，显著超越了单独的ESM2（0.33）或SeqDance（0.24）。这表明**“进化信息+物理动力学”是预测蛋白质稳定性的黄金组合**。

最后，研究展示了该模型在进化信息失效场景下的“杀手锏”级应用。

![](https://mmbiz.qpic.cn/mmbiz_png/3a2JCUU90QUj254s9QwHrCQdicr6KcJWw1EEdNf13dumRUHliczS9E7KGemAjtibxOsZlDTzR8KSeHSTLQIIpy7CA/640?wx_fmt=png&from=appmsg)

**图5：预测设计蛋白质和病毒蛋白质的突变效应**

这是本研究最令人兴奋的部分。对于**从头设计的蛋白质（De novo designed proteins）**，由于它们在自然界中没有进化历史，基于进化的ESM2模型表现往往不佳。

图A-F显示，在缺乏同源序列的设计蛋白上，**ESMDance的表现（中位相关性 0.46）大幅碾压ESM2（0.21）和SaProt（0.06）**。

图E和F的具体案例展示了ESM2完全无法预测某些设计蛋白的突变效应（相关性接近0），而ESMDance依然能保持高相关性（>0.5）。

图I表明，对于快速变异的**病毒蛋白质**，ESMDance同样表现出优于ESM2-35M的性能。这证明了引入物理动力学信息成功填补了进化信息缺失留下的空白。

### 三.总结分析
这篇论文通过引入SeqDance和ESMDance，成功打破了蛋白质语言模型对静态结构和进化信息的单一依赖。其核心发现与价值总结如下：

**物理信息的有效注入：** 即使不输入三维坐标，仅通过学习MD和NMA数据，SeqDance也能从序列中“感知”到残基间的共运动和全局构象变化。这证明了**语言模型具备学习复杂物理规律的潜力**。

**“双剑合璧”的ESMDance：** ESMDance巧妙地结合了ESM2的进化知识与新学到的动力学知识。数据表明，这种融合使得模型在通用任务上表现强劲，并在特定任务（如设计蛋白）上具有不可替代的优势。

**解决“孤儿”难题：** 对于缺乏同源序列的设计蛋白和病毒蛋白，传统的基于进化的方法往往束手无策。本研究证明，**基于生物物理动力学的预训练是解决这一痛点的有效途径**，为蛋白质工程和新药设计提供了全新的工具。

**效率与精度的平衡：** 该方法提供了一种无需运行昂贵且耗时的MD模拟，就能快速获得蛋白质动态特征的手段，实现了计算效率数量级的提升。

SeqDance和ESMDance不仅让AI读懂了蛋白质的“字母表”，更让AI看懂了蛋白质在微观世界中复杂的“舞蹈”，从而在缺乏进化路标的荒原上，依然能精准预测突变的命运。

