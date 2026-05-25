---
source_url: https://arxiv.org/abs/2604.19562
ingested: 2026-05-21
sha256: 067dab431ae8fd038320422caf3b48b3faa226e26a3e2c36c18b97663bd4e834
citation: "Navarro C, Tholke P, de Fabritiis G. Structure-guided molecular design with contrastive 3D protein–ligand learning. arXiv:2604.19562, 2026."
domain: ai-drug-discovery
tags: [contrastive-learning, protein-ligand, virtual-screening, drug-design, generative-model, se3-equivariant, transformer]
---

# arXiv 2026 | 从 DrugCLIP 到 MCLM：对比学习在 AIDD 里能不能从"检索"走到"生成"

> **来源：** 陷入鞍点

---

![](https://mmbiz.qpic.cn/sz_mmbiz_png/Lgu1K9Ehrgdg03vhsFuCooliabwT2VLwDoLdb1YyuTC6rM4zY4w76KZdR22uszLbCLEL25nlicrbNUfuQiciaSJksw/640?from=appmsg)

**来源：arXiv 2026 (2604.19562)，Acellera Labs / Universitat Pompeu Fabra，Gianni de Fabritiis 团队。在他们之前的 TorchMD、ACEMD 等分子动力学引擎之外，这次把战线推到了 AIDD 的 representation + generation。

![](https://mmecoa.qpic.cn/mmecoa_png/NYQt9rr8A02C3T5QEAYDkicr7qUALPHCSStuKSuobXBHeoLkiaRVbicicYUibISUmZrzuuQXwVGBJc3W2zZOOo6iaECg/640?from=appmsg)

01对比学习这把火，在 AIDD 里已经烧了三年

![](https://mmbiz.qpic.cn/sz_mmbiz_png/a5yW0MNUS2B35YuvvOcHKH4Nyc8qkXE92WGR1H2T2y4EV3SSq0OuwIKTOw0akzKow9ibibGyLJmw5fywKWgrwYRg/640?from=appmsg)

如果让你回忆过去三年 AIDD 领域最"出圈"的几个范式，对比学习（contrastive learning）肯定排得进前三。记忆中，最早出圈的是DrugCLIP（NeurIPS 2023），它把 CLIP 的范式直接搬到虚拟筛选——蛋白口袋一个塔、配体一个塔，对比学习对齐两个嵌入空间，筛选时直接做向量检索，速度比传统 docking 快几个数量级。这个工作之后，contrastive learning 在 AIDD 里就像被点了把火：- CLEAN（Science 2023）用对比学习去预测酶功能（EC number），把"序列→功能"这种本来要靠规则和同源性的任务变成了向量空间里的 nearest neighbor。
- S-PLM把蛋白序列和结构对比起来训练一个统一的 protein language model，让序列编码器隐式获得结构感知。
- ProTrek更激进，把蛋白质的序列、结构、自然语言功能统一进一个三模态对比空间，任意两种模态之间都能互检。
这条线的共识很清晰：对比学习是个非常好用的"对齐工具"，用来把两种或多种异构信号（口袋/配体、序列/结构、结构/语言）拉到同一个空间里，让下游任务统统变成向量检索。但仔细盘一下，你会发现一个现象：这些工作绝大多数的下游任务都是discriminative的——筛选、检索、分类、ranking。把对比学习用到generative任务上的工作，在 AIDD 里相对罕见。

![](https://mmecoa.qpic.cn/mmecoa_png/NYQt9rr8A02C3T5QEAYDkicr7qUALPHCSStuKSuobXBHeoLkiaRVbicicYUibISUmZrzuuQXwVGBJc3W2zZOOo6iaECg/640?from=appmsg)

02从 CV 借范式：对比 + 生成的两条路

![](https://mmbiz.qpic.cn/sz_mmbiz_png/a5yW0MNUS2B35YuvvOcHKH4Nyc8qkXE92WGR1H2T2y4EV3SSq0OuwIKTOw0akzKow9ibibGyLJmw5fywKWgrwYRg/640?from=appmsg)

这件事在 CV 里其实已经走了好几轮。把 CLIP 这类对比模型和扩散/自回归生成模型组合起来，主要有两条路线：- 第一条是 training-free 的引导路线。代表是早期的 CLIP-guided diffusion——预训练好的 CLIP 不参与训练，只在采样时给扩散模型提供梯度信号，把生成往文本描述的方向"推"。优点是即插即用、不用重训生成器；缺点是引导信号弱、生成质量看采样器调参。
- 第二条是 training-based 的条件路线。代表是 DALL·E 2（unCLIP）和 Stable Diffusion——把 CLIP 嵌入直接作为条件输入，从头训练一个生成器去拟合 "嵌入 → 像素" 的条件分布。优点是条件信号强、生成质量稳定；缺点是必须重训整套 pipeline，对比模型的表征质量直接决定上限。
那么 AIDD 里有人尝试过这两条路吗？第一条路在分子领域的尝试零零星星——比如把 docking score 作为引导信号去 guide 扩散模型，但这本质上不是对比学习的用法。第二条路里，最接近的工作是COATI（2024，Pfizer）：用对比学习把 SMILES 和分子图对齐，再训一个自回归 decoder 从对比嵌入生成 SMILES。但 COATI 是纯 2D 的，没有 3D 结构、没有口袋信息，更像是"分子翻译器"。真正把"3D 口袋-配体对比空间" + "条件式自回归生成"组装在一起、并且还专门加了 "商业化学空间" 这一层 steering 的工作，本文应该是第一个把这条工程链路跑通的。这就是这篇 arXiv 2026 工作的位置：它不是在算法层做天才式的突破，而是把 AIDD 圈子已经熟悉的几个零件——SE(3) 等变 transformer、CLIP 式对比、Llama2 自回归 decoder、dataset embedding token——以一种相当干净的方式拼到了一起，并且认真回答了一个工业界一直关心的问题：生成出来的分子，能不能尽量长得像我货架上能买到的东西？

![](https://mmecoa.qpic.cn/mmecoa_png/NYQt9rr8A02C3T5QEAYDkicr7qUALPHCSStuKSuobXBHeoLkiaRVbicicYUibISUmZrzuuQXwVGBJc3W2zZOOo6iaECg/640?from=appmsg)

03整体看：输入是什么，输出是什么

![](https://mmbiz.qpic.cn/sz_mmbiz_png/a5yW0MNUS2B35YuvvOcHKH4Nyc8qkXE92WGR1H2T2y4EV3SSq0OuwIKTOw0akzKow9ibibGyLJmw5fywKWgrwYRg/640?from=appmsg)

把方法部分拆成两个独立但相关的子系统理解，会清楚很多。****子系统 A：CLIPP-SET 对比编码器- 输入：配体的 3D 构象（atomic numbers + 坐标）或蛋白口袋的 3D 结构
- 输出：256 维的共享嵌入向量
- 训练目标：把同一个 ligand-pocket 复合物的两个塔的输出在嵌入空间拉近，把不同复合物拉远
- 用途：零样本虚拟筛选 + 大库检索（pocket → ligand 或 ligand → ligand）
****子系统 B：MCLM 多模态化学语言模型- 输入：CLIPP-SET 给出的 3D 嵌入 + 一个 dataset token + 已生成的 SMILES 前缀
- 输出：下一个 SMILES token（自回归）
- 训练目标：标准的 next-token prediction
- 用途：基于 pocket 或 ligand 结构生成 target-specific 分子，并通过 dataset token 控制生成的化学空间

![](https://mmbiz.qpic.cn/mmbiz_png/b4dmDNrOlLtuibodNqiaFjOo8A1qMOm2d2u70PdEJf0uLg1rbQvsgpWCBoZZupHuRekibxFiagfLmwtXtyfF3iccicf8N8wp99AYgtaD7ZhjGicicQ0/640?from=appmsg)

▲ Figure 1｜CLIPP-SET 的对比训练流程。一个 batch 里有 n 对 ligand-pocket 配对，分别送进各自的 SET 编码器，得到投影向量 L*i 和 P*i。损失函数是相似度矩阵的对角对齐——对角上的正样本对要拉近，非对角的拉远。但这里有个要命的细节：在 SBDD 数据里，同一个 pocket 经常对应多个 active ligand，所以"非对角的都是负样本"这个 CLIP 默认假设会出错。作者用了CF-InfoNCE（Collision-Free InfoNCE）来处理这个问题——同 batch 内如果有多个 ligand 共享同一个 pocket，动态选亲和力最强的那个作为该 pocket 的真正正样本。这是一个看起来不起眼但实际很重要的修正，本质上承认了 "drug-target interaction 不是一对一的双射" 这个化学事实。接下来分别看两个子系统的关键设计。

![](https://mmecoa.qpic.cn/mmecoa_png/NYQt9rr8A02C3T5QEAYDkicr7qUALPHCSStuKSuobXBHeoLkiaRVbicicYUibISUmZrzuuQXwVGBJc3W2zZOOo6iaECg/640?from=appmsg)

04CLIPP-SET：又一个 SE(3)-equivariant transformer，区别在哪？

![](https://mmbiz.qpic.cn/sz_mmbiz_png/a5yW0MNUS2B35YuvvOcHKH4Nyc8qkXE92WGR1H2T2y4EV3SSq0OuwIKTOw0akzKow9ibibGyLJmw5fywKWgrwYRg/640?from=appmsg)

3D 分子表示这块，等变 GNN（EGNN、SchNet、Equiformer 等）几乎是默认选择。但作者选择了基于 transformer 的路线，并且专门设计了一个SET（Scalable Equivariant Transformer）架构。为什么不直接用现成的 Equiformer 或者 TorchMD-Net？作者的动机很务实：要在 ~287M 构象数据集上预训练，并且后续要在 1.29M ligand-pocket 对上做对比训练，算力可扩展性比表达力更重要。SET 的核心创新是把"距离感知 + 反射等变"塞进了标准的 dot-product attention，从而能直接复用 FlashAttention 这类高度优化的实现。说白了：牺牲一点点理论上的等变完备性，换工程上的可扩展性。这个权衡在 AIDD 这种"数据规模碾压架构精巧度"的场景下，往往是对的。但代价是，如果你做的是需要精细几何感知的下游任务（比如 binding pose 的精确预测），SET 可能不是最佳选择。数据组装也值得说几句：- 配体编码器：在 287M 构象的大杂烩数据集上做 MLM 预训练（PubChem + Enamine Diversity + Mcule + ChEMBL + GEOM-drugs）。注意 Mcule 和 Enamine 的构象是 RDKit ETKDG + MMFF 跑出来的，不是实验构象。
- 口袋编码器：在 ProFSA 的 5.5M fragment-pocket pairs 上预训练。
- 对比训练：在 SIU 数据集上联合微调，1.29M 个 ligand-pocket pairs，9662 个唯一口袋。
SIU 这个数据集要重点说一下：里面的 binding pose 是用 consensus docking 生成的，不是晶体结构。也就是说，CLIPP-SET 学到的 "binding conformation" 在统计意义上是 "docking 给出的 plausible pose"，不是物理真实结构。这个细节在后面讨论 limitation 时还会回来。

![](https://mmecoa.qpic.cn/mmecoa_png/NYQt9rr8A02C3T5QEAYDkicr7qUALPHCSStuKSuobXBHeoLkiaRVbicicYUibISUmZrzuuQXwVGBJc3W2zZOOo6iaECg/640?from=appmsg)

05MCLM：把 3D 嵌入塞进 Llama 的 prompt 里

![](https://mmbiz.qpic.cn/sz_mmbiz_png/a5yW0MNUS2B35YuvvOcHKH4Nyc8qkXE92WGR1H2T2y4EV3SSq0OuwIKTOw0akzKow9ibibGyLJmw5fywKWgrwYRg/640?from=appmsg)

MCLM 的设计可以一句话概括：把 CLIPP-SET 的 3D 嵌入当成一个特殊的 visual token，prepend 到 SMILES token 序列前面，然后用一个 Llama2-style 的 decoder 跑标准的 next-token prediction。

![](https://mmbiz.qpic.cn/mmbiz_png/b4dmDNrOlLtib1tX0WhibHicm5xSzX4OokDn6OpB3fBtJQagVQuNdBicI8sYcGu9DLUPunNsdOHVed9PVo87coXFQO6aasGQOmjibgCoYwWicG9C0/640?from=appmsg)

▲ Figure 2｜MCLM 的整体架构。输入序列长这样：[dataset*token, projected*3D*embedding, SMILES*token*1, SMILES*token*2, ...]。3D context 来自冻结的 CLIPP-SET 编码器（pocket 或 ligand 都行），通过一个可训练的线性投影 W*P 映射到 decoder 的 hidden dimension。dataset token 是从 PubChem / ChEMBL / Enamine / Mcule 这些来源 DB 里挑一个，告诉模型 "我希望你生成的分子风格往这个库靠"。整个 decoder 用标准 cross-entropy 训练，跟训 LLaMA 一模一样——这正是这个设计的优雅之处：所有 LLM 工程化的红利（FlashAttention、RoPE、KV cache、推理优化）都能直接复用。这里有几个非平凡的设计选择值得讨论：- 训练时只用 ligand embedding，推理时既能用 ligand 也能用 pocket。这是个赌博。MCLM 在训练时压根没见过 pocket embedding 作为条件，但因为 CLIPP-SET 已经把 pocket 和 ligand 拉到了同一个嵌入空间，作者赌"对齐良好"这件事——pocket embedding 在嵌入空间里大致落在它能 bind 的 ligand 们的"质心"附近，所以 decoder 见到 pocket embedding 时，能猜到大致该生成什么风格的分子。**后面的实验结果会告诉我们这个赌博赢了一半：pocket-conditioned generation 能用，但 ligand-conditioned 的预测亲和力明显更高（pIC50 6.31 vs 5.89）。这个 gap 就是嵌入空间对齐不完美带来的代价。**
- dataset token 的设计。这是这篇论文里最实用的工程点之一。同样的 3D 条件，配上不同的 dataset token，能让模型生成"PubChem 风"、"ChEMBL 风"或"Enamine REAL 风"的分子。工业界为什么需要这个？因为绝大多数生成模型最大的痛点不是"生成不出能 bind 的分子"，而是"生成出来的分子要么没法买、要么没法合成"。Enamine REAL 这种 readily-accessible 的库为什么能在工业界席卷？因为 99% 的下游问题都被 "现货" 二字解决了。dataset token 的本质是领域条件式生成——把"化学空间分布"这个软约束变成一个 1-token 的 hard switch。简单粗暴，但效果立竿见影。**
- SMILES 而不是 3D point cloud。这是和 SBDD 扩散模型（Pocket2Mol、TargetDiff、DiffSBDD、PocketXMol、MolCRAFT 等）的根本路线分歧。3D 扩散模型直接在原子点云空间里生成，理论上能更精确地利用口袋几何，但代价是：训练数据稀缺（只有晶体结构能用）、几何错误（价键违反、应变构象）需要后处理、生成的分子很容易"游离"在已知化学空间之外（合成起来困难）。走 SMILES 路线，相当于把"几何精确性"换成了"化学合理性 + 训练数据规模 + 推理效率"。在工业落地的视角下，这笔交易是划算的——尤其是在 dataset token 进一步约束化学空间分布之后。

![](https://mmecoa.qpic.cn/mmecoa_png/NYQt9rr8A02C3T5QEAYDkicr7qUALPHCSStuKSuobXBHeoLkiaRVbicicYUibISUmZrzuuQXwVGBJc3W2zZOOo6iaECg/640?from=appmsg)

06实验结果：值得拆开看的几个发现

![](https://mmbiz.qpic.cn/sz_mmbiz_png/a5yW0MNUS2B35YuvvOcHKH4Nyc8qkXE92WGR1H2T2y4EV3SSq0OuwIKTOw0akzKow9ibibGyLJmw5fywKWgrwYRg/640?from=appmsg)

我不会逐一覆盖论文里的所有数字，只挑三个有信息量的发现讲清楚。****发现一：LIT-PCBA 上的"早期富集强、整体排序弱"这是 contrastive 模型的一个典型签名。看 Table 1 的数字：- BEDROC、EF(0.5%)、EF(1%)：CLIPP-SET 都是最优
- AUROC：53.76%，明显输给 Gnina (60.93%)、BigBind (60.80%) 等 docking-based 方法
- EF(5%)：2.06，输给所有传统方法
这个 pattern 几乎是 contrastive embedding model 的"职业病"。它在嵌入空间里能把"最像 active 的那一小撮"和其他东西分得很开（所以 top-k 富集很好），但当你把 ranking 列表拉长到 5%，它的"广义判别能力"就跟 docking 拉不开差距了。对实际项目的含义：如果你做 hit identification、想从百万级库里捞 top-1000 候选，CLIPP-SET 这类对比模型的 high-precision-low-recall 特性正好对上号。但如果你做的是 binding affinity ranking、想精确排序最后剩下的几百个分子去做 prospective validation，单靠对比嵌入是不够的，你还得叠 docking + scoring。****发现二：50 亿级库的检索——pocket 给"多样性"，ligand 给"scaffold hopping"

![](https://mmbiz.qpic.cn/sz_mmbiz_png/b4dmDNrOlLtIB7mXjslpthicURIpCWDWBvI6TgLY7ygs1ULFlJ58uXeu1TJu37jqvV8B19xHVbnMxtMqOQmBzUK8x1hh3t90ghe5q8KVYsaI/640?from=appmsg)

▲ Figure 3｜三种检索策略的平均指标对比（15 个 LIT-PCBA target，每 target 检索 100 个分子，用 Boltz-2 打分）。关键观察：（1）CLIPP-SET ligand search 的 shape similarity 和 affinity probability 都最高，但 chemical similarity 显著低于 Morgan FP search——这正是 scaffold hopping 想要的"形状像但化学骨架不同"。（2）CLIPP-SET pocket search 的 chemical similarity 最低（0.11），diversity 最高，意味着它在不参考已知 ligand 的前提下能找到化学上最"野"的候选。（3）Morgan FP 拿到了最高的 affinity probability，但代价是分子都是已知配体的近亲——chemical similarity 0.36，diversity 只有 0.59。这张图传达的核心信息是三种检索方式各有人设：- Morgan FP：保守派。给我一个已知 active，我给你一堆它的"亲戚"。适合 ME-too 优化、对已知 IP 做 me-better。
- CLIPP-SET ligand：scaffold hopper。给我一个已知 active，我给你一堆"形状/药效团相似但化学骨架不同"的候选。适合 IP 突围、规避专利空间。
- CLIPP-SET pocket：盲探员。我连 active 都不需要，光看口袋几何就给你一堆可能 bind 的多样化分子。适合冷启动项目、orphan target、cryptic pocket。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/b4dmDNrOlLsv6B09HetWylXibYctb0khibRYb7DUjG8dUGeI5I8Uf4KYDiapgkB7ic8vjyoJyffiaBFzjhmsm6ibKpgZZeaIFOJuae6MrlkKM1E4U/640?from=appmsg)

▲ Figure 4｜(a) 二维分布图最有信息量：Morgan FP 检索结果集中在 "化学相似度高、形状相似度高" 的右上角，本质就是"近亲"；CLIPP-SET ligand 检索结果集中在 "化学相似度低（~0.2）但形状相似度依然不低（~0.5）" 的左中偏上区域——这就是论文宣称的 scaffold hopping 区域。(b) ADRB2 (3p0g) 的具体案例：CLIPP-SET ligand 检索到的分子 Tanimoto 只有 0.200（化学上完全不同），但 Boltz-2 预测 pIC50 是 7.18（最高），shape similarity 0.523（中上）。Morgan FP 那个分子结构上跟参考 ligand 像得多（Tanimoto 0.431），pIC50 却只有 6.46。(c) 是两条 pipeline 的架构对比图——CLIPP-SET 通过 SE(3) 等变 transformer 编码 3D 结构后做 cosine 检索，Morgan 走 2D circular fingerprint + Tanimoto。需要泼一盆冷水的是：这里所有的 pIC50 都是 Boltz-2 预测的，不是实验值。Boltz-2 是个相当不错的 affinity prediction 模型，但用一个 ML 模型评估另一个 ML 模型生成的分子，评估集和被评估方法之间难免存在某种"特征同源性"。如果 CLIPP-SET 的嵌入空间偏好的几何/化学特征恰好也是 Boltz-2 学到的"binding-relevant features"，那 Boltz-2 给高分就不一定意味着真实活性。这是整篇论文最大的一个评估 caveat，后面会细讲。****发现三：MCLM 真的"超越"了 50 亿规模的 Enamine REAL

![](https://mmbiz.qpic.cn/mmbiz_png/b4dmDNrOlLuo80lrgrkt3qzFU9gPmuJT2gibaNNkCClnqwdV0z7BfKdPLeuo8vxwFjDgkMDseG8PpaKPynUZhzVTI7nq01H88T6GCewV4CPU/640?from=appmsg)

▲ Figure 5｜15 个 LIT-PCBA target 上的 per-target 比较。(a) Predicted affinity probability：MCLM (ligand) 在 15 个 target 里有 12 个排第一。MCLM (pocket) 大致和 CLIPP-SET 两种 search 持平，意味着在没有已知 active 的纯 pocket-conditioning 场景下，生成模型并没有显著优于检索。(b) 3D shape similarity：ligand-conditioned 的两个方法（MCLM ligand 和 CLIPP-SET ligand search）显著领先于 pocket-based 方法和 Morgan FP，且生成模型再次优于检索模型。这说明了一个重要的结论：当你已经有一个 active reference 时，"用嵌入做检索" 是 "用嵌入做生成" 的下界——生成模型能在比 50 亿规模 Enamine 大得多的隐式化学空间里采样。但是当你没有 active reference、只有 pocket 时，这种优势就不明显了。这个结论的核心 takeaway 是：生成模型相比检索的优势，依赖于条件信号的"信息密度"。Ligand embedding 携带了更多 pharmacophore + geometry 信息，所以 decoder 能在隐式空间里精细地"插值"出更优的候选。Pocket embedding 信息密度较低（毕竟 cross-modal 对齐不完美），decoder 的优势就被压缩了。****发现四：dataset token 真的有用，但要小心"作弊"

![](https://mmbiz.qpic.cn/sz_mmbiz_png/b4dmDNrOlLtlee4eRczYSlibH0JOvxDQyaXicDtNXOSbBsPzjc0W9QVCuHu7FI8xPbHvMtAf6VYJiaibMxg2HAzZvEGO8iaMKjws4aWc3JR7j3Jo/640?from=appmsg)

▲ Figure 6｜(a) 每个 target 上，生成的分子到 Enamine REAL 最近邻的 Tanimoto 相似度。pocket-conditioned 生成（红色圆点）几乎在所有 target 上都比参考 ligand（蓝色菱形）更接近 Enamine 分布，验证了 dataset token 的 steering 效果。ligand-conditioned 生成（深红色方块）变化更大——当参考 ligand 本身离 Enamine 近时，模型生成的分子和 Enamine 重合度极高（最高接近 1.0，意味着可能直接命中库里的分子）；当参考 ligand 离 Enamine 远时，模型还是会努力把生成往 Enamine 方向拉。(b) 消融实验最有说服力：同样的 pocket 嵌入，不开 Enamine token（灰色）vs 开 Enamine token（红色）。开 token 后整个分布右移，最高相似度处的尖峰是"生成的分子和 Enamine 库里某个分子精确匹配"。（原文 Figure 6）dataset token 工作得不错。但 (b) 图里那个 1.0 处的尖峰其实是个double-edged sword：- 正面看：模型确实学会了 Enamine 的化学空间分布，能生成 "买得到" 的分子。
- 负面看：精确匹配意味着模型在某种程度上是在从训练分布里 retrieve，而不是真正"创造"。如果你是为了找 IP novel 的化合物去 Enamine 之外，这个 token 反而会把你 trap 在 Enamine 里。

![](https://mmecoa.qpic.cn/mmecoa_png/NYQt9rr8A02C3T5QEAYDkicr7qUALPHCSStuKSuobXBHeoLkiaRVbicicYUibISUmZrzuuQXwVGBJc3W2zZOOo6iaECg/640?from=appmsg)

07批判性评估：哪些是真创新，哪些要打折扣

![](https://mmbiz.qpic.cn/sz_mmbiz_png/a5yW0MNUS2B35YuvvOcHKH4Nyc8qkXE92WGR1H2T2y4EV3SSq0OuwIKTOw0akzKow9ibibGyLJmw5fywKWgrwYRg/640?from=appmsg)

****值得肯定的把"多模态对齐"作为 SBDD 生成的统一接口，这个工程视角很成熟。把 3D 信息压缩成 256 维向量再丢给 SMILES decoder，相当于把"几何感知"和"化学语法"解耦——encoder 和 decoder 可以独立升级，工程化优势明显。dataset token则是这套设计里最实用的一个工业化设计点：把"化学空间归属"做成 1-token 的 hard switch，比 SA score 后过滤或 chemistry-aware reward 简单优雅得多，可以直接进生产环境。ligand-side 的 scaffold hopping能力也是真实可信的——Figure 4(a) 那张分布图比那些 affinity probability 数字硬得多，因为 shape/chemical similarity 都是确定性计算，不依赖另一个 ML 评估器。****需要直面的局限- 评估几乎完全依赖 Boltz-2，存在评估循环风险。这是最大的方法论隐患。如果 CLIPP-SET 嵌入偏好的几何/化学特征和 Boltz-2 学到的 "binding-relevant features" 高度同源（很可能），那 Boltz-2 给 CLIPP-SET 检索结果打高分并不能严格证明湿实验也会高活性。作者完全没做任何 docking-based 的交叉验证，哪怕跑一遍 GNINA 或 Vina 都好。LIT-PCBA 那张表用的还是真实 active/inactive 标签，到了 Enamine REAL 检索完全切到 Boltz-2，这个评估口径的切换论文没充分讨论。**
- CLIPP-SET 训练用的 SIU pose 是 docking 出来的，不是晶体结构。这意味着 CLIPP-SET 学到的 "binding conformation" 在统计意义上等价于"docking 觉得合理的 pose"。这个 noise 会被 contrastive learning 吸收进嵌入空间，但具体吸收成什么样、对哪种 target 影响最大，没给分析。**
- 推理时的"64 个构象选最优"有点掩耳盗铃。本质是 inference-time conformer search——把"找 binding-compatible conformer"这个困难子问题用穷举绕过去了。50 亿规模库检索时每分子要算 64 次嵌入取 max，工程化代价不小，论文没给具体数据。**
- 缺少和 SBDD 扩散模型的直接对比。对标的 baseline 是 docking 和早期对比模型，但 pocket-conditioned generation 赛道 2024-2026 最热的是 Pocket2Mol、TargetDiff、DiffSBDD、PocketXMol、MolCRAFT、FLOWR 这些扩散/流匹配方法。两条路产出物不一样不假，但完全不比就让读者很难判断 MCLM 在这个细分任务上到底处于什么位置。**
- "scaffold hopping" 用 Morgan FP Tanimoto 论证太粗糙。Tanimoto < 0.3 不一定等于 scaffold 真的变了——加个甲基也能让数字大幅下降。真正的 scaffold hopping 该看 Bemis-Murcko 拓扑变化或 ring system 替换的统计，论文没给。**
- 完全没有 prospective validation。这是这类工作的通病。所有"提升"都是 in-silico → in-silico 的递归。

![](https://mmecoa.qpic.cn/mmecoa_png/NYQt9rr8A02C3T5QEAYDkicr7qUALPHCSStuKSuobXBHeoLkiaRVbicicYUibISUmZrzuuQXwVGBJc3W2zZOOo6iaECg/640?from=appmsg)

08延伸思考

![](https://mmbiz.qpic.cn/sz_mmbiz_png/a5yW0MNUS2B35YuvvOcHKH4Nyc8qkXE92WGR1H2T2y4EV3SSq0OuwIKTOw0akzKow9ibibGyLJmw5fywKWgrwYRg/640?from=appmsg)

从 DrugCLIP 到 MCLM，对比学习在 AIDD 里的路径很清晰：先解决 retrieval，再解决 generation。但下一步还有几个关键链路没走通：retrieval-augmented generation——把对比检索出的 top-k 类似物作为生成器的额外 context（CV 里 RDM、KNN-Diffusion 那种思路），比现在的 single-embedding-condition 信息密度高得多；连续维度的 controllability——dataset token 只能切换离散域，如果嵌入空间能在 ADMET、合成步数、专利空间这些连续维度上做 disentangled control，会更贴近 medchem 实际工作流；嵌入空间的可解释性——CLIPP-SET 的 256 维到底学到了什么，哪些维度对应 H-bond donor pattern、hydrophobic pocket size，目前完全是黑箱。我的判断是：对比学习作为 AIDD 的"通用对齐工具"已经成熟，下一步真正拉开差距的是"如何把对比空间作为多任务接口"——retrieval、generation、optimization、ranking 都跑在同一个嵌入空间里。MCLM 是这个方向上比较干净的一步，但还远没到终点。

![](https://mmecoa.qpic.cn/mmecoa_png/NYQt9rr8A02C3T5QEAYDkicr7qUALPHCSStuKSuobXBHeoLkiaRVbicicYUibISUmZrzuuQXwVGBJc3W2zZOOo6iaECg/640?from=appmsg)

09Q&A

![](https://mmbiz.qpic.cn/sz_mmbiz_png/a5yW0MNUS2B35YuvvOcHKH4Nyc8qkXE92WGR1H2T2y4EV3SSq0OuwIKTOw0akzKow9ibibGyLJmw5fywKWgrwYRg/640?from=appmsg)

Q1：那么这套和现在主流的 SBDD 扩散模型（Pocket2Mol、PocketXMol、FLOWR 这些）相比，到底谁强？论文没直接比，所以没法定论。但从设计哲学上讲，这两条路各有优劣。SBDD 扩散模型的优势是直接利用 3D 几何 + 端到端给 binding pose；劣势是训练数据稀缺、合成可行性差、产出 pose 经常需要 minimization 修复。MCLM 的优势是训练数据规模大（287M conformer vs PDBbind 量级的几万）、产出 SMILES 化学合理性高、dataset token 控制商业化学空间；劣势是没有原生 binding pose、几何感知靠 contrastive embedding 间接传递。如果你做 lead optimization 或 hit-to-lead 阶段、看重可合成性，MCLM 这条路更工业化；如果你做 fragment-based design 或者要细抠 binding pose，SBDD 扩散模型的工具栈更合适。Q2：CF-InfoNCE 这个改动到底有多大影响？是不是过度解读了？这个改动从原理上是必要的，但具体增益论文里没有做单独消融（比如对比 CF-InfoNCE vs 标准 InfoNCE 的 LIT-PCBA 数字）。在 batch size 大、unique pocket 多的训练 setup 下，pocket collision 的频率其实不高，所以 CF-InfoNCE 的实际影响可能没想象中大。我的猜测是：这是个 "做了肯定不亏、不做可能掉点" 的安全选项，但不是这篇论文性能的主导因素。Q3：如果我已经在用 DrugCLIP，要不要切到 CLIPP-SET？看你的 use case。如果你只用 retrieval、且 LIT-PCBA 这个 distribution 跟你项目的 target 类似，CLIPP-SET 的 EF(0.5%)/EF(1%) 比 DrugCLIP 提了大概 10-15%（看 Table 1），值得切。但如果你需要广义 ranking（EF(5%)、AUROC），两个模型都不如老老实实跑 docking。CLIPP-SET 的真正价值在它附带的 MCLM——能从对比空间往生成端走，DrugCLIP 没这个能力。Q4：dataset token 看起来很简单，为什么之前没人做？其实 COATI 已经有过类似设计（用一个 dataset embedding 来标识不同化学空间），但 COATI 没有 3D 维度。在 SBDD 扩散模型那条路上，"控制生成偏向某个化学空间" 是一个很难的事情——扩散模型是直接在原子点云空间采样，没有自然的 "domain conditioning" 接口。SMILES 自回归路线天然支持 prompt 式的 token-level 控制，所以 dataset token 是这条路线特有的红利。这也回过头来支持了"不一定要走 3D pure generation 路线"的论点。- 论文：Structure-guided molecular design with contrastive 3D protein–ligand learning
- 作者：Carles Navarro, Philipp Tholke, Gianni de Fabritiis（Acellera Labs / Universitat Pompeu Fabra）
- 预印本：arXiv:2604.19562v1，2026 年 4 月 21 日
- 提交状态：预印本，尚未经过同行评审。
本文为个人解读，如有疏漏或理解不当之处，欢迎指正交流。觉得有价值的话，欢迎转发分享~本公众号主要介绍AIDD中小分子、多肽、PROTAC的前沿算法、综述、评估。欢迎关注本公众号获取领域最新文献解读。
