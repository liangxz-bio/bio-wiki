---
source_url: https://mp.weixin.qq.com/s/vNh8cFSACRzhGM0TISExkg
ingested: 2026-05-09
sha256: 4c25bdb47eea7accb7bcbbca06419a3ab7836c9a97af00023f34d79b1df6ccbe
citation: "Shafin K, et al. Accurate somatic small variant discovery for multiple sequencing technologies with DeepSomatic. Nat Biotechnol. 2025. doi:10.1038/s41587-025-02839-x. 微信公众号「优在生物」解读."
domain: bioinfo-pipeline
see-also: [raw/articles/deepvariant-tool-overview]
---

# Nat Biotechnol | Google DeepSomatic：多测序技术体细胞小变异检测

> 来源：微信公众号「优在生物」（作者：八戒聚焦）
> 原文链接：https://mp.weixin.qq.com/s/vNh8cFSACRzhGM0TISExkg
> 论文链接：https://www.nature.com/articles/s41587-025-02839-x

---

## 导语

体细胞变异检测是癌症基因组学分析的核心环节。短读长测序虽碱基准确性高，但在重复区域定位能力有限；长读长测序虽能解决重复区域mapping和变异定相问题，却因研究不足导致现有工具适配性差。

现有两大关键问题：
1. **缺乏公开可用的高质量训练和基准数据集**，尤其长读长数据稀缺
2. **现有工具在多测序技术中的兼容性和准确性不足**

2025年10月16日，美国Google公司 **Kishwar Shafin** 团队联合加州大学 **Benedict Paten** 团队在 **Nature Biotechnology** 发表论文 *Accurate somatic small variant discovery for multiple sequencing technologies with DeepSomatic*。

---

## DeepSomatic 核心亮点

- **开发工具**：基于深度学习的体细胞小变异检测工具 **DeepSomatic**
- **适配数据**：Illumina短读长、PacBio HiFi、ONT长读长
- **支持场景**：全基因组、全外显子测序及多种样本类型
- **公开数据集**：**CASTLE** 数据集（6对肿瘤-正常细胞系多技术测序数据）
- **性能**：在短读长和长读长数据上均持续优于现有工具

---

## 方法与结果

### 技术路线

DeepSomatic 基于 **DeepVariant** 改造开发，三步流程：

| 步骤 | 功能 |
|------|------|
| **1. make_examples** | 筛选体细胞变异候选位点，排除正常样本中高于阈值的变异，将reads特征构建为张量（长读长额外加入单倍型区分通道） |
| **2. call_variants** | CNN将候选位点分类为参考序列、生殖系变异或体细胞变异 |
| **3. postprocess_variants** | 对分类结果进行标注 |

> 仅肿瘤样本场景：用群体VCF文件替代正常样本数据

### SEQC2 HCC1395 基准测试结果

在三种测序技术上的性能对比：

| 数据类型 | 指标 | DeepSomatic | Strelka2 | ClairS |
|---------|------|-------------|---------|-------|
| **Illumina SNV** | F1 | **0.9829** | 0.9521 | 0.9692 |
| **Illumina Indel** | F1 | **0.8944** | 0.7935 | - |
| **PacBio Indel** | F1 | **0.8151** | - | 0.4550 |

**核心优势：**
- 长读长 indel 检测优势显著（PacBio F1 0.8151 vs ClairS 0.4550）
- 低等位基因频率（VAF < 0.1）变异检测中召回率显著更高
- FFPE样本 SNV F1 达 **0.8803**（优于 Strelka2 0.7894 和 ClairS 0.8072）

### 多癌症模型验证

研究人员对另外5对肿瘤-正常细胞系（4种乳腺癌、2种肺癌）进行多技术测序，构建多癌症训练数据集，通过四重评估验证：
1. 未参与训练的1号染色体数据评估
2. 正交工具基准集评估（Strelka2 + ClairS）
3. 正交技术基准集评估
4. 未参与训练的 Hs578 和 HG008 细胞系评估

结果：**DeepSomatic 多癌症模型在7个细胞系中中位F1值均高于ClairS、Strelka2等工具**

### 拓展应用场景

- 仅肿瘤样本模型
- **FFPE样本模型**（SNV F1=0.8803）
- 全外显子测序（WES）专用模型

---

## 总结

该研究开发的 **DeepSomatic** 实现了多测序技术下体细胞小变异的精准检测，并公开了 **CASTLE** 数据集（6对肿瘤-正常细胞系多技术测序数据及高质量基准变异集）。在长读长 indel 检测、低 VAF 变异检测及 FFPE 样本处理上优势显著，填补了长读长数据相关基准数据集的空白。

---

## 参考文献

- **论文**：Shafin et al. *Accurate somatic small variant discovery for multiple sequencing technologies with DeepSomatic*. *Nature Biotechnology* (2025)
- **链接**：https://www.nature.com/articles/s41587-025-02839-x

*END*
