---
title: ProtDBench — Protein Binder Design Unified Benchmark
created: 2026-05-14
updated: 2026-05-14
type: entity
tags: [protein-design, binder, benchmark, evaluation, protdbench, ai-drug-discovery]
domain: ai-drug-discovery
sources:
  - raw/articles/ProtDBench-蛋白Binder设计统一评测基准-arXiv2026.md
  - raw/papers/liu2026-protdbench-arxiv.md
confidence: high
---

# ProtDBench

> 来源: Liu C, Ren M, Guan J, Gong C, Sun J, Chen X, Xiao W. ProtDBench: A Unified Benchmark of Protein Binder Design and Evaluation. arXiv:2605.04118. 2026. | BioAIDesign 中文解读

ProtDBench 是 protein binder design 领域首个**统一评测基准**，核心贡献不是选出一个"第一名"，而是揭示了长期被忽视的问题——不同论文间的 success rate 因 target/hotspot/verifier/filter threshold/算力预算不同而**无法直接横向比较**。

## 核心发现链

蛋白 binder 设计方法缺乏横向可比性 → Verifier 偏差分析（AF2/ColabFold/Boltz/Chai/ESMFold 互不重叠）→ Throughput-aware 标准化（24h×1xA100）→ Diffusion vs Hallucination 两类方法的效率-多样性权衡 → 统一评估框架公开

## 关键证据摘要

### Verifier 偏差
- 对比 AF2-IG / ColabFold / Boltz-1/2 / Chai-1 / Protenix / ESMFold 等 verifier
- 没有哪个 verifier 在所有 target 上都最优
- 不同 verifier 筛出的真阳性重叠不高，取并集后 recall 还能提高
- 结论：很多论文的"模型更强"结果可能掺杂了 verifier 偏差

### Throughput-aware 评估
- 固定 **24h × 1xA100** GPU 预算
- 纳入 backbone generation + inverse folding + verifier 全流程耗时
- 统计 24h 内的成功 backbone 数量，而非单条 success rate

### 关键结果
- **扩散模型**（RFdiffusion-3/BoltzGen/PXDesign/ODesign）整体 throughput 更高
- **Hallucination 方法**（BindCraft/BoltzDesign-1）结构多样性更高
- 困难 target（如 TNFα）对所有方法仍是挑战
- 高 success rate ≠ 高多样性——diffusion 方法易集中于少数结构模式

### 方法覆盖
- Diffusion: RFdiffusion-3, BoltzGen, Protpardelle-1c, ODesign, PXDesign
- Hallucination: BindCraft, BoltzDesign-1
- 10 个统一 target + hotspot + verifier + filtering protocol

## 与 de-novo-protein-design 的关系
ProtDBench 恰好回答了 Kosonocky et al. (COSB 2026) 综述中提出的"实验验证方法缺乏横向可比性"问题——它不是在实验室里做验证，而是用统一的 in silico 规则让方法之间的差距可见。

## 相关概念

- [[entities/diamond-deepclust]] — DeepClust 数据库的 3.35 亿聚类可为 binder 设计的 MSA 检索提供 5.5 倍于 BFD 的序列多样性
- [[entities/alphafold3]] — AF3 为 protdbench 评测的 binder 设计方法提供结构验证基准
- [[entities/boltz2]] — Boltz-2 共折叠虚拟筛选，ProtDBench 评测的 Boltz-1 后续版本
- [[entities/rxnbench]] — RxnBench 化学反应理解 MLLM 评测基准，同为 AI 领域评测基准
- [[de-novo-protein-design]] — 蛋白设计方法全景，ProtDBench 提供标准化评估
- [[ovo]] — Ovo 蛋白设计生态系统（ProtDBench 同类评估生态）
- [[mmseqs2]] — GPU 加速同源搜索（方法评估性能比较类似逻辑）
