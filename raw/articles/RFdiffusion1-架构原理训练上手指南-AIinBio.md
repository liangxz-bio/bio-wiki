---
source_url: https://mp.weixin.qq.com/s/9FPcxIhjS2raHPDEwMqmeQ
ingested: 2026-05-13
sha256: 6e814a56697d65b23d2cebb0b48c35b5cbc1a1c7759e7444fd8332b74495d06a
citation: "RFdiffusion Series Part 1 — Architecture, Training and Getting Started Guide. WeChat Article. AI in Bio (Vigour123). 2026."
domain: ai-drug-discovery
tags: [protein-design, rfdiffusion, diffusion-model, tutorial, ai-drug-discovery]
---

# 精读经典｜万字详解 RFdiffusion1：架构原理、训练过程和上手指南（一）

> 原文链接：https://mp.weixin.qq.com/s/9FPcxIhjS2raHPDEwMqmeQ
> 来源：AI in Bio（Vigour123）

---

## 1. RFDiffusion1 模型架构

### 1.1 微调 RoseTTAFold1

RFDiffusion1 复用了 RoseTTAFold1 的三轨结构，并加载 RoseTTAFold1 已训练权重进行微调，仅修改输入输出适配扩散任务。

- **输入替换**：从【MSA + 序列】改为【带噪 3D Frame + 时间步编码】
- **输出替换**：从【结构预测】改为【噪声残差 / 干净 Frame 预测】

### 1.2 SE(3) 扩散（加噪）

**两种噪声：**

- 平移噪声：普通高斯（欧氏空间）
- 旋转噪声：IG-SO(3)（流形上高斯）

**残基表示：**Cα 位置 + 局部坐标系

**前向加噪：**

- Cα：加普通高斯
- 局部坐标系：加 IG-SO(3) 旋转噪声

### 1.3 几何引擎：SE(3)-Transformer 等变模块

在 RoseTTAFold1 中，SE(3) 是附加精修模块；而 RFdiffusion1 整个网络从输入到去噪输出全程应用 SE(3) 等变。

### 1.4 约束系统

- ContigMap 约束系统：定义固定区域、设计区域、模板锚定区域，实现基序条件生成（motif scaffolding）
- 序列 / 骨架掩码机制

### 1.5 迭代优化

**自条件机制（Self-conditioning）：**将上一步去噪预测结果回传至当前输入，提升迭代稳定性与结构精细度。

**迭代模拟器（Iterative Simulator）：**去噪后结构局部优化，修正键角、二面角不合理构象。

### 1.6 训练和推理

- 在训练时，加噪是通过公式计算一步完成的；去噪必须逐步骤迭代，从纯噪声到合理结构，每一步 RosettaFold 预测一次残基旋转和坐标，一步步收敛到天然合理蛋白三维结构。
- 在推理时，直接从纯高斯噪声起步，随后遵循与训练一致的反向马尔可夫链流程，由后向前逐步迭代去噪（50/200 步），最终还原出完整蛋白三维结构。
- 训练是有真实结构做监督的逐步去噪学习；推理是无监督、自回归迭代，逐步去噪生成。

---

## 2. RoseTTAFold1

由于 RFDiffusion1 复用 RoseTTAFold1，并加载其权重进行微调，有必要单独研究一下 RoseTTAFold1。

### 2.1 氨基酸残基骨架的表示方法

蛋白质主链由一个个氨基酸残基串联而成，RoseTTAFold1（RF1）采用**刚性残基框架**对蛋白质主链进行建模，每个残基都基于 Cα 原子，使用 (r, z_Cα)（旋转 + 平移）表示。

z_Cα 是平移向量，代表该残基的 Cα 碳原子在三维空间的真实绝对坐标。

定义一个理想残基模板，在该理想模板中，Cα 原子位于坐标原点，Cα—C 沿 x 轴方向，Cα—N 位于 xy 平面内，将这个理想残基模板的坐标表示记作 z*。

对于任意给定残基的主链原子（C、N、Cα），都可以通过 Gram-Schmidt 正交化计算得到一个 3×3 旋转矩阵 r。

到这里，对于给定的残基，可以得到旋转矩阵 r，然后将该残基在三维空间中的真实绝对坐标表示为：z = z_Cα + r·z*，即 Cα 原子的真实绝对坐标 z_Cα 加上旋转矩阵 r 与理想残基模板坐标 z* 的乘积。

为什么将氨基酸残基表示为"理想残基模板 + 旋转矩阵 + 平移"的刚性框架？因为把复杂的原子坐标简化为 (r, z_Cα) 两个参数，大幅降低了模型学习的维度，同时保留了蛋白质主链的核心几何约束，此外，还可以实现"原子坐标↔刚性残基框架"的双向转换，既可以从坐标算框架，也可以从框架重建坐标，后者适配模型的结构生成需求。

### 2.2 RoseTTAFold1 三轨架构

RoseTTAFold1 采用三轨并行设计，同时利用序列进化、残基相互作用、空间结构三类关键信息，包含主三轨迹模块和结构精修模块。

**三轨：**

- 1D 序列轨道：提取序列进化与上下文信息
- 2D 残基对轨道：建模残基间距离与相互作用约束
- 3D 结构轨道：生成与细化三维坐标

**三轨迹模块包含：**

- 对 1D 序列信息、2D 残基间关系的注意力层；
- 更新 3D 结构坐标的 SE(3) 等变层；
- 1D/2D/3D 特征间的信息交互层。

SE(3)-equivariant layers（SE(3) 等变层）保证对输入结构做旋转/平移时，输出坐标会做相同的变换，完美符合蛋白质结构的物理不变性。

**结构精修模块：**

结构精修模块采用 SE(3) 等变网络，针对三轨迹模块输出的初步 3D 结构做迭代优化（如修正键长、键角，消除原子碰撞等）。SE(3) 等变网络保证每一轮迭代的坐标更新都符合旋转/平移不变性。每一轮迭代循环的 3D 输入，直接复用前一轮的预测坐标，实现"逐步精修"。

---

## 3. RFdiffusion1 的训练和在特定任务上的微调

### 3.1 RFdiffusion1 基础模型

RFdiffusion1 采用 RoseTTAFold1 训练所用的 PDB 单体蛋白质结构进行训练。训练样本中，无条件生成任务占 20%，基序条件生成任务占 80%。在基序条件任务中，选取一段连续的氨基酸残基作为基序（motif），并将其真实序列与结构信息输入模型。

**损失函数：** L = L_frame + L_dist

- L_frame：骨架帧坐标损失
- L_dist：二维距离 / 接触图损失

**包含 4 个关键几何自由度：**

- d_ij：残基 i 与 j 的 β 原子间的欧氏距离，限制在 0-18.5 Å 内。
- ω_ij：以两个残基的 β 原子连线为轴的二面角，由 α_i, β_i, β_j, α_j 四个原子定义，范围为 [−π,π]，用于描述相对扭转。
- θ_ij：由 Cα_i, Cβ_i, Cβ_j 三个原子定义的二面角，用于描述残基 j 相对于残基 i 的方位角。
- φ_ij：由 Cα_i, Cβ_i, Cβ_j 三个原子定义的平面角，范围为 [0,π]，用于描述残基 j 相对于残基 i 的极角。

**细节处理：**

- 训练过程中的基序中心化处理：在训练与 scaffold 生成阶段，均采取将基序中心化至坐标原点，而非将全局中心化至坐标原点。
- 为了避免其他损失项将基序从预设的初始位置偏移，在基序上完成距离损失计算后，基序坐标会从计算图中分离。
- 在扩散过程中对坐标进行降尺度处理：在每一步扩散前，将 x(t) 与 x(0) 的所有坐标值乘以 0.25，推理时再放大还原回原单位。

**训练：**RFdiffusion1 以训练完成的 RoseTTAFold1（RF1）权重为起点进行训练，模型不使用循环迭代（recycling）机制进行多次重排优化；训练 5 个 epoch 即可收敛，在 8 块 NVIDIA A100 GPU 上训练耗时 3 天。

**训练超参数：**

- 在骨架帧损失中，平移距离权重 = 0.5，旋转距离权重 = 1.0，对旋转误差的惩罚是平移误差的 2 倍，优先保证取向正确。
- 损失权重 L_dist = 1.0
- 扩散时间步数 T = 200
- 坐标缩放系数：0.25
- 每 epoch 训练样本数：25600
- 指数加权系数 γ = 0.99
- 学习率：0.0005，无预热；每 10000 optimization steps 后乘以 0.95
- 提供基序时的残基掩码比例：从 20% 到 100%（含）的均匀分布中随机选取。
- 基序为连续 / 不连续的概率：0.5

### 3.2 RFdiffusion1 在酶活性位点 scaffold 构建任务上的微调

针对酶活性位点支架设计的 RFdiffusion1 微调模型，以基础版 RFdiffusion1 为起点进行训练。

微调过程中，30% 的训练任务沿用基础模型的任务集，剩余 70% 为三重接触任务：

- 从单体结构中随机选取 3 个残基作为活性位点基序，要求这 3 个残基在序列上间隔均大于 10 个残基，且两两之间的 β–β 距离小于 6 Å。
- 这 3 个残基均被纳入基序，每个残基有 50% 的概率额外包含一个相邻侧翼残基。
- 若单体结构中无法找到符合条件的残基三元组（约 23% 的训练 PDB 结构存在此情况），则任务回退为基础模型的训练任务。

针对基础 RFdiffusion1 模型在处理极短输入基序时，有时无法在输出结构中固定基序位置的问题，该酶活性位点微调模型将基序特定位移损失权重提升了 10 倍，以此强制网络保持基序位置不变。该模型以此方式微调了 5 个 epoch。

### 3.3 RFdiffusion1 在蛋白质-蛋白质相互作用设计任务上的微调

**蛋白质 - 蛋白质界面热点残基：**目标（固定）链上，与结合（扩散）链上残基的 β–β 距离在 10 Å 以内的所有残基。

**模块邻接矩阵（block adjacency matrix）：**告诉模型哪些二级结构模块（α 螺旋、β 折叠、无规卷曲的连续区域）在空间上必须靠近，是控制蛋白质拓扑结构的核心约束。

**蛋白质折叠由两部分定义：**

- 其所包含的二级结构模块
- 这些模块之间的精确空间取向（平移与旋转）

**因此向模型提供以下两类特征：**

- 维度为 [L, 4] 的独热张量：每个位置被分配为一种二级结构类型 {螺旋，折叠，环区，掩码}
- 维度为 [L, L, 3] 的独热张量（称为模块邻接矩阵）：矩阵元素表示模块间是否在距离阈值内邻近 {非邻接，邻接，掩码}

**模块邻接矩阵中，两个模块被标记为邻接的条件是：**

- 两个模块均非环区
- 模块间任意残基对的 Cα–Cα 最小距离 ≤ 8 Å

**训练：**

- 以训练了 5 个 epoch 的基础版 RFdiffusion1 为起点进行训练；
- 训练任务包含 50% 的单体样本（单链蛋白）与 50% 的复合物样本（两条链或多链蛋白）。
- 当输入复合物样本时，模型仅对其中一侧进行加噪（noised）处理，另一侧目标蛋白保持固定（PPI 设计通用方式）。
- 在输入复合物样本时，模型还会获得固定链一侧界面中 0-20% 热点残基的残基索引信息，以便在推理时精准定位设计的结合蛋白。
- 对于加噪区域，模型均有 50% 的概率获得二级结构信息，且相互独立地有 50% 的概率获得模块邻接信息。
- 进一步，对给予的二级结构（以及相应的邻接信息，若提供）以 0-75% 的比例进行掩码，掩码长度为 1-8 个残基。

---

## 4. RFdiffusion1 基序条件生成任务的训练过程

在 RFdiffusion1 基础模型的训练中，无条件生成任务占 20%，**基序条件生成任务占 80%**。

### 4.1 基序和支架

RFdiffusion1 将基序支架（scaffold）构建视作一个条件生成建模问题，其将蛋白质结构中的残基划分为两部分：构成基序的残基和构成骨架其余部分的残基（基序的支架）。

用 S_m 表示基序残基的结构，S_s 表示支架残基的结构，完整的（未加噪）蛋白质结构表示为：S = [S_m, S_s]

### 4.2 基于基序条件的支架生成

**训练步骤：**

- 以完整结构 S 为起点；
- 随机将结构划分为基序与支架两部分；
- 对支架部分施加噪声，得到 S_s(t) ~ q(S_s(t)|S_s)；
- 输入 [S_m, S_s(t)]，由 RFdiffusion1 预测 S_0_pred，以原始结构 S = [S_m, S_s] 为监督计算预测损失。

**对基序残基 S_m 的处理：**

- 为了约束 RFdiffusion1 不改变基序的结构，将基序残基的时间步输入设为 t=0，使其不参与加噪过程。
- 在每一步迭代中，将 S_s(t) 中的基序主链坐标替换为未加噪的基序坐标。
- 在所有其他损失计算中，模型预测的基序坐标均被视为常量。
- 将基序 S_m 中心化至坐标原点。

### 4.3 总结与思考

(1) 酶活性位点支架构建任务和蛋白质-蛋白质相互作用设计任务都可以看作是基序条件生成任务的变体/具体实践。

(2) 扩散模型的本质是在高维概率分布中采样，由于它从纯随机噪声开始还原，路径有无数种可能，已经超越了单纯的模仿，具备了组合创新的能力。

(3) 尽管 RFdiffusion1 能创新，但它仍然受到训练集（PDB 数据库）的深度影响——生成 α-螺旋束非常拿手，但对于部分无序蛋白则很难生成得好。

---

## 5. 基于 external potentials 引导 RFdiffusion1 推理

除了能够基于基序进行条件生成外，还可以通过外部势函数引导推理过程，生成具备任意目标属性的蛋白质。

RFdiffusion1 可无需微调，通过额外外部势场给生成过程加约束；本质是在扩散反向迭代中，叠加目标属性分类概率的梯度，矫正结构走向；论文中势场只作用于 Cα 骨架，不干涉残基侧链取向，只约束整体拓扑与空腔/对称特征。

**带外部势场引导的生成算法：**

```python
 1: function SampleGuided(L, P, GuideScale):
 2:    ▷ 生成含L个残基的蛋白质主链结构，由势场P进行引导
 3:    从参考分布M中采样初始化带噪结构 x(T) = SampleReference(M)
 4:    for t = T, ..., 1 do:
 5:        通过RFdiffusion由当前带噪结构x(t)预测原始结构 ˆx(0)
 6:        执行扩散反向步，得到中间结构 x(t-1) = ReverseStep(x(t), ˆx(0))
 7:        叠加引导梯度：x(t-1) = x(t-1) + GuideScale(t)·∇_{x(t)}P(x(t))
 8:    end for
 9: return ˆx(0)
10: end function
```

---

## 6. RFdiffusion1 的 in silico 实验方法

### 6.1 工作流

RFdiffusion1 生成 100 组骨架结构，每组骨架通过 ProteinMPNN 生成 8 条候选序列，再用 AF2/ESMFold 预测结构进行初筛。

### 6.2 ProteinMPNN

- 结合剂/靶点结合蛋白设计的采样温度设置为 0.0001
- 其余所有任务的采样温度默认设置为 0.1
- 全程去掉半胱氨酸

### 6.3 AF2

AF2 均采用单序列预测模式，不输入多序列比对 MSA 结果。

### 6.4 计算机模拟成功率判定标准

**单体通用成功标准：**

AF2 预测骨架与 RFdiffusion1 设计骨架的 RMSD < 2 Å，AF2 pAE < 5；
含天然功能 motif 时，AF2 预测 motif 骨架与天然结构 RMSD < 1 Å，AF2 pAE < 5。

为什么使用 pAE < 5，不使用 pLDDT > 80？因为 pAE 筛选更严格，可规避长螺旋等高 pLDDT 但实际折叠稳定性差的边缘案例。

**细分任务专属判定标准：**

- Binder 结合蛋白：链间 pAE < 10，整体 RMSD < 1 Å
- 酶活性中心支架：链间 pAE < 10，整体 RMSD < 1 Å，侧链 RMSD < 1.5 Å
- 折叠约束设计：TM-score > 0.5
- 对称寡聚体 / 镍结合：pLDDT > 80 保底；镍结合再加侧链 RMSD

### 6.5 设计结构多样性评估

使用 TM-score 评估：

- 和现有 PDB100 库比：看有没有撞已知天然蛋白
- 内部互相比：看生成的 100 个设计是否各不相同

以两两 TM-score 0.6 为阈值，低于就算全新构象。

### 6.6 序列同源性分析

- 将所有设计序列使用 blastp 与 UniRef90 数据库比对
- 筛选 E-value < 1e-1 的同源结果
- 统计存在同源匹配的设计占比，以及非基序区域的序列一致性百分位数

命令示例：

```bash
blastp -query example_fasta.fasta \
-db /path/to/databases/uniref90 \
-evalue 1e-1 \
-num_threads $SLURM_JOB_CPUS_PER_NODE \
> output.fasta
```

### 6.7 指标含义

- **RMSD**：Root Mean Square Deviation，单位 Å，看局部精细结构是否一致
- **TM-score**：Template Modeling score，范围 0~1，看整体折叠是否一样
- **pAE**：Predicted Aligned Error，AF2 输出的全局折叠置信度指标
- **pLDDT**：predicted Local Distance Difference Test，AF2 每个氨基酸的局部结构置信度，范围 0~100

---

## 7. RFdiffusion1 环境配置

### 7.1 模型权重

```bash
git clone https://github.com/RosettaCommons/RFdiffusion.git
cd RFdiffusion
mkdir models && cd models

wget http://files.ipd.uw.edu/pub/RFdiffusion/6f5902ac237024bdd0c176cb93063dc4/Base_ckpt.pt
wget http://files.ipd.uw.edu/pub/RFdiffusion/e29311f6f1bf1af907f9ef9f44b8328b/Complex_base_ckpt.pt
wget http://files.ipd.uw.edu/pub/RFdiffusion/60f09a193fb5e5ccdc4980417708dbab/Complex_Fold_base_ckpt.pt
wget http://files.ipd.uw.edu/pub/RFdiffusion/74f51cfb8b440f50d70878e05361d8f0/InpaintSeq_ckpt.pt
wget http://files.ipd.uw.edu/pub/RFdiffusion/76d00716416567174cdb7ca96e208296/InpaintSeq_Fold_ckpt.pt
wget http://files.ipd.uw.edu/pub/RFdiffusion/5532d2e1f3a4738decd58b19d633b3c3/ActiveSite_ckpt.pt
wget http://files.ipd.uw.edu/pub/RFdiffusion/12fc204edeae5b57713c5ad7dcb97d39/Base_epoch8_ckpt.pt

# Optional:
wget http://files.ipd.uw.edu/pub/RFdiffusion/f572d396fae9206628714fb2ce00f72e/Complex_beta_ckpt.pt

# original structure prediction weights
wget http://files.ipd.uw.edu/pub/RFdiffusion/1befcb9b28e2f778f53d47f18b7597fa/RF_structure_prediction_weights.pt
```

### 7.2 安装 SE3-Transformer

以 (nvidia4090 + cuda11.8) 为例：

```bash
conda create -n SE3nv python=3.9 -y
conda activate SE3nv
pip install -U pip setuptools wheel -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install torch==2.0.0+cu118 torchvision==0.15.0+cu118 torchaudio==2.0.0 -f https://download.pytorch.org/whl/torch_stable.html
pip install dgl -f https://data.dgl.ai/wheels/cu118/repo.html
pip install hydra-core pyrsistent -i https://pypi.tuna.tsinghua.edu.cn/simple

cd ..
cd env/SE3Transformer
pip install --no-cache-dir -r requirements.txt

# github 访问受限，本地安装 dllogger
wget https://gitee.com/mirrors_nvidia/dllogger/repository/archive/master.zip -O dllogger.zip
unzip dllogger.zip
cd dllogger-master
pip install .
cd ..
rm -rf dllogger.zip dllogger-master
python setup.py install
cd ../..
pip install -e .

# 每次运行 diffusion 时激活
conda activate SE3nv
```

### 7.3 获取 PPI 脚手架示例

```bash
tar -xvf examples/ppi_scaffolds_subset.tar.gz -C examples/
```

---

## 8. 使用 RFDiffusion1 进行设计——上手指南

实际执行脚本为 `scripts/run_inference.py`，通过 Hydra 配置控制。

### 8.1 无条件单体

```bash
# 生成 10 个 100-200 aa 的蛋白质设计
../scripts/run_inference.py inference.output_prefix=example_outputs/design_unconditional 'contigmap.contigs=[100-200]' inference.num_designs=10
```

### 8.2 基序支架（Motif Scaffolding）

输入规则：

- `A10-25`：pdb 文件中 A 链上的残基 10-25
- 未加字母前缀表示待构建的蛋白质，可以以长度范围形式输入
- 使用 `/0` 指定链中断

示例：

```bash
# 构建 RSV-F 蛋白 site 5 的支架
../scripts/run_inference.py inference.output_prefix=example_outputs/design_motifscaffolding inference.input_pdb=input_pdbs/5TPN.pdb 'contigmap.contigs=[10-40/A163-181/10-40]' inference.num_designs=10
```

**酶活性位点支架构建使用微调模型：**

```bash
# 使用 ActiveSite 微调模型固定小基序位置
inference.ckpt_override_path=models/ActiveSite_ckpt.pt
```

**使用 inpaint_seq 掩码部分残基序列：**

```bash
# 掩码 A163-168、A170-171、A179 的序列
../scripts/run_inference.py inference.output_prefix=example_outputs/design_motifscaffolding_inpaintseq inference.input_pdb=input_pdbs/5TPN.pdb 'contigmap.contigs=[10-40/A163-181/10-40]' inference.num_designs=10 'contigmap.inpaint_seq=[A163-168/A170-171/A179]'
```

### 8.3 部分扩散

部分扩散：从输入的已有结构出发，只做有限步数的加噪/去噪，输出改造后的结构，不能增加/减少氨基酸数量。

默认 `diffuser.partial_T` 值为 20（对应 T=50 的配置）。**强烈建议尝试不同的 partial_T 值。**

示例：

```bash
# 对 2KL8 蛋白进行部分扩散多样化
../scripts/run_inference.py inference.output_prefix=example_outputs/design_partialdiffusion inference.input_pdb=input_pdbs/2KL8.pdb 'contigmap.contigs=[79-79]' inference.num_designs=10 diffuser.partial_T=10
```

使用 provide_seq 固定部分残基序列：

```bash
../scripts/run_inference.py inference.output_prefix=example_outputs/design_partialdiffusion_peptidewithmultiplesequence inference.input_pdb=input_pdbs/peptide_complex_ideal_helix.pdb 'contigmap.contigs=["172-172/0 34-34"]' diffuser.partial_T=10 inference.num_designs=10 'contigmap.provide_seq=[172-177,200-205]'
```

### 8.4 Binder Design

**推荐使用热点残基 hotspots 而非提供整个目标蛋白**（避免扩散速度过慢）。

热点残基参数：`ppi.hotspot_res=[A30,A33,A34]`

示例：

```bash
# 设计针对胰岛素受体的结合蛋白
../scripts/run_inference.py inference.output_prefix=example_outputs/design_ppi inference.input_pdb=input_pdbs/insulin_target.pdb 'contigmap.contigs=[A1-150/0 70-100]' 'ppi.hotspot_res=[A59,A83,A91]' inference.num_designs=10 denoiser.noise_scale_ca=0 denoiser.noise_scale_frame=0
```

**使用 beta 模型生成更多样化拓扑：**

```bash
inference.ckpt_override_path=models/Complex_beta_ckpt.pt
```

### 8.5 Binder Design 建议

**选择靶蛋白位点：**

- 位点应至少包含 3 个疏水性残基
- 与带电荷的极性位点结合仍然困难
- 与含聚糖的位点结合也很难
- 此前避免选择无序环状结构，但 RFdiffusion1 已可用于结合无序肽段

**截短目标蛋白：**RFdiffusion1 时间复杂度 O(N²)，截短大型目标蛋白可显著降低成本。建议在目标位点两侧各保留约 10 Å 的蛋白，使用 PyMol 操作。

**选择热点残基 hotspots：**界面热点残基定义为目标（固定）链上，与结合（扩散）链上残基的 β–β 距离在 10 Å 以内的残基。通常建议使用 3-6 个热点。在 RFdiffusion1 论文中，通过 PatchDock/RifDock 流程从目标蛋白中识别得到热点残基。

**Binder Design 规模：**论文中为每个靶标生成了约 10000 个骨架，再用 ProteinMPNN-FastRelax 为每个骨架生成两个序列，用 AF2 筛选。实际实验中可能只需约 1000 个骨架即可。

**Binder 序列设计：**RFdiffusion1 只生成骨架（带聚甘氨酸序列），序列需用 ProteinMPNN-FastRelax 协议设计。代码在 https://github.com/nrbennet/dl_binder_design

---

*未完待续……*
