# bioRxiv｜蛋白设计有统一工作台了！Ovo把生成、筛选、质控和管理接到了一起

> 原创 徐东，Jonty · AI药物设计实验室

🔔关注我们，每日分享最新的人工智能与生命科学论文～

---

做 de novo protein design，难点已经不只在模型本身。

现在工具很多，脚本很多，输出更多。任务一上规模，工作流怎么提交、结果怎么统一存、候选怎么按阈值筛、参数怎么回头追，都会变成硬问题。

这篇文章要解决的，就是这条链路。作者提出了 **Ovo**，一个开源的蛋白设计生态，把 scaffold design（骨架设计）、binder design（结合蛋白设计）、binder diversification（结合蛋白多样化） 和 ProteinQC（蛋白质质量控制） 放进同一个平台里。

它想做的，是把"模型拼接"变成"流程化设计"。

---

## Ovo 的架构

> **Figure 1**：a为平台架构，b为前端交互，c为内置工作流，d为插件扩展。

Ovo 有四个核心部件：**数据库、Nextflow 工作流、Web app、CLI/Python 接口**。

- **数据库**：负责存 design（设计条目）、descriptor（描述符）和任务参数
- **Nextflow**：负责把不同模型串起来，并把任务发到本地、HPC（高性能计算）或 cloud（云）环境
- **Web app**：负责分步提交任务、看结构、拉阈值
- **CLI 和 Python 接口**：更适合熟悉脚本的用户做批量任务和下游分析

**使用流程：**

1. 用户先选入口，再选任务类型，然后上传 PDB、选链、设 hotspot（热点位点）或 contig（片段连接定义）、给定长度范围和筛选条件。
2. 任务提交后，Nextflow 在后端并行执行。
3. 结果会回写到数据库和存储层，每个 design 都有稳定 ID，后面可以按 pool、round、project 继续组织。
4. 跑完以后，用户先在 "Job Results" 里看结构和分数，再到 "Designs" 里调阈值、筛候选。
5. 最后把通过的设计送去 ProteinQC，或者导出到 Jupyter 做下一轮分析。

> **Table 1**：方法栈清单。骨架生成有 RFdiffusion，序列设计有 ProteinMPNN 和 LigandMPNN，复合物和单体验证有 AlphaFold2，binder 协同设计有 BindCraft，界面和能量打分有 PyRosetta，质控侧还有 ESM-1v、ESM-IF、DSSP、PEP-Patch、APBS 和 Protein-Sol。

---

## 1. 功能位点骨架设计

**适合的场景：**

你已经有一个活性位点，或者有一段想保留的结合界面，现在想围着它重新长出一个新骨架。

**工作流：**

1. 先给一个源结构，选定要固定的残基片段。
2. 然后写 contig，定义这些固定片段怎么连接，中间设计多少残基，N/C 端再补多少残基。
3. 接着可以先用少量 RFdiffusion 步数做预览，确认 contig 没问题。
4. 确认后进入正式流程：RFdiffusion 生成 backbone，LigandMPNN 或 ProteinMPNN 设计 sequence 和 side chain，再用 AlphaFold2 或 ESMFold 做 refolding 回折验证和筛选。

这里有一个很实用的功能，叫 **sequence inpainting（序列重绘）**。有些残基的 backbone 想保留，但原序列不想锁死，就可以把这些位置对 RFdiffusion 隐藏，后面再交给 ProteinMPNN 重设计。

**案例结果：**

- oxidoreductase 活性位点任务：共生成 4800 条序列，最后过了 **13 个**
- PD-1 界面任务：生成了 5000 条，过了 **275 个**；不做 inpainting 时，只过了 **1 个**

> **Figure 2**：左边是输入 motif 或界面，中间是围绕它长出来的新骨架，右边再用 AF2 的 PAE、pLDDT、Design RMSD、Native Motif RMSD 去筛。

> 💡 **建议**：如果你要用这条流程，先跑 100–200 个 backbones 做试跑，把输入和参数先确认一遍，再放大规模。

---

## 2. Binder 设计

Ovo 里 binder 设计有两条路，一条是 **RFdiffusion 路线**，一条是 **BindCraft 路线**。两条都能做 mini-protein（迷你蛋白）和 linear peptide（线性肽）binder 设计。

### RFdiffusion 路线

这条路更像标准的生成式设计流程。

- 先给 target 结构，选 target chain，必要时裁剪 target 来省算力，再设 binder 长度范围和 hotspot。
- 之后 RFdiffusion 在 target 表面生成 binder backbone，ProteinMPNN + FastRelax 或 LigandMPNN 负责序列和侧链，再用 AlphaFold2 multimer initial guess 做复合物验证，同时用 PyRosetta 算 ddG、界面面积和 SAP 等指标。

**筛选指标（论文默认阈值）：**

| 指标 | 阈值 |
|------|------|
| interaction PAE（界面预测对齐误差） | < 10 |
| pLDDT | > 80 |
| target-aligned binder RMSD | < 2 Å |
| Rosetta ddG | < -30 |

### BindCraft 路线

BindCraft 走的是 **sequence-structure co-design（序列—结构协同设计）**。

- 先通过 AlphaFold2 backpropagation（反向传播优化）直接生成 binder 的序列和构象
- 再用 SolubleMPNN 或 ProteinMPNN 重采样非界面位点
- 最后用 AF2 monomer 和 PyRosetta 过滤

它通常更容易在单位算力下产出通过设计，但**更吃 GPU 显存**。

> **Figure 3**：左半边是 RFdiffusion 路线，右半边是 BindCraft 路线。两条路的输入都类似，都是 target + hotspot。

**胰岛素受体案例结果：**

- RFdiffusion 路线：共做了 4000 条序列，最后过了 **5 个**
- BindCraft：在单张 A10G GPU 上跑 6 小时，拿到 **13 个**通过默认过滤的设计

> 💡 **简单判断原则**：
> - 想做**大规模采样** → 用 RFdiffusion
> - 想直接走**协同设计**，GPU 显存也够 → 用 BindCraft

---

## 3. 部分扩散（Binder Diversification）

部分扩散对应 **binder diversification**。

**适合的场景：**

这个流程适合**第二轮优化**。你已经有一个 binder-target 复合物了，现在想围着现有设计继续采样，看能不能把亲和力、几何匹配或整体性质再往前推一步。

**工作流：**

1. 给一个或多个 binder-target 复合物，可以直接上传 PDB，也可以用 Ovo 内部 design ID。
2. 然后决定是重做整个 binder，还是只改一段残基。
3. 接着用 partial RFdiffusion（部分扩散）先加噪声，再去噪，得到和原 binder 相近但不一样的新 backbone。
4. 后面还是接 ProteinMPNN + FastRelax，再用 AF2 和 Rosetta 过滤。

> ⚠️ **注意**：部分扩散是**固定长度重设计**。输入和输出一一对应，整体长度和残基顺序都保留。它更像围着已有解做局部搜索。

> **Figure 4**：展示了两种玩法：全 binder 重做，或者只改非界面区域。

**案例结果：**

- 拿三个 AF2 置信度最高的胰岛素受体 binder 做部分扩散
- 450 个设计里有 **37 个**通过默认过滤，in silico 成功率是 **8.2%**
- 同任务的 full diffusion（全扩散）成功率只有 **0.075%**

> 这说明手里有可用 binder 时，继续在它附近采样，效率会高很多。

---

## 4. ProteinQC

ProteinQC 是 Ovo 里很关键的一层。

它把你的设计放进一个 **reference set（参考集合）** 里看，判断它在序列、表面和结构这些维度上处在什么位置。

- 可以直接接在设计工作流后面跑，也可以单独对一批用户上传结构跑。
- 输出是一整套 descriptor（描述符），再和 PDB 全库或子集的分布做对照。

**描述符分类：**

| 类别 | 关键指标 |
|------|----------|
| **稳定性相关** | sequence entropy（序列熵）、ESM-1v likelihood、ESM-IF likelihood |
| **表达/溶解性相关** | ProteinSol、疏水表面斑块比例、pH 7.4 下净电荷 |
| **结构相关** | helix%、sheet%、radius of gyration（回转半径）、asphericity（非球形度） |

> **Figure 5**：ProteinQC 分析。

**论文分析结果（361 个通过过滤的 design）：**

- 序列熵偏低，说明有些序列带重复片段
- ESM-1v 落在 PDB 分布范围里，但更靠低尾
- ESM-IF 基本落在中间
- 溶解性和疏水表面指标整体接近自然蛋白
- 结构维度上大体也接近 PDB，只是拖出一条细长 helical bundle（螺旋束）尾部

作者还把 ProteinQC 用到抗体案例里，**Hydrophobic SAP 和 HIC 保留时间有相关，Spearman ρ = 0.58**。

> ProteinQC 让你筛选时不只看 AF2 分数，也能把表达、溶解性、表面疏水和几何形状一起看进去。

---

## 5. Plugins（插件系统）

论文把 plugin 定义得很宽：它可以是一整条 Nextflow pipeline（工作流），也可以是某个步骤的替代方法，也可以是一个分析工具，甚至只是一个新页面。

**一个插件需要四样东西：**

- 一个前端页面脚本
- 一个工作流定义类
- 一些 descriptor 定义
- 一个 Nextflow pipeline

这样做模型开发者可以独立发插件，用户用 `pip install` 装上就能接进 Ovo。

**论文接入的两个示例：**

- **ProteinDJ**：用来说明新的 de novo 设计流程可以接进来
- **Promb**：用来说明分析类工具也能接进来

---

## 总结评述

这篇文章更像平台说明书。读它时，重点应该放在三件事上：

1. **它把蛋白设计从"脚本能跑"推进到了"整条链路能管"**。这对大规模项目很重要。
2. **它把 Web、CLI、Python 三个入口都留了出来**。新手和熟手都能用。
3. **ProteinQC 和 plugins 这两层补得很到位**。前者负责设计后筛选，后者负责后续扩展。

---

## 边界与局限

Ovo 能跑在本地，但平台真正的效率还是建立在 HPC 或云资源上。很多底层方法本身就很吃算力。它把流程标准化了，但有些高度定制的需求还得自己扩。

---

## 论文信息

- **标题**：Ovo, an Open-Source Ecosystem for De Novo Protein Design
- **作者**：David Prihoda, Marco Ancona, Tereza Calounova, Adam Kral, Lukas Polak, Hugo Hrban, Nicholas J. Dickens, Danny A. Bitton
- **机构**：MSD Czech Republic；Amazon Web Services
- **代码**：[https://github.com/MSDLLCpapers/ovo](https://github.com/MSDLLCpapers/ovo)
- **文档**：[https://ovo.dichlab.org/docs](https://ovo.dichlab.org/docs)

---

> 来源：微信公众号「AI药物设计实验室」
> 原文链接：https://mp.weixin.qq.com/s/lX6sxunapNP7AtcQ9lABdg
