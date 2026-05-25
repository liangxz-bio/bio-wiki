---
source_url: https://mp.weixin.qq.com/s/unknown
ingested: 2026-05-18
sha256: 0069c73edaeaf3d58a23359707023c6876303dc095401d6583900c68bd858b63
citation: "青禾矩点. DualGPT-AB双阶段生成式抗体设计——Nature Computational Science 论文解读. 微信公众号. 2026."
domain: ai-drug-discovery
---

# DualGPT AB双阶段生成式抗体设计

> **来源：** 青禾矩点

---

![img](https://mmbiz.qpic.cn/sz_mmbiz_gif/Yibf44sj4hmH2znStK5QhcibEaehJMdSrJz8xRmoER94JmCH4gtgX7CRwGiaB8HHXGoxDVaBMlxMYExRMsE8N3wIoXibnafC9MLEOcxcBycaLqM/640?wx_fmt=gif&from=appmsg)

![img](https://mmbiz.qpic.cn/sz_mmbiz_png/Yibf44sj4hmEH6W2HDbyMz5fCFic2YibNdePgv0hVa51993t5UHsX9tPF845icu7vNHiaxM3ZTt3WBwTvezicdIroWLnRatvyMuskyWIU1Q07jwO4/640?wx_fmt=png&from=appmsg)

在药物研发里，抗体一直是最重要的武器之一。

它们可以像“精准导弹”一样识别特定抗原，触发免疫系统攻击病灶。自 1986 年首个单克隆抗体获批以来，抗体药物已经广泛用于癌症、自身免疫疾病等领域。

但真正设计一个能进临床的抗体，远比“找到一个能结合靶点的序列”复杂。

一个好抗体，需要同时满足很多条件：能精准识别靶点，亲和力足够强；黏度合适，便于制剂开发；清除率可控，在体内停留时间合理；免疫原性低，减少不良反应；稳定性、溶解性也要过关。

这些性质往往互相牵制。单独优化一个指标并不难，难的是把多个指标一起拉到理想区间。

最近，一篇发表于 Nature Computational Science 的研究提出了一个名为 DualGPT AB 的双阶段生成式框架，尝试用 GPT 加强化学习来设计治疗性抗体，目标直指 HER2 阳性肿瘤相关抗体。

#### 01 抗体设计的难点：不能只看"结合力"

这篇论文聚焦的是抗体中的 CDRH3 区域，也就是重链互补决定区 3。

可以把 CDRH3 理解为抗体识别靶点时最关键的“指纹区域”之一。它高度可变，直接影响抗体对抗原的识别能力。研究团队选择 HER2 作为靶点，并把生成出来的 CDRH3 序列嫁接到 Herceptin，也就是曲妥珠单抗的抗体模板上，再评估这些新抗体的性质。

传统 AI 抗体设计方法常见做法是：先生成一批序列，再用打分模型筛选；或者把多个性质简单加权，得到一个总分后优化。

问题在于，抗体性质之间存在复杂依赖。亲和力、黏度、清除率、免疫原性并非彼此独立。一个序列可能在结合力上表现很好，却在成药性上暴露风险。

DualGPT AB 的核心思路，就是让模型在生成序列时同步理解“序列”和“性质”之间的关系。

#### 02 DualGPT AB 怎么工作？

整个框架分成两个阶段。

![img](https://mmbiz.qpic.cn/mmbiz_png/Yibf44sj4hmHYwRibz15JxPO74J8eZzMaPmTK4h0HKsJ3HalgMfa3pf0Yn4Nxdg1NTygKxNhgcGtiazmTQ6HoWHxicGUF24l1Jeib3lFKQBUpD3g/640?wx_fmt=png&from=appmsg)

第一阶段，研究团队从 OAS 抗体数据库中获得大规模 CDRH3 序列，并对其中可快速评估的性质进行标注，包括 FvNetCharge、FvCSP 和 HISum。这些指标分别与抗体清除、黏度等成药性相关。随后，团队训练了一个 prior GPT，让模型先学会在有限性质约束下生成合理的 CDRH3 序列。

第二阶段，模型进入强化学习过程。研究团队引入更多性质约束，包括 HER2 特异性和 MHC II minPR。前者对应靶点特异性，后者与免疫原性风险相关。强化学习不断探索新的 CDRH3 序列空间，把高质量、多样化的序列积累下来，再用这些数据训练 enhanced GPT。

换句话说，第一阶段帮助模型“站稳脚跟”，第二阶段推动模型朝着更复杂的多目标设计前进。

论文第 3 页的流程图很好地展示了这个设计：OAS 数据先经过性质评估和标签化，再训练 prior GPT；随后强化学习代理基于五类性质打分持续更新，最终训练 enhanced GPT，用于生成满足更多约束的 CDRH3 序列。

#### 03 结果有多强？
研究团队把 DualGPT AB 与 FBGAN、AB Gen、DynaPPO、PEX、AdaLead 等方法进行比较。每种方法生成 10000 条唯一 CDRH3 序列，然后统计同时满足五个预设性质的成功率。

DualGPT AB 的成功率达到 78.2%。

排名第二的 AdaLead 为 40.4%。

也就是说，在这项设定下，DualGPT AB 显著提高了多性质同时达标的比例。

![img](https://mmbiz.qpic.cn/mmbiz_png/Yibf44sj4hmFGjwcwGW3Y26sdUQbcV2xyibST6vPfld7G0fpgyPzib8RRH7HRbtp6OKK9biandtSzGaY92ZibqFNnRz7p94OffueVfIW3CCzI2aU/640?wx_fmt=png&from=appmsg)

论文第 4 页的柱状图进一步显示，即使分别移除某个性质约束进行测试，DualGPT AB 依然保持较稳定的领先。尤其在 HER2 特异性和 MHC II minPR 相关评估中，它的优势非常明显。

阶段分析也很关键。

训练数据本身和第一阶段 prior GPT 生成的序列，在同时满足五个性质时成功率为 0。到了第二阶段 enhanced GPT，成功率接近 80%。这说明强化学习积累高质量序列，再反过来训练增强模型，是这项工作的关键环节。

![img](https://mmbiz.qpic.cn/sz_mmbiz_png/Yibf44sj4hmGRsIHMo0kb90ajZkLu43gGTic8yyrAqMt1OJmC9tJn9zIMts11jTI1nIlOhLDKkeQFCiccmmDKvDeJmJpekM0TBQ7PndoNHnzibM/640?wx_fmt=png&from=appmsg)

#### 04 从10000条序列中，筛出一个高质量候选库
研究团队用 DualGPT AB 先生成 10000 条唯一 CDRH3 序列，再用 CamSol 分数评估溶解性。

最终，有 1274 条序列的溶解性分数高于 Herceptin，并被纳入后续 CDRH3 候选库。相比其他方法，DualGPT AB 在溶解性筛选后保留下来的序列数量明显更多。

![img](https://mmbiz.qpic.cn/mmbiz_png/Yibf44sj4hmFaEHFhMkJhricofaxZcicd8BUJ2GkuzwpjKvX5AqEgsY2gdw53ic20PR4xDFqEicrvrl0ic8nyibd9QBh9ibSgJEo0jWCzNKWDzteZxM/640?wx_fmt=png&from=appmsg)

论文第 6 页的序列 logo 还显示，G103、Y105、A106、F107、D108 等位点高度保守。其中 Gly 和 Tyr 的富集尤其值得注意，因为 G103 和 Y105 被认为与 HER2 结合相关。

这说明模型生成的序列并非随机拼接氨基酸，而是在多目标约束下形成了一些与 HER2 识别相关的模式。

#### 05 计算预测：100个候选里，8个亲和力超过Herceptin

接下来，研究团队随机选取 100 个候选抗体，用 AlphaFold3 和 AREA AFFINITY 预测它们与 HER2 的结合亲和力。

结果显示，超过一半的候选抗体预测亲和力与 Herceptin 相当。其中 8 个候选抗体的预测亲和力强于 Herceptin，最高者达到 −log10KD = 9.13，而 Herceptin 为 −log10KD = 8.84。

![img](https://mmbiz.qpic.cn/mmbiz_png/Yibf44sj4hmHR9ko6lhicdnXGgVcUPrMJQNicbLVr3Iic3voryibhiaicq9Kzfc7IqF5GOojzCOAECaViarlCuvSdnZOXicWQgHLNX6GBSyEUD5CC9As/640?wx_fmt=png&from=appmsg)

论文第 7 页的结构图展示了这些候选抗体可能形成的新相互作用。例如，CDRH3_39 中的突变带来了 E102 与 HER2 上 T504 之间的潜在氢键；CDRH3_68 则可能通过 Y105 与 HER2 上 G486 形成相互作用。

研究团队也提醒，相关结构预测需要谨慎解读，因为部分界面置信度分数较低。这一点很重要：AI 给出了候选方向，真正价值仍然需要实验验证。

#### 06 湿实验验证：CDRH3_39展现更强肿瘤杀伤潜力

研究团队进一步筛选出 9 个候选抗体进入湿实验验证，并加入 Herceptin 和随机 CDRH3 对照。

SPR 实验显示，Herceptin、CDRH3_39、CDRH3_68 与 HER2 的 KD 分别为 1.85 nM、36.4 nM、117 nM，而随机对照 CDRH3_RAW 大于 1 mM。ELISA 结果也确认，CDRH3_39、Herceptin、CDRH3_68 对 HER2 的结合能力明显优于随机对照，其中 CDRH3_39 显示出最强结合信号。

更值得关注的是功能实验。

研究团队在 HER2 阳性乳腺癌细胞系 SK BR 3 和 AU565 上测试了 ADCC 和 CDC 活性。结果显示，Herceptin、CDRH3_39、CDRH3_68 都具有明显 ADCC 活性；在 SK BR 3 细胞中，CDRH3_39 的细胞裂解活性高于 Herceptin；在 AU565 细胞中，CDRH3_68 的 ADCC 活性高于 Herceptin。

CDC 实验中，Herceptin 在两个细胞系中没有诱导明显 CDC 活性，而 CDRH3_39 和 CDRH3_68 在 AU565 与 SK BR 3 中均表现出 CDC 活性。综合来看，CDRH3_39 相对 Herceptin 展现出增强的肿瘤杀伤能力。

![img](https://mmbiz.qpic.cn/sz_mmbiz_png/Yibf44sj4hmEZ5dMp7Qvic3Lk11bUeChHNOyEe0jicC9CTxiaLy88V51QR5gkRZ8hibRyjEiaQL7oA0sZnaglV2xcPibfDHDyx26ICGo7NG5FYcAvo/640?wx_fmt=png&from=appmsg)

这也是这篇论文最有分量的部分：模型生成的序列经过了真实湿实验验证，并且出现了功能优于经典抗体模板的候选分子。

#### 07 这项研究真正重要在哪里？

这篇文章的价值，并不只是“又做了一个抗体生成模型”。

它更像是把 AI 抗体设计推进到了一个更接近药物研发真实需求的阶段。

真实药物研发从来不是单目标竞赛。一个分子结合力再强，如果黏度高、清除快、免疫原性风险大，后续开发就会遇到巨大阻力。DualGPT AB 尝试把这些约束前置到生成过程中，让模型在一开始就朝“可开发”的方向探索。

这代表一种重要趋势：AI 药物设计正在从“生成看起来合理的分子”，走向“生成满足多重工程约束的候选药物”。

当然，这项工作仍然属于早期研究。论文也明确提到，DualGPT AB 依赖针对 HER2 训练的特异性预测器，这会限制其直接迁移到其他抗原上的能力。模型目前主要生成 CDRH3 序列，并嫁接到 Herceptin 模板上，未来若能纳入更长抗体片段甚至全长抗体上下文，可能进一步提高功能抗体的发现概率。

此外，体外实验成功并不等于可以直接进入临床。候选抗体还需要经历更系统的体内药效、安全性、药代动力学、生产工艺和临床研究验证。

但无论如何，这项研究展示了一个清晰信号：

AI 设计抗体，已经开始跨过“纸面预测”的门槛，进入计算设计与实验验证相互闭环的新阶段。

当 GPT 类模型遇上强化学习，再与湿实验形成反馈，抗体药物发现的速度和空间，可能正在被重新定义。
