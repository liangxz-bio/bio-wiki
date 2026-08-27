---
title: Nature Methods（IF=28.3）｜谁说单序列 PLM 到头了？VESM 让 ESM 家族互相教学，性能大幅跃升
created: 2026-08-27
updated: 2026-08-27
type: article
tags: [wechat-article, Unknown]
domain: general
sources: [raw/articles/Nature-Methods-IF-28-3-谁说单序列-PLM-到头了-VESM-让-ESM-家族互相教学-性能大幅跃升.md]
confidence: medium
---

# Nature Methods（IF=28.3）｜谁说单序列 PLM 到头了？VESM 让 ESM 家族互相教学，性能大幅跃升

> 来源：微信公众号 · Unknown
> 原文链接：[微信文章](https://mp.weixin.qq.com/s/k6Dt84BKAXbfUvLvefoHiQ)

预测一个错义变异是否会影响蛋白质功能，最直接简单的方法就是从序列出发。
近年来，蛋白语言模型（protein language model, PLM）已经成为变异效应预测（variant effect prediction, VEP） 的重要技术路线。与依赖多序列比对、蛋白结构或人群遗传信息的方法相比，单序列 PLM 不需要额外构建复杂输入，因而具有更好的通用性和部署便利性。然而，在多个 VEP benchmark 中，仅使用原始未比对序列的模型通常仍落后于融合 MSA、结构或群体遗传信息的混合模型。
这项性能差距背后可能存在一个容易被忽视的问题：即使属于同一个 ESM 家族，不同模型所捕获的进化约束也并不完全一致。某个模型未能识别一个功能敏感区域，并不意味着整个模型家族都遗漏了这一信息。相反，不同 ESM 模型可能在不同蛋白区域形成互补的预测盲区，因此模型之间的差异本身就可能成为一种可以利用的知识来源。
加州大学旧金山分校与加州大学洛杉矶分校的研究团队据此提出 VESM。其核心思路并不是简单地增加模型规模或引入新的生物学模态，而是首先从多个 ESM 模型中提取每个变异上最强的进化约束信号，再将这些模型集合所包含的互补知识重新蒸馏回单个模型。
因此，这篇工作的意义并不只是再刷新一次 benchmark 排名。它更重要的贡献在于提出了一条可以被检验和推广的路线：当同一家族 PLM 的误差模式并不完全重合时，可以先利用模型集合构建一个更强的教师信号，再将这种“集体知识”压缩回一个能够独立部署的学生模型。

01
PAPER INFO

####
论文信息
####

英文标题
Compressing the collective knowledge of ESM into a single protein language model

作者
Tuan Dinh, Seon-Kyeong Jang, Noah Zaitlen, Vasilis Ntranos

通讯作者
Vasilis Ntranos

通讯单位
Department of Epidemiology and Biostatistics / Institute for Human Genetics / Diabetes Center / Bakar Computational Health Sciences Institute, University of California, San Francisco

期刊 / 日期
Nature Methods，2026 年 4 月，23 卷，772–784 页；2026 年 3 月 30 日在线发表

DOI / 论文
10.1038/s41592-026-03050-9

Code
https://github.com/ntranoslab/vesm

02
OVERVIEW & ABSTRACT

####
核心概括与摘要
####

一句话概括
作者用多模型的最小 log-likelihood ratio（LLR，表示野生型相对突变氨基酸的模型偏好）构造每个错义变异的教学信号，再以 co-distillation 更新 ESM；最终得到的 VESM-3B 在所用 ClinVar、ProteinGym DMS 和 UK Biobank 汇总统计分析中表现出较强的变异效应预测能力。
摘要翻译
背景
蛋白语言模型已经成为下一代 VEP 的重要候选方法。当前许多高性能 VEP 模型会进一步结合蛋白同源性、多序列比对、三维结构和人群遗传信息，以提高预测准确性。相比之下，仅在未比对原始蛋白序列上训练的纯 PLM，例如 evolutionary scale modeling（ESM）模型，虽然更加简洁、通用，也更容易部署，但其预测性能通常被认为存在明显上限
技术方案
作者对这一观点进行了重新检验，并提出一种高效的 co-distillation 框架。该方法不向模型加入新的结构、MSA 或人群遗传数据，而是从同一家族的多个 ESM 模型中提取每个变异上置信度最高的预测，并以这些信号指导其他模型更新。换言之，不同模型会根据具体变异交替承担“教师”和“学生”的角色，从而使模型家族在预训练阶段已经获得、但分散在不同成员中的进化信息得到重新整合。
结果
作者报告，仅依赖 ESM 家族内部知识进行 co-distillation，就能够显著提高不同规模 ESM 模型的 VEP 表现，并在多个临床和 DMS benchmark 中达到或超过多种现有方法。在此基础上，作者进一步将模型用于 UK Biobank 的 Genebass 汇总统计，评估预测分数与连续临床表型效应量之间的关系，从而将模型应用从传统的良性/致病二分类扩展到定量变异效应分...

**完整内容**：见 [raw/articles/Nature-Methods-IF-28-3-谁说单序列-PLM-到头了-VESM-让-ESM-家族互相教学-性能大幅跃升.md](Nature-Methods-IF-28-3-谁说单序列-PLM-到头了-VESM-让-ESM-家族互相教学-性能大幅跃升.md)
