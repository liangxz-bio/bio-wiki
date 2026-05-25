# YuelDesign || 针对柔性蛋白口袋设计的扩散模型分子生成框架

> **来源：** DrugAutoPilot

---

---

蛋白质配体结合不是一把钥匙开一把锁——蛋白口袋会随着配体的进入而发生构象调整，这就是经典的诱导契合效应。然而，目前主流的深度学习分子生成方法（如 DiffSBDD）大多将蛋白口袋视为刚性的静态结构，相当于用一张固定尺寸的锁孔图纸来设计钥匙，难以捕捉结合过程中的动态变化。YuelDesign 正是针对这一核心痛点提出了新的解决方案。⭐「一、模型简介」⭐YuelDesign 于 2025 年 5 月发表在 bioRxiv 预印本平台上，文章题目是《A Diffusion-Based Framework for Designing Molecules in Flexible Protein Pockets》，作者是 Jian Wang 和 Dong Yan Zhang（共同第一作者），通讯作者为 Nikolay V. Dokholyan，来自宾夕法尼亚州立大学医学院（Department of Neuroscience and Experimental Therapeutics, Penn State College of Medicine）。原文链接：https://doi.org/10.1101/2025.05.27.656443。

![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/pPAHJZ6KpWeBwxnv2cVh6xjSPyfv1wriaqPfVynYnlibXqWkDr5HI9g6SlLCgFsn245d7lPicYOsJIGt49u7kEKcAwevVfB6UByJXqIOrCUDZY/640?wx_fmt=jpeg)

现有的基于扩散模型的结构-based 分子生成方法（如 DiffSBDD）通常使用蛋白口袋的全部原子，并通过距离截断构建稀疏图。这种做法存在三个问题：（1）大量原子迫使模型使用距离截断而非全连接图，限制了计算可行性；（2）侧链原子可能将模型偏向晶体结构中的刚性构象；（3）距离截断创建的固定图结构无法捕捉去噪过程中原子距离的动态变化。

YuelDesign 的核心创新在于提出了一套面向蛋白柔性的编码与生成方案。在蛋白编码方面，模型仅保留骨架的 α 碳（CA）原子，并根据氨基酸类型对每个 CA 原子进行独热编码。这一设计大幅减少了节点数量，使得全连接图（所有节点对均有边连接）在计算上可行，同时避免了侧链刚性构象的约束。在生成模型方面，YuelDesign 采用 DDPM（Denoising Diffusion Probabilistic Model）框架，核心网络为 16 层 E(3)-等变图神经网络（EGNN），隐藏维度 64，使用 SiLU 激活函数。模型同时扩散连续特征（3D 坐标）和离散特征（原子类型），通过 L2 损失函数训练噪声预测。生成完成后，使用专用键重建模块 YuelBond（CDG + 2D 模式组合为 YuelBond*）预测键顺序，确保化学有效性。训练数据来自 MOAD 数据库，包含约 41,409 个蛋白-配体晶体结构。

**

![](https://mmbiz.qpic.cn/sz_mmbiz_png/pPAHJZ6KpWeI5Op4Q0g94N8wUaN6bOtQulRGKEl1dm00Pib08YdmEklwtZThDzmicWX6bSkNxv1qdeKcMgzDj2TIp9FCAwqc0wic5Cpa12Qib6E/640?wx_fmt=png&from=appmsg)

**

6 原子）生成率仅 2.5%，远低于 DiffSBDD 的 20.6%。总体化学有效性 53.7%，连通性 99.5%。有效性随分子尺寸减小而提升，10 重原子时达 77.8%。","27:\"11\"|31:2|inline-dir:\"ltr\""]]" data-copy-origin="https://shimo.im" data-pm-slice="0 0 []">**📊「二、模型性能」📊**

YuelDesign 在药物属性方面全面优于 DiffSBDD：QED 得分更高，Lipinski RO5 合规率达 56.7%（DiffSBDD 仅 28.9%），合成可行性 SAS 得分更低（更易合成）。大环（>6 原子）生成率仅 2.5%，远低于 DiffSBDD 的 20.6%。总体化学有效性 53.7%，连通性 99.5%。有效性随分子尺寸减小而提升，10 重原子时达 77.8%。

**

![](https://mmbiz.qpic.cn/mmbiz_png/pPAHJZ6KpWddOfOk8uwFaKibo2klbBuyA4tODWfWHUnyezHw7XuQicS8d66sZsR3zIz3ib4rgKsDriaYOsdAdpSoxH2pGRab3KfV16mlCDlvZL8/640?wx_fmt=png&from=appmsg)

**

五种键重建方法对比中，YuelBond*（CDG + 2D 模式组合）在有效性上达 65%，远超 RDKit 的 13%。这是因为 RDKit 的 xyz2mol 需要理想的键长和键角，而扩散模型生成的几何构象往往存在偏差。YuelBond* 在有效性和药物属性之间取得了最佳平衡。

**

![](https://mmbiz.qpic.cn/mmbiz_png/pPAHJZ6KpWfSeCfARBNia45LnuDCQY2mqaAibjNd7wmGmtSebd9bPjMRtjribIT01UfQKs1nAhyhoiayib2tI6r6tm7XWacmBWerXD5OJazs7zAU/640?wx_fmt=png&from=appmsg)

**

YuelDesign 生成的分子探索了训练集中不存在的功能团：环氧化物（Epoxide，训练集 0.1% → 生成 5.0%）、氰化物、羧酸、酯、酰胺、酮、醛、磺酰胺等。常见功能团（醇、胺、醚）频率有所降低，表明模型在保持化学合理性的同时扩展了化学空间。

**

![](https://mmbiz.qpic.cn/mmbiz_png/pPAHJZ6KpWeE5UQRSdBHH8iaTAkUoGbPRezr3U4cEY7wic3K0AxXmLDFKDjeqp3q6CeWiafmwnhiaib6nB4QKhiaudx3gUoxv6KKBMyadOgmfJrWQ/640?wx_fmt=png&from=appmsg)

**

使用 MedusaDock 重对接评估结合性能。对于小分子（≤20 重原子），生成分子的结合能与天然配体相当。典型案例：人体烟酰胺磷酸核糖转移酶（PDB ID: 4LV9），生成分子的 MedusaScore 为 -107.13 kcal/mol，显著优于天然配体的 -24.07 kcal/mol。RMSD 分析显示平均偏差 5-6 Å，随分子尺寸增大而增加，但也有例外如 PDB 3OFM 仅 0.4 Å。

**

![](https://mmbiz.qpic.cn/mmbiz_png/pPAHJZ6KpWeo9TicgfPGOxFvv43iazFiaZ7hicMmV6QeHeYFaoG2nPoL718ibUTBWauZhLxfZCTnJpMvs8Og1icFyN7DPopj5Z2s03S2XqKYFHGNI/640?wx_fmt=png&from=appmsg)

**

多巴胺受体（7CKZ）：生成分子平均结合能 -34.54 ± 10.27 kcal/mol，优于天然配体多巴胺的 -30.60 kcal/mol。药效团聚类分析显示生成分探索了与多巴胺显著不同的化学空间。5-羟色胺受体（7E2Y）：类似趋势，平均结合能 -29.21 ± 7.66 kcal/mol，优于天然配体的 -23.89 kcal/mol。

**

![](https://mmbiz.qpic.cn/sz_mmbiz_png/pPAHJZ6KpWdWNjegDayWdynjUVibBm8XUAqthKSUYdZfIEwRJiaO3bEDPZbRf02DHmNuSjkWN7ibE8kq6xKALYuFJE2JYqAzStGxQflGccdv4g/640?wx_fmt=png&from=appmsg)

**

对 PDB 2REQ 的去噪过程详细分析揭示了分层优化策略：（1）原子类型在最后 25 步趋于稳定；（2）坐标 RMSD 匀速下降；（3）键在第 200 步开始形成，第 450 步后基本稳定；（4）整体构象在最后 50 步进入精细调整阶段。这表明模型采用了"先定骨架、再调细节"的层次化生成策略。

**

![](https://mmbiz.qpic.cn/sz_mmbiz_png/pPAHJZ6KpWeSiaDIQCg3kBwMztEGib6hK2iaKD0lDbrP5sfG2W52XftDDSJLr8Hom6kS0riba9bkulOK7MyZhFWWEqx6Sgkn0ZpT5hvDIFcTscE/640?wx_fmt=png&from=appmsg)

**

🖥️「三、总结」🖥️

**

YuelDesign 是专门针对柔性蛋白口袋设计的扩散模型分子生成框架，通过 CA-only 全连接图表示突破了刚性口袋假设的局限。创新的蛋白编码方案（仅编码骨架 α 碳 + 氨基酸类型）大幅降低计算复杂度，使全连接图可行，同时避免侧链刚性构象的约束。**此外，专用键重建模块 YuelBond* 针对扩散模型生成的非理想几何构象，有效性达 65%，远超传统 RDKit 方法的 13%。YuelDesign 生成分子在药物属性、合成可行性和功能团多样性上表现同样优异，且探索了训练集之外的化学空间，去噪过程分析揭示了清晰的分层优化机制，为未来改进（如解耦原子类型确定与坐标优化）提供了方向。******
