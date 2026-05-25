---
title: 新一代病毒疫苗设计策略（2025 Review）
created: 2026-05-09
updated: 2026-05-09
type: concept
tags: [immunology, vaccine, review, vaccine-strategy]
domain: vaccine-design
sources: [raw/papers/yuan2025-nextgen-vaccines-vaccines.md]
confidence: high
---

# 新一代病毒疫苗设计策略

> 来源: Yuan F, Bluth MH. Vaccines 2025. doi:10.3390/vaccines13090979

## 核心发现链

新一代病毒疫苗设计的四大前沿方向: (1) **结构指导的抗原工程** — 通过稳定融合前构象、引入突变和糖基化屏蔽来优化免疫原; (2) **mRNA 平台 + LNP 递送** — 自扩增 mRNA (saRNA) 和环状 RNA (circRNA) 作为下一代 RNA 疫苗平台, 结合脂质纳米颗粒递送; (3) **先进佐剂系统** — 靶向特定 PRR (STING/TLR/CLR) 以增强细胞和体液免疫; (4) **免疫印记 (immune imprinting) 应对** — 通过嵌合抗原、glycan shielding、COBRA 逻辑、B 细胞克隆操控来克服变异株的免疫印记。

## 关键证据摘要

- **结构工程**: 融合前稳定的 RSV F 蛋白和 SARS-CoV-2 S-2P/HexaPro 突变是成功案例 (2M-2P 突变稳定 Spike 融合前构象)^[raw/papers/yuan2025-nextgen-vaccines-vaccines.md]
- **mRNA 平台**有三大形式: 传统线性 mRNA (最成熟)、saRNA (低剂量, 含 RdRp 自复制)、circRNA (共价闭环, 抗降解, 成本更低; 但环化效率和安全数据不足)^[raw/papers/yuan2025-nextgen-vaccines-vaccines.md]
- **LNP 四组分**: 可电离脂质 + 辅助脂质 + PEG 化脂质 + 胆固醇; 关键挑战是靶向递送至特定器官/细胞类型 (下一代 LNP 通过调整脂质比例和表面修饰实现)^[raw/papers/yuan2025-nextgen-vaccines-vaccines.md]
- **免疫印记**: 重复加强针对变异株的 Spike 疫苗 → 记忆 B 细胞主导的 anti-Wuhan-hu-1 抗体, 对变异株中和活性低; 应对策略包括嵌合 HA 抗原 (head 替换 + stem 保守)、glycan shielding (mask 免疫优势表位)、COBRA (系统发育保守表位筛选)^[raw/papers/yuan2025-nextgen-vaccines-vaccines.md]
- **抗独特型 (anti-idiotypic) 疫苗**: 利用抗体 F(ab) 区域作为免疫原, 产生模拟病原抗原的 anti-idiotypic 抗体, 诱导 anti-anti-idiotypic 反应 (HIV-1 gp120 模型已验证)^[raw/papers/yuan2025-nextgen-vaccines-vaccines.md]
- **动物模型局限**: 传统小鼠模型不能充分预测人类免疫应答; 人源化小鼠和 NHP 模型仍是 gold standard^[raw/papers/yuan2025-nextgen-vaccines-vaccines.md]
- **佐剂分类**: (1) TLR 激动剂 (CpG/MPLA/R848/imiquimod); (2) CLR 激动剂 (TDB/Curlian/GLA); (3) STING 激动剂 (cGAMP / DMXAA / MSA-2); (4) 其他 (皂苷 QS-21, Alum, 弗氏佐剂, MF59/AS03 油乳剂)^[raw/papers/yuan2025-nextgen-vaccines-vaccines.md]

## 对你疫苗设计管线的影响

| 策略方向 | 你的管线中对应步骤 |
|---------|----------------|
| 结构指导的抗原稳定 (S-2P/HexaPro) | Step 17: 三级结构建模 - 可借鉴 2M-2P 突变稳定融合前构象 |
| adjvuant 选择 (STING/TLR/CLR) | Step 14: 佐剂串联设计 - 此论文总结了 4 大类佐剂结构 |
| chimeric antigen + COBRA | Step 9: 保守性分析 - COBRA 逻辑可复用 |
| glycan shielding | 可内置为 Step 17 的可选策略 |
| immune imprinting 评估 | 验证环节: 多表位疫苗 vs 自然感染的免疫印记分析 |
| anti-idiotypic 疫苗平台 | 独立管线, 可选择性地在你的设计软件中预留接口 |

## 产业转化案例
- 科医诺生物 KNY0003 管线：徐可团队原研广谱流感疫苗，与步长制药合作推进临床开发，基于抗原理性再设计策略 ^[raw/articles/科医诺生物-携手步长制药-共拓广谱流感疫苗.md]
- Ca-DelMut 减毒株：S蛋白 S1/S2 弗林切割位点缺失→病毒减毒，兼具强复制能力+不致病性→鼻喷减毒活疫苗候选 ^[raw/articles/Ca-DelMut新冠S蛋白缺失减毒株-NatureComms.md]

## 交叉引用

- [[raw/papers/yuan2025-nextgen-vaccines-vaccines.md]] — Yuan F, Bluth MH. Vaccines 2025
- [[universal-intranasal-vaccine-gla-3m052]] — GLA-3M-052-LS+OVA 鼻喷广谱疫苗，整合器官免疫范式，超越传统抗原特异性框架（Zhang H, Science 2026）
- [[span-vaccine]] — S_pan 广谱新冠疫苗：基于 S 蛋白进化轨迹的系统发育共识抗原设计，跨进化支保护 WT/Beta/Delta/Omicron（Zhao Y/K Xu, Sci Transl Med 2023）
- [[entities/tcr-prp]]
- `virus-vaccine-pipeline` (to be created) — 你的病毒疫苗设计全流程管线
