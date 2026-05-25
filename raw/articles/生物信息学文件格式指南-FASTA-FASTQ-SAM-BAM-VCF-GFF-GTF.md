---
source_url: https://mp.weixin.qq.com/s/rHpgHirq3r_XPuFLJAw_vQ
ingested: 2026-05-07
sha256: 3e634b49949834ceeecc149df5c5a3162a5d4dd2e8bb4d08829bf7f030610708
citation: "生信技术（微信公众号）. 生物信息学分析中必须知道的几种文件格式. 微信公众号. 2025."
domain: bioinfo-pipeline
---

# 生物信息学分析中必须知道的几种文件格式

**来源**：生信技术（微信公众号）

---

## 介绍

下面将会介绍几种常见的生物信息学常见文件格式，包括：FASTA、FASTQ、SAM、BAM、VCF、BCF、GFF、GTF。

---

## FASTA 格式

FASTA 格式是一种基于文本用于表示核酸序列或多肽序列的格式。其中核酸或氨基酸均以单个字母来表示，且允许在序列前添加序列名及注释。该格式已成为生物信息学领域的一项标准。

FASTA 文件的第一行以 `>`（大于）符号开头，后跟序列的描述或标识符。在第一行（用于序列的唯一描述）之后是标准单字母代码中的实际序列本身。

**几个示例序列：**

```
>KX580312.1 Homo sapiens truncated breast cancer 1 (BRCA1) gene, exon 15 and partial cds
GTCATCCCCTTCTAAATGCCCATCATTAGATGATAGGTGGTACATGCACAGTTGCTCTGGGAGTCTTCAGAATAGAAACTACCCATCTCAAGAGGAGCTCATTAAGGTTGTTGATGTGGAGGAGTAACAGCTGGAAGAGTCTGGGCCACACGATTTGACGGAAACATCTTACTTGCCAAGGCAAGATCTAG

>KRN06561.1 heat shock [Lactobacillus sucicola DSM 21376 = JCM 15457]
MSLVMANELTNRFNNWMKQDDFFGNLGRSFFDLDNSVNRALKTDVKETDKAYEVRIDVPGIDKKDITVDYHDGVLSVNAKRDSFNDESDSEGNVIASERSYGRFARQYSLPNVDESGIKAKCEDGVLKLTLPKLAEEKINGNHIEIE
```

---

## FASTQ 格式

FASTQ 格式也是一种基于文本的格式来表示核苷酸序列，而且还包含每个核苷酸的相应质量。它是存储高通量测序仪器（如 *Illumina*）输出的标准。

FASTQ 文件每个序列使用四行：

- **第 1 行**：以 `@` 字符开头，后跟序列标识符和可选描述（如 FASTA 标题行）。
- **第 2 行**：是原始序列字母。
- **第 3 行**：以 `+` 字符开始，并且可以选择后跟相同的序列标识符（和任何描述）。
- **第 4 行**：对第 2 行中序列的质量值进行编码，并且必须包含与序列中的字母相同数量的符号。

**FASTQ 格式的示例序列：**

```
@K00188:208:HFLNGBBXX:3:1101:1428:1508 2:N:0:CTTGTA
ATAATAGGATCCCTTTTCCTGGAGCTGCCTTTAGGTAATGTAGTATCTNATNGACTGNCNCCANANGGCTAAAGT
+
AAAFFJJJJJJJJJJJJJJFJJFJJJJJFJJJJJJJJJJJJJJJJJ#FJ#JJJJF#F#FJJ#F#JJJFJJJJJ
```

**FASTQ 格式的扩展描述：**

- **第 1 行**：`@K00188:208:HFLNGBBXX:3:1101:1428:1508 2:N:0:CTTGTA`
- **第 2 行**：`ATAATAGGATCCCTTTTCCTGGAGCTGCCTTTAGGTAATGTAGTATCTNATNGACTGNCNCCANANGGCTAAAGT`（DNA 序列）
- **第 3 行**：`+`（随后为 Illumina 测序标识符）
- **第 4 行**：`AAAFFJJJJJJJJJJJJJJFJJFJJJJJFJJJJJJJJJJJJJJJJJJJ#FJ#JJJJF#F#FJJ#F#JJJFJJJJJ`（每个碱基对的质量得分）

### Phred 质量分数

> **Q = -10log₁₀(p)**
> 其中 **p** 是相应碱基调用不正确的概率

| Phred 质量分数 | 碱基错误的概率 | 碱基检出的准确性 |
|:---:|:---:|:---:|
| 10 | 1 in 10 | 90% |
| 20 | 1 in 100 | 99% |
| 30 | 1 in 1,000 | 99.90% |
| 40 | 1 in 10,000 | 99.99% |
| 50 | 1 in 100,000 | 100.00% |

- *Fastq-sanger* 的 *PHRED* 分数为 0 - 93
- *Fastq-Illumina* 的 *PHRED* 分数为 0 - 62

它们不是给出 PHRED 分数的数值，而是以 33 到 126 的 ASCII 字符代码提供。

基于基本字符（代表零 PHRED 分数的字符），PHRED 比例通常称为 **PHRED+33** 或 **PHRED+64**。

---

## SAM

**Sequence Alignment/Map (SAM)** 是一种基于文本的比对格式，支持由不同测序平台产生的单端和双端 reads。

它可以支持短读和长读（最高 128Mbp）。该格式已扩展为包括未映射的序列，并且它可能包含其他数据，例如碱基调用和比对质量。

SAM 格式由标题和比对部分组成。标题以符号开头 `@`，将它们与对齐部分区分开来。

SAM 文件中的所有行都是制表符分隔的。对齐部分包含 11 个必填字段，其他字段是可选的。

### SAM 手册中使用的一些术语

- **Template**：测量的 DNA 片段
- **Reads**：根据方法，模板可以产生一个或多个 reads。这些 reads 可以覆盖整个模板或只是将其小节划分。源自同一模板的 reads 通常涵盖模板的不同部分，并且可以表示模板本身或其反向互补。
- **Segments**：每个 read 都可以产生一个或多个对齐，又将具有称为段的对齐区域。来自这些段，可以推断出原始模板的大小。

### Fields（字段说明）

**Col.1 QNAME** — 查询模板名称。具有相同 QNAME 的 read/片段被认为来自相同的模板。QNAME `*` 表示信息不可用。

**Col.2 FLAG** — 按位标志组合：

| BIT | Description |
|:---:|:---|
| 0x1 | read 是 pair 中的一条 |
| 0x2 | pair 一正一负完美比对上 |
| 0x4 | 片段未比对上 |
| 0x8 | mate 没有比对上 |
| 0x10 | 这条 read 反向比对 |
| 0x20 | mate 反向比对 |
| 0x40 | 这条 read 是 read1 |
| 0x80 | 这条 read 是 read2 |
| 0x100 | 第二次比对 |
| 0x200 | 没有通过质量控制 |
| 0x400 | read 是 PCR 或光学重复产生 |
| 0x800 | 辅助比对结果 |

**Col.3 RNAME** — 参考序列的名称。它通常是指染色体编号。

**Col.4 POS** — read 中第一个匹配碱基的最左侧映射位置。它具有基于 1 的索引。如果 pos 设置为 0，则表示未映射读取。

**Col.5 MAPQ** — 映射质量。MAPQ = -10log₁₀（映射位置错误的概率）。MAPQ=255 表示映射质量不可用。

**Col.6 CIGAR** — 描述比对的字符串：

| OP | BAM | 描述 |
|:---:|:---:|:---|
| M | 0 | 比对匹配（可以是序列匹配或错配） |
| I | 1 | 插入参考 |
| D | 2 | 从参考中删除 |
| N | 3 | 从参考中跳过的区域 |
| S | 4 | 软剪切（被剪切的序列存在于序列中） |
| H | 5 | 硬剪切（被剪切的序列不存在于序列中） |
| P | 6 | 填充（从填充引用中无声删除） |
| = | 7 | 序列匹配 |
| X | 8 | 序列不匹配 |

> **H 和 S 之间的区别在于：**
> 如果不匹配序列被报告为对齐文件中的读取序列的一部分，则它是一个软剪切（S）；
> 经常在参考序列中的其他位置匹配不匹配区域，并且在这种情况下，不匹配区域被从报告的读取序列中移除在对齐中并且被称为硬剪切（H）。

**Col.7 RNEXT, Col.8 PNEXT** — RNEXT 和 PNEXT 是为可视化工具了解配对末端读取伙伴的参考和位置。

- RNEXT：mate 的参考序列名称，实际上就是 mate 比对到的染色体号。RNEXT 的值 `=` 表示与相同的参考对齐，`*` 表示没有可用信息（单端测序）。
- PNEXT：其中对的另一个比对 read（信息不可用 = 0）

**Col.9 TLEN** — 如果 R1 端的 read 和 R2 端的 read 能够 mapping 到同一条 Reference 序列上（即第三列 RNAME 相同），则该列的值表示第 8 列减去第 4 列加上第 6 列的值。R1 文件的 reads 和 R2 文件的 reads，相同 id 的 reads 要相对来看。在进行该列值的计算时，如果取第 6 列的数值，一定要取出现 M 的值，S 或 H 的值不能取。

**Col.10 SEQ** — reads 或片段的序列。

**Col.11 QUAL** — 读取的 PHRED 得分值。如果 `*` 则不存储任何值。

---

## BAM

| 项目 | 内容 |
|:---:|:---|
| 文件格式 | BAM |
| 文件扩展名 | file.bam |

**Binary Alignment Map (BAM)** 文件是 **Sequence Alignment/Map (SAM)** 的压缩二进制版本，是核苷酸序列比对的紧凑和可索引表示。

SAM 和 BAM 之间的数据完全相同。二进制 BAM 文件体积小，非常适合存储对齐文件。需要 samtools 才能查看文件。

---

## VCF

| 项目 | 内容 |
|:---:|:---|
| 文件格式 | VCF |
| 文件扩展名 | file.vcf |

VCF 是一种带有标题的文本文件格式（信息 VCF 版本、样本等），数据行构成文件的主体。

**标题：**

这包含元信息并包含在 `##` 字符串之后。建议包含 INFO、FILTER 和 FORMAT 条目，以便更好地解释数据字段。

元数据格式：
```
##INFO=<ID=ID,Number=number,Type=type,Description="description",Source="source",Version="version">
##FILTER=<ID=ID,Description="description">
##FORMAT=<ID=ID,Number=number,Type=type,Description="description">
```

还可以包括其他信息，例如替代等位基因、组装字段、Contig 字段、样本字段、谱系字段。

**数据字段：**

数据行有 8 个必填列：`#CHROM`, `POS`, `ID`, `REF`, `ALT`, `QUAL`, `FILTER`, `INFO`。

---

## BCF

**Binary Call Format (BCF)** 是 VCF 的二进制表示，包含二进制格式的相同信息以提高性能。

---

## GFF

GFF 格式是 Sanger 研究所定义，是一种简单的、方便的对于 DNA、RNA 以及蛋白质序列的特征进行描述的一种数据格式，比如序列的哪里到哪里是基因。已经成为序列注释的通用格式，比如基因组的基因预测，许多软件都支持输入或者输出 gff 格式。

| 项目 | 内容 |
|:---:|:---|
| 文件格式 | GFF |
| 文件扩展名 | file.gff2, file.gff3, file.gtf |

### GFF (General Feature Format or Gene Finding Format)

GFF 可用于与序列相关的任何类型的特征（转录本、外显子、内含子、启动子、3' UTR、重复元件等），而 GTF 主要用于基因/转录本。GFF3 是最新版本，是对 GFF2 格式的改进。

GFF 格式有 9 列，它们是 TAB 分隔的：

**Col.1 Reference Sequence** — 这是用于建立注释坐标系的参考序列的 ID。通常是染色体名称或编号。

**Col.2 Source** — 这解释了特征注释是如何派生的。来源是一个自由文本限定符，旨在描述生成此特征的算法或操作过程。通常这是软件的名称，例如 "Genescan" 或数据库名称，例如 "Genbank"。实际上，源用于通过向类型添加限定符来扩展特征本体，从而创建新的复合类型。如果没有必要指定来源，输入 `.`。

**Col.3 Type** — 注释信息的类型，比如 Gene、cDNA、mRNA 等，或者是 SO 对应的编号。

**Col.4 Start** — 对应片段的起点。从 1 开始计数。

**Col.5 End** — 对应片段的终点。结束位置不能大于序列的长度。

**Col.6 Score** — 得分，数字，是注释信息可能性的说明，可以是序列相似性比对时的 E-values 值或者基因预测是的 P-values 值。`.` 表示为空。

**Col.7 Strand** — 序列的方向，`+` 表示正义链, `-` 反义链, `?` 表示未知。

**Col.8 Frame (GFF2 and GTF) or Phase (GFF3)** — 对于编码蛋白质的 CDS 来说，本列指定下一个密码子开始的位置。可以是 0，1 或 2，表示到达下一个密码子的第一个碱基的碱基数。对于其它属性，则用点（`.`）代替。对于 "CDS" 类型的特征，相位指示特征从阅读框开始的位置。相位是整数 0、1 或 2 中的一个，表示应该从该特征的开头移除以到达下一个密码子的第一个碱基的碱基数。

对于所有 CDS 功能都需要该阶段。`###` 和 `***` 代表连续的外显子。

**Col.9 Attributes or Group field** — 一个包含众多属性的列表。格式为 `标签＝值`（*tag=value*）。不同属性之间以分号相隔。可以存在空格，不过若有 `",=; ` 则用 URL 转义（*URL escaping rule*），同时 TAB 也需要转换为 `%09` 表示。

### GFF2 的问题

GFF2 的问题之一是它只能表示一层嵌套的特征。在处理具有多个可变剪接转录本的基因时，这主要是一个问题。GFF2 无法处理基因 → 转录本 → 外显子的三级层次。大多数人通过声明一系列转录本并赋予它们相似的名称来解决这个问题，以表明它们来自同一个基因。

第二个限制是，虽然 GFF2 允许创建两级层次结构，例如转录本 → 外显子，但它没有任何层次结构方向的概念。所以它不知道外显子是否是转录本的子特征，反之亦然。这意味着必须使用 "aggregators" 来整理关系。出于这个原因，GFF2 格式已被弃用，取而代之的是 GFF3 格式数据库。

---

## GTF

GTF 全称为 *gene transfer format*，主要是用来对基因进行注释。

| 项目 | 内容 |
|:---:|:---|
| 文件格式 | GTF |
| 文件扩展名 | file.gtf |

GTF 与 GFF 文件具有相同的格式。它具有描述基因/转录相关特征的相同 9 列。

group/attribute 字段已扩展为属性列表。每个属性都由一个 type/value 对组成。属性必须以分号结尾，并且与任何后续属性之间仅用一个空格分隔。

属性列表必须以两个必需属性开头：

- **gene_id value** — 序列基因组来源的全局唯一标识符。
- **transcript_id value** — 预测 transcript 的全局唯一标识符。

---

## 总结

本文介绍了生物信息学分析中八种最常用的文件格式：

| 格式 | 用途 | 类型 |
|:---:|:---|:---:|
| **FASTA** | 序列存储（核酸/氨基酸） | 文本 |
| **FASTQ** | 序列 + 质量值存储 | 文本 |
| **SAM** | 序列比对结果 | 文本 |
| **BAM** | 序列比对结果（压缩） | 二进制 |
| **VCF** | 变异位点信息 | 文本 |
| **BCF** | 变异位点信息（压缩） | 二进制 |
| **GFF** | 基因组注释（通用） | 文本 |
| **GTF** | 基因组注释（基因/转录本） | 文本 |

*原文链接：https://mp.weixin.qq.com/s/rHpgHirq3r_XPuFLJAw_vQ*
