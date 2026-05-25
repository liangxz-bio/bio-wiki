---
source_url: https://mp.weixin.qq.com/s/unknown
ingested: 2026-05-18
sha256: b63bffa0cc8dd0660780639f05bf35bcc1ef9e5a41d5e9e8442bb05ab11e97f5
citation: "榴莲忘返AIDD. AI造药新范式：分子不再硬塞，而是在口袋里「生长」. 微信公众号. 2026. | 综述：BOAT/ProteomeScan/SeFMol/FlyPredictome/TarPass."
domain: ai-drug-discovery
---

# AI 造药新范式：分子不再硬塞，而是在口袋里「生长」

> **来源：** 榴莲忘返 AIDD

---

![](https://mmbiz.qpic.cn/mmbiz_png/K5924eDcsicAqjQjS2E2af1vSIOH9fwKk0dsia31uagAZYwqjGXvicnUM4YobMpJdXVyXRXnBpmx1iceBB09RTBm01SJRJAKiau02QpGMice1PV9k/640?from=appmsg)

![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/K5924eDcsicB2YZ1f82GztxzcF72Ubq4chibmqewSxyfGTjlgBwR2CH21ECY37TPZzQfPZRp7RzoIwP6QOqSwqBic4dicRQrmTHTM19P35oZHzM/640?from=appmsg)

![](https://mmbiz.qpic.cn/mmbiz_jpg/K5924eDcsicAyAcF1uV7AeXNqffcqticEohViajpe1FcGcpibSNUdkqppVCwItfIJbpKUJfrFHWwkdDsFutuyDAdHicC10382m4ZWTTcBxqgC1uA/640?from=appmsg)

![](https://mmbiz.qpic.cn/mmbiz_jpg/K5924eDcsicC6zsEuEEXXtKvYWAjcWL7JnxuibBBspSJvHqDBJKcicdT8oJFDevtXuiaeianwb7WiaA139b1aOibtricAjk2vYf8ATeDBykpmTTTtf8/640?from=appmsg)

![](https://mmbiz.qpic.cn/mmbiz_jpg/K5924eDcsicDmhbGCxEnjBpnZhZ7v6T58HFr0ClbRtJ5pDBhhullGDB5u9D7cBy2oNOKxXqCBGmYt1Lwj30OAT0fhs8fE7Yzhge9gE7XicDiag/640?from=appmsg)

<<< 左右滑动见更多 >>>

**

**  ****榴莲忘返AIDD  **

供稿 | 刘思源**审稿 | 吉星

#### 目录

- BOAT 是一个高效的贝叶斯优化框架，它通过智能采样，在亲和力、可开发性等多个冲突目标间寻找帕累托最优解，从而替代了传统低效的抗体设计流程。
- ProteomeScan 提供了一套系统的全蛋白组对接方法，其核心是识别并过滤掉那些普遍结合的「滥竽充数」蛋白，从而大幅提高真实靶点发现的信噪比。
- SeFMol 通过强化学习实时引导扩散模型的生成过程，让分子在蛋白质口袋中动态调整构象，从而直接产出结合力强、成药性好且三维姿态正确的高质量候选药物。
- 研究者用 AlphaFold-Multimer 预测了整个果蝇蛋白质组的相互作用，并开发了一种新的局部评分方法 iLIS，它能更准确地识别真实结合界面，尤其擅长处理柔性蛋白，最终构建了一个经过遗传学验证的、包含大量新发现的结构互作图谱。
- 目前多数号称「靶点感知」的 AI 分子生成模型，在利用靶点三维结构信息上表现欠佳，其生成分子的质量和靶向性甚至不比从现有药物库里随机抽样更好。

![](https://mmbiz.qpic.cn/mmbiz_png/K5924eDcsicAvxXspHKicMHlbaVaeZtwgCekIUYJ4c7FzDMRvTpOQUibgqhkCpBLFH2nTzcAzDJtTaj9VzJhpW18fLM02piaewnLia9L1FGfL7Ns/640?from=appmsg)

## 1. BOAT 框架：用贝叶斯优化高效设计多目标抗体

![](https://mmbiz.qpic.cn/mmbiz_png/K5924eDcsicAqjQjS2E2af1vSIOH9fwKk0dsia31uagAZYwqjGXvicnUM4YobMpJdXVyXRXnBpmx1iceBB09RTBm01SJRJAKiau02QpGMice1PV9k/640?from=appmsg)

在抗体药物研发中，我们总是在玩一个「平衡游戏」。我们想要极高的亲和力，但这常常会牺牲分子的可开发性（developability）。我们希望它足够「人源化」以降低免疫原性，但这又可能影响其结合能力。传统的做法是串联筛选：先找到一大批有活性的分子，然后测试它们的可开发性，再看免疫原性。这个过程就像一个漏斗，每一步都会淘汰掉一批分子。这种做法最大的问题是，我们很可能在第一步就丢掉了那个综合性能最好的「最优解」。

**BOAT 的核心思路：从「串联漏斗」到「同步导航**」

阿斯利康的研究者们开发的 BOAT 框架，就是为了解决这个难题。它的想法很简单：我们能不能不按顺序筛选，而是在一开始就同时考虑所有目标，在一个巨大的可能性空间里直接找到那些「鱼与熊掌兼得」的分子？

BOAT (Bayesian Optimization for Antibody Design) 的工作原理，可以把它想象成一个勘探机器人。它不是对整个序列空间进行地毯式搜索，那太耗费资源了。相反，它会这样做：

- **建立一个初始地图**：它先测试少量几个序列，利用我们手头已有的各种计算预测工具（研究者称之为「神谕」，oracles）来评估这些序列的各项属性（比如亲和力、溶解度等）。这些工具可以是任何东西，从简单的序列打分到复杂的结构对接模型。
- **智能预测下一步**：基于这些初始数据，BOAT 会建立一个高斯过程（Gaussian Process）代理模型。这个模型不仅能预测任何一个新序列的表现，还能告诉我们这个预测的「不确定性」有多大。
- **平衡「探索」与「利用**」：接下来，BOAT 使用一个叫做「采集函数」（acquisition function）的策略来决定下一个该测试哪个序列。这个函数会巧妙地平衡两件事：去那些模型预测表现好的地方（利用，exploitation），以及去那些模型还不太确定、可能藏着惊喜的地方（探索，exploration）。

这个过程不断迭代，每一次测试都会让代理模型对整个序列空间的「地图」认知更精确，从而越来越快地逼近那个由最佳权衡点组成的「帕累托前沿」（Pareto front）。

**它如何处理真实的抗体序列**？

这里有一个很巧妙的工程设计。蛋白质序列是离散的，你不能把一个丙氨酸（Alanine）变成半个甘氨酸（Glycine）。传统的贝叶斯优化更擅长处理连续空间。为了解决这个问题，BOAT 在优化采集函数、寻找下一个最佳候选序列时，引入了遗传算法（Genetic Algorithm）。这确保了每一步提出的新序列都是由合法的氨基酸组成的，非常贴近实际应用。

在他们的案例研究中，研究者们优化了一个 VHH 抗体的互补决定区（CDR），目标是同时提升对两种相关抗原的结合力、人源化程度和蛋白语言模型打分。他们可以精确地限制只在 CDR1/2/3 区进行突变，甚至可以根据已有的实验数据，为每个位点指定一个允许突变的氨基酸「字典」。这种灵活性，对于一个真实的药物优化项目来说至关重要。

**效果如何？关键在于「神谕」的质量**

在基准测试中，BOAT 的表现很亮眼。与传统的遗传算法（如 NSGA-II）相比，在相同的计算预算（1000 次「神谕」调用）下，BOAT 能更快地找到一个质量更高的解集。当优化目标增加到 3 个或 4 个时，传统方法的性能明显下降，而 BOAT 依然稳健。

但这里有一个关键点，也是从业者最需要警惕的地方：BOAT 的成功高度依赖于你提供给它的预测模型（「神谕」）的质量。如果你的亲和力预测模型有漏洞，BOAT 会像一个顶级的黑客一样，无情地利用这个漏洞，找到一个在计算上得分极高、但在湿实验中却一败涂地的序列。

研究者在与一个生成式模型的对比中也发现了这一点。BOAT 倾向于找到一些「离群」的序列来最大化得分。为了解决这个问题，他们聪明地加入了一个来自蛋白大语言模型（ESM-2）的「自然度」打分作为额外的优化目标。这相当于一个正则化项，把搜索范围拉回到了更符合生物学规律的空间里。这给我们提了个醒：AI 工具不是万能的，它的性能上限，被我们输入的数据和模型的质量牢牢锁定了。

BOAT 并不是一个能自动产出完美抗体的「魔法盒」。它是一个强大的导航系统，能帮助我们在抗体设计的复杂、多维空间中，更高效地探索和发现最佳的「航线」。对于拥有成熟计算预测平台和数据的团队来说，这无疑是一个能显著加速先导化合物优化阶段的利器。

📜Title: BOAT: Navigating the Sea of In Silico Predictors for Antibody Design via Multi-Objective Bayesian Optimization**
🌐Paper: https://arxiv.org/abs/2404.13980**
💻Code: https://github.com/AstraZeneca/boat

![](https://mmbiz.qpic.cn/mmbiz_png/K5924eDcsicBuIYWg5j0C2p148pg5NheFT8tqweUA0t4eDKEu54FBicGOzqZibWVUBgcRdQnEiah0AibN49OJBYgdAMOlPURmzAQpure3wW39EXs/640?from=appmsg)

## 2. 全蛋白组对接，如何避开「万金油」蛋白的坑？

![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/K5924eDcsicB2YZ1f82GztxzcF72Ubq4chibmqewSxyfGTjlgBwR2CH21ECY37TPZzQfPZRp7RzoIwP6QOqSwqBic4dicRQrmTHTM19P35oZHzM/640?from=appmsg)

做表型筛选（phenotypic screen）的同行都懂这种痛：你找到了一个活性很强的苗头化合物，但它的靶点是什么？一个常规操作就是做反向对接，也就是把这个分子扔到整个蛋白质组里去「撞」，看看它最可能跟谁结合。这个想法很好，但在实践中，结果往往是一场灾难。你会得到一长串所谓的「潜在靶点」，其中大部分都是噪音。

问题出在哪？因为有些蛋白天生就是「交际花」，或者说「万金油」。它们结构上有一些大而灵活的口袋，对很多小分子都有一定的亲和力。比如细胞色素 P450 家族（CYP450s）、谷胱甘肽 S-转移酶（GSTs）、热休克蛋白 90（HSP90）等。在全蛋白组对接中，这些蛋白就像一群「万能胶」，几乎能粘住你扔进去的任何东西，从而严重干扰我们寻找真正的特异性靶点。这就好比在一大群人里找一个只对特定口味披萨感兴趣的人，结果总有那么几个「吃货」，不管什么披萨都说好吃，把你的数据全搞乱了。

这篇论文提出的 ProteomeScan 工具，就是为了解决这个头疼的问题。作者们没有去发明什么全新的对接算法，而是从一个更实际、更工程化的角度出发，建立了一套筛选和分析流程。

它的工作原理是这样的。

首先，他们建立了一个庞大的、经过严格筛选的人类蛋白质结构库，覆盖了 7,657 个独特的基因。这是进行全蛋白组对接的基础。

关键的一步来了。他们没有直接用你的化合物去对接，而是先用 20 个已知的、结构多样的药物分子对整个蛋白质组进行了一轮「摸底考试」。通过这次考试，他们就能识别出那些「学渣」——也就是无论面对哪种分子（考卷），都名列前茅的蛋白。一个蛋白如果在 20 种不同分子的对接中，有 n 次都排在亲和力前 m%，那它就会被贴上「滥交 (promiscuous)」的标签。

这个「滥交性」分析是整个工具的精髓。在用你的目标化合物进行正式对接后，ProteomeScan 会利用这个「黑名单」来过滤结果。效果立竿见影。数据显示，在移除了 166 个最「滥交」的蛋白后，已知药物 - 靶点对的回收率（Known Target Recovery, KTR）从 13.64% 提升到了 20.45%。别小看这 7% 的提升，在靶点发现这个大海捞针的工作里，这意味着信噪比的大幅改善，能帮我们省下大量无效的后续验证实验。

当然，一个好的计算工具不能只看一个对接分数。ProteomeScan 还加入了另一个至关重要的验证步骤：结合姿势分析。一个高分对接结果，如果分子只是「漂浮」在蛋白表面一个平坦、不可成药的区域，那这个结果基本没有意义。所以，研究者使用 fpocket 等工具来预先识别出蛋白上真正「可成药」的口袋，然后检查对接上的配体是不是真的落在了这些口袋里，以及占据了口袋多大的体积。只有分数高，且姿势合理的结果，才会被认为是高质量的命中。

为了证明这套方法的实用性，研究者还做了一些非常贴近药物研发实际的测试。比如，他们用 BRAF 抑制剂达拉非尼（dabrafenib）进行测试，ProteomeScan 成功地预测出它与临床相关的 BRAF V600E 突变体的结合要优于野生型。同样，对于 PIK3CA 抑制剂阿培利司（alpelisib），工具也准确地将 H1047R 和 E545K 等突变体排在了野生型之前。这种区分能力对于开发靶向特定突变体的精准药物来说，价值巨大。

最值得称赞的一点是，作者们非常坦诚地指出了这个方法的局限性。比如，对于像紫杉醇（paclitaxel）这样需要与微管蛋白组装体结合的药物，或者像 RMC-6236 这种需要形成三元复合物的「分子胶」，ProteomeScan 就无能为力了。这种清晰的边界划定，反而让使用者更加信赖这个工具。

最后，作者们把 ProteomeScan 的核心算法在 DeepChem 生态系统内开源，并提供了可通过云平台扩展的工作流程。这意味着，它不是一个只停留在纸面上的概念，而是一个我们可以实际部署到自己研发管线中的工具。

ProteomeScan 没有提出什么颠覆性的理论，但它用一套严谨、务实的工程化方法，漂亮地解决了一个长期困扰计算辅助药物发现的现实问题。它告诉我们，在处理复杂生物数据时，过滤和验证策略，往往比追求单一算法的极致性能更加重要。

📜Title: ProteomeScan: A Toolkit For Target Validation By Proteome-Wide Docking And Analysis**
🌐Paper: https://www.biorxiv.org/content/10.64898/2026.04.14.718479**
💻Code: https://github.com/deepforestsci/ProteomeScan

![](https://mmbiz.qpic.cn/mmbiz_png/K5924eDcsicBuIYWg5j0C2p148pg5NheFT8tqweUA0t4eDKEu54FBicGOzqZibWVUBgcRdQnEiah0AibN49OJBYgdAMOlPURmzAQpure3wW39EXs/640?from=appmsg)

## 3. AI 造药新范式：分子不再硬塞，而是在口袋里「生长」

![](https://mmbiz.qpic.cn/mmbiz_jpg/K5924eDcsicAyAcF1uV7AeXNqffcqticEohViajpe1FcGcpibSNUdkqppVCwItfIJbpKUJfrFHWwkdDsFutuyDAdHicC10382m4ZWTTcBxqgC1uA/640?from=appmsg)

做基于结构的药物设计（SBDD），就像是在一个形状奇特的瓶子里造船。传统 AI 生成模型的工作方式，是先把整艘船在瓶子外面造好，然后想办法塞进去。这个过程很别扭，经常因为船的某个桅杆角度不对就卡住了，导致大量生成分子其实根本放不进口袋里。

SeFMol 这篇工作换了个思路：为什么我们不能直接在瓶子里造船？

#### 扩散模型里加个「方向盘」：强化学习

SeFMol 的核心是一个扩散模型。你可以把扩散模型想象成一个从一团随机噪声（一堆无序的原子）开始，逐步把它「去噪」成一个完整分子的过程。

它的绝妙之处在于，研究者在这个去噪过程中加入了强化学习（Reinforcement Learning, RL）作为「方向盘」。每一步去噪，RL 都会评估当前原子位置和类型的「好坏」。如果调整后的分子在口袋里待得更舒服（比如形成了氢键，或者减少了与蛋白的碰撞），RL 就给一个「奖励」；反之，则给一个「惩罚」。

这样一来，分子生成就不再是一个盲目的过程。它被引导着，朝着与口袋完美互补的方向「生长」。这就是所谓的「半柔性」（semi-flexible）——分子不是一个生成后才被评估的僵硬成品，而是在形成过程中不断调整自身构象以适应环境。

#### 如何确保不「跑偏」？

只用 RL 引导，可能会产生一些结合力超强但化学结构很奇怪的东西。为了防止模型「为了拿高分而作弊」，研究者设计了一个「锚点」。

他们保留了一个预训练好的、不做 RL 优化的参考模型。在 RL 模型探索新构象的同时，这个参考模型会不断地把它往「正常的、化学上合理的」分子结构上拉。这就像一个经验丰富的老化学家在你旁边看着，既鼓励你创新，又在你画出五配位的碳时及时把你拉回来。这个机制在技术上通过 KL 散度正则化实现，确保了生成分子的化学合理性。

#### 结果怎么样？不看广告，看疗效

- **又快又好**：生成一个分子平均耗时 0.81 秒，速度很快。更关键的是，它直接生成的分子 Vina 对接分数平均达到 -7.23 kcal/mol，这是一个相当不错的水平。
- **姿态很准**：让我印象深刻的一个数据，是它直接生成构象的 Vina 分数和经过重新对接（redocking）后的分数，相关性高达 0.95。这意味着什么？意味着 SeFMol 生成的分子三维姿态非常可靠，基本不需要对接软件来「修正」。它学到的是真实的物理化学相互作用，而不只是在应付打分函数。
- **避免「偏科**」：很多模型只顾着优化结合力，生成的分子奇形怪状，成药性一塌糊涂。SeFMol 可以同时对 8 个常用理化性质（QED, LogP, TPSA 等）进行条件控制。你需要一个能过血脑屏障的分子？那就限制一下 TPSA。你需要一个口服吸收好的？那就把类药五规则加进去。这让它生成的分子不仅能结合，更有可能成为真正的药物。

#### 在真实靶点上的表现

研究者在 CDK2 和 ROCK1 这两个真实药物靶点上进行了测试，甚至用了 AlphaFold 预测的蛋白结构。结果显示，SeFMol 不仅能重现已知的核心相互作用，还能发现新的化学骨架和作用模式，比如在 ROCK1 靶点上提出了新的卤键或盐桥作用。这展示了模型真正的实用价值：不仅是复现，更是发现。

SeFMol 把强化学习巧妙地融入扩散模型的生成步骤中，从「生成 - 再 - 对接」的传统模式，转向了「边生成、边拟合」的新范式。这不仅提升了效率和准确性，也让我们离真正意义上的 AI 设计药物又近了一步。

📜Title: Steering semi-flexible molecular diffusion model for structure-based drug design with reinforcement learning**
🌐Paper: https://doi.org/10.1126/sciadv.ady9955

![](https://mmbiz.qpic.cn/mmbiz_png/K5924eDcsicDIdIGURwGBP36xBOtrnSmD2ubRPuI82TlAtnMAEwVVsnxIta3aE3zMiafQ9fcCUGhs6k6AF3kqMAKXyFQialg8rREuYqTU5mRpQ/640?from=appmsg)

## 4. AlphaFold 算蛋白互作，为何需要一把新「尺子」？

![](https://mmbiz.qpic.cn/mmbiz_jpg/K5924eDcsicC6zsEuEEXXtKvYWAjcWL7JnxuibBBspSJvHqDBJKcicdT8oJFDevtXuiaeianwb7WiaA139b1aOibtricAjk2vYf8ATeDBykpmTTTtf8/640?from=appmsg)

AlphaFold 改变了结构生物学，这已经是共识。下一步自然是把它用到极致，比如预测整个物种的蛋白质相互作用网络（Interactome）。最近，一个团队就对果蝇（_Drosophila_）干了这件事，他们跑了 150 万对蛋白质的预测，试图构建一个完整的结构互作图谱。但这件事的真正挑战，不是计算量，而是如何从海量预测结果中沙里淘金。

做过结构预测的人都知道，AlphaFold-Multimer 会给出一个叫 ipTM 的分数，用来评估整个复合物结构的置信度。分数高，通常意味着预测得不错。但问题是，ipTM 衡量的是全局结构。如果两个蛋白通过一小段柔性的肽链相互作用，主体结构域离得比较远，ipTM 分数可能就很低。这就好比你想判断两个人是不是在握手，但你只看他们全身的相对位置。如果他们只是伸长了胳膊，身体离得远，你可能就会误判他们没有接触。

这正是这篇工作的核心切入点。作者们意识到，我们需要一个「放大镜」，只看那只「手」握得怎么样，而不是管身体离得多远。

他们为此设计了一个新的评分标准，叫做 iLIS（integrated Local Interaction Score，整合局部相互作用分数）。它的工作原理很简单：

先定义一个「相互作用区域」，也就是两个蛋白链靠得足够近的局部空间（比如链间 PAE ≤ 12 Å）。在这个区域内，计算一个局部的置信度分数，这叫 LIS。

然后，再用更严格的尺子量一下，只看那些真正形成物理接触的原子（比如 Cβ 间距 ≤ 8 Å），再算一个分数，叫 cLIS。

最后，把 LIS 和 cLIS 通过几何平均整合起来，得到最终的 iLIS。这样做的好处是，它能有效惩罚那些「靠得很近但没接触」的假阳性情况。只有当一个区域既有整体较高的局部置信度，又有确凿的原子接触时，iLIS 分数才会高。

这把新「尺子」好用吗？作者们用一个包含 774 个阳性复合物和超过 6000 个阴性对照的基准集做了测试。结果显示，iLIS 的表现全面优于 ipTM。在 10% 假阳性率的阈值下，iLIS 的分数线是 0.223，而 ipTM 则需要 0.48。这意味着 iLIS 能用更宽松的条件捞出更多真家伙。

更具说服力的是，他们用这张图谱去「复现」已有的实验数据。结果很有意思：

这个差异恰恰说明 iLIS 做对了。因为 Y2H 主要检测直接的、二元的相互作用，而 AP-MS 钓上来的是一个「团伙」，里面有很多是间接的「围观群众」。iLIS 成功地区分了核心成员和外围成员。

最精彩的部分，是用遗传学数据做的验证。如果一个预测的结合界面在生物学上是重要的，那么破坏这个界面的氨基酸突变，就应该会导致功能异常，也就是产生表型。作者们从 FlyBase 数据库里找到了 2700 多个与表型相关的错义突变，然后把它们映射到自己的结构图谱上。

结果发现，这些致病突变显著地富集在 iLIS 预测的结合界面上。而且，这种富集效应在排除了进化保守性的干扰后依然存在。这说明，不仅仅是「重要的氨基酸容易被保守」，而是「位于结合界面的氨基酸」这个身份本身，就使其突变变得更有害。这是对预测准确性最有力的背书。

最终，这份名为 FlyPredictome 的资源库诞生了。它包含了大约 14.7 万个异源二聚体和 5400 多个同源二聚体。其中，超过 10 万个是全新的预测，没有任何文献或同源物种的证据支持。这对于果蝇研究社区来说，是一个巨大的、等待挖掘的宝库。

作者们还展示了如何使用这个图谱。他们构建了一个高置信度的网络，通过聚类分析，不仅重现了经典的信号通路（如 EGFR/Ras-MAPK, Notch, Wnt），还为一些功能未知的蛋白提供了具体的角色假设。比如，他们预测一个叫 CG3448 的蛋白是 DNA 连接酶 IV 的结合伴侣，功能上类似人源的 XRCC4。还有一个叫 CG12645 的蛋白，被预测会结合 Hippo 信号通路里的 Sav 和 Kibra。这些都是可以直接拿到实验室里去验证的、非常有价值的线索。

📜Title: FlyPredictome: A structural atlas of predicted protein-protein interactions in Drosophila**
🌐Paper: https://www.biorxiv.org/content/10.64898/2026.04.14.718529**
💻Code: https://github.com/flyark/AFM-LIS

![](https://mmbiz.qpic.cn/mmbiz_png/K5924eDcsicBuIYWg5j0C2p148pg5NheFT8tqweUA0t4eDKEu54FBicGOzqZibWVUBgcRdQnEiah0AibN49OJBYgdAMOlPURmzAQpure3wW39EXs/640?from=appmsg)

## 5. AI 靶向药物生成：多数模型为何跑不赢随机抽样？

![](https://mmbiz.qpic.cn/mmbiz_jpg/K5924eDcsicDmhbGCxEnjBpnZhZ7v6T58HFr0ClbRtJ5pDBhhullGDB5u9D7cBy2oNOKxXqCBGmYt1Lwj30OAT0fhs8fE7Yzhge9gE7XicDiag/640?from=appmsg)

做药物研发，我们经常会看到一种模式，研究者用一个新算法生成了一堆分子，然后挑出几个对接分数最高、构象看起来最漂亮的作为成果展示。这就像一个德州神枪手，先随意开枪，然后在弹孔最密集的地方画上靶心，宣称自己枪法精准。这篇论文开篇就指出了这个要害问题：很多所谓的「靶点感知」的分子生成模型，可能根本没有真正「理解」靶点结构，只是在玩这种先开枪后画靶的游戏。

为了搞清楚这些模型是真本事还是假把式，研究者开发了一个叫 TarPass 的基准测试集。这个测试集非常实在，专门用来进行公平的、有靶点依据的评估。

它包含了 18 个研究得很透彻的药物靶点，都是制药界的老朋友，比如各种激酶。每个靶点都有专家标注的关键相互作用信息，这是做药化的人最看重的东西。同时，它还从 BindingDB 数据库里为每个靶点匹配了大约 1000 个已验证的活性分子。最关键的是，TarPass 还引入了一个基线参照：从 ChEMBL 数据库里随机抽取一批类药性分子。这就好比一场考试，不仅要看你得了多少分，还要看你是不是比蒙对答案的学生强。

这个测试集在设计上也很严谨，为了防止模型「作弊」，所有靶点结构和数据都选自 2019 年之后，并且刻意避开了像 CrossDocked2020 和 PDBbind 这类常见的训练集，最大限度地减少了数据泄漏。

研究者选了 15 个有代表性的模型，把它们分成三类：

- **非 3D 模型**：不直接处理靶点三维结构，比如只看氨基酸序列。
- **3D 原位（in situ）生成模型**：在靶点口袋的 3D 空间里直接「长」出分子。
- **基于优化的模型**：先生成一个分子，再根据对接分数等指标迭代优化。

然后，让这些模型针对每个靶点生成最多 1000 个分子，用一套统一的流程进行分子对接，再从两个维度进行评分：一是分子与蛋白的相互作用（PLIs），二是分子本身的质量（比如有效性、类药性、合成可及性）。

结果非常发人深省。

首先，那些看起来最高级的 3D 原位生成模型，在对接分数和相互作用指标上，平均只比非 3D 模型好一点点。更让人意外的是，很多 3D 模型在不少靶点上的表现，甚至还不如从 ChEMBL 库里随机抽样的结果。这意味着，你费尽心机开发的复杂 3D 算法，效果可能还不如简单粗暴地筛选一下现有分子库。

对接分数高不代表一切。研究者用了一个更严格的标准：关键相互作用的复现率。这就像考试里的简答题，不仅答案要对，解题步骤也得对。结果显示，就算是把已知的活性分子重新对接到靶点里，也只能复现大约 51% 的关键相互作用。而绝大多数 AI 模型在这项测试中的表现基本等同于随机猜测。只有少数几个模型（如 DrugFlow 和 MolCraft）的表现接近真实活性分子的水平。

问题出在哪？主要瓶颈在于生成分子的三维构象。许多 3D 模型生成的初始构象存在严重的原子碰撞（steric clashes），分子摆放的位置从一开始就错了。这就好比你想把一把钥匙插进锁里，如果连钥匙拿的方向都错了，那这把钥匙设计得再精妙也没用。对于一些特殊的靶点，比如需要与金属离子（如 HDAC6 中的锌）配位的口袋，很多模型更是束手无策，要么不支持，要么处理得一团糟。

最后，研究者发现了一个普遍的权衡困境。

所以，我们面临一个两难选择：要么得到一个能塞进口袋但几乎无法合成的分子，要么得到一个化学性质优良但完全不匹配靶点的分子。这两种都不是我们想要的。

文章最后提出了一个务实的后处理策略：一个多层虚拟筛选流程。先用硬性标准（比如相互作用、成药性）过滤掉 90% 的生成分子，再用软性标准（比如化学家的经验、聚类分析）从剩下的 10% 中筛选出二三十个候选分子。这个方法确实能富集到一些有潜力的化合物，但它也清楚地表明，后处理只是一个补丁。它无法替代生成模型本身在构象准确性、相互作用保真度和化学合理性上的改进。

这篇论文告诉我们，AI 制药，特别是分子生成领域，还有很长的路要走。我们需要的是能同时理解蛋白质三维空间和药物化学基本规则的模型，而不是只会画靶心的「神枪手」。

📜Title: Revisiting Target-Aware de novo Molecular Generation with TarPass: Between Rational Design and Texas Sharpshooter**
🌐Paper: https://doi.org/10.1002/advs.75411
— 完 —

![](https://mmbiz.qpic.cn/mmbiz_png/K5924eDcsicAZ4IwDAMy6sicauZnWRN1e0jj43ckbooicezr0n3XlMibnzDvMhiaIpib9KZP6ewg4pfAz1dLTIhaS6kQ5cPwbic53Ovkbk6e7LnumI/640?from=appmsg)
