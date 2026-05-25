---
title: qmNGS — 定量宏基因组测序 (internal DNA standards + Yseq)
created: 2026-05-04
updated: 2026-05-04
type: entity
tags: [method, quantification, internal-standard, mngs, argenv, environmental]
sources: [raw/papers/li2021-qmngs-aem.md]
confidence: high
---

# qmNGS — 定量宏基因组测序

Li B, Li X, Yan T (University of Hawaii at Manoa / University of Nebraska-Lincoln), 2021, *Appl Environ Microbiol*.

环境微生物领域的定量宏基因组方法，通过掺入**异源合成内参片段（ISF）** 实现高通量基因绝对定量，在抗生素抗性基因（ARG）检测中验证。

## 核心概念：Sequencing Yield (Yseq)

qmNGS 的关键参数 **Yseq**（测序产率，无量纲）连接 掺入质量比 和 测序碱基比：

```
Yseq-i = (C_ISF-i / C_TOT) / (n_ISF-i / n_TOT)

C_ISF-i: ISF 掺入浓度 (ng/uL)
C_TOT:   样本总 DNA 浓度 (ng/uL)
n_ISF-i: ISF 检测到的总碱基数 (bp)
n_TOT:   样本总测序碱基数 (bp)
```

理论值 Yseq = 1（IS 和样本 DNA 行为一致）。实际值 ~**0.55**（波动范围 0.48-0.56），稳定不随掺入浓度和测序深度变化。

### 定量流程
1. 掺入 IS → 宏基因组测序
2. BLASTN 识别 ISF → 计算各 ISF 的 Yseq-i
3. 取所有 ISF 的 Yseq 均值（LOQ以上）
4. 用 Yseq 代入目标基因公式：
   ```
   C_target / C_TOT * Yseq = n_target / n_TOT
   ```
5. 转为拷贝数：
   ```
   C_target-GC = (C_target * NA) / (L_target * 1e9 * 650)
   ```

## 内参设计

| 参数 | 值 |
|---|---|
| ISF 数量 | 20 个合成片段（3个block: IS-1/-2/-3） |
| 分析单元 | 79 个（包含单个 + 连续组合） |
| 总插入长度 | IS-1: 758bp / IS-2: 1509bp / IS-3: 1732bp |
| 单片段长度 | 94-1732bp |
| GC 含量 | 26.8%-63.8% |
| 合成标记 | 3个连续终止密码子 (TAGTAATGA) 框内插入 |
| 载体 | pUCIDT (IDT) |
| 掺入质量比范围 | 1.8×10⁻⁸ ~ 1.8×10⁻³ ng/ng |
| 基因来源 | 16S rRNA + intI1 + 18种常见 ARG |

### IS 设计原理
- 3个终止密码子插入使 ISF 完全**异源**（自然界不存在），避免环境样本的同源干扰
- 同一 block 上的 ISF 天然等拷贝，不同 block 可用不同浓度
- 连续 ISF 可合并分析，增加统计自由度

## 方法性能

| 指标 | 值 |
|---|---|
| 线性（r²） | **0.98**（spiked mass ratio vs detected sequence base ratio） |
| LOQ | 7.8×10⁻⁷ ng/ng |
| LOD | 1.0×10⁻⁷ ng/ng |
| LOQ以上 Yseq CV | 16.2% (SD 4.9%) |
| Yseq 均值 | 0.55 (E. coli mix) / 0.53 (manure) |
| 样本间 Yseq 差异 | CV 3.9%（两种复杂度样本间） |
| 单 ISF Yseq 差异 | ANOVA P=0.08（无显著差异） |

## 测序与生信流程

| 环节 | 详情 |
|---|---|
| 测序平台 | Illumina HiSeq 4000, 150bp PE |
| 数据量 | ~3.1 Gb/sample |
| 质控 | Trimmomatic (slidingwindow:4:20 leading:10 trailing:10 minlen:50) |
| ISF识别 | BLASTN vs ISF local DB, E-value<1e-5, bit score>50, identity>90% |
| ARG检测 | BLASTX vs CARD DB, bit score>50, identity>90% |
| 16S检测 | METAXA2 (default) |
| 分析工具 | Jupyter + Python (scipy, pandas, numpy, seaborn) + R (ggplot2) |

## 测序深度影响

| 深度 (×10⁸ bp) | LOQ (ng/ng) | Yseq |
|---|---|---|
| 62 (原始) | 7.8×10⁻⁷ | 0.55 |
| 30 | 2.7×10⁻⁶ | 0.49 |
| 15 | 5.4×10⁻⁵ | 0.48 |
| 7.5 | 5.0×10⁻⁶ | 0.53 |
| 3.75 | 5.4×10⁻⁵ | 0.52 |
| 1.88 | 5.4×10⁻⁵ | 0.50 |
| 0.94 | 2.0×10⁻⁴ | 0.26 (显著下降) |

**结论**: 测序深度影响 LOQ 但不影响 Yseq（除非极低深度 <1.88×10⁸ bp）。

## qPCR 验证结果

| 基因 | qmNGS vs qPCR |
|---|---|
| 16S rRNA | 无显著差异 (paired t test P=0.56) |
| tetO, sulI, tetM, ermB | 无显著差异 |
| qnrS | 均未检出 |
| **qmNGS CV** | **6.7% (SD 4.4%)** |
| **qPCR CV** | **25.8% (SD 10.3%)** — qmNGS 变异更小 |

## 优势与局限

**优势：**
- 无需先验知识，高通量（一次测序 200+ ARG）
- 绝对定量（不是相对丰度）
- 不受 PCR 扩增偏倚影响（无 PCR 步骤）
- 变异比 qPCR 小
- Yseq 在不同样本类型间稳定

**局限：**
- 灵敏度低于 qPCR（LOQ ~10³·⁵ copies/mL vs qPCR 10¹-10⁴）
- 仅验证环境样本（E. coli mix, manure）
- 未验证临床样本
- 内参在建库前加入，不校正提取效率
- 需要较深测序量才能达到低 LOQ
- SRA 编号疑似有误（SRR1139176-1801 可能不完整）

## 与 KCQ 的对比
- **相同点**: 都用合成 DNA 内参做绝对定量，都用线性回归模型
- **本质差异**: qmNGS 是基因水平（靶向功能基因如 ARG），KCQ 是微生物水平（靶向物种）
- **内参设计**: qmNGS 用终止密码子标记，非自然来源；KCQ 用无同源合成序列
- **掺入时机**: 都是建库前加入，不校正提取效率
- **验证场景**: qmNGS 环境样本 vs KCQ 临床样本

## 数据公开
- SRA: SRR1139176-SRR11391801
- PMID: 34085862
- PMCID: PMC8373253
- 基金: USDA NIFA 2017-68003-26497

## 相关页面
- [[kingcreate-tngs-panel]] — 金圻睿tNGS产品
- 对比 KCQ 与其他定量方法
