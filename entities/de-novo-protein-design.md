---
title: "David Baker Nature 2026 综述：蛋白质从头设计（de novo protein design）"
created: 2026-05-13
updated: 2026-05-13
type: entity
tags: [review, protein-design, de-novo, ai-drug-discovery, immunology]
domain: ai-drug-discovery
sources:
  - raw/articles/David-Baker-Nature综述-蛋白质从头设计的过去现在与未来.md
  - raw/articles/RFdiffusion-蛋白质从头设计-Nature论文详解.md
  - raw/papers/watson2023-rfdiffusion-nature.md
  - raw/articles/AI蛋白设计实验验证方法综述-CurrOpinStructBiol2026.md
  - raw/papers/kosonocky2026-ai-protein-design-cosb.md
  - raw/articles/RFdiffusion1-架构原理训练上手指南-AIinBio.md
  - raw/articles/RFdiffusion-SE3扩散框架详解-Nature2023-Bits&Bases.md
  - raw/articles/RFdiffusion五大设计任务详解-Nature2023-Bits&Bases.md
  - raw/articles/ProtDBench-蛋白Binder设计统一评测基准-arXiv2026.md
  - raw/papers/prihoda2025-ovo-biorxiv.md
confidence: high
---

> 来源: Yang W, Wang S, Lee GR, Zhang JZ, Baker D. The past, present and future of de novo protein design. Nature. 2026. | 微信公众号 BrainMed Lab 中文解读

## 核心发现链

从头蛋白设计正在经历从**物理化学模型→机器学习生成模型**的范式转换。折叠/组装/结合三大基础挑战**已基本解决**；催化设计显著进展但仍集中在低能垒反应；真正前沿是**多状态动态纳米机器**——整合靶标结合→构象变化→催化反应的蛋白系统。

## 关键证据摘要

### 方法学转折
- 2020年前：Rosetta 物理能量模型（力场+序列采样+能量排序），代表作 Top7（首次证明全新折叠可被计算设计并实验验证）
- 2020年后：深度学习生成模型 — **RFdiffusion**（扩散模型生成蛋白骨架）、**ProteinMPNN**（固定骨架序列设计）、AlphaFold/RoseTTAFold（结构预测替代能量排序）
- 关键认识转变：物理模型追求**低能量**（单序列-结构对），概率模型追求**高概率**（隐式竞争结构排除）— 两者不等价

### 实验验证全景（Curr Opin Struct Biol 2026 综述补充）
Curr Opin Struct Biol 2026 综述从实验验证视角补充了 Baker Nature 2026 综述未系统覆盖的方法学格局：
- **binder 设计最成熟**：RFdiffusion/BindCraft/AlphaProteo/BoltzGen 等方法已在 PD-L1/IL-7RA/TrkA/VEGF-A/SC2RBD/MCL1/GABARAP 等靶点达到可比较的实验成功率（十到几十个百分点），进入系统比较阶段
- **antibody 设计快速升温但更复杂**：AbDiffuser/BoltzGen/RFantibody/IgGM 等方法已实现 subnanomolar affinity 和双位数成功率，但框架稳定性/免疫原性/developability 仍需筛选和反馈
- **enzyme 设计最难**：RFdiffusion2/RFdiffusion3/PLACER/RiffDiff 已做出 luciferase/serine hydrolase/zinc metallohydrolase/Cas9 等活性酶，但高度依赖特定 assay/底物/反应体系，远未到广泛通用
- **筛选策略关键**：很多成功并非模型一步到位，而依赖 AF2 过滤/多序列采样/人工检查等筛选链路

### 原始论文增补（Kosonocky et al., Curr Opin Struct Biol 2026）
- 第一/通讯作者来自 **Microsoft Research**（Ava P. Amini, Kevin K. Yang）+ UT Austin，非 Baker 体系独立视角
- 核心贡献：将 AI 蛋白设计分解为 **data curation → model training → sample generation → filtering → experimental validation** 完整 pipeline
- Table 1-2：binder 设计方法系统比较（实验验证方法/靶点/成功率），涵盖 RFdiffusion/BindCraft/AlphaProteo/EvoBind2/BoltzGen/Chai-2/LatentX/PXDesign/RFpeptides
- Table 3：antibody/nanobody 设计方法（AbDiffuser/RFantibody/BoltzGen/Chai-2/GeoFlowV3/Germinal/IgGM/mBER），subnanomolar affinity 和双位数成功率
- Table 4：enzyme 设计方法（RFdiffusion2/RFdiffusion3/PLACER/RiffDiff/ProGen2/ZymCTRL/ProtGPT2），luciferase/serine hydrolase/zinc metallohydrolase/Cas9 等
- 核心结论：**实验反馈是 AI 蛋白设计真正的裁判**，模型本身只占 pipeline 一环

### 已基本解决的挑战
| 领域 | 水平 | 代表性结果 |
|------|------|-----------|
| 蛋白折叠设计 | 单体→跨膜→非天然骨架（D-aa/大环肽） | Top7, TIM barrel, 跨膜β桶孔道 |
| 蛋白组装体 | 对称纳米颗粒→准对称→1D/2D/3D材料 | SKYCovione（首个临床获批从头设计蛋白——新冠疫苗） |
| 蛋白结合物 | 200+实验验证，靶向病毒/毒素/癌症/免疫靶点 | 蛇毒毒素皮摩尔级结合物，TNF受体结合物 |
| 小分子结合 | 药物海绵/传感器/化学诱导二聚化 | 地高辛/甲氨蝶呤/芬太尼传感器，设计纳米孔 |

### 取得进展但仍需突破的挑战
| 领域 | 当前水平 | 瓶颈 |
|------|---------|------|
| 酶设计 | kcat/KM 可达 10^4-10^5 M^-1s^-1 | 高能垒反应、低活化程度底物仍需突破；RFdiffusion2 + PLACER 提高预组织但未全面解决 |
| 多状态系统 | LOCKR 布尔逻辑、铰链开关、轴-转子 | 机械耦合、化学燃料驱动、循环构象切换 |
| 蛋白-核酸设计 | RFdiffusion→DNA/RNA结合蛋白 | 仍在早期，替代锌指/TALE/CRISPR需进一步验证 |

### 治疗应用进展
- **抗病毒**：鼻喷剂型设计蛋白（流感/SARS-CoV-2/MERS-CoV），动物模型预防+治疗，高热稳定性免冷链
- **抗毒素**：蛇毒/艰难梭菌毒素保护，稳定性优势适合发展中国家
- **受体调控**：IL-2类似物已进入临床；人工受体亚基二联体产生新信号输出
- **转运/降解**：EndoTags驱动溶酶体靶向清除；PepPrCLIP E3连接酶靶向降解
- **免疫原性**：初步数据提示高稳定性/高溶解性设计蛋白可能免疫原性较低，但需系统评价

### 展望
- 未来5-10年：复杂蛋白纳米机器和功能材料有望超越自然进化产物
- 风险边界：免疫原性、规模化生产、新技术误用可能性、知识产权边界
|- EvoBind2 — binder 序列进化优化

## 警告与局限

> RFdiffusion 处于"统计性成功"阶段，尚未达到"确定性设计"境界。

基于 RFdiffusion 冷思考的总结 ^[raw/articles/RFdiffusion-爆款论文冷思考-潜力与局限.md]：

1. **成功率仍低** — 对称寡聚体 ~14.3%，结合蛋白 ~19%，实际是"高通量筛选辅助工具"而非"确定性生成器"
2. **AF2 依赖陷阱** — RFdiffusion 基于 RoseTTAFold 微调，验证却用 AF2（训练数据高度重叠），存在模型同源性偏差
3. **骨架-序列解耦假设脆弱** — 扩散模型可能生成几何好看但能量上不稳定的骨架（疏水核心未包好）
4. **静态结构验证局限** — 功能通常依赖动态构象，但验证只看静态 RMSD 和结合力
5. **训练数据边界** — 完全超出自然进化拓扑空间时可能产生"幻觉结构"

## 交叉引用

- [[entities/diamond-deepclust]] — DeepClust 数据库（190 亿序列）可增强 AlphaFold2 结构预测，为从头蛋白质设计提供更丰富的序列多样性
- [[entities/gps-ai-drug-platform]] — GPS 从转录表型驱动药物发现，可为从头设计的蛋白/多肽药物提供表型筛选验证

## 来源与交叉引用
- [[nextgen-vaccine-strategies]] — 疫苗设计四大策略，SKYCovione 为结构抗原工程+纳米颗粒的成功案例
- [[mmseqs2]] — 同源搜索用于蛋白家族筛选，可整合进蛋白设计管线
- [[deeprare]] — AI智能体范式对比（蛋白设计 vs 临床诊断）
- [[entities/pplm]] — PPLM 蛋白配对语言模型，蛋白对表示可为 binder 设计中 PPI 评估提供底座

## 相关工具
### 平台/生态系统
- [[ovo]] — Ovo 开源蛋白设计生态系统，Nextflow 编排 + 存储层 + Web GUI，整合 RFdiffusion/ProteinMPNN/AF2/BindCraft 管线
- [[entities/ori-protein-design]] — ORI 腾讯 AI4S 闭环蛋白设计框架（本体提示生成 + USM 筛选 + RLWF 实验反馈优化），Three G 三酶体系验证（Nat Commun 2026）
### 结构生成
- RFdiffusion / RFdiffusion2 / RFdiffusion3 — 扩散模型生成蛋白骨架，RFdiffusion2/3 扩展至酶活性中心设计
- Chroma — 蛋白骨架生成
- ProteinMPNN — 固定骨架序列设计
### 跨膜蛋白与GPCR设计
- [[entities/gem-gpcr-modulator]] — GEMs 从头设计 GPCR 跨膜区变构调节蛋白，RFdiffusion+MPNN+AF2 迭代 + 结构提示词（Nature 2025 浙江大学）
### 结合物与抗体设计
- BindCraft — 基于梯度幻觉的结合物设计
- AlphaProteo — Google DeepMind 蛋白结合物生成
- BoltzGen / LatentX / Chai-2 — binder 生成
- AbDiffuser / GeoFlowV3 / Germinal / IgGM / mBER — 抗体/nanobody 扩散生成
- [[entities/dual-gpt-ab]] — DualGPT-AB 双阶段 GPT+RL 抗体设计，多性质约束内置 + 湿实验验证（HER2, Nat Comput Sci 2026）
- [[entities/highfold]] — HighFold 系列环肽结构预测工具（CycPOEM/ncAA/Cyclization 开关）
- RFantibody — RFdiffusion 适配抗体框架
- RFpeptides / HELM-GPT — 环肽设计
- [[raw/articles/计算驱动靶向肽药物设计-物理建模到生成式AI-InnovDrugDiscov2026]] — 靶向肽药物设计综述：生成→逆折叠→预测→ADMET→物理验证闭环框架，PepMLM/EvoDiff/HighFold/PepFlow/LigandMPNN工具全景（Hu W, Innov Drug Discov 2026）
### 酶设计
- PLACER — 活性中心预组织构象集合生成
- RiffDiff — 酶活性位点支架设计
- ProGen2 / ZymCTRL / ProtGPT2 — 蛋白语言模型用于酶设计
### 多状态系统
- LOCKR — 多状态蛋白逻辑开关
- EvoBind2 — binder 序列进化优化
