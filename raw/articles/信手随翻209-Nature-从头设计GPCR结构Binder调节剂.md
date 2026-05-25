---
source_url: https://mp.weixin.qq.com/s/unknown
ingested: 2026-05-18
sha256: 5ac0b635db9a599d04e5ee3004fc5b5537436bb2bcd499a43113704a1c074439
citation: "浙江大学. De novo design of transmembrane binding proteins for GPCR modulation. Nature. 2025. doi:10.1038/s41586-025-09957-1 | 微信公众号·信手随翻 中文解读"
domain: ai-drug-discovery
---

# 「信手随翻 209」Nature:从头设计GPCR 结构Binder调节剂

> **来源：** 信手随翻

---

**

这篇浙大发在 Nature 上的文章非常有意思，堪称跨膜蛋白设计和 GPCR 调控领域的一项突破性工作。文章的核心是利用深度学习工具，从头设计（De novo design）了直接靶向 GPCR 跨膜结构域的“外骨架调节剂”（GEMs）。**

**

导入

![](https://mmbiz.qpic.cn/sz_mmbiz_png/4C4SQ5wUiaGzOMfA1OtmP1OOFHkYsrVAPTEjGKrtV1cYv4NBrNs9mCINvXB8DLMSVtEywicUX1OtC05DsWfqib21SwysIYvCjAQuFRvTaCOEbc/640?wx_fmt=png&from=appmsg)

https://www.nature.com/articles/s41586-025-09957-1

**创新的靶向策略**：打破了传统 GPCR 药物主要靶向正构结合位点（orthosteric site）的局限，从天然跨膜蛋白对 GPCR 的调节机制中汲取灵感，直接针对跨膜结构域（TMD）设计变构调节蛋白 。

**精妙的 AI 计算管线**：作者构建了一个基于 RFDiffusion 生成初始骨架，再结合 ProteinMPNN 和 AlphaFold2 (AF2) 进行迭代重折叠（refolding）的计算管线 。在这个过程中，他们放弃了传统的能量学评估，转而使用自洽性（self-consistency）和局部自信度（pLDDT）作为核心过滤指标 。

**结构提示词（Structural Prompting）策略**：这是整个序列设计环节非常巧妙的一笔。为了约束 AF2 并实现面向特定功能的构象选择，他们引入了“定点插入”（Site-directed insertion）、“位点阻断”（Site-blocking pre-occupation）和“构象诱导”（Conformation induction）三种物理约束策略，极大地提高了设计的成功率 。

**高精度的 Cryo-EM 结构验证**：以多巴胺 D1 受体（D1R）为模型，团队不仅成功设计了四种不同功能的 GEMs（包括 ago-PAM、NAM 和 BAM），还通过冷冻电镜解析了它们与 D1R 结合的高分辨率结构（2.6 Å – 3.0 Å），完美验证了计算设计与实验构象之间的高保真度 。

**挽救功能丧失（LOF）突变**：在治疗潜力上，设计的 ago-PAM GEM 能够直接稳定 TM6 的向外移动，从而成功挽救了多种导致 D1R 失活的 LOF 突变，为 GPCR 相关的遗传疾病提供了一种全新的治疗模式 。

这篇文章把扩散模型生成、序列/结构协同优化以及严谨的结构生物学验证完美地闭环了。

**

**

认识自然改造自然

**RAMPs（受体活性修饰蛋白，Receptor activity-modifying proteins）**：这可能是最著名的 GPCR 辅助蛋白家族。根据文章附图展示，不同的 RAMP 成员（如 RAMP1、RAMP2、RAMP3）可以与降钙素基因相关肽受体（CGRPR）、肾上腺髓质素受体（AM1R, AM2R）以及胰淀素受体（AMY1R, AMY2R, AMY3R）等 GPCR 结合并调控它们的活性和配体选择性 。

**MRAPs（黑皮质素受体辅助蛋白，Melanocortin receptor accessory proteins）**：例如 MRAP1，它在自然界中可以与黑皮质素-2受体（MC2R）形成复合物，这对于该受体的正常表达和信号传导是必不可少的 。

**CD69**：文章引用了近期的研究指出，跨膜蛋白 CD69 实际上可以作为 1-磷酸鞘氨醇受体 1（S1PR1）的激动剂来发挥作用 。

**

### 天然蛋白与本文设计的 GEMs 的核心区别
虽然自然界已经证实了“跨膜蛋白调节 GPCR”的可行性，但作者在 Discussion 部分特别指出了一点机制上的不同：

**天然互作（如 RAMPs 和 MRAPs）**：它们的跨膜区确实参与了与受体的结合（通常结合在 GPCR 的 TM4 或 TM5/6 区域 ），但这些天然蛋白**主要依赖于它们在细胞外的结构域（extracellular domain）的相互作用来发挥主要的调节功能** 。

**人工设计的 GEMs**：被称为“外骨架（exoframe）”，是因为它们摒弃了对胞外结构域的依赖，**直接且完全通过与 GPCR 的跨膜结构域（TMD）界面的相互作用**来实现各种期望的变构调节功能（如激活、抑制或偏向性信号传导） 。

可以说，作者是观察到了大自然中“跨膜辅助蛋白可以像伴侣一样抱着 GPCR 调节它”的现象，然后利用深度学习工具去掉了复杂的胞外互作，纯粹在跨膜层面上重新构建并扩展了这种控制机制。

## 技术路线
**

这篇文章的技术路线设计得非常严密且成体系，可以看作是一个从“大范围盲扫”到“精准设计”，再到“实验验证与应用”的完美闭环。根据原文内容，整个研究的技术路线主要可以分为以下五个核心阶段：

### 1. 跨膜结构域（TMD）的高通量盲扫与热点定位 (Probing)
为了弄清楚多巴胺 D1 受体（D1R）跨膜区到底有哪些地方可以结合蛋白，作者团队首先开发了一种高通量的表面分析方法 。

**生成探针**：他们利用 RFDiffusion 工具生成了单次跨膜的  螺旋骨架，并通过 ProteinMPNN 为这些骨架设计序列，生成了大量“探针” 。

![](https://mmbiz.qpic.cn/mmbiz_png/4C4SQ5wUiaGxKL1cRyleaO2a2eVKo0fSnfcODpSHibSZNRGFJeJhuWbV62YMAIQ7R0Jt14libwU1PoiaGgXzfRsv7LLw3kwjJbmKOjNGR1ichpnI/640?wx_fmt=png&from=appmsg)

**

**计算对接**：使用 AlphaFold2-multimer 预测这些探针与受体结合的复合体结构，总共生成了 1500 个对接构象 。

**锁定靶区**：通过分析这些探针的分布，他们成功锁定了三个适合跨膜螺旋结合的空腔：TM1/2/4、TM3/4/5 和 TM5/6/7 。

### 2. 结合“结构提示词”的从头设计管线 (De novo design)
在确定了靶点位置后，团队搭建了一个高度定制化的 AI 蛋白质设计管线，这也是全篇最亮眼的技术核心 。

**初始骨架与迭代优化**：首先用 RFDiffusion 生成初始结合骨架，然后进入一个类似于“幻觉（hallucination-like）”的 MPNN-AF2 迭代重折叠过程 。在这个过程中，ProteinMPNN 负责生成序列，AF2 负责预测结构，两者不断循环优化 。

**引入结构提示词（Structural Prompting）**：为了让 AI 乖乖听话，生成特定结合模式和功能的调节剂，他们首创了三种巧妙的物理约束策略 ：

**定点插入（Site-directed insertion）**：把设计的序列强行插入到受体的特定 loop 区，限制它跑到别的地方去 。

**位点阻断（Site-blocking pre-occupation）**：如果在预测时发现设计出的蛋白总往错误的强结合位点跑，就提前在环境里放一个高亲和力的假蛋白把错误位点占住 。

**构象诱导（Conformation induction）**：为了设计能激活受体的 GEM，他们在计算环境里加入了 G 蛋白，逼迫受体保持激活状态下的 TM6 向外移动构象，从而让设计出的调节剂专门适配这个激动状态 。

### 3. 体外功能筛选与药理学表征 (Functional characterization)
计算筛选出高自信度的序列后，他们对候选蛋白进行了细胞水平的实验验证。

通过 cAMP 积累和 -arrestin 招募实验，测试了这些设计蛋白对 D1R 下游信号传导的影响 。

最终成功筛选出四种功能各异的 GEM：只结合但不影响功能的“锚定剂”（GEM-TM1/2/4）、偏向性变构调节剂 BAM（GEM-TM3/4/5）、负向变构调节剂 NAM（失活态 GEM-TM5/6/7），以及正向激动变构调节剂 ago-PAM（激活态 GEM-TM5/6/7） 。

### 4. 高分辨率冷冻电镜（Cryo-EM）结构解析
为了“眼见为实”地验证计算模型，团队进行了硬核的结构生物学验证。

他们将挑选出的 GEM 直接融合到 D1R 受体上，以保证复合体的稳定性

最终解析了这些 GEM 与 D1R 结合的高分辨率冷冻电镜结构（分辨率在 2.6 到 2.9 埃之间） 。结果表明，实验解析出的真实结构与 AF2 最初的预测模型高度一致，证实了设计策略的高保真度 。

### 5. 治疗潜力的应用测试 (Rescuing LOF mutations)
在完成了设计和验证后，作者将技术推向了实际的疾病治疗应用层面。

GPCR 的功能丧失（LOF）突变会导致许多严重疾病，且传统药物很难起效 。

他们测试了设计的激动型调节剂（ago-PAM GEM），发现它能够像“外骨骼”一样直接稳定受体的激活构象，成功挽救了多种不同机制导致的 D1R 失活突变，恢复了受体的信号传导能力 。

总体来看，这条技术路线将大语言模型的提示词工程（Prompt Engineering）思想极为巧妙地迁移到了蛋白质三维结构的生成中，是一套非常成熟且极具通用性的膜蛋白设计范式。

## 看图说话
**

### **Fig. 1 | GEM 的从头设计（De novo Design）管线**
这张图是整个计算设计框架的总览，重点展示了作者是如何从零开始，把大语言模型里的“Prompt（提示词）”思维平移到蛋白质生成的物理约束上的。

![](https://mmbiz.qpic.cn/mmbiz_png/4C4SQ5wUiaGyId6iamzUhc8NaicKZ2O0tccUmhE5ty1u9rGUYrQylq8D8mZmNjY8z71WlFI5YKfTwttIibah5AciaHney5HkGibGbGslhrW3A4MdM/640?wx_fmt=png&from=appmsg)

Fig. 1a (大范围盲扫与热点定位): **在干什么：** 这展示了高通量“Probing（探针探测）”的结果 。作者用单次跨膜螺旋作为探针，去试探 D1R 跨膜区（TMD）哪里可以结合。

**怎么看：** 左边俯视图的“点云（point cloud）”和右边侧视图的“体积（volume）”展示了探针最喜欢结合的位置 。点云的颜色代表了归一化密度（红色代表结合频率高，蓝色代表低）。这直接帮他们锁定了 TM1/2/4、TM3/4/5 和 TM5/6/7 这几个潜在的靶向口袋。

**

- **Fig. 1b (设计目标定义):**
**在干什么：** 明确了要设计的四种 GEM 的物理位置和构象目标 。

![](https://mmbiz.qpic.cn/mmbiz_png/4C4SQ5wUiaGwTXU0Yncolmtric78v3iauQGGjkhBBLCoJ6ibm2hAled7PBXlVBBHIWmrhR2jVHIhj3pDeUvnuB5WrpnYngwRVpwon6Gu3FIvWcA/640?wx_fmt=png&from=appmsg)

**怎么看：** 上方的静电势表面图展示了期望的 GEM 结合面 。右下角的抽象 Logo 极其直观地定义了四种模式：绿色长方形靶向 TM1/2/4；红色三角形靶向 TM4/5/6；黄色的 U 型代表与激活态（active state）TM5/6/7 结合；黄色的梯形代表与失活态（inactive state）TM5/6/7 结合 。

**

- **Fig. 1c (计算设计核心管线):**
**在干什么：** 展示了利用 RFDiffusion 和 ProteinMPNN-AF2 迭代的干实验流程 。

**怎么看：** 靶向的 TMD 区域被标为黄色 。在自洽性（self-consistency）过滤环节，蓝绿色的代表成功结合在目标位点的理想 GEM，而紫色的则是跑偏了的“失败品” 。这个管线不依赖传统的能量打分，而是靠局部自信度（pLDDT）和自洽性来筛选 。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/4C4SQ5wUiaGyOs2lvs4k2OIdS0YDGyHZAGlEFF41kxKkjHWhDaR2gTicchwJV3s6ehsGh8yFMfGDG1JQD2RFwzwjwwQCeiaobQCznm7JE5wuYg/640?wx_fmt=png&from=appmsg)

******- **Fig. 1d (结构提示词策略 - 核心亮点):**
**在干什么：** 为了防止 AI 生成的结构到处乱跑，作者发明了三种“物理提示词（Structural prompt）”来约束网络 。

**怎么看：** 1.  **定点插入（Site-directed insertion）：** 把序列强行塞进特定的环区（如 ECL3，黄色区域），物理上限制它不可能跑到对侧（紫色区域）去 。 2.  **位点阻断（Site-blocking pre-occupation）：** 提前用一个已知的高亲和力设计（紫色）占住不需要的竞争位点，逼迫当前设计（蓝绿色）只能去目标位置 。 3.  **构象诱导（Conformation induction）：** 引入 G 蛋白（灰色部分）作为“垫脚石”，强制受体在迭代优化期间保持激活态的向外扩张构象 。

### **Fig. 2 | 计算内（*In silico*）的设计结果评估**
如果说 Fig. 1 是宏观战略，Fig. 2 就是展示模型收敛过程的硬核数据。这里证明了 ProteinMPNN 和 AF2 是如何在这套管线下达成高度共识的。

- **Fig. 2a (结合构象):**
**在干什么：** 直观展示了最终生成的 GEMs 的侧视图 。

**怎么看：** 突出了 GEM 结构与 TMD 靶向表面的高度形状互补性 。

- **Fig. 2b (序列演化与 AF2 自信度):**
**在干什么：** 跟踪了在折叠迭代（refolding iterations）过程中，序列和结构的收敛情况 。怎么看：  **Top (SeqLogo)：** 最终迭代轮次中 ProteinMPNN 设计序列的分布，可以看出大量疏水氨基酸（I, V, L, F）的富集，这非常符合跨膜螺旋的特征 。

**Middle (平均 pLDDT)：** 随着迭代轮次增加，AF2 预测的 pLDDT 曲线整体上移，说明模型对结构的信心越来越强 。Bottom (pLDDT 标准差)： 阴影区域越来越窄，标准差变小，说明 AF2 预测出的 5 个模型的构象越来越一致（高自洽性） 。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/4C4SQ5wUiaGwySBsSjd0w4rx3YeNGzicFCfjJ9dmvicX4sZ3F88dxNNpiaeC0RFib3bWY4icqv7zE6uEwpW1AsBuYuYYp8WxxA2RZY96ia9d6SSgUU/640?wx_fmt=png&from=appmsg)

- **Fig. 2c (ProteinMPNN 概率收敛分析):**
**在干什么：** 专门挑出了设计中最关键的 20 个残基（大多是界面互作残基），分析模型给出的最大预测概率  的变化 。

**怎么看：** * **Top (Heat map)：** 颜色越深（偏红）代表概率越接近 1.0。可以清晰看到随着 Round 增加，这些关键位置的  逐渐变深，说明 ProteinMPNN 对这些位置该放什么氨基酸越来越笃定 。

**Bottom (聚焦残基的 SeqLogo)：** 这 20 个残基的相对索引位置和高度保守的氨基酸分布 。(注：GEM-TM3/4/5 是因为用了软启动策略，从已有序列适配来的，所以第一轮没有显示  )。

**

### **Fig. 3 | 结构与功能验证：从单点调控到“逻辑门”模块化拼装**
Fig. 3 的主要任务是证明这些人工设计的 GEMs 不仅在功能上表现出预期的变构调节作用，在三维空间中也确实是以设计的方式与 D1R 结合的。

**Fig. 3 a–d (药理学功能表征)：**

这里对比了不同 GEMs 对 D1R 两条主要下游信号通路（Gs 介导的 cAMP 积累和 -arrestin 2 招募）的影响。

![](https://mmbiz.qpic.cn/mmbiz_png/4C4SQ5wUiaGzoF30DicDaYcjV1pSZZnrBmkCLjPjOOZ660OBb1gg08yFthr6Ezc4zJ4t8wySMwDRSkLxyhB263RDZgP7iaq4jdazpSPicKxyCibw/640?wx_fmt=png&from=appmsg)

**a (Anchor)：** 锚定剂。曲线和野生型（WT）基本重合，说明它虽然结合，但完全不干扰受体本身的构象变化。

**b (BAM)：** 偏向性调节剂。cAMP 信号只是略微下降，但 -arrestin 招募被显著抑制。这说明它阻断了受体向特定的下游构象转变。

**c (NAM)：** 负向变构调节剂。两条信号通路的曲线都被极大地压制，受体被“锁死”在失活状态。

**d (ago-PAM)：** 激动型正向变构调节剂。在没有多巴胺（配体）的情况下就能引起基础信号的提升，并且增强了配体诱导的信号响应。

**

**Fig. 3 e–j (Cryo-EM 密度图与原子模型)：**

这里展示了高质量的冷冻电镜重构结果。为了稳定不同的构象，实验设计非常严谨：对于激活态的复合物（如 anchor, BAM, ago-PAM），利用了 Gs 蛋白和 Nb35 纳米抗体来稳定（e, f, h, j）；而对于抑制态的 NAM，则巧妙地利用了融合在 ICL3 的 BRIL 结构来辅助重构（g, i）。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/4C4SQ5wUiaGx6gUVRxfjg1RJMa4M323uA3RjVanKbWVng00uibeYcCqnNbH85Gf8Nz7V1YiccxD78s2WnNKYYqg8Ws4WNHNxJJuOfY7zAtmMs8/640?wx_fmt=png&from=appmsg)

**i 和 j (多模块复合物)：** 这是一个极为惊艳的实验。他们把三种不同的 GEM 同时结合到了一个 D1R 上，在 3.0 Å 和 2.6 Å 的分辨率下看到了三个外挂螺旋互不干扰地贴在受体上。这直接证明了 TMD 表面存在多个独立且互不重叠的变构位点。

**Fig. 3 k (逻辑门模块化)：**

既然证明了可以同时结合，作者做了一个“AND 逻辑门”实验。当把 BAM（抑制 -arrestin）和 ago-PAM（全面激活）同时加上去时，组合结果是：Gs 通路保持激活，而 -arrestin 通路依然被抑制。这说明 GEMs 具有极强的功能模块化（functional modularity）潜力，可以通过像搭积木一样组合不同的 GEM 来编程 GPCR 的信号输出。

### **Fig. 4 | 结构评估与分子机制：计算与实验的对齐，以及 TM6 的空间博弈**
Fig. 4 探讨了两个深层问题：AF2 预测得准不准？这些 GEMs 到底是通过什么物理机制改变受体活性的？

**Fig. 4 a, c, e, g (计算预测 vs 实验真实的 RMSD 对比)：**

灰色是 AF2 的预测模型，彩色是 Cryo-EM 解析出的真实结构。

对于 Anchor (a), NAM (e) 和 ago-PAM (g)，预测与真实的  RMSD 分别只有 1.0 Å, 0.9 Å 和 0.9 Å。这种亚埃级别的对齐证明了这套 MPNN-AF2 迭代管线具有极高的保真度。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/4C4SQ5wUiaGwsG4d4gL8dLHR1fHGsGztzEOszIiaibcxwrw6yUhwYRV0U2Jh2QMicNcXjg8x7yxStnFXd6SlegDR2FgCAeZZjLia0m4MCAD8C2pc/640?wx_fmt=png&from=appmsg)

唯一偏差较大的是 BAM (c)，RMSD 达到了 2.7 Å。偏差主要集中在 D1R 的 TM5 和 TM6。考虑到 TM5/6 在 GPCR 激活过程中本身就具有极高的构象柔性（conformational flexibility），预测难度自然更大。

**Fig. 4 b, d, f, h (构象扰动分析)：**

这组图将结合了 GEM 的受体结构与天然激活态的 D1R（PDB: 7F0T）进行比对，受体上的颜色深浅代表了 RMSD 的大小。可以清晰地看到不同 GEM 结合对跨膜螺旋（尤其是 TM5, 6, 7）产生的局部结构扰动。

**Fig. 4 i & j (变构调节的核心机制 - 聚焦 TM6)：**

这部分揭示了变构调节的本质——针对 GPCR 激活标志性动作（TM6 的向外摆动）的空间控制。

**i (NAM 的机制)：** 当把结合了 NAM 的失活态 D1R 与天然激活态 D1R 叠合时，发现 NAM 所占据的空间直接与激活态的 TM6 发生了严重的空间位阻（Clash）。也就是，NAM 像一块石头一样堵死了 TM6 向外扩张的路径，强制受体保持失活态。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/4C4SQ5wUiaGwQTibbzcAab8CZMqpyLNe8mP3UN6YVcFLNxibVhPojvjRat8icTj9ibMeX9yicJACVpdjdF05ibiacoLNC9w8uRTmF05SviaeBibCliacnM/640?wx_fmt=png&from=appmsg)

**j (ago-PAM 的机制)：** 结合了 ago-PAM 的激活态结构与 AF2 预测的失活态比对。ago-PAM 本身具有非常刚性的钳状结构，它紧紧抓住并拉扯 TM6，从外部主动提供了一个力（Pulled by GEM），直接在物理层面稳定了 TM6 向外移动的构象，从而跨过了配体，直接将受体的构象平衡推向了激活态。

小结：非常好的工作，逻辑链非常的流畅，无论是选择区域，还是逻辑门之间的变构叠加效果
