---
title: CNVPipe — 基于机器学习的WGS拷贝数变异检测流程 (Snakemake)
created: 2026-05-11
updated: 2026-05-11
type: entity
tags: [bioinformatics, cnv, wgs, snakemake, pipeline, svm, machine-learning, single-cell]
sources: [raw/articles/CNVPipe-基于机器学习的WGS拷贝数变异检测流程-bioRxiv2025.md]
confidence: high
---

# CNVPipe — WGS CNV 分析流程

CNVPipe 是由 Sun J 等（bioRxiv 2025）开发的基于 Snakemake 的 WGS 拷贝数变异检测流程，整合 5 种 CNV 检测工具 + SVM 机器学习过滤 + 单细胞 CNV 模块。代码：<https://github.com/sunjh22/CNVPipe>

## 核心设计

### 深度感知的合并策略（核心创新）

| 测序深度 | 优先方法 | 逻辑 |
|----------|----------|------|
| ≤5× | 读深方法（CNVKit / CNVpytor / cn.MOPS） | 低深度下读深信号更可靠 |
| >5× | 分裂读/读对方法（Delly / Smoove） | 高深度下精确映射小事件 |

合并规则：CNV 相互重叠 > 0.75 时递归合并，合并后以读深法估计拷贝数。

### SVM 过滤

5 个特征输入 SVM 分类器：相邻读深模式、SNP 等位基因频率、重复/低复杂度区域重叠、CNV 大小、GC 含量。可提供参考集或模拟数据训练。

### 单细胞 CNV 模块

倍性估计创新：最小二乘舍入 + 峰值逼近策略取代传统"所有细胞二倍体"假设，可准确推断单倍体/非整倍体细胞。

## 性能关键数据

- 真实数据 FDR < 0.35（对照 > 0.85 的 ClinSV）
- iPSC ddPCR 验证真阳性率：CNVPipe-SVM 65.1%/61.3% vs sv-callers 42.3%/40.9%
- CRISPR 脱靶评估真阳性率 96.4%/97.6%
- 150 例儿童发育障碍队列：识别 16 个神经发育基因相关 CNV + 8 个免疫调节因子复发性 CNV

## 局限与待改进

- 低深度（≤0.5×）检测率仍低
- 未整合长读长数据（PacBio/ONT）
- 使用传统 SVM 而非深度学习

## 交叉引用

- [[wisecondorx]] — 另一种 CNV 检测工具（sWGS，within-sample PCA 归一化），侧重 NIPT/小片段 CNV
- [[fastsc]] — 单细胞分析 R 包，含 inferCNV 接口
- [[scmbench]] — 单细胞多组学基准测试，含 CNV 方法对比
- [[svm]] — 支持向量机分类器（CNVPipe SVM 过滤模块的核心算法）

## 来源

- Sun J, Wong NK, Jiang Z, et al. CNVPipe: An enhanced pipeline for accurate analysis of copy number variation from whole-genome sequencing. bioRxiv 2025. doi:10.1101/2025.05.18.654763
- 微信公众号「我爱学生信」解读文

## 交叉引用
- [[concepts/cfdna-fragmentomics-mced]] — MCED 检测整合 CNV 作为四大信号维度之一，CNVPipe 方法可复用
