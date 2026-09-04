---
title: ProteinDPO — 用 DPO 对齐蛋白生成模型到实验稳定性
created: 2026-09-04
updated: 2026-09-04
type: entity
tags: [protein-language-model, direct-preference-optimization, protein-design, stability-prediction, esm-if1, influenza-vaccine, h5-ha, alignment, nature-methods]
domain: ai-drug-discovery
sources: [raw/papers/widatalla2026-proteindpo-naturemethods.md]
confidence: high
---

# ProteinDPO — 用 DPO 对齐蛋白生成模型到实验稳定性

Widatalla T, Borah AA, King SH, Driscoll CL, Rafailov R, Hie BL. *Aligning protein-generative models to experimental fitness with ProteinDPO*. Nature Methods (2026). doi: 10.1038/s41592-026-03137-3.

**核心贡献**：用 DPO 把结构条件化蛋白语言模型 ESM-IF1 对齐到实验测量的稳定性数据，**保留无监督预训练知识的同时注入任务特定的偏好信号**。

## 核心问题：alignment gap

无监督训练学的规则与目标属性不一致——导致在稳定性等任务上落后于专用监督模型（如 ThermoMPNN）。

- SFT 只最大化正样本似然，容易过拟合并丢失预训练的一般知识
- **DPO** 用偏好对（正/负样本）进行轻量对齐，鼓励模型在"看起来相似但实验偏好不同"的样本之间学会区分

## 方法：DPO 对齐 ESM-IF1

### 数据

- **Megascale 数据集** ~184 万变体，过滤后得到 **~66 万变体偏好对**，覆盖 **403 个蛋白结构域**
- 三种 DPO 训练目标：paired（偏好对）、ranked（排名）、weighted（标量标签）——weighted 是本文新提出的

### 防数据泄漏

用 **FoldSeek** 在 0.5 相似度阈值上做结构聚类，将簇随机分布到训练/测试 split。

## 关键结果

### Megascale holdout 上的稳定性预测

| 模型 | Pearson R | Spearman ρ | AUROC |
|------|-----------|------------|-------|
| Vanilla ESM-IF1 | 0.55 | 0.53 | 0.74 |
| SFT ESM-IF1 | 0.59 | 0.57 | 0.76 |
| **ProteinDPO** | **0.72-0.73** | **0.69-0.72** | **0.82-0.84** |

### 双突变子集 vs ThermoMPNN

按整条序列似然求和评分时，ProteinDPO 超越 ThermoMPNN：

| 模型 | Pearson R（双突变） |
|------|---------------------|
| ThermoMPNN | 0.41 |
| **ProteinDPO** | **0.44-0.48** |

**关键优势**：ProteinDPO 即使只用单突变训练，整序列推理能捕捉多突变非线性效应；ThermoMPNN 受限于"加性"假设。

### 跨数据集泛化

| 数据集 | ProteinDPO Pearson R | ThermoMPNN Pearson R |
|--------|----------------------|----------------------|
| S669 | 0.44-0.47 | 0.43 |
| FireProt | 0.6 | 0.65 |

SFT 模型在 FireProt/S669 上反而下降（过拟合证据），ProteinDPO 持续提升（Vanilla R 增加 0.13-0.14）。

### 零样本迁移

- **SKEMPIv2 结合亲和力**：Pearson R 增加 0.02-0.04
- **AB-Bind 抗体-抗原**：Pearson R 增加 0.04-0.08
- **抗体 Tm（483 个临床/人源抗体）**：Pearson R 增加 0.08-0.12
- ThermoMPNN **无法** 评估多链蛋白（如大多数抗体）或绝对稳定性指标

### 序列生成质量

三个大蛋白 backbone（136-394 残基）：staphylococcal nuclease (1STN)、PGK (1PHP)、claudin-15 (4P79)

- ProteinDPO 生成的序列被 Rosetta 能量评估为**显著更稳定**（更低 REU）
- ESMFold 预测结构 pLDDT > 80，与原生结构匹配
- **计算 sanity-check**：优化稳定性没有丢失结构条件化序列设计的规则

### 应用：H5N1 流感 HA pre-fusion 稳定化

只用了 **45 个设计**：
- **27 个稳定变体**（~80% 设计达到 native 或更高稳定性）
- 2004 株 Tm 提升 **+17°C**
- 2024 株最高 **+32°C**，且保留广谱中和抗体结合能力

## DPO 三种训练目标

| 目标 | 输入格式 | 备注 |
|------|----------|------|
| **paired** | 偏好对（正/负） | 原文 DPO 设定 |
| **ranked** | 排名 | 偏好排序 |
| **weighted** | 标量标签（ΔΔG） | **本文新提出**——直接利用连续标量数据 |

## 代码与数据

- GitHub: https://github.com/evo-design/protein-dpo
- DOI: 10.1038/s41592-026-03137-3

## 相关页面

- [[raw/papers/widatalla2026-proteindpo-naturemethods.md]] — 原文 MinerU 解析
- [[raw/articles/RNS-蛋白质embedding不确定性-NatureMethods2026]] — 同期 Nature Methods：embedding 不确定性
- [[entities/vesm]] — VESM：ESM 家族 co-distillation
- [[entities/random-neighbor-score]] — RNS：embedding 不确定性度量
