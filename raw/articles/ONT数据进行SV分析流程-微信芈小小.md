---
source_url: https://mp.weixin.qq.com/s/4mnMHipZoTV7p5kvcyUCcg
ingested: 2026-05-08
sha256: 051dfd0656cd4d8e99c8827c1fa7ce491d775a9782270808977fe5888d924ee0
citation: "芈小小. ONT数据进行SV分析流程. 一问一答-2024启航++（微信公众号）. 2025."
domain: bioinfo-pipeline
---

# ONT 数据进行SV分析流程

> 来源：微信公众号"一问一答-2024启航++"（芈小小）
> 原文链接：https://mp.weixin.qq.com/s/4mnMHipZoTV7p5kvcyUCcg

以下是基于ONT数据检测结构变异（SV）的常见脚本示例及流程，仅供参考：

---

## 1. 数据比对脚本（使用minimap2）

将ONT测序数据比对到参考基因组，生成BAM文件：

```bash
#比对ONT数据
minimap2 --paf-no-hit -ax map-ont -t 30 ref.fa ont.fq.gz | \
samtools view -bs - | \
samtools sort -@ 30 -o ont_aligned.bam
#建立索引
samtools index -@ 30 ont_aligned.bam

# ONT的深度检测: SVIM
svim alignment \
  --min_sv_size 50 \
  --sample sample_name \
  output_svim ont_aligned.bam ref.fa
```

## 2. SV检测脚本（使用cuteSV）

基于比对结果检测SV：

```bash
#运行cuteSV
cutesv --genotype \
-l 50 -s 5 \
--max_cluster_bias_ins 100 --diff_ratio_merging_ins 0.3 \
--max_cluster_bias_del 100 --diff_ratio_merging_del 0.3 \
ont_aligned.bam ref.fasta ont_cutesv_sv.vcf work_dir
```

## 3. SV检测脚本（使用Sniffles2）

```bash
#另一种常用工具Sniffles2的检测脚本：运行Sniffles2
sniffles -i ont_aligned.bam -o ont_sniffles_sv.vcf -t 30
```

## 4. 结果过滤与整合脚本

可结合多个工具的结果，过滤低质量SV：

```python
#!/usr/bin/env python3
# 4_merge_filter.py
import sys
import pysam
import pandas as pd
from collections import defaultdict

def merge_sv_results(tool_vcfs, output_vcf):
    """
    合并多个工具检测结果
    """
    sv_dict = defaultdict(list)

    for tool, vcf_file in tool_vcfs.items():
        with pysam.VariantFile(vcf_file) as vcf:
            for rec in vcf:
                key = (rec.chrom, rec.pos, rec.info.get('SVTYPE', 'UNK'),
                       rec.info.get('SVLEN', 0))
                sv_dict[key].append({
                    'tool': tool,
                    'qual': rec.qual,
                    'support': rec.info.get('RE', 0) or rec.info.get('SUPPORT', 0)
                })

    # 过滤逻辑：至少2个工具支持，且质量>20
    filtered_svs = []
    for (chrom, pos, svtype, svlen), tools in sv_dict.items():
        if len(tools) >= 2 and all(t['qual'] > 20 for t in tools):
            filtered_svs.append({
                'chrom': chrom, 'pos': pos,
                'svtype': svtype, 'svlen': svlen,
                'tools': ','.join([t['tool'] for t in tools]),
                'support': sum(t['support'] for t in tools)
            })

    # 输出过滤后VCF
    df = pd.DataFrame(filtered_svs)
    df.to_csv('merged_filtered_svs.tsv', sep='\t', index=False)

    return df

if __name__ == '__main__':
    tool_results = {
        'sniffles': 'output.sniffles.vcf',
        'cuteSV': 'output.cutesv.vcf',
        'svim': 'output_svim/variants.vcf'
    }
    merge_sv_results(tool_results, 'merged_svs.vcf')
```

```bash
python 4_merge_filter.py \
        --sniffles ont_sniffles_sv.vcf \
        --cutesv ont_cutesv_sv.vcf \
        --output merged.vcf
```

- 建议结合多个工具交叉验证，以提高SV检测的准确性。如：

```bash
#NanoSV
NanoSV -b ont_aligned.bam -o output.vcf -s samtools -r ref.fasta
```

## 5. 注释SV

```bash
#!/bin/bash
# 5_annotation.sh

# 使用ANNOVAR或VEP注释
vep -i merged.vcf \
    -o annotated.vcf \
    --format vcf \
    --species homo_sapiens \
    --cache --offline \
    --force_overwrite \
    --everything
```

## 6. SV区域覆盖深度可视化

```python
#!/usr/bin/env python3
# 6_visualization.py
import pysam
import matplotlib.pyplot as plt
import numpy as np

def plot_sv_coverage(bam_file, chrom, start, end, sv_type):
    """
    绘制SV区域覆盖深度图
    """
    bam = pysam.AlignmentFile(bam_file, "rb")

    # 获取覆盖深度
    depths = []
    positions = range(start, end)

    for pileupcolumn in bam.pileup(chrom, start, end):
        depths.append(pileupcolumn.n)

    # 绘图
    fig, ax = plt.subplots(2, 1, figsize=(15, 8))

    # 深度图
    ax[0].plot(positions[:len(depths)], depths, 'b-', linewidth=0.5)
    ax[0].set_xlabel('Position')
    ax[0].set_ylabel('Coverage Depth')
    ax[0].set_title(f'{sv_type} at {chrom}:{start}-{end}')

    # 添加SV位置标记
    ax[0].axvline(x=start, color='r', linestyle='--', alpha=0.5, label='SV Start')
    ax[0].axvline(x=end, color='r', linestyle='--', alpha=0.5, label='SV End')
    ax[0].legend()

    plt.tight_layout()
    plt.savefig(f'sv_visualization_{chrom}_{start}_{end}.png', dpi=300)
    plt.close()

    return depths

plot_sv_coverage(ont_aligned.bam, chrom, start, end, sv_type)
```

---

## 工具汇总

| 步骤 | 工具 | 用途 |
|------|------|------|
| 数据比对 | minimap2 | ONT reads比对到参考基因组 |
| SV检测 | cuteSV | 基于三代长reads的SV检测 |
| SV检测 | Sniffles2 | 另一种SV检测工具 |
| SV检测 | SVIM | ONT数据SV检测 |
| SV检测 | NanoSV | 补充SV检测工具 |
| 结果整合 | SURVIVOR / Python脚本 | 多工具结果合并与过滤 |
| 注释 | VEP / ANNOVAR | SV功能注释 |
| 可视化 | matplotlib + pysam | SV区域覆盖深度绘图 |
