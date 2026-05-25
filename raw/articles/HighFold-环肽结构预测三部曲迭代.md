---
source_url: https://mp.weixin.qq.com/s/unknown
ingested: 2026-05-18
sha256: b83c4078758c85e2db144c5c9719b9b9166c1426254c00f65e49af19ddc4297d
citation: "AI药物设计实验室. 从 HighFold 到 HighFold3，环肽结构预测三部曲迭代解读. 微信公众号. 2026. | Hongliang Duan Lab, HighFold series."
domain: ai-drug-discovery
---

# 从 HighFold 到 HighFold3，环肽结构预测三部曲，是如何迭代的

> **来源：** AI药物设计实验室

---

🔔关注我们，每日分享最新的人工智能与生命科学论文～**从 HighFold-1 到 HighFold-3，都在思考：如何让结构模型真正按**环肽**的规则去预测结构。

**普通蛋白结构模型放到环肽上，一般会有四个难点：

第一，模型默认把序列当成线性链，环肽的闭环没有被明确写进去。

第二，很多环肽还有二硫键，谁和谁成键，会直接改拓扑。

第三，很多药用环肽会加非天然氨基酸，原模型对这类残基的表示不完整。

第四，同一条肽有时是线性，有时是环化。模型得先知道这次要预测的是哪一种拓扑。

HighFold 线，就是按这个顺序进行迭代的。

HighFold 先完善了闭环 + 二硫键 + 复合物拓扑。

HighFold2 再完善非天然氨基酸怎么表示。

HighFold3 把约束结合到 AlphaFold 3 上，再完善线性和环化怎么切换。

## 一、HighFold：先把环肽最基本的拓扑补进去

![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/xawOdPt2Qv52X8Aselbh9mpSOktpptRhg9knPN6F5nrW0LN6yylRTRPugTbUu4cz703KJbs0g3P19BoImnBPbK0Yib4rbibDnr2icw5vAmkpGQ/640?wx_fmt=jpeg&from=appmsg)

### 解决了什么问题
HighFold 先解决一个最基础的问题，让模型知道输入是一条环肽。
### 方法
HighFold 建在 AlphaFold-Multimer 上，核心改动是一套新的相对位置编码矩阵 CycPOEM 。

原来的位置编码只看线性序列上的距离，CycPOEM 改成看环肽拓扑上的最短路径。这样head-to-tail 闭环、二硫键、肽键方向都会进矩阵。

如果序列里有多个半胱氨酸，问题还会再多一层，因为二硫键配对不止一种。**HighFold 就把可能的配对都枚举出来，每一种配对都各跑一遍预测。**再按置信度排序，选最好的结果。

这代模型分成两支。**HighFold_Monomer 做环肽单体。**HighFold_Multimer 做环肽复合物。**复合物里，受体还是原来的线性位置编码，环肽配体换成 CycPOEM。

### 做了什么
HighFold 最关键的变化，是把 拓扑距离关系 直接写进了输入。

模型同时看到线性顺序、闭环后的邻接关系和二硫键带来的额外约束。

### 结果
在天然环肽单体上，HighFold_Monomer 的中位 RMSD 是 1.058 Å，平均 1.478 Å。63 个样本里，有 37 个低于 1.5 Å。**

在外部 15 个复合物数据上，HighFold_Multimer 的平均 Fnat 是 0.821，AlphaFold_Multimer 是 0.787，ADCP 是 0.220。但AlphaFold_Multimer 给出的很多配体其实还是线性肽构象。

### 边界
第一代把天然环肽的拓扑问题明确建模。但输入对象还是天然氨基酸。非天然氨基酸还没被处理。

## 二、HighFold2：把非天然氨基酸也放进这套框架

![](https://mmbiz.qpic.cn/sz_mmbiz_png/xawOdPt2Qv4ZPB1ia1bOr30t7nAVW0RmPA0Xf4BF2ibmcYLppUFr4g9t1Aw2DGmHZ1afBRNoibyhRia0umicKYM1HVOhqbDKSaibbuWmCxRLuJHro/640?wx_fmt=png&from=appmsg)

### 解决了什么问题
很多药用环肽不会只用 20 种天然氨基酸，它们会加入非天然氨基酸，去改稳定性、结合和通透性。

第一代能处理“环”。**第二代要处理“残基的定义”。

在原有流程里，这类残基有两个明显问题。**第一，MSA（多序列比对）阶段容易把它们当成 X。**第二，模型没有现成的原子级定义去放这些残基的侧链和几何。

HighFold2 让模型既知道环肽拓扑，也知道非天然氨基酸的原子组成和几何。

### 方法
HighFold2 在 AlphaFold-Multimer 上加了三层设置。

第一层是原子级特征模块。**

模型同时看残基级信息、原子信息和键信息，这样不同的非天然氨基酸就能被区分开。

第二层是扩充刚体分组和坐标系。

把原来只给天然氨基酸准备的刚体分组和初始原子坐标，扩到非天然氨基酸，模型可以端到端地给这些残基生成原子坐标。

第三层是把环肽的位置编码也改掉。

head-to-tail 闭环和二硫键约束继续写进相对位置矩阵。这一步沿用了第一代的思路。

因为带非天然氨基酸的环肽结构太少，作者先在线性肽数据上微调，再把位置编码改到环肽上。线性数据里一共筛出 382 个样本，覆盖 23 种非天然氨基酸。环肽测试集最后是 34 个样本。

预测之后，HighFold2 还加了一步结构松弛，给非天然氨基酸补了兼容 Amber 的力场参数。然后做能量最小化，主要用来清理原子碰撞和局部几何。

### 做了什么
HighFold2 的本质是把模型对肽的理解从“残基级”推进到“残基级 + 原子级”。

让模型能真正分清不同非天然氨基酸，也才能把这些残基放到合理的三维位置上。

### 结果**
消融实验**去掉改过的相对位置编码，影响最大。很多环肽会被直接预测成线性肽。去掉原子级特征模块，结果也会继续变差，连二硫键位置都会偏。

### 相比 HighFold，前进在哪里
和第一代比，HighFold2 有三个改进。

第一，模型从天然氨基酸扩到了 23 种常见非天然氨基酸。**第二，输入从残基级扩到了原子级。**第三，预测后多了一步结构松弛，用来修原子碰撞和局部几何。

### 边界
底座还是 AlphaFold-Multimer。非天然氨基酸的覆盖范围也还是 23 种。线性和环化两种拓扑，也还没有在同一套入口里显式切换。

## 三、HighFold3：约束结合 AlphaFold 3，再把线性和环化拆开

![](https://mmbiz.qpic.cn/sz_mmbiz_png/xawOdPt2Qv5s0mk2eibhKNx2KWd9djMe2GgnhwWyJneBuuGmwWW0BMDANPD9RJ3cVrkqDrySLsDswBLrEu74J1GWgNv8pGvm1mnJycEwhCPU/640?wx_fmt=png&from=appmsg)

### 解决了什么问题
到了 AlphaFold 3，模型已经能处理更复杂的化学成分。但环肽的闭环拓扑，还是没有被单独强调出来。

论文里直接做了对比。对没见过的环肽序列，AlphaFold 3 还是会把一些样本预测成线性构象。

### 方法
HighFold3 底座使用 AlphaFold 3 上。作者保留了 CycPOEM，又加了一个 Cyclization 开关。

开关的作用：**如果用户输入的是线性肽，模型就用线性位置矩阵。**如果用户输入的是环肽，模型就用 CycPOEM。**所以同一条序列，模型可以按两种不同拓扑去预测。

HighFold3 还有四个扩展。

第一，AlphaFold 3 的预训练参数被冻结，主要改的是输入特征。

第二，复合物里还是把受体和肽分开编码。受体走线性矩阵，环肽配体走 CycPOEM。

第三，支持同一个受体配多个环肽配体一起建模。

第四，接上了 CCD（Chemical Component Dictionary，化学组分字典）。把支持范围从 HighFold2 的 23 种常见非天然氨基酸，扩到了 CCD 里已知的非天然氨基酸类型。

### 做了什么
前两代的重点，主要还是环肽。第三代直接拆成 HighFold3-Linear 和 HighFold3-Cyclic 两个子模型。同一条序列，到底按线性还是按环化来预测，可以由输入直接决定。

### 结果如何
消融实验**去掉 CycPOEM，内部数据的平均 RMSD_Cα 会从 0.305 Å 升到 0.804 Å。外部数据会从 0.612 Å 升到 1.032 Å。有些样本会直接从环肽变回线性预测。

论文拿 100 条来自环肽数据库的序列去测。**AlphaFold 3 的 head-to-tail 闭环成功率只有 21%。**HighFold3-Cyclic 是 100%。

同一条序列 KLARLLT 也被拿来做了演示。**HighFold3-Linear 会给出线性构象。**HighFold3-Cyclic 会给出闭环构象。**这正是这代最核心的变化。

![](https://mmbiz.qpic.cn/mmbiz_png/xawOdPt2Qv6aiaJRPnFQHEujbc5eXfU8rBmX9ricKNibddTYf7B5DluicMbGWjpz6fSjqn26kmXpPoD843KjK3Is1Io7D2uPBomRIt6eoqC6Uxw/640?wx_fmt=png&from=appmsg)

### 相比 HighFold2，前进在哪里
和第二代比，HighFold3 主要有四个变化。

第一，底座从 AlphaFold-Multimer 换成了 AlphaFold 3。**第二，非天然氨基酸的覆盖范围从 23 种常见类型，扩到了 CCD 里已知的类型。**第三，线性肽和环肽被拆成了两条入口，可以显式切换。**第四，它在天然环肽、环肽复合物、带非天然氨基酸的环肽、线性肽这几类任务上，都比前一代更好。

### 边界
HighFold3 主要还是静态结构预测。**

## 四、三代放在一起对比
版本补了什么结果主线HighFold闭环、二硫键、复合物拓扑先让模型知道“这是环肽”HighFold2非天然氨基酸表示再让模型知道“这个残基到底是什么”HighFold3AlphaFold 3 的环肽拓扑 + 线性/环化切换最后让模型知道“这次该按线性还是按环肽来算”**
