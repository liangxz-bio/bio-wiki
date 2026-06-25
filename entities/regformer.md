---
title: RegFormer
created: 2026-05-07
updated: 2026-05-07
type: entity
tags: [model, foundation-model, single-cell, ai-drug-discovery]
domain: ai-drug-discovery
sources: [raw/articles/RegFormer-单细胞大模型Mamba架构-NatCommun2026.md]
confidence: medium
---

# RegFormer — 单细胞 Mamba 基础模型

> 华大生命科学研究院开发，融合基因调控层级先验的 Mamba 架构单细胞基础模型。

## 概述

RegFormer 是首个将 **Mamba 状态空间模型** 与 **基因调控网络先验** 结合的单细胞基础模型，发表于 *Nature Communications* 2026 年。核心创新在于用调控层级拓扑排序替代传统的随机基因排列，并通过双重嵌入机制保留表达值与调控身份信息。

## 技术架构

- **拓扑排序输入**：基于图论的有向无环图拓扑排序，基因输入序列严格遵循转录级联方向（上游转录因子 → 下游靶基因），而非随机排列
- **双重嵌入（Dual-embedding）**：每个基因同时编码「定量表达值」与「调控身份标识」（TF / co-factor / target）
- **Mamba SSM 编码器**：线性时间复杂度的状态空间模块替代 Transformer 注意力机制，突破全基因组级长序列建模的计算瓶颈

## 关键能力

| 任务 | 方法 | 亮点 |
|------|------|------|
| 跨组织细胞聚类 | 调控拓扑 + Mamba 提取特征向量 | 区分罕见亚型、消除批次效应，超越 scGPT 等主流模型 |
| 调控网络重建 | 预训练基因嵌入 → 多维相似度反向推导 | 高保真捕捉 GATA3 / SPI1 等主效 TF → 靶基因层级 |
| 基因扰动推演 | 联合 GEARS 推断框架 | 扰动 TF 与差异表达靶基因在潜空间呈紧密几何距离 |
| 药物敏感性预测 | 细胞嵌入 + 药物分子 GCN 多模态融合 | IC50 预测 SOTA，可溯源到 NF-κB / p53 信号通路 |

## 与已有 wiki 条目的关联

- [[scrnaseq-workflow]] — RegFormer 可嵌入单细胞分析流程的前端编码环节
- [[fastsc]] — fastSC 提供传统 scRNA-seq 分析管线，RegFormer 代表端到端预训练路线的补充
- [[scnotebooks]] — scNotebooks 培训平台可覆盖 RegFormer 的方法学教学

## 局限

- 当前为单模态（仅转录组），未见多模态扩展方案
- Mamba 在超长序列（>100K 基因）上的实际推理效率尚需大规模基准验证
- 药物敏感性预测依赖分子 GCN，跨模态融合仍处于早期集成阶段
