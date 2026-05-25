---
title: "Protenix-v2 — ByteDance Seed 抗体设计+结构预测引擎"
created: 2026-05-15
updated: 2026-05-15
type: entity
tags: [protein-design, antibody, structure-prediction, drug-design, ai-drug-discovery, protenix]
domain: ai-drug-discovery
sources:
  - raw/articles/Protenix-v2-抗体设计-结构预测-药物发现.md
confidence: medium
---

> 来源: ByteDance Seed. *Protenix-v2: Broadening the Reach of Structure Prediction and Biomolecular Design*. GitHub. 2026. (微信公众号·BioAIDesign, JoeYo)

## 核心发现链

**Protenix-v2** 是字节跳动 Seed 团队发布的统一结构预测+分子设计系统。同时覆盖两类任务：biomolecular complex 结构预测（特别是抗体-抗原复合体）和 zero-shot binder 设计。核心突破在于**结构预测精度提升带来了真实的实验命中能力**——不再只算得准，而是能直接设计出可验证的 binder。

## 关键证据摘要

### 抗体-抗原结构预测
- 在三个抗体 benchmark 上超越 AF3、Boltz-1、OpenFold3-preview2、Protenix-v1
- **DockQ > 0.23** 成功率相比 Protenix-v1 提升约 **9–13 个百分点**
- 提升在高精度区间也持续存在

### 采样效率
- **5 seeds 下表现超过 Protenix-v1 在 1000 seeds 下的结果**
- 单次采样更有效，不依赖暴力枚举

### Zero-shot antibody design
- 测试靶点面板上达到 **100% target-level success rate**（每个靶点至少找到 1 个实验确认 binder）
- **VHH-Fc hit rate: 2%–48%**（novelty-controlled targets）
- 在 **16–30 个设计/靶点**的小预算下有效

### GPCR 抗体设计
- **VHH-Fc hit rate: 16%–88%**
- **mAb hit rate: 最高 50%**
- GPCR 是公认难靶点（表位小、柔性高），此结果说明模型开始触及真实药物研发场景

### Developability
| 指标 | 通过率 |
|------|--------|
| 热稳定性 | 100% |
| 自相互作用 | 97.5% |
| 多反应性 | 93.3% |

候选分子不仅是"能结合"，生物物理性质也接近可推进的分子。

## 与现有工具对比

| 维度 | Protenix-v2 | AF3 | Boltz-1 | IsoDDE |
|------|-------------|-----|---------|--------|
| 抗体-抗原结构预测 | **最优**（三 benchmark 均领先） | 基准 | 次优 | DockQ>0.8 最优（39%） |
| Zero-shot 抗体设计 | **已验证**（实验命中） | N/A | N/A | 未公开 |
| 采样效率 | 5 seeds > v1 1000 seeds | — | — | — |
| GPCR 抗体设计 | **16-88% VHH hit** | N/A | N/A | N/A |
| Developability 评估 | 有（3 项） | N/A | N/A | 未公开 |
| 开源 | **GitHub 公开** | 部分代码 | 完全开源 | 闭源技术报告 |
| 蛋白-小分子预测 | N/A | 有 | N/A | **最优**（50% vs AF3 23%） |

> Protenix-v2 和 IsoDDE 存在互补关系：前者侧重抗体设计和验证 pipeline，后者侧重蛋白-小分子+亲和力预测。

## 交叉引用
|- [[isodde]] — IsoDDE 统一预测引擎，蛋白-小分子/亲和力预测更强
|- [[entities/alphafold3]] — AF3 全生物分子统一预测框架，与 Protenix-v2 同为结构预测工具
|- [[de-novo-protein-design]] — 蛋白设计方法学背景
- [[protdbench]] — binder 设计统一评测基准，Protenix-v2 或可纳入评估
- [[nextgen-vaccine-strategies]] — 抗体设计可用于疫苗策略

## 交叉引用

- [[entities/gps-ai-drug-platform]] — GPS 的转录组预测可补充验证 Protenix-v2 设计抗体的靶点通路实际影响
- [[entities/gem-gpcr-modulator]] — GEMs 从头设计 GPCR 跨膜区变构调节蛋白，与 Protenix-v2 的 GPCR 抗体设计互补
- [[entities/dual-gpt-ab]] — DualGPT-AB 双阶段 GPT+RL 抗体设计框架，多性质约束内置，可与 Protenix-v2 zero-shot 策略对比
- [[entities/pplm]] — PPLM 蛋白配对语言模型，提供基于序列的 PPI 表示，与 Protenix-v2 结构预测互补

## 局限与未公开
- 来源为微信公众号文章（二次解读），非原始技术报告，部分数据可能不完整
- 精确 benchmark 数据集/实验协议未完全披露
- GPCR 测试的靶点数目和多样性未说明
- 与 IsoDDE 的直接对比未在原文中提供（上表为交叉分析）
