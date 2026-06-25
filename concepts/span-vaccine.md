---
title: S_pan 广谱新冠疫苗 — 基于 S 蛋白进化轨迹的抗原设计
created: 2026-05-21
updated: 2026-05-21
type: concept
tags: [vaccine, sars-cov-2, universal-vaccine, antigen-design, viral-evolution, covid-19]
domain: vaccine-design
sources: [raw/papers/xu2023-span-scitranslmed.md]
confidence: high
---

# S_pan 广谱新冠疫苗

> 来源: Zhao Y et al. Vaccination with S_pan, an antigen guided by SARS-CoV-2 S protein evolution, protects against challenge with viral variants in mice. *Sci Transl Med* 2023;15(710):eabo3332. doi:10.1126/scitranslmed.abo3332

## 核心发现链

徐可/蓝柯团队（武汉大学）提出一种**基于病毒进化轨迹预测的通用疫苗抗原设计策略** S_pan。核心逻辑：SARS-CoV-2 S 蛋白的进化并非随机，而是沿两条互斥路径进行——**高感染力 + 低免疫逃逸** 或 **低感染力 + 高免疫逃逸**。基于此发现，团队通过系统发育分析 2675 条 S 蛋白序列（Delta 变异株出现前），计算各进化支的高频共有突变，设计出广谱抗原 S_pan。在小鼠模型中，S_pan 疫苗对 WT / Beta / Delta / Omicron (BA.1-BA.5) 等跨进化支变异株均提供显著优于原型 S_wt 疫苗的保护，其中异源 S_pan 加强针实现 Omicron BA.1 100% 保护。^[raw/papers/xu2023-span-scitranslmed.md]

## SARS-CoV-2 S 蛋白进化规律

### 54 株假病毒系统分析

通过对 54 株 SARS-CoV-2 假病毒（覆盖 45 个单点突变 + 8 个变异株）在 4 种细胞系（BHK21-hACE2 / Vero E6 / Caco-2 / A549-hACE2）中的感染力和 14 株单克隆抗体中和逃逸能力的系统检测，揭示 S 蛋白进化规律：^[raw/papers/xu2023-span-scitranslmed.md]

- **感染力增强突变簇**: del157 (5.70x), S982A (3.88x), del157-158, H655Y, R246I, Q52R, L452R — 主要集中在 NTD 和 S2 亚基的 HR1 区
- **免疫逃逸突变簇**: E484K (2.65x 逃逸), N501Y (1.47x) — 主要集中在 RBD
- **极少突变同时增强两者**: 仅 Beta 和 Kappa 变异株位于对角线（高感染力+高免疫逃逸）
- **Delta 变异株**: 感染力最强（4.24x），但免疫逃逸能力弱
- **Omicron 变异株**: 免疫逃逸最强（3.32x），但仅 Vero E6 细胞高感染力，提示偏好内吞途径

### 互斥进化路径

基于 11,650,487 条 GISAID S 蛋白序列的综合分析显示，S 蛋白沿三条路径进化：

1. **垂直轴**（高感染力 + 低逃逸）：Delta, Lambda
2. **水平轴**（低感染力 + 高逃逸）：Gamma, Eta, Omicron
3. **对角线**（高感染力 + 高逃逸）：Beta, Kappa（但株系数量有限）

此互斥性意味着单毒株疫苗只能保护同一路径的变异株，无法覆盖跨路径变异。^[raw/papers/xu2023-span-scitranslmed.md]

## S_pan 抗原设计策略

### 设计流程

1. 从 NCBI 下载 2675 条 S 蛋白氨基酸序列（截至 2021-02-28，Delta 变异株出现前）
2. MEGA 10.0 构建系统发育树 → 分为 5 个进化支
3. 支 1（2539 条序列，占 95%）计算共有序列
4. 支 2-5（136 条序列）加入计算高频突变频率
5. 确定 5 个最高频突变: **D614G, del69-70, del144, N501Y, P681H**
6. 结合中和实验结果（E484K 是最关键免疫逃逸位点）→ 加入 **E484K**
7. 最终 S_pan 序列 = 野生型骨架 + 这 6 个突变

> 值得注意的是，额外 9 个高频突变的 S_15 抗原并未比 S_pan 诱导更强的广谱中和活性，说明疫苗抗原设计不是突变的简单叠加，需要严谨的生物信息学计算与实验证据联合分析。^[raw/papers/xu2023-span-scitranslmed.md]

## 验证数据

### 血清中和活性（假病毒系统）

| 变异株 | S_wt 疫苗 GMT | S_pan 疫苗 GMT | S_pan 优势 |
|--------|--------------|---------------|-----------|
| WT | 3013 | 863 | 基线略低 |
| Beta | 170 | 1149 | **7x 提升** |
| Delta | 878 | 830 | 可比 |
| Omicron BA.1 | — | 925 (异源加强) | S_wt 同源 <100 |
| Omicron BA.2 | — | 630 | 同上 |
| Omicron BA.2.12.1 | — | 2001 | 同上 |
| Omicron BA.3 | — | 1798 | 同上 |
| Omicron BA.4/5 | — | 931 | 同上 |

### 活病毒攻毒保护（K18-hACE2 小鼠）

- **WT 攻毒**: S_pan 80% vs S_wt ~100% 保护
- **Beta 攻毒**: S_pan **60%** vs S_wt **0%** (P < 0.05)
- **Delta 攻毒**: S_pan 100% vs S_wt 100%
- **Omicron BA.1 攻毒（异源加强）**: S_pan **100%** vs S_wt 同源 **50%**

S_pan 异源加强组肺组织病毒 RNA 载量比安慰剂组降低 **2846 倍**，而 S_wt 同源加强组在死亡个体中仍有高载量病毒。^[raw/papers/xu2023-span-scitranslmed.md]

### 对未覆盖突变的广谱性

S_pan 血清对设计中未包含的突变（E484Q, T478K, F490S）也表现出优于 S_wt 的中和活性，提示 S_pan 可能暴露了 S 蛋白上的保守表位。^[raw/papers/xu2023-span-scitranslmed.md]

## 与传统疫苗设计方法对比

| 方面 | 传统单毒株 (S_wt) | S_pan 策略 |
|------|-----------------|-----------|
| 抗原设计原则 | 原始序列 | 进化轨迹高频共有突变 |
| 对 Beta 变异株 | 0% 保护 | 60% 保护 |
| 对 Omicron (异源加强) | 50% 保护 | 100% 保护 |
| 对新变异株预测性 | 无 | 基于进化历史的部分预测能力 |
| 设计时间点 | 疫情初期 | Delta 出现之前 |
| 局限性 | 无法跨进化支 | 依赖序列数据质量和进化分析准确性 |

## 局限性

- **仅在小鼠模型验证**: 样本量有限，需更多动物模型和临床试验验证^[raw/papers/xu2023-span-scitranslmed.md]
- **免疫持久性未评估**: 特别对异源加强方案的长期保护力^[raw/papers/xu2023-span-scitranslmed.md]
- **抗原设计基于 Delta 出现前的数据**: 后续 Omicron 亚型的变异积累可能超出 S_pan 覆盖范围^[raw/papers/xu2023-span-scitranslmed.md]
- **S_pan 设计依赖系统发育树的共有序列计算**: 若病毒进化出现全新跳跃（非渐进式突变），此策略可能失效

## 交叉引用

- [[nextgen-vaccine-strategies]] — 结构抗原工程是新一代疫苗设计的四大策略之一，S_pan 是结构指导抗原设计的具体案例
- [[universal-intranasal-vaccine-gla-3m052]] — 另一种广谱疫苗策略（非抗原特异性），通过整合器官免疫实现广谱防护
- [[as-neoantigen-hcc-vaccine]] — 另一类通用型疫苗策略（mRNA 新抗原），与 S_pan 的蛋白亚单位策略形成对比
