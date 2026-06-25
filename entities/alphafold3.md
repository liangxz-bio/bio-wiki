---
title: "AlphaFold 3 — 全生物分子统一结构预测框架"
created: 2026-05-16
updated: 2026-05-16
type: entity
tags: [structure-prediction, alphafold, deepmind, diffusion-model, protein-ligand, ai-drug-discovery]
domain: ai-drug-discovery
sources:
  - raw/articles/AlphaFold3深度解读-扩散模型与全生物分子时代.md
confidence: high
---

# AlphaFold 3

## 核心发现链

AlphaFold 3 (AF3) 是 Google DeepMind 发布的全生物分子统一结构预测框架，标志从"蛋白质专用工具"向"全生物分子通用框架"的范式转移。首次在单一深度学习框架中覆盖蛋白质、核酸（DNA/RNA）、小分子配体、离子、共价修饰残基组成的任意复杂复合物。核心架构创新：用**扩散模型（Diffusion Model）**取代 AF2 的结构模块，从随机噪声递归去噪生成原子坐标；用 **Pairformer 模块**取代 Evoformer，不再保留 MSA 表示，降低计算成本。在 PoseBusters 基准测试中，蛋白-配体对接成功率 76.4%，碾压传统对接工具 Vina（52.3%）。

## 关键证据摘要

- **全生物分子覆盖**：统一 Token 化处理蛋白质序列、核酸序列、配体 SMILES，单一框架预测任意复合物结构 ^[raw/articles/AlphaFold3深度解读-扩散模型与全生物分子时代.md]
- **蛋白-配体对接**：PoseBusters 基准成功率 76.4%，Vina 仅 52.3%
- **架构革新**：扩散模块取代结构模块——低噪声优化局部化学细节，高噪声优化全局结构；Pairformer × 48 模块处理成对相互作用，MSA 嵌入精简为仅 4 个块
- **交叉蒸馏（Cross-Distillation）**：抑制无序区"幻觉"，减少虚假紧凑结构
- **置信度校准**：高置信度与高准确度正相关；通过扩散 rollout 训练置信度头

## 与现有工具对比

| 维度 | AlphaFold 3 | AlphaFold 2 | IsoDDE | Protenix-v2 |
|------|-------------|-------------|--------|-------------|
| 覆盖分子类型 | 蛋白+核酸+配体+离子+修饰 | 蛋白单体/复合物 | 蛋白+小分子+抗体-抗原 | 蛋白+抗体 |
| 核心架构 | Diffusion + Pairformer | Evoformer + IPA | Transformer | Transformer |
| 蛋白-配体对接(PoseBusters) | 76.4% | N/A（不原生支持配体） | 声称优于 AF3 | N/A |
| MSA 依赖 | 低（Pairformer 不保留 MSA 表示） | 高（Evoformer 保留 MSA） | 未公开 | 未公开 |
| 开源 | 服务器仅用，未开源代码/权重 | 开源（DeepMind/ColabFold） | 未开源 | 未开源 |
| 动态构象 | 否（静态结构） | 否 | 否 | 否 |

AF3 与 IsoDDE 的关系：两者都声称超越对方——AF3 用论文 benchmark，IsoDDE 用技术报告 benchmark，缺乏第三方独立验证。

## 已知局限

- **立体化学违规**：约 4.4% 手性错误或原子重叠
- **静态结构**：无法捕捉溶液中的动态行为
- **构象偏差**：倾向于预测"结合态"，难以还原开放态
- **无序区假秩序**：尽管置信度低，仍可能产生虚假结构
- **未开源**：仅限服务器非商业使用，限制本地部署和二次开发
- **发现新 PPI 能力弱**：AF/RF 高置信预测中严格新 PPI 远少于实验方法（酵母 30 vs YeRI 1,382），偏向大界面/稳定复合物/已知蛋白，domain-motif 互作恢复率差 ^[raw/articles/Nature Communications 2026｜AlphaFold 能算出完整互作组吗？这篇实验评估给出答案.md]

## 交叉引用

- [[entities/isodde]] — AI 药物设计引擎，声称全面超越 AF3 的预测系统
- [[entities/de-novo-protein-design]] — AF3 为从头蛋白质设计提供结构预测基础
- [[entities/protenix-v2]] — 同类结构预测工具，ByteDance Seed 版
- [[entities/protdbench]] — 蛋白 binder 设计评测基准，涵盖 Boltz/Chai 等 AF 复现方法
- [[entities/ovo]] — 蛋白设计生态系统，可整合 AF3 结构预测作为验证环节
- [[entities/tcr-prp]] — TCR-PRP pLM 在 T 细胞激活预测中全面超越 AF3（Nat Biotechnol 2026），说明结构预测在功能预测场景的局限
- [[entities/highfold]] — HighFold 系列环肽结构预测工具（CycPOEM + ncAA + Cyclization 开关），构建于 AF2/AF3 之上
- [[entities/pplm]] — PPLM 蛋白配对语言模型，PPLM-Contact2 融合 AF2.3/AF3/DMFold 链间距离图提升接触精度
- [[concepts/ai-interactome-mapping-limitations]] — AI 互作组预测边界：AF 适合为实验发现的 PPI 补结构，不适合发现新 PPI
