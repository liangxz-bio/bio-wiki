---
source_url: https://mp.weixin.qq.com/ (微信公众号·生信钱同学)
ingested: 2026-05-19
sha256: 20589c06e16ccd556fd2345ba26bb48566c649a9e7673c44b31ae09370b2d143
citation: "Chen SQ, et al. Dissecting epigenetic heterogeneity in single-cell DNA methylomes with a unified framework. Nature Communications (2026)."
domain: bioinfo-pipeline
---

# Nat Commun | 单细胞甲基化数据不会分析？scMethCraft这款统一框架神器太全能了

> **来源：** 生信钱同学

南开大学数学科学学院陈盛泉教授团队构建了新型计算框架sc-MethCraft，攻克了单细胞DNA甲基化数据存在显式缺失、数据分布复杂、传统算法适配性差的痛点。该框架依托混合神经网络并融合了基因组序列特征完成建模，可实现单细胞DNA甲基化数据增强、细胞聚类、多批次数据整合、差异甲基化区域挖掘等多项分析，助力单细胞水平表观异质性与基因调控机制研究。

💡 日常好的生信代码已放入服务器的公共文件夹中：https://vip.r-py.com/

![](https://mmbiz.qpic.cn/sz_mmbiz_png/s9PJ3TYvaia3ep2GB3PSZgZ8rAibklT1UxgHiaYplSpL3uXE0ThSUklyDIFibxic65LyP9ibuXGSuXYOWSqmOz2Ux8dhVnCsd2EmKtqYsf7pUszSY/640?wx_fmt=png&from=appmsg)

## 🔥 核心突破
一起来看看寻因生物的单细胞DNA甲基化测序技术技术，该为解析细胞表观遗传异质性提供了前所未有的机遇，但长期以来，该领域一直缺乏专门针对其数据特性的分析工具。传统方法多直接套用单细胞转录组或单细胞ATAC的分析框架，忽略了scDNAm数据中广泛存在的显性缺失值以及其独特的连续比率分布特征，导致建模效果不佳、生物学信息丢失严重。本研究提出的scMethCraft框架，从根本上解决了这一痛点。该工具直接接受包含显性NA值的scDNAm矩阵作为输入，创新性地构建了混合神经网络架构，不仅整合了DNA序列特征与基因组位置信息，还通过迭代更新的细胞间相似度模块精准量化细胞关联。scMethCraft实现了从数据降维、聚类、批次整合、信号增强到差异甲基化区域识别的全流程覆盖，显著提升了scDNAm数据的分析精度与深度。更重要的是，研究利用该工具在人类大脑少突胶质细胞中鉴定出了现有数据库中未被充分表征的低甲基化区域及相关基因，为探究细胞潜在的生物学机制提供了全新的表观遗传视角。

![](https://mmbiz.qpic.cn/mmbiz_png/s9PJ3TYvaia0bmmXqkDS7XSuic3urt0YabyUJtpCbB6ukfRKou5ptmlVVmaHlA8TQsIzGQr92cGMxr6mWUkcnyLeoRKvXOibhQ2TbRj9j8zzjI/640?wx_fmt=png&from=appmsg)

![](https://mmbiz.qpic.cn/sz_mmbiz_jpg/s9PJ3TYvaia2MEeWPq4TiaOI4hq0tnVfz7mNbk0ibrMteD6hL3t1P0H8xQZhFpop6niaIyFtwgNxw6nZzycLGz6Fu0hNx5ncYibVMqzRYib7icu2PE/640?wx_fmt=jpeg&from=appmsg)

文章摘要：scMethCraft通过混合神经网络和迭代相似性加权模块，精准建模单细胞DNA甲基化数据，实现细胞嵌入、数据整合、信号增强及差异甲基化区域识别的统一分析框架

## 📚 研究背景
单细胞DNA甲基化测序开启了在单细胞分辨率下探索表观遗传异质性的大门。在哺乳动物基因组中，DNA甲基化主要发生在CpG位点，其全基因组图谱对于揭示细胞多样性、基因调控及疾病致病机制至关重要。目前，分析scDNAm数据的常见做法是先将基因组划分为固定长度的bin，计算每个bin内的平均甲基化水平，从而构建细胞-by区域的甲基化矩阵。然而，针对该矩阵的分析方法多借鉴自scRNA-seq或scATAC-seq的常规流程。这种"张冠李戴"的做法存在严重缺陷：首先，scDNAm矩阵含有大量显性缺失值（NA），即某区域完全无reads覆盖，这与scRNA-seq中未观测到计数即视为0的逻辑截然不同；其次，scDNAm的值是0到1之间的连续比率，而非计数分布；最后，在数值尺度与生物学语义上，scDNAm中数值大并不代表信号强，反而常对应低表达。这些差异导致传统流程无法最优捕捉scDNAm矩阵中的生物学信息，亟需开发专门针对其数据特性的综合分析工具。

## 🔬 技术创新
- **混合神经网络特征提取**：scMethCraft引入了卷积神经网络（CNN）与全连接神经网络（FCNN）来多视角捕获DNA序列特征，并创新性地结合Kolmogorov-Arnold网络（KAN）进行特征融合，同时将基因组位置信息嵌入最终的特征表示中，极大增强了对甲基化模式的表征能力。
- **迭代细胞间相似性加权**：提出了一种相似性加权模块，通过迭代计算并更新细胞间的关联矩阵，利用细胞群体间的共识信息来增强甲基化水平的预测，有效降低了预测偏差并提升了模型精度。
- **原生NA值处理机制**：区别于传统方法将缺失值填充为0或丢弃，scMethCraft的框架直接在包含显式NA值的矩阵上运行，在去噪的同时对NA值进行合理插补，保留了数据的真实生物学结构。

## 📊 实验结果

### scMethCraft模型架构与核心机制
研究首先详细展示了scMethCraft模型的整体架构。模型以scDNAm细胞-by-region矩阵作为输入，针对每个基因组区域提取对应的DNA序列并获取序列编码。在序列特征提取模块中，CNN和FCNN分别从不同维度捕获序列特征，随后KAN网络融合这些特征并引入位置信息，生成50维的特征嵌入，再通过密集线性层预测各细胞的甲基化水平。进一步，相似性加权模块基于细胞间关联迭代增强预测结果。最终，模型输出增强的scDNAm矩阵、细胞间相似性矩阵以及用于下游分析的50维细胞嵌入。该架构设计从底层逻辑上契合了scDNAm数据特性，为后续多维度的分析任务奠定了坚实基础。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/s9PJ3TYvaia0FMD51zeB1HxygWCmKf2SeZ2gvw3x9AEzgRiak15XeT1Qg4azLmYy31MWmgUDyBeBIhw5eQYibGnOwY3B5FT3oapFERb8E6PsYg/640?wx_fmt=png&from=appmsg)

Figure 1：scMethCraft模型的整体架构与核心模块设计，包括序列特征提取和迭代相似性加权过程

### 细胞嵌入与聚类性能评估
为了验证scMethCraft在降维和聚类方面的表现，研究团队在多个真实scDNAm数据集上进行了测试。结果显示，相较于直接使用传统PCA或其他单细胞降维方法，scMethCraft提取的细胞嵌入能够更清晰、准确地分离不同的细胞类型。通过聚类指标评估，scMethCraft在ARI、NMI等得分上均显著优于现有方法，证明其能够有效剔除技术噪声，捕捉到深层保守的表观遗传异质性，使得细胞亚群的界定更加精准可靠。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/s9PJ3TYvaia1vGd3QWcQ2j7e6ICuq1uqBia7QE9o7O80HOoFHy8DX7c2g6iaDpMoRa0bM6hOgd1cblhDmaS4NpJxYWmpS2n5SK83kialVx3LeZI/640?wx_fmt=png&from=appmsg)

Figure 2：scMethCraft在多个数据集上的细胞嵌入与聚类性能评估，展示其比传统方法更优异的细胞类型分离效果

### 多源数据整合与批次效应消除
单细胞数据往往面临严重的批次效应，scDNAm数据也不例外。研究评估了scMethCraft在整合来自不同数据源、不同测序平台及不同供体的scDNAm数据时的能力。实验表明，scMethCraft的相似性加权模块在迭代过程中能够有效学习并消除不同批次间的技术差异，将相同细胞类型的细胞在嵌入空间中紧密对齐。这种强大的整合能力为跨数据集的联合分析提供了可靠保障，极大提升了数据利用率。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/s9PJ3TYvaia2HCzOYUIIttYyraPL9YGsWn0oTZLgDpu29xgBhCnibXsu2lRlOicHgmNicsV62bDMuBLobaFic7geHDTX5oemowkTxaP8VANnlMibo/640?wx_fmt=png&from=appmsg)

Figure 3：scMethCraft在多源scDNAm数据整合中的表现，成功消除批次效应并实现对齐相同细胞类型

### 跨数据集细胞类型注释
基于强大的数据整合与嵌入能力，scMethCraft进一步支持了跨数据集的细胞类型注释任务。研究者利用参考数据集的细胞标签，通过scMethCraft的嵌入空间映射，对查询数据集中的未知细胞进行精准注释。结果表明，scMethCraft的注释准确率远超未适配甲基化数据的常规流程，有效解决了scDNAm领域参考图谱匮乏、细胞身份难以界定的难题，为新生成数据的快速解读提供了捷径。

![](https://mmbiz.qpic.cn/mmbiz_png/s9PJ3TYvaia1xusHbliccyMIJciafyooADMOGdVgicXbIJvMOic8ibd2sKWQB611JXvWvzNcoVAQlKUu9CMh0kjcxrT2poFHyX6eKFicy18eQ3icuRs/640?wx_fmt=png&from=appmsg)

Figure 4：基于scMethCraft嵌入空间的跨数据集细胞类型注释，展现出高准确率的标签转移能力

### 表观遗传信号增强与NA值插补
针对scDNAm数据极度稀疏且存在大量NA值的问题，scMethCraft展示了其信号增强与插补的强大功能。通过引入细胞间相似性信息，模型能够在去噪的同时对缺失区域进行合理插补。与简单的均值插补或其他插补算法相比，scMethCraft恢复的甲基化水平更接近真实状态，有效提升了信噪比，重构了更完整的单细胞DNA甲基化图谱，为后续的机制解读提供了高质量的数据支撑。

![](https://mmbiz.qpic.cn/mmbiz_png/s9PJ3TYvaia1MDb2FGdpZjnHZDaKaHiaUtChUKsV1kx4Lk3aZIZLaztIXVU9aKaEcIFe0mXRBcMzKW7AHOWCq4AHDekOTpg1iaonAQul6JWmIg/640?wx_fmt=png&from=appmsg)

Figure 5：scMethCraft对scDNAm矩阵的信号增强与NA值插补效果，显著提升了信噪比

### 差异甲基化区域（DMR）精准识别
寻找不同细胞类型间的差异甲基化区域是揭示基因调控机制的关键。scMethCraft利用增强后的甲基化矩阵，能够高精度地识别细胞类型特异性的DMR。测试结果显示，scMethCraft鉴定的DMR不仅具有更高的统计显著性，而且在基因组功能区域的富集上更具生物学合理性。与现有方法相比，它有效降低了假阳性，捕捉到了更多细微但关键的表观遗传差异。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/s9PJ3TYvaia2FogbibWCDkjm9NA3BrTuxbLzwzZJ7nWrXUGWwmHV3udKsY6lF6omfIaLPEGLlibYe7g98M4OkeHibn6ogFZ4HlJfiakve269KwPs/640?wx_fmt=png&from=appmsg)

Figure 6：scMethCraft在识别细胞类型特异性差异甲基化区域（DMR）方面的优越性能

### 生物学功能富集与组织特异表达分析
在鉴定出特异性DMR后，研究进一步将其与基因调控程序相联系。通过生物功能富集与组织特异表达分析，scMethCraft成功揭示了多种人类大脑细胞类型的关键生物学过程。结果显示，不同细胞类型的DMR相关基因显著富集于与其生理功能匹配的GO通路中，且这些基因在大脑特定区域的表达呈现高度特异性，验证了scMethCraft挖掘出的表观信号具有深刻的生物学意义。

![](https://mmbiz.qpic.cn/sz_mmbiz_png/s9PJ3TYvaia250tibjYxV4XyynT5uwqCBupjicCM7VuZ3gmhsxydjpL6IjcD6GjN6iaswKPhCu545sxmClnxsWI0gDYJ64zzfcTCaTQ9CibfJ8FQ/640?wx_fmt=png&from=appmsg)

## 💡 应用前景和未来展望
scMethCraft为单细胞DNA甲基化数据提供了一站式的分析解决方案，极大地降低了该领域的数据挖掘门槛。其在细胞类型注释、多源数据整合及DMR识别上的出色表现，将直接加速发育生物学、神经科学及肿瘤微环境中的表观调控机制研究。未来，随着单细胞多组学技术的发展，scMethCraft有望进一步拓展至多模态联合分析，实现甲基化与转录组、染色质可及性的深度对齐，为构建更全面的细胞状态图谱提供核心算法支撑。

## 🔍 生信视角解读
从生信算法的角度看，scMethCraft最大的亮点在于打破了"套用scRNA-seq流程"的惯性思维，针对scDNAm数据的NA值和连续比率分布进行了底层架构重构。其引入的KAN网络融合位置编码，以及迭代更新的细胞相似性加权机制，巧妙地解决了极度稀疏条件下的特征表征难题。然而，混合神经网络架构不可避免地带来了较高的计算开销，对大型数据集的内存消耗较大。后续研究可借鉴其NA值原生处理的思路，开发更轻量级的图神经网络或矩阵分解模型，以在保证精度的同时提升计算效率。

**互动问题：** 在你的单细胞甲基化数据分析中，最让你头疼的是缺失值处理、批次效应还是细胞注释？欢迎在评论区分享你的经验！

👇 关注「公众号」，每日获取前沿生信研究解读

**📚 文献引用：**Dissecting epigenetic heterogeneity in single-cell DNA methylomes with a unified framework. Nature Communications, 2026.

![](https://mmbiz.qpic.cn/mmbiz_jpg/s9PJ3TYvaia39OeVCFiaO6m62dInsthYyy7ibm1dQ6RzUINY64MCHyhjup8AKTtoiagfSw47EU33KpCK04FTOibIRgM7VhHicLThxGaMYlCibGIYhI/640?wx_fmt=jpeg&from=appmsg)

**
