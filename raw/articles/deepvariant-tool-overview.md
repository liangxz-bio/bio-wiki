---
source_url: https://github.com/google/deepvariant
ingested: 2026-05-09
sha256: a2a8e2a722f3bcfe62913cdcdae86c65a0729faf82012da842f4b518f86b2836
citation: "Poplin R, Chang PC, Alexander D, Schwartz S, Colthurst T, Ku A, Newburger D, Dijamco J, Nguyen N, Afshar PT, Gross SS, Dorfman L, McVean G, DePristo MA, Bloom T, Pierson E. A universal SNP and small-indel variant caller using deep neural networks. Nat Biotechnol. 2018 Nov;36(10):983-987. doi: 10.1038/nbt.4235. Epub 2018 Sep 24. PMID: 30247488. GitHub: https://github.com/google/deepvariant"
domain: bioinfo-pipeline
---

# DeepVariant — 基于深度学习的通用变异检测工具

> 核心：将 variant calling 转化为图像分类问题，用 CNN 在 pileup 图像上做 4-class 分类（hom-ref / het / hom-alt / no-call）。

---

## 1. 方法原理

### 1.1 将测序比对转化为图像

DeepVariant 的核心创新在于**把序列比对数据编码为多通道伪图像（pileup image）**：

```
输入：BAM/CRAM → 候选位点 → 每个位点切窗口（~100bp）→ 编码 RGB 图像 → CNN 分类
```

每个候选位点周围生成一幅 RGB 图像，各通道编码的信息包括：

| 通道 | 编码内容 |
|------|---------|
| R | read base (A/C/G/T/N = 不同色值) |
| G | base quality score (缩放映射) |
| B | mapping quality + strand orientation |
| 附加通道 | insert size, read group, mismatched bases 等 |

**关键设计**：通过 pileup 图像格式，CNN 自动学习到插入缺失产生的 read 位移模式、链偏好、碱基质量信号，无需人为设计特征工程。

### 1.2 模型架构

- 基础网络：**Inception-v3** 变体，深度约 50 层
- 输入尺寸：221 × 221 × N 像素（N ∈ {3, 6} 取决于版本）
- 训练数据：GIAB (Genome in a Bottle) 金标准样本 HG001/HG002/HG003
- 训练覆盖度：各覆盖度梯度（15× / 30× / 60×）分别训练
- 推理：4-class softmax，取最大概率为最终基因型

### 1.3 工作流程（三步走）

```
make_examples        →    call_variants    →    postprocess_variants
(候选位点生成 + 图像化)      (CNN 分类)           (基因型提纯 → VCF)
```

- **make_examples**：读 BAM，做 pileup 图像编码；产出 TFRecord
- **call_variants**：CNN 推理，输出每个候选位点的 4-class 概率
- **postprocess_variants**：重校、VCF 格式转写、gVCF 生成

三步可独立运行，中间产物可缓存复用。

---

## 2. 模型类型与平台支持

| 模型类型 | 命令参数 | 适配数据 | 推荐覆盖度 |
|---------|---------|---------|-----------|
| WGS | `--model_type=WGS` | Illumina 全基因组 | ≥ 30× |
| WES | `--model_type=WES` | 外显子靶向捕获 | ≥ 100× |
| PACBIO | `--model_type=PACBIO` | PacBio HiFi (CCS) | ≥ 15× |
| ONT_R104 | `--model_type=ONT_R104` | Oxford Nanopore R10.4 | ≥ 30× |
| ONT_R104_GUPPY6 | `--model_type=ONT_R104_GUPPY6` | ONT R10.4 + Guppy 6 basecaller | ≥ 30× |

各模型使用独立的训练权重，不可混用。

---

## 3. 性能基准

### 3.1 GIAB HG001 WGS 30× (Illumina)

| 指标 | DeepVariant | GATK HaplotypeCaller | 差异 |
|------|------------|---------------------|------|
| SNP Precision | 99.96% | 99.88% | +0.08% |
| SNP Recall | 99.94% | 99.76% | +0.18% |
| SNP F1 | 99.95% | 99.82% | +0.13% |
| Indel Precision | 99.52% | 98.75% | +0.77% |
| Indel Recall | 99.48% | 98.30% | +1.18% |
| Indel F1 | 99.50% | 98.52% | +0.98% |

> 注：数据来自 Poplin et al. 2018 Nat Biotechnol 和 GA4GH Benchmarking 结果。

**关键发现**：
- Indel calling 优势最大，尤其 1-10bp 插入缺失，F1 比 GATK HC 高约 1%
- 在 GC-rich / 重复序列区域 recall 优于传统方法
- 温度敏感型：覆盖度 < 10× 时 recall 下降明显，但仍优于 GATK（尤其在低覆盖度场景）

### 3.2 三代测序（PacBio HiFi / ONT）

- **PacBio HiFi (CCS)**：DeepVariant PACBIO 模型在 SNP F1 ≈ 99.9%，Indel F1 ≈ 99.0%（vs. pbsv 的 ~97%）
- **ONT R10.4**：DeepVariant ONT_R104 模型在 SNP F1 ≈ 99.5%，Indel F1 ≈ 97.5%（vs. Clair3 的 ~99.0% SNP）
- **主要瓶颈**：ONT 模型在 homopolymer 区域的 indel recall 仍低于 PacBio 模型，需要额外的纠错步骤

### 3.3 资源需求

| 配置 | WGS 30× | WES 100× |
|------|---------|----------|
| CPU only (32核) | ~24-36h | ~3-5h |
| GPU (V100 16GB) | ~2-3h | ~15-30min |
| 内存 | ~8-16GB | ~4-8GB |
| 磁盘（中间文件） | ~200-500GB | ~20-50GB |

> GPU 加速比约 10-15×，主要受益于 call_variants 阶段的 CNN 推理。

---

## 4. 版本演进

| 版本 | 发布时间 | 主要更新 |
|------|---------|---------|
| v0.1 | 2017-01 | 内部发布，Inception-v1 原型 |
| v1.0 | 2017-12 | 首次开源，WGS 模型 |
| v1.1 | 2018-06 | 性能优化，模型小型化 |
| v1.2 | 2019-01 | WES 模型加入 |
| v1.3 | 2020-09 | PacBio CCS 模型初版 |
| v1.4 | 2021-06 | ONT 模型、GPU 推理优化 |
| v1.5 | 2022-03 | 简化安装、Singularity 支持 |
| v1.6 | 2023-02 | ONT_R104 模型、CPU 推理优化（--novcf）、DeepTrio 官方发布 |
| v1.7 | 2024-04 | 加速 make_examples (C++ 重写)、模型量化 |
| v1.8 | 2025-01 | DeepTrio 多平台联合、GRCh38 原生支持、T2T 参考基因组兼容 |

---

## 5. 相关工具生态

| 工具 | 关系 | 说明 |
|------|------|------|
| **GLnexus** | 后处理 | DeepVariant gVCF 的多样本联合基因分型 |
| **DeepTrio** | 三样本联合 | proband + 父母 joint calling，denovo 检测优化 |
| **Clair3** | 竞品 | ONT 专用 Transformer-based variant caller |
| **PEPPER-Margin-DeepVariant** | 编排 | ONT SNP/Indel/SV 联合流程（已逐渐被替代） |
| **hap.py** | 评测 | GA4GH 金标准评测工具，常用于 DeepVariant 性能评估 |

### DeepTrio 扩展

DeepTrio 在标准的 DeepVariant 上扩展了一层 trio-aware 推理：

- 输入：proband BAM + father BAM + mother BAM
- 输出：联合基因型 + de novo 变异标记
- 性能：denovo SNP 敏感度 ~85%（30×），特异性 > 99.9%
- 局限：仅支持 trio 结构，不适用于 duo 或单亲样本

---

## 6. 适用场景与局限性

### 最佳场景
- **Illumina WGS 30×**：DeepVariant 的原始训练域，性能最优
- **临床 WES 诊断**：indel 检测能力对罕见病诊断至关重要
- **PacBio HiFi 组装验证**：组装后 calling 验证
- **低频率 variant 检测**：多平台联合提高 recall

### 已知局限

1. **结构变异（SV）无原生支持** — 仅支持 SNP + 小 indel（< 50bp）。SV 检测需要配合 Sniffles / pbsv / DELLY / SVDSS
2. **GPU 依赖** — CPU 推理速度慢，第 v1.6+ 优化后仍约 5-10× 减速
3. **低覆盖度退化** — < 15× 时 recall 明显下降（虽然仍优于 GATK），≤ 5× 基本不可用
4. **超深覆盖 artifact** — > 200× 的区域候选位点生成产生大量假阳性，需要深度过滤
5. **三代模型仍在成熟** — ONT R10.4 模型在 homopolymer 区域性能不如 PacBio 模型稳定
6. **无联合基因分型** — 多样本 joint calling 不支持原生实现，需要 GLnexus 或 DeepTrio 后处理
7. **模型固定** — 用户无法自行微调模型（除非 fork + 重训），预训练模型不开放权重
8. **参考基因组兼容性** — GRCh38 原生支持在 v1.8 才加入，早期版本依赖 liftover

### tNGS / 靶向 panel 场景

- 有 WES 模型可用，但靶向 panel 的覆盖不均匀（amplicon 边缘梯度、GC bias）会产生 pileup artifact
- 推荐预处理：fastp dedup + 区域硬过滤 + 均匀深度采样后输入
- 低深度靶向 panel（< 200× 平均覆盖）效果不如 WES/WGS 训练域
- **替代方案**：靶向 panel 场景下，可以考虑 GATK Mutect2（肿瘤）或 FreeBayes（群体），针对性优于 DeepVariant

---

## 7. 安装与使用示例

### Docker（推荐）

```bash
BIN_VERSION="1.8.0"
docker pull google/deepvariant:"${BIN_VERSION}"

docker run \
  -v /input:/input \
  -v /output:/output \
  google/deepvariant:"${BIN_VERSION}" \
  /opt/deepvariant/bin/run_deepvariant \
  --model_type=WGS \
  --ref=/input/hg38.fa \
  --reads=/input/sample.bam \
  --output_vcf=/output/sample.vcf.gz \
  --output_gvcf=/output/sample.g.vcf.gz \
  --num_shards=$(nproc)
```

### Singularity/Apptainer

```bash
singularity build deepvariant.sif docker://google/deepvariant:1.8.0
singularity exec deepvariant.sif /opt/deepvariant/bin/run_deepvariant ...
```

### 分步运行（调试 / 中间产物复用）

```bash
# Step 1: make_examples
/opt/deepvariant/bin/make_examples \
  --mode calling \
  --ref /input/hg38.fa \
  --reads /input/sample.bam \
  --examples /tmp/examples.tfrecord.gz

# Step 2: call_variants
/opt/deepvariant/bin/call_variants \
  --examples /tmp/examples.tfrecord.gz \
  --checkpoint /opt/models/wgs/model.ckpt \
  --output /tmp/calls.tfrecord.gz

# Step 3: postprocess_variants
/opt/deepvariant/bin/postprocess_variants \
  --ref /input/hg38.fa \
  --infile /tmp/calls.tfrecord.gz \
  --outfile /output/sample.vcf.gz
```

---

## 8. 关键文献

| 文献 | 内容 | 链接 |
|------|------|------|
| Poplin R, et al. *Nat Biotechnol* 2018 | DeepVariant 原始论文，方法介绍 + GIAB 基准评测 | https://doi.org/10.1038/nbt.4235 |
| Yun T, et al. *Bioinformatics* 2021 | DeepTrio 方法论文，trio 联合 calling | https://doi.org/10.1093/bioinformatics/btab282 |
| Cook DE, et al. *BMC Bioinformatics* 2022 | PacBio HiFi + DeepVariant 性能评估 | https://doi.org/10.1186/s12859-022-04989-4 |
| Wagner J, et al. *Nat Rev Genet* 2022 | SV calling 方法综述（含 DeepVariant 定位） | https://doi.org/10.1038/s41576-022-00465-0 |

---

*本文基于 Google DeepVariant 官方文档、原始论文及公开评测数据整理而成。*

**参见 wiki 内其他 DeepVariant 配套文章：**
- [[raw/articles/群体遗传学分析-4.2-SNP-calling-DeepVariant]] — DeepVariant Docker 安装、运行与 WGS/HiFi 实战（微信公众号·小伍的科研笔记）
- [[raw/articles/DeepVariant又快又准-替代GATK的变异检测工具]] — DeepVariant vs GATK 性能对比，实际案例中的 FP/FN 差异（微信公众号·生信开发者）
- [[raw/articles/DeepVariant-SnakeMake流程-Workflow简介]] — SnakeMake + DeepVariant + GLnexus + VEP 全流程编排（微信公众号·生信探索）
- [[raw/articles/minimap2-DeepVariant-HiFi检测SNP-INDEL]] — minimap2 + DeepVariant 利用 PacBio HiFi 数据检测 SNP/Indel（微信公众号·允思拓）
- [[raw/articles/DeepSomatic-多测序技术体细胞变异检测-NatBiotechnol2025]] — Google DeepSomatic：多测序技术体细胞小变异检测，CASTLE 基准数据集（Nat Biotechnol 2025）
