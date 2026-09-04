---
title: Claude Science 五条设计哲学与 AI4S Agent 架构
created: 2026-09-04
updated: 2026-09-04
type: concept
tags: [ai4s, ai-agent, claude-science, anthropic, skill-design, artifact, anti-hallucination, context-management, calibration]
domain: ai4s
sources: [raw/articles/Claude-Science五条设计哲学-AI4S-Agent.md]
confidence: medium
---

# Claude Science 五条设计哲学与 AI4S Agent 架构

> 来源：[raw/articles/Claude-Science五条设计哲学-AI4S-Agent.md](raw/articles/Claude-Science五条设计哲学-AI4S-Agent.md)（微信公众号文章解读，2026）

非技术 AI 从业者 + 科研训练 + 商业/产品视角解读 Anthropic Claude Science 五条设计哲学。

## 五条设计哲学

### 1. AI4S Agent 比起博士生更像科学仪器

Claude Science 把每个约束都附带**测量依据和适用边界**，让模型理解"为什么"，从而在边界之外自行判断。

- 三个关键词：**校准（calibration）、理解而非命令（understanding over instruction）、边界（boundary）**
- skill-creator 提示词："ALWAYS或NEVER视作黄旗"
- 不鼓励科学家下武断判断："今天的LLM很聪明，给了好的harness就能超越死记硬背，真正做成事情"

### 2. 配置调优记录或实验日志

多数 Agent 系统的 metadata 是"屎山"：

```
enable_thinking: false
skills_locked: true
max_tool_result_chars: 262144
```

工程师离职后没人敢动——为什么 thinking 关了？262144 怎么来的？

Claude Science 每个配置项讲三件事：**为什么这么设、用什么实验数据支撑、改动它会触发什么**。

**实测案例**：bench-reviewer 6 reps，thinking 占 ~72% 输出 token，但追溯类任务召回率几乎无变化（opus-nothink 0.94±0.02 vs baseline 0.895）。

### 3. Artifact-first：科研交付物必须可引用、可溯源

> Produce artifacts, not just answers. Whenever your work produces user-facing outputs (figures, tables, reports, structure files), call save_artifacts before moving on.
> —— operon/working_style

- workspace 是 Agent 的草稿纸，只有显式保存的才进入用户视野
- 每个产物有 `{{artifact:VERSION_ID}}`——身份（artifact_id）+ 版本（version_id）
- 底层数据库建**血缘 DAG**：
  - `artifact_versions`：checksum、environment_snapshot、lineage
  - `artifact_dependencies`：产物间依赖 DAG
  - `producing_cell_id`：直接指向 execution_log

### 4. 从架构上反幻觉

GPT-3 时代 LLM 经常编数据和文献。Claude Science 双管齐下：

**数据库**：bio-tools 内置约 **63 个权威数据源客户端**（UniProt、PDB、AlphaFold、Ensembl、ClinVar、gnomAD、ChEMBL、PubMed、Rfam、GTEx、KEGG、Reactome、STRING…），聚合成 **24 个领域化 MCP server**。

**行为规范**：operon working_style 两条硬规矩：
- "**Compute, don't confabulate**"——能算的必须算，不许猜数
- MCP 数据源调用只能在 repl 里以循环形式发，一个 cell 一个逻辑步，配 inline assert 当场校验
- 能力本身也要 grounding：用某个技能前先 `search_skills` 确认它真存在

### 5. 渐进式暴露上下文

29 个技能、约 90 个数据源，全塞进上下文会撑爆。Claude Science 分三级：

| 层级 | 内容 | 加载时机 |
|------|------|----------|
| **Metadata** | name + description（约100词） | 永远在上下文 |
| **SKILL.md 正文** | 理想 <500 行 | 只在技能触发时 |
| **Bundled 资源** | 无限大 | 按需读取；scripts/ 可直接执行 |

- 常驻成本：29 × 100 词 = **2900 词**
- description 写得主动（pushy）防欠触发

## 结论：AI4S Agent 的护城河

- 生物是 LLM 比较好切入的场景：生物科学非常大数据、有大量基于文本的逻辑涌现
- 物理、化学、材料学、地质等其他领域规则更明确、逻辑链条更长，对 Agent 能力要求更高
- 谁能构建出后面领域的 Agent 架构，就会有深不可测的护城河

## 边界与解读说明

- 本页内容来源是微信公众号对 Anthropic Claude Science 的解读，**非 Claude Science 原始论文/官方文档**
- 引用的具体数字（72% token、63 数据源、24 MCP server、29 技能、90 数据源、6 reps 等）均来自该解读文章，未独立核验 Anthropic 原始材料
- 解读视角："非技术AI行业从业者+科研训练+商业/产品视角"，带有作者主观解读

## 相关页面

- [[raw/articles/晶泰科技-XtalPiScience-GeniusAgents-科学智能开放生态联盟]] — XtalPi Science 科学智能体平台（AI4S 工程实践）
- [[raw/articles/Claude-Science五条设计哲学-AI4S-Agent]] — 来源原文
