---
title: "TCR-PRP — 多肽识别谱与T细胞功能预测系统"
created: 2026-05-18
updated: 2026-05-18
type: entity
tags: [immunology, TCR, epitope, t-cell, protein-language-model, autoantigen, vaccine-design]
domain: vaccine-design
sources:
  - raw/articles/TCR-PRP-多肽识别谱-语言模型-T细胞功能预测-NatBiotechnol2026.md
confidence: medium
---

> 来源: *Deep peptide recognition profiling decodes TCR specificity and enables disease-associated antigen discovery*. Nature Biotechnology. 2026. (微信公众号「游离的DNA」中文解读)

## 核心发现链

**TCR-PRP** 系统将高通量酵母表面展示技术与微调蛋白质语言模型（pLM）深度整合，通过构建 T 细胞受体的"多肽识别谱"（Peptide Recognition Profiles, PRPs），实现从序列到 T 细胞激活功能的高精度预测。核心突破在于证明：**TCR 的功能相关性隐藏在多肽识别模式中，而非传统的 CDR3β 序列相似性**。系统成功从人类蛋白质组中发现 HLA-B\*27:05 相关自身免疫疾病（强直性脊柱炎 AS / 急性前葡萄膜炎 AAU）的新抗原（PRPF3、PSG5）。

## 关键证据摘要

### 高通量 PRP 构建
- 从 AS/AAU 患者分离 **16 个致病性 TCR**（HLA-B\*27:05 限制），CDR3β 环主导多肽结合
- 每个 TCR 面对 **~100 万随机 9-mer 多肽库**（酵母展示）
- 各 TCR 识别的多肽数量差异巨大：**数百种到 >6,000 种**

### 序列 vs 功能聚类
- CDR3β 编辑距离（edit distance）：序列差异大的 TCR 看似无关
- **Jensen-Shannon 散度**比较 PRP：序列无关的 TCR 在多肽识别空间紧密聚集
- 结论：序列相似 ≠ 功能相似；PRP 揭示的识别模式才是功能相关的真正尺度

### 语言模型 vs 结构预测
- 基于 PRP 数据微调 pLM，注意力集中于 CDR3β（引入 α 链不提升性能）
- **中位数 AUPRC 0.85-0.86**
- **全面超越 AlphaFold3 和 tFold-TCR**（ipTM 评分）在预测 T 细胞激活（CD69 上调）中的表现
- 原因：静态结构预测只能给出能量最低"快照"，语言模型捕获了结合动力学和上下文依赖的序列模式

### 自身抗原发现
- 筛选人类蛋白质组 >20 万种 HLA-B\*27:05 呈递 9-mer
- 排名前 15 多肽验证：**PRPF3 和 PSG5** 激活绝大多数疾病相关 TCR，部分超强于已知细菌表位
- NetMHCpan 预测的 20 个最高亲和力多肽 **无一引起任何 TCR 激活**
  → MHC 亲和力 ≠ 免疫病理反应；TCR 层面的 PRP 特异性跨越才是关键
- **PSG5** 在虹膜色素上皮细胞表达（AAU 炎症部位），患者外周血 PSG5 特异性 CD8+ T 细胞显著升高

### 不确定性量化
- 马氏距离（Mahalanobis distance）在 TCR-多肽联合嵌入空间中衡量新 TCR 偏离训练分布的程度
- 马氏距离与实验 PRP 散度显著统计相关
- 小马氏距离 → 预测与实验真实值几乎完全重合
- 大马氏距离 → 预测偏差明显增加
- 意义：模型具备量化"自我不确定性"的能力，可指导实验资源优先投入功能独特的 TCR

## 局限与未公开

- 当前仅验证于 HLA-B\*27:05 限制性 TCR 家族（CDR3β 主导的对接几何），其他 HLA 限制性和 α 链主导的 TCR 尚待验证
- 酵母展示多肽库为随机 9-mer，可能遗漏天然加工呈递中更长的多肽
- 来源为微信公众号二次解读，原始论文具体技术细节需查阅原文
- 自身抗原的体内致病性仍需独立队列和动物模型验证

## 交叉引用

- [[entities/alphafold3]] — AF3 在本文中被作为结构预测基线对比，表现不如 PRP-pLM
- [[de-novo-protein-design]] — 蛋白质语言模型（pLM）为蛋白设计提供基础架构
- [[entities/protenix-v2]] — 同为结构预测工具，与 AF3/tFold 并列对比
- [[concepts/nextgen-vaccine-strategies]] — T 细胞表位预测与自身抗原发现是疫苗设计的关键上游
- [[entities/isodde]] — 另一类免疫分子相互作用预测系统
- [[entities/pplm]] — PPLM 蛋白配对语言模型，与 TCR-PRP 共享 pLM 基础，但聚焦 PPI 而非 TCR-多肽识别
