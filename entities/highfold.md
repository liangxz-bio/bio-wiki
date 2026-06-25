---
title: "HighFold / HighFold2 / HighFold3 — 环肽结构预测工具家族"
created: 2026-05-18
updated: 2026-05-18
type: entity
tags: [cyclic-peptide, structure-prediction, alphafold, non-natural-amino-acid, drug-design, ai-drug-discovery]
domain: ai-drug-discovery
sources:
  - raw/articles/HighFold-环肽结构预测三部曲迭代.md
confidence: medium
---

> 来源: Hongliang Duan Lab. HighFold / HighFold2 / HighFold3 series. GitHub: hongliangduan/HighFold.

## 核心发现链

HighFold 系列是针对环肽结构预测的三代迭代工具，构建于 AlphaFold 之上，核心创新在于将环肽特有的拓扑约束显式编码进模型输入。逐步解决环肽结构预测的四大难点：闭环拓扑 → 二硫键配对 → 非天然氨基酸表示 → 线性/环化切换。

## 关键证据摘要

1.  HighFold（v1）— 建立环肽拓扑框架
    - 底座：AlphaFold-Multimer
    - 核心创新：**CycPOEM**（环肽位置编码矩阵），用拓扑最短路径替代线性序列距离
        - head-to-tail 闭环、二硫键、肽键方向全部编码进矩阵
        - 多半胱氨酸时枚举所有配对方案，按置信度排序选最优
    - 分支：HighFold_Monomer（环肽单体）+ HighFold_Multimer（环肽复合物）
        - 复合物中受体用线性编码，环肽配体用 CycPOEM
    - 性能：天然环肽单体中位 RMSD 1.058 Å，平均 1.478 Å（63 样本中 37 个 <1.5 Å）
    - 边界：仅支持天然氨基酸

2.  HighFold2 — 扩展到非天然氨基酸
    - 底座：AlphaFold-Multimer
    - 三层改造：
        - 原子级特征模块：同时看残基级/原子级/键信息，区分不同非天然氨基酸
        - 扩充刚体分组和坐标系：为 23 种常见非天然氨基酸生成端到端原子坐标
        - 沿用 CycPOEM 位置编码
    - 训练策略：先在 382 个线性肽样本（覆盖 23 种 ncAA）微调，再改位置编码到环肽
    - 后处理：结构松弛（Amber 力场参数 + 能量最小化，清理原子碰撞）
    - 消融：去掉 CycPOEM 影响最大（环肽被预测成线性）；去掉原子特征后二硫键位置偏

3.  HighFold3 — 接入 AlphaFold 3 + 线性/环化切换
    - 底座：AlphaFold 3
    - 核心改动：
        - 保留 CycPOEM
        - **Cyclization 开关**：同一条序列可分别按线性或环化预测
        - 预训练参数冻结，只改输入特征
        - 复合物中受体/环肽配体分开编码
        - 支持同一受体多环肽配体共建模
        - 接入 CCD（Chemical Component Dictionary），ncAA 覆盖从 23 种扩到所有 CCD 已知类型
    - 子模型：HighFold3-Linear + HighFold3-Cyclic
    - 性能：head-to-tail 闭环成功率 **HighFold3-Cyclic 100% vs AF3 21%**
    - 边界：主要做静态结构预测

4.  三代对比

    | 版本 | 核心改进 | 解决的问题 |
    |------|---------|-----------|
    | HighFold | CycPOEM + 二硫键枚举 | "这是环肽" |
    | HighFold2 | 原子级特征 + 23种 ncAA | "这个残基是什么" |
    | HighFold3 | AF3 底座 + Cyclization 开关 + CCD | "按线性还是环化来算" |

## 局限与未公开
- 三代均面向静态结构预测，不覆盖构象动态
- HighFold2 仅覆盖 23 种常见 ncAA，非全覆盖
- HighFold3 虽接入 CCD，但非天然氨基酸结构的预测精度仍需更多验证
- 来源为微信公众号二次解读（非原始论文），参数细节需查阅 GitHub 和原始论文

## 交叉引用
- [[entities/alphafold3]] — HighFold3 构建于 AF3 之上，Cyclization 开关解决了 AF3 对环肽拓扑感知不足的问题
- [[entities/de-novo-protein-design]] — 蛋白结构预测方法全景，HighFold 系列是环肽子领域的专用工具
- [[raw/articles/计算驱动靶向肽药物设计-物理建模到生成式AI-InnovDrugDiscov2026]] — 靶向肽药物设计综述，HighFold 被列为环肽结构预测首选工具
