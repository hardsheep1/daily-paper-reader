---
title: Ambiguity-Aware Multi-Stage Cell-Type Annotation for Spatial Transcriptomics
title_zh: 空间转录组学中基于模糊感知的多阶段细胞类型注释
authors: "Mahmud, M. I., Kochat, V., Anzum, H., Satpati, S., Dwarampudi, J. M. R., Rai, K., Banerjee, T."
date: 2026-06-25
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.21.733596v1.full.pdf"
tags: ["query:ac"]
score: 8.0
evidence: 使用大语言模型进行聚类
tldr: "空间转录组学细胞注释面临异质性、混合群体和过渡状态等模糊性问题，现有方法强制单一标签导致过度自信。本文提出模糊感知的多阶段框架，融合空间特征聚类与轻量语言模型约束推理，依据标记覆盖、分离度和熵分配置信度，低置信度簇局部重聚类，未解决簇保留混合。在胆管癌Xenium数据上，簇级模糊性从16.1%降至2.27%，细胞级从18.4%降至0.86%，校准改善。显式处理模糊性提升了异质性肿瘤空间注释的可靠性。"
source: biorxiv
selection_source: fresh_fetch
motivation: 现有强制单一标签的细胞注释方法掩盖了异质性、混合群体和过渡状态的模糊性，导致过度自信的错误分配。
method: 提出模糊感知多阶段框架，结合空间特征聚类与约束语言模型推理，基于标记覆盖、分离度和熵分配置信度，对低置信度区域局部重聚类并保留混合簇。
result: "在胆管癌Xenium数据上，簇级模糊性从16.1%降至2.27%，细胞级从18.4%降至0.86%，且置信度校准改善。"
conclusion: 显式处理模糊性并结合拓扑集成与轻量语言模型约束，能实现更可靠且生物学一致的空间细胞注释。
---

## 摘要
空间转录组学能够表征完整组织中细胞的组织结构，但由于异质性表达谱、混合细胞群和过渡状态，稳健的细胞类型注释仍然具有挑战性。现有方法通常为每个聚类强制分配单一标签，掩盖了生物学上有意义的模糊性，并产生过于自信的分配。我们提出了一种基于模糊感知的多阶段框架用于空间细胞类型注释。该方法将混合空间特征聚类与基于精选标签集的受限语言模型推理相结合，并根据标记覆盖度、候选分离度和熵分配置信度分数。低置信度聚类通过选择性局部重新聚类模糊区域进行细化，而未解决的聚类则保留为混合状态而非强制标记。应用于来自胆管癌的10x Genomics Xenium空间转录组数据，所提出的细化方法将聚类级模糊性从16.1%降低到2.27%，细胞级模糊性从18.4%降低到0.86%，同时改善了置信度校准。空间消融实验证实，拓扑整合解决了仅基于特征基线的结构模糊性，而通过轻量级语言模型的受限推理确保了可扩展且生物学上一致的注释。这些结果强调了在异质性肿瘤中，明确的模糊性处理对于可靠的空间注释的重要性。

## Abstract
Spatial transcriptomics enables characterization of cellular organization in intact tissue, but robust cell type annotation remains challenging due to heterogeneous expression profiles, mixed populations, and transitional states. Existing methods often enforce a single label per cluster, obscuring biologically meaningful ambiguity and producing overconfident assignments. We propose an ambiguity-aware, multi-stage framework for spatial cell-type annotation. The method combines hybrid spatial feature clustering with constrained language-model inference over curated label sets, and assigns confidence scores based on marker coverage, candidate separation, and entropy. Low-confidence clusters are selectively refined via local reclustering of ambiguous regions, while unresolved clusters are preserved as mixed rather than forcibly labeled. Applied to 10x Genomics Xenium spatial transcriptomics data from cholangiocarcinoma, the proposed refinement reduces cluster-level ambiguity from 16.1% to 2.27% and cell-level ambiguity from 18.4% to 0.86%, while improving confidence calibration. Spatial ablation confirms that topological integration resolves structural ambiguity over feature-only baselines, while constrained inference via a lightweight language model ensures scalable and biologically coherent annotations. These results highlight the importance of explicit ambiguity handling for reliable spatial annotation in heterogeneous tumors.

---

## 论文详细总结（自动生成）

好的，以下是根据您提供的论文内容生成的结构化、深入、客观的中文总结。

---

### 论文核心总结：基于模糊感知的多阶段空间转录组细胞类型注释

#### 1. 核心问题与整体含义（研究动机与背景）

-   **核心问题**：空间转录组学技术能够绘制基因表达的空间图谱，但在异质性肿瘤（如胆管癌）微环境中，细胞类型注释面临巨大挑战。主要挑战包括：① 细胞转录谱的异质性；② 区域中混合细胞群的存在；③ 细胞处于不同过渡状态。
-   **现有方法的不足**：传统注释方法（如全局聚类后单一标签分配）通常为每个聚类强制分配一个标签，这不仅掩盖了生物学上真实存在的“模糊性”和混合状态，还会产生“过度自信”的错误分配。
-   **研究动机**：现有方法将模糊性视为噪声，而本文认为模糊性是一种需要被明确量化和处理的生物学信号。
-   **整体含义**：本文旨在提出一种能显式处理、量化并保留注释模糊性的多阶段框架，以提高空间注释在异质性组织中的**可靠性**和**生物学一致性**。

#### 2. 方法论：核心思想、关键技术细节

-   **核心思想**：通过“**阶段化处理**”和“**置信度感知**”来管理注释中的不确定性。框架优先执行全局注释，然后对低置信度区域进行“放大镜”式的精细局部处理。对于无法明确分离的区域，选择“**弃权（Abstention）**”，而非强行标记。
-   **关键技术细节**：
    1.  **混合空间-特征聚类（Hybrid Spatial-Feature Clustering）**：
        -   利用**分层稀疏非负矩阵分解（hSNMF）** 从基因表达数据 `X` 中学习低维嵌入 `Z`。
        -   分别构建**特征图**（基于基因表达相似性）和**空间图**（基于物理邻近性）。
        -   将两者融合为混合邻接矩阵：`A_mix = α * A_spatial + (1-α) * A_feature`。
        -   在 `A_mix` 上执行 **Leiden 聚类**，获得初始聚类结果。
    2.  **约束语言模型注释**：
        -   使用 **Qwen2.5-1.5B-Instruct** 轻量级语言模型作为注释器。
        -   **约束空间**：从 **PanglaoDB** 和 **CellMarker** 数据库构建精选的候选标签集，限制模型的预测范围。
        -   **输入内容丰富**：为模型提供排名靠前的标记基因、GO-Slim功能富集结果和标记基因证据，引导模型进行更精准的推断。
    3.  **置信度评分与模糊检测**：
        -   基于**标记基因覆盖率**、**顶部分数**、**候选标签分离度**和 **Shannon 熵**等指标，计算每个聚类的标量**置信度分数**。
        -   任何未通过分离度准则（如覆盖率<0.25）的聚类被标记为“**模糊**”，不进行最终标记。
    4.  **多阶段细化与弃权**：
        -   对于标记为“模糊”的聚类，进入**第二阶段**：提取对应细胞在原混合图中的诱导子图，重新进行Leiden聚类。
        -   分裂出的子聚类满足最小尺寸等结构约束后，重新进行语义注释和置信度评分。
        -   如果细化后仍无法达到分离度标准，则**保留为混合状态，不分配标签**。

#### 3. 实验设计：数据集、Benchmark与对比方法

-   **数据集**：使用 **10x Genomics Xenium** 平台获取的**胆管癌**空间转录组数据。
-   **基准与对比方法**：框架本身即为一种新方法，其实验设计主要评估其关键组件的有效性。对比了三种配置：
    1.  **Hybrid 1-Stage**：单阶段混合图聚类 + 注释，**作为基线基准**。
    2.  **Hybrid 2-Stage**：**本文提出的完整方法**，混合图 + 多阶段细化。
    3.  **Feature (2-Stage)**：**消融实验基准**，与 Hybrid 2-Stage 结构相同，但**移除空间信息**，仅使用特征图。
-   **评估指标**：聚类级和细胞级的**模糊率（Ambiguous Rate）**、**平均置信度（Mean Confidence）**。

#### 4. 资源与算力

-   论文明确指出使用了 **Qwen2.5-1.5B-Instruct** 模型，这是一个**轻量级语言模型**。
-   **未明确说明**训练该语言模型所用的具体 GPU 型号、数量或训练时长。框架本身仅使用该模型进行*推理（Inference）*，无需额外的微调，因此对算力要求相对较低。

#### 5. 实验数量与充分性

-   **实验数量**：论文主要进行了**三组**核心定量对比实验（表1），并辅以一个**定性案例研究**（表2，图3）。
-   **充分性评估**：
    -   **客观**：实验设计清晰地比较了单阶段 vs. 两阶段，以及纯特征 vs. 混合图，构成了一组有效的**消融研究**，直接验证了“多阶段细化”和“空间信息集成”两个核心贡献。
    -   **局限**：
        1.  **单一数据集**：所有实验都基于一个特定的胆管癌 Xenium 数据集，**缺乏跨组织类型、跨技术水平（如 Visium, Slide-seq）的外部泛化性验证**。
        2.  **缺乏与外部方法对比**：虽然通过消融实验证明了自身组件的有效性，但**没有与当前流行的其他方法（如 spaGCN, GraphST, 或基于大模型的 scGPT 等）进行直接性能对比**，这使得其在现有方法“竞技场”中的相对优势不够明确。
        3.  **无统计显著性与误差**：实验没有报告多次运行的平均值和标准差，**无法判断结果的统计显著性和稳定性**。

#### 6. 主要结论与发现

-   **核心发现**：显式处理模糊性并将其作为可操作的信号，能显著提高空间注释的**可靠性**。
-   **具体结果**：
    -   **模糊性显著降低**：在胆管癌数据集上，多阶段细化使**聚类级模糊率**从16.1%降至2.27%，**细胞级模糊率**从18.4%降至0.86%。
    -   **空间信息至关重要**：混合空间-特征图相较于纯特征图，能产生更低的模糊率，证明拓扑集成有助于解决结构上的模糊性。
    -   **细化的有效性**：定性案例（图3，表2）清晰展示了细化过程如何将一个模糊的“**混合区**”成功分离为空间和转录上均更一致的高置信度细胞种群（如反应性胆管细胞与巨噬细胞）。
    -   **生物学一致性**：保留未解决区域的混合状态，比强行标记更符合生物学事实，避免了误分类。

#### 7. 优点：方法与实验设计亮点

-   **创新性**：明确提出“将模糊性视为生物学信号而非噪声”的观点，并设计了相应的处理流程（量化和保留），这是领域内较新的思路。
-   **方法设计的完整性**：框架形成了一个完整的闭环：发现模糊 -> 量化置信度 -> 选择性细化 -> 决定标记或弃权。逻辑严谨，步骤清晰。
-   **实用导向**：采用**轻量级语言模型**进行推理，避免了高算力成本和复杂的模型微调，使得方法更易推广和应用。
-   **验证的直观性**：图3的定性案例非常具有说服力，直观地展示了空间上下文信息在分辨复杂混合区域时的决定性作用，有力地支撑了其核心论点。

#### 8. 不足与局限

-   **实验验证的局限性**：
    -   **泛化性不足**：仅在单一数据集的单次运行上验证，结果可能存在数据特异性，**外部有效性存疑**。
    -   **缺乏权威基准对比**：未与SOTA方法（如spaGCN, Seurat, GraphST等）直接比较性能指标，评估不够全面，无法证明其相较于主流方法有压倒性优势。
    -   **缺乏统计严谨性**：没有报告标准误差或进行统计检验，实验结果存在偶然性风险，可信度打折扣。
-   **应用限制**：
    -   **依赖先验知识**：方法的有效性高度依赖于所选用的**数据库（PanglaoDB, CellMarker）的质量**和**设定的阈值参数**（如最小覆盖率）。在不同组织中，这些阈值和知识库可能需要手动调整。
    -   **无法根除模糊性**：即使经过两阶段处理，最终仍有0.86%的细胞被认为是模糊的。在需要完全确定性注释的下游任务（如病理诊断）中，这些剩余的模糊细胞需要谨慎处理。

（完）
