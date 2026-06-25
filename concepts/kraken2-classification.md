---
title: Kraken2 + Bracken 物种分类与丰度估计
created: 2026-05-08
updated: 2026-05-08
type: concept
tags: [classification, taxonomony, k-mer, lca, kraken, bracken, mngs, microbiome]
domain: bioinfo-pipeline
algorithms: [kraken2, bracken, krakenuniq]
confidence: high
---

# Kraken2 + Bracken 物种分类与丰度估计

将测序 reads 快速分类到物种/属/株水平，并基于分类结果重新估计相对丰度。InnoPAP mNGS pipeline 的核心分类引擎。

## 流程总览

```
去宿主后的 reads (unmapHuman)
    ↓
Kraken2 — 基于 k-mer 精确匹配 + LCA 分类
    ↓ 原始分类输出 (raw.out) + 报告 (raw.report)
Kraken Filter — 保留 classified reads，去除人类误分类
    ↓ 过滤后报告 (report)
Bracken — Bayesian 重新估计各层级丰度
    ↓ species / genus / strain 三个层级
下游验证 — BLAST + Bowtie2 ref genome 确认
```

## Kraken2 — 快速物种分类

### 核心算法

**k-mer 精确匹配 + LCA (Lowest Common Ancestor)**

1. **建库**：将参考基因组中所有 k-mer（默认 k=35）映射到其来源物种的 LCA
   - 如果一个 k-mer 存在于多个物种，则其分类赋值为这些物种的最近共同祖先
   - 例如 k-mer 同时出现在 E. coli 和 S. flexneri → 赋值为 Enterobacteriaceae 科
2. **分类**：对每条 read，提取所有 k-mer，查数据库，统计每个分类单元的命中数
3. **打分**：选择覆盖 read 得分最高的分类路径
   - 得分 = 该分类单元及其子节点的命中 k-mer 数 / read 总 k-mer 数
   - 只有得分 ≥ `--confidence` 阈值的分类才会被接受

**关键数据结构和改进**

| 版本 | 核心改进 | 内存 |
|------|---------|:----:|
| Kraken1 | 标准 k-mer + 哈希表 | ~70GB (标准库) |
| Kraken2 | **spaced seeds (minimizers)** — 只索引部分 k-mer 代表序列，极大减小数据库大小 | ~50GB |
| | 使用 **compact hash table** 替代原哈希表 | |

### 评分机制：confidence 参数

`--confidence 0.5`（你的默认值）

```
Read: ACGTACGTACGTACGTACGT
       ^^^^^^  ↑  ↑  ↑
       k-mers  |  |  |
               v  v  v
       E.coli: 8/12 k-mers match → score=0.67 ≥ 0.5 → classified as E.coli
       S.flex: 3/12 k-mers match → score=0.25 < 0.5 → rejected
```

- **高 confidence（如 0.5）**：减少假阳性，但可能增加未分类 reads
- **低 confidence（如 0.1）**：更多 reads 被分类，但假阳性上升
- **`--confidence 0`**：只要有一个 k-mer 命中就接受，最灵敏但最不严格

### pipeline 中的实际命令

```bash
# mNGS SE
numactl --interleave=all kraken2 --db {kraken_db} --confidence 0.5 \
  --output {raw.out} --report {raw.report} --threads 16 \
  --gzip-compressed {unmapHuman.fq.gz}

# mNGS PE
numactl --interleave=all kraken2 --db {kraken_db} --confidence 0.5 \
  --output {raw.out} --report {raw.report} --threads 16 \
  --gzip-compressed --paired {R1.fq.gz} {R2.fq.gz}
```

**注意**: `numactl --interleave=all` 是 v1.2.00 加的优化，解决双路 CPU 的 NUMA 内存瓶颈问题（Kraken2 内存需求大，单节点内存可能不够）。

## Kraken Filter — 去除人类残留

Kraken2 原始输出中，被分类到 `Homo sapiens` (taxid 9606) 和 `Homo sapiens neanderthalensis` (9605) 的 reads 会被过滤掉，只保留 `$1=="C"` (classified) 且非人类的 reads，再用 `krakenuniq-report` 重新生成报告。

```bash
# sub.mNGS_kraken_flt.sh
cat $kraken_raw_out | awk '$1=="C" && $3!="9606" && $3!="9605"' > $kraken_out
krakenuniq-report --db {krakenuniq_db} $kraken_out > $report
```

## Bracken — 丰度重新估计

### 为什么需要 Bracken？

Kraken2 的问题是：**一个 k-mer 可能属于多个物种**（LCA 赋值为更高的分类层级）。导致：
- 属水平的 read 没有被正确分配到下属的种
- 物种丰度被低估，属丰度偏高

### Bracken 的 Bayesian 重估

1. 使用已知的**基因组大小**和**k-mer 分布**，预先计算各物种之间 k-mer 重叠概率
2. 将 Kraken2 报告中每个分类层级的 reads，按概率重新分配到更低层级
3. 最终输出：校正后的物种/属/株水平 reads 数和丰度

### pipeline 中的配置

```bash
# species 级别
bracken -d {db} -i {kraken_report} -o {species_out} -w {species_report} \
  -r {read_length} -l S -t {threshold}

# genus 级别
bracken -d {db} -i {kraken_report} -o {genus_out} -w {genus_report} \
  -r {read_length} -l G -t {threshold}

# strain 级别
bracken -d {db} -i {kraken_report} -o {strain_out} -w {strain_report} \
  -r {read_length} -l S1 -t {threshold}
```

**三个层级并行输出**：species (S)、genus (G)、strain (S1)

**NTC 特殊处理**：-t 1（任何 1 条 read 就报），临床样本用正常阈值（RPM ≥ 3）

## 参数速查

| 参数 | 你的值 | 说明 |
|------|:------:|------|
| **Kraken2** | | |
| `--confidence` | 0.5 | 最低分类置信度 |
| `--threads` | 16 | 线程数 |
| `--gzip-compressed` | ✓ | 输入为 gz 压缩 |
| `--paired` | mNGS PE | 双端输入 |
| **Bracken** | | |
| `-l` | S / G / S1 | 分类层级 |
| `-r` | 从样本表读取 | read 长度 |
| `-t` | RPM 阈值 (default 3) | 最低 reads 数门槛，NTC 强制 = 1 |

## Kraken2 vs KrakenUniq

你的 pipeline 同时用了：
- **Kraken2**：实际分类引擎，速度快、内存优化好
- **KrakenUniq**：只用于 `krakenuniq-report` 生成最终报告（KrakenUniq 的报告格式支持 unique k-mer count，可进一步评估分类可信度）

## 设计思路

```
Kraken2 分类（敏感性高，特异性中等）
    ↓ filter 去掉人类残留（特异性提升）
    ↓ Bracken 重新分配丰度（精度提升）
    ↓ BLAST 验证 + ref genome 比对（金标准确认）
```

四层过滤确保最终结果准确：Kraken2 做第一遍分类，filter 去人类，Bracken 校正丰度，最后 BLAST 和 ref genome 比对确认。

## 关联

- [[codebase-index]] — InnoPAP mNGS pipeline 中 Kraken2/Bracken 的 Snakemake rules
- [[tngs-vs-mngs]] — mNGS 分类策略与其他方法的对比
- [[kingcreate-kcq]] — 绝对定量内参系统，可与 Kraken2/Bracken 相对定量互补
- [[bowtie2-alignment]] — 去宿主比对的前置步骤
- [[raw/articles/宏基因组-Kraken-RTL路径打分算法解析]] — 深入理解 Kraken 原始 RTL (Root To Leaf) 路径打分算法，与 Kraken2 --confidence 评分机制对比的底层原理
