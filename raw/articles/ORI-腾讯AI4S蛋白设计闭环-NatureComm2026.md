---
source_url: https://doi.org/10.1038/s41467-026-69855-6
ingested: 2026-05-22
sha256: 10759446d82b8944906f6a525842bdf752bef5362e75473fefab3119b53c8f11
citation: "He B, Qin C, Zhao Y, Huang L-K, Wu Z, Wang F, Wu F, Yang F, Yao J. Functional protein design and enhancement with ontology reinforcement iteration. Nature Communications (2026). DOI: 10.1038/s41467-026-69855-6"
domain: ai-drug-discovery
tags: [protein-design, generative-model, reinforcement-learning, wet-lab-feedback, enzyme-design]
---

# Nature Communications 2026｜腾讯AI4S提出ORI：把蛋白设计做成"生成—实验—再优化"的闭环

> **来源：** AI药物设计实验室

AI 设计蛋白这几年进展很快，但很多方法都会遇到一个现实问题：模型觉得分数高的序列，到了实验里可能表达不好、酶活不高、稳定性也不一定够。

蛋白工程里真正难的目标，比如酶活、表达量、热稳定性，很多都是稀疏、有噪声、不可微的实验指标。

这篇论文提出 ORI（Ontology Reinforcement Iteration，本体强化迭代）。

它的核心思路：先用结构化本体提示模型生成蛋白，再用预测模型筛选候选，最后把湿实验结果反馈给模型，让模型下一轮生成得更接近实验目标。

论文在溶菌酶、几丁质酶和双功能酶上做了验证，结果包括：活性提升超过两个数量级的溶菌酶、85°C 下仍能保持活性的几丁质酶，以及同时具有溶菌酶和几丁质酶活性的双功能蛋白。

## 一眼看懂
**问题**：计算蛋白设计和真实实验之间有落差，尤其是酶活、表达、热稳定性这类实验目标。

**方法**：ORI 把 PDA（Protein Design Agent，蛋白设计智能体）、PGM（Protein Generation Model，蛋白生成模型）和 USM（Unified Sequence Model，统一序列模型）放进同一个闭环。

**关键机制**：RLWF（Reinforcement Learning from Wet-lab Feedback，基于湿实验反馈的强化学习）把湿实验结果变成模型偏好，用来更新生成模型。

**结果**：RLWF 后，模型生成的蛋白表达和酶活明显提升；TX-RL15 相比原始高活性设计 TX-L6 提升 66 倍，相比天然鸡蛋清溶菌酶超过两个数量级。

## 论文做了什么

### 1. 方法：把"需求"翻译成可控的蛋白生成流程
ORI 由三个部分组成。

PDA 负责理解用户需求，把自然语言需求转成 ontology prompts（本体提示）。

这里的本体包括物种、结构、功能、热稳定性、溶解性等标签。

PGM 是 3B 参数的自回归蛋白生成模型。

它用带本体标注的代表性蛋白数据训练，训练数据包括超过 1.66 亿条 UniRef90 代表序列，整合后的 token 规模约 600 亿。PGM 根据本体提示生成候选蛋白序列。

USM 负责质量控制。

它预测结构置信度、功能相关性、热稳定性、溶解性等性质，再筛出更适合进入湿实验的候选蛋白。实验结果回来后，ORI 用 RLWF 更新模型，让下一轮生成更贴近真实实验目标。

### 2. USM：用统一序列模型做候选筛选
论文中的 USM 同时支持 SS（single sequence，单序列）和 MSA（multiple sequence alignment，多序列比对）输入。

模型结构加入 Column Attention（列注意力）、Fast Row Attention（快速行注意力）和 SwiGLU Activation（门控线性单元激活），目的是提升 MSA 特征提取效率。

USM 100M 在训练速度上达到 2939 GFLOPS，超过同参数 ESM2 的 100 倍以上。在荧光、稳定性、抗生素耐药性和金属离子结合等预测任务上，USM 100M 的表现达到或超过 ESM2 15B。

## 结果：从酶活、热稳定性到双功能酶

### 1. 溶菌酶：先生成，再用实验反馈继续提升
论文先让 PGM 生成 10000 条候选蛋白，再用质量控制流程筛选。

原始输出里，只有 45.89% 的候选带有目标功能结构域；经过 USM 筛选后，这一比例提高到 98.91%。

随后研究者随机选择 100 个预测具有溶菌酶活性的候选进行表达实验。

在初始模型中，TX-L6 在 pH 7.5 下活性达到 1598 U/mg，大约是天然鸡蛋清溶菌酶的 2.7 倍。

接着，研究者把表达和酶活数据用于 RLWF 更新 PGM。

更新后，模型生成蛋白的 pLDDT（预测局部距离差异测试，结构置信度指标）中位数从约 75 提高到约 85。

表达实验显示三类溶菌酶样蛋白的表达量都有提高。

### 2. 热稳定几丁质酶：把目标温度写进生成提示
几丁质酶在工业应用中很重要，但很多天然几丁质酶在 60°C 左右就会失活。

ORI 的做法是把 melting temperature（熔解温度）作为本体条件加入提示，让模型生成带有热稳定性约束的候选蛋白。

USM 的熔解温度预测与实验值的 Pearson 相关系数达到 0.818。

### 3. 双功能酶：一个蛋白同时做两件事
论文最后测试 ORI 是否能设计 multi-enzyme activity（多酶活性）蛋白。目标是让一个蛋白同时具备溶菌酶和几丁质酶活性。

研究者用 PDA 条件提示生成 10000 个潜在双功能蛋白，再用 3 个 USM 模型筛选出 426 个候选。

随后随机选择 100 个做实验，其中 65 个成功表达，25 个表现出双酶活性。

排名最高的 TX-ME 采用 TIM-barrel fold（TIM 桶折叠），并且在溶菌酶活性和几丁质酶活性上都超过了天然双功能酶 Hevamine A，也超过了对应的单功能参考酶。

## 我们的看法
很多蛋白生成模型强调生成能力，但实验指标往往进入得太晚。

ORI 把湿实验反馈放进训练环节，让模型直接学习哪些序列更容易表达、更有活性、更耐热。

另外，ORI 没有让用户直接指定复杂结构模板，而是用功能、结构、性质标签来约束生成。这样更接近实际蛋白工程的需求：研究者通常知道自己想要什么功能和性质，但未必知道最合适的结构解法。

## 边界
论文也提到，ORI 目前还没有显式纳入一些专门任务的归纳偏置，比如治疗肽优化、翻译后修饰建模、DNA 结合蛋白设计等。

对于高构象柔性、强翻译后修饰、内在无序的蛋白类型，比如抗体、治疗肽和病毒糖蛋白，ORI 仍然面临挑战。

## 论文信息
**论文标题**：Functional protein design and enhancement with ontology reinforcement iteration

**期刊**：Nature Communications，2026

**作者**：Bing He、Chenchen Qin、Yu Zhao、Long-Kai Huang、Zihan Wu、Fang Wang、Fandi Wu、Fan Yang、Jianhua Yao

**机构**：Tencent AI for Life Sciences Lab

**DOI**：https://doi.org/10.1038/s41467-026-69855-6

**代码**：https://github.com/TencentAI4S/ori
