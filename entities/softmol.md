---
title: "SoftMol — 块扩散分子生成（Soft Block Diffusion）"
created: 2026-05-16
updated: 2026-05-16
type: entity
tags: [molecular-generation, drug-design, diffusion-model, fragment-based, ai-drug-discovery]
domain: ai-drug-discovery
sources:
  - raw/articles/AI筛药新工具-Boltz2-SoftMol-AINN-P1-DeepClust.md
confidence: medium
---

# SoftMol

## 核心发现链

SoftMol 提出**块扩散（Soft Block Diffusion, SoftBD）**分子生成新范式——从传统的"逐 token 拼写"分子 SMILES 切换到"软片段搭积木"。SoftBD 架构：块内双向纠错 + 块间顺序搭建，实现 100% 有效性，采样速度比传统扩散模型快 6.6 倍。集成 MCTS 目标导向生成 + 可行性门预筛选（QED/SA 打分）。5 个靶点刷新纪录，top 5% 对接分数提升 9.7%，新化学骨架多 2-3 倍。

## 关键证据

- 块扩散架构：100% 有效性，采样速度 6.6x 加速 ^[raw/articles/AI筛药新工具-Boltz2-SoftMol-AINN-P1-DeepClust.md]
- MCTS 目标导向生成 + QED/SA 可行性门
- 精选 ZINC-Curated 数据集（4.27 亿分子），类药性 94%
- 5 靶点 top 5% 对接分数提升 9.7%
- 新化学骨架多样性多 2-3 倍

## 交叉引用
- [[entities/sefmoll]] — SeFMol RL+扩散模型分子生成（口袋内生长），与 SoftMol 块扩散策略互补
- [[entities/boltz2]] — Boltz-2 共折叠虚拟筛选，SoftMol 生成的分子可用 Boltz-2 验证
- [[entities/de-novo-protein-design]] — 分子生成/蛋白设计的互补工具，SoftMol 提供小分子配体端

## 局限

- 来源仅为微信公众号摘要，原始方法需进一步查阅
- 块划分策略的通用性尚未验证（不同分子类型是否共享同一片段库）
