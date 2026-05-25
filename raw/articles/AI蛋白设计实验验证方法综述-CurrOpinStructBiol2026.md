---
source_url: https://mp.weixin.qq.com/s/dxpOckpM3I6F_8qkLG-6sQ
ingested: 2026-05-13
sha256: c9857d027c3427c3ddcedd444f32be8a27dd7fd458bd9e93833193e28e300e83
citation: "Closing the loop: Experimentally validated methods in artificial intelligence-driven protein design. Curr Opin Struct Biol. 2026. doi:10.1016/j.sbi.2026.103272 | 微信公众号 BioAIDesign (JoeYo) 中文解读"
domain: ai-drug-discovery
tags: [review, protein-design, de-novo, binder, antibody, enzyme, experimental-validation, ai-drug-discovery]
---

# Curr. Opin. Struct. Biol. | AI 蛋白设计到底哪类方法真正在实验里"跑通了"？

> 原文链接：https://mp.weixin.qq.com/s/dxpOckpM3I6F_8qkLG-6sQ
> 来源：BioAIDesign（JoeYo）
> 原始文献：*Closing the loop: Experimentally validated methods in artificial intelligence–driven protein design*，Curr. Opin. Struct. Biol., 2026，Doi: 10.1016/j.sbi.2026.103272

## ✨ 导语

这几年，AI 蛋白设计几乎每个月都有新模型出现：

- 做 binder 的
- 做 antibody 的
- 做 enzyme 的
- 做序列生成的
- 做结构扩散的
- 做全原子设计的

但真正让人关心的问题其实很简单：

> **这些方法到底哪些是真的在实验里做出来了？**

这篇综述最有价值的地方，就在于它没有只讲"模型概念"，而是把视角放回了整个完整流程：

> **数据 → 模型 → 生成 → 筛选 → 实验验证**

作者想回答的核心问题不是"哪个模型最炫"，而是：

> **哪些 AI 蛋白设计方法，已经在实验层面证明自己真的有效。**

---

## 🔍 这篇综述到底讲了什么？

文章把 AI 驱动的蛋白设计整理成一个完整 pipeline，包括：

1. **数据集构建**
2. **模型训练**
3. **候选生成**
4. **计算筛选**
5. **实验验证**
6. **最终部署**

作者强调，AI 蛋白设计不能只看模型本身。
很多时候，一个方法能不能成功，取决于：

- 用了什么训练数据
- 怎么定义条件输入
- 怎么筛候选
- 最后用什么实验验证

也就是说，真正决定方法价值的，不只是"能不能生成序列"，而是：

> **能不能把设计闭环走到实验这一步。**

---

## 🧠 这篇文章一个很重要的观点：AI 蛋白设计本质上不是"单模型竞赛"

综述里反复强调一个判断：

> **AI 蛋白设计真正有效的，不是某一个孤立模型，而是一整条设计—验证闭环。**

比如 structure-based 路线，往往会经历：

- 先生成 backbone
- 再做 sequence design
- 再用结构预测模型回查是否真能折叠
- 最后再选一些做实验

而 sequence-based 路线则更多依赖：

- 大规模序列数据
- 自回归或 masked language model
- 条件生成或微调
- 再结合实验读出验证功能

所以这篇综述最值得看的地方，不只是列模型，而是它提醒我们：

> **离开了实验验证，AI 蛋白设计很难说真正成功。**

---

## 🧬 第一大方向：binder 设计已经进入"实验成功率可比较"的阶段

在所有应用里，**binder design** 可能是当前最成熟的一类。

文章总结了很多近两年的代表模型，比如：

- **RFdiffusion**
- **BindCraft**
- **BoltzGen**
- **AlphaProteo**
- **PXDesign**
- **LatentX**
- **Chai-2**
- **EvoBind2**
- **RFpeptides**

这些方法已经在很多经典靶点上做出了实验验证的 binder，比如：

- **PD-L1**
- **IL-7RA**
- **TrkA**
- **VEGF-A**
- **SC2RBD**
- **MCL1**
- **GABARAP**

而且有些模型的实验成功率已经不低。
文章里给出的表格显示，不同方法在不同靶点上的成功率可以达到：

- 十几个百分点
- 甚至更高

这说明 binder 设计已经从"少量偶然成功案例"，进入到某种程度上可以系统比较 success rate 的阶段。

---

## 💉 第二大方向：antibody 设计正在快速升温，但 still harder than binder

相比一般 binder，**antibody design** 的难点更复杂。

因为抗体不仅要考虑：

- 抗原结合
- CDR 环的灵活性
- 框架区兼容性
- developability
- humanization
- 甚至后续的工程化可行性

综述里总结了多个已经有实验验证的 antibody / nanobody 设计方法，比如：

- **AbDiffuser**
- **BoltzGen**
- **Chai-2**
- **GeoFlowV3**
- **Germinal**
- **IgGM**
- **mBER**
- **RFantibody**

这些方法已经能在一些目标上做出：

- **subnanomolar affinity**
- **double-digit success rate**
- 甚至成体系的 epitope-targeted generation

但作者也很明确指出：

> **抗体设计虽然进展很快，但整体仍然比普通 protein binder 更难。**

尤其在：

- 框架稳定性
- 免疫原性
- 多目标 developability

这些问题上，仍然很依赖后续筛选和实验反馈。

---

## ⚗️ 第三大方向：enzyme design 进展明显，但依然是最难的一类

作者对 enzyme design 的判断很清楚：

> **它是最难的蛋白设计任务之一。**

原因也很直接。做酶设计，不只是让蛋白折起来，还要同时满足：

- 活性位点几何
- 底物定位
- 过渡态稳定
- 电荷与微环境
- 甚至反应路径本身

综述里提到的代表方法包括：

- **RFdiffusion2**
- **RFdiffusion3**
- **PLACER**
- **RiffDiff**
- **ProGen2**
- **ZymCTRL**
- **ProtGPT2**

这些方法已经能在一些任务上做出活性酶，比如：

- luciferase
- serine hydrolase
- zinc metallohydrolase
- retro-aldolase
- Cas9 nuclease
- triosephosphate isomerase

但总体来看，enzyme design 目前仍然比 binder 和 antibody 更依赖：

- 特定 assay
- 特定底物
- 特定反应体系
- 以及更强的物理/化学先验

所以这篇综述给出的信号是：

> **酶设计已经在前进，但还远没有到"广泛通用"的阶段。**

---

## 📊 最值得收藏的部分

这篇综述最实用的地方，其实不是正文，而是后面的几张总表。
作者把不同方法在实验里的表现，按任务分门别类地整理成了表格：

- **Table 1 / 2：AI 设计的 binder**
- **Table 3：AI 设计的 antibody**
- **Table 4：AI 设计的 enzyme**
- **Appendix Table A.5–A.7：更多额外实验验证案例**

可以很直观地看到：

- 哪个模型在哪个任务上做过实验
- 用了什么验证手段
- 成功率大概是多少
- 哪些靶点是大家反复测试的"标准靶点"

这类信息，比单独读每篇方法论文更适合快速建立全局认识。

---

## 💡 核心观点总结

> **AI 蛋白设计的真正进步，不在于模型越来越复杂，而在于有多少设计真的闭环到了实验。**

它其实在提醒我们三件事：

1. **数据依然是起点**——没有高质量序列/结构/功能数据，模型学不到真正有用的规律。
2. **筛选策略很关键**——很多成功并不是因为模型"一步到位"，而是因为后面有合理的过滤、排序和人工检查。
3. **实验反馈才是真正的裁判**——特别是在 binder、antibody、enzyme 这些任务里，实验定义了最终的"成功"。

所以这篇文章给人的最大启发是：

> **AI 蛋白设计不是模型替代实验，而是越来越依赖一个"设计—验证—再设计"的闭环。**

---

## 📝 一句话总结

如果说过去大家会问：

> **这个 AI 模型能不能设计蛋白？**

那么这篇综述更进一步在问：

> **这个模型设计出来的蛋白，有多少真的在实验里成功了？成功的是哪一类任务？又是靠怎样的数据、筛选和验证流程闭环起来的？**

这正是这篇综述最值得读的地方。

---

## 📚 文献信息

**Title:** *Closing the loop: Experimentally validated methods in artificial intelligence–driven protein design*
**Journal:** Current Opinion in Structural Biology, 2026
**Doi:** 10.1016/j.sbi.2026.103272
