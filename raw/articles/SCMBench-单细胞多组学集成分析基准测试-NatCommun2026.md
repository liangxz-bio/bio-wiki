---
source_url: https://mp.weixin.qq.com/s/P4wQrQTCMcZEgY09XhEWYQ
ingested: 2026-05-08
sha256: 50f808363c5221987bc905628fcb0cb20cf65bf967e1077ee0d81b933323e0b5
citation: "Wang Y, Fan Y, Wang X, Yu T, Zong Y, Liu X, Zhong G, Liu M, Li Q, Lee KH, Dallakyan K, Hu Z, Qi Y, Huang J, Jia G, Yuan J, Chan TF, Gao X, King I, Li Y. SCMBench: benchmarking domain-specific and foundation models for single-cell multi-omics data integration. Nat Commun. 2026 May 2. doi:10.1038/s41467-026-72570-x. Epub ahead of print. PMID: 42069651."
domain: bioinfo-pipeline
---

# SCMBench：单细胞多组学集成分析的基准测试新利器

> **来源：** Nature Communications | **作者单位：** 香港中文大学 | **公众号：** 生物信息前沿进展
> **原文链接：** https://mp.weixin.qq.com/s/P4wQrQTCMcZEgY09XhEWYQ

---

## 文章基本信息

| 字段 | 内容 |
|------|------|
| **期刊** | Nature Communications (Nat Commun) |
| **论文标题** | SCMBench: benchmarking domain-specific and foundation models for single-cell multi-omics data integration |
| **通讯作者** | Yu Li（李煜），香港中文大学计算机科学与工程系教授 |
| **第一作者** | Yixuan Wang、Yimin Fan、Xuesong Wang |
| **DOI** | https://doi.org/10.1038/s41467-026-72570-x |

---

## 研究背景

单细胞多组学技术（如 **scRNA-seq**、**scATAC-seq** 和 **DNA甲基化测序** 等）的飞速发展，为在单细胞水平深入理解以下方面提供了前所未有的机遇：

- 细胞异质性
- 基因表达调控
- 细胞发育轨迹

研究者们开发了大量计算工具，主要分为两类：

| 类型 | 全称 | 代表工具 |
|------|------|----------|
| **DMs** | 领域特定模型（Domain-specific Models） | 基于统计学或深度学习的专门工具 |
| **FMs** | 大规模基础模型（Foundation Models） | scGPT、scFoundation 等 |

**现存问题：**
1. 缺乏全面的评价标准，难以指导研究者选择最优模型；
2. FMs 在处理非转录组数据（如染色质可及性）时的有效性尚待系统性评估；
3. FMs 与传统 DMs 的优劣对比不明确。

---

## 论文概要

本研究提出了名为 **SCMBench** 的综合性基准测试框架，对 **23种** 单细胞多组学集成方法进行了系统评估：

- **19种** 领域特定模型（DMs）
- **4种** 基础模型（FMs）

**评估维度涵盖：**

- 集成准确性
- 生物标志物检测
- 轨迹推断
- 批次效应校正
- 计算效率
- 可扩展性

---

## 主要研究结果

### 一、SCMBench 评估框架概述（图1）

- **数据集：** 6个真实数据集 + 模拟数据集
  - 人类 PBMC
  - 小鼠大脑
  - 小鼠皮肤
  - 小鼠肾脏
- **评估指标：** MAP、NMI、ASW、ARI 等
- **FMs 处理策略：** 使用 MAESTRO 等工具将 ATAC 数据转换为基因活性矩阵，实现跨模态处理

---

### 二、数据集成准确性：DMs 目前仍占据优势（图2）

| 方法 | 适用场景 | 表现 |
|------|----------|------|
| **GLUE** | 成对（Paired）数据集成 | 性能最稳定 |
| **scJoint** | 非成对（Unpaired）数据集成 | 具有明显优势 |
| **FMs（零样本模式）** | 多组学集成 | 普遍逊于顶尖 DMs |

> **关键发现：** FMs 在多组学集成中的挑战主要源于**不同模态间的数据分布差异**。

---

### 三、生物标志物识别：GLUE 与 scJoint 保持领先（图3）

- **评估指标：** Jaccard 相似性指数（JSI）
- **评估内容：** 转录组标志物 + 表观遗传调节基元（Motifs）的保留一致性

| 方法 | 表现 |
|------|------|
| **GLUE** | 更可靠地保留细胞身份特征 |
| **scJoint** | 更可靠地保留细胞身份特征 |
| **DeepMAPS** | 偶尔能捕捉到顶尖方法漏掉的特定细胞表面受体（如 SLAMF7） |

> **有趣发现：** 不同模型在生物学特征提取上具有**互补性**，中等表现方法有时能捕捉到顶尖方法遗漏的信息。

---

### 四、轨迹推断与发育动态保留：FMs 展现出潜力（图4）

| 场景 | 最佳方法 | 备注 |
|------|----------|------|
| 成对场景 | **MOFA** | 轨迹推断表现最佳 |
| 非成对场景 | **Harmony** | 领先于其他方法 |
| 跨物种轨迹保留 | **UCE（FM）** | TC 得分 > 0.7，显著优于许多 DMs |

> **关键发现：** FMs 虽在对齐准确性上稍逊，但在**保留生物发育信号**方面表现不俗，体现了强大的预训练生物学知识背景。

---

### 五、批次效应校正：scJoint 与 MMD-MA 表现强劲（图5）

| 方法 | 表现 |
|------|------|
| **scJoint** | 最佳平衡"消除技术偏差"与"保留生物信号" |
| **MMD-MA** | 同样表现强劲 |
| **Cobolt** | 过于激进，可能抹除真实生物学差异 |

> **警示：** 过于激进的批次校正虽能让不同批次数据完美混合，但也可能**意外抹除生物学上的真实差异**。

---

### 六、FMs 的适配策略：轻量级适配器显著提升性能（图6）

**核心策略：** 轻量级适配策略（以 **FM-scVI** 为代表）

```
FM 提取嵌入向量（Embeddings）
        ↓
  作为 scVI 框架的输入
        ↓
      微调处理
        ↓
  集成性能质的提升
```

**效果验证：**

| 方法 | 提升幅度 | 数据集 |
|------|----------|--------|
| **UCE-scVI** vs 零样本 UCE | +65% 以上 | 小鼠数据集 |

---

## 全文总结：不同场景的工具选择指南

| 应用场景 | 推荐方法 | 理由 |
|----------|----------|------|
| **集成准确性优先** | GLUE 或 scJoint | 综合表现最优 |
| **处理非成对数据** | scJoint | 最佳平衡点 |
| **计算资源受限** | scJoint 或 Harmony | 内存占用少、运行时间短 |
| **跨物种分析** | FMs（如 UCE） | 具有独特泛化优势 |

---

## 展望

1. **当前结论：** DMs 在大多数指标上领先，但通过合理的"适配层"设计，FMs 能够发挥强大的泛化能力；
2. **未来方向：** 随着更多真正意义上的"多组学基础模型"出现，**单细胞虚拟细胞（AI Virtual Cells）** 的构建将有望实现跨模态信息的完美融合。

---

## 研究团队与资助

**研究团队：**
- **第一作者：** Yixuan Wang、Yimin Fan、Xuesong Wang（均来自香港中文大学）
- **通讯作者：** Yu Li（李煜）教授，香港中文大学计算机科学与工程系

**资助来源：**
- 国家重点研发计划（2025YFA0923500）
- 深圳医学研究基金
- 香港研究资助局（RGC）
- 阿卜杜拉国王科技大学（KAUST）相关基金
