---
title: Bowtie2 比对算法
created: 2026-05-08
updated: 2026-05-08
type: concept
tags: [alignment, mapping, bwt, fm-index, bowtie2, ngs, host-depletion]
domain: bioinfo-pipeline
algorithms: [bowtie2]
confidence: high
---

# Bowtie2 比对算法

将测序 reads 比对到参考基因组，用于去宿主（human read filtering）和产物比对（product/病原 mapping）。InnoPAP tNGS/mNGS pipeline 中作为核心比对引擎。

## 核心算法

**BWT (Burrows-Wheeler Transform) + FM-index**
- 参考基因组经过 BWT 变换 + FM-index 构建，实现 O(L) 级别的快速搜索（L = read 长度）
- 支持 **gapped alignment**（区别于 Bowtie1 的无 gap），可处理 indel

**质量感知回溯（quality-aware backtracking）**
- 结合 Phred quality score 指导回溯路径
- 低质量碱基允许更多 mismatch，高质量碱基严格匹配

**FM-index 的双向搜索**
- 同时在 BWT 正反两个方向搜索，加速敏感比对

## 比对模式

### 1. end-to-end（端到端）
全 read 从头到尾比对，不允许 soft-clipping

```
Read:   ACGTACGTACGTACGT
Ref:    ACGTACGTACGTACGT
        ^^^^^^^^^^^^^^^^   全部匹配
```

- **更严格**，适合全基因组鸟枪法（mNGS）
- InnoPAP 中 mNGS 去宿主和产物比对都用此模式

### 2. local（局部）
允许 read 两端 soft-clipping，只保留匹配良好的中间片段

```
Read:   xxACGTACGTACGTxx
Ref:       ACGTACGTACGT
        ^^^^^^^^^^^^^^^^   两端 clips，中间对齐
```

- **更灵敏**，适合扩增子（tNGS）：引物区质量差，但中间靶向区能比对
- InnoPAP 中 tNGS 去宿主使用此模式

## Pipeline 中的实际配置

### 去宿主比对（human read filtering）

| 参数 | tNGS | mNGS SE | mNGS PE |
|------|:----:|:-------:|:-------:|
| 模式 | `--local` | `--end-to-end` | `--end-to-end` |
| 输入 | `-U` (SE) | `-U` (SE) | `-1`/`-2` + `--no-mixed` |
| 线程 | 16 | 16 | 16 |
| 参考 | T2T 人类基因组 | T2T 人类基因组 | T2T 人类基因组 |
| 输出 | mapHuman.bam | mapHuman.bam | mapHuman.bam |

**为什么 tNGS 用 local 而 mNGS 用 end-to-end？**
tNGS 是扩增子文库，read 两端包含引物序列，local 模式可 soft-clip 掉引物区，让中间的靶序列更好比对到人类基因组进行过滤；mNGS 是随机打断，read 全长都是基因组片段，end-to-end 更合适。

### 产物比对（product mapping）— tNGS only

| 参数 | 值 |
|------|:---:|
| 模式 | `--end-to-end` |
| 报告 | `-k 1`（每个 read 只报 1 个最优比对） |
| 输入 | `-U` (SE) |

## 关键参数

| 参数 | 你在用的值 | 说明 |
|------|:----------:|------|
| `--local` / `--end-to-end` | 见上表 | 比对模式 |
| `-k 1` | 产物比对 | 每个 read 只报告 1 个比对位置 |
| `--no-mixed` | mNGS PE | 双端不比配时不做单端补救 |
| `-U` / `-1 -2` | 见上表 | SE 或 PE 输入 |
| mismatch 阈值 | 5 (tNGS) | 传给下游 primer_mapInfo 做过滤，不是 bowtie2 本身的参数 |

## 设计思路

去宿主比对和产物比对使用同一套 pipeline 逻辑：
1. **去宿主**（比对到人类 T2T 基因组）— 滤掉人类 reads，保留非人类 reads
2. **产物比对**（比对到病原数据库）— 鉴定 reads 来自哪种病原
3. **引物比对**（比对到引物序列库）— tNGS 特有，鉴定扩增子对应的靶标

## 关联

- [[codebase-index]] — InnoPAP tNGS/mNGS pipeline 中 bowtie2 的实际脚本
- [[tngs-vs-mngs]] — 两种方法比对策略差异的背景
- [[kingcreate-tngs-panel]] — tNGS 引物/探针体系
