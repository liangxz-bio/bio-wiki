---
title: "Whole-genome doubling drives immune evasion via epigenetic silencing of antigen presentation"
created: 2026-05-08
updated: 2026-05-08
type: concept
tags: [ngs, bioinformatics, clinical, cancer, wgd, immune-evasion]
domain: cancer-biology
sources: [raw/papers/foidart2026-wgd-cancercell.md]
confidence: high
---

# WGD (Whole-Genome Doubling) 与免疫逃逸

全基因组加倍（WGD）是肿瘤发生的常见起始事件，在癌前病变中即可检出，与不良预后密切相关。Foidart 等人（Cancer Cell 2026）通过细胞融合模型首次系统揭示了 WGD 驱动免疫逃逸的代谢-表观遗传机制。

## 核心发现链

```
WGD → 琥珀酸 (succinate) ↑ → KDM6 活性 ↓ → H3K27me3 ↑
→ MHC-I 转录调控因子（IRF3/STAT1/IRF7）可及性 ↓
→ 抗原呈递 ↓ → CD8+ T 细胞浸润 ↓ → 免疫逃逸
```

## 关键实验证据

1. **模型构建**：通过体细胞融合（PEG）构建 TA3 小鼠乳腺癌 WGD+ 细胞株，近二倍体（2n WGD-）vs 四倍体（4n WGD+），TP53 WT，核型稳定
2. **体内生长差异**：WGD+ 肿瘤在免疫正常小鼠（A/J）中生长显著加快，但在免疫缺陷（NSG）中无差异 → 免疫介导
3. **免疫环境**：WGD+ 肿瘤 CD45+ 白细胞 / CD3+ / CD8+ T 细胞显著减少，CD8/Treg 比值下降，CXCL9/CXCL10 趋化因子降低
4. **抗原呈递沉默**：WGD+ 肿瘤细胞 B2M / H2-K1 / H2-D1 / Psmb9 等 MHC-I 基因下调，IFNγ 应答减弱
5. **表观遗传机制**：WGD+ 细胞 H3K27me3 升高，KDM6A/B 活性降低（蛋白水平不变但酶活降低），琥珀酸积聚抑制 KDM6
6. **PRC2 抑制**：EED226 处理降低 H3K27me3 → 增加抗原呈递 → CD8+ T 细胞浸润增加 → WGD+ 肿瘤生长抑制

## 治疗意义

| 靶点 | 化合物 | 效果 |
|------|--------|------|
| BIRC5 (survivin) | YM155 | 体外 WGD+ 特异性敏感，体内抑制 WGD+ 肿瘤生长 |
| BIRC6 (APOLLON) | - | DepMap CRISPR 筛选中 WGD+ 特异性依赖 |
| PRC2 (EED) | EED226 | 降低 H3K27me3，恢复抗原呈递，免疫浸润增加 |
| PD-L1 | 抗 PD-L1 抗体 | WGD+ 肿瘤更敏感（与抗原呈递缺陷互补） |

## WGD 产生途径对比

| 途径 | 方法 | 特征 | 免疫表型 |
|------|------|------|----------|
| 体细胞融合 | PEG 融合 | 稳定四倍体，正常核型 | 一致免疫逃逸 |
| 胞质分裂失败 | 细胞松弛素 D | 不稳定，部分还原为二倍体 | 异质性较大 |
| TP53 突变相关 | 自然发生 | 基因组不稳定性高 | TP53 WT 中 WGD 预后更差 |

## 临床验证

- METABRIC 队列：WGD+ 肿瘤免疫评分显著降低（各亚型一致）
- TCGA 队列：趋势一致（低纯度肿瘤中显著）
- 配对骨转移灶：WGD+ 病灶 T 细胞浸润减少，HLA 基因表达下调
- 患者来源类器官（PDO）：WGD+ 类器官 IFNγ 诱导 HLA/B2M 较弱

## 与其他免疫逃逸机制的关联

WGD-associated 免疫逃逸可能通过多重机制驱动，除本文发现的代谢-表观遗传轴外，卵巢癌中报道 WGD 抑制 cGAS-STING 信号（McPherson et al. Nature 2025），提示多样化的免疫抑制机制。

## Cross-references

- [[wisecondorx]] — sWGS CNV 检测，可用于 WGD 的拷贝数评估
- [[scrnaseq-workflow]] — scRNA-seq/scATAC-seq 分析流程，本文核心工具
- [[star-rsem-rnaseq]] — RNA-seq 比对定量（本研究使用 STAR + DESeq2 + GSVA）
- [[gbt46943-2025-mngs-standard]] — NGS 质控标准，WGD calling 中的测序质量控制
