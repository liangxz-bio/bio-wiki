---
source_url: https://doi.org/10.1093/bioinformatics/btag069
ingested: 2026-05-04
sha256: dd11297040dcf3a9f644049f23b9a331c8895624c38e37403abefa782d5c2474
source_file: /Users/liang/Desktop/Ref/2026-DrugBLIP-btag069.pdf
extracted_by: MinerU (magic-pdf)
extraction_date: 2026-05-04
domain: ai-drug-discovery
citation: "Wang R, Gao X, Zhao P. DrugBLIP: exploring the protein-molecule interaction mechanisms with a multi-task learning graph transformer. Bioinformatics. 2026 Apr 7;42(4):btag069. doi:10.1093/bioinformatics/btag069. PMID: 41967848; PMCID: PMC13080933."
---

# DrugBLIP: exploring the protein–molecule interaction mechanisms with a multi-task learning graph transformer

Rubo Wang1,2 , Xingyu Gao1,2, \* , , Peilin Zhao3, \*

1 Institute of Microelectronics, Chinese Academy of Sciences, Beijing, 100029, China   
2 University of Chinese Academy of Sciences, Beijing, 100049, China   
3 School of Artificial Intelligence, Shanghai Jiao Tong University, Shanghai, 200230, China   
\*Corresponding authors. Xingyu Gao, Institute of Microelectronics, Chinese Academy of Sciences, Beijing, 100029, China. E-mail: gxy9910@gmail.com; Peilin Zhao, School of Artificial Intelligence, Shanghai Jiao Tong University, Shanghai, 200230, China. E-mail: peilinzhao@hotmail.com   
Associate Editor: Lenore Cowen

# Abstract

Motivation: Traditional drug discovery methods are costly and inefficient, while existing deep learning approaches remain limited by task specificity and practical applicability. Accurately modeling protein–molecule interactions is critical for advancing virtual screening, docking, and drug design.

Results: We propose DrugBLIP, a multi-task graph transformer model based on SE(3)-equivariant architectures, to unify protein–molecule interaction learning. By integrating contrastive learning, matching tasks, and docking optimization, DrugBLIP captures 3D spatial relationships through a hybrid graph transformer framework. Evaluations demonstrate state-of-the-art performance: DrugBLIP achieves an AUROC of 0.8217 and BEDROC of 0.5743 on virtual screening, outperforming traditional and deep learning baselines by $1 0 \% - 1 2 7 \%$ across metrics. It also attains $9 1 . 2 \%$ top-1 docking success on CASF-2016 and $4 1 . 8 \%$ target fishing accuracy, showcasing robustness in diverse scenarios. Additionally, DrugBLIP reduces computational time by $7 0 0 \times$ compared to traditional docking tools.

Availability and implementation: Code is available at https://github.com/Wolkenwandler/DrugBLIP and archived at Zenodo with DOI: 10.5281/zenodo.16990700.

# 1 Introduction

Drug discovery is one of the most challenging and innovative fields in medical science, playing a crucial role in safeguarding human health. According to statistics, the average cost of developing a new drug exceeds 2.6 billion dollars, and the success rate is ${ < } 1 0 \%$ (Zhang et al. 2025), which underscores the substantial risk and investment required. However, with the continuous progress of technology, the methods and tools for drug discovery are becoming increasingly diverse, ranging from traditional drug screening to modern biotechnology. In recent years, the efficiency and success rate of drug discovery have shown measurable improvement (Paul et al. 2010). High-throughput screening technology enables researchers to quickly screen millions of compounds in search of potential drug candidates, while the application of computer-aided drug design and artificial intelligence has created new opportunities to enhance both accuracy and efficiency (Zhang et al. 2025).

Traditional scoring functions are integral to the entire drug design process, including virtual screening, docking, and scoring and ranking. However, traditional methods for docking and scoring protein pockets and ligand molecules primarily rely on force fields and empirically developed physical principles (Forli et al. 2016). These methods are highly dependent on experience and the limitations of simulated force fields, making them timeconsuming and unsuitable for large-scale virtual screening. For instance, precise calculations of intermolecular interactions, such as van der Waals forces, electrostatic forces, and hydrogen bonds, are complex and time-intensive. Additionally, the accuracy of these methods is constrained by the parameters of the force fields used, leading to variability and increased uncertainty in results. In contrast, modern machine learning approaches can significantly reduce computation time and improve accuracy by learning from large datasets, making them more suitable for large-scale virtual screening. Therefore, integrating advanced data-driven approaches with existing strategies is a promising direction to overcome the inherent limitations of force fieldbased methods.

Inspired by the fact that multi-task training can achieve relatively good results in multiple downstream tasks (Li et al. 2022), we propose to unify the objectives of traditional scoring functions and investigate drug–protein interaction mechanisms through a multi-objective hybrid training framework, termed DrugBLIP (Drug action mechanism via Boosting LIgand–Protein interaction), as a potential replacement for conventional scoring functions. Our method demonstrates competitive performance across diverse tasks, highlighting its potential as a robust alternative. Specifically, we constructed a pre-training dataset that captures the structural information of protein pockets and ligand molecules based on their structural matching and existing data (Wang et al. 2005, Francoeur et al. 2020). We pre-trained a model based on a SE(3) graph transformer on this dataset, allowing it to effectively learn 3D representations of protein pockets and ligand molecules. Later, we performed fine-tuning for tasks such as virtual screening, target fishing, and docking power. Experimental evaluations indicate that our approach consistently outperforms both traditional and state-of-the-art deep learning-based methods across multiple benchmarks.

# 2 Materials and methods

# 2.1 Related work

# 2.1.1 Scoring function

Scoring functions play a crucial role in drug discovery. Traditional scoring functions primarily rely on physics-based approaches (An et al. 2015), including force field-based scoring functions, solvation models, and quantum mechanics methods. In parallel, empirical scoring functions (Zheng and Merz Jr 2011) estimate the binding affinity of complexes by aggregating key energetic contributions during the protein–ligand binding process (such as hydrogen bonds, hydrophobic effects, steric hindrance, etc.). Although scoring functions based on machine learning have surpassed traditional scoring functions in some scenarios (Cheng et al. 2012), their applicability often remains limited to specific cases, making sustained deployment in real-world workflows challenging. In contrast, our proposed DrugBLIP is designed for broad applicability across diverse scenarios, offering the potential to serve as a comprehensive replacement for traditional scoring functions.

# 2.1.2 Multi-task learning

Multi-task learning (MTL) is a learning paradigm that effectively leverages both task-specific and shared information to address multiple related tasks simultaneously. Liu et al. (2019) proposed several architectures based on the shared-backbone idea, enabling the realization of multiple functions through a single network. Liu et al. (2019) further introduced specific task modules integrated within the shared architecture to enhance task performance. Li et al. (2022) proposed modal fusion strategies that combine vision and natural language representations, achieving competitive results on multiple vision–text tasks. Inspired by these works, we design DrugBLIP to use a unified backbone network capable of performing the diverse roles traditionally fulfilled by separate scoring functions.

# 2.2 Model architecture

As shown in Fig. 1, the overall flow of DrugBLIP consists of two primary components. The first is the Protein Pocket Encoder. Although the architecture supports full protein structures, in this study, we specifically focus on the binding pocket region to reduce noise and computational cost. The encoder takes the atomic graph of the protein pocket as input.

To ensure invariance to coordinate rotations and translations while capturing local chemistry, we adopt a dual-representation strategy maintaining both atom-level and pair-level features.

· Atom Representation: Each atom $j$ is initialized with a feature vector $\widehat { \mathbf { a } } _ { i } ^ { 0 }$ derived solely from its atom type via an embedding layer, which is inherently invariant to SE(3) transformations.

![](images/wang2026-drugblip-fig1.jpg)  
Figure 1 Overview of DrugBLIP. We propose a hybrid structure that can unify the representations of proteins and molecules for learning the interaction between proteins and molecules. It consists of three parts: (i) The Protein Pocket Encoder and Molecular Encoder are trained through a Contrastive loss to align the representations between proteins and molecules. (ii) The Docking Model models the relative position information between proteins and molecules by constructing a fully connected graph of proteins and molecules. While achieving docking, it outputs affinity ranking scores. (iii) The Protein Pocket-grounded mol Encoder is trained through matching loss to distinguish positive and negative protein pocket-molecule pairs.

· Pair Representation: Simultaneously, the pair representation ${ \bf q } _ { i j } ^ { 0 }$ is initialized by combining Gaussian RBF-encoded Euclidean distances with bond type embeddings.

The RBF kernel maps squared Euclidean distances into a high-dimensional similarity measure:

$$
K ( \mathbf { x } _ { i } , \mathbf { x } _ { j } ) = \exp \big ( - \gamma \cdot \| \mathbf { x } _ { i } - \mathbf { x } _ { j } \| ^ { 2 } \big ) ,
$$

Crucially, using this RBF encoding to initialize the pair representation anchors the geometric awareness of the model at the input stage, serving as an invariant 3D spatial positional encoding.

The backbone is based on a Transformer modified for 3D spatial data. To facilitate information exchange between 3D geometry and semantic features, the model performs iterative updates:

1) Atom-to-pair communication: The pair representation $q _ { i j }$ is updated using the attention map from the self-attention mechanism. Formally, for layer $l$ :

$$
q _ { i j } ^ { l + 1 } = q _ { i j } ^ { l } + \Biggl \{ \frac { Q _ { i } ^ { l , h } ( K _ { j } ^ { l , h } ) ^ { T } } { \sqrt { d } } \mid h \in [ 1 , H ] \Biggr \} ,
$$

where $H$ is the number of heads and $d$ is the hidden dimension.

2) Pair-to-atom communication: To leverage 3D information in atom updates, the pair representation acts as a bias term in the self-attention calculation:

$$
\mathsf { A t t e n t i o n } ( Q _ { i } , K _ { j } , V _ { j } ) = \mathsf { s o f t m a x } \left( \frac { Q _ { i } K _ { j } ^ { T } } { \sqrt { d } } + q _ { i j } ^ { l - 1 } \right) V _ { j } .
$$

This mechanism ensures that the attention weights are modulated by the geometric distance and bond information encoded in $q _ { i j }$ , rendering the internal representations invariant to global rotation and translation.

Finally, to predict 3D structural changes (docking poses), an SE(3)-equivariant head predicts delta positions (coordinate updates). It uses the SE(3)-invariant pair features and the equivariant vector difference $\left( { { X } _ { i } } - { { X } _ { j } } \right)$ :

$$
\begin{array} { r l } & { \widehat { x } _ { i } = x _ { i } + \displaystyle \frac { 1 } { n } \sum _ { j = 1 } ^ { n } ( x _ { i } - x _ { j } ) c _ { i j } , } \\ & { c _ { i j } = { \mathsf { R e L U } } \Big ( ( q _ { i j } ^ { L } - q _ { i j } ^ { 0 } ) U \Big ) W , } \end{array}
$$

where $U$ and $W$ are projection matrices. Note that $c _ { i j }$ is derived from the difference in pair representations $( q _ { i j } ^ { L } - q _ { i j } ^ { 0 } )$ , allowing the model to focus on relative structural refinement.

To train a unified model for understanding protein–molecule interactions, we adopt a multimodal hybrid approach including three modules. By encoding both atomic and 3D information, a virtual atom Pocket "[CLS]" is inserted into the Pocket Encoder. It is initialized with a learnable embedding vector, placed at the geometric center, and connected to all real atoms. The specific modules are:

1) Mol encoder: Adds a virtual atom Mol "[CLS]" located at the molecular center. Following the same initialization strategy as the Pocket "[CLS]," it is assigned a learnable embedding, placed at the geometric center, and connected to all molecular atoms.

2) Docking model: Processes the complete protein pocket– molecule pair and outputs docking scores for downstream screening. Feature representations from the respective encoders are fused through a global interaction stage, where the molecule is positioned relative to the protein pocket before scoring.

3) Pocket-grounded mol encoder: Introduces a virtual atom "[Encode]" at the molecular center. This token is initialized identically (using a learnable embedding at the geometric center) to encode cross-context information between the protein pocket and the molecule.

# 2.3 Pre-training objectives

Traditional force-field-based methods can simultaneously perform various functions for proteins and molecules, such as virtual screening and protein–ligand docking. Inspired by this and previous work (Li et al. 2022), we pre-train DrugBLIP with three objectives, each targeting a specific aspect of protein–molecule correlation. Two objectives focus on property understanding, and one on positional generation. Each protein pocket–molecule pair only requires a single forward pass through the computationally intensive protein encoder and three forward passes through the mixed model. These distinct functionalities are jointly optimized via the following loss functions.

# 2.3.1 Protein pocket–molecule contrastive loss

Contrastive loss is used to align the feature spaces of the protein pocket encoder and the molecule model. Existing studies (Singh et al. 2023, Bhat et al. 2025) have also applied contrastive learning to molecular data; however, most rely on protein sequences or molecular fingerprints. In contrast, we perform structurelevel alignment between proteins and small molecules. This encourages similar feature representations for positive pocket– molecule pairs and repels negative pairs.

We aim to learn a similarity function s such that paired pocket-molecule structures have higher similarity scores. Let $\boldsymbol { \mathsf { v } } _ { \mathtt { c l s } } ^ { P }$ and $\boldsymbol { \mathsf { v } } _ { \mathtt { c l s } } ^ { M }$ be the embeddings of the "[CLS]" tokens from the Protein and Mol encoders, respectively. We use linear transformations $g _ { P } ( \cdot )$ and $g _ { M } ( \cdot )$ to map these embeddings to normalized lower-dimensional representations. The similarity between a protein pocket $P$ and a molecule $M$ is defined as the dot product of their projected features:

$$
\begin{array} { r } { s ( P , M ) = g _ { P } ( \pmb { v } _ { { \mathrm { c l } } _ { 5 } } ^ { P } ) ^ { \top } g _ { M } ( \pmb { v } _ { { \mathrm { c l } } _ { 5 } } ^ { M } ) . } \end{array}
$$

For a mini-batch of $N$ pairs, we calculate the softmaxnormalized pocket-to-molecule and molecule-to-pocket similarity. For the ith pair $\left( P _ { i } , M _ { i } \right)$ :

$$
\begin{array} { r } { p _ { i } ^ { \mathsf { p } 2 \mathsf { m } } ( P _ { i } ) = \frac { \mathsf { e x p } ( s ( P _ { i } , M _ { i } ) / \tau ) } { \sum _ { k = 1 } ^ { N } \mathsf { e x p } ( s ( P _ { i } , M _ { k } ) / \tau ) } , } \\ { p _ { i } ^ { \mathsf { m } 2 \mathsf { p } } ( M _ { i } ) = \frac { \mathsf { e x p } ( s ( M _ { i } , P _ { i } ) / \tau ) } { \sum _ { k = 1 } ^ { N } \mathsf { e x p } ( s ( M _ { k } , P _ { i } ) / \tau ) } , } \end{array}
$$

where $\tau$ is a learnable temperature parameter.

Let $\pmb { \mathsf { y } } ^ { \mathsf { p 2 m } } ( P _ { i } )$ and $\mathbf { y } ^ { \mathsf { m 2 p } } ( M _ { i } )$ denote the ground-truth one-hot similarity, where the positive pair $\left( P _ { i } , M _ { i } \right)$ has a probability of 1 and negative pairs have 0. The contrastive loss is defined as the cross-entropy CE between the predicted probability $\pmb { \ p }$ and the ground truth y:

$$
\begin{array} { r l } & { \mathcal { L } _ { \mathsf { p m c } } = \displaystyle \frac { 1 } { 2 } \mathbb { E } _ { ( P , M ) \sim D } \big [ \mathsf { C E } ( \mathbf { y } ^ { \mathsf { p } 2 \mathsf { m } } ( P ) , \mathsf { p } ^ { \mathsf { p } 2 \mathsf { m } } ( P ) ) } \\ & { \qquad + \thinspace \mathsf { C E } ( \mathbf { y } ^ { \mathsf { m } 2 \mathsf { p } } ( M ) , \mathsf { p } ^ { \mathsf { m } 2 \mathsf { p } } ( M ) ) \big ] . } \end{array}
$$

# 2.3.2 Protein pocket–molecule matching loss

This objective captures fine-grained compatibility between a protein pocket and a molecule by treating the task as binary classification. The output embedding of the virtual atom "[Encode]," which aggregates cross-modal information, is fed into a linear classifier to predict whether the pair is matched (positive) or unmatched (negative). To enhance discriminability, we adopt hard negative mining. Specifically, we select negative pairs that are incorrectly predicted with high similarity scores in the contrastive learning stage. The loss is defined as:

$$
\begin{array} { r } { \mathcal { L } _ { \mathsf { p m m } } = \mathbb { E } _ { ( P , M ) \sim D } \mathsf { C E } ( \mathsf { y } ^ { \mathsf { p m m } } , \mathsf { p } ^ { \mathsf { p m m } } ( P , M ) ) , } \end{array}
$$

where $\mathbf { y } ^ { \mathsf { p m m } }$ is the 2D one-hot ground-truth label, and $\mathsf { p } ^ { \mathsf { p } \mathsf { m m } }$ is the predicted probability.

# 2.3.3 Protein pocket–molecule docking loss

This objective enables the model to predict accurate binding positions by explicitly optimizing the geometry structure. The loss consists of two components: (i) Intra-molecular Loss for maintaining the valid local structure of the molecule, and (ii) Inter-molecular Loss for determining the precise binding pose relative to the pocket.

For the internal structure, we use the Huber loss on pairwise atomic distances to ensure robustness against structural outliers (e.g. flexible rotatable bonds).

$$
\mathcal { L } _ { \mathrm { i n t r a } } = \frac { 1 } { | \mathcal { P } _ { m o l } | } \sum _ { ( i , j ) \in \mathcal { P } _ { m o l } } H _ { \delta } ( d _ { i j } - d _ { i j } ^ { * } ) ,
$$

where $\mathcal { P } _ { m o l }$ is the set of atom pairs within the molecule, $d _ { i j }$ and $d _ { i j } ^ { * }$ are the predicted and ground-truth distances, respectively. $H _ { \delta } ( \cdot )$ is the Huber function with threshold $\delta$ :

$$
H _ { \delta } ( a ) = \left\{ \begin{array} { l l } { \displaystyle \frac { 1 } { 2 } a ^ { 2 } } & { \mathrm { i f ~ } \left| a \right| \leq \delta , } \\ { \displaystyle \delta \bigg ( \left| a \right| - \frac { 1 } { 2 } \delta \bigg ) } & { \mathrm { o t h e r w i s e . } } \end{array} \right.
$$

For the binding pose, we use the Mean Squared Error (MSE) loss on the coordinate deviations to encourage high positional fidelity of the ligand atoms relative to the protein pocket.

$$
\mathcal { L } _ { \mathrm { i n t e r } } = \frac { 1 } { N _ { m o l } } \sum _ { i = 1 } ^ { N _ { m o l } } \| \widehat { \mathbf { x } } _ { i } - \mathbf { x } _ { i } ^ { * } \| ^ { 2 } ,
$$

where $N _ { m o l }$ is the number of atoms in the molecule, $\widehat { \mathbf { x } } _ { i }$ is the predicted coordinate, and $\pmb { x } _ { j } ^ { * }$ is the ground-truth coordinate.

The overall pre-training objective combines these terms:

$$
\mathcal { L } = \mathcal { L } _ { \sf p m c } + \mathcal { L } _ { \sf p m m } + \mathcal { L } _ { \sf i n t r a } + \mathcal { L } _ { \sf i n t e r } .
$$

# 2.4 Model pretraining

# 2.4.1 Pre-training data

We constructed the pre-training dataset from the PDBBind database (Wang et al. 2005) and the CrossDocked 2020 dataset (Francoeur et al. 2020), selecting protein pockets and ligand molecules whose binding site deviation was ${ \bf < } 1 \mathring { \mathsf { A } } .$ Motivated by Gao et al. (2024), we used the HomoAug strategy for data augmentation. Unlike direct noise injection, which can introduce instability or chemically implausible structures, HomoAug replaces the original protein with a homologous counterpart, thereby generating new valid complex structures. All samples used in downstream evaluation were removed to prevent data leakage. This process yielded a pre-training dataset of 217 146 protein pocket–small molecule pairs.

# 2.4.2 Pre-training settings

Our model is implemented in PyTorch and pre-trained on sixteen NVIDIA V100 GPUs. For both the Protein encoder and Mol encoder, we initialize weights from UniMol models pre-trained on unpaired data (Zhou et al. 2023). Training details for this initialization stage are provided in Supplementary B.1, available as supplementary data at Bioinformatics online. During pre-training, the Protein and Mol encoders are optimized jointly using the curated dataset. We adopt the AdamW optimizer with a weight decay of 0.05. The learning rate follows a 1000-step linear warmup followed by cosine annealing, with a peak and minimum learning rate of $1 \times 1 0 ^ { - 4 }$ and $5 \times 1 0 ^ { - 6 }$ , respectively. Additional hyperparameters and settings are listed in Supplementary B, available as supplementary data at Bioinformatics online.

# 2.5 Model fine-tuning

All fine-tuning experiments use the pre-trained parameters from the first stage for initialization.

# 2.5.1 Fine-tuning DrugBLIP for drug virtual screening

The objective of virtual screening is to identify active molecules from a small-molecule library that can effectively bind to a given target protein. For this task, and to ensure a fair comparison with prior studies, we use true positive protein–ligand complexes with accurate structures from the PDBBind 2019 dataset. As with pre-training, all samples overlapping with test sets are removed, yielding 65 989 protein–ligand pairs for fine-tuning.

As illustrated in Fig. 2, each candidate molecule is augmented with a virtual atom, denoted as Mol "[CLS]," placed at the molecular centroid, and passed to the Mol Encoder. Likewise, each protein pocket is augmented with a virtual atom, Protein "[CLS]," placed at the pocket centroid, before being fed into the Protein Encoder. The resulting molecular and protein feature embeddings are compared via a similarity function: higher similarity scores indicate a stronger likelihood of the molecule being an active compound, whereas lower scores suggest inactivity.

# 2.5.2 Fine-tuning DrugBLIP for target fishing

Target fishing seeks to identify potential biological targets for known or candidate drug molecules. This approach deepens our understanding of target mechanisms of action, facilitates the investigation of side effects, helps address drug resistance, and ultimately improves therapeutic efficacy. It also enables the discovery of novel drug targets, broadens the applications of existing drugs, and supports drug repurposing, thereby offering more possibilities for disease treatment.

To ensure fair comparison with prior methods, we use the PDBbind General set v.2020, excluding complexes that overlap with the test set. Data preprocessing includes adding missing atoms to both proteins and ligands, manually correcting fileloading errors, and filtering pockets containing fewer than 5 residues within $5 \mathring { \mathsf { A } }$ of the ligand. We further remove complexes from PDBbind whose protein sequence identity to test proteins exceeds $4 0 \%$ [measured with MMseqs2 (Steinegger and Soding € 2017)] or whose ligand fingerprint similarity to test ligands exceeds $8 0 \%$ [computed with RDKit (Landrum et al. 2013)].

As illustrated in Fig. 2, the molecule is augmented with a virtual atom labeled "[Encode]" placed at the molecular centroid. The candidate protein pocket is processed by the Protein Encoder, and its features are combined with those of the molecule containing the virtual atom before being passed to the Molecular Encoder. The "[Encode]" token facilitates feature alignment between the two modalities. The model outputs a matching score between the protein target and the molecule, along with a ranking that prioritizes target proteins most likely to bind effectively.

# 2.5.3 Fine-tuning DrugBLIP for protein–molecule docking and ranking

Protein–molecule docking aims to predict both the ligand binding pose and the conformation of the resulting complex. This requires considering conformational changes in both partners as well as their relative positioning. Accurately modeling these interactions is critical for elucidating biological processes and for rational drug design.

Following the protocol of Zhou et al. (2023), we construct the docking training set from the PDBbind General set v.2020, a widely used benchmark comprising diverse protein–ligand complexes. Prior to training, we filter out erroneous or incomplete entries, retaining only complexes whose binding pockets contain at least five residues within $5 \mathring { \mathsf { A } }$ of the ligand. This yields 165 630 protein–ligand pairs.

As shown in Fig. 2, after protein features are extracted by the Protein Encoder, they are concatenated with molecule features within the Molecular Encoder equipped with a pair-wise MLP. Within this encoder, cross-modal interactions update the pair representations to capture fine-grained positional relationships between protein and ligand atoms. The updated features are projected into a distance space via the MLP, where intramolecular distances refine ligand conformation and intermolecular distances localize the ligand within the binding site.

# 3 Results

In this section, we conduct experiments on three functions commonly used in traditional scoring functions, including virtual screening, Protein–Ligand docking, and targetfishing to evaluate whether DrugBLIP can effectively learn the interaction between proteins and molecules. We conducted ablation experiments on different training strategies. The results can be found in the Supplementary C.1, available as supplementary data at Bioinformatics online.

# 3.1 Virtual screening

For virtual screening, we used DUD-E (Mysinger et al. 2012) as our test benchmark, which contains 102 proteins and their corresponding 22 886 bioactive molecules. We evaluated performance using common indicators in virtual screening, including AUROC, Boltzmann-enhanced Discrimination of ROC (BEDROC), and Enrichment Factor (EF). BEDROC incorporates exponential weights and assigns higher weights to top-ranked molecules.

![](images/wang2026-drugblip-fig2.jpg)  
Figure 2 Model architecture. The model inputs the atom types and coordinates of Protein and Ligand. Obtain the atomic features through the atom types, calculate the distance through the coordinates, obtain the initial Pair features through RBF transformation. In the update process, it will pass through Multi-Head Attention. The Pair features are used as the bias of Attention. While updating the atom features, it is updated. According to different tasks, it can be selected whether to input the features of protein into the feature update process of Ligand. The updated Ligand features and protein features are used for downstream tasks.

In order to effectively evaluate the generalization of DrugBLIP, we adopted a zero-shot evaluation setting, meaning that no fine-tuning was performed on DUD-E. We compared our approach with various methods, including traditional techniques such as $\Delta$ -VinaRF and RFscore, as well as deep learningbased methods like TANKBind and EquiScore. The experimental results are shown in Table 1.

On the DUD-E dataset, DrugBLIP achieved the best performance in terms of AUROC, scoring 0.8217, surpassing all baseline methods, including the latest deep learning approaches DrugCLIP (Gao et al. 2024) and EquiScore (Cao et al. 2024). Traditional methods often suffer from high false positive or false negative rates due to their limited ability to capture the complex interactions between proteins and ligands. Scoring functions (Wojcikowski et al. 2017) rely on force-field interactions and cannot flexibly adapt to diverse docking scenarios, and in practice are generally weaker than deep learning-based methods. Glide SP performs best among various scoring functions and even surpasses many deep learning methods; however, due to methodological limitations, it is very slow to run, though it exhibits higher stability across indicators and can replace deep learning methods in some scenarios. For deep learning methods, the use of large-scale training data and algorithmic advances can improve performance to some extent, but most prior methods are trained on a single task, limiting their ability to fully capture protein–ligand interactions.

DrugBLIP, by leveraging advanced protein–molecule interaction mechanisms learned from multi-task training and graph transformer-based models, shows remarkable performance. Compared with the previous best scoring function, DrugBLIP achieves a $4 1 \%$ improvement in BEDROC. For the more challenging enrichment factor metric, EF $0 . 5 \%$ increases by $1 2 7 \%$ , EF $1 \%$ by $1 2 8 . 8 \%$ , and EF $5 \%$ by $6 3 . 2 \%$ . Compared with the best deep learning method, it achieves a $1 4 . 8 \%$ improvement in BEDROC, with EF $0 . 5 \%$ increased by $1 3 . 0 \%$ , EF $1 \%$ by $1 5 . 8 \%$ , and EF $5 \%$ by $1 0 . 8 \%$ . This significant improvement demonstrates the effectiveness of DrugBLIP.

To further verify that the above improvements are not due to potential data leakage between training and test sets, we conducted a protein-level deduplication experiment on DUD-E. Specifically, we removed from the training set any complex whose protein shared the same UniProt ID as a protein in the DUD-E test set, ensuring that all proteins in the test set were unseen during both pre-training and fine-tuning. The detailed procedure and dataset statistics after deduplication are provided in the Supplementary Materials, available as supplementary data at Bioinformatics online (Extended Data Table 1). Under this stricter evaluation protocol, DrugBLIP still achieved strong results (AUROC 0.7311, BEDROC 0.3432, EF $0 . 5 \%$ 21.37, EF $1 \%$ 18.75, EF $5 \%$ 8.06), outperforming all traditional scoring functions and deep learning baselines. This confirms that DrugBLIP maintains a clear advantage even when evaluated on entirely unseen protein targets, highlighting its robust generalization capability.

# 3.2 Docking power

We evaluated the docking power of DrugBLIP, particularly its ability to distinguish native binding poses from those generated by established docking software packages. Our assessment primarily focused on the benchmark datasets CASF-2013 and

Table 1 Performance comparison between DrugBLIP and different baseline approaches on DUD-E datasets.a   

<table><tr><td>Methods</td><td></td><td>AUROC</td><td></td><td>BEDROC</td><td>EF</td><td></td><td></td><td></td><td></td><td></td><td></td></tr><tr><td></td><td></td><td>Mean</td><td> Median</td><td>Mean</td><td>Median</td><td colspan="2">0.5%</td><td colspan="2">1%</td><td colspan="2">5%</td></tr><tr><td></td><td></td><td></td><td></td><td></td><td></td><td>Mean</td><td> Median</td><td>Mean</td><td>Median</td><td>Mean</td><td> Median</td></tr><tr><td></td><td> pafnucy</td><td>0.6313</td><td>0.6389</td><td>0.1645</td><td>0.1450</td><td>4.24</td><td>2.64</td><td>3.86</td><td>2.59</td><td>2.77</td><td>2.41</td></tr><tr><td></td><td>RFscorev1</td><td>0.6105</td><td>0.6295</td><td>0.1158</td><td>0.0540</td><td>3.11</td><td>1.54</td><td>2.76</td><td>1.50</td><td>2.24</td><td>1.69</td></tr><tr><td></td><td>RFscorev2</td><td>0.6484</td><td>0.6602</td><td>0.1536</td><td>0.1180</td><td>6.19</td><td>4.05</td><td>5.56</td><td>4.14</td><td>3.47</td><td>3.29</td></tr><tr><td></td><td>RFscorev3</td><td>0.6373</td><td>0.6447</td><td>0.1573</td><td>0.1084</td><td>6.68</td><td>3.73</td><td>5.78</td><td>1.90</td><td>2.41</td><td>3.08</td></tr><tr><td></td><td>RFscorev4</td><td>0.6525</td><td>0.6546</td><td>0.1242</td><td>0.1044</td><td>4.90</td><td>3.07</td><td>4.52</td><td>3.27</td><td>2.98</td><td>2.76</td></tr><tr><td></td><td>PLECRF</td><td>0.6370</td><td>0.6447</td><td>0.1819</td><td>0.1069</td><td>8.47</td><td>8.47</td><td>7.09</td><td>3.26</td><td>3.66</td><td>2.67</td></tr><tr><td></td><td>△-VinaRF</td><td>0.6967</td><td>0.6954</td><td>0.2134</td><td>0.1489</td><td>9.52</td><td>5.56</td><td>8.00</td><td>5.52</td><td>4.38</td><td>3.66</td></tr><tr><td></td><td>Glide SP</td><td>0.7670</td><td>0.7833</td><td>0.4073</td><td>0.4062</td><td>19.39</td><td>19.19</td><td>16.18</td><td>15.99</td><td>7.23</td><td>7.20</td></tr><tr><td></td><td>OnionNet</td><td>0.5971</td><td>0.6065</td><td>0.0855</td><td>0.0855</td><td>2.84</td><td>1.77</td><td>2.84</td><td>1.77</td><td>2.20</td><td>2.00</td></tr><tr><td></td><td>NetworkScore</td><td>0.6331</td><td>0.6409</td><td>0.1295</td><td>0.1153</td><td>5.75</td><td>2.58</td><td>4.99</td><td>2.41</td><td>2.74</td><td>2.01</td></tr><tr><td></td><td>DeepCoy</td><td>0.6681</td><td>0.6594</td><td>0.1240</td><td>0.1026</td><td>3.29</td><td>0.91</td><td>3.64</td><td>1.91</td><td>2.16</td><td>1.93</td></tr><tr><td></td><td>BATScore</td><td>0.6388</td><td>0.6340</td><td>0.1636</td><td>0.1215</td><td>6.11</td><td>2.89</td><td>4.80</td><td>2.38</td><td>2.82</td><td>2.29</td></tr><tr><td></td><td>CSM-Lig</td><td>0.6710</td><td>0.6741</td><td>0.2175</td><td>0.1430</td><td>12.71</td><td>5.44</td><td>10.38</td><td>4.68</td><td>4.93</td><td>3.50</td></tr><tr><td></td><td>LEC</td><td>0.6538</td><td>0.6726</td><td>0.1456</td><td>0.1349</td><td>7.72</td><td>3.56</td><td>6.08</td><td>2.86</td><td>3.42</td><td>1.90</td></tr><tr><td></td><td>Vinardo</td><td>0.6657</td><td>0.6880</td><td>0.2030</td><td>0.1736</td><td>6.12</td><td>3.87</td><td>5.19</td><td>3.89</td><td>3.60</td><td>3.19</td></tr><tr><td></td><td>Bionoi</td><td>0.6751</td><td>0.6804</td><td>0.1810</td><td>0.1687</td><td>7.04</td><td>4.76</td><td>6.12</td><td>4.30</td><td>3.69</td><td>2.82</td></tr><tr><td></td><td>GAT-Score</td><td>0.7243</td><td>0.7229</td><td>0.2937</td><td>0.2503</td><td>14.42</td><td>8.69</td><td>12.49</td><td>8.04</td><td>5.45</td><td>4.70</td></tr><tr><td></td><td>EquiScore</td><td>0.7630</td><td>0.7705</td><td>0.3808</td><td>0.3385</td><td>21.93</td><td>18.45</td><td>17.97</td><td>14.04</td><td>7.28</td><td>6.59</td></tr><tr><td></td><td>DrugCLIP</td><td>0.8051</td><td>0.8183</td><td>0.5006</td><td>0.5165</td><td>28.79</td><td>28.68</td><td>22.79</td><td>22.57</td><td>9.16</td><td>8.98</td></tr><tr><td></td><td>DrugBLIP</td><td>0.8217</td><td>0.8274</td><td>0.5743</td><td>0.5895</td><td>32.52</td><td>33.93</td><td>26.39</td><td>27.29</td><td>10.15</td><td>10.09</td></tr></table>

a Bold indicate state-of-the-art.

CASF-2016. A pose is considered native if its root mean square deviation (RMSD) from the true binding pose is ${ < } 2 \mathring { \mathsf { A } } .$ . During docking, the molecular encoder facilitates the movement of the molecule to the correct docking position based on the interactions between the molecule and the protein, and it outputs a docking score. The CASF-2013 dataset contains 195 protein–ligand pairs along with multiple decoys, while the CASF-2016 dataset comprises 285 protein–ligand pairs with multiple decoys. We compare DrugBLIP against a range of widely used scoring functions to provide a comprehensive performance benchmark.

As shown in Table 2, DrugBLIP exhibited outstanding performance on CASF-2013, achieving a top-1 success rate of $9 0 . 2 7 \%$ , surpassing methods such as AutoDock Vina. Additionally, it outperformed previous methods in top-2 and top-3 success rates, reaching $9 2 . 8 2 \%$ and $9 4 . 3 6 \%$ , respectively. On CASF-2016, DrugBLIP achieved a top-1 success rate of $9 1 . 2 3 \%$ , exceeding all prior methods. While its top-2 and top-3 success rates are marginally lower than those of a few earlier approaches, the top-1 metric is often the most relevant for real-world applications, as it reflects the ability to identify the correct pose in the first prediction. This underscores the diversity and potential of the proposed method in enhancing docking accuracy, providing a comprehensive and innovative solution to docking challenges.

We sought to understand why DrugBLIP achieves such high docking power. To investigate this, we compared the positions of the ligand before and after docking. It is evident that when the initial position of the ligand is far from the correct binding site, the movement of the molecule is substantial; conversely, when the ligand starts from a position close to the correct binding site, the movement is minimal. This indicates that DrugBLIP can effectively discern whether a given initial position is correct when faced with various starting configurations. Representative case studies illustrating these behaviors are provided in Supplementary C.2, available as supplementary data at Bioinformatics online. If the initial position deviates significantly from the correct binding site, the model docks the molecule to the correct position, resulting in greater movement. On the other hand, if the initial position is already close to the correct binding site, only minimal adjustments are needed. DrugBLIP learns this behavior through prior docking experiences, which contributes to its robust docking capabilities. Furthermore, we compared the efficiency of DrugBLIP with that of common docking tools, as reported in Supplementary C.3, available as supplementary data at Bioinformatics online, showing that DrugBLIP achieves competitive or superior runtime performance while maintaining high accuracy.

# 3.3 Target fishing

To further validate the interactions between protein pockets and molecules facilitated by DrugBLIP, we evaluated its performance on the target fishing task. The core idea of target fishing is to identify potential biological targets from known or candidate drug molecules through computational or experimental methods (Chen et al. 2020). This approach aids in understanding the mechanisms of target action, addressing issues of resistance and side effects, enhancing the efficacy of drug treatments, and expanding the applications of drugs. Additionally, it enables drug repurposing, providing more possibilities for disease treatment.

Table 3 shows the performance of various methods on the CASF-2016 benchmark. From the perspective of traditional methods, X-Score, LigScore2, AutoDock Vina, etc. exhibit relatively low performance on Top-1, Top-5, and Top-10 indicators. For example, the Top-1 success rate of X-Score is only $7 . 0 0 \%$ , indicating that these methods have limited ability to accurately identify the most likely protein targets. Most are based on traditional docking algorithms or empirical scoring functions, making it difficult to fully capture the complex interaction patterns between small molecules and proteins, which leads to higher false positive or false negative rates in target fishing.

With the development of technology, some improved methods such as GlideScore-XP, VinaRF20, and DrugScoreCSD have achieved better performance. The Top-1 success rate of GlideScore-XP reaches $1 4 . 4 0 \%$ , which is a certain improvement compared to traditional methods, yet it still falls short of the requirements for high-precision target fishing. These methods benefit from advances in molecular conformation prediction and scoring function optimization, but due to the limitations of their underlying principles, there remains substantial room for improvement when dealing with complex protein–small molecule interactions.

Table 2 Performance on CASF 2016 and CASF 2013 benchmark.a   

<table><tr><td>Method</td><td colspan="3">CASF 2016</td><td colspan="3">CASF 2013</td></tr><tr><td></td><td>Top1</td><td>Top2</td><td>Top3</td><td>Top1</td><td>Top2</td><td>Top3</td></tr><tr><td>X-Score</td><td>65.3</td><td>77.9</td><td>83.5</td><td>61.0</td><td>71.3</td><td>76.4</td></tr><tr><td>ChemScore</td><td>80.4</td><td>86.0</td><td>90.9</td><td>61.0</td><td>84.1</td><td>88.7</td></tr><tr><td>ASP</td><td>81.1</td><td>88.4</td><td>93.0</td><td>73.3</td><td>82.1</td><td>87.7</td></tr><tr><td>GoldScore</td><td>75.1</td><td>86.3</td><td>90.5</td><td>72.3</td><td>84.1</td><td>88.7</td></tr><tr><td>dSAS</td><td>30.2</td><td>44.6</td><td>51.6</td><td>26.2</td><td>37.9</td><td>58.7</td></tr><tr><td>LigScore2</td><td>85.6</td><td>93.3</td><td>96.5</td><td>78.5</td><td>84.6</td><td>87.2</td></tr><tr><td>GlideScore-SP</td><td>87.7</td><td>91.9</td><td>93.7</td><td>79.0</td><td>86.7</td><td>88.7</td></tr><tr><td>ChemPLP</td><td>86.0</td><td>93.7</td><td>96.1</td><td>81.0</td><td>86.7</td><td>89.7</td></tr><tr><td>AutodockVina</td><td>90.2</td><td>95.8</td><td>97.2</td><td>85.6</td><td>90.8</td><td>92.8</td></tr><tr><td>DrugBLIP</td><td>91.2</td><td>94.7</td><td>96.1</td><td>90.3</td><td>92.8</td><td>94.4</td></tr></table>

a Here, top1, 2, and 3 are defined as the proportion of top1, top2, and top3 with rmsd $< 2 \mathring { \mathsf { A } } .$ This is defined as the success rate with the unit of $\%$ . Bold indicates the best performance, and underline indicates the second-best.

Table 3 Performance on CASF 2016 benchmark.a   

<table><tr><td>Method</td><td>Top 1</td><td>Top 5</td><td>Top 10</td></tr><tr><td>X-Score (Zhang et al. 2009)</td><td>7.00</td><td>13.30</td><td>18.20</td></tr><tr><td>LigScore2 (Krammer et al. 2005)</td><td>11.20</td><td>17.50</td><td>29.50</td></tr><tr><td>AutodockVina (Trott and Olson 2010)</td><td>13.70</td><td>22.80</td><td>31.20</td></tr><tr><td>GlideScore-XP (Friesner et al. 2006)</td><td>14.40</td><td>23.50</td><td>34.70</td></tr><tr><td>VinaRF20 (Wang et al. 2021)</td><td>15.10</td><td>24.90</td><td>31.60</td></tr><tr><td>DrugScoreCSD (Velec et al. 2005)</td><td>15.40</td><td>23.90</td><td>33.00</td></tr><tr><td>GlideScore-SP (Friesner et al. 2006)</td><td>16.50</td><td>27.00</td><td>37.50</td></tr><tr><td>ChemPLP (Liebeschuetz et al. 2012)</td><td>17.50</td><td>29.10</td><td>41.10</td></tr><tr><td>DrugCLIP (Gao et al. 2024)</td><td>36.12</td><td>78.71</td><td>86.31</td></tr><tr><td>DrugBLIP (Ours)</td><td>41.83</td><td>86.69</td><td>93.16</td></tr></table>

a Here, top1, 5, and 10 are defined as the proportion of top1, top5, and top10. This is defined as the success rate with the unit of $\%$ . Bold indicates the best performance, and underline indicates the second-best.

Among deep learning-based methods, DrugCLIP shows significant advantages, with Top-1, Top-5, and Top-10 success rates of $3 6 . 1 2 \%$ , $7 8 . 7 1 \%$ , and $8 6 . 3 1 \%$ , respectively, leading among existing methods and demonstrating the potential of deep learning in target fishing tasks. However, our proposed DrugBLIP achieves even higher accuracy: a Top-1 success rate of $4 1 . 8 3 \%$ Top-5 of $8 6 . 6 9 \%$ , and Top-10 of $9 3 . 1 6 \%$ . By leveraging a multitask trained graph Transformer architecture, DrugBLIP can more effectively learn deep-level features of protein–small molecule interactions and accurately capture key interaction patterns, thereby delivering superior performance in the target fishing task.

# 4 Conclusion

In this paper, we propose DrugBLIP, a machine learning model based on a SE(3) graph transformer designed to learn the interactions between proteins and molecules. As the first model to unify contrastive learning, matching learning, and docking tasks, it enables the model to understand the protein–molecule interaction process more comprehensively. Through multi-task learning, it effectively captures the protein–molecule interaction mechanism and achieves excellent performance on tasks such as virtual screening, docking, and target prediction. DrugBLIP is also expected to accelerate the drug development process, reduce research and development costs, and increase the success rate of drug development. In the future, we will continue to expand the functions of the model to enable it to support more types of biological macromolecules and design capabilities.

# Acknowledgements

The authors thank the anonymous reviewers for their valuable suggestions. The authors also thank the many open-source codebases that this work was based on, including PyTorch, PyTorch Lightning, and rdkit.

# Author contributions

Rubo Wang (Conceptualization [equal], Data curation [lead], Investigation [lead], Methodology [equal], Software [lead], Validation [lead], Visualization [lead], Writing—original draft [lead]), Xingyu Gao (Funding acquisition [lead], Methodology [supporting], Project administration [supporting], Supervision [equal], Writing— review & editing [equal]), and Peilin Zhao (Conceptualization [equal], Methodology [equal], Project administration [equal], Supervision [equal], Writing—review & editing [equal])

# Supplementary material

Supplementary material is available at Bioinformatics online.

# Conflict of interests

None declared.

# Funding

This work was supported in part by the Science and Technology Innovation (STI) 2030—Major Projects [Grant number 2022ZD0208700], the National Natural Science Foundation of China [Grant number 62376264] and the open funds of the State Key Laboratory of Chemo and Biosensing (Hunan University).

# Data availability

The data underlying this article are available in Github at https://github.com/Wolkenwandler/DrugBLIP.

# References

An D, Kim NH, Choi J-H. Practical options for selecting datadriven or physics-based prognostics algorithms with reviews. Reliab Eng Syst Saf 2015;133:223–36.   
Bhat S, Palepu K, Hong L et al. De novo design of peptide binders to conformationally diverse targets with contrastive language modeling. Sci Adv 2025;11:eadr8638.   
Cao D, Chen G, Jiang J et al. Generic protein–ligand interaction scoring by integrating physical prior knowledge and data augmentation modelling. Nat Mach Intell 2024;6:688–700.   
Chen X, Wang Y, Ma N et al. Target identification of natural medicine with chemical proteomics approach: probe synthesis, target fishing and protein identification. Signal Transduct Target Ther 2020;5:72.   
Cheng T, Li Q, Zhou Z et al. Structure-based virtual screening for drug discovery: a problem-centric review. AAPS J 2012; 14:133–41.   
Forli S, Huey R, Pique ME et al. Computational protein–ligand docking and virtual drug screening with the autodock suite. Nat Protoc 2016;11:905–19.   
Francoeur PG, Masuda T, Sunseri J et al. Three-dimensional convolutional neural networks and a cross-docked data set for structure-based drug design. J Chem Inf Model 2020;60:4200–15.   
Friesner RA, Murphy RB, Repasky MP et al. Extra precision glide: docking and scoring incorporating a model of hydrophobic enclosure for protein–ligand complexes. J Med Chem 2006; 49:6177–96.   
Gao B, Qiang B, Tan H et al. Drugclip: Contrastive protein-molecule representation learning for virtual screening. In: Oh A, Naumann T, Globerson A et al. (eds), Advances in Neural Information Processing Systems, Vol. 36, Curran Associates, Inc., 2023, 44595–614. https://proceedings.neurips.cc/paper_ files/paper/2023/file/8bd31288ad8e9a31d519fdeede7ee47d-Paper-Conference.pdf   
Krammer A, Kirchhoff PD, Jiang X et al. Ligscore: a novel scoring function for predicting binding affinities. J Mol Graph Model 2005;23:395–407.   
Landrum G et al. Rdkit: a software suite for cheminformatics, computational chemistry, and predictive modeling. Greg Landrum 2013;8:5281.   
Li J, Li D, Xiong C et al. BLIP: Bootstrapping languageimage pre-training for unified vision-language understanding and generation. In: Chaudhuri K, Jegelka S, Song L et al. (eds), Proceedings of the 39th International Conference on Machine Learning, Volume 162 of Proceedings of Machine Learning Research, PMLR, 2022, 12888–900. https://proceedings.mlr. press/v162/li22n.htm   
Liebeschuetz JW, Cole JC, Korb O. Pose prediction and virtual screening performance of gold scoring functions in a standardized test. J Comput Aided Mol Des 2012;26:737–48.   
Liu S, Johns E, Davison AJ. End-to-end multi-task learning with attention. In: 2019 IEEE/CVF Conference on Computer Vision and Pattern Recognition (CVPR), 2019, 1871–80. https://doi. org/10.1109/CVPR.2019.00197   
Mysinger MM, Carchia M, Irwin JJ et al. Directory of useful decoys, enhanced (dud-e): better ligands and decoys for better benchmarking. J Med Chem 2012;55:6582–94.   
Paul SM, Mytelka DS, Dunwiddie CT et al. How to improve R&D productivity: the pharmaceutical industry's grand challenge. Nat Rev Drug Discov 2010;9:203–14.   
Singh R, Sledzieski S, Bryson B et al. Contrastive learning in protein language space predicts interactions between drugs and protein targets. Proc Natl Acad Sci USA 2023;120:e2220778120.   
Steinegger M, Soding J. Mmseqs2 enables sensitive protein sequence searching for the analysis of massive data sets. Nat Biotechnol 2017;35:1026–8.   
Trott O, Olson AJ. Autodock vina: improving the speed and accuracy of docking with a new scoring function, efficient optimization, and multithreading. J Comput Chem 2010;31:455–61.   
Velec HF, Gohlke H, Klebe G. Drugscorecsd knowledge-based scoring function derived from small molecule crystal data with superior recognition rate of near-native ligand poses and better affinity prediction. J Med Chem 2005;48:6296–303.   
Wang R, Fang X, Lu Y et al. The pdbbind database: methodologies and updates. J Med Chem 2005;48:4111–9.   
Wang Y, Wu S, Duan Y et al. Resatom system: protein and ligand affinity prediction model based on deep learning. arXiv, arXiv: 2105.05125, 2021, preprint: not peer reviewed.   
Wojcikowski M, Ballester PJ, Siedlecki P. Performance of machine-learning scoring functions in structure-based virtual screening. Sci Rep 2017;7:46710.   
Zhang K, Yang X, Wang Y et al. Artificial intelligence in drug development. Nat Med 2025;31:45–59.   
Zhang X, Li X, Wang R. Interpretation of the binding affinities of ptp1b inhibitors with the mm-gb/sa method and the x-score scoring function. J Chem Inf Model 2009;49:1033–48.   
Zheng Z, Merz KM. Jr. Ligand identification scoring algorithm (lisa). J Chem Inf Model 2011;51:1296–306.   
Zhou G, Gao Z, Ding Q et al. Uni-mol: A universal 3D molecular representation learning framework. In: The Eleventh International Conference on Learning Representations, 2023.
