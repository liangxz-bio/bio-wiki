---
title: DynaMate — 蛋白-配体 MD 自动化多智能体框架
created: 2026-09-04
updated: 2026-09-04
type: entity
tags: [molecular-dynamics, ai-agent, self-correction, mm-pbsa, multi-agent, gromacs, amber, protodpo]
domain: ai-drug-discovery
sources: [raw/articles/DynaMate-AI-Agent蛋白配体MD模拟-ResearchSquare2026.md]
confidence: high
---

# DynaMate — 蛋白-配体 MD 自动化多智能体框架

Masschelein C, Guilbert S, Goumaz J, Naida B, Rothlisberger U, Schwaller P. *DynaMate: An Autonomous Agent for Protein-Ligand Molecular Dynamics Simulations*. Research Square (2026) preprint v1. doi: 10.21203/rs.3.rs-10243410/v1.

EPFL + NCCR Catalysis（Schwaller 组）多智能体 MD 自动化框架，专门针对**异常恢复**而非单纯命令执行。

## 核心架构：三个 Agent 分工

| Agent | 角色 | 职责 |
|-------|------|------|
| **PrepAgent** | 规划 | 读取自然语言需求、获取 PDB 结构、判断是否含配体、生成带文件路径和任务顺序的模拟计划 |
| **MDAgent** | 执行 | 接收计划，进入工具调用循环：蛋白/配体拆分、质子化、力场参数化、建系、最小化、平衡、生产模拟；失败时可读取/编辑相关文件再重新调用工具 |
| **AnalyzerAgent** | 解释 | 后处理：检查平衡阶段温度/密度/能量，分析 RMSD/RMSF/回转半径/氢键；流程未完成时汇总失败步骤、修复尝试与后续排查建议 |

## MD 协议

- 蛋白：AMBER ff14SB 力场
- 配体：Open Babel 在 pH 7.0 质子化 → AmberTools/antechamber + GAFF2 参数化
- 体系：TIP3P 水模型 + Na⁺/Cl⁻ 中和
- 模拟：GROMACS 2023，积分步长 2 fs，LINCS 约束，PME 长程静电
- 默认流程：100 ps NVT + 100 ps NPT 平衡 → NPT 生产阶段
- 体系特异信息：PaperQA 在 90 篇领域论文中 RAG 检索

## 关键结果

### 15 体系 × 5 模型 × 5 重复

- **10 个未预设故障体系**：Claude 和 GPT 系列均达 100% 成功率，7-13 次工具调用
- **5 个故意选入的故障体系**：GPT-5.5 全部 100%；Claude/GPT-5.4 mini 随错误类型下降；Llama 3.3 一个都没完成
- **GPT-5.5 在故障体系恢复能力最强**

### 827 任务规模化测试（PLINDER）

| 任务类型 | 总数 | 成功数 | 成功率 |
|----------|------|--------|--------|
| 蛋白-配体 | 589 | 365 | 62% |
| 仅蛋白 | 238 | 188 | 79% |
| **合计** | **827** | **553** | **~67%** |

**确定性对照**（相同底层工具、不给 LLM 错误恢复能力）：
- 蛋白-配体：301→365，额外恢复 **+10.9 个百分点**（64 个）
- 仅蛋白：118→188，增加 **+29.4 个百分点**（70 个）

**67% 的边界**：已排除多配体、金属配位、共价配体、缺失残基等更复杂体系。

### MM/PBSA 与对接打分对比

BRD4 BD1 10 个同系列抑制剂：

| 方法 | 与实验 IC₅₀ 相关系数 r |
|------|------------------------|
| DynaMate MM/PBSA ΔΔG | **0.597** |
| GNINA docking score | 0.385 |

**注**：单靶点、小样本，MM/PBSA 是端点近似方法，限制了外推范围。

## 三类典型错误与修复

| 错误类型 | 案例 | 修复条件 |
|----------|------|----------|
| **末端封端冲突** | 1U6Q_745, 1C5S_ESX | 找到封端残基中的 H1/H2/H3 并删除冲突氢原子 |
| **配体质子化错误** | 2RBQ_263（叠氮基）、1C5S_ESX（脒基） | 识别末端多余 H 并删除，或恢复合理电荷态 |
| **原子命名不一致** | 3SQ6_EPJ（PDB 写"CL"，力场用"Cl"） | 修改 PDB 或力场相关文件 |

共同点：软件日志没有直接给出最终修复动作——Agent 必须把报错映射到结构文件、化学价态或力场约定。

## 能力边界

1. **基础模型差异**：Agent 框架不抹平模型差距，故障体系上差距明显
2. **工具上限**：当前面向单一配体体系，未覆盖多配体、核酸和膜蛋白
3. **质子化建议**：组氨酸/天冬氨酸/谷氨酸及可滴定配体基团需用户体系特异核查
4. **无持久记忆**：每个新任务独立开始，之前成功解决过的错误不沉淀
5. **API 限制**：外部 LLM API 带来访问费用、调用频率限制和数据处理要求

## 启发

- DynaMate 更值得借鉴的是对**Agent 评价问题**的处理：正常输入上的任务完成率不足以体现科学执行能力
- 应主动构造底层工具会失败的场景，记录失败分母、恢复动作和最终是否回到可执行路径
- 模块之间的接口错误和失败恢复，可能与单个模型的 benchmark 分数同样重要

## 代码与数据

- GitHub: https://github.com/schwallergroup/DynaMate

## 相关页面

- [[raw/articles/DynaMate-AI-Agent蛋白配体MD模拟-ResearchSquare2026]] — 来源原文（中文解读）
- [[raw/articles/GOLLuM-大模型不确定性校准优化-NatureMachineIntelligence2026]] — 同一团队 Nature MI 论文
- [[concepts/bayesian-optimization]] — Bayesian Optimization（采集函数与 GP 框架）
