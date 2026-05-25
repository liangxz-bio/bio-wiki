---
title: "DIAMOND DeepClust — 行星级蛋白质超快速聚类工具"
created: 2026-05-16
updated: 2026-05-16
type: entity
tags: [protein-clustering, sequence-search, homology-search, nature-methods, bioinfo-method]
domain: bioinfo-pipeline
sources:
  - raw/articles/DIAMOND-DeepClust-蛋白质宇宙聚类-NatureMethods-另一视角.md
  - raw/articles/DIAMOND-DeepClust-蛋白质宇宙超快速聚类-NatureMethods2026.md
confidence: high
---

# DIAMOND DeepClust

## 核心发现链

DIAMOND DeepClust 是基于 DIAMOND v.2 的级联超快速蛋白质聚类方法，首次实现**行星级蛋白质序列空间**的高效深度聚类。在 27 个计算节点、18 天（25 万 CPU 小时）内将 190 亿条生物圈蛋白质序列聚类为 17 亿个簇（5.44 亿个非单例簇覆盖 94% 的序列），获得 3.35 亿个≥3 成员的簇——是此前最大深度聚类数据库 BFD（Big Fantastic Database，6100 万簇）的 **5.5 倍**，新增 1.18 亿个全新蛋白质家族。集成至 AlphaFold2 后，低序列覆盖度蛋白的 pLDDT 评分平均提升 7.73 分。

## 关键证据摘要

- **速度突破：** 对 NCBI NR（5.46 亿序列）深度聚类仅需 **19 小时（64 核单服务器）**，比 MMseqs2（29 天）快 36x，比 FLSHclust（17 天）快 21x
- **灵敏度领先：** 灵敏度 68.6%，精度 95.5%（Pfam 域架构基准），显著优于 MMseqs2（62.3%）和 FLSHclust（49.7%）
- **线性模式：** 3.9 小时完成 NR 深度聚类，比 FLSHclust 快 102 倍，灵敏度 53.8% vs FLSHclust 49.7% ^[raw/articles/DIAMOND-DeepClust-蛋白质宇宙超快速聚类-NatureMethods2026.md]
- **双向覆盖策略：** 相比单向覆盖，灵敏度从 52.3% 提升至 68.6%，精度从 64.9% 提升至 95.5%
- **大规模验证：** 190 亿序列 → 17 亿簇（5.44 亿非单例），3.35 亿个≥3 成员的簇，92% 序列可压缩为代表序列
- **AF2 结构预测增强：** 473 个低 MSA 深度序列的 pLDDT 平均提升 7.73 分，366 个序列预测精度显著提高 ^[raw/articles/DIAMOND-DeepClust-蛋白质宇宙聚类-NatureMethods-另一视角.md]

## 与现有工具对比

| 维度 | DIAMOND DeepClust | MMseqs2 | FLSHclust | CD-HIT / UClust |
|------|-------------------|---------|-----------|-----------------|
| NR 聚类时间（64核） | 19 小时 | 29 天 | 17 天 | 不适用（无法扩展到此规模） |
| 灵敏度（Pfam 基准） | 68.6% | 62.3% | 49.7% | N/A |
| 精度 | 95.5% | 95.5% | 96.5% | N/A |
| 线性模式时间 | 3.9 小时 | 2.8 小时（Linclust，灵敏度仅 21.6%） | N/A | N/A |
| 可扩展性 | 万亿级（多节点/云） | 40 亿（单节点） | 数十亿 | 远低于 10 亿 |
| 双向覆盖 | 是 | 是 | 否（贪婪方法） | 否 |
| 级联聚类（多轮） | 是（4 轮：faster→fast→default→sensitive） | 否 | 否 | 否 |

## 交叉引用

- [[entities/mmseqs2]] — DIAMOND DeepClust 的主要对比基准工具，MMseqs2 在同规模聚类中慢 36 倍
- [[entities/de-novo-protein-design]] — DIAMOND DeepClust 构建的 DeepClust 数据库可增强 AlphaFold2 结构预测，推动从头蛋白质设计
- [[entities/crocodeel]] — 同为生物信息学方法工具，应用于宏基因组领域
- [[entities/protdbench]] — 蛋白 binder 设计评测基准，受益于 DeepClust 数据库的序列多样性扩充
- [[raw/articles/AI筛药新工具-Boltz2-SoftMol-AINN-P1-DeepClust]] — AI筛药四工具综述，含DeepClust/Boltz-2/SoftMol/AINN-P1

## 局限与未公开

- 需要 DIAMOND v.2 环境依赖，未提供独立 conda/container 包
- 线性模式虽快但灵敏度显著低于全模式（53.8% vs 68.6%）
- 标记优化（MCL 整合）尚在规划中，远同源检测仍有提升空间
- 直系同源/旁系同源区分功能尚未实现
