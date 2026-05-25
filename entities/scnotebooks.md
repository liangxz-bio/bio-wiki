---
title: scNotebooks
created: 2026-05-06
updated: 2026-05-06
type: entity
tags: [bioinformatics, pipeline, ngs]
domain: bioinfo-pipeline
sources: [raw/articles/NatGen-scNotebooks开源单细胞空间组学培训笔记库.md]
confidence: high
---

# scNotebooks

全球首个面向低资源环境、多语言支持的单细胞与空间组学分析体系化培训平台。发表于 **Nature Genetics (2026)**。

## 基本信息

- **论文**：The Single Cell Notebooks for inclusive and accessible training in single-cell and spatial omics. *Nature Genetics, 2026.*
- **GitHub 仓库**：https://github.com/integrativebioinformatics/scNotebooks
- **免费共享服务器**：https://vip.r-py.com/

## 核心设计原则（普惠性）

- **多语言覆盖**：英语、西班牙语、葡萄牙语
- **云端运行**：Google Colab 免安装运行 + Jupyter/Docker 本地部署
- **体系化能力框架**：基于领域专家验证的组学分析能力框架

## 13 个模块（三层结构）

### 基础层（零基础入门）
1. Notebook 使用
2. R 编程与 ggplot2 可视化
3. 公共数据库使用

### 核心层（独立分析能力）
4. 单细胞上游数据处理
5. 质控聚类注释
6. 多样本整合
7. 轨迹分析
8. 细胞通讯

### 进阶层（前沿分析方向）
9. 多模态分析
10. TCR 分析
11. 空间转录组
12. scATAC-seq 分析
13. 可变多聚腺苷酸化分析
14. FAIR 数据共享

## 关键数据指标

| 指标 | 数据 |
|------|------|
| 云端环境运行成功率 | 98% |
| 覆盖国家数 | 12 个 |
| 官方培训场次 | 27 场 |
| 累计服务研究者 | 1,800+ 名 |
| 中低收入国家学员占比 | 82% |
| 完成核心层后独立分析能力 | 76% |

## 工具栈

使用领域主流分析工具：Seurat、Harmony、Monocle3 等。

## 生信视角

- **核心创新**：云端免安装部署，解决初级研究者缺乏本地算力和环境配置能力的痛点
- **可改进点**：未提供多工具基准测试内容
- **参考价值**：普惠性设计应成为培训资源的核心考量维度
- **与 [[scrnaseq-workflow]] 的关系**：scNotebooks 的 13 个模块覆盖了该流程中的 Step 1-5 及高级分析
