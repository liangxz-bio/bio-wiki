---
source_url: https://mp.weixin.qq.com/s/rMztQWe08jTlx4kVCdrnCA
ingested: 2026-05-09
sha256: cd57021f881a520a142ceab0a2300ed0c8490db6be239f90ff8d08e1f079efd6
citation: "01. 种系变异DeepVariant SnakeMake流程：Workflow简介. 微信公众号「生信探索」."
domain: bioinfo-pipeline
see-also: [raw/articles/deepvariant-tool-overview]
---

# 01. 种系变异DeepVariant SnakeMake流程：Workflow简介

> 来源：微信公众号「生信探索」
> 原文链接：https://mp.weixin.qq.com/s/rMztQWe08jTlx4kVCdrnCA

---

**流程代码地址：**
- https://jihulab.com/BioQuest/SnakeMake-DeepVariant
- https://github.com/BioQuestX/SnakeMake-DeepVariant

---

DeepVariant仅支持二倍体、germline变异。

**注意：** `"--bind /home/victor/DataHub/Homo_sapiens"` 是 `config/config.yaml` 文件中设置的基因组存放文件目录。DeepVariant docker运行时需要用到 `.fai` 文件，但在参数中没有体现，所以需要在外部 mount 上基因组文件的目录。

## 运行命令

```bash
mamba activate smk
nohup snakemake --cores 20 --use-conda \
   --use-singularity \
   --singularity-args "--bind /home/victor/DataHub/Homo_sapiens" \
   &> 1.log &
```

---

## Pipeline 流程

### Reference（参考基因组）
- Genome reference 下载自 Ensembl

### Align（比对流程）
| 步骤 | 工具 |
|------|------|
| 1. Adapter trimming | `fastp` |
| 2. Aligner | `BWA mem2` |
| 3. Mark duplicates | `samblaster` |
| 4. Merge CRAMs | `Picard` |
| 5. Create CRAM index | `samtools` |

### Quality control report（质控报告）
- Fastp report → `MultiQC`
- Alignment report → `MultiQC`

### Germline variants（种系变异检测）
- Predict variants → `DeepVariant`
- Joint variant calling → `GLnexus`

### Annotation（注释）
- Annotate variant calls with `VEP`

---

## 输出目录结构

```
├── config
│   ├── captured_regions.bed
│   ├── config.yaml
│   └── samples.tsv
├── dag.svg
├── GLnexus.DB
├── logs
├── raw
│   ├── SRR24443166.fastq.gz
│   └── SRR24443168.fastq.gz
├── README.md
├── report
│   ├── align_multiqc_data
│   ├── align_multiqc.html
│   ├── fastp_multiqc_data
│   ├── fastp_multiqc.html
│   └── vep_report.html
├── report.html
├── results
│   ├── aligned
│   ├── called
│   ├── GLnexus
│   └── vep
└── workflow
    ├── envs
    ├── report
    ├── rules
    ├── schemas
    ├── scripts
    └── Snakefile
```

---

## SnakeMake 简介

Snakemake workflow management system 是一个用于创建可重复、可扩展数据分析的工具。工作流通过人类可读的、基于Python的语言来描述。可以无缝扩展到服务器、集群、网格和云环境，无需修改工作流定义。同时可包含所需软件的描述，自动部署到任何执行环境中。

---

*END*
