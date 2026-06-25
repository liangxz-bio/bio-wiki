---
title: "SeFMol — 强化学习引导的半柔性分子扩散生成"
created: 2026-05-18
updated: 2026-05-19
type: entity
tags: [diffusion-model, reinforcement-learning, SBDD, molecular-generation, drug-design, AI-drug-discovery]
domain: ai-drug-discovery
sources:
  - raw/articles/AI造药新范式-口袋生长分子-BOAT-ProteomeScan-SeFMol-TarPass.md
  - raw/articles/SeFMol-强化学习引导半柔性分子扩散模型-SciAdv2026.md
confidence: medium
---

> 来源: *Steering semi-flexible molecular diffusion model for structure-based drug design with reinforcement learning*. Sci Adv. doi:10.1126/sciadv.ady9955.

## 核心发现链

SeFMol 将强化学习嵌入扩散模型的去噪过程，实现分子在蛋白口袋中的"边生长边拟合"。传统方法先生成再对接（先造船再塞瓶），SeFMol 让分子在生成过程中动态调整构象以适应口袋几何。^[raw/articles/SeFMol-强化学习引导半柔性分子扩散模型-SciAdv2026.md]

核心思路：静态配体建模在计算方法中占主导地位，但真实分子相互作用本质上是动态的。受配体在半柔性对接中发生构象变化的启发，SeFMol 将去噪过程定义为马尔可夫决策过程（MDP），RL 通过迭代探索动态调整分子结构。^[raw/articles/SeFMol-强化学习引导半柔性分子扩散模型-SciAdv2026.md]

## 三阶段架构

1. **预训练阶段**：从 Molecule3D 数据集中选 100 万无靶点分子，计算 8 个分子属性（QED/SA/LogP/TPSA/HBA/HBD/Fsp³/ROTB），作为条件嵌入约束去噪策略，赋予模型属性偏差 ^[raw/articles/SeFMol-强化学习引导半柔性分子扩散模型-SciAdv2026.md]
2. **微调阶段**：用 CrossDocked2020 中 10 万蛋白-配体对，结合口袋几何空间和相互作用信息继续噪声注入/去除 ^[raw/articles/SeFMol-强化学习引导半柔性分子扩散模型-SciAdv2026.md]
3. **半柔性优化阶段（RL）**：去噪步建模为 MDP。参考去噪器（参数冻结）保持基础能力，策略去噪器（微调）执行当前动作，两侧预测位置用 KL 散度约束，防止 RL 产生化学不合理结构（如五配位碳）^[raw/articles/SeFMol-强化学习引导半柔性分子扩散模型-SciAdv2026.md]

**快速采样策略**：采样步数从 1000 步减少到 50 步（20x 加速），生成质量不降。平均采样时间 0.81s/分子，生成成功率 98.3%。^[raw/articles/SeFMol-强化学习引导半柔性分子扩散模型-SciAdv2026.md]

## 关键证据摘要

- **结合亲和力**：Vina 评分 -7.23 kcal/mol，比参考分子高 13.7%，全面优于 8 个基线模型（AR/ResGen/Pocket2Mol/FLAG + TargetDiff/DecompDiff/MolCRAFT/IPDiff）^[raw/articles/SeFMol-强化学习引导半柔性分子扩散模型-SciAdv2026.md]
- **药物样属性**：QED/SA/Lipinski 规则均最优，分子量相当时提高 Vina 能量和 LBE ^[raw/articles/SeFMol-强化学习引导半柔性分子扩散模型-SciAdv2026.md]
- **生成成功率**：11.53%，比 IPDiff（3.44%）高出 8.09% ^[raw/articles/SeFMol-强化学习引导半柔性分子扩散模型-SciAdv2026.md]
- **构象可靠性**：直接生成构象 vs 重新对接后构象相关性 r = 0.95 ^[raw/articles/SeFMol-强化学习引导半柔性分子扩散模型-SciAdv2026.md]
- **多样性**：略低于 TargetDiff，高于 Pocket2Mol 和 DecompDiff ^[raw/articles/SeFMol-强化学习引导半柔性分子扩散模型-SciAdv2026.md]
- **真实靶点验证**：CDK2 和 ROCK1，能重现已知相互作用 + 发现新骨架（如 ROCK1 卤键/盐桥）^[raw/articles/SeFMol-强化学习引导半柔性分子扩散模型-SciAdv2026.md]

## 与基线对比

| 指标 | SeFMol | TargetDiff | DecompDiff | Pocket2Mol | IPDiff |
|------|--------|------------|------------|------------|--------|
| Vina 评分 (kcal/mol) | -7.23 | — | — | — | — |
| 生成成功率 | 11.53% | — | — | — | 3.44% |
| 采样时间 (s) | 0.81 | — | — | — | — |
| QED | 最高 | — | — | — | — |
| 多样性 | 中 | 高 | 低 | 低 | — |

## 交叉引用
- [[entities/softmol]] — 同为分子生成模型（块扩散分子生成），可对比方法学
- [[entities/de-novo-protein-design]] — 蛋白/分子设计全景背景
- [[entities/boltz2]] — 虚拟筛选与结构预测工具，可与 SeFMol 生成分子配合使用
- [[entities/tarpass]] — TarPass 靶点感知分子生成评测基准，SeFMol 可纳入未来评估
- [[entities/boat]] — BOAT 抗体多目标贝叶斯优化，同为 RL 驱动的分子优化框架

## 局限
- 来源为微信公众号二次解读，原始方法学细节需读原文确认
- 多样性略低于 TargetDiff，提示生成分子的骨架多样性有改善空间
