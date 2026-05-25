# Wiki Schema

## Domain
肿瘤早筛 / 多组学 / 宏基因组 mNGS / tNGS / 临床微生物检测 / 生物信息学方法

## Conventions
- File names: lowercase-hyphens, no spaces (e.g., `kingcreate-tngs-panel.md`)
- Every wiki page starts with YAML frontmatter
- Use `[[wikilinks]]` to cross-link pages (minimum 2 outbound links per page)
- Every new page added to `index.md` under the correct section
- Every action appended to `log.md`
- Provenance markers: `^[raw/papers/source-file.md]` for claims from specific sources
- When updating, bump the `updated` date

## Frontmatter
```yaml
---
title: Page Title
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | query | summary
tags: [from taxonomy below]
domain: quantitative-mngs | tngs-comparison | panel-design | bioinfo-pipeline | clinical-evaluation | resistance-detection | ai-drug-discovery | vaccine-design
sources: [raw/papers/source-file.md]
confidence: high | medium | low
contested: true   # set when page has unresolved contradictions
contradictions: [other-page-slug]
---
```

### raw/ Frontmatter
```yaml
---
source_url: https://doi.org/...
ingested: YYYY-MM-DD
sha256: <hex digest of body>
citation: "完整 PubMed/NLM citation 格式"
---
```

## Tag Taxonomy

### Methods/Technologies
- ngs: ngs, mngs, tngs, mp-tngs, hc-tngs, sequencing
- pcr: pcr, multiplex-pcr, qpcr, digital-pcr
- enrichment: hybrid-capture, amplicon, probe, primer
- bioinformatics: alignment, assembly, database, pipeline, normalization
- quantification: absolute-quant, spike-in, standard-curve, lod

### Diseases/Pathogens
- infection: respiratory-infection, lrti, pneumonia
- pathogen: bacteria, virus, fungi, mycobacterium, pneumocystis
- resistance: drug-resistance, amr, antibiotic

### Companies/Products
- company: kingcreate, vision-medicals, genskey, jieyi, weiyan, yuge
- product: rp100-kit, kcq-system, panel

### Meta
- comparison: head-to-head, benchmark, validation
- review: review, survey, meta-analysis
- clinical: clinical-trial, prospective, retrospective, diagnostic

### Immunology/Vaccine
- immunology: vaccine, antigen, epitope, adjuvant, mhc, hla, antibody
- platform: mrna, lnp, viral-vector, protein-subunit, dna-vaccine
- vaccine-strategy: reverse-vaccinology, multi-epitope, structure-guided, immune-imprinting, idiotypic

## Domain Taxonomy

| Domain | Description |
|--------|-------------|
| `quantitative-mngs` | mNGS 绝对定量方法（ISF / KCQ / 内参体系） |
| `tngs-comparison` | mp-tNGS / hc-tNGS / mNGS 方法学对比 |
| `panel-design` | 靶标选型、引物探针设计、探针池构建 |
| `bioinfo-pipeline` | 生信流程、数据库、阈值、标准化方法 |
| `clinical-evaluation` | 大规模临床验证、诊断效能评估 |
| `ai-drug-discovery` | AI 药物发现：蛋白-分子相互作用、虚拟筛选、分子对接 |
| `cancer-biology` | 肿瘤生物学：WGD/免疫逃逸/代谢-表观遗传/肿瘤微环境 |
| `vaccine-design` | 疫苗设计：反向疫苗学、表位预测、多表位构建、免疫信息学、蛋白质抗原工程 |
| `market-watch` | 市场关注：行业融资、BD合作、并购、政策动态、企业竞争格局 |
| `machine-learning` | 机器学习基础算法与模型：监督/无监督/集成学习、特征工程、模型评估、可解释性 |

## Page Thresholds
- Create a page: entity/concept appears in 2+ sources OR central to one source
- Add to existing page: source mentions something already covered
- Split a page: exceeds ~200 lines — break into sub-topics
- Archive: fully superseded content → `_archive/`

## Update Policy
- Newer sources generally supersede older ones
- Genuine contradictions: note both positions with dates/sources
- Mark in frontmatter: `contradictions: [page-name]`
