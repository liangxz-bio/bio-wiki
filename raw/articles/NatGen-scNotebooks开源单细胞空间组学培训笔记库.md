---
source_url: https://mp.weixin.qq.com/s/dMzobbY0RafLYdVlzSK4zw
ingested: 2026-05-06
sha256: 775f22631c602676383a51dfc2c46a380df49ce4c7fce9daead4bc867f7f695e
citation: "Rojas-Hidalgo A, Arias-Carrasco R, Silva JK, Armingol E, Urquiza-Zurich S, Vinagre B, Prada-Medina CA, Triana S, Russo DD, Pérez-Stuardo D, Vicencio E, Muñoz G, de Lanna CA, Santos L, Rapozo G, Tavares N, Moreno-Estrada A, Randell J, Severino P, Khouri R, Ashenberg O, Shalek AK, Boroni M, Cuesta-Astroz Y, Carvalho BS, Maracaja-Coutinho V. The Single Cell Notebooks for inclusive and accessible training in single-cell and spatial omics. Nat Genet. 2026 May 5. doi: 10.1038/s41588-026-02584-0. Epub ahead of print. PMID: 42086855."
domain: bioinfo-pipeline
---

# Nat Gen | 牛，顶刊做的开源的单细胞与空间组学分析培训笔记库scNotebooks，降低语言与算力门槛，推动全球组学研究的公平参与

> **作者**：生信钱同学  
> **来源**：Nature Genetics  
> **资源链接**：https://github.com/integrativebioinformatics/scNotebooks/tree/main

---

## Core Breakthrough

这项发表于《Nature Genetics》的研究，核心成果是构建了**全球首个面向低资源环境、多语言支持的单细胞与空间组学分析体系化培训平台scNotebooks**。

### Key Design Principles（普惠性）：

- **多语言覆盖**：英语、西班牙语、葡萄牙语，打破全球非英语母语研究者的语言壁垒
- **云端运行模式**：无需本地安装复杂分析环境、无需高端算力设备，仅通过浏览器访问谷歌Colab即可完成全流程实操
- **体系化能力框架**：基于领域专家验证的组学分析能力框架，覆盖从基础编程入门到高级多组学整合分析

### Impact Statistics：

- 在非洲、美洲、亚洲的**20余场**组学培训活动中使用
- 累计服务超过**1700名**来自中低收入国家的研究者
- 显著提升了当地研究者的独立组学分析能力

---

## 研究背景

近年来，**单细胞RNA测序、空间转录组技术**已成为生命科学领域的核心工具。但存在两大核心痛点：

1. **语言壁垒**：全球绝大多数优质培训资源均为英语
2. **算力匮乏**：要求本地高性能算力支撑
3. **资源分散**：现有教程多为零散的代码片段，缺乏成体系的能力培养框架

**受影响地区**：中低收入国家，特别是拉美、非洲等组学发展相对滞后地区

---

## 技术创新

| 创新点 | 具体内容 |
|--------|----------|
| **体系化模块化课程设计** | 基于领域专家共同制定的单细胞组学分析能力框架，开发13个递进式模块 |
| **多环境适配方案** | 支持谷歌Colab云端免安装运行、Jupyter与Docker本地部署两种模式 |
| **多语言覆盖** | 所有课程材料同步提供英语、西班牙语、葡萄牙语三个版本 |

---

## 实验结果

### 模块结构（13个模块分三层）

**基础层**（零基础入门）：

- Notebook使用
- R编程与ggplot2可视化
- 公共数据库使用

**核心层**（独立分析能力）：

- 单细胞上游数据处理
- 质控聚类注释
- 多样本整合
- 轨迹分析
- 细胞通讯

**进阶层**（前沿分析方向）：

- 多模态分析
- TCR分析
- 空间转录组
- scATAC-seq分析
- 可变多聚腺苷酸化分析
- FAIR数据共享

### 核心数据指标：

| 指标 | 数据 |
|------|------|
| 云端环境运行成功率 | **98%** |
| 覆盖国家数 | **12个** |
| 官方培训场次 | **27场** |
| 累计服务研究者 | **1800+名** |
| 中低收入国家学员占比 | **82%** |
| 完成核心层后独立分析能力 | **76%** |

---

## 应用前景和未来展望

1. **全球培训班的官方教材**：尤其适配中低收入国家的培训需求
2. **个体研究者的自学材料**：零基础入门，无需额外承担算力成本
3. **高校生信专业配套实操课程**

### 未来计划：

- 拓展法语、中文等更多语言支持
- 补充最新的空间多组学分析流程
- 搭建社区交流平台
- 持续扩大普惠覆盖范围

---

## 生信视角解读

- **主流工具**：Seurat、Harmony、Monocle3等均为领域主流分析工具
- **核心创新**：云端免安装的部署逻辑，解决了初级研究者缺乏本地算力、不会配置环境的核心痛点
- **可改进点**：目前流程暂未提供多工具基准测试内容，后续可补充不同分析策略的效果对比
- **参考价值**：普惠性设计应成为培训资源的核心考量维度

---

## 文献引用

> The Single Cell Notebooks for inclusive and accessible training in single-cell and spatial omics. **Nature Genetics, 2026.**

---

## Related Resources

| Resource | Link |
|----------|------|
| GitHub Code Repository | https://github.com/integrativebioinformatics/scNotebooks |
| Free Shared Server | https://vip.r-py.com/ |
