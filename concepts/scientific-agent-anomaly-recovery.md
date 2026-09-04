---
title: 科学 Agent 的异常恢复能力 — 从 DynaMate 看 MD 自动化
created: 2026-09-04
updated: 2026-09-04
type: concept
tags: [ai-agent, molecular-dynamics, self-correction, multi-agent, scientific-evaluation, dynamate]
domain: ai-drug-discovery
sources: [raw/articles/DynaMate-AI-Agent蛋白配体MD模拟-ResearchSquare2026.md]
confidence: high
---

# 科学 Agent 的异常恢复能力 — 从 DynaMate 看 MD 自动化

> 来源：[raw/articles/DynaMate-AI-Agent蛋白配体MD模拟-ResearchSquare2026.md](raw/articles/DynaMate-AI-Agent蛋白配体MD模拟-ResearchSquare2026.md)（Research Square 2026 预印本，EPFL Schwaller 组）

## 核心问题：MD 自动化卡在"准备和排错"而非"运行命令"

MD 模拟进入生产阶段前还要经历结构清理、末端处理、质子化、配体参数化、溶剂化、加离子、能量最小化、NVT/NPT 平衡。**输入文件、化学状态或软件约定只要有一处不一致，后续步骤就可能停止。**

- 固定脚本只知道"下一步运行什么"，并不知道失败的 tLEaP 或 GROMACS 报错究竟来自配体电荷、原子命名、封端冲突还是文件缺失
- **DynaMate 所针对的正是这类异常状态**：让 LLM Agent 读取错误、检查文件、查询资料并决定如何重新执行
- 实验设计专门加入了已知会触发错误的体系，而非只在"干净输入"上比较运行速度

## 智能体自动化的关键能力

1. **行动轨迹（action trace）记忆**：工具调用是否成功、错误信息、输出摘要都记录下来
2. **原始计划持续写入上下文**：标记已完成和仍待执行的子任务
3. **周期性压缩对话历史**：同时保留最近两条 assistant message
4. **自我纠错反馈环**：软件报错 → 错误上下文返回 LLM → 重新判断 → 编辑文件或调整动作 → 再执行
5. **RAG 补充体系特异信息**：PaperQA 在 90 篇领域论文中检索温度、pH、组氨酸或配体质子化状态、结晶条件、tLEaP/ParmEd/GROMACS 报错解释

## Agent 评价方法学

| 评价维度 | 含义 |
|----------|------|
| **工作流成功率** | "已正确生成的必要非空文件 / 完整工作流应生成的必要文件"的比例 |
| **效率** | 工具调用次数（无错误时至少 7 次蛋白、8 次蛋白-配体） |

**关键边界**：**工作流成功 ≠ 轨迹收敛**——文件正确生成说明软件流程可执行，但不能自动保证采样充分。

## 关键经验

- 在**正常输入**上，五种 LLM 差异很小（Claude/GPT 系列 100% 成功率）
- 在**故障输入**上，模型差距明显（GPT-5.5 100% vs Llama 3.3 0%）
- **GPT-5.5 在正常体系上反而更频繁读取/列出文件**——推测它对环境状态进行了更深入检查

**对其他科学 Agent 的启示**：任务完成率不足以体现科学执行能力，应主动构造底层工具会失败的场景，记录失败分母、恢复动作和最终是否回到可执行路径。

## 相关页面

- [[entities/dynamate]] — DynaMate 实体页
- [[raw/articles/DynaMate-AI-Agent蛋白配体MD模拟-ResearchSquare2026]] — 来源原文
