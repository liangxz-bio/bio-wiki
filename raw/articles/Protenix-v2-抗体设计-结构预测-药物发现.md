---
source_url: https://github.com/bytedance/Protenix
ingested: 2026-05-15
sha256: b40a37cea6b8d79f5b91068cfeb792e2e29dc18958bf8466b5a2577fc118ffb0
citation: "ByteDance Seed. Protenix-v2: Broadening the Reach of Structure Prediction and Biomolecular Design. GitHub. 2026."
domain: ai-drug-discovery
tags: [protein-design, antibody, structure-prediction, drug-design, ai-drug-discovery, protenix]
confidence: medium
---

# Protenix v2 | 从结构预测走向抗体设计，开始真正触达药物发现

- **作者**：JoeYo
- **来源**：BioAIDesign

---

## ✨ 导语

**Protenix-v2** 的核心信息其实很简单：

它不只是把结构预测继续做强，而是开始把这些能力真正往**抗体设计和药物发现**上推进。

作者展示了两件事：

- **抗体-抗原复合体预测更准**
- **Zero-shot已经能直接设计出有实验命中的抗体和 binder**

---

## 🔍 Protenix-v2 是什么？

Protenix-v2 同时覆盖两类任务：

- **结构预测**：更准确预测 biomolecular complex，尤其是抗体-抗原复合体
- **分子设计**：直接生成候选 binder，并筛出可实验验证的 hits

---

> **Figure 1.** Zero-shot 抗体设计命中率

---

## 🧠 抗体-抗原结构预测明显更强了

> **Figure 2.** PX-v2 在抗原抗体预测上的评估

作者在三个抗体相关 benchmark 上比较了 Protenix-v2 与 AlphaFold3, Boltz-1, OpenFold3-preview2 和 Protenix-v1。

结果显示，Protenix-v2 在三个数据集上都更强，在 **DockQ > 0.23** 的成功率上，相比 Protenix-v1 提升大约 **9–13 个百分点**。

更重要的是，这种提升不只是"勉强预测对"，在更高质量的区间也依然存在。

---

## 🚀 采样效率提升很明显

> **Figure 3.** 卓越的采样效率和推理时间缩放

这篇文章里一个很亮眼的点是：**Protenix-v2 在 5 seeds 下的表现，已经超过了 Protenix-v1 在 1000 seeds 下的结果。**

这意味着它不是靠疯狂堆采样数量才变强，而是单次采样本身就更有效。

对实际应用来说，这很重要，因为真正有价值的不是"算很多次总会碰对"，而是**在有限预算下更快找到高质量结果**。

---

## 🧪 真正吸引人的地方，是 zero-shot antibody design

Protenix-v2 已经开始展示 **zero-shot antibody design** 的能力。

> **Figure 4.** VHH zero-shot 评估、结构多样性和动力学表征

在作者当前测试的 soluble target panel 上，Protenix-v2 达到 **100% target-level success rate**，也就是每个靶点都至少找到了一个实验确认的 binder。

对于 novelty-controlled targets，**VHH-Fc 的 hit rate 在 2%–48% 之间**。这说明模型已经不只是"会生成很多序列"，而是真正开始具备了实验命中能力。

---

## 🧬 在 GPCR 这种难靶点上也有结果

GPCR 一直是很难做抗体设计的一类靶点，因为表位小、柔性高、实验筛选难。

Protenix-v2 在 **16–30 个设计 / 靶点** 这样很小的预算下，仍然在多个 GPCR 上拿到了命中：

- **VHH-Fc hit rate：16%–88%**
- **mAb hit rate：最高 50%**

这说明它不只是对"容易靶点"有效，也开始触及更接近真实药物研发场景的问题。

---

## 🌈 作者还特别看了 developability

> **Figure 5.** Developability

作者没有停在"找到 binder"这一步，而是进一步测试了这些命中分子的可开发性。结果显示：

- **热稳定性通过率：100%**
- **自相互作用通过率：97.5%**
- **多反应性通过率：93.3%**

也就是说，这些候选不只是"能结合"，在生物物理性质上也比较像真正值得继续推进的分子。

---

## 💡 我的理解

> **结构模型的价值，正在从"解释分子怎么结合"，走向"直接帮助你发现能结合的分子"。**

换句话说，它不再只是一个更强的结构预测器，而开始像一个更完整的 **binder discovery engine**。

---

## 📝 一句话总结

如果说以前大家更关心的是：

> **这个模型能不能把复合体结构算准？**

那么 Protenix-v2 更进一步在回答：

> **结构算得更准之后，能不能真正帮助我们更快找到可实验验证的 binder？**

---

## 📚 文献信息

- **Title：** *Protenix-v2: Broadening the Reach of Structure Prediction and Biomolecular Design*
- **Authors：** ByteDance Seed
- **URL：** https://github.com/bytedance/Protenix
