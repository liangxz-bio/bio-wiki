---
source_url: https://mp.weixin.qq.com/s/ (微信公众号"陷入鞍点")
ingested: 2026-05-18
sha256: c3aaa7ef039662e1b0be79fb923180b015a25d54c8ae33f3c8bda91695bd75eb
citation: "Hu W., Sun Y., Li T., et al. The evolution of computation-driven paradigms in targeted peptide drug design: From predictive modeling to generative AI and clinical translation. The Innovation Drug Discovery. 2026;1(1):100009. | 微信公众号·陷入鞍点 中文解读"
domain: ai-drug-discovery
---

# Innov. Drug Discov. 2026 | 综述解读：计算驱动的靶向肽药物设计——从物理建模到生成式 AI 与临床转化

> **来源：** 陷入鞍点

---

编者按如果你是一名 CADD 科学家或药化学家，正在考虑将 AI 工具引入多肽/环肽设计流程，你面临的第一个困惑大概是：RFdiffusion、ProteinMPNN、AlphaFold、ESMFold、EvoDiff、PepMLM、BindCraft……到底该选哪个？它们各自能做什么、不能做什么？怎么组装成一个可落地的 pipeline？如果你是一名 AIDD 算法工程师，你更关心的可能是：当前多肽生成领域的算法格局是什么样的？哪些问题已经被较好解决，哪些方向仍有明确的 research gap 可以切入？非天然氨基酸建模、环肽生成、柔性线性肽的构象预测——哪个是最值得投入的方向？这篇 2026 年 4 月刚发表的综述（13 页，131 篇参考文献，6 张核心图 + 2 张高价值工具对比表）为上述问题提供了一份系统的回答。它的核心贡献不在于文献的堆叠，而在于构建了一个「生成 → 逆折叠 → 预测评估 → ADMET 过滤 → 物理验证」的闭环设计框架，并在这个框架下为每个环节标注了"当前最佳工具"和"已知瓶颈"。以下解读将围绕这个框架，把综述中最有价值的信息——尤其是图表和工具细节——系统呈现给读者。

![](https://mmbiz.qpic.cn/mmbiz_png/b4dmDNrOlLvThkrtbibaxz95W5flS0cSibKnGGticoXvqbCcibgdRhMFo1FAZc5kIhDOEW91QPyuwbwvPPVhsTk9WxzZFte5bk0MicRRYbfA8v7c/640?from=appmsg)

*一、多肽药物的独特位置与核心挑战为什么多肽值得专门讨论？多肽（2–50 个氨基酸残基）在药物类型谱系中占据一个独特的生态位，兼具小分子和生物大分子的部分优势：- 相比小分子：多肽拥有更大的界面接触面积（400–1000 Å²），能结合小分子无法企及的平坦 PPI 界面和"不可成药"靶点
- 相比抗体：更好的组织穿透性、更低的免疫原性潜力、更低的生产成本
- 临床验证：GLP-1 受体激动剂（Liraglutide、Semaglutide）的巨大商业成功已经证明了这一药物类型的市场价值
三大 PK 瓶颈

![](https://mmbiz.qpic.cn/mmbiz_png/b4dmDNrOlLv8cTbOlRzibaLRIzWicLtBD053KhrP4pY2sO1RnFCSJE6SuK4PzqibIxNTSg8fsicfTsZpH7Vpy7dPQ9G32LdQDkHMb2WmSKVkCxg/640?from=appmsg)

*Figure 1 是全文的起点图，概括了多肽药物的优势与挑战。上半部分展示了多肽的三大优势——高选择性、靶向 PPI 网络的能力、广泛的界面结合能力；下半部分展示了制约临床转化的三座大山：

![](https://mmbiz.qpic.cn/sz_mmbiz_png/b4dmDNrOlLvhyPYict8OwtCpomrNEFoVZKJ8ZWdicR5KTHI4BpAwgcIkhO8toPsqA3ZiaIaf9OVZfSVWsfuHoF0DerVHmx0iah0ElDUoggf8TQA/640?from=appmsg)

*传统的解决方案（骨架环化、非天然氨基酸引入、PEG 化）本质上是 trial-and-error，无法在 activity、stability、permeability 三者之间高效搜索最优解。这正是计算方法介入的核心动因。二、全局技术图谱：三个范式层 + 闭环整合综述的分类框架综述将计算多肽设计的技术演进梳理为三个递进的范式层。这不是简单的时间线罗列，而是一个"每一层解决什么问题、有什么方法、有什么局限"的功能分类。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/b4dmDNrOlLuq60r6Hp6Br7CgfMU9khYECoAHWXmFxlHVjrJhfgof7k2FA153rXAicOBHyqlLJ1ialMHnaZ6k3Gmib0iad7GWHPmLzjqFB2WQ1Lg/640?from=appmsg)

*Figure 3 用左（A）→ 中（B）→ 右（C）三栏清晰展示了这个演进：- Panel A — 传统方法：从随机肽库中物理筛选（高失败率、低效命中）
- Panel B — 理性设计三板斧：① 同源建模预测折叠 → ② 分子对接计算结合亲和力（ΔG = -9.5 Kcal/mol 示例）→ ③ MD 模拟评估动态稳定性
- Panel C — 优化输出：经过 in silico 优化后的高亲和力精确靶向序列
三层架构总览第一层｜经典分子建模（物理驱动，"验证层"）- 同源建模 → 预测折叠构象（序列一致性 >30% 时骨架可靠）
- 分子对接 → 评估结合模式与亲和力
- MD 模拟 → 验证动态稳定性与相互作用持久性
第二层｜AI 预测模型（数据驱动，"评估与筛选层"）- 结构预测：AlphaFold / ESMFold / PepFlow / HighFold
- ADMET 与可开发性预测：CellPPD, MLCPP, PlifePred, ToxinPred
第三层｜生成式 AI（创造驱动，"探索层"）- 序列优先生成：VAE, GAN, ProGen2/ProtGPT2, EvoDiff, PepMLM
- 结构优先生成：RFdiffusion, RFdiffusion3
- 多模态交互设计：ODesign
- 逆折叠（结构→序列桥梁）：ProteinMPNN, ESM-IF1, LigandMPNN
闭环整合：从 in silico 到临床候选物的翻译漏斗Figure 2 展示了核心的闭环工作流：生成式 AI 模型 → 物理验证（同源建模、分子对接、MD 模拟）→ 湿实验反馈 → 候选肽迭代优化。蛋白语言模型为生成过程提供序列先验，AI 驱动的生成设计与物理验证形成闭环。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/b4dmDNrOlLtBVZOSX79myleOicziaCcXpOvgVF8evVAq8JJNicF4QgCqgY4xs9s6icKPL0FWG4FKYUbXI0cdoZlsPfNjqibibkMAAX6yFeAeib4AIQ/640?from=appmsg)

*Figure 5 进一步将这个闭环拆解为三个功能流：

![](https://mmbiz.qpic.cn/mmbiz_png/b4dmDNrOlLsRTWlOuribTrGd9sYZtMkvZ9L9w9dzJjwVoabP1iaOlsTMDfZuktibMDavePGEh7K3ibftyYfhLJqSHy5nLicQylvZjf7CC3fSSM1k/640?from=appmsg)

*- Panel A — 预测基础流：序列输入 → AlphaFold/ESMFold → 结构与功能输出
- Panel B — 生成创新流：靶点约束/潜空间 → RFdiffusion/ProteinMPNN → 全新 de novo 设计
- Panel C — 闭环优化流（核心）：① Generate backbone → ② Inverse fold（获得序列）→ ③ Evaluate/Score（亲和力排序）→ ④ Refine/Feedback → 迭代回到 ①
Figure 6 是全文信息密度最高的图之一，分上下两部分：

![](https://mmbiz.qpic.cn/sz_mmbiz_png/b4dmDNrOlLsgpRNaz3njiaOYXmLAs6DkFOxBIY4qzRLqAGkM0oXJEb3CzNiaax8ydcicoG2LolMUNutUPEEYOcjnicSyyhWjnlmohJvao2fSAFo/640?from=appmsg)

*上半部分展示了三个里程碑临床案例的范式演进：- 过去 — Semaglutide：理性设计（PK 工程，linker 几何优化），方法是 MD 验证 + 脂肪酸修饰，成果是延长半衰期
- 当下 — MK-0616：计算刚性化（大环肽口服生物利用度），方法是将骨架锁定为生物活性构象以屏蔽极性基团，成果是解耦分子量与生物利用度
- 未来 — SARS-CoV-2 miniprotein inhibitors：生成式 de novo 设计，方法是 RFdiffusion + ProteinMPNN，成果是皮摩尔亲和力、月级开发周期
下半部分展示了「从生成到临床」的五层翻译漏斗：1）Generative design（RFdiffusion, ProteinMPNN）→ 大量候选骨架2）Physicochemical & synthesizability filter（溶解度预测、合成可行性检查）→ 淘汰不可合成的设计3）Affinity & specificity validation（MD, docking, AlphaFold）→ 验证结合亲和力4）PK/ADMET assessment（ADMET 预测、PK 建模）→ 筛除 PK 不佳的候选物5）Clinical candidate → 最终进入临床的分子三、经典建模方法：在 AI 时代的准确定位同源建模- 核心逻辑：序列相似性 → 结构相似性。序列一致性 >30% 时骨架构象可靠
- 当前最佳定位：不再是"结构预测"的首选（已被 AlphaFold 取代），而是作为 PPI 界面解析工具——建模靶蛋白与天然结合伙伴的复合物，提取界面肽段作为设计模板（竞争性肽设计逻辑）
- 已验证案例：MDM2/MDMX 抑制剂、BCL-2 家族抑制剂的设计中，通过界面 hot spot 残基的系统表征指导了天然片段到高亲和力抑制剂的转化
- 已知局限：高度柔性的 loop 区和无序结构域建模仍不可靠
分子对接

![](https://mmbiz.qpic.cn/sz_mmbiz_png/b4dmDNrOlLtia3RT0mzAHCcMLg0jyhd6GJyQ3Wh98cnTia5ibOuVzANBbibQWhNXiaQeAe2k8JDerZ2Ux5v9PSbJdkWVQUOrlbibiasJpIpiaIBtde4/640?from=appmsg)

*Figure 4.  Classification of strategies and commonly used tools for peptide-protein molecular docking.

![](https://mmbiz.qpic.cn/mmbiz_png/b4dmDNrOlLudkjUJ8TFdA4tZq8paRcw6YfO2Tk8M5ib2ylvR4F7UKGTibkibZVWKGR02LoB0JoNEic9rlhBwmYxdZEDiaLdq4RQktuEfmILnaHMk/640?from=appmsg)

*最佳策略是分层对接（Ciemny et al. 推荐）：先用 Global 工具定位结合热点，再用 Local 工具优化原子接触。对于环肽，标准算法往往无法有效处理骨架约束，需要 AutoDock CrankPep 等专用工具。值得特别关注的新进展：RAPiDock——一个基于扩散模型的 AI-native 对接框架。与传统采样方法不同，它通过渐进去噪随机坐标来搜索高质量 binding pose，在高维能量景观中的导航效率显著优于传统方法。这代表了对接领域从"物理采样"到"生成式采样"的范式转移。MD 模拟- 在 AI 时代的核心角色：生成模型输出的"验证关卡"，而非独立的探索工具
- 具体验证逻辑：解决 Newton 方程获得原子轨迹 → 观察肽折叠、构象变化、相互作用随时间的演变 → 区分"真正稳定"和"结构幻觉"
- 经典案例：Semaglutide 的 MD 验证——确认亲水 linker 具有足够柔性以适应白蛋白结合，且不干扰 GLP-1 与受体的关键相互作用
- 增强采样技术：REMD（多温度 replica 穿越能量壁垒）、aMD + metadynamics 混合策略（快速构象扫描 + 精确自由能估算）、ML 引导的 MD（Weber et al.，高效探索关键构象区域）
- 溶剂模型选择：显式溶剂精度更高但计算昂贵，隐式溶剂高效但可能牺牲氢键网络等细节
四、AI 预测工具全景：Table 1 逐工具解析Table 1（Common AI methods for predicting peptide 3D structures）是 CADD 工程师选型的核心参考表。以下为每个工具的详细分析：

![](https://mmbiz.qpic.cn/mmbiz_png/b4dmDNrOlLu6vUrsCNSlahw4nj159eoFzRE4dIpgbWPKmeYsYUeVZVG7ME7fxlI14LsA0DlBqIUZWHROkKv9ibCwJXL6DrC2vGWicdgiaDvmfE/640?from=appmsg)

*AlphaFold (AF2/AF3)- 技术原理：基于 MSA（多序列比对）的深度学习模型，利用进化信息 + attention 机制捕获长程残基关联
- 精度：接近实验精度的结构预测
- 对肽设计的特殊价值：建模肽-蛋白复合物、评估 binding mode，尤其能处理 induced-fit 构象变化——传统对接算法在此常常失败
- 动态支持：主要预测单一静态结构。但 AF-Cluster 协议通过 MSA 聚类可解锁多构象预测
- 拓扑支持：原生支持线性肽；环肽需要特殊输入格式或使用 AF3。AF3 的重大突破是原生支持非标准残基和非标准配体（广义全原子表征）
- 开源：AlphaFold Server 免费用于非商业研究；代码在 GitHub 上可用；AF3 代码可下载用于学术用途
- 关键局限：依赖 MSA 是内在瓶颈——对 de novo 设计的人工序列或缺乏同源数据的孤儿肽，预测置信度显著下降
ColabFold v1.5.5- 定位：AlphaFold 的用户友好封装，支持在单次会话中批量预测结构
- 实现形式：Google Colab notebook，也可本地安装或部署在 HPC 集群上
- 动态支持：支持 MSA 子采样和随机 seeding 来采样结构多样性，可生成超越单一折叠态的构象集合
- 拓扑支持：线性肽为主；通过自定义格式支持环肽约束
- 实操价值：对于没有 AlphaFold 本地部署资源的团队，ColabFold 是最快的上手路径
HighFold v1.0- 定位：专为环肽和含约束复合物设计的 AlphaFold 实现
- 核心差异化：显式支持 head-to-tail 环化和二硫键约束
- 动态支持：静态/约束导向——聚焦于精修单一高置信度环肽构象，不探索动态集合
- 适用场景：如果你的项目涉及环肽设计且需要结构预测，HighFold 是目前最直接的选择
- 开源：GitHub (hongliangduan/HighFold)
AfCycDesign- 定位：在 ColabDesign 框架内优化的环肽单体快速预测与设计工具
- 核心优势：解决了环肽训练数据不足的问题
- 适用范围：仅限单体，专精于 head-to-tail 环肽
- 便捷性：可通过 Neurosnap 或 Google Colab 直接使用，无需本地安装
ESMFold (基于 ESM-2)- 技术原理：基于 Meta AI 的 ESM-2 蛋白语言模型，直接从单条氨基酸序列预测 3D 结构，不依赖 MSA
- 核心优势：推理速度比 AlphaFold 快一个数量级，适合百万级候选肽的高通量筛选
- 动态支持：仅预测单一确定性静态结构，缺乏 MSA 多样性机制
- 拓扑支持：严格限于线性肽，无原生环肽支持
- 最佳使用方式：作为分层策略的第一道筛——用 ESMFold 快速过滤海量序列库，保留 top candidates 交给 AlphaFold 精修
PepFlow- 技术原理：基于能量景观的深度学习模型，使用 flow matching 预测动态折叠路径
- 训练数据：≤15 残基的短肽
- 核心差异化：这是 Table 1 中唯一被显式设计来预测动态折叠路径和再现实验构象集合的工具。其他工具（AlphaFold、ESMFold）本质上预测的是静态结构或需要额外协议来采样多构象，而 PepFlow 天然地输出构象集合
- 适用场景：短肽的柔性行为理解、构象集合分析
- 实际意义：对于需要理解肽在溶液中"真实行为"而非"冻结构象"的研究者（如药化学家评估候选肽的动态稳定性），PepFlow 填补了一个重要空白
- 开源：GitLab (oabdin/pepflow)
分层预测策略（Practical Recommendation）综述推荐的实操策略非常清晰：

![](https://mmbiz.qpic.cn/mmbiz_png/b4dmDNrOlLvda2XPSxya8UnTP4dSLwwkU3vjRjm3MwduoRoqXXPRia4luWMTBPpE0libr52wcAzFxaEuSq5rnYLd98Baupiav0TodgWiaa4gGkI/640?from=appmsg)

*Cordoves-Delgado and García-Jacas (2024) 的建议是：百万分子级别的初筛用语言模型，lead 优化阶段精确表征界面空间位阻时用 AlphaFold。理性的计算 pipeline 应根据项目阶段灵活切换。五、生成式 AI 工具全景：Table 2 逐工具解析Table 2（Comprehensive summary of mainstream generative AI frameworks for peptide design and inverse folding）是本综述另一张高价值表格。综述提出的分类框架基于驱动生成过程的主要数据模态：

![](https://mmbiz.qpic.cn/sz_mmbiz_png/b4dmDNrOlLv2JpvDlh2ACQmUYXtBWM2fDnDYj8YeZCZYyzbdkcOwG4fmno1buAtZnYuUibOcKbFEnYAkTiaFTx5qXZUbmJoU5P2YOhLDfqxeI/640?from=appmsg)

*分类一：序列优先生成（Sequence Generation Prioritization）核心理念：在序列空间中探索，发现具有理想理化性质的新 motif。适合功能肽（3D 结构动态或不严格定义的场景）。ProGen2 / ProtGPT2 — 蛋白语言大模型- 功能：非条件或属性引导的序列空间生成
- 输入：起始序列 prompt + 期望属性（如家族、功能）
- 核心局限：完全在 1D 序列空间操作，生成的线性序列缺乏定义的 3D 几何，需要后续结构预测
- 开源：github.com/salesforce/progen
EvoDiff — 离散扩散序列生成- 功能：直接在序列空间进行离散扩散生成，绕过 3D 结构模板依赖
- 输入：序列长度 + 部分序列上下文
- 核心优势：特别适合内在无序区域（IDR）和柔性线性肽——这些肽缺乏稳定的 3D 几何结构，结构导向的生成模型反而不适用
- 开源：github.com/microsoft/evodiff
PepMLM — 靶点感知的肽序列生成- 功能：基于 Span Masked Language Modeling 的靶点条件肽生成
- 输入：仅需靶蛋白序列或表征（不需要 3D 结构！）
- 核心优势：直接 sequence-to-sequence 生成靶点结合肽，完全绕过 3D 结构依赖
- 核心局限：输出缺乏显式空间结合 pose，需要后续对接验证
- 最佳适用场景：靶点缺乏高质量 3D 结构数据时，PepMLM 是最直接的选择
- 开源：github.com/programmablebio/pepmlm
分类二：结构优先生成（Structure Generation Prioritization）核心理念：以靶蛋白结合口袋为条件输入，de novo 生成几何互补的骨架。适合需要精确结构匹配的靶点（如平坦 PPI 界面）。RFdiffusion — 结构导向 de novo 生成的标杆- 功能：以靶蛋白 3D 结构和结合口袋/motif 为条件输入，通过扩散去噪从随机坐标生成互补骨架
- 核心优势：几何互补性优化的利器，SARS-CoV-2 miniprotein inhibitors（皮摩尔亲和力，月级开发周期）是其标志性验证
- 关键局限：
- 原始优化目标是刚性 miniprotein，迁移到线性肽时骨架常常需要严格的 MD 验证以防止溶液中展开
- "结构幻觉"问题（structural hallucination）——生成的骨架几何上与靶点互补，但违反基本理化原则：孤立的二级结构、缺乏有效疏水核心、过多溶剂暴露疏水面。后果是实验中聚集、溶解度差或无法折叠
- 开源：github.com/RosettaCommons/RFdiffusion
RFdiffusion3 — 全原子生物分子交互设计- 功能：将 RFdiffusion 从蛋白骨架扩展到全原子生物分子交互的 de novo 设计
- 意义：向"可以直接设计包含非标准化学修饰的肽"迈出了重要一步
分类三：序列-结构协同 / 多模态交互设计ODesign — 世界模型思路的全对全交互设计- 功能：统一多模态生物分子交互的设计——不将靶点作为静态约束，而是建模肽与靶点之间的动态互适应
- 核心差异化：传统方法将靶点当作"固定背景"，ODesign 则捕获结合过程中双方的结构变化（reciprocal structural adaptations）
- 意义：代表了向多模态、交互感知生成模型的快速演进，特别适合需要 induced-fit 建模的场景（线性肽结合靶点时常见）
- 开源：odesign.lglab.ac.cn
分类四：逆折叠工具（结构→序列的关键桥梁）ProteinMPNN — 逆折叠的事实标准- 功能：将 3D 骨架坐标翻译为能够折叠成目标构象的氨基酸序列
- 在 pipeline 中的定位：不仅是"逆折叠求解器"，更是连接几何设计与化学现实的关键过滤器——将扩散模型的几何意图保存到最终的化学组成中
- 核心局限：仅操作 20 种天然氨基酸；需要定制化再训练才能处理非天然修饰
- 开源：github.com/dauparas/ProteinMPNN
ESM-IF1 — 基于语言模型架构的逆折叠替代方案- 功能：利用 ESM 语言模型架构进行序列恢复
- 核心优势：对多样化的骨架几何高度鲁棒
- 局限：同样限于标准氨基酸表征
LigandMPNN — 突破天然氨基酸限制的逆折叠- 功能：在非蛋白原子（非天然氨基酸、金属离子、小分子配体）空间坐标条件下进行序列设计
- 核心突破：克服了 ProteinMPNN 和 ESM-IF1 仅限天然氨基酸的根本限制
- 实际意义：考虑到几乎所有临床成功的多肽药物都依赖化学修饰，LigandMPNN 是目前唯一能在逆折叠阶段就"看到"非天然组分的工具
- 开源：github.com/dauparas/LigandMPNN
端到端自动化方案BindCraft — 降门槛的一体化 pipeline- 功能：自动化 binder 设计流水线，整合 RFdiffusion + ProteinMPNN，内置置信度过滤
- 核心价值：显著降低入门门槛——对于没有足够算法工程资源来自行组装复杂 pipeline 的 CADD 团队，BindCraft 提供了"开箱即用"的方案
- 适用场景：生成线性和结构约束的 binder
- 开源：github.com/martinpacesa/BindCraft
六、场景导向的工具选择指南基于综述内容，以下是不同场景下的工具推荐：场景 1：靶点有高质量 3D 结构，目标是设计结构化 binder

![](https://mmbiz.qpic.cn/mmbiz_png/b4dmDNrOlLvKhlO4d7tfDEkKFnezpeZxMltj1FKnUnnUyUrwn6RAawnz3r2uqvf8WEwsuxR7QSM3UjRktn4ID1WUrBNsdD6j4qRFuVsTeQ/640?from=appmsg)

*场景 2：靶点缺乏 3D 结构，或目标是高度柔性的线性肽

![](https://mmbiz.qpic.cn/mmbiz_png/b4dmDNrOlLt3fWKlwKYWLwCb9MgfL5PIhMQO6qKrhxBOfx6UTH0Wickobj4XYg0uQJbxrl4Ehlggspf27Tom4jW17wicp0AibqUCXImIBYolS8/640?from=appmsg)

*场景 3：环肽设计

![](https://mmbiz.qpic.cn/sz_mmbiz_png/b4dmDNrOlLuS4G5Ouy3PPcT23Q0ribibGk8rCHJMZK59GejZw9B3ITuVgxOBxFIicJ51xBjelJnIszjbkE2noCkCic1jHjGPeib514sAavJAAMsw/640?from=appmsg)

*场景 4：涉及非天然氨基酸修饰

![](https://mmbiz.qpic.cn/mmbiz_png/b4dmDNrOlLv2HDytxFfAv8MZw5JXf19xRpdZI91BvCwHGWicVFOL3SE8vCdM5ftkgDicC9DDtkbFEfpHk8yuP89Z0Mc8KEC8IWwyf1bESibUe8/640?from=appmsg)

*场景 5：高通量虚拟筛选 / 海量候选序列评估

![](https://mmbiz.qpic.cn/mmbiz_png/b4dmDNrOlLvwHnOCFIRVWOY2icgXoSibiaZt6vZYjpOs4omIvibX0ymXNfMn6jG0dwxOx8rpuIzC4ByUFRTkae0Y12WGXyeCgBbV0GkjiaMJOYBs/640?from=appmsg)

*七、ADMET 与可开发性评估——常被忽视的关键环节综述强调：高 in silico 亲和力 ≠ in vivo 治疗效果。多肽候选物的临床淘汰主要由 ADMET 问题驱动。当前可用的预测工具包括：

![](https://mmbiz.qpic.cn/mmbiz_png/b4dmDNrOlLstdOIGvXwwSvib3HjJsY0qf8BCISd6KKMp9TJicj0aeAYPGR8g0XnEzA0P1ELGX8qF117caib4pGUHfol2xujzICy0IoGLSXdoQw/640?from=appmsg)

*已知问题：纯序列级别的属性预测器对高度新颖的线性骨架容易产生假阳性（训练数据未覆盖的化学空间）。综述推荐的解决方案是 1D 序列预测 + 3D 结构/动力学验证的整合：候选序列通过初筛后，用 AlphaFold 可视化溶剂可及表面积和蛋白酶切割位点的静电势，再用 GROMACS MD 模拟在显式水环境中跟踪 RMSD 和回转半径——区分物理上稳定的序列与倾向结构坍塌或聚集的序列。综述提出在闭环优化中嵌入 ADMET 约束的具体实现方式：- 硬规则过滤器：对违反关键理化阈值（如分子量过大、聚集倾向评分过高）的序列立即淘汰
- 多目标奖励函数：在主动学习迭代的打分函数中加入溶解度、蛋白酶稳定性、膜渗透性代理项
- 负设计信号：显式惩罚与免疫原性或代谢毒性相关的特征，将生成分布引导远离"有效但不可成药"的化学空间
八、Research Gaps 与算法开发机会综述揭示的以下 research gap 对 AIDD 算法工程师尤为重要——它们不仅是学术挑战，更是可以明确定义的技术需求：- Gap 1：非天然氨基酸（ncAA）的端到端建模：当前绝大多数 AI 模型（AlphaFold2、ESMFold、ProteinMPNN、所有 PLM 基生成模型）的训练数据以天然蛋白为主，其 tokenizer 基于 20 种天然氨基酸。然而临床上成功的多肽药物几乎都依赖化学修饰。
- Gap 2：结构幻觉的系统性解决：扩散模型（RFdiffusion 等）生成的骨架在几何上与靶点互补，但常违反理化原则（孤立二级结构、缺疏水核心、暴露疏水面），导致实验失败。
- Gap 3：环肽专用生成模型：环肽在成药性上有显著优势（蛋白酶抗性更强、构象熵惩罚更小、膜渗透性更好），但当前生成工具主要为线性肽/miniprotein 优化。结构预测方面有 HighFold 和 AfCycDesign，但 de novo 环肽骨架生成领域几乎是空白。
- Gap 4：从 affinity-only 优化到多目标 PK 协同优化：大多数闭环 pipeline 的核心优化目标仍是结合亲和力（通过 AlphaFold PAE/ipTM 代理）。PK/ADMET 约束虽被讨论但多停留在"后置过滤"而非"前置引导"
- Gap 5：可解释性与多模态数据融合
九、三个里程碑临床案例的技术拆解案例 1：Semaglutide — 理性 PK 工程的典范- 挑战：天然 GLP-1 骨架被酶快速降解、肾脏快速清除
- 计算策略：MD 模拟指导精确的氨基酸替换 + C18 脂肪酸 diacid 通过亲水 spacer 连接
- 设计逻辑：通过可逆白蛋白结合产生"储库效应"延长半衰期，同时 linker 几何设计避免与受体活性位点的空间位阻
- 启示：PK 属性可以被"设计为内在结构特征"，而不是依赖事后的化学修补
案例 2：MK-0616 — 计算刚性化实现口服大环肽- 挑战：抑制平坦 PPI 界面（PCSK9）需要的分子尺寸超过了肠道通透性极限
- 计算策略：计算指导大环支架的刚性化——将肽锁定为生物活性构象
- 设计逻辑：结构约束最小化结合的熵惩罚 + 优化分子内氢键以屏蔽极性基团（跨膜转运时）
- 范式意义：解耦了分子量与生物利用度——证明通过刚性化可以让口服大分子肽成为可能
案例 3：SARS-CoV-2 miniprotein inhibitors — 生成式 de novo 设计的速度验证- 挑战：新冠大流行需要快速产生治疗性分子
- 计算策略：RFdiffusion 类生成算法直接基于 Spike 蛋白的几何互补性设计合成骨架
- 结果：在数月内实现皮摩尔亲和力和动物模型中的有效中和
- 范式意义：验证了 compute-first discovery model——高亲和力治疗性分子可按需生成以应对新兴生物威胁
十、必读文献精选

![](https://mmbiz.qpic.cn/sz_mmbiz_png/b4dmDNrOlLuCDibcF4BcArhGdO22cN4JEnNxKqERBWFiaVCvEvgdsSXNReeAbiaxiaTmOSQUxutQMExicCe8ibWh1RumeIASaJW7ibM5JgeoJhdseU/640?from=appmsg)

*十一、总结这篇综述的核心贡献在于构建了一个跨越物理模拟、AI 预测和生成式 AI 三个层次的统一框架，并通过临床案例验证了这个框架的转化价值。对于不同角色的读者：对 CADD 科学家 / 药化学家：Table 1 和 Table 2 是你的工具索引。根据你的项目阶段（高通量初筛 vs. lead 优化）、靶点信息（有/无 3D 结构）、和肽拓扑（线性 vs. 环肽）选择工具组合。Figure 4 的对接工具分类图值得打印出来贴在工位上。对 AIDD 算法工程师：五个 research gap（ncAA 建模、结构幻觉、环肽生成、多目标 PK 优化、XAI + 多模态融合）都是有明确需求定义的算法开发方向。其中"非天然氨基酸的端到端建模"和"环肽专用生成模型"的价值最高——它们直接对应临床转化中最尖锐的瓶颈，且现有解决方案最为稀缺。对所有读者的共同提醒：这个领域当前最大的共识性教训是——生成模型的"置信度分数"不等于实验成功率。分层验证不是可选项，是必选项。在"结构幻觉"问题被从根本上解决之前，任何 pipeline 都需要在生成之后嵌入严格的物理验证阶梯。本文基于 Hu et al. (2026) 发表在 The Innovation Drug Discovery 上的综述文章进行解读分析。- 原文：Hu W., Sun Y., Li T., et al. (2026). The evolution of computation-driven paradigms in targeted peptide drug design: From predictive modeling to generative AI and clinical translation. The Innovation Drug Discovery 1(1):100009.
