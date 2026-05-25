---
source_url: https://mp.weixin.qq.com/s/DjJI1pbl4JxQe8Laz1HlKQ
ingested: 2026-05-07
sha256: dcb66a0f6457569214cfd7c8faebf88f2280c99da37de0e5b973867996823530
citation: "Hu L, Qin H, Zhang Y, Lu Y, Qiu P, Guo Z, Cao L, Jiang W, Shen Y, Chen Q, Shang Y, Xia T, Deng Z, Zhao H, Xu X, Fang S, Li Y, Zhang Y. RegFormer: a single-cell foundation model powered by gene regulatory hierarchies. Nat Commun. 2026 May 5. doi: 10.1038/s41467-026-72198-x. Epub ahead of print. PMID: 42086551."
domain: ai-drug-discovery
---
# Nature Commun. | 突破长序列瓶颈！华大发布首个Mamba架构单细胞大模型RegFormer

**作者**：YLLian  
**发布平台**：微信公众号「生物信息与人工智能」  
**论文来源**：*Nature Communications*  
**论文标题**：*RegFormer: a single-cell foundation model powered by gene regulatory hierarchies*  
**DOI**：https://doi.org/10.1038/s41467-026-72198-x  
**原文链接**：https://mp.weixin.qq.com/s/DjJI1pbl4JxQe8Laz1HlKQ

---

## 论文导读

2026年4月，**华大生命科学研究院**研究团队在《Nature Communications》上发表题为《RegFormer: a single-cell foundation model powered by gene regulatory hierarchies》的研究。

该研究实现了**基因调控网络先验**与**Mamba架构**的创新融合，构建了突破长序列建模瓶颈的单细胞基础模型，在以下任务上全面实现了性能跃升：

- 跨组织细胞聚类
- 精准调控网络重建
- 动态基因扰动推演
- 药物敏感性预测

---

## 研究背景

单细胞转录组测序（scRNA-seq）技术极大地推动了我们对细胞异质性与发育动态的认知。然而，目前单细胞计算模型普遍存在重大技术瓶颈：

- 主流基础模型（如基于Transformer的**scGPT**等）大多直接套用自然语言处理算法
- 面临**两重困境**：
  1. 无法处理无序基因表达背后的复杂**非线性层级调控关联**
  2. 注意力机制的**二次计算复杂度**，严重限制了全基因组级别的长序列建模

---

## 主要结果

### Part 01 | 融合调控先验与Mamba架构的单细胞底层设计

为打破传统序列模型缺乏生物学网络感知以及注意力机制带来的长序列计算瓶颈，研究从头构建了全新的单细胞大模型计算基座：

- **拓扑排序策略**：摒弃常规随机基因排列，引入基于图论的"拓扑排序"策略，使输入模型的基因序列严格遵守底层真实的转录级联方向
- **双重嵌入机制**：在特征编码阶段，结合"定量表达值"与"调控身份标识"的双重嵌入
- **Mamba模块替代Transformer**：采用具备**线性时间复杂度**的Mamba状态空间模块替代传统Transformer作为核心编码器

---

### Part 02 | 跨组织细胞异质性的高维精准解构

对多种具有复杂异质性和批次效应的组织数据集进行高维降维与评估，结果表明：

- 基于调控拓扑排序与Mamba架构提取的细胞特征向量，在**区分罕见细胞亚型**及**消除批次干扰**方面显著超越现有主流单细胞大模型
- 底层方法：采用"表达值 + 调控身份"的**双重嵌入（Dual-embedding）**策略输入Mamba模块，完整保留了长距离的转录模块依赖关系

---

### Part 03 | 高保真基因调控网络模块的从头重建

通过提取预训练模型的基因嵌入特征并计算多维相似度，成功反向推导出具有高度生物学连贯性的细胞特异性基因调控网络：

- 模型不仅拟合表层数据共表达模式，而是通过**"调控拓扑预测"等自监督任务**，将上游转录因子（如主效因子 **GATA3** 和 **SPI1**）与下游靶基因的层级级联关系刻画到了高维潜空间中

---

### Part 04 | 基因组扰动下转录级联反应的动态精准推演

结合 **GEARS 推断框架**，模型在模拟 CRISPR 靶向敲除后的基因表达变化中展现出极高的预测保真度：

- 受扰动的转录因子与其引发差异表达的靶基因，在模型特征空间中维持着**紧密的几何距离（余弦相似度极高）**
- Mamba架构所捕获的状态空间动态，精准映射了扰动信号在真实细胞层级网络中的**级联传导逻辑**

---

### Part 05 | 跨癌种药物敏感性的机制驱动预测

将模型生成的细胞系转录组结构化嵌入与**药物分子图卷积网络（GCN）**进行多模态融合后：

- 在 **IC50 药物半抑制浓度预测**上取得了同类极佳的准确率表现
- 突破性亮点：模型不仅能提供精准数值，还能将预测的敏感性直接溯源映射到特定信号通路上，例如：
  - **NF-κB 信号通路**
  - **p53 凋亡通路**

---

## 参考论文

> **DOI**: https://doi.org/10.1038/s41467-026-72198-x
