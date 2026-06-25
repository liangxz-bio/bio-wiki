---
title: MMseqs2
created: 2026-05-12
updated: 2026-05-12
type: entity
tags: [tool, homology-search, gpu, sequence-alignment, bioinfo-method]
domain: bioinfo-pipeline
sources: [raw/articles/MMseqs2-GPU同源搜索-NatureMethods2025.md]
confidence: high
---

# MMseqs2

## 核心发现链

MMseqs2-GPU 将蛋白质同源搜索的核心计算步骤（无空位 Gapless filter → 有空位 Smith-Waterman alignment）从 CPU 移植到 GPU，利用共享内存 + warp shuffles + 16-bit packing 最大化吞吐量。单 L40S GPU 比 128 核 CPU 快 6x（单查询），ColabFold 端到端加速 31.8x，MSA 生成占比从 83% 降至 14.7%。同时加速 Foldseek 结构比对 4-27 倍。

## 关键证据摘要

- **速度提升：** 单 L40S GPU 比 JackHMMER 快 177x，比 BLAST 快 6.4x；8xL40S 比 128 核 MMseqs2-CPU 快 2.4x（批量 6,370 查询）
- **ColabFold 加速：** 端到端 31.8x，MSA 生成时间从数十分钟降至秒级；预测 TM-score 与标准流程持平（0.70 ± 0.05）
- **数据库流式处理：** 处理超显存数据库（3,000 万序列的 16x 大小），性能降低仅至内存速度的 63-65%
- **成本效率：** AWS 单 L40S 方案为所有批处理规模下成本最低方案；能效比 JackHMMER 高 ~80x
- **普惠性：** 低至每残基 1 字节内存占用，可在 Google Colab 免费/Pro（L4 GPU）运行
- **Foldseek 协同加速：** 相同架构加速 Foldseek 4-27 倍 ^[raw/articles/MMseqs2-GPU同源搜索-NatureMethods2025.md]

## 局限性与注意事项

- 需要 NVIDIA GPU（CUDA），传统 CPU 集群需硬件升级
- 不同于 k-mer 方法，无法通过牺牲灵敏度换取极速
- GPU 初始化开销 ~300ms，频繁短任务可通过持久化服务器模式缓解

|- [[diamond-deepclust]] — DIAMOND DeepClust 在 NR 聚类中比 MMseqs2 快 36 倍，同为蛋白质序列搜索/聚类工具

## 来源与交叉引用

- [[raw/articles/MMseqs2-GPU同源搜索-NatureMethods2025.md]] — 微信公众号·MetagenomeBioin，Ethan Ethan，基于 Nature Methods 2025 原文总结
- [[nextgen-vaccine-strategies]] — MMseqs2-GPU 加速 MSA 构建，为结构疫苗设计中的 AlphaFold 流程降本增效
- [[crocodeel]] — 同为生物信息学工具，应用于宏基因组领域
