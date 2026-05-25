---
source_url: https://mp.weixin.qq.com/s/9grGnDhB2YRikMs-jsnByA
ingested: 2026-05-09
sha256: e438440006c570b113f1c895e679e2335420592072f5f2fd725b97acd863bbba
citation: "xiongxu. Variant calling还在用GATK？DeepVariant又快又准. 微信公众号「生信开发者」. 参考: Poplin R, et al. A universal SNP and small-indel variant caller using deep neural networks. Nat Biotechnol. 2018;36(10):983-987. doi:10.1038/nbt.4235"
domain: bioinfo-pipeline
see-also: [raw/articles/deepvariant-tool-overview]
---

# Variant calling还在用GATK？DeepVariant又快又准

> 来源：微信公众号「生信开发者」（作者：xiongxu）
> 原文链接：https://mp.weixin.qq.com/s/9grGnDhB2YRikMs-jsnByA

---

DeepVariant（*A universal SNP and small-indel variant caller using deep neural networks*. *Nature Biotechnology* 36, 983–987 (2018)）为谷歌开源的基于机器学习的变异分析工具。

今年年初有篇 *Scientific Reports* 上的文献[^1]，对GATK与DeepVariant做了详细的比较，最终得出的结论是：

> **Compared to GATK, DeepVariant had a shorter execution time and higher accuracy for clinical samples.**

[^1]: https://www.nature.com/articles/s41598-022-05833-4

---

## DeepVariant 核心优势

DeepVariant production model 使用 **6个core channels**（read base, base quality, mapping quality, strand of alignment, read supports variant, base differs from ref）作为基本训练模型[^2]。1.4版本引入了 `insert_size` channel后准确性进一步提升：

> For Illumina WGS and WES, we add an additional feature of read insert size (`insert_size`). This reduces errors by 4-10% for Illumina WGS and WES model.[^3]

[^2]: https://google.github.io/deepvariant/posts/2022-06-09-adding-custom-channels/
[^3]: https://github.com/google/deepvariant/releases

---

## 实际对比案例

### 案例1：non-uniqueness mappability 边缘变异

GATK HaplotypeCaller 没有 call 出 proband 的变异（**假阴性**），只 call 出了母亲的变异，而 DeepVariant 都准确 call 出来了。

### 案例2：n-polymer（polyA）附近的序列

GATK 报了一个低 VAF 的 indel，但 DeepVariant 认为此处是 refCall，不是变异。

**个人直观感受：** DeepVariant 假阳性明显比 GATK 少很多，假阴性也比 GATK 少。大家有兴趣也可用 `rtgtools`、`hap.py` 等工具对 NA12878 金标准数据做一个评测。

---

## Docker 运行命令

```bash
docker run --privileged --rm --user `id -u`:`id -g` \
-v "/sg2/8.xuxiong/WES_Clinical/workstation_V6.2.0_WES_20220916A_T7/b.cram":"/input" \
-v "/bi/8.xuxiong":"/output" \
-v "/sg2/8.xuxiong/TargetSeqV6/genome":"/reference" \
-v "/bi/8.xuxiong/database":"/database" \
google/deepvariant:"latest" \
/opt/deepvariant/bin/run_deepvariant \
--model_type=WES \
--ref=/reference/ucsc.hg19.fasta \
--reads=/input/PES22090081-HE.deduped.cram \
--regions chr1:215913883-215915883 \
--output_vcf=/output/PES22090081.dv.vcf.gz \
--output_gvcf=/output/PES22090081.raw.g.vcf.gz \
--intermediate_results_dir /output/PES22090081_tmp_dir \
--num_shards=8
```

---

## 参考链接

1. DeepVariant 原始论文：Poplin et al. *Nat Biotechnol* 36, 983–987 (2018)
2. GATK vs DeepVariant 比较：https://www.nature.com/articles/s41598-022-05833-4
3. DeepVariant custom channels 文档：https://google.github.io/deepvariant/posts/2022-06-09-adding-custom-channels/
4. DeepVariant Release 说明：https://github.com/google/deepvariant/releases

*END*
