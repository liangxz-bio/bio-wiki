---
title: CLIPP-SET / MCLM — 对比 3D 蛋白-配体学习与条件分子生成
created: 2026-05-21
updated: 2026-05-21
type: entity
tags: [contrastive-learning, protein-ligand, SE3-equivariant, virtual-screening, drug-design, generative-model, CLIP]
domain: ai-drug-discovery
sources: [raw/articles/DrugCLIP-MCLM-对比学习-AIDD-arXiv2026.md]
confidence: moderate
---

# CLIPP-SET / MCLM
## 对比 3D 蛋白-配体学习：从向量检索到条件分子生成

## 研究概览

Acellera Labs / UPF (Navarro, Tholke, de Fabritiis) 在 **arXiv 2026 (2604.19562)** 提出两阶段系统：先通过对比学习构建 3D 蛋白-配体统一嵌入空间（CLIPP-SET），再基于该嵌入做条件式自回归分子生成（MCLM）。核心思路是把对比学习从"检索"扩展到"生成"。^[raw/articles/DrugCLIP-MCLM-对比学习-AIDD-arXiv2026.md]

## 模型架构

### 子系统 A：CLIPP-SET（对比编码器）

| 组件 | 说明 |
|------|------|
| 架构 | SET（Scalable Equivariant Transformer），SE(3) 等变 dot-product attention |
| 输入 | 配体 3D 构象（原子类型+坐标）或蛋白口袋 3D 结构 |
| 输出 | 256 维共享嵌入向量 |
| 预训练 | 配体：287M 构象 MLM（PubChem + Enamine + Mcule + ChEMBL + GEOM-drugs） |
| | 口袋：ProFSA 5.5M fragment-pocket pairs |
| 对比微调 | SIU 数据集 1.29M ligand-pocket pairs（9662 唯一口袋），docking 生成 pose |
| 损失函数 | **CF-InfoNCE**（Collision-Free InfoNCE），处理多 ligand 共享同一 pocket 的非双射场景 |
| 下游用途 | 零样本虚拟筛选、大库检索（pocket→ligand / ligand→ligand） |

### 子系统 B：MCLM（多模态化学语言模型）

| 组件 | 说明 |
|------|------|
| 架构 | Llama2-style autoregressive decoder |
| 输入序列 | [dataset_token, projected_3D_embedding, SMILES_token_1, SMILES_token_2, ...] |
| 3D 条件 | CLIPP-SET 冻结编码器 → 可训练线性投影 W_P 映射到 decoder hidden dim |
| 训练目标 | 标准 next-token prediction（cross-entropy） |
| 条件控制 | **dataset token**：选择 PubChem/ChEMBL/Enamine/Mcule 等来源，硬切换化学空间 |
| 推理 | 支持 ligand embedding 或 pocket embedding 作为条件 |

### 关键设计选择

- **3D 嵌入 + SMILES** 而非 3D point cloud：用"几何感知→化学合理性"的交易换取训练数据规模、推理效率和合成可行性
- **训练时只用 ligand embedding**，推理时 ligand/pocket 通用：赌 CLIPP-SET 对齐足够好，pocket embedding 落在可 bind 配体的嵌入质心附近
- **FlashAttention / RoPE / KV cache** 直接复用 LLM 工程化红利

## 实验结果

### CLIPP-SET 虚拟筛选（LIT-PCBA）

| 指标 | CLIPP-SET | Gnina (docking) | BigBind (docking) | DrugCLIP |
|------|:---------:|:---------------:|:-----------------:|:--------:|
| BEDROC | **最优** | — | — | +10-15% |
| EF(0.5%) / EF(1%) | **最优** | — | — | +10-15% |
| EF(5%) | 2.06 | **更高** | **更高** | — |
| AUROC | 53.76% | **60.93%** | **60.80%** | — |

> 早期富集强（top-k 检索优势）→ 广义排序弱（contrastive embedding 的典型"职业病"）

### 三种检索策略的定位

| 策略 | 化学相似度 | 多样性 | 适用场景 |
|------|:---------:|:------:|----------|
| Morgan FP 检索 | 0.36（高） | 0.59（低） | ME-too 优化、已知配体近亲 |
| CLIPP-SET ligand 检索 | 0.20（低） | 中高 | **scaffold hopping**、IP 突围 |
| CLIPP-SET pocket 检索 | 0.11（最低） | **最高** | 冷启动、orphan target、cryptic pocket |

### MCLM 生成 vs 检索

- MCLM (ligand) 在 15 个 LIT-PCBA target 中 12 个预测亲和力排第一
- MCLM (pocket) 大致与 CLIPP-SET 检索持平 — 无 active reference 时生成不显著优于检索
- dataset token 有效将生成分子拉向 Enamine REAL 商业化学空间，但存在精确匹配（retrieve 而非 create）的副作用

## 创新与局限

### 真正创新
1. **对比空间作为多任务接口** — retrieval + generation + ranking 跑在同一嵌入空间
2. **SET 架构的工程取舍** — FlashAttention 兼容的 SE(3) 等变 transformer，可扩展到亿级数据
3. **dataset token 工业化设计** — 化学空间硬切换，比 SA score 后过滤优雅
4. **CF-InfoNCE** — 承认 drug-target 非双射关系的损失修正

### 必须直面的局限
1. **评估循环风险** — 几乎完全依赖 Boltz-2 评估，无 docking 交叉验证、无 prospective validation
2. **SIU 训练 pose 非晶体** — binding conformation 本质是 docking 的 plausible pose 而非真实结构
3. **推理时 64 conformer 选最优** — 穷举绕过了 binding-compatible conformer 子问题
4. **缺少 SBDD 扩散模型直接对比** — 未对标 Pocket2Mol / TargetDiff / DiffSBDD / MolCRAFT

## 交叉引用

- [[entities/softmol]] — SoftMol 块扩散分子生成（同为 AIDD 生成模型，技术路线对比）
- [[entities/boltz2]] — Boltz-2 蛋白-配体共折叠，本文用作 affinity 评估器（评估循环风险来源）
- [[entities/boat]] — BOAT 抗体多目标贝叶斯优化（对比 learning + generation 的另一路径）
- [[concepts/transformer]] — SET 基于 Transformer 架构，MCLM 使用 Llama2 decoder
