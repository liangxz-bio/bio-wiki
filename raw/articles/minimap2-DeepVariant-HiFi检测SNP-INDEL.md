---
source_url: https://mp.weixin.qq.com/s/eM1gXGH_WQt2uQXwzRCOAA
ingested: 2026-05-09
sha256: d421512cabf25d7873d8d2833602fc073323ec8cc097c637c164ce4eaf3e9fe8
citation: "minimap2 + DeepVariant 流程利用HiFi数据检测SNP和INDEL. 微信公众号「允思拓」."
domain: bioinfo-pipeline
see-also: [raw/articles/deepvariant-tool-overview, raw/articles/DeepVariant又快又准-替代GATK的变异检测工具]
---

# minimap2 + DeepVariant 流程利用HiFi数据检测SNP和INDEL

> 来源：微信公众号「允思拓」
> 原文链接：https://mp.weixin.qq.com/s/eM1gXGH_WQt2uQXwzRCOAA

---

DeepVariant 是由 Google 的人工智能研究团队开发的一个用于基因组变异检测的深度学习工具。它使用深度学习模型来预测测序数据中的小变异，包括单核苷酸变异（SNVs）和短插入与缺失（indels）。

## DeepVariant 特点

1. **高准确性**：利用先进的卷积神经网络（CNNs）提高变异检测准确性
2. **适用于多种测序平台**：Illumina、PacBio、Oxford Nanopore
3. **快速**：并行处理和优化算法设计
4. **易于使用**：简单命令行界面
5. **可扩展**：支持多线程和分布式计算

## 工作流程

1. **输入**：BAM/SAM格式比对文件
2. **模型训练**：大量训练数据训练深度学习模型
3. **变异调用**：CNN分析预测变异位点
4. **输出**：VCF格式变异文件

> 2018年以Letters形式发表在 *Nature Biotechnology* (IF2022: 46.9)，并在GitHub开源。

---

## 安装 DeepVariant

### Docker 方式
```bash
BIN_VERSION=1.6.1
sudo docker pull google/deepvariant:"${BIN_VERSION}"
```

### Singularity 方式
```bash
BIN_VERSION=1.6.1
singularity pull docker://google/deepvariant:${BIN_VERSION}
```

> 代码仓库：https://github.com/google/deepvariant/tree/r1.6.1

---

## 完整流程

### 第一步：minimap2 比对

```bash
minimap2 -ax map-pb -a -k 19 -O 5,56 \
  -E 4,1 -B 5 -z 400,50 -r 2k --eqx \
  --secondary=no Fv.fa pra.fq.gz > aln.sam
```

**参数说明：**
| 参数 | 说明 |
|------|------|
| `-ax map-pb` | PacBio CCS比对模式 |
| `-a` | 输出所有比对 |
| `-k 19` | 种子链长度19 |
| `-O 5,56` | 种子链容错率 |
| `-E 4,1` | 扩展种子链额外错误容许度 |
| `-B 5` | 比对起始位置错误容许度 |
| `-z 400,50` | 缩放参数 |
| `-r 2k` | 读取序列内存限制 |
| `--eqx` | 等位基因比对模式 |
| `--secondary=no` | 不输出次优比对 |

### 第二步：SAM转BAM

```bash
samtools sort -@ 12 -O BAM -o aln.sorted.bam aln.sam
samtools index aln.sorted.bam
```

### 第三步：DeepVariant 变异检测

```bash
singularity run ~/deepvariant_1.6.1.sif \
  /opt/deepvariant/bin/run_deepvariant --model_type PACBIO \
  --ref Fv.fa \
  --reads aln.sorted.bam \
  --output_vcf output.vcf.gz \
  --output_gvcf output.g.vcf.gz \
  --sample_name abc \
  --num_shards 24
```

**参数说明：**
| 参数 | 说明 |
|------|------|
| `--model_type PACBIO` | PacBio测序模式 |
| `--ref` | 参考基因组 |
| `--reads` | 排序后BAM文件 |
| `--output_vcf` | 输出VCF |
| `--output_gvcf` | 输出gVCF |
| `--sample_name` | 样本名称 |
| `--num_shards` | 并行分片数 |

---

## 流程图

```
minimap2 比对 → samtools sort/index → DeepVariant 变异检测 → VCF/gVCF
```

- **输入**：Fv.fa（参考基因组）、pra.fq.gz（PacBio HiFi数据）
- **输出**：output.vcf.gz、output.g.vcf.gz

*END*
