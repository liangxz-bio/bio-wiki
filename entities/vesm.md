---
title: VESM — co-distillation 压缩 ESM 家族集体知识的单序列变异效应预测模型
created: 2026-08-27
updated: 2026-08-27
type: entity
tags: [model, variant-effect-prediction, protein-language-model, distillation, esm, clinvar, proteingym]
domain: ai-drug-discovery
sources: [raw/articles/NatureMethods-VESM-ESM家族互相教学-变异效应预测.md]
confidence: high
---

# VESM — ESM 家族 co-distillation 变异效应预测模型

Dinh T, Jang S-K, Zaitlen N, Ntranos V (UCSF), 2026, *Nature Methods* 23: 772–784.

从多个 ESM 模型中提取互补进化约束信号，通过三轮 co-distillation 压缩为单一高性能 PLM，在 ClinVar、ProteinGym DMS 和 UK Biobank 表型关联中均达到 SOTA 水平。

## 核心方法

**问题**：单序列 PLM（如 ESM）VEP 性能通常落后于融合 MSA/结构/群体遗传信息的混合模型。但同一家族内不同 ESM 模型的预测盲区存在系统性互补。

**解法**：不增加新模态，仅重新组织 ESM 预训练已学得的序列知识。

### 三阶段流程

| 阶段 | 教师信号 | 训练蛋白 | 可训练参数 |
|---|---|---|---|
| 第 1 轮 | ESMIN（11 模型逐变异最小 LLR） | 18,683 人类蛋白（排除 >30% 同源） | 最后 hidden layer + LM head |
| 第 2 轮 | 4 模型 LLR 平均值 | 同左 | 同左 |
| 第 3 轮 | 4 模型 LLR 平均值 | +23,803 非人类蛋白 | model head + 最后 3 个 embedding layers |

最终模型：**VESM-3B**（ESM2-3B backbone）

### 后续蒸馏

- VESM-3B → 蒸馏至 ESM2-650M/150M/35M backbone → 小型 VESM 系列
- VESM-3B 序列知识 → 蒸馏进 ESM3 sequence module → **VESM3**（推理时恢复结构输入）
- VESM3 + VESM-3B LLR 平均 → **VESM++**（结构感知 ensemble）

## 关键性能

| Benchmark | VESM-3B | 最佳对比 |
|---|---|---|
| Balanced ClinVar AUC | **0.938** | 最高（含 MSA/结构/人群频率方法） |
| ProteinGym DMS Spearman ρ | 与 ensemble 持平 | — |
| ClinVar 90% 准确率覆盖率 | **90%** | ESM1b 79%, SaProt 87% |
| 小型化保留率 | 650M: >98% ClinVar / >93% DMS | 35M 相对提升 >20%/70% |
| UK Biobank Genebass β 关联 | VESM++ 最高 | 优于 AlphaMissense、等位基因频率基线 |
| 效应方向一致性 | 167/169 (98.8%) 与 pLoF burden 一致 | Pearson ρ = 0.883 |

## 核心洞察

1. **逐变异选教师**：不同 ESM 在不同位点/氨基酸替换上提供最强监督，不是固定全局教师
2. **minimum → average 切换**：第一轮最小 LLR（方差结构利于分离），后两轮平均（类别间离散度趋近）
3. **数据效率**：仅 1% 训练蛋白（~200 条）即达完整训练 97% 性能
4. **通用表征无损**：co-distillation 后下游蛋白表征任务无一致性下降
5. **跨物种外推**：人类蛋白训练 → 病毒蛋白 DMS assay 显著提升

## 局限

- 监督信号仅来自 ESM 家族自身，无法引入家族外新约束
- minimum aggregation 依赖特定方差结构，不可直接泛化
- 仍以 benchmark 为主，缺少罕见病临床队列验证
- ClinVar 比较无法完全消除数据循环

## 资源

- Code: https://github.com/ntranoslab/vesm
- DOI: 10.1038/s41592-026-03050-9
- 解读: [[Nature-Methods-IF-28-3-谁说单序列-PLM-到头了-VESM-让-ESM-家族互相教学-性能大幅跃升]]

## 相关页面

- [[esm-world-model]] — ESM World Model（Biohub/CZI）
- [[seqdance-esmdance]] — SeqDance/ESMDance 基于 MD 动力学的 pLM 突变效应预测
- [[raw/articles/NatureMethods-VESM-ESM家族互相教学-变异效应预测]] — 全文解读（Bio暗物质雷达）
- [[raw/articles/SeqDance-ESMDance-蛋白质动力学语言模型-PNAS2026-biomath]] — 同类 VEP 工作
