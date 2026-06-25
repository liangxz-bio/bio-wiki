---
title: "ORI — 腾讯AI4S 本体强化迭代蛋白设计闭环（Ontology Reinforcement Iteration）"
created: 2026-05-22
updated: 2026-05-22
type: entity
tags: [protein-design, generative-model, reinforcement-learning, wet-lab-feedback, enzyme-design, nature-communications]
domain: ai-drug-discovery
sources:
  - raw/articles/ORI-腾讯AI4S蛋白设计闭环-NatureComm2026.md
confidence: medium
---

# ORI — Ontology Reinforcement Iteration

> He B, Qin C, Zhao Y, Huang L-K, Wu Z, Wang F, Wu F, Yang F, Yao J. Functional protein design and enhancement with ontology reinforcement iteration. Nature Communications (2026). | AI药物设计实验室 微信公众号

## 研究概览

ORI（Ontology Reinforcement Iteration）是腾讯 AI for Life Sciences Lab 提出的蛋白设计闭环框架，核心创新在于将**湿实验反馈直接纳入模型训练循环**，而非仅作为验证手段。框架由三个组件（PDA + PGM + USM）和一个反馈机制（RLWF）组成，在溶菌酶、热稳定几丁质酶和双功能酶上完成实验验证。

## 模型架构

ORI 是四部分闭环：PDA（需求理解）→ PGM（蛋白生成）→ USM（质量控制）→ RLWF（实验反馈更新）。

### PDA — Protein Design Agent（需求翻译层）

- 输入：自然语言描述（如"耐热溶菌酶"）
- 输出：ontology prompt（结构化本体提示），包含物种/结构/功能/热稳定性/溶解性等标签
- 作用：将用户功能需求转化为 PGM 可理解的条件约束，不需要结构模板

### PGM — Protein Generation Model（蛋白生成模型）

| 属性 | 值 |
|------|-----|
| 参数规模 | 3B 参数 |
| 模型类型 | 自回归蛋白生成模型 |
| 训练数据 | 1.66 亿条 UniRef90 代表序列 |
| 训练 token 规模 | 约 600 亿 |
| 生成机制 | 基于 ontology prompt 条件生成候选蛋白序列 |
| 更新机制 | RLWF 接收湿实验反馈后更新模型参数 |

### USM — Unified Sequence Model（统一序列模型）

- 模型类型：蛋白性质预测模型
- 输入模式：SS（单序列）和 MSA（多序列比对）双模式
- 架构创新：Column Attention + Fast Row Attention + SwiGLU Activation
- 规模：USM 100M

| 指标 | USM 100M | 对比 |
|------|---------|------|
| 训练吞吐 | 2939 GFLOPS | 同参数 ESM2 的 100 倍以上 |
| 荧光预测 | 达到或超过 ESM2 15B | — |
| 稳定性预测 | 达到或超过 ESM2 15B | — |
| 抗生素耐药性 | 达到或超过 ESM2 15B | — |
| 金属离子结合 | 达到或超过 ESM2 15B | — |
| 熔解温度预测 | Pearson r = 0.818 vs 实验值 | — |

### RLWF — Reinforcement Learning from Wet-lab Feedback

- 核心机制：将湿实验数据（表达量、酶活、热稳定性等不可微指标）转化为模型偏好信号
- 更新对象：PGM 生成模型
- 效果：闭环迭代后，pLDDT 中位数从约 75 提高到约 85

## 关键创新

1. **湿实验反馈进入训练循环** — 不同于多数蛋白生成模型（生成→筛选→验证即止），ORI 将表达/酶活数据用于 RL 更新 PGM，让生成模型直接学习"什么序列在实验中表现好"
2. **本体提示代替结构模板** — 用户用功能/性质标签（物种、功能、热稳定性、溶解性）指定需求，无需提供结构模板或骨架坐标，更贴合蛋白工程实际需求
3. **USM 效率优势** — USM 100M 以比 ESM2 15B 小 150 倍的参数量达到可比或更优的预测性能，训练速度超 100 倍，兼顾筛选效率和质量

## 实验结果

### 溶菌酶（闭环验证）
- PGM 初始生成 10,000 候选 → USM 筛选后结构域比例从 45.89% 提升至 98.91%
- TX-L6（初始最优）：1598 U/mg（~2.7 倍天然鸡蛋清溶菌酶）
- TX-RL15（RLWF 迭代后）：相比 TX-L6 提升 66 倍，相比天然溶菌酶超两个数量级
- RLWF 后 pLDDT 中位数从 75 → 85

### 热稳定几丁质酶
- 将 melting temperature 作为本体条件约束生成
- USM 熔解温度预测 Pearson r = 0.818
- 成功生成 85°C 下保持活性的几丁质酶

### 双功能酶（溶菌酶 + 几丁质酶）
- PDA 生成 10,000 候选 → 3 个 USM 模型筛选 → 426 候选 → 随机选 100 实验
- 65 个成功表达，25 个表现出双酶活性
- TX-ME：TIM-barrel fold，溶菌酶/几丁质酶活性均超过天然双功能酶 Hevamine A 和单功能参考酶

## 局限性

- 未纳入专门任务归纳偏置（治疗肽优化、翻译后修饰建模、DNA 结合蛋白设计）
- 对高构象柔性、强翻译后修饰、内在无序蛋白（抗体、治疗肽、病毒糖蛋白）仍面临挑战
- 实验结果基于 3 个酶体系，泛化性需更多验证
- RLWF 的收敛效率和稳定性有待在大规模任务中验证

## 交叉引用

- [[entities/de-novo-protein-design]] — David Baker 从头蛋白设计综述，ORI 是"实验反馈驱动"路线的重要实践
- [[entities/ovo]] — Ovo 开源蛋白设计生态（工作流编排类），与 ORI（闭环优化类）互补
- [[entities/seqdance-esmdance]] — SeqDance/ESMDance pLM 在突变效应预测上与 ORI PGM/USM 为不同技术路线（MD 动力学预训练 vs 本体条件生成 + RL 反馈）
- [[entities/isodde]] — IsoDDE 蛋白-小分子/抗体-抗原统一预测（同为 AI 蛋白/药物设计但聚焦预测而非生成+反馈）
- [[concepts/transformer]] — PGM 自回归架构和 USM 注意力机制均基于 Transformer
