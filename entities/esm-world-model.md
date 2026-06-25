---
title: ESM World Model
created: 2026-05-29
updated: 2026-05-29
type: entity
domain: ai-drug-discovery
tags: [biohub, czif, esm, esmfold2, esm-atlas, protein-world-model, protein-design, protein-folding, protein-language-model, open-source, meta-fair]
sources:
  - raw/articles/ESM-World-Model-Biohub-Zuckerberg-智药局.md
confidence: medium
---

# ESM World Model

> Biohub（CZI/陈-扎克伯格倡议）发布的开源蛋白质世界模型系统，由 ESMC（语言模型）、ESMFold2（折叠模型）、ESM Atlas（结构图谱）三大组件构成。

## 组件架构

### ESMC — 蛋白质语言模型
- 参数规模：300M / 600M / 6B 三档
- 训练数据：~28 亿序列（含宏基因组扩展，较 ESM2 的 5,000 万提升 56x）
- 定位：系统的基础编码器，对标 ESM2 的升级版

### ESMFold2 — 蛋白质折叠预测
- 速度显著快于 AlphaFold3 及其他现有折叠模型
- 准确性：保持 SOTA 水平，在蛋白复合物预测（含抗体-抗原）上略超 AlphaFold3
- 前代基础：ESMFold（2022，Meta-FAIR，150 亿参数，比 AF2 快一个数量级）

### ESM Atlas — 蛋白质结构与功能图谱
- 规模：68 亿蛋白质，11 亿预测结构
- 对比：比 AlphaFold 数据库多约 8 亿条目
- 能力：生命尺度上的蛋白质分析与发现

## 实测表现
| 靶点 | 蛋白命中率 | 抗体模式命中率 |
|------|-----------|---------------|
| EGFR | 36–88% | 15–29% |
| PDGFRβ | 同上 | 同上 |
| PD-L1 | 同上 | 同上 |
| CTLA-4 | 同上 | 同上 |
| CD45 | 同上 | 同上 |

## 与同类方法对比

| 维度 | ESM World Model | AlphaFold3 | RFdiffusion |
|------|----------------|------------|-------------|
| 开源 | ✅ 完全开源 | ❌ 闭源 | ✅ 开源 |
| 蛋白复合物（含抗体-抗原） | 略胜 AF3 | — | — |
| 从头蛋白设计 | 含设计模块 | ❌ 仅预测 | ✅ 核心能力 |
| 规模 | 68 亿蛋白图谱 | ~2 亿结构（AFDB） | 无独立数据库 |
| 可解释性工具 | ✅ 含 | ❌ | ❌ |

## 开发背景
- 团队前身：Meta-FAIR（Meta AI Research）蛋白质小组，2022 年推出 ESMFold
- 现归属：Biohub（非营利），由 CZI（陈-扎克伯格倡议）资助
- CZI 愿景：本世纪治愈人类所有疾病
- 2026.04：$500M / 5 年计划，构建生命预测模型技术栈
- 相关项目：rBio 虚拟细胞推理模型、十亿细胞项目（Billion Cells Project）

## 交叉引用
- [[esmfold2]] — ESMFold2 蛋白质折叠模型（如有专页）
- [[de-novo-protein-design]] — 从头蛋白设计综述，RFdiffusion/ProteinMPNN 工具上下文
- [[alphafold3]] — AlphaFold 3 全生物分子统一结构预测
- [[ori-protein-design]] — ORI 蛋白设计闭环，不同技术路线
- [[seqdance-esmdance]] — ESMDance 蛋白动力学 pLM，ESM2 替代视角
