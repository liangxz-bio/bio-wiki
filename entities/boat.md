---
title: "BOAT — 抗体多目标贝叶斯优化框架"
created: 2026-05-18
updated: 2026-05-18
type: entity
tags: [antibody, bayesian-optimization, multi-objective, developability, AI-drug-discovery]
domain: ai-drug-discovery
sources:
  - raw/articles/AI造药新范式-口袋生长分子-BOAT-ProteomeScan-SeFMol-TarPass.md
confidence: medium
---

> 来源: AstraZeneca. *BOAT: Navigating the Sea of In Silico Predictors for Antibody Design via Multi-Objective Bayesian Optimization*. arXiv:2404.13980. GitHub: github.com/AstraZeneca/boat.

## 核心发现链

BOAT 是阿斯利康开发的抗体多目标贝叶斯优化框架，用高斯过程代理模型替代传统"串联筛选"范式，在亲和力、可开发性、免疫原性等多个冲突目标间同步搜索帕累托最优解。核心创新在于用采集函数平衡探索与利用，并用遗传算法解决离散序列空间的优化问题。

## 关键证据摘要

- **高斯过程代理模型**：从少量初始序列出发，学习各属性（亲和力、溶解度等）的预测函数及其不确定性
- **采集函数**：平衡"去已知好区域"（exploitation）和"去未知区域"（exploration）
- **离散序列处理**：用遗传算法在合法氨基酸空间中搜索，支持按位点指定突变字典
- **ESM-2 自然度正则化**：加入蛋白语言模型打分约束搜索空间，防止模型利用预测工具的漏洞产生离群序列
- **性能**：在相同计算预算（1000 次 oracle 调用）下，比传统遗传算法（NSGA-II）更快收敛到更优解集；3-4 目标时传统方法显著下降，BOAT 保持稳健
- **关键限制**：高度依赖预测模型（oracle）质量——oracle 有漏洞时 BOAT 会无情利用

## 交叉引用
- [[entities/dual-gpt-ab]] — DualGPT-AB 做抗体序列生成（多性质约束前置），BOAT 做已有序列的多目标优化（后置搜索），互补
- [[entities/protenix-v2]] — 抗体设计工具全家桶
- [[de-novo-protein-design]] — 蛋白设计方法学背景
- [[sefmoll]] — SeFMol RL+扩散半柔性分子生成，同为 RL 驱动的分子优化框架
