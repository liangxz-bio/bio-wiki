---
title: DeepRare
created: 2026-05-12
updated: 2026-05-12
type: entity
tags: [model, clinical, diagnosis, ai-agent, rare-disease]
domain: clinical-evaluation
sources: [raw/articles/DeepRare-AI驱动罕见病诊断智能体-Nature2026.md, raw/papers/zhao2026-deeprare-nature.md]
confidence: medium
---

# DeepRare

DeepRare 是全球首个专为罕见病差异诊断设计的**可追溯智能体系统**，发表于 Nature 2026（Zhao W, et al. Nature 2026）。核心模型为 DeepSeek-V3，集成超过 40 个专业工具和实时知识源，支持自由文本、HPO 术语和 WES VCF 多种输入，采用 MCP 启发的三层架构（中央 LLM 主机 + 专用代理服务器群 + 外部知识环境）。^[raw/articles/DeepRare-AI驱动罕见病诊断智能体-Nature2026.md]

## 核心发现链

罕见病诊断四大难题（异质性强、病例稀少、知识更新快、需可解释性）→ MCP 启发三层智能体架构 → 表型提取 + 知识检索 + 病例匹配 + 基因分析的并行信息收集 → 自反射诊断迭代 → Top-K 诊断列表（附可追溯推理链）→ **首次 AI 在罕见病表型诊断中全面超越人类专家**（Recall@5 78.5% vs 65.6%）。

## 关键证据摘要

- **性能**：仅 HPO 表型输入 Recall@1 57.18%，超越现有方法 23.79%；引入 WES 基因数据后新华医院 Recall@1 从 39.9% 提升至 69.1%（Exomiser 55.9%）
- **人机对比**：新华医院 163 例门诊自由文本盲测，DeepRare Recall@5 78.5% vs 10年以上专家 65.6%，**AI 首次超越人类专家**
- **可解释性**：180 例推理链由 10 名副高以上医师盲评，参考文献事实准确率 95.4%；200 例失败分析中事实性错误仅 2.5%
- **跨专科**：肾脏泌尿系统 Recall@1 66%，长尾罕见病（≤10 例）Recall@1 > 0.8 比例 31.8%
- **基准规模**：6,401 例病例、9 个数据集、2,919 种罕见病、14 个专科，来源包括新华医院和湖南儿童医院

### 增补细节（原始论文）

- **Fig 1 (Architecture)**：MCP 启发三层架构，中央主机（DeepSeek-V3 + 记忆库）+ 4 代理服务器（表型提取器/知识检索器/病例匹配器/基因分析器）+ 外部知识环境（PubMed/Orphanet/OMIM/HPO/67,795 例去重病例库）。系统类比个人电脑架构
- **Fig 2 (HPO-wise)**：7 个公开 + 2 个临床数据集，RareBench-MME 达 78% Recall@1；完整结果见 Supplementary Table 1
- **Fig 3 (Clinical Validation)**：多模态（HPO+WES）Recall@1 69.1%（Exomiser 55.9%）。MIMIC-IV-Rare 上 Recall@1 29%
- **Fig 4 (Traceability)**：5 类失败模式精确占比 — 推理权重误差 41%、表型模拟 38.5%、过度搜索 10%、证据幻觉 8%、事实性错误仅 2.5%
- **Fig 5 (Ablation)**：DeepSeek-V3 作为中央主机表现最优；Agentic 架构使 GPT-4o 从 25.60% 跃升至 54.67%（+28.56%）；自反射模块贡献最大增益
- **支持的 LLM 后端**：DeepSeek-V3（默认）、Claude-3.7-Sonnet、DeepSeek-R1、GPT-4o、Gemini-2.0-flash
- **Web 应用**：5 阶段流程（临床数据录入 → AI 引导问诊 → HPO 映射 → 辅助检查上传 → 诊断报告）^[raw/papers/zhao2026-deeprare-nature.md]

## 来源与交叉引用

- 原始论文 (WeChat 中文解读)：Zhao W, et al. Nature 2026. doi:10.1038/s41586-025-10097-9 ^[raw/articles/DeepRare-AI驱动罕见病诊断智能体-Nature2026.md]
- 原始论文 (Nature 主刊)：Zhao W, Wu C, Fan Y, et al. Nature. 2026 Mar;651(8106):775-784. doi:10.1038/s41586-025-10097-9. PMID:41708847; PMCID:PMC12999473 ^[raw/papers/zhao2026-deeprare-nature.md]
- [[raw/articles/deepvariant-tool-overview]] — 变异检测工具，DeepRare 使用 WES VCF 作为基因分析输入
- [[gbt46943-2025-mngs-standard]] — 临床诊断性能确认国家标准（同类临床验证框架）

## 交叉引用
- [[concepts/cfdna-fragmentomics-mced]] — MCED 多癌早检与 DeepRare 同为 AI 驱动诊断范式，互补覆盖癌症+罕见病
- [[concepts/five-cancer-screening-guide]] — 五大癌种筛查现状，AI 辅助筛查与 DeepRare 诊断范式互补
