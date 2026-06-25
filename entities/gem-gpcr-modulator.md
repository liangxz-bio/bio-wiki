---
title: "GEMs — 从头设计 GPCR 跨膜结构域变构调节蛋白"
created: 2026-05-18
updated: 2026-05-18
type: entity
tags: [GPCR, protein-design, de-novo, RFdiffusion, cryo-em, AI-drug-discovery, structural-prompting]
domain: ai-drug-discovery
sources:
  - raw/articles/信手随翻209-Nature-从头设计GPCR结构Binder调节剂.md
confidence: medium
---

> 来源: 浙江大学. *De novo design of transmembrane binding proteins for GPCR modulation*. Nature. 2025. doi:10.1038/s41586-025-09957-1

## 核心发现链

GEMs（GPCR Exofacial Modulators）是浙江大学团队发表于 Nature 的突破性工作：利用 RFdiffusion + ProteinMPNN + AF2 迭代管线，**从头设计直接靶向 GPCR 跨膜结构域（TMD）的外骨架变构调节蛋白**。核心创新在于将 NLP 的"提示词工程"思想迁移到蛋白质设计中，通过三种物理约束策略（定点插入/位点阻断/构象诱导）精确控制生成蛋白的结合位置和功能。以多巴胺 D1 受体（D1R）为模型，成功设计出四种功能各异的 GEM（Anchor/BAM/NAM/ago-PAM），Cryo-EM 验证 RMSD <1.0 Å，且证明可通过组合不同 GEM 实现 GPCR 信号的模块化编程。

## 关键证据摘要

### 技术管线
- **高通量盲扫**：RFDiffusion 生成 1,500 个单次跨膜螺旋探针 → AF2-multimer 对接 → 锁定 D1R 的三个靶向空腔（TM1/2/4、TM3/4/5、TM5/6/7）
- **迭代重折叠**：RFDiffusion 生成骨架 → ProteinMPNN 设计序列 → AF2 预测结构 → 循环优化（hallucination-like）
- **三种结构提示词（Structural Prompting）**：
    - 定点插入（Site-directed insertion）：将序列插入受体特定 loop 区，限制空间位置
    - 位点阻断（Site-blocking pre-occupation）：用高亲和力假蛋白占住竞争位点
    - 构象诱导（Conformation induction）：引入 G 蛋白保持激活态构象，引导设计适配

### 四种功能 GEM
| GEM | 靶向口袋 | 功能 | 机制 |
|-----|---------|------|------|
| Anchor | TM1/2/4 | 锚定剂（无功能扰动） | 结合但不干扰受体构象 |
| BAM | TM3/4/5 | 偏向性变构调节剂 | 抑制 β-arrestin 通路，保留 Gs 信号 |
| NAM | TM5/6/7（失活态） | 负向变构调节剂 | 空间位阻堵死 TM6 向外扩张路径 |
| ago-PAM | TM5/6/7（激活态） | 激动型正向变构调节剂 | 钳状结构主动拉扯 TM6，稳定激活构象 |

### Cryo-EM 验证
- Anchor / NAM / ago-PAM 的 AF2 预测与实构 RMSD ≤ 1.0 Å
- BAM（TM5/6 高度柔性区域）RMSD 2.7 Å，偏差集中在构象柔性区
- 最高分辨率 2.6-3.0 Å

### 功能模块化
- 三种 GEM 可同时结合于同一 D1R，互不干扰（Cryo-EM 验证）
- "AND 逻辑门"实验：BAM + ago-PAM → Gs 通路保持激活，β-arrestin 被抑制
- 证明了 GPCR 信号输出可像搭积木一样编程

### 治疗潜力
- ago-PAM 能稳定 TM6 向外移动，成功挽救多种机制导致 D1R 失活的功能丧失（LOF）突变
- 为 GPCR 遗传疾病提供了全新的治疗模式（外骨架补偿，而非传统小分子拮抗）

## 与现有工具对比

| 维度 | GEMs（本文） | Protenix-v2 |
|------|-------------|-------------|
| 设计目标 | GPCR 跨膜区变构调节蛋白 | 全长抗体/VHH binder |
| 靶点类型 | 跨膜螺旋界面（疏水平台） | 胞外结构域/GPCR 胞外区 |
| 设计方法 | RFdiffusion + MPNN + AF2 迭代 + 结构提示词 | 统一结构预测 + zero-shot |
| 功能调控 | **变构调节**（激活/抑制/偏向/逻辑门） | 结合（hit rate） |
| 实验验证 | **Cryo-EM 2.6-3.0 Å**（4 种功能态） | 体外 hit rate |
| 结构提示词 | **首创三种物理约束策略** | 无 |

## 局限与未公开
- 目前仅在 D1R 一个模型受体上验证，其他 GPCR 家族的泛化性待证明
- 设计管线依赖大量计算资源（1500 探针 + 多轮迭代重折叠）
- 跨膜区的疏水性质增加了蛋白表达和纯化难度
- 来源为微信公众号二次解读，原始论文的完整技术细节需查阅原文

## 交叉引用
- [[entities/protenix-v2]] — Protenix-v2 在 GPCR 抗体设计（VHH 16-88% hit rate）与本工作的 GPCR 跨膜区蛋白设计互补
- [[de-novo-protein-design]] — 从头蛋白设计方法学全景，GEMs 为跨膜蛋白设计的前沿案例
- [[entities/isodde]] — 蛋白-蛋白/抗体-抗原相互作用预测，可为 GEMs 的扩展设计提供预测支撑
- [[entities/alphafold3]] — AF2 在此工作中作为核心过滤工具，AF3 的改进或能进一步提升迭代管线精度
