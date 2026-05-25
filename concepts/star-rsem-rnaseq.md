---
title: STAR + RSEM 转录组比对与定量
created: 2026-05-08
updated: 2026-05-08
type: concept
tags: [alignment, rnaseq, quantification, transcriptome, star, rsem]
domain: bioinfo-pipeline
algorithms: [star, rsem]
confidence: high
---

# STAR + RSEM 转录组比对与定量

将 RNA-seq 双端 reads 比对到基因组和转录组，并进行基因/转录本水平的定量。transcriptome pipeline 的核心。

## 流程

```
fastp QC → STAR 比对 → RSEM 定量 → EBSeq 差异表达
                      ↓               ↓
                  BAM + counts    gene/isoform FPKM
```

## STAR — 剪接感知比对

### 核心算法

**种子-延伸（Seed-and-Extend）+ 剪接感知（Splice-aware）**

1. **建索引**：基于 **suffix array**（不是 BWT），支持高压缩比的基因组索引
2. **种子搜索**：将 read 拆成 `--seedSearchLstart` 长度的种子，在基因组找匹配位置
3. **种子聚类**：将比到相近位置的种子聚集成候选比对区域
4. **延伸**：在候选区域内用 **Needleman-Wunsch/Smith-Waterman** 局部对齐延伸，找到最优比对
5. **剪接识别**：如果 read 跨过外显子-外显子接头，STAR 通过检测种子之间的 gap 来识别剪接位点
   - 不需要剪接位点数据库（de novo 识别）
   - 同时对比对已知剪接位点（GT-AG 信号）

**STAR 相比其他 RNA-seq 比对器的优势：**

| 特性 | STAR | HISAT2 | TopHat2 |
|------|:----:|:------:|:-------:|
| 比对速度 | 极快 | 快 | 慢 |
| de novo 剪接位点 | ✓ | ✓ | ✓ |
| 内存占用 | 较高 (~30GB) | 低 (~5GB) | 中等 |
| 双端比对精度 | 高 | 高 | 中等 |

## Pipeline 中的 STAR 配置

```bash
STAR --genomeDir $index                    # 预构建基因组索引
     --runThreadN 20                       # 20 线程
     --readFilesCommand zcat               # gz 输入
     --readFilesIn {R1}.clean.fq.gz {R2}.clean.fq.gz  # PE
     --outFileNamePrefix {sample}_         # 输出前缀
     --outSAMtype BAM SortedByCoordinate   # 按坐标排序的 BAM
     --quantMode TranscriptomeSAM GeneCounts  # 关键参数
     --outSAMattributes Standard           # SAM 标签
```

### `--quantMode` 双输出

| 模式 | 输出 | 用途 |
|------|------|------|
| `TranscriptomeSAM` | `*_Aligned.toTranscriptome.out.bam` | 比对到转录组的 BAM，**直接输入 RSEM** |
| `GeneCounts` | `*_ReadsPerGene.out.tab` | 每基因 read count 表，可直接用于差异表达分析 |

**设计思路**：一次 STAR 比对同时产出两种数据，避免跑两次。

## RSEM — 转录本定量

### 核心算法

**期望最大化（EM）估计转录本表达量**

1. **问题**：一条 read 可能比对上多个转录本（同一基因的不同 isoform 或同源基因），无法简单分配
2. **解法**：使用 **EM (Expectation-Maximization) 算法**：
   - **E 步**：基于当前表达量估计，计算每条 read 来自每个转录本的概率
   - **M 步**：基于 read 分配概率，重新估计每个转录本的表达量
   - 迭代直到收敛
3. **输出**：
   - `*.genes.results` — 基因水平 FPKM/TPM
   - `*.isoforms.results` — 转录本水平 FPKM/TPM
   - `*.model` — 比对模型参数

### RSEM 与 featureCounts/htseq-count 的区别

| 工具 | 方法 | 多转录本分配 | 输出 |
|------|------|:----------:|------|
| **RSEM** | EM + 转录本模型 | ✓ 概率分配 | FPKM/TPM + 计数 |
| featureCounts | 简单计数 | ✗ 丢弃多匹配 | raw count |
| htseq-count | 简单计数 | ✗ 丢弃多匹配 | raw count |

RSEM 的优势在于**处理同一基因多个 isoform 的 read 分配问题**。

### Pipeline 中的 RSEM 配置

```bash
rsem-calculate-expression \
    --paired-end \                          # PE 模式
    --no-bam-output \                       # 不输出 BAM（STAR 已有）
    --alignments \                          # 输入是 BAM 比对结果
    -p 20 \                                 # 20 线程
    --append-names \                        # 基因名追加到 ID
    {sample}_Aligned.toTranscriptome.out.bam \  # STAR 转录组 BAM
    {rsem_index} \                          # 预构建 RSEM 索引
    {sample}_rsem                           # 输出前缀
```

### 与 STAR TranscriptomeSAM 的联动

STAR `--quantMode TranscriptomeSAM` 将 reads 比对到**转录组序列**（由基因组外显子拼接而成），生成 `*_Aligned.toTranscriptome.out.bam`。RSEM 直接读取该 BAM 进行 EM 定量：

```
STAR 基因组比对
  → 提取比对到外显子的 reads
  → 重新映射到转录组坐标 (TranscriptomeSAM)
  → RSEM: EM 算法估计 isoform 丰度
  → gene/isoform FPKM
```

这样比 RSEM 自己从头比对的模式**更快、更准**（STAR 的剪接比对精度高于 RSEM 内置的 bowtie）。

## 下游：EBSeq 差异表达

定量完成后，`step3.rsem-run-ebseq.sh` 使用 RSEM 自带的 EBSeq 做差异表达：

```bash
rsem-generate-data-matrix {samples}_rsem.genes.results > gene_readsCount.matrix
rsem-run-ebseq gene_readsCount.matrix {cond1_samples},{cond2_samples} ebseq.result
rsem-control-fdr ebseq.result 0.05 ebseq.fdr.txt
```

## 关键参数速查

| 参数 | 你的值 | 说明 |
|------|:------:|------|
| **STAR** | | |
| `--runThreadN` | 20 | 线程 |
| `--quantMode` | `TranscriptomeSAM GeneCounts` | 转录组 BAM + 基因 counts |
| `--outSAMtype` | `BAM SortedByCoordinate` | 排序 BAM |
| `--readFilesCommand` | `zcat` | gz 输入 |
| **RSEM** | | |
| `--paired-end` | ✓ | PE 模式 |
| `--alignments` | ✓ | 从 BAM 读入比对结果 |
| `-p` | 20 | 线程 |
| `--append-names` | ✓ | 基因名后缀 |
| **EBSeq** | | |
| `rsem-run-ebseq` | cond1,cond2 | 双条件比较 |
| `rsem-control-fdr` | 0.05 | FDR 阈值 |

## 关联

- [[codebase-index]] — transcriptome pipeline 代码
- [[kraken2-classification]] — 另一款分类/比对工具的原理对比
- [[scrnaseq-workflow]] — RNA-seq 分析全流程上下文
