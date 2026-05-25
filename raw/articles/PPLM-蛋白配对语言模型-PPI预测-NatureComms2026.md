---
source_url: https://mp.weixin.qq.com/ (微信公众号·AI药物设计实验室)
ingested: 2026-05-19
sha256: 705df0bd98e2f9ab943d4e5660c34b936ea7b57d5babdbb418735c0c1d3485cc
citation: "Jun Liu, Hungyu Chen, Yang Zhang. A paired sequence language model for protein-protein interaction modeling. Nature Communications (2026) 17:3733."
domain: ai-drug-discovery
---

# Nature Communications 2026｜把两条蛋白链一起读：PPLM让PPI预测更接近真实相互作用

> **来源：** AI药物设计实验室

---

🔔关注我们，每日分享最新的人工智能与生命科学论文～

![](https://mmbiz.qpic.cn/mmbiz_png/xawOdPt2Qv6mSohvm5uN4Ab79fno1AxplNgFdbwG2xd9O5UydujUx4ibF5OgRLz2mtJrKVwvGqKG1awibpgyFybibC9NcEju34CRu1cuK0f5kU/640?wx_fmt=png&from=appmsg)

做 protein-protein interaction（蛋白质-蛋白质相互作用，PPI）预测时，很多模型会先分别编码两条蛋白序列，再把表示拼起来做分类或回归。

这个流程漏掉一个关键点：两条链放在一起以后，残基之间会出现新的上下文关系，尤其是界面区域的链间依赖。

这篇论文提出 Protein Pair Language Model（蛋白质配对语言模型，PPLM）。

它直接把一对蛋白序列作为语言模型输入，让模型在预训练阶段学习链内关系和链间关系。

基于这个底座，作者又做了三个下游模型：PPLM-PPI 判断两条蛋白是否互作，PPLM-Affinity 预测结合强度，PPLM-Contact 预测链间接触和界面残基。

## 一眼看懂
- **核心思路**：把蛋白语言模型的建模对象从单条序列扩展到蛋白对，让模型直接学习 inter-protein dependency（链间依赖）。
- **关键结构**：hybrid intra-/inter-protein attention（链内/链间混合注意力）。链内残基用 RoPE（旋转位置编码），链间残基不用位置编码，避免把序列距离误当成空间先验。
- **训练数据**：整合 PDB 复合物和 STRING 相互作用数据，构建超过 330 万对蛋白序列。
- **下游任务**：覆盖 PPI 二分类、binding affinity（结合亲和力）预测、inter-protein contact map（链间接触图）预测。
- **主要结果**：PPLM 在困惑度、PPI 预测、亲和力预测、链间接触预测、界面残基识别上都超过多个基线模型。

## 论文做了什么

### 方法：把“两条链一起读”做成语言模型
PPLM 的输入是两条蛋白序列。模型先分别 tokenize，再拼接成一个完整输入。之后通过 33 个 Transformer block 联合建模。

![](https://mmbiz.qpic.cn/mmbiz_png/xawOdPt2Qv5qlE4oP1phceialIHgcZL7shE3eoSXsCkmEfhYIvW0SFNciaamuBy9tdwLslkTnHBHcvtyKrzicKAOgxibQickNHjicDVUQwO6F13ibc/640?wx_fmt=png&from=appmsg)

链内残基之间保留位置编码，用来表达同一条链上的顺序关系；链间残基之间去掉位置编码，再加上可学习的链间注意力权重和显式 mask，让模型把注意力集中到蛋白对之间的关系上。

训练时，作者用 masked language modeling（掩码语言建模）。

PDB 里的高质量复合物会更重视界面残基：界面残基 mask 30%，非界面残基 mask 15%。

STRING 数据则统一 mask 15%。

这个设计让模型在预训练阶段就更多接触界面区域的信息。

### 结果一：PPLM 比单链语言模型更懂蛋白对
作者用 perplexity（困惑度）比较 PPLM 和 ESM2。困惑度越低，说明模型预测被遮盖氨基酸的能力越强。

在随机 mask 下，PPLM 在同源二聚体、异源二聚体和 STRING 蛋白对上的平均困惑度分别为 7.30、5.08 和 4.50，低于 ESM2 的 8.40、6.73 和 5.70。

在只 mask 界面残基时，Dual mode（双链界面残基同时遮盖）下，PPLM 平均困惑度为 6.79，ESM2 为 8.50；Single mode（单链界面残基遮盖）下，PPLM 为 7.81，ESM2 为 10.36。说明 PPLM 的提升主要来自对链间上下文的建模。

### 结果二：PPLM-PPI 跨物种预测更稳
PPLM-PPI 用 PPLM 输出的 embedding、链内注意力和链间注意力做 PPI 二分类。

模型使用 max pooling（最大池化）和 mean pooling（平均池化）两条分支，再通过 MLP（多层感知机）输出相互作用概率。

![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/xawOdPt2Qv5a5x8Fhtc9TKSic59hHFicVtvia2Z0fibZffDciajSJnz8c9tNexnMUNRmojZib4DibFwXHibSPHr7ADib0eic1c2VhObZGxib8Gg0Wnic39c/640?wx_fmt=jpeg&from=appmsg)

在 M. musculus、D. melanogaster、C. elegans、S. cerevisiae 和 E. coli 五个物种上，PPLM-PPI 的 AUPRC 分别达到 0.920、0.906、0.883、0.745 和 0.784。相比第二名 TUnA，提升幅度为 4.0%、6.2%、7.2%、17.6% 和 12.9%。F1-score 也在五个物种上全部最高。

### 结果三：PPLM-Affinity 可以直接从序列预测结合强度
PPLM-Affinity 用 PPLM 的最后一个 Transformer block 做微调，预测蛋白复合物的 ΔG（结合自由能）。

作者在 PPB-Affinity 数据集上重新按结构相似性划分五折，避免同源结构泄漏。

整体数据上，PPLM-Affinity 的 PCC（Pearson 相关系数）和 SRCC（Spearman 秩相关系数）分别为 0.643 和 0.636，高于 ESM2-Affinity 和结构方法 PPB-Affinity。

### 结果四：PPLM-Contact 提升链间接触和界面残基识别
PPLM-Contact 把三类信息合在一起：PPLM 的链间注意力、MSA（多序列比对）特征、单体结构距离图。之后用 inter-protein transformer（链间 Transformer）预测链间接触图。

在 Homodimer300 上，PPLM-Contact 使用实验单体结构时 top L 接触精度达到 77.8%，使用 AlphaFold2 预测单体结构时达到 66.6%。

在 Heterodimer99 上，对应精度为 48.9% 和 45.1%，均超过 DeepInter、CDPred、GLINTER 等方法。

作者还提出 PPLM-Contact2，把 AlphaFold2.3、AlphaFold3 和 DMFold 预测复合物里的链间距离图加进来。这个版本把同源二聚体 top L 精度从 65.0% 提高到 85.1%，异源二聚体从 44.3% 提高到 88.0%。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/xawOdPt2Qv53ibyj8hSowlszDzbZLONOVOw2QJbfwXORKf1rw7AtWDNFCGdDAsjoxicwSw756hGa3y47kibEhYHeRAUspaymZuVy4VAZegroIk/640?wx_fmt=png&from=appmsg)

## 我们的看法
这篇论文把蛋白语言模型的基本单位从“单条蛋白链”推进到“蛋白对”。

PPI 本来就是成对发生的任务，单链表示再强，也需要额外模块去补链间关系。

PPLM 把这一步提前放到预训练里，后面的互作预测、亲和力预测、接触预测都能共享同一个表示底座。

PPLM-Contact2 的结果也说明，语言模型和结构预测模型可以互补。AlphaFold 系列提供强结构先验，PPLM 提供从蛋白对预训练中学到的交互表示。两类信息合在一起，链间接触和界面残基定位会更稳。

## 边界
PPLM 的训练数据来自 PDB 和 STRING，覆盖面很大，但瞬时相互作用、弱相互作用、条件依赖相互作用仍然不足。论文也给出一个失败案例：纳米抗体-抗原复合物界面很小，由 loop（环区）介导，同时共进化信号较弱，PPLM 特征仍然难以完整恢复界面。

PPLM-Contact 对 MSA 和结构特征仍有依赖。完整模型需要 MSA、单体距离图，PPLM-Contact2 还需要复合物结构预测结果。对于低同源蛋白、柔性界面、构象变化明显的体系，预测结果需要更谨慎地解释。

## 论文信息
- **论文标题**：A paired sequence language model for protein-protein interaction modeling
- **期刊**：Nature Communications，2026，17:3733
- **作者**：Jun Liu，Hungyu Chen，Yang Zhang
- **机构**：National University of Singapore；Cancer Science Institute of Singapore；School of Computing；Yong Loo Lin School of Medicine
- **Github**：https://github.com/junliu621/PPLM
- **Webserver**：https://zhanggroup.org/PPLM/
