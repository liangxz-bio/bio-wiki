---
title: "AnewDDE — Anew Labs 闭环智能体药物研发引擎"
created: 2026-09-17
updated: 2026-09-17
type: entity
tags: [ai-drug-discovery, ai4s, ai-agent, agentic-drug-discovery, anewdde, anew-labs, bytedance, structure-prediction, affinity-prediction, nanobody, lab-in-the-loop, pharmbench, closed-loop]
domain: ai-drug-discovery
sources:
  - raw/articles/AnewDDE-AnewLabs-闭环智能体-AI制药-BioTender.md
confidence: medium
---

> 来源: Anew Labs. AnewDDE (Technical Report, 52 pages). 2026-09-16. 原始报告未公开获取；本页内容基于微信公众号·BioTender 的解读转述，所有性能数字未经独立核验。公司背景：2021 年字节跳动内部孵化，2026-06 拆分独立（总部上海，另设新加坡/圣何塞），2026-09-16 完成 2.9 亿美元首轮融资，投后估值约 15 亿美元，字节保留 56% 控股。

## 核心定位

AnewDDE 不是单个模型，而是把结构预测、亲和力评估、分子设计与科学推理决策装进同一个能消化湿实验反馈的闭环智能体。报告把当前 AI 制药的痛点命名为"决策摩擦"（decision friction）：单点工具各自很强（AF3 的结构、Boltz-2 的秒级亲和力、扩散模型生成 binder），但碎片化预测需要科学家手工拼合，再凭经验决定哪些候选进湿实验。

四模块分工：AnewFold 看结构（含口袋识别）、AnewAffinity 算亲和力、AnewDesign 做设计、AnewMind 管推理决策。

## 关键指标（全部为解读转述，未独立核验）

| 模块 | 任务 | 结果 | 对照 |
|------|------|------|------|
| AnewFold | 抗体-抗原 top-1 成功率（FoldBench-AbAg） | 76.2% | AF3 48.8%、Boltz-1 34.4%、IsoDDE 75.6% |
| AnewFold | 蛋白-配体共折叠 | 77.4% | 相似度过滤后的新 benchmark |
| AnewFold | 分子胶三元复合物 | 71.8% | 同上 |
| AnewFold | 口袋识别 AUPRC / 隐式口袋子集 | 0.751 / 0.663 | 隐式口袋（apo 闭合、holo 张开）仍是弱项 |
| AnewAffinity | 单配体对耗时 | 约 1.5 s | Boltz-2 约 20 s；精度相当的 AnewFEP 约 33000 s（差约 2.2 万倍） |
| AnewAffinity | ΔG 相关性 | Pearson R² 0.553 / Spearman 0.724 | Boltz-2 0.486 / 0.620 |
| AnewAffinity | BACE 转化 ΔΔG | 预测 +1.12 kcal/mol | 实测 +1.33 kcal/mol |
| AnewDesign | 湿实验命中 | 150 克隆中 16 个达个位数 nM，成功率 10.7%，最好 KD 1.8 nM，最高提升 100 倍 | Carterra LSA XT 多浓度 SPR 实测 |
| AnewMind | PharmBench 总分 | 54.65%，17 个模型中第三 | 落后 GPT-5.6-sol；落后 Kimi K3 0.23 pp |
| AnewMind | 分子设计与合成维度 | 60.83%，第一 | — |
| AnewMind | ADMET 四项评测 | 全部进前五（环肽膜通透性第一、三个 CYP 抑制终点最高估计值） | Elo 排名 |
| AnewMind | 通用能力 | MMLU-Pro 80.8% / GPQA Diamond 88.4% | 领域后训练未造成明显偏科 |

AnewAffinity 的机制思路：把 FEP（自由能微扰）的严格物理缝进分布预测框架——不输出单一数值，而输出整条自由能分布并逐窗口自检不确定性。原文自陈：八个 JACS 体系上没有方法通吃，部分靶点 Boltz-2 更好，因此逐窗口不确定性估计（告诉使用者哪条预测该信、哪条该打回重算）比单点精度更重要。

## 湿实验闭环（Lab-in-the-Loop）

1. 科学家在浏览器里用自然语言下指令（例：为某靶点设计 VHH 结合物）
2. 智能体自主查文献与专利、检索结构数据库、圈定候选表位
3. 云端拉起 64 个 worker 跑生成
4. 过滤：3092 个初筛候选 → 界面评分 + 成药性风险 motif + 序列聚类 → 287 个簇 → 最终推荐 50 条序列进湿实验
5. 第一轮：7/50 克隆 SPR KD < 400 nM（命中率 14%）
6. 实测数据回喂智能体 → 启动 in silico 亲和力成熟 → 再提出 100 个新克隆
7. 两轮累计：150 克隆中 16 个达个位数 nM，最好 KD 1.8 nM

原文对这一段的定性：证明的是"设计—测试—学习"循环可由智能体驱动，而非某个靶点的成药性突破（靶点被匿名化，且只是单靶点概念验证）。

## AnewMind 与 PharmBench

- AnewMind：千亿参数规模、经全参数后训练的科学推理 LLM
- PharmBench：团队自建基准，50 个真实研发场景 / 200 道递进式问题，全部由内部药企研发专家出题，覆盖七大能力维度
- 报告自陈：总分只排第三，说明通用前沿模型的进化速度不会停下来等任何垂直模型

## 边界与解读说明

本页内容源是微信公众号对 52 页技术报告的二次解读，不是报告原文（报告无公开下载地址；官网 anewbt.com 的 Research 列表截至 2026-09-17 最新条目为 2026-03 的 AnewSampling）。因此：

- 所有性能数字（76.2% / 1.5 s / 54.65% 等）转述自解读文章，未独立核验，confidence 标 medium
- PharmBench 是内部基准，外界无法复现；题目的区分度无法第三方检验
- 纳米抗体案例只有一个匿名靶点，属单点概念验证，不能外推为通用成功率
- 四个模型（AnewFold/AnewAffinity/AnewDesign/AnewMind）全部闭源，无 API
- 模块命名待核验：文中的 AnewFold / AnewAffinity / AnewDesign 与官网现有平台条目（AnewSampling / AnewOmni / AnewMind）不一致，可能是报告中改名或整合
- 相对可信的部分：公司分拆与融资（Reuters 2026-09-16，2.9 亿美元，投后约 15 亿美元，字节保留 56%）、IL-17 口服小分子管线（AAI 2026 公开）有独立报道可查

## 交叉引用

- [[isodde]] — 同一 FoldBench-AbAg 基准上的正面对照：AnewFold 76.2% vs IsoDDE 75.6%（自报数字，均无第三方独立评测）；IsoDDE 走的也是"技术报告 + 内部引擎"披露路径
- [[boltz2]] — AnewAffinity 在速度-精度版图上直接对照的基线（1.5 s vs 20 s；R² 0.553 vs 0.486）；原文同时承认部分靶点 Boltz-2 更好
- [[alphafold3]] — 抗体-抗原 48.8% 基线来源，与 AnewFold 的 76.2% 构成新一代预测器的对照
- [[protenix-v2]] — 同源团队血缘：字节跳动的蛋白结构预测团队（Protenix/Seedfold 一线）已并入刘凯带的 AI 制药团队，AnewDDE 是这条线的下游产物
- [[dynamate]] — 同为科学 Agent 自动化，但 DynaMate 面向 MD 模拟流程的异常恢复，AnewDDE 面向药物设计闭环的决策
- [[scientific-agent-anomaly-recovery]] — Agent 评价应主动构造失败场景；AnewDDE 的边界（内部基准、单匿名靶点）正是同一类问题
- [[claude-science-ai4s-design-philosophy]] — AI4S Agent 的架构与上下文工程视角，可用于审视 AnewDDE 的智能体设计
- [[raw/articles/AnewDDE-AnewLabs-闭环智能体-AI制药-BioTender.md]] — 公众号解读原文（入库来源）
- [[raw/articles/晶泰科技-XtalPiScience-GeniusAgents-科学智能开放生态联盟]] — 国产 AI4S 闭环平台对照（XtalPi Science 的"数字假设—专业预测—物理验证—数据反馈"闭环）
- [[raw/articles/NISE-哈佛团队零样本设计小分子结合蛋白-Nature2026]] — Lab-in-the-loop 迭代式结合蛋白设计的学术侧对照
- [[raw/articles/Protenix-v2-抗体设计-结构预测-药物发现]] — 同血缘团队的 zero-shot 抗体设计（GPCR 靶点 16-88% VHH hit rate）
