---
title: Promera — 蛋白 Binder 折叠·筛选·设计统一模型
created: 2026-06-12
updated: 2026-06-12
type: entity
tags: [protein-design, binder, co-folding, inverse-folding, ai-drug-discovery, diffusion-model]
domain: ai-drug-discovery
sources:
  - raw/articles/MIT全能蛋白 Binder 模型——Promera！接管设计、折叠、筛选全流程！！.md
confidence: medium
---

# Promera

> Jing B, Berger B, Klivans A. Promera: a unified model for biomolecular structure prediction, filtering, and design. bioRxiv 2026 Jun 10. Code: [github.com/bjing2016/promera](https://github.com/bjing2016/promera)

Promera 是 Bowen Jing (DiffDock 一作) × Bonnie Berger (MIT) × Adam Klivans (UT Austin) 联合开发的开源统一模型，将结构预测（co-folding）、binder 筛选和可控 binder 设计压入同一套扩散模型权重。Apache 风格开源。

## 问题

开源 co-folding 模型（OpenFold3、Boltz-2、Protenix-v1）在结构预测精度上快速追赶 AF3，但 binder 筛选能力一塌糊涂——而 binder 设计流水线恰恰依赖 confidence metric 从数千条候选里挑出几十条送湿实验。预测-筛选的断裂是开源设计栈的真实瓶颈。

## 架构

- **基础模型**: 扩散模型，训练时随机 mask 5% 氨基酸 + 移除侧链原子，使模型学会为缺失位置生成可设计的 backbone
- **条件控制**: 叠加 epitope / paratope / template 三种约束信号
- **逆折叠**: 以 ProteinMPNN 收尾将 backbone 转为序列
- **筛选指标**: ipSAE（interface pLDDT-based SAE）用于 binder 筛选；iCS（interface contact score）用于 nanobody-antigen 界面评估

## 关键结果

### Co-folding 精度
在 2025 PDB 全新复合物测试集上，蛋白-蛋白、抗体-抗原、蛋白-配体几乎所有类别压过 OpenFold3-p2 和 Boltz-2，与 Protenix-v1 互有胜负。

### Binder 筛选（核心贡献）
- **Adaptyv 尼帕病毒设计竞赛**（916 条提交，94 真 binder / 822 假 binder）: Promera ipSAE 以 AUROC 0.70 碾压 Boltz-2、AF-Multimer、Protenix-v1。若用 top-20 送实验，理论命中率从 10% → 50%。
- **Nanobody 筛选**: iCS 在 10% recall 下做到 20× 富集，是 AF-Multimer ipTM>0.7 阈值（10×）的两倍。

### 可控 Binder 设计
在 IL7Ra / InsulinR / PDGFRb / PDL1 四个 benchmark 上全面超越 BoltzGen，且在 2/4 靶点超过 mBER（BindCraft 风格的反向传播方法，单样本成功率通常比生成式高一个量级）。

### In silico 案例
- **安第斯汉坦病毒 glycoprotein**: 表面切 8 个 epitope，Promera 在 6 个上成功生成高置信度、区位正确的 VHH
- **β2 肾上腺素受体 (GPCR)**: 单独折叠时 I121/F282 开关残基在 active/inactive 间横跳；加上设计 VHH 后 inactive mode 被彻底消除

### Scaling Law
对比 Promera / AF3 / Boltz-2 / Protenix-v1 / SeedFold 五个模型，co-folding 性能可用 log(data) + log(compute) 线性解释（R²=0.969, p=0.03）。每提升 10% 归一化性能需 ~80% 更多 distillation 数据或 ~110% 训练算力 —— 开源与 AF3 的差距是数据/算力问题，非架构问题。

## 意义
将折叠·筛选·生成三个既往分离的环节统一进一套权重，每一环节均达或接近开源 SOTA。BindCraft、BoltzGen、RFdiffusion3 等方法需重新解释自身差异化价值。
