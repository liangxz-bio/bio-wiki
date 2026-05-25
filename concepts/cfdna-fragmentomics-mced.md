---
title: "cfDNA 片段组学多癌早检（MCED）"
created: 2026-05-16
updated: 2026-05-16
type: concept
tags: [cancer-screening, mced, liquid-biopsy, cfDNA, fragmentomics, early-detection]
domain: cancer-biology
sources:
  - raw/articles/南京医科大学-NatureMedicine-多癌早检-cfDNA片段组学.md
  - raw/articles/NatureReviews-癌症筛查指南-五大癌种-AI-MCED.md
confidence: high
---

# cfDNA 片段组学多癌早检（MCED）

## 核心发现链

多癌早检（MCED, Multi-Cancer Early Detection）通过分析血浆 cfDNA 的多维度特征实现一次性筛查多种癌症类型，核心突破在于**覆盖目前缺乏标准筛查方案的癌种**（卵巢癌、胰腺癌、胆管癌等）。当前主流技术路线整合 cfDNA 的四大信号维度：片段组学（fragmentomics）、甲基化（methylation）、拷贝数变异（CNV）和突变（mutation）。南京医科大学邵阳团队 2026 年发表 Nat Med 研究，通过无PCR全基因组测序整合多种基因组和片段组学特征，I 期敏感率达 79.3%，特异性 98.9%，组织起源 TOP1 准确率 82.3%，覆盖 13 种癌症类型。

## 关键证据摘要

### 南京医科大学 Nat Med MCED 研究（Shao Y et al. 2026）

- **研究规模**：三阶段 6,160 癌症患者 + 5,134 非癌症对照（前瞻性多中心）^[raw/articles/南京医科大学-NatureMedicine-多癌早检-cfDNA片段组学.md]
- **检测敏感性**：I 期 79.3% → II 期 86.9% → III 期 92.4% → IV 期 97.1%
- **特异性**：标准 98.9%（内部）/ 97.8%（独立）；高阈值 99.9%（内部）/ 98.9%（独立）
- **癌种覆盖**：13 种（乳腺癌、宫颈癌、结直肠癌、子宫内膜癌、食管癌、胃癌、肝癌、肺癌、卵巢癌、胰腺癌、前列腺癌、胆管癌、淋巴瘤）
- **高敏感性癌种**：肝癌/胆管癌 100%，卵巢癌 90.5%（目前无标准筛查方案）
- **组织起源（TOO）**：TOP1 82.3%，TOP2 90.7%
- **无症状队列（金陵研究）**：3,724 人中敏感性 53.5%，特异性 98.1%，PPV 25.0%，NPV 99.4%；检出癌中早期占 93.0%

### GRAIL Galleri MCED

- NHS-Galleri 试验：14 万人前瞻性队列，可识别 50 余种癌症信号 ^[raw/articles/NatureReviews-癌症筛查指南-五大癌种-AI-MCED.md]
- 12 种无标准筛查方案的癌症检出敏感性 > 79%
- 在卵巢癌、胰腺癌等难治癌种早期检出潜巨大

## cfDNA 四大信号维度对比

| 维度 | 原理 | 技术路线 | 代表研究 |
|------|------|---------|---------|
| **片段组学（Fragmentomics）** | 癌细胞凋亡片段长度分布异常 | 低深度 WGS | NJMU Shao 2026, DELFI |
| **甲基化（Methylation）** | 组织特异性甲基化模式异常 | 靶向甲基化测序 | CancerSEEK, GRAIL |
| **拷贝数变异（CNV）** | 肿瘤基因组非整倍体 | WGS / WES | 多方法整合 |
| **突变（Mutation）** | 驱动基因体细胞突变 | 靶向深度测序 | CancerSEEK |

MCED 的核心性能约束：早期（I 期）cfDNA 信号浓度极低（~0.01% 肿瘤 fraction），决定了灵敏度的上界。采样时机、肿瘤类型和肿瘤负荷共同决定检测性能。

## 五大癌种筛查现状

| 癌种 | 推荐方法 | 效果 | MCED 补充价值 |
|------|---------|------|-------------|
| 乳腺癌 | 钼靶 | 死亡率↓20% | AI 辅助降假阳性 |
| 结直肠癌 | FIT / 肠镜 | 发病率↓25% | 提高依从性 |
| 宫颈癌 | HPV 检测 | 发病率↓50%+ | 居家自采样 |
| 前列腺癌 | PSA + MRI | 死亡率↓20% | 减少过度诊断 |
| 肺癌 | LDCT | 死亡率↓20% | 辅助结节良恶性判断 |

## 交叉引用

- [[entities/deeprare]] — AI 智能体罕见病诊断，与 MCED 同为 AI 驱动早期诊断范式
- [[entities/methylvi]] — 单细胞甲基化 VAE 模型，MCED 甲基化信号可借鉴其降噪思路
- [[entities/cnvpipe]] — WGS CNV 检测流程，MCED 的 CNV 信号可复用

## 局限与未公开

- MCED 的 PPV 在无症状人群中仍偏低（金陵研究 25.0%），导致高假阳性率
- 不同癌种各期敏感性差异大（I 期胆管癌 vs I 期前列腺癌可能完全不同）
- 金陵研究仅中期分析，完整 15,000 人数据尚未发表
- GRAIL Galleri 完整数据尚未从 NHS 试验公开
- 成本问题：WGS/Methylation 测序仍远高于传统筛查方法
